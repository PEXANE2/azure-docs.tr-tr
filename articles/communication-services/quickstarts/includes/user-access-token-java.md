---
title: include dosyası
description: include dosyası
services: azure-communication-services
author: tomaschladek
manager: nmurav
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: tchladek
ms.openlocfilehash: a802ea69dc093d8549fa43e271d99bee9ae9f7ab
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102244809"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Java Development Kit (JDK)](/java/azure/jdk/) sürüm 8 veya üzeri.
- [Apache Maven](https://maven.apache.org/download.cgi).
- Dağıtılan bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../create-communication-resource.md).

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-java-application"></a>Yeni bir Java uygulaması oluşturma

Terminal veya komut pencerenizi açın. Java uygulamanızı oluşturmak istediğiniz dizine gidin. Maven-,-hızlı başlangıç şablonundan Java projesi oluşturmak için aşağıdaki komutu çalıştırın.

```console
mvn archetype:generate -DgroupId=com.communication.quickstart -DartifactId=communication-quickstart -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

' Oluştur ' görevinin ile aynı ada sahip bir dizin oluşturduğunu fark edeceksiniz `artifactId` . Bu dizin altında src/Main/Java dizini proje kaynak kodunu içerir, `src/test/java directory` Test kaynağını içerir ve `pom.xml` dosya projenin proje nesne modeli veya Pod olur.

### <a name="install-the-package"></a>Paketi yükler

**pom.xml** dosyasını metin düzenleyicinizde açın. Aşağıdaki bağımlılık öğesini bağımlılıklar grubuna ekleyin.

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-identity</artifactId>
    <version>1.0.0-beta.3</version> 
</dependency>
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. */Src/Main/Java/com/Communication/QuickStart* dizinine gidin
1. Düzenleyicinizde *app. Java* dosyasını açın
1. İfadesini Değiştir `System.out.println("Hello world!");`
1. `import`Yönergeler ekleme

Başlamak için aşağıdaki kodu kullanın:

```java
import com.azure.communication.identity.*;
import com.azure.communication.common.*;
import java.io.*;
import java.util.*;
import java.time.*;

import com.azure.core.http.*;

public class App
{
    public static void main( String[] args ) throws IOException
    {
        System.out.println("Azure Communication Services - Access Tokens Quickstart");
        // Quickstart code goes here
    }
}
```

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`CommunicationIdentityClient`Kaynağınızın erişim anahtarı ve uç noktasıyla bir a örneği oluşturun. [Kaynak bağlantı dizesini yönetme](../create-communication-resource.md#store-your-connection-string)hakkında bilgi edinin.

`main` yöntemine aşağıdaki kodu ekleyin:

```java
// Your can find your endpoint and access key from your resource in the Azure Portal
String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";
String accessKey = "SECRET";

// Create an HttpClient builder of your choice and customize it
// Use com.azure.core.http.netty.NettyAsyncHttpClientBuilder if that suits your needs
// -> Add "import com.azure.core.http.netty.*;"
// -> Add azure-core-http-netty dependency to file pom.xml

HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();

CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
    .endpoint(endpoint)
    .accessKey(accessKey)
    .httpClient(httpClient)
    .buildClient();
```

İstemciyi, arabirimini uygulayan herhangi bir özel HTTP istemcisiyle başlatabilirsiniz `com.azure.core.http.HttpClient` . Yukarıdaki kod, tarafından sunulan [Azure Core Netty http istemcisinin](/java/api/overview/azure/core-http-netty-readme) kullanımını gösterir `azure-core` .

Ayrıca, uç nokta ve erişim anahtarı sağlamak yerine connectionString () işlevini kullanarak tüm bağlantı dizesini de sağlayabilirsiniz. 
```java
// Your can find your connection string from your resource in the Azure Portal
String connectionString = "<connection_string>";
CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
    .connectionString(connectionString)
    .httpClient(httpClient)
    .buildClient();
```

## <a name="create-an-identity"></a>Kimlik oluşturma

Azure Iletişim Hizmetleri, hafif bir kimlik dizini sağlar. `createUser`Dizinde benzersiz olan yeni bir giriş oluşturmak için yöntemini kullanın `Id` . Uygulamanın kullanıcılarına eşleme ile alınan kimliği depola. Örneğin, bunları uygulama sunucunuzun veritabanında depolayarak. Daha sonra erişim belirteçleri vermek için kimlik gereklidir.

```java
CommunicationUser identity = communicationIdentityClient.createUser();
System.out.println("\nCreated an identity with ID: " + identity.getId());
```

## <a name="issue-access-tokens"></a>Erişim belirteçleri verme

`issueToken`Zaten var olan Iletişim Hizmetleri kimliği için bir erişim belirteci vermek üzere metodunu kullanın. Parametresi `scopes` , bu erişim belirtecini yetkilendirecek temel öğeler kümesini tanımlar. [Desteklenen eylemlerin listesine](../../concepts/authentication.md)bakın. Parametresinin yeni örneği, `user` Azure Iletişim hizmeti kimliğinin dize gösterimine göre oluşturulabilir.

```java
// Issue an access token with the "voip" scope for an identity
List<String> scopes = new ArrayList<>(Arrays.asList("voip"));
CommunicationUserToken response = communicationIdentityClient.issueToken(identity, scopes);
OffsetDateTime expiresOn = response.getExpiresOn();
String token = response.getToken();
System.out.println("\nIssued an access token with 'voip' scope that expires at: " + expiresOn + ": " + token);
```

Erişim belirteçleri yeniden verilmesini gerektiren kısa ömürlü kimlik bilgileridir. Bunu yapmamak, uygulamanızın kullanıcı deneyiminin kesintiye uğramasına neden olabilir. `expiresAt`Response özelliği, erişim belirtecinin ömrünü gösterir.

## <a name="refresh-access-tokens"></a>Erişim belirteçlerini yenileme

Bir erişim belirtecini yenilemek için, öğesini kullanarak yeniden `CommunicationUser` yayımlayın:

```java  
// Value existingIdentity represents identity of Azure Communication Services stored during identity creation
CommunicationUser identity = new CommunicationUser(existingIdentity);
response = communicationIdentityClient.issueToken(identity, scopes);
```

## <a name="revoke-access-tokens"></a>Erişim belirteçlerini iptal et

Bazı durumlarda, erişim belirteçlerini açıkça iptal edebilirsiniz. Örneğin, bir uygulamanın kullanıcısı hizmetinize kimlik doğrulaması yapmak için kullandıkları parolayı değiştirdiğinde. Yöntem `revokeTokens` , kimliğe verilen tüm etkin erişim belirteçlerini geçersiz kılar.

```java  
communicationIdentityClient.revokeTokens(identity, OffsetDateTime.now());
System.out.println("\nSuccessfully revoked all access tokens for identity with ID: " + identity.getId());
```

## <a name="delete-an-identity"></a>Kimlik silme

Bir kimlik silindiğinde tüm etkin erişim belirteçleri iptal olur ve kimlik için erişim belirteçleri yayınınızdan sonra. Ayrıca, kimlik ile ilişkili tüm kalıcı içeriği de kaldırır.

```java
communicationIdentityClient.deleteUser(identity);
System.out.println("\nDeleted the identity with ID: " + identity.getId());
```

## <a name="run-the-code"></a>Kodu çalıştırma

*pom.xml* dosyasını içeren dizine gidin ve aşağıdaki komutu kullanarak projeyi derleyin `mvn` .

```console
mvn compile
```

Ardından, paketini oluşturun.

```console
mvn package
```

`mvn`Uygulamayı yürütmek için aşağıdaki komutu çalıştırın.

```console
mvn exec:java -Dexec.mainClass="com.communication.quickstart.App" -Dexec.cleanupDaemonThreads=false
```
