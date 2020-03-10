---
title: Azure Data Lake Storage 2. giriş
description: Azure Data Lake Storage 2. bir genel bakış sağlar
author: normesta
ms.service: storage
ms.topic: overview
ms.date: 02/25/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 2f920e29fafdc55478e0e2c16d683bd1c3bc81d8
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78942926"
---
# <a name="introduction-to-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. giriş

Azure Data Lake Storage 2., [Azure Blob depolama](storage-blobs-introduction.md)üzerinde oluşturulmuş, büyük veri analizi için adanmış bir yetenekler kümesidir. Data Lake depolama Gen2 bizim iki var olan depolama hizmetleri, Azure Blob Depolama ve Azure Data Lake depolama Gen1 yeteneklerini yakınsamaya sonucudur. Dosya sistemi semantiği, dizin ve dosya düzeyi güvenliği ve ölçeği gibi [Azure Data Lake Storage 1.](https://docs.microsoft.com/azure/data-lake-store/index)özellikler, [Azure Blob depolama](storage-blobs-introduction.md)'dan düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik/olağanüstü durum kurtarma özellikleriyle birleştirilir.

## <a name="designed-for-enterprise-big-data-analytics"></a>Kurumsal büyük veri analizi için tasarlanmış

Data Lake depolama 2. nesil, kurumsal veri gölleri Azure'da oluşturmaya yönelik temel Azure depolama sağlar. Aktarım hızının Gigabit yüzlerce dayanıklılık geçtikten bilgilerini petabaytlarca birden çok hizmet vermek için baştan tasarlanan Data Lake depolama Gen2 oldukça büyük miktardaki verileri kolayca yönetmenize olanak sağlar.

Data Lake Storage 2. temel bir parçası, blob depolamaya [hiyerarşik bir ad alanının](data-lake-storage-namespace.md) eklenmesinin bir parçasıdır. Hiyerarşik ad alanı nesneler/dosya dizinleri verimli veri erişimi için bir hiyerarşi halinde düzenler. Ortak bir nesne deposu adlandırma kuralı, bir hiyerarşik dizin yapısını taklit edecek şekilde adlarında eğik çizgi kullanır. Bu yapı ile Data Lake depolama Gen2 gerçek haline gelir. Bir dizini silme veya yeniden adlandırma gibi işlemler atomik meta veri işlemleri dizin yerine numaralandırma ve dizin adı öneki paylaşan tüm nesneleri işleme haline gelir.

Data Lake Storage 2. blob Storage üzerinde yapılar ve aşağıdaki yollarla performansı, yönetimi ve güvenliği geliştirir:

-   Verileri Analize bir önkoşul olarak kopyalamanız veya dönüştürmeniz gerekmiyorsa **performans** iyileştirilir. Blob depolamada düz ad alanı ile karşılaştırıldığında, hiyerarşik ad alanı, genel iş performansını artıran dizin yönetimi işlemlerinin performansını önemli ölçüde artırır.

-   Dosyaları dizinler ve alt dizinler aracılığıyla düzenleyebileceğiniz ve işleyebileceğiniz için **Yönetim** daha kolay hale getirir.

-   Dizinler veya tek dosyalarda POSIX izinleri tanımlayabilmeniz için **güvenlik güvenliği** uygulanabilir.

Ayrıca, düşük maliyetli [Azure Blob depolama](storage-blobs-introduction.md)alanının üzerine inşa edildiği için Data Lake Storage 2. çok maliyetli hale gelir. Ek özellikleri daha düşük maliyetli sahipliği büyük veri analizini Azure'da çalıştırmaya yönelik daha fazla.

## <a name="key-features-of-data-lake-storage-gen2"></a>Data Lake depolama Gen2 temel özellikleri

-   **Hadoop uyumlu erişim**: Data Lake Storage 2., verileri [Hadoop Dağıtılmış dosya sistemi (bir)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)ile yaptığınız gibi yönetmenizi ve erişmenize olanak tanır. Yeni [ABFS sürücüsü](data-lake-storage-abfs-driver.md) , Data Lake Storage 2. depolanan verilere erişmek için [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/index) *,* [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/index)ve [SQL veri ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/) dahil olmak üzere tüm Apache Hadoop ortamlarında kullanılabilir.

-   **POSIX Izinlerinin bir üst kümesi**: Data Lake Gen2 için güvenlik modeli, Data Lake Storage 2. özgü bazı ayrıntı düzeyi Ile birlikte ACL 'LERI ve POSIX izinlerini destekler. Ayarları, Depolama Gezgini veya Hive ve Spark gibi çerçeveleri aracılığıyla yapılandırılabilir.

-   Uygun **maliyetli**: Data Lake Storage 2. düşük maliyetli depolama kapasitesi ve işlemler sunar. Tam yaşam döngüsü boyunca veri geçişleri olarak faturalandırma ücretleri, [Azure Blob depolama yaşam döngüsü](storage-lifecycle-management-concepts.md)gibi yerleşik özellikler aracılığıyla maliyetleri en düşük düzeyde tutmaya göre değişir.

-   **İyileştirilmiş sürücü**: ABFS sürücüsü özellikle büyük veri analizi için [iyileştirilmiştir](data-lake-storage-abfs-driver.md) . Karşılık gelen REST API 'Leri, uç nokta `dfs.core.windows.net`üzerinden ortaya çıkmış.

### <a name="scalability"></a>Ölçeklenebilirlik

Azure depolama tasarım gereği ölçeklenebilir olup Data Lake depolama Gen2'ye veya Blob Depolama arabirimleri erişin. *Birçok eksabaytlarca veri*depolayıp sunabilir. Bu depolama miktarını en yüksek düzeylerde giriş/çıkış işlemi (IOPS) saniye başına Gigabit / saniye (Gbps) cinsinden ölçülen aktarım hızı ile kullanılabilir. Yalnızca Kalıcılık hizmeti, hesap ve dosya düzeyinde ölçülen her istek için sabit yakın gecikmeleri, işleme yürütülür.

### <a name="cost-effectiveness"></a>Ekonomi

Düşük maliyetli depolama kapasitesi ve işlemleri Azure Blob Depolama üzerine Data Lake depolama Gen2 oluşturmanın birçok avantaj biridir. Diğer bulut Depolama hizmetlerinin aksine, Data Lake depolama 2. nesil'deki depolanan verileri taşınamaz veya Analiz gerçekleştirmeden önce dönüştürülmüş için gerekli değildir. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage).

Ayrıca, [hiyerarşik ad alanı](data-lake-storage-namespace.md) gibi özellikler birçok analiz işinin genel performansını önemli ölçüde artırır. Bu performans artışı uçtan uca analizi işi için bir alt toplam sahip olma maliyetini (TCO) bunun sonucunda aynı miktarda veri işlemek için daha az işlem gücü gerektiren anlamına gelir.

### <a name="one-service-multiple-concepts"></a>Bir hizmet, birden çok kavramları

Data Lake depolama Gen2, büyük veri analizi, Azure Blob Depolama üzerine kurulu için ek bir özelliktir. Varken birçok Avantajdan yararlanarak mevcut platform bileşenleri oluşturmak ve analiz için veri gölleri işletmek için BLOB içinde aynı, paylaşılan öğeleri açıklayan birden çok kavramlara neden olmaz.

Eşdeğer varlıklar farklı kavramları açıklandığı aşağıda verilmiştir. Bu varlıkların doğrudan eşanlamlı aksi belirtilmediği sürece:

| Kavram                                | Üst düzey kuruluş | Daha düşük bir düzenleme düzeyi                                            | Veri kapsayıcısı |
|----------------------------------------|------------------------|---------------------------------------------------------------------|----------------|
| BLOB'lar-genel amaçlı nesne depolama | Kapsayıcı              | Sanal dizin (SDK'sı yalnızca – sağlamaz atomik işleme) | Blob           |
| Azure Data Lake Storage 2. – analiz depolaması          | Kapsayıcı            | Dizin                                                           | Dosya           |

## <a name="supported-blob-storage-features"></a>Desteklenen BLOB depolama özellikleri

 [Tanılama günlüğü](../common/storage-analytics-logging.md), [erişim katmanları](storage-blob-storage-tiers.md)ve [BLOB depolama yaşam döngüsü yönetim ilkeleri](storage-lifecycle-management-concepts.md) gibi BLOB depolama özellikleri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu nedenle, bu özelliklere erişimi kaybetmeksizin BLOB depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz. 

Desteklenen BLOB depolama özelliklerinin bir listesi için, bkz. [Azure Data Lake Storage 2. bulunan BLOB Storage özellikleri](data-lake-storage-supported-blob-storage-features.md).

## <a name="supported-azure-service-integrations"></a>Desteklenen Azure hizmeti tümleştirmeleri

Data Lake Storage Gen2, verileri almak, analiz yapmak ve görsel gösterimler oluşturmak için kullanabileceğiniz çeşitli Azure hizmetlerini destekler. Desteklenen Azure hizmetlerinin listesi için bkz. [Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md).

## <a name="supported-open-source-platforms"></a>Desteklenen açık kaynaklı platformlar

Data Lake depolama Gen2'ye birden fazla açık kaynak platformları destekler. Tüm liste için, bkz. [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md).

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)


