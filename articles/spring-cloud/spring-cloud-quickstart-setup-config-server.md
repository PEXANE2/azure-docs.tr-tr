---
title: Hızlı başlangıç-Azure yay bulut yapılandırması sunucusunu ayarlama
description: Uygulama dağıtımı için Azure Spring Cloud config Server 'ın kurulumunu açıklar.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/03/2020
ms.custom: devx-track-java
ms.openlocfilehash: 47f74b551919177b13f5a72d6eedeae3c77b9f14
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88951915"
---
# <a name="quickstart-set-up-azure-spring-cloud-configuration-server"></a>Hızlı başlangıç: Azure yay bulut yapılandırması sunucusunu ayarlama

Azure Spring Cloud config Server, dağıtılmış sistemler için merkezi bir yapılandırma hizmetidir. Şu anda yerel depolama, git ve alt sürümü destekleyen bir takılabilir depo katmanı kullanır.  Mikro hizmet uygulamalarını Azure Spring Cloud 'a dağıtmak için yapılandırma sunucusunu ayarlayın.

## <a name="prerequisites"></a>Önkoşullar

* [JDK 8 ' i yükler](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
* [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)
* Seçim [Azure CLI sürüm 2.0.67 veya üstünü yükleyip](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Azure Spring Cloud uzantısını şu komutla birlikte yüklersiniz: `az extension add --name spring-cloud`
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

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Uygulama oluşturma ve dağıtma](spring-cloud-quickstart-deploy-apps.md)
