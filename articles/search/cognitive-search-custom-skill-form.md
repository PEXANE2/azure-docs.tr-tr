---
title: Form Tanıma özel beceri (C#)
titleSuffix: Azure Cognitive Search
description: C# ve Visual Studio'u kullanarak Form Recognizer özel becerisini nasıl oluşturabilirsiniz öğrenin.
manager: nitinme
author: PatrickFarley
ms.author: pafarley
ms.service: cognitive-search
ms.topic: article
ms.date: 01/21/2020
ms.openlocfilehash: 713b790c432f0e416392243262aed4b0fcda8892
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81274581"
---
# <a name="example-create-a-form-recognizer-custom-skill"></a>Örnek: Form Tanıyıcı özel beceri oluşturma

Bu Azure Bilişsel Arama beceri örneğinde, C# ve Visual Studio'u kullanarak Form Tanıyıcı özel beceri oluşturmayı öğreneceksiniz. Form Tanıyıcısı belgeleri analiz eder ve anahtar/değer çiftleri ve tablo verilerini ayıklar. Form Recognizer'ı [özel beceri arabirimine](cognitive-search-custom-skill-interface.md)sararak, bu özelliği uçtan uca zenginleştirme ardışık bir ardışık lıkta bir adım olarak ekleyebilirsiniz. Ardışık hatlar daha sonra belgeleri yükleyebilir ve diğer dönüşümleri yapabilir.

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (herhangi bir baskı).
- Aynı türden en az beş form. Bu kılavuzla sağlanan örnek verileri kullanabilirsiniz.

## <a name="create-a-form-recognizer-resource"></a>Form Tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](../cognitive-services/form-recognizer/includes/create-resource.md)]

## <a name="train-your-model"></a>Modelinizi eğitme

Bu beceriyi kullanmadan önce giriş formlarınızla bir Form Tanıyıcı modeli eğitmeniz gerekir. Bir modeli nasıl eğitin öğrenince [cURL quickstart'ı](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/quickstarts/curl-train-extract) izleyin. Bu hızlı başlatmada sağlanan örnek formları kullanabilir veya kendi verilerinizi kullanabilirsiniz. Model eğitildikten sonra, kimlik değerini güvenli bir konuma kopyalayın.

## <a name="set-up-the-custom-skill"></a>Özel beceri yi ayarlama

Bu öğretici, Azure Arama [Güç Becerileri](https://github.com/Azure-Samples/azure-search-power-skills) GitHub deposunda [AnalyzeForm](https://github.com/Azure-Samples/azure-search-power-skills/tree/master/Vision/AnalyzeForm) projesini kullanır. Bu depoyu yerel makinenize kopyala ve projeye erişmek için **Vision/AnalyzeForm/'a** gidin. Ardından Visual _Studio'da AnalyzeForm.csproj'u_ açın. Bu proje, [özel beceri arabirimini](cognitive-search-custom-skill-interface.md) karşılayan ve Azure Bilişsel Arama zenginleştirme için kullanılabilecek bir Azure İşlevi kaynağı oluşturur. Form belgelerini giriş olarak alır ve belirttiğiniz anahtar/değer çiftlerini (metin olarak) çıkarır.

İlk olarak, proje düzeyinde ortam değişkenleri ekleyin. **AnalyzeForm** projesini sol bölmede bulun, sağ tıklayın ve **Özellikler'i**seçin. **Özellikler** penceresinde Hata **Ayıklama** sekmesini tıklatın ve ardından **Çevre değişkenleri** alanını bulun. Aşağıdaki değişkenleri eklemek için **Ekle'yi** tıklatın:
* `FORMS_RECOGNIZER_ENDPOINT_URL`son nokta URL'nize ayarlanmış değerle.
* `FORMS_RECOGNIZER_API_KEY`abonelik anahtarına ayarlanan değerle.
* `FORMS_RECOGNIZER_MODEL_ID`eğittiğiniz modelin kimliğine ayarlanmış değerle.
* `FORMS_RECOGNIZER_RETRY_DELAY`değeri 1000 olarak ayarlanır. Bu değer, programın sorguyu yeniden denemeden önce bekleyeceği milisaniye cinsinden süredir.
* `FORMS_RECOGNIZER_MAX_ATTEMPTS`değeri 100 olarak ayarlanır. Bu değer, başarılı bir yanıt almaya çalışırken programın hizmeti kaç kez sorgulayacakolmasıdır.

Ardından, _AnalyzeForm.cs_ açın `fieldMappings` ve *alan eşlemeleri.json* dosyasına başvuran değişkeni bulun. Bu dosya (ve buna başvuran değişken) formlarınızdan ayıklamak istediğiniz anahtarların listesini ve her anahtar için özel bir etiket tanımlar. Örneğin, bir değer `{ "Address:", "address" }, { "Invoice For:", "recipient" }` komut dosyası yalnızca algılanan `Address:` ve `Invoice For:` alanlar için değerleri kaydedecek anlamına `"address"` `"recipient"`gelir ve bu değerleri etiketleyecek ve sırasıyla.

Son olarak, `contentType` değişkene dikkat edin. Bu komut dosyası, verilen Form Recognizer modelini URL'ye göre `application/json`başvurulan uzak belgelerde çalıştırdığı için içerik türü . Yerel dosyaları HTTP isteklerine bayt akışlarını ekleyerek çözümlemek istiyorsanız, dosyanız için `contentType` uygun [MIME türüne](https://developer.mozilla.org/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types) değiştirmeniz gerekir.

## <a name="test-the-function-from-visual-studio"></a>Visual Studio'dan işlevi test edin

Projenizi düzenledikten sonra kaydedin ve **AnalyzeForm** projesini Visual Studio'da başlangıç projesi olarak ayarlayın (önceden ayarlı değilse). Ardından yerel ortamınızdaki işlevi çalıştırmak için **F5** tuşuna basın. İşlevi aramak için [Postacı](https://www.postman.com/) gibi bir REST hizmetini kullanın.

### <a name="http-request"></a>HTTP isteği

İşlevi aramak için aşağıdaki isteği yaparsınız.

```HTTP
POST https://localhost:7071/api/analyze-form
```

### <a name="request-body"></a>İstek gövdesi

Aşağıdaki istek gövdesi şablonu yla başlayın.

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

Burada, eğitim aldığınız formlarla aynı türde bir formun URL'sini sağlamanız gerekir. Test amacıyla, eğitim formlarınızdan birini kullanabilirsiniz. CURL hızlı başlatılan ı izlediyseniz, formlarınız bir Azure blob depolama hesabında bulunur. Azure Depolama Gezgini'ni açın, bir form dosyası bulun, sağ tıklatın ve **Paylaşılan Erişim İmzası Al'ı**seçin. Sonraki iletişim penceresi bir URL ve SAS belirteci sağlar. İstek gövdenizin `"formUrl"` alanlarına ve `"formSasToken"` alanlarına sırasıyla bu dizeleri girin.

> [!div class="mx-imgBorder"]
> ![Azure depolama gezgini; pdf belgesi seçilir](media/cognitive-search-skill-form/form-sas.png)

Azure blob depolama alanında olmayan uzak bir belgeyi çözümlemek istiyorsanız, `"formUrl"` URL'sini `"formSasToken"` alana yapıştırın ve alanı boş bırakın.

> [!NOTE]
> Beceri bir beceri ye entegre edildiğinde, URL ve belirteç Bilişsel Arama tarafından sağlanacaktır.

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

İşlev davranışından memnun olduğunuzda, yayımlayabilirsiniz.

1. Visual Studio'daki **Çözüm Gezgini'nde** projeyi sağ tıklatın ve **Yayımla'yı**seçin.  >  **Yeni Yayımla'yı Seçin.****Publish**
1. Visual Studio'yı Azure hesabınıza zaten bağlamadıysanız, **hesap ekle'yi seçin....**
1. Ekrandaki istemleri izleyin. Uygulama hizmetiniz, Azure aboneliğiniz, kaynak grubunuz, barındırma planınız ve kullanmak istediğiniz depolama hesabı için benzersiz bir ad belirtin. Bunlara zaten sahip değilseniz yeni bir kaynak grubu, yeni bir barındırma planı ve yeni bir depolama hesabı oluşturabilirsiniz. Bitirdikten sonra **Oluştur'u**seçin.
1. Dağıtım tamamlandıktan sonra Site URL'sini not edin. Bu URL, Azure'daki işlev uygulamanızın adresidir. Geçici bir konuma kaydedin.
1. Azure [portalında](https://portal.azure.com)Kaynak Grubu'na gidin ve `AnalyzeForm` yayınladığınız İşlev'i arayın. **Yönet** bölümünün altında Ana Bilgisayar Anahtarlarını görmeniz gerekir. *Varsayılan* ana bilgisayar anahtarını kopyalayın ve geçici bir konuma kaydedin.

## <a name="connect-to-your-pipeline"></a>Ardınıza bağlanın

Bilişsel Arama ardışık bir şekilde bu beceriyi kullanmak için becerinize bir beceri tanımı eklemeniz gerekir. Aşağıdaki JSON bloğu örnek bir beceri tanımıdır (giriş ve çıktıları belirli senaryo nuzu ve beceri ortamınızı yansıtacak şekilde güncelleştirmeniz gerekir). İşlev URL'nizle `AzureFunctionEndpointUrl` `AzureFunctionDefaultHostKey` değiştirin ve ana bilgisayar anahtarınızla değiştirin.

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

Bu kılavuzda, Azure Form Tanıyıcı hizmetinden özel bir beceri oluşturdunuz. Özel beceriler hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın. 

* [Azure Arama Gücü Becerileri: özel becerilerin deposu](https://github.com/*zure-Samples/azure-search-power-skills)
* [AI zenginleştirme hattına özel bir beceri ekleme](cognitive-search-custom-skill-interface.md)
* [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
* [Bir skillset (REST) oluşturun](https://docs.microsoft.com/rest/api/*earchservice/create-skillset)
* [Harita zenginleştirilmiş alanlar](cognitive-search-output-field-mapping.md)
