---
title: StorSimple Sanal dizisi güncelleştirme 0,5 sürüm notları | Microsoft Docs
description: Güncelleştirme 0,5 çalıştıran StorSimple Sanal dizisine yönelik kritik açık sorunlar ve çözümleri açıklar.
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
ms.date: 05/08/2017
ms.author: alkohli
ms.openlocfilehash: 385d9126d578250064659153f6f0f54eec696790
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "60870681"
---
# <a name="storsimple-virtual-array-update-05-release-notes"></a>StorSimple Sanal dizisi güncelleştirme 0,5 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, Microsoft Azure StorSimple Sanal dizi güncelleştirmelerine yönelik kritik açık sorunları ve çözümlenen sorunları belirler.

Sürüm notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar keşfedildiğinde eklenirler. StorSimple Sanal dizinizi dağıtmadan önce, sürüm notlarında bulunan bilgileri dikkatle gözden geçirin.

Güncelleştirme 0,5, yazılım sürümü **10.0.10290.0**'e karşılık gelir.

> [!NOTE]
> Güncelleştirmeler kesintiye uğratan ve cihazınızı yeniden başlatacak. G/ç devam ediyorsa, cihaz kapalı kalma süresini doğurur. Güncelleştirmeyi uygulama hakkında ayrıntılı yönergeler için [güncelleştirme 0,5](storsimple-virtual-array-install-update-05.md)' a gidin.


## <a name="whats-new-in-the-update-05"></a>Güncelleştirme 0,5 ' deki yenilikler
Güncelleştirme 0,5, birincil olarak bir hata düzeltmesini derleme. Ana geliştirmeler ve hata düzeltmeleri aşağıdaki gibidir:

- **Yedekleme dayanıklılığı iyileştirmeleri** -bu sürüm, yedekleme esnekliğini artıran düzeltmeler içerir. Önceki sürümlerde yedeklemeler yalnızca belirli özel durumlar için yeniden denenir. Bu sürüm tüm yedekleme özel durumlarını yeniden dener ve yedeklemeleri daha dayanıklı hale getirir.

- **Depolama kullanımı Izleme güncelleştirmeleri** -30 Haziran 2017 ' den başlayarak, StorSimple sanal cihaz serisi için depolama kullanımı izleme kullanımdan kaldırılacak. Bu, güncelleştirme 0,4 veya daha düşük bir çalışan tüm sanal dizilerde izleme grafikleri için geçerlidir. Bu güncelleştirme, Azure portal depolama kullanımı izlemenin kullanımına devam etmeniz için gereken değişiklikleri içerir. İzleme özelliğini kullanmaya devam etmek için 30 Haziran 2017 ' den önce bu kritik güncelleştirmeyi yüklemelisiniz.


## <a name="issues-fixed-in-the-update-05"></a>Güncelleştirme 0,5 ' de düzeltilen sorunlar

Aşağıdaki tabloda, bu sürümde düzeltilen sorunların bir özeti verilmiştir.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Yedekleme esnekliği| Önceki sürümlerde yedeklemeler yalnızca belirli özel durumlar için yeniden denenir. Bu sürüm, tüm yedekleme özel durumlarını yeniden denemeden yedeklemeleri daha dayanıklı hale getirmek için bir çözüm içerir.|
| 2 |İzleme| StorSimple sanal cihaz serisi için depolama kullanımı izleme 30 Haziran 2017 ' den itibaren kullanım dışı bırakılacak. Bu eylem, StorSimple Sanal dizileri (1200 modeli) üzerinde çalışan StorSimple Aygıt Yöneticisi hizmetindeki izleme grafiklerini etkiler. Bu sürümde, kullanıcının 30 Haziran 2017 ' den sonraki sanal dizilerde depolama kullanımı izlemenin kullanılmasına devam etmesine izin veren güncelleştirmeler vardır.|
| 3 |Dosya sunucusu| Önceki sürümlerde, bir kullanıcı şifrelenmiş dosyaları yanlışlıkla sanal diziye kopyalayabilir. Bu sürüm, şifrelenmiş dosyaların sanal diziye kopyalanmasına izin verilmeyen bir onarım içerir. Cihazınızda güncelleştirme öncesinde mevcut şifrelenmiş dosyalar varsa, tüm şifreli dosyalar sistemden silinene kadar yedeklemeler başarısız olmaya devam eder. |


## <a name="known-issues-in-the-update-05"></a>Güncelleştirme 0,5 ' de bilinen sorunlar

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
| **240.** |Paylaşımlar için kullanılan kapasite |Paylaşımda veri olmadığında paylaşma tüketimi görebilirsiniz. Bu tüketim, paylaşımlar için kullanılan kapasitenin meta verileri içermesine yöneliktir. | |
| **tuşlarına.** |Olağanüstü durum kurtarma |Yalnızca bir dosya sunucusunun olağanüstü durum kurtarma işlemini kaynak cihazla aynı etki alanına gerçekleştirebilirsiniz. Bu sürümde, başka bir etki alanındaki hedef cihaza olağanüstü durum kurtarma desteklenmiyor. |Bu, sonraki bir sürümde uygulanır. Daha fazla bilgi için, [StorSimple Sanal diziniz Için yük devretme ve olağanüstü durum kurtarma](storsimple-virtual-array-failover-dr.md) bölümüne gidin |
| **(.** |Azure PowerShell |StorSimple Sanal cihazları bu sürümdeki Azure PowerShell tarafından yönetilemez. |Sanal cihazların tüm yönetimi Azure portal ve yerel Web Kullanıcı arabirimi aracılığıyla yapılmalıdır. |
| **üst.** |Parola değiştirme |Sanal dizi cihaz konsolu yalnızca en-US klavye biçimindeki girişi kabul eder. | |
| **+.** |CHAP |Oluşturulduktan sonra CHAP kimlik bilgileri kaldırılamıyor. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, bu birimleri çevrimdışına almanız ve değişikliğin etkili olması için bunları çevrimiçi duruma getirmeniz gerekir. |Bu sorun, sonraki bir sürümde giderilmiştir. |
| **hatası.** |Iscsı sunucusu |Bir Iscsı birimi için görüntülenmiş ' kullanılan depolama alanı, StorSimple Aygıt Yöneticisi hizmetinde ve Iscsı konağında farklı olabilir. |Iscsı konağında dosya sistemi görünümü bulunur.<br></br>Cihaz, birim en büyük boyuttan ayrıldığı blokları görür. |
| **May.** |Dosya sunucusu |Bir klasördeki bir dosya ile ilişkili alternatif veri akışı (ADS) varsa, reklamlar olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde kurtarma aracılığıyla yedeklenmez veya geri yüklenmez. | |
| **aşamaz.** |Dosya sunucusu |Sembolik bağlantılar desteklenmiyor. | |
| **k.** |Dosya sunucusu |StorSimple Sanal dizisi dosya sunucusu üzerinde kopyalanırken veya depolanırken Windows şifreleme dosya sistemi (EFS) tarafından korunan dosyalar desteklenmeyen bir yapılandırmayla sonuçlanır.  | |

## <a name="next-step"></a>Sonraki adım
StorSimple Sanal diziniz üzerinde [0,5 güncelleştirmesini yükler](storsimple-virtual-array-install-update-05.md) .

## <a name="references"></a>Başvurular
Daha eski bir sürüm notuna mi bakıyorsunuz? Şuraya gidin:

* [StorSimple Sanal dizisi güncelleştirme 0,4 sürüm notları](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple Sanal dizisi güncelleştirme 0,3 sürüm notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple Sanal dizisi güncelleştirme 0,1 ve 0,2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple Sanal dizisi genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)

