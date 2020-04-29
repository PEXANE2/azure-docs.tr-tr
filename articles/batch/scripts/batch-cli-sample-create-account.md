---
title: Azure CLı betik örneği-Batch hesabı oluşturma-Batch hizmeti
description: Bu betik Batch hizmeti modunda bir Azure Batch hesabı oluşturur ve hesabın çeşitli özelliklerini sorgulamayı veya güncelleştirmeyi gösterir.
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: labrenne
ms.openlocfilehash: 2a11e3d7b8ba2b9f4a0d0d373367575a779eebc4
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77017097"
---
# <a name="cli-example-create-a-batch-account-in-batch-service-mode"></a>CLI örneği: Batch hizmeti modunda Batch hesabı oluşturma

Bu betik Batch hizmeti modunda bir Azure Batch hesabı oluşturur ve hesabın çeşitli özelliklerini sorgulamayı veya güncelleştirmeyi gösterir. Varsayılan Batch hizmeti modunda bir Batch hesabı oluşturduğunuzda, işlem düğümleri Batch hizmeti tarafından dahili olarak atanır. Ayrılmış işlem düğümleri ayrı bir vCPU (çekirdek) kotasına tabidir ve hesap paylaşılan anahtar kimlik bilgileri veya bir Azure Active Directory belirteci aracılığıyla doğrulanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0.20 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). 

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
