---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 63c524880a47c6e519649bd871a6216d6faeefce
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69907085"
---
[!INCLUDE [Prerequisites](prerequisites-java.md)]

[!INCLUDE [Setup and use environment variables](setup-env-variables.md)]

## <a name="initialize-a-project-with-gradle"></a>Gradle ile proje başlatma

Bu proje için bir çalışma dizini oluşturarak başlayalım. Komut satırından (veya terminalden) şu komutu çalıştırın:

```console
mkdir alt-translation-sample
cd alt-translation-sample
```

Sonra, bir Gradle projesinin başlatılmasını yapacaksın. Bu komut, `build.gradle.kts`uygulamanızı oluşturmak ve yapılandırmak için çalışma zamanında kullanılan Gradle için temel yapı dosyaları oluşturur. Bu komutu çalışma dizininizden çalıştırın:

```console
gradle init --type basic
```

Bir **DSL**seçmek istendiğinde, **Kotlin**seçin.

## <a name="configure-the-build-file"></a>Yapı dosyasını yapılandırma

En `build.gradle.kts` sevdiğiniz IDE veya metin düzenleyicisi ile bulun ve açın. Sonra bu yapı yapılandırmasında kopyalayın:

```
plugins {
    java
    application
}
application {
    mainClassName = "AltTranslation"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("com.squareup.okhttp:okhttp:2.5.0")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Bu örnekte HTTP istekleri için OkHttp'de bağımlılıklar olduğunu ve Gson'un JSON'u işlemek ve ayrıştırmak için olduğunu unutmayın. Yapı yapılandırmaları hakkında daha fazla bilgi edinmek istiyorsanız, [bkz.](https://guides.gradle.org/creating-new-gradle-builds/)

## <a name="create-a-java-file"></a>Java dosyası oluşturma

Örnek uygulamanız için bir klasör oluşturalım. Çalışma dizininizden çalıştırın:

```console
mkdir -p src\main\java
```

Ardından, bu klasörde, adlı `AltTranslation.java`bir dosya oluşturun.

## <a name="import-required-libraries"></a>Gerekli kitaplıkları içe aktarma

Bu `AltTranslation.java` alma deyimlerini açın ve ekleyin:

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;
```


## <a name="define-variables"></a>Değişkenleri tanımlama

İlk olarak, projeniz için bir ortak sınıf oluşturmanız gerekir:

```java
public class AltTranslation {
  // All project code goes here...
}
```

Bu satırları sınıfa `AltTranslation` ekleyin. İlk olarak, abonelik anahtarı ve bitiş noktası ortam değişkenlerinden okunuyor. Daha sonra, `api-version`iki ek parametre ile birlikte eklenmiştir fark `url`edeceksiniz . Bu parametreler çeviri giriş ve çıktısını ayarlamak için kullanılır. Bu örnekte, bunlar`en`İngilizce (`es`) ve İspanyolca ( )

```java
private static String subscriptionKey = System.getenv("TRANSLATOR_TEXT_SUBSCRIPTION_KEY");
private static String endpoint = System.getenv("TRANSLATOR_TEXT_ENDPOINT");
String url = endpoint + "/dictionary/lookup?api-version=3.0&from=en&to=es";
```

Bilişsel Hizmetler çok hizmet aboneliği kullanıyorsanız, istek parametrelerinize `Ocp-Apim-Subscription-Region` de eklemeniz gerekir. [Çoklu hizmet aboneliği ile kimlik doğrulama hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)

## <a name="create-a-client-and-build-a-request"></a>İstemci oluşturma ve istek oluşturma

Anında sınıfa `AltTranslation` bu satırı `OkHttpClient`ekleyin:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

Sonra, POST isteği oluşturalım. Çeviri için metni değiştirmekten çekinmeyin.

```java
// This function performs a POST request.
public String Post() throws IOException {
    MediaType mediaType = MediaType.parse("application/json");
    RequestBody body = RequestBody.create(mediaType,
            "[{\n\t\"Text\": \"Pineapples\"\n}]");
    Request request = new Request.Builder()
            .url(url).post(body)
            .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey)
            .addHeader("Content-type", "application/json").build();
    Response response = client.newCall(request).execute();
    return response.body().string();
}
```

## <a name="create-a-function-to-parse-the-response"></a>Yanıtı ayrıştırmak için bir işlev oluşturma

Bu basit işlev, Translator Text hizmetinden JSON yanıtını ayrıştırır ve güzelbir şekilde verir.

```java
// This function prettifies the json response.
public static String prettify(String json_text) {
    JsonParser parser = new JsonParser();
    JsonElement json = parser.parse(json_text);
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
}
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Son adım bir istekte bulunmak ve yanıt almaktır. Projenize şu satırları ekleyin:

```java
public static void main(String[] args) {
    try {
        AltTranslation altTranslationRequest = new AltTranslation();
        String response = altTranslationRequest.Post();
        System.out.println(prettify(response));
    } catch (Exception e) {
        System.out.println(e);
    }
}
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu, örnek uygulamanızı çalıştırmaya hazırsınız. Komut satırından (veya terminal oturumundan), çalışma dizinizin köküne gidin ve çalıştırın:

```console
gradle build
```

Yapı tamamlandığında çalıştırın:

```console
gradle run
```

## <a name="sample-response"></a>Örnek yanıt

```json
[
  {
    "normalizedSource": "pineapples",
    "displaySource": "pineapples",
    "translations": [
      {
        "normalizedTarget": "piñas",
        "displayTarget": "piñas",
        "posTag": "NOUN",
        "confidence": 0.7016,
        "prefixWord": "",
        "backTranslations": [
          {
            "normalizedText": "pineapples",
            "displayText": "pineapples",
            "numExamples": 5,
            "frequencyCount": 158
          },
          {
            "normalizedText": "cones",
            "displayText": "cones",
            "numExamples": 5,
            "frequencyCount": 13
          },
          {
            "normalizedText": "piña",
            "displayText": "piña",
            "numExamples": 3,
            "frequencyCount": 5
          },
          {
            "normalizedText": "ganks",
            "displayText": "ganks",
            "numExamples": 2,
            "frequencyCount": 3
          }
        ]
      },
      {
        "normalizedTarget": "ananás",
        "displayTarget": "ananás",
        "posTag": "NOUN",
        "confidence": 0.2984,
        "prefixWord": "",
        "backTranslations": [
          {
            "normalizedText": "pineapples",
            "displayText": "pineapples",
            "numExamples": 2,
            "frequencyCount": 16
          }
        ]
      }
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Çevirmen Metin API'si ile yapabileceğiniz her şeyi anlamak için API başvurusuna bir göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
