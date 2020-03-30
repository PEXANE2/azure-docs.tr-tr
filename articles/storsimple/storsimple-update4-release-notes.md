---
title: StorSimple 8000 Serisi Güncelleme 4 sürüm notları | Microsoft Dokümanlar
description: StorSimple 8000 Serisi Güncelleştirme4'ün yeni özelliklerini, sorunlarını ve geçici çözümlerini açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: ef95ca7b9f94690b607e37fbf5d9378c2f2bcfda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254592"
---
# <a name="storsimple-8000-series-update-4-release-notes"></a>StorSimple 8000 Serisi Güncelleme 4 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları yeni özellikleri açıklar ve StorSimple 8000 Serisi Güncellemesi 4 için kritik açık sorunları tanımlar. Ayrıca, bu sürümde yer alan StorSimple yazılım güncelleştirmelerinin bir listesini de içerirler. 

Güncelleme 4, Sürüm (GA) veya Güncelleme 0.1'i Güncelleştirme 3.1 ile çalıştıran herhangi bir StorSimple aygıtına uygulanabilir. Güncelleştirme 4 ile ilişkili aygıt sürümü 6.3.9600.17820'dir.

Güncelleştirmeyi StorSimple çözümünüze dağıtmadan önce lütfen sürüm notlarında yer alan bilgileri gözden geçirin.

> [!IMPORTANT]
> * Güncelleme 4 cihaz yazılımı, USM firmware, LSI sürücü ve firmware, disk firmware, Storport ve Spaceport, güvenlik ve diğer işletim sistemi güncellemeleri vardır. Bu güncelleştirmeyi yüklemek yaklaşık 4 saat sürer. Disk firmware güncelleştirmesi rahatsız edici bir güncelleştirmedir ve cihazınız için bir kapalı kalma süresi yle sonuçlanır. Cihazınızı güncel tutmak için Güncelleme 4'e uygulamanızı öneririz. 
> * Yeni sürümler için güncelleştirmeleri aşamalı olarak kullanıma sunduğumuz için güncelleştirmeleri hemen göremeyebilirsiniz. Birkaç gün bekleyin ve bu yakında kullanılabilir olacak gibi yeniden güncelleştirmeleri için tarar.

## <a name="whats-new-in-update-4"></a>Güncelleme 4'teki yenilikler

Güncelleştirme 4'te aşağıdaki önemli geliştirmeler ve hata düzeltmeleri yapılmıştır.

* **Akıllı otomatik alan ıslah algoritmaları** – Update 4'te, otomatik alan ıslah algoritmaları, bulutta bulunan beklenen geri kazanılmış alana göre alan ıslah döngülerini ayarlamak için geliştirilmiştir. 

* **Yerel olarak sabitlenmiş birimler için performans geliştirmeleri** – Update 4, yüksek veri alımına (birim boyutuyla karşılaştırılabilir veri) sahip senaryolarda yerel olarak sabitlenmiş birimlerin performansını artırmıştır.

* **Heatmap tabanlı geri yükleme** - Önceki sürümlerde, olağanüstü durum kurtarma (DR) sonrasında, veriler yavaş bir performansla sonuçlanan erişim desenlerine göre buluttan geri yüklendi. 

    Dr'den önce cihaz kullanımdayken Bir ısı haritası oluşturmak için sık erişilen verileri izleyen Update 4'te yeni bir özellik uygulanır (Çoğu kullanılan veri parçaları yüksek ısıya sahipken, daha az kullanılan parçalar düşük ısıya sahiptir). DR'den sonra StorSimple, buluttaki verileri otomatik olarak geri yüklemek ve yeniden sulamak için ısı haritasını kullanır. 

    Tüm geri yüklemeler şimdi ısı haritası tabanlı geri yüklemeler vardır. Nasıl sorgu ve ısı haritası tabanlı geri yükleme ve rehidrasyon işleri iptal hakkında daha fazla bilgi için, [StorSimple cmdlet referans için Windows PowerShell](https://technet.microsoft.com/library/dn688168.aspx)gidin.

* **StorSimple Diagnostics aracı** – Güncelleme 4'te, sistem, ağ, performans ve donanım bileşeni durumuyla ilgili sorunların kolayca tanılanması ve sorun giderilmesi için bir StorSimple Diagnostics aracı piyasaya sürüldü. Bu araç StorSimple için Windows PowerShell üzerinden çalıştırılır. Daha fazla bilgi için [StorSimple Diagnostics aracını kullanarak sorun gidermeye](storsimple-8000-diagnostics.md)gidin.

* **Kullanıcı Arabirimi tabanlı StorSimple Geçiş aracı** - Bu sürümden önce, 5000-7000 serisinden veri geçişi, kullanıcıların Azure PowerShell arabirimini kullanarak geçiş iş akışının bir bölümünü yürütmesini gerektiriyor. Bu sürümde, aynı geçiş iş akışını kolaylaştırmak için Destek için kullanımı kolay bir Kullanıcı Arabirimi tabanlı StorSimple Geçiş aracı kullanıma sunulmuştur. Bu araç aynı zamanda kurtarma kovalarının konsolidasyonuna da olanak sağlar. 

* **FIPS ile ilgili değişiklikler** - Bu sürümden itibaren, FIPS varsayılan olarak hem Microsoft Azure Kamu kuruluşu hem de Azure genel bulut hesapları için tüm StorSimple 8000 serisi cihazlarda etkinleştirilir.

* **Güncelleştirme değişiklikleri** - Bu sürümde, güncelleştirme hataları ile ilgili hatalar düzeltildi.

* **Disk hataları için uyarı** - Bu sürümde, kullanıcıyı yaklaşan disk hataları konusunda uyaran yeni bir uyarı eklendi. Bu uyarıyla karşılaşırsanız, yeni bir disk göndermeiçin Microsoft Destek'e başvurun. Daha fazla bilgi için [StorSimple cihazınızdaki donanım uyarılarına](storsimple-8000-manage-alerts.md#hardware-alerts)gidin.

* **Denetleyici değiştirme değişiklikleri** - Bu sürümde, kullanıcının denetleyici değiştirme işleminin durumunu sorgulamasına olanak tanıyan bir cmdlet eklenir. Daha fazla bilgi [için, denetleyici değiştirme durumunu sorgulamak için cmdlet'e](https://technet.microsoft.com/library/dn688168.aspx)gidin.


## <a name="issues-fixed-in-update-4"></a>Güncelleştirme 4'te düzeltilen sorunlar

Aşağıdaki tablo, Güncelleştirme 4'te düzeltilen sorunların bir özetini sağlar.    

| Hayır | Özellik | Sorun | Fiziksel cihaz için geçerlidir | Sanal aygıt için geçerlidir |
| --- | --- | --- | --- | --- |
| 1 |Yük devretme |Önceki sürümde, failover sonra, müşteri sitesinde gözlenen temizlik ile ilgili bir sorun vardı. Bu sorun bu sürümde giderilmiştir. |Evet |Evet |
| 2 |Yerel olarak sabitlenmiş birimler |Önceki sürümde, yerel olarak sabitlenmiş birimler için ilgili birim oluşturmayla ilgili bir sorun vardı ve bu da birim oluşturma hatalarına neden olacaktı. Bu sorun kök neden oldu ve bu sürümde düzeltildi. |Evet |Hayır |
| 3 |Destek paketi |Önceki sürümde, Bir System.OutOfMemory özel durum veya destek paketi oluşturma hatası ile sonuçlanan diğer hatalar neden olacak Destek paketi ile ilgili sorunlar vardı. Bu hatalar bu sürümde düzeltilir. |Evet |Evet |
| 4 |İzleme |Önceki sürümde, tüketimin EB'de gösterildiği yerel olarak sabitlenmiş hacimler için izleme çizelgeleri ile ilgili bir sorun vardı. Bu hata bu sürümde çözülür. |Evet |Evet |
| 5 |Geçiş |Önceki sürümde, 5000-7000 serisinden 8000 serisi cihazlara geçişin güvenilirliği ile ilgili çeşitli sorunlar vardı. Bu sorunlar bu sürümde ele alınmıştır. |Evet |Evet |
| 6 |Güncelleştirme |Önceki sürümlerde, bir güncelleştirme hatası varsa, denetleyiciler kurtarma moduna geçer ve bu nedenle kullanıcı güncelleştirmeye devam edemeseydi ve Microsoft Desteği'ne başvurması gerekir. <br> Bu sürümde bu davranış değiştirildi. Her iki denetleyici de aynı sürümü çalıştırdıktan sonra kullanıcıda bir güncelleştirme hatası varsa (Güncelleştirme 4), denetleyiciler kurtarma moduna geçmez. Kullanıcı bu hatayla karşılaşırsa, biraz beklemelerini ve güncelleştirmeyi yeniden denemelerini öneririz. Yeniden deneme başarılı olabilir. Yeniden deneme başarısız olursa, Microsoft Destek'e başvurmaları gerekir. |Evet |Evet |


## <a name="known-issues-in-update-4-from-previous-releases"></a>Önceki sürümlerden Güncelleştirme 4'te bilinen sorunlar

Güncelleştirme 4'te bilinen yeni sorunlar yok. Önceki sürümlerden Güncelleştirme 4'e taşınan sorunların listesi için, [Güncelleştirme 3 sürüm notlarına](storsimple-update3-release-notes.md#known-issues-in-update-3)gidin.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-4"></a>Güncelleme 4'te seri eklenmiş SCSI (SAS) denetleyici ve firmware güncellemeleri

Bu sürümde SAS denetleyicisi ve LSI sürücüsü ve firmware güncelleştirmeleri vardır. Bu güncelleştirmelerin nasıl yüklenirhakkında daha fazla bilgi için StorSimple cihazınıza [Update 4 yükle'ye](storsimple-install-update-4.md) bakın.

## <a name="virtual-device-updates-in-update-4"></a>Güncelleme 4'te sanal cihaz güncellemeleri

Bu güncelleştirme StorSimple Cloud Appliance 'a (sanal aygıt olarak da bilinir) uygulanamaz. Yeni sanal aygıtların oluşturulması gerekir. 

## <a name="next-step"></a>Sonraki adım

StorSimple cihazınıza Update 4'ün nasıl [yüklenir](storsimple-install-update-4.md) olduğunu öğrenin.

