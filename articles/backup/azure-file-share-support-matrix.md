---
title: Azure dosya paylaşma yedeklemesi için destek matrisi
description: Azure dosya paylaşımları yedeklenirken destek ayarlarının ve sınırlamaların özetini sağlar.
ms.topic: conceptual
ms.date: 5/07/2020
ms.openlocfilehash: 38e8cdba4913a56b4dc6b4398a99db9e9f93be51
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83116739"
---
# <a name="support-matrix-for-azure-file-share-backup"></a>Azure dosya paylaşma yedeklemesi için destek matrisi

Azure dosya paylaşımlarını yedeklemek için [Azure Backup hizmetini](https://docs.microsoft.com/azure/backup/backup-overview) kullanabilirsiniz. Bu makale, Azure Backup ile Azure dosya paylaşımlarını yedeklerken destek ayarlarını özetler.

## <a name="supported-geos"></a>Desteklenen GEOS

Azure dosya paylaşımları için yedekleme aşağıdaki GEOS 'da kullanılabilir:

**Ga bölgeleri**:<br>
Avustralya Güney Doğu (Ao), Kanada Orta (CNC), Orta Batı ABD (WCUS), Batı ABD 2 (WUS 2), Hindistan Güney (INS), Orta Kuzey ABD (NCUS), Japonya Doğu (JPE), Brezilya Güney (BRS), Güney Doğu Asya (SEA), İsviçre Batı (SZW), BAE Orta (UAC), Norveç Doğu (NWE), Hindistan Batı (ıNW), Avustralya Orta (ACL), Kore orta (KRC), Japonya Batı (JPW), Güney Afrika Kuzey (SAN), UK Batı (UKW) , Kore Güney (KRS), Almanya Kuzey (GN), Norveç Batı (NWW), Güney Afrika Batı (gördüğünüz), İsviçre Kuzey (SZN), Almanya Orta Batı (GWC), BAE Kuzey (UA), Fransa Orta (FRC), Hindistan Orta (ıNC), Kanada Doğu (CNE), Doğu Asya (EA), Avustralya Doğu (AE), Orta ABD (cu DÜZEYINDE KAPSANıR), Batı ABD (WUS), US gov Arizona (ÇGA), US gov Teksas (UGT), US gov Virginia (UGV), US DOD Orta (UDC), US DOD Doğu (NCESI)

**Desteklenen bölgeler (Önizleme kapsamında) ancak henüz GA**:<br>
Doğu ABD (EUS), Doğu ABD 2 (EUS2), Kuzey Avrupa (NE), Orta Güney ABD (SCUS), UK Güney (UKS), Batı Avrupa (WE)

## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

| Depolama hesabı ayrıntıları | Destek                                                      |
| ------------------------ | ------------------------------------------------------------ |
| Hesap türü            | Azure Backup, genel amaçlı v1, genel amaçlı v2 ve dosya depolama türü depolama hesaplarında bulunan Azure dosya paylaşımlarını destekler |
| Performans              | Azure Backup, hem standart hem de Premium Depolama hesaplarında dosya paylaşımlarını destekler |
| Çoğaltma              | Depolama hesaplarında bulunan Azure dosya paylaşımları, herhangi bir çoğaltma türüyle desteklenir |

## <a name="supported-file-shares"></a>Desteklenen dosya paylaşımları

| Dosya paylaşma türü                                   | Destek   |
| -------------------------------------------------- | --------- |
| Standart                                           | Destekleniyor |
| Büyük                                              | Destekleniyor |
| Premium                                            | Destekleniyor |
| Azure dosya eşitleme hizmeti ile bağlantılı dosya paylaşımları | Destekleniyor |

## <a name="protection-limits"></a>Koruma sınırları

| Ayar                                                      | Sınır |
| ------------------------------------------------------------ | ----- |
| Kasa başına günlük olarak korunabilecek en fazla dosya paylaşımı sayısı | 200   |
| Her gün kasa başına kaydedileen fazla depolama hesabı sayısı | 50    |

## <a name="backup-limits"></a>Yedekleme limitleri

| Ayar                                      | Sınır |
| -------------------------------------------- | ----- |
| Gün başına en fazla isteğe bağlı yedekleme sayısı | 4     |
| Gün başına en fazla zamanlanmış yedekleme sayısı | 1     |

## <a name="restore-limits"></a>Geri yükleme sınırları

| Ayar                                                      | Sınır   |
| ------------------------------------------------------------ | ------- |
| Gün başına en fazla geri yükleme sayısı                           | 10      |
| Geri yükleme başına en fazla dosya sayısı                         | 10      |
| Büyük dosya paylaşımları için geri yükleme başına önerilen en yüksek geri yükleme boyutu | 15 TiB |

## <a name="retention-limits"></a>Bekletme sınırları

| Ayar                                                      | Sınır    |
| ------------------------------------------------------------ | -------- |
| Her zaman bir noktada dosya paylaşımında en fazla toplam kurtarma noktası | 200      |
| İsteğe bağlı yedekleme tarafından oluşturulan kurtarma noktası için en fazla bekletme | 10 yıl |
| Dosya paylaşma başına günlük kurtarma noktası (anlık görüntü) en fazla bekletme| 200 gün |
| Dosya paylaşma başına haftalık kurtarma noktası (anlık görüntü) en fazla bekletme | 200 hafta |
| Dosya paylaşma başına aylık kurtarma noktaları (anlık görüntüler) en fazla bekletme | 120 ay |
| Dosya paylaşma başına yıllık en fazla kurtarma noktası (anlık görüntü) saklama | 10 yıl |

## <a name="supported-restore-methods"></a>Desteklenen geri yükleme yöntemleri

| Restore yöntemi     | Ayrıntılar                                                      |
| ------------------ | ------------------------------------------------------------ |
| Tam paylaşma geri yükleme | Dosya paylaşımının tamamını orijinal veya alternatif bir konuma geri yükleyebilirsiniz |
| Öğe düzeyinde geri yükleme | Tek tek dosyaları ve klasörleri özgün veya alternatif bir konuma geri yükleyebilirsiniz |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure dosya paylaşımlarını nasıl yedekleyeceğinizi](backup-afs.md) öğrenin
* [Azure dosya paylaşımlarını nasıl geri](restore-afs.md) yükleyeceğinizi öğrenin
* [Azure dosya paylaşımının yedeklerini yönetme](manage-afs-backup.md) hakkında bilgi edinin
