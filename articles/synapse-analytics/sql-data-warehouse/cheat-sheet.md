---
title: Azure SYNAPSE Analytics için bir sayfa sayfası (eski adıyla SQL DW)
description: Azure SYNAPSE Analytics (eski adıyla SQL DW) çözümlerinizi hızlıca oluşturmak için bağlantıları ve en iyi uygulamaları bulun.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql-dw
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 74ffb54b13783b4945376e1717777fa1da39ab44
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87543325"
---
# <a name="cheat-sheet-for-azure-synapse-analytics-formerly-sql-dw"></a>Azure SYNAPSE Analytics için bir sayfa sayfası (eski adıyla SQL DW)

Bu görsel sayfa, Azure SYNAPSE çözümleri oluşturmak için yardımcı ipuçları ve en iyi uygulamalar sağlar.

Aşağıdaki grafikte, veri ambarı tasarlama işlemi gösterilmektedir:

![Taslak](./media/cheat-sheet/picture-flow.png)

## <a name="queries-and-operations-across-tables"></a>Tablolardaki sorgular ve işlemler

Veri ambarınızda çalıştırılacak birincil işlemleri ve sorguları önceden bildiğinizde, bu işlemler için veri ambarı mimarinizi öncelik sırasına koyabilirsiniz. Bu sorgular ve işlemler şunları içerebilir:

* Bir veya iki olgu tablosunu boyut tabloları ile birleştirme, birleşik tabloyu filtreleme ve sonra sonuçları bir veri reyonuna ekleme.
* Olgu satışınıza yönelik büyük veya küçük güncelleştirmeler yapma.
* Tablolarınıza yalnızca veri ekleme.

İşlemlerin türlerini önceden bilmeniz, tablolarınızın tasarımını iyileştirmenize yardımcı olur.

## <a name="data-migration"></a>Veri geçişi

İlk olarak, verilerinizi [Azure Data Lake Storage](../../data-factory/connector-azure-data-lake-store.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) veya Azure Blob depolama alanına yükleyin. Daha sonra, PolyBase kullanarak verilerinizi hazırlama tablolarına yükleyin. Aşağıdaki yapılandırmayı kullanın:

| Tasarım | Öneri |
|:--- |:--- |
| Dağıtım | Hepsini Bir Kez Deneme |
| Dizinleme | Yığın |
| Bölümleme | Hiçbiri |
| Kaynak Sınıfı | largerc veya xlargerc |

[Veri geçişi](https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-data-to-azure-sql-data-warehouse-in-practice/), [veri yükleme](design-elt-data-loading.md) ve [Ayıklama, Yükleme ve Dönüştürme (ELT) işlemi](design-elt-data-loading.md) hakkında daha fazla bilgi edinin.

## <a name="distributed-or-replicated-tables"></a>Dağıtılmış veya çoğaltılmış tablolar

Tablo özelliklerini bağlı olarak aşağıdaki stratejileri kullanın:

| Tür | İdeal olduğu öğe...| İzlenmesi gereken şey...|
|:--- |:--- |:--- |
| Çoğaltılmış | * Sıkıştırmadan sonra 2 GB 'den az depolama alanı içeren bir yıldız şemasında küçük boyut tabloları (~ 5x sıkıştırması) |* Tablo üzerinde birçok yazma işlemi (INSERT, upsert, DELETE, Update gibi)<br></br>* Veri ambarı birimleri (DWU) sağlamayı sıklıkla değiştirirsiniz<br></br>* Yalnızca 2-3 sütun kullanıyorsunuz, ancak tablonuzda çok sayıda sütun vardır<br></br>* Çoğaltılan bir tabloyu dizinsiz olursunuz |
| Hepsini Bir Kez Deneme (varsayılan) | * Geçici/hazırlama tablosu<br></br> * Belirgin katılım anahtarı veya iyi aday sütun yok |* Veri taşıma nedeniyle performans yavaş |
| Karma | * Olgu tabloları<br></br>* Büyük boyut tabloları |* Dağıtım anahtarı güncelleştirilemiyor |

**İpuçları:**

* Hepsini Bir Kez Deneme ile başlayın, ancak yüksek düzeyde paralel bir mimariden yararlanmak için karma dağıtım stratejisini amaçlayın.
* Genel karma anahtarların aynı veri biçimine sahip olduğundan emin olun.
* Varchar biçiminde dağıtmayın.
* Sık birleştirme işlemleri ile bir olgu tablosuna yönelik genel karma anahtar içeren boyut tabloları karma dağıtılmış olabilir.
* Verilerdeki eğrilikleri analiz etmek için *[sys.dm_pdw_nodes_db_partition_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)* komutunu kullanın.
* Sorguların arkasındaki veri taşımalarını analiz etmek, zaman yayınını izlemek ve karıştırma işlemleri yapmak için *[sys. dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)* kullanın. Bu, dağıtım stratejinizi gözden geçirmek için faydalıdır.

[Çoğaltılmış tablolar](design-guidance-for-replicated-tables.md) ve [dağıtılmış tablolar](sql-data-warehouse-tables-distribute.md) hakkında daha fazla bilgi edinin.

## <a name="index-your-table"></a>Tablonuzu dizinleme

Dizinleme, tabloların hızlı şekilde okunması için faydalıdır. Gereksinimlerinize göre kullanabileceğiniz bir dizi benzersiz teknoloji vardır:

| Tür | İdeal olduğu öğe... | İzlenmesi gereken şey...|
|:--- |:--- |:--- |
| Yığın | * Hazırlama/geçici tablo<br></br>* Küçük aramalar içeren küçük tablolar |* Tüm arama, tam tabloyu tarar |
| Kümelenmiş dizin | * En fazla 100.000.000 satır içeren tablolar<br></br>* Büyük tablolar (100.000.000 satırdan fazla) yalnızca 1-2 sütunlu yoğun olarak kullanılır |* Çoğaltılan bir tabloda kullanılır<br></br>* İşleme göre birden çok JOIN ve Group içeren karmaşık sorgulara sahipsiniz<br></br>* Dizinli sütunlarda güncelleştirmeler yaparsınız: bellek alıyor |
| Kümelenmiş columnstore dizini (CCI) (varsayılan) | * Büyük tablolar (100.000.000 satırdan fazla) | * Çoğaltılan bir tabloda kullanılır<br></br>* Tablonuzda büyük ölçüde güncelleştirme işlemleri yaparsınız<br></br>* Tablonuzu fazla bölümleyebilirsiniz: satır grupları farklı dağıtım düğümleri ve bölümleri arasında yayılmaz |

**İpuçları:**

* Kümelenmiş bir dizin üzerinde, filtreleme için yoğun şekilde kullanılan bir sütuna kümelenmemiş bir dizin eklemek isteyebilirsiniz.
* CCI ile bir tablodaki belleği nasıl yöneteceğiniz konusunda dikkatli olun. Veri yüklediğinizde, kullanıcının (veya sorgunun) büyük bir kaynak sınıfından avantaj elde etmesini istersiniz. Kırpmadan ve çok sayıda küçük sıkıştırılmış satır grupları oluşturmaktan kaçınmaya dikkat edin.
* Gen2’de, CCI tabloları performansı en üst düzeye çıkarmak için işlem düğümlerinde yerel olarak önbelleğe alınır.
* CCI için, satır gruplarınızın sıkıştırmasının zayıf olması nedeniyle düşük performans gerçekleşebilir. Bu durumda CCI’nizi yeniden derleyin veya yeniden düzenleyin. Sıkıştırılmış satır grubu başına en az 100.000 satır istiyorsunuz. Bir satır grubunda 1 milyon satır olması idealdir.
* Artımlı yük sıklığı ve boyutuna bağlı olarak, dizinlerinizi yeniden düzenlerken veya yeniden derlerken otomatikleştirme sağlamak istersiniz. Bahar temizliği her zaman yararlıdır.
* Bir satır grubunun kırpmak istediğinizde stratejik olun. Açık satır grupları ne kadar büyüktür? Yaklaşan günlerde ne kadar veri yüklemeyi bekliyorsunuz?

[Dizinler](sql-data-warehouse-tables-index.md) hakkında daha fazla bilgi edinin.

## <a name="partitioning"></a>Bölümleme

Büyük bir olgu tablonuz (1 milyardan fazla satır) olduğunda tablonuzu bölümleyebilirsiniz. Durumların yüzde 99’unda bölüm anahtarı, tarihi temel almalıdır. Özellikle de kümelenmiş bir columnstore dizininiz olduğunda aşırı bölümleme yapmamaya dikkat edin.

ELT gerektiren hazırlama tabloları ile bölümlemeden yararlanabilirsiniz. Bu, veri yaşam döngüsü yönetimini kolaylaştırır.
Özellikle kümelenmiş bir columnstore dizininde verilerinizi aşırı bölümlememeye dikkat edin.

[Bölümler](sql-data-warehouse-tables-partition.md) hakkında daha fazla bilgi edinin.

## <a name="incremental-load"></a>Artımlı yükleme

Verilerinizi artımlı olarak yükleyecekseniz ilk olarak verilerinizi yüklemeye daha büyük kaynak sınıfları ayırdığınızdan emin olun.  Bu, kümelenmiş columnstore dizinleri olan tablolara yüklenirken özellikle önemlidir.  Daha fazla ayrıntı için bkz. [kaynak sınıfları](resource-classes-for-workload-management.md) .  

ELT işlem hatlarınızı veri ambarınıza otomatik hale getirmek için PolyBase ve ADF v2 kullanmanızı öneririz.

Geçmiş verilerinizde oluşan büyük toplu güncelleştirmeler için, INSERT, UPDATE ve DELETE kullanmak yerine bir tabloda tutmak istediğiniz verileri yazmak üzere bir [CTAS](sql-data-warehouse-develop-ctas.md) kullanmayı düşünün.

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın

 Otomatik istatistikler genel kullanıma sunulduğunda, istatistiklerin el ile Bakımı gereklidir. Verilerinizde *önemli* değişiklikler olacağından, istatistiklerin güncelleştirilmesi önemlidir. Bu, sorgu planlarınızın iyileştirilmesine yardımcı olur. Tüm istatistiklerinizi tutmanız çok uzun sürerse, hangi sütunlarda istatistikler olduğu konusunda daha seçici olun.

Güncelleştirmelerin sıklığını da tanımlayabilirsiniz. Örneğin, yeni değerlerin eklenme ihtimali olan tarih sütunlarını her gün güncelleştirmeyi tercih edebilirsiniz. En çok faydayı, birleştirmelerin bulunduğu sütunlar, WHERE yan tümcesinde kullanılan sütunlar ve GROUP BY içinde bulunan sütunlar için istatistik tutarak elde edebilirsiniz.

[İstatistikler](sql-data-warehouse-tables-statistics.md) hakkında daha fazla bilgi edinin.

## <a name="resource-class"></a>Kaynak sınıfı

Kaynak grupları, sorgulara bellek ayırmak için bir yol olarak kullanılır. Sorgu veya yükleme hızını artırmak için daha fazla bellek gerekiyorsa, daha yüksek kaynak sınıfları ayırmanız gerekir. Çevirme tarafında büyük kaynak sınıfları kullanılması, eşzamanlılığı etkiler. Tüm kullanıcılarınızı büyük bir kaynak sınıfına taşımadan önce bunu dikkate almak istersiniz.

Sorguların çok uzun sürdüğünü fark ederseniz, kullanıcılarınızın büyük kaynak sınıflarında çalışmadığından emin olun. Büyük kaynak sınıfları birçok eşzamanlı yuva kullanır. Bunlar diğer sorguların kuyruğa alınmasına neden olabilir.

Son olarak, [SQL havuzu](sql-data-warehouse-overview-what-is.md#synapse-sql-pool-in-azure-synapse)Gen2 kullanarak her kaynak sınıfı, Gen1 'den daha fazla bellek 2,5 alır.

[Kaynak sınıfları ve eşzamanlılık](resource-classes-for-workload-management.md) ile çalışma hakkında daha fazla bilgi edinin.

## <a name="lower-your-cost"></a>Maliyetinizi düşürme

Azure SYNAPSE 'ın temel bir özelliği, [işlem kaynaklarını yönetme](sql-data-warehouse-manage-compute-overview.md)olanağıdır. SQL havuzunu kullanmadığınız sırada duraklatabilirsiniz, bu da işlem kaynaklarının faturalandırmasını sonlandırır. Performans taleplerinizi karşılamak için kaynakları ölçeklendirebilirsiniz. Duraklatmak için [Azure portalını](pause-and-resume-compute-portal.md) veya [PowerShell](pause-and-resume-compute-powershell.md)’i kullanın. Ölçeklendirmek için [Azure portalını](quickstart-scale-compute-portal.md), [Powershell](quickstart-scale-compute-powershell.md)’i, [T-SQL](quickstart-scale-compute-tsql.md)’i veya [REST API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute)’sini kullanın.

Azure İşlevleri ile istediğiniz anda otomatik ölçeklendirme yapın:

[!["Azure 'a dağıt" etiketli bir düğmeyi gösteren resim.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json)

## <a name="optimize-your-architecture-for-performance"></a>Performans için mimarinizi iyileştirme

Hub ve bağlı bileşen mimarisindeki SQL Database ve Azure Analysis Services’in dikkate alınmasını öneririz. Bu çözüm, bir yandan SQL Database ve Azure Analysis Services’teki gelişmiş güvenlik özelliklerini kullanırken diğer yandan farklı kullanıcı grupları arasında iş yükü yalıtımı sağlayabilir. Bu, kullanıcılarınıza sınırsız eşzamanlılık sağlamanın da bir yoludur.

[Azure SYNAPSE 'ın avantajlarından yararlanan tipik mimariler](https://blogs.msdn.microsoft.com/sqlcat/20../../common-isv-application-patterns-using-azure-sql-data-warehouse/)hakkında daha fazla bilgi edinin.

SQL havuzundaki SQL veritabanlarındaki bağlı uçlarınıza tek bir tıklama ile dağıtın:

[!["Azure 'a dağıt" etiketli bir düğmeyi gösteren resim.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json)
