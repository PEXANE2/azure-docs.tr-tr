---
title: Azure CLı betik örneği-Batch hesabı oluşturma-kullanıcı aboneliği
description: Bu betik, kullanıcı aboneliği modunda bir Azure Batch hesabı oluşturur. Bu hesap, işlem düğümlerini aboneliğinize ayırır.
ms.topic: sample
ms.date: 01/29/2018
ms.custom: devx-track-azurecli
ms.openlocfilehash: c9b8ba2ef782dcdc99cb18698175b8b53a53f0dd
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93076784"
---
# <a name="cli-example-create-a-batch-account-in-user-subscription-mode"></a>CLI örneği: Kullanıcı aboneliği modunda Batch hesabı oluşturma

Bu betik, kullanıcı aboneliği modunda bir Azure Batch hesabı oluşturur. İşlem düğümlerini aboneliğinize ayıran bir hesabın Azure Active Directory belirteci aracılığıyla kimliğinin doğrulanması gerekir. Ayrılmış işlem düğümleri, aboneliğinizin vCPU (çekirdek) kotasını etkiler. 

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Bu öğretici, Azure CLı 'nin sürüm 2.0.20 veya üstünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.  

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh "Create Account using user subscription")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az role assignment create](/cli/azure/role) | Bir kullanıcı, grup veya hizmet sorumlusu için yeni bir rol ataması oluşturun. |
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az keyvault create](/cli/azure/keyvault#az-keyvault-create) | Bir anahtar kasasını güncelleştirir. |
| [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy) | Belirtilen anahtar kasasının güvenlik ilkesini güncelleştirin. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Batch hesabını oluşturur.  |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Daha fazla CLI etkileşimi için belirtilen Batch hesabına karşı kimlik doğrulaması yapar.  |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).
