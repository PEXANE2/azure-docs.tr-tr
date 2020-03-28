---
title: Bahar Önyükleme uygulamasında özellik bayraklarını kullanma nın öğreticisi - Azure Uygulama Yapılandırması | Microsoft Dokümanlar
description: Bu eğitimde, Bahar Önyükleme uygulamalarında özellik bayraklarını nasıl uygulayacağınızı öğrenirsiniz.
services: azure-app-configuration
documentationcenter: ''
author: mrm9084
manager: zhenlwa
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: java
ms.topic: tutorial
ms.date: 09/26/2019
ms.author: mametcal
ms.custom: mvc
ms.openlocfilehash: d519ac44d617f725aa9b3d3f11671122bd9477bc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78944322"
---
# <a name="tutorial-use-feature-flags-in-a-spring-boot-app"></a>Öğretici: Bahar Önyükleme uygulamasında özellik bayraklarını kullanma

Bahar Önyükleme Çekirdek Özellik Yönetimi kitaplıkları, Bir Bahar Önyükleme uygulamasında özellik bayraklarının uygulanması için destek sağlar. Bu kitaplıklar, kodunuza bildirimsel olarak özellik bayrakları eklemenize olanak sağlar.

Özellik Yönetimi kitaplıkları, sahne arkasında özellik bayrak yaşam döngülerini de yönetir. Örneğin, kitaplıklar yeniler ve önbellek bayrağı durumları veya bir istek çağrısı sırasında değişmez bir bayrak durumu garanti. Buna ek olarak, Bahar Önyükleme kitaplığı MVC denetleyici eylemleri, rotalar ve ara yazılım lar da dahil olmak üzere tümleştirmeler sunar.

[Bir Bahar Önyükleme uygulamasına özellik bayrakları ekle Quickstart,](./quickstart-feature-flag-spring-boot.md) Bahar Önyükleme uygulamasında özellik bayrakları eklemenin çeşitli yollarını gösterir. Bu öğretici, bu yöntemleri daha ayrıntılı olarak açıklar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Özellik kullanılabilirliğini denetlemek için uygulamanızın önemli bölümlerine özellik bayrakları ekleyin.
> * Özellik bayraklarını yönetmek için kullanırken Uygulama Yapılandırması ile tümleştirin.

## <a name="set-up-feature-management"></a>Özellik yönetimini ayarlama

Bahar Önyükleme `FeatureManager` özellik yöneticisi, çerçevenin yerel yapılandırma sisteminden özellik bayrakları alır. Sonuç olarak, yerel *bootstrap.yml* dosyası veya ortam değişkenleri de dahil olmak üzere Bahar Önyükleme'nin desteklediği herhangi bir yapılandırma kaynağını kullanarak uygulamanızın özellik bayraklarını tanımlayabilirsiniz. `FeatureManager`bağımlılık enjeksiyonuna dayanır. Özellik yönetimi hizmetlerini standart kuralları kullanarak kaydedebilirsiniz:

```java
private FeatureManager featureManager;

public HelloController(FeatureManager featureManager) {
    this.featureManager = featureManager;
}
```

Özellik bayraklarını uygulamanın dışında tutmanızı ve bunları ayrı ayrı yönetmenizi öneririz. Bunu yapmak, bayrak durumlarını istediğiniz zaman değiştirmenize ve bu değişikliklerin uygulamada hemen etkili olmasını sağlar. App Configuration, özel bir portal kullanıcı arabirimi aracılığıyla tüm özellik bayraklarınızı düzenlemek ve denetlemek için merkezi bir yer sağlar. App Configuration ayrıca bayrakları doğrudan Bahar Önyükleme istemci kitaplıkları aracılığıyla uygulamanıza sunar.

Bahar Önyükleme uygulamanızı App Configuration'a bağlamanın en kolay yolu yapılandırma sağlayıcısıdır:

### <a name="spring-cloud-11x"></a>Bahar Bulutu 1.1.x

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spring-cloud-azure-feature-management-web</artifactId>
    <version>1.1.2</version>
</dependency>
```

### <a name="spring-cloud-12x"></a>Bahar Bulutu 1.2.x

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spring-cloud-azure-feature-management-web</artifactId>
    <version>1.2.2</version>
</dependency>
```

## <a name="feature-flag-declaration"></a>Özellik bayrağı bildirimi

Her özellik bayrağının iki bölümü vardır: bir ad ve bir özelliğin durumunun *olup* olmadığını değerlendirmek için kullanılan bir `True`veya daha fazla filtrenin listesi (diğer bir süre, değeri olduğunda). Filtre, bir özelliğin ne zaman açık olması gerektiği ne zaman için bir kullanım örneği tanımlar.

Bir özellik bayrağında birden çok filtre varsa, filtrelerden biri özelliğin etkinleştirilmesi gerektiğini belirleyene kadar filtre listesi sırayla geçiş yapılır. Bu noktada, özellik bayrağı *açık*ve kalan filtre sonuçları atlanır. Hiçbir filtre özelliğinin etkinleştirilmesi gerektiğini göstermiyorsa, özellik bayrağı *kapalıdır.*

Özellik yöneticisi özellik bayrakları için bir yapılandırma kaynağı olarak *application.yml* destekler. Aşağıdaki örnek, bir YAML dosyasında özellik bayraklarının nasıl ayarlanır olduğunu gösterir:

```yml
feature-management:
  feature-set:
    feature-a: true
    feature-b: false
    feature-c:
      enabled-for:
        -
          name: Percentage
          parameters:
            value: 50
```

Kural olarak, `feature-management` bu YML belgenin bölümü özellik bayrağı ayarları için kullanılır. Önceki örnekte, `EnabledFor` özellikte tanımlı filtreleri olan üç özellik bayrağı gösterilmektedir:

* `feature-a`*üzerindedir.*
* `feature-b`*kapalı.*
* `feature-c`özelliği ile adlı `Percentage` bir `parameters` filtre belirtir. `Percentage`yapılandırılabilir bir filtredir. Bu örnekte, `Percentage` bayrağın `feature-c` *üzerinde*olması için yüzde 50'lik bir olasılık belirtir.

## <a name="feature-flag-checks"></a>Özellik bayrağı denetimleri

Özellik yönetiminin temel deseni, önce bir özellik *bayrağının açık*olarak ayarlı p'li olup olmadığını denetlemektir. Bu nedenle, özellik yöneticisi daha sonra özelliğin içerdiği eylemleri çalıştırır. Örnek:

```java
private FeatureManager featureManager;
...
if (featureManager.isEnabledAsync("feature-a").block()) {
    // Run the following code
}
```

## <a name="dependency-injection"></a>Bağımlılık ekleme

İlkbahar Önyükleme'de, özellik `FeatureManager` yöneticisine bağımlılık enjeksiyonu yoluyla erişebilirsiniz:

```java
@Controller
@ConfigurationProperties("controller")
public class HomeController {
    private FeatureManager featureManager;

    public HomeController(FeatureManager featureManager) {
        this.featureManager = featureManager;
    }
}
```

## <a name="controller-actions"></a>Denetleyici eylemleri

MVC denetleyicilerinde, belirli `@FeatureGate` bir eylemin etkin olup olmadığını denetlemek için özniteliği kullanırsınız. Aşağıdaki `Index` eylemin `feature-a` çalıştırılamadan önce *üzerinde* olması gerekir:

```java
@GetMapping("/")
@FeatureGate(feature = "feature-a")
public String index(Model model) {
    ...
}
```

Denetleyici özellik bayrağı *kapalı*olduğu için bir MVC denetleyicisi `IDisabledFeaturesHandler` veya eylem engellendiğinde, kayıtlı arabirim çağrılır. Varsayılan `IDisabledFeaturesHandler` arabirim, yanıt gövdesi olmayan istemciye 404 durum kodu döndürür.

## <a name="mvc-filters"></a>MVC filtreler

MVC filtreleri, özellik bayrağının durumuna göre etkinleştirilmelerini sağlayabilirsiniz. Aşağıdaki kod adlı `FeatureFlagFilter`bir MVC filtresi ekler. Bu filtre, yalnızca etkinleştirildiğinde `feature-a` MVC ardışık ardışık ardışık ardışık lıkta tetiklenir.

```java
@Component
public class FeatureFlagFilter implements Filter {

    @Autowired
    private FeatureManager featureManager;

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        if(!featureManager.isEnabledAsync("feature-a").block()) {
            chain.doFilter(request, response);
            return;
        }
        ...
        chain.doFilter(request, response);
    }
}
```

## <a name="routes"></a>Yollar

Yolları yönlendirmek için özellik bayraklarını kullanabilirsiniz. Aşağıdaki kod, bir kullanıcıyı `feature-a` etkinleştirildinden yönlendirecektir:

```java
@GetMapping("/redirect")
@FeatureGate(feature = "feature-a", fallback = "/getOldFeature")
public String getNewFeature() {
    // Some New Code
}

@GetMapping("/getOldFeature")
public String getOldFeature() {
    // Some New Code
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, kitaplıkları kullanarak Bahar Önyükleme uygulamanızda `spring-cloud-azure-feature-management-web` özellik bayraklarının nasıl uygulanacağını öğrendiniz. Bahar Önyükleme ve Uygulama Yapılandırmasında özellik yönetimi desteği hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Bahar Önyükleme özelliği bayrak örnek kodu](/azure/azure-app-configuration/quickstart-feature-flag-spring-boot)
* [Özellik bayraklarını yönetme](./manage-feature-flags.md)
