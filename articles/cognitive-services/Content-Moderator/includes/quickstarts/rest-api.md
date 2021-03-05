---
title: Content Moderator REST API hızlı başlangıç
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Azure Content Moderator REST API kullanmaya nasıl başlaleyeceğinizi öğrenin. Yönetmeliklerle uyum sağlamak veya kullanıcılarınız için amaçlanan ortamı sürdürmek üzere uygulamanıza içerik filtreleme yazılımı oluşturun.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: include
ms.date: 12/08/2020
ms.author: pafarley
ms.openlocfilehash: 346a30b538af8006eaada13b00c77762b9cdfca8
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102193748"
---
Azure Content Moderator REST API kullanmaya başlayın. 

Content Moderator, rahatsız edici, riskli veya başka türlü istenmeyen içerikleri işlemenize imkan tanıyan bir AI hizmetidir. Metin, resim ve videoları taramak ve içerik bayraklarını otomatik olarak uygulamak için AI destekli içerik denetleme hizmetini kullanın. Ardından, bir insan gözden geçirenler ekibi için çevrimiçi bir moderatör ortamı olan gözden geçirme aracı ile uygulamanızı tümleştirin. Yönetmeliklerle uyum sağlamak veya kullanıcılarınız için amaçlanan ortamı sürdürmek üzere uygulamanıza içerik filtreleme yazılımı oluşturun.

Content Moderator REST API için kullanın:

* Orta metin
* Orta görüntüler

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduktan sonra, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir Content moderator kaynağı oluşturun Content moderator bir kaynak oluşturun. Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    * Uygulamanızı Content Moderator bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* [PowerShell sürüm 6.0 +](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-7.1)veya benzer bir komut satırı uygulaması.


## <a name="moderate-text"></a>Orta metin

Bir metin gövdesini çözümlemek ve sonuçları konsola yazdırmak için Content Moderator API 'sini çağırmak üzere aşağıdaki gibi bir komut kullanacaksınız.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/content-moderator/quickstart.sh" ID="textmod":::

Komutu bir metin düzenleyicisine kopyalayın ve aşağıdaki değişiklikleri yapın:

1. `Ocp-Apim-Subscription-Key`Geçerli yüz abonelik anahtarınıza atayın.
1. Sorgu URL 'sinin ilk kısmını, abonelik anahtarınıza karşılık gelen uç noktayla eşleşecek şekilde değiştirin.
   [!INCLUDE [subdomains-note](../../../../../includes/cognitive-services-custom-subdomains-note.md)]
1. İsteğe bağlı olarak, isteğin gövdesini çözümlemek istediğiniz metin dizesiyle değiştirin.

Değişikliklerinizi yaptıktan sonra, bir komut istemi açın ve yeni komutu girin. 

### <a name="examine-the-results"></a>Sonuçları inceleme

Metin denetleme sonuçlarının konsol penceresinde JSON verileri olarak görüntülendiğini görmeniz gerekir. Örnek:

```json
{
  "OriginalText": "Is this a crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255,\n1 Microsoft Way, Redmond, WA 98052\n",
  "NormalizedText": "Is this a crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, \n1 Microsoft Way, Redmond, WA 98052",
  "AutoCorrectedText": "Is this a crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, \n1 Microsoft Way, Redmond, WA 98052",
  "Misrepresentation": null,
  "PII": {
    "Email": [
      {
        "Detected": "abcdef@abcd.com",
        "SubType": "Regular",
        "Text": "abcdef@abcd.com",
        "Index": 21
      }
    ],
    "IPA": [
      {
        "SubType": "IPV4",
        "Text": "255.255.255.255",
        "Index": 61
      }
    ],
    "Phone": [
      {
        "CountryCode": "US",
        "Text": "6657789887",
        "Index": 45
      }
    ],
    "Address": [
      {
        "Text": "1 Microsoft Way, Redmond, WA 98052",
        "Index": 78
      }
    ]
  },
 "Classification": {
    "Category1": 
    {
      "Score": 0.5
    },
    "Category2": 
    {
      "Score": 0.6
    },
    "Category3": 
    {
      "Score": 0.5
    },
    "ReviewRecommended": true
  },
  "Language": "eng",
  "Terms": [
    {
      "Index": 10,
      "OriginalIndex": 10,
      "ListId": 0,
      "Term": "crap"
    }
  ],
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "TrackingId": "1717c837-cfb5-4fc0-9adc-24859bfd7fac"
}
```

Ekranları Content Moderator için görüntülenen metin öznitelikleri hakkında daha fazla bilgi için bkz. [metin denetleme kavramları](../../text-moderation-api.md) Kılavuzu.

## <a name="moderate-images"></a>Orta görüntüler

Aşağıdaki gibi bir komut kullanarak, uzak bir görüntüye Content Moderator API 'sini çağırıp sonuçları konsola yazdırabilirsiniz.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/content-moderator/quickstart.sh" ID="imagemod":::

Komutu bir metin düzenleyicisine kopyalayın ve aşağıdaki değişiklikleri yapın:

1. `Ocp-Apim-Subscription-Key`Geçerli yüz abonelik anahtarınıza atayın.
1. Sorgu URL 'sinin ilk kısmını, abonelik anahtarınıza karşılık gelen uç noktayla eşleşecek şekilde değiştirin.
1. İsteğe bağlı olarak `"Value"` istek gövdesindeki URL 'yi, orta olmasını istediğiniz uzak görüntüye göre değiştirin.

> [!TIP]
> Ayrıca, yerel görüntüleri, bayt verilerini istek gövdesine geçirerek de oluşturabilirsiniz. Bunun nasıl yapılacağını öğrenmek için [başvuru belgelerine](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c) bakın.

Değişikliklerinizi yaptıktan sonra, bir komut istemi açın ve yeni komutu girin. 

### <a name="examine-the-results"></a>Sonuçları inceleme

Görüntü denetleme sonuçlarının konsol penceresinde JSON verileri olarak görüntülendiğini görmeniz gerekir. 

```json
{
  "AdultClassificationScore": x.xxx,
  "IsImageAdultClassified": <Bool>,
  "RacyClassificationScore": x.xxx,
  "IsImageRacyClassified": <Bool>,
  "AdvancedInfo": [],
  "Result": false,
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "TrackingId": "<Request Tracking Id>"
}
```

Ekranları Content Moderator görüntü öznitelikleri hakkında daha fazla bilgi için bkz. [görüntü denetleme kavramları](../../image-moderation-api.md) Kılavuzu.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, denetleme görevlerini yapmak için Content Moderator REST API nasıl kullanacağınızı öğrendiniz. Daha sonra, bir kavramsal kılavuz okuyarak görüntülerin veya diğer ortamların denetimi hakkında daha fazla bilgi edinin.

* [Görüntü denetleme kavramları](../../image-moderation-api.md)
* [Metin denetleme kavramları](../../text-moderation-api.md)
* [Azure Content Moderator nedir?](../../overview.md)