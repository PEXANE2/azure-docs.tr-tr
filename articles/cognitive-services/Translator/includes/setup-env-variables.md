---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 54ee19f3e278b424384ff55d7065713584235df1
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86144237"
---
## <a name="set-up"></a>Kurulum

### <a name="create-a-translator-resource"></a>Çevirmen kaynağı oluşturma

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak çevirmen için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* [Azure Portal](https://portal.azure.com/)var olan bir kaynağı görüntüleyin.

Deneme aboneliğinizden veya kaynağından bir anahtar aldıktan sonra, iki [ortam değişkeni](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)oluşturun:

* `TRANSLATOR_TEXT_SUBSCRIPTION_KEY`-Çevirmen kaynağınız için abonelik anahtarı.
* `TRANSLATOR_TEXT_ENDPOINT`-Çevirmen için genel uç nokta. `https://api.cognitive.microsofttranslator.com/` adresini kullanın.
