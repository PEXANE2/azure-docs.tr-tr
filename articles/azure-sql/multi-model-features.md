---
title: Çok modelli özellikler
description: SQL Microsoft Azure, aynı veritabanında birden çok veri modeli ile çalışmanıza olanak sağlar.
services: sql-database
ms.service: sql-db-mi
ms.subservice: features
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
ms.date: 12/17/2018
ms.openlocfilehash: 59a709a206eb29b875272674ee19e414023cc37f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87073312"
---
# <a name="multi-model-capabilities-of-azure-sql-database--sql-managed-instance"></a>Azure SQL veritabanı & SQL yönetilen örneği 'nin çok modelli özellikleri
[!INCLUDE[appliesto-sqldb-sqlmi](includes/appliesto-sqldb-sqlmi.md)]

Çok modelli veritabanları, ilişkisel veriler, grafikler, JSON/XML belgeleri, anahtar-değer çiftleri gibi birden çok veri biçiminde temsil edilen verileri depolamanıza ve bunlarla çalışmanıza olanak sağlar.

## <a name="when-to-use-multi-model-capabilities"></a>Çok modelli yetenekler ne zaman kullanılır?

[Azure SQL ürün ailesi](azure-sql-iaas-vs-paas-what-is-overview.md) , çeşitli genel amaçlı uygulamalar için çoğu durumda en iyi performansı sağlayan ilişkisel modelle çalışacak şekilde tasarlanmıştır. Ancak, Azure SQL ürün ailesi yalnızca ilişkisel verilerle sınırlı değildir. Azure SQL ürün ailesi, ilişkisel modele sıkı bir şekilde tümleştirilmiş, ilişkisel olmayan çeşitli biçimleri kullanmanıza olanak sağlar.
Aşağıdaki durumlarda, Azure SQL ürün ailesinin çok modelli yeteneklerini kullanmayı göz önünde bulundurmanız gerekir:

- NoSQL modellerine daha iyi uyan bazı bilgi veya yapılar vardır ve ayrı NoSQL veritabanı kullanmak istemezsiniz.
- Verilerinizin çoğunluğu ilişkisel model için uygundur ve NoSQL stilinde verilerinizin bazı parçalarını modelleyebilirsiniz.
- Hem ilişkisel hem de NoSQL verilerini sorgulamak ve analiz etmek için zengin Transact-SQL dilinden yararlanmak ve bunu SQL dilini kullanan çeşitli araçlar ve uygulamalarla bütünleştirmek istiyorsunuz.
- NoSQL Veri yapılarınızın analitik veya işlemesinin performansını artırmak üzere [bellek içi teknolojiler](in-memory-oltp-overview.md) gibi veritabanı özelliklerini uygulamak, verilerinizin başka bir yerinde kopyasını oluşturmak ve bazı analitik iş yüklerinin birincil veritabanından yük devretmek için [işlem çoğaltma](managed-instance/replication-transactional-overview.md) veya [okunabilir çoğaltmalar](database/read-scale-out.md) kullanın.

## <a name="overview"></a>Genel Bakış

Azure SQL ürün ailesi aşağıdaki çok modelli özellikleri sağlar:

- [Grafik özellikleri](#graph-features) , verilerinizi düğüm ve kenar kümesi olarak temsil ediyor ve `MATCH` Graf verilerini sorgulamak için Graph Işleciyle geliştirilmiş standart Transact-SQL sorguları kullanmanızı sağlar.
- [JSON özellikleri](#json-features) , JSON belgelerini tablolara yerleştirmeniz, ILIŞKISEL verileri JSON belgelerine dönüştürebilmeniz ve tam tersi de sağlar. Sorguları iyileştirmek için JSON işlevleriyle geliştirilmiş standart Transact-SQL dilini kullanabilir ve kümelenmemiş dizinler, columnstore dizinleri ya da bellek için iyileştirilmiş tablolar kullanabilirsiniz.
- [Uzamsal Özellikler](#spatial-features) , coğrafi ve geometrik verileri depolamanıza, uzamsal dizinleri kullanarak dizinlemenize ve uzamsal sorgular kullanarak verileri almanıza olanak sağlar.
- XML [özellikleri](#xml-features) , veritabanınıza XML verilerini depolayıp DIZINLERINIZI ve XML verileriyle çalışmak Için yerel XQuery/XPath işlemlerini kullanmanızı sağlar. Azure SQL ürünlerinin, XML verilerini işleyen özelleştirilmiş bir yerleşik XML sorgu altyapısı vardır.
- Anahtar-değer çiftleri, iki sütunlu tablolar olarak yerel olarak modellendirildiğinden, [anahtar-değer çiftleri](#key-value-pairs) özel özellikler olarak açıkça desteklenmez.

  > [!Note]
  > Veritabanında depoladığınız tüm verilere erişmek için aynı Transact-SQL sorgusunda JSON yol ifadesini, XQuery/XPath ifadelerini, uzamsal işlevleri ve Graph-Query ifadelerini de kullanabilirsiniz. Ayrıca, Transact-SQL sorgularını yürüteerişebilen herhangi bir araç veya programlama dili, çok modelli verilere erişmek için bu sorgu arabirimini de kullanabilir. Bu, farklı veri modelleri için özelleştirilmiş API sağlayan [Azure Cosmos DB](/azure/cosmos-db/) gibi çok modelli veritabanlarıyla karşılaştırıldığında önemli farktır.

Aşağıdaki bölümlerde, Azure SQL ürün ailesinin en önemli çok modelli özellikleri hakkında bilgi edinebilirsiniz.

## <a name="graph-features"></a>Grafik özellikleri

Azure SQL ürün ailesi, veritabanında çok-çok ilişkilerini modelleyebilir grafik veritabanı yetenekleri sunar. Grafik, düğümlerin (veya köşelerin) ve kenarlarının (ya da ilişkilerin) bir koleksiyonudur. Düğüm, bir varlığı (örneğin, bir kişi veya kuruluş) temsil eder ve bir kenar, bağlandığı iki düğüm (örneğin, beğeni veya arkadaşları) arasındaki ilişkiyi temsil eder. Grafik veritabanının benzersiz olmasını sağlayan bazı özellikler şunlardır:

- Kenarlar veya ilişkiler bir grafik veritabanındaki ilk sınıf varlıklardır ve bunlarla ilişkili özniteliklere veya özelliklere sahip olabilir.
- Tek bir kenar, bir grafik veritabanında birden çok düğümü esnek bir şekilde bağlanabilir.
- Model eşleştirme ve çok atlamalı gezinti sorgularını kolayca ifade edebilirsiniz.
- Geçişli kapanış ve Polimorfik sorguları kolayca ifade edebilirsiniz.

[Grafik ilişkileri ve grafik sorgu özellikleri](/sql/relational-databases/graphs/sql-graph-overview) Transact-SQL ile tümleşiktir ve temel veritabanı yönetim sistemi olarak SQL Server veritabanı altyapısını kullanmanın avantajlarını alır.

### <a name="when-to-use-a-graph-capability"></a>Grafik özelliği ne zaman kullanılır?

Bir grafik veritabanının elde edilebileceği, ilişkisel bir veritabanı kullanılarak ulaşılabilecek bir şey yok. Ancak, bir grafik veritabanı belirli sorguları hızlı bir şekilde ifade edebilir. Diğeri arasından birini seçme kararınız aşağıdaki etkenlere bağlı olabilir:

- Bir düğümün birden çok üst öğesi olabilecek hiyerarşik verileri modelleyin, bu nedenle HierarchyId kullanılamaz
- Modelde, uygulamanızın çok-çok ilişkileri karmaşık; Uygulama geliştikçe yeni ilişkiler eklenir.
- Bağlantılı verileri ve ilişkileri analiz etmeniz gerekir.

## <a name="json-features"></a>JSON özellikleri

Azure SQL ürünleri ailesi, JavaScript Nesne Gösterimi [(JSON)](https://www.json.org/) biçiminde temsil edilen verileri ayrıştırmanıza ve sorgulamanızı ve ILIŞKISEL verilerinizi JSON metni olarak dışarı aktarmayı sağlar.

JSON, Modern Web ve mobil uygulamalarda veri alışverişi için kullanılan popüler bir veri biçimidir. JSON, yarı yapılandırılmış verileri günlük dosyalarında veya [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)gibi NoSQL veritabanlarında depolamak için de kullanılır. Birçok REST Web hizmeti, JSON metni olarak biçimlendirilen sonuçları döndürür veya JSON olarak biçimlendirilen verileri kabul eder. [Azure bilişsel arama](https://azure.microsoft.com/services/search/), [azure depolama](https://azure.microsoft.com/services/storage/)ve [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) gibi birçok Azure HIZMETI, JSON döndüren veya tüketen REST uç noktalarına sahiptir.


Azure SQL ürünleri, JSON verileriyle kolayca çalışmanıza ve veritabanınızı modern hizmetlerle tümleştirmenize olanak tanır ve JSON verileriyle çalışmak için aşağıdaki işlevleri sağlar:

![JSON Işlevleri](./media/multi-model-features/image_1.png)

JSON metinleriniz varsa JSON 'dan veri ayıklayabilir veya [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql), [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql)ve [ıSJSON](/sql/t-sql/functions/isjson-transact-sql)yerleşik işlevlerini kullanarak JSON 'ın düzgün biçimlendirildiğinden emin olabilirsiniz. [JSON_MODIFY](/sql/t-sql/functions/json-modify-transact-sql) IşLEVI, JSON metni içindeki değeri güncelleştirmenizi sağlar. Daha gelişmiş sorgulama ve çözümleme için, [openjson](/sql/t-sql/functions/openjson-transact-sql) işlevi BIR dizi JSON nesnesini bir dizi satır içine dönüştürebilir. Döndürülen sonuç kümesinde herhangi bir SQL sorgusu çalıştırılabilir. Son olarak, ilişkisel Tablolarınızda depolanan verileri JSON metni olarak biçimlendirmenize imkan tanıyan bir [for JSON](/sql/relational-databases/json/format-query-results-as-json-with-for-json-sql-server) yan tümcesi vardır.

Daha fazla bilgi için bkz. [JSON verileriyle çalışma](database/json-features.md).
[JSON](/sql/relational-databases/json/json-data-sql-server) , bir çekirdek SQL Server veritabanı altyapısı özelliğidir.

### <a name="when-to-use-a-json-capability"></a>JSON özelliği ne zaman kullanılır?

Belge modelleri, bazı belirli senaryolarda ilişkisel modeller yerine kullanılabilir:

- Tek seferde tüm nesne alanlarına erişirken veya nesnelerin normalleştirilmiş kısımlarını hiçbir zaman güncelleştirmeyeceğinden şemanın yüksek düzeyde normalleştirilmesi önemli avantajlar getirmez. Ancak, verileri almak için katılmanız gereken çok sayıda tablo olması nedeniyle, normalleştirilmiş model, sorgularınızın karmaşıklığını artırır.
- JSON belgelerini yerel olarak kullanan uygulamalarla çalışıyorsunuz iletişim veya veri modelleridir ve ilişkisel verileri JSON ve tam tersi dönüştüren ek katmanlar tanıtmak istemezsiniz.
- Alt tabloları veya varlık-nesne-değer düzenlerini yeniden normalleştirerek veri modelinizi basitleştirmeniz gerekir.
- Verileri çözümleyen ek bir araç olmadan JSON biçiminde depolanan verileri yüklemeniz veya dışarı aktarmanız gerekir.

## <a name="spatial-features"></a>Uzamsal Özellikler

Uzamsal veriler, geometrik nesnelerin fiziksel konumu ve şekli hakkındaki bilgileri temsil eder. Bu nesneler, nokta konumları veya ülkeler/bölgeler, yollar veya Lakes gibi daha karmaşık nesneler olabilir.

 Desteklenen iki uzamsal veri türü: 

- Geometri türü, bir Euclidean (düz) koordinat sistemindeki verileri temsil eder.
- Coğrafya türü, bir yuvarlak dünya koordinat sistemindeki verileri temsil eder.

Ürünlerin Azure SQL ailesinde kullanılabilen birçok uzamsal nesne vardır ve JavaScript Nesne Gösterimi [(JSON)](https://www.json.org/) biçiminde gösterilen verileri ayrıştırmanıza ve sorgulamanızı ve ILIŞKISEL verilerinizi JSON metni olarak dışarı aktarmanız gerekir.
[nokta](/sql/relational-databases/spatial/point), [LineString](/sql/relational-databases/spatial/linestring), [Çokgen](/sql/relational-databases/spatial/polygon)vb. gibi.

Azure SQL ürünleri, uzamsal Sorgularınızın performansını geliştirmek için kullanılabilecek özelleştirilmiş [uzamsal dizinler](/sql/relational-databases/spatial/spatial-indexes-overview) de sağlar.

[Uzamsal destek](/sql/relational-databases/spatial/spatial-data-sql-server) bir çekirdek SQL Server veritabanı altyapısı özelliğidir.

## <a name="xml-features"></a>XML özellikleri

SQL Server veritabanı altyapısı, yarı yapılandırılmış veri yönetimi için zengin uygulamalar geliştirmeye yönelik güçlü bir platform sağlar. XML desteği, veritabanı altyapısının tüm bileşenleriyle tümleşiktir ve şunları içerir:

- XML veri türü. Xml değerleri, bir XML şemaları koleksiyonuna göre yazılabilecek bir XML veri türü sütununda yerel olarak depolanabilir veya sola yazılmamış olabilir. XML sütununu dizinlemek için.
- XML veri türünde sütunlarda ve değişkenlerde depolanan XML verilerinde bir XQuery sorgusu belirtme özelliği. XQuery işlevleri, veritabanınızda kullandığınız herhangi bir veri modeline erişen herhangi bir Transact-SQL sorgusunda kullanılabilir.
- [BIRINCIL XML dizinini](/sql/relational-databases/xml/xml-indexes-sql-server#primary-xml-index) kullanarak XML belgelerindeki tüm öğeleri otomatik olarak dizine alarak, [İkincil xml dizini](/sql/relational-databases/xml/xml-indexes-sql-server#secondary-xml-indexes)kullanılarak dizin oluşturulması gereken tam yolları belirtin.
- XML verilerinin toplu yüklenmesine izin veren OPENROWSET.
- İlişkisel verileri XML biçimine dönüştürün.

[XML](/sql/relational-databases/xml/xml-data-sql-server) , bir çekirdek SQL Server veritabanı altyapısı özelliğidir.

### <a name="when-to-use-an-xml-capability"></a>XML özelliği ne zaman kullanılır?

Belge modelleri, bazı belirli senaryolarda ilişkisel modeller yerine kullanılabilir:

- Tek seferde tüm nesne alanlarına erişirken veya nesnelerin normalleştirilmiş kısımlarını hiçbir zaman güncelleştirmeyeceğinden şemanın yüksek düzeyde normalleştirilmesi önemli avantajlar getirmez. Ancak, verileri almak için katılmanız gereken çok sayıda tablo olması nedeniyle, normalleştirilmiş model, sorgularınızın karmaşıklığını artırır.
- XML belgelerinin yerel olarak kullanıldığı uygulamalarla çalışıyorsunuz iletişim veya veri modelleridir ve ilişkisel verileri XML ve tam tersi dönüştüren ek katmanlar tanıtmak istemezsiniz.
- Alt tabloları veya varlık-nesne-değer düzenlerini yeniden normalleştirerek veri modelinizi basitleştirmeniz gerekir.
- Verileri çözümleyen ek bir araç olmadan XML biçiminde depolanan verileri yüklemeniz veya dışarı aktarmanız gerekir.

## <a name="key-value-pairs"></a>Anahtar-değer çiftleri

Anahtar-değer yapıları standart ilişkisel tablolar olarak yerel olarak temsil edilemeyeceği için, Azure SQL ürünlerinin, anahtar-değer çiftlerini destekleyen özel türleri veya yapıları yoktur:

```sql
CREATE TABLE Collection (
  Id int identity primary key,
  Data nvarchar(max)
)
```

Bu anahtar-değer yapısını, herhangi bir kısıtlama olmadan gereksinimlerinize uyacak şekilde özelleştirebilirsiniz. Örnek olarak, değeri türü yerine XML belgesi olabilir `nvarchar(max)` . değer JSON belgesi ise, `CHECK` JSON içeriğinin geçerliliğini doğrulayan bir kısıtlama koyabilirsiniz. Ek sütunlarda bir anahtarla ilgili herhangi bir sayıda değer koyabilirsiniz, veri erişimini basitleştirmek ve iyileştirmek için hesaplanmış sütunlar ve dizinler ekleyebilirsiniz, daha iyi performans sağlamak için tabloyu bellek/en iyileştirilmiş şema tablosu olarak tanımlayın, vb.

Bkz. BWin 'nın, bir saniyede 1.200.000 toplu işlem elde eden, bir örnek olarak anahtar-değer çifti çözümü olarak nasıl verimli bir şekilde kullanılabileceği gibi, ASP.NET önbelleğe alma çözümü için [bellek ıçı OLTP kullanma](https://blogs.msdn.microsoft.com/sqlcat/20../../how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/) .

## <a name="next-steps"></a>Sonraki adımlar

Azure SQL ürün ailesinde bulunan çok modelli özellikler ayrıca Azure SQL ürünleri ailesi arasında paylaşılan temel SQL Server veritabanı altyapısı özellikleridir. Bu özellikler hakkında daha fazla bilgi edinmek için SQL Ilişkisel veritabanı belge sayfalarını ziyaret edin:

- [Grafik işleme](/sql/relational-databases/graphs/sql-graph-overview)
- [JSON verileri](/sql/relational-databases/json/json-data-sql-server)
- [Uzamsal destek](/sql/relational-databases/spatial/spatial-data-sql-server)
- [XML verileri](/sql/relational-databases/xml/xml-data-sql-server)
