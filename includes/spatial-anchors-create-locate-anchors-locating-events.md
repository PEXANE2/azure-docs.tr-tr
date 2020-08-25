---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 08/23/2019
ms.author: rgarcia
ms.openlocfilehash: 9a02dfbf3f2976489c10ccfeb935915a65d8dc84
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "71180080"
---
İzleyici oluşturulduktan sonra, `AnchorLocated` istenen her tutturucu için olay harekete geçmeyecektir. Bu olay, bir bağlayıcı bulunduğunda veya bağlayıcı bulunamıyorsa ateşlenir. Bu durum oluşursa, nedeni durumunda belirtilecektir. Bir izleyici için tüm Tutturucuların işlenmesi, bulunması veya bulunamadıktan sonra `LocateAnchorsCompleted` olay harekete geçmeyecektir. İzleyici başına 35 tanımlayıcıda bir sınır vardır. 
