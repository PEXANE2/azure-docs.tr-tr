---
title: Bahar Önyükleme uygulamasında dinamik yapılandırma yı kullanma
titleSuffix: Azure App Configuration
description: Bahar Önyükleme uygulamaları için yapılandırma verilerini dinamik olarak nasıl güncelleştirebilirsiniz öğrenin
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: tutorial
ms.date: 3/5/2020
ms.author: lcozzens
ms.openlocfilehash: 37c832e3b6d1430da0b45558c9632f0486a7233b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79216751"
---
# <a name="tutorial-use-dynamic-configuration-in-a-java-spring-app"></a>Öğretici: Java Spring uygulamasında dinamik yapılandırmayı kullanın

App Configuration İlkbahar Önyükleme istemci kitaplığı, bir uygulamanın yeniden başlatılmasına neden olmadan bir dizi yapılandırma ayarını isteğe bağlı olarak güncelleştirmeyi destekler. İstemci kitaplığı, yapılandırma deposuna çok fazla çağrı dan kaçınmak için her ayarı önbelleğe aldı. Yenileme işlemi, önbelleğe alınmış değer süresi dolana kadar, yapılandırma deposundaki değer değişse bile değeri güncelleştirmez. Her istek için varsayılan son kullanma süresi 30 saniyedir. Gerekirse geçersiz kılınabilir.

''s `AppConfigurationRefresh` `refreshConfigurations()` yöntemini arayarak isteğe bağlı güncelleştirilmiş ayarları kontrol edebilirsiniz.

Alternatif olarak, otomatik `spring-cloud-azure-appconfiguration-config-web` yenileme işlemek `spring-web` için bir bağımlılık alır paketi kullanabilirsiniz.

## <a name="use-automated-refresh"></a>Otomatik yenileme kullanma

Otomatik yenilemeyi kullanmak [için, Uygulama Yapılandırması için Bahar Önyükleme hızlı başlat'ını](quickstart-java-spring-app.md)izleyerek oluşturduğunuz uygulama gibi Uygulama Yapılandırması'nı kullanan bir Bahar Önyükleme uygulamasıyla başlayın.

Sonra, bir metin düzenleyicisi *pom.xml* dosyasını `spring-cloud-azure-appconfiguration-config-web`açın ve için bir `<dependency>` ekleyin.

**Bahar Bulutu 1.1.x**

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spring-cloud-azure-appconfiguration-config-web</artifactId>
    <version>1.1.2</version>
</dependency>
```

**Bahar Bulutu 1.2.x**

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spring-cloud-azure-appconfiguration-config-web</artifactId>
    <version>1.2.2</version>
</dependency>
```

Dosyayı kaydedin, ardından uygulamanızı her zamanki gibi oluşturun ve çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Bahar Önyükleme uygulamanızın Uygulama Yapılandırması'ndan yapılandırma ayarlarını dinamik olarak yenilemesini sağladınız. Uygulama Yapılandırması'na erişimi kolaylaştırmak için Azure yönetilen bir kimliği nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
