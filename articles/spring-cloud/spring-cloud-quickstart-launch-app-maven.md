---
title: Hızlı başlangıç-Azure Spring Cloud ile Maven kullanarak uygulama başlatma
description: Bu hızlı başlangıçta, Maven kullanarak örnek bir uygulama başlatın
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 11/04/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 95ae9c596742b075b5d78d22b925b3adf9285a7d
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89048066"
---
# <a name="quickstart-launch-an-azure-spring-cloud-app-using-the-maven-plug-in"></a>Hızlı başlangıç: Maven eklentisini kullanarak bir Azure Spring Cloud uygulaması başlatın

Azure Spring Cloud Maven eklentisini kullanarak Azure Spring Cloud uygulamalarınızı kolayca oluşturabilir ve güncelleştirebilirsiniz. Bir yapılandırmayı önceden tanımlayarak, mevcut Azure yay bulut hizmetinize uygulama dağıtabilirsiniz. Bu makalede, bu özelliği göstermek için POF ölçümleri adlı örnek bir uygulama kullanırsınız.

Bu örneği çalıştırmadan önce [temel hızlı](spring-cloud-quickstart.md)başlangıcı deneyebilirsiniz.

Bu hızlı başlangıcı izleyerek şunları nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet örneği sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Mikro hizmetler uygulamasını yerel olarak kopyalama ve derleme
> * Her mikro hizmeti dağıtma
> * Uygulamanız için genel bir uç nokta atama

>[!Note]
> Azure yay bulutu Şu anda genel önizleme olarak sunulmaktadır. Genel Önizleme teklifleri, müşterilerin resmi sürümünden önceki yeni özelliklerle deneme yapmasına olanak tanır.  Genel Önizleme özellikleri ve Hizmetleri üretim kullanımı için tasarlanmamıştır.  Önizleme sırasında destek hakkında daha fazla bilgi için lütfen [SSS](https://azure.microsoft.com/support/faq/) veya dosya dosyası [destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) inceleyerek daha fazla bilgi edinebilirsiniz.


>[!TIP]
> Azure Cloud Shell, bu makaledeki komutları çalıştırmak için kullanabileceğiniz, ücretsiz bir etkileşimli kabuktur. Git 'in en son sürümleri, Java Geliştirme Seti (JDK), Maven ve Azure CLı dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız [Azure Cloud Shell](https://shell.azure.com)başlatın. Daha fazla bilgi için bkz. [Azure Cloud Shell Genel Bakış](../cloud-shell/overview.md).

Bu hızlı başlangıcı tamamlamak için:

1. [Git 'ı yükler](https://git-scm.com/).
2. [JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)' i yükler.
3. [Maven 3,0 veya üstünü yükler](https://maven.apache.org/download.cgi).
4. [Ücretsiz bir Azure aboneliğine kaydolun](https://azure.microsoft.com/free/).

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Azure portal bir hizmet örneği sağlayın

1. Bir Web tarayıcısında, [Bu bağlantıyı Azure Portal Azure Spring Cloud 'a](https://ms.portal.azure.com/#create/Microsoft.AppPlatform)açın ve hesabınızda oturum açın.

1. Örnek uygulama için **proje ayrıntılarını** aşağıdaki gibi sağlayın:

    1. Uygulamanın ilişkilendirileceği **aboneliği** seçin.
    1. Uygulama için bir kaynak grubu seçin veya oluşturun. Yeni bir kaynak grubu oluşturmanızı öneririz.  Aşağıdaki örnekte adlı yeni bir kaynak grubu gösterilmektedir `myspringservice` .
    1. Yeni Azure yay bulut hizmeti için bir ad sağlayın.  Ad 4 ila 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir. Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.  Aşağıdaki örnekteki hizmetin adı vardır `contosospringcloud` .
    1. Belirtilen seçeneklerden uygulamanız için bir konum seçin.  Bu örnekte, ' yi seçtik `East US` .
    1. Yeni hizmetinizin özetini gözden geçirmek için **gözden geçir + oluştur** ' u seçin.  Her şey doğru görünüyorsa **Oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > ![Gözden geçir + oluştur ' u seçin](media/maven-qs-review-create.jpg)

Hizmetin dağıtılması yaklaşık 5 dakika sürer. Hizmet dağıtıldıktan sonra **Kaynağa Git** ' i seçin ve hizmet örneği Için **genel bakış** sayfası görüntülenir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-maven-quickstart&step=provision)

## <a name="set-up-your-configuration-server"></a>Yapılandırma sunucunuzu ayarlama

1. Hizmete **genel bakış** sayfasında, **yapılandırma sunucusu**' nu seçin.
1. **Varsayılan depo** bölümünde, **URI değerini** olarak ayarlayın **https://github.com/Azure-Samples/piggymetrics-config** ve ardından değişikliklerinizi kaydetmek için **Uygula** ' yı seçin.

    > [!div class="mx-imgBorder"]
    > ![Yapılandırma ayarlarını tanımlama ve uygulama](media/maven-qs-apply-config.jpg)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-maven-quickstart&step=config-server)

## <a name="clone-and-build-the-sample-application-repository"></a>Örnek uygulama deposunu kopyalama ve derleme

1. [Azure Cloud Shell](https://shell.azure.com)başlatın.

1. Aşağıdaki komutu çalıştırarak Git deposunu kopyalayın:

    ```console
    git clone https://github.com/Azure-Samples/piggymetrics
    ```
  
1. Dizini değiştirin ve aşağıdaki komutu çalıştırarak projeyi derleyin:

    ```console
    cd piggymetrics
    mvn clean package -DskipTests
    ```

## <a name="generate-configurations-and-deploy-to-the-azure-spring-cloud"></a>Azure yay bulutuna yapılandırma ve dağıtım oluşturma

1. Üst POB 'yi içeren Pumetrik ölçümleri kök klasöründe aşağıdaki komutu çalıştırarak yapılandırma oluşturun:

    ```console
    mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:1.1.0:config
    ```

    a. , Ve modüllerini `gateway` seçin `auth-service` `account-service` .

    b. Aboneliğinizi ve Azure yay bulut hizmeti kümenizi seçin.

    c. Belirtilen projeler listesinde, `gateway` ortak erişim sağlamak için öğesine karşılık gelen sayıyı girin.
    
    d. Yapılandırmayı onaylayın.

1. Pod artık eklenti bağımlılıklarını ve yapılandırmasını içerir. Aşağıdaki komutu kullanarak uygulamaları dağıtın:

   ```console
   mvn azure-spring-cloud:deploy
   ```

1. Dağıtım tamamlandıktan sonra, önceki komutun çıktısında belirtilen URL 'YI kullanarak Pudmetriye erişebilirsiniz.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-maven-quickstart&step=deploy)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Maven deposundan bir yay bulutu uygulaması dağıttınız. Azure Spring Cloud hakkında daha fazla bilgi edinmek için uygulamanızı dağıtıma hazırlama hakkında öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Spring Cloud uygulamanızı dağıtıma hazırlama](spring-cloud-tutorial-prepare-app-deployment.md) 
>  [Azure Için Maven eklentileri hakkında daha fazla bilgi edinin](https://github.com/microsoft/azure-maven-plugins)

GitHub 'da daha fazla örnek vardır: [Azure Spring Cloud Samples](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
