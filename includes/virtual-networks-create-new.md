---
title: include dosyası
description: include dosyası
services: virtual-network
author: asudbring
ms.service: virtual-network
ms.topic: include
ms.date: 03/01/2020
ms.author: allensu
ms.custom: include file
ms.openlocfilehash: 81db46b52c9b4fe800f2fbfeadad966995d66e12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78244965"
---
## <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Bu bölümde, sanal bir ağ ve alt ağ oluşturursunuz.

1. Ekranın sol üst tarafında, Sanal **ağ > Ağ** > kaynak oluştur'u veya arama kutusunda **Sanal ağ** aramasını seçin.

2. **Sanal ağ oluştur'da** **TemelLer** sekmesine bu bilgileri girin veya seçin:

    | **Ayar**          | **Değer**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Proje Detayları**  |                                                                 |
    | Abonelik     | Azure aboneliğinizi seçin                                  |
    | Kaynak Grubu   | **Yeni Oluştur'** u seçin, ** \<kaynak grubu adı>** girin, ardından Tamam'ı seçin veya parametrelere göre varolan ** \<** bir kaynak grubu adı>seçin. |
    | **Örnek ayrıntıları** |                                                                 |
    | Adı             | ** \<Sanal ağ adı>girin**                                    |
    | Bölge           | ** \<Bölge adı>seçin** |

3. IP **Adresleri** sekmesini seçin veya sayfanın altındaki **Sonraki: IP Adresleri** düğmesini seçin.

4. IP **Adresleri** sekmesine şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | IPv4 adres alanı | ** \<IPv4 adres-boşluk>girin** |

5. **Alt net adı altında,** **varsayılan**sözcüğü seçin.

6. **Alt ağı edit'e**bu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | Alt ağ adı | ** \<Alt ağ adı>girin** |
    | Alt ağ adres aralığı | ** \<Alt net adres aralığı>girin**

7. **Kaydet'i**seçin.

8. Gözden **Geçir + oluştur** sekmesini seçin veya **Gözden Geçir + oluştur** düğmesini seçin.

9. **Oluştur'u**seçin.