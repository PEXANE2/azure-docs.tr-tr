---
title: Azure Synapse Analytics için hile sayfası (eski adıyla SQL DW)
description: Azure Synapse Analytics (eski adıyla SQL DW) çözümlerinizi hızlı bir şekilde oluşturmak için bağlantılar ve en iyi uygulamaları bulun.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 8e0515727c2155b91f18398bd9def700f4a15b34
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619401"
---
# <a name="cheat-sheet-for-azure-synapse-analytics-formerly-sql-dw"></a>Azure Synapse Analytics için hile sayfası (eski adıyla SQL DW)

Bu hile sayfası, Azure Synapse çözümleri oluşturmak için yararlı ipuçları ve en iyi uygulamalar sağlar. 

Aşağıdaki grafikte, veri ambarı tasarlama işlemi gösterilmektedir:

![Taslak](./media/cheat-sheet/picture-flow.png)

## <a name="queries-and-operations-across-tables"></a>Tablolardaki sorgular ve işlemler

Veri ambarınızda çalıştırılacak birincil işlemleri ve sorguları önceden bildiğinizde, bu işlemler için veri ambarı mimarinizi öncelik sırasına koyabilirsiniz. Bu sorgular ve işlemler şunları içerebilir:
* Bir veya iki olgu tablosunu boyut tabloları ile birleştirme, birleşik tabloyu filtreleme ve sonra sonuçları bir veri reyonuna ekleme.
* Olgu satışınıza yönelik büyük veya küçük güncelleştirmeler yapma.
* Tablolarınıza yalnızca veri ekleme.

İşlemlerin türlerini önceden bilmeniz, tablolarınızın tasarımını iyileştirmenize yardımcı olur.

## <a name="data-migration"></a>Veri geçişi

İlk olarak, verilerinizi [Azure Veri Gölü Depolama](../../data-factory/connector-azure-data-lake-store.md) veya Azure Blob Depolama'ya yükleyin. Ardından, verilerinizi hazırlama tablolarına yüklemek için PolyBase'i kullanın. Aşağıdaki yapılandırmayı kullanın:

| Tasarım | Öneri |
|:--- |:--- |
| Dağıtım | Hepsini Bir Kez Deneme |
| Dizin Oluşturma | Yığın |
| Bölümleme | None |
| Kaynak Sınıfı | largerc veya xlargerc |

[Veri geçişi](https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-data-to-azure-sql-data-warehouse-in-practice/), [veri yükleme](design-elt-data-loading.md) ve [Ayıklama, Yükleme ve Dönüştürme (ELT) işlemi](design-elt-data-loading.md) hakkında daha fazla bilgi edinin. 

## <a name="distributed-or-replicated-tables"></a>Dağıtılmış veya çoğaltılmış tablolar

Tablo özelliklerini bağlı olarak aşağıdaki stratejileri kullanın:

| Tür | İdeal olduğu öğe...| İzlenmesi gereken şey...|
|:--- |:--- |:--- |
| Çoğaltılmış | * Sıkıştırma sonrası 2 GB'dan az depolama alanı olan bir yıldız şemaküçük boyutlu tablolar (~ 5x sıkıştırma) |* Birçok yazma işlemleri tabloda (ekle, yukarı, silme, güncelleme gibi)<br></br>* Veri Ambarı Birimlerini (DWU) sık sık değiştirirsiniz<br></br>* Sadece 2-3 sütun kullanın ama tablobirçok sütun var<br></br>* Çoğaltılan bir tabloyu dizine dizin |
| Hepsini Bir Kez Deneme (varsayılan) | * Geçici/evreleme tablosu<br></br> * Hiçbir bariz katılma anahtarı veya iyi aday sütun |* Performans veri hareketi nedeniyle yavaş |
| Karma | * Olgu tabloları<br></br>* Büyük boyutlu tablolar |* Dağıtım anahtarı güncelleştirilemez |

**İpuçları:**
* Hepsini Bir Kez Deneme ile başlayın, ancak yüksek düzeyde paralel bir mimariden yararlanmak için karma dağıtım stratejisini amaçlayın.
* Genel karma anahtarların aynı veri biçimine sahip olduğundan emin olun.
* Varchar formatında dağıtmayın.
* Sık birleştirme işlemleri ile bir olgu tablosuna yönelik genel karma anahtar içeren boyut tabloları karma dağıtılmış olabilir.
* Verilerdeki eğrilikleri analiz etmek için *[sys.dm_pdw_nodes_db_partition_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql)* komutunu kullanın.
* Sorguların ardındaki veri hareketlerini analiz etmek, yayın ve karıştırma işlemlerinin aldığı süreyi izlemek için *[sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql)* komutunu kullanın. Bu, dağıtım stratejinizi gözden geçirmek için faydalıdır.

[Çoğaltılmış tablolar](design-guidance-for-replicated-tables.md) ve [dağıtılmış tablolar](sql-data-warehouse-tables-distribute.md) hakkında daha fazla bilgi edinin.

## <a name="index-your-table"></a>Tablonuzu dizinleme

Dizinleme, tabloların hızlı şekilde okunması için faydalıdır. Gereksinimlerinize göre kullanabileceğiniz bir dizi benzersiz teknoloji vardır:

| Tür | İdeal olduğu öğe... | İzlenmesi gereken şey...|
|:--- |:--- |:--- |
| Yığın | * Evreleme/geçici tablo<br></br>* Küçük aramalar ile küçük tablolar |* Herhangi bir arama tam tablo tarar |
| Kümelenmiş dizin | * 100 milyon satıra kadar olan tablolar<br></br>* Büyük tablolar (100 milyondan fazla satır) sadece 1-2 sütun ağır kullanılan |* Çoğaltılmış bir tabloda kullanılır<br></br>* Birden çok birleştirme ve Grup By işlemlerini içeren karmaşık sorgularınız var<br></br>* Dizinlenen sütunlarda güncellemeler yaparsınız: bellek alır |
| Kümelenmiş columnstore dizini (CCI) (varsayılan) | * Büyük tablolar (100 milyondan fazla satır) | * Çoğaltılmış bir tabloda kullanılır<br></br>* Masanızda büyük güncelleme işlemleri yaparsınız<br></br>* Tablonuza aşırı bölme: satır grupları farklı dağıtım düğümleri ve bölümleri arasında yayılmaz |

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

Verilerinizi artımlı olarak yükleyecekseniz ilk olarak verilerinizi yüklemeye daha büyük kaynak sınıfları ayırdığınızdan emin olun.  Bu, kümelenmiş sütun deposu dizinleri içeren tablolara yüklenirken özellikle önemlidir.  Daha fazla ayrıntı için [kaynak sınıfları'na](resource-classes-for-workload-management.md) bakın.  

ELT ardışık hatlarınızı veri ambarınıza otomatikleştirmek için PolyBase ve ADF V2 kullanmanızı öneririz.

Geçmiş verilerinizdeki büyük bir güncelleştirme grubu için, INSERT, UPDATE ve DELETE kullanmak yerine tabloda tutmak istediğiniz verileri yazmak için [CTAS](sql-data-warehouse-develop-ctas.md) kullanmayı düşünün.

## <a name="maintain-statistics"></a>İstatistiklerin bakımını yapın
 Otomatik istatistikler genel olarak kullanılabilir olana kadar, istatistiklerin manuel olarak bakımı gereklidir. Verilerinizde *önemli* değişiklikler olacağından, istatistiklerin güncelleştirilmesi önemlidir. Bu, sorgu planlarınızın iyileştirilmesine yardımcı olur. Tüm istatistiklerinizi tutmanız çok uzun sürerse, hangi sütunlarda istatistikler olduğu konusunda daha seçici olun. 

Güncelleştirmelerin sıklığını da tanımlayabilirsiniz. Örneğin, yeni değerlerin eklenme ihtimali olan tarih sütunlarını her gün güncelleştirmeyi tercih edebilirsiniz. En çok faydayı, birleştirmelerin bulunduğu sütunlar, WHERE yan tümcesinde kullanılan sütunlar ve GROUP BY içinde bulunan sütunlar için istatistik tutarak elde edebilirsiniz.

[İstatistikler](sql-data-warehouse-tables-statistics.md) hakkında daha fazla bilgi edinin.

## <a name="resource-class"></a>Kaynak sınıfı
Kaynak grupları sorgulara bellek ayırmak için bir yol olarak kullanılır. Sorgu veya yükleme hızını artırmak için daha fazla bellek gerekiyorsa, daha yüksek kaynak sınıfları ayırmanız gerekir. Çevirme tarafında büyük kaynak sınıfları kullanılması, eşzamanlılığı etkiler. Tüm kullanıcılarınızı büyük bir kaynak sınıfına taşımadan önce bunu dikkate almak istersiniz.

Sorguların çok uzun sürdüğünü fark ederseniz, kullanıcılarınızın büyük kaynak sınıflarında çalışmadığından emin olun. Büyük kaynak sınıfları birçok eşzamanlı yuva kullanır. Bunlar diğer sorguların kuyruğa alınmasına neden olabilir.

Son olarak, SQL [havuzunun](sql-data-warehouse-overview-what-is.md#synapse-sql-pool-in-azure-synapse)Gen2'sini kullanarak, her kaynak sınıfı Gen1'den 2,5 kat daha fazla bellek alır.

[Kaynak sınıfları ve eşzamanlılık](resource-classes-for-workload-management.md) ile çalışma hakkında daha fazla bilgi edinin.

## <a name="lower-your-cost"></a>Maliyetinizi düşürme
Azure Synapse'nin önemli bir [özelliği, hesaplama kaynaklarını yönetebilme](sql-data-warehouse-manage-compute-overview.md)yeteneğidir. Kullanmadığınızda SQL havuzunu duraklatabilirsiniz, bu da işlem kaynaklarının faturalandırmasını durdurur. Performans taleplerinizi karşılamak için kaynakları ölçeklendirebilirsiniz. Duraklatmak için [Azure portalını](../../synapse-analytics/sql-data-warehouse/pause-and-resume-compute-portal.md) veya [PowerShell](../../synapse-analytics/sql-data-warehouse/pause-and-resume-compute-powershell.md)’i kullanın. Ölçeklendirmek için [Azure portalını](quickstart-scale-compute-portal.md), [Powershell](quickstart-scale-compute-powershell.md)’i, [T-SQL](quickstart-scale-compute-tsql.md)’i veya [REST API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute)’sini kullanın.


Azure Synapse'nin önemli bir [özelliği, hesaplama kaynaklarını yönetebilme](sql-data-warehouse-manage-compute-overview.md)yeteneğidir. Kullanmadığınızda SQL havuzunu duraklatabilirsiniz, bu da işlem kaynaklarının faturalandırmasını durdurur. Performans taleplerinizi karşılamak için kaynakları ölçeklendirebilirsiniz. Duraklatmak için [Azure portalını](../../sql-data-warehouse/pause-and-resume-compute-portal.md) veya [PowerShell](../../sql-data-warehouse/pause-and-resume-compute-powershell.md)’i kullanın. Ölçeklendirmek için [Azure portalını](quickstart-scale-compute-portal.md), [Powershell](quickstart-scale-compute-powershell.md)’i, [T-SQL](quickstart-scale-compute-tsql.md)’i veya [REST API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute)’sini kullanın.

Azure İşlevleri ile istediğiniz anda otomatik ölçeklendirme yapın:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-your-architecture-for-performance"></a>Performans için mimarinizi iyileştirme

Hub ve bağlı bileşen mimarisindeki SQL Database ve Azure Analysis Services’in dikkate alınmasını öneririz. Bu çözüm, bir yandan SQL Database ve Azure Analysis Services’teki gelişmiş güvenlik özelliklerini kullanırken diğer yandan farklı kullanıcı grupları arasında iş yükü yalıtımı sağlayabilir. Bu, kullanıcılarınıza sınırsız eşzamanlılık sağlamanın da bir yoludur.

[Azure Synapse'den yararlanan tipik mimariler](https://blogs.msdn.microsoft.com/sqlcat/20../../common-isv-application-patterns-using-azure-sql-data-warehouse/)hakkında daha fazla bilgi edinin.

SQL havuzundan SQL veritabanlarında sözcülerinizi tek bir tıklamayla dağıtın:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>