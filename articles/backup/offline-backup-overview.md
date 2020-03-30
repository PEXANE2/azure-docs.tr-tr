---
title: Çevrimdışı yedeklemeye genel bakış
description: Çevrimdışı yedekleme bileşenleri hakkında bilgi edinin. Bunlar, Azure Veri Kutusu'na dayalı çevrimdışı yedekleme ve Azure İçe Alma/Dışa Aktarma hizmetini temel alan çevrimdışı yedeklemeyi içerir.
ms.topic: conceptual
ms.date: 1/28/2020
ms.openlocfilehash: 4dae68c46e0072a726bba13139e405b44be0f008
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78196187"
---
# <a name="overview-of-offline-backup"></a>Çevrimdışı yedeklemeye genel bakış

Bu makalede, çevrimdışı yedekleme genel bir bakış sağlar.

Azure'a ilk tam yedeklemeler genellikle büyük miktarda veriyi çevrimiçi olarak aktarAbilir ve yalnızca artımlı değişiklikler aktaran sonraki yedeklemelerle karşılaştırıldığında daha fazla ağ bant genişliği gerektirir. Belirli coğrafyalarda uzak ofisler veya veri merkezleri her zaman yeterli ağ bant genişliğine sahip değildir. Bu nedenle, bu ilk yedeklemeler birkaç gün sürer. Bu süre zarfında, yedeklemeler şirket içi veri merkezinde çalışan uygulamalar için sağlanan aynı ağı sürekli olarak kullanır.

Azure Yedekleme, ağ bant genişliği kullanmadan ilk yedekleme verilerini çevrimdışı olarak aktaran çevrimdışı yedeklemeyi destekler. Yedekleme verilerini fiziksel depolama aygıtlarına kopyalamak için bir mekanizma sağlar. Aygıtlar daha sonra yakındaki bir Azure veri merkezine gönderilir ve Kurtarma Hizmetleri kasasına yüklenir. Bu işlem, herhangi bir ağ bant genişliği kullanmadan yedekleme verilerinin sağlam aktarılmasını sağlar.

## <a name="offline-backup-options"></a>Çevrimdışı yedekleme seçenekleri

Çevrimdışı yedekleme, depolama aygıtlarının sahipliğine göre iki modda sunulur:

- Azure Veri Kutusu'na dayalı çevrimdışı yedekleme (önizleme)
- Azure İçe Alma/Dışa Aktarma hizmetine dayalı çevrimdışı yedekleme

## <a name="offline-backup-based-on-azure-data-box-preview"></a>Azure Veri Kutusu'na dayalı çevrimdışı yedekleme (önizleme)

Bu mod şu anda önizlemede Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı ile desteklenir. Bu seçenek, Microsoft'a özel, güvenli ve kurcalamaya dayanıklı aktarım cihazlarını USB konektörleri ile veri merkezinize veya uzak ofisinize nakletmek için [Azure Veri Kutusu'ndan](https://azure.microsoft.com/services/databox/) yararlanır. Yedekleme verileri doğrudan bu aygıtlara yazılır. Bu seçenek, kendi Azure uyumlu diskve konektörlerinizi satın almak veya geçici depolamayı bir hazırlama konumu olarak sağlamak için gereken çabayı kaydeder. Microsoft, Azure portalı üzerinden izleyebileceğiniz uçtan uca aktarım lojistiğini de yönetir. 

Bu seçenekle yedekleme verilerinin hareketini açıklayan bir mimari burada gösterilir.

![Azure Yedekleme Veri Kutusu mimarisi](./media/offline-backup-overview/azure-backup-databox-architecture.png)

Mimarinin bir özeti aşağıda veda edilmiştir:

1. Azure Yedekleme yedekleme verilerini doğrudan bu önceden yapılandırılmış aygıtlara kopyalar.
2. Daha sonra bu aygıtları bir Azure veri merkezine geri taşıyabilirsiniz.
3. Azure Veri Kutusu verileri müşteriye ait bir depolama hesabına kopyalar.
4. Azure Yedekleme, yedekleme verilerini depolama hesabından atanmış Kurtarma Hizmetleri kasasına otomatik olarak kopyalar. Artımlı çevrimiçi yedeklemeler zamanlanır.

Azure Veri Kutusu'na dayalı çevrimdışı yedeklemeyi kullanmak için [Azure Veri Kutusu'ni kullanarak Çevrimdışı yedekleme'ye](offline-backup-azure-data-box.md)bakın.

## <a name="offline-backup-based-on-the-azure-importexport-service"></a>Azure İçe Alma/Dışa Aktarma hizmetine dayalı çevrimdışı yedekleme

Bu seçenek Microsoft Azure Yedekleme Sunucusu (MABS), System Center Data Protection Manager (DPM) DPM-A ve MARS Aracısı tarafından desteklenir. [Azure İçe Alma/Dışa Aktarma hizmetini](https://docs.microsoft.com/azure/storage/common/storage-import-export-service)kullanır. İlk yedekleme verilerini Azure uyumlu kendi disklerinizi ve bağlayıcılarınızı kullanarak Azure'a aktarabilirsiniz. Bu yaklaşım, hazırlama konumu olarak bilinen geçici depolama sağlamanızı ve yedekleme verilerini müşteriye ait disklere biçimlendirmek ve kopyalamak için önceden oluşturulmuş yardımcı programları kullanmanızı gerektirir. 

Bu seçenekle yedekleme verilerinin hareketini açıklayan bir mimari burada gösterilir.

![Azure Yedekleme İçe Alma/Dışa Aktarma hizmeti mimarisi](./media/offline-backup-overview/azure-backup-import-export.png)

Mimarinin bir özeti aşağıda veda edilmiştir:

1. Azure Yedekleme, yedekleme verilerini ağ üzerinden göndermek yerine bir hazırlama konumuna yedek verileri yazar.
2. Hazırlama konumundaki veriler, özel bir yardımcı program kullanılarak bir veya daha fazla SATA diskine yazılır.
3. Hazırlık çalışmalarının bir parçası olarak, yardımcı program bir Azure alma işi oluşturur. SATA sürücüleri en yakın Azure veri merkezine gönderilir ve etkinlikleri bağlamak için alma işine başvurur.
4. Azure veri merkezinde, diskler üzerindeki veriler bir Azure depolama hesabına kopyalanır.
5. Azure Yedekleme, yedekleme verilerini depolama hesabından Kurtarma Hizmetleri kasasına kopyalar. Artımlı yedeklemeler zamanlanır.

MARS Aracısı ile Azure İçe Alma/Dışa Aktarma hizmetini temel alan çevrimdışı yedeklemeyi kullanmak için [Azure Yedekleme'de Çevrimdışı yedekleme iş akışına](https://docs.microsoft.com/azure/backup/backup-azure-backup-import-export)bakın.

Aynı şeyi MABS veya DPM-A ile birlikte kullanmak [için DPM ve Azure Yedekleme Sunucusu için Çevrimdışı yedekleme iş akışına](https://docs.microsoft.com/azure/backup/backup-azure-backup-server-import-export-)bakın.

## <a name="offline-backup-support-summary"></a>Çevrimdışı yedekleme destek özeti

Aşağıdaki tablo, senaryonuza göre uygun seçimleri yapabilmeniz için kullanılabilir iki seçeneği karşılaştırır.

| **Dikkate Alınacak Nokta**                                            | **Azure Veri Kutusu'na dayalı çevrimdışı yedekleme**                     | **Azure İçe Alma/Dışa Aktarma hizmetine dayalı çevrimdışı yedekleme**                |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Azure Yedekleme dağıtım modelleri                              | MARS Agent (önizleme)                                              | MARS Ajanı, MABS, DPM-A                                           |
| Sunucu başına maksimum yedekleme verisi (MARS) veya koruma grubu başına (MABS, DPM-A) | [Azure Veri Kutusu diski](https://docs.microsoft.com/azure/databox/data-box-disk-overview) - 7.2 TB <br> [Azure Veri Kutusu](https://docs.microsoft.com/azure/databox/data-box-overview) - 80 TB       | 80 TB (her biri 8 TB'den 10'a kadar disk)                          |
| Güvenlik (veri, aygıt ve hizmet)                           | [Veri](https://docs.microsoft.com/azure/databox/data-box-security#data-box-data-protection) - AES 256-bit şifreli <br> [Aygıt](https://docs.microsoft.com/azure/databox/data-box-security#data-box-device-protection) - Sağlam kasa, verileri kopyalamak için özel, kimlik bilgisi tabanlı arayüz <br> [Hizmet](https://docs.microsoft.com/azure/databox/data-box-security#data-box-service-protection) - Azure güvenlik özellikleriyle korumalı | Veri - BitLocker şifreli                                 |
| Geçici evreleme yeri sağlama                     | Gerekli değil                                                | Tahmini yedekleme veri boyutundan daha fazla veya eşit        |
| Desteklenen bölgeler                                           | [Azure Veri Kutusu disk bölgeleri](https://docs.microsoft.com/azure/databox/data-box-disk-overview#region-availability) <br> [Azure Veri Kutusu bölgeleri](https://docs.microsoft.com/azure/databox/data-box-disk-overview#region-availability) | [Azure İçe Alma/Dışa Aktarma hizmet bölgeleri](https://docs.microsoft.com/azure/storage/common/storage-import-export-service#region-availability) |
| Kros nakliye                                     | Desteklenmiyor  <br>    Kaynak adresi ve hedef Azure veri merkezi aynı ülkede olmalıdır* | Destekleniyor                                                    |
| Transfer lojistiği (teslimat, nakliye, pikap)           | Tamamen Microsoft yönetilen                                     | Müşteri yönetimi                                            |
| Fiyatlandırma                                                      | [Azure Veri Kutusu fiyatlandırması](https://azure.microsoft.com/pricing/details/databox/) <br> [Azure Veri Kutusu disk fiyatlandırması](https://azure.microsoft.com/pricing/details/databox/disk/) | [Azure İçe Alma/Dışa Aktarma hizmet fiyatlandırması](https://azure.microsoft.com/pricing/details/storage-import-export/) |

*Ülkenizde Bir Azure veri merkezi yoksa, disklerinizi başka bir ülkedeki bir Azure veri merkezine göndermeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Kutusu'ni kullanarak Azure Yedekleme çevrimdışı yedekleme](offline-backup-azure-data-box.md#backup-data-size-and-supported-data-box-skus)
* [Azure Yedekleme'de çevrimdışı yedekleme iş akışı](backup-azure-backup-import-export.md) 
* [DPM ve Azure Yedekleme Sunucusu için çevrimdışı yedekleme iş akışı](backup-azure-backup-server-import-export-.md)
