---
title: Çevrimdışı yedeklemeye genel bakış
description: Azure Data Box tabanlı çevrimdışı yedekleme ve Azure Içeri/dışarı aktarma tabanlı çevrimdışı yedekleme gibi çeşitli çevrimdışı yedekleme bileşenleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 1/28/2020
ms.openlocfilehash: 35bc15488624f3648bdc765a36d10607b4ca2de9
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77027023"
---
# <a name="overview-of-offline-backup"></a>Çevrimdışı yedeklemeye genel bakış

Bu makalede, çevrimdışı yedeklemeye genel bakış sunulmaktadır.

Azure 'a ilk tam yedeklemeler genellikle büyük miktarda veriyi çevrimiçi olarak aktarır ve yalnızca artımlı değişiklikleri aktarmaya yönelik sonraki yedeklemelere kıyasla daha fazla ağ bant genişliği gerektirir. Belirli coğrafi bölgelerde bulunan uzak ofisler veya veri merkezleri her zaman yeterli ağ bant genişliğine sahip değildir. Bu nedenle, bu ilk yedeklemeler birkaç gün sürer ve bu süre boyunca Şirket içi veri merkezinde çalışan uygulamalar için sağlanan ağı sürekli olarak kullanır.

Azure Backup, ağ bant genişliği kullanılmadan ilk yedekleme verilerinin çevrimdışı aktarılmasına izin veren "çevrimdışı yedekleme" desteği sağlar. Bu, yakın bir Azure veri merkezine gönderilen ve bir kurtarma hizmetleri kasasında karşıya yüklenen fiziksel depolama cihazlarına yedekleme verilerini kopyalamak için bir mekanizma sağlar. Bu işlem, herhangi bir ağ bant genişliği kullanmadan yedekleme verilerinin sağlam aktarılmasını sağlar.

## <a name="offline-backup-options"></a>Çevrimdışı yedekleme seçenekleri

Çevrimdışı yedekleme, depolama cihazlarının sahipliğini temel alan iki modda sunulur.

1. Azure Data Box tabanlı çevrimdışı yedekleme (Önizleme)
2. Azure Içeri/dışarı aktarma tabanlı çevrimdışı yedekleme

## <a name="azure-data-box-based-offline-backup-preview"></a>Azure Data Box tabanlı çevrimdışı yedekleme (Önizleme)

Bu mod, şu anda MARS aracısında, önizleme aşamasında desteklenir. Bu seçenek, USB bağlayıcılarına Microsoft 'a özel, güvenli ve yetkisiz aktarım gereçleri göndermek için [Azure Data Box hizmetten](https://azure.microsoft.com/services/databox/) yararlanır ve veri merkezinize veya uzak ofisinize ve yedekleme verilerinize doğrudan bu cihazlara yazılır. **Bu seçenek, kendi Azure uyumlu disklerini ve bağlayıcılarınızı temin etmek veya bir hazırlama konumu olarak geçici depolamayı sağlamak için gereken çabayı kaydeder.** Ayrıca, Microsoft, Azure portal aracılığıyla izleyebileceğiniz uçtan uca aktarım lojistik 'yı işler. Bu seçenekle yedekleme verilerinin hareketini açıklayan bir mimari aşağıda gösterilmiştir.

![Azure Backup Data Box mimarisi](./media/offline-backup-overview/azure-backup-databox-architecture.png)

Mimarinin özeti aşağıda verilmiştir:

1. Azure Backup, bu önceden yapılandırılmış cihazlara yedekleme verilerini doğrudan kopyalar.
2. Daha sonra bu cihazları bir Azure veri merkezine gönderebilirsiniz.
3. Azure Data Box hizmeti, verileri müşterinin sahip olduğu bir depolama hesabına kopyalar.
4. Azure Backup, yedekleme verilerini depolama hesabından belirlenen kurtarma hizmetleri kasasına otomatik olarak kopyalar ve artımlı çevrimiçi yedeklemeler zamanlanır.

Azure Data Box tabanlı çevrimdışı yedekleme kullanmak için [Bu makaleye](offline-backup-azure-data-box.md)bakın.

## <a name="azure-importexport-based-offline-backup"></a>Azure Içeri/dışarı aktarma tabanlı çevrimdışı yedekleme

Bu seçenek Azure Backup Sunucusu (MABS)/DPM-A/MARS Aracısı tarafından desteklenir. Azure ile uyumlu disklerinizi ve bağlayıcılarınızı kullanarak ilk yedekleme verilerini Azure 'a aktarmanızı sağlayan [Azure içeri/dışarı aktarma hizmeti](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) 'ni kullanır. Bu yaklaşım, yedekleme verilerini biçimlendirmek ve müşteriye ait disklere kopyalamak için önceden oluşturulmuş yardımcı programların **hazırlama konumu** ve kullanımı olarak bilinen geçici bir depolama sağlamasını gerektirir. Bu seçenekle yedekleme verilerinin hareketini açıklayan bir mimari aşağıda verilmiştir:

![Azure Backup Içeri/dışarı aktarma mimarisi](./media/offline-backup-overview/azure-backup-import-export.png)

Mimarinin özeti aşağıda verilmiştir:

1. Yedekleme verilerini ağ üzerinden göndermek yerine, Azure Backup yedekleme verilerini bir hazırlama konumuna yazar.
2. Hazırlama konumundaki veriler özel bir yardımcı program kullanılarak bir veya daha fazla SATA diskine yazılır.
3. Hazırlık işinin bir parçası olarak yardımcı program bir Azure Içeri aktarma işi oluşturur. SATA sürücüler en yakın Azure veri merkezine gönderilir ve etkinlikleri bağlamak için içeri aktarma işine başvurur.
4. Azure veri merkezinde, disklerdeki veriler bir Azure depolama hesabına kopyalanır.
5. Azure Backup, yedekleme verilerini depolama hesabından kurtarma hizmetleri kasasına kopyalar ve artımlı yedeklemeler zamanlanır.

Azure Içeri/dışarı aktarma tabanlı çevrimdışı yedeklemeyi MARS aracısıyla kullanmak için [Bu makaleye](https://docs.microsoft.com/azure/backup/backup-azure-backup-import-export)bakın.

MABS/DPM-A ile aynı şekilde kullanmak için, [Bu makaleye](https://docs.microsoft.com/azure/backup/backup-azure-backup-server-import-export-)bakın.

## <a name="offline-backup-support-summary"></a>Çevrimdışı yedekleme desteği Özeti

Aşağıdaki tabloda, senaryonuza göre uygun seçimleri yapabilmek için kullanılabilecek iki seçenek karşılaştırılmaktadır.

| **Dikkate Alınacak Nokta**                                            | **Azure Data Box tabanlı çevrimdışı yedekleme**                     | **Azure Içeri/dışarı aktarma tabanlı çevrimdışı yedekleme**                |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Azure Backup dağıtım modelleri                              | MARS Aracısı (Önizleme)                                              | MARS Aracısı, Azure Backup Sunucusu (MABS), DPM-A                                           |
| Sunucu başına (MARS) veya koruma grubu başına en fazla yedekleme verisi (MABS, DPM-A) | [Azure Data Box disk](https://docs.microsoft.com/azure/databox/data-box-disk-overview) -7,2 TB <br> [Azure Data Box](https://docs.microsoft.com/azure/databox/data-box-overview) -80 TB       | 80 TB (her biri 8 TB 'a kadar)                          |
| Güvenlik (veri, cihaz & hizmeti)                           | [Data](https://docs.microsoft.com/azure/databox/data-box-security#data-box-data-protection) -AES-256 bit şifreli <br> [Cihaz](https://docs.microsoft.com/azure/databox/data-box-security#data-box-device-protection) -Rugged Case, verileri kopyalamak için özel kimlik bilgisi tabanlı arabirim <br> [Hizmet](https://docs.microsoft.com/azure/databox/data-box-security#data-box-service-protection) tarafından korunan Azure Güvenlik özellikleri | Veri-BitLocker şifreli                                 |
| Geçici hazırlama konumu sağlama                     | Gerekli değil                                                | Tahmini yedekleme verileri boyutundan daha fazla veya eşit        |
| Desteklenen bölgeler                                           | [Azure Data Box Disk bölgeler](https://docs.microsoft.com/azure/databox/data-box-disk-overview#region-availability) <br> [Azure Data Box bölgeler](https://docs.microsoft.com/azure/databox/data-box-disk-overview#region-availability) | [Azure Içeri/dışarı aktarma bölgeleri](https://docs.microsoft.com/azure/storage/common/storage-import-export-service#region-availability) |
| Çapraz ülke içi teslim *                                     | **Desteklenmiyor**  <br>    *Kaynak adresi & hedef Azure veri merkezi aynı ülkede olmalıdır* | Desteklenen                                                    |
| Lojistik (teslim, taşıma, çekme)           | Tamamen Microsoft tarafından yönetilen                                     | Müşteri tarafından yönetilen                                            |
| Fiyatlandırma                                                      | [Azure Data Box fiyatlandırması](https://azure.microsoft.com/pricing/details/databox/) <br> [Azure Data Box Disk fiyatlandırması](https://azure.microsoft.com/pricing/details/databox/disk/) | [Azure Içeri/dışarı aktarma fiyatlandırması](https://azure.microsoft.com/pricing/details/storage-import-export/) |

* *Ülkenizde bir Azure Veri Merkezi yoksa, farklı bir ülkede disklerinizi bir Azure veri merkezine teslim etmeniz gerekir.*

## <a name="next-steps"></a>Sonraki adımlar

* [MARS Aracısı için Azure Data Box tabanlı çevrimdışı yedekleme](offline-backup-azure-data-box.md#backup-data-size-and-supported-data-box-skus)
* [MARS Aracısı için Azure Içeri/dışarı aktarma tabanlı çevrimdışı yedekleme](backup-azure-backup-import-export.md)  
* [MABS/DPM için Azure Içeri/dışarı aktarma tabanlı çevrimdışı yedekleme-A](backup-azure-backup-server-import-export-.md)
