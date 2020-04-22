---
title: Azure Uygulama Yapılandırması ile Bahar Önyüklemesine özellik bayrakları eklemek için hızlı başlangıç
description: Azure Uygulama Yapılandırması'nı kullanarak Bahar Önyükleme uygulamalarına özellik bayrakları ekleyin ve bunları yönetin
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 04/18/2020
ms.author: lcozzens
ms.openlocfilehash: e6dc51250987e0282530209ffa13e52d6e75aa9c
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687357"
---
# <a name="quickstart-add-feature-flags-to-a-spring-boot-app"></a>Hızlı başlangıç: Bahar Önyükleme uygulamasına özellik bayrakları ekleme

Bu hızlı başlangıçta, özellik yönetiminin uçtan uca uygulanmasını oluşturmak için Azure Uygulama Yapılandırmasını Bahar Önyükleme web uygulamasına dahil eleştirirsiniz. Tüm özellik bayraklarınızı merkezi olarak depolamak ve durumlarını denetlemek için Uygulama Yapılandırma hizmetini kullanabilirsiniz.

Bahar Önyükleme Özellik Yönetimi kitaplıkları, kapsamlı özellik bayrak desteğiyle çerçeveyi genişletir. Bu kitaplıkların azure kitaplıkları üzerinde bir bağımlılığı **yoktur.** Bahar Önyükleme yapılandırma sağlayıcısı aracılığıyla App Configuration ile sorunsuz bir şekilde entegre olurlar.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* Sürüm 8 ile desteklenen [Bir Java Geliştirme Kiti SDK.](https://docs.microsoft.com/java/azure/jdk)
* [Apache Maven](https://maven.apache.org/download.cgi) sürüm 3.0 veya üzeri.

## <a name="create-an-app-configuration-instance"></a>Uygulama Yapılandırma örneği oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. **Özellik Yöneticisi** > **+Ekle** adlı bir `Beta`özellik bayrağı eklemek için seçin.

    > [!div class="mx-imgBorder"]
    > ![Beta adlı özellik bayrağını etkinleştirme](media/add-beta-feature-flag.png)

    Şimdilik `label` tanımsız bırak.

## <a name="create-a-spring-boot-app"></a>Bahar Önyükleme uygulaması oluşturma

Yeni bir Bahar Önyükleme projesi oluşturmak için [Bahar Başlatıcısı'nı](https://start.spring.io/) kullanın.

1. <https://start.spring.io/> adresine gidin.

1. Aşağıdaki seçenekleri belirtin:

   * **Java** ile bir **Maven** projesi oluşturun.
   * 2.0'a eşit veya daha büyük bir **Bahar Önyükleme** sürümü belirtin.
   * Uygulamanız için **Grup** ve **Yapıt** adlarını belirtin.  Bu makalede `com.example` `demo`kullanır ve .
   * Bahar **Web** bağımlılığını ekleyin.

1. Önceki seçenekleri belirttikten sonra **Project Oluştur'u**seçin. İstendiğinde, projeyi yerel bilgisayarınıza indirin.

## <a name="add-feature-management"></a>Özellik yönetimi ekleme

1. Yerel sisteminizdeki dosyaları ayıkladıktan sonra, Bahar Önyükleme uygulamanız düzenlemeye hazırdır. Uygulamanızın kök dizininde *pom.xml'i* bulun.

1. Bir metin düzenleyicisi *içinde pom.xml* dosyasını açın `<dependencies>`ve aşağıdakileri listeye ekleyin:

    **Bahar Bulutu 1.1.x**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-appconfiguration-config</artifactId>
        <version>1.1.2</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-feature-management-web</artifactId>
        <version>1.1.2</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    ```

    **Bahar Bulutu 1.2.x**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-appconfiguration-config</artifactId>
        <version>1.2.2</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-feature-management-web</artifactId>
        <version>1.2.2</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    ```

> [!Note]
> Bahar-web'e bağımlı olmayan bir web Özellik Yönetimi Kitaplığı vardır. Farklılıklar için GitHub'ın [belgelerine](https://github.com/microsoft/spring-cloud-azure/tree/master/spring-cloud-azure-feature-management) bakın.

## <a name="connect-to-an-app-configuration-store"></a>Uygulama Yapılandırma mağazasına bağlanma

1. Uygulamanızın `resources` dizinine gidin ve `bootstrap.properties`açın.  Dosya yoksa, dosyayı oluşturun. Aşağıdaki satırı dosyaya ekleyin.

    ```properties
    spring.cloud.azure.appconfiguration.stores[0].connection-string= ${APP_CONFIGURATION_CONNECTION_STRING}
    ```

1. Config mağazanızın Uygulama Yapılandırma portalında `Access keys` kenar çubuğundan seçim yapın. Salt okunur tuşlar sekmesini seçin. Birincil bağlantı dizesinin değerini kopyalayın.

1. Değişken adını `APP_CONFIGURATION_CONNECTION_STRING`kullanarak birincil bağlantı dizesini ortam değişkeni olarak ekleyin.

1. Ana uygulama Java dosyasını `@EnableConfigurationProperties` açın ve bu özelliği etkinleştirmek için ekleyin.

    ```java
    package com.example.demo;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.context.properties.ConfigurationProperties;
    import org.springframework.boot.context.properties.EnableConfigurationProperties;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    @EnableConfigurationProperties(MessageProperties.class)
    public class DemoApplication {

        public static void main(String[] args) {
            SpringApplication.run(DemoApplication.class, args);
        }
    }
    ```

1. Uygulamanızın paket dizininde *MessageProperties.java* adında yeni bir Java dosyası oluşturun.

    ```java
    package com.example.demo;

    import org.springframework.boot.context.properties.ConfigurationProperties;
    import org.springframework.context.annotation.Configuration;

    @Configuration
    @ConfigurationProperties(prefix = "config")
    public class MessageProperties {
        private String message;

        public String getMessage() {
            return message;
        }

        public void setMessage(String message) {
            this.message = message;
        }
    }
    ```

1. Uygulamanızın paket dizininde *HelloController.java* adında yeni bir Java dosyası oluşturun.

    ```java
    package com.example.demo;

    import org.springframework.boot.context.properties.ConfigurationProperties;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;

    import com.microsoft.azure.spring.cloud.feature.manager.FeatureManager;
    import org.springframework.web.bind.annotation.GetMapping;


    @Controller
    @ConfigurationProperties("controller")
    public class HelloController {

        private FeatureManager featureManager;

        public HelloController(FeatureManager featureManager) {
            this.featureManager = featureManager;
        }

        @GetMapping("/welcome")
        public String mainWithParam(Model model) {
            model.addAttribute("Beta", featureManager.isEnabledAsync("Beta").block());
            return "welcome";
        }
    }
    ```

1. Uygulamanızın şablon dizinde *welcome.html* adlı yeni bir HTML dosyası oluşturun.

    ```html
    <!DOCTYPE html>
    <html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <title>Feature Management with Spring Cloud Azure</title>

        <link rel="stylesheet" href="/css/main.css">
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">

        <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
        <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>

    </head>
    <body>
        <header>
        <!-- Fixed navbar -->
        <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
            <a class="navbar-brand" href="#">TestFeatureFlags</a>
            <button class="navbar-toggler" aria-expanded="false" aria-controls="navbarCollapse" aria-label="Toggle navigation" type="button" data-target="#navbarCollapse" data-toggle="collapse">
            <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarCollapse">
            <ul class="navbar-nav mr-auto">
                <li class="nav-item active">
                <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
                </li>
                <li class="nav-item" th:if="${Beta}">
                <a class="nav-link" href="#">Beta</a>
                </li>
                <li class="nav-item">
                <a class="nav-link" href="#">Privacy</a>
                </li>
            </ul>
            </div>
        </nav>
        </header>
        <div class="container body-content">
            <h1 class="mt-5">Welcome</h1>
            <p>Learn more about <a href="https://github.com/microsoft/spring-cloud-azure/blob/master/spring-cloud-azure-feature-management/README.md">Feature Management with Spring Cloud Azure</a></p>

        </div>
        <footer class="footer">
            <div class="container">
            <span class="text-muted">&copy; 2019 - Projects</span>
        </div>

        </footer>
    </body>
    </html>

    ```

1. Altında `static` ve içinde *Main.css*adlı yeni bir CSS dosyası CSS adlı yeni bir klasör oluşturun.

    ```css
    html {
     position: relative;
     min-height: 100%;
    }
    body {
     margin-bottom: 60px;
    }
    .footer {
     position: absolute;
     bottom: 0;
     width: 100%;
     height: 60px;
     line-height: 60px;
     background-color: #f5f5f5;
    }

    body > .container {
     padding: 60px 15px 0;
    }

    .footer > .container {
     padding-right: 15px;
     padding-left: 15px;
    }

    code {
     font-size: 80%;
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. Spring Boot uygulamanızı Maven’le oluşturun ve çalıştırın.

    ```shell
    mvn clean package
    mvn spring-boot:run
    ```

1. Tarayıcı penceresi açın ve URL'ye `http://localhost:8080/welcome`gidin: .

    ![Quickstart uygulaması yerel başlatmak](./media/quickstarts/spring-boot-feature-flag-local-before.png)

1. Uygulama Yapılandırma portalında **Özellik Yöneticisi'ni**seçin ve **Beta** anahtarının durumunu **On**olarak değiştirin:

    | Anahtar | Durum |
    |---|---|
    | Beta | Açık |

1. Yeni yapılandırma ayarlarını görmek için tarayıcı sayfasını yenileyin.

    ![Quickstart uygulaması yerel başlatmak](./media/quickstarts/spring-boot-feature-flag-local-after.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir Uygulama Yapılandırma mağazası oluşturdunuz ve [Özellik Yönetimi kitaplıkları](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla Bahar Önyükleme web uygulamasındaki özellikleri yönetmek için kullandınız.

* [Özellik yönetimi](./concept-feature-management.md)hakkında daha fazla bilgi edinin.
* [Özellik bayraklarını yönetin.](./manage-feature-flags.md)
* [Bahar Önyükleme Core uygulamasında özellik bayraklarını kullanın.](./use-feature-flags-spring-boot.md)
