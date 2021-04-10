---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 22a9cf3338f422341928a77f2bf14c497aa2ba31
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105563794"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/dotnet/).
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?tabs=azure-cli) 'yı yükler 

## <a name="create-azure-communication-resource"></a>Azure Iletişim kaynağı oluşturma

Azure Communication Services kaynağı oluşturmak için [Azure CLI 'da oturum açın](/cli/azure/authenticate-azure-cli). Bunu, ```az login``` komutunu kullanarak ve kimlik bilgilerinizi sunarak terminalden yapabilirsiniz. Kaynağı oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli
az communication create --name "<communicationName>" --location "Global" --data-location "United States" --resource-group "<resourceGroup>"
```

Belirli bir abonelik seçmek isterseniz, bayrağı da belirtebilir ```--subscription``` ve ABONELIK kimliğini sağlayabilirsiniz.
```
az communication create --name "<communicationName>" --location "Global" --data-location "United States" --resource-group "<resourceGroup> --subscription "<subscriptionID>"
```

Iletişim Hizmetleri kaynağınızı aşağıdaki seçeneklerle yapılandırabilirsiniz:

* Kaynak grubu
* Iletişim Hizmetleri kaynağının adı
* Kaynağın ilişkilendirileceği Coğrafya

Sonraki adımda, kaynağa etiketler atayabilirsiniz. Etiketler, Azure kaynaklarınızı düzenlemek için kullanılabilir. Etiketler hakkında daha fazla bilgi için bkz. [kaynak etiketleme belgeleri](../../../azure-resource-manager/management/tag-resources.md) .

## <a name="manage-your-communication-services-resource"></a>Iletişim Hizmetleri kaynağınızı yönetme

Iletişim Hizmetleri kaynağına Etiketler eklemek için aşağıdaki komutları çalıştırın. Belirli bir aboneliği de hedefleyebilirsiniz.

```azurecli
az communication update --name "<communicationName>" --tags newTag="newVal1" --resource-group "<resourceGroup>"

az communication update --name "<communicationName>" --tags newTag="newVal2" --resource-group "<resourceGroup>" --subscription "<subscriptionID>"

az communication show --name "<communicationName>" --resource-group "<resourceGroup>"

az communication show --name "<communicationName>" --resource-group "<resourceGroup>" --subscription "<subscriptionID>"
```

Ek komutlar hakkında daha fazla bilgi için bkz. [az Communication](/cli/azure/ext/communication/communication).
