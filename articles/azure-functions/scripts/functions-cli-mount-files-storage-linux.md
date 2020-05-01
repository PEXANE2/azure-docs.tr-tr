---
title: Bir Python işlev uygulamasına dosya paylaşma bağlama-Azure CLı
description: Azure CLı kullanarak sunucusuz bir Python işlev uygulaması oluşturun ve var olan bir dosya paylaşımının bağlama.
ms.topic: sample
ms.date: 03/01/2020
ms.openlocfilehash: 3d66f84d124b36f1be335c3a2204f21690510ee8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79086463"
---
# <a name="mount-a-file-share-to-a-python-function-app-using-azure-cli"></a>Azure CLı kullanarak bir Python işlev uygulamasına bir dosya paylaşma bağlama

Bu Azure Işlevleri örnek betiği, bir işlev uygulaması oluşturur ve Azure dosyalarında bir paylaşma oluşturur. Bunlar, verilere işlevleriniz tarafından erişilebilmesi için paylaşımdan takar.  

>[!NOTE]
>Oluşturulan işlev uygulaması Python sürüm 3,7 ' de çalışır. Azure Işlevleri Ayrıca [, 3,6 ve 3,8 Python sürümlerini de destekler](../functions-reference-python.md#python-version).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). Örnekler bash kabuğu için yazılmıştır ve bir Windows komut isteminde çalışacak şekilde değiştirilmelidir. 

## <a name="sample-script"></a>Örnek betik

Bu betik, [Tüketim planını](../functions-scale.md#consumption-plan)kullanarak bir Azure işlev uygulaması oluşturur.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/functions-cli-mount-files-storage-linux/functions-cli-mount-files-storage-linux.sh "Create an Azure Function on a Consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Tablodaki her komut, komuta özgü belgelere yönlendirir. Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Azure Depolama hesabı oluşturur. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Bir işlev uygulaması oluşturur. |
| [az storage share create](/cli/azure/storage/share#az-storage-share-create) | Depolama hesabında bir Azure dosya paylaşma oluşturur. | 
| [az Storage Directory Create](/cli/azure/storage/directory#az-storage-directory-create) | Paylaşımda bir dizin oluşturur. |
| [az WebApp config Storage-Account Add](/cli/azure/webapp/config/storage-account#az-webapp-config-storage-account-add) | , Paylaşılan uygulamayı işlev uygulamasına bağlar. |
| [az WebApp config Storage-Account List](/cli/azure/webapp/config/storage-account#az-webapp-config-storage-account-list) | İşlev uygulamasına bağlanan dosya paylaşımlarını gösterir. | 

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
