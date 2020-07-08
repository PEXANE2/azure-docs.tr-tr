---
title: Azure Kinect bilinen sorunlar ve sorun giderme
description: Azure ınect DK ile algılayıcı SDK kullanırken, bilinen sorunlar ve sorun giderme ipuçları hakkında bilgi edinin.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: sorun giderme, güncelleştirme, hata, Kinect, geri bildirim, kurtarma, günlüğe kaydetme, ipuçları
ms.openlocfilehash: 2db7b17ffc8b6595107dbc52fae719aa9f2d15db
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85277744"
---
# <a name="azure-kinect-known-issues-and-troubleshooting"></a>Azure Kinect bilinen sorunlar ve sorun giderme

Bu sayfa, Azure Kinect DK ile algılayıcı SDK kullanırken oluşan bilinen sorunlar ve sorun giderme ipuçları içerir. Ayrıca ürün donanımlarına özgü sorunlar için [ürün destek sayfalarına](https://aka.ms/kinectsupport) bakın.

## <a name="known-issues"></a>Bilinen sorunlar

- ASMedia USB ana bilgisayar denetleyicileri ile uyumluluk sorunları (örneğin, ASM1142 yonga kümesi)
  - Microsoft USB sürücüsünü kullanan bazı durumlar, engellemeyi kaldırabilir
  - Birçok bilgisayar aynı zamanda alternatif ana bilgisayar denetleyicilerine sahiptir ve USB3 bağlantı noktasını değiştirme konusunda yardımcı olabilir

Daha fazla algılayıcı SDK ile ilgili sorunlar için [GitHub sorunlarını](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/issues) denetleyin

## <a name="collecting-logs"></a>Günlükleri toplama

K4A.dll için günlük oluşturma, ortam değişkenleri aracılığıyla etkinleştirilir. Varsayılan olarak günlük kaydı stdout 'a gönderilir ve yalnızca hatalar ve kritik iletiler oluşturulur. Bu ayarlar, günlüğe kaydetme işleminin bir dosyaya aktarılması için değiştirilebilir. Ayrıntı düzeyi de gerektiğinde ayarlanabilir. Aşağıda, Windows için k4a. log adlı bir dosyaya günlük kaydı etkinleştirmenin yanı sıra uyarı ve daha üst düzey iletileri yakalayan bir örnek verilmiştir.

1. `set K4A_ENABLE_LOG_TO_A_FILE=k4a.log`
2. `set K4A_LOG_LEVEL=w`
3. Komut isteminden senaryo Çalıştır (örneğin, görüntüleyiciyi Başlat)
4. K4a. log dosyasına gidin ve dosyayı paylaşma.

Daha fazla bilgi için bkz. üstbilgi dosyasından klip dosyası:

```console
/**
* environment variables
* K4A_ENABLE_LOG_TO_A_FILE =
*    0    - completely disable logging to a file
*    log\custom.log - log all messages to the path and file specified - must end in '.log' to
*                     be considered a valid entry
*    ** When enabled this takes precedence over the value of K4A_ENABLE_LOG_TO_STDOUT
*
* K4A_ENABLE_LOG_TO_STDOUT =
*    0    - disable logging to stdout
*    all else  - log all messages to stdout
*
* K4A_LOG_LEVEL =
*    'c'  - log all messages of level 'critical' criticality
*    'e'  - log all messages of level 'error' or higher criticality
*    'w'  - log all messages of level 'warning' or higher criticality
*    'i'  - log all messages of level 'info' or higher criticality
*    't'  - log all messages of level 'trace' or higher criticality
*    DEFAULT - log all message of level 'error' or higher criticality
*/
```

Gövde Izleme SDK K4ABT.dll günlüğe kaydetme, kullanıcıların farklı bir ortam değişken adları kümesini değiştirmesi dışında benzerdir:

```console
/**
* environment variables
* K4ABT_ENABLE_LOG_TO_A_FILE =
*    0    - completely disable logging to a file
*    log\custom.log - log all messages to the path and file specified - must end in '.log' to
*                     be considered a valid entry
*    ** When enabled this takes precedence over the value of K4A_ENABLE_LOG_TO_STDOUT
*
* K4ABT_ENABLE_LOG_TO_STDOUT =
*    0    - disable logging to stdout
*    all else  - log all messages to stdout
*
* K4ABT_LOG_LEVEL =
*    'c'  - log all messages of level 'critical' criticality
*    'e'  - log all messages of level 'error' or higher criticality
*    'w'  - log all messages of level 'warning' or higher criticality
*    'i'  - log all messages of level 'info' or higher criticality
*    't'  - log all messages of level 'trace' or higher criticality
*    DEFAULT - log all message of level 'error' or higher criticality
*/
```

## <a name="device-doesnt-enumerate-in-device-manager"></a>Cihaz, Cihaz Yöneticisi 'nde numaralandırılamıyor

- Cihazın arka planda çalıştığını kontrol edin, bu durum yanıp söndüğü için USB bağlantı sorununuz varsa ve yeterli güç gelmiyor. Güç kaynağı kablosunun, belirtilen güç bağdaştırıcısına takılmış olması gerekir. Güç kablosu bağlı bir USB türüne sahip olsa da, cihaz USB bağlantı noktasının sağlayabileceğinden daha fazla güç gerektirir. Bu nedenle, bilgisayara bir bilgisayar bağlantı noktasına veya USB hub 'ına bağlanmayın.
- Güç kablosu bağlı olup olmadığınızı ve veriler için USB3 bağlantı noktası kullandığınızı denetleyin.
- Veri bağlantısı için USB3 bağlantı noktasını değiştirmeyi deneyin (örneğin, PC 'nin arkasında, USB bağlantı noktasını ana karta kapatma önerisi).
- Kablonuzu denetleyin, hasarlı veya daha düşük kaliteli kablolar güvenilir olmayan numaralandırmaya neden olabilir (cihaz, Cihaz Yöneticisi 'nde "yanıp sönen" olarak kalır).
- Dizüstü bilgisayara bağlandıysanız ve pille çalışıyorsanız, bağlantı noktası gücünü azaltmış olabilir.
- Ana bilgisayarı yeniden başlatın.
- Sorun devam ederse, uyumluluk sorunu olabilir.
- Üretici yazılımı güncelleştirmesi sırasında hata oluştuysa ve cihaz kendi kendine kurtarılmıyorsa, [Fabrika Sıfırlaması](https://support.microsoft.com/help/4494277/reset-azure-kinect-dk)gerçekleştirin.

## <a name="azure-kinect-viewer-fails-to-open"></a>Azure Kinect Görüntüleyicisi açılamıyor

- Cihazınızın Windows Aygıt Yöneticisi numaralandırdığından önce kontrol edin.

    ![Windows cihaz yöneticisi 'nde Azure Kinect kameraları](./media/resources/viewer-fails.png)

- Cihazı kullanan başka bir uygulama olup olmadığını denetleyin (örneğin, Windows kamera uygulaması). Tek seferde yalnızca bir uygulama cihaza erişebilir.
- Hata iletileri için k4aviewer. err günlüğünü kontrol edin.
- Windows Kamera uygulamasını açın ve bu uygulamayı denetleyin.
- Güç dönüşü cihazı, cihazı kullanmadan önce akış gücünün kapalı olmasını bekleyin.
- Ana bilgisayarı yeniden başlatın.
- Bilgisayarınızda en son grafik sürücülerini kullandığınızdan emin olun.
- Kendi SDK yapınızı kullanıyorsanız, bu sorunu düzelttiğinde resmi olarak yayınlanan sürümü kullanmayı deneyin.
- Sorun devam ederse, [günlükleri](troubleshooting.md#collecting-logs) ve dosya geri bildirimini toplayın.

## <a name="cannot-find-microphone"></a>Mikrofon bulunamıyor

- Aygıt Yöneticisi ' de mikrofon dizisinin numaralandırılmış olduğunu denetleyin.
- Bir cihaz numaralandırılmışsa ve Windows 'da doğru şekilde çalışırsa, bu sorun, bellenim güncelleştirme Windows 'un derinlik kamerasına farklı kapsayıcı KIMLIĞI atamasından sonra olabilir.
- Aygıt Yöneticisi giderek sıfırlamayı deneyebilirsiniz, "Azure Kinect Microphone Array" öğesine sağ tıklayıp "Cihazı kaldır" ı seçebilirsiniz. Bu tamamlandıktan sonra algılayıcıyı ayırın ve yeniden ekleyin.

    ![Azure Kinect mik dizisi](./media/resources/mic-not-found.png)

- Bundan sonra Azure Kinect görüntüleyicisini yeniden başlattıktan sonra yeniden deneyin.

## <a name="device-firmware-update-issues"></a>Cihaz üretici yazılımı güncelleştirme sorunları

- Güncelleştirme sonrasında doğru sürüm numarası bildirilmezse, cihazı güç dönüşü yapmanız gerekebilir.
- Üretici yazılımı güncelleştirmesi kesintiye uğrarsa, bu durum hatalı duruma gelebilir ve numaralandırılamıyor. Cihazı ayırın ve yeniden bağlayın ve kurtarılamadığında görmek için 60 saniye bekleyin.
Daha sonra [Fabrika Sıfırlaması](https://support.microsoft.com/help/4494277/reset-azure-kinect-dk) gerçekleştirmez

## <a name="image-quality-issues"></a>Görüntü kalitesi sorunları

- [Azure Kinect görüntüleyicisini](azure-kinect-viewer.md) başlatın ve cihazın girişim için konumlandırılmasını veya algılayıcının engellenip engellenmediğini veya lens kirli olduğunu denetleyin.
- Sorun belirli bir modda gerçekleşirse daraltmak için farklı işletim modlarını deneyin.
- Ekiple görüntü kalitesi sorunlarını paylaşmak için şunları yapabilirsiniz:

1) [Azure Kinect görüntüleyicisinde](azure-kinect-viewer.md) duraklatma görünümü al ve bir ekran görüntüsü al veya
2) [Azure Kinect Kaydedicisi](azure-kinect-recorder.md)kullanarak kayıt gerçekleştirin, örneğin`k4arecorder.exe -l 5 -r 5 output.mkv`

## <a name="inconsistent-or-unexpected-device-timestamps"></a>Cihaz zaman damgaları tutarsız veya beklenmedik

Çağırmak, ```k4a_device_set_color_control``` cihaza, sabitlenebilmesi için birkaç yakalama götürebileceği zaman zamanlama değişikliklerini geçici olarak geçirebilir. Her yeni görüntüyle iç zamanlama hesaplamasını sıfırlamadan kaçınmak için görüntü yakalama döngüsünde API 'YI çağırmadan kaçının. Bunun yerine, kamerayı başlatmadan önce veya görüntü yakalama döngüsü içindeki değeri değiştirmek zorunda kalmadan API 'YI çağırın. Özellikle çağrılmasını önleyin ```k4a_device_set_color_control(K4A_COLOR_CONTROL_AUTO_EXPOSURE_PRIORITY)``` .

## <a name="usb3-host-controller-compatibility"></a>USB3 konak denetleyicisi uyumluluğu

Cihaz, Cihaz Yöneticisi altında numaralandırmadıysanız, bunun nedeni desteklenmeyen bir USB3 denetleyicisine takılmış olması olabilir. 

Windows üzerinde Azure Kinect DK, **Intel**, **Texas Instruments (TI)** ve **Renesas** için *desteklenen tek konak denetleyicileridir*. Windows platformlarındaki Azure Kinect SDK 'Sı birleştirilmiş bir kapsayıcı KIMLIĞINI kullanır ve SDK 'nın, fiziksel olarak aynı cihazda bulunan derinlik, renk ve ses cihazlarını bulabilmesi için, USB 2,0 ve 3,0 cihazlarını kapsamalıdır. Linux 'ta, bu platform kapsayıcı KIMLIĞI üzerinde daha az ve cihaz seri numaraları üzerinde daha az kullanıldığı için daha fazla konak denetleyicisi desteklenebilir. 

BILGISAYARDA birden çok konak denetleyicisi yüklü olduğunda, USB ana bilgisayar denetleyicilerinin konusu, daha da karmaşıktır. Ana bilgisayar denetleyicileri karma olduğunda, bir Kullanıcı bazı bağlantı noktalarının iyi çalıştığı ve diğer bir yerde çalışmayan sorunlarla karşılaşabilir. Bağlantı noktalarının bu durum için nasıl kablolu olduğuna bağlı olarak, Azure Kinect ile ilgili sorun yaşayan tüm ön bağlantı noktalarını görebilirsiniz

**Windows:** Hangi konak denetleyicisini açbileceğinizi öğrenmek için Aygıt Yöneticisi

1. Cihazları türe göre görüntüle-> 
2. Azure Kinect bağlı kameraları seçin->Azure Kinect 4K kamera
3. Cihazları bağlantıya göre görüntüle->

![USB bağlantı noktası sorunlarını giderme](./media/resources/usb-troubleshooting.png)

Bilgisayarınızda hangi USB bağlantı noktasının bağlı olduğunu daha iyi anlamak için, Azure Kinect DK 'yi BILGISAYARDAKI farklı USB bağlantı noktalarına bağladığınızda her USB bağlantı noktası için bu adımları tekrarlayın.

## <a name="depth-camera-auto-powers-down"></a>Derinlik Kamerası otomatik güçleri

Görüntü derinliği verilerini hesaplamak için derinlik kamerası tarafından kullanılan lazer, sınırlı bir lifespan içeriyor. Balıklarını ömrünü en üst düzeye çıkarmak için derinlik verilerinin ne zaman tüketilmediğini tespit eder. Cihaz birkaç dakika boyunca akış yaparken derinlik Kamerası güçlendirin, ancak ana bilgisayar, verileri okumaz. Ayrıca, alt cihazların, derinlik kameranın akış olduğu bir durumda çalıştığı ve derinlik çerçevelerinin, ana cihazın yakalamaları eşitlemeye başlamasını beklerken etkin bir şekilde yardım ettiği çoklu cihaz eşitlemesini da etkiler. Çoklu cihaz yakalama senaryolarında bu sorundan kaçınmak için, ana cihazın başlatıldığı ilk alt öğe için bir dakika içinde başladığından emin olun. 

## <a name="using-body-tracking-sdk-with-unreal"></a>Gövde Izleme SDK 'Sı Unreal ile kullanma

Gövde Izleme SDK 'sını Unreal ile birlikte kullanmak için, ortam değişkenine eklediğinizden ve `<SDK Installation Path>\tools` `PATH` ' a `dnn_model_2_0.onnx` ve ' ye kopyaladığınızdan emin `cudnn64_7.dll` olun `Program Files/Epic Games/UE_4.23/Engine/Binaries/Win64` .

## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla destek bilgisi](support.md)
