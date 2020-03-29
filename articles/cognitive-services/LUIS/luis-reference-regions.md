---
title: Yayımlama bölgeleri & uç noktalar - LUIS
description: Azure portalında belirtilen bölge, LUIS uygulamasını yayınlayacağınız ve aynı bölge için bir uç nokta URL'si oluşturulduğunuz bölgeyle aynıdır.
ms.topic: reference
ms.date: 11/19/2019
ms.openlocfilehash: a7709d15a51637772d7a4775cd71c307dc21c52b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80292095"
---
# <a name="authoring-and-publishing-regions-and-the-associated-keys"></a>Yazma ve yayımlama bölgeleri ve ilişkili anahtarlar

Üç yazma bölgesi ilgili LUIS portalları tarafından desteklenir. Bir LUIS uygulamasını birden fazla bölgeye yayınlamak için bölge başına en az bir anahtara ihtiyacınız var.

<a name="luis-website"></a>

## <a name="luis-authoring-regions"></a>LUIS Yazma bölgeleri
Bölgeye göre üç LUIS yazma portalı vardır. Aynı bölgede yazmanız ve yayımlamanız gerekir.

|LUIS|Yazma bölgesi|Azure bölge adı|
|--|--|--|
|[www.luis.ai][www.luis.ai] <br>[preview.luis.ai](https://preview.luis.ai)|ABD<br>Avrupa değil<br>Avustralya değil| `westus`|
|[au.luis.ai][au.luis.ai] <br>[preview.au.luis.ai](https://preview.au.luis.ai)|Avustralya| `australiaeast`|
|[eu.luis.ai][eu.luis.ai] <br>[preview.eu.luis.ai](https://preview.eu.luis.ai)|Avrupa|`westeurope`|

Yazma [bölgeleri, başarısız bölgeleri eşledi.](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)

<a name="regions-and-azure-resources"></a>

## <a name="publishing-regions-and-azure-resources"></a>Yayımlama bölgeleri ve Azure kaynakları
Uygulama, LUIS portalına eklenen LUIS kaynaklarıyla ilişkili tüm bölgelerde yayınlanır. Örneğin, [www.luis.ai'da][www.luis.ai]oluşturulan bir uygulama için, **westus'ta** bir LUIS veya Bilişsel Hizmet kaynağı oluşturur ve [bunu bir kaynak olarak uygulamaya eklerseniz,](luis-how-to-azure-subscription.md)uygulama bu bölgede yayınlanır.

## <a name="public-apps"></a>Genel uygulamalar
Bölge tabanlı LUIS kaynak anahtarına sahip bir kullanıcının uygulamanın kaynak anahtarıyla hangi bölgede ilişkili olduğu yla tüm bölgelerde genel bir uygulama yayınlanır.

<a name="publishing-regions"></a>

## <a name="publishing-regions-are-tied-to-authoring-regions"></a>Yayımlama bölgeleri yazma bölgelerine bağlıdır

Yazma bölgesi uygulaması yalnızca ilgili yayımlama bölgesinde yayınlanabilir. Uygulamanız şu anda yanlış yazma bölgesindeyse, uygulamayı dışa aktarın ve yayımlama bölgeniz için doğru yazma bölgesine aktarın.

Luis uygulamaları https://www.luis.ai üzerinde oluşturulan [Avrupa](#publishing-to-europe) ve [Avustralya](#publishing-to-australia) bölgeleri hariç tüm uç noktalarda yayınlanabilir.

## <a name="publishing-to-europe"></a>Avrupa'ya Yayıncılık

Avrupa bölgelerinde yayımlamak için https://eu.luis.ai yalnızca LUIS uygulamaları oluşturursunuz. Avrupa bölgesinde bir anahtar kullanarak başka bir yerde yayımlamaya çalışırsanız, LUIS bir uyarı iletisi görüntüler. Bunun yerine, kullanın. https://eu.luis.ai LUIS uygulamalarında [https://eu.luis.ai][eu.luis.ai] oluşturulan otomatik olarak diğer bölgelere geçirilmemektedir. Geçiş yapmak için LUIS uygulamasını dışa aktarın ve sonra içe aktarın.

## <a name="europe-publishing-regions"></a>Avrupa yayın bölgeleri

 Küresel bölge | YAZMA API bölgesi & yazma web sitesi| & sorgulama bölgesini yayımlama<br>`API region name`   |  Bitiş Noktası URL biçimi   |
|-----|------|------|------|
| [Avrupa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Orta Fransa<br>`francecentral`     | `https://francecentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Avrupa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Kuzey Avrupa<br>`northeurope`     | `https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Avrupa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Batı Avrupa<br>`westeurope`    |  `https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Avrupa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Güney Birleşik Krallık<br>`uksouth`    |  `https://uksouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="publishing-to-australia"></a>Avustralya'da Yayıncılık

Avustralya bölgelerine yayınlamak için https://au.luis.ai yalnızca LUIS uygulamaları oluşturursunuz. Avustralya bölgesinde bir anahtarı kullanarak başka bir yerde yayımlamaya çalışırsanız, LUIS bir uyarı iletisi görüntüler. Bunun yerine, kullanın. https://au.luis.ai LUIS uygulamalarında [https://au.luis.ai][au.luis.ai] oluşturulan otomatik olarak diğer bölgelere geçirilmemektedir. Geçiş yapmak için LUIS uygulamasını dışa aktarın ve sonra içe aktarın.

## <a name="australia-publishing-regions"></a>Avustralya yayın bölgeleri

 Küresel bölge | YAZMA API bölgesi & yazma web sitesi| & sorgulama bölgesini yayımlama<br>`API region name`   |  Bitiş Noktası URL biçimi   |
|-----|------|------|------|
| [Avustralya](#publishing-to-australia) | `australiaeast`<br>[au.luis.ai][au.luis.ai]| Doğu Avustralya<br>`australiaeast`     |  `https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="publishing-to-other-regions"></a>Diğer bölgelere yayımlama

Diğer bölgelere yayımlamak için [https://www.luis.ai](https://www.luis.ai) yalnızca LUIS uygulamaları oluşturursunuz.

## <a name="other-publishing-regions"></a>Diğer yayın bölgeleri

 Küresel bölge | YAZMA API bölgesi & yazma web sitesi| & sorgulama bölgesini yayımlama<br>`API region name`   |  Bitiş Noktası URL biçimi   |
|-----|------|------|------|
| Afrika | `westus`<br>[www.luis.ai][www.luis.ai]| Güney Afrika Kuzey<br>`southafricanorth` |  `https://southafricanorth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Orta Hindistan<br>`centralindia` |  `https://centralindia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Doğu Asya<br>`eastasia`     |  `https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Doğu Japonya<br>`japaneast`     |   `https://japaneast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Batı Japonya<br>`japanwest`     |   `https://japanwest.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Güney Kore - Orta<br>`koreacentral`     |   `https://koreacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Güneydoğu Asya<br>`southeastasia`     |   `https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Orta Kanada<br>`canadacentral`     |   `https://canadacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Orta ABD<br>`centralus`     |   `https://centralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Doğu ABD<br>`eastus`      |  `https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Doğu ABD 2<br>`eastus2`     |  `https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Orta Kuzey ABD<br>`northcentralus`  |  `https://northcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Orta Güney ABD<br>`southcentralus`  |  `https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Orta Batı ABD<br>`westcentralus`    |  `https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Batı ABD<br>`westus`  |   `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`  |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Batı ABD 2<br>`westus2`    |  `https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`  |
| Güney Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Güney Brezilya<br>`brazilsouth`    |  `https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="endpoints"></a>Uç Noktalar

Yazma ve [tahmin bitiş noktaları](developer-reference-resource.md)hakkında daha fazla bilgi edinin.

## <a name="failover-regions"></a>Yük devretme bölgeleri

Her bölgenin başarısız olması gereken ikincil bir bölgesi vardır. Avrupa Avrupa içinde başarısız olurken, Avustralya da Avustralya'da başarısız oluyor.

Yazma [bölgeleri, başarısız bölgeleri eşledi.](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş varlıklar başvurusu](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai
