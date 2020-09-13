---
title: Azure CLı kullanarak bir savunma Konağı oluşturma | Azure savunma
description: Bu makalede, bir savunma konağını oluşturma ve silme hakkında bilgi edinin
services: bastion
author: mialdrid
ms.service: bastion
ms.topic: how-to
ms.date: 03/26/2020
ms.author: mialdrid
ms.openlocfilehash: 8ee90d80230f9115946525ede325e874e98e358e
ms.sourcegitcommit: 70ee014d1706e903b7d1e346ba866f5e08b22761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90024341"
---
# <a name="create-an-azure-bastion-host-using-azure-cli"></a>Azure CLı kullanarak Azure savunma Konağı oluşturma

Bu makalede, Azure CLı kullanarak Azure savunma ana bilgisayarı oluşturma işlemi gösterilmektedir. Sanal ağınızda Azure savunma hizmetini sağladığınızda, sorunsuz RDP/SSH deneyimi aynı sanal ağdaki tüm VM 'Ler tarafından kullanılabilir. Azure savunma dağıtımı, abonelik/hesap veya sanal makine başına değil, sanal ağ başına değildir.

İsteğe bağlı olarak, [Azure Portal](bastion-create-host-portal.md)kullanarak veya [Azure PowerShell](bastion-create-host-powershell.md)kullanarak bir Azure savunma ana bilgisayarı oluşturabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Azure aboneliğiniz olduğunu doğrulayın. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

[!INCLUDE [cloudshell cli](../../includes/vpn-gateway-cloud-shell-cli.md)]

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Savunma Konağı oluşturma

Bu bölüm, Azure CLı kullanarak yeni bir Azure savunma kaynağı oluşturmanıza yardımcı olur.

> [!NOTE]
> Örneklerde gösterildiği gibi, `--location` `--resource-group` kaynakların birlikte dağıtıldığından emin olmak için her komutla birlikte parametresini kullanın.

1. Bir sanal ağ ve bir Azure savunma alt ağı oluşturun. **AzureBastionSubnet**ad değerini kullanarak Azure savunma alt ağını oluşturmanız gerekir. Bu değer, Azure 'un savunma kaynaklarını hangi alt ağa dağıtacağınızı bilmesini sağlar. Bu, bir ağ geçidi alt ağından farklıdır. En az/27 veya daha büyük alt ağın (/27,/26, vb.) bir alt ağını kullanmanız gerekir. Rota tabloları veya temsilcileri olmadan **AzureBastionSubnet** oluşturun. **AzureBastionSubnet**üzerinde ağ güvenlik grupları kullanıyorsanız [Nsgs ile çalışma](bastion-nsg.md) makalesine başvurun.

   ```azurecli-interactive
   az network vnet create --resource-group MyResourceGroup --name MyVnet --address-prefix 10.0.0.0/16 --subnet-name AzureBastionSubnet --subnet-prefix 10.0.0.0/24 --location northeurope
   ```

2. Azure savunma için genel bir IP adresi oluşturun. Genel IP, RDP/SSH 'ye erişilecek savunma kaynağına genel IP adresidir (443 numaralı bağlantı noktası üzerinden). Genel IP adresi, oluşturmakta olduğunuz savunma kaynağıyla aynı bölgede olmalıdır.

   ```azurecli-interactive
   az network public-ip create --resource-group MyResourceGroup --name MyIp --sku Standard --location northeurope
   ```

3. Sanal ağınızın AzureBastionSubnet yeni bir Azure savunma kaynağı oluşturun. Savunma kaynağının oluşturulması ve dağıtılması yaklaşık 5 dakika sürer.

   ```azurecli-interactive
   az network bastion create --name MyBastion --public-ip-address MyIp --resource-group MyResourceGroup --vnet-name MyVnet --location northeurope
   ```

## <a name="next-steps"></a>Sonraki adımlar

* Ek bilgi için savunma [hakkında SSS](bastion-faq.md) makalesini okuyun.

* Azure savunma alt ağıyla ağ güvenlik gruplarını kullanmak için bkz. [NSG Ile çalışma](bastion-nsg.md).
