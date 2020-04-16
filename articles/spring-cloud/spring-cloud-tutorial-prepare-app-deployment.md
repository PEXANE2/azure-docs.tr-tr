---
title: Nasıl Kullanılır - Azure Bahar Bulutu'nda dağıtım için Java Spring uygulaması hazırlama
description: Bu konuda, Azure Bahar Bulutu'na dağıtım için bir Java Bahar uygulaması hazırlarsınız.
author: bmitchell287
ms.service: spring-cloud
ms.topic: how-to
ms.date: 02/03/2020
ms.author: brendm
ms.openlocfilehash: 025dbc56fb46ef5b6225d35564b8e4ac3c82e6e3
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414455"
---
# <a name="prepare-a-java-spring-application-for-deployment-in-azure-spring-cloud"></a>Azure Bahar Bulutu'nda dağıtım için java spring uygulaması hazırlama

Bu konu, varolan bir Java Bahar uygulamasının Azure Bahar Bulutu'na dağıtım için nasıl hazırlanacağını gösterir. Azure İlkbahar Bulutu, doğru şekilde yapılandırılırsa, Java Bahar Bulutu uygulamanızı izlemek, ölçeklendirmek ve güncellemek için güçlü hizmetler sağlar.

Diğer örnekler, POM dosyası yapılandırıldığında bir uygulamanın Azure İlkbahar Bulutu'na nasıl dağıtılanınca açıklanır. 
* [Azure portalını kullanarak Uygulamayı Başlat](spring-cloud-quickstart-launch-app-portal.md)
* [Azure CLI'yi kullanarak Uygulamayı Başlat](spring-cloud-quickstart-launch-app-cli.md)

Bu makalede, gerekli bağımlılıkları ve bunları POM dosyasına nasıl ekleyeceğiniz açıklanmaktadır.

## <a name="java-runtime-version"></a>Java Runtime sürümü

Azure Bahar Bulutu'nda yalnızca Bahar/Java uygulamaları çalıştırılabilir.

Azure Yay Bulutu hem Java 8 hem de Java 11'i destekler. Barındırma ortamı, Azure için Azul Zulu OpenJDK'nın en son sürümünü içerir. Azure için Azul Zulu OpenJDK hakkında daha fazla bilgi için [JDK'yı yükleyin' e](https://docs.microsoft.com/azure/java/jdk/java-jdk-install)bakın.

## <a name="spring-boot-and-spring-cloud-versions"></a>Bahar Önyükleme ve Bahar Bulutu sürümleri

Azure Bahar Bulutu'na dağıtım için varolan bir Bahar Önyükleme uygulamasını hazırlamak için, aşağıdaki bölümlerde gösterildiği gibi uygulama POM dosyasındaki Bahar Önyükleme ve Bahar Bulutu bağımlılıklarını ekleyin.

Azure İlkbahar Bulutu, yalnızca Bahar Önyükleme uygulamalarını destekler veya Bahar Önyükleme sürüm 2.1 veya sürüm 2.2'dir. Aşağıdaki tabloda desteklenen Bahar Önyükleme ve Bahar Bulutu kombinasyonları listelenir:

Bahar Önyükleme sürümü | Bahar Bulutu sürümü
---|---
2.1 | Greenwich.RELEASE
2,2 | Hoxton.RELEASE

### <a name="dependencies-for-spring-boot-version-21"></a>İlkbahar Önyükleme sürüm 2.1 için bağımlılıklar

İlkbahar Önyükleme sürüm 2.1 için uygulama POM dosyasına aşağıdaki bağımlılıkları ekleyin.

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

### <a name="dependencies-for-spring-boot-version-22"></a>İlkbahar Önyükleme sürüm 2.2 için bağımlılıklar

İlkbahar Önyükleme sürüm 2.2 için uygulama POM dosyasına aşağıdaki bağımlılıkları ekleyin.

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

## <a name="azure-spring-cloud-client-dependency"></a>Azure Bulut istemci bağımlılığı

Azure Bahar Bulutu, Bahar Bulutu bileşenlerini barındırıyor ve yönetiyor. Bileşenler arasında Spring Cloud Service Registry ve Spring Cloud Config Server bulunmaktadır. Azure İlkbahar Bulutu hizmet örneğinizle iletişime izin vermek için bağımlılıklarınıza Azure İlkbahar Bulutu istemci kitaplığını ekleyin.

Aşağıdaki tabloda, uygulamanız için Bahar Önyükleme ve Bahar Bulutu kullanan doğru Azure Bahar Bulutu sürümleri listeleniz.

Bahar Önyükleme sürümü | Bahar Bulutu sürümü | Azure Bahar Bulutu sürümü
---|---|---
2.1 | Greenwich.RELEASE | 2.1
2,2 | Hoxton.RELEASE | 2,2

Pom.xml dosyanıza aşağıdaki bağımlılıklardan birini ekleyin. Azure İlkbahar Bulutu sürümü yle eşleşen bağımlılığı seçin.

### <a name="dependency-for-azure-spring-cloud-version-21"></a>Azure İlkbahar Bulutu sürümü 2.1 için bağımlılık

İlkbahar Önyükleme sürüm 2.1 için uygulama POM dosyasına aşağıdaki bağımlılığı ekleyin.

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.1</version>
</dependency>
```

### <a name="dependency-for-azure-spring-cloud-version-22"></a>Azure İlkbahar Bulutu sürümü 2.2 için bağımlılık

İlkbahar Önyükleme sürüm 2.2 için uygulama POM dosyasına aşağıdaki bağımlılığı ekleyin.

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.2.0</version>
</dependency>
```

## <a name="other-required-dependencies"></a>Diğer gerekli bağımlılıklar

Azure İlkbahar Bulutu'nun yerleşik özelliklerini etkinleştirmek için uygulamanızın aşağıdaki bağımlılıkları içermesi gerekir. Bu ekleme, uygulamanızın kendisini her bileşenle doğru şekilde yapılandırması sağlar.

### <a name="enablediscoveryclient-annotation"></a>DiscoveryClient ek açıklamasını etkinleştirme

Uygulama kaynak koduna aşağıdaki ek açıklamayı ekleyin.
```java
@EnableDiscoveryClient
```
Örneğin, önceki örneklerden piggymetrics uygulamasına bakın:
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

### <a name="service-registry-dependency"></a>Hizmet Kayıt Defteri bağımlılığı

Yönetilen Azure Hizmet Kayıt Defteri hizmetini `spring-cloud-starter-netflix-eureka-client` kullanmak için, burada gösterildiği gibi pom.xml dosyasındaki bağımlılığı ekleyin:

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
```

Hizmet Kayıt Defteri sunucusunun bitiş noktası, uygulamanızla ortam değişkeni olarak otomatik olarak enjekte edilir. Uygulamalar kendilerini Hizmet Kayıt Defteri sunucusuna kaydedebilir ve diğer bağımlı mikro hizmetleri keşfedebilir.

### <a name="distributed-configuration-dependency"></a>Dağıtılmış Yapılandırma bağımlılığı

Dağıtılmış Yapılandırmayı etkinleştirmek `spring-cloud-config-client` için, pom.xml dosyanızın bağımlılıklar bölümüne aşağıdaki bağımlılıkları ekleyin:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

> [!WARNING]
> Bootstrap yapılandırmanızda belirtmeyin. `spring.cloud.config.enabled=false` Aksi takdirde, uygulamanız Config Server ile çalışmayı durdurur.

### <a name="metrics-dependency"></a>Ölçümler bağımlılığı

Burada `spring-boot-starter-actuator` gösterildiği gibi pom.xml dosyanızın bağımlılıklar bölümüne bağımlılık ekleyin:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

 Ölçümler periyodik olarak JMX uç noktalarından çekilir. Azure portalını kullanarak ölçümleri görselleştirebilirsiniz.

### <a name="distributed-tracing-dependency"></a>Dağıtılmış İzleme bağımlılığı

Pom.xml dosyanızın bağımlılıklar bölümüne aşağıdaki `spring-cloud-starter-sleuth` leri ve `spring-cloud-starter-zipkin` bağımlılıkları ekleyin:

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

 Ayrıca, Azure İlkbahar Bulutu hizmet örneğinizle çalışmak için bir Azure Uygulama Öngörüleri örneğini etkinleştirmeniz gerekir. Azure Bahar Bulutu ile Uygulama Öngörüleri'ni nasıl kullanacağınızı öğrenmek için [dağıtılmış izleme yle ilgili öğreticiyi](spring-cloud-tutorial-distributed-tracing.md) okuyun.

## <a name="see-also"></a>Ayrıca bkz.
* [Uygulama günlüklerini ve ölçümlerini analiz edin](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)
* [Yapılandırma Sunucunuzu ayarlama](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-config-server)
* [Azure İlkbahar Bulutu ile dağıtılmış izleme yi kullanma](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)
* [Bahar Quickstart Kılavuzu](https://spring.io/quickstart)
* [Bahar Önyükleme belgeleri](https://spring.io/projects/spring-boot)

## <a name="next-steps"></a>Sonraki adımlar

Bu konuda, Azure Bahar Bulutu'na dağıtım için Java Spring uygulamanızı nasıl yapılandırabileceğinizi öğrendiniz. Config Server örneğini nasıl ayarlayatılabildiğini öğrenmek için aşağıdaki makaleye bakın.

> [!div class="nextstepaction"]
> [Config Server örneğini nasıl ayarlayatılamayı öğrenin](spring-cloud-tutorial-config-server.md)

GitHub: Azure Bahar [Bulut Örnekleri'nde](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples)daha fazla örnek mevcuttur.
