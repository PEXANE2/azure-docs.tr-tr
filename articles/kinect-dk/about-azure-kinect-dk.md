---
title: Azure ınect DK hakkında
description: Azure Kinect geliştirici seti (DK) araçlarına ve tümleşik hizmetlere genel bakış.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.topic: overview
ms.date: 06/26/2019
keywords: Azure, Kinect, genel bakış, Dev Kit, DK, cihaz, derinlik, gövde izleme, konuşma, bilişsel hizmetler, SDK 'lar, SDK, bellenim
ms.openlocfilehash: 6b3215ba6761521214ebffa2a100c5a1a030ab4c
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "85278006"
---
# <a name="about-azure-kinect-dk"></a>Azure ınect DK hakkında

 ![Azure Kinect DK](./media/index/device-image.jpg)

Azure Kinect DK, gelişmiş AI sensörleriyle gelişmiş, görüntü işleme ve konuşma modelleri sağlayan bir geliştirici settir.  Kinect, bir derinlik algılayıcısı, video kamerası olan uzamsal mikrofon dizisi ve birden çok modu, seçeneği ve yazılım geliştirme setlerine (SDK) sahip tüm tek bir küçük cihaz olarak yönlendirme algılayıcısı içerir. [Microsoft Online Mağazası](https://www.microsoft.com/p/azure-kinect-dk/8pp5vxmd9nhq)'nda satın alınabilir.

Azure Kinect DK geliştirme ortamı aşağıdaki birden çok SDK 'dan oluşur:

- Düşük düzey algılayıcı ve cihaz erişimi için algılayıcı SDK.
- 3B içindeki izleme gövdeleri için gövde Izleme SDK 'Sı.
- Mikrofon erişimini ve Azure bulut tabanlı konuşma hizmetlerini etkinleştirmek için konuşma bilişsel hizmetler SDK.

Ayrıca, bilişsel Vision Hizmetleri cihaz RGB kamerayla birlikte kullanılabilir.

   ![Azure Kinect SDK 'Ları diyagramı](./media/quickstarts/sdk-diagram.jpg)

## <a name="azure-kinect-sensor-sdk"></a>Azure Kinect algılayıcı SDK 'Sı

Azure Kinect algılayıcı SDK 'Sı, Azure ınect DK donanım algılayıcısı ve cihaz yapılandırması için düşük düzeyde algılayıcı erişimi sağlar.

Azure Kinect algılayıcı SDK 'Sı hakkında daha fazla bilgi edinmek için bkz. [algılayıcı SDK 'Sını kullanma](about-sensor-sdk.md).

### <a name="azure-kinect-sensor-sdk-features"></a>Azure Kinect algılayıcı SDK özellikleri

Algılayıcı SDK 'Sı, Azure Kinect DK üzerinde yüklenip çalıştırıldığında bir kez çalışan aşağıdaki özelliklere sahiptir:

- Derinlik kamera erişimi ve mod denetimi (pasif IR modu, ayrıca geniş ve dar alan görüntüleme derinliği modları) 
- RGB kamera erişimi ve denetimi (örneğin, pozlama ve beyaz Bakiye) 
- Hareket algılayıcısı (jroscope ve ivometer) erişimi 
- Kameralar arasında yapılandırılabilir gecikmeyle eşitlenmiş derinlik-RGB kamera akışı 
- Cihazlar arasında yapılandırılabilir gecikme fark ile dış cihaz eşitleme denetimi 
- Resim çözünürlüğü, zaman damgası, vb. için kamera çerçevesi meta veri erişimi 
- Cihaz ayarlama veri erişimi 

### <a name="azure-kinect-sensor-sdk-tools"></a>Azure Kinect algılayıcı SDK araçları

Aşağıdaki araçlar algılayıcı SDK 'sında mevcuttur:

- Cihaz veri akışlarını izlemek ve farklı modlar yapılandırmak için bir Görüntüleyici aracı.
- Matroska kapsayıcı biçimini kullanan bir algılayıcı kayıt aracı ve kayıttan yürütme okuyucusu API 'SI.
- Bir Azure Kinect DK üretici yazılımı güncelleştirme aracı.

## <a name="azure-kinect-body-tracking-sdk"></a>Azure Kinect gövde Izleme SDK 'Sı

Gövde Izleme SDK 'Sı, Azure Kinect DK donanımıyla birlikte kullanıldığında, gövdeleri 3B olarak izlemek için bir Windows kitaplığı ve çalışma zamanı içerir.

### <a name="azure-kinect-body-tracking-features"></a>Azure Kinect gövde Izleme özellikleri

Aşağıdaki gövde izleme özellikleri, eşlik eden SDK 'da kullanılabilir:

- Gövde segmentlemesini sağlar.
- FOV içindeki her kısmi veya tam gövde için kısmen doğru bir iskelet içerir.
- Her gövde için benzersiz bir kimlik sunar.
- , Zaman içinde gövdeler izleyebilir.

### <a name="azure-kinect-body-tracking-tools"></a>Azure Kinect gövde Izleme Araçları

- Gövde Izleyicisi, 3B 'teki gövdeyi izlemek için bir Görüntüleyici aracına sahiptir.

## <a name="speech-cognitive-services-sdk"></a>Konuşma bilişsel hizmetler SDK 'Sı

Konuşma SDK 'Sı, Azure bağlantılı konuşma Hizmetleri 'ni sunar.

### <a name="speech-services"></a>Konuşma hizmetleri

- Konuşmayı metne dönüştürme
- Konuşma çevirisi
- Metin Okuma

>[!NOTE]
>Azure Kinect DK 'nin hoparlörleri yok.

Ek ayrıntılar ve bilgiler için [konuşma hizmeti belgelerini](https://docs.microsoft.com/azure/cognitive-services/speech-service/)ziyaret edin.

## <a name="vision-services"></a>Vision Hizmetleri

Aşağıdaki Azure bilişsel [Vision Hizmetleri](https://azure.microsoft.com/services/cognitive-services/directory/vision/) , görüntüler ve videolar içindeki içerikleri belirleyebilen ve çözümleyebilen Azure hizmetlerini sağlar.

- [Bilgisayar vizyonu](https://azure.microsoft.com/services/cognitive-services/computer-vision/)
- [Yüz Tanıma](https://azure.microsoft.com/services/cognitive-services/face/)
- [Video Dizin Oluşturucu](https://azure.microsoft.com/services/media-services/video-indexer/)
- [İçerik Yöneticisi](https://azure.microsoft.com/services/cognitive-services/content-moderator/)
- [Özel Vizyon](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/)

Hizmetler sürekli geliştikçe ve iyileştirilmesine göre uygulamanızı geliştirmek üzere yeni veya ek bilişsel [Hizmetler](https://azure.microsoft.com/services/cognitive-services/) için düzenli olarak kontrol etmeyi unutmayın. Yeni hizmetlere yönelik erken bakış için bilişsel [Hizmetler laboratuvarlarını](https://labs.cognitive.microsoft.com/)inceleyin.

## <a name="azure-kinect-hardware-requirements"></a>Azure Kinect donanım gereksinimleri

Azure Kinect DK, Microsoft 'un en son algılayıcı teknolojisini tek bir USB bağlantılı aksesuar olarak tümleştirir. Daha fazla bilgi için bkz. [Azure Kinect dk donanım belirtimi](hardware-specification.md).

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Kinect DK 'ye bir genel bakış sunulmaktadır. Sonraki adım, içine ve ayarlamaya göre belirlenir!

> [!div class="nextstepaction"]
>[Hızlı başlangıç: Azure Kinect DK 'yi ayarlama](set-up-azure-kinect-dk.md)
