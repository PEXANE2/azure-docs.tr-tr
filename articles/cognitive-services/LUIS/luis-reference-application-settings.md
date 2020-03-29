---
title: Uygulama ayarları - LUIS
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler dil anlama uygulamaları nın uygulama ayarları uygulama ve portalda depolanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 11/12/2019
ms.author: diberry
ms.openlocfilehash: d1ead09f6248a6ad14646371aa70b42b57cf8e3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270815"
---
# <a name="application-settings"></a>Uygulama ayarları

Bu uygulama ayarları [dışa aktarılan](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) uygulamada depolanır ve REST API'leri ile [güncelleştirilir.](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) Uygulama sürüm ayarlarınızı değiştirmek, uygulama eğitim durumunuzu eğitimsiz olarak sıfırlar.

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
