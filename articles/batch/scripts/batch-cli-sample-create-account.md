---
title: Azure CLı betik örneği-Batch hesabı oluşturma-Batch hizmeti
description: Bu betik Batch hizmeti modunda bir Azure Batch hesabı oluşturur ve hesabın çeşitli özelliklerini sorgulamayı veya güncelleştirmeyi gösterir.
ms.topic: sample
ms.date: 01/29/2018
ms.custom: devx-track-azurecli
ms.openlocfilehash: 2349b6b373f271a5aa0f169e5a9ebc9f58f6f608
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93076819"
---
# <a name="cli-example-create-a-batch-account-in-batch-service-mode"></a>CLI örneği: Batch hizmeti modunda Batch hesabı oluşturma

Bu betik Batch hizmeti modunda bir Azure Batch hesabı oluşturur ve hesabın çeşitli özelliklerini sorgulamayı veya güncelleştirmeyi gösterir. Varsayılan Batch hizmeti modunda bir Batch hesabı oluşturduğunuzda, işlem düğümleri Batch hizmeti tarafından dahili olarak atanır. Ayrılmış işlem düğümleri ayrı bir vCPU (çekirdek) kotasına tabidir ve hesap paylaşılan anahtar kimlik bilgileri veya bir Azure Active Directory belirteci aracılığıyla doğrulanabilir.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Bu öğretici, Azure CLı 'nin sürüm 2.0.20 veya üstünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür. 

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Batch hesabını oluşturur. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Bir depolama hesabı oluşturur. |
| [az batch account set](/cli/azure/batch/account#az-batch-account-set) | Batch hesabının özelliklerini güncelleştirir.  |
| [az batch account show](/cli/azure/batch/account#az-batch-account-show) | Belirtilen Batch hesabının ayrıntılarını alır.  |
| [az batch account keys list](/cli/azure/batch/account/keys#az-batch-account-keys-list) | Belirtilen Batch hesabının erişim anahtarlarını alır.  |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Daha fazla CLI etkileşimi için belirtilen Batch hesabına karşı kimlik doğrulaması yapar.  |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).
