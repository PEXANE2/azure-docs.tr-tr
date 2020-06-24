---
title: Video Indexer kaydolun ve ilk videonuzu karşıya yükleyin-Azure
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
ms.openlocfilehash: f6c3953947e6f7e84f4cf9b565d2f66648b177f7
ms.sourcegitcommit: 666303748238dfdf9da30d49d89b915af73b0468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85130764"
---
# <a name="quickstart-how-to-sign-up-and-upload-your-first-video"></a>Hızlı başlangıç: nasıl kaydolun ve ilk videonuzu karşıya yükleyebilirsiniz

Bu başlangıç hızlı başlangıcı, Video Indexer Web sitesinde oturum açmayı ve ilk videonuzu karşıya yüklemeyi gösterir.

Video Indexer hesabınızı oluştururken ücretsiz bir deneme hesabı (belirli sayıda ücretsiz dizin oluşturma dakikası elde edersiniz) veya ücretli bir seçenek (kota sınırlaması olmaz) arasından seçim yapabilirsiniz. Ücretsiz deneme kullanıldığında Video Indexer, web sitesi kullanıcılarına 600 dakikaya kadar ve API kullanıcılarına ise 2400 dakikaya kadar ücretsiz dizin oluşturma olanağı sunar. Ücretli seçenekle, [Azure aboneliğinize ve bir Azure Media Services hesabına bağlı](connect-to-azure.md)bir video Indexer hesabı oluşturursunuz. Dizin oluşturma faaliyeti yapılan dakika sayısının yanı sıra Azure Media Services hesabıyla ilgili ücretler için ödeme yaparsınız. 

## <a name="sign-up-for-video-indexer"></a>Video Indexer’a kaydolma

Video Indexer ile geliştirmeye başlamak için [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip kaydolun.

> [!NOTE]
> Video Indexer kullanmaya başladıktan sonra, tüm depolanmış verileriniz ve karşıya yüklenen içerikler, Microsoft tarafından yönetilen bir anahtarla geri kalanında şifrelenir.

## <a name="upload-a-video-using-the-video-indexer-website"></a>Video Indexer web sitesini kullanarak karşıya video yükleme

### <a name="supported-file-formats-for-video-indexer"></a>Video Indexer için desteklenen dosya biçimleri

Video Indexer ile kullanabileceğiniz dosya biçimlerinin bir listesi için bkz. [Giriş kapsayıcı/dosya biçimleri](../latest/media-encoder-standard-formats.md#input-containerfile-formats) makalesi.

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

Daha fazla ayrıntı için bkz. [videoları karşıya yükleme ve Dizin](upload-index-videos.md) .

Bir videoyu karşıya yükleyip dizinledikten sonra, videonun öngörülerini görmek için [video Indexer](video-indexer-view-edit.md) Web sitesini veya [video Indexer geliştirici portalını](video-indexer-use-apis.md) kullanmaya başlayabilirsiniz. 

[API 'Leri kullanmaya başlama](video-indexer-use-apis.md)

## <a name="next-steps"></a>Sonraki adımlar

Ayrıntılı giriş için lütfen [giriş laboratuvarımızı](https://github.com/Azure-Samples/media-services-video-indexer/blob/master/IntroToVideoIndexer.md)ziyaret edin. 

Atölyenin sonunda, video ve ses içeriğinden ayıklanabilen bilgi türünü iyi anlayabilirsiniz, içerik zekası ile ilgili fırsatları belirlemek için daha hazırlanacaktır, Azure 'da video AI 'yi aralıklı olarak kullanabilirsiniz ve Video Indexer çeşitli senaryolar tanıtımına sahip olacaksınız.

