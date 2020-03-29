---
title: Microsoft Azure StorBasit Sanal Dizi Güncelleştirmesi 1.2 sürüm notları| Microsoft Dokümanlar
description: Güncelleştirme 1.2 çalıştıran StorSimple Virtual Array için kritik açık sorunları ve çözümleri açıklar.
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 05/29/2019
ms.author: alkohli
ms.openlocfilehash: ea7e4801dfaad533403c0f927a03735ae409cc52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66420611"
---
# <a name="storsimple-virtual-array-update-12-release-notes"></a>StorSimple Virtual Array Update 1.2 sürüm notları

Aşağıdaki sürüm notları, Microsoft Azure StorBasit Sanal Dizi güncelleştirmeleri için kritik açık sorunları ve çözülmüş sorunları tanımlar.

Sürüm notları sürekli olarak güncellenir. Çözüm gerektiren kritik sorunlar bulundukça eklenir. StorSimple Virtual Array'inizi dağıtmadan önce, sürüm notlarında yer alan bilgileri dikkatle inceleyin.

Güncelleme 1.2 yazılım sürümü **10.0.10311.0**karşılık gelir.

> [!IMPORTANT]
> - Güncelleştirmeler kesintiye uğrar ve cihazınızı yeniden başlatın. G/Ç devam ediyorsa, aygıt kapalı kalma süresine neden olur. Bu güncelleştirmeyi uygulamak için kullanılan paketlerle ilgili ayrıntılı talimatlar için [Güncelleştirme 1.2'yi Karşıdan Yükle'ye](#download-update-12)gidin.
> - Güncelleme 1.2, yalnızca aygıtınız 1.0 veya 1.1 güncellemesi çalıştırıyorsa Azure portalı üzerinden kullanılabilir.

## <a name="whats-new-in-update-12"></a>Güncelleme 1.2'deki yenilikler

Bu güncelleştirme aşağıdaki hata düzeltmelerini içerir:

- Silinen dosyaları işlerken daha iyi esneklik.
- Yedekleme, geri yükleme, bulut okuma ve otomatik alan ıslahında daha az hataya yol açan kod başlatma yolundaki gelişmiş işleme özel durumları.

## <a name="download-update-12"></a>Güncelleme 1.2'yi İndir

Bu güncelleştirmeyi indirmek için [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/Home.aspx) sunucusuna gidin ve KB4502035 paketini indirin. Bu paket aşağıdaki paketleri içerir:

 - Nisan 2019'a kadar 2012 R2 için kümülatif Windows Güncelleştirmeleri içeren **KB4493446.** Bu toplama da nelerin yer aldığı hakkında daha fazla bilgi için [Nisan ayı aylık güvenlik toplama'ya](https://support.microsoft.com/help/4493446/windows-8-1-update-kb4493446)gidin.
 - **KB3011067** bir Microsoft Update Bağımsız Paket dosyası WindowsTH-KB3011067-x64. Bu dosya aygıt yazılımını güncelleştirmek için kullanılır.

KB4502035 indirin ve [yerel web Kullanıcı Telefonu üzerinden güncelleştirmeyi uygulamak](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui)için bu yönergeleri izleyin.

## <a name="issues-fixed-in-update-12"></a>Güncelleştirme 1.2'de düzeltilen sorunlar

Aşağıdaki tablo, bu sürümde düzeltilen sorunların bir özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Silme| Yazılımın önceki sürümlerinde, dosyalar silindiğinde bile aygıtın kullanımının değişmediği bir sorun vardı. Bu sorun bu sürümde giderilmiştir. Silinen dosyaları işlerken kod yolunu katmanlama daha esnek hale getirilmiş.|
| 2 |Özel durum işleme| Yazılımın önceki sürümlerinde, sistem yeniden başlatılmasından sonra yedeklemelerde, geri yüklemede, buluttan okumada ve otomatik alan ıslahında hatalara yol açabilecek bir sorun vardı. Bu sürüm, başlangıç yolunda özel durumların nasıl işlendiğine dair değişiklikler içerir.|

## <a name="known-issues-in-update-12"></a>Güncelleştirme 1.2'de bilinen sorunlar

Güncelleştirme 1.2'de yeni sorunlar kaydedilemedi. Tüm sürüm-kaydetti konular önceki sürümlerden taşınır. Önceki sürümlerden alınan bilinen sorunların özetini görmek için [Güncelleştirme 1.1'deki Bilinen sorunlara](storsimple-virtual-array-update-11-release-notes.md#known-issues-in-update-11)gidin.

## <a name="next-steps"></a>Sonraki adımlar

KB4502035 indirin ve [yerel web Kullanıcı Telefonu üzerinden güncelleme uygulayın.](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui)

## <a name="references"></a>Başvurular

Eski bir sürüm notu mu arıyorsunuz? Şuraya gidin:
* [StorSimple Sanal Dizi Güncelleme 1.1 Sürüm Notları](storsimple-virtual-array-update-11-release-notes.md)
* [StorSimple Sanal Dizi Güncelleme 1.0 Sürüm Notları](storsimple-virtual-array-update-1-release-notes.md)
* [StorSimple Sanal Dizi Güncelleme 0.6 Sürüm Notları](storsimple-virtual-array-update-06-release-notes.md)
* [StorSimple Sanal Dizi Güncelleme 0.5 Sürüm Notları](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple Sanal Dizi Güncelleme 0.4 Sürüm Notları](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple Sanal Dizi Güncelleme 0.3 Sürüm Notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple Virtual Array Update 0.1 ve 0.2 Sürüm Notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple Virtual Array Genel Kullanılabilirlik Sürüm Notları](storsimple-ova-pp-release-notes.md)
