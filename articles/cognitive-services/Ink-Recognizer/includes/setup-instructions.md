---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: 942bcc6b150f990f9a9acab0d4ef68bfb6125c1b
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "71996845"
---
>[!NOTE]
> 1 Temmuz 2019 ' den sonra oluşturulan kaynaklar için uç noktalar aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. [Azure Portal](../../cognitive-services-apis-create-account.md)kullanarak mürekkep tanıyıcı için bir kaynak oluşturun. 

* Ayrıca, ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) edinebilirsiniz. Kaydolduktan sonra, [Azure Web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)bir uç nokta kullanıma sunulacaktır.

Kaynak oluşturduktan sonra, [Azure Portal](https://ms.portal.azure.com#blade/HubsExtension/BrowseResourceGroupBlade)kaynağınızı açıp **hızlı başlangıç**' a tıklayarak uç noktanızı ve anahtarınızı alın.

İki [ortam değişkeni](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource)oluşturun:

* `INK_RECOGNITION_SUBSCRIPTION_KEY`-Kaynağınızın bitiş noktası. Şöyle görünür: <br> `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

* `INK_RECOGNITION_ENDPOINT`-İsteklerinizin kimliğini doğrulamak için abonelik anahtarı.   
