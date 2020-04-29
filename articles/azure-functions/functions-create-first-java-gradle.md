---
title: Azure 'da bir işlev yayımlamak için Java ve Gradle kullanma
description: Java ve Gradle ile Azure 'da HTTP ile tetiklenen bir işlev oluşturun ve yayımlayın.
author: KarlErickson
ms.author: karler
ms.topic: how-to
ms.date: 04/08/2020
ms.openlocfilehash: 34aab24bf39e387715cfa5783b801d45ed488750
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81732720"
---
# <a name="use-java-and-gradle-to-create-and-publish-a-function-to-azure"></a>Azure 'da bir işlev oluşturmak ve yayımlamak için Java ve Gradle kullanma

Bu makalede, Gradle komut satırı aracıyla Azure Işlevleri 'nde bir Java işlevi projesi oluşturma ve yayımlama işlemi gösterilmektedir. İşiniz bittiğinde, işlev kodunuz Azure 'da [sunucusuz bir barındırma planında](functions-scale.md#consumption-plan) çalışır ve bir http isteği tarafından tetiklenir. 

> [!NOTE]
> Gradle, tercih edilmemiş geliştirme aracınız değilse [Maven](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java), [IntelliJ fikrini](/azure/developer/java/toolkit-for-intellij/quickstart-functions) ve [vs Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-java)kullanarak Java geliştiricileri için benzer öğreticilerimize göz atın.

## <a name="prerequisites"></a>Ön koşullar

Java kullanarak işlev uygulamaları geliştirebilmeniz için şunlar yüklü olmalıdır:

- [Java Developer Kit](https://aka.ms/azure-jdks), sürüm 8
- [Azure CLI]
- [Azure Functions Core Tools](./functions-run-local.md#v2) sürüm 2.6.666 veya üzeri
- [Gradle](https://gradle.org/), sürüm 4,10 ve üzeri

Ayrıca etkin bir Azure aboneliğine de ihtiyacınız vardır. [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

## <a name="prepare-a-functions-project"></a>Işlevler projesi hazırlama

Örnek projeyi kopyalamak için aşağıdaki komutu kullanın:

```bash
git clone https://github.com/Azure-Samples/azure-functions-samples-java.git
cd azure-functions-samples-java/
```

' `build.gradle` İ açın ve `appName` Azure 'a dağıtma sırasında etki alanı adı çakışmasını önlemek için aşağıdaki bölümde yer alan öğesini benzersiz bir adla değiştirin. 

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

Yeni Function. Java dosyasını bir metin düzenleyicisinde *src/Main/Java* yolundan açın ve oluşturulan kodu gözden geçirin. Bu kod, isteğin gövdesini yansıtan [http ile tetiklenen](functions-bindings-http-webhook.md) bir işlevdir. 

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=generate-project)

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

İşlev projesini oluşturmak için aşağıdaki komutu çalıştırın:

```bash
gradle jar --info
gradle azureFunctionsRun
```
Projeyi yerel olarak çalıştırdığınızda Azure Functions Core Tools aşağıdakine benzer bir çıktı görürsünüz:

<pre>
...

Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

    HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
...
</pre>

Yeni bir Terminal penceresinde aşağıdaki kıvrımlı komutunu kullanarak işlevi komut satırından tetikleyin:

```bash
curl -w "\n" http://localhost:7071/api/HttpExample --data AzureFunctions
```

Beklenen çıkış şunlardır:

<pre>
Hello AzureFunctions!
</pre>

Yerel olarak çalıştırılırken [işlev anahtarı](functions-bindings-http-webhook-trigger.md#authorization-keys) gerekli değildir.  
İşlev kodunu durdurmak için terminalde `Ctrl+C` komutunu kullanın.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=local-run)

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

İşlev uygulamanızı ilk kez dağıttığınızda Azure 'da bir işlev uygulaması ve ilgili kaynaklar oluşturulur. Dağıtmadan önce, Azure aboneliğinizde oturum açmak için [az Login](/cli/azure/authenticate-azure-cli) Azure CLI komutunu kullanın. 

```azurecli
az login
```

> [!TIP]
> Hesabınız birden çok aboneliğe erişebildeyse, bu oturum için varsayılan aboneliği ayarlamak üzere [az Account set](/cli/azure/account#az-account-set) kullanın. 

Projenizi yeni bir işlev uygulamasına dağıtmak için aşağıdaki komutu kullanın. 

```bash
gradle azureFunctionsDeploy
```

Bu, Build. Gradle dosyasındaki değerlere bağlı olarak aşağıdaki kaynakları Azure 'da oluşturur:

+ Kaynak grubu. Sağladığınız _resourceGroup_ ile adlandırılmış.
+ Depolama hesabı. Işlevleri için gereklidir. Ad, depolama hesabı adı gereksinimlerine göre rastgele oluşturulur.
+ Planı App Service. Belirtilen _Appregion_'da işlev uygulamanız için barındırılan sunucusuz tüketim planı. Ad rastgele oluşturulur.
+ İşlev uygulaması. İşlev uygulaması, işlevleriniz için dağıtım ve yürütme birimidir. Ad, rastgele oluşturulmuş bir sayıyla eklenmiş olan _uygulamadır_. 

Dağıtım ayrıca proje dosyalarını paketleyip, paket Çalıştır modu etkin olan [ZIP dağıtımını](functions-deployment-technologies.md#zip-deploy)kullanarak yeni işlev uygulamasına dağıtır.

Yayımladığımız HTTP tetikleyicisi kullandığından `authLevel = AuthorizationLevel.FUNCTION`, http üzerinden işlev uç noktasını çağırmak için işlev anahtarını almanız gerekir. İşlev anahtarını almanın en kolay yolu [Azure Portal].

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=deploy)

## <a name="get-the-http-trigger-url"></a>HTTP tetikleyici URL 'sini al

İşlevinizi tetiklemek için gereken URL 'yi, Azure portal işlev anahtarıyla alabilirsiniz. 

1. [Azure Portal]gidin, oturum açın, sayfanın üst kısmında **arama** yapmak için işlev uygulamanızın _appname_ öğesini yazın ve ENTER tuşuna basın.
 
1. İşlev uygulamanızda Işlevler ' i genişletin **(salt okunurdur)**, işlevinizi seçin, sonra **</> Işlev URL 'sini al** ' ı seçin. 

    ![Azure portalından işlev URL’sini kopyalama](./media/functions-create-java-maven/get-function-url-portal.png)

1. **Varsayılan (işlev anahtarı)** öğesini seçin ve **Kopyala**' yı seçin. 

Şimdi, işlevinizi erişmek için kopyalanmış URL 'YI kullanabilirsiniz.

## <a name="verify-the-function-in-azure"></a>Azure 'da işlevi doğrulama

Kullanarak `cURL`Azure 'da çalışan işlev uygulamasını doğrulamak için, aşağıdaki örnekteki URL 'yi portaldan kopyaladığınız URL ile değiştirin.

```console
curl -w "\n" https://fabrikam-functions-20190929094703749.azurewebsites.net/api/HttpExample?code=zYRohsTwBlZ68YF.... --data AzureFunctions
```

Bu, isteğin gövdesinde bulunan `AzureFunctions` işlev uç noktasına bir post isteği gönderir. Aşağıdaki yanıtı görürsünüz.

<pre>
Hello AzureFunctions!
</pre>

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=verify-deployment)

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen bir işlev içeren bir Java işlevleri projesi oluşturdunuz, yerel makinenizde çalıştırın ve Azure 'a dağıttınız. Şimdi, işlevinizi genişletin...

> [!div class="nextstepaction"]
> [Azure depolama kuyruğu çıkış bağlaması ekleme](functions-add-output-binding-storage-queue-java.md)


[Azure CLI]: /cli/azure
[Azure portal]: https://portal.azure.com
