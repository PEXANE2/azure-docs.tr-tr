---
ms.topic: include
ms.date: 09/10/2020
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.technology: azure-cli
ms.service: azure-cli
ms.devlang: azurecli
ms.custom: devx-track-azurecli
ms.openlocfilehash: 01e1f956270ebbc9b9d5d0ebdfdff52875cafedf
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96000070"
---
### <a name="prepare-your-environment-for-the-azure-cli"></a>Ortamınızı Azure CLI’ye hazırlama

- Bash ortamından yararlanarak [Azure Cloud Shell](../articles/cloud-shell/quickstart.md)’i kullanın.

   [![Ekli başlatma](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell’i başlatma")](https://shell.azure.com)   
- Dilerseniz CLI başvuru komutlarını çalıştırmak için Azure CLI’yi [yükleyebilirsiniz](/cli/azure/install-azure-cli).
   - Yerel yükleme kullanıyorsanız [az login](/cli/azure/reference-index#az-login) komutunu kullanarak Azure CLI ile oturum açın.  Kimlik doğrulama işlemini tamamlamak için terminalinizde görüntülenen adımları izleyin.  Ek oturum açma seçenekleri için bkz. [Azure CLI ile oturum açma](/cli/azure/authenticate-azure-cli).
  - İstendiğinde, ilk kullanımda Azure CLI uzantılarını yükleyin.  Uzantılar hakkında daha fazla bilgi için bkz. [Azure CLI ile uzantıları kullanma](/cli/azure/azure-cli-extensions-overview).
  - Yüklü sürümü ve bağımlı kitaplıkları bulmak için [az version](/cli/azure/reference-index?#az_version) komutunu çalıştırın. En son sürüme yükseltmek için [az upgrade](/cli/azure/reference-index?#az_upgrade) komutunu çalıştırın.