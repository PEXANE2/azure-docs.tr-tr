---
title: Teyp altyapınızı değiştirme
description: Azure Yedekleme'nin Azure'da verileri yedeklemenize ve geri yüklemenize olanak tanıyan teyp benzeri anlamtisi nasıl sağladığını öğrenin
ms.topic: conceptual
ms.date: 04/30/2017
ms.openlocfilehash: aeda1cefc84d425855c40b793f8334936541e63f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77425111"
---
# <a name="move-your-long-term-storage-from-tape-to-the-azure-cloud"></a>Uzun süreli depolama alanınızı banttan Azure bulutuna taşıyın

Azure Yedekleme ve Sistem Merkezi Veri Koruma Yöneticisi müşterileri şunları yapabilir:

* Kuruluş gereksinimlerine en uygun zamanlamalarda verileri yedekle.
* Yedekleme verilerini daha uzun süre saklar.
* Azure'u uzun süreli bekletme gereksinimlerinin bir parçası haline getirin (bant yerine).

Bu makalede, müşterilerin yedekleme ve bekletme ilkelerini nasıl etkinleştirebileceği açıklanmaktadır. Uzun süreli saklama gereksinimlerini karşılamak için teyp kullanan müşteriler artık bu özelliğin kullanılabilirliği yle güçlü ve uygulanabilir bir alternatife sahiptir. Özellik, Azure Yedekleme'nin en son sürümünde etkinleştirilir [(burada](https://aka.ms/azurebackup_agent)mevcuttur). Sistem Merkezi DPM müşterileri, Azure Yedekleme hizmetiyle DPM'yi kullanmadan önce en azından DPM 2012 R2 UR5'e güncelleştirmelidir.

## <a name="what-is-the-backup-schedule"></a>Yedekleme Çizelgesi nedir?

Yedekleme zamanlaması yedekleme işleminin sıklığını gösterir. Örneğin, aşağıdaki ekrandaki ayarlar yedeklemelerin her gün 18:00'de ve gece yarısı alındığını gösterir.

![Günlük Program](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Müşteriler ayrıca haftalık yedekleme zamanlayabilir. Örneğin, aşağıdaki ekrandaki ayarlar, yedeklemelerin her pazar & Çarşamba günü saat 09:30 ve 01:00'de alındığını gösterir.

![Haftalık Program](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-the-retention-policy"></a>Bekletme İlkesi nedir?

Bekletme ilkesi, yedeklemenin depolanması gereken süreyi belirtir. Müşteriler, tüm yedekleme noktaları için "düz ilke" belirtmek yerine, yedeklemenin ne zaman alındığına bağlı olarak farklı bekletme ilkeleri belirtebilir. Örneğin, operasyonel bir kurtarma noktası olarak hizmet veren günlük olarak alınan yedekleme noktası 90 gün boyunca korunur. Denetim amacıyla her üç aylık dönem sonunda alınan yedekleme noktası daha uzun bir süre korunur.

![Bekletme İlkesi](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

Bu politikada belirtilen toplam "bekletme noktası" sayısı 90 (günlük puan) + 40 (10 yıl için her üç ayda bir) = 130'dur.

## <a name="example--putting-both-together"></a>Örnek – Her ikisini bir araya getirmek

![Örnek Ekran](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Günlük saklama ilkesi**: Günlük alınan yedeklemeler yedi gün boyunca saklanır.
2. **Haftalık saklama ilkesi**: Cumartesi gece yarısı ve 18:00'de alınan yedeklemeler dört hafta boyunca saklanır.
3. **Aylık saklama ilkesi**: Her ayın son Cumartesi günü gece yarısı ve 18:00'de alınan yedeklemeler 12 ay boyunca saklanır.
4. **Yıllık saklama ilkesi**: Her Mart'ın son Cumartesi günü gece yarısı alınan yedeklemeler 10 yıl boyunca saklanır.

Önceki diyagramdaki toplam "bekletme noktası" sayısı (müşterinin verileri geri yükleyebileceği noktalar) aşağıdaki gibi hesaplanır:

* yedi gün boyunca günde iki puan = 14 kurtarma noktası
* dört hafta boyunca haftada iki puan = 8 kurtarma noktası
* 12 ay için ayda iki puan = 24 kurtarma noktası
* yılda bir puan / 10 kurtarma noktası

Toplam kurtarma noktası sayısı 56'dır.

> [!NOTE]
> Azure Yedekleme'yi kullanarak korumalı örnek başına en fazla 9999 kurtarma noktası oluşturabilirsiniz. Korumalı örnek, Azure’a yedeklenen bir bilgisayar, sunucu (fiziksel veya sanal) veya iş yüküdür.
>

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma

Önceki ekranda **Değiştir'i** tıklatarak, müşteriler bekletme zamanlamalarını belirtmede daha fazla esnekliğe sahiptir.

![Değiştir](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Yedekleme hakkında daha fazla bilgi için bkz:

* [Azure Backup'a giriş](backup-introduction-to-azure-backup.md)
* [Azure Backup'ı deneyin](backup-try-azure-backup-in-10-mins.md)
