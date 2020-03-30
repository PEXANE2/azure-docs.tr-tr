---
title: Azure Uygulama Yapılandırması'nı kullanarak özellik yönetimini anlama
description: Azure Uygulama Yapılandırması'nı kullanarak özellikleri açıp kapatma
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 8227810c154078fc8424b2cadd373394d07e9730
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523739"
---
# <a name="feature-management-overview"></a>Özellik yönetimine genel bakış

Geleneksel olarak, yeni bir uygulama özelliği nakliye uygulamanın kendisi tam bir yeniden dağıtım gerektirir. Bir özelliği sınaması genellikle uygulamanın birden çok dağıtımını gerektirir.  Her dağıtım özelliği değiştirebilir veya sınama için farklı müşterilere özelliği ortaya çıkarabilir.  

Özellik yönetimi, özellik yayımını kod dağıtımından ayıran ve isteğe bağlı özellik kullanılabilirliği için hızlı değişiklikler sağlayan modern bir yazılım geliştirme uygulamasıdır. Bir özelliğin yaşam döngüsünü dinamik olarak yönetmek için *özellik bayrakları* *(özellik geçişleri,* *özellik anahtarları*vb. olarak da bilinir) adı verilen bir teknik kullanır.

Özellik yönetimi, geliştiricilerin aşağıdaki sorunları gidermesine yardımcı olur:

* **Kod şube yönetimi**: Şu anda geliştirilmekte olan yeni uygulama işlevselliğini sarmak için özellik bayraklarını kullanın. Bu tür işlevler varsayılan olarak "gizlidir". Tamamlanmamış olsa bile özelliği güvenli bir şekilde sevk edebilirsiniz ve üretimde hareketsiz kalır. *Karanlık dağıtım*olarak adlandırılan bu yaklaşımı kullanarak, her geliştirme döngüsünün sonunda tüm kodunuzu serbest bırakabilirsiniz. Belirli bir özelliğin tamamlanması için birden fazla döngü gerektirdiğinden, artık birden çok geliştirme döngüsünde kod dallarını korumanız gerekmez.
* **Üretimde Test**: Üretimdeki yeni işlevlere erken erişim sağlamak için özellik bayraklarını kullanın. Örneğin, ekip üyelerine veya dahili beta sınayıcılara erişimi sınırlandırabilirsiniz. Bu kullanıcılar, bir test ortamında simüle edilmiş veya kısmi bir deneyim yerine tam doğruluküretim deneyimini deneyimleyecekler.
* **Uçuş**: Son kullanıcılara yeni işlevleri aşamalı olarak kullanıma çıkarmak için özellik bayraklarını kullanın. Önce kullanıcı popülasyonunuzun küçük bir yüzdesini hedefleyebilir ve bu yüzdeyi zaman içinde kademeli olarak artırabilirsiniz.
* **Anında öldürme anahtarı**: Özellik bayrakları, yeni işlevleri serbest bırakmak için doğal bir güvenlik ağı sağlar. Uygulama özelliklerini herhangi bir kodu yeniden dağıtmadan açıp kapatabilirsiniz. Gerekirse, uygulamanızı yeniden oluşturmadan ve yeniden dağıtmadan bir özelliği hızla devre dışı kullanabilirsiniz.
* **Seçici etkinleştirme**: Kullanıcılarınızı segmente etmek ve her gruba belirli bir özellik kümesi sunmak için özellik bayraklarını kullanın. Yalnızca belirli bir web tarayıcısı üzerinde çalışan bir özelliğiniz olabilir. Yalnızca bu tarayıcının kullanıcılarının özelliği görebilmeleri ve kullanabilmeleri için bir özellik bayrağı tanımlayabilirsiniz. Bu yaklaşımla, desteklenen tarayıcı listesini daha sonra herhangi bir kod değişikliği yapmak zorunda kalmadan kolayca genişletebilirsiniz.

## <a name="basic-concepts"></a>Temel kavramlar

Özellik yönetimiyle ilgili birkaç yeni terim şunlardır:

* **Özellik bayrağı**: Özellik *bayrağı,* ikili durumu kapalı veya *kapalı*olan bir değişkendir. Özellik bayrağı da ilişkili bir kod bloğu vardır. Özellik bayrağının durumu, kod bloğunun çalışıp çalıştırmadığını tetikler.
* Özellik yöneticisi : **Özellik yöneticisi,** bir uygulamadaki tüm özellik bayraklarının yaşam döngüsünü işleyen bir uygulama paketidir. Özellik yöneticisi, özellik bayraklarını önbelleğe alma ve durumlarını güncelleştirme gibi ek işlevler de sağlar.
* **Filtre**: Filtre, özellik bayrağının durumunu değerlendirmek için bir kuraldır. Olası filtreler kullanıcı gruplarını, aygıt veya tarayıcı türlerini, coğrafi konumları ve zaman pencerelerini içerir.

Özellik yönetiminin etkin bir şekilde uygulanması, birlikte çalışan en az iki bileşenden oluşur:

* Özellik bayraklarını kullanan bir uygulama.
* Özellik bayraklarını ve geçerli durumlarını depolayan ayrı bir depo.

## <a name="using-feature-flags-in-your-code"></a>Kodunuzda özellik bayraklarını kullanma

Bir uygulamada özellik bayrakları uygulamak için temel desen basittir. Özellik bayrağı, kodunuzda koşullu bir ifadeyi kontrol eden bir Boolean durum değişkenidir:

```csharp
if (featureFlag) {
    // Run the following code
}
```

Statik değerini ayarlayabilirsiniz. `featureFlag`

```csharp
bool featureFlag = true;
```

Bayrağın durumunu belirli kurallara göre değerlendirebilirsiniz:

```csharp
bool featureFlag = isBetaUser();
```

Koşullu uygulama davranışını her iki durum için de ayarlamak için uzatabilirsiniz:

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
} else {
    // This following code will run if the featureFlag value is false
}
```

## <a name="feature-flag-repository"></a>Özellik bayrak deposu

Özellik bayraklarını etkili bir şekilde kullanmak için, bir uygulamada kullanılan tüm özellik bayraklarını haricileştirmeniz gerekir. Bu, uygulamanın kendisini değiştirmeden ve yeniden dağıtmadan özellik bayrağı durumlarını değiştirmenize olanak tanır.

Azure Uygulama Yapılandırması, özellik bayrakları için merkezi bir depo sağlar. Farklı özellik bayrakları tanımlamak ve durumlarını hızlı ve güvenli bir şekilde işlemek için kullanabilirsiniz. Daha sonra uygulamanızdan bu özellik bayraklarına kolayca erişmek için çeşitli programlama dili çerçeveleri için Uygulama Yapılandırma kitaplıklarını kullanabilirsiniz.

[ASP.NET Core uygulamasında özellik bayraklarını kullanmak,](./use-feature-flags-dotnet-core.md) ASP.NET web uygulamanız için özellik bayraklarını uygulamak için .NET Core App Configuration sağlayıcısı ve Özellik Yönetimi kitaplıklarının birlikte nasıl kullanıldığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core web uygulamasına özellik bayrakları ekleme](./quickstart-feature-flag-aspnet-core.md)  
