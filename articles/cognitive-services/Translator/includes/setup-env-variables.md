---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: c737447c3a3bd2d76d3ed620b7c61aaa81250130
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70393836"
---
## <a name="set-up"></a>Ayarla

### <a name="create-a-translator-text-resource"></a>Çevirmen Metin kaynağı oluşturma

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. Yerel makinenizde [Azure portalını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI'yi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak Çevirmen Metni için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* Ücretsiz olarak 7 gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services) alın. Kaydolduktan sonra Azure web sitesinde kullanılabilir.
* [Azure portalında](https://portal.azure.com/)varolan bir kaynağı görüntüleyin.

Deneme aboneliğinizden veya kaynağınızdan bir anahtar aldıktan sonra, iki [ortam değişkeni](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)oluşturun:

* `TRANSLATOR_TEXT_SUBSCRIPTION_KEY`- Çevirmen Metin kaynağınızın abonelik anahtarı.
* `TRANSLATOR_TEXT_ENDPOINT`- Çevirmen Metni için küresel bitiş noktası. `https://api.cognitive.microsofttranslator.com/` adresini kullanın.
