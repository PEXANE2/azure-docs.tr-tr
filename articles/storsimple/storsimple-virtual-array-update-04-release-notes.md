---
title: StorSimple Sanal dizisi güncelleştirme 0,4 sürüm notları | Microsoft Docs
description: Güncelleştirme 0,4 çalıştıran StorSimple Sanal dizisine yönelik kritik açık sorunlar ve çözümleri açıklar.
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
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "60334803"
---
# <a name="storsimple-virtual-array-update-04-release-notes"></a>StorSimple Sanal dizisi güncelleştirme 0,4 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, Microsoft Azure StorSimple Sanal dizi güncelleştirmelerine yönelik kritik açık sorunları ve çözümlenen sorunları belirler.

Sürüm notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar keşfedildiğinde eklenirler. StorSimple Sanal dizinizi dağıtmadan önce, sürüm notlarında bulunan bilgileri dikkatle gözden geçirin.

Güncelleştirme 0,4, yazılım sürümü **10.0.10289.0**'e karşılık gelir.

> [!NOTE]
> Güncelleştirmeler kesintiye uğratan ve cihazınızı yeniden başlatacak. G/ç devam ediyorsa, cihaz kapalı kalma süresini doğurur.


## <a name="whats-new-in-the-update-04"></a>Güncelleştirme 0,4 ' deki yenilikler
Güncelleştirme 0,4, öncelikle birkaç geliştirmelerle bağlanmış bir hata çözme derlemesi olur. Bu sürümde, önceki sürümde yedekleme hatalarıyla sonuçlanan bazı hatalar giderilmiştir. Ana geliştirmeler ve hata düzeltmeleri aşağıdaki gibidir:

- **Yedekleme performansı geliştirmeleri** -bu sürüm, yedekleme performansını geliştirmek için birkaç temel geliştirme yaptı. Sonuç olarak, çok sayıda dosya içeren yedeklemeler, tam ve artımlı yedeklemeler için, tamamlanma süresi boyunca önemli bir azalma sağlar.

- **Gelişmiş geri yükleme performansı** -bu sürüm, çok sayıda dosya kullanırken geri yükleme performansını önemli ölçüde artıran geliştirmeler içerir. 2-4 milyon dosya kullanıyorsanız, geliştirmeleri görmek için 16 GB RAM 'e sahip bir sanal dizi sağlamanızı öneririz. 2.000.000 'den az dosya kullanırken, sanal makine için en düşük gereksinim 8 GB RAM olmaya devam eder.

- **Destek paketini geliştirmeye yönelik iyileştirmeler** -geliştirmeler, DISK, CPU, bellek, ağ ve bulut istatistiklerinde destek paketine günlük kaydı, bu sayede cihaz sorunlarını tanılama/hata ayıklama sürecini geliştirir.

- **Yerel olarak sabitlenmiş iSCSI birimlerini 200 GB 'A sınırlayın** -yerel olarak sabitlenmiş birimlerde, StorSimple Sanal dizinizdeki 200 GB 'Lik bir iSCSI birimiyle sınırlamanızı öneririz. Katmanlı birimler için yerel ayırma, sağlanan birim boyutunun %10 ' unu olmaya devam eder, ancak 200 GB ' de kadık. 

- **Yedeklemeyle ilgili hata düzeltmeleri** -yazılımın önceki sürümlerinde, yedekleme hatalarının oluşmasına neden olacak yedeklemelerle ilgili sorunlar vardı. Bu hatalar bu sürümde giderilmiştir.


## <a name="issues-fixed-in-the-update-04"></a>Güncelleştirme 0,4 ' de düzeltilen sorunlar

Aşağıdaki tabloda, bu sürümde düzeltilen sorunların bir özeti verilmiştir.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Yedekleme performansı|Önceki sürümlerde, çok sayıda dosya içeren yedeklemelerin tamamlanması uzun zaman alır (günler). Bu sürümde, tam ve artımlı yedeklemeler, tamamlanma süresi içinde önemli bir düşüş görür. |
| 2 |Destek paketi|Disk, CPU, bellek, ağ ve bulut istatistikleri artık, destek paketlerinin tüm cihaz sorunlarını gidermede çok etkili hale getirilmesi için destek günlüklerine oturum açtı.|
| 3 |Backup |Önceki sürümlerde, uzun süre çalışan yedeklemeler, cihaz üzerinde yedekleme hatalarıyla sonuçlanan bir boşluk oluşmasına neden olabilir. Bu hata, tek seferde 5 ' ten fazla yedekleme yapılmasına izin vererek bu sürümde giderilmiştir.|
| 4 |iSCSI | Önceki sürümlerde, katmanlı veya yerel olarak sabitlenmiş birimler için yerel ayırma, sağlanan birim boyutunun %10 ' u idi. Bu sürümde, tüm Iscsı birimleri (yerel olarak sabitlenmiş veya katmanlı) için yerel ayırma, en fazla 200 GB (2 TB 'den büyük katmanlı birimler için) ile, bu nedenle yerel diskte daha fazla alan boşaltmasına sahip %10 ile sınırlıdır. Bu sürümdeki yerel olarak sabitlenmiş birimlerin 200 GB ile sınırlı olmasını öneririz.|


## <a name="known-issues-in-the-update-04"></a>Güncelleştirme 0,4 ' de bilinen sorunlar

Aşağıdaki tabloda, StorSimple Sanal dizisi için bilinen sorunların özeti verilmiştir ve önceki sürümlerden gelen sorunlar sürümü dahildir. 

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamalar |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal cihazlar desteklenen bir genel kullanılabilirlik sürümüne güncelleştirilemez. |Bu sanal cihazlar, olağanüstü durum kurtarma (DR) iş akışı kullanan genel kullanılabilirlik sürümü için yük devretmelidir. |
| **2.** |Sağlanan veri diski |Belirtilen boyutta bir veri diski sağladıktan ve karşılık gelen StorSimple Sanal cihazını oluşturduktan sonra, veri diskini genişletmemelidir veya küçültmemelidir. Bu durum, cihazın yerel katmanlarında tüm verilerin kaybedilmesine neden olur. | |
| **3.** |Grup İlkesi |Bir cihaz etki alanına katılmış olduğunda, bir grup ilkesi uygulandığında cihaz işlemi olumsuz etkilenebilir. |Sanal diziniz Active Directory için kendi kuruluş biriminde (OU) olduğundan ve buna hiçbir Grup İlkesi nesnesi (GPO) uygulanmadığından emin olun. |
| **4.** |Yerel Web Kullanıcı arabirimi |Internet Explorer 'da (IE ESC) Artırılmış güvenlik özellikleri etkinse, sorun giderme veya bakım gibi bazı yerel Web Kullanıcı arabirimi sayfaları düzgün çalışmayabilir. Bu sayfalardaki düğmeler de çalışmayabilir. |Internet Explorer 'da gelişmiş güvenlik özelliklerini devre dışı bırakın. |
| **e.** |Yerel Web Kullanıcı arabirimi |Hyper-V sanal makinesinde, Web Kullanıcı arabirimindeki ağ arabirimleri 10 Gbps arabirim olarak görüntülenir. |Bu davranış, Hyper-V ' y i bir yansıdır. Hyper-V, sanal ağ bağdaştırıcıları için her zaman 10 Gbps gösterir. |
| **inç.** |Katmanlı birimler veya paylaşımlar |StorSimple katmanlı birimlerle çalışan uygulamalar için bayt aralığı kilitleme desteklenmez. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen ölçüler şunlardır: <br></br>Uygulama mantığınızdaki bayt aralığı kilitlemeyi devre dışı bırakın.<br></br>Bu uygulamaya yönelik verileri katmanlı birimlerin aksine yerel olarak sabitlenmiş birimlerde koymak için seçin.<br></br>*Desteklenmediği uyarısıyla*: yerel olarak sabitlenmiş birimler kullanılırken ve bayt aralığı kilitleme etkinleştirildiğinde, geri yükleme tamamlanmadan önce yerel olarak sabitlenmiş birim çevrimiçi olabilir. Bu tür örneklerde, bir geri yükleme işlemi devam ediyorsa geri yüklemenin tamamlanmasını beklemeniz gerekir. |
| **7.** |Katmanlı paylaşımlar |Büyük dosyalarla çalışma, yavaş katmana neden olabilir. |Büyük dosyalarla çalışırken, en büyük dosyanın paylaşma boyutunun %3 ' inden küçük olmasını öneririz. |
| **240.** |Paylaşımlar için kullanılan kapasite |Paylaşımda veri olmadığında paylaşma tüketimi görebilirsiniz. Bunun nedeni, paylaşımlar için kullanılan kapasitenin meta verileri içerir. | |
| **tuşlarına.** |Olağanüstü durum kurtarma |Yalnızca bir dosya sunucusunun olağanüstü durum kurtarma işlemini kaynak cihazla aynı etki alanına gerçekleştirebilirsiniz. Bu sürümde, başka bir etki alanındaki hedef cihaza olağanüstü durum kurtarma desteklenmiyor. |Bu, sonraki bir sürümde uygulanır. |
| **(.** |Azure PowerShell |StorSimple Sanal cihazları bu sürümdeki Azure PowerShell tarafından yönetilemez. |Sanal cihazların tüm yönetimi, klasik Azure portalı ve yerel Web Kullanıcı arabirimi aracılığıyla yapılmalıdır. |
| **üst.** |Parola değiştirme |Sanal dizi cihaz konsolu yalnızca en-US klavye biçimindeki girişi kabul eder. | |
| **+.** |CHAP |Oluşturulduktan sonra CHAP kimlik bilgileri kaldırılamıyor. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, bu birimleri çevrimdışına almanız ve değişikliğin etkili olması için bunları çevrimiçi duruma getirmeniz gerekir. |Bu sorun, sonraki bir sürümde giderilmiştir. |
| **hatası.** |Iscsı sunucusu |Bir Iscsı birimi için görünen ' kullanılan depolama alanı, StorSimple Yöneticisi hizmeti ve Iscsı ana bilgisayarında farklı olabilir. |Iscsı konağında dosya sistemi görünümü bulunur.<br></br>Cihaz, birim en büyük boyuttan ayrıldığı blokları görür. |
| **May.** |Dosya sunucusu |Bir klasördeki bir dosya ile ilişkili alternatif veri akışı (ADS) varsa, reklamlar olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde kurtarma aracılığıyla yedeklenmez veya geri yüklenmez. | |
| **aşamaz.** |Dosya sunucusu |Sembolik bağlantılar desteklenmiyor. | |
| **k.** |Dosya sunucusu |StorSimple Sanal dizisi dosya sunucusu üzerinde kopyalanırken veya depolanırken Windows şifreleme dosya sistemi (EFS) tarafından korunan dosyalar desteklenmeyen bir yapılandırmayla sonuçlanır.  | |

## <a name="next-step"></a>Sonraki adım
StorSimple Sanal diziniz üzerinde [0,4 güncelleştirmesini yükler](storsimple-virtual-array-install-update-04.md) .

## <a name="references"></a>Başvurular
Daha eski bir sürüm notuna mi bakıyorsunuz? Şuraya gidin: 

* [StorSimple Sanal dizisi güncelleştirme 0,3 sürüm notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple Sanal dizisi güncelleştirme 0,1 ve 0,2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple Sanal dizisi genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)

