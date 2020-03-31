---
title: "Quickstart: Azure CLI'yi kullanarak bir Java Spring uygulaması başlatma"
description: Bu hızlı başlangıçta, Azure CLI'de Azure İlkbahar Bulutu'na örnek bir uygulama dağısınız.
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 02/15/2020
ms.author: brendm
ms.openlocfilehash: c05e53bd8ad8ade8c1e42729f46c99a0059c4dce
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79470869"
---
# <a name="quickstart-launch-a-java-spring-application-using-the-azure-cli"></a>Quickstart: Azure CLI'yi kullanarak bir Java Spring uygulaması başlatma

Azure İlkbahar Bulutu, Azure'da Bahar Önyükleme tabanlı mikro hizmetler uygulamasını kolayca çalıştırmanızı sağlar.

Bu hızlı başlangıç, varolan bir Java İlkbahar Bulutu uygulamasını Azure'a nasıl dağıtabileceğinizi gösterir. İşinizi bitirdiğinde, uygulamayı Azure CLI üzerinden yönetmeye veya Azure portalını kullanmaya devam edebilirsiniz.

Bu hızlı başlangıcın ardından, nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet örneğini sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak bir mikrohizmet uygulaması oluşturma
> * Her microservice dağıtma
> * Başvurunuz için genel bitiş noktası atama

## <a name="prerequisites"></a>Ön koşullar

>[!Note]
> Azure İlkbahar Bulutu şu anda genel önizleme olarak sunulmaktadır. Genel önizleme teklifleri, müşterilerin resmi yayınlanmalarından önce yeni özelliklerle denemeler e-sayılsa.  Genel önizleme özellikleri ve hizmetleri üretim kullanımı için değildir.  Önizlemeler sırasında destek hakkında daha fazla bilgi için lütfen [SSS'mizi](https://azure.microsoft.com/support/faq/) gözden geçirin veya daha fazla bilgi edinmek için bir [Destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) dosyalayın.

>[!TIP]
> Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLI'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, Azure [Bulut Shell'inizi](https://shell.azure.com) shell.azure.com başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Bulut BulutU Hakkında Daha Fazla Bilgi Edinebilirsiniz

Bu hızlı başlangıcı tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
2. [JDK 8'i yükleyin](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Maven 3.0 veya üzeri yükleme](https://maven.apache.org/download.cgi)
4. [Azure CLI sürümünü 2.0.67 veya daha yüksek yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Azure aboneliği için kaydolun](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Azure CLI uzantısını yükleme

Aşağıdaki komutu kullanarak Azure CLI için Azure İlkbahar Bulutu uzantısını yükleme

```azurecli
az extension add --name spring-cloud
```

## <a name="provision-a-service-instance-on-the-azure-cli"></a>Azure CLI'de bir hizmet örneği sağlama

1. Azure CLI'ye giriş yapın ve etkin aboneliğinizi seçin. Azure İlkbahar Bulutu için beyaz listeye alınan etkin aboneliği seçtiğinizden emin olun

    ```azurecli
        az login
        az account list -o table
        az account set --subscription <Name or ID of subscription from the last step>
    ```

2. Azure Bahar Bulutu hizmetiniz için bir ad hazırlayın.  Ad 4 ile 32 karakter uzunluğunda olmalı ve yalnızca küçük harfler, sayılar ve tireler içerebilir.  Hizmet adının ilk karakteri bir harf, son karakter ise bir harf veya sayı olmalıdır.

3. Azure İlkbahar Bulut hizmetinizi içerecek bir kaynak grubu oluşturun.

    ```azurecli
        az group create --location eastus --name <resource group name>
    ```

    [Azure Kaynak Grupları](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin.

4. Azure CLI penceresini açın ve Azure İlkbahar Bulutu örneğini sağlamak için aşağıdaki komutları çalıştırın.

    ```azurecli
        az spring-cloud create -n <service instance name> -g <resource group name>
    ```

    Hizmet örneğinin dağıtılması yaklaşık beş dakika sürer.

5. Varsayılan kaynak grup adınızı ve küme adınızı aşağıdaki komutları kullanarak ayarlayın:

    ```azurecli
        az configure --defaults group=<resource group name>
        az configure --defaults spring-cloud=<service instance name>
    ```

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=provision)

## <a name="setup-your-configuration-server"></a>Yapılandırma sunucunuzu ayarla

Config sunucunuzu projemiz için git deposunun konumuyla güncelleyin:

```azurecli
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/Azure-Samples/piggymetrics-config
```

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=config-server)

## <a name="build-the-microservices-applications-locally"></a>Mikro hizmetler uygulamalarını yerel olarak oluşturun

1. Yeni bir klasör oluşturun ve örnek uygulama deposunu Azure Bulut hesabınıza kopyala.  

    ```console
        mkdir source-code
        git clone https://github.com/Azure-Samples/piggymetrics
    ```

2. Dizini değiştirin ve projeyi oluşturun.

    ```console
        cd piggymetrics
        mvn clean package -D skipTests
    ```

Projenin derlenmesine yaklaşık 5 dakika sürer.  Tamamlandıktan sonra, her hizmet için kendi klasörlerinde ayrı JAR dosyaları olmalıdır.

## <a name="create-the-microservices"></a>Mikro hizmetleri oluşturun

Önceki adımda oluşturulan JAR dosyalarını kullanarak Bahar Bulutu mikro hizmetlerini oluşturun. Üç mikro hizmet oluşturacaksınız: **ağ geçidi,** **auth-service**ve **hesap hizmeti.**

```azurecli
az spring-cloud app create --name gateway
az spring-cloud app create --name auth-service
az spring-cloud app create --name account-service
```

## <a name="deploy-applications-and-set-environment-variables"></a>Uygulamaları dağıtma ve ortam değişkenlerini ayarlama

Uygulamalarımızı Azure'a dağıtmamız gerekiyor. Üç uygulamayı dağıtmak için aşağıdaki komutları kullanın:

```azurecli
az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
```

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=deploy)

## <a name="assign-public-endpoint-to-gateway"></a>Ağ geçidine genel bitiş noktası atama

Biz bir web tarayıcısı üzerinden uygulamaya erişmek için bir yol gerekir. Ağ geçidi uygulamamızın halka açık bir uç noktaya ihtiyacı var.

1. Aşağıdaki komutu kullanarak bitiş noktasını atayın:

```azurecli
az spring-cloud app update -n gateway --is-public true
```

2. Uygulamanın çalıştığını doğrulamak için ortak IP'si için **ağ geçidi** uygulamasını sorgulayın:

Linux:

```azurecli
az spring-cloud app show --name gateway | grep url
```

Windows:

```azurecli
az spring-cloud app show -s <service name> -g <resource group> -n gateway -o table
```

3. PiggyMetrics uygulamasını çalıştırmak için önceki komutun sağladığı URL'ye gidin.
    ![PiggyMetrics çalışan ekran görüntüsü](media/spring-cloud-quickstart-launch-app-cli/launch-app.png)

URL'yi bulmak için Azure portalında da gezinebilirsiniz. 
1. Hizmete gidin
2. **Uygulamaları** Seçin
3. **Ağ geçidini** seçin

    ![PiggyMetrics çalışan ekran görüntüsü](media/spring-cloud-quickstart-launch-app-cli/navigate-app1.png)
    
4. PiggyMetrics'in çalışan **ağ geçidine Genel Bakış** sayfasındaki ![URL'yi bulma](media/spring-cloud-quickstart-launch-app-cli/navigate-app2-url.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=public-endpoint)

## <a name="next-steps"></a>Sonraki Adımlar

Bu hızlı başlangıçta, Azure CLI'den bir Bahar Bulutu uygulaması dağıttınız.  Azure Bahar Bulutu hakkında daha fazla bilgi edinmek için uygulamanızı dağıtıma hazırlama eğitimine devam edin.

> [!div class="nextstepaction"]
> [Azure Bahar Bulutu uygulamanızı dağıtıma hazırlayın](spring-cloud-tutorial-prepare-app-deployment.md)

GitHub: Azure Bahar [Bulut Örnekleri'nde](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql)daha fazla örnek mevcuttur.
