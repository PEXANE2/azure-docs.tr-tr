---
title: Azure Medya Hizmetleri v3 API'ye bağlanın - Java
description: Bu makalede, Java ile Azure Media Services v3 API'ye nasıl bağlanılabağlanabileceğiniz açıklanmaktadır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74888504"
---
# <a name="connect-to-media-services-v3-api---java"></a>Medya Hizmetlerine bağlan v3 API - Java

Bu makalede, hizmet ana oturum açma yöntemini kullanarak Azure Media Services v3 Java SDK'ya nasıl bağlanabileceğinizi gösterir.

Bu makalede, Örnek uygulamayı geliştirmek için Visual Studio Code kullanılır.

## <a name="prerequisites"></a>Ön koşullar

- Yüklemek için [Visual Studio Code ile Java Yazma](https://code.visualstudio.com/docs/java/java-tutorial) izleyin:

   - JDK
   - Apaçi Maven
   - Java Uzatma Paketi
- Değişkenleri `JAVA_HOME` ayarladık `PATH` ve ortama ayarladıklarından emin olun.
- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md) Kaynak grup adını ve Medya Hizmetleri hesap adını hatırladığından emin olun.
- [Access API'leri](access-api-cli-how-to.md) konusundaki adımları izleyin. Abonelik kimliğini, uygulama kimliğini (istemci kimliğini), kimlik doğrulama anahtarını (gizli) ve daha sonraki bir adımda gereksinim duyduğunuz kiracı kimliğini kaydedin.

Ayrıca gözden geçirin:

- [Görsel Stüdyo Kodunda Java](https://code.visualstudio.com/docs/languages/java)
- [VS Kodunda Java Proje Yönetimi](https://code.visualstudio.com/docs/java/java-project)

> [!IMPORTANT]
> Adlandırma kurallarını gözden [geçirin.](media-services-apis-overview.md#naming-conventions)

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturma

Komut satırı aracını `cd` ve projeyi oluşturmak istediğiniz bir dizine açın.
    
```
mvn archetype:generate -DgroupId=com.azure.ams -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Komutu çalıştırdığınızda, `pom.xml` `App.java`, , ve diğer dosyalar oluşturulur. 

## <a name="add-dependencies"></a>Bağımlılıkekleme

1. Visual Studio Code'da, projenizin olduğu klasörü açın
1. Bulun ve açın`pom.xml`
1. Gerekli bağımlılıkları ekleme

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

## <a name="connect-to-the-java-client"></a>Java istemcisine bağlanın

1. Aşağıdaki `src\main\java\com\azure\ams` `App.java` dosyayı açın ve paketinizin en üstte yer aldığından emin olun:

    ```java
    package com.azure.ams;
    ```
1. Paket deyiminin altına şu alma ekstrelerini ekleyin:
   
   ```java
   import com.microsoft.azure.AzureEnvironment;
   import com.microsoft.azure.credentials.ApplicationTokenCredentials;
   import com.microsoft.azure.management.mediaservices.v2018_07_01.implementation.MediaManager;
   import com.microsoft.rest.LogLevel;
   ```
1. İstekte bulunmak için gereken Active Directory kimlik bilgilerini oluşturmak için, Uygulama sınıfının ana yöntemine aşağıdaki kodu ekleyin ve [Access API'lerinden](access-api-cli-how-to.md)aldığınız değerleri ayarlayın:
   
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

- [Medya Hizmetleri kavramları](concepts-overview.md)
- [Java SDK](https://aka.ms/ams-v3-java-sdk)
- [Java başvurusu](https://aka.ms/ams-v3-java-ref)
- [com.microsoft.azure.mediaservices.v2018_07_01:azure-mgmt-media](https://search.maven.org/artifact/com.microsoft.azure.mediaservices.v2018_07_01/azure-mgmt-media/1.0.0-beta/jar)

## <a name="next-steps"></a>Sonraki adımlar

Artık varlıkları `import com.microsoft.azure.management.mediaservices.v2018_07_01.*;` dahil edebilir ve işlemeye başlayabilirsiniz.

Daha fazla kod örneği için [Java SDK örnekleri](https://docs.microsoft.com/samples/azure-samples/media-services-v3-java/azure-media-services-v3-samples-using-java/) repo'ya bakın.
