---
author: msftradford
ms.author: parkerra
ms.date: 11/20/2020
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 41c0a6292bf7369a30fbb1e4bd474e70ffaedcb6
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95999780"
---
## <a name="configure-the-cloud-spatial-anchor-session"></a>Bulut uzamsal bağlantı noktası oturumunu yapılandırma

Daha sonra bulut uzamsal bağlayıcı oturumunu yapılandırma konusunda dikkatli olunacağız. İlk satırda, oturumda algılayıcı sağlayıcısını ayarlayacağız. Bu andan itibaren, oturum sırasında oluşturduğumuz her bir tutturucu, bir algılayıcı okumaları kümesiyle ilişkilendirilir. Ardından, neredeyse cihaz bulma ölçütlerini örnekliyoruz ve uygulama gereksinimleriyle eşleşecek şekilde başlatın. Son olarak, neredeyse cihazımız ölçütlerimizden bir izleyici oluşturarak, bu oturum, bağlantıları bulmada, kullanıcıdan algılayıcı verileri kullanmasını sağlıyoruz.