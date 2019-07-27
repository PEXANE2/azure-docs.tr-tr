---
title: Yayımlama bölgeleri & uç noktaları-LUSıS
titleSuffix: Azure Cognitive Services
description: 3 yazma bölgesi ve portalları birçok yayımlama bölgesini destekler. LUIS uygulamanızı yayımladığınız bölge, bölge veya Azure portalında bir Azure LUIS uç noktası anahtarı oluştururken belirttiğiniz konuma karşılık gelir. LUIS, otomatik olarak bir uygulama yayımladığınızda, bir anahtar ile ilişkili bölge için uç nokta URL'si oluşturur.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/02/2019
ms.author: diberry
ms.openlocfilehash: 44742d2a3d545af6c3f141500ed4d41cabd3d548
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563350"
---
# <a name="authoring-and-publishing-regions-and-the-associated-keys"></a>Yazma ve yayımlama bölgeleri ve ilişkili anahtarlar

Üç yazma bölgesi, karşılık gelen LUI portalları tarafından desteklenir. Birden fazla bölgeye LUIS uygulaması yayımlamak için bölge başına en az bir anahtar gerekir. 

<a name="luis-website"></a>

## <a name="luis-authoring-regions"></a>LUSıS yazma bölgeleri
Bölgeye göre üç LUı yazma portalı vardır. Yazar ve aynı bölgede yayımlama gerekir. 

|LUIS|Yazma bölgesi|Azure bölge adı|
|--|--|--|
|[www.luis.ai][www.luis.ai]|ABD<br>Avrupa'değil<br>Avustralya'değil| `westus`|
|[au.luis.ai][au.luis.ai]|Avustralya| `australiaeast`|
|[eu.luis.ai][eu.luis.ai]|Avrupa|`westeurope`|

Yazma bölgeleri, [Yük devretme bölgelerini eşleştirilmiştir](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

<a name="regions-and-azure-resources"></a>

## <a name="publishing-regions-and-azure-resources"></a>Yayımlama bölgeleri ve Azure kaynakları
Uygulama LUIS Portalı'nda eklenen LUIS kaynaklarla ilişkili tüm bölgeler için yayımlanır. Örneğin, [www.Luis.ai][www.luis.ai]üzerinde oluşturulan bir uygulama için, **WESTUS** 'de bir lusıs veya bilişsel hizmet kaynağı oluşturup [bunu bir kaynak olarak uygulamaya eklerseniz](luis-how-to-azure-subscription.md), uygulama o bölgede yayımlanır. 

## <a name="public-apps"></a>Genel uygulamalar
LUIS kaynak bölge tabanlı anahtarı bir kullanıcıyla hangi bölge kendi kaynak anahtarı ile ilişkili uygulama erişebilmesi için tüm bölgelerde genel bir uygulama yayımlanır.

<a name="publishing-regions"></a>

## <a name="publishing-regions-are-tied-to-authoring-regions"></a>Yayımlama bölgeleri yazma bölgelerine bağlıdır

Yazma bölgesi uygulama yalnızca bir karşılık gelen yayımlanabilir bölge yayımlayın. Uygulamanız şu anda geliştirme yanlış bölgede ise, uygulamayı dışarı aktarma ve yayımlama bölgeniz için doğru yazma bölgesini almak. 

LUIS uygulamalar üzerinde oluşturulan https://www.luis.ai tüm uç noktalar için yayımlanan [Avrupa](#publishing-to-europe) ve [Avustralya](#publishing-to-australia) bölgeleri. 

## <a name="publishing-to-europe"></a>Avrupa yayımlama

Uygulamaları LUIS oluşturduğunuz Avrupa bölgelerine yayımlamak için https://eu.luis.ai yalnızca. LUIS, herhangi bir başka Avrupa bölgesinde bir anahtar kullanarak yayımlamak çalışırsanız, bir uyarı iletisi görüntüler. Bunun yerine, https://eu.luis.ai. Tarafından oluşturulan [https://eu.luis.ai][eu.luis.ai] lusıs uygulamaları diğer bölgelere otomatik olarak geçirilmez. Dışarı aktarma ve ardından geçirmek için LUIS uygulaması alın.

## <a name="europe-publishing-regions"></a>Avrupa yayımlama bölgeleri

 Genel bölge | Yazma API bölgesi & yazma Web sitesi| Yayımlama & bölgesi sorgulama<br>`API region name`   |  Uç nokta URL'si biçimi   |
|-----|------|------|------|
| [Avrupa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Fransa Orta<br>`francecentral`     | https://francecentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| [Avrupa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Kuzey Avrupa<br>`northeurope`     | https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| [Avrupa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Batı Avrupa<br>`westeurope`    |  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| [Avrupa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Birleşik Krallık Güney<br>`uksouth`    |  https://uksouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-australia"></a>Avustralya yayımlama

Avustralya bölgeleri için yayımlamak için LUIS uygulamaları oluşturmanız https://au.luis.ai yalnızca. LUIS, herhangi bir başka Avustralya bölgesinde bir anahtar kullanarak yayımlamak çalışırsanız, bir uyarı iletisi görüntüler. Bunun yerine, https://au.luis.ai. Tarafından oluşturulan [https://au.luis.ai][au.luis.ai] lusıs uygulamaları diğer bölgelere otomatik olarak geçirilmez. Dışarı aktarma ve ardından geçirmek için LUIS uygulaması alın.

## <a name="australia-publishing-regions"></a>Avustralya yayımlama bölgeleri

 Genel bölge | Yazma API bölgesi & yazma Web sitesi| Yayımlama & bölgesi sorgulama<br>`API region name`   |  Uç nokta URL'si biçimi   |
|-----|------|------|------|
| [Avustralya](#publishing-to-australia) | `australiaeast`<br>[au.luis.ai][au.luis.ai]| Avustralya Doğu<br>`australiaeast`     |  https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-other-regions"></a>Diğer bölgelere yayımlama

Diğer bölgelere yayımlamak için [https://www.luis.ai](https://www.luis.ai) yalnızca lusıs uygulamaları oluşturursunuz. 

## <a name="other-publishing-regions"></a>Diğer yayımlama bölgeleri

 Genel bölge | Yazma API bölgesi & yazma Web sitesi| Yayımlama & bölgesi sorgulama<br>`API region name`   |  Uç nokta URL'si biçimi   |
|-----|------|------|------|
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Orta Hindistan<br>`centralindia` |  https://centralindia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Doğu Asya<br>`eastasia`     |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Japonya Doğu<br>`japaneast`     |   https://japaneast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Japonya Batı<br>`japanwest`     |   https://japanwest.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Kore Orta<br>`koreacentral`     |   https://koreacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Güneydoğu Asya<br>`southeastasia`     |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Orta Kanada<br>`canadacentral`     |   https://canadacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Orta ABD<br>`centralus`     |   https://centralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | East US<br>`eastus`      |  https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Doğu ABD 2<br>`eastus2`     |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Orta Kuzey ABD<br>`northcentralus`  |  https://northcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Orta Güney ABD<br>`southcentralus`  |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Batı Orta ABD<br>`westcentralus`    |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Batı ABD<br>`westus`  |   https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Batı ABD 2<br>`westus2`    |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Güney Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Güney Brezilya<br>`brazilsouth`    |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="endpoints"></a>Uç Noktalar

LUSıS Şu anda 2 uç noktaya sahiptir: bir diğeri sorgu tahmin analizi için.

|Amaç|URL'si|
|--|--|
|Yazma|`https://{region}.api.cognitive.microsoft.com/luis/api/v2.0/apps/{appID}/`|
|Metin analizi (sorgu tahmin)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]`|

Küme ayracı ile belirtilen parametreler, aşağıdaki tabloda açıklanmaktadır `{}`, önceki tabloda.

|Parametre|Amaç|
|--|--|
|bölge|Farklı bölgelerdeki Azure bölgesi - yazma ve yayımlama sahip|
|Uygulama Kimliği|URL rotasına kullanılan ve uygulama panosunda bulunan LUIS uygulama kimliği|
|q|Sohbet Robotu gibi istemci uygulamasından gönderilen utterance metin|

## <a name="failover-regions"></a>Yük devretme bölgeleri

Her bölgenin yük devretmek için bir ikincil bölgesi vardır. Avrupa, Avrupa ve Avustralya 'Nın içinde devredilmekte ve Avustralya 'Nın içinde devredildi.

Yazma bölgeleri, [Yük devretme bölgelerini eşleştirilmiştir](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş varlıklar başvurusu](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai
