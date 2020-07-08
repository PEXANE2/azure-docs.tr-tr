---
title: Azure Media Services v3 API 'sine bağlanma-Java
description: Bu makalede, Java ile Azure Media Services v3 API 'sine nasıl bağlanabileceğinizi açıklamaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2019
ms.author: juliako
ms.openlocfilehash: 6b0f21c3fa7a9c827f7201f4b899a33ea77eaf08
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74888504"
---
# <a name="connect-to-media-services-v3-api---java"></a>Media Services v3 API 'sine bağlanma-Java

Bu makalede hizmet sorumlusu oturum açma yöntemi kullanılarak Azure Media Services v3 Java SDK 'sına nasıl bağlanabilmeniz gösterilmektedir.

Bu makalede, örnek uygulamayı geliştirmek için Visual Studio Code kullanılır.

## <a name="prerequisites"></a>Ön koşullar

- Yüklemek için [Visual Studio Code Java yazma](https://code.visualstudio.com/docs/java/java-tutorial) ' yı izleyin:

   - JDK
   - Apache Maven
   - Java Uzantı paketi
- `JAVA_HOME`Ve ortam değişkenlerini ayarladığınızdan emin olun `PATH` .
- [Media Services hesabı oluşturun](create-account-cli-how-to.md). Kaynak grubu adını ve Media Services hesap adını unutduğunuzdan emin olun.
- [Erişim API 'leri](access-api-cli-how-to.md) konusundaki adımları izleyin. Abonelik KIMLIĞI, uygulama KIMLIĞI (istemci KIMLIĞI), kimlik doğrulama anahtarı (gizli) ve daha sonraki bir adımda ihtiyacınız olan kiracı KIMLIĞINI kaydedin.

Ayrıca şunları gözden geçirin:

- [Visual Studio Code 'de Java](https://code.visualstudio.com/docs/languages/java)
- [VS Code 'de Java proje yönetimi](https://code.visualstudio.com/docs/java/java-project)

> [!IMPORTANT]
> [Adlandırma kurallarını](media-services-apis-overview.md#naming-conventions)gözden geçirin.

## <a name="create-a-maven-project"></a>Maven projesi oluşturma

Bir komut satırı aracı açın ve `cd` Projeyi oluşturmak istediğiniz dizine gidin.
    
```
mvn archetype:generate -DgroupId=com.azure.ams -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Komutunu çalıştırdığınızda,, `pom.xml` `App.java` ve diğer dosyalar oluşturulur. 

## <a name="add-dependencies"></a>Bağımlılık Ekle

1. Visual Studio Code, projenizin bulunduğu klasörü açın
1. Bul ve aç`pom.xml`
1. Gerekli bağımlılıkları Ekle

    ```xml
   <dependency>
     <groupId>com.microsoft.azure.mediaservices.v2018_07_01</groupId>
     <artifactId>azure-mgmt-media</artifactId>
     <version>1.0.0-beta-3</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.rest</groupId>
     <artifactId>client-runtime</artifactId>
     <version>1.6.6</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-client-authentication</artifactId>
     <version>1.6.6</version>
   </dependency>
    ```

## <a name="connect-to-the-java-client"></a>Java istemcisine bağlanma

1. `App.java`Altındaki dosyayı açın `src\main\java\com\azure\ams` ve paketinizin en üste eklendiğinden emin olun:

    ```java
    package com.azure.ams;
    ```
1. Paket deyimi altında bu içeri aktarma deyimlerini ekleyin:
   
   ```java
   import com.microsoft.azure.AzureEnvironment;
   import com.microsoft.azure.credentials.ApplicationTokenCredentials;
   import com.microsoft.azure.management.mediaservices.v2018_07_01.implementation.MediaManager;
   import com.microsoft.rest.LogLevel;
   ```
1. İstek yapmak için ihtiyaç duyduğunuz Active Directory kimlik bilgilerini oluşturmak için, uygulama sınıfının Main yöntemine aşağıdaki kodu ekleyin ve [erişim API 'lerinden](access-api-cli-how-to.md)aldığınız değerleri ayarlayın:
   
   ```java
   final String clientId = "00000000-0000-0000-0000-000000000000";
   final String tenantId = "00000000-0000-0000-0000-000000000000";
   final String clientSecret = "00000000-0000-0000-0000-000000000000";
   final String subscriptionId = "00000000-0000-0000-0000-000000000000";

   try {
      ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(clientId, tenantId, clientSecret, AzureEnvironment.AZURE);
      credentials.withDefaultSubscriptionId(subscriptionId);

      MediaManager manager = MediaManager
              .configure()
              .withLogLevel(LogLevel.BODY_AND_HEADERS)
              .authenticate(credentials, credentials.defaultSubscriptionId());
      System.out.println("signed in");
   }
   catch (Exception e) {
      System.out.println("Exception encountered.");
      System.out.println(e.toString());
   }
   ```
1. Uygulamayı çalıştırın.

## <a name="see-also"></a>Ayrıca bkz.

- [Media Services kavramlar](concepts-overview.md)
- [Java SDK](https://aka.ms/ams-v3-java-sdk)
- [Java başvurusu](https://aka.ms/ams-v3-java-ref)
- [com. Microsoft. Azure. mediaservices. v2018_07_01: Azure-MGMT-Media](https://search.maven.org/artifact/com.microsoft.azure.mediaservices.v2018_07_01/azure-mgmt-media/1.0.0-beta/jar)

## <a name="next-steps"></a>Sonraki adımlar

Artık `import com.microsoft.azure.management.mediaservices.v2018_07_01.*;` varlıkları ekleme ve düzenleme ' ye başlayabilirsiniz.

Daha fazla kod örneği için bkz. [Java SDK örnek](https://docs.microsoft.com/samples/azure-samples/media-services-v3-java/azure-media-services-v3-samples-using-java/) deposu.
