---
author: msftradford
ms.author: parkerra
ms.date: 01/28/2021
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 386c2f8a7cc32cf65381d9d62e2e6940754e3606
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99214739"
---
## <a name="configure-the-sensor-fingerprint-provider"></a>Algılayıcı parmak izi sağlayıcısını yapılandırma

Bir algılayıcı parmak izi sağlayıcısı oluşturup yapılandırarak başlayacağız. Algılayıcı parmak izi sağlayıcısı, cihazınızdaki platforma özel sensörleri okumanın ve kullanımlarını, bulut uzamsal bağlayıcı oturumu tarafından tüketilen ortak bir gösterimde dönüştürmektir.

> [!IMPORTANT]
> Etkinleştirdiğinizden ilgili sensörlerinizin platformunda kullanılabilir olup olmadığını [burada kontrol ettiğinizden emin olun](../articles/spatial-anchors/concepts/coarse-reloc.md#platform-availability) .