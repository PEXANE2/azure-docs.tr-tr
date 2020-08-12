---
title: Spring Boot uygulamasında dinamik yapılandırma kullanma
titleSuffix: Azure App Configuration
description: Spring Boot uygulamaları için yapılandırma verilerini dinamik olarak güncelleştirme hakkında bilgi edinin
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: tutorial
ms.date: 3/5/2020
ms.custom: devx-track-java
ms.author: lcozzens
ms.openlocfilehash: 653fcb6f6590fd503a97800ec8196025cf14a3b9
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88121570"
---
# <a name="tutorial-use-dynamic-configuration-in-a-java-spring-app"></a>Öğretici: bir Java Spring uygulamasında dinamik yapılandırma kullanma

Uygulama yapılandırması Spring Boot istemci kitaplığı, bir uygulamanın yeniden başlatılmasına neden olmadan, bir yapılandırma ayarları kümesinin isteğe bağlı olarak güncelleştirilmesini destekler. Yapılandırma deposuna çok fazla çağrı yapmaktan kaçınmak için istemci kitaplığı her ayarı önbelleğe alır. Yenileme işlemi, değeri yapılandırma deposunda değiştiği zaman, önbelleğe alınan değerin süresi doluncaya kadar değeri güncelleştirmez. Her istek için varsayılan sona erme saati 30 saniyedir. Gerekirse, geçersiz kılınabilir.

' In metodunu çağırarak, güncelleştirilmiş ayarları isteğe bağlı olarak `AppConfigurationRefresh` denetleyebilirsiniz `refreshConfigurations()` .

Alternatif olarak, `spring-cloud-azure-appconfiguration-config-web` `spring-web` otomatik yenilemeyi işlemek için bir bağımlılığı alan paketi kullanabilirsiniz.

## <a name="use-automated-refresh"></a>Otomatik yenilemeyi kullan

Otomatik yenilemeyi kullanmak için, uygulama [yapılandırması Için Spring Boot hızlı](quickstart-java-spring-app.md)başlangıcını izleyerek oluşturduğunuz uygulama gibi uygulama yapılandırması kullanan bir Spring Boot uygulamasıyla başlayın.

Sonra, *pom.xml* dosyasını bir metin düzenleyicisinde açın ve için bir ekleyin `<dependency>` `spring-cloud-azure-appconfiguration-config-web` .

**Yay bulutu 1.1. x**

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spring-cloud-azure-appconfiguration-config-web</artifactId>
    <version>1.1.5</version>
</dependency>
```

**Yay bulutu 1.2. x**

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spring-cloud-azure-appconfiguration-config-web</artifactId>
    <version>1.2.7</version>
</dependency>
```

## <a name="run-and-test-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma ve test etme

1. Spring Boot uygulamanızı Maven’le oluşturun ve çalıştırın.

    ```shell
    mvn clean package
    mvn spring-boot:run
    ```

1. Bir tarayıcı penceresi açın ve URL 'ye gidin: `http://localhost:8080` .  Anahtarınızla ilişkili iletiyi görürsünüz. 

    Ayrıca, uygulamanızı test etmek için *kıvrımlı* da kullanabilirsiniz, örneğin: 
    
    ```cmd
    curl -X GET http://localhost:8080/
    ```

1. Dinamik yapılandırmayı test etmek için uygulamanızla ilişkili Azure uygulama yapılandırma portalını açın. **Yapılandırma Gezgini**' ni seçin ve görüntülenmiş anahtarın değerini güncelleştirin, örneğin:
    | Anahtar | Değer |
    |---|---|
    | uygulama/yapılandırma. ileti | Merhaba-güncelleştirildi |

1. Yeni iletinin görüntülendiğini görmek için tarayıcı sayfasını yenileyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uygulama yapılandırmasından yapılandırma ayarlarını dinamik olarak yenilemek için Spring Boot uygulamanızı etkinleştirdiniz. Azure yönetilen kimliğin uygulama yapılandırmasına erişimi kolaylaştırmak için nasıl kullanılacağını öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
