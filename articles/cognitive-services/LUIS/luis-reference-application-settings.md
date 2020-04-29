---
title: Uygulama ayarları-LUSıS
description: Azure bilişsel hizmetler dil dilleri uygulamaları için uygulama ayarları, uygulama ve portalda depolanır.
ms.topic: reference
ms.date: 04/14/2020
ms.openlocfilehash: 9e17736cd6ff5074a6eab76a6cf5bdb8acedc185
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81382196"
---
# <a name="application-settings"></a>Uygulama ayarları

Bu uygulama ayarları, [dışarıya](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) kaydedilen uygulamada DEPOLANıR ve REST API 'leri ile [güncelleştirilir](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) . Uygulama sürümü ayarlarınızı değiştirmek, uygulama eğitim durumunuzu eğitilen olarak sıfırlar.

Aksanların ve noktalama işaretlerinin [kavramlarını](luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) öğrenin.

|Ayar|Varsayılan değer|Notlar|
|--|--|--|
|Normalizenoktalama|True|Noktalama işaretlerini kaldırır.|
|Normalizediacritika|True|Aksanları kaldırır.|

## <a name="diacritics-normalization"></a>Aksanların normalleştirilmesi

`settings` Parametresindeki lusıs JSON uygulama dosyanıza Aksanlar için söylenişi normalleştirmesini açın.

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
]
```

Aşağıdaki söyleyenlerdeki aksanların normalleştirmeyi nasıl etkilediği gösterilmektedir:

|Aksanların yanlış olarak ayarlandığı|Aksanların değeri true olarak ayarlandı|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||

### <a name="language-support-for-diacritics"></a>Aksanlar için dil desteği

#### <a name="brazilian-portuguese-pt-br-diacritics"></a>Brezilya Portekizcesi `pt-br` vurguları

|Aksanların değeri false olarak ayarlandı|Aksanların değeri true olarak ayarlandı|
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

#### <a name="dutch-nl-nl-diacritics"></a>Hollanda `nl-nl` vurgu işaretleri

|Aksanların değeri false olarak ayarlandı|Aksanların değeri true olarak ayarlandı|
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

#### <a name="french-fr--diacritics"></a>Fransızca `fr-` aksanlar

Bu hem Fransızca hem de Kanada alt kültürlerini içerir.

|Aksanların değeri false olarak ayarlandı|Aksanların değeri true olarak ayarlandı|
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

#### <a name="german-de-de-diacritics"></a>Alman `de-de` vurguları

|Aksanların değeri false olarak ayarlandı|Aksanların değeri true olarak ayarlandı|
|--|--|
|`ä`|`a`|
|`ö`|`o`|
|`ü`|`u`|

#### <a name="italian-it-it-diacritics"></a>İtalyanca `it-it` aksanlar

|Aksanların değeri false olarak ayarlandı|Aksanların değeri true olarak ayarlandı|
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

#### <a name="spanish-es--diacritics"></a>İspanyolca `es-` aksanlar

Buna hem İspanyolca hem de Kanada Meksika dahildir.

|Aksanların değeri false olarak ayarlandı|Aksanların değeri true olarak ayarlandı|
|-|-|
|`á`|`a`|
|`é`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ú`|`u`|
|`ü`|`u`|
|`ñ`|`u`|


## <a name="punctuation-normalization"></a>Noktalama normalleştirmesi

Noktalama işaretleri için, `settings` parametresindeki halın JSON uygulama dosyanıza noktalama işareti normalleştirmesini açın.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
]
```

Aşağıdaki noktalamalar, noktalama işaretlerinin nasıl etkilediğini göstermektedir:

|Noktalama işareti yanlış olarak ayarlandı|Noktalama işareti doğru olarak ayarlandı|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>Noktalama kaldırıldı

Aşağıdaki noktalama işaretleri `NormalizePunctuation` true olarak ayarlanmıştır.

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

* Aksanların ve noktalama işaretlerinin [kavramlarını](luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) öğrenin.
