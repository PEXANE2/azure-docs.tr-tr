---
title: Video Indexer sahneleri, görüntüleri ve ana kareleri-Azure
titlesuffix: Azure Media Services
description: Bu konu, Video Indexer sahneleri, görüntüleri ve ana kareleri için bir genel bakış sunar.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 07/05/2019
ms.author: juliako
ms.openlocfilehash: cdabc1b6bfed519098f656710ef49a946e676cf2
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815661"
---
# <a name="scenes-shots-and-keyframes"></a>Sahneler, çekimler, ana kareler

Video Indexer, Videoları yapısal ve anlam özelliklerine göre zamana bağlı birimlere parçalara ayırır. Bu özellik, müşterilerin, değişen granlular temelinde video içeriğine kolayca gözatmasını, yönetmesini ve düzenlemesini sağlar. Örneğin, bu konuda açıklanan sahneleri, görüntüleri ve ana kareleri temel alan.   

![Sahneler, çekimler, ana kareler](./media/scenes-shots-keyframes/scenes-shots-keyframes.png)
 
## <a name="scene-detection"></a>Sahne algılama  
 
Video Indexer görsel ipuçlarıyla videoda bir sahnenin ne zaman değişiklik olduğunu belirler. Bir sahne, tek bir olay gösterir ve bu, anlam ile ilgili olan bir dizi ardışık anlık görüntüsünü meydana gelir. Sahne küçük resmi, temel alınan görüntüsündeki ilk ana karedir. Video Indexer, bir videoyu ardışık görüntülerdeki renk tutarlılılığını temel alarak sahneleri parçalara ayırır ve her sahnenin başlangıç ve bitiş saatini alır. Sahne algılama, videoların anlamlı yönlerini içerdiğinden zorlayıcı bir görev olarak değerlendirilir.

> [!NOTE]
> En az 3 sahne içeren videolar için geçerlidir.

## <a name="shot-detection"></a>Çekim algılama

Video Indexer, ardışık çerçevelerin renk düzeninde hem sert hem de dereceli geçişleri izleyerek görsel ipuçlarıyla videoda bir çekneye ne zaman değişiklik olduğunu belirler. Alıcının meta verileri bir başlangıç ve bitiş zamanı ve bu çekte yer alan ana kare listesini içerir. Bu görüntüleri aynı kameradaki aynı kameradan alınan ardışık çerçevelerdir.

## <a name="keyframe-detection"></a>Ana kare algılama

Resmi en iyi temsil eden çerçeveleri seçer. Ana kareler, Aesthetic Characteristics özelliklerine (örneğin, kontrast ve stablete) göre videonun tamamının seçtiği temsili çerçevelerdir. Video Indexer, ana kare kimliklerinin bir listesini, hangi müşterilerin ana kare küçük resmini ayıklayabileceği temel alınarak, alıcının meta verilerinin bir parçası olarak alır. 

Ana kareler, JSON çıkışı içindeki görüntüleriyle ilişkilendirilir. 

## <a name="next-steps"></a>Sonraki adımlar

[API tarafından üretilen Video Indexer çıkışını inceleyin](video-indexer-output-json-v2.md#scenes)
