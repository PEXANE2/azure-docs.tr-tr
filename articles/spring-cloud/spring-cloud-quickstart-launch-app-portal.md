---
title: Hızlı başlangıç-Azure portal kullanarak mevcut bir Azure Spring Cloud uygulamasını başlatın
description: Bu hızlı başlangıçta, Azure portal kullanarak Azure Spring buluta bir yay bulutu uygulaması dağıtın.
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 02/03/2020
ms.author: brendm
ms.openlocfilehash: b65fbf7882c3ce7f6eb7e88c89eca83340ee2d05
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77251830"
---
# <a name="quickstart-launch-an-existing-azure-spring-cloud-application-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak mevcut bir Azure Spring Cloud uygulamasını başlatma

Bu hızlı başlangıçta, mevcut bir yay bulutu uygulamasının Azure 'a nasıl dağıtılacağı gösterilir. Azure Spring Cloud, Azure 'da Spring Cloud tabanlı mikro hizmet uygulamalarını kolayca çalıştırmanıza olanak sağlar. 

Bu öğreticide kullanılan örnek uygulama kodunu [GitHub örnekleri depomız](https://github.com/Azure-Samples/PiggyMetrics)bölümünde bulabilirsiniz. İşiniz bittiğinde, belirtilen örnek uygulama çevrimiçi olarak erişilebilir ve Azure portal aracılığıyla yönetilmeye hazırlıyordu.

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

```Azure CLI
az extension add --name spring-cloud
```

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Azure portal bir hizmet örneği sağlayın

1. Bir Web tarayıcısında, [Azure Portal Azure Spring Cloud bağlantısını](https://ms.portal.azure.com/#create/Microsoft.AppPlatform)açın.

1. Azure yay bulutu **Oluştur** sayfasında formu doldurun.  Aşağıdaki yönergeleri göz önünde bulundurun:
    - Hizmet adı: hizmet örneğinizin adını belirtin.  Ad 4 ila 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir.  Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.
    - Abonelik: Bu kaynak için faturalandırılması istediğiniz aboneliği seçin.  Bu aboneliğin Azure yay bulutu için izin verilenler listenize eklendiğinden emin olun.
    - Kaynak grubu: yeni kaynaklar için yeni kaynak grupları oluşturmak en iyi uygulamadır.
    - Konum: hizmet örneğinizin konumunu seçin. Şu anda desteklenen konumlar Doğu ABD, Batı ABD 2, Batı Avrupa ve Güneydoğu Asya 'yı içerir.

1. **Gözden geçir ve oluştur**' a tıklayın.

1. Belirtimlerinizi doğrulayıp **Oluştur**' a tıklayın.

>[!Note]
> Şablon doğrulamasının tamamlanmasını 3 dakikadan fazla sürerse, lütfen izlemeyi devre dışı bırakıp yeniden deneyin.

Hizmetin dağıtılması yaklaşık 5 dakika sürer.  Hizmet örneği için **genel bakış** sayfası, dağıtıldıktan sonra görüntülenir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=provision)


## <a name="set-up-your-configuration-server"></a>Yapılandırma sunucunuzu ayarlama

1. Hizmete **genel bakış** sayfasına gidin ve **yapılandırma sunucusu**' nu seçin.

1. **Varsayılan depo** bölümünde, **urı** 'yi "https://github.com/Azure-Samples/piggymetrics-config" olarak ayarlayın.

1. Değişikliklerinizi kaydetmek için **Uygula** ' yı seçin.

    ![ASC portalının ekran görüntüsü](media/spring-cloud-quickstart-launch-app-portal/portal-config.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=config-server)

## <a name="build-and-deploy-microservice-applications"></a>Mikro hizmet uygulamaları oluşturma ve dağıtma

1. Bir [Azure Cloud Shell](https://shell.azure.com) açın ve örnek uygulama deposunu yerel makinenize kopyalayın.  Burada, önce uygulamayı kopyalamadan önce `source-code` adlı geçici bir dizin oluşturacağız.

    ```azurecli
    mkdir source-code
    cd source-code
    git clone https://github.com/Azure-Samples/piggymetrics
    ```

1. Kopyalanmış paketi oluşturun.

    ```azurecli
    cd piggymetrics
    mvn clean package -DskipTests
    ```
1. Kaynak grubunuza ve hizmetinize ad atayın. Aşağıdaki yer tutucuları, bu öğreticide daha önce sağladığınız kaynak grubu adı ve hizmet adı ile değiştirdiğinizden emin olun.

    ```azurecli
    az configure --defaults group=<resource group name>
    az configure --defaults spring-cloud=<service instance name>
    ```

1. `gateway` uygulamasını oluşturun ve JAR dosyasını dağıtın.

    ```azurecli
    az spring-cloud app create -n gateway
    az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
    ```

1. Aynı kalıbı izleyerek `account-service` ve `auth-service` uygulamaları oluşturun ve JAR dosyalarını dağıtın.

    ```azurecli
    az spring-cloud app create -n account-service
    az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
    az spring-cloud app create -n auth-service
    az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
    ```

1. Uygulamaların dağıtılmasının tamamlanmasının birkaç dakika sürer. Dağıtıldığını doğrulamak için Azure portal **uygulamalar** dikey penceresine gidin. Her üç uygulamanın bir satırını görmeniz gerekir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=deploy)

## <a name="assign-a-public-endpoint-to-gateway"></a>Ağ geçidine genel uç nokta atama

1. Soldaki menüden **uygulamalar** sekmesini açın.

1. **Genel bakış** sayfasını göstermek için `gateway` uygulamayı seçin.

1. Ağ geçidine bir genel uç nokta atamak için **etki alanı ata** ' yı seçin. Bu işlem birkaç dakika sürebilir.

    ![ASC portalının ekran görüntüsü](media/spring-cloud-quickstart-launch-app-portal/portal-endpoint.png)

1. Çalışan uygulamanızı görüntülemek için tarayıcınıza atanan ortak uç noktayı ( **URL etiketli URL**) girin.

    ![ASC portalının ekran görüntüsü](media/spring-cloud-quickstart-launch-app-portal/sample-app.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=public-endpoint)

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

GitHub 'da daha fazla örnek vardır: [Azure Spring Cloud Samples](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
