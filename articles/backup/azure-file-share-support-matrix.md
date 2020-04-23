---
title: Azure dosya paylaşma yedeklemesi için destek matrisi
description: Azure dosya paylaşımları yedeklenirken destek ayarlarının ve sınırlamaların özetini sağlar.
ms.topic: conceptual
ms.date: 1/26/2020
ms.openlocfilehash: e74d04cf8ae9010a860b8467d0de771524bd3f3a
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82103210"
---
# <a name="support-matrix-for-azure-file-share-backup"></a>Azure dosya paylaşma yedeklemesi için destek matrisi

Azure dosya paylaşımlarını yedeklemek için [Azure Backup hizmetini](https://docs.microsoft.com/azure/backup/backup-overview) kullanabilirsiniz. Bu makale, Azure Backup ile Azure dosya paylaşımlarını yedeklerken destek ayarlarını özetler.

## <a name="supported-geos"></a>Desteklenen GEOS

Azure dosya paylaşımları için yedekleme aşağıdaki GEOS 'da kullanılabilir:

| GA bölgeleri | Desteklenen bölgeler, ancak GA değil                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Avustralya Doğu (AE), Kanada Orta (CNC), Orta Batı ABD (WCUS), Hindistan Güney (INS), Orta Kuzey ABD (NCUS), Japonya Doğu (JPE), Brezilya Güney (BRS)                                                     |Avustralya Güney Doğu (ASE), Kanada Doğu (CE), Orta ABD (cu DÜZEYINDE KAPSANıR), Doğu Asya (EA), Doğu ABD (EUS), Doğu ABD 2 (EUS2), Japonya Batı (JPW), Hindistan Orta (ıNC), Kore Orta (KRC), Kore Güney (KRS), Kuzey Avrupa (NE), Orta Güney ABD (SCUS), Güney Doğu Asya (denız), UK Güney (UKS), UK Batı (ÇW), Batı Avrupa (am), Batı ABD (WU), US gov Arizona (UGA), US gov Teksas (UGT) , US Gov Virginia (UGV), Avustralya Orta (ACL), Hindistan Batı (ıNW), Güney Afrika Kuzey (SAN), BAE Kuzey (UAN), Fransa Orta (FRC), Almanya Kuzey (GN), Almanya Orta Batı (GWC), Güney Afrika Batı (gördüğünüz), BAE Orta (UAC), Norveç Doğu (NWE), Norveç Batı (NWW), İsviçre Kuzey (SZN), Batı ABD 2 (WUS 2)             |

## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

| Depolama hesabı ayrıntıları | Destek                                                      |
| ------------------------ | ------------------------------------------------------------ |
| Hesap türü            | Azure Backup hem genel amaçlı v1 hem de genel amaçlı v2 depolama hesaplarında Azure dosya paylaşımlarını destekler |
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
