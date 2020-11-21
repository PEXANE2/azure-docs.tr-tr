---
title: Bilişsel Hizmetler kapsayıcı görüntüsü etiketleri
titleSuffix: Azure Cognitive Services
description: Tüm bilişsel hizmet kapsayıcısı resim etiketlerinin kapsamlı bir listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 11/17/2020
ms.author: aahi
ms.openlocfilehash: 90ac4891dbca34ff2d9bdb9653e27ad2a495b317
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95021261"
---
# <a name="azure-cognitive-services-container-image-tags-and-release-notes"></a>Azure bilişsel hizmetler kapsayıcı görüntü etiketleri ve sürüm notları

Azure bilişsel hizmetler birçok kapsayıcı görüntüsü sunar. Kapsayıcı kayıt defterleri ve ilgili depolar kapsayıcı görüntüleri arasında farklılık gösterir. Her kapsayıcı görüntüsü adı birden çok etiket sunar. Kapsayıcı görüntüsü etiketi, kapsayıcı görüntüsünü sürüm oluşturma mekanizmasından oluşur. Bu makale, tüm bilişsel hizmetler kapsayıcı görüntülerini ve bunların kullanılabilir etiketlerini listelemek için kapsamlı bir başvuru olarak kullanılmak üzere tasarlanmıştır.

> [!TIP]
> Kullanırken [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) , **büyük/küçük harfe duyarlı** olduğu için kapsayıcı kayıt defteri, depo, kapsayıcı görüntüsü adı ve karşılık gelen etiket büyük küçük harfe yakın bir ilgi ödeyin.

## <a name="anomaly-detector"></a>Anomali Algılayıcısı

[Anomali algılayıcı][ad-containers] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/decision` ve olarak adlandırılır `anomaly-detector` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/decision/anomaly-detector/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-version"></a>[En son sürüm](#tab/current)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013560003-amd64-preview` |      |

# <a name="previous-versions"></a>[Önceki sürümler](#tab/previous)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `1.1.012300001-amd64-preview` |       |

---

## <a name="read-ocr-optical-character-recognition"></a>OCR 'yi oku (optik karakter tanıma)

[Görüntü işleme][cv-containers] okuma OCR kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services` ve olarak adlandırılır `read` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/vision/read` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/vision/read/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notları `v2.0.013250001-amd64-preview` :

* Kapsayıcı için bellek kullanımını daha da azaltın.
* Çoklu pods kurulumu için dış önbellek gereklidir. Örneğin, önbelleği önbelleğe alma için ayarlama Reddır.
* Redsıs önbelleği ayarlandığında ve `ResultExpirationPeriod` 0 olarak ayarlandığında, eksik sonuçlar düzeltildi.
* 26MB için istek gövdesi boyut sınırlamasını kaldır. Kapsayıcı artık >26MB dosyalarını kabul edebilir.
* Konsol günlüğüne zaman damgası ve derleme sürümü ekleyin.

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `2.0.013250001-amd64-preview` |  |

# <a name="previous-versions"></a>[Önceki sürümler](#tab/previous)

İçin sürüm notları `1.1.013050001-amd64-preview`

* `ReadEngineConfig:ResultExpirationPeriod`Sistemin, tanınma sonuçlarını temizlemesi gerektiğinde belirtmek için kapsayıcı başlatma yapılandırması eklendi. 
    * Ayar saat cinsinden ve varsayılan değer 48 saattir.
    * Bu ayar, özellikle kapsayıcı bellek içi depolama kullanılırken sonuç depolamaya yönelik bellek kullanımını azaltabilir.
    * Örnek 1. ReadEngineConfig: ResultExpirationPeriod = 1, sistem, işlemden sonraki tanınma sonucunu 1hr olarak temizler.
    * Bu yapılandırma 0 olarak ayarlandıysa, sonuç alındıktan sonra sistem tanınma sonucunu temizler.

* Sisteme geçersiz bir görüntü biçimi geçirildiğinde 500 iç sunucu hatası düzeltildi. Şimdi 400 hatası döndürür:

    ```json
    {
        "error": {
        "code": "InvalidImageSize",
        "message": "Image must be between 1024 and 209715200 bytes."
        }
    }
    ```

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `1.1.013050001-amd64-preview` |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

---


## <a name="form-recognizer"></a>Form Tanıma

[Form tanıyıcı][fr-containers] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/custom-form` ve olarak adlandırılır `labeltool` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/custom-form/labeltool/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-version"></a>[En son sürüm](#tab/current)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |


# <a name="previous-versions"></a>[Önceki sürümler](#tab/previous)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

---

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

[Lusıs][lu-containers] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/language` ve olarak adlandırılır `luis` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/language/luis` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/language/luis/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-version"></a>[En son sürüm](#tab/current)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012280003-amd64-preview` |       |


# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `1.1.012130003-amd64-preview` |       |

---

## <a name="custom-speech-to-text"></a>Özel Konuşma Tanıma metin

[Özel konuşma tanıma-metin][sp-cstt] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel düzeyinde bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `custom-speech-to-text` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text` . Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-speech-to-text/tags/list)tam listesini de bulabilirsiniz.


# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notunun `2.7.0-amd64` :

**Özellikler**
* Noktalama işaretleri varsayılan olarak etkindir olarak ayarlanır.

Dahil edilen tümcecik listeleri nedeniyle bu kapsayıcı görüntüsünün boyutunun arttığı unutulmamalıdır.

| Resim etiketleri                    | Notlar | Bilgisi                                                                  |
|-------------------------------|:------|:------------------------------------------------------------------------|
| `latest`                      |       | `sha256:d1573c2543cb7afedb0122da0995f345767b02f9c5f181950acf1509ca65726` |
| `2.7.0-amd64`                 |       | `sha256:d1573c2543cb7afedb0122da0995f345767b02f9c5f181950acf1509ca65726` |


# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)
İçin sürüm notunun `2.6.0-amd64` :

**Özellikler**
* PhraseList v2 desteği 
* Tümcecik listeleri aşağıdaki yerel ayarlarda desteklenir:
    * En-au
    * en-CA
    * en-GB
    * En-ın
    * tr-tr
    * zh-cn
* Özel temel model indirme desteği. 
    * `BaseModelLocale=<locale>` `docker run` Komutuyla kullanın
* .NET 3,1 'ye tam olarak geçirildi

**Düzeltilen**
* Güvenilirlik puanının her zaman bir ayırma modunda 1 olduğu bir sorun düzeltildi
* TextAnalytics 3,0 API 'sine geçirildi

Dahil edilen tümcecik listeleri nedeniyle bu kapsayıcı görüntüsünün boyutunun arttığı unutulmamalıdır.

İçin sürüm notunun `2.5.0-amd64` :

**Özellikler**
* Özel modellerdeki özel söylenişi destekleme
* Azure ve Azure ABD kamu bulutunu destekleme

**Düzeltilen**
* Farklı Çalıştır modunda kök olmayan kullanıcı sorunu giderme

| Resim etiketleri                    | Notlar               |
|-------------------------------|:--------------------|
| `2.6.0-amd64`                 |                     |
| `2.5.0-amd64`                 |   1. GA sürümü    |

---

## <a name="custom-text-to-speech"></a>Özel metin okuma

[Özel metin okuma][sp-ctts] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `custom-text-to-speech` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech` . Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-text-to-speech/tags/list)tam listesini de bulabilirsiniz.


# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notunun `1.9.0-amd64` :

Normal aylık yayın

| Resim etiketleri                    | Notlar | Bilgisi                                                                  |
|-------------------------------|:------|:------------------------------------------------------------------------|
| `latest`                      |       | `sha256:e0397cf12d1367b13dd258f782bb513c93afcd5ee4b897794fe533205336355` |
| `1.9.0-amd64`                 |       | `sha256:e0397cf12d1367b13dd258f782bb513c93afcd5ee4b897794fe533205336355` |


# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)
İçin sürüm notunun `1.8.0-amd64` :

**Özellikler**
* .NET 3,1 'ye tam olarak geçirildi

İçin sürüm notunun `1.7.0-amd64` :

**Özellik**
* Kısmen .NET 3,1 'e geçirildi

| Resim etiketleri                    | Notlar               |
|-------------------------------|:--------------------|
| `1.8.0-amd64`                 |                     |
| `1.7.0-amd64`                 |   1. GA sürümü    |

---

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

[Konuşmadan metne][sp-stt] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel düzeyinde bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `speech-to-text` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text` . [Mr 'de etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/speech-to-text/tags/list)tam listesini bulabilirsiniz.

Konuşmadan metne v 2.5.0 bu yana, *ABD devlet Virginia* bölgesinde görüntüler desteklenir. Lütfen bu bölgeyi kullanırken *ABD devlet Virginia* faturalandırma uç noktasını ve API anahtarlarını kullanın.

# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notunun `2.7.0-amd64-<locale>` :

**Özellikler**
* Aşağıdaki yeni yerel ayarlar için destek:
    * AR-BH, AR-IQ, AR-Jo, AR-lb, AR-OM, AR-Sy
    * bg-bg
    * el-gr
    * En-HK, en-ie, en-pH, en-SG, en-za
    * es-ar, es-Bo, es-CL, es-Co, es-k, es-CL, es-i, es-u, es-cu, es-do, es-EC, es-gt, es-pa, es-PE, es-PR
    * et-ee
    * ga-IE
    * hr-hr
    * hu-hu
    * lt-lt
    * lv-lv
    * MT-MT
    * ro-ro
    * sk-sk
    * SL-SL
* Noktalama işaretleri varsayılan olarak etkindir.

Dahil edilen tümcecik listeleri nedeniyle bu kapsayıcı görüntüsünün boyutunun arttığı unutulmamalıdır. 

| Resim etiketleri                    | Notlar                                                                                                |
|-------------------------------|:-----------------------------------------------------------------------------------------------------|
| `latest`                      | Yerel ayara sahip kapsayıcı görüntüsü `en-US` .                                                             |
| `2.7.0-amd64-<locale>`        | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `2.7.0-amd64-en-us`. |

Bu kapsayıcıda aşağıdaki yerel ayarlar var.

| V 2.7.0 için yerel ayar           | Notlar                                    | Bilgisi                                                                  |
|-----------------------------|:-----------------------------------------|:------------------------------------------------------------------------|
| `ar-ae`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-bh`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-BH` . | `sha256:5a2c140661f50d0c95587121ec1ab8895289f4dda5b3ad14074413e869e6bd4` |
| `ar-eg`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . | `sha256:783bb8321fcfb7890b0c99935099f7e84c85a698c2fe0031c661e265358d79c` |
| `ar-iq`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-IQ` . | `sha256:abd0101f73c1cf71f30da7b11b93d2a7ac8877dbfcfc2d34553d20705aca7a2` |
| `ar-jo`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-JO` . | `sha256:d4c7fd2a1637e163aa106c23b6a759e8c78366c60ece83b3aabfe93ebabae07` |
| `ar-kw`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-lb`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-LB` . | `sha256:20e5c9105e86625c72de54290a6eb07630d35c3760f729c4b855e3661583dfe` |
| `ar-om`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-OM` . | `sha256:97f1b44f2cbb837a2ef86441a0a52a07f706240edb6ef6618ee4db8cbbe1c19` |
| `ar-qa`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sa`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sy`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-SY` . | `sha256:51980a2e2c3dd3548deedcedaf5fc688db602a5eced1a4b7df7d10750393623` |
| `bg-bg`                     | Yerel ayara sahip kapsayıcı görüntüsü `bg-BG` . | `sha256:1c1acf0fbb353ebb04692f37eb4d4cdf0b4e309720dd7e709001dada0d1ea81` |
| `ca-es`                     | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . | `sha256:c60baa0007f61c7652b97b49645215de63411125d627c974c09222e316df204` |
| `cs-cz`                     | Yerel ayara sahip kapsayıcı görüntüsü `cs-CZ` . | `sha256:3fa09fc3a6bde6b77df2444aae8fc78b5f25fb9010171d1682db116ea5801f5` |
| `da-dk`                     | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . | `sha256:4b26dbba50c2771943880b68e0e4ea0713d0e3bb8bad884454849bccc9e94a3` |
| `de-de`                     | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . | `sha256:5109ed80b1fecf4db0328adcd50528d0aa9e726b5fc84587c40aaea4e91256d` |
| `el-gr`                     | Yerel ayara sahip kapsayıcı görüntüsü `el-GR` . | `sha256:fc8b466c588bf097efac2b79454d5ac0df5c6990398f07ede9be7e1d536e4bd` |
| `en-au`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . | `sha256:3461892a27fc3eb3f9610b2def00bc15f380c6b9797c90ceca19e6abb55f6a6` |
| `en-ca`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . | `sha256:a0509be39785f1e869bd96ab10e7c07d3f4e61c9aa17ff5900076e7bd64ba11` |
| `en-gb`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . | `sha256:1b976fc7ac109e61dcf74af3652c12535e3db92931d2d0bb2ea59bd46f9efed` |
| `en-hk`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-HK` . | `sha256:0b1e1df101f978869c98f6e50632712016b8311fc89b334e7f44e968d64bf2f` |
| `en-ie`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-IE` . | `sha256:c5ba0d3c7219ce39f0b918a51a7cae8a65c277f564279cad920e068725aa39f` |
| `en-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . | `sha256:e907f07be498f024103f6fe6abffa23e242bf3585724741b29a2f3f41d0899c` |
| `en-nz`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . | `sha256:66845f6ce20ae71d609867c6eb4772366ce042499e4bcdce4c1b579daf7fad7` |
| `en-ph`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-PH` . | `sha256:e7874653bf66b1a1ab344b3391eb8767be34260b7f11b62fd057cbe17b805b2` |
| `en-sg`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-SG` . | `sha256:827cdb158280e6f4037f4815410c7aa78abf9c6467876c1504aecfef787bdd7` |
| `en-us`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . | `sha256:248d17340055e3e137219ddc234c605e6a53ceead136ea55c9697c352da6a8d` |
| `en-za`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-ZA` . | `sha256:a8abc99f498db7088bb25acec47da81e90b6a5eaa1c6f78e0f9a314d839d0ae` |
| `es-ar`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-AR` . | `sha256:edf78429630851b6eb01f54f8a8a1aeeda9971c6a834403a204662eda22b3b9` |
| `es-bo`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-BO` . | `sha256:5832b44f1da2f6b9a097c99babfbc370d8d0eabe1ff8daabec2c3f482dc9d63` |
| `es-cl`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-CL` . | `sha256:409a712b96235e154472134f96ff9272265f1e5b555e00ad03c2260b0781009` |
| `es-co`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-CO` . | `sha256:99792bc083dc16e0edf15491e6a840d786c9140b747551563a8d98f66f0b415` |
| `es-cr`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-CR` . | `sha256:21fe14a538e5b8b2d288b00b8f5a02d87469e285f32e725155042079f336ac9` |
| `es-cu`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-CU` . | `sha256:05d40eae01cec4c42c4febd379cd61373eb43d0aacfd47b988bb95e6a6ad216` |
| `es-do`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-DO` . | `sha256:73dd0e0d4f39a259563ee7cc18c2e72c9ab20c52905fe343e0413ca7c4b3f0d` |
| `es-ec`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-EC` . | `sha256:c3e69139ef365fe9332b5b68b43458242c7dad9d9f2b557431272306e81cb9e` |
| `es-es`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . | `sha256:bd83fcfc116ba645a0e12a7a93b6ada74a8f701172f826a91c5f223a1dbaa61` |
| `es-gt`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-GT` . | `sha256:5bb9b18b91b74e123e3720893d88bfcb0a87dac31a1f7171d23c7cb1fa09fee` |
| `es-hn`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-HN` . | `sha256:941d108a4b76eb554e8f13cf5090665a702de3ebf35b75e4350f0916dfccd72` |
| `es-mx`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . | `sha256:cebea03732781b4425500d162ae6580bbd7ce9b5f4ede988c4570fe311d8567` |
| `es-ni`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-NI` . | `sha256:8ba165f94ad840936ebd0af17a0a63aa08a6292e7ad9029f5b93eef41165eb9` |
| `es-pa`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-PA` . | `sha256:c61b7f1b6801a03c3eab0dd1aede87017a86bc7368ded2f8bad8d9e5f60d0d3` |
| `es-pe`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-PE` . | `sha256:447a3ab3f302aba24d201d9f5b2877ffcd64dfd5e9d6b88d9924847160b2de2` |
| `es-pr`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-PR` . | `sha256:a53b3295c986e91ee8cf93ebe1057b997c76ef7f99913508b859311a194fdd4` |
| `es-py`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-PY` . | `sha256:85b3f75e75e63e29521daf772ee68a59ac2428579512501aa81dc51a2315652` |
| `es-sv`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-SV` . | `sha256:db5ece7ba536e38d5de59cd37807630ab76589dcf1c97e253f98d7f44d9424e` |
| `es-us`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-US` . | `sha256:99f2743725bb71e25543484f49bcfde14584ccbbaaa912678938d69d965075a` |
| `es-uy`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-UY` . | `sha256:a3e11c16a97a1ae76408d812b2fee1e4b3ba07160bbcb62a22814523568ee5d` |
| `es-ve`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-VE` . | `sha256:8cb431aafd84263ead8de946377c1d3f2ddfa7e172b8a4c5aa7ba477c5b41f0` |
| `et-ee`                     | Yerel ayara sahip kapsayıcı görüntüsü `et-EE` . | `sha256:943e7cf894e9d75341a58993104824c1c8cd8da1322cc5a732e9d53882c6523` |
| `fi-fi`                     | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . | `sha256:35658e9dce796cb96a1371f250398e86351ea1b5ada080da7ce8471b30c7cae` |
| `fr-ca`                     | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . | `sha256:62256cad671e8baa03fdd4c5f4eca7d5c5effedd64cafd9020ba72c9c4210e0` |
| `fr-fr`                     | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . | `sha256:b385993232d9daa327d1a7b067268927b17f36eed3e8d423748794544c62746` |
| `ga-ie`                     | Yerel ayara sahip kapsayıcı görüntüsü `ga-IE` . | `sha256:ab9abdb993b0f7487edda8200f1393ac44ba4888c0f444a02afb6c85ca3e393` |
| `gu-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . | `sha256:328e69488f2948722d7ccc97e266071f61a8c9f65cd671688490955806526de` |
| `hi-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . | `sha256:b9b0bfec80aa53d06ea2cbd9097f753ec5caaf00ac2f00321ae7ad916fd7fa6` |
| `hr-hr`                     | Yerel ayara sahip kapsayıcı görüntüsü `hr-HR` . | `sha256:ab849cd2eeea682f8958bba8986fe90f0f7bb3b447512a10cf464e8e1ce4ea5` |
| `hu-hu`                     | Yerel ayara sahip kapsayıcı görüntüsü `hu-HU` . | `sha256:30f239b155d91523442cf74a1f2732304fa2b50ae7b786833bb6a020b982621` |
| `it-it`                     | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . | `sha256:288f95413870eb9d33bf1dabfa6fbd6b55b0faa52e4d5face3171d1dd4ddbdd` |
| `ja-jp`                     | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . | `sha256:e3ab37a80c215dec565eca212f57eb81887fc2894452868dff92e3bd42c4bb9` |
| `ko-kr`                     | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . | `sha256:c1208b8459333b606af516cd7806e9d4d5e002247bb1225e1f246563b356890` |
| `lt-lt`                     | Yerel ayara sahip kapsayıcı görüntüsü `lt-LT` . | `sha256:8dec331161d3c29fc65ba6651fcc6cfe69fa314519f408b5f9f8eb27da09830` |
| `lv-lv`                     | Yerel ayara sahip kapsayıcı görüntüsü `lv-LV` . | `sha256:7cf31282910b339666bb2b0a555caa7fc6ae414eea4423a41f35c3527f83235` |
| `mr-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . | `sha256:9cb012bd58ef7723d4905d6fa3c1fde96e33c354b3d96d4e3ff69cf6e1bfe3a` |
| `mt-mt`                     | Yerel ayara sahip kapsayıcı görüntüsü `mt-MT` . | `sha256:a0094c032ea555b168ec5751ab3257337d902d526e9ae335671fb751a352378` |
| `nb-no`                     | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . | `sha256:6bbc326e20a6a785b1ca33143b42a060858efb67b863a267d6efb7aebb48f87` |
| `nl-nl`                     | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . | `sha256:94b4ddf4cc80fa666e422f8416aea3f98ebe4842dfe9b1f4bfea7c47eb61127` |
| `pl-pl`                     | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . | `sha256:58e5f78bf772c3c8cbd5f0c5d6e67f5348e04e3f893d84738a2a3e964bab256` |
| `pt-br`                     | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . | `sha256:f500ef956bd28807f40df1f9f0520e437c5084f61a3be6d1379e746887d5b7c` |
| `pt-pt`                     | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . | `sha256:c841d2dbe5f40adf6039242c106985febb1a44212feb55d9769fe31134ec116` |
| `ro-ro`                     | Yerel ayara sahip kapsayıcı görüntüsü `ro-RO` . | `sha256:93271c39c0a134e987a069c2a65289acff9869ae0d90fdcb39928c9ef0fd86b` |
| `ru-ru`                     | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . | `sha256:8d6b3c600e56cc96813b8c14b7916c5539a20ba561dc1c6d5bbef6285d6eef6` |
| `sk-sk`                     | Yerel ayara sahip kapsayıcı görüntüsü `sk-SK` . | `sha256:6d604092cc6c964663a1c97d91c8f1c8cf4b46d07427d03f7041c0cc55eb521` |
| `sl-si`                     | Yerel ayara sahip kapsayıcı görüntüsü `sl-SI` . | `sha256:f237ed58fedefcc749e74be1258cc70e5a690ee6c5a6b6388bd24075faa61da` |
| `sv-se`                     | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . | `sha256:da4233e6658b00eefdadb9d4acd889c6550a5e2a4a7af7a9f915c878abd4c9c` |
| `ta-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . | `sha256:22b77606d25e9c2f52bf3cad6218782b4719f6a9dcfadc770468d266758a56c` |
| `te-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . | `sha256:7f4d11372862ca1d65fc9b868e2d775701b8e6eabd786c90c4e9ab82ba86e88` |
| `th-th`                     | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . | `sha256:69033bcd7c0f59d31bafec6c2b7a9ff343928cdd58c16105415c291d555d37b` |
| `tr-tr`                     | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . | `sha256:4b7d339846a0d371dfe25aa2e626f131003c01329c9a1da468eb3703ef176ea` |
| `zh-cn`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . | `sha256:a428459830fb766083212f71c5638a65ce30d8dd84f6c624ae22768e8a76976` |
| `zh-hk`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . | `sha256:7a2903462b67336a6ce4c8e2faac42052f0a4392d1d5eb3839758cc8d0429f1` |
| `zh-tw`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . | `sha256:30fd2b3660e047d24a46fbba14ba282f15bc0339ec93f49afd0d02ff4069146` |


# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

İçin sürüm notunun `2.6.0-amd64-<locale>` :

**Özellikler**
* En son modellere yükseltildi ve tamamen .NET 3,1 'e geçirildi
* PhraseList v2 desteği
* Tümcecik listeleri aşağıdaki yerel ayarlarda desteklenir:
    * En-au
    * en-CA
    * en-GB
    * En-ın
    * tr-tr
    * zh-cn
* Yeni yerel ayar desteği `cs-CZ` 
    * Büyük harfler ve noktalama işaretleri Şu anda desteklenmiyor.

**Düzeltilen**
* Güven puanlarının her zaman bir artırma modunda 1 olduğu bir sorunu düzeltir
* Geçirilmiş TextAnalytics 3,0 API 'sini kullanın

Dahil edilen tümcecik listeleri nedeniyle bu kapsayıcı görüntüsünün boyutunun arttığı unutulmamalıdır. 

İçin sürüm notunun `2.5.0-amd64-<locale>` :

**Özellikler**
* Azure US kamu bulutu desteği

**Düzeltilen**
* Kök olmayan kullanıcı olarak çalışırken bir sorunu düzeltme modunda düzeltir

| Resim etiketleri                  | Notlar                                    |
|-----------------------------|:-----------------------------------------|
| `2.6.0-amd64-<locale>`      | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `2.6.0-amd64-en-us`. |
| `2.5.0-amd64-<locale>`      | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `2.5.0-amd64-en-us`. |


Bu kapsayıcıda aşağıdaki yerel ayarlar var.

| V 2.6.0 için yerel ayar           | Notlar                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `ar-eg`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `ar-kw`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `ar-qa`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `ar-sa`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `ca-es`                     | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `cs-cz`                     | Yerel ayara sahip kapsayıcı görüntüsü `cs-CZ` . |
| `da-dk`                     | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `de-de`                     | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `en-au`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `en-ca`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `en-gb`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `en-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `en-nz`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `en-us`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `es-es`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `es-mx`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `fi-fi`                     | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `fr-ca`                     | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `fr-fr`                     | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `gu-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `hi-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `it-it`                     | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `ja-jp`                     | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `ko-kr`                     | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `mr-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `nb-no`                     | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `nl-nl`                     | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `pl-pl`                     | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `pt-br`                     | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `pt-pt`                     | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `ru-ru`                     | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `sv-se`                     | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `ta-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `te-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `th-th`                     | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `tr-tr`                     | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `zh-cn`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `zh-hk`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `zh-tw`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |

| V 2.5.0 için yerel ayar           | Notlar                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-AE` . |
| `ar-eg`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-EG` . |
| `ar-kw`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-KW` . |
| `ar-qa`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-QA` . |
| `ar-sa`                     | Yerel ayara sahip kapsayıcı görüntüsü `ar-SA` . |
| `ca-es`                     | Yerel ayara sahip kapsayıcı görüntüsü `ca-ES` . |
| `da-dk`                     | Yerel ayara sahip kapsayıcı görüntüsü `da-DK` . |
| `de-de`                     | Yerel ayara sahip kapsayıcı görüntüsü `de-DE` . |
| `en-au`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-AU` . |
| `en-ca`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-CA` . |
| `en-gb`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-GB` . |
| `en-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-IN` . |
| `en-nz`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-NZ` . |
| `en-us`                     | Yerel ayara sahip kapsayıcı görüntüsü `en-US` . |
| `es-es`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-ES` . |
| `es-mx`                     | Yerel ayara sahip kapsayıcı görüntüsü `es-MX` . |
| `fi-fi`                     | Yerel ayara sahip kapsayıcı görüntüsü `fi-FI` . |
| `fr-ca`                     | Yerel ayara sahip kapsayıcı görüntüsü `fr-CA` . |
| `fr-fr`                     | Yerel ayara sahip kapsayıcı görüntüsü `fr-FR` . |
| `gu-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `gu-IN` . |
| `hi-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `hi-IN` . |
| `it-it`                     | Yerel ayara sahip kapsayıcı görüntüsü `it-IT` . |
| `ja-jp`                     | Yerel ayara sahip kapsayıcı görüntüsü `ja-JP` . |
| `ko-kr`                     | Yerel ayara sahip kapsayıcı görüntüsü `ko-KR` . |
| `mr-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `mr-IN` . |
| `nb-no`                     | Yerel ayara sahip kapsayıcı görüntüsü `nb-NO` . |
| `nl-nl`                     | Yerel ayara sahip kapsayıcı görüntüsü `nl-NL` . |
| `pl-pl`                     | Yerel ayara sahip kapsayıcı görüntüsü `pl-PL` . |
| `pt-br`                     | Yerel ayara sahip kapsayıcı görüntüsü `pt-BR` . |
| `pt-pt`                     | Yerel ayara sahip kapsayıcı görüntüsü `pt-PT` . |
| `ru-ru`                     | Yerel ayara sahip kapsayıcı görüntüsü `ru-RU` . |
| `sv-se`                     | Yerel ayara sahip kapsayıcı görüntüsü `sv-SE` . |
| `ta-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `ta-IN` . |
| `te-in`                     | Yerel ayara sahip kapsayıcı görüntüsü `te-IN` . |
| `th-th`                     | Yerel ayara sahip kapsayıcı görüntüsü `th-TH` . |
| `tr-tr`                     | Yerel ayara sahip kapsayıcı görüntüsü `tr-TR` . |
| `zh-cn`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-CN` . |
| `zh-hk`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-HK` . |
| `zh-tw`                     | Yerel ayara sahip kapsayıcı görüntüsü `zh-TW` . |

---

## <a name="text-to-speech"></a>Metin okuma

[Metin okuma][sp-tts] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel düzeyinde bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `text-to-speech` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/text-to-speech/tags/list)tam listesini de bulabilirsiniz.


# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notunun `1.9.0-amd64-<locale-and-voice>` :

* Normal aylık yayın

| Resim etiketleri                                  | Notlar                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `latest`                                    | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.                                            | 
| `1.9.0-amd64-<locale-and-voice>`            | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `1.9.0-amd64-en-us-ariarus`.  |


| V 1.9.0 için yerel ayarlar                          | Notlar                                                                      | Bilgisi                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | `ar-EG`Yerel ayar ve `ar-EG-Hoda` sesle kapsayıcı görüntüsü.            | `sha256:2b19cfd2212d6517b286aa18617d2f9d1dd1520078b559cbbf9240599270d10` | 
| `ar-sa-naayf`                               | `ar-SA`Yerel ayar ve `ar-SA-Naayf` sesle kapsayıcı görüntüsü.           | `sha256:6063aae5fb15c62b234cf945220916516a06ca81354c5311dee02af4d8cb0d3` |
| `bg-bg-ivan`                                | `bg-BG`Yerel ayar ve `bg-BG-Ivan` sesle kapsayıcı görüntüsü.            | `sha256:c6786916464755e64ffa64e69e8f3e7ef16115bac00bb6ea1e45368c42c58d1` |
| `ca-es-herenarus`                           | `ca-ES`Yerel ayar ve `ca-ES-HerenaRUS` sesle kapsayıcı görüntüsü.       | `sha256:2a8a1accbf99e2746c9345b77e2f261e0111227312c402cc2e1cd8760cdc82a` |
| `cs-cz-jakub`                               | `cs-CZ`Yerel ayar ve `cs-CZ-Jakub` sesle kapsayıcı görüntüsü.           | `sha256:3e464356bb08c9c966af2b28a88ccafd591aecd2e37a0fedb356bd443720e8d` |
| `da-dk-hellerus`                            | `da-DK`Yerel ayar ve `da-DK-HelleRUS` sesle kapsayıcı görüntüsü.        | `sha256:b85c43080804103673ff99dddea644a516c4103e8b1f11fa3dd34857492cd40` |
| `de-at-michael`                             | `de-AT`Yerel ayar ve `de-AT-Michael` sesle kapsayıcı görüntüsü.         | `sha256:87b57ee61f964e4d72e75d860c499fa3b3d8dbda6a96c97d696beb20aa8b2a9` |
| `de-ch-karsten`                             | `de-CH`Yerel ayar ve `de-CH-Karsten` sesle kapsayıcı görüntüsü.         | `sha256:ab1385b9746f4f054204302b9d564a433ae03748021b8ed71b4a3a224af1e9b` |
| `de-de-heddarus`                            | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-hedda`                               | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-stefan-apollo`                       | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   | `sha256:56a1c63e7e6a0f5623ddc1f6a44ac6e51471d073e02e14e8c8b1e577930d816` |
| `el-gr-stefanos`                            | `el-GR`Yerel ayar ve `el-GR-Stefanos` sesle kapsayıcı görüntüsü.        | `sha256:ccbbb09f29ff8f276e246037183c7a3e9a3eb5bf33a942b22205cce3c6857f2` |
| `en-au-catherine`                           | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       | `sha256:0c7374890f963e1ae9507e89dc9965a94723bd57802826c0677cd5262189783` |
| `en-au-hayleyrus`                           | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       | `sha256:7430bf8eace8294ca085f36ea56399261b2b4f69027e86649e8f3868fc3d811` |
| `en-ca-heatherrus`                          | `en-CA`Yerel ayar ve `en-CA-HeatherRUS` sesle kapsayıcı görüntüsü.      | `sha256:0166ce1de3d669ea4ad80738c63369b7032125a54ecabade07241d740a94cfe` |
| `en-ca-linda`                               | `en-CA`Yerel ayar ve `en-CA-Linda` sesle kapsayıcı görüntüsü.           | `sha256:50bed6a7bde9b793d307bcc3ace4c0f28d4a33c7a4dad9b3a394dc39a3e1c28` |
| `en-gb-george-apollo`                       | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   | `sha256:50b800c0018a39609ddb1cee1b10062bf38a907644c393d20786db7c3ade748` |
| `en-gb-hazelrus`                            | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        | `sha256:2aa79394dfeac8cec0cc1704a5199949cfccf347fe61161d02c7000c4ffcfa6` |
| `en-gb-susan-apollo`                        | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    | `sha256:7a3174b3aae5f10241e731d392b56f124808cdd506f881ced919ced73d836c0` |
| `en-ie-sean`                                | `en-IE`Yerel ayar ve `en-IE-Sean` sesle kapsayıcı görüntüsü.            | `sha256:2457202fadb2354fc8d3666432096bd87c07760a4e3f4dbcc49853fff658577` |
| `en-in-heera-apollo`                        | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    | `sha256:e4068cd7ca4272ea94819e2ba8743d2a76c8710b162db5e9ecbde6c92c12877` |
| `en-in-priyarus`                            | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        | `sha256:9d63a0ed53ac06178ab84588551421c0e1d04b8bad3321410ebb99c3ca2a9e8` |
| `en-in-ravi-apollo`                         | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     | `sha256:67049c9ce591336655943f5030afcfdaa150a8aace7b372425a69cc33a6b7b9` |
| `en-us-aria24krus`                          | `en-US`Yerel ayar ve `en-US-Aria24kRUS` sesle kapsayıcı görüntüsü.      | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-ariarus`                             | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.         | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-benjaminrus`                         | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     | `sha256:93cd49adaaa2a1bdfb06ab655be164ae66f206cb7c03a2cbd59e5fba70610ab` |
| `en-us-guy24krus`                           | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       | `sha256:7b788bfcaae4c63c274ca15924bfd861cfcafd5fec13f685d80babc25b2949d` |
| `en-us-zirarus`                             | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         | `sha256:bfc87a77df5695ad43481348500fba8f6a7b495708fba200706049469b5ba97` |
| `es-es-helenarus`                           | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       | `sha256:0b6c17aca75efb64aa9bfc0d83303038fe58d4b2fb1fc94c9380a4335b80796` |
| `es-es-laura-apollo`                        | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    | `sha256:d6fcffc944c37a2dd0de29c39b82f3f8cce3a95ad925d2814ed7538335d5d4f` |
| `es-es-pablo-apollo`                        | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    | `sha256:a460bc53d9083d3c3770129995cf96cc1069ae4e8101f1739d304fe210f0af0` |
| `es-mx-hildarus`                            | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        | `sha256:5b7578fc5b00158dfa674d95a3f1d57f22eb285e8333b4006d1fe1808bda7ba` |
| `es-mx-raul-apollo`                         | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     | `sha256:03922fb017783c86d788c72e01c7ede440f8f3c913c86cab19bad4dfc2e4a2b` |
| `fi-fi-heidirus`                            | `fi-FI`Yerel ayar ve `fi-FI-HeidiRUS` sesle kapsayıcı görüntüsü.        | `sha256:146c1f98d6fa061016eba41db6e7b654eef222d37f35406d4b43477bb2ff897` |
| `fr-ca-caroline`                            | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        | `sha256:1ee2e53f12ad1c72665d2aef64e9d4a7f9ea05670cad84dcae5e75409494f32` |
| `fr-ca-harmonierus`                         | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     | `sha256:a21d25d3ac699af4e9ba9194aadd9b45f35fd9205224f3429a4c7da41fc38fe` |
| `fr-ch-guillaume`                           | `fr-CH`Yerel ayar ve `fr-CH-Guillaume` sesle kapsayıcı görüntüsü.       | `sha256:216125a9bd89a95d3c4dc2d7e031398659427b3aa7d4663d23a65737972e42b` |
| `fr-fr-hortenserus`                         | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     | `sha256:795a698120eecbd80c48e738f73300739c1698ca859130ddb4236317bcdf70f` |
| `fr-fr-julie-apollo`                        | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    | `sha256:f6eb70d523c435c2e3a713b32a8af4a781df7ec043caad2fc7f458ee341eb2f` |
| `fr-fr-paul-apollo`                         | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     | `sha256:28864c662a20f459b3051b1da2967a605e06267e6408285f7c2552748cf4eed` |
| `he-il-asaf`                                | `he-IL`Yerel ayar ve `he-IL-Asaf` sesle kapsayıcı görüntüsü.            | `sha256:eaa834bac6b69abef096b36a8baead741db78fe438af3d30f60abde3631d639` |
| `hi-in-hemant`                              | `hi-IN`Yerel ayar ve `hi-IN-Hemant` sesle kapsayıcı görüntüsü.          | `sha256:cfea0fa7cce9cc512f2fbb8b76f1c00fe5c32fad853c90b15934cf4ee6262fa` |
| `hi-in-kalpana-apollo`                      | `hi-IN`Yerel ayar ve `hi-IN-Kalpana-Apollo` sesle kapsayıcı görüntüsü.  | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hi-in-kalpana`                             | `hi-IN`Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hr-hr-matej`                               | `hr-HR`Yerel ayar ve `hr-HR-Matej` sesle kapsayıcı görüntüsü.           | `sha256:86683597c62752b4d769b69e5294979fafd4c277aaef1536e1cb19f9f06c0bf` |
| `hu-hu-szabolcs`                            | `hu-HU`Yerel ayar ve `hu-HU-Szabolcs` sesle kapsayıcı görüntüsü.        | `sha256:aa64eed28ca2ad060e2e02188e0401bf34e4caf7e2182b70a30ce33b3c11c9c` |
| `id-id-andika`                              | `id-ID`Yerel ayar ve `id-ID-Andika` sesle kapsayıcı görüntüsü.          | `sha256:0e1394d231a57a1df8163ccb634dc2ef2f8103b10608a40ab3efc5c0fbe9ded` |
| `it-it-cosimo-apollo`                       | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   | `sha256:eef97f2817fc24405823a5fe4e825244db32279b44c0e6631e8ad9a5c1acf40` |
| `it-it-luciarus`                            | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        | `sha256:ebc331b0685f482d2f55619fa81fd451fd7c8f107f9cd7ad159bc6213ae4e33` |
| `ja-jp-ayumi-apollo`                        | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    | `sha256:e9cb7dfd2eec154c8f3d530c16b66e8558c5955a2edaede69740067f00e43cf` |
| `ja-jp-harukarus`                           | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       | `sha256:93ce2ef6177c0d8ac70b61df8b11fcbcdfd3c0be0cc51cd8644f26679a741c2` |
| `ja-jp-ichiro-apollo`                       | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   | `sha256:6a18bae69ac63b42ba992b8b74d8d31d91ca984d61b5f62f38be988cf38645e` |
| `ko-kr-heamirus`                            | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        | `sha256:7a48252d4ada2af43f9266a70113426d330bac192348cbdc929022295a0e727` |
| `ms-my-rizwan`                              | `ms-MY`Yerel ayar ve `ms-MY-Rizwan` sesle kapsayıcı görüntüsü.          | `sha256:90e2ecac14f8e960934fd013d208fc2a0afe1bfff037d5648d422bda8d8a76e` |
| `nb-no-huldarus`                            | `nb-NO`Yerel ayar ve `nb-NO-HuldaRUS` sesle kapsayıcı görüntüsü.        | `sha256:217b61bd6244b5effda8f12a2c563ce1b4572e9c5b8a08df143665f9ff754e4` |
| `nl-nl-hannarus`                            | `nl-NL`Yerel ayar ve `nl-NL-HannaRUS` sesle kapsayıcı görüntüsü.        | `sha256:fbff48dfc9dfadadf377867b28f6e3a3bd605e59da20f77a531efcc7d85d16e` |
| `pl-pl-paulinarus`                          | `pl-PL`Yerel ayar ve `pl-PL-PaulinaRUS` sesle kapsayıcı görüntüsü.      | `sha256:856a033a09925773fa4b4531e199ab7c03c537f366acecbda60f8d21735725e` |
| `pt-br-daniel-apollo`                       | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   | `sha256:2d1ec975f1aee56a6fc6039d154fb3f2fbeb4636f7078c5dfe99aeddb6a3634` |
| `pt-br-heloisarus`                          | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      | `sha256:b7d629f37ab3305274764264dc08fab5236e60ef18d40e987618115db67ce44` |
| `pt-pt-heliarus`                            | `pt-PT`Yerel ayar ve `pt-PT-HeliaRUS` sesle kapsayıcı görüntüsü.        | `sha256:8b380ae7e4aac9d4ada4d15fa9e667387bc9ca038796d9b6999953bfbc97259` |
| `ro-ro-andrei`                              | `ro-RO`Yerel ayar ve `ro-RO-Andrei` sesle kapsayıcı görüntüsü.          | `sha256:b00ca7f1411169a5baf7263a8d7e5eed1a72084d9489eaf458429dfc338564a` |
| `ru-ru-ekaterinarus`                        | `ru-RU`Yerel ayar ve `ru-RU-EkaterinaRUS` sesle kapsayıcı görüntüsü.    | `sha256:31c588c31e3ac67305af66091e7756dfc4ca454317d0228116ea0b2fedf5d71` |
| `ru-ru-irina-apollo`                        | `ru-RU`Yerel ayar ve `ru-RU-Irina-Apollo` sesle kapsayıcı görüntüsü.    | `sha256:e76437f8da7c279b38d2643defc997a13b4a364e9a212895cdb33a9a3f6457f` |
| `ru-ru-pavel-apollo`                        | `ru-RU`Yerel ayar ve `ru-RU-Pavel-Apollo` sesle kapsayıcı görüntüsü.    | `sha256:461c1efa6cce0b10a87f338bc637aca76aef8458061a688870fb3343d682da0` |
| `sk-sk-filip`                               | `sk-SK`Yerel ayar ve `sk-SK-Filip` sesle kapsayıcı görüntüsü.           | `sha256:7fb0cfab4c0fe2913eb20f28a25c6663015d62f82e7e7864d9f7fac2d27697b` |
| `sl-si-lado`                                | `sl-SI`Yerel ayar ve `sl-SI-Lado` sesle kapsayıcı görüntüsü.            | `sha256:5336173d410e10ffeb5dc211a583887e33754319c757914955057d398dfbb0a` |
| `sv-se-hedvigrus`                           | `sv-SE`Yerel ayar ve `sv-SE-HedvigRUS` sesle kapsayıcı görüntüsü.       | `sha256:5dc8cdcc3054386bf69596707d9d261d4db5bfd09f1882ceb4e29238a34b24e` |
| `ta-in-valluvar`                            | `ta-IN`Yerel ayar ve `ta-IN-Valluvar` sesle kapsayıcı görüntüsü.        | `sha256:74ea485f23e4c1fe0029e06894860aa0188c36c0e14ea3584a06d4216ccef56` |
| `te-in-chitra`                              | `te-IN`Yerel ayar ve `te-IN-Chitra` sesle kapsayıcı görüntüsü.          | `sha256:ff2977a98ef691da543db08be9cfe04d7fc3bf8f78b29310c163e47303b2ddd` |
| `th-th-pattara`                             | `th-TH`Yerel ayar ve `th-TH-Pattara` sesle kapsayıcı görüntüsü.         | `sha256:ba7e2c0e5e75d9f2b52aa50c97728616c43e81f48c15e24665e4c2ea5770a8f` |
| `tr-tr-sedarus`                             | `tr-TR`Yerel ayar ve `tr-TR-SedaRUS` sesle kapsayıcı görüntüsü.         | `sha256:375a8ceae89ea1f0dda551feff30ae3679231189b527992edbc49988d042d66` |
| `vi-vn-an`                                  | `vi-VN`Yerel ayar ve `vi-VN-An` sesle kapsayıcı görüntüsü.              | `sha256:b6f82148295b38b4039c45c48695ec50b4e97cd02b18d49c39bf9fca3bec958` |
| `zh-cn-huihuirus`                           | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       | `sha256:3e773931f3adaac92cba43773a241692a2b471ebe73ec51c475df8ff63b7ee1` |
| `zh-cn-kangkang-apollo`                     | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. | `sha256:05fc0d5075a1094caf70d98b4a9469952be52cb6eb4d9f7b9ff4ae961100c7b` |
| `zh-cn-yaoyao-apollo`                       | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   | `sha256:d7613bcefc48e85b9d6f07c8cd223c16d4958bcf7f24087575250e97c593ac1` |
| `zh-hk-danny-apollo`                        | `zh-HK`Yerel ayar ve `zh-HK-Danny-Apollo` sesle kapsayıcı görüntüsü.    | `sha256:efe22bc123dac9312dcaeb859a377d81f61fbb25ef46e4678d36ec6bebc5d32` |
| `zh-hk-tracy-apollo`                        | `zh-HK`Yerel ayar ve `zh-HK-Tracy-Apollo` sesle kapsayıcı görüntüsü.    | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-hk-tracyrus`                            | `zh-HK`Yerel ayar ve `zh-HK-TracyRUS` sesle kapsayıcı görüntüsü.        | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-tw-hanhanrus`                           | `zh-TW`Yerel ayar ve `zh-TW-HanHanRUS` sesle kapsayıcı görüntüsü.       | `sha256:95d58922463d577d4c4722ab722a5768af35fb62236d47f6709717dea758909` |
| `zh-tw-yating-apollo`                       | `zh-TW`Yerel ayar ve `zh-TW-Yating-Apollo` sesle kapsayıcı görüntüsü.   | `sha256:33eec6e3aaaedafaf3969746eeaf97a1760e763505decfe2abaa03f5054bfd2` |
| `zh-tw-zhiwei-apollo`                       | `zh-TW`Yerel ayar ve `zh-TW-Zhiwei-Apollo` sesle kapsayıcı görüntüsü.   | `sha256:456db2898b2e5a9c30b7071ce6ea3f141438cbf1aa4899c7ffccfc2f0dde5bd` |


# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

İçin sürüm notunun `1.8.0-amd64-<locale-and-voice>` :

**Özellik**

* .NET 3,1 'ye tam olarak geçirildi

İçin sürüm notunun `1.7.0-amd64-<locale-and-voice>` :

**Özellik**

* Bileşenler .NET 3,1 'ye yükseltildi

| Resim etiketleri                                  | Notlar                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `1.8.0-amd64-<locale-and-voice>`            | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `1.8.0-amd64-en-us-ariarus`.  |
| `1.7.0-amd64-<locale-and-voice>`            | 1. GA sürümü. `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `1.7.0-amd64-en-us-ariarus`.  |


| V 1.8.0 için yerel ayarlar                          | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | `ar-EG`Yerel ayar ve `ar-EG-Hoda` sesle kapsayıcı görüntüsü.            |
| `ar-sa-naayf`                               | `ar-SA`Yerel ayar ve `ar-SA-Naayf` sesle kapsayıcı görüntüsü.           |
| `bg-bg-ivan`                                | `bg-BG`Yerel ayar ve `bg-BG-Ivan` sesle kapsayıcı görüntüsü.            |
| `ca-es-herenarus`                           | `ca-ES`Yerel ayar ve `ca-ES-HerenaRUS` sesle kapsayıcı görüntüsü.       |
| `cs-cz-jakub`                               | `cs-CZ`Yerel ayar ve `cs-CZ-Jakub` sesle kapsayıcı görüntüsü.           |
| `da-dk-hellerus`                            | `da-DK`Yerel ayar ve `da-DK-HelleRUS` sesle kapsayıcı görüntüsü.        |
| `de-at-michael`                             | `de-AT`Yerel ayar ve `de-AT-Michael` sesle kapsayıcı görüntüsü.         |
| `de-ch-karsten`                             | `de-CH`Yerel ayar ve `de-CH-Karsten` sesle kapsayıcı görüntüsü.         |
| `de-de-hedda`                               | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `de-de-heddarus`                            | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `de-de-stefan-apollo`                       | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `el-gr-stefanos`                            | `el-GR`Yerel ayar ve `el-GR-Stefanos` sesle kapsayıcı görüntüsü.        |
| `en-au-catherine`                           | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `en-au-hayleyrus`                           | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `en-ca-heatherrus`                          | `en-CA`Yerel ayar ve `en-CA-HeatherRUS` sesle kapsayıcı görüntüsü.      |
| `en-ca-linda`                               | `en-CA`Yerel ayar ve `en-CA-Linda` sesle kapsayıcı görüntüsü.           |
| `en-gb-george-apollo`                       | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `en-gb-hazelrus`                            | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `en-gb-susan-apollo`                        | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `en-ie-sean`                                | `en-IE`Yerel ayar ve `en-IE-Sean` sesle kapsayıcı görüntüsü.            |
| `en-in-heera-apollo`                        | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `en-in-priyarus`                            | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `en-in-ravi-apollo`                         | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `en-us-benjaminrus`                         | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `en-us-guy24krus`                           | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `en-us-aria24krus`                          | `en-US`Yerel ayar ve `en-US-Aria24kRUS` sesle kapsayıcı görüntüsü.      |
| `en-us-ariarus`                             | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.         |
| `en-us-zirarus`                             | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `es-es-helenarus`                           | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `es-es-laura-apollo`                        | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `es-es-pablo-apollo`                        | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `es-mx-hildarus`                            | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `es-mx-raul-apollo`                         | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `fi-fi-heidirus`                            | `fi-FI`Yerel ayar ve `fi-FI-HeidiRUS` sesle kapsayıcı görüntüsü.        |
| `fr-ca-caroline`                            | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `fr-ca-harmonierus`                         | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `fr-ch-guillaume`                           | `fr-CH`Yerel ayar ve `fr-CH-Guillaume` sesle kapsayıcı görüntüsü.       |
| `fr-fr-hortenserus`                         | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `fr-fr-julie-apollo`                        | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `fr-fr-paul-apollo`                         | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `he-il-asaf`                                | `he-IL`Yerel ayar ve `he-IL-Asaf` sesle kapsayıcı görüntüsü.            |
| `hi-in-hemant`                              | `hi-IN`Yerel ayar ve `hi-IN-Hemant` sesle kapsayıcı görüntüsü.          |
| `hi-in-kalpana-apollo`                      | `hi-IN`Yerel ayar ve `hi-IN-Kalpana-Apollo` sesle kapsayıcı görüntüsü.  |
| `hi-in-kalpana`                             | `hi-IN`Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `hr-hr-matej`                               | `hr-HR`Yerel ayar ve `hr-HR-Matej` sesle kapsayıcı görüntüsü.           |
| `hu-hu-szabolcs`                            | `hu-HU`Yerel ayar ve `hu-HU-Szabolcs` sesle kapsayıcı görüntüsü.        |
| `id-id-andika`                              | `id-ID`Yerel ayar ve `id-ID-Andika` sesle kapsayıcı görüntüsü.          |
| `it-it-cosimo-apollo`                       | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `it-it-luciarus`                            | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `ja-jp-ayumi-apollo`                        | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `ja-jp-harukarus`                           | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `ja-jp-ichiro-apollo`                       | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `ko-kr-heamirus`                            | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `ms-my-rizwan`                              | `ms-MY`Yerel ayar ve `ms-MY-Rizwan` sesle kapsayıcı görüntüsü.          |
| `nb-no-huldarus`                            | `nb-NO`Yerel ayar ve `nb-NO-HuldaRUS` sesle kapsayıcı görüntüsü.        |
| `nl-nl-hannarus`                            | `nl-NL`Yerel ayar ve `nl-NL-HannaRUS` sesle kapsayıcı görüntüsü.        |
| `pl-pl-paulinarus`                          | `pl-PL`Yerel ayar ve `pl-PL-PaulinaRUS` sesle kapsayıcı görüntüsü.      |
| `pt-br-daniel-apollo`                       | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `pt-br-heloisarus`                          | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `pt-pt-heliarus`                            | `pt-PT`Yerel ayar ve `pt-PT-HeliaRUS` sesle kapsayıcı görüntüsü.        |
| `ro-ro-andrei`                              | `ro-RO`Yerel ayar ve `ro-RO-Andrei` sesle kapsayıcı görüntüsü.          |
| `ru-ru-ekaterinarus`                        | `ru-RU`Yerel ayar ve `ru-RU-EkaterinaRUS` sesle kapsayıcı görüntüsü.    |
| `ru-ru-irina-apollo`                        | `ru-RU`Yerel ayar ve `ru-RU-Irina-Apollo` sesle kapsayıcı görüntüsü.    |
| `ru-ru-pavel-apollo`                        | `ru-RU`Yerel ayar ve `ru-RU-Pavel-Apollo` sesle kapsayıcı görüntüsü.    |
| `sk-sk-filip`                               | `sk-SK`Yerel ayar ve `sk-SK-Filip` sesle kapsayıcı görüntüsü.           |
| `sl-si-lado`                                | `sl-SI`Yerel ayar ve `sl-SI-Lado` sesle kapsayıcı görüntüsü.            |
| `sv-se-hedvigrus`                           | `sv-SE`Yerel ayar ve `sv-SE-HedvigRUS` sesle kapsayıcı görüntüsü.       |
| `ta-in-valluvar`                            | `ta-IN`Yerel ayar ve `ta-IN-Valluvar` sesle kapsayıcı görüntüsü.        |
| `te-in-chitra`                              | `te-IN`Yerel ayar ve `te-IN-Chitra` sesle kapsayıcı görüntüsü.          |
| `th-th-pattara`                             | `th-TH`Yerel ayar ve `th-TH-Pattara` sesle kapsayıcı görüntüsü.         |
| `tr-tr-sedarus`                             | `tr-TR`Yerel ayar ve `tr-TR-SedaRUS` sesle kapsayıcı görüntüsü.         |
| `vi-vn-an`                                  | `vi-VN`Yerel ayar ve `vi-VN-An` sesle kapsayıcı görüntüsü.              |
| `zh-cn-huihuirus`                           | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `zh-cn-kangkang-apollo`                     | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `zh-cn-yaoyao-apollo`                       | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `zh-hk-danny-apollo`                        | `zh-HK`Yerel ayar ve `zh-HK-Danny-Apollo` sesle kapsayıcı görüntüsü.    |
| `zh-hk-tracy-apollo`                        | `zh-HK`Yerel ayar ve `zh-HK-Tracy-Apollo` sesle kapsayıcı görüntüsü.    |
| `zh-hk-tracyrus`                            | `zh-HK`Yerel ayar ve `zh-HK-TracyRUS` sesle kapsayıcı görüntüsü.        |
| `zh-tw-hanhanrus`                           | `zh-TW`Yerel ayar ve `zh-TW-HanHanRUS` sesle kapsayıcı görüntüsü.       |
| `zh-tw-yating-apollo`                       | `zh-TW`Yerel ayar ve `zh-TW-Yating-Apollo` sesle kapsayıcı görüntüsü.   |
| `zh-tw-zhiwei-apollo`                       | `zh-TW`Yerel ayar ve `zh-TW-Zhiwei-Apollo` sesle kapsayıcı görüntüsü.   |

| V 1.7.0 için yerel ayarlar                          | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | `ar-EG`Yerel ayar ve `ar-EG-Hoda` sesle kapsayıcı görüntüsü.            |
| `ar-sa-naayf`                               | `ar-SA`Yerel ayar ve `ar-SA-Naayf` sesle kapsayıcı görüntüsü.           |
| `bg-bg-ivan`                                | `bg-BG`Yerel ayar ve `bg-BG-Ivan` sesle kapsayıcı görüntüsü.            |
| `ca-es-herenarus`                           | `ca-ES`Yerel ayar ve `ca-ES-HerenaRUS` sesle kapsayıcı görüntüsü.       |
| `cs-cz-jakub`                               | `cs-CZ`Yerel ayar ve `cs-CZ-Jakub` sesle kapsayıcı görüntüsü.           |
| `da-dk-hellerus`                            | `da-DK`Yerel ayar ve `da-DK-HelleRUS` sesle kapsayıcı görüntüsü.        |
| `de-at-michael`                             | `de-AT`Yerel ayar ve `de-AT-Michael` sesle kapsayıcı görüntüsü.         |
| `de-ch-karsten`                             | `de-CH`Yerel ayar ve `de-CH-Karsten` sesle kapsayıcı görüntüsü.         |
| `de-de-hedda`                               | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `de-de-heddarus`                            | `de-DE`Yerel ayar ve `de-DE-Hedda` sesle kapsayıcı görüntüsü.           |
| `de-de-stefan-apollo`                       | `de-DE`Yerel ayar ve `de-DE-Stefan-Apollo` sesle kapsayıcı görüntüsü.   |
| `el-gr-stefanos`                            | `el-GR`Yerel ayar ve `el-GR-Stefanos` sesle kapsayıcı görüntüsü.        |
| `en-au-catherine`                           | `en-AU`Yerel ayar ve `en-AU-Catherine` sesle kapsayıcı görüntüsü.       |
| `en-au-hayleyrus`                           | `en-AU`Yerel ayar ve `en-AU-HayleyRUS` sesle kapsayıcı görüntüsü.       |
| `en-ca-heatherrus`                          | `en-CA`Yerel ayar ve `en-CA-HeatherRUS` sesle kapsayıcı görüntüsü.      |
| `en-ca-linda`                               | `en-CA`Yerel ayar ve `en-CA-Linda` sesle kapsayıcı görüntüsü.           |
| `en-gb-george-apollo`                       | `en-GB`Yerel ayar ve `en-GB-George-Apollo` sesle kapsayıcı görüntüsü.   |
| `en-gb-hazelrus`                            | `en-GB`Yerel ayar ve `en-GB-HazelRUS` sesle kapsayıcı görüntüsü.        |
| `en-gb-susan-apollo`                        | `en-GB`Yerel ayar ve `en-GB-Susan-Apollo` sesle kapsayıcı görüntüsü.    |
| `en-ie-sean`                                | `en-IE`Yerel ayar ve `en-IE-Sean` sesle kapsayıcı görüntüsü.            |
| `en-in-heera-apollo`                        | `en-IN`Yerel ayar ve `en-IN-Heera-Apollo` sesle kapsayıcı görüntüsü.    |
| `en-in-priyarus`                            | `en-IN`Yerel ayar ve `en-IN-PriyaRUS` sesle kapsayıcı görüntüsü.        |
| `en-in-ravi-apollo`                         | `en-IN`Yerel ayar ve `en-IN-Ravi-Apollo` sesle kapsayıcı görüntüsü.     |
| `en-us-benjaminrus`                         | `en-US`Yerel ayar ve `en-US-BenjaminRUS` sesle kapsayıcı görüntüsü.     |
| `en-us-guy24krus`                           | `en-US`Yerel ayar ve `en-US-Guy24kRUS` sesle kapsayıcı görüntüsü.       |
| `en-us-aria24krus`                          | `en-US`Yerel ayar ve `en-US-Aria24kRUS` sesle kapsayıcı görüntüsü.      |
| `en-us-ariarus`                             | `en-US`Yerel ayar ve `en-US-AriaRUS` sesle kapsayıcı görüntüsü.         |
| `en-us-zirarus`                             | `en-US`Yerel ayar ve `en-US-ZiraRUS` sesle kapsayıcı görüntüsü.         |
| `es-es-helenarus`                           | `es-ES`Yerel ayar ve `es-ES-HelenaRUS` sesle kapsayıcı görüntüsü.       |
| `es-es-laura-apollo`                        | `es-ES`Yerel ayar ve `es-ES-Laura-Apollo` sesle kapsayıcı görüntüsü.    |
| `es-es-pablo-apollo`                        | `es-ES`Yerel ayar ve `es-ES-Pablo-Apollo` sesle kapsayıcı görüntüsü.    |
| `es-mx-hildarus`                            | `es-MX`Yerel ayar ve `es-MX-HildaRUS` sesle kapsayıcı görüntüsü.        |
| `es-mx-raul-apollo`                         | `es-MX`Yerel ayar ve `es-MX-Raul-Apollo` sesle kapsayıcı görüntüsü.     |
| `fi-fi-heidirus`                            | `fi-FI`Yerel ayar ve `fi-FI-HeidiRUS` sesle kapsayıcı görüntüsü.        |
| `fr-ca-caroline`                            | `fr-CA`Yerel ayar ve `fr-CA-Caroline` sesle kapsayıcı görüntüsü.        |
| `fr-ca-harmonierus`                         | `fr-CA`Yerel ayar ve `fr-CA-HarmonieRUS` sesle kapsayıcı görüntüsü.     |
| `fr-ch-guillaume`                           | `fr-CH`Yerel ayar ve `fr-CH-Guillaume` sesle kapsayıcı görüntüsü.       |
| `fr-fr-hortenserus`                         | `fr-FR`Yerel ayar ve `fr-FR-HortenseRUS` sesle kapsayıcı görüntüsü.     |
| `fr-fr-julie-apollo`                        | `fr-FR`Yerel ayar ve `fr-FR-Julie-Apollo` sesle kapsayıcı görüntüsü.    |
| `fr-fr-paul-apollo`                         | `fr-FR`Yerel ayar ve `fr-FR-Paul-Apollo` sesle kapsayıcı görüntüsü.     |
| `he-il-asaf`                                | `he-IL`Yerel ayar ve `he-IL-Asaf` sesle kapsayıcı görüntüsü.            |
| `hi-in-hemant`                              | `hi-IN`Yerel ayar ve `hi-IN-Hemant` sesle kapsayıcı görüntüsü.          |
| `hi-in-kalpana-apollo`                      | `hi-IN`Yerel ayar ve `hi-IN-Kalpana-Apollo` sesle kapsayıcı görüntüsü.  |
| `hi-in-kalpana`                             | `hi-IN`Yerel ayar ve `hi-IN-Kalpana` sesle kapsayıcı görüntüsü.         |
| `hr-hr-matej`                               | `hr-HR`Yerel ayar ve `hr-HR-Matej` sesle kapsayıcı görüntüsü.           |
| `hu-hu-szabolcs`                            | `hu-HU`Yerel ayar ve `hu-HU-Szabolcs` sesle kapsayıcı görüntüsü.        |
| `id-id-andika`                              | `id-ID`Yerel ayar ve `id-ID-Andika` sesle kapsayıcı görüntüsü.          |
| `it-it-cosimo-apollo`                       | `it-IT`Yerel ayar ve `it-IT-Cosimo-Apollo` sesle kapsayıcı görüntüsü.   |
| `it-it-luciarus`                            | `it-IT`Yerel ayar ve `it-IT-LuciaRUS` sesle kapsayıcı görüntüsü.        |
| `ja-jp-ayumi-apollo`                        | `ja-JP`Yerel ayar ve `ja-JP-Ayumi-Apollo` sesle kapsayıcı görüntüsü.    |
| `ja-jp-harukarus`                           | `ja-JP`Yerel ayar ve `ja-JP-HarukaRUS` sesle kapsayıcı görüntüsü.       |
| `ja-jp-ichiro-apollo`                       | `ja-JP`Yerel ayar ve `ja-JP-Ichiro-Apollo` sesle kapsayıcı görüntüsü.   |
| `ko-kr-heamirus`                            | `ko-KR`Yerel ayar ve `ko-KR-HeamiRUS` sesle kapsayıcı görüntüsü.        |
| `ms-my-rizwan`                              | `ms-MY`Yerel ayar ve `ms-MY-Rizwan` sesle kapsayıcı görüntüsü.          |
| `nb-no-huldarus`                            | `nb-NO`Yerel ayar ve `nb-NO-HuldaRUS` sesle kapsayıcı görüntüsü.        |
| `nl-nl-hannarus`                            | `nl-NL`Yerel ayar ve `nl-NL-HannaRUS` sesle kapsayıcı görüntüsü.        |
| `pl-pl-paulinarus`                          | `pl-PL`Yerel ayar ve `pl-PL-PaulinaRUS` sesle kapsayıcı görüntüsü.      |
| `pt-br-daniel-apollo`                       | `pt-BR`Yerel ayar ve `pt-BR-Daniel-Apollo` sesle kapsayıcı görüntüsü.   |
| `pt-br-heloisarus`                          | `pt-BR`Yerel ayar ve `pt-BR-HeloisaRUS` sesle kapsayıcı görüntüsü.      |
| `pt-pt-heliarus`                            | `pt-PT`Yerel ayar ve `pt-PT-HeliaRUS` sesle kapsayıcı görüntüsü.        |
| `ro-ro-andrei`                              | `ro-RO`Yerel ayar ve `ro-RO-Andrei` sesle kapsayıcı görüntüsü.          |
| `ru-ru-ekaterinarus`                        | `ru-RU`Yerel ayar ve `ru-RU-EkaterinaRUS` sesle kapsayıcı görüntüsü.    |
| `ru-ru-irina-apollo`                        | `ru-RU`Yerel ayar ve `ru-RU-Irina-Apollo` sesle kapsayıcı görüntüsü.    |
| `ru-ru-pavel-apollo`                        | `ru-RU`Yerel ayar ve `ru-RU-Pavel-Apollo` sesle kapsayıcı görüntüsü.    |
| `sk-sk-filip`                               | `sk-SK`Yerel ayar ve `sk-SK-Filip` sesle kapsayıcı görüntüsü.           |
| `sl-si-lado`                                | `sl-SI`Yerel ayar ve `sl-SI-Lado` sesle kapsayıcı görüntüsü.            |
| `sv-se-hedvigrus`                           | `sv-SE`Yerel ayar ve `sv-SE-HedvigRUS` sesle kapsayıcı görüntüsü.       |
| `ta-in-valluvar`                            | `ta-IN`Yerel ayar ve `ta-IN-Valluvar` sesle kapsayıcı görüntüsü.        |
| `te-in-chitra`                              | `te-IN`Yerel ayar ve `te-IN-Chitra` sesle kapsayıcı görüntüsü.          |
| `th-th-pattara`                             | `th-TH`Yerel ayar ve `th-TH-Pattara` sesle kapsayıcı görüntüsü.         |
| `tr-tr-sedarus`                             | `tr-TR`Yerel ayar ve `tr-TR-SedaRUS` sesle kapsayıcı görüntüsü.         |
| `vi-vn-an`                                  | `vi-VN`Yerel ayar ve `vi-VN-An` sesle kapsayıcı görüntüsü.              |
| `zh-cn-huihuirus`                           | `zh-CN`Yerel ayar ve `zh-CN-HuihuiRUS` sesle kapsayıcı görüntüsü.       |
| `zh-cn-kangkang-apollo`                     | `zh-CN`Yerel ayar ve `zh-CN-Kangkang-Apollo` sesle kapsayıcı görüntüsü. |
| `zh-cn-yaoyao-apollo`                       | `zh-CN`Yerel ayar ve `zh-CN-Yaoyao-Apollo` sesle kapsayıcı görüntüsü.   |
| `zh-hk-danny-apollo`                        | `zh-HK`Yerel ayar ve `zh-HK-Danny-Apollo` sesle kapsayıcı görüntüsü.    |
| `zh-hk-tracy-apollo`                        | `zh-HK`Yerel ayar ve `zh-HK-Tracy-Apollo` sesle kapsayıcı görüntüsü.    |
| `zh-hk-tracyrus`                            | `zh-HK`Yerel ayar ve `zh-HK-TracyRUS` sesle kapsayıcı görüntüsü.        |
| `zh-tw-hanhanrus`                           | `zh-TW`Yerel ayar ve `zh-TW-HanHanRUS` sesle kapsayıcı görüntüsü.       |
| `zh-tw-yating-apollo`                       | `zh-TW`Yerel ayar ve `zh-TW-Yating-Apollo` sesle kapsayıcı görüntüsü.   |
| `zh-tw-zhiwei-apollo`                       | `zh-TW`Yerel ayar ve `zh-TW-Zhiwei-Apollo` sesle kapsayıcı görüntüsü.   |

---

## <a name="neural-text-to-speech"></a>Sinir metin okuma

[Sinir metin okuma][sp-ntts] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel düzeyinde bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `neural-text-to-speech` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/neural-text-to-speech/tags/list)tam listesini de bulabilirsiniz.


# <a name="latest-version"></a>[En son sürüm](#tab/current)

İçin sürüm notları `v1.3.0` :
* Sinir metin okuma kapsayıcısı artık genel kullanıma sunulmuştur. 

| Resim etiketleri                                  | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US`Yerel ayar ve `en-US-AriaNeural` sesle kapsayıcı görüntüsü.      |
| `1.3.0-amd64-<locale-and-voice>`    | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `1.3.0-amd64-en-us-arianeural`. |


| v 1.3.0 yerel ayarları ve sesler           | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `de-de-katjaneural`                 | `de-DE`Yerel ayar ve `de-DE-KatjaNeural` sesle kapsayıcı görüntüsü.     |
| `en-au-natashaneural`               | `en-AU`Yerel ayar ve `en-AU-NatashaNeural` sesle kapsayıcı görüntüsü.   |
| `en-ca-claraneural`                 | `en-CA`Yerel ayar ve `en-CA-ClaraNeural` sesle kapsayıcı görüntüsü.     |
| `en-gb-libbyneural`                 | `en-GB`Yerel ayar ve `en-GB-LibbyNeural` sesle kapsayıcı görüntüsü.     |
| `en-gb-mianeural`                   | `en-GB`Yerel ayar ve `en-GB-MiaNeural` sesle kapsayıcı görüntüsü.       |
| `en-us-arianeural`                  | `en-US`Yerel ayar ve `en-US-AriaNeural` sesle kapsayıcı görüntüsü.      |
| `en-us-guyneural`                   | `en-US`Yerel ayar ve `en-US-GuyNeural` sesle kapsayıcı görüntüsü.       |
| `es-es-elviraneural`                | `es-ES`Yerel ayar ve `es-ES-ElviraNeural` sesle kapsayıcı görüntüsü.    |
| `es-mx-dalianeural`                 | `es-MX`Yerel ayar ve `es-MX-DaliaNeural` sesle kapsayıcı görüntüsü.     |
| `fr-ca-sylvieneural`                | `fr-CA`Yerel ayar ve `fr-CA-SylvieNeural` sesle kapsayıcı görüntüsü.    |
| `fr-fr-deniseneural`                | `fr-FR`Yerel ayar ve `fr-FR-DeniseNeural` sesle kapsayıcı görüntüsü.    |
| `it-it-elsaneural`                  | `it-IT`Yerel ayar ve `it-IT-ElsaNeural` sesle kapsayıcı görüntüsü.      |
| `ja-jp-nanamineural`                | `ja-JP`Yerel ayar ve `ja-JP-NanamiNeural` sesle kapsayıcı görüntüsü.    |
| `ko-kr-sunhineural`                 | `ko-KR`Yerel ayar ve `ko-KR-SunHiNeural` sesle kapsayıcı görüntüsü.     |
| `pt-br-franciscaneural`             | `pt-BR`Yerel ayar ve `pt-BR-FranciscaNeural` sesle kapsayıcı görüntüsü. |
| `zh-cn-xiaoxiaoneural`              | `zh-CN`Yerel ayar ve `zh-CN-XiaoxiaoNeural` sesle kapsayıcı görüntüsü.  |

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

| Resim etiketleri                                  | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US`Yerel ayar ve `en-US-AriaNeural` sesle kapsayıcı görüntüsü.      |
| `1.2.0-amd64-<locale-and-voice>-preview`    | `<locale>`Aşağıda listelenen kullanılabilir yerel ayarlardan biriyle değiştirin. Örneğin, `1.2.0-amd64-en-us-arianeural-preview`. |


| v 1.2.0 Preview yerel ayarları ve sesler           | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | `en-US`Yerel ayar ve `en-US-AriaNeural` sesle kapsayıcı görüntüsü.      |
| `de-de-katjaneural-preview`                 | `de-DE`Yerel ayar ve `de-DE-KatjaNeural` sesle kapsayıcı görüntüsü.     |
| `en-au-natashaneural-preview`               | `en-AU`Yerel ayar ve `en-AU-NatashaNeural` sesle kapsayıcı görüntüsü.   |
| `en-ca-claraneural-preview`                 | `en-CA`Yerel ayar ve `en-CA-ClaraNeural` sesle kapsayıcı görüntüsü.     |
| `en-gb-libbyneural-preview`                 | `en-GB`Yerel ayar ve `en-GB-LibbyNeural` sesle kapsayıcı görüntüsü.     |
| `en-gb-mianeural-preview`                   | `en-GB`Yerel ayar ve `en-GB-MiaNeural` sesle kapsayıcı görüntüsü.       |
| `en-us-arianeural-preview`                  | `en-US`Yerel ayar ve `en-US-AriaNeural` sesle kapsayıcı görüntüsü.      |
| `en-us-guyneural-preview`                   | `en-US`Yerel ayar ve `en-US-GuyNeural` sesle kapsayıcı görüntüsü.       |
| `es-es-elviraneural-preview`                | `es-ES`Yerel ayar ve `es-ES-ElviraNeural` sesle kapsayıcı görüntüsü.    |
| `es-mx-dalianeural-preview`                 | `es-MX`Yerel ayar ve `es-MX-DaliaNeural` sesle kapsayıcı görüntüsü.     |
| `fr-ca-sylvieneural-preview`                | `fr-CA`Yerel ayar ve `fr-CA-SylvieNeural` sesle kapsayıcı görüntüsü.    |
| `fr-fr-deniseneural-preview`                | `fr-FR`Yerel ayar ve `fr-FR-DeniseNeural` sesle kapsayıcı görüntüsü.    |
| `it-it-elsaneural-preview`                  | `it-IT`Yerel ayar ve `it-IT-ElsaNeural` sesle kapsayıcı görüntüsü.      |
| `ja-jp-nanamineural-preview`                | `ja-JP`Yerel ayar ve `ja-JP-NanamiNeural` sesle kapsayıcı görüntüsü.    |
| `ko-kr-sunhineural-preview`                 | `ko-KR`Yerel ayar ve `ko-KR-SunHiNeural` sesle kapsayıcı görüntüsü.     |
| `pt-br-franciscaneural-preview`             | `pt-BR`Yerel ayar ve `pt-BR-FranciscaNeural` sesle kapsayıcı görüntüsü. |
| `zh-cn-xiaoxiaoneural-preview`              | `zh-CN`Yerel ayar ve `zh-CN-XiaoxiaoNeural` sesle kapsayıcı görüntüsü.  |

---

## <a name="speech-language-detection"></a>Konuşma dili algılama

[Konuşma dili algılama][sp-lid] kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/speechservices/` ve olarak adlandırılır `language-detection` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/language-detection/tags/list)tam listesini de bulabilirsiniz.

| Resim etiketleri                                  | Notlar                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                       |      |
| `1.1.0-amd64-preview`                       |      |

## <a name="key-phrase-extraction"></a>Anahtar İfade Ayıklama

kapsayıcı görüntüsü, `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/textanalytics/` ve olarak adlandırılır `keyphrase` . Tam kapsayıcı görüntü adı, olur `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase` .

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/keyphrase/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-version"></a>[En son sürüm](#tab/current)


| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` |       |

# <a name="previous-versions"></a>[Önceki sürümler](#tab/previous)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `1.1.012840001-amd64` |       |
| `1.1.012830001-amd64`    |       |

---

## <a name="text-language-detection"></a>Metin dili algılama

[Dil algılama][ta-la] kapsayıcı görüntüsü `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/textanalytics/` ve olarak adlandırılır `language` . Tam kapsayıcı görüntü adı, `mcr.microsoft.com/azure-cognitive-services/textanalytics/language`


Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/language/tags/list)tam listesini de bulabilirsiniz.

# <a name="latest-versions"></a>[En son sürümler](#tab/current)

| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` | |
   

# <a name="previous-versions"></a>[Önceki sürümler](#tab/previous)


| Resim etiketleri                    | Notlar |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012840001-amd64` |   |
| `1.1.012830001-amd64` |   |

---

## <a name="sentiment-analysis"></a>Yaklaşım analizi

[Yaklaşım Analizi][ta-se] kapsayıcı görüntüsü `mcr.microsoft.com` kapsayıcı kayıt defteri genel konumunda bulunabilir. Depo içinde bulunur `azure-cognitive-services/textanalytics/` ve olarak adlandırılır `sentiment` . Tam kapsayıcı görüntü adı, `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment`

Bu kapsayıcı görüntüsünde aşağıdaki Etiketler kullanılabilir. Ayrıca [, MCR üzerinde etiketlerin](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/sentiment/tags/list)tam listesini de bulabilirsiniz.

| Resim etiketleri | Notlar                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | Yaklaşım Analizi v3 (Ingilizce)               |
| `3.0-es`   | Yaklaşım Analizi v3 (Ispanyolca)               |
| `3.0-fr`   | Yaklaşım Analizi v3 (Fransızca)                |
| `3.0-it`   | Yaklaşım Analizi v3 (Italyanca)               |
| `3.0-de`   | Yaklaşım Analizi v3 (Almanca)                |
| `3.0-zh`   | Yaklaşım Analizi v3 (Çince-Basitleştirilmiş)  |
| `3.0-zht`  | Yaklaşım Analizi v3 (Çince-Geleneksel) |
| `3.0-ja`   | Yaklaşım Analizi v3 (Japonca)              |
| `3.0-pt`   | Yaklaşım Analizi v3 (Portekizce)            |
| `3.0-nl`   | Yaklaşım Analizi v3 (Felemenkçe)                 |
| `2.1`    | Yaklaşım Analizi v2      |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[sp-ntts]: ../speech-service/speech-container-howto.md?tabs=ntts
[sp-lid]: ../speech-service/speech-container-howto.md?tabs=lid
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
