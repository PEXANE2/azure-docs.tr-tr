---
title: Azure'da bir işlev yayımlamak için Java ve Gradle'yi kullanma
description: Java ve Gradle ile Azure'da HTTP tarafından tetiklenen bir işlev oluşturun ve yayımlayın.
author: KarlErickson
ms.author: karler
ms.topic: how-to
ms.date: 04/08/2020
ms.openlocfilehash: 5e18e035bd237fd489b715986e58d7ede726348d
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886611"
---
# <a name="use-java-and-gradle-to-create-and-publish-a-function-to-azure"></a>Azure'da bir işlev oluşturmak ve yayımlamak için Java ve Gradle'yi kullanın

Bu makalede, Gradle komut satırı aracıyla Azure İşlevleri'ne bir Java işlevi projesi oluşturma ve yayımlama şekli gösterilmektedir. İşinizi bitirdiğinizde, işlev kodunuz [Azure'da sunucusuz](functions-scale.md#consumption-plan) bir barındırma planında çalışır ve bir HTTP isteği tarafından tetiklenir. 

> [!NOTE]
> Gradle tercih geliştirme aracı değilse, [Maven,](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java) [IntelliJ IDEA](/azure/java/intellij/azure-toolkit-for-intellij-quickstart-functions) ve [VS Kodu](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-java)kullanarak Java devlopers için benzer öğreticiler göz atın.

## <a name="prerequisites"></a>Ön koşullar

Java kullanarak işlev uygulamaları geliştirebilmeniz için şunlar yüklü olmalıdır:

- [Java Developer Kit](https://aka.ms/azure-jdks), sürüm 8
- [Azure CLI]
- [Azure İşlevler Çekirdek Araçları](./functions-run-local.md#v2) sürüm 2.6.666 veya üzeri
- [Gradle](https://gradle.org/), sürüm 4.10 ve üzeri

Ayrıca etkin bir Azure aboneliğine de ihtiyacınız var. [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

## <a name="prepare-a-functions-project"></a>Fonksiyonlar projesi hazırlama

Örnek projeyi klonlamak için aşağıdaki komutu kullanın:

```bash
git clone https://github.com/Azure-Samples/azure-functions-samples-java.git
cd azure-functions-samples-java/
```

Azure'a dağıtılırken etki alanı adı çakışmasını önlemek için aşağıdaki bölümü açın `build.gradle` ve aşağıdaki bölümde benzersiz bir adla değiştirin. `appName` 

```gradle
azurefunctions {
    resourceGroup = 'java-functions-group'
    appName = 'azure-functions-sample-demo'
    pricingTier = 'Consumption'
    region = 'westus'
    runtime {
      os = 'windows'
    }
    localDebug = "transport=dt_socket,server=y,suspend=n,address=5005"
}
```

Yeni Function.java dosyasını bir metin düzenleyicisinde *src/main/java* yolundan açın ve oluşturulan kodu gözden geçirin. Bu kod, isteğin gövdesini yansıtan bir [HTTP tetikleme](functions-bindings-http-webhook.md) işlevidir. 

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=generate-project)

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Oluşturmak için aşağıdaki komutu çalıştırın ve ardından işlev projesini çalıştırın:

```bash
gradle jar --info
gradle azureFunctionsRun
```
Projeyi yerel olarak çalıştırdığınızda Azure İşlevler Temel Araçları'ndan aşağıdaki gibi çıktı görürsünüz:

<pre>
...

Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

    HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
...
</pre>

Yeni bir terminal penceresinde aşağıdaki cURL komutunu kullanarak komut satırından işlevi tetikle:

```bash
curl -w "\n" http://localhost:7071/api/HttpExample --data AzureFunctions
```

Beklenen çıktı aşağıdaki gibidir:

<pre>
Hello AzureFunctions!
</pre>

Yerel olarak çalışırken [işlev anahtarı](functions-bindings-http-webhook-trigger.md#authorization-keys) gerekli değildir.  
İşlev kodunu durdurmak için terminalde `Ctrl+C` komutunu kullanın.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=local-run)

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

İşlev uygulamanızı ilk dağıttığınızda Azure'da bir işlev uygulaması ve ilgili kaynaklar oluşturulur. Dağıtmadan önce Azure aboneliğinizde oturum açmak için [az giriş](/cli/azure/authenticate-azure-cli) Azure CLI komutunu kullanın. 

```azurecli
az login
```

> [!TIP]
> Hesabınız birden çok aboneye erişebiliyorsa, bu oturum için varsayılan aboneliği ayarlamak için [az hesap kümesini](/cli/azure/account#az-account-set) kullanın. 

Projenizi yeni bir işlev uygulamasına dağıtmak için aşağıdaki komutu kullanın. 

```bash
gradle azureFunctionsDeploy
```

Bu, yapı.gradle dosyasındaki değerleri temel alan Azure'da aşağıdaki kaynakları oluşturur:

+ Kaynak grubu. Sağladığınız _kaynak Grubu_ ile adlandırılır.
+ Depolama hesabı. Fonksiyonlar tarafından gereklidir. Ad, Depolama hesabı adı gereksinimlerine göre rasgele oluşturulur.
+ Uygulama Hizmeti planı. Belirtilen _appRegion'daki_işlev uygulamanız için Sunucusuz Tüketim planı barındırma. Ad rasgele oluşturulur.
+ Fonksiyon uygulaması. İşlevler için dağıtım ve yürütme birimidir. Adı _appName_, rasgele oluşturulan bir sayı ile eklenen. 

Dağıtım ayrıca proje dosyalarını paketler ve paketten çalıştırma modu etkinleştirilmiş [zip dağıtımını](functions-deployment-technologies.md#zip-deploy)kullanarak yeni işlev uygulamasına dağıtır.

Yayınladığımız HTTP tetikleyicisi kullandığından, `authLevel = AuthorizationLevel.FUNCTION`işlev bitiş noktasını HTTP'den aramak için işlev anahtarını almanız gerekir. İşlev anahtarını almanın en kolay yolu [Azure portalıdır.]

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=deploy)

## <a name="get-the-http-trigger-url"></a>HTTP tetikleyici URL'sini alın

İşlev anahtarıyla işlevinizi tetiklemek için gereken URL'yi Azure portalından alabilirsiniz. 

1. [Azure portalına]göz atın , oturum açın, işlev uygulamanızın _appName'sini_ sayfanın üst **kısmındaki Arama'ya** yazın ve enter tuşuna basın.
 
1. İşlev **uygulamanızda, İşlevlerinizi genişletin (Salt oku)** işlevinizi seçin, ardından sağ üstteki **</> İşlev URL'sini alın'ı** seçin. 

    ![Azure portalından işlev URL’sini kopyalama](./media/functions-create-java-maven/get-function-url-portal.png)

1. **Varsayılan (İşlev tuşu) seçin** ve **Kopyala'yı**seçin. 

Artık işlevinize erişmek için kopyalanan URL'yi kullanabilirsiniz.

## <a name="verify-the-function-in-azure"></a>Azure'da işlevi doğrulama

Azure'da çalışan işlev uygulamasını `cURL`kullanarak doğrulamak için aşağıdaki örnekteki URL'yi portaldan kopyaladığınız URL ile değiştirin.

```console
curl -w "\n" https://fabrikam-functions-20190929094703749.azurewebsites.net/api/HttpExample?code=zYRohsTwBlZ68YF.... --data AzureFunctions
```

Bu, isteğin gövdesinde olan `AzureFunctions` işlev bitiş noktasına bir POST isteği gönderir. Aşağıdaki yanıtı görürsünüz.

<pre>
Hello AzureFunctions!
</pre>

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=verify-deployment)

## <a name="next-steps"></a>Sonraki adımlar

HTTP tetiklenen işlevi olan bir Java işlevleri projesi oluşturdunuz, yerel makinenizde çalıştırın ve Azure'a dağıttınız. Şimdi, fonksiyonunuzu genişletin ...

> [!div class="nextstepaction"]
> [Azure Depolama sıra çıktısı bağlama ekleme](functions-add-output-binding-storage-queue-java.md)


[Azure CLI]: /cli/azure
[Azure portalı]: https://portal.azure.com
