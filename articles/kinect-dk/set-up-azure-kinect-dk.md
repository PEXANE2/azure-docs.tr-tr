---
title: Hızlı Başlangıç - Azure Kinect DK’yi kurma
description: Bu hızlı başlangıçta Azure Kinect DK donanımını kurma hakkında yönergeler sağlanır
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.topic: quickstart
ms.date: 02/12/2020
keywords: azure, kinect, cihaz seti, azure dk, kurma, donanım, hızlı, usb, güç, görüntüleyici, algılayıcı, akış, kurulum, SDK, üretici yazılımı
ms.custom:
- CI 114092
- CSSTroubleshooting
audience: ITPro
manager: dcscontentpm
ms.localizationpriority: high
ms.openlocfilehash: b33565f319de77385c966f742fa52391f4869eed
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77211287"
---
# <a name="quickstart-set-up-your-azure-kinect-dk"></a>Hızlı Başlangıç: Azure Kinect DK donanımınızı kurma

Bu hızlı başlangıçta Azure Kinect DK donanımınızı kurma yönergeleri sağlanır. Algılayıcı akışı görselleştirmesini test etmeyi ve [Azure Kinect Görüntüleyicisi](azure-kinect-viewer.md)’ni kullanmayı göstereceğiz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="system-requirements"></a>Sistem gereksinimleri

Konak bilgisayar yapılandırmanızın Azure Kinect DK’nin en düşük gereksinimlerinin tümünü karşıladığını doğrulamak için [Sistem gereksinimleri](system-requirements.md)’ni denetleyin.

## <a name="set-up-hardware"></a>Donanımı kurma

> [!NOTE]
> Cihazı kullanmadan önce kameranın koruyucu tabakasını çıkardığınızdan emin olun.

1. Güç bağlayıcısını cihazınızın arkasındaki güç jakına takın. Kablonun diğer ucuna USB güç bağdaştırıcısını bağlayın ve bağdaştırıcıyı elektrik prizine takın.
2. USB veri kablosunu cihazınıza ve sonra da bilgisayarınızdaki USB 3.0 bağlantı noktasına bağlayın.
   >[!NOTE]
   >En iyi sonuçları elde etmek için kabloyu cihaza ve bilgisayara doğrudan bağlayın. Bağlantıda uzatma kablosu veya fazladan bağdaştırıcı kullanmaktan kaçının.

3. Güç göstergesi LED’inin (USB kablosunun yanında) düz beyaz yandığını doğrulayın.
  
   Cihazın gücünün açılması birkaç saniye sürer. Ön taraftaki LED akış göstergesi kapandığında cihaz kullanmaya hazırdır.  

   Güç göstergesi LED’i hakkında daha fazla bilgi için bkz. [Işık ne anlama geliyor?](hardware-specification.md#what-does-the-light-mean)

    ![Tüm cihaz özellikleri](./media/quickstarts/full-device-features.png)

## <a name="download-the-sdk"></a>SDK’yı indirme

1. [SDK’yı indir](sensor-sdk-download.md) bağlantısını seçin.
2. SDK’yı bilgisayarınıza yükleyin.

## <a name="update-firmware"></a>Üretici Yazılımını güncelleştirme

SDK’nın düzgün çalışması için cihaz üretici yazılımının en son sürümü yüklü olmalıdır. Üretici yazılımı sürümünüzü denetlemek ve güncelleştirmek için [Azure Kinect DK üretici yazılımını güncelleştirme](update-device-firmware.md) başlığı altında verilen adımları izleyin.

## <a name="verify-that-the-device-streams-data"></a>Cihazın veri akışı yaptığını doğrulama

1. [Azure Kinect Görüntüleyicisi](azure-kinect-viewer.md)’ni başlatın. Bu aracı başlatmak için şu yöntemlerden birini kullanabilirsiniz:
   - Görüntüleyiciyi komut satırından veya yürütülebilir dosyaya çift tıklayarak başlatabilirsiniz. `k4aviewer.exe` dosyası SDK araç dizininde yer alır (örneğin `C:\Program Files\Azure Kinect SDK vX.Y.Z\tools\k4aviewer.exe`; burada `X.Y.Z`, SDK’nın yüklü sürümüdür).
   - Azure Kinect Görüntüleyicisi’ni cihazın **Başlat** menüsünden başlatabilirsiniz.
2. Azure Kinect Görüntüleyicisi’nde **Cihazı Aç** > **Başlat**’ı seçin.

    ![Azure Kinect Görüntüleyicisi](./media/quickstarts/viewer.png)

3. Aracın her algılayıcı akışını görselleştirdiğini doğrulayın:
   - Derinlik kamerası
   - Renkli kamera
   - Kızılötesi kamera
   - IMU
   - Mikrofonlar

    ![Görselleştirme Aracı](./media/quickstarts/visualization-tool.png)

Azure Kinect DK kurulumunuzu tamamladınız. Şimdi uygulamanızı geliştirmeye veya hizmetlerinizi tümleştirmeye başlayabilirsiniz.

Sorunla karşılaşırsanız [Sorun giderme](troubleshooting.md) konusunu gözden geçirin.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Kinect DK donanım bilgileri](hardware-specification.md)
- [Cihaz üretici yazılımını güncelleştirme](update-device-firmware.md)
- [Azure Kinect Görüntüleyicisi](azure-kinect-viewer.md) hakkında daha fazla bilgi edinin

## <a name="next-steps"></a>Sonraki adımlar

Azure Kinect DK hazır olduktan ve çalışır duruma geldikten sonra
> [!div class="nextstepaction"]
> [Algılayıcı akışlarının dosyaya kaydı](record-sensor-streams-file.md) konusunu da öğrenebilirsiniz
