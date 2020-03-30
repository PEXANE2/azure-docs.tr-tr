---
title: Azure Veri Gölü Depolama Gen2 Giriş
description: Azure Veri Gölü Depolama Gen2'ye genel bakış sağlar
author: normesta
ms.service: storage
ms.topic: overview
ms.date: 02/25/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 2f920e29fafdc55478e0e2c16d683bd1c3bc81d8
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78942926"
---
# <a name="introduction-to-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'ye Giriş

Azure Veri Gölü Depolama Gen2, [Azure Blob depolamasına](storage-blobs-introduction.md)dayalı büyük veri analitiğine adanmış bir dizi özelliktir. Data Lake Storage Gen2, mevcut iki depolama hizmetimiz olan Azure Blob depolama ve Azure Veri Gölü Depolama Gen1'in yeteneklerini biraraya gelmenin bir sonucudur. Dosya sistemi semantikleri, dizin ve dosya düzeyi güvenliği ve ölçeği gibi [Azure Veri Gölü Depolama Gen1'inin](https://docs.microsoft.com/azure/data-lake-store/index)özellikleri, Azure [Blob depolamadan](storage-blobs-introduction.md)düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik/olağanüstü durum kurtarma özellikleriyle birleştirilir.

## <a name="designed-for-enterprise-big-data-analytics"></a>Kurumsal büyük veri analitiği için tasarlanmıştır

Data Lake Storage Gen2, Azure Depolama'yı Azure'da kurumsal veri gölleri oluşturmak için temel haline getirir. Veri Gölü Depolama Gen2, yüzlerce gigabayt lık bir iş parçasını sürdürürken, birden fazla petabayt bilgi nin hizmet vermek üzere başlangıçtan itibaren tasarlanan Veri Gölü Depolama Gen2, büyük miktarda veriyi kolayca yönetmenize olanak tanır.

Veri Gölü Depolama Gen2'nin temel bir parçası, Blob depolamasına [hiyerarşik](data-lake-storage-namespace.md) bir ad alanı eklenmesidir. Hiyerarşik ad alanı, nesneleri/dosyaları verimli veri erişimi için dizinler hiyerarşisi içinde düzenler. Ortak bir nesne deposu adlandırma kuralı, hiyerarşik bir dizin yapısını taklit etmek için addaki kesikleri kullanır. Bu yapı Data Lake Storage Gen2 ile gerçek olur. Dizin yeniden adlandırma veya silme gibi işlemler, dizinin ad önekini paylaşan tüm nesneleri sıralayıp işlemek yerine dizinde tek atomik meta veri işlemleri ne olur.

Data Lake Storage Gen2 Blob depolama sını oluşturur ve performansı, yönetimi ve güvenliği aşağıdaki yollarla artırır:

-   **Performans,** verileri çözümleme için ön koşul olarak kopyalamanız veya dönüştürmeniz gerekmediği için en iyi duruma getirilir. Blob depolamadaki düz ad alanıyla karşılaştırıldığında, hiyerarşik ad alanı genel iş performansını artıran dizin yönetimi işlemlerinin performansını büyük ölçüde artırır.

-   Dizinler ve alt dizinler aracılığıyla dosyaları düzenleyip değiştirebileceğiniz için **yönetim** daha kolaydır.

-   Dizinler veya tek tek dosyalarda POSIX izinlerini tanımlayabildiğiniz için **güvenlik** uygulanabilir.

Ayrıca, Düşük maliyetli [Azure Blob depolama](storage-blobs-introduction.md)üzerine inşa edilir, çünkü Veri Gölü Depolama Gen2 çok uygun maliyetlidir. Ek özellikler, Azure'da büyük veri analitiği çalıştırmak için toplam sahip olma maliyetini daha da düşürür.

## <a name="key-features-of-data-lake-storage-gen2"></a>Veri Gölü Depolama Gen2'nin temel özellikleri

-   **Hadoop uyumlu erişim**: Data Lake Storage Gen2, [Hadoop Dağıtılmış Dosya Sistemi (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)ile olduğu gibi verileri yönetmenize ve erişmenize olanak tanır. Yeni [ABFS sürücüsü,](data-lake-storage-abfs-driver.md) Veri Gölü Depolama Gen2'de depolanan verilere erişmek için [Azure HDInsight,](https://docs.microsoft.com/azure/hdinsight/index)*,* [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/index)ve [SQL Veri Ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/) dahil olmak üzere tüm Apache Hadoop ortamlarında kullanılabilir.

-   **POSIX izinlerinin bir üst kümesi**: Data Lake Gen2'nin güvenlik modeli, Data Lake Storage Gen2'ye özgü bazı ekstra parçalılıkla birlikte ACL ve POSIX izinlerini destekler. Ayarlar Depolama Gezgini veya Hive ve Spark gibi çerçeveler aracılığıyla yapılandırılabilir.

-   **Uygun maliyetli**: Data Lake Storage Gen2 düşük maliyetli depolama kapasitesi ve işlemleri sunar. Veriler tam kullanım ömrü boyunca geçiş yaptığından, faturalandırma oranları [Azure Blob depolama yaşam döngüsü](storage-lifecycle-management-concepts.md)gibi yerleşik özellikler aracılığıyla maliyetleri minimumda tutar.

-   **Optimize edilmiş sürücü**: ABFS sürücüsü özellikle büyük veri analitiği için [optimize edilebistir.](data-lake-storage-abfs-driver.md) İlgili REST API'leri bitiş `dfs.core.windows.net`noktasından yüzeye çıkar.

### <a name="scalability"></a>Ölçeklenebilirlik

Azure Depolama, Data Lake Storage Gen2 veya Blob depolama arabirimleri üzerinden erişin, tasarım gereği ölçeklenebilir. *Birçok eksabayt veriyi*saklayıp servis edebiliyor. Bu depolama miktarı, saniyede gigalet cinsinden ölçülen iş verimi (Gbps) ile saniyede yüksek düzeyde giriş/çıkış işlemlerinde (IOPS) kullanılabilir. Sadece kalıcılığın ötesinde, işleme hizmet, hesap ve dosya düzeylerinde ölçülen istek başına neredeyse sabit gecikmelerde yürütülür.

### <a name="cost-effectiveness"></a>Maliyet uygunluğu

Azure Blob depolamasının üstüne Veri Gölü Depolama Gen2 oluşturmanın birçok avantajından biri, depolama kapasitesinin ve işlemlerinin düşük maliyetidir. Diğer bulut depolama hizmetlerinin aksine, Veri Gölü Depolama Gen2'de depolanan verilerin, analiz gerçekleştirmeden önce taşınması veya dönüştürülmesi gerekmez. Fiyatlandırma hakkında daha fazla bilgi için Azure [Depolama fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage)bakın.

Ayrıca, [hiyerarşik ad alanı](data-lake-storage-namespace.md) gibi özellikler birçok analitik işin genel performansını önemli ölçüde artırır. Performanstaki bu iyileşme, aynı miktarda veriyi işlemek için daha az bilgi işlem gücüne ihtiyaç duyduğunuz ve uçtan uca analiz işi için toplam toplam sahip olma maliyetinin (TCO) daha düşük bir maliyetle sonuçlandığı anlamına gelir.

### <a name="one-service-multiple-concepts"></a>Tek bir hizmet, birden çok kavram

Data Lake Storage Gen2, Azure Blob depolamasının üzerine inşa edilmiş büyük veri analitiği için ek bir özelliktir. Analiz için veri gölleri oluşturmak ve işletmek için Blobs'un mevcut platform bileşenlerinden yararlanmanın birçok faydası olmakla birlikte, aynı paylaşılan şeyleri açıklayan birden çok kavrama yol açar.

Farklı kavramlarla açıklandığı gibi, aşağıdaki eşdeğer varlıklar vetlerdir. Aksi belirtilmedikçe bu varlıklar doğrudan eşanlamlıdır:

| Kavram                                | Üst Düzey Organizasyon | Alt Düzey Organizasyon                                            | Veri Kapsayıcısı |
|----------------------------------------|------------------------|---------------------------------------------------------------------|----------------|
| Blobs – Genel amaçlı nesne depolama | Kapsayıcı              | Sanal dizin (Sadece SDK – atomik manipülasyon sağlamaz) | Blob           |
| Azure Veri Gölü Depolama Gen2 – Analitik Depolama          | Kapsayıcı            | Dizin                                                           | Dosya           |

## <a name="supported-blob-storage-features"></a>Desteklenen Blob depolama özellikleri

 [Tanılama günlüğü,](../common/storage-analytics-logging.md) [erişim katmanları](storage-blob-storage-tiers.md)ve Blob Depolama yaşam döngüsü yönetim ilkeleri gibi [Blob](storage-lifecycle-management-concepts.md) depolama özellikleri artık hiyerarşik ad alanına sahip hesaplarla çalışır. Bu nedenle, bu özelliklere erişimi kaybetmeden Blob depolama hesaplarınızda hiyerarşik ad alanlarını etkinleştirebilirsiniz. 

Desteklenen Blob depolama özelliklerinin listesi için Azure [Veri Gölü Depolama Gen2'de bulunan Blob Depolama özelliklerine](data-lake-storage-supported-blob-storage-features.md)bakın.

## <a name="supported-azure-service-integrations"></a>Desteklenen Azure hizmet tümleştirmeleri

Veri Gölü Depolama gen2, verileri yutmak, analitik gerçekleştirmek ve görsel gösterimler oluşturmak için kullanabileceğiniz birkaç Azure hizmetlerini destekler. Desteklenen Azure hizmetlerinin listesi için, [Azure Veri Gölü Depolama Gen2'yi destekleyen Azure hizmetlerine](data-lake-storage-supported-azure-services.md)bakın.

## <a name="supported-open-source-platforms"></a>Desteklenen açık kaynak platformları

Çeşitli açık kaynak platformları Data Lake Storage Gen2'yi destekler. Tam liste için bkz: [Azure Veri Gölü Depolama Gen2'yi destekleyen Açık kaynak platformları.](data-lake-storage-supported-open-source-platforms.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Veri Gölü Depolama Gen2 ile bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Veri Gölü Depolama'da çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)


