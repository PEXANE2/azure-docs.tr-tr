---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 08/23/2019
ms.author: rgarcia
ms.openlocfilehash: 9a02dfbf3f2976489c10ccfeb935915a65d8dc84
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "71180080"
---
İzleyici oluşturulduktan sonra, istenen her tutturucu `AnchorLocated` için olay harekete geçmeyecektir. Bu olay, bir bağlayıcı bulunduğunda veya bağlayıcı bulunamıyorsa ateşlenir. Bu durum oluşursa, nedeni durumunda belirtilecektir. Bir izleyici için tüm Tutturucuların işlenmesi, bulunması veya bulunamadıktan sonra `LocateAnchorsCompleted` olay harekete geçmeyecektir. İzleyici başına 35 tanımlayıcıda bir sınır vardır. 
