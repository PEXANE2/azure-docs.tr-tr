---
title: Bir Python işlev uygulamasına dosya paylaşma bağlama-Azure CLı
description: Azure CLı kullanarak sunucusuz bir Python işlev uygulaması oluşturun ve var olan bir dosya paylaşımının bağlama.
ms.topic: sample
ms.date: 03/01/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 00bf0ecc9db3cf369fd75b427dcfba686aed0ed5
ms.sourcegitcommit: c4c554db636f829d7abe70e2c433d27281b35183
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98035130"
---
# <a name="mount-a-file-share-to-a-python-function-app-using-azure-cli"></a>Azure CLı kullanarak bir Python işlev uygulamasına bir dosya paylaşma bağlama

Bu Azure Işlevleri örnek betiği, bir işlev uygulaması oluşturur ve Azure dosyalarında bir paylaşma oluşturur. Bunlar, verilere işlevleriniz tarafından erişilebilmesi için paylaşımdan takar.  

>[!NOTE]
>Oluşturulan işlev uygulaması Python sürüm 3,7 ' de çalışır. Azure Işlevleri Ayrıca [, 3,6 ve 3,8 Python sürümlerini de destekler](../functions-reference-python.md#python-version).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Bu öğretici, Azure CLı 'nin 2,0 veya sonraki bir sürümünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür. 

## <a name="sample-script"></a>Örnek betik

Bu betik, Azure Işlevleri 'nde [Tüketim planını](../consumption-plan.md)kullanarak bir işlev uygulaması oluşturur.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/functions-cli-mount-files-storage-linux/functions-cli-mount-files-storage-linux.sh "Create a function app on a Consumption plan")]

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
