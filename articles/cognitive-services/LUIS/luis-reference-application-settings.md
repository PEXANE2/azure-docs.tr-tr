---
title: Uygulama ayarları - LUIS
description: Azure Bilişsel Hizmetler dil anlama uygulamaları nın uygulama ayarları uygulama ve portalda depolanır.
ms.topic: reference
ms.date: 04/14/2020
ms.openlocfilehash: 9e17736cd6ff5074a6eab76a6cf5bdb8acedc185
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382196"
---
# <a name="application-settings"></a>Uygulama ayarları

Bu uygulama ayarları [dışa aktarılan](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) uygulamada depolanır ve REST API'leri ile [güncelleştirilir.](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) Uygulama sürüm ayarlarınızı değiştirmek, uygulama eğitim durumunuzu eğitimsiz olarak sıfırlar.

Aksama ve noktalama [kavramları](luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) öğrenin.

|Ayar|Varsayılan değer|Notlar|
|--|--|--|
|Normalleştirme Noktalama|True|Noktalama işaretlerini kaldırır.|
|NormalizeDiacritics|True|Aksamaları kaldırır.|

## <a name="diacritics-normalization"></a>Akseler normalleştirme

Parametredeki LUIS JSON uygulama dosyanıza aksama `settings` eleştirmenleri için söyleyiş normalleştirmeyi açın.

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
]
```

Aşağıdaki söyleyiş, aksamaların normalleştirmenin söyleyemeleri nasıl etkilediğini gösterir:

|Akdieleştirmenler yanlış ayarlanmış|Akdieleştirmenler doğru ayarlanmış|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||

### <a name="language-support-for-diacritics"></a>Aksateleştirmenler için dil desteği

#### <a name="brazilian-portuguese-pt-br-diacritics"></a>Brezilyalı `pt-br` Portekizli aksateleştirmenler

|Akdeler yanlış olarak ayarlanmış|Aksama eleştirmenleri doğru ayarlandı|
|-|-|
|`á`|`a`|
|`â`|`a`|
|`ã`|`a`|
|`à`|`a`|
|`ç`|`c`|
|`é`|`e`|
|`ê`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ô`|`o`|
|`õ`|`o`|
|`ú`|`u`|
|||

#### <a name="dutch-nl-nl-diacritics"></a>Hollandalı `nl-nl` aksamaeleştirmenleri

|Akdeler yanlış olarak ayarlanmış|Aksama eleştirmenleri doğru ayarlandı|
|-|-|
|`á`|`a`|
|`à`|`a`|
|`é`|`e`|
|`ë`|`e`|
|`è`|`e`|
|`ï`|`i`|
|`í`|`i`|
|`ó`|`o`|
|`ö`|`o`|
|`ú`|`u`|
|`ü`|`u`|
|||

#### <a name="french-fr--diacritics"></a>Fransız `fr-` aksateleştirmenleri

Bu hem Fransız hem de Kanada alt kültürlerini içerir.

|Akdeler yanlış olarak ayarlanmış|Aksama eleştirmenleri doğru ayarlandı|
|--|--|
|`é`|`e`|
|`à`|`a`|
|`è`|`e`|
|`ù`|`u`|
|`â`|`a`|
|`ê`|`e`|
|`î`|`i`|
|`ô`|`o`|
|`û`|`u`|
|`ç`|`c`|
|`ë`|`e`|
|`ï`|`i`|
|`ü`|`u`|
|`ÿ`|`y`|

#### <a name="german-de-de-diacritics"></a>Alman `de-de` aksama eleştirmenleri

|Akdeler yanlış olarak ayarlanmış|Aksama eleştirmenleri doğru ayarlandı|
|--|--|
|`ä`|`a`|
|`ö`|`o`|
|`ü`|`u`|

#### <a name="italian-it-it-diacritics"></a>İtalyan `it-it` akli eleştirmenleri

|Akdeler yanlış olarak ayarlanmış|Aksama eleştirmenleri doğru ayarlandı|
|--|--|
|`à`|`a`|
|`è`|`e`|
|`é`|`e`|
|`ì`|`i`|
|`í`|`i`|
|`î`|`i`|
|`ò`|`o`|
|`ó`|`o`|
|`ù`|`u`|
|`ú`|`u`|

#### <a name="spanish-es--diacritics"></a>İspanyol `es-` aksateleştirmenleri

Buna hem İspanyol hem de Kanada Lı Meksika dahildir.

|Akdeler yanlış olarak ayarlanmış|Aksama eleştirmenleri doğru ayarlandı|
|-|-|
|`á`|`a`|
|`é`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ú`|`u`|
|`ü`|`u`|
|`ñ`|`u`|


## <a name="punctuation-normalization"></a>Noktalama normalleştirme

Parametredeki LUIS JSON uygulama dosyanıza noktalama işaretleri için `settings` söyleyiş normalleştirmesini açın.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
]
```

Aşağıdaki söyleyişler noktalama işaretlerinin söyleyemeleri nasıl etkilediğini gösterir:

|Noktalama işaretleri False olarak ayarlanmış|Noktalama işaretleri True olarak ayarlanmış|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>Noktalama işaretleri kaldırıldı

Aşağıdaki noktalama işaretleri ile `NormalizePunctuation` kaldırılır doğru ayarlanır.

|Noktalama işaretleri|
|--|
|`-`|
|`.`|
|`'`|
|`"`|
|`\`|
|`/`|
|`?`|
|`!`|
|`_`|
|`,`|
|`;`|
|`:`|
|`(`|
|`)`|
|`[`|
|`]`|
|`{`|
|`}`|
|`+`|
|`¡`|

## <a name="next-steps"></a>Sonraki adımlar

* Aksama ve noktalama [kavramları](luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) öğrenin.
