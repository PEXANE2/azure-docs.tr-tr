---
title: Azure CLı betik örneği-Azure uygulama yapılandırma deposu oluşturma | Microsoft Docs
description: Azure CLı betik örneği-Azure uygulama yapılandırma deposu oluşturma
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: cd150368898b3f02b4366debbe775ece76b23043
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74184899"
---
# <a name="create-an-azure-app-configuration-store"></a>Azure uygulama yapılandırma deposu oluşturma

Bu örnek betik, yeni bir kaynak grubunda rastgele bir ada sahip yeni bir Azure Uygulama yapılandırması örneği oluşturur.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

Aşağıdaki komutu yürüterek önce Azure uygulama yapılandırma CLı uzantısını yüklemeniz gerekir:

        az extension add -n appconfig

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
  -o tsv)

# Get the AppConfig primary key 
appConfigPrimaryKey=$(az appconfig key list --name $myAppConfigStoreName \
  --resource-group $myResourceGroupName --query primaryKey -o tsv)

# Form the connection string for use in your application
connstring="Endpoint=https://$appConfigHostname;AccessKey=$appConfigPrimaryKey;"
echo "$connstring"
```

Yeni kaynak grubu için oluşturulan gerçek adı not edin. Tüm grup kaynaklarını silmek istediğinizde bu kaynak grubu adını kullanacaksınız.

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yeni bir kaynak grubu ve bir uygulama yapılandırma deposu oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az appconfig Create](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-create) | Bir uygulama yapılandırma deposu kaynağı oluşturur. |
| [az appconfig kV List](/cli/azure/ext/appconfig/appconfig/kv#ext-appconfig-az-appconfig-kv-list) | Bir uygulama yapılandırma deposunda depolanan anahtarları listeleyin. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek uygulama yapılandırması CLı betiği örnekleri, [Azure Uygulama YAPıLANDıRMASı CLI örneklerinde](../cli-samples.md)bulunabilir.
