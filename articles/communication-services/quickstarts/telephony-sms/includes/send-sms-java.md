---
title: include dosyası
description: include dosyası
services: azure-communication-services
author: chrwhit
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: chrwhit
ms.openlocfilehash: cb8e6934125630590a337ed7bf7f4c81b2b73bb3
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94915142"
---
SMS mesajları göndermek için Iletişim Hizmetleri Java SMS istemci Kitaplığı ' nı kullanarak Azure Iletişim Hizmetleri ile çalışmaya başlayın.

Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.

<!--**TODO: update all these reference links as the resources go live**

[API reference documentation](../../../references/overview.md) | [Library source code](https://github.com/Azure/azure-sdk-for-net-pr/tree/feature/communication/sdk/communication/Azure.Communication.Sms#todo-update-to-public) | [Artifact (Maven)](#todo-nuget) | [Samples](#todo-samples)-->

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Java Development Kit (JDK)](/java/azure/jdk/?preserve-view=true&view=azure-java-stable) sürüm 8 veya üzeri.
- [Apache Maven](https://maven.apache.org/download.cgi).
- Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).
- SMS etkin telefon numarası. [Telefon numarası alın](../get-phone-number.md).

### <a name="prerequisite-check"></a>Önkoşul denetimi

- Bir Terminal veya komut penceresinde, `mvn -v` Maven 'nin yüklenip yüklenmediğini denetlemek için komutunu çalıştırın.
- Iletişim Hizmetleri kaynağınız ile ilişkili telefon numaralarını görüntülemek için [Azure Portal](https://portal.azure.com/)oturum açın, iletişim hizmetleri kaynağınızı bulun ve sol gezinti bölmesinden **telefon numaraları** sekmesini açın.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-java-application"></a>Yeni bir Java uygulaması oluşturma

Terminal veya komut pencerenizi açın ve Java uygulamanızı oluşturmak istediğiniz dizine gidin. Maven-,-hızlı başlangıç şablonundan Java projesi oluşturmak için aşağıdaki komutu çalıştırın.

```console
mvn archetype:generate -DgroupId=com.communication.quickstart -DartifactId=communication-quickstart -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

' Oluştur ' hedefi, ArtifactId ile aynı ada sahip bir dizin oluşturur. Bu dizin altında **src/Main/Java** dizini proje kaynak kodunu içerir, **src/test/Java dizini** test kaynağını içerir ve **pom.xml** dosyası projenin proje nesne modeli veya Pod olur.

### <a name="install-the-package"></a>Paketi yükler

**pom.xml** dosyasını metin düzenleyicinizde açın. Aşağıdaki bağımlılık öğesini bağımlılıklar grubuna ekleyin.

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-sms</artifactId>
    <version>1.0.0-beta.3</version>
</dependency>
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

`azure-core-http-netty` **pom.xml** dosyanıza bağımlılığı ekleyin.

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-core-http-netty</artifactId>
    <version>1.3.0</version>
</dependency>
```

**/Src/Main/Java/com/Communication/QuickStart/App.Java** 'i bir metin düzenleyicisinde açın, içeri aktarma yönergeleri ekleyin ve `System.out.println("Hello world!");` ifadesini kaldırın:

```java
package com.communication.quickstart;

import java.io.IOException;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.util.ArrayList;
import java.util.List;

import com.azure.communication.common.PhoneNumber;
import com.azure.communication.sms.SmsClient;
import com.azure.communication.sms.SmsClientBuilder;
import com.azure.communication.sms.models.SendSmsOptions;
import com.azure.communication.sms.models.SendSmsResponse;
import com.azure.core.http.HttpClient;
import com.azure.core.http.netty.NettyAsyncHttpClientBuilder;

public class App
{
    public static void main( String[] args ) throws IOException, NoSuchAlgorithmException, InvalidKeyException
    {
        // Quickstart code goes here
    }
}

```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Java için Azure Iletişim Hizmetleri SMS istemci kitaplığı 'nın bazı önemli özelliklerinden bazılarını işler.

| Ad                                                             | Açıklama                                                                                     |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| SmsClientBuilder              | Bu sınıf, SmsClient oluşturur. Bunu uç nokta, kimlik bilgileri ve bir http istemcisiyle sağlarsınız. |
| SmsClient                    | Bu sınıf tüm SMS işlevleri için gereklidir. SMS mesajları göndermek için bunu kullanırsınız.                |
| SendSmsResponse               | Bu sınıf SMS hizmetinden yanıtı içerir.                                          |
| PhoneNumber                   | Bu sınıf telefon numarası bilgilerini barındırır

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`SmsClient`Bağlantı dizeniz ile bir örneğini oluşturun. Aşağıdaki kod, ve adlı ortam değişkenlerinden kaynak için uç nokta ve kimlik bilgisi dizelerini alır `COMMUNICATION_SERVICES_ENDPOINT_STRING` `COMMUNICATION_SERVICES_CREDENTIAL_STRING` (kimlik bilgileri `Key` Azure Portal. [Kaynak bağlantı dizesini yönetme](../../create-communication-resource.md#store-your-connection-string)hakkında bilgi edinin.

`main` yöntemine aşağıdaki kodu ekleyin:

```java
// Your can find your endpoint and access key from your resource in the Azure Portal
String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";
String accessKey = "SECRET";

// Create an HttpClient builder of your choice and customize it
HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();

// Configure and build a new SmsClient
SmsClient client = new SmsClientBuilder()
    .endpoint(endpoint)
    .accessKey(accessKey)
    .httpClient(httpClient)
    .buildClient();
```

İstemciyi, arabirimini uygulayan herhangi bir özel HTTP istemcisiyle başlatabilirsiniz `com.azure.core.http.HttpClient` . Yukarıdaki kod, tarafından sunulan [Azure Core Netty http istemcisinin](/java/api/overview/azure/core-http-netty-readme?preserve-view=true&view=azure-java-stable) kullanımını gösterir `azure-core` .

Ayrıca, uç nokta ve erişim anahtarı sağlamak yerine connectionString () işlevini kullanarak tüm bağlantı dizesini de sağlayabilirsiniz. 
```java
// Your can find your connection string from your resource in the Azure Portal
String connectionString = "<connection_string>";
SmsClient client = new SmsClientBuilder()
    .connectionString(connectionString)
    .httpClient(httpClient)
    .buildClient();
```

## <a name="send-an-sms-message"></a>SMS iletisi gönderme

SendMessage metodunu çağırarak SMS iletisi gönderin. Metodun sonuna bu kodu ekleyin `main` :

```java
List<PhoneNumber> to = new ArrayList<PhoneNumber>();
to.add(new PhoneNumber("<to-phone-number>"));

// SendSmsOptions is an optional field. It can be used
// to enable a delivery report to the Azure Event Grid
SendSmsOptions options = new SendSmsOptions();
options.setEnableDeliveryReport(true);

// Send the message and check the response for a message id
SendSmsResponse response = client.sendMessage(
    new PhoneNumber("<leased-phone-number>"),
    to,
    "<message-text>",
    options);

System.out.println("MessageId: " + response.getMessageId());
```

`<leased-phone-number>`Iletişim Hizmetleri kaynağınız ile ILIŞKILI SMS etkin telefon numarasıyla ve `<to-phone-number>` ileti göndermek istediğiniz telefon numarasıyla değiştirmelisiniz.

`enableDeliveryReport`Parametresi, teslim raporlamayı yapılandırmak için kullanabileceğiniz isteğe bağlı bir parametredir. Bu, SMS iletileri teslim edildiğinde olayları yayma isteyebileceğiniz senaryolar için yararlıdır. SMS iletilerinize yönelik teslim raporlamayı yapılandırmak için [SMS olayları](../handle-sms-events.md) Hızlı Başlangıç Kılavuzu ' na bakın.

<!--todo: the signature of the `sendMessage` method changes when configuring delivery reporting. Need to confirm that this is how our client library is to be used.-->

## <a name="run-the-code"></a>Kodu çalıştırma

**pom.xml** dosyasını içeren dizine gidin ve komutunu kullanarak projeyi derleyin `mvn` .

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