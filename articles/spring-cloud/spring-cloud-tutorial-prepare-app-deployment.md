---
title: Öğretici-Azure yay bulutu 'nda dağıtım için bir Java Spring uygulaması hazırlama
description: Bu öğreticide, bir Java Spring uygulamasını Azure yay bulutuna dağıtım için hazırlarsınız.
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 02/03/2020
ms.author: brendm
ms.openlocfilehash: 1af4bdcfc0fea319b9e5c9f39a1ade3f4492404f
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77137623"
---
# <a name="prepare-a-java-spring-application-for-deployment-in-azure-spring-cloud"></a>Azure yay bulutu 'nda bir Java Spring uygulamasını dağıtıma hazırlama

Bu hızlı başlangıçta, mevcut bir Java Spring uygulamasının Azure Spring Cloud 'a dağıtılması için hazırlanması gösterilmektedir. Doğru yapılandırılmışsa, Azure yay bulutu, Java Spring Cloud uygulamanızı izlemek, ölçeklendirmek ve güncelleştirmek için güçlü hizmetler sunar.

## <a name="java-runtime-version"></a>Java çalışma zamanı sürümü

Azure yay bulutu 'nda yalnızca Spring/Java uygulamaları çalıştırılabilir.

Azure yay bulutu hem Java 8 hem de Java 11 ' i destekler. Barındırma ortamı, Azure için Azul Zulu OpenJDK 'nin en son sürümünü içerir. Azure için Azul Zulu OpenJDK hakkında daha fazla bilgi için bkz. [JDK 'Yi yüklemeyin](https://docs.microsoft.com/azure/java/jdk/java-jdk-install).

## <a name="spring-boot-and-spring-cloud-versions"></a>Yay önyükleme ve yay bulut sürümleri

Azure yay bulutu yalnızca Spring Boot uygulamalarını destekler. Hem Spring Boot sürüm 2,1 hem de 2,2 sürümünü destekler. Aşağıdaki tabloda desteklenen Spring Boot ve Spring Cloud kombinasyonlarını listelenmektedir:

Spring Boot sürümü | Yay bulutu sürümü
---|---
2.1 | Greenwich. RELEASE
2.2 | Hoxton. RELEASE

Pod. XML dosyanızın, Spring Boot sürümünüze bağlı olarak doğru yay önyüklemesinde ve Spring Cloud Dependencies 'e sahip olduğunu doğrulayın.

### <a name="dependencies-for-spring-boot-version-21"></a>Spring Boot sürüm 2,1 için bağımlılıklar

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

Azure yay bulutu, sizin için bahar bulut bileşenlerini barındırır ve yönetir. Bu tür bileşenler yay bulut hizmeti kayıt defteri ve Spring Cloud config Server ' ı içerir. Azure Spring Cloud Service örneğiniz ile iletişime izin vermek için bağımlılıklarınızı Azure yay bulutu istemci Kitaplığı ' nı dahil edin.

Aşağıdaki tabloda, uygulamanız için Spring Boot ve Spring Cloud kullanan doğru Azure yay bulut sürümleri listelenmektedir.

Spring Boot sürümü | Yay bulutu sürümü | Azure Spring Cloud sürümü
---|---|---
2.1 | Greenwich. RELEASE | 2.1
2.2 | Hoxton. RELEASE | 2.2

Pod. xml dosyanıza aşağıdaki bağımlılıklardan birini ekleyin. Azure yay bulutu sürümü kendi ile eşleşen bağımlılığı seçin.

### <a name="dependency-for-azure-spring-cloud-version-21"></a>Azure Spring Cloud sürüm 2,1 için bağımlılık

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.1</version>
</dependency>
```

### <a name="dependency-for-azure-spring-cloud-version-22"></a>Azure Spring Cloud sürüm 2,2 için bağımlılık

```xml
<dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.2.0</version>
</dependency>
```

## <a name="other-required-dependencies"></a>Diğer gerekli bağımlılıklar

Azure Spring Cloud 'ın yerleşik özelliklerini etkinleştirmek için, uygulamanız aşağıdaki bağımlılıkları içermelidir. Bu içerme, uygulamanızın her bileşenle doğru bir şekilde yapılandırmasını sağlar.  

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

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Java Spring uygulamanızı Azure Spring Cloud 'a dağıtmak üzere nasıl yapılandıracağınızı öğrendiniz. Bir yapılandırma sunucusu örneğini ayarlamayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Bir yapılandırma sunucusu örneğini ayarlamayı öğrenin](spring-cloud-tutorial-config-server.md)

GitHub 'da daha fazla örnek vardır: [Azure Spring Cloud Samples](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
