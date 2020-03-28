---
title: Azure CLI Script Örneği - App Configuration Store'da anahtar değerlerle çalışma
titleSuffix: Azure App Configuration
description: Uygulama Yapılandırma deposundan temel değerleri oluşturmak, görüntülemek, güncelleştirmek ve silmek için Azure CLI komut dosyasını kullanın
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/19/2020
ms.author: lcozzens
ms.openlocfilehash: 1a4edabe666a554ccd01d110f0f71226221dfc67
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77523666"
---
# <a name="work-with-key-values-in-an-azure-app-configuration-store"></a>Azure Uygulama Yapılandırma mağazasında anahtar değerlerle çalışma

Bu örnek komut dosyası nasıl gösterin:
* Yeni bir anahtar değer çifti oluşturma
* Varolan tüm anahtar değeri çiftlerini listele
* Yeni oluşturulan anahtarın değerini güncelleştirme
* Yeni anahtar değeri çiftini silme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bu makalede Azure CLI sürüm 2.0 veya sonrası gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

```azurecli-interactive
#!/bin/bash

appConfigName=myTestAppConfigStore
newKey="TestKey"
refKey="KeyVaultReferenceTestKey"
uri="[URL to value stored in Key Vault]"
uri2="[URL to another value stored in Key Vault]"

# Create a new key-value 
az appconfig kv set --name $appConfigName --key $newKey --value "Value 1"

# List current key-values
az appconfig kv list --name $appConfigName

# Update new key's value
az appconfig kv set --name $appConfigName --key $newKey --value "Value 2"

# List current key-values
az appconfig kv list --name $appConfigName

# Create a new key-value referencing a value stored in Azure Key Vault
az appconfig kv set --name $appConfigName --key $refKey --content-type "application/vnd.microsoft.appconfig.keyvaultref+json;charset=utf-8" --value "{\"uri\":\"$uri\"}"

# List current key-values
az appconfig kv list --name $appConfigName

# Update Key Vault reference
az appconfig kv set --name $appConfigName --key $refKey --value "{\"uri\":\"$uri2\"}"

# List current key-values
az appconfig kv list --name $appConfigName

# Delete new key
az appconfig kv delete  --name $appConfigName --key $newKey

# Delete Key Vault reference
az appconfig kv delete --name $appConfigName --key $refKey

# List current key-values
az appconfig kv list --name $appConfigName
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu tablo, örnek komutumuzda kullanılan komutları listeler. 

| Komut | Notlar |
|---|---|
| [az appconfig kv seti](/cli/azure/appconfig/kv#az-appconfig-kv-set) | Anahtar değeri çifti oluşturun veya güncelleştirin. |
| [az appconfig kv listesi](/cli/azure/appconfig/kv#az-appconfig-kv-list) | Bir Uygulama Yapılandırma mağazasında anahtar değeri çiftleri listeleyin. |
| [az appconfig kv silme](/cli/azure/appconfig/kv#az-appconfig-kv-delete) | Anahtar değeri çiftini silin. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Uygulama Yapılandırması CLI komut dosyası örnekleri [Azure App Configuration CLI örnekleri](../cli-samples.md)bulunabilir.
