---
title: Video Indexer bir kişi modelini özelleştirme-Azure
titleSuffix: Azure Media Services
description: Bu makale, Video Indexer kişi modeli nedir ve özelleştirmeyi özelleştirmek için bir genel bakış sunar.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: kumud
ms.openlocfilehash: 613ad2d0ab90bef016e80a33be12b71cba5e2f71
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87047047"
---
# <a name="customize-a-person-model-in-video-indexer"></a>Video Indexer bir kişi modelini özelleştirme

Video Indexer, videolarınızdaki ünlüleri tanımayı destekler. Ünlüçilerin tanınma özelliği, ıMDB, Viveze ve en popüler LinkedIn etkileyen, sık istenen veri kaynağına göre yaklaşık 1.000.000 yüz içerir. Video Indexer tarafından tanınmayan yüzler hala algılanır, ancak adlandırılmamaktadır. Müşteriler özel kişi modelleri oluşturabilir ve varsayılan olarak tanınmayan yüzeyleri tanımak için Video Indexer etkinleştirebilir. Müşteriler kişilerin adını kişinin yüzlerindeki görüntü dosyalarıyla eşleştirerek bu kişi modellerini oluşturabilir.  

Hesabınız farklı kullanım durumlarına karşı, hesap başına birden fazla kişi modeli oluşturabilmeniz için avantaj sağlayabilirsiniz. Örneğin, hesabınızdaki içerik farklı kanallarda sıralanacaksa, her kanal için ayrı bir kişi modeli oluşturmak isteyebilirsiniz. 

> [!NOTE]
> Her kişi modeli en fazla 1.000.000 kişiyi destekler ve her hesabın 50 kişi modeli sınırlaması vardır. 

Bir model oluşturulduktan sonra, bir videoyu karşıya yüklerken/dizinleme yaparken veya yeniden dizinlerken belirli bir kişi modelinin model KIMLIĞINI sağlayarak onu kullanabilirsiniz. Video için yeni bir yüz eğitimi yapın, videonun ilişkilendirildiği özel modeli günceller. 

Birden çok kişi modeli desteği gerekmiyorsa, karşıya yükleme/dizinleme veya yeniden dizin oluşturma sırasında videonuza bir kişi modeli KIMLIĞI atamayın. Bu durumda Video Indexer hesabınızda varsayılan kişi modelini kullanacaktır. 

Video Indexer Web sitesini kullanarak bir videoda algılanan yüzeyleri düzenleyebilir ve bir [Web sitesi kullanarak kişi modeli özelleştirme](customize-person-model-with-website.md) konu başlığı altında açıklandığı gibi hesabınızdaki birden fazla özel kişi modelini yönetebilirsiniz. API 'yi, API ['leri kullanarak bir kişi modelini özelleştirme](customize-person-model-with-api.md)bölümünde açıklandığı gibi de kullanabilirsiniz.
