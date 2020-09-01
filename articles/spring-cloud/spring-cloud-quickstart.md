---
title: Hızlı başlangıç - İlk Azure Spring Cloud uygulamanızı dağıtma
description: Bu hızlı başlangıçta, Azure Spring buluta bir Spring Cloud Hello uygulaması dağıdık.
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/05/2020
ms.author: brendm
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 6a7a489367aa7348a3449a72de6889476e5d38db
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89177608"
---
# <a name="quickstart-deploy-your-first-azure-spring-cloud-application"></a>Hızlı başlangıç: ilk Azure Spring Cloud uygulamanızı dağıtın

Bu hızlı başlangıçta, Azure 'da çalıştırmak üzere basit bir Azure yay bulutu mikro hizmet uygulamasının nasıl dağıtılacağı açıklanmaktadır. 

Bu öğreticide kullanılan uygulama kodu, Spring ınitialsır ile oluşturulmuş basit bir uygulamadır. Bu örneği tamamladığınızda, uygulama çevrimiçi olarak erişilebilir olur ve Azure portal aracılığıyla yönetilebilir.

Bu hızlı başlangıçta nasıl yapılacağı açıklanmaktadır:

> [!div class="checklist"]
> * Temel yay bulutu projesi oluşturma
> * Hizmet örneği sağlama
> * Uygulamayı ortak bir uç nokta ile derleyin ve dağıtın
> * Gerçek zamanlı akış günlükleri

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* [JDK 8 ' i yükler](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
* [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)
* Seçim [Azure CLI sürüm 2.0.67 veya üstünü](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve Azure yay bulutu uzantısını şu komutla birlikte yüklersiniz: `az extension add --name spring-cloud`
* Seçim [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij/) ve [oturum açmayı](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in) yükleyip

## <a name="generate-a-spring-cloud-hello-project"></a>Yay bulutu Hello projesi oluşturma

Azure Spring Cloud için önerilen bağımlılıklara sahip bir örnek proje oluşturmak üzere [Spring ınitialku](https://start.spring.io/#!type=maven-project&language=java&platformVersion=2.3.3.RELEASE&packaging=jar&jvmVersion=1.8&groupId=com.example&artifactId=hellospring&name=hellospring&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.hellospring&dependencies=web,cloud-eureka,actuator,cloud-starter-sleuth,cloud-starter-zipkin) ile başlayın. Aşağıdaki görüntüde, bu örnek proje için ayarlanan ınitialr gösterilmektedir.
```url
https://start.spring.io/#!type=maven-project&language=java&platformVersion=2.3.3.RELEASE&packaging=jar&jvmVersion=1.8&groupId=com.example&artifactId=hellospring&name=hellospring&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.hellospring&dependencies=web,cloud-eureka,actuator,cloud-starter-sleuth,cloud-starter-zipkin
```

  ![Initialkr sayfası](media/spring-cloud-quickstart-java/initializr-page.png)

1. Tüm bağımlılıklar ayarlandığında **Oluştur** ' a tıklayın. Paketi indirip paketini açın ve aşağıdaki şekilde ekleyerek basit bir Web uygulaması için bir Web denetleyicisi oluşturun `src/main/java/com/example/hellospring/HelloController.java` :

    ```java
    package com.example.hellospring;
    
    import org.springframework.web.bind.annotation.RestController;
    import org.springframework.web.bind.annotation.RequestMapping;
    
    @RestController
    public class HelloController {
    
        @RequestMapping("/")
        public String index() {
            return "Greetings from Azure Spring Cloud!";
        }
    
    }
    ```
## <a name="provision-an-instance-of-azure-spring-cloud"></a>Azure yay bulutu örneği sağlama

Aşağıdaki yordam Azure portal kullanarak bir Azure Spring Cloud örneği oluşturur.

1. Yeni bir sekmede [Azure Portal](https://ms.portal.azure.com/)açın. 

2. Üst arama kutusundan *Azure yay bulutu*' nı arayın.

3. Sonuçlardan *Azure yay bulutu* ' nı seçin.

    ![ASC simgesi başlangıç](media/spring-cloud-quickstart-launch-app-portal/find-spring-cloud-start.png)

4. Azure yay bulutu sayfasında **+ Ekle**' ye tıklayın.

    ![ASC simgesi ekle](media/spring-cloud-quickstart-launch-app-portal/spring-cloud-add.png)

5. Azure yay bulutu **Oluştur** sayfasında formu doldurun.  Aşağıdaki yönergeleri göz önünde bulundurun:
    - **Abonelik**: Bu kaynak için faturalandırılması istediğiniz aboneliği seçin.
    - **Kaynak grubu**: yeni kaynaklar için yeni kaynak grupları oluşturmak en iyi uygulamadır. Bu, sonraki adımlarda olduğu gibi kullanılacaktır **\<resource group name\>** .
    - **Hizmet Ayrıntıları/adı**: öğesini belirtin **\<service instance name\>** .  Ad 4 ila 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir.  Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.
    - **Konum**: hizmet örneğiniz için bölgeyi seçin.

    ![ASC portalı başlangıcı](media/spring-cloud-quickstart-launch-app-portal/portal-start.png)

6. **Gözden geçir ve oluştur**’a tıklayın.

## <a name="build-and-deploy-the-app"></a>Uygulama derleme ve dağıtma
    
#### <a name="cli"></a>[CLI](#tab/Azure-CLI)
Aşağıdaki yordam, Azure CLı kullanarak uygulamayı oluşturur ve dağıtır. Projenin kökünde aşağıdaki komutu yürütün.

1. Maven kullanarak projeyi derleyin:

    ```console
    mvn clean package -DskipTests
    ```

1. (Henüz yüklemediyseniz) Azure CLı için Azure yay bulutu uzantısını yükler:

    ```azurecli
    az extension add --name spring-cloud
    ```
    
1. Atanmış ortak uç nokta ile uygulamayı oluşturun:

    ```azurecli
    az spring-cloud app create -n hellospring -s <service instance name> -g <resource group name> --is-public
    ```

1. Uygulama için jar dosyasını dağıtın:

    ```azurecli
    az spring-cloud app deploy -n hellospring -s <service instance name> -g <resource group name> --jar-path target\hellospring-0.0.1-SNAPSHOT.jar
    ```
    
1. Uygulamanın dağıtım işleminin tamamlanmasının birkaç dakika sürer. Dağıtıldığını doğrulamak için Azure portal **uygulamalar** dikey penceresine gidin. Uygulamanın durumunu görmeniz gerekir.

#### <a name="intellij"></a>[IntelliJ](#tab/IntelliJ)

Aşağıdaki yordam, IntelliJ FIKRINDEN örnek uygulamayı dağıtmak üzere Azure Spring Cloud için IntelliJ eklentisini kullanır.  

### <a name="import-hello-project"></a>Merhaba projeyi içeri aktar

1. IntelliJ **hoş geldiniz** iletişim kutusunu açın ve içeri aktarma Sihirbazı 'nı açmak Için **projeyi içeri aktar** ' ı seçin.
1. `hellospring`Klasör seçin.

    ![Projeyi içeri aktar](media/spring-cloud-quickstart-java/intellij-new-project.png)

### <a name="deploy-the-app"></a>Uygulamayı dağıtma
Azure 'a dağıtmak için Azure hesabınızla oturum açmanız ve aboneliğinizi seçmeniz gerekir.  Oturum açma ayrıntıları için bkz. [yükleme ve oturum açma](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in).

1. IntelliJ Proje Gezgini ' nde projenize sağ tıklayın ve **Azure**  ->  **yay bulutu 'na Azure dağıtımı**' nı seçin.

    [![Azure 'a dağıtma 1 ](media/spring-cloud-quickstart-java/intellij-deploy-azure-1.png)](media/spring-cloud-quickstart-java/intellij-deploy-azure-1.png#lightbox)

1. **Ad** alanındaki uygulamanın adını kabul edin. **Ad** , uygulama adı değil, yapılandırmaya başvurur. Kullanıcıların genellikle bunu değiştirmesi gerekmez.
1. **Yapıt** metin kutusunda *HELLOSPRING-0.0.1-Snapshot. jar*öğesini seçin.
1. **Abonelik** metin kutusunda aboneliğinizi doğrulayın.
1. **Yay bulutu** metin kutusunda, [Azure Spring Cloud Instance sağlama](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-provision-service-instance)bölümünde oluşturduğunuz Azure Spring Cloud örneğini seçin.
1. **Genel uç noktayı** *Etkinleştir*olarak ayarlayın.
1. **Uygulama:** metin kutusunda **uygulama oluştur...** seçeneğini belirleyin.
1. *Merhaba yay*girin ve ardından **Tamam**' a tıklayın.

    [![Azure 'a dağıtma Tamam ](media/spring-cloud-quickstart-java/intellij-deploy-to-azure.png)](media/spring-cloud-quickstart-java/intellij-deploy-to-azure.png#lightbox)

1. **Azure yay bulut uygulaması dağıtma** iletişim kutusunun alt kısmındaki **Çalıştır** düğmesine tıklayarak dağıtımı başlatın. Eklenti uygulamada komutunu çalıştırır `mvn package` `hellospring` ve komut tarafından oluşturulan jar 'yi dağıtır `package` .
---

Dağıtım tamamlandıktan sonra uygulamasına ' de erişebilirsiniz `https://<service instance name>-hellospring.azuremicroservices.io/` .

  [![Tarayıcıdan ](media/spring-cloud-quickstart-java/access-app-browser.png) uygulamaya erişme](media/spring-cloud-quickstart-java/access-app-browser.png#lightbox)

## <a name="streaming-logs-in-real-time"></a>Akış günlükleri gerçek zamanlı olarak

#### <a name="cli"></a>[CLI](#tab/Azure-CLI)

Uygulamadan gerçek zamanlı günlükleri almak için aşağıdaki komutu kullanın.

```azurecli
az spring-cloud app logs -n hellospring -s <service instance name> -g <resource group name> --lines 100 -f

```
Günlükler sonuçlarda görüntülenir:

[![Akış günlükleri ](media/spring-cloud-quickstart-java/streaming-logs.png)](media/spring-cloud-quickstart-java/streaming-logs.png#lightbox)

>[!TIP]
> `az spring-cloud app logs -h`Daha fazla parametre ve günlük akışı işlevlerini araştırmak için kullanın.

#### <a name="intellij"></a>[IntelliJ](#tab/IntelliJ)

1. **Azure Gezgini**' ni ve ardından **yay bulutu**' nı seçin.
1. Çalışan uygulamaya sağ tıklayın.
1. Açılan listeden **akış günlükleri** ' ni seçin.
1. Örnek seçin.

    [![Akış günlüklerini ](media/spring-cloud-quickstart-java/intellij-get-streaming-logs.png) seçme](media/spring-cloud-quickstart-java/intellij-get-streaming-logs.png)

1. Akış günlüğü, çıkış penceresinde görünür olur.

    [![Akış günlüğü çıkışı ](media/spring-cloud-quickstart-java/intellij-streaming-logs-output.png)](media/spring-cloud-quickstart-java/intellij-streaming-logs-output.png)
---

Gelişmiş Günlükler analiz özellikleri için [Azure Portal](https://portal.azure.com/)menüsündeki **Günlükler** sekmesini ziyaret edin. Burada günlüklerde birkaç dakika gecikme vardır.

[![Günlük Analizi ](media/spring-cloud-quickstart-java/logs-analytics.png)](media/spring-cloud-quickstart-java/logs-analytics.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu portaldan silin veya Cloud Shell aşağıdaki komutu çalıştırın:
```azurecli
az group delete --name <your resource group name; for example: hellospring-1558400876966-rg> --yes
```

Bu hızlı başlangıçta şunları öğrendiniz:

> [!div class="checklist"]
> * Temel bir Azure yay bulutu projesi oluşturma
> * Hizmet örneği sağlama
> * Uygulamayı ortak bir uç nokta ile derleyin ve dağıtın
> * Akış günlükleri gerçek zamanlı olarak
## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Mikro hizmetler oluşturun ve çalıştırın](spring-cloud-quickstart-sample-app-introduction.md)

GitHub 'da daha fazla örnek vardır: [Azure Spring Cloud Samples](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
