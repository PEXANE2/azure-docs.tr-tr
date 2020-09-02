---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: 9d46b304d598b4830cf325909f77eea6b68af757
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304002"
---
>[!NOTE]
> 1 Temmuz 2019 ' den sonra oluşturulan kaynaklar için uç noktalar aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. [Azure Portal](../../cognitive-services-apis-create-account.md)kullanarak mürekkep tanıyıcı için bir kaynak oluşturun.

Kaynak oluşturduktan sonra, [Azure Portal](https://ms.portal.azure.com#blade/HubsExtension/BrowseResourceGroupBlade)kaynağınızı açıp **hızlı başlangıç**' a tıklayarak uç noktanızı ve anahtarınızı alın.

İki [ortam değişkeni](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource)oluşturun:

* `INK_RECOGNITION_SUBSCRIPTION_KEY` -İsteklerinizin kimliğini doğrulamak için abonelik anahtarı. 

* `INK_RECOGNITION_ENDPOINT` -Kaynağınızın bitiş noktası. Şu şekilde görünür: <br> `https://<your-custom-subdomain>.api.cognitive.microsoft.com`   
