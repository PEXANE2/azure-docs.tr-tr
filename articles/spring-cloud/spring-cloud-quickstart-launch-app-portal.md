---
title: Azure portal kullanarak bir Azure Spring Cloud uygulaması başlatın
description: Azure portal kullanarak Azure yay bulutuna örnek bir uygulama dağıtın.
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/04/2019
ms.author: v-vasuke
ms.openlocfilehash: 11e9fadc240b90ceb7e4a0e6690978dac9bae859
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255087"
---
# <a name="quickstart-launch-an-azure-spring-cloud-application-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak bir Azure Spring Cloud uygulaması başlatın

Azure Spring Cloud, Azure 'da Spring Cloud tabanlı mikro hizmet uygulamalarını kolayca çalıştırmanıza olanak sağlar.

Bu hızlı başlangıçta, mevcut bir yay bulutu uygulamasının Azure 'a nasıl dağıtılacağı gösterilir. Bu öğreticide kullanılan örnek uygulama koduna yönelik [bir bağlantı aşağıda](https://github.com/Azure-Samples/PiggyMetrics) verilmiştir. İşiniz bittiğinde, belirtilen örnek uygulama çevrimiçi olarak erişilebilir ve Azure portal aracılığıyla yönetilmeye hazırlıyordu.

Bu hızlı başlangıcı izleyerek şunları nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet örneği sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak bir mikro Hizmetler uygulaması oluşturma
> * Her mikro hizmeti dağıtma
> * Uygulamanız için genel uç nokta atama

## <a name="prerequisites"></a>Prerequisites

>[!Note]
> Bu hızlı başlangıca başlamadan önce Azure aboneliğinizin Azure Spring Cloud 'a erişimi olduğundan emin olun.  Bir önizleme hizmeti olarak, aboneliğinizin izin verilenler listenize ekleyebilmemiz için bize bize ulaşmaları istenir.  Azure Spring Cloud 'ın yeteneklerini araştırmak istiyorsanız lütfen bize e-posta ile ulaşın: azure-spring-cloud@service.microsoft.com.

>[!TIP]
> Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLı 'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, shell.azure.com adresinden [Azure Cloud Shell](https://shell.azure.com) başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Cloud Shell hakkında daha fazla bilgi edinebilirsiniz

Bu hızlı başlangıcı gerçekleştirmek için:

1. [Git 'i yükler](https://git-scm.com/)
2. [JDK 8 ' i yükler](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Maven 3,0 veya üstünü yükler](https://maven.apache.org/download.cgi)
4. [Azure CLı 'yı yükler](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Azure CLı uzantısını yükler

Aşağıdaki komutu kullanarak Azure CLı için Azure yay bulutu uzantısını yükler

```Azure CLI
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Azure portal bir hizmet örneği sağlayın

1. Bir Web tarayıcısında, [Azure Portal Azure Spring Cloud bağlantısını](https://ms.portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=AppPlatformExtension#blade/Microsoft_Azure_Marketplace/MarketplaceOffersBlade/selectedMenuItemId/home/searchQuery/Azure%20Spring%20Cloud)açın.

1. Genel Bakış sayfasına gitmek için **Azure yay bulutu** ' nı seçin. Sonra, başlamak için **Oluştur** düğmesini seçin.

1. Aşağıdaki yönergeleri göz önünde bulundurarak formu doldurun:
    - Hizmet adı: hizmet örneğinizin adını belirtin.  Ad 4 ila 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir.  Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.
    - Abonelik: Bu kaynak için faturalandırılması istediğiniz aboneliği seçin.  Bu aboneliğin Azure yay bulutu için izin verilenler listenize eklendiğinden emin olun.
    - Kaynak grubu: yeni kaynaklar için yeni kaynak grupları oluşturmak en iyi uygulamadır.
    - Konum: hizmet örneğinizin konumunu seçin. Şu anda desteklenen konumlar Doğu ABD, Batı ABD 2, Batı Avrupa ve Güneydoğu Asya 'yı içerir.
    
Hizmetin dağıtılması yaklaşık 5 dakika sürer.  Hizmet örneği için **genel bakış** sayfası, dağıtıldıktan sonra görüntülenir.

## <a name="set-up-your-configuration-server"></a>Yapılandırma sunucunuzu ayarlama

1. Hizmete **genel bakış** sayfasına gidin ve **yapılandırma sunucusu**' nu seçin.

1. **Varsayılan depo** bölümünde, **urı** 'yi "https://github.com/Azure-Samples/piggymetrics" olarak ayarlayın, **etiketi** "config" olarak ayarlayın ve değişikliklerinizi kaydetmek için **Uygula** ' yı seçin.

## <a name="build-and-deploy-microservice-applications"></a>Mikro hizmet uygulamaları oluşturma ve dağıtma

1. Örnek uygulama deposunu yerel makinenize kopyalamak için bir komut penceresi açın ve aşağıdaki komutu çalıştırın.

    ```cli
    git clone https://github.com/Azure-Samples/piggymetrics
    ```

1. Aşağıdaki komutu çalıştırarak projeyi derleyin.

    ```cli
    cd PiggyMetrics
    mvn clean package -DskipTests
    ```

1. Azure CLı 'da oturum açın ve etkin aboneliğinizi ayarlayın.

    ```cli
    # Login to Azure CLI
    az login

    # List all subscriptions
    az account list -o table

    # Set active subscription
    az account set --subscription <target subscription ID>
    ```

1. Kaynak grubunuza ve hizmetinize ad atayın. Aşağıdaki yer tutucuları, bu öğreticide daha önce sağladığınız kaynak grubu adı ve hizmet adı ile değiştirdiğinizden emin olun.

    ```azurecli
    az configure --defaults group=<resource group name>
    az configure --defaults spring-cloud=<service instance name>
    ```

1. @No__t-0 uygulaması oluşturun ve JAR dosyasını dağıtın.

    ```azurecli
    az spring-cloud app create -n gateway
    az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
    ```

1. Aynı kalıbı izleyerek `account-service` ve `auth-service` uygulamaları oluşturun ve JAR dosyalarını dağıtın.

    ```cli
    az spring-cloud app create -n account-service
    az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
    az spring-cloud app create -n auth-service
    az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
    ```

1. Uygulamaların dağıtılmasının tamamlanmasının birkaç dakika sürer. Dağıtıldığını doğrulamak için Azure portal **uygulamalar** dikey penceresine gidin. Her üç uygulamanın bir satırını görmeniz gerekir.

## <a name="assign-a-public-endpoint-to-gateway"></a>Ağ geçidine genel uç nokta atama

1. **Uygulama panosu** sayfasını açın.
2. **Uygulama ayrıntıları** sayfasını göstermek için `gateway` uygulamasını seçin.
3. Ağ geçidine bir genel uç nokta atamak için **etki alanı ata** ' yı seçin. Bu birkaç dakika sürebilir. 
4. Çalışan uygulamanızı görüntülemek için tarayıcınıza atanan ortak IP 'yi girin.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

> [!div class="checklist"]
> * Hizmet örneği sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak bir mikro Hizmetler uygulaması oluşturma
> * Her mikro hizmeti dağıtma
> * Uygulama ağ geçidiniz için genel uç nokta atama

> [!div class="nextstepaction"]
> [Azure Spring Cloud uygulamanızı dağıtıma hazırlama](spring-cloud-tutorial-prepare-app-deployment.md)
