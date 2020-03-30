---
title: 'Azure VPN Ağ Geçidi: Bir ağ geçidini silme: portal'
description: Kaynak Yöneticisi dağıtım modelindeki Azure portalını kullanarak sanal ağ ağ ağ geçidini silin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.date: 10/23/2018
ms.author: cherylmc
ms.topic: conceptual
ms.openlocfilehash: fba213be2de3b747d3ba962674a03c0bc7df3ed5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75863674"
---
# <a name="delete-a-virtual-network-gateway-using-the-portal"></a>Portalı kullanarak sanal ağ ağ geçidini silme

> [!div class="op_single_selector"]
> * [Azure portalında](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Powershell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klasik)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)

Bu makalede, Kaynak Yöneticisi dağıtım modelini kullanarak dağıtılan bir Azure VPN ağ geçitlerini silme yönergeleri verilmektedir. VPN ağ geçidi yapılandırması için sanal ağ ağ ağ geçidini silmek istediğinizde kullanabileceğiniz birkaç farklı yaklaşım vardır.

- Her şeyi silmek ve baştan başlamak istiyorsanız, bir test ortamında olduğu gibi, kaynak grubunu silebilirsiniz. Bir kaynak grubunu sildiğinizde, grup içindeki tüm kaynakları siler. Bu yöntem yalnızca kaynak grubundaki kaynaklardan herhangi birini tutmak istemiyorsanız önerilir. Bu yaklaşımı kullanarak yalnızca birkaç kaynağı seçarak silemezsiniz.

- Kaynak grubunuzdaki bazı kaynakları tutmak istiyorsanız, sanal ağ ağ ağ geçidini silerseniz biraz daha karmaşık hale gelir. Sanal ağ ağ ağ geçidini silmeden önce ağ geçidine bağlı olan kaynakları silmeniz gerekir. İzlediğiniz adımlar, oluşturduğunuz bağlantı türüne ve her bağlantı için bağımlı kaynaklara bağlıdır.

> [!IMPORTANT]
> Aşağıdaki yönergelerde, Kaynak Yöneticisi dağıtım modelini kullanarak dağıtılan Azure VPN ağ geçitlerinin nasıl silinir. Klasik dağıtım modelini kullanarak dağıtılan bir VPN ağ geçidini silmek için lütfen [burada](vpn-gateway-delete-vnet-gateway-classic-powershell.md)açıklandığı gibi Azure PowerShell'i kullanın.


## <a name="delete-a-vpn-gateway"></a>VPN ağ geçidi silme

Sanal ağ ağ ağ geçidini silmek için öncelikle sanal ağ ağ ağ geçidiyle ilgili her kaynağı silmeniz gerekir. Kaynaklar, bağımlılıklar nedeniyle belirli bir sırada silinmelidir.

[!INCLUDE [delete gateway](../../includes/vpn-gateway-delete-vnet-gateway-portal-include.md)]

Bu noktada, sanal ağ ağ geçidi silinir. Sonraki adımlar, artık kullanılmayan kaynakları silmenize yardımcı olur.

### <a name="to-delete-the-local-network-gateway"></a>Yerel ağ ağ ağ geçidini silmek için

1. **Tüm kaynaklarda,** her bağlantıyla ilişkili yerel ağ ağ geçitlerini bulun.
2. Yerel ağ ağ geçidiiçin **Genel Bakış** bıyığında **Sil'i**tıklatın.

### <a name="to-delete-the-public-ip-address-resource-for-the-gateway"></a>Ağ geçidinin Genel IP adresi kaynağını silmek için

1. **Tüm kaynaklarda,** ağ geçidiyle ilişkili Genel IP adresi kaynağını bulun. Sanal ağ ağ ağ geçidi etkinse, iki Genel IP adresi görürsünüz. 
2. Genel IP adresine **genel bakış** sayfasında, onaylamak için **Sil'** i ve **ardından Evet'i** tıklatın.

### <a name="to-delete-the-gateway-subnet"></a>Ağ geçidi alt netini silmek için

1. **Tüm kaynaklarda,** sanal ağı bulun. 
2. **Subnets** bıçağında **GatewaySubnet'i**tıklatın, ardından **Sil'i**tıklatın. 
3. Ağ geçidi alt netini silmek istediğinizi doğrulamak için **Evet'i** tıklatın.

## <a name="delete-a-vpn-gateway-by-deleting-the-resource-group"></a><a name="deleterg"></a>Kaynak grubunu silerek VPN ağ geçidini silme

Kaynaklarınızdan herhangi birini kaynak grubunda tutmakla ilgilenmiyorsanız ve baştan başlamak istiyorsanız, tüm kaynak grubunu silebilirsiniz. Bu her şeyi kaldırmak için hızlı bir yoldur. Aşağıdaki adımlar yalnızca Kaynak Yöneticisi dağıtım modeli için geçerlidir.

1. **Tüm kaynaklarda,** kaynak grubunu bulun ve bıçağı açmak için tıklatın.
2. **Sil'i**tıklatın. Sil bıyıküzerinde, etkilenen kaynakları görüntüleyin. Tüm bu kaynakları silmek istediğinizden emin olun. Değilse, bu makalenin üst kısmındaki VPN ağ geçidini sil adımlarını kullanın.
3. Devam etmek için, silmek istediğiniz kaynak grubunun adını yazın ve sonra **Sil'i**tıklatın.
