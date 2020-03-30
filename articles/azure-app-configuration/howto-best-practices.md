---
title: Azure Uygulama Yapılandırması en iyi uygulamalar | Microsoft Dokümanlar
description: Azure Uygulama Yapılandırmasını en iyi nasıl kullanacağınızı öğrenin
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
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348670"
---
# <a name="azure-app-configuration-best-practices"></a>Azure Uygulama Yapılandırması en iyi uygulamaları

Bu makalede, Azure Uygulama Yapılandırması kullanırken sık karşılaşılan desenler ve en iyi uygulamalar anlatılmaktadır.

## <a name="key-groupings"></a>Anahtar gruplandırmaları

Uygulama Yapılandırması anahtarları düzenlemek için iki seçenek sağlar:

* Anahtar önekleri
* Etiketler

Anahtarlarınızı gruplandırmak için bir veya her iki seçeneği de kullanabilirsiniz.

*Anahtar önekleri* tuşların başlangıç parçalarıdır. Adlarında aynı önek kullanarak bir anahtar kümesini mantıksal olarak gruplandırma yapabilirsiniz. Önekler, `/`ad alanı oluşturmak için URL yoluna benzer şekilde bir sınır layıcıyla bağlanan birden çok bileşen içerebilir. Bu tür hiyerarşiler, birçok uygulama, bileşen hizmeti ve ortam için anahtarları tek bir Uygulama Yapılandırma deposunda depolarken yararlıdır.

Unutulmaması gereken önemli bir şey, anahtarların ilgili ayarların değerlerini almak için başvurur ne olduğudur. Anahtarlar değişmemeli, aksi takdirde her seferinde kodunuzu değiştirmeniz gerekir.

*Etiketler* anahtarlar üzerinde bir öznitelik vardır. Bir anahtarın varyantlarını oluşturmak için kullanılırlar. Örneğin, bir anahtarın birden çok sürümüne etiket atayabilirsiniz. Sürüm yineleme, ortam veya başka bir bağlamsal bilgi olabilir. Uygulamanız başka bir etiket belirterek tamamen farklı bir anahtar değer kümesi isteyebilir. Sonuç olarak, tüm önemli başvurular kodunuzda değişmeden kalır.

## <a name="key-value-compositions"></a>Anahtar değer kompozisyonları

App Configuration, birlikte depolanan tüm anahtarları bağımsız varlıklar olarak ele alırım. Uygulama Yapılandırması, anahtarlar arasında herhangi bir ilişki kurmaya veya hiyerarşilerine dayalı temel değerleri devralmaya çalışmaz. Ancak, uygulama kodunuzdaki uygun yapılandırma istiflemesiyle birleştiğinde etiketleri kullanarak birden çok anahtar kümesini toşifre edebilirsiniz.

Şimdi örneği inceleyelim. Değeri geliştirme ortamına bağlı olarak değişenenen **Asset1**adlı bir ayar olduğunu varsayalım. Boş bir etiket ve "Geliştirme" adlı bir etiket içeren "Asset1" adlı bir anahtar oluşturursunuz. İlk etikette, **Varlık1**için varsayılan değeri koyarsınız ve ikincisine "Geliştirme" için belirli bir değer koyarsınız.

Kodunuzda, önce anahtar değerlerini etiket siz alırsınız, ardından "Geliştirme" etiketiyle aynı anahtar değerleri kümesini ikinci kez alırsınız. Değerleri ikinci kez aldığınızda, anahtarların önceki değerleri üzerine yazılır. .NET Core yapılandırma sistemi, birden çok yapılandırma veri kümesini üst üste "yığınla" yapmanızı sağlar. Birden fazla kümede bir anahtar varsa, onu içeren son küme kullanılır. .NET Core gibi modern bir programlama çerçevesi yle, App Configuration'a erişmek için yerel bir yapılandırma sağlayıcısı kullanırsanız bu istifleme özelliğini ücretsiz olarak elde elabilirsiniz. Aşağıdaki kod snippet bir .NET Core uygulamasında istifleme nasıl uygulayabileceğinizi gösterir:

```csharp
// Augment the ConfigurationBuilder with Azure App Configuration
// Pull the connection string from an environment variable
configBuilder.AddAzureAppConfiguration(options => {
    options.Connect(configuration["connection_string"])
           .Select(KeyFilter.Any, LabelFilter.Null)
           .Select(KeyFilter.Any, "Development");
});
```

[Farklı ortamlar için farklı yapılandırmaları etkinleştirmek için etiketleri kullanmak](./howto-labels-aspnet-core.md) tam bir örnek sağlar.

## <a name="app-configuration-bootstrap"></a>Uygulama Yapılandırma bootstrap

Bir Uygulama Yapılandırma mağazasına erişmek için Azure portalında bulunan bağlantı dizesini kullanabilirsiniz. Bağlantı dizeleri kimlik bilgisi içerdiğinden, sır olarak kabul edilirler. Bu sırların Azure Key Vault'ta depolanması gerekir ve kodunuzu almak için Key Vault'a kimlik doğrulaması gerekir.

Daha iyi bir seçenek, Azure Etkin Dizini'nde yönetilen kimlikler özelliğini kullanmaktır. Yönetilen kimliklerle, Uygulama Yapılandırma mağazanıza erişimi önyüklemeiçin yalnızca Uygulama Yapılandırmauç Noktası URL'sine ihtiyacınız vardır. URL'yi uygulama kodunuza (örneğin, *appsettings.json* dosyasına) katıştırabilirsiniz. Ayrıntılar için [Azure yönetilen kimliklerle tümleştir'e](howto-integrate-azure-managed-service-identity.md) bakın.

## <a name="app-or-function-access-to-app-configuration"></a>Uygulama Yapılandırmasına uygulama veya işlev erişimi

Aşağıdaki yöntemlerden herhangi birini kullanarak web uygulamaları veya işlevleri için Uygulama Yapılandırması'na erişim sağlayabilirsiniz:

* Azure portalı aracılığıyla, Uygulama Hizmeti'nin Uygulama ayarlarında Uygulama Yapılandırma mağazanıza bağlantı dizesini girin.
* Bağlantı dizesini Key Vault'taki Uygulama Yapılandırma mağazanıza saklayın ve [Uygulama Hizmeti'nden başvurun.](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references)
* Uygulama Yapılandırma deposuna erişmek için Azure yönetilen kimlikleri kullanın. Daha fazla bilgi için [bkz.](howto-integrate-azure-managed-service-identity.md)
* Yapılandırmayı Uygulama Yapılandırmasından Uygulama Hizmetine itin. Uygulama Yapılandırması, verileri doğrudan Uygulama Hizmetine gönderen bir dışa aktarma işlevi (Azure portalında ve Azure CLI'de) sağlar. Bu yöntemle, uygulama kodunu hiç değiştirmeniz gerekmez.

## <a name="reduce-requests-made-to-app-configuration"></a>Uygulama Yapılandırmasına yapılan istekleri azaltın

Uygulama Yapılandırması'na yapılan aşırı istekler, azaltma veya fazla ücretlendirmeye neden olabilir. Yapılan istek sayısını azaltmak için:

* Özellikle yapılandırma değerleriniz sık sık değişmiyorsa, yenileme zaman dilimini artırın. [ `SetCacheExpiration` Yöntemi](/dotnet/api/microsoft.extensions.configuration.azureappconfiguration.azureappconfigurationrefreshoptions.setcacheexpiration)kullanarak yeni bir yenileme zaman anına belirtin.

* Tek tek anahtarları izlemek yerine tek bir *sentinel tuşunu*izleyin. Yalnızca sentinel anahtarı değişirse tüm yapılandırmayı yenileyin. Bkz. Bir örnek için [ASP.NET Core uygulamasında dinamik yapılandırmayı kullanın.](enable-dynamic-configuration-aspnet-core.md)

* Yapılandırma değiştiğinde bildirimleri almak için Azure Olay Izgarası'nı kullanın, herhangi bir değişiklik için sürekli yoklama yapmak yerine. Daha fazla bilgi [için Azure Uygulama Yapılandırma olaylarını web bitiş noktasına taşıyın](./howto-app-configuration-event.md)

## <a name="importing-configuration-data-into-app-configuration"></a>Yapılandırma verilerini Uygulama Yapılandırmasına alma

Uygulama Yapılandırması, Azure portalı veya CLI'yi kullanarak yapılandırma ayarlarınızı geçerli yapılandırma dosyalarınızdan toplu olarak [alma](https://aka.ms/azconfig-importexport1) seçeneği sunar. Uygulama Yapılandırması'ndan değerleri dışa aktarmak için de aynı seçenekleri kullanabilirsiniz, örneğin ilgili mağazalar arasında. GitHub repo'nuzla devam eden bir eşitleme ayarlamak istiyorsanız, Uygulama Yapılandırması'ndan yararlanırken mevcut kaynak denetim uygulamalarınızı kullanmaya devam etmek için [GitHub Eylemimizi](https://aka.ms/azconfig-gha2) kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtarlar ve değerler](./concept-key-value.md)
