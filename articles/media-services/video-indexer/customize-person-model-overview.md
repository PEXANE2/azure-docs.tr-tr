---
title: Video Dizinleyici'de Kişi modelini özelleştirme - Azure
titleSuffix: Azure Media Services
description: Bu makalede, Video Dizinleyici'de Kişi modelinin ne olduğu ve nasıl özelleştirilene ilgili genel bir bakış yer almaktadır.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 3fabba98cb137975da749411ca9accb5a951742d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73838306"
---
# <a name="customize-a-person-model-in-video-indexer"></a>Video Dizinleyici'de Kişi modelini özelleştirme

Video Indexer videolarınızda ünlülerin tanınmasını destekler. Ünlü tanıma özelliği, IMDB, Wikipedia ve en çok LinkedIn etkileyenler gibi yaygın olarak istenen veri kaynağına dayalı yaklaşık bir milyon yüzü kapsar. Video Dizinleyici tarafından tanınmayan yüzler hala algılanır, ancak adsız bırakılır. Müşteriler özel Kişi modelleri oluşturabilir ve Video Dizinleyici'nin varsayılan olarak tanınmayan yüzleri tanımasını sağlayabilir. Müşteriler bu Kişi modellerini, bir kişinin adını kişinin yüzünün görüntü dosyalarıyla eşleştirerek oluşturabilir.  

Hesabınız farklı kullanım durumlarına hitap ediyorsa, hesap başına birden çok Kişi modeli oluşturabilme avantajından yararlanabilirsiniz. Örneğin, hesabınızdaki içeriğin farklı kanallarda sıralanmış olması gerekiyorsa, her kanal için ayrı bir Kişi modeli oluşturmak isteyebilirsiniz. 

> [!NOTE]
> Her Kişi modeli 1 milyon adede kadar kişiyi destekler ve her hesabın 50 Kişi modeli sınırı vardır. 

Bir model oluşturulduktan sonra, bir videoyu yüklerken/dizine eklerken veya yeniden dizine eklerken belirli bir Kişi modelinin model kimliğini sağlayarak onu kullanabilirsiniz. Bir video için yeni bir yüz eğitimi, video ile ilişkili olduğu özel modeli güncelleştirir. 

Birden çok Kişi modeli desteğine ihtiyacınız yoksa, yükleme/dizin oluşturma veya yeniden dizine ekin oluşturma yaparken videonuza Bir Kişi model kimliği atayın. Bu durumda, Video Dizinleyici hesabınızda varsayılan Kişi modelini kullanır. 

Video Indexer web sitesini, bir videoda algılanan yüzleri düzenlemek ve bir web sitesi konusunu kullanarak [Kişiyi Özelleştir modelinde](customize-person-model-with-website.md) açıklandığı gibi hesabınızdaki birden çok özel Kişi modelini yönetmek için kullanabilirsiniz. API'leri [kullanarak Kişi modelini özelleştir'de](customize-person-model-with-api.md)açıklandığı gibi API'yi de kullanabilirsiniz.
