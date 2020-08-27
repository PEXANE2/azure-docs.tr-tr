---
title: 'CLı: Azure CLı ile Web uygulaması için özel uç nokta dağıtma'
description: Web uygulamanız için özel uç nokta dağıtmak üzere Azure CLı 'yı nasıl kullanacağınızı öğrenin
author: ericgre
ms.assetid: a56faf72-7237-41e7-85ce-da8346f2bcaa
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/06/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: 08e1878adab680329a33bc44020b49dce38de4d6
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88959740"
---
# <a name="create-an-app-service-app-and-deploy-private-endpoint-using-azure-cli"></a>Azure CLı kullanarak App Service uygulaması oluşturma ve özel uç nokta dağıtma

Bu örnek betik, App Service içinde ilgili kaynaklarıyla bir uygulama oluşturur ve sonra özel bir uç nokta dağıtır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLı 'yı yüklemek ve kullanmak isterseniz, bu hızlı başlangıç, Azure CLı sürüm 2.0.28 veya sonraki bir sürümünü kullanmanızı gerektirir. Yüklü sürümünüzü bulmak için öğesini çalıştırın `az --version` . Bkz. Install veya Upgrade Info for [Azure CLI](/cli/azure/install-azure-cli) .

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Herhangi bir kaynak oluşturabilmeniz için önce Web uygulamasını, sanal ağı ve diğer ağ bileşenlerini barındırmak üzere bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Bu örnek, *francecta al* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location francecentral 
```

## <a name="create-an-app-service-plan"></a>App Service Planı oluşturma

Web uygulamanızı barındırmak için bir App Service planı oluşturmanız gerekir.
[Az appservice plan Create](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create)komutuyla bir App Service planı oluşturun.
Bu örnek, *P1V2* SKU 'su ve yalnızca bir Worker ile *francecına al* konumunda *Myappserviceplan* adlı App Service planı oluşturur: 

```azurecli-interactive
az appservice plan create \
--name myAppServicePlan \
--resource-group myResourceGroup \
--location francecentral \
--sku P1V2 \
--number-of-workers 1
```

## <a name="create-a-web-app"></a>Web Uygulaması oluşturma

Artık bir App Service planınız olduğuna göre, bir Web uygulaması dağıtabilirsiniz.
[Az appservice plan Create] (/cli/Azure/WebApp? View = Azure-CLI-latest # az-WebApp-Create ile bir Web uygulaması oluşturun.
Bu örnek, *Myappserviceplan* adlı planda *mysitename* adlı bir Web uygulaması oluşturur

```azurecli-interactive
az webapp create \
--name mySiteName \
--resource-group myResourceGroup \
--plan myAppServicePlan
```

## <a name="create-a-vnet"></a>Sanal ağ oluşturma

[Az Network VNET Create](/cli/azure/network/vnet)komutuyla bir sanal ağ oluşturun. Bu örnek, *Mysubnet*adlı bir alt ağ Ile *myvnet* adlı varsayılan bir sanal ağ oluşturur:

```azurecli-interactive
az network vnet create \
--name myVNet \
--resource-group myResourceGroup \
--location francecentral \
--address-prefixes 10.8.0.0/16 \
--subnet-name mySubnet \
--subnet-prefixes 10.8.100.0/24
```

## <a name="configure-the-subnet"></a>Alt ağı yapılandırma 

Özel uç nokta ağ ilkelerini devre dışı bırakmak için alt ağı güncelleştirmeniz gerekir. [Az Network VNET subnet Update](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update)Ile *mysubnet* adlı bir alt ağ yapılandırmasını güncelleştirin:

```azurecli-interactive
az network vnet subnet update \
--name mySubnet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--disable-private-endpoint-network-policies true
```

## <a name="create-the-private-endpoint"></a>Özel uç nokta oluşturma

[Az Network Private-Endpoint Create](/cli/azure/network/private-endpoint)komutuyla Web uygulamanız Için özel uç nokta oluşturun. Bu örnek, *Myconnectionname* *adlı alt ağda* *Myprivateendpoint* adlı özel *myVNet* BIR uç noktasını, Web uygulamamın kaynak kimliği olan/Subscriptions/SubscriptionID/resourceGroups/myResourceGroup/Providers/Microsoft.Web/Sites/MyWebApp, Grup parametresi ise Web uygulaması için *siteler* olan bir bağlantı ile birlikte oluşturur. 

```azurecli-interactive
az network private-endpoint create \
--name myPrivateEndpoint \
--resource-group myResourceGroup \
--vnet-name myVNet \
--subnet mySubnet \
--connection-name myConnectionName \
--private-connection-resource-id /subscriptions/SubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/myWebApp \
--group-id sites
```

## <a name="configure-the-private-zone"></a>Özel bölgeyi yapılandırma

Son olarak, Web uygulamasının DNS adını çözümlemek için VNet 'e bağlı *Privatelink.azurewebsites.net* adlı özel bir DNS bölgesi oluşturmanız gerekir.


```azurecli-interactive
az network private-dns zone create \
--name privatelink.azurewebsites.net \
--resource-group myResourceGroup

az network private-dns link vnet create \
--name myDNSLink \
--resource-group myResourceGroup \
--registration-enabled false \
--virtual-network myVNet \
--zone-name privatelink.azurewebsites.net

az network private-endpoint dns-zone-group create \
--name myZoneGroup \
--resource-group myResourceGroup \
--endpoint-name myPrivateEndpoint \
--private-dns-zone privatelink.azurewebsites.net \
--zone-name privatelink.azurewebsites.net
```






[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a>Sonraki adımlar

- Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).
- Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde](../samples-cli.md) bulunabilir.