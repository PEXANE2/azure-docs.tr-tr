---
title: dosya dahil etme
description: dosya dahil etme
services: azure-communication-services
author: chrwhit
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: chrwhit
ms.openlocfilehash: 72bb38b7d9d28366fb75c5656832a1f37c770fd2
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948339"
---
SMS mesajları göndermek için Iletişim Hizmetleri Java SMS istemci Kitaplığı ' nı kullanarak Azure Iletişim Hizmetleri ile çalışmaya başlayın.

Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.

<!--**TODO: update all these reference links as the resources go live**

[API reference documentation](../../../references/overview.md) | [Library source code](https://github.com/Azure/azure-sdk-for-net-pr/tree/feature/communication/sdk/communication/Azure.Communication.Sms#todo-update-to-public) | [Artifact (Maven)](#todo-nuget) | [Samples](#todo-samples)-->

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Java Development Kit (JDK)](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable&preserve-view=true) sürüm 8 veya üzeri.
- [Apache Maven](https://maven.apache.org/download.cgi).
- Dağıtılan bir Iletişim Hizmetleri kaynağı. [Iletişim Hizmetleri kaynağı oluşturun](../../create-communication-resource.md).

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
    <version>1.0.0-beta.1</version>
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

import com.azure.communication.common.CommunicationClientCredential;
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
| CommunicationClientCredential | Bu sınıf, imzalama isteklerini işler.                                                            |
| PhoneNumber                   | Bu sınıf telefon numarası bilgilerini barındırır

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

`SmsClient`Bağlantı dizeniz ile bir örneğini oluşturun. Aşağıdaki kod, ve adlı ortam değişkenlerinden kaynak için uç nokta ve kimlik bilgisi dizelerini alır `COMMUNICATION_SERVICES_ENDPOINT_STRING` `COMMUNICATION_SERVICES_CREDENTIAL_STRING` (kimlik bilgileri `Key` Azure Portal. [Kaynak bağlantı dizesini yönetme](../../create-communication-resource.md#store-your-connection-string)hakkında bilgi edinin.

`main` yöntemine aşağıdaki kodu ekleyin:

```java
// Create an HttpClient builder of your choice and customize it
HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();

CommunicationClientCredential credential = new CommunicationClientCredential(accessKey);

// Configure and build a new SmsClient
SmsClient client = new SmsClientBuilder()
    .endpoint(endpoint)
    .credential(credential)
    .httpClient(httpClient)
    .buildClient();
```

İstemciyi, arabirimini uygulayan herhangi bir özel HTTP istemcisiyle başlatabilirsiniz `com.azure.core.http.HttpClient` . Yukarıdaki kod, tarafından sunulan [Azure Core Netty http istemcisinin](https://docs.microsoft.com/java/api/overview/azure/core-http-netty-readme?view=azure-java-stable&preserve-view=true) kullanımını gösterir `azure-core` .

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

`<leased-phone-number>`Iletişim Hizmetleri kaynağınız ile ILIŞKILI SMS etkin telefon numarasıyla ve `<to-phone-number>` ileti göndermek istediğiniz telefon numarasıyla değiştirmelisiniz. Tüm telefon numarası parametreleri [E. 164 standardına](../../../concepts/telephony-sms/plan-solution.md#optional-reading-international-public-telecommunication-numbering-plan-e164)bağlı olmalıdır.

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
