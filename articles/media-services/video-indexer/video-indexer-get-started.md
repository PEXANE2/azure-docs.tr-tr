---
title: Video Indexer'a kaydolun ve ilk videonuzu yükleyin - Azure
titleSuffix: Azure Media Services
description: Video Indexer portalını kullanarak kaydolmak ve ilk videonuzu karşıya yüklemek hakkında bilgi edinin.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: quickstart
ms.date: 03/17/2020
ms.author: juliako
ms.openlocfilehash: 22b06ce99333750d48d1cb65d9f60779572723b9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79499634"
---
# <a name="quickstart-how-to-sign-up-and-upload-your-first-video"></a>Quickstart: Nasıl kaydolun ve ilk video yüklemek için

Bu hızlı başlangıç, Video Indexer web sitesinde nasıl oturum açılabildiğini ve ilk videonuzu nasıl yükleyip yükleyirdiğinizi gösterir.

Video Indexer hesabınızı oluştururken ücretsiz bir deneme hesabı (belirli sayıda ücretsiz dizin oluşturma dakikası elde edersiniz) veya ücretli bir seçenek (kota sınırlaması olmaz) arasından seçim yapabilirsiniz. Ücretsiz deneme kullanıldığında Video Indexer, web sitesi kullanıcılarına 600 dakikaya kadar ve API kullanıcılarına ise 2400 dakikaya kadar ücretsiz dizin oluşturma olanağı sunar. Ücretli seçeneğiyle, [Azure aboneliğinize ve Azure Medya Hizmetleri hesabına bağlı bir](connect-to-azure.md)Video Dizinleyici hesabı oluşturursunuz. Dizin oluşturma faaliyeti yapılan dakika sayısının yanı sıra Azure Media Services hesabıyla ilgili ücretler için ödeme yaparsınız. 

## <a name="sign-up-for-video-indexer"></a>Video Indexer’a kaydolma

Video Indexer ile geliştirmeye başlamak için [Video Indexer](https://www.videoindexer.com) web sitesine gidip kaydolun.

> [!NOTE]
> Video Indexer'ı kullanmaya başladığınızda, depolanan tüm verileriniz ve yüklenen içerik, Microsoft tarafından yönetilen bir anahtarla birlikte şifrelenir.

## <a name="upload-a-video-using-the-video-indexer-website"></a>Video Indexer web sitesini kullanarak karşıya video yükleme

### <a name="supported-file-formats-for-video-indexer"></a>Video Indexer için desteklenen dosya biçimleri

Video Indexer ile kullanabileceğiniz dosya biçimlerinin listesi için [giriş kapsayıcısı/dosya biçimleri](../latest/media-encoder-standard-formats.md#input-containerfile-formats) makalesine bakın.

### <a name="upload-a-video"></a>Karşıya video yükleme

1. [Video Indexer](https://www.videoindexer.ai/) web sitesinde oturum açın.
2. Karşıya video yüklemek için **Karşıya Yükle** düğme veya bağlantısına basın.

    > [!NOTE]
    > Videonun adı 80 karakterden büyük olmamalıdır.

    ![Karşıya Yükle](./media/video-indexer-get-started/video-indexer-upload.png)

    Videonuz karşıya yüklendikten sonra Video Indexer videoyu dizinlemeye ve analiz etmeye başlar.

    ![Karşıya yüklendi](./media/video-indexer-get-started/video-indexer-uploaded.png) 

    Video Indexer analizi tamamladıktan sonra videonuzun bağlantısını ve videonuzda neler bulunduğunun kısa bir açıklamasını içeren bir bildirim alırsınız. Örnek: kişiler, konular, OCR’ler.

## <a name="see-also"></a>Ayrıca bkz.

Daha fazla bilgi için [Yükleme ve dizin videolarını](upload-index-videos.md) görün.

Bir videoyu yükledikten ve dizine dizine yükledikten sonra, videonun istatistiklerini görmek için [Video Indexer](video-indexer-view-edit.md) web sitesini veya [Video Indexer Geliştirici Portalını](video-indexer-use-apis.md) kullanmaya başlayabilirsiniz. 

[API'leri kullanmaya başlayın](video-indexer-use-apis.md)

## <a name="next-steps"></a>Sonraki adımlar

Detaylı giriş için lütfen tanıtım laboratuvarımızı ziyaret [ediniz.](https://github.com/Azure-Samples/media-services-video-indexer/blob/master/IntroToVideoIndexer.md) 

Atölyenin sonunda video ve ses içeriğinden çıkarılabilen bilgilerin türünü iyi anlayacak, içerik istihbaratıyla ilgili fırsatları belirlemeye, Azure'da video yapay zekasını sunmaya ve birkaç ını demi yapmaya daha hazır hale geleceksiniz. Video Indexer üzerinde senaryolar.

