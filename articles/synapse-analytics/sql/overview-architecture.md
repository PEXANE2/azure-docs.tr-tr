---
title: Azure Synapse SQL mimarisi
description: Azure Synapse SQL'in yüksek performans ve ölçeklenebilirlik elde etmek için azure depolama ile büyük ölçüde paralel işleme (MPP) nasıl birleştirdiğini öğrenin.
services: synapse-analytics
author: mlee3gsd
manager: rothja
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 570e84b3a545736aad6983c7f0d8c0f0296ca589
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431819"
---
# <a name="azure-synapse-sql-architecture"></a>Azure Synapse SQL mimarisi 

Bu makalede, Synapse SQL'in mimari bileşenleri açıklanmaktadır.

[!INCLUDE [preview](../includes/note-preview.md)]

## <a name="synapse-sql-architecture-components"></a>Synapse SQL mimari bileşenleri

Synapse SQL, birden çok düğümüzerinden verilerin hesaplamalı işlemedağıtmak için bir ölçeklendirme mimarisi yararlanır. İşlem, bilgisayarınızdaki verilerden bağımsız olarak hesaplamayı ölçeklendirmenize olanak tanıyan depolamadan ayrıdır. 

SQL havuzu için ölçek birimi, [veri ambarı birimi](resource-consumption-models.md)olarak bilinen bilgi işlem gücünün soyutlamasIdır. 

İsteğe bağlı SQL için, sunucusuz olmak, sorgu kaynak gereksinimlerini karşılamak için otomatik olarak ölçekleme yapılır. Topoloji düğümleri veya arızaları ekleyerek, kaldırarak zaman içinde değiştikçe, değişikliklere uyum sağlar ve sorgunuzun yeterli kaynağa sahip olmasını ve başarıyla bitmesini sağlar. Örneğin, aşağıdaki resimde, bir sorguyu yürütmek için 4 işlem düğümü kullanan isteğe bağlı SQL gösterilmektedir.

![Synapse SQL mimarisi](./media//overview-architecture/sql-architecture.png)

Synapse SQL düğüm tabanlı bir mimari kullanır. Uygulamalar, Synapse SQL'in tek giriş noktası olan Bir Denetim düğümüne T-SQL komutlarını bağlar ve sorun. 

SQL havuzu Denetim düğümü paralel işleme sorguları optimize etmek için MPP altyapısı kullanır ve daha sonra işlemleri paralel olarak işlerini yapmak için İşlem düğümlerine geçirir. 

SQL on-demand Control düğümü, Kullanıcı sorgusunun işlem nodlarında yürütülecek daha küçük sorgulara bölerek dağıtılmış yürütmesini optimize etmek ve düzenlemek için Dağıtılmış Sorgu İşleme (DQP) altyapısını kullanır. Her küçük sorgu görev olarak adlandırılır ve dağıtılmış yürütme birimini temsil eder. Depolamadan dosya(lar)ı okur, diğer görevlerden alınan diğer görevlerden, gruplardan veya siparişlerden elde edilen sonuçları birleştirir. 

İşlem düğümleri tüm kullanıcı verilerini Azure Depolama’da depolar ve paralel sorgular çalıştırır. Veri Taşıma Hizmeti (DMS), sorguları paralel olarak çalıştırmak ve doğru sonuçlar döndürmek için verileri düğümler arasında taşıyan, sistem düzeyindeki bir dahili hizmettir. 

Ayrılmış depolama ve işlem ile, Synapse SQL kullanırken depolama gereksinimlerinize bakılmaksızın işlem gücünün bağımsız boyutlandırılmasından yararlanabilirsiniz. SQL isteğe bağlı ölçekleme otomatik olarak yapılırken, SQL havuzu için biri şunları yapabilir:

* Verileri taşımadan, bir SQL havuzu (veri ambarı) içinde bilgi işlem gücünü büyütün veya küçültün.
* Verileri olduğu gibi bırakıp işlem kapasitesini duraklatır, böylece yalnızca depolama için ödeme yaparsınız.
* Çalışma saatleri içinde işlem kapasitesini sürdürür.

## <a name="azure-storage"></a>Azure Storage

Synapse SQL, kullanıcı verilerinizi güvende tutmak için Azure Depolama'dan yararlanır. Verileriniz Azure Depolama tarafından depolanıp yönetildiği için, depolama tüketiminiz için ayrı bir ücret ödenir. 

İsteğe bağlı SQL, veri gölünüzdeki dosyaları salt okunur şekilde sorgulamanızı sağlarken, SQL havuzu da veri yutmanızı sağlar. Veriler SQL havuzuna yutulduğunda, sistemin performansını optimize etmek için veriler **dağıtımlara** ayrılır. Tabloyu tanımlarken verileri dağıtmak için hangi parçalama deseni kullanacağınızı seçebilirsiniz. Bu parçalama desenleri desteklenir:

* Karma
* Hepsini Bir Kez Deneme
* Çoğaltma

## <a name="control-node"></a>Denetim düğümü

Kontrol düğümü mimarinin beynidir. Tüm uygulamalarla ve bağlantılarla etkileşim kuran ön uçtur. 

SQL havuzunda, MPP altyapısı paralel sorguları en iyi duruma getirmek ve koordine etmek için Denetim düğümünde çalışır. SQL havuzuna bir T-SQL sorgusu gönderdiğinizde, Denetim düğümü onu her dağıtıma paralel olarak çalışan sorgulara dönüştürür.

İsteğe bağlı SQL'de, DQP altyapısı, Kullanıcı sorgusunun dağıtılmış yürütmesini, İşlem düğümlerinde yürütülecek daha küçük sorgulara bölerek optimize etmek ve koordine etmek için Denetim düğümüüzerinde çalışır. Ayrıca, her düğüm tarafından işlenecek dosya kümeleri atar.

## <a name="compute-nodes"></a>İşlem düğümleri

İşlem düğümleri, hesaplama gücü sağlar. 

SQL havuzunda, işleme için Bilgi İşlem düğümlerine eşolarak dağıtışlar. Daha fazla işlem kaynağı için ödeme yaptığınızda, havuz dağıtımları kullanılabilir İşlem düğümlerine yeniden eşler. İşlem düğümlerinin sayısı 1 ile 60 arasında değişir ve SQL havuzunun hizmet düzeyine göre belirlenir. Her İşlem düğümünde sistem görünümlerinde görünen bir düğüm kimliği vardır. Adları sys.pdw_nodes ile başlayan sistem görünümlerinde node_id sütununa bakarak İşlem düğümü kimliğini görebilirsiniz. Bu sistem görünümlerinin bir listesi için [MPP sistem görünümlerini görün.](/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?view=azure-sqldw-latest)

İsteğe bağlı SQL'de, her Bir İşlem düğümüne görev ve görev yürütmek için dosya kümesi atanır. Görev aslında sorgu kullanıcı gönderilen bir parçası olan sorgu yürütme birimi, dağıtılır. Kullanıcı sorgusunu yürütmek için yeterli İşlem düğümünin kullanılmasını sağlamak için otomatik ölçekleme etkindir.

## <a name="data-movement-service"></a>Veri Taşıma Hizmeti

Veri Hareketi Hizmeti (DMS), Sql havuzunda, Bilgi İşlem düğümleri arasındaki veri hareketini koordine eden veri aktarım teknolojisidir. Bazı sorgular, paralel sorguların doğru sonuçları döndürmesini sağlamak için veri hareketi gerektirir. Veri hareketi gerektiğinde, DMS doğru verilerin doğru konuma ulaştığından emin olur.

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="distributions"></a>Dağıtım

Dağıtım, SQL havuzunda dağıtılmış verilerüzerinde çalışan paralel sorgular için temel depolama ve işleme birimidir. SQL havuzu bir sorgu çalıştırdığında, çalışma paralel olarak çalışan 60 küçük sorguya bölünür. 

60 küçük sorgunun her biri veri dağıtımlarından birinde çalışır. Her İşlemci düğümü 60 dağıtımdan birini veya daha fazlasını yönetir. Maksimum işlem kaynağına sahip bir SQL havuzunun Işlem düğümü başına bir dağıtımı vardır. En az işlem kaynaklarına sahip bir SQL havuzu, tek bir işlem düğümündeki tüm dağılımlara sahiptir. 

## <a name="hash-distributed-tables"></a>Karma dağıtılmış tablolar
Karma dağıtılmış bir tablo, büyük tablolarsa birleştirmeler ve toplamalar için en yüksek sorgu performansını sunabilir. 

Verileri karma dağıtılmış bir tabloya parçalamak için, SQL havuzu her satırı deterministically bir dağıtıma atamak için karma işlevi kullanır. Tablo tanımında sütunlardan biri dağıtım sütunu olarak atanır. Karma işlevi, her satırı bir dağıtıma atamak için dağıtım sütunundaki değerleri kullanır.

Aşağıdaki diyagram, tam (dağıtılmamış tablonun) karma dağıtılmış tablo olarak nasıl depolandığını göstermektedir. 

![Dağıtılmış tablo](media//overview-architecture/hash-distributed-table.png "Dağıtılmış tablo") 

* Her satır bir dağıtıma aittir. 
* Deterministik karma algoritmaher satırı bir dağıtıma atar. 
* Dağıtı başına tablo satırlarının sayısı tabloların farklı boyutlarına göre gösterildiği gibi değişir.

Bir dağıtım sütununun seçimi için fark, veri eğriliği ve sistemde çalışan sorgu türleri gibi performans konuları vardır.

## <a name="round-robin-distributed-tables"></a>Hepsini bir kez deneme dağıtılmış tabloları

Round-robin tablo oluşturmak için en basit tablo ve yükler için bir evreleme tablosu olarak kullanıldığında hızlı performans sunar.

Hepsini bir kez deneme dağıtılmış tablosu, verileri tabloda eşit olarak dağıtır ancak başka bir iyileştirme yapmaz. Bir dağılım önce rasgele seçilir ve ardından satır arabellekleri sırayla dağılımlara atanır. Verileri hepsini bir kez deneme tablosuna yüklemek hızlı bir işlemdir ancak sorgu performansı genellikle karma dağıtılmış tablolar ile daha iyi olabilir. Round-robin tablolarına katılırsa, verileri değiştirme gerekir ve bu da ek zaman alır.

## <a name="replicated-tables"></a>Çoğaltılmış tablolar
Çoğaltılmış bir tablo, küçük tablolar için en hızlı sorgu performansını sağlar.

Çoğaltılan bir tablo, her işlem düğümünde tablonun tam bir kopyasını önbelleğe alar. Sonuç olarak, bir tablo çoğaltıldığında bir birleştirme veya toplama öncesinde işlem düğümleri arasında verileri aktarma gereksinimi ortadan kalkar. Çoğaltılmış tablolar, küçük tablolar ile en iyi şekilde kullanılır. Extra storage is required and there is additional overhead that is incurred when writing data, which make large tables impractical. 

Aşağıdaki diyagram, her bir işlem düğümünde ilk dağıtımda önbelleğe alınmış çoğaltılmış bir tabloyu gösterir. 

![Çoğaltılmış tablo](media/overview-architecture/replicated-table.png "Çoğaltılmış tablo") 

## <a name="next-steps"></a>Sonraki adımlar

Synapse SQL hakkında biraz bilginiz olduğuna göre, hızlı bir şekilde [SQL havuzu oluşturmayı](../quickstart-create-sql-pool.md) ve örnek verileri nasıl [yükleyin](../sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) (./sql-data-warehouse-load-sample-databases.md). Ya da [isteğe bağlı SQL kullanmaya](../quickstart-sql-on-demand.md)başlarsınız. Azure’da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü](../../azure-glossary-cloud-terminology.md) yararlı bulabilirsiniz. 
