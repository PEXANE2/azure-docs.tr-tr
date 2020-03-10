---
title: 'Hızlı başlangıç: Azure CLı kullanarak bir Java Spring uygulaması başlatma'
description: Bu hızlı başlangıçta, Azure CLı üzerinde Azure Spring Cloud 'a örnek bir uygulama dağıtırsınız.
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 02/15/2020
ms.author: brendm
ms.openlocfilehash: 48d05dad45a5ff4c561f492e424b53c918998c7c
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78945451"
---
# <a name="quickstart-launch-a-java-spring-application-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak bir Java Spring uygulaması başlatma

Azure Spring Cloud, Azure 'da bir Spring Boot tabanlı mikro hizmetler uygulamasını kolayca çalıştırmanıza olanak sağlar.

Bu hızlı başlangıçta, mevcut bir Java Spring Cloud uygulamasının Azure 'a nasıl dağıtılacağı gösterilir. İşiniz bittiğinde, uygulamayı Azure CLı aracılığıyla yönetmeye devam edebilir veya Azure portal kullanarak geçiş yapabilirsiniz.

Bu hızlı başlangıcı izleyerek şunları nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet örneği sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak bir mikro Hizmetler uygulaması oluşturma
> * Her mikro hizmeti dağıtma
> * Uygulamanız için genel uç nokta atama

## <a name="prerequisites"></a>Önkoşullar

>[!Note]
> Azure yay bulutu Şu anda genel önizleme olarak sunulmaktadır. Genel Önizleme teklifleri, müşterilerin resmi sürümünden önceki yeni özelliklerle deneme yapmasına olanak tanır.  Genel Önizleme özellikleri ve Hizmetleri üretim kullanımı için tasarlanmamıştır.  Önizleme sırasında destek hakkında daha fazla bilgi için lütfen [SSS](https://azure.microsoft.com/support/faq/) veya dosya dosyası [destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) inceleyerek daha fazla bilgi edinebilirsiniz.

>[!TIP]
> Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLı 'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, shell.azure.com adresinden [Azure Cloud Shell](https://shell.azure.com) başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Cloud Shell hakkında daha fazla bilgi edinebilirsiniz

Bu hızlı başlangıcı tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
2. [JDK 8 ' i yükler](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Maven 3,0 veya üstünü yükler](https://maven.apache.org/download.cgi)
4. [Azure CLı sürüm 2.0.67 veya üstünü yükler](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Azure CLı uzantısını yükler

Aşağıdaki komutu kullanarak Azure CLı için Azure yay bulutu uzantısını yükler

```azurecli
az extension add --name spring-cloud
```

## <a name="provision-a-service-instance-on-the-azure-cli"></a>Azure CLı üzerinde bir hizmet örneği sağlama

1. Azure CLı 'da oturum açın ve etkin aboneliğinizi seçin. Azure yay bulutu için beyaz listeye eklenen etkin aboneliği seçtiğinizden emin olun

    ```azurecli
        az login
        az account list -o table
        az account set --subscription <Name or ID of subscription from the last step>
    ```

2. Azure yay bulut hizmetiniz için bir ad hazırlayın.  Ad 4 ila 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir.  Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.

3. Azure yay bulut hizmetinizi içeren bir kaynak grubu oluşturun.

    ```azurecli
        az group create --location eastus --name <resource group name>
    ```
    [Azure Kaynak Grupları](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin.

4. Bir Azure CLı penceresi açın ve Azure Spring Cloud 'ın bir örneğini sağlamak için aşağıdaki komutları çalıştırın.

    ```azurecli
        az spring-cloud create -n <service instance name> -g <resource group name>
    ```

    Hizmet örneğinin dağıtılması yaklaşık beş dakika sürer.

5. Aşağıdaki komutları kullanarak varsayılan kaynak grubu adınızı ve küme adınızı ayarlayın:

    ```azurecli
        az configure --defaults group=<resource group name>
        az configure --defaults spring-cloud=<service instance name>
    ```

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=provision)

## <a name="setup-your-configuration-server"></a>Yapılandırma sunucunuzu kurun

Yapılandırma-sunucunuzu, projemizin git deposunun konumuyla güncelleştirin:

```azurecli
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/Azure-Samples/piggymetrics-config
```

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=config-server)

## <a name="build-the-microservices-applications-locally"></a>Mikro hizmet uygulamalarını yerel olarak oluşturma

1. Yeni bir klasör oluşturun ve örnek uygulama deposunu Azure bulut hesabınıza kopyalayın.  

    ```azurecli
        mkdir source-code
        git clone https://github.com/Azure-Samples/piggymetrics
    ```

2. Dizini değiştirin ve projeyi derleyin.

    ```azurecli
        cd piggymetrics
        mvn clean package -D skipTests
    ```

Projenin derlenmesi yaklaşık 5 dakika sürer.  Tamamlandıktan sonra, ilgili klasörlerinde her bir hizmet için bireysel JAR dosyaları olmalıdır.

## <a name="create-the-microservices"></a>Mikro hizmetleri oluşturma

Önceki adımda oluşturulan JAR dosyalarını kullanarak yay bulutu mikro hizmetleri oluşturun. Üç mikro hizmet oluşturacaksınız: **Gateway**, **AUTH-Service**ve **Account-Service**.

```azurecli
az spring-cloud app create --name gateway
az spring-cloud app create --name auth-service
az spring-cloud app create --name account-service
```

## <a name="deploy-applications-and-set-environment-variables"></a>Uygulamaları dağıtma ve ortam değişkenlerini ayarlama

Uygulamalarımızı gerçekten Azure 'a dağıtmamız gerekiyor. Üç uygulamayı da dağıtmak için aşağıdaki komutları kullanın:

```azurecli
az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
```

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=deploy)

## <a name="assign-public-endpoint-to-gateway"></a>Ağ geçidine genel uç nokta ata

Bir Web tarayıcısı aracılığıyla uygulamaya erişmek için bir yönteme ihtiyacımız var. Ağ Geçidi uygulamamız, herkese açık bir uç nokta gerektirir.

1. Aşağıdaki komutu kullanarak bitiş noktasını atayın:

```azurecli
az spring-cloud app update -n gateway --is-public true
```
2. Uygulamanın çalıştığını doğrulayabilmeniz için **ağ geçidi** uygulamasını ortak IP 'si için sorgulayın:

Linux:
```azurecli
az spring-cloud app show --name gateway | grep url
```
Windows:
```azurecli
az spring-cloud app show -s <service name> -g <resource group> -n gateway -o table
```
3. Plımetrimetrik uygulamasını çalıştırmak için önceki komutun sunduğu URL 'ye gidin.
    ](media/spring-cloud-quickstart-launch-app-cli/launch-app.png) çalıştıran Pıof ölçümlerinin ekran görüntüsünü ![

URL 'YI bulmak için Azure portal de gidebilirsiniz. 
1. Hizmete git
2. **Uygulama** seçin
3. **Ağ geçidini** seçin

    ![Çalıştırılan POF ölçümleri ekran görüntüsü](media/spring-cloud-quickstart-launch-app-cli/navigate-app1.png)
    
4. **Ağ geçidine genel bakış** sayfasında URL 'yi ![, çalışan pıof ölçümlerinin ekran görüntüsünü bulun](media/spring-cloud-quickstart-launch-app-cli/navigate-app2-url.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=public-endpoint)

## <a name="next-steps"></a>Sonraki Adımlar

Bu hızlı başlangıçta, Azure CLı 'dan bir yay bulutu uygulaması dağıttık.  Azure Spring Cloud hakkında daha fazla bilgi edinmek için uygulamanızı dağıtıma hazırlama öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Azure Spring Cloud uygulamanızı dağıtıma hazırlama](spring-cloud-tutorial-prepare-app-deployment.md)

GitHub 'da daha fazla örnek vardır: [Azure Spring Cloud Samples](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
