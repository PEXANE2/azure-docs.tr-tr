---
title: StorSimple Sanal Dizi Güncellemeleri sürüm notları| Microsoft Dokümanlar
description: Update 0.3 çalıştıran StorSimple Virtual Array için kritik açık sorunları ve çözümleri açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: b197651a-3c40-4185-b23d-4c8f22cfa8f4
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/15/2016
ms.author: alkohli
ms.openlocfilehash: 635b5f4edf5d403c569b4957540fc105997b3e8e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60629280"
---
# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple Virtual Array Update 0.3 sürüm notları
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, Microsoft Azure StorBasit Sanal Dizi güncelleştirmeleri için kritik açık sorunları ve çözülmüş sorunları tanımlar.

Sürüm notları sürekli olarak güncelleştirilir ve geçici çözüm gerektiren kritik sorunlar keşfedildikçe eklenir. StorSimple Virtual Array'inizi dağıtmadan önce, sürüm notlarında yer alan bilgileri dikkatle inceleyin.

Güncelleme 0.3 yazılım sürümü **10.0.10288.0**karşılık gelir.

> [!NOTE]
> Güncelleştirmeler kesintiye uğrar ve cihazınızı yeniden başlatın. G/Ç devam ediyorsa, aygıt kapalı kalma süresine neden olur.
> 
> 

## <a name="whats-new-in-the-update-03"></a>Güncelleme 0.3'teki yenilikler
Güncelleştirme 0.3 öncelikle bir hata düzeltme yapıdır. Bu sürümde, önceki sürümde yedekleme hataları ile sonuçlanan birkaç hata ele alınmıştır.

## <a name="issues-fixed-in-the-update-03"></a>Güncelleştirme 0.3'te düzeltilen sorunlar
Aşağıdaki tablo, bu sürümde düzeltilen sorunların bir özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Yedeklemeler |Önceki sürümde, yedeklemelerin dosya paylaşımı için tamamlayamadığı bir sorun görüldü. Bu sorun oluştuysa, yedekleme işi başarısız olur ve kullanıcıyı bilgilendirmek için StorSimple Manager hizmetinde kritik bir uyarı yükseltildi. Bu sorun, paylaşımlar veya verilere erişim verileri etkilemedi. Bu sürümde temel neden tanımlandı ve düzeltildi. <br></br> Düzeltme, bu sorunu zaten görmekte olan paylaşımlar için geriye dönük olarak geçerli değildir. Bu sorunu gören müşterilerin önce Güncelleştirme 0.3'ü uygulaması, ardından sorunu gidermek için tam bir sistem yedeklemesi gerçekleştirmek için Microsoft Destek'e başvurması gerekir. Müşteriler Microsoft Destek'e başvurmak yerine, etkilenen paylaşımlar için sağlıklı bir yedeklemeden yeni bir paya geri yüklenebilir. |
| 2 |iSCSI |Önceki sürümde, StorSimple Virtual Array'deki bir birimden veri kopyalarken birimlerin kaybolacağı bir sorun görüldü. Bu sorun bu sürümde giderilmiştir. <br></br> Düzeltmeler yalnızca yeni oluşturulan birimler üzerinde etkili olur. Düzeltmeler, bu sorunu zaten görmekte olan birimleriçin geriye dönük olarak uygulanmaz. Müşterilerin etkilenen birimleri Azure klasik portalı üzerinden çevrimiçi duruma getirmeleri, bu birimler için yedekleme yapmaları ve bu birimleri yeni birimlere geri yüklemeleri önerilir. |

## <a name="known-issues-in-the-update-03"></a>Güncelleştirme 0.3'te bilinen sorunlar
Aşağıdaki tablo, StorSimple Virtual Array için bilinen sorunların bir özetini sağlar ve önceki sürümlerden belirtilen sorunları içerir. 

| Hayır. | Özellik | Sorun | Geçici çözüm/yorumlar |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal aygıtlar desteklenen genel kullanılabilirlik sürümüne güncelleştirilemez. |Bu sanal aygıtlar, olağanüstü durum kurtarma (DR) iş akışı kullanılarak Genel Kullanılabilirlik sürümü için başarısız olunmalıdır. |
| **2.** |Sağlanan veri diski |Belirli bir boyutta bir veri diski oluşturduktan ve ilgili StorSimple sanal aygıtı oluşturduktan sonra, veri diskini genişletmemeniz veya küçültmemeniz gerekir. Yapmaya çalışmak, aygıtın yerel katmanlarındaki tüm verilerin kaybolmasına neden olabilir. | |
| **3.** |Grup ilkesi |Bir aygıt etki alanına katıldığında, grup ilkesi uygulamak aygıt çalışmasını olumsuz etkileyebilir. |Sanal dizinizin Active Directory için kendi kuruluş biriminde (OU) olduğundan ve buna grup ilkesi nesnesi (GPO) uygulanmadığından emin olun. |
| **4.** |Yerel web Web UI |Internet Explorer'da (IE ESC) gelişmiş güvenlik özellikleri etkinse, Sorun Giderme veya Bakım gibi bazı yerel web ui sayfaları düzgün çalışmayabilir. Bu sayfalardaki düğmeler de çalışmayabilir. |Internet Explorer'daki gelişmiş güvenlik özelliklerini kapatın. |
| **5.** |Yerel web Web UI |Hyper-V sanal makinede, web arabirimi ndeki ağ arabirimleri 10 Gbps arabirimi olarak görüntülenir. |Bu davranış Hyper-V'nin bir yansımasıdır. Hyper-V her zaman sanal ağ bağdaştırıcıları için 10 Gbps gösterir. |
| **6.** |Katmanlı hacimler veya hisseler |StorSimple katmanlı birimleriyle çalışan uygulamalar için bayt aralığı kilitleme desteklenmez. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen önlemler şunlardır: <br></br>Uygulama mantığınızda bayt aralığı kilitlemeyi kapatın.<br></br>Katmanlı birimlerin aksine, bu uygulama için verileri yerel olarak sabitlenmiş birimlere koymayı seçin.<br></br>*Uyarı*: Yerel olarak sabitlenmiş hacimler ve bayt aralığı kilitleme etkinleştirildiğinde, yerel olarak sabitlenmiş birim, geri yükleme tamamlanmadan önce bile çevrimiçi olabilir. Bu gibi durumlarda, geri yükleme devam ediyorsa, geri yüklemenin tamamlanmasını beklemeniz gerekir. |
| **7.** |Katmanlı hisseler |Büyük dosyalarla çalışmak, yavaş katman dışına çıkmasına neden olabilir. |Büyük dosyalarla çalışırken, en büyük dosyanın paylaşım boyutunun %3'ünden daha küçük olduğunu öneririz. |
| **8.** |Hisseler için kullanılan kapasite |Payla ilgili veri olmadığında hisse tüketimini görebilirsiniz. Bunun nedeni, paylaşımlar için kullanılan kapasitenin meta verileri içermesidir. | |
| **9.** |Olağanüstü durum kurtarma |Bir dosya sunucusunun olağanüstü durum kurtarma durumunu yalnızca kaynak aygıtla aynı etki alanına gerçekleştirebilirsiniz. Bu sürümde, başka bir etki alanında hedef aygıta olağanüstü durum kurtarma desteklenmez. |Bu daha sonraki bir sürümde uygulanır. |
| **10.** |Azure PowerShell |StorSimple sanal aygıtları bu sürümde Azure PowerShell üzerinden yönetilemez. |Sanal cihazların tüm yönetimi Azure klasik portalı ve yerel web web ui üzerinden yapılmalıdır. |
| **11.** |Parola değiştirme |Sanal dizi aygıt konsolu yalnızca en-ABD klavye formatında giriş kabul eder. | |
| **12.** |CHAP |Oluşturulduktan sonra CHAP kimlik bilgileri kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimleri çevrimdışı duruma getirmeniz ve değişikliğin etkili olması için bunları çevrimiçi duruma getirmeniz gerekir. |Bu sorun daha sonraki bir sürümde ele alınmıştır. |
| **13.** |iSCSI sunucusu |iSCSI birimi için görüntülenen 'Kullanılmış depolama' StorSimple Manager hizmetinde ve iSCSI ana bilgisayarda farklı olabilir. |iSCSI ana bilgisayarı dosya sistemi görünümüne sahiptir.<br></br>Aygıt, birim maksimum boyuttayken ayrılan blokları görür. |
| **14.** |Dosya sunucusu |Klasördeki bir dosyada onunla ilişkili alternatif veri akışı (ADS) varsa, ADS olağanüstü durum kurtarma, klonlama ve Öğe Düzeyi Kurtarma yoluyla yedeklenmez veya geri yüklenmez. | |

## <a name="next-step"></a>Sonraki adım
StorSimple Virtual Array'inize [Güncelleme 0.3'e yükleyin.](storsimple-ova-install-update-01.md)

## <a name="references"></a>Başvurular
Eski bir sürüm notu mu arıyorsunuz? Şuraya gidin: 

* [StorSimple Virtual Array Update 0.1 ve 0.2 Sürüm Notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple Virtual Array Genel Kullanılabilirlik Sürüm Notları](storsimple-ova-pp-release-notes.md)

