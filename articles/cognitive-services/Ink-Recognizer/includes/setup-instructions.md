---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: ede1fb4bd2a9a6e6536959053e3ca4d8e4a82f87
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91327382"
---
>[!NOTE]
> 1 Temmuz 2019 ' den sonra oluşturulan kaynaklar için uç noktalar aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Azure bilişsel hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. [Azure Portal](../../cognitive-services-apis-create-account.md)kullanarak mürekkep tanıyıcı için bir kaynak oluşturun.

Kaynak oluşturduktan sonra, [Azure Portal](https://ms.portal.azure.com#blade/HubsExtension/BrowseResourceGroupBlade)kaynağınızı açıp **hızlı başlangıç**' a tıklayarak uç noktanızı ve anahtarınızı alın.

İki [ortam değişkeni](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource)oluşturun:

* `INK_RECOGNITION_SUBSCRIPTION_KEY` -İsteklerinizin kimliğini doğrulamak için abonelik anahtarı. 

* `INK_RECOGNITION_ENDPOINT` -Kaynağınızın bitiş noktası. Şu şekilde görünür: <br> `https://<your-custom-subdomain>.api.cognitive.microsoft.com`   
