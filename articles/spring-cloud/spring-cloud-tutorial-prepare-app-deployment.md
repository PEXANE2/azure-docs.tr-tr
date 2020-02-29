---
title: Öğretici-Azure yay bulutu 'nda dağıtım için bir Java Spring uygulaması hazırlama
description: Bu öğreticide, bir Java Spring uygulamasını Azure yay bulutuna dağıtım için hazırlarsınız.
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 02/03/2020
ms.author: brendm
ms.openlocfilehash: 49ebfec131c8b9fa7b8535163c03eb7cb692790d
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78200031"
---
# <a name="prepare-a-java-spring-application-for-deployment-in-azure-spring-cloud"></a>Azure yay bulutu 'nda bir Java Spring uygulamasını dağıtıma hazırlama

Bu hızlı başlangıçta, mevcut bir Java Spring uygulamasının Azure Spring Cloud 'a dağıtılması için hazırlanması gösterilmektedir. Doğru yapılandırılmışsa, Azure yay bulutu, Java Spring Cloud uygulamanızı izlemek, ölçeklendirmek ve güncelleştirmek için güçlü hizmetler sunar.

Diğer örneklerde pod dosyası yapılandırıldığında bir uygulamanın Azure Spring Cloud 'a nasıl dağıtılacağı açıklanmaktadır. 
* [Azure portal kullanarak uygulama başlatma](spring-cloud-quickstart-launch-app-portal.md)
* [Azure CLı kullanarak uygulama başlatma](spring-cloud-quickstart-launch-app-cli.md)

Bu makalede, gerekli bağımlılıklar ve bunların Pod dosyasına nasıl ekleneceği açıklanmaktadır.

## <a name="java-runtime-version"></a>Java çalışma zamanı sürümü

Azure yay bulutu 'nda yalnızca Spring/Java uygulamaları çalıştırılabilir.

Azure yay bulutu hem Java 8 hem de Java 11 ' i destekler. Barındırma ortamı, Azure için Azul Zulu OpenJDK 'nin en son sürümünü içerir. Azure için Azul Zulu OpenJDK hakkında daha fazla bilgi için bkz. [JDK 'Yi yüklemeyin](https://docs.microsoft.com/azure/java/jdk/java-jdk-install).

## <a name="spring-boot-and-spring-cloud-versions"></a>Yay önyükleme ve yay bulut sürümleri

Mevcut bir Spring Boot uygulamasını Azure yay bulutu 'na dağıtmak üzere hazırlamak için, aşağıdaki bölümlerde gösterildiği gibi, Application Pod dosyasına Spring Boot ve Spring Cloud bağımlılıklarını ekleyin.

Azure yay bulutu, Spring Boot sürüm 2,1 veya sürüm 2,2 ' nin yalnızca Spring Boot uygulamalarını destekler. Aşağıdaki tabloda desteklenen Spring Boot ve Spring Cloud kombinasyonlarını listelenmektedir:

Spring Boot sürümü | Yay bulutu sürümü
---|---
2.1 | Greenwich. RELEASE
2.2 | Hoxton. RELEASE

### <a name="dependencies-for-spring-boot-version-21"></a>Spring Boot sürüm 2,1 için bağımlılıklar

Spring Boot sürüm 2,1 için aşağıdaki bağımlılıkları uygulama Pod dosyasına ekleyin.

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.12.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Greenwich.SR4</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

### <a name="dependencies-for-spring-boot-version-22"></a>Spring Boot sürüm 2,2 için bağımlılıklar

Spring Boot sürüm 2,2 için aşağıdaki bağımlılıkları uygulama Pod dosyasına ekleyin.

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.4.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

## <a name="azure-spring-cloud-client-dependency"></a>Azure yay bulutu istemci bağımlılığı

Azure yay bulutu, bahar bulut bileşenlerini barındırır ve yönetir. Bileşenler yay bulut hizmeti kayıt defteri ve yay bulut yapılandırma sunucusu içerir. Azure Spring Cloud Service örneğiniz ile iletişime izin vermek için bağımlılıklarınızı Azure yay bulutu istemci Kitaplığı ' nı dahil edin.

Aşağıdaki tabloda, uygulamanız için Spring Boot ve Spring Cloud kullanan doğru Azure yay bulut sürümleri listelenmektedir.

Spring Boot sürümü | Yay bulutu sürümü | Azure Spring Cloud sürümü
---|---|---
2.1 | Greenwich. RELEASE | 2.1
2.2 | Hoxton. RELEASE | 2.2

Pod. xml dosyanıza aşağıdaki bağımlılıklardan birini ekleyin. Azure yay bulutu sürümü kendi ile eşleşen bağımlılığı seçin.

### <a name="dependency-for-azure-spring-cloud-version-21"></a>Azure Spring Cloud sürüm 2,1 için bağımlılık

Spring Boot sürüm 2,1 için aşağıdaki bağımlılığı uygulama Pod dosyasına ekleyin.

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.1</version>
</dependency>
```

### <a name="dependency-for-azure-spring-cloud-version-22"></a>Azure Spring Cloud sürüm 2,2 için bağımlılık

Spring Boot sürüm 2,2 için aşağıdaki bağımlılığı uygulama Pod dosyasına ekleyin.

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.2.0</version>
</dependency>
```

## <a name="other-required-dependencies"></a>Diğer gerekli bağımlılıklar

Azure Spring Cloud 'ın yerleşik özelliklerini etkinleştirmek için, uygulamanız aşağıdaki bağımlılıkları içermelidir. Bu içerme, uygulamanızın her bileşenle doğru bir şekilde yapılandırmasını sağlar.

### <a name="enablediscoveryclient-annotation"></a>EnableDiscoveryClient ek açıklaması

Uygulama kaynak koduna aşağıdaki ek açıklamayı ekleyin.
```java
@EnableDiscoveryClient
```
Örneğin, önceki örneklerden plımetriler uygulamasına bakın:
```java
package com.piggymetrics.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.zuul.EnableZuulProxy;

@SpringBootApplication
@EnableDiscoveryClient
@EnableZuulProxy

public class GatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(GatewayApplication.class, args);
    }
}
```

### <a name="service-registry-dependency"></a>Hizmet kayıt defteri bağımlılığı

Yönetilen Azure hizmeti kayıt defteri hizmetini kullanmak için, Pod. xml dosyasına `spring-cloud-starter-netflix-eureka-client` bağımlılığı aşağıda gösterildiği gibi ekleyin:

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
```

Hizmet kayıt defteri sunucusunun uç noktası, uygulamanıza ortam değişkenleri olarak otomatik olarak eklenir. Uygulamalar, kendilerini hizmet kayıt defteri sunucusuna kaydedebilir ve diğer bağımlı mikro hizmetleri bulabilir.

### <a name="distributed-configuration-dependency"></a>Dağıtılmış yapılandırma bağımlılığı

Dağıtılmış yapılandırmayı etkinleştirmek için, Pok. XML dosyanızın bağımlılıklar bölümüne aşağıdaki `spring-cloud-config-client` bağımlılığını ekleyin:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

> [!WARNING]
> Önyükleme yapılandırmanızda `spring.cloud.config.enabled=false` belirtmeyin. Aksi takdirde, uygulamanız yapılandırma sunucusu ile çalışmayı durduruyor.

### <a name="metrics-dependency"></a>Ölçüm bağımlılığı

Pod. XML dosyanızın bağımlılıklar bölümüne `spring-boot-starter-actuator` bağımlılığı aşağıda gösterildiği gibi ekleyin:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

 Ölçümler, JMX uç noktalarından düzenli aralıklarla çekilir. Azure portal kullanarak ölçümleri görselleştirebilirsiniz.

### <a name="distributed-tracing-dependency"></a>Dağıtılmış Izleme bağımlılığı

Pod. XML dosyanızın bağımlılıklar bölümüne aşağıdaki `spring-cloud-starter-sleuth` ve `spring-cloud-starter-zipkin` bağımlılıklarını ekleyin:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

 Ayrıca Azure Spring Cloud Service örneğiniz ile çalışmak için bir Azure Application Insights örneğini etkinleştirmeniz gerekir. Azure Spring Cloud ile Application Insights kullanmayı öğrenmek için [Dağıtılmış izleme öğreticisini](spring-cloud-tutorial-distributed-tracing.md) okuyun.

## <a name="see-also"></a>Ayrıca bkz.
* [Uygulama günlüklerini ve ölçümleri çözümleme](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)
* [Yapılandırma sunucunuzu ayarlama](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-config-server)
* [Azure Spring Cloud ile dağıtılmış izleme kullanma](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)
* [Yay hızlı başlangıç kılavuzu](https://spring.io/quickstart)
* [Spring Boot belgeleri](https://spring.io/projects/spring-boot)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Java Spring uygulamanızı Azure Spring Cloud 'a dağıtmak üzere nasıl yapılandıracağınızı öğrendiniz. Bir yapılandırma sunucusu örneğini ayarlamayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Bir yapılandırma sunucusu örneğini ayarlamayı öğrenin](spring-cloud-tutorial-config-server.md)

GitHub 'da daha fazla örnek vardır: [Azure Spring Cloud Samples](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples).
