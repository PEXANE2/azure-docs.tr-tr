---
title: Ağ yönlendirme tercihini yapılandırma
titleSuffix: Azure Storage
description: Ağ trafiğinin Internet üzerinden kendi hesabınıza nasıl yönlendirildiğini belirtmek için, Azure depolama hesabınız için ağ yönlendirmesi tercihini yapılandırın.
services: storage
author: normesta
ms.service: storage
ms.topic: how-to
ms.date: 02/21/2020
ms.author: normesta
ms.reviewer: santoshc
ms.subservice: common
ms.openlocfilehash: fb427de170764e5cd1fca57f9fb2d1410829e521
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101700909"
---
# <a name="configure-network-routing-preference-for-azure-storage"></a>Azure depolama için ağ yönlendirme tercihini yapılandırma

Bu makalede, depolama hesabınız için ağ yönlendirme tercihini ve rotaya özgü uç noktaların nasıl yapılandırılacağı açıklanır. 

Ağ yönlendirme tercihi, ağ trafiğinin Internet üzerinden istemcilerden hesabınıza nasıl yönlendirildiğini belirtir. Rotaya özgü uç noktalar, Azure Storage 'ın depolama hesabınız için oluşturduğu yeni uç noktalardır. Bu uç noktalar, varsayılan yönlendirme tercihinizi değiştirmeden trafiği istenen bir yol üzerinden yönlendirir. Daha fazla bilgi için bkz. [Azure depolama Için ağ yönlendirme tercihi](network-routing-preference.md).

## <a name="configure-the-routing-preference-for-the-default-public-endpoint"></a>Varsayılan genel uç nokta için yönlendirme tercihini yapılandırma

Varsayılan olarak, depolama hesabının genel uç noktası için yönlendirme tercihi Microsoft Global Network olarak ayarlanır. Depolama hesabınızın genel uç noktası için varsayılan yönlendirme tercihi olarak Microsoft küresel ağ ve Internet yönlendirmesi arasında seçim yapabilirsiniz. Bu iki yönlendirme türü arasındaki fark hakkında daha fazla bilgi edinmek için bkz. [Azure depolama Için ağ yönlendirme tercihi](network-routing-preference.md). 

Yönlendirme tercihinizi Internet yönlendirme olarak değiştirmek için:

1. Portalda depolama hesabınıza gidin.

2. **Ayarlar** altında **ağ**' ı seçin.

    > [!div class="mx-imgBorder"]
    > ![Ağ menü seçeneği](./media/configure-network-routing-preference/networking-option.png)

3.  **Güvenlik duvarları ve sanal ağlar** sekmesinde, **ağ yönlendirme** altında, **yönlendirme tercihi** ayarını **Internet yönlendirme** olarak değiştirin.

4.  **Kaydet**’e tıklayın.

    > [!div class="mx-imgBorder"]
    > ![İnternet yönlendirme seçeneği](./media/configure-network-routing-preference/internet-routing-option.png)

## <a name="configure-a-route-specific-endpoint"></a>Rotaya özgü uç nokta yapılandırma

Ayrıca, yönlendirmeye özgü bir uç nokta da yapılandırabilirsiniz. Örneğin, varsayılan uç nokta için yönlendirme tercihini *İnternet yönlendirme* olarak ayarlayabilir ve ardından İnternet üzerindeki istemciler ve depolama hesabınızın Microsoft Global ağı aracılığıyla yönlendirilmesi için bir rotaya özgü uç nokta yayımlayabilirsiniz.

1.  Portalda depolama hesabınıza gidin.

2.  **Ayarlar** altında **ağ**' ı seçin.

3.  **Güvenlik duvarları ve sanal ağlar** sekmesindeki **rotaya özgü uç noktalar**' ın altında, yönlendirmeye özgü uç noktaıza ait yönlendirme tercihini seçin ve ardından **Kaydet**' e tıklayın. Bu tercih yalnızca rotaya özgü uç noktayı etkiler. Bu tercih, varsayılan yönlendirme tercihinizi etkilemez.  

    Aşağıdaki görüntüde **Microsoft ağ yönlendirme** seçeneği seçili gösterilmektedir.

    > [!div class="mx-imgBorder"]
    > ![Microsoft ağ yönlendirme seçeneği](./media/configure-network-routing-preference/microsoft-network-routing-option.png)

## <a name="find-the-endpoint-name-for-a-route-specific-endpoint"></a>Rotaya özgü uç nokta için uç nokta adı bulma

Bir rotaya özgü uç nokta yapılandırdıysanız, uç noktayı depolama hesabınızın özelliklerinde bulabilirsiniz.

1.  **Ayarlar** altında **Özellikler**' i seçin.

    > [!div class="mx-imgBorder"]
    > ![Özellikler menü seçeneği](./media/configure-network-routing-preference/properties.png)

2.  **Microsoft ağ yönlendirme** uç noktası, yönlendirme tercihlerini destekleyen her bir hizmet için gösterilir. Bu görüntüde blob ve dosya hizmetleri için uç nokta gösterilmektedir.

    > [!div class="mx-imgBorder"]
    > ![Rotaya özgü uç noktalar için Microsoft ağ yönlendirme seçeneği](./media/configure-network-routing-preference/routing-url.png)


## <a name="see-also"></a>Ayrıca bkz.

- [Ağ yönlendirme tercihi](network-routing-preference.md)
- [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](storage-network-security.md)
- [BLOB depolama için güvenlik önerileri](../blobs/security-recommendations.md)
