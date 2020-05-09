---
title: Sorun giderme
description: Azure uzaktan Işleme ilgili sorun giderme bilgileri
author: florianborn71
ms.author: flborn
ms.date: 02/25/2020
ms.topic: troubleshooting
ms.openlocfilehash: c1b807c6e4fa269ac2ab8d7eacd3ca1d4f81a1ca
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792624"
---
# <a name="troubleshoot"></a>Sorun giderme

Bu sayfada, Azure uzaktan Işleme ile müdahale eden genel sorunlar ve bunları çözme yolları listelenmektedir.

## <a name="client-cant-connect-to-server"></a>İstemci sunucuya bağlanamıyor

Güvenlik duvarlarınızın (cihazda, yönlendiricilerin içinde, vb.) aşağıdaki bağlantı noktalarını engellemediğinden emin olun:

* **50051 (TCP)** -ilk bağlantı için GEREKLIDIR (http el sıkışması)
* **8266 (TCP + UDP)** -veri aktarımı için gereklidir
* **5000 (TCP)**, **5433 (tcp)**, **8443 (TCP)** - [ArrInspector](tools/arr-inspector.md) için gereklidir

## <a name="error-disconnected-videoformatnotavailable"></a>' Bağlantısı kesildi: VideoFormatNotAvailable ' hatası

GPU 'nun donanım video kodunu çözmeyi destekleyip desteklemediğini denetleyin. Bkz. [GELIŞTIRME bilgisayarı](../overview/system-requirements.md#development-pc).

İki GPU içeren bir dizüstü bilgisayarda çalışıyorsanız, varsayılan olarak üzerinde çalıştırdığınız GPU, donanım video kod çözme işlevselliği sağlamaz. Bu durumda, uygulamanızı diğer GPU 'YU kullanmaya zorlamaya çalışın. Bu durum genellikle GPU sürücü ayarlarında mümkündür.

## <a name="h265-codec-not-available"></a>H265 codec bileşeni kullanılamıyor

Sunucunun bir **codec bileşeni kullanılamıyor** hatası ile bağlanmayı reddedebileceği iki neden vardır.

**H265 codec bileşeni yüklü değil:**

İlk olarak, sistem gereksinimlerinin [yazılım](../overview/system-requirements.md#software) bölümünde belirtildiği gibi **HEVC video uzantılarını** yüklediğinizden emin olun.

Hala sorun yaşıyorsanız, grafik kartınızın h265 desteklediğinden ve en son grafik sürücüsünün yüklü olduğundan emin olun. Satıcıya özgü bilgiler için sistem gereksinimlerinin [GELIŞTIRME bilgisayarı](../overview/system-requirements.md#development-pc) bölümüne bakın.

**Codec bileşeni yüklendi, ancak kullanılamıyor:**

Bu sorunun nedeni, dll 'lerde yanlış bir güvenlik ayarıdır. Bu sorun, h265 ile kodlanan videoları izlerken bildirim yapmaz. Codec bileşeninin yeniden yüklenmesi sorunu çözmez. Bunun yerine, aşağıdaki adımları uygulayın:

1. **Yönetici haklarına sahip bir PowerShell** açın ve çalıştırın

    ```PowerShell
    Get-AppxPackage -Name Microsoft.HEVCVideoExtension
    ```
  
    Bu komut, codec bileşenini `InstallLocation` şöyle bir şekilde çıktı almalıdır:
  
    ```cmd
    InstallLocation   : C:\Program Files\WindowsApps\Microsoft.HEVCVideoExtension_1.0.23254.0_x64__5wasdgertewe
    ```

1. Bu klasörü Windows Gezgini 'nde aç
1. **X86** ve **x64** alt klasörü olmalıdır. Klasörlerden birine sağ tıklayın ve **Özellikler** ' i seçin
    1. **Güvenlik** sekmesini seçin ve **Gelişmiş** Ayarlar düğmesine tıklayın
    1. **Sahip** için **Değiştir** 'e tıklayın
    1. Metin alanına **yönetici** yazın
    1. **Adları denetle** ve **Tamam 'a** tıklayın
1. Diğer klasör için yukarıdaki adımları yineleyin
1. Ayrıca, yukarıdaki adımları her iki klasörün içindeki her DLL dosyasında da yineleyin. Tamamen dört dll olmalıdır.

Ayarların artık doğru olduğunu doğrulamak için, bunu dört dll 'nin her biri için yapın:

1. **Özellikleri seçin > güvenlik > Düzenle**
1. Tüm **gruplar/kullanıcılar** listesini okuyun ve her birinin **Oku & Çalıştır** sağ ayarlanmış olduğundan emin olun ( **izin verme** sütunundaki onay işareti oluşturulmalıdır)

## <a name="low-video-quality"></a>Düşük video kalitesi

Video kalitesi ağ kalitesiyle veya eksik h265 video codec bileşeniyle karşı tehlikeye girebilir.

* [Ağ sorunlarını belirlemek](#unstable-holograms)için adımlara bakın.
* En son grafik sürücüsünü yüklemek için [sistem gereksinimleri](../overview/system-requirements.md#development-pc) bölümüne bakın.

## <a name="video-recorded-with-mrc-does-not-reflect-the-quality-of-the-live-experience"></a>MRC ile kaydedilen video, canlı deneyim kalitesini yansıtmaz

Bir video, [karma gerçeklik yakalama (MRC)](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers)aracılığıyla HoloLens 'te kaydedilebilir. Ancak, sonuçta elde edilen videonun iki nedenden dolayı canlı deneyiminden daha kötü bir kalite vardır:
* Video kare hızı 60 Hz 'in aksine 30 Hz 'e göre belirlenir.
* Video görüntüleri, [geç aşama yeniden projeksiyonu](../overview/features/late-stage-reprojection.md) işleme adımını etkilemez, bu nedenle video choppier gibi görünür.

Her ikisi de kayıt tekniğinin devralınmış kısıtlamalarıdır.

## <a name="black-screen-after-successful-model-loading"></a>Başarılı model yüklendikten sonra siyah ekran

İşleme çalışma zamanına bağlıysanız ve bir modeli başarıyla yüklediyseniz, ancak daha sonra yalnızca bir siyah ekran görürseniz, bu birkaç farklı neden olabilir.

Daha ayrıntılı bir analiz yapmadan önce aşağıdaki şeyleri test etmenizi öneririz:

* H265 codec bileşeni yüklendi mi? H264 codec bileşenine geri dönüş olması gerekse de, bu geri dönüş düzgün şekilde çalışmayan durumlar görülmüştür. En son grafik sürücüsünü yüklemek için [sistem gereksinimleri](../overview/system-requirements.md#development-pc) bölümüne bakın.
* Unity projesi kullanırken Unity 'yi kapatın, proje dizinindeki geçici *kitaplığı* ve *obj* klasörlerini silin ve projeyi yeniden yükleyin/derleyin. Bazı durumlarda önbelleğe alınmış veriler, örnek olarak belirgin bir nedenle düzgün şekilde çalışmayabilir.

Bu iki adım yardımcı olduysa, video çerçevelerinin istemci tarafından alınıp alınmadığını öğrenmek gerekir. Bu, [sunucu tarafı performans sorguları](../overview/features/performance-queries.md) bölümünde açıklandığı gibi programlı bir şekilde sorgulanabilir. , `FrameStatistics struct` Kaç video karesinin alındığını gösteren bir üyeye sahiptir. Bu sayı 0 ' dan büyükse ve zaman içinde arttırıldığında, istemci sunucudan gerçek video çerçevelerini alır. Sonuç olarak, istemci tarafında bir sorun olması gerekir.

### <a name="common-client-side-issues"></a>Ortak istemci tarafı sorunları

**Model, seçilen VM 'nin sınırlarını, özellikle de en yüksek sayıda poligonu aşıyor:**

Bkz. belirli [VM boyut sınırlamaları](../reference/limits.md#overall-number-of-polygons).

**Model Frustum görünümü içinde değil:**

Çoğu durumda, model doğru şekilde görüntülenir, ancak kameranın dışında bulunur. Yaygın bir nedenden dolayı modelin, kameranın en kırpma düzlemine göre kırpılması için, modelin en dışı bir özete aktarılmasının yaygın bir nedeni vardır. Modelin sınırlayıcı kutusunun programlı bir şekilde sorgulanmasına ve kutuyu bir satır kutusu olarak görselleştirmeye veya değerlerini hata ayıklama günlüğüne yazdırmaya yardımcı olur.

Dönüştürme işleminin yanı sıra, dönüştürülmüş modelle birlikte bir [çıktı JSON dosyası](../how-tos/conversion/get-information.md) da oluşturur. Model konumlandırma sorunlarını ayıklamak için, `boundingBox` [outputstatıstıcs bölümündeki](../how-tos/conversion/get-information.md#the-outputstatistics-section)girişe göz uydurulmaya değecektir:

```JSON
{
    ...
    "outputStatistics": {
        ...
        "boundingBox": {
            "min": [
                -43.52,
                -61.775,
                -79.6416
            ],
            "max": [
                43.52,
                61.775,
                79.6416
            ]
        }
    }
}
```

Sınırlayıcı kutu, ölçüm olarak 3B alanında `min` bir `max` ve konumu olarak tanımlanır. Bu nedenle 1000,0 koordinatı, kaynağın kaynağın 1 kilometreden fazla olması anlamına gelir.

Bu sınırlama kutusuyla ilgili olarak görünmeyen geometriye yol açabilecek iki sorun olabilir:
* **Kutu**, en çok orta basıyor olabilir, bu nedenle nesne, uzak düzlem kırpması nedeniyle tamamen kırpıldı. Bu `boundingBox` örnekte yer alan değerler şöyle görünür: `min = [-2000, -5,-5], max = [-1990, 5,5]`, burada örnek olarak x ekseninde büyük bir konum kullanılıyor. Bu tür bir sorunu çözmek için `recenterToOrigin` [model dönüştürme yapılandırmasındaki](../how-tos/conversion/configure-model-conversion.md)seçeneği etkinleştirin.
* **Kutu ortalanmış, ancak çok büyük boyutlu siparişler olabilir**. Bu, kameranın modelin merkezinde başladığı anlamına gelir; bu, geometrisi her yönde kırpılır. Bu `boundingBox` örnekte tipik değerler şuna benzer: `min = [-1000,-1000,-1000], max = [1000,1000,1000]`. Bu tür bir sorunun nedeni genellikle bir birim ölçek uyuşmazlığıdır. Dengelemek için, [dönüştürme sırasında bir ölçekleme değeri](../how-tos/conversion/configure-model-conversion.md#geometry-parameters) belirtin veya kaynak modeli doğru birimlerle işaretleyin. Ölçek, çalışma zamanında model yüklenirken kök düğümüne da uygulanabilir.

**Unity oluşturma işlem hattı işleme kancalarını içermez:**

Azure uzaktan Işleme, video ile çerçeve oluşturmayı ve yeniden projeksiyonu yapmak için Unity işleme ardışık düzenine takılır. Bu kancalarının mevcut olduğunu doğrulamak için, *> Analysis > Frame Debugger menü penceresini*açın. Bunu etkinleştirin ve ardışık düzende `HolographicRemotingCallbackPass` ' ın iki girişi olduğundan emin olun:

![Unity çerçevesi hata ayıklayıcısı](./media/troubleshoot-unity-pipeline.png)

## <a name="unity-code-using-the-remote-rendering-api-doesnt-compile"></a>Uzaktan Işleme API 'sini kullanan Unity kodu derlenmiyor

### <a name="use-debug-when-compiling-for-unity-editor"></a>Unity Düzenleyicisi için derlerken hata ayıklamayı kullan

Unity çözümünün *derleme türünü* **hata ayıklaması**için değiştirin. Unity düzenleyicisinde ARR testi yaparken, tanımlama `UNITY_EDITOR` yalnızca ' Debug ' yapılarında kullanılabilir. Bu, [dağıtılan uygulamalar](../quickstarts/deploy-to-hololens.md)için kullanılan yapı türü ile ilgili değildir ve burada ' Release ' derlemelerini tercih etmelisiniz.

### <a name="compile-failures-when-compiling-unity-samples-for-hololens-2"></a>HoloLens 2 için Unity örnekleri derlenirken derleme sorunları

HoloLens 2 için Unity örneklerini (hızlı başlangıç, ShowCaseApp,..) derlemeye çalışırken silatik arızalarıyla karşılaştık. Visual Studio şikayet eden bazı dosyaları burada kopyalayamıyorum. Bu soruna ulaşırsanız:
* Tüm geçici Unity dosyalarını projeden kaldırın ve yeniden deneyin.
* Kopyalama adımı bazen uzun dosya adlarıyla sorunlar halinde çalıştırıldıklarından, projelerin olabildiğince kısa yola sahip bir diskte dizinde bulunduğundan emin olun.
* Bu yardım yoksa, MS Sense kopyalama adımını kesintiye uğratır. Bir özel durum ayarlamak için komut satırından bu kayıt defteri komutunu çalıştırın (yönetici hakları gerektirir):
    ```cmd
    reg.exe ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows Advanced Threat Protection" /v groupIds /t REG_SZ /d "Unity”
    ```
    
## <a name="unstable-holograms"></a>Dengesiz Hologragram

İşlenen nesnelerin baş hareketlerle birlikte taşınması gibi görünse de, *geç aşama yeniden projeksiyonu* (LSR) ile ilgili sorunlarla karşılaşıyor olabilirsiniz. Böyle bir duruma yaklaşıma ilişkin yönergeler için, [geç aşama yeniden projeksiyonunun](../overview/features/late-stage-reprojection.md) bölümüne bakın.

Kararsız hologragram (Wobbling, warping, su ya da atlama hologramlar) için başka bir neden, ağ bağlantısı yetersizliğinde veya çok yüksek gecikme süresine sahip olabilir. Ağ bağlantınızın kalitesi için iyi bir gösterge, [performans istatistikleri](../overview/features/performance-queries.md) değeridir `ARRServiceStats.VideoFramesReused`. Yeniden kullanılan çerçeveler, yeni video çerçevesi kullanılabilir olmadığından (örneğin, paket kaybı nedeniyle veya ağ gecikmede Çeşitlemeler nedeniyle), eski video çerçevesinin istemci tarafında yeniden kullanılması gereken durumları gösterir. `ARRServiceStats.VideoFramesReused` Sıfırdan sık büyükse bu bir ağ sorununu gösterir.

Bakmak için başka bir değer de `ARRServiceStats.LatencyPoseToReceiveAvg`vardır. Sürekli olarak 100 ms 'nin altında olması gerekir. Daha yüksek değerler görürseniz, bu, çok uzakta olan bir veri merkezine bağlı olduğunu gösterir.

Olası azaltmaları bir liste için bkz. [ağ bağlantısı yönergeleri](../reference/network-requirements.md#guidelines-for-network-connectivity).

## <a name="next-steps"></a>Sonraki adımlar

* [Sistem gereksinimleri](../overview/system-requirements.md)
* [Ağ gereksinimleri](../reference/network-requirements.md)
