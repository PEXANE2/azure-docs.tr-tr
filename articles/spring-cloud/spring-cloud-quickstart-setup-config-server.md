---
title: Hızlı başlangıç-Azure yay bulut yapılandırması sunucusunu ayarlama
description: Uygulama dağıtımı için Azure Spring Cloud config Server 'ın kurulumunu açıklar.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 09/08/2020
ms.custom: devx-track-java
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: 503eb4bf23c66ca8a9a73c32327f466721024b26
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91326142"
---
# <a name="quickstart-set-up-azure-spring-cloud-configuration-server"></a>Hızlı başlangıç: Azure yay bulut yapılandırması sunucusunu ayarlama

Azure Spring Cloud config Server, dağıtılmış sistemler için merkezi bir yapılandırma hizmetidir. Şu anda yerel depolama, git ve alt sürümü destekleyen bir takılabilir depo katmanı kullanır. Bu hızlı başlangıçta, bir git deposundan veri almak için yapılandırma sunucusunu ayarlarsınız.

::: zone pivot="programming-language-csharp"

## <a name="prerequisites"></a>Önkoşullar

* Bu serideki önceki hızlı başlangıcı doldurun: [Azure Spring Cloud Service sağla](spring-cloud-quickstart-provision-service-instance.md).

## <a name="azure-spring-cloud-config-server-procedures"></a>Azure yay bulut yapılandırması sunucu yordamları

Aşağıdaki komutu çalıştırarak yapılandırma-sunucunuzu projenin git deposunun konumuyla ayarlayın. `<service instance name>`Daha önce oluşturduğunuz hizmetin adıyla değiştirin. Önceki hızlı başlangıçta ayarladığınız hizmet örneği adı için varsayılan değer bu komutla birlikte çalışmaz.

```azurecli
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples --search-paths steeltoe-sample/config
```

Bu komut yapılandırma sunucusuna, örnek uygulama deposunun [steeltoe-Sample/config](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/steeltoe-sample/config) klasöründe yapılandırma verilerini bulmasını söyler. Yapılandırma verilerini alacak uygulamanın adı olduğundan `planet-weather-provider` , kullanılacak dosya [Planet-Weather-Provider. yıml](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/blob/master/steeltoe-sample/config/planet-weather-provider.yml)olacaktır.

::: zone-end

::: zone pivot="programming-language-java"
Azure Spring Cloud config Server, dağıtılmış sistemler için merkezi bir yapılandırma hizmetidir. Şu anda yerel depolama, git ve alt sürümü destekleyen bir takılabilir depo katmanı kullanır.  Mikro hizmet uygulamalarını Azure Spring Cloud 'a dağıtmak için yapılandırma sunucusunu ayarlayın.

## <a name="prerequisites"></a>Önkoşullar

* [JDK 8 ' i yükler](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable&preserve-view=true)
* [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)
* Seçim [Azure CLI sürüm 2.0.67 veya üstünü yükleyip](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) Azure Spring Cloud uzantısını şu komutla birlikte yüklersiniz: `az extension add --name spring-cloud`
* Seçim [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij/) ve [oturum açmayı](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in) yükleyip

## <a name="azure-spring-cloud-config-server-procedures"></a>Azure yay bulut yapılandırması sunucu yordamları

#### <a name="portal"></a>[Portal](#tab/Azure-portal)

Aşağıdaki yordam, [Pılem ölçümleri örneğini](spring-cloud-quickstart-sample-app-introduction.md)dağıtmak için Azure Portal kullanarak yapılandırma sunucusunu ayarlar.

1. Hizmete **genel bakış** sayfasına gidin ve **yapılandırma sunucusu**' nu seçin.

2. **Varsayılan depo** bölümünde, **URI** 'yi "" olarak ayarlayın https://github.com/Azure-Samples/piggymetrics-config .

3. Yaptığınız değişiklikleri kaydetmek için **Apply**'ı (Uygula) seçin.

    ![ASC portalının ekran görüntüsü](media/spring-cloud-quickstart-launch-app-portal/portal-config.png)

#### <a name="cli"></a>[CLI](#tab/Azure-CLI)

Aşağıdaki yordam, [Pılem ölçümleri örneğini](spring-cloud-quickstart-sample-app-introduction.md)dağıtmak üzere yapılandırma sunucusunu ayarlamak IÇIN Azure CLI 'yi kullanır.

Yapılandırma-sunucunuzu proje için git deposunun konumuyla ayarlayın:

```azurecli
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/Azure-Samples/piggymetrics-config
```

---
::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, abonelikleriniz varsa ücretlendirmeye devam edecek Azure kaynakları oluşturdunuz. Sonraki hızlı başlangıca devam etmeyi düşünmüyorsanız bkz. [Kaynakları Temizleme](spring-cloud-quickstart-logs-metrics-tracing.md#clean-up-resources). Aksi takdirde, sonraki hızlı başlangıca ilerleyin:

> [!div class="nextstepaction"]
> [Uygulama oluşturma ve dağıtma](spring-cloud-quickstart-deploy-apps.md)
