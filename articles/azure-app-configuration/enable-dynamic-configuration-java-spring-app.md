---
title: Spring Boot uygulamasında dinamik yapılandırma kullanma
titleSuffix: Azure App Configuration
description: Spring Boot uygulamaları için yapılandırma verilerini dinamik olarak güncelleştirme hakkında bilgi edinin
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: tutorial
ms.date: 3/5/2020
ms.author: lcozzens
ms.openlocfilehash: 6445b9707273d273c562b7d643da34f5ba26e1fc
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78967497"
---
# <a name="tutorial-use-dynamic-configuration-in-a-java-spring-app"></a>Öğretici: bir Java Spring uygulamasında dinamik yapılandırma kullanma

Uygulama yapılandırması Spring Boot istemci kitaplığı, bir uygulamanın yeniden başlatılmasına neden olmadan, bir yapılandırma ayarları kümesinin isteğe bağlı olarak güncelleştirilmesini destekler. Yapılandırma deposuna çok fazla çağrı yapmaktan kaçınmak için istemci kitaplığı her ayarı önbelleğe alır. Yenileme işlemi, değeri yapılandırma deposunda değiştiği zaman, önbelleğe alınan değerin süresi doluncaya kadar değeri güncelleştirmez. Her istek için varsayılan sona erme saati 30 saniyedir. Gerekirse, geçersiz kılınabilir.

`AppConfigurationRefresh``refreshConfigurations()` metodunu çağırarak, güncelleştirilmiş ayarları isteğe bağlı olarak denetleyebilirsiniz.

Alternatif olarak, otomatik yenilemeyi işlemek için `spring-web` bağımlılığı alan `spring-cloud-azure-appconfiguration-config-web` paketini kullanabilirsiniz.

## <a name="use-automated-refresh"></a>Otomatik yenilemeyi kullan

Otomatik yenilemeyi kullanmak için, uygulama [yapılandırması Için Spring Boot hızlı](quickstart-java-spring-app.md)başlangıcını izleyerek oluşturduğunuz uygulama gibi uygulama yapılandırması kullanan bir Spring Boot uygulamasıyla başlayın.

Ardından, *POI. xml* dosyasını bir metin düzenleyicisinde açın ve `spring-cloud-azure-appconfiguration-config-web`için `<dependency>` ekleyin.

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

Bu öğreticide, uygulama yapılandırmasından yapılandırma ayarlarını dinamik olarak yenilemek için Spring Boot uygulamanızı etkinleştirdiniz. Daha fazla bilgi için bkz. [Azure 'Da yay](https://docs.microsoft.com/java/azure/spring-framework/).
