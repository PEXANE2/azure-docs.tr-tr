---
title: Azure Uygulama yapılandırması en iyi uygulamalar | Microsoft Docs
description: Azure Uygulama yapılandırması 'nı en iyi şekilde kullanmayı öğrenin
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: df56f53b64a35737700529b80c004efeb31eaabc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80348670"
---
# <a name="azure-app-configuration-best-practices"></a>Azure Uygulama yapılandırması en iyi uygulamaları

Bu makalede, Azure Uygulama yapılandırması 'nı kullanırken yaygın desenler ve en iyi uygulamalar ele alınmaktadır.

## <a name="key-groupings"></a>Anahtar gruplandırmaları

Uygulama yapılandırması anahtarları düzenlemek için iki seçenek sunar:

* Anahtar ön ekleri
* Etiketler

Anahtarlarınızı gruplandırmak için bir veya iki seçenekten birini kullanabilirsiniz.

*Anahtar ön ekleri* anahtarların başlangıç parçalarından oluşur. Adlarında aynı öneki kullanarak bir anahtar kümesini mantıksal olarak gruplandırabilirsiniz. Ön ekler, bir ad alanı oluşturmak için, gibi bir sınırlayıcı `/`tarafından bağlı bir URL yoluna benzer birden çok bileşen içerebilir. Bu tür hiyerarşiler, tek bir uygulama yapılandırma deposundaki birçok uygulama, bileşen hizmeti ve ortam için anahtar depolarken yararlı olur.

Dikkat edilmesi gereken önemli bir şey, anahtarların ilgili ayarların değerlerini almak için uygulama kodunuzun başvurduğu şeydir. Anahtarların değişmemesi veya herhangi bir zamanda kodunuzun değiştirilmesini değiştirmeniz gerekir.

*Etiketler* anahtarlar üzerindeki bir özniteliktir. Anahtar çeşitleri oluşturmak için kullanılır. Örneğin, bir anahtarın birden çok sürümüne etiketler atayabilirsiniz. Bir sürüm yineleme, ortam ya da başka bir bağlamsal bilgi olabilir. Uygulamanız, başka bir etiket belirterek tamamen farklı bir anahtar değerleri kümesi isteyebilir. Sonuç olarak, tüm anahtar başvuruları kodunuzda değişmeden kalır.

## <a name="key-value-compositions"></a>Anahtar-değer kompozisyonları

Uygulama yapılandırması, onunla depolanan tüm anahtarları bağımsız varlıklar olarak değerlendirir. Uygulama yapılandırması, anahtarlar arasında herhangi bir ilişki çıkarması veya hiyerarşiye göre anahtar değerleri devralması denenmez. Ancak uygulama kodunuzda uygun yapılandırma yığınlayarak bağlanmış Etiketler kullanarak birden çok anahtar kümesini toplayabilirsiniz.

Şimdi örneği inceleyelim. Değeri geliştirme ortamına göre değişebilen **Asset1**adlı bir ayarınız olduğunu varsayalım. Boş bir etiketi ve "geliştirme" adlı bir etiketi içeren "Asset1" adlı bir anahtar oluşturun. İlk etikette, **Asset1**için varsayılan değeri yerleştirmelisiniz ve "geliştirme" için belirli bir değeri ikinciden yerleştirebilirsiniz.

Kodunuzda, önce herhangi bir etiket olmadan anahtar değerlerini alır ve ardından "geliştirme" etiketiyle ikinci kez aynı anahtar değerleri kümesini alırsınız. İkinci seferinde değerleri aldığınızda, anahtarların önceki değerlerinin üzerine yazılır. .NET Core yapılandırma sistemi, her birinin üzerine birden çok yapılandırma verisi kümesini "yığmanızı" sağlar. Birden çok küme içinde bir anahtar varsa, onu içeren son küme kullanılır. .NET Core gibi modern programlama çerçevesiyle, uygulama yapılandırmasına erişmek için yerel bir yapılandırma sağlayıcısı kullanıyorsanız bu yığınlama özelliğini ücretsiz olarak alırsınız. Aşağıdaki kod parçacığı, .NET Core uygulamasında nasıl yığın uygulayabileceğinizi göstermektedir:

```csharp
// Augment the ConfigurationBuilder with Azure App Configuration
// Pull the connection string from an environment variable
configBuilder.AddAzureAppConfiguration(options => {
    options.Connect(configuration["connection_string"])
           .Select(KeyFilter.Any, LabelFilter.Null)
           .Select(KeyFilter.Any, "Development");
});
```

[Farklı ortamlara yönelik farklı yapılandırmaların etkinleştirilmesi için etiketleri kullanın](./howto-labels-aspnet-core.md) , tüm bir örnek sağlar.

## <a name="app-configuration-bootstrap"></a>Uygulama yapılandırması önyükleme

Bir uygulama yapılandırma deposuna erişmek için, Azure portal kullanılabilir olan bağlantı dizesini kullanabilirsiniz. Bağlantı dizeleri kimlik bilgisi bilgilerini içerdiğinden, bu değerler gizli olarak değerlendirilir. Bu gizli dizileri Azure Key Vault depolanması gerekir ve kodunuzun bunları almak için Key Vault kimlik doğrulaması gerekir.

Daha iyi bir seçenek, Azure Active Directory Yönetilen kimlikler özelliğini kullanmaktır. Yönetilen kimlikler ile, uygulama yapılandırma deponuza erişimi önyüklemek için yalnızca uygulama yapılandırma uç noktası URL 'sine ihtiyacınız vardır. URL 'YI uygulama kodunuza (örneğin, *appSettings. JSON* dosyasına) ekleyebilirsiniz. Ayrıntılar için bkz. [Azure tarafından yönetilen kimlikler Ile tümleştirme](howto-integrate-azure-managed-service-identity.md) .

## <a name="app-or-function-access-to-app-configuration"></a>Uygulama yapılandırmasına uygulama veya işlev erişimi

Aşağıdaki yöntemlerden birini kullanarak Web uygulamaları veya işlevleri için uygulama yapılandırmasına erişim sağlayabilirsiniz:

* Azure portal aracılığıyla, App Service uygulama ayarlarındaki uygulama yapılandırma deponuza bağlantı dizesini girin.
* Bağlantı dizesini Key Vault, uygulama yapılandırma deponuza depolayın ve [App Service başvurun](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references).
* Uygulama yapılandırma deposuna erişmek için Azure yönetilen kimliklerini kullanın. Daha fazla bilgi için bkz. [Azure yönetilen kimliklerle tümleştirme](howto-integrate-azure-managed-service-identity.md).
* Uygulama yapılandırmasından App Service olarak yapılandırma gönderin. Uygulama yapılandırması, doğrudan App Service içine veri gönderen bir dışarı aktarma işlevi (Azure portal ve Azure CLı) sağlar. Bu yöntemle, uygulama kodunu her zaman değiştirmeniz gerekmez.

## <a name="reduce-requests-made-to-app-configuration"></a>Uygulama yapılandırmasına yapılan istekleri azaltma

Uygulama yapılandırmasına yönelik aşırı istek, azaltma veya fazla kullanım ücretlerine neden olabilir. Yapılan isteklerin sayısını azaltmak için:

* Özellikle yapılandırma değerlerinizin sık değişmeme durumunda yenileme zaman aşımını artırın. Yöntemini kullanarak yeni bir yenileme zaman aşımı belirtin. [ `SetCacheExpiration` ](/dotnet/api/microsoft.extensions.configuration.azureappconfiguration.azureappconfigurationrefreshoptions.setcacheexpiration)

* Tek tek tuşları izlemek yerine tek bir *Sentinel anahtarı*izleyin. Yalnızca Sentinel anahtarı değişirse tüm yapılandırmaları yenileyin. Bir örnek için bkz. [ASP.NET Core uygulamasında dinamik yapılandırmayı kullanma](enable-dynamic-configuration-aspnet-core.md) .

* Değişikliklerin sürekli olarak yoklanması yerine, yapılandırma değiştiğinde bildirim almak için Azure Event Grid kullanın. Daha fazla bilgi için bkz. [Azure uygulama yapılandırma olaylarını bir Web uç noktasına yönlendirme](./howto-app-configuration-event.md)

## <a name="importing-configuration-data-into-app-configuration"></a>Yapılandırma verilerini uygulama yapılandırmasına aktarma

Uygulama yapılandırması, Azure portal veya CLı kullanarak geçerli yapılandırma dosyalarınızda yapılandırma ayarlarınızı toplu olarak [içeri aktarma](https://aka.ms/azconfig-importexport1) seçeneğini sunar. Aynı seçenekleri, örneğin ilgili mağazalar arasında uygulama yapılandırmasından değerleri dışarı aktarmak için de kullanabilirsiniz. GitHub deponuz ile devam eden bir eşitleme ayarlamak isterseniz, [GitHub eylemmizi](https://aka.ms/azconfig-gha2) kullanarak, uygulama yapılandırmasının avantajlarını alırken mevcut kaynak denetimi uygulamalarınızı kullanmaya devam edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtarlar ve değerler](./concept-key-value.md)
