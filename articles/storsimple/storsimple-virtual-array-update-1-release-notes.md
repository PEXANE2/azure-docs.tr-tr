---
title: StorSimple Sanal dizisi güncelleştirme 1,0 sürüm notları
description: Güncelleştirme 1,0 çalıştıran StorSimple Sanal dizisine yönelik kritik açık sorunlar ve çözümleri açıklar.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: 48dec3a87ab540af224ae4ac59dd37cee7c9d0ee
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76271334"
---
# <a name="storsimple-virtual-array-update-10-release-notes"></a>StorSimple Sanal dizisi güncelleştirme 1,0 sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, Microsoft Azure StorSimple Sanal dizi güncelleştirmelerine yönelik kritik açık sorunları ve çözümlenen sorunları belirler.

Sürüm notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar keşfedildiğinde eklenirler. StorSimple Sanal dizinizi dağıtmadan önce, sürüm notlarında bulunan bilgileri dikkatle gözden geçirin.

Güncelleştirme 1,0, yazılım sürümü **10.0.10296.0**'e karşılık gelir.

> [!IMPORTANT]
> - Güncelleştirmeler kesintiye uğratan ve cihazınızı yeniden başlatacak. G/ç devam ediyorsa, cihaz kapalı kalma süresini doğurur. Güncelleştirmeyi uygulama hakkında ayrıntılı yönergeler için [güncelleştirme 1,0](storsimple-virtual-array-install-update-1.md)' a gidin.
>
> - Güncelleştirme 1 yalnızca cihazınız güncelleştirme 0,6 çalıştırıyorsa Azure portal aracılığıyla kullanılabilir.

## <a name="whats-new-in-update-10"></a>Güncelleştirme 1,0 ' deki yenilikler

**Güncelleştirme 1,0, StorSimple Aygıt Yöneticisi hizmeti 'nin kimlik doğrulamasıyla ilgili değişiklikler içerir ve en kısa zamanda dağıtılmalıdır.** Bu güncelleştirme aşağıdaki geliştirmeleri ve hata düzeltmelerini içerir:

 - **Storsimple Aygıt Yöneticisi hizmeti ile kimlik doğrulamak için Azure Active Directory (AAD) kullanımı** : güncelleştirme 1,0 ' den sonraki sürümlerde, storsimple Aygıt Yöneticisi hizmetinde kimlik doğrulaması yapmak için Azure Active Directory kullanılır. Eski kimlik doğrulama mekanizması, Aralık 2017 tarafından kullanım dışı bırakılacak. Tüm kullanıcıların güvenlik duvarı kurallarında yeni kimlik doğrulama URL 'Leri içermesi gerekir. Daha fazla bilgi için, [StorSimple Sanal diziniz Için ağ gereksinimleri](storsimple-ova-system-requirements.md)' nde listelenen kimlik doğrulama URL 'lerine gidin.
 
    Kimlik doğrulama URL 'SI güvenlik duvarı kurallarında yoksa, kullanıcılar StorSimple cihazının hizmetten kimlik doğrulayamadığından önemli bir uyarı görür. Kullanıcılar bu uyarıyı görtiklerinde, yeni kimlik doğrulama URL 'sini içermesi gerekir. Daha fazla bilgi için, [StorSimple ağ uyarıları](storsimple-virtual-array-manage-alerts.md)' na gidin.

 - **Performans geliştirme** -bulut okumalarının, katman ve katman aşımlarının hızını artırmak için çeşitli hata düzeltmeleri yapıldı. Sonuç olarak, hem yedekleme hem de geri yükleme performansı Iscsı ve dosya sunucusu cihazları için geliştirilmiştir.

 - **Çöp toplama geliştirmesi** -bu sürümde, cihaz ve depolama hesabı iki uzak bölgede olduğunda çöp toplama döngüsünün performansını artıran hata düzeltmeleri vardır.

 - **Günlük geliştirme** -bu sürüm, çöp toplama ve g/ç yoluyla ilgili günlüğe kaydetme geliştirmeleri içerir.


## <a name="issues-fixed-in-update-10"></a>Güncelleştirme 1,0 ' de düzeltilen sorunlar

Aşağıdaki tabloda, bu sürümde düzeltilen sorunların bir özeti verilmiştir.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |AAD tabanlı kimlik doğrulaması| Bu sürüm, AAD 'nin StorSimple Aygıt Yöneticisi kimlik doğrulamasına izin veren değişiklikler içerir.|
| 2 |Atık toplama| Bu sorun, cihaz ve depolama hesaplarının farklı bölgelerde olduğu ve müşterinin zaman aralıklı ağ hataları bildirdiği ve bu sayede faturalandırmayı etkileyebilecek bir müşteri sitesinde bildirilmiştir. Bu sürümde, bu sorun düzeltildi. |
| 3 |Performans| Bu sürüm, geri yükleme/bulut okuma/katmanı/katman performansı iyileştirmesinin sonucu olan değişiklikleri içerir.|
| 4 |Güncelleştir| Önceki sürümde, bir müşteri sitesindeki yedekleme hatalarıyla sonuçlanan güncelleştirmede bir sorun oluştu. Bu sorun bu sürümde düzeltilmiştir.|

## <a name="known-issues-in-update-10"></a>Güncelleştirme 1,0 ' de bilinen sorunlar

Aşağıdaki tabloda, StorSimple Sanal dizisi için bilinen sorunların özeti verilmiştir ve önceki sürümlerden gelen sorunlar sürümü dahildir.

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamalar |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal diziler desteklenen bir genel kullanılabilirlik sürümüne güncelleştirilemez. |Bu sanal diziler, olağanüstü durum kurtarma (DR) iş akışı kullanan genel kullanılabilirlik sürümü için yük devretmelidir. |
| **2.** |Sağlanan veri diski |Belirtilen boyutta bir veri diski sağladıktan ve karşılık gelen StorSimple Sanal dizisini oluşturduktan sonra, veri diskini genişletmemelidir veya küçültmelisiniz. Bu durum, cihazın yerel katmanlarında tüm verilerin kaybedilmesine neden olur. | |
| **3.** |Grup İlkesi |Bir cihaz etki alanına katılmış olduğunda, bir grup ilkesi uygulandığında cihaz işlemi olumsuz etkilenebilir. |Sanal diziniz Active Directory için kendi kuruluş biriminde (OU) olduğundan ve buna hiçbir Grup İlkesi nesnesi (GPO) uygulanmadığından emin olun. |
| **4.** |Yerel Web Kullanıcı arabirimi |Internet Explorer 'da (IE ESC) Artırılmış güvenlik özellikleri etkinse, sorun giderme veya bakım gibi bazı yerel Web Kullanıcı arabirimi sayfaları düzgün çalışmayabilir. Bu sayfalardaki düğmeler de çalışmayabilir. |Internet Explorer 'da gelişmiş güvenlik özelliklerini devre dışı bırakın. |
| **e.** |Yerel Web Kullanıcı arabirimi |Hyper-V sanal makinesinde, Web Kullanıcı arabirimindeki ağ arabirimleri 10 Gbps arabirim olarak görüntülenir. |Bu davranış, Hyper-V ' y i bir yansıdır. Hyper-V, sanal ağ bağdaştırıcıları için her zaman 10 Gbps gösterir. |
| **inç.** |Katmanlı birimler veya paylaşımlar |StorSimple katmanlı birimlerle çalışan uygulamalar için bayt aralığı kilitleme desteklenmez. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen ölçüler şunlardır: <br></br>Uygulama mantığınızdaki bayt aralığı kilitlemeyi devre dışı bırakın.<br></br>Bu uygulamaya yönelik verileri katmanlı birimlerin aksine yerel olarak sabitlenmiş birimlerde koymak için seçin.<br></br>*Desteklenmediği uyarısıyla*: yerel olarak sabitlenmiş birimler kullanılırken ve bayt aralığı kilitleme etkinleştirildiğinde, geri yükleme tamamlanmadan önce yerel olarak sabitlenmiş birim çevrimiçi olabilir. Bu tür örneklerde, bir geri yükleme işlemi devam ediyorsa geri yüklemenin tamamlanmasını beklemeniz gerekir. |
| **7.** |Katmanlı paylaşımlar |Büyük dosyalarla çalışma, yavaş katmana neden olabilir. |Büyük dosyalarla çalışırken, en büyük dosyanın paylaşma boyutunun %3 ' inden küçük olmasını öneririz. |
| **240.** |Paylaşımlar için kullanılan kapasite |Paylaşımda veri olmadığında paylaşma tüketimi görebilirsiniz. Bu tüketim, paylaşımlar için kullanılan kapasitenin meta verileri içermesine yöneliktir. | |
| **tuşlarına.** |Olağanüstü durum kurtarma |Yalnızca bir dosya sunucusunun olağanüstü durum kurtarma işlemini kaynak cihazla aynı etki alanına gerçekleştirebilirsiniz. Bu sürümde, başka bir etki alanındaki hedef cihaza olağanüstü durum kurtarma desteklenmiyor. |Bu, sonraki bir sürümde uygulanır. Daha fazla bilgi için, [StorSimple Sanal diziniz Için yük devretme ve olağanüstü durum kurtarma](storsimple-virtual-array-failover-dr.md) bölümüne gidin |
| **(.** |Azure PowerShell |StorSimple Sanal dizileri, bu sürümdeki Azure PowerShell tarafından yönetilemez. |Sanal cihazların tüm yönetimi Azure portal ve yerel Web Kullanıcı arabirimi aracılığıyla yapılmalıdır. |
| **üst.** |Parola değiştirme |Sanal dizi cihaz konsolu yalnızca en-US klavye biçimindeki girişi kabul eder. | |
| **+.** |CHAP |Oluşturulduktan sonra CHAP kimlik bilgileri kaldırılamıyor. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, bu birimleri çevrimdışına almanız ve değişikliğin etkili olması için bunları çevrimiçi duruma getirmeniz gerekir. |Bu sorun, sonraki bir sürümde giderilmiştir. |
| **hatası.** |Iscsı sunucusu |Bir Iscsı birimi için görüntülenmiş ' kullanılan depolama alanı, StorSimple Aygıt Yöneticisi hizmetinde ve Iscsı konağında farklı olabilir. |Iscsı konağında dosya sistemi görünümü bulunur.<br></br>Cihaz, birim en büyük boyuttan ayrıldığı blokları görür. |
| **May.** |Dosya sunucusu |Bir klasördeki bir dosya ile ilişkili alternatif veri akışı (ADS) varsa, reklamlar olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde kurtarma aracılığıyla yedeklenmez veya geri yüklenmez. | |
| **aşamaz.** |Dosya sunucusu |Sembolik bağlantılar desteklenmiyor. | |
| **k.** |Dosya sunucusu |StorSimple Sanal dizisi dosya sunucusu üzerinde kopyalanırken veya depolanırken Windows şifreleme dosya sistemi (EFS) tarafından korunan dosyalar desteklenmeyen bir yapılandırmayla sonuçlanır.  | |
| **17.** |Güncelleştirmeler |Yerel kullanıcı arabiriminden bir düzeltme yüklemeye çalışırken hata kodu: 2359302 (onaltılı 0x240006) görürseniz, bu düzeltmenin cihazınızda zaten yüklü olduğunu gösterir.   | |
| **18.** |Güncelleştirmeler |Sanal dizinize güncelleştirme 1 ' i yüklemek için yerel Web Kullanıcı arabirimini kullanıyorsanız güncelleştirme 0,6 ' i çalıştırıyor olduğunuzdan emin olmalısınız. Güncelleştirme 0,6 ' den daha düşük bir sürüm çalıştırıyorsanız, önce güncelleştirme 0,6 ' i yüklemeli ve ardından güncelleştirme 1 ' i uygulamanız gerekir. Güncelleştirme 1,0 ' yi güncelleştirme öncesi 0,6 sürümünden doğrudan yüklerseniz bazı güncelleştirmeleri kaçırır ve izleme grafikleri çalışmaz.   | |


## <a name="next-steps"></a>Sonraki adımlar
StorSimple Sanal diziniz üzerinde [1,0 güncelleştirmesini yükler](storsimple-virtual-array-install-update-1.md) .

## <a name="references"></a>Başvurular
Daha eski bir sürüm notuna mi bakıyorsunuz? Şuraya gidin:
*  [StorSimple Sanal dizisi güncelleştirme 0,6 sürüm notları](storsimple-virtual-array-update-06-release-notes.md)
* [StorSimple Sanal dizisi güncelleştirme 0,5 sürüm notları](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple Sanal dizisi güncelleştirme 0,4 sürüm notları](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple Sanal dizisi güncelleştirme 0,3 sürüm notları](storsimple-ova-update-03-release-notes.md)
* [StorSimple Sanal dizisi güncelleştirme 0,1 ve 0,2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple Sanal dizisi genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)
