---
title: 'Hızlı başlangıç: .NET için Azure yönetim istemcisi kitaplığı'
description: Bu hızlı başlangıçta, .NET için Azure Yönetim istemci kitaplığı 'nı kullanmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: 93ea0ffc94b78e014b30ab1d45d589eba50fe524
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88607792"
---
[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/management?view=azure-dotnet)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Microsoft.Azure.Management.CognitiveServices)  |  [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Management.CognitiveServices/)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Microsoft.Azure.Management.CognitiveServices/tests)

## <a name="prerequisites"></a>Önkoşullar

* Geçerli bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/).
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.

## <a name="create-an-azure-service-principal"></a>Azure hizmet sorumlusu oluşturma

Uygulamanızın Azure hesabınızla etkileşime geçmesini sağlamak için izinleri yönetmek üzere bir Azure hizmet sorumlusu gerekir. [Azure hizmet sorumlusu oluşturma](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-4.4.0&viewFallbackFrom=azps-3.3.0)bölümündeki yönergeleri izleyin.

Bir hizmet sorumlusu oluşturduğunuzda gizli bir değer, bir KIMLIK ve bir uygulama KIMLIĞI olduğunu görürsünüz. Sonraki adımlar için uygulama KIMLIĞI ve gizli anahtarı geçici bir konuma kaydedin.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bilişsel hizmetler kaynağı oluşturmadan önce, hesabınızın kaynağı içermesi için bir Azure Kaynak grubu olması gerekir. Henüz bir kaynak grubunuz yoksa devam etmeden önce [Azure Portal](https://ms.portal.azure.com/) bir tane oluşturun.

## <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Yeni bir .NET Core uygulaması oluşturun. Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `azure-management-quickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: *program.cs*. 

```console
dotnet new console -n azure-management-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```console
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Uygulama dizini içinde, aşağıdaki komutla .NET için Azure Yönetim istemci Kitaplığı ' nı yükleyeceksiniz:

```console
dotnet add package Microsoft.Azure.Management.CognitiveServices
dotnet add package Microsoft.Azure.Management.Fluent
dotnet add package Microsoft.Azure.Management.ResourceManager.Fluent
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.

### <a name="import-libraries"></a>Kitaplıkları içeri aktarma

*Program.cs* açın ve aşağıdaki `using` deyimlerini dosyanın en üstüne ekleyin:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_using)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Aşağıdaki alanları *program.cs* köküne ekleyin ve oluşturduğunuz hizmet sorumlusunu ve Azure hesap bilgilerinizi kullanarak değerlerini doldurun.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_constants)]

Ardından, **ana** yönteminizin bu değerleri kullanarak bir **Biliveservicesmanagementclient** nesnesi oluşturun. Bu nesne, tüm Azure yönetim işlemleri için gereklidir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_assigns)]

## <a name="call-management-methods"></a>Arama yönetimi yöntemleri

Kullanılabilir kaynakları listelemek için aşağıdaki kodu **Main** yöntesisteminize ekleyin, örnek bir kaynak oluşturun, sahip olduğunuz kaynakları listeleyin ve ardından örnek kaynağı silin. Sonraki adımlarda bu yöntemleri tanımlayacaksınız.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_calls)]

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

### <a name="choose-a-service-and-pricing-tier"></a>Hizmet ve fiyatlandırma katmanı seçin

Yeni bir kaynak oluşturduğunuzda, kullanmak istediğiniz hizmet için "tür" i ve istediğiniz [fiyatlandırma katmanını](https://azure.microsoft.com/pricing/details/cognitive-services/) (veya SKU) birlikte bilmeniz gerekir. Kaynağı oluştururken bu ve diğer bilgileri parametre olarak kullanacaksınız. Betiğinizdeki aşağıdaki yöntemi çağırarak, kullanılabilir bilişsel hizmetin "türleri" listesini bulabilirsiniz:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

Aşağıdaki SKU 'ların ve fiyatlandırma bilgilerinin listesine bakın. 

#### <a name="multi-service"></a>Çoklu hizmet

| Hizmet                    | Tip                      |
|----------------------------|---------------------------|
| Birden çok hizmet. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/) sayfasına bakın.            | `CognitiveServices`     |


#### <a name="vision"></a>Görsel

| Hizmet                    | Tip                      |
|----------------------------|---------------------------|
| Görüntü İşleme            | `ComputerVision`          |
| Özel Görüntü İşleme tahmin | `CustomVision.Prediction` |
| Özel Görüntü İşleme-eğitim   | `CustomVision.Training`   |
| Yüz                       | `Face`                    |
| Form Tanıma            | `FormRecognizer`          |
| Mürekkep Tanıma             | `InkRecognizer`           |

#### <a name="search"></a>Arama

| Hizmet            | Tip                  |
|--------------------|-----------------------|
| Bing Otomatik Öneri   | `Bing.Autosuggest.v7` |
| Bing Özel Arama | `Bing.CustomSearch`   |
| Bing Varlık Arama | `Bing.EntitySearch`   |
| Bing Arama        | `Bing.Search.v7`      |
| Bing Yazım Denetimi   | `Bing.SpellCheck.v7`  |

#### <a name="speech"></a>Konuşma

| Hizmet            | Tip                 |
|--------------------|----------------------|
| Konuşma Hizmetleri    | `SpeechServices`     |
| Konuşma Tanıma | `SpeakerRecognition` |

#### <a name="language"></a>Dil

| Hizmet            | Tip                |
|--------------------|---------------------|
| Form anlama | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| Soru-Cevap Oluşturucu          | `QnAMaker`          |
| Metin Analizi     | `TextAnalytics`     |
| Metin Çevirisi   | `TextTranslation`   |

#### <a name="decision"></a>Karar

| Hizmet           | Tip               |
|-------------------|--------------------|
| Anomali Algılayıcısı  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Kişiselleştirme      | `Personalizer`     |


#### <a name="pricing-tiers-and-billing"></a>Fiyatlandırma katmanları ve faturalandırma

Fiyatlandırma katmanları (ve faturalandırılan miktar), kimlik doğrulama bilgilerinizi kullanarak göndereceğiniz işlem sayısına bağlıdır. Her fiyatlandırma katmanı şunları belirtir:
* saniye başına izin verilen en fazla işlem sayısı (TPS).
* fiyatlandırma katmanında etkinleştirilen hizmet özellikleri.
* önceden tanımlı işlem sayısı için maliyet. Bu sayının üzerine gitmek, hizmetinize ilişkin [fiyatlandırma ayrıntılarında](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) belirtilen ek ücret oluşmasına neden olur.

> [!NOTE]
> Bilişsel hizmetlerin birçoğu hizmeti denemek için kullanabileceğiniz ücretsiz bir katmana sahiptir. Ücretsiz katmanı kullanmak için, `F0` kaynağınız IÇIN SKU olarak kullanın.

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

Yeni bir bilişsel hizmetler kaynağı oluşturup abone olmak için **Create** yöntemini kullanın. Bu yöntem, geçirdiğiniz kaynak grubuna yeni bir faturalanabilir kaynak ekler. Yeni kaynağınızı oluştururken, kullanmak istediğiniz hizmetin "tür" değerini, fiyatlandırma katmanını (veya SKU 'SU) ve bir Azure konumunu bilmeniz gerekir. Aşağıdaki yöntem, bunların tümünü bağımsız değişken olarak alır ve bir kaynak oluşturur.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_create)]

## <a name="view-your-resources"></a>Kaynaklarınızı görüntüleyin

Azure hesabınızın altındaki tüm kaynakları (tüm kaynak grupları arasında) görüntülemek için aşağıdaki yöntemi kullanın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_list)]

## <a name="delete-a-resource"></a>Kaynak silme

Aşağıdaki yöntem verilen kaynak grubundan belirtilen kaynağı siler.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_delete)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı komut ile uygulama dizininizden çalıştırın `dotnet run` .

```dotnet
dotnet run
```

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Yönetim SDK 'Sı başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/management?view=azure-dotnet)
* [Azure bilişsel hizmetler nedir?](../../Welcome.md)
* [Azure bilişsel hizmetler 'e yönelik isteklerin kimliğini doğrulama](../../authentication.md)
* [Azure portalını kullanarak yeni kaynak oluşturma](../../cognitive-services-apis-create-account.md)