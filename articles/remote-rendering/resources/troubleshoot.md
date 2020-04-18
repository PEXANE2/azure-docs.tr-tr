---
title: Sorun giderme
description: Azure Uzaktan İşleme için sorun giderme bilgileri
author: florianborn71
ms.author: flborn
ms.date: 02/25/2020
ms.topic: troubleshooting
ms.openlocfilehash: ac7e721a863414cf0617177885e0ff1c9e9a35d4
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617858"
---
# <a name="troubleshoot"></a>Sorun giderme

Bu sayfa, Azure Uzaktan İşleme'yi engelleyen yaygın sorunları ve bunları çözme yollarını listeler.

## <a name="client-cant-connect-to-server"></a>İstemci sunucuya bağlanamıyor

Güvenlik duvarlarınızın (aygıtta, yönlendiricilerin içinde vb.) aşağıdaki bağlantı noktalarını engellemediğinden emin olun:

* **50051 (TCP)** - ilk bağlantı için gerekli (HTTP el sıkışma)
* **8266 (TCP+UDP)** - veri aktarımı için gerekli
* **5000 (TCP)**, **5433 (TCP)**, **8443 (TCP)** - [ArrInspector](tools/arr-inspector.md) için gerekli

## <a name="error-disconnected-videoformatnotavailable"></a>Hata 'Bağlantısı Kesildi: VideoFormatNotAvailable'

GPU'nuzun donanım video çözmeyi desteklediğini kontrol edin. [Bkz. Geliştirme PC.](../overview/system-requirements.md#development-pc)

İki GPU'lu bir dizüstü bilgisayarüzerinde çalışıyorsanız, varsayılan olarak üzerinde çalıştığınız GPU'nun donanım video çözme işlevini sağlamaması mümkündür. Bu nedenle, uygulamanızı diğer GPU'yu kullanmaya zorlamaya çalışın. Bu genellikle GPU sürücü ayarlarında mümkündür.

## <a name="h265-codec-not-available"></a>H265 codec kullanılamıyor

Sunucunun **codec'de bulunmayan** bir hataya bağlanmayı reddetmesinin iki nedeni vardır.

**H265 codec yüklü değil:**

Öncelikle sistem gereksinimlerinin [Yazılım](../overview/system-requirements.md#software) bölümünde belirtildiği gibi **HEVC Video Uzantılarını** yüklediğinizden emin olun.

Hala sorunlarla karşılaşırsanız, grafik kartınızın H265'i desteklediğinden ve en son grafik sürücüsünün yüklü olduğundan emin olun. Satıcıya özel bilgiler için sistem gereksinimlerinin [Geliştirme PC](../overview/system-requirements.md#development-pc) bölümüne bakın.

**Codec yüklenir, ancak kullanılamaz:**

Bu sorunun nedeni, DL'lerde yanlış bir güvenlik ayarıdır. H265 ile kodlanmış videoları izlemeye çalışırken bu sorun ortaya değildir. Codec'i yeniden yüklemek de sorunu çözmez. Bunun yerine, aşağıdaki adımları gerçekleştirin:

1. Yönetici **haklarıyla** bir PowerShell açın ve çalıştırın

    ```PowerShell
    Get-AppxPackage -Name Microsoft.HEVCVideoExtension
    ```
  
    Bu komut codec, gibi bir şey çıktı `InstallLocation` gerekir:
  
    ```cmd
    InstallLocation   : C:\Program Files\WindowsApps\Microsoft.HEVCVideoExtension_1.0.23254.0_x64__5wasdgertewe
    ```

1. Windows Gezgini'nde bu klasörü açma
1. Bir **x86** ve bir **x64** alt klasörü olmalıdır. Klasörlerden birine sağ tıklayın ve **Özellikler'i** seçin
    1. **Güvenlik** sekmesini seçin ve **Gelişmiş** ayarlar düğmesini tıklatın
    1. **Sahibi** için **Değiştir'e** tıklayın
    1. Metin alanına **Yöneticiler** yazın
    1. **Adları Ve Tamam'ı** tıklatın **OK**
1. Diğer klasör için yukarıdaki adımları yineleme
1. Ayrıca, her iki klasör içinde her DLL dosyasında yukarıdaki adımları yineleyin. Toplamda dört DL olmalıdır.

Ayarların artık doğru olduğunu doğrulamak için, dört DL'nin her biri için bunu yapın:

1. **Özellikler > Güvenlik >'ni Seçin**
1. Tüm Grupların / **Kullanıcıların** listesini gözden geçirin ve her birinin **Okuma & Yürüt** sağ kümesine sahip olduğundan emin olun **(izin sütunundaki** onay işareti işaretlenmelidir)

## <a name="low-video-quality"></a>Düşük video kalitesi

Video kalitesi ağ kalitesi veya eksik H265 video codec tarafından tehlikeye olabilir.

* Ağ sorunlarını tanımlamak için gereken [adımlara](#unstable-holograms)bakın.
* En son grafik sürücüsünü yüklemek için [sistem gereksinimlerine](../overview/system-requirements.md#development-pc) bakın.

## <a name="video-recorded-with-mrc-does-not-reflect-the-quality-of-the-live-experience"></a>MRC ile kaydedilen video canlı deneyimin kalitesini yansıtmaz

Bir video Karışık Gerçeklik Yakalama [(MRC)](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers)ile Hololens kaydedilebilir. Ancak ortaya çıkan video iki nedenden dolayı canlı deneyim daha kötü kaliteye sahiptir:
* Video kare hızı 60 Hz yerine 30 Hz olarak kapatılır.
* Video görüntüleri geç aşama [yeniden projeksiyon](../overview/features/late-stage-reprojection.md) işleme adımından geçmez, bu nedenle video daha doğranır gibi görünür.

Her ikisi de kayıt tekniğinin doğal sınırlamalarıdır.

## <a name="black-screen-after-successful-model-loading"></a>Başarılı model yüklendikten sonra siyah ekran

Render çalışma süresine bağlıysanız ve bir modeli başarıyla yüklediyseniz, ancak daha sonra yalnızca siyah bir ekran görüyorsanız, bunun birkaç farklı nedeni olabilir.

Daha ayrıntılı bir analiz yapmadan önce aşağıdaki şeyleri test etmenizi öneririz:

* H265 codec yüklü mü? H264 codec bir geri dönüş olması gerekir rağmen, biz bu geri dönüş düzgün çalışmadı durumlarda gördük. En son grafik sürücüsünü yüklemek için [sistem gereksinimlerine](../overview/system-requirements.md#development-pc) bakın.
* Birlik projesini kullanırken, Unity'yi kapatın, proje dizinindeki geçici *kitaplık* ve *obj* klasörlerini silin ve projeyi yeniden yükleyin/oluşturun. Bazı durumlarda önbelleğe alınan veriler, örneğin belirgin bir neden olmadan düzgün çalışmamasına neden oldu.

Bu iki adım yardımcı olmadıysa, video karelerinin istemci tarafından alınıp alınmadığını öğrenmek gerekir. Bu, [sunucu tarafı performans sorguları](../overview/features/performance-queries.md) bölümünde açıklandığı gibi programlı olarak sorgulanabilir. Kaç `FrameStatistics struct` video çerçevesi alındığını gösteren bir üye vardır. Bu sayı 0'dan büyükse ve zaman içinde artıyorsa, istemci sunucudan gerçek video kareleri alır. Sonuç olarak istemci tarafında bir sorun olmalıdır.

### <a name="common-client-side-issues"></a>Ortak istemci tarafı sorunları

**Model görünümü frustum içinde değildir:**

Birçok durumda, model doğru görüntülenir ama kamera frustum dışında yer alır. Ortak bir nedeni modeli kameranın uzak kırpma düzlemi tarafından kırpılmış böylece çok off-center pivot ile ihraç edilmiş olmasıdır. Modelin sınırlayıcı kutusunu programlı olarak sorgulamaya ve kutuyu unity ile bir satır kutusu olarak görselleştirmeye veya değerlerini hata ayıklama günlüğüne yazdırmaya yardımcı olur.

**Unity render ardışık hattı render kancalarını içermez:**

Azure Uzaktan İşleme, videoyla çerçeve kompozisyonu yapmak ve yeniden projeksiyon yapmak için Birliğe dönüştürme ardışık hattını bağlar. Bu kancaların var olduğunu doğrulamak *için, Çerçeve hata ayıklayıcı> Çözümleme >* menüsünü açın. Etkinleştirin ve ardışık hatlar `HolographicRemotingCallbackPass` için iki giriş olduğundan emin olun:

![Birlik çerçeve hata ayıklama](./media/troubleshoot-unity-pipeline.png)

## <a name="unity-code-using-the-remote-rendering-api-doesnt-compile"></a>Uzaktan İşleme API'sini kullanarak birlik kodu derlemez

Unity çözümünün *yapı türünü* **Hata**Ayıklama'ya çevirin. Unity düzenleyicisinde ARR test `UNITY_EDITOR` ederken tanım yalnızca 'Hata Ayıklama' yapılarında kullanılabilir. Bunun, 'Release' yapılarını tercih etmeniz gereken [dağıtılmış uygulamalar](../quickstarts/deploy-to-hololens.md)için kullanılan yapı türüyle ilgisi olmadığını unutmayın.

## <a name="unstable-holograms"></a>Kararsız Hologramlar

İşlenen nesnelerin kafa hareketleriyle birlikte hareket ediyor gibi görünmesi durumunda, *Geç Aşama Yeniden Projeksiyonu* (LSR) ile ilgili sorunlarla karşılaşabilirsiniz. Böyle bir duruma nasıl yaklaşılalabilmek için [geç aşama yeniden projeksiyonu](../overview/features/late-stage-reprojection.md) bölümüne bakın.

Kararsız hologramların (sallantı, çözgü, titreme veya atlama hologramları) bir diğer nedeni de zayıf ağ bağlantısı, özellikle yetersiz ağ bant genişliği veya çok yüksek gecikme durumu olabilir. Ağ bağlantınızın kalitesi için iyi bir gösterge `ARRServiceStats.VideoFramesReused`performans [istatistikleri](../overview/features/performance-queries.md) değeridir. Yeniden kullanılan kareler, yeni bir video çerçevesi bulunmadığından istemci tarafında eski bir video çerçevesinin yeniden kullanılması gereken durumları gösterir ( örneğin paket kaybı veya ağ gecikmesi farklılıkları nedeniyle). Sık `ARRServiceStats.VideoFramesReused` sık sıfırdan büyükse, bu bir ağ sorununu gösterir.

Başaçıkolmak için bir diğer değer değer . `ARRServiceStats.LatencyPoseToReceiveAvg` Sürekli olarak 100 ms'nin altında olmalıdır. Daha yüksek değerler görüyorsanız, bu çok uzaktaki bir veri merkezine bağlı olduğunuzu gösterir.

Olası azaltıcı etkenlerin listesi [için ağ bağlantısı yönergelerine](../reference/network-requirements.md#guidelines-for-network-connectivity)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Sistem gereksinimleri](../overview/system-requirements.md)
* [Ağ gereksinimleri](../reference/network-requirements.md)
