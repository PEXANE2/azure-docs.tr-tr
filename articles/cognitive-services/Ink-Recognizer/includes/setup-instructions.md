---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: 942bcc6b150f990f9a9acab0d4ef68bfb6125c1b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71996845"
---
>[!NOTE]
> 1 Temmuz 2019'dan sonra oluşturulan kaynakların bitiş noktaları, aşağıda gösterilen özel alt etki alanı biçimini kullanmaktadır. Daha fazla bilgi ve bölgesel uç noktaların tam listesi [için, Bilişsel Hizmetler için Özel alt alan adları bölümüne](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains)bakın. 

Azure Bilişsel Hizmetler, abone olduğunuz Azure kaynakları tarafından temsil edilir. [Azure portalını](../../cognitive-services-apis-create-account.md)kullanarak Mürekkep Tanıyıcısı için bir kaynak oluşturun. 

* Ayrıca ücretsiz olarak yedi gün boyunca geçerli bir [deneme anahtarı](https://azure.microsoft.com/try/cognitive-services/#decision) alabilirsiniz. Kaydolduktan sonra Azure [web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)bir bitiş noktası sunulacaktır.

Bir kaynak oluşturduktan sonra, Azure [portalında](https://ms.portal.azure.com#blade/HubsExtension/BrowseResourceGroupBlade)kaynağınızı açarak ve **Hızlı Başlat'ı**tıklatarak bitiş noktanızı ve anahtarınızı alın.

İki [ortam değişkeni](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource)oluşturun:

* `INK_RECOGNITION_SUBSCRIPTION_KEY`- Kaynağınızın bitiş noktası. Bu gibi görünecektir: <br> `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

* `INK_RECOGNITION_ENDPOINT`- İsteklerinizi doğrulamak için abonelik anahtarı.   
