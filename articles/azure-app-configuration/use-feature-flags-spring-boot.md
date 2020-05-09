---
title: Spring Boot App 'te Özellik bayraklarını kullanma öğreticisi-Azure Uygulama yapılandırması | Microsoft Docs
description: Bu öğreticide, Spring Boot uygulamalarında Özellik bayraklarını nasıl uygulayacağınızı öğreneceksiniz.
services: azure-app-configuration
documentationcenter: ''
author: mrm9084
manager: zhenlan
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: java
ms.topic: tutorial
ms.date: 09/26/2019
ms.author: mametcal
ms.custom: mvc
ms.openlocfilehash: d924975d852320fcddd5ae988f1d52f10d366f81
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82790754"
---
# <a name="tutorial-use-feature-flags-in-a-spring-boot-app"></a>Öğretici: bir Spring Boot uygulamasında Özellik bayraklarını kullanma

Spring Boot Core özelliği yönetim kitaplıkları, bir Spring Boot uygulamasında Özellik bayraklarını uygulamaya yönelik destek sağlar. Bu kitaplıklar, kodunuza bildirimli olarak özellik bayrakları eklemenize olanak tanır.

Özellik Yönetimi kitaplıkları Ayrıca arka planda özellik bayrakları yaşam döngülerini yönetir. Örneğin, kitaplıklar yenileme ve önbellek bayrak durumlarını veya bir istek çağrısı sırasında bayrak durumunun sabit olmasını garanti eder. Ayrıca, Spring Boot kitaplığı, MVC denetleyici eylemleri, rotalar ve ara yazılım gibi tümleştirmeler sunar.

[Yay önyükleme uygulaması hızlı başlangıç 'a özellik ekleme bayrakları](./quickstart-feature-flag-spring-boot.md) , bir Spring Boot uygulamasına özellik bayrakları eklemenin birkaç yolunu gösterir. Bu öğreticide bu yöntemler daha ayrıntılı olarak açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Özellik kullanılabilirliğini denetlemek için uygulamanızın önemli bölümlerine özellik bayrakları ekleyin.
> * Özellik bayraklarını yönetmek için kullandığınızda uygulama yapılandırmasıyla tümleştirin.

## <a name="set-up-feature-management"></a>Özellik yönetimini ayarlama

Spring Boot Feature Manager `FeatureManager` , Framework 'ün yerel yapılandırma sisteminden Özellik bayraklarını alır. Sonuç olarak, yerel *önyükleme. yıml* dosyası veya ortam değişkenleri dahil olmak üzere Spring Boot tarafından desteklenen herhangi bir yapılandırma kaynağını kullanarak uygulamanızın Özellik bayraklarını tanımlayabilirsiniz. `FeatureManager`bağımlılık ekleme işlemini kullanır. Özellik yönetimi hizmetlerini standart kuralları kullanarak kaydedebilirsiniz:

```java
private FeatureManager featureManager;

public HelloController(FeatureManager featureManager) {
    this.featureManager = featureManager;
}
```

Özellik bayraklarını uygulamanın dışında tutmanızı ve ayrı olarak yönetmenizi öneririz. Bunun yapılması, bayrak durumlarını dilediğiniz zaman değiştirmenize ve bu değişikliklerin uygulamada hemen etkili olmasını sağlar. Uygulama yapılandırması, özel bir Portal Kullanıcı arabirimi aracılığıyla tüm özellik Bayraklarınızı düzenlemek ve denetlemek için merkezi bir yer sağlar. Uygulama yapılandırması, bayrakları uygulamanıza doğrudan Spring Boot istemci kitaplıkları aracılığıyla da sunar.

Spring Boot uygulamanızı uygulama yapılandırmasına bağlanmanın en kolay yolu yapılandırma sağlayıcısı üzerinden yapılır:

### <a name="spring-cloud-11x"></a>Yay bulutu 1.1. x

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spring-cloud-azure-feature-management-web</artifactId>
    <version>1.1.2</version>
</dependency>
```

### <a name="spring-cloud-12x"></a>Yay bulutu 1.2. x

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spring-cloud-azure-feature-management-web</artifactId>
    <version>1.2.2</version>
</dependency>
```

## <a name="feature-flag-declaration"></a>Özellik bayrağı bildirimi

Her özellik bayrağının iki bölümü vardır: bir özelliğin durumunun *Açık* olup olmadığını değerlendirmek için kullanılan bir veya daha fazla filtrenin adı ve listesi (yani, değeri olduğunda `True`). Bir filtre, bir özelliğin açılması gerektiğinde kullanım durumunu tanımlar.

Bir özellik bayrağının birden çok filtresi olduğunda, filtrenin etkin olması gerektiğini belirlerken filtre listesine bir düzen eklenir. Bu noktada, özellik bayrağı *Açık*ve kalan filtre sonuçları atlanır. Filtre yoksa, özelliğin etkinleştirilmesi gerektiğini gösteriyorsa, özellik bayrağı *kapalıdır*.

Özellik Yöneticisi, *uygulama. yıml* 'yi özellik bayrakları için yapılandırma kaynağı olarak destekler. Aşağıdaki örnekte, bir YAML dosyasında özellik bayraklarının nasıl ayarlanacağı gösterilmektedir:

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

Kurala göre, bu `feature-management` iml belgesinin bölümü Özellik bayrağı ayarları için kullanılır. Önceki örnekte, içinde `EnabledFor` kendi filtreleri tanımlanmış olan üç özellik bayrakları gösterilmektedir:

* `feature-a`*Açık*.
* `feature-b`*kapalıdır*.
* `feature-c``parameters` özelliği olan adlı `Percentage` bir filtre belirtir. `Percentage`, yapılandırılabilir bir filtredir. Bu örnekte, `Percentage` `feature-c` bayrağın *Açık*olması için yüzde 50 olasılık değerini belirtir.

## <a name="feature-flag-checks"></a>Özellik bayrağı denetimleri

Özellik yönetiminin temel stili, ilk olarak bir özellik bayrağının *Açık*olarak ayarlanmış olup olmadığını denetme. Bu durumda, Özellik Yöneticisi özelliğin içerdiği eylemleri çalıştırır. Örneğin:

```java
private FeatureManager featureManager;
...
if (featureManager.isEnabledAsync("feature-a").block()) {
    // Run the following code
}
```

## <a name="dependency-injection"></a>Bağımlılık ekleme

Spring Boot 'da, bağımlılık ekleme aracılığıyla Özellik yöneticisine `FeatureManager` erişebilirsiniz:

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

MVC denetleyicileri ' nde, belirli bir `@FeatureGate` eylemin etkin olup olmadığını denetlemek için özniteliğini kullanırsınız. Aşağıdaki `Index` eylemin çalıştırılabilmesi için `feature-a` önce *Açık* olması gerekir:

```java
@GetMapping("/")
@FeatureGate(feature = "feature-a")
public String index(Model model) {
    ...
}
```

Denetim özelliği bayrağı *kapalı*olduğu IÇIN bir MVC denetleyicisi veya eylemi engellendiğinde, kayıtlı `IDisabledFeaturesHandler` bir arabirim çağırılır. Varsayılan `IDisabledFeaturesHandler` arabirim, yanıt gövdesi olmayan istemciye 404 durum kodu döndürür.

## <a name="mvc-filters"></a>MVC filtreleri

MVC filtrelerini bir özellik bayrağının durumuna göre etkinleştirilecek şekilde ayarlayabilirsiniz. Aşağıdaki kod adlı `FeatureFlagFilter`bir MVC filtresi ekler. Bu filtre, yalnızca `feature-a` etkinse MVC işlem hattı içinde tetiklenir.

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

Yolları yeniden yönlendirmek için özellik bayraklarını kullanabilirsiniz. Aşağıdaki kod bir kullanıcıyı ' dan `feature-a` yeniden yönlendirecektir:

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

Bu öğreticide, `spring-cloud-azure-feature-management-web` kitaplıkları kullanarak Spring Boot uygulamanızda nasıl özellik bayrakları uygulayacağınızı öğrendiniz. Spring Boot ve App Configuration içindeki özellik yönetimi desteği hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Yay önyükleme özelliği bayrağı örnek kodu](/azure/azure-app-configuration/quickstart-feature-flag-spring-boot)
* [Özellik bayraklarını yönetme](./manage-feature-flags.md)
