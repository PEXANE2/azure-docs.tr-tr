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
ms.openlocfilehash: ee9f2f0366cc680c874c6748d27e02dcadb82e35
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84673318"
---
# <a name="introduction-to-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. giriş

Azure Data Lake Storage 2., [Azure Blob depolama](storage-blobs-introduction.md)üzerinde oluşturulmuş, büyük veri analizi için adanmış bir yetenekler kümesidir. Data Lake Storage 2., mevcut iki depolama hizmeti, Azure Blob depolama ve Azure Data Lake Storage 1. yeteneklerini yakınsama sonucudur. Dosya sistemi semantiği, dizin ve dosya düzeyi güvenliği ve ölçeği gibi [Azure Data Lake Storage 1.](https://docs.microsoft.com/azure/data-lake-store/index)özellikler, [Azure Blob depolama](storage-blobs-introduction.md)'dan düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik/olağanüstü durum kurtarma özellikleriyle birleştirilir.

## <a name="designed-for-enterprise-big-data-analytics"></a>Kurumsal büyük veri analizi için tasarlandı

Data Lake Storage 2. Azure Storage 'ı Azure 'da kurumsal veri oluşturma için temel oluşturur. Başlangıçtan itibaren yüzlerce Gigabit işleme petabaytlarca, çok büyük miktarlarda veriyi kolay bir şekilde yönetmenize olanak tanılarken, Data Lake Storage 2. birden fazla bilgi için başlangıç sürümünden tasarlanan.

Data Lake Storage 2. temel bir parçası, blob depolamaya [hiyerarşik bir ad alanının](data-lake-storage-namespace.md) eklenmesinin bir parçasıdır. Hiyerarşik ad alanı, etkili veri erişimi için nesneleri/dosyaları bir dizin hiyerarşisi halinde düzenler. Ortak bir nesne deposu adlandırma kuralı, hiyerarşik bir dizin yapısını taklit etmek için adında eğik çizgiler kullanır. Bu yapı Data Lake Storage 2. ile gerçek hale gelir. Dizini yeniden adlandırma veya silme gibi işlemler, dizinin ad önekini paylaşan tüm nesneleri listelemek ve işlemek yerine dizinde tek Atomik meta veri işlemleri haline gelir.

Data Lake Storage 2. blob Storage üzerinde yapılar ve aşağıdaki yollarla performansı, yönetimi ve güvenliği geliştirir:

-   Verileri Analize bir önkoşul olarak kopyalamanız veya dönüştürmeniz gerekmiyorsa **performans** iyileştirilir. Blob depolamada düz ad alanı ile karşılaştırıldığında, hiyerarşik ad alanı, genel iş performansını artıran dizin yönetimi işlemlerinin performansını önemli ölçüde artırır.

-   Dosyaları dizinler ve alt dizinler aracılığıyla düzenleyebileceğiniz ve işleyebileceğiniz için **Yönetim** daha kolay hale getirir.

-   Dizinler veya tek dosyalarda POSIX izinleri tanımlayabilmeniz için **güvenlik güvenliği** uygulanabilir.

Ayrıca, düşük maliyetli [Azure Blob depolama](storage-blobs-introduction.md)alanının üzerine inşa edildiği için Data Lake Storage 2. çok maliyetli hale gelir. Ek özellikler, Azure 'da büyük veri analizlerini çalıştırmaya yönelik toplam sahip olma maliyetini daha düşüktür.

## <a name="key-features-of-data-lake-storage-gen2"></a>Data Lake Storage 2. temel özellikleri

-   **Hadoop uyumlu erişim**: Data Lake Storage 2., verileri [Hadoop Dağıtılmış dosya sistemi (bir)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)ile yaptığınız gibi yönetmenizi ve erişmenize olanak tanır. Yeni [ABFS sürücüsü](data-lake-storage-abfs-driver.md) , Data Lake Storage 2. depolanan verilere erişmek için [Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/index)*,* [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/index)ve [Azure SYNAPSE Analytics](https://docs.microsoft.com/azure/synapse-analytics) dahil olmak üzere tüm Apache Hadoop ortamlarında kullanılabilir.

-   **POSIX Izinlerinin bir üst kümesi**: Data Lake Gen2 için güvenlik modeli, Data Lake Storage 2. özgü bazı ayrıntı düzeyi Ile birlikte ACL 'LERI ve POSIX izinlerini destekler. Ayarlar, Depolama Gezgini veya Hive ve Spark gibi çerçeveler aracılığıyla yapılandırılabilir.

-   Uygun **maliyetli**: Data Lake Storage 2. düşük maliyetli depolama kapasitesi ve işlemler sunar. Tam yaşam döngüsü boyunca veri geçişleri olarak faturalandırma ücretleri, [Azure Blob depolama yaşam döngüsü](storage-lifecycle-management-concepts.md)gibi yerleşik özellikler aracılığıyla maliyetleri en düşük düzeyde tutmaya göre değişir.

-   **İyileştirilmiş sürücü**: ABFS sürücüsü özellikle büyük veri analizi için [iyileştirilmiştir](data-lake-storage-abfs-driver.md) . Karşılık gelen REST API 'Leri uç noktadan geliyor `dfs.core.windows.net` .

### <a name="scalability"></a>Ölçeklenebilirlik

Azure depolama, Data Lake Storage 2. veya blob depolama arabirimlerine erişip erişemeyeceğini tasarlayarak ölçeklenebilir. *Birçok eksabaytlarca veri*depolayıp sunabilir. Bu depolama alanı miktarı, saniye başına giriş/çıkış işlemi (ıOPS) düzeyinde Gigabit/saniye (Gbps) cinsinden ölçülen aktarım hızı ile kullanılabilir. Yalnızca kalıcılığın ötesinde, işleme hizmet, hesap ve dosya düzeylerinde ölçülen istek başına gecikme süreleriyle yürütülür.

### <a name="cost-effectiveness"></a>Maliyet uygunluğu

Azure Blob depolama alanının en üstünde Data Lake Storage 2. oluşturmanın avantajlarından biri, depolama kapasitesi ve işlemlerinin düşük maliyetlidir. Diğer bulut depolama hizmetlerinden farklı olarak, Data Lake Storage 2. ' de depolanan verilerin analiz gerçekleştirilmeden önce taşınması veya dönüştürülmesi gerekmez. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage).

Ayrıca, [hiyerarşik ad alanı](data-lake-storage-namespace.md) gibi özellikler birçok analiz işinin genel performansını önemli ölçüde artırır. Bu performans artışı, aynı miktarda veriyi işlemek için daha az işlem gücü yapmanız gerektiği anlamına gelir ve bu, uçtan uca analiz işi için daha düşük toplam sahip olma maliyeti (TCO) ile sonuçlanır.

### <a name="one-service-multiple-concepts"></a>Tek hizmet, birden çok kavram

Data Lake Storage 2., Azure Blob depolama alanı üzerinde oluşturulmuş, büyük veri analizi için ek bir özelliktir. Analiz için veri Lakes oluşturmak ve çalıştırmak üzere Blobların mevcut platform bileşenlerinin avantajlarından yararlanan birçok avantaj olsa da, bu, aynı paylaşılan şeyleri açıklayan birden çok kavram oluşmasına neden olur.

Aşağıdakiler, farklı kavramlarla açıklandığı gibi eşdeğer varlıklardır. Aksi belirtilmedikçe, bu varlıklar doğrudan eş anlamlı değildir:

| Konsept                                | Üst düzey kuruluş | Alt düzey kuruluş                                            | Veri kapsayıcısı |
|----------------------------------------|------------------------|---------------------------------------------------------------------|----------------|
| Blob 'lar – genel amaçlı nesne depolaması | Kapsayıcı              | Sanal dizin (yalnızca SDK – atomik düzenleme sağlamaz) | Blob           |
| Azure Data Lake Storage 2. – analiz depolaması          | Kapsayıcı            | Dizin                                                           | Dosya           |

## <a name="supported-blob-storage-features"></a>Desteklenen Blob depolama özellikleri

 [Tanılama günlüğü](../common/storage-analytics-logging.md), [erişim katmanları](storage-blob-storage-tiers.md)ve [BLOB depolama yaşam döngüsü yönetim ilkeleri](storage-lifecycle-management-concepts.md) gibi BLOB depolama özellikleri artık hiyerarşik bir ad alanı olan hesaplarla çalışır. Bu nedenle, bu özelliklere erişimi kaybetmeksizin BLOB depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz. 

Desteklenen BLOB depolama özelliklerinin bir listesi için, bkz. [Azure Data Lake Storage 2. bulunan BLOB Storage özellikleri](data-lake-storage-supported-blob-storage-features.md).

## <a name="supported-azure-service-integrations"></a>Desteklenen Azure hizmeti tümleştirmeleri

Data Lake Storage Gen2, verileri almak, analiz yapmak ve görsel gösterimler oluşturmak için kullanabileceğiniz çeşitli Azure hizmetlerini destekler. Desteklenen Azure hizmetlerinin listesi için bkz. [Azure Data Lake Storage 2. destekleyen Azure hizmetleri](data-lake-storage-supported-azure-services.md).

## <a name="supported-open-source-platforms"></a>Desteklenen açık kaynaklı platformlar

Birçok açık kaynak platformu Data Lake Storage 2. destekler. Tüm liste için, bkz. [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md).

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)


