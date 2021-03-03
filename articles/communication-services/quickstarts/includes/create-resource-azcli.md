---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 1/28/2021
ms.author: mikben
ms.openlocfilehash: 32d869e6285e76b57a7b8e462e8110a43de82dd9
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101656587"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/dotnet/).

## <a name="create-azure-communication-resource"></a>Azure Iletişim kaynağı oluşturma

Azure Iletişim Hizmetleri kaynağı oluşturmak için [Azure CLI 'da oturum açın](/cli/azure/authenticate-azure-cli)ve ardından şu komutu çalıştırın:

```azurecli
az communication create --name "<communicationName>" --location "Global" --data-location "United States" --resource-group "<resourceGroup>"
```

Iletişim Hizmetleri kaynağınızı aşağıdaki seçeneklerle yapılandırabilirsiniz:

* Kaynak grubu
* Iletişim Hizmetleri kaynağının adı
* Kaynağın ilişkilendirileceği Coğrafya

Sonraki adımda, kaynağa etiketler atayabilirsiniz. Etiketler, Azure kaynaklarınızı düzenlemek için kullanılabilir. Etiketler hakkında daha fazla bilgi için bkz. [kaynak etiketleme belgeleri](../../../azure-resource-manager/management/tag-resources.md) .

## <a name="manage-your-communication-services-resource"></a>Iletişim Hizmetleri kaynağınızı yönetme

Iletişim Hizmetleri kaynağına Etiketler eklemek için aşağıdaki komutları çalıştırın:

```azurecli
az communication update --name "<communicationName>" --tags newTag="newVal" --resource-group "<resourceGroup>"

az communication show --name "<communicationName>" --resource-group "<resourceGroup>"
```

Ek komutlar hakkında daha fazla bilgi için bkz. [az Communication](/cli/azure/ext/communication/communication).