---
title: Python işlev uygulamasına dosya paylaşımı nı takma - Azure CLI
description: Sunucusuz bir Python işlev uygulaması oluşturun ve Azure CLI'yi kullanarak varolan bir dosya paylaşımını birleştirin.
ms.topic: sample
ms.date: 03/01/2020
ms.openlocfilehash: 3d66f84d124b36f1be335c3a2204f21690510ee8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79086463"
---
# <a name="mount-a-file-share-to-a-python-function-app-using-azure-cli"></a>Azure CLI'yi kullanarak Python işlev uygulamasına dosya paylaşımı nı montaj

Bu Azure İşlevler örnek komut dosyası bir işlev uygulaması oluşturur ve Azure Dosyaları'nda bir paylaşım oluşturur. Verilere işlevleriniz tarafından erişilebilmeleri için paylaşımı bağlar.  

>[!NOTE]
>Oluşturulan işlev uygulaması Python sürüm 3.7'de çalışır. Azure İşlevleri, [Python 3.6 ve 3.8 sürümlerini](../functions-reference-python.md#python-version)de destekler.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). Örnekler Bash kabuğu için yazılır ve Windows komut isteminde çalışacak şekilde değiştirilmesi gerekir. 

## <a name="sample-script"></a>Örnek betik

Bu komut dosyası, [Tüketim planını](../functions-scale.md#consumption-plan)kullanarak bir Azure İşlevi uygulaması oluşturur.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/functions-cli-mount-files-storage-linux/functions-cli-mount-files-storage-linux.sh "Create an Azure Function on a Consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Tablodaki her komut, komuta özgü belgelere yönlendirir. Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Azure Depolama hesabı oluşturur. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Bir işlev uygulaması oluşturur. |
| [az storage share create](/cli/azure/storage/share#az-storage-share-create) | Depolama hesabında bir Azure Files paylaşımı oluşturur. | 
| [az depolama dizini oluşturmak](/cli/azure/storage/directory#az-storage-directory-create) | Paylaşımda bir dizin oluşturur. |
| [az webapp config depolama-hesap eklemek](/cli/azure/webapp/config/storage-account#az-webapp-config-storage-account-add) | Paylaşımı işlev uygulamasına bağlar. |
| [az webapp config depolama-hesap listesi](/cli/azure/webapp/config/storage-account#az-webapp-config-storage-account-list) | İşlev uygulamasına monte edilmiş dosya paylaşımlarını gösterir. | 

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
