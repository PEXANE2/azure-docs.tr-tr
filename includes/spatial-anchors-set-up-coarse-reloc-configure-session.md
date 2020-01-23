---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 574003a150ef294aa6a2ebc035fe48cf877dec66
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76545235"
---
## <a name="configure-the-cloud-spatial-anchor-session"></a>Bulut uzamsal bağlantı noktası oturumunu yapılandırma

Daha sonra bulut uzamsal bağlayıcı oturumunu yapılandırma konusunda dikkatli olunacağız. İlk satırda, oturumda algılayıcı sağlayıcısını ayarlayacağız. Bu andan itibaren, oturum sırasında oluşturduğumuz her bir tutturucu, bir algılayıcı okumaları kümesiyle ilişkilendirilir. Ardından, neredeyse cihaz bulma ölçütlerini örnekliyoruz ve uygulama gereksinimleriyle eşleşecek şekilde başlatın. Son olarak, neredeyse cihazımız ölçütlerimizden bir izleyici oluşturarak, bu oturum, bağlantıları bulmada, kullanıcıdan algılayıcı verileri kullanmasını sağlıyoruz.