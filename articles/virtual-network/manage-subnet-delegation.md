---
title: Bir Azure sanal ağında alt ağ temsili ekleme veya kaldırma
titlesuffix: Azure Virtual Network
description: Azure 'da Azure 'da bir hizmet için Temsilcili bir alt ağ ekleme veya kaldırma hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/01/2019
ms.author: kumud
ms.openlocfilehash: 9bf44aa5ab18f94ec91650548dc13360ce1b1f3d
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71938505"
---
# <a name="add-or-remove-a-subnet-delegation"></a>Alt ağ temsili ekleme veya kaldırma

Alt ağ temsili, hizmeti dağıtma sırasında benzersiz bir tanımlayıcı kullanarak alt ağda hizmete özel kaynaklar oluşturmak için hizmete açık izinler verir. Bu makalede, bir Azure hizmeti için Temsilcili bir alt ağın nasıl ekleneceği veya kaldırılacağı açıklanır.

## <a name="sign-in-to-azure"></a>Azure 'da oturum açın

Azure portal https://portal.azure.com ' da oturum açın.

## <a name="create-the-virtual-network"></a>Sanal ağı oluşturma

Bu bölümde, bir Azure hizmetine daha sonra temsilciliceksiniz bir sanal ağ ve alt ağ oluşturursunuz.

1. Ekranın sol üst kısmında, **kaynak oluştur** > **ağ** > **sanal ağ**' ı seçin.
1. **Sanal ağ oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Name | *MyVirtualNetwork*girin. |
    | Adres alanı | *10.0.0.0/16*girin. |
    | Aboneliğiniz | Aboneliğinizi seçin.|
    | Kaynak grubu | **Yeni oluştur**' u seçin, *myresourcegroup*yazın ve ardından **Tamam**' ı seçin. |
    | Konum | **EastUS**öğesini seçin.|
    | Alt ağ adı | *Mysubnet*yazın. |
    | Alt ağ adresi aralığı | *10.0.0.0/24*girin. |
    |||
1. Kalanı varsayılan olarak bırakın ve ardından **Oluştur**' u seçin.

## <a name="delegate-a-subnet-to-an-azure-service"></a>Bir Azure hizmetine alt ağ atama

Bu bölümde, önceki bölümde oluşturduğunuz alt ağı bir Azure hizmetine devredebilirsiniz.

1. Portalın arama çubuğunda *myVirtualNetwork*girin. Arama sonuçlarında **myVirtualNetwork** göründüğünde seçin.
2. Arama sonuçlarında *myVirtualNetwork*' yi seçin.
3. **Ayarlar**altında **alt ağlar**' ı seçin ve ardından **mysubnet**' i seçin.
4. *Mysubnet* sayfasındaki **alt ağ temsilcisi** listesi için, **alt ağı bir hizmete devretmek** (örneğin, **Microsoft. dbforpostgresql/serversv2**) altında listelenen hizmetlerden seçim yapın.  

## <a name="remove-subnet-delegation-from-an-azure-service"></a>Azure hizmetinden alt ağ temsilcisini kaldırma

1. Portalın arama çubuğunda *myVirtualNetwork*girin. Arama sonuçlarında **myVirtualNetwork** göründüğünde seçin.
2. Arama sonuçlarında *myVirtualNetwork*' yi seçin.
3. **Ayarlar**altında **alt ağlar**' ı seçin ve ardından **mysubnet**' i seçin.
4. *Mysubnet* sayfasında **alt ağ temsilcisi** listesi için **alt ağ temsilcisi**altında listelenen hizmetlerden **hiçbiri** ' ni seçin. 

## <a name="next-steps"></a>Sonraki adımlar
- [Azure 'da alt ağları yönetmeyi](virtual-network-manage-subnet.md)öğrenin.
