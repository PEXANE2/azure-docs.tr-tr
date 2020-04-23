---
title: Azure Uygulama Yapılandırmasını nasıl kullanacağınızı öğrenmek için hızlı başlangıç
description: Java Spring uygulamalarıyla Azure Uygulama Yapılandırması'nı kullanmak için hızlı bir başlangıç.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 04/18/2020
ms.author: lcozzens
ms.openlocfilehash: 69b9733f4d79f5fc42fe86ca0f8a97305205aeef
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085068"
---
# <a name="quickstart-create-a-java-spring-app-with-azure-app-configuration"></a>Quickstart: Azure Uygulama Yapılandırması ile Bir Java Bahar uygulaması oluşturun

Bu hızlı başlangıçta, uygulama ayarlarının kodunuzdan ayrı olarak depolanmasını ve yönetimini merkezileştirmek için Azure Uygulama Yapılandırmasını bir Java Spring uygulamasına dahil edersiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- Sürüm 8 ile desteklenen [bir Java Geliştirme Kiti (JDK).](https://docs.microsoft.com/java/azure/jdk)
- [Apache Maven](https://maven.apache.org/download.cgi) sürüm 3.0 veya üzeri.

## <a name="create-an-app-configuration-store"></a>Uygulama Yapılandırma mağazası oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki anahtar değeri çiftleri eklemek için **Configuration Explorer** > **+ Create** > **Key-value'ı** seçin:

    | Anahtar | Değer |
    |---|---|
    | /application/config.message | Hello |

    **Etiket** ve **İçerik Türünü** şimdilik boş bırakın.

7. **Uygula**’yı seçin.

## <a name="create-a-spring-boot-app"></a>Bahar Önyükleme uygulaması oluşturma

Yeni bir Bahar Önyükleme projesi oluşturmak için [Bahar Başlatıcısı'nı](https://start.spring.io/) kullanın.

1. <https://start.spring.io/> adresine gidin.

1. Aşağıdaki seçenekleri belirtin:

   - **Java** ile bir **Maven** projesi oluşturun.
   - 2.0'a eşit veya daha büyük bir **Bahar Önyükleme** sürümü belirtin.
   - Uygulamanız için **Grup** ve **Yapıt** adlarını belirtin.
   - Bahar **Web** bağımlılığını ekleyin.

1. Önceki seçenekleri belirttikten sonra **Project Oluştur'u**seçin. İstendiğinde, projeyi yerel bilgisayarınızda bir yola indirin.

## <a name="connect-to-an-app-configuration-store"></a>Uygulama Yapılandırma mağazasına bağlanma

1. Yerel sisteminizdeki dosyaları ayıkladıktan sonra, basit Bahar Önyükleme uygulamanız düzenlemeye hazırdır. *Pom.xml* dosyasını uygulamanızın kök dizininde bulun.

1. Bir metin düzenleyicisinde *pom.xml* dosyasını açın ve Spring Cloud Azure `<dependencies>`Config başlatıcısını aşağıdakiler listesine ekleyin:

    **Bahar Bulutu 1.1.x**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-appconfiguration-config</artifactId>
        <version>1.1.2</version>
    </dependency>
    ```

    **Bahar Bulutu 1.2.x**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-appconfiguration-config</artifactId>
        <version>1.2.2</version>
    </dependency>
    ```

1. Uygulamanızın paket dizininde *MessageProperties.java* adında yeni bir Java dosyası oluşturun. Aşağıdaki satırları ekleyin:

    ```java
    package com.example.demo;

    import org.springframework.boot.context.properties.ConfigurationProperties;

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

1. Uygulamanızın paket dizininde *HelloController.java* adında yeni bir Java dosyası oluşturun. Aşağıdaki satırları ekleyin:

    ```java
    package com.example.demo;

    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class HelloController {
        private final MessageProperties properties;

        public HelloController(MessageProperties properties) {
            this.properties = properties;
        }

        @GetMapping
        public String getMessage() {
            return "Message: " + properties.getMessage();
        }
    }
    ```

1. Ana uygulama Java dosyasını `@EnableConfigurationProperties` açın ve bu özelliği etkinleştirmek için ekleyin.

    ```java
    import org.springframework.boot.context.properties.EnableConfigurationProperties;

    @SpringBootApplication
    @EnableConfigurationProperties(MessageProperties.class)
    public class DemoApplication {
        public static void main(String[] args) {
            SpringApplication.run(DemoApplication.class, args);
        }
    }
    ```

1. Uygulamanızın kaynak `bootstrap.properties` dizininin altında yeni bir dosya oluşturun ve aşağıdaki satırları dosyaya ekleyin. Örnek değerleri Uygulama Yapılandırma mağazanız için uygun özelliklerle değiştirin.

    ```CLI
    spring.cloud.azure.appconfiguration.stores[0].connection-string= ${APP_CONFIGURATION_CONNECTION_STRING}
    ```

1. **APP_CONFIGURATION_CONNECTION_STRING**adlı bir ortam değişkeni ayarlayın ve Uygulama Yapılandırma mağazanızın erişim anahtarına ayarlayın. Komut satırında, aşağıdaki komutu çalıştırın ve değişikliğin etkili olmasını sağlamak için komut istemini yeniden başlatın:

    ```cmd
    setx APP_CONFIGURATION_CONNECTION_STRING "connection-string-of-your-app-configuration-store"
    ```

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

    ```azurepowershell
    $Env:APP_CONFIGURATION_CONNECTION_STRING = "connection-string-of-your-app-configuration-store"
    ```

    macOS veya Linux kullanıyorsanız, aşağıdaki komutu çalıştırın:

    ```cmd
    export APP_CONFIGURATION_CONNECTION_STRING='connection-string-of-your-app-configuration-store'
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. Bahar Önyükleme uygulamanızı Maven ile oluşturun ve örneğin çalıştırın:

    ```cmd
    mvn clean package
    mvn spring-boot:run
    ```

2. Uygulamanız çalışmaya devam ettikten sonra, örneğin uygulamanızı test etmek için *bukle'yi* kullanın:

      ```cmd
      curl -X GET http://localhost:8080/
      ```

    Uygulama Yapılandırma sıcağında girdiğiniz iletiyi görürsünüz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir Uygulama Yapılandırma mağazası oluşturdunuz ve java spring uygulamasıyla kullandınız. Daha fazla bilgi için [Azure'da Bahar'a](https://docs.microsoft.com/java/azure/spring-framework/)bakın. Java Spring uygulamanızın yapılandırma ayarlarını dinamik olarak nasıl yenileyebildiğini öğrenmek için bir sonraki eğitime devam edin.

> [!div class="nextstepaction"]
> [Dinamik yapılandırmayı etkinleştirme](./enable-dynamic-configuration-java-spring-app.md)
