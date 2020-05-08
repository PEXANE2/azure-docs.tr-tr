---
title: Form tanıyıcı özel yeteneği (C#)
titleSuffix: Azure Cognitive Search
description: C# ve Visual Studio kullanarak form tanıyıcı özel becerisi oluşturmayı öğrenin.
manager: nitinme
author: PatrickFarley
ms.author: pafarley
ms.service: cognitive-search
ms.topic: article
ms.date: 01/21/2020
ms.openlocfilehash: 050848b0bff65b19e2b17bd170e1d3e9ff0176f1
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792012"
---
# <a name="example-create-a-form-recognizer-custom-skill"></a>Örnek: form tanıyıcı özel yeteneği oluşturma

Bu Azure Bilişsel Arama Beceri örneğinde, C# ve Visual Studio kullanarak form tanıyıcı özel becerisi oluşturmayı öğreneceksiniz. Form tanıyıcı, belgeleri analiz eder ve anahtar/değer çiftlerini ve tablo verilerini ayıklar. Form tanıyıcısını [özel beceri arabirimine](cognitive-search-custom-skill-interface.md)sarmalayarak, bu özelliği uçtan uca bir zenginleştirme ardışık düzeninde bir adım olarak ekleyebilirsiniz. İşlem hattı daha sonra belgeleri yükleyebilir ve diğer dönüştürmeleri gerçekleştirebilir.

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (herhangi bir sürüm).
- Aynı türde en az beş form. Bu kılavuzla birlikte sunulan örnek verileri kullanabilirsiniz.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../cognitive-services/form-recognizer/includes/create-resource.md)]

## <a name="train-your-model"></a>Modelinizi eğitme

Bu yeteneği kullanmadan önce, giriş formlarınıza bir form tanıyıcı modeli eğmeniz gerekir. Bir modeli eğitme hakkında bilgi edinmek için [kıvrımlı hızlı](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/quickstarts/curl-train-extract) başlangıcı izleyin. Bu hızlı başlangıçta sunulan örnek formları kullanabilir veya kendi verilerinizi kullanabilirsiniz. Model eğitilirken, KIMLIK değerini güvenli bir konuma kopyalayın.

## <a name="set-up-the-custom-skill"></a>Özel yeteneği ayarlama

Bu öğreticide, [Azure Search Power beceriler](https://github.com/Azure-Samples/azure-search-power-skills) GitHub deposundaki [analiz zeform](https://github.com/Azure-Samples/azure-search-power-skills/tree/master/Vision/AnalyzeForm) projesi kullanılmaktadır. Bu depoyu yerel makinenize kopyalayın ve projeye erişmek için **Vision/Analzeform/** ' a gidin. Ardından Visual Studio 'da _analiz Zeform. csproj_ ' u açın. Bu proje, [özel beceri arabirimini](cognitive-search-custom-skill-interface.md) yerine getiren ve Azure bilişsel arama zenginleştirme için kullanılabilen bir Azure işlevi kaynağı oluşturur. Form belgelerini giriş olarak alır ve belirttiğiniz anahtar/değer çiftleri (metin olarak) verir.

İlk olarak, proje düzeyi ortam değişkenlerini ekleyin. Sol bölmedeki **analiz Zeformu** projesini bulun, sağ tıklayın ve **Özellikler**' i seçin. **Özellikler** penceresinde **Hata Ayıkla** sekmesine tıklayın ve ardından **ortam değişkenleri** alanını bulun. Aşağıdaki değişkenleri eklemek için **Ekle** ' ye tıklayın:
* `FORMS_RECOGNIZER_ENDPOINT_URL`değerini Endpoint URL 'niz olarak ayarlayın.
* `FORMS_RECOGNIZER_API_KEY`değerini abonelik anahtarınız olarak ayarlayın.
* `FORMS_RECOGNIZER_MODEL_ID`değer ile, eğitilen modelin KIMLIĞI olarak ayarlanır.
* `FORMS_RECOGNIZER_RETRY_DELAY`değerini 1000 olarak ayarlayın. Bu değer, programın sorguyu yeniden denemeden önce bekleyeceği süre (milisaniye olarak).
* `FORMS_RECOGNIZER_MAX_ATTEMPTS`değerini 100 olarak ayarlayın. Bu değer, başarılı bir yanıt almaya çalışırken programın hizmeti sorgulama işleminin sayısıdır.

Sonra, _AnalyzeForm.cs_ açın ve `fieldMappings` *alan-eşlemeleri. JSON* dosyasına başvuran değişkeni bulun. Bu dosya (ve buna başvuran değişken), formlarınızda ayıklamak istediğiniz anahtarların listesini ve her anahtar için özel bir etiketi tanımlar. Örneğin, bir değeri, betiğin `{ "Address:", "address" }, { "Invoice For:", "recipient" }` yalnızca algılanan `Address:` ve `Invoice For:` alanların değerlerini kaydedebileceği anlamına gelir ve sırasıyla bu değerleri ve `"address"` `"recipient"`ile etiketlemesini sağlayacaktır.

Son olarak, `contentType` değişkenine göz önünde. Bu betik, URL tarafından başvurulan uzak belgelerde verilen form tanıyıcı modelini çalıştırır, bu nedenle içerik türü olur `application/json`. Yerel dosyaları, bayt akışlarını HTTP isteklerine dahil ederek çözümlemek istiyorsanız, `contentType` öğesini dosyanız Için uygun [MIME türüyle](https://developer.mozilla.org/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types) değiştirmeniz gerekir.

## <a name="test-the-function-from-visual-studio"></a>İşlevi Visual Studio 'dan test etme

Projenizi düzenledikten sonra kaydedin ve **analiz Zeformu** projesini Visual Studio 'da başlangıç projesi olarak ayarlayın (henüz ayarlanmamışsa). Sonra, işlevi yerel ortamınızda çalıştırmak için **F5** tuşuna basın. İşlevi çağırmak için [Postman](https://www.postman.com/) gıbı bir rest hizmetini kullanın.

### <a name="http-request"></a>HTTP isteği

İşlevi çağırmak için aşağıdaki isteği yaparsınız.

```HTTP
POST https://localhost:7071/api/analyze-form
```

### <a name="request-body"></a>İstek gövdesi

Aşağıdaki istek gövdesi şablonuyla başlayın.

```json
{
    "values": [
        {
            "recordId": "record1",
            "data": { 
                "formUrl": "<your-form-url>",
                "formSasToken": "<your-sas-token>"
            }
        }
    ]
}
```

Burada, eğitilen formlarla aynı türde olan bir formun URL 'sini sağlamanız gerekir. Test amacıyla, eğitim formlarınızın birini kullanabilirsiniz. Kıvrımlı hızlı başlangıcı izlediyseniz, formlarınızın bir Azure Blob depolama hesabında yer alır. Azure Depolama Gezgini açın, bir form dosyasını bulun, sağ tıklayın ve **paylaşılan erişim Imzasını al**' ı seçin. Sonraki iletişim kutusu penceresi bir URL ve SAS belirteci sağlar. Bu dizeleri sırasıyla, istek `"formUrl"` gövdesinin `"formSasToken"` ve alanlarına girin.

> [!div class="mx-imgBorder"]
> ![Azure Depolama Gezgini; PDF belgesi seçildi](media/cognitive-search-skill-form/form-sas.png)

Azure Blob depolamada olmayan bir uzak belgeyi çözümlemek isterseniz, `"formUrl"` alanına URL 'sini yapıştırın ve `"formSasToken"` alanı boş bırakın.

> [!NOTE]
> Yetenek bir beceri tümleştirildiğinde, URL ve belirteç Bilişsel Arama tarafından sağlanır.

### <a name="response"></a>Yanıt

Aşağıdaki örneğe benzer bir yanıt görmeniz gerekir:

```json
{
    "values": [
        {
            "recordId": "record1",
            "data": {
                "address": "1111 8th st. Bellevue, WA 99501 ",
                "recipient": "Southridge Video 1060 Main St. Atlanta, GA 65024 "
            },
            "errors": null,
            "warnings": null
        }
    ]
}
```

## <a name="publish-the-function-to-azure"></a>İşlevi Azure’a yayımlama

İşlev davranışından memnun kaldığınızda, bunu yayımlayabilirsiniz.

1. Visual Studio 'daki **Çözüm Gezgini** projeye sağ tıklayın ve **Yayımla**' yı seçin. **Yeni** > **Yayımlama**oluştur öğesini seçin.
1. Zaten Visual Studio 'Yu Azure hesabınıza bağladıysanız **Hesap Ekle...** seçeneğini belirleyin.
1. Ekrandaki istemleri izleyin. App Service, Azure aboneliği, kaynak grubu, barındırma planı ve kullanmak istediğiniz depolama hesabı için benzersiz bir ad belirtin. Henüz yoksa yeni bir kaynak grubu, yeni bir barındırma planı ve yeni bir depolama hesabı oluşturabilirsiniz. İşiniz bittiğinde **Oluştur**' u seçin.
1. Dağıtım tamamlandıktan sonra, site URL 'sine dikkat edin. Bu URL, Azure 'daki işlev uygulamanızın adresidir. Geçici bir konuma kaydedin.
1. [Azure Portal](https://portal.azure.com), kaynak grubuna gidin ve yayımladığınız `AnalyzeForm` işlevi arayın. **Yönet** bölümünün altında ana bilgisayar anahtarlarını görmeniz gerekir. *Varsayılan* ana bilgisayar anahtarını kopyalayın ve geçici bir konuma kaydedin.

## <a name="connect-to-your-pipeline"></a>İşlem hattınızla bağlantı kurmak

Bu yeteneği bir Bilişsel Arama işlem hattında kullanmak için, Beceri ' e bir yetenek tanımı eklemeniz gerekir. Aşağıdaki JSON bloğu bir örnek yetenek tanımıdır (giriş ve çıkışları, belirli senaryonuzu ve beceri ortamınızı yansıtacak şekilde güncelleştirmeniz gerekir). İşlev `AzureFunctionEndpointUrl` URL 'niz ile değiştirin ve yerine ana `AzureFunctionDefaultHostKey` bilgisayar anahtarınızla değiştirin.

```json
{ 
  "description":"Skillset that invokes the Form Recognizer custom skill",
  "skills":[ 
    "[... your existing skills go here]",
    { 
      "@odata.type":"#Microsoft.Skills.Custom.WebApiSkill",
      "name":"formrecognizer",
      "description":"Extracts fields from a form using a pre-trained form recognition model",
      "uri":"[AzureFunctionEndpointUrl]/api/analyze-form?code=[AzureFunctionDefaultHostKey]",
      "httpMethod":"POST",
      "timeout":"PT30S",
      "context":"/document",
      "batchSize":1,
      "inputs":[ 
        { 
          "name":"formUrl",
          "source":"/document/metadata_storage_path"
        },
        { 
          "name":"formSasToken",
          "source":"/document/metadata_storage_sas_token"
        }
      ],
      "outputs":[ 
        { 
          "name":"address",
          "targetName":"address"
        },
        { 
          "name":"recipient",
          "targetName":"recipient"
        }
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, Azure form tanıyıcı hizmetinden özel bir yetenek oluşturdunuz. Özel yetenekler hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın. 

* [Azure Search Power becerileri: özel yeteneklerin bir deposu](https://github.com/Azure-Samples/azure-search-power-skills)
* [Bir AI zenginleştirme ardışık düzenine özel yetenek ekleme](cognitive-search-custom-skill-interface.md)
* [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
* [Beceri oluşturma (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
* [Zenginleştirilmiş alanları Eşle](cognitive-search-output-field-mapping.md)
