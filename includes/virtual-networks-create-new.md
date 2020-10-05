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
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "78244965"
---
## <a name="create-the-virtual-network"></a>Sanal ağı oluşturma

Bu bölümde, bir sanal ağ ve alt ağ oluşturacaksınız.

1. Ekranın sol üst kısmındaki **Kaynak oluştur > Ağ > Sanal ağ** seçeneğini belirleyin veya arama kutusuna **Sanal ağ** yazarak arama yapın.

2. **Sanal ağ oluştur**' da, **temel** bilgiler sekmesinde bu bilgileri girin veya seçin:

    | **Ayar**          | **Değer**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Proje ayrıntıları**  |                                                                 |
    | Abonelik     | Azure aboneliğinizi seçin                                  |
    | Kaynak Grubu   | **Yeni oluştur**' u seçin, girin ve **\<resource-group-name>** Tamam ' ı seçin ya da mevcut bir **\<resource-group-name>** temel parametre seçin. |
    | **Örnek ayrıntıları** |                                                                 |
    | Name             | Girmesini **\<virtual-network-name>**                                    |
    | Region           | Seçin **\<region-name>** |

3. **IP adresleri** sekmesini seçin veya sayfanın altındaki **Sonraki: IP adresleri** düğmesini seçin.

4. **IP adresleri** sekmesinde şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | IPv4 adres alanı | Girmesini **\<IPv4-address-space>** |

5. **Alt ağ adı**altında, **varsayılan**sözcük ' ı seçin.

6. **Alt ağı Düzenle**' de şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | Alt ağ adı | Girmesini **\<subnet-name>** |
    | Alt ağ adres aralığı | Girmesini **\<subnet-address-range>**

7. **Kaydet**’i seçin.

8. **Gözden geçir + oluştur** sekmesini seçin ya da **gözden geçir + oluştur** düğmesini seçin.

9. **Oluştur**’u seçin.