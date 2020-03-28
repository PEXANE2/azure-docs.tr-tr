---
title: Azure CLI Script Örneği - Azure Uygulama Yapılandırma Mağazası Oluşturma
titleSuffix: Azure App Configuration
description: Azure Uygulama Yapılandırma mağazası oluşturmak için Azure CLI Script'i kullanma
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: sample
ms.date: 01/24/2020
ms.author: lcozzens
ms.openlocfilehash: 667cf9545d87bd9277c59d066933710339b6c079
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77623078"
---
# <a name="create-an-azure-app-configuration-store"></a>Azure Uygulama Yapılandırma Mağazası Oluşturma

Bu örnek komut dosyası, yeni bir kaynak grubunda Azure Uygulama Yapılandırmasının yeni bir örneğini oluşturur.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

```azurecli-interactive
#!/bin/bash

appConfigName=myTestAppConfigStore
#resource name must be lowercase
myAppConfigStoreName=${appConfigName,,}
myResourceGroupName=$appConfigName"Group"

# Create resource group 
az group create --name $myResourceGroupName --location eastus

# Create the Azure AppConfig Service resource and query the hostName
appConfigHostname=$(az appconfig create \
  --name $myAppConfigStoreName \
  --location eastus \
  --resource-group $myResourceGroupName \
  --query hostName \
  --sku free \
  -o tsv
  )

# Get the AppConfig connection string 
appConfigConnectionString=$(az appconfig credential list \
--resource-group $myResourceGroupName \
--name $myAppConfigStoreName \
--query "[?name=='Secondary Read Only'] .connectionString" -o tsv)

# Echo the connection string for use in your application
echo "$appConfigConnectionString"
```

Yeni kaynak grubu için oluşturulan gerçek adı not edin. Tüm grup kaynaklarını silmek istediğinizde bu kaynak grubu adını kullanacaksınız.

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, yeni bir kaynak grubu ve bir Uygulama Yapılandırma deposu oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az appconfig oluşturmak](/cli/azure/appconfig#az-appconfig-create) | Bir Uygulama Yapılandırma deposu kaynağı oluşturur. |
| [az appconfig kimlik listesi](/cli/azure/appconfig/credential#az-appconfig-credential-list) | Bir Uygulama Yapılandırma mağazasının erişim anahtarlarını listeleyin. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Uygulama Yapılandırması CLI komut dosyası örnekleri [Azure App Configuration CLI örnekleri](../cli-samples.md)bulunabilir.
