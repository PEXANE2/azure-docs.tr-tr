---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 34ff0e792fc388f3083e2d490b2658822793988f
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69906989"
---
[!INCLUDE [Prerequisites](prerequisites-java.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="initialize-a-project-with-gradle"></a>Gradle ile proje başlatma

Bu proje için bir çalışma dizini oluşturarak başlayalım. Komut satırından (veya terminalden) şu komutu çalıştırın:

```console
mkdir get-languages-sample
cd get-languages-sample
```

Sonra, bir Gradle projesinin başlatılmasını yapacaksın. Bu komut, `build.gradle.kts`uygulamanızı oluşturmak ve yapılandırmak için çalışma zamanında kullanılan Gradle için temel yapı dosyaları oluşturur. Bu komutu çalışma dizininizden çalıştırın:

```console
gradle init --type basic
```

Bir **DSL**seçmek istendiğinde, **Kotlin**seçin.

## <a name="configure-the-build-file"></a>Yapı dosyasını yapılandırma

En `build.gradle.kts` sevdiğiniz IDE veya metin düzenleyicisi ile bulun ve açın. Sonra bu yapı yapılandırmasında kopyalayın:

```java
plugins {
    java
    application
}
application {
    mainClassName = "GetLanguages"
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
mkdir -p src/main/java
```

Ardından, bu klasörde, adlı `GetLanguages.java`bir dosya oluşturun.

## <a name="import-required-libraries"></a>Gerekli kitaplıkları içe aktarma

Bu `GetLanguages.java` alma deyimlerini açın ve ekleyin:

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
public class GetLanguages {
  // All project code goes here...
}
```

Bu satırları sınıfa `GetLanguages` ekleyin. Abonelik anahtarının ve bitiş noktasının ortam değişkenlerinden okunduğunu fark edeceksiniz:

```java
private static String subscriptionKey = System.getenv("TRANSLATOR_TEXT_SUBSCRIPTION_KEY");
private static String endpoint = System.getenv("TRANSLATOR_TEXT_ENDPOINT");
String url = endpoint + "/languages?api-version=3.0";
```

Bilişsel Hizmetler çok hizmet aboneliği kullanıyorsanız, istek parametrelerinize `Ocp-Apim-Subscription-Region` de eklemeniz gerekir. [Çoklu hizmet aboneliği ile kimlik doğrulama hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)

## <a name="create-a-client-and-build-a-request"></a>İstemci oluşturma ve istek oluşturma

Anında sınıfa `GetLanguages` bu satırı `OkHttpClient`ekleyin:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

Sonra, `GET` isteği oluşturalım.

```java
// This function performs a GET request.
public String Get() throws IOException {
    Request request = new Request.Builder()
            .url(url).get()
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
        GetLanguages getLanguagesRequest = new GetLanguages();
        String response = getLanguagesRequest.Get();
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

Bu dil listesinde ülke/bölge [kısaltmasını](https://docs.microsoft.com/azure/cognitive-services/translator/language-support)bulun.

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
  },
  "transliteration": {
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "scripts": [
        {
          "code": "Arab",
          "name": "Arabic",
          "nativeName": "العربية",
          "dir": "rtl",
          "toScripts": [
            {
              "code": "Latn",
              "name": "Latin",
              "nativeName": "اللاتينية",
              "dir": "ltr"
            }
          ]
        },
        {
          "code": "Latn",
          "name": "Latin",
          "nativeName": "اللاتينية",
          "dir": "ltr",
          "toScripts": [
            {
              "code": "Arab",
              "name": "Arabic",
              "nativeName": "العربية",
              "dir": "rtl"
            }
          ]
        }
      ]
    },
...
  },
  "dictionary": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
...
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Çevirmen Metin API'si ile yapabileceğiniz her şeyi anlamak için API başvurusuna bir göz atın.

> [!div class="nextstepaction"]
> [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
