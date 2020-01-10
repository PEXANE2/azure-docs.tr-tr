---
title: Azure uygulama yapılandırma özelliği yönetimi
description: Azure uygulama yapılandırmasının, isteğe bağlı uygulama özelliklerini açmak ve kapatmak için nasıl kullanılabileceği hakkında genel bakış.
author: yegu-ms
ms.author: yegu
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 04/19/2019
ms.openlocfilehash: 48aebfe1ba6af348e98e5600075f3a9e9dce1a8e
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/26/2019
ms.locfileid: "75495763"
---
# <a name="feature-management-overview"></a>Özellik yönetimine genel bakış

Geleneksel olarak, yeni bir uygulama özelliği teslim etmek, uygulamanın kendisi için tamamen yeniden dağıtımı gerektirir. Bir özelliği test etmek için uygulamanızı birçok kez dağıtmanız gerekir. Bu özellik, özelliğin görünür hale geldiğini veya onu görmek için ne zaman olacağını denetleyebilir.

Özellik Yönetimi, özellik yayınını kod dağıtımından ayrışar ve isteğe bağlı olarak özellik kullanılabilirliğine hızlı değişiklikler sağlayan modern bir yazılım geliştirme uygulamasıdır. Özelliğin yaşam döngüsünü dinamik olarak yönetmek için *özellik bayrakları* ( *özellik* *geçişleri, özellik anahtarları*vb. olarak da bilinir) adlı bir teknik kullanır.

Özellik Yönetimi, geliştiricilerin aşağıdaki sorunlarla ilgilenmelerine yardımcı olur:

* **Kod dalı yönetimi**: özellik bayrakları, geliştirme aşamasındaki yeni uygulama işlevselliğini sarmak için sıkça kullanılır. Bu işlevsellik varsayılan olarak "gizlidir". Tamamlanmamış olsa bile özelliği güvenle gönderebilirsiniz ve üretimde devre dışı kalır. *Koyu dağıtım*olarak adlandırılan bu yaklaşımı kullanarak, her bir geliştirme döngüsünün sonunda tüm kodunuzu serbest bırakabilirsiniz. Bir özellik için birden fazla döngü alınması gerektiğinden, daha fazla döngü genelinde herhangi bir kod dalını sürdürmenize gerek kalmaz.
* **Üretimde test**: Özellik bayraklarını, üretimde yeni işlevlere erken erişim sağlamak için kullanabilirsiniz. Örneğin, bu erişimi yalnızca takım üyelerinizle veya iç beta sınayıcılarına sınırlayabilirsiniz. Bu kullanıcılar, bir test ortamında benzetimli veya kısmi bir deneyim yerine tam uygunlukta üretim deneyimi alır.
* **Fışıklandırma**: yeni işlevselliği son kullanıcılara artımlı olarak almak için özellik bayraklarını de kullanabilirsiniz. Uygulamanızda daha fazla güven kazandıktan sonra, önce Kullanıcı popülasyonunun küçük bir yüzdesini hedefleyebilir ve zaman içinde giderek bu yüzdeyi artırabilirsiniz.
* **Anında sonlandırma anahtarı**: özellik bayrakları, yeni işlevselliği serbest bırakmak için devralınmış bir güvenlik ağı sağlar. Bunlarla, uygulama özelliklerini kolay bir şekilde açıp kapatabilirsiniz. Gerekirse, uygulamanızı yeniden derlemek ve yeniden dağıtmak zorunda kalmadan bir özelliği hızlıca devre dışı bırakabilirsiniz.
* **Seçmeli etkinleştirme**: çoğu özellik bayrağı yalnızca ilişkili işlevleri başarıyla yayımlanana kadar mevcut olsa da, bazıları uzun bir süre için en fazla olabilir. Özellik bayraklarını kullanarak kullanıcılarınızı segmentlere ayırın ve her gruba belirli bir özellik kümesi sunun. Örneğin, yalnızca belirli bir Web tarayıcısında çalışacak bir özelliğe sahip olabilirsiniz. Özellik bayrağını, yalnızca bu tarayıcı kullanıcılarının özelliği görebilmesi ve kullanabilmesi için tanımlayabilirsiniz. Bu yaklaşımda, daha sonra herhangi bir kod değişikliği yapmak zorunda kalmadan desteklenen tarayıcı listesini kolayca genişletebilirsiniz.

## <a name="basic-concepts"></a>Temel kavramlar

Özellik yönetimiyle ilgili birkaç yeni terim aşağıda verilmiştir:

* **Özellik bayrağı**: Özellik bayrağı, ikili durumu *Açık* veya *kapalı*olan bir değişkendir. Özellik bayrağının ayrıca ilişkili bir kod bloğu vardır. Özellik bayrağının durumu, kod bloğunun çalışıp çalışmadığını tetikler.
* **Özellik Yöneticisi**: bir özellik Yöneticisi, bir uygulamadaki tüm özellik bayraklarının yaşam döngüsünü işleyen bir uygulama paketidir. Özellik Yöneticisi genellikle özellik bayraklarını önbelleğe alma ve durumlarını güncelleştirme gibi ek işlevler sağlar.
* **Filtre**: filtre, bir özellik bayrağının durumunu değerlendirmek için bir kuraldır. Bir Kullanıcı grubu, cihaz veya tarayıcı türü, coğrafi konum ve zaman penceresi, bir filtrenin temsil edilebilmesi için örnek olarak verilebilir.

Özellik yönetiminin etkili bir şekilde uygulanması, Concert üzerinde çalışan en az iki bileşenden oluşur:

* Özellik bayraklarını kullanan bir uygulama.
* Özellik bayraklarını ve bunların geçerli durumlarını depolayan ayrı bir depo.

Bu bileşenlerin etkileşimi aşağıdaki örneklerde gösterilmiştir.

## <a name="feature-flag-usage-in-code"></a>Koddaki Özellik bayrağı kullanımı

Bir uygulamada Özellik bayraklarını uygulamaya yönelik temel model basittir. Kodunuzda bir `if` Conditional ifadesiyle kullanılan bir Boole durum değişkeni olarak bir özellik bayrağını düşünebilirsiniz:

```csharp
if (featureFlag) {
    // Run the following code
}
```

Bu durumda, `featureFlag` `True`olarak ayarlanırsa, eklenen kod bloğu yürütülür; Aksi takdirde, atlanır. `featureFlag` değerini, aşağıdaki kod örneğinde olduğu gibi statik olarak ayarlayabilirsiniz:

```csharp
bool featureFlag = true;
```

Ayrıca, bayrağın durumunu belirli kurallara göre de değerlendirebilirsiniz:

```csharp
bool featureFlag = isBetaUser();
```

Biraz daha karmaşık bir özellik bayrağı, bir `else` ifadesini de içerir:

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
} else {
    // This following code will run if the featureFlag value is false
}
```

Ancak, bu davranışı temel düzende yeniden yazabilirsiniz. [ASP.NET Core uygulamasında Özellik bayraklarını kullanma](./use-feature-flags-dotnet-core.md) , basit bir kod düzeniyle standartlaştırın avantajlarını gösterir. Örneğin:

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
}

if (!featureFlag) {
    // This following code will run if the featureFlag value is false
}
```

## <a name="feature-flag-repository"></a>Özellik bayrağı deposu

Özellik bayraklarını etkin bir şekilde kullanmak için, bir uygulamada kullanılan tüm özellik bayraklarını externalize gerekir. Bu yaklaşım, uygulamanın kendisini değiştirmeden ve yeniden dağıtmaya gerek kalmadan Özellik bayrağı durumlarını değiştirmenize olanak sağlar.

Azure Uygulama yapılandırması, özellik bayrakları için merkezi bir depo olacak şekilde tasarlanmıştır. Farklı özellik bayraklarını tanımlamak ve durumlarını hızla ve güvenle işlemek için kullanabilirsiniz. Bundan sonra, bu özellik bayraklarını uygulamanızdan kolayca erişmek için çeşitli programlama dilleri çerçeveleri için uygulama yapılandırma kitaplıklarını kullanabilirsiniz.

[ASP.NET Core uygulamasında Özellik bayraklarını kullanma](./use-feature-flags-dotnet-core.md) , .NET Core uygulama yapılandırma sağlayıcısı ve özellik yönetimi kitaplıklarının, ASP.NET Web uygulamanıza yönelik özellik bayraklarını uygulamak için birlikte nasıl kullanıldığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulamasına özellik bayrakları ekleme](./quickstart-feature-flag-aspnet-core.md)  
