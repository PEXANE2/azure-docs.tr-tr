---
title: Ana bilgisayar depolama alanını Azure Depolama alanına taşıma
description: Büyük ölçüde ölçeklenebilir Azure depolama kaynakları, ana bilgisayar tabanlı kuruluşların IBM z14 uygulamalarını geçirmelerine ve modernize lerine yardımcı olabilir.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: storage
ms.openlocfilehash: 86419811cdf2c11204caae0ca5bf6f65fba063d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76288924"
---
# <a name="move-mainframe-storage-to-azure"></a>Ana bilgisayar depolama alanını Azure'a taşıma

Microsoft Azure'da ana bilgisayar iş yüklerini çalıştırmak için ana bilgisayarınızın yeteneklerinin Azure ile nasıl karşılaştırılacağını bilmeniz gerekir. Büyük ölçüde ölçeklenebilir depolama kaynakları, kuruluşların güvendikleri uygulamaları terk etmeden modernizasyonu başlatmalarına yardımcı olabilir.

Azure, IBM z14 tabanlı sistemlerle karşılaştırılabilir ana bilgisayar benzeri özellikler ve depolama kapasitesi sağlar (bu yazıyla ilgili en güncel model). Bu makalede, Azure'da karşılaştırılabilir sonuçlar elde etme şeklinizi anlatabilirsiniz.

## <a name="mainframe-storage-at-a-glance"></a>Bir bakışta ana bilgisayar depolama

IBM ana bilgisayarı depolamayı iki şekilde karakterize eder. Bunlardan ilki doğrudan erişim depolama aygıtıdır (DASD). İkincisi sıralı depolama. Depolamayı yönetmek için ana bilgisayar, Veri Tesisi Depolama Yönetimi Alt Sistemini (DFSMS) sağlar. Çeşitli depolama aygıtlarına veri erişimini yönetir.

[DASD,](https://en.wikipedia.org/wiki/Direct-access_storage_device) verilere doğrudan erişim için benzersiz bir adresin kullanılmasına izin veren ikincil (bellek içi olmayan) depolama için ayrı bir aygıt anlamına gelir. Başlangıçta, DASD terimi dönen disklere, manyetik tamburlara veya veri hücrelerine uygulanır. Ancak, artık bu terim katı hal depolama aygıtları (SSD'ler), depolama alanı ağları (SN'ler), ağa bağlı depolama (NAS) ve optik sürücüler için de geçerli olabilir. Bu belgenin amaçları doğrultusunda DASD dönen diskler, SN'ler ve SSD'ler anlamına gelir.

DASD depolamanın aksine, ana bilgisayardaki sıralı depolama, verilere başlangıç noktasından erişilen, ardından bir satırda okunduğu veya yazıldığı teyp sürücüleri gibi aygıtları ifade eder.

Depolama aygıtları genellikle bir fiber bağlantı (FICON) kullanılarak takılır veya hipervizörlü bir sunucudaki bölümler arasında yüksek hızlı iletişim için bir IBM teknolojisi olan [HiperSockets](https://www.ibm.com/support/knowledgecenter/zosbasics/com.ibm.zos.znetwork/znetwork_85.htm)kullanılarak doğrudan ana bilgisayarın IO veri yolundan erişilir.

Çoğu ana bilgisayar sistemi depolamayı iki türe ayırır:

- *Günlük* işlemler için çevrimiçi depolama (sıcak depolama alanı olarak da bilinmesi) gereklidir. DASD depolama genellikle bu amaç için kullanılır. Ancak, günlük teyp yedeklemeleri (mantıksal veya fiziksel) gibi sıralı depolama da bu amaç için kullanılabilir.

- *Arşiv depolama* (soğuk depolama olarak da bilinir) belirli bir zamanda monte edilmesi garanti edilmez. Bunun yerine, monte edilir ve gerektiği gibi erişilir. Arşiv depolama genellikle depolama için sıralı teyp yedekleri (mantıksal veya fiziksel) kullanılarak uygulanır.

## <a name="mainframe-versus-io-latency-and-iops"></a>Ana bilgisayar ve IO gecikme süresi ve IOPS

Ana bilgisayarlar genellikle yüksek performanslı IO ve düşük IO gecikme süresi gerektiren uygulamalar için kullanılır. Bunu IO aygıtlarına ve HiperSockets'e FICON bağlantılarını kullanarak yapabilirler. HiperSockets uygulamaları ve cihazları doğrudan bir ana bilgisayarın IO kanalına bağlamak için kullanıldığında, mikrosaniyelerde gecikme süresi elde edilebilir.

## <a name="azure-storage-at-a-glance"></a>Bir bakışta Azure depolama

Hizmet olarak azure altyapısı[(IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)depolama seçenekleri karşılaştırılabilir ana bilgisayar kapasitesi sağlar.

Microsoft, Azure'da barındırılan uygulamalar için petabayt değerinde depolama alanı sunar ve çeşitli depolama seçenekleriniz vardır. Bunlar, yüksek performans için SSD depolamadan, yüksek depolama ve arşivler için düşük maliyetli blob depolamaya kadar çeşitlilik sağlar. Ayrıca, Azure depolama için bir veri artıklığı seçeneği sağlar ve bu da bir ana bilgisayar ortamında kurulum için daha fazla çaba gerektirir.

Azure depolama, aşağıdaki tabloda özetlendiği gibi [Azure Diskleri,](/azure/virtual-machines/windows/managed-disks-overview) [Azure Dosyaları](/azure/storage/files/storage-files-introduction)ve [Azure Blobs](/azure/storage/blobs/storage-blobs-overview) olarak kullanılabilir. [Her birinin ne zaman kullanılacağı](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks)hakkında daha fazla bilgi edinin.

<!-- markdownlint-disable MD033 -->

<table>
<thead>
    <tr><th>Tür</th><th>Açıklama</th><th>İstediğince kullanın:</th></tr>
</thead>
<tbody>
<tr><td>Azure Dosyaları
</td>
<td>
SMB arabirimi, istemci kitaplıkları ve depolanan dosyalara her yerden erişim sağlayan bir <a href="https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api">REST</a> arabirimi sağlar.
</td>
<td><ul>
<li>Uygulama, azure'da çalışan diğer uygulamalarla verileri paylaşmak için yerel dosya sistemi API'lerini kullandığında bir uygulamayı kaldırın ve buluta kaydırın.</li>
<li>Birçok VM'den erişilmesi gereken geliştirme ve hata ayıklama araçlarını depolayın.</li>
</ul>
</td>
</tr>
<tr><td>Azure Blobları
</td>
<td>İstemci kitaplıkları ve yapılandırılmamış verilerin blok lekelerinde büyük bir ölçekte depolanıp erişilmesine olanak tanıyan bir <a href="https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api">REST</a> arabirimi sağlar. Ayrıca kurumsal büyük veri analizi çözümleri için <a href="/azure/storage/blobs/data-lake-storage-introduction">Azure Veri Gölü Depolama Gen2'yi</a> de destekler.
</td>
<td><ul>
<li>Bir uygulamada akış ve rasgele erişim senaryolarını destekleyin.</li>
<li>Uygulama verilerine her yerden erişin.</li>
<li>Azure'da kurumsal veri gölü oluşturun ve büyük veri analitiği gerçekleştirin.</li>
</ul></td>
</tr>
<tr><td>Azure Diskleri
</td>
<td>İstemci kitaplıkları ve verilerin kalıcı olarak depolanıp ekli bir sanal sabit diskten erişilmesine olanak tanıyan bir <a href="https://docs.microsoft.com/rest/api/compute/disks">REST</a> arabirimi sağlar.
</td>
<td><ul>
<li>Kalıcı disklere veri okumak ve yazmak için yerel dosya sistemi API'lerini kullanan uygulamaları kaldırın ve kaydırın.</li>
<li>Diskin bağlı olduğu VM'nin dışından erişilmesi gerekmeyen verileri depolayın.</li>
</ul></td>
</tr>
</tbody>
</table>
<!-- markdownlint-enable MD033 -->

## <a name="azure-hot-online-and-cold-archive-storage"></a>Azure sıcak (çevrimiçi) ve soğuk (arşiv) depolama

Belirli bir sistem için depolama türü, depolama boyutu, iş ortası ve IOPS dahil olmak üzere sistemin gereksinimlerine bağlıdır. Ana bilgisayardaki DASD tipi depolama için Azure'daki uygulamalar genellikle Azure Diskleri sürücü depolama alanını kullanır. Ana bilgisayar arşiv depolamaiçin Azure'da blob depolama kullanılır.

SSD'ler Azure'daki en yüksek depolama performansını sağlar. Aşağıdaki seçenekler mevcuttur (bu belgenin yazılması ndan itibaren):

| Tür         | Boyut           | IOPS                  |
|--------------|----------------|-----------------------|
| Ultra SSD    | 4 GB - 64 TB  | 1.200 ila 160.000 IOPS |
| Premium SSD  | 32 GB - 32 TB | 12 ila 15.000 IOPS     |
| Standart SSD | 32 GB - 32 TB | 12 ila 2.000 IOPS      |

Blob depolama, Azure'daki en büyük depolama hacmini sağlar. Azure, depolama boyutuna ek olarak hem yönetilen hem de yönetilmeyen depolama alanı sunar. Azure, yönetilen depolama yla temel depolama hesaplarını yönetmeyi sağlar. Yönetilmeyen depolama ile kullanıcı, depolama gereksinimlerini karşılamak için uygun boyutta Azure depolama hesapları oluşturma sorumluluğunu üstleniyor.

## <a name="next-steps"></a>Sonraki adımlar

- [Ana bilgisayar geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Azure Sanal Makinelerde Ana Bilgisayar yeniden barındırma](/azure/virtual-machines/workloads/mainframe-rehosting/overview)
- [Ana bilgisayar işlem işlem'ini Azure'a taşıma](mainframe-compute-Azure.md)
- [Azure Blob'ları, Azure Dosyaları'nı veya Azure Disklerini ne zaman kullanacağınıza karar verme](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks)
- [Azure VM iş yükleri için Standart SSD Yönetilen Diskler](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)

### <a name="ibm-resources"></a>IBM kaynakları

- [IBM Z'de Paralel Sysplex](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources)
- [IBM CICS ve Kaplin Tesisi: Temellerin Ötesinde](https://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf)
- [Db2 pureScale Özellik yüklemesi için gerekli kullanıcı oluşturma](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)
- [Db2icrt - Örnek komutu oluşturma](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)
- [Db2 pureScale Kümelenmiş Veritabanı Çözümü](https://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)
- [IBM Veri Stüdyosu](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

### <a name="azure-government"></a>Azure Kamu

- [Ana bilgisayar uygulamaları için Microsoft Azure Genel bulutu](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/)
- [Microsoft ve FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/FedRAMP)

### <a name="more-migration-resources"></a>Daha fazla geçiş kaynağı

- [Azure Sanal Veri Merkezi Kaldırma ve Kaydırma Kılavuzu](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
- [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)
