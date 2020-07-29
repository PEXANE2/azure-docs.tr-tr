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
ms.openlocfilehash: a2864d610d71c6b3a86c131dabb3c0b9ed138bec
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87327931"
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
    <version>1.1.2</version>
</dependency>
```

**Yay bulutu 1.2. x**

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spring-cloud-azure-appconfiguration-config-web</artifactId>
    <version>1.2.2</version>
</dependency>
```

Dosyayı kaydedin, sonra uygulamanızı her zamanki gibi derleyin ve çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uygulama yapılandırmasından yapılandırma ayarlarını dinamik olarak yenilemek için Spring Boot uygulamanızı etkinleştirdiniz. Azure yönetilen kimliğin uygulama yapılandırmasına erişimi kolaylaştırmak için nasıl kullanılacağını öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
