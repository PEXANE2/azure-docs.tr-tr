---
title: Azure CLI kullanarak Bir Burç ana bilgisayar oluşturma | Azure Kalesi
description: Bu makalede, bir Bastion ana bilgisayar oluşturma ve silme
services: bastion
author: mialdrid
ms.service: bastion
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: mialdrid
ms.openlocfilehash: f98c965ad3b776f3688a716ba28b5367a00c9119
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619217"
---
# <a name="create-an-azure-bastion-host-using-azure-cli"></a>Azure CLI'yi kullanarak Azure Bastion ana bilgisayar oluşturma

Bu makalede, Azure CLI kullanarak bir Azure Bastion ana bilgisayar oluşturmak için nasıl gösterir. Azure Bastion hizmetini sanal ağınızda kullanıma saðlamaniz sonra, sorunsuz RDP/SSH deneyimi ayný sanal ağdaki tüm Sanal M'ler için kullanılabilir. Azure Bastion dağıtımı, abonelik/hesap veya sanal makine başına değil, sanal ağ başınadır.

İsteğe bağlı olarak, [Azure portalını](bastion-create-host-portal.md)kullanarak veya Azure [PowerShell'i](bastion-create-host-powershell.md)kullanarak bir Azure Bastion ana bilgisayar oluşturabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

[!INCLUDE [cloudshell cli](../../includes/vpn-gateway-cloud-shell-cli.md)]

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Bastion konağı oluşturma

Bu bölüm, Azure CLI'yi kullanarak yeni bir Azure Bastion kaynağı oluşturmanıza yardımcı olur.

1. Sanal ağ ve Azure Bastion alt ağı oluşturun. **AzureBastionSubnet**ad değerini kullanarak Azure Bastion alt netini oluşturmanız gerekir. Bu değer, Azure'un Bastion kaynaklarını hangi alt ağa dağıtacağını bilmesini sağlar. Bu, Ağ Geçidi alt ağından farklıdır. En az /27 veya daha büyük bir alt ağ (/27, /26 vb.) alt ağı kullanmalısınız. **AzureBastionSubnet'i** herhangi bir rota tabloları veya delegasyonları olmadan oluşturun. **AzureBastionSubnet'te**Ağ Güvenlik Grupları kullanıyorsanız, [NSG'lerle Çalışma](bastion-nsg.md) makalesine bakın.

   ```azurecli-interactive
   az network vnet create -g MyResourceGroup -n MyVnet --address-prefix 10.0.0.0/16 --subnet-name AzureBastionSubnet  --subnet-prefix 10.0.0.0/24
   ```

2. Azure Bastion için genel bir IP adresi oluşturun. Genel IP, RDP/SSH'ye erişilen Bastion kaynağının ortak IP adresidir (bağlantı noktası 443'ten fazladır). Ortak IP adresi, oluşturduğunuz Bastion kaynağıyla aynı bölgede olmalıdır.

   ```azurecli-interactive
   az network public-ip create -g MyResourceGroup -n MyIp
   ```

3. Sanal ağınızın AzureBastionSubnet'inde yeni bir Azure Bastion kaynağı oluşturun. Bastion kaynağının oluşturması ve dağıtması yaklaşık 5 dakika sürer.

   ```azurecli-interactive
   az network bastion create --name $name--public-ip-address $publicip--resource-group $RgName --vnet-name $VNetName --location $location
                           
   ```

## <a name="next-steps"></a>Sonraki adımlar

* Ek bilgi için [Bastion SSS'yi](bastion-faq.md) okuyun.

* Azure Bastion alt ağıyla Ağ Güvenlik Grupları'nı kullanmak için Bkz. [NSG'lerle Çalışma.](bastion-nsg.md)
