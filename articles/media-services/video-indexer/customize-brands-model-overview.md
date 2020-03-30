---
title: Video Dizinleyici'de Markalar modelini özelleştirme - Azure
titleSuffix: Azure Media Services
description: Bu makalede, Video Indexer'da Markalar modelinin ne olduğu ve nasıl özelleştirilene ilgili genel bir bakış ve res.) yer almaktadır.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: ca3d825fb2f4184448cc279d9408f47ad4ad004a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73838367"
---
# <a name="customize-a-brands-model-in-video-indexer"></a>Video Indexer'da Bir Markalar modelini özelleştirin

Video Indexer, video ve ses içeriğinin dizinlenme ve yeniden dizine ekinleme sırasında konuşma ve görsel metinden marka algılamayı destekler. Marka algılama özelliği, Bing'in marka veritabanı tarafından önerilen ürünler, hizmetler ve şirketlerden bahsedildiğini tanımlar. Örneğin, Microsoft bir video veya ses içeriğinde adı geçiyorsa veya bir videoda görsel metinde görüntülenmişse, Video Indexer içeriği içerikte bir marka olarak algılar. Markalar bağlam kullanılarak diğer terimlerden farklılaştırılır.

Marka algılama, içerik arşivi ve keşfi, bağlamsal reklamcılık, sosyal medya analizi, perakende rekabet analizi ve daha birçok iş senaryosunda yararlıdır. Video Indexer marka algılama, Bing'in marka veritabanını kullanarak ve her Video Indexer hesabı için özel bir Brands modeli oluşturarak özelleştirmeyle, konuşma ve görsel metinde marka sözlerini dizine ekmenize olanak tanır. Özel Markalar modeli özelliği, Video Indexer'ın Bing markaları veritabanından markaları algılayıp algılamayacağını seçmenize, belirli markaların algılanmamasını hariç tutup tutmayacağını (aslında markaların kara listesini oluşturmanıza) ve markalarınızın bir parçası olması gereken markaları eklemenize olanak tanır Bing'in marka veritabanında olmayan bir model (aslında beyaz bir marka listesi oluşturma). Oluşturduğunuz özel Markalar modeli yalnızca modeli oluşturduğunuz hesapta kullanılabilir.

## <a name="out-of-the-box-detection-example"></a>Kutunun dışında algılama örneği

Microsoft [Build 2017 Day 2](https://www.videoindexer.ai/media/ed6ede78ad/) sunusunda "Microsoft Windows" markası birden çok kez görünür. Bazen transkriptte, bazen görsel metin olarak ve asla tam olarak. Video Indexer, bir terimin gerçekten de içeriğe dayalı bir marka olduğunu, 90k'dan fazla markayı kutunun dışına çıkardığını ve sürekli güncellediğini yüksek hassasiyetle algılar. Saat 02:25'te Video Indexer markayı konuşmadan algılar ve saat 02:40'da windows logosunun bir parçası olan görsel metinden tekrar tespit eder.

![Markalara genel bakış](./media/content-model-customization/brands-overview.png)

Inşaat bağlamında pencereler hakkında konuşmak bir marka olarak kelime "Windows" algılamaz, ve Box için aynı, Apple, Fox, vb, nasıl bağlamdan ayrıştırmak için biliyorum gelişmiş Machine Learning algoritmaları dayalı. Marka Algılama, desteklenen tüm dillerimiz için çalışır. [Tam Microsoft Build 2017 Gün 2 açılış video ve dizin](https://www.videoindexer.ai/media/ed6ede78ad/)için buraya tıklayın.

Kendi markalarınızı getirmek için Sonraki adımlara göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[API'leri kullanarak Markalar modelini özelleştirin](customize-brands-model-with-api.md)

[Web sitesini kullanarak Markalar modelini özelleştirin](customize-brands-model-with-website.md)
