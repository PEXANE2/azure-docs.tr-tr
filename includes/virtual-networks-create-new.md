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
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78244965"
---
## <a name="create-the-virtual-network"></a>Sanal ağı oluşturma

Bu bölümde, bir sanal ağ ve alt ağ oluşturacaksınız.

1. Ekranın sol üst kısmında, **kaynak oluştur > ağ > sanal ağ** ' ı seçin veya arama kutusunda **sanal ağ** ara ' yı seçin.

2. **Sanal ağ oluştur**' da, **temel** bilgiler sekmesinde bu bilgileri girin veya seçin:

    | **Ayar**          | **Değer**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Proje ayrıntıları**  |                                                                 |
    | Abonelik     | Azure aboneliğinizi seçin                                  |
    | Kaynak Grubu   | **Yeni oluştur**' u seçin, **\<kaynak grubu-adı >** girin, sonra Tamam ' ı seçin veya parametrelere göre mevcut bir **\<kaynak grubu adı >** seçin. |
    | **Örnek ayrıntıları** |                                                                 |
    | Adı             | **\<sanal ağ adını girin >**                                    |
    | Bölge           | **\<bölge adını seçin >** |

3. **IP adresleri** sekmesini seçin veya sayfanın altındaki **Sonraki: IP adresleri** düğmesini seçin.

4. **IP adresleri** sekmesinde şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | IPv4 adres alanı | **\<IPv4-adres-alanı >** girin |

5. **Alt ağ adı**altında, **varsayılan**sözcük ' ı seçin.

6. **Alt ağı Düzenle**' de şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | Alt ağ adı | **\<alt ağ adı girin >** |
    | Alt ağ adres aralığı | **\<alt ağ-adres aralığı >** girin

7. **Kaydet**’i seçin.

8. **Gözden geçir + oluştur** sekmesini seçin ya da **gözden geçir + oluştur** düğmesini seçin.

9. **Oluştur**’u seçin.