---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 11/20/2020
ms.author: parkerra
ms.openlocfilehash: 5360cbb7bdfbdc59e87366e73a891b5c2583672e
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993134"
---
İzleyici oluşturulduktan sonra, `AnchorLocated` istenen her tutturucu için olay harekete geçmeyecektir. Bu olay, bir bağlayıcı bulunduğunda veya bağlayıcı bulunamıyorsa ateşlenir. Bu durum oluşursa, nedeni durumunda belirtilecektir. Bir izleyici için tüm Tutturucuların işlenmesi, bulunması veya bulunamadıktan sonra `LocateAnchorsCompleted` olay harekete geçmeyecektir. İzleyici başına 35 tanımlayıcıda bir sınır vardır. 
