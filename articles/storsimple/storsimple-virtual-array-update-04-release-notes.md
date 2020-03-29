---
title: StorSimple Virtual Array Update 0.4 sürüm notları| Microsoft Dokümanlar
description: Update 0.4 çalıştıran StorSimple Virtual Array için kritik açık sorunları ve çözümleri açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/05/2017
ms.author: alkohli
ms.openlocfilehash: 06a3469507631d032535bce62b01d964e99dc603
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60334803"
---
# <a name="storsimple-virtual-array-update-04-release-notes"></a>StorSimple Virtual Array Update 0.4 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, Microsoft Azure StorBasit Sanal Dizi güncelleştirmeleri için kritik açık sorunları ve çözülmüş sorunları tanımlar.

Sürüm notları sürekli olarak güncelleştirilir ve geçici çözüm gerektiren kritik sorunlar keşfedildikçe eklenir. StorSimple Virtual Array'inizi dağıtmadan önce, sürüm notlarında yer alan bilgileri dikkatle inceleyin.

Güncelleme 0.4 yazılım sürümü **10.0.10289.0**karşılık gelir.

> [!NOTE]
> Güncelleştirmeler kesintiye uğrar ve cihazınızı yeniden başlatın. G/Ç devam ediyorsa, aygıt kapalı kalma süresine neden olur.


## <a name="whats-new-in-the-update-04"></a>Güncelleme 0.4'teki yenilikler
Güncelleştirme 0.4 öncelikle bir hata düzeltme yapı birkaç geliştirmeleri ile birleştiğinde. Bu sürümde, önceki sürümde yedekleme hataları ile sonuçlanan birkaç hata ele alınmıştır. Ana geliştirmeler ve hata düzeltmeleri aşağıdaki gibidir:

- **Yedekleme performansı geliştirmeleri** - Bu sürüm, yedekleme performansını artırmak için birkaç önemli geliştirmeler yapmıştır. Sonuç olarak, çok sayıda dosya içeren yedeklemeler, tam ve artımlı yedeklemeler için tamamlanma süresinde önemli bir azalma görür.

- **Geliştirilmiş geri yükleme performansı** - Bu sürüm, çok sayıda dosya kullanırken geri yükleme performansını önemli ölçüde artıran geliştirmeler içerir. 2 - 4 milyon dosya kullanıyorsanız, iyileştirmeleri görmek için 16 GB RAM'li sanal bir dizi sağlamanızı öneririz. 2 milyondan az dosya kullanırken, sanal makine için minimum gereksinim 8 GB RAM olmaya devam ediyor.

- **Destek paketinde yapılan geliştirmeler** - Geliştirmeler, disk, CPU, bellek, ağ ve bulut istatistiklerini Destek paketine kaydetmeyi ve bu nedenle aygıt sorunlarını tanılama/hata ayıklama işlemini geliştirmeyi içerir.

- **Yerel olarak sabitlenmiş iSCSI hacimlerini 200 GB** ile sınırlandırın - Yerel olarak sabitlenmiş birimler için, StorSimple Virtual Array'inizde 200 GB iSCSI hacmiyle sınırlandırmanızı öneririz. Katmanlı hacimler için yerel rezervasyon, sağlanan hacim boyutunun %10'u olmaya devam eder, ancak 200 GB ile kapatılır. 

- **Yedeklemeyle ilgili hata düzeltmeleri** - Yazılımın önceki sürümlerinde yedekleme hatalarına neden olacak yedeklemelerle ilgili sorunlar vardı. Bu hatalar bu sürümde ele alınmıştır.


## <a name="issues-fixed-in-the-update-04"></a>Güncelleştirme 0.4'te düzeltilen sorunlar

Aşağıdaki tablo, bu sürümde düzeltilen sorunların bir özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Yedekleme performansı|Önceki sürümlerde, çok sayıda dosya içeren yedeklemelerin tamamlanması uzun zaman alır (gün sırasına göre). Bu sürümde, hem tam hem de artımlı yedeklemeler tamamlanma süresinde önemli bir azalma görür. |
| 2 |Destek paketi|Disk, CPU, bellek, ağ ve bulut istatistikleri artık Destek günlüklerine giriş yaparak Destek paketlerini aygıt sorunlarını gidermede çok etkili hale getiriyor.|
| 3 |Backup |Önceki sürümlerde, uzun süren yedeklemeler aygıtta bir boşluk krizine neden olabilir ve bu da yedekleme hatalarına neden olabilir. Bu hata, aynı anda en fazla 5 yedeklemenin sıraya girmesine izin vererek bu sürümde ele alınır.|
| 4 |iSCSI | Daha önceki sürümlerde, katmanlı veya yerel olarak sabitlenmiş hacimler için yerel rezervasyon, sağlanan hacim boyutunun %10'uydu. Bu sürümde, tüm iSCSI birimleri (yerel olarak sabitlenmiş veya katmanlı) için yerel rezervasyon, en fazla 200 GB (2 TB'den büyük katmanlı birimler için) en fazla %10 ile sınırlıdır ve böylece yerel diskte daha fazla alan serbest bırakılabilir. Bu sürümdeki yerel olarak sabitlenmiş birimlerin 200 GB ile sınırlandırılmasını öneririz.|


## <a name="known-issues-in-the-update-04"></a>Güncelleştirme 0.4'te bilinen sorunlar

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
| **15.** |Dosya sunucusu |Sembolik bağlantılar desteklenmez. | |
| **16.** |Dosya sunucusu |StorSimple Virtual Array dosya sunucusunda kopyalandığında veya depolandığında Windows Şifreleme Dosya Sistemi (EFS) tarafından korunan dosyalar, desteklenmeyen bir yapılandırmayla sonuçlanır.  | |

## <a name="next-step"></a>Sonraki adım
StorSimple Virtual Array'inize [Güncelleme 0.4'u yükleyin.](storsimple-virtual-array-install-update-04.md)

## <a name="references"></a>Başvurular
Eski bir sürüm notu mu arıyorsunuz? Şuraya gidin: 

* [StorSimple Sanal Dizi Güncelleme 0.3 Sürüm Notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple Virtual Array Update 0.1 ve 0.2 Sürüm Notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple Virtual Array Genel Kullanılabilirlik Sürüm Notları](storsimple-ova-pp-release-notes.md)

