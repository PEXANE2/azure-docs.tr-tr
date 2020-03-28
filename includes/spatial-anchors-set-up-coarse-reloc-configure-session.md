---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 574003a150ef294aa6a2ebc035fe48cf877dec66
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76545235"
---
## <a name="configure-the-cloud-spatial-anchor-session"></a>Bulut uzamsal bağlantı oturumunu yapılandırma

Bulut uzamsal bağlantı oturumunu daha sonra yapılandırmaya özen ilacaktır. İlk hatta, sensör sağlayıcısını oturuma ayarlıyoruz. Şu andan itibaren, oturum sırasında oluşturduğumuz herhangi bir çapa bir dizi sensör okumasıyla ilişkilendirilecektir. Ardından, cihaza yakın bir yer bulma ölçütlerini anında anlayıp uygulama gereksinimlerine uyacak şekilde başlatılmasını sağlarız. Son olarak, oturuma, cihaza yakın kriterlerimizden bir izleyici oluşturarak çapaları tespit ederken sensör verilerini kullanmatalimatı veririz.