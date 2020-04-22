---
title: Quickstart - Azure Bahar Bulutu ile Maven'i kullanarak uygulama başlatın
description: Bu hızlı başlangıçta, Maven kullanarak örnek bir uygulama başlatın
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 11/04/2019
ms.author: brendm
ms.openlocfilehash: 4eddf34f4887f0e2bad08eb5700d83eaeae0ef7e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687085"
---
# <a name="quickstart-launch-an-azure-spring-cloud-app-using-the-maven-plug-in"></a>Hızlı başlangıç: Maven eklentisini kullanarak Bir Azure Bahar Bulutu uygulaması başlatın

Azure Bahar Bulutu Maven eklentisini kullanarak Azure Bahar Bulutu uygulamalarınızı kolayca oluşturabilir ve güncelleyebilirsiniz. Bir yapılandırmayı önceden tanımlayarak, uygulamaları mevcut Azure Bulut hizmetinize dağıtabilirsiniz. Bu makalede, bu özelliği göstermek için PiggyMetrics adlı örnek bir uygulama kullanıyorsunuz.

Bu hızlı başlangıcın ardından, nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet örneğini sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak mikro hizmetler uygulamasını klonla ve oluşturun
> * Her microservice dağıtma
> * Uygulamanız için genel bir bitiş noktası atama

>[!Note]
> Azure İlkbahar Bulutu şu anda genel önizleme olarak sunulmaktadır. Genel önizleme teklifleri, müşterilerin resmi yayınlanmalarından önce yeni özelliklerle denemeler e-sayılsa.  Genel önizleme özellikleri ve hizmetleri üretim kullanımı için değildir.  Önizlemeler sırasında destek hakkında daha fazla bilgi için lütfen [SSS'mizi](https://azure.microsoft.com/support/faq/) gözden geçirin veya daha fazla bilgi edinmek için bir [Destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) dosyalayın.


>[!TIP]
> Azure Bulut Kabuğu, bu makaledeki komutları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Git, Java Geliştirme Kiti (JDK), Maven ve Azure CLI'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız Azure [Bulut Bulutu'nu](https://shell.azure.com)başlatın. Daha fazla bilgi için Azure [Bulut BulutU'na Genel Bakış](../cloud-shell/overview.md)bölümüne bakın.

Bu hızlı başlangıcı tamamlamak için:

1. [Git'i yükleyin.](https://git-scm.com/)
2. [JDK 8'i yükleyin.](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Maven 3.0 veya sonrası yükleyin.](https://maven.apache.org/download.cgi)
4. [Ücretsiz Azure aboneliği için kaydolun.](https://azure.microsoft.com/free/)

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Azure portalında bir hizmet örneği sağlama

1. Bir web tarayıcısında, [Azure portalındaki Azure İlkbahar Bulutu'nun bu bağlantısını](https://ms.portal.azure.com/#create/Microsoft.AppPlatform)açın ve hesabınızda oturum açın.

1. Örnek uygulama için **Proje Ayrıntılarını** aşağıdaki gibi sağlayın:

    1. Uygulamanın ilişkilendirileceği **Abonelik'i** seçin.
    1. Uygulama için bir kaynak grubu seçin veya oluşturun. Yeni bir kaynak grubu oluşturmanızı öneririz.  Aşağıdaki örnekte yeni bir `myspringservice`kaynak grubu .
    1. Yeni Azure İlkbahar Bulutu hizmeti için bir ad sağlayın.  Ad 4 ile 32 karakter uzunluğunda olmalı ve yalnızca küçük harfler, sayılar ve tireler içerebilir. Hizmet adının ilk karakteri bir harf, son karakter ise bir harf veya sayı olmalıdır.  Aşağıdaki örnekteki hizmetin adı `contosospringcloud`vardır.
    1. Sağlanan seçeneklerden uygulamanız için bir konum seçin.  Bu örnekte, `East US`biz seçin.
    1. Yeni hizmetinizin özetini incelemek için **Gözden Geçir + oluşturun'u** seçin.  Her şey doğru **görünüyorsa, Oluştur'u**seçin.

    > [!div class="mx-imgBorder"]
    > ![Gözden Geçir ' i seçin + oluştur](media/maven-qs-review-create.jpg)

Hizmetin dağıtılması yaklaşık 5 dakika sürer. Hizmet dağıtıldıktan sonra **kaynağa Git'i** seçin ve hizmet örneği için **Genel Bakış** sayfası görüntülenir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-maven-quickstart&step=provision)

## <a name="set-up-your-configuration-server"></a>Yapılandırma sunucunuzu ayarlama

1. Hizmete **Genel Bakış** sayfasında **Config Server'ı**seçin.
1. Varsayılan **depo** bölümünde **https://github.com/Azure-Samples/piggymetrics-config**URI'yi **URI** ayarlayın ve değişikliklerinizi kaydetmek için **Uygula'yı** seçin.

    > [!div class="mx-imgBorder"]
    > ![Config ayarlarını tanımlama ve uygulama](media/maven-qs-apply-config.jpg)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-maven-quickstart&step=config-server)

## <a name="clone-and-build-the-sample-application-repository"></a>Örnek uygulama deposunu klonla ve oluşturun

1. Azure [Bulut Kabuğunu](https://shell.azure.com)başlatın.

1. Aşağıdaki komutu çalıştırarak Git deposunu klonla:

    ```console
    git clone https://github.com/Azure-Samples/PiggyMetrics
    ```
  
1. Dizin değiştirin ve aşağıdaki komutu çalıştırarak projeyi oluşturun:

    ```console
    cd piggymetrics
    mvn clean package -DskipTests
    ```

## <a name="generate-configurations-and-deploy-to-the-azure-spring-cloud"></a>Yapılandırmalar oluşturun ve Azure İlkbahar Bulutu'na dağıtın

1. Üst POM'u içeren PiggyMetrics'in kök klasöründe aşağıdaki komutu çalıştırarak yapılandırmalar oluşturun:

    ```console
    mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:1.1.0:config
    ```

    a. Modülleri `gateway``auth-service`seçin ve `account-service`.

    b. Aboneliğinizi ve Azure Bahar Bulutu hizmet kümenizi seçin.

    c. Sağlanan projeler listesine, genel erişim sağlamak `gateway` için karşılık gelen numarayı girin.
    
    d. Yapılandırmayı onaylayın.

1. POM artık eklenti bağımlılıklarını ve yapılandırmalarını içerir. Uygulamaları aşağıdaki komutu kullanarak dağıtın:

   ```console
   mvn azure-spring-cloud:deploy
   ```

1. Dağıtım tamamlandıktan sonra, önceki komuttan çıktıda sağlanan URL'yi kullanarak PiggyMetrics'e erişebilirsiniz.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-maven-quickstart&step=deploy)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Bir Maven deposundan bir Bahar Bulutu uygulaması dağıttınız. Azure Bahar Bulutu hakkında daha fazla bilgi edinmek için uygulamanızı dağıtıma hazırlama konusunda öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure Bahar Bulutu uygulamanızı dağıtıma hazırlayın](spring-cloud-tutorial-prepare-app-deployment.md)
> [Azure için Maven eklentileri hakkında daha fazla bilgi edinin](https://github.com/microsoft/azure-maven-plugin)

GitHub: Azure Bahar [Bulut Örnekleri'nde](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql)daha fazla örnek mevcuttur.
