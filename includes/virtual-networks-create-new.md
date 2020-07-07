---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-network
author: asudbring
ms.service: virtual-network
ms.topic: include
ms.date: 03/01/2020
ms.author: allensu
ms.custom: include file
ms.openlocfilehash: 81db46b52c9b4fe800f2fbfeadad966995d66e12
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
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
    | Kaynak Grubu   | **Yeni oluştur**' u seçin, girin ve **\<resource-group-name>** Tamam ' ı seçin ya da mevcut bir **\<resource-group-name>** temel parametre seçin. |
    | **Örnek ayrıntıları** |                                                                 |
    | Name             | Girmesini**\<virtual-network-name>**                                    |
    | Bölge           | Seçin**\<region-name>** |

3. **IP adresleri** sekmesini seçin veya sayfanın altındaki **Sonraki: IP adresleri** düğmesini seçin.

4. **IP adresleri** sekmesinde şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | IPv4 adres alanı | Girmesini**\<IPv4-address-space>** |

5. **Alt ağ adı**altında, **varsayılan**sözcük ' ı seçin.

6. **Alt ağı Düzenle**' de şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | Alt ağ adı | Girmesini**\<subnet-name>** |
    | Alt ağ adres aralığı | Girmesini**\<subnet-address-range>**

7. **Kaydet**’i seçin.

8. **Gözden geçir + oluştur** sekmesini seçin ya da **gözden geçir + oluştur** düğmesini seçin.

9. **Oluştur**'u seçin.