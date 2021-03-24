---
title: 'Hızlı başlangıç: .NET için Azure yönetim istemcisi kitaplığı'
description: Bu hızlı başlangıçta, .NET için Azure Yönetim istemci kitaplığı 'nı kullanmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 3/22/2021
ms.author: pafarley
ms.openlocfilehash: 1ecfd58e961e70182bfc2e0cc8eaf77f5053eb77
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104879559"
---
[Başvuru belgeleri](/dotnet/api/overview/azure/cognitiveservices/management)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Microsoft.Azure.Management.CognitiveServices)  |  [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Management.CognitiveServices/)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Microsoft.Azure.Management.CognitiveServices/tests)

## <a name="c-prerequisites"></a>C# önkoşulları

* Geçerli bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/).
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.

[!INCLUDE [Create a service principal](./create-service-principal.md)]

[!INCLUDE [Create a resource group](./create-resource-group.md)]

## <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Yeni bir .NET Core uygulaması oluşturun. Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `azure-management-quickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: *program. cs*. 

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

*Program. cs* dosyasını açın ve aşağıdaki `using` deyimlerini dosyanın en üstüne ekleyin:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_using)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Aşağıdaki alanları *program. cs* köküne ekleyin ve oluşturduğunuz hizmet sorumlusunu ve Azure hesap bilgilerinizi kullanarak değerlerini doldurun.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_constants)]

Ardından, **ana** yönteminizin bu değerleri kullanarak bir **Biliveservicesmanagementclient** nesnesi oluşturun. Bu nesne, tüm Azure yönetim işlemleri için gereklidir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_assigns)]

## <a name="call-management-methods"></a>Arama yönetimi yöntemleri

Kullanılabilir kaynakları listelemek için aşağıdaki kodu **Main** yöntesisteminize ekleyin, örnek bir kaynak oluşturun, sahip olduğunuz kaynakları listeleyin ve ardından örnek kaynağı silin. Sonraki adımlarda bu yöntemleri tanımlayacaksınız.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_calls)]

## <a name="create-a-cognitive-services-resource-c"></a>Bilişsel hizmetler kaynağı oluşturma (C#)

Yeni bir bilişsel hizmetler kaynağı oluşturup abone olmak için **Create** yöntemini kullanın. Bu yöntem, geçirdiğiniz kaynak grubuna yeni bir faturalanabilir kaynak ekler. Yeni kaynağınızı oluştururken, kullanmak istediğiniz hizmetin "tür" değerini, fiyatlandırma katmanını (veya SKU 'SU) ve bir Azure konumunu bilmeniz gerekir. Aşağıdaki yöntem, bunların tümünü bağımsız değişken olarak alır ve bir kaynak oluşturur.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_create)]

### <a name="choose-a-service-and-pricing-tier"></a>Hizmet ve fiyatlandırma katmanı seçin

Yeni bir kaynak oluşturduğunuzda, kullanmak istediğiniz hizmet için "tür" i ve istediğiniz [fiyatlandırma katmanını](https://azure.microsoft.com/pricing/details/cognitive-services/) (veya SKU) birlikte bilmeniz gerekir. Kaynağı oluştururken bu ve diğer bilgileri parametre olarak kullanacaksınız. Betiğinizdeki aşağıdaki yöntemi çağırarak, kullanılabilir bilişsel hizmetin "türleri" listesini bulabilirsiniz:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/azure_management_service/create_delete_resource.cs?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

[!INCLUDE [SKUs and pricing](./sku-pricing.md)]

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

* Bilişsel hizmetlerle güvenli bir şekilde çalışmak için bkz. Azure bilişsel **[Hizmetler 'e yönelik kimlik doğrulama istekleri](../../authentication.md)** .
* Bilişsel hizmetler 'deki farklı kategorilerin listesini almak için bkz. **[Azure bilişsel hizmetler nedir?](../../what-are-cognitive-services.md)** .
* Bilişsel hizmetlerin desteklediği doğal dillerin listesini görmek için bkz. **[doğal dil desteği](../../language-support.md)** .
* Bilişsel hizmetler 'in şirket içi hizmetlerini nasıl kullanacağınızı anlamak için bkz. bilişsel **[Hizmetler 'i kapsayıcı olarak kullanma](../../cognitive-services-container-support.md)** .
* Bilişsel hizmetler 'in kullanım maliyetini tahmin etmek için bilişsel **[Hizmetler 'in maliyetlerini planlayın ve yönetin](../../plan-manage-costs.md)** bölümüne bakın.
* Yönetim SDK 'Sı hakkında daha fazla bilgi için bilişsel **[Hizmetler Yönetim SDK 'sı başvuru belgelerine](/dotnet/api/overview/azure/cognitiveservices/management)** bakın.
