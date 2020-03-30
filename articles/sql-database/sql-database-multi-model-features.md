---
title: Çok modelli özellikler
description: Azure SQL Veritabanı, aynı veritabanında birden çok veri modeliyle çalışmanızı sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
ms.date: 12/17/2018
ms.openlocfilehash: 2e8519fa8d96b7fe016b9da4ba84ce481a57d94e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73802824"
---
# <a name="multi-model-capabilities-of-azure-sql-database"></a>Azure SQL Veritabanı'nın çok modelli özellikleri

Çok modelli veritabanları, ilişkisel veriler, grafikler, JSON/XML belgeleri, anahtar değer çiftleri gibi birden çok veri biçiminde temsil edilen verileri depolamanızı ve bunlarla çalışmanızı sağlar.

## <a name="when-to-use-multi-model-capabilities"></a>Çok modelli yetenekler ne zaman kullanılır?

Azure SQL Veritabanı, çeşitli genel amaçlı uygulamalar için çoğu durumda en iyi performansı sağlayan ilişkisel modelle çalışmak üzere tasarlanmıştır. Ancak, Azure SQL Veritabanı yalnızca ilişkisel verilerle sınırlı değildir. Azure SQL Veritabanı, ilişkisel modele sıkıca entegre edilmiş çeşitli ilişkisel olmayan biçimleri kullanmanıza olanak tanır.
Aşağıdaki durumlarda Azure SQL Veritabanı'nın çok model liyeteneklerini kullanmayı düşünmelisiniz:
- NoSQL modelleri için daha uygun olan bazı bilgilere veya yapılara sahipsiniz ve ayrı NoSQL veritabanı kullanmak istemiyorsunuz.
- Verilerinizin çoğu ilişkisel model için uygundur ve verilerinizin bazı bölümlerini NoSQL stilinde modellemeniz gerekir.
- Hem ilişkisel hem de NoSQL verilerini sorgulamak ve çözümlemek ve sql dilini kullanabilen çeşitli araçlar ve uygulamalarla tümleştirmek için zengin Transact-SQL dilinden yararlanmak istiyorsunuz.
- NoSQL veri yapılarınızın analitik veya işleme performansını artırmak, verilerinizin başka bir yerde kopyasını oluşturmak ve birincil veritabanından bazı analitik iş yüklerini boşaltmak için [işlem çoğaltma](sql-database-managed-instance-transactional-replication.md) veya [okunabilir yinelemeler](sql-database-read-scale-out.md) kullanmak için [bellek içi teknolojiler](sql-database-in-memory.md) gibi veritabanı özelliklerini uygulamak istiyorsunuz.

## <a name="overview"></a>Genel Bakış

Azure SQL aşağıdaki çok modelözelliklerini sağlar:
- [Grafik özellikleri,](#graph-features) verilerinizi düğüm ler ve kenarlar kümesi olarak temsil etmenizi ve grafik verilerini `MATCH` sorgulamak için grafik işleciyle geliştirilmiş standart Transact-SQL sorgularını kullanmanızı sağlar.
- [JSON özellikleri,](#json-features) JSON belgelerini tablolara koymanızı, ilişkisel verileri JSON belgelerine dönüştürmenizi ve bunun tersi yapmanızı sağlar. Belgeleri ayrışdırmak için JSON işlevleriyle geliştirilmiş standart Transact-SQL dilini kullanabilir ve sorgularınızı optimize etmek için kümelenmiş olmayan dizinler, sütun deposu dizinleri veya bellek için optimize edilmiş tablolar kullanabilirsiniz.
- [Uzamsal özellikler,](#spatial-features) coğrafi ve geometrik verileri depolamanızı, uzamsal dizinleri kullanarak dizine ekmenizi ve uzamsal sorguları kullanarak verileri almanıza olanak tanır.
- [XML özellikleri,](#xml-features) XML verilerini veritabanınızda depolamanızı ve dizine eklemenizi ve XML verileriyle çalışmak için yerel XQuery/XPath işlemlerini kullanmanıza olanak tanır. Azure SQL veritabanında, XML verilerini işleyen özel yerleşik XML sorgu altyapısı vardır.
- [Anahtar değer çiftleri](#key-value-pairs) iki sütunlu tablolar olarak yerel olarak modellenebileceğinden, anahtar değer çiftleri özel özellikler olarak açıkça desteklenmez.

  > [!Note]
  > Veritabanında depoladığınız verilere erişmek için aynı Transact-SQL sorgusunda JSON Path ifadesini, XQuery/XPath ifadelerini, uzamsal işlevleri ve grafik sorgu ifadelerini kullanabilirsiniz. Ayrıca, Transact-SQL sorgularını yürütebilen herhangi bir araç veya programlama dili, çoklu model verilerine erişmek için bu sorgu arabirimini de kullanabilir. Bu, farklı veri modelleri için özel API sağlayan [Azure Cosmos DB](/azure/cosmos-db/) gibi çok modelli veritabanlarıyla karşılaştırıldığında önemli bir farktır.

Aşağıdaki bölümlerde, Azures SQL Veritabanı'nın en önemli çok modelli özellikleri hakkında bilgi edinebilirsiniz.

## <a name="graph-features"></a>Grafik özellikleri

Azure SQL Veritabanı, veritabanındaki çok-çok ilişkileri modellemek için grafik veritabanı özellikleri sunar. Grafik düğümler (veya vertices) ve kenarları (veya ilişkileri) bir koleksiyondur. Düğüm bir varlığı (örneğin, bir kişiyi veya kuruluşu) temsil eder ve kenar bağlandığı iki düğüm arasındaki ilişkiyi (örneğin, beğenmeler veya arkadaşlar) temsil eder. Grafik veritabanını benzersiz yapan bazı özellikler şunlardır:
- Kenarlar veya ilişkiler, Grafik Veritabanı'ndaki birinci sınıf varlıklardır ve bunlarla ilişkili özniteliklere veya özelliklere sahip olabilir.
- Tek bir kenar, Grafik Veritabanı'ndaki birden çok düğümü esnek bir şekilde bağlayabilir.
- Desen eşleştirme ve çoklu atlama gezinti sorgularını kolayca ifade edebilirsiniz.
- Geçişli kapatma ve polimorfik sorguları kolayca ifade edebilirsiniz.

Grafik ilişkileri ve grafik sorgu yetenekleri Transact-SQL'e entegre edilir ve temel veritabanı yönetim sistemi olarak SQL Server'ı kullanmanın avantajlarından yararlanır.
[Grafik işleme](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) çekirdek SQL Server Database Engine özelliğidir, böylece orada Grafik işleme hakkında daha fazla bilgi bulabilirsiniz.

### <a name="when-to-use-a-graph-capability"></a>Grafik özelliği ne zaman kullanılır?

İlişkisel bir veritabanı kullanılarak elde edilemeyen bir grafik veritabanının elde edebileceği hiçbir şey yoktur. Ancak, grafik veritabanı belirli sorguları ifade etmeyi kolaylaştırabilir. Birini diğerine tercih etme kararınız aşağıdaki etkenlere bağlı olabilir:

- Bir düğümün birden çok ebeveyni olabileceği hiyerarşik verileri modelle, bu nedenle HiyerarşiK Kimlik kullanılamaz
- Model uygulamanız karmaşık çok-çok ilişkileri vardır; uygulama geliştikçe, yeni ilişkiler eklenir.
- Birbirine bağlı verileri ve ilişkileri çözümlemeniz gerekir.

## <a name="json-features"></a>JSON özellikleri

Azure SQL Veritabanı, JavaScript Nesne Gösterimi [(JSON)](https://www.json.org/) biçiminde temsil edilen verileri ayrıştırmanızı ve sorgulamanızı ve ilişkisel verilerinizi JSON metni olarak dışa aktarmanızı sağlar.

JSON, modern web ve mobil uygulamalarda veri alışverişi için kullanılan popüler bir veri biçimidir. JSON ayrıca yarı yapılandırılmış verileri günlük dosyalarında veya [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)gibi NoSQL veritabanlarında depolamak için de kullanılır. Birçok REST web hizmeti, JSON metni olarak biçimlendirilmiş sonuçları döndürer veya JSON olarak biçimlendirilmiş verileri kabul eder. [Azure Bilişsel Arama, Azure](https://azure.microsoft.com/services/search/) [Depolama](https://azure.microsoft.com/services/storage/)ve [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) gibi çoğu Azure hizmeti, JSON'u döndürebilen veya tüketen REST uç noktalarına sahiptir.

Azure SQL Veritabanı, JSON verileriyle kolayca çalışmanızı ve veritabanınızı modern hizmetlerle tümleştirmenizi sağlar. Azure SQL Veritabanı, JSON verileriyle çalışmak için aşağıdaki işlevleri sağlar:

![JSON Fonksiyonları](./media/sql-database-json-features/image_1.png)

Eğer JSON metin varsa, JSON veri ayıklamak veya JSON düzgün [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx)dahili işlevleri kullanılarak biçimlendirilmiş olduğunu doğrulamak , [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), ve [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) işlevi, JSON metninin içindeki değeri güncelleştirmenizi sağlar. Daha gelişmiş sorgulama ve çözümleme için [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) işlevi, json nesnelerinin bir dizisini bir satır kümesine dönüştürebilir. Herhangi bir SQL sorgusu döndürülen sonuç kümesinde yürütülebilir. Son olarak, ilişkisel tablolarınızda depolanan verileri JSON metni olarak biçimlendirmenize olanak tanıyan bir [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) yan tümcesi vardır.

Daha fazla bilgi için [azure SQL Veritabanı'nda JSON verileriyle nasıl çalışılabildiğini](sql-database-json-features.md)öğrenin.
[JSON](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) çekirdek SQL Server Database Engine özelliğidir, böylece orada JSON özelliği hakkında daha fazla bilgi bulabilirsiniz.

### <a name="when-to-use-a-json-capability"></a>JSON özelliği ne zaman kullanılır?

Belge modelleri, bazı belirli senaryolarda ilişkisel modeller yerine kullanılabilir:
- Şemonun yüksek normalleştirilmesi önemli faydalar sağlamaz, çünkü nesnelerin tüm alanlarına aynı anda erişebilirsiniz veya nesnelerin normalleştirilmiş bölümlerini asla güncellemezsiniz. Ancak, normalleştirilmiş model, verileri almak için birleştirmeniz gereken çok sayıda tablo nedeniyle sorgularınızın karmaşıklığını artırır.
- JSON belgelerini yerel olarak kullanan uygulamalarla iletişim veya veri modellerisiniz ve ilişkisel verileri JSON'a dönüştüren ek katmanları json'a veya tam tersi olarak tanıtmak istemezsiniz.
- Alt tabloları veya Varlık-Nesne-Değer modellerini normalleştirerek veri modelinizi basitleştirmeniz gerekir.
- Verileri ayrıştıran ek bir araç olmadan JSON biçiminde depolanan verileri yüklemeniz veya dışa aktarmanız gerekir.

## <a name="spatial-features"></a>Uzamsal özellikler

Uzamsal veriler, geometrik nesnelerin fiziksel konumu ve şekli hakkındaki bilgileri temsil eder. Bu nesneler nokta konumları veya ülkeler/bölgeler, yollar veya göller gibi daha karmaşık nesneler olabilir.

Azure SQL Veritabanı iki uzamsal veri türünü destekler : geometri veri türü ve coğrafya veri türü.
- Geometri türü Öklid (düz) koordinat sistemindeki verileri temsil eder.
- Coğrafya türü, yuvarlak dünya koordinat sistemindeki verileri temsil eder.

[Point](https://docs.microsoft.com/sql/relational-databases/spatial/point), [LineString](https://docs.microsoft.com/sql/relational-databases/spatial/linestring), [Polygon,](https://docs.microsoft.com/sql/relational-databases/spatial/polygon)vb. gibi Azure SQL veritabanında kullanılabilecek bir dizi Uzamsal nesne vardır.

Azure SQL Veritabanı, uzamsal sorgularınızın performansını artırmak için kullanılabilecek özel [Uzamsal dizinler](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-indexes-overview) de sağlar.

[Uzamsal destek](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) temel SQL Server Database Engine özelliğidir, böylece mekansal özellik hakkında daha fazla bilgi bulabilirsiniz.

## <a name="xml-features"></a>XML özellikleri

SQL Server, yarı yapılandırılmış veri yönetimi için zengin uygulamalar geliştirmek için güçlü bir platform sağlar. XML desteği SQL Server'daki tüm bileşenlere entegre edilmiştir ve aşağıdakileri içerir:

- XML veri türü. XML değerleri, XML şemakoleksiyonuna göre yazılabilen veya yazılmamış bırakılmış bir XML veri türü sütununda yerel olarak depolanabilir. XML sütununa dizin bekleyebilirsiniz.
- XML türündeki sütunlarda ve değişkenlerde depolanan XML verilerine karşı bir XQuery sorgusu belirtme yeteneği. XQuery işlevleri, veritabanınızda kullandığınız herhangi bir veri modeline erişen herhangi bir Transact-SQL sorgusunda kullanılabilir.
- [Birincil XML dizini](https://docs.microsoft.com/sql/relational-databases/xml/xml-indexes-sql-server#primary-xml-index) kullanarak XML belgelerindeki tüm öğeleri otomatik olarak dizine dizin veya [ikincil XML dizini](https://docs.microsoft.com/sql/relational-databases/xml/xml-indexes-sql-server#secondary-xml-indexes)kullanılarak dizine eklenmeniz gereken yolları belirtin.
- XML verilerinin toplu yüklenmesine izin veren OPENROWSET.
- İlişkisel verileri XML biçimine dönüştürün.

[XML](https://docs.microsoft.com/sql/relational-databases/xml/xml-data-sql-server) temel SQL Server Database Engine özelliğidir, böylece xml özelliği hakkında daha fazla bilgi bulabilirsiniz.

### <a name="when-to-use-an-xml-capability"></a>XML özelliği ne zaman kullanılır?

Belge modelleri, bazı belirli senaryolarda ilişkisel modeller yerine kullanılabilir:
- Şemonun yüksek normalleştirilmesi önemli faydalar sağlamaz, çünkü nesnelerin tüm alanlarına aynı anda erişebilirsiniz veya nesnelerin normalleştirilmiş bölümlerini asla güncellemezsiniz. Ancak, normalleştirilmiş model, verileri almak için birleştirmeniz gereken çok sayıda tablo nedeniyle sorgularınızın karmaşıklığını artırır.
- XML belgelerini yerel olarak kullanan uygulamalarla iletişim veya veri modellerisiniz ve ilişkisel verileri XML'e dönüştüren ek katmanları veya tam tersi tanıtmak istemezsiniz.
- Alt tabloları veya Varlık-Nesne-Değer modellerini normalleştirerek veri modelinizi basitleştirmeniz gerekir.
- Verileri ayrıştıran ek bir araç olmadan XML biçiminde depolanan verileri yüklemeniz veya dışa aktarmanız gerekir.

## <a name="key-value-pairs"></a>Anahtar-değer çiftleri

Azure SQL Veritabanı'nda anahtar değer yapıları standart ilişkisel tablolar olarak yerel olarak temsil edilebildiği için anahtar değer çiftlerini destekleyen özel türleri veya yapıları yoktur:

```sql
CREATE TABLE Collection (
  Id int identity primary key,
  Data nvarchar(max)
)
```

Bu anahtar değer yapısını, herhangi bir kısıtlama olmaksızın gereksinimlerinize uyacak şekilde özelleştirebilirsiniz. Örnek olarak, değer `nvarchar(max)` tür yerine XML belgesi olabilir, değer JSON belgesi `CHECK` ise, JSON içeriğinin geçerliliğini doğrulayan kısıtlama koyabilirsiniz. Ek sütunlara bir anahtarla ilgili herhangi bir sayıda değer koyabilir, veri erişimini basitleştirmek ve optimize etmek için hesaplanmış sütunlar ve dizinler ekleyebilir, daha iyi performans elde etmek için tabloyu bellek/en iyi duruma getirilmiş şema tablosu olarak tanımlayabilirsiniz, vb.

[BWin'in,](https://blogs.msdn.microsoft.com/sqlcat/20../../how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/) ilişkisel modelin pratikte anahtar değer çifti çözümü olarak nasıl etkin bir şekilde kullanılabileceğini örnek olarak, saniyede 1.200.000 toplu iş elde eden ASP.NET önbelleğe alma çözümü için benzeri görülmemiş bir performans ve ölçek lendirmek için Bellek içi OLTP'yi nasıl kullandığını görün.

## <a name="next-steps"></a>Sonraki adımlar
Azure SQL Veritabanlarındaki çok modelli özellikler, Azure SQL Veritabanı ve SQL Server arasında paylaşılan temel SQL Server Veritabanı Altyapısı özellikleridir. Bu özellikler hakkında daha fazla bilgi edinmek için SQL İlişkisel veritabanı dokümantasyon sayfalarını ziyaret edin:

* [Grafik işleme](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview)
* [JSON verileri](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server)
* [Mekansal destek](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [XML verileri](https://docs.microsoft.com/sql/relational-databases/xml/xml-data-sql-server)
