---
title: Azure Uygulama yapılandırması en iyi uygulamalar | Microsoft Docs
description: Azure Uygulama yapılandırması 'nı en iyi şekilde kullanmayı öğrenin
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 3b43ca5b6bec64d9283a64c9bcc0a3b60c21bca4
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75750424"
---
# <a name="azure-app-configuration-best-practices"></a>Azure Uygulama yapılandırması en iyi uygulamaları

Bu makalede, Azure Uygulama yapılandırması 'nı kullanırken yaygın desenler ve en iyi uygulamalar ele alınmaktadır.

## <a name="key-groupings"></a>Anahtar gruplandırmaları

Uygulama yapılandırması anahtarları düzenlemek için iki seçenek sunar:

* Anahtar ön ekleri
* Etiketler

Anahtarlarınızı gruplandırmak için bir veya iki seçenekten birini kullanabilirsiniz.

*Anahtar ön ekleri* anahtarların başlangıç parçalarından oluşur. Adlarında aynı öneki kullanarak bir anahtar kümesini mantıksal olarak gruplandırabilirsiniz. Ön ekler, bir ad alanı oluşturmak için bir URL yoluna benzer `/`gibi sınırlayıcıyla bağlı birden çok bileşen içerebilir. Bu tür hiyerarşiler, tek bir uygulama yapılandırma deposundaki birçok uygulama, bileşen hizmeti ve ortam için anahtar depolarken yararlı olur.

Dikkat edilmesi gereken önemli bir şey, anahtarların ilgili ayarların değerlerini almak için uygulama kodunuzun başvurduğu şeydir. Anahtarların değişmemesi veya herhangi bir zamanda kodunuzun değiştirilmesini değiştirmeniz gerekir.

*Etiketler* anahtarlar üzerindeki bir özniteliktir. Anahtar çeşitleri oluşturmak için kullanılır. Örneğin, bir anahtarın birden çok sürümüne etiketler atayabilirsiniz. Bir sürüm yineleme, ortam ya da başka bir bağlamsal bilgi olabilir. Uygulamanız, başka bir etiket belirterek tamamen farklı bir anahtar değerleri kümesi isteyebilir. Sonuç olarak, tüm anahtar başvuruları kodunuzda değişmeden kalır.

## <a name="key-value-compositions"></a>Anahtar-değer kompozisyonları

Uygulama yapılandırması, onunla depolanan tüm anahtarları bağımsız varlıklar olarak değerlendirir. Uygulama yapılandırması, anahtarlar arasında herhangi bir ilişki çıkarması veya hiyerarşiye göre anahtar değerleri devralması denenmez. Ancak uygulama kodunuzda uygun yapılandırma yığınlayarak bağlanmış Etiketler kullanarak birden çok anahtar kümesini toplayabilirsiniz.

Bir örneğe göz atalım. Değeri geliştirme ortamına göre değişebilen **Asset1**adlı bir ayarınız olduğunu varsayalım. Boş bir etiketi ve "geliştirme" adlı bir etiketi içeren "Asset1" adlı bir anahtar oluşturun. İlk etikette, **Asset1**için varsayılan değeri yerleştirmelisiniz ve "geliştirme" için belirli bir değeri ikinciden yerleştirebilirsiniz.

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

## <a name="app-configuration-bootstrap"></a>Uygulama yapılandırması önyükleme

Bir uygulama yapılandırma deposuna erişmek için, Azure portal kullanılabilir olan bağlantı dizesini kullanabilirsiniz. Bağlantı dizeleri kimlik bilgisi bilgilerini içerdiğinden, bu değerler gizli olarak değerlendirilir. Bu gizli dizileri Azure Key Vault depolanması gerekir ve kodunuzun bunları almak için Key Vault kimlik doğrulaması gerekir.

Daha iyi bir seçenek, Azure Active Directory Yönetilen kimlikler özelliğini kullanmaktır. Yönetilen kimlikler ile, uygulama yapılandırma deponuza erişimi önyüklemek için yalnızca uygulama yapılandırma uç noktası URL 'sine ihtiyacınız vardır. URL 'YI uygulama kodunuza (örneğin, *appSettings. JSON* dosyasına) ekleyebilirsiniz. Ayrıntılar için bkz. [Azure tarafından yönetilen kimlikler Ile tümleştirme](howto-integrate-azure-managed-service-identity.md) .

## <a name="app-or-function-access-to-app-configuration"></a>Uygulama yapılandırmasına uygulama veya işlev erişimi

Aşağıdaki yöntemlerden birini kullanarak Web uygulamaları veya işlevleri için uygulama yapılandırmasına erişim sağlayabilirsiniz:

* Azure portal aracılığıyla, App Service uygulama ayarlarındaki uygulama yapılandırma deponuza bağlantı dizesini girin.
* Bağlantı dizesini Key Vault, uygulama yapılandırma deponuza depolayın ve [App Service başvurun](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references).
* Uygulama yapılandırma deposuna erişmek için Azure yönetilen kimliklerini kullanın. Daha fazla bilgi için bkz. [Azure yönetilen kimliklerle tümleştirme](howto-integrate-azure-managed-service-identity.md).
* Uygulama yapılandırmasından App Service olarak yapılandırma gönderin. Uygulama yapılandırması, doğrudan App Service içine veri gönderen bir dışarı aktarma işlevi (Azure portal ve Azure CLı) sağlar. Bu yöntemle, uygulama kodunu her zaman değiştirmeniz gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtarlar ve değerler](./concept-key-value.md)
