---
title: Yayımlama bölgeleri & uç noktaları-LUSıS
description: Azure portal belirtilen bölge, LUO uygulamasını yayımlayacağınız yer ve aynı bölge için bir uç nokta URL 'SI oluşturulur.
ms.topic: reference
ms.date: 11/19/2019
ms.openlocfilehash: a7709d15a51637772d7a4775cd71c307dc21c52b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80292095"
---
# <a name="authoring-and-publishing-regions-and-the-associated-keys"></a>Yazma ve yayımlama bölgeleri ve ilişkili anahtarlar

Üç yazma bölgesi, karşılık gelen LUI portalları tarafından desteklenir. Bir LUO uygulamasını birden fazla bölgeye yayımlamak için, bölge başına en az bir anahtara ihtiyacınız vardır.

<a name="luis-website"></a>

## <a name="luis-authoring-regions"></a>LUSıS yazma bölgeleri
Bölgeye göre üç LUı yazma portalı vardır. Aynı bölgede yazmanız ve yayımlamanız gerekir.

|LUIS|Yazma bölgesi|Azure bölge adı|
|--|--|--|
|[www.luis.ai][www.luis.ai] <br>[preview.luis.ai](https://preview.luis.ai)|ABD<br>Avrupa değil<br>Avustralya değil| `westus`|
|[au.luis.ai][au.luis.ai] <br>[preview.au.luis.ai](https://preview.au.luis.ai)|Avustralya| `australiaeast`|
|[eu.luis.ai][eu.luis.ai] <br>[preview.eu.luis.ai](https://preview.eu.luis.ai)|Avrupa|`westeurope`|

Yazma bölgeleri, [Yük devretme bölgelerini eşleştirilmiştir](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

<a name="regions-and-azure-resources"></a>

## <a name="publishing-regions-and-azure-resources"></a>Yayımlama bölgeleri ve Azure kaynakları
Uygulama, Lue portalına eklenen LUO kaynaklarıyla ilişkili tüm bölgelerde yayımlanır. Örneğin, [www.Luis.ai][www.luis.ai]üzerinde oluşturulan bir uygulama için, **WESTUS** 'de bir lusıs veya bilişsel hizmet kaynağı oluşturup [bunu bir kaynak olarak uygulamaya eklerseniz](luis-how-to-azure-subscription.md), uygulama o bölgede yayımlanır.

## <a name="public-apps"></a>Ortak uygulamalar
Ortak bir uygulama tüm bölgelerde yayımlanır, böylece bölge tabanlı bir LUTO kaynak anahtarı olan bir Kullanıcı, kaynak anahtarı ile ilişkili olan herhangi bir bölgede uygulamaya erişebilir.

<a name="publishing-regions"></a>

## <a name="publishing-regions-are-tied-to-authoring-regions"></a>Yayımlama bölgeleri yazma bölgelerine bağlıdır

Yazma bölgesi uygulaması yalnızca ilgili yayımlama bölgesine yayımlanabilir. Uygulamanız Şu anda yanlış yazma bölgedeyse, uygulamayı dışarı aktarın ve yayımlama bölgeniz için doğru yazma bölgesine aktarın.

Üzerinde https://www.luis.ai oluşturulan lusıs uygulamaları [Avrupa](#publishing-to-europe) ve [Avustralya](#publishing-to-australia) bölgeleri hariç tüm uç noktalara yayımlanabilir.

## <a name="publishing-to-europe"></a>Avrupa 'da yayımlama

Avrupa bölgelerine yayımlamak için https://eu.luis.ai yalnızca lusıs uygulamaları oluşturursunuz. Avrupa bölgesinde bir anahtar kullanarak başka bir yerde yayımlamayı denerseniz, Luz bir uyarı mesajı görüntüler. Bunun yerine, https://eu.luis.aikullanın. Tarafından oluşturulan [https://eu.luis.ai][eu.luis.ai] lusıs uygulamaları diğer bölgelere otomatik olarak geçirilmez. ' İ dışa aktarıp, daha sonra geçirmek için LUO uygulamasını içeri aktarın.

## <a name="europe-publishing-regions"></a>Avrupa yayımlama bölgeleri

 Genel bölge | Yazma API bölgesi & yazma Web sitesi| Bölge & yayımlanıyor<br>`API region name`   |  Uç nokta URL biçimi   |
|-----|------|------|------|
| [Avrupa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Orta Fransa<br>`francecentral`     | `https://francecentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Avrupa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Kuzey Avrupa<br>`northeurope`     | `https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Avrupa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Batı Avrupa<br>`westeurope`    |  `https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Avrupa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Güney Birleşik Krallık<br>`uksouth`    |  `https://uksouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="publishing-to-australia"></a>Avustralya 'da yayımlama

Avustralya bölgelerinde yayımlamak için https://au.luis.ai yalnızca lusıs uygulamaları oluşturursunuz. Avustralya bölgesindeki bir anahtar kullanarak başka bir yerde yayımlamayı denerseniz, Luz bir uyarı mesajı görüntüler. Bunun yerine, https://au.luis.aikullanın. Tarafından oluşturulan [https://au.luis.ai][au.luis.ai] lusıs uygulamaları diğer bölgelere otomatik olarak geçirilmez. ' İ dışa aktarıp, daha sonra geçirmek için LUO uygulamasını içeri aktarın.

## <a name="australia-publishing-regions"></a>Avustralya yayımlama bölgeleri

 Genel bölge | Yazma API bölgesi & yazma Web sitesi| Bölge & yayımlanıyor<br>`API region name`   |  Uç nokta URL biçimi   |
|-----|------|------|------|
| [Avustralya](#publishing-to-australia) | `australiaeast`<br>[au.luis.ai][au.luis.ai]| Doğu Avustralya<br>`australiaeast`     |  `https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="publishing-to-other-regions"></a>Diğer bölgelere yayımlama

Diğer bölgelere yayımlamak için [https://www.luis.ai](https://www.luis.ai) yalnızca lusıs uygulamaları oluşturursunuz.

## <a name="other-publishing-regions"></a>Diğer yayımlama bölgeleri

 Genel bölge | Yazma API bölgesi & yazma Web sitesi| Bölge & yayımlanıyor<br>`API region name`   |  Uç nokta URL biçimi   |
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

[Yazma ve tahmin uç noktaları](developer-reference-resource.md)hakkında daha fazla bilgi edinin.

## <a name="failover-regions"></a>Yük devretme bölgeleri

Her bölgenin yük devretmek için bir ikincil bölgesi vardır. Avrupa, Avrupa ve Avustralya 'Nın içinde devredilmekte ve Avustralya 'Nın içinde devredildi.

Yazma bölgeleri, [Yük devretme bölgelerini eşleştirilmiştir](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş varlıklar başvurusu](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai
