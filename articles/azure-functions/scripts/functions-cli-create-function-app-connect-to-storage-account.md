---
title: Bağlı depolama ile işlev uygulaması oluşturma-Azure CLı
description: Azure CLI Betiği Örneği - Azure Depolama’ya bağlanan bir Azure İşlevi oluşturma
ms.topic: sample
ms.date: 04/20/2017
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: b95ec2da96e385ba4595a5552e64d6cc039c0e16
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94565339"
---
# <a name="create-a-function-app-with-a-named-storage-account-connection"></a>Adlandırılmış depolama hesabı bağlantısı ile bir işlev uygulaması oluşturma 

Bu Azure İşlevleri örnek betiği, bir işlev uygulaması oluşturur ve işlevi bir Azure Depolama hesabına bağlar. Bağlantıyı içeren oluşturulan uygulama ayarı, [depolama tetikleyicisi veya bağlaması](../functions-bindings-storage-blob.md)ile birlikte kullanılabilir. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Bu öğretici, Azure CLı 'nin 2,0 veya sonraki bir sürümünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.

## <a name="sample-script"></a>Örnek betik

Bu örnek bir Azure İşlev uygulaması oluşturur ve depolama bağlantı dizesini bir uygulama ayarına ekler.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Konum ile bir kaynak grubu oluşturun. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Depolama hesabı oluşturma. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Sunucusuz [Tüketim planında](../functions-scale.md#consumption-plan)bir işlev uygulaması oluşturur. |
| [az Storage Account Show-Connection-String](/cli/azure/storage/account#az-storage-account-show-connection-string) | Hesap için bağlantı dizesini alır. |
| [az functionapp config appSettings kümesi](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) | İşlev uygulamasındaki uygulama ayarı olarak bağlantı dizesini ayarlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
