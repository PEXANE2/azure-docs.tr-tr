---
title: Bir işlevi Azure'da yayımlamak için Java ve Maven/Gradle'yi kullanma
description: Java ve Maven veya Gradle ile Azure'da HTTP tarafından tetiklenen bir işlev oluşturun ve yayımlayın.
author: KarlErickson
ms.author: karler
ms.topic: quickstart
ms.date: 08/10/2018
ms.custom: mvc, devcenter, seo-java-july2019, seo-java-august2019, seo-java-september2019
zone_pivot_groups: java-build-tools-set
ms.openlocfilehash: ad3b38a12020c56c31e03879b3fbcb9a8dda25f1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79136876"
---
# <a name="quickstart-use-java-and-mavengradle-to-create-and-publish-a-function-to-azure"></a>Hızlı başlatma: Azure'da bir işlev oluşturmak ve yayımlamak için Java ve Maven/Gradle'yi kullanın

Bu makalede, Maven/Gradle komut satırı aracıyla Azure İşlevler'e bir Java işlevi oluşturma ve yayımlama şekli gösterilmektedir. İşinizi bitirdiğinizde, işlev kodunuz [Azure'da sunucusuz](functions-scale.md#consumption-plan) bir barındırma planında çalışır ve bir HTTP isteği tarafından tetiklenir.

<!--
> [!NOTE] 
> You can also create a Kotlin-based Azure Functions project by using the azure-functions-kotlin-archetype instead. Visit the [GitHub repository](https://github.com/microsoft/azure-maven-archetypes/tree/develop/azure-functions-kotlin-archetype) for more information.
-->

## <a name="prerequisites"></a>Ön koşullar

Java kullanarak işlev uygulamaları geliştirebilmeniz için şunlar yüklü olmalıdır:

- [Java Developer Kit](https://aka.ms/azure-jdks), sürüm 8
- [Azure CLI]
- [Azure İşlevler Çekirdek Araçları](./functions-run-local.md#v2) sürüm 2.6.666 veya üzeri
::: zone pivot="java-build-tools-maven" 
- [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri
::: zone-end

::: zone pivot="java-build-tools-gradle"  
- [Gradle](https://gradle.org/), sürüm 4.10 ve üzeri
::: zone-end 

Ayrıca etkin bir Azure aboneliğine de ihtiyacınız var. [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


> [!IMPORTANT]
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

## <a name="prepare-a-functions-project"></a>Fonksiyonlar projesi hazırlama

::: zone pivot="java-build-tools-maven" 
İşlevler projesini bir [Maven arketipinden](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) oluşturmak için boş bir klasörde aşağıdaki komutu çalıştırın.

```bash
mvn archetype:generate -DarchetypeGroupId=com.microsoft.azure -DarchetypeArtifactId=azure-functions-archetype 
```

> [!NOTE]
> Powershell kullanıyorsanız, parametrelerin etrafına "" eklemeyi unutmayın.

> [!NOTE]
> Komutu çalıştırmayla ilgili sorunlar yaşıyorsanız, hangi `maven-archetype-plugin` sürümün kullanıldığına bir göz atın. Komutu dosyası olmayan `.pom` boş bir dizinde çalıştırdığınıziçin, Maven'inizi eski bir sürümden yükselttiyseniz eski sürümün `~/.m2/repository/org/apache/maven/plugins/maven-archetype-plugin` eklentisini kullanmaya çalışıyor olabilir. Bu durumda, dizini `maven-archetype-plugin` silmeyi ve komutu yeniden çalıştırmayı deneyin.

Maven, dağıtımı sırasında projeoluşturmayı bitirmek için gereken değerleri sizden sorar. İstendiğinde aşağıdaki değerleri sağlayın:

| Değer | Açıklama |
| ----- | ----------- |
| **groupId** | Java için [paket adlandırma kurallarını](https://docs.oracle.com/javase/specs/jls/se6/html/packages.html#7.7) izleyerek, projenizi tüm projelerde benzersiz olarak tanımlayan bir değer. Bu quickstart kullanımıörnekleri `com.fabrikam.functions`. |
| **Artifactıd** | Bir sürüm numarası olmayan kavanozun adı olan bir değer. Bu quickstart kullanımıörnekleri `fabrikam-functions`. |
| **Sürüm** | `1.0-SNAPSHOT`Varsayılan değerini seçin. |
| **Paket** | Oluşturulan işlev kodu için Java paketi olan bir değer. Varsayılan değeri kullanın. Bu quickstart kullanımıörnekleri `com.fabrikam.functions`. |
| **Uygadı** | Azure'daki yeni işlev uygulamanızı tanımlayan genel olarak benzersiz ad. Rasgele bir sayıyla eklenen _artifakı Olan varsayılanı_ kullanın. Bu değeri not edin, daha sonra ihtiyacınız olacak. |
| **appRegion** | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. Varsayılan değer: `westus`. Tüm bölgelerin listesini almak için bu [Azure CLI] komutunu çalıştırın:<br/>`az account list-locations --query '[].{Name:name}' -o tsv` |
| **resourceGroup** | İşlev uygulamanızı oluşturmak için yeni [kaynak grubunun](../azure-resource-manager/management/overview.md) adı. Bu `myResourceGroup`hızlı başlatmadaki örneklertarafından kullanılan kullanın. Bir kaynak grubunun Azure aboneliğinize özgü olması gerekir.|

Onaylamak `Y` için Enter yazın veya enter tuşuna basın.

Maven, bu örnekte yer alan _artifactId_adında yeni bir klasörde proje dosyalarını `fabrikam-functions`oluşturur. Dizin oluşturulan proje klasörüne değiştirmek için aşağıdaki komutu çalıştırın.
```bash
cd fabrikam-function
```

::: zone-end 
::: zone pivot="java-build-tools-gradle"
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
::: zone-end

Yeni Function.java dosyasını bir metin düzenleyicisinde *src/main/java* yolundan açın ve oluşturulan kodu gözden geçirin. Bu kod, isteğin gövdesini yansıtan bir [HTTP tetikleme](functions-bindings-http-webhook.md) işlevidir. 

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=generate-project)

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Oluşturmak için aşağıdaki komutu çalıştırın ve ardından işlev projesini çalıştırın:

::: zone pivot="java-build-tools-maven" 
```bash
mvn clean package 
mvn azure-functions:run
```
::: zone-end 

::: zone pivot="java-build-tools-gradle"  
```bash
gradle jar --info
gradle azureFunctionsRun
```
::: zone-end 

Projeyi yerel olarak çalıştırdığınızda Azure İşlevler Temel Araçları'ndan aşağıdaki gibi çıktı görürsünüz:

```output
...

Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

    HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
...
```

Yeni bir terminal penceresinde cURL kullanarak komut satırından işlevi tetikle:

```bash
curl -w "\n" http://localhost:7071/api/HttpExample --data AzureFunctions
```

```output
Hello AzureFunctions!
```
Yerel olarak çalışırken [işlev anahtarı](functions-bindings-http-webhook-trigger.md#authorization-keys) gerekli değildir. İşlev kodunu durdurmak için terminalde `Ctrl+C` komutunu kullanın.

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


::: zone pivot="java-build-tools-maven" 
```bash
mvn azure-functions:deploy
```
::: zone-end 

::: zone pivot="java-build-tools-gradle"  
```bash
gradle azureFunctionsDeploy
```
::: zone-end

Bu, Azure'da aşağıdaki kaynakları oluşturur:

+ Kaynak grubu. Sağladığınız _kaynak Grubu_ ile adlandırılır.
+ Depolama hesabı. Fonksiyonlar tarafından gereklidir. Ad, Depolama hesabı adı gereksinimlerine göre rasgele oluşturulur.
+ Uygulama hizmet planı. Belirtilen _appRegion_işlev uygulaması için sunucusuz barındırma . Ad rasgele oluşturulur.
+ Fonksiyon uygulaması. İşlevler için dağıtım ve yürütme birimidir. Adı _appName_, rasgele oluşturulan bir sayı ile eklenen. 

Dağıtım ayrıca proje dosyalarını paketler ve paketten çalıştırma modu etkinleştirilmiş [zip dağıtımını](functions-deployment-technologies.md#zip-deploy)kullanarak yeni işlev uygulamasına dağıtır.

Dağıtım tamamlandıktan sonra, işlev uygulama uç noktalarınıza erişmek için kullanabileceğiniz URL'yi görürsünüz. Yayınladığımız HTTP tetikleyicisi kullandığından, `authLevel = AuthorizationLevel.FUNCTION`işlev bitiş noktasını HTTP'den aramak için işlev anahtarını almanız gerekir. İşlev anahtarını almanın en kolay yolu [Azure portalıdır.]

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=deploy)

## <a name="get-the-http-trigger-url"></a>HTTP tetikleyici URL'sini alın

<!--- We can updates this to remove portal dependency after the Maven archetype returns the full URLs with keys on publish (https://github.com/microsoft/azure-maven-plugins/issues/571). -->

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

```output
Hello AzureFunctions!
```

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=verify-deployment)

## <a name="next-steps"></a>Sonraki adımlar

HTTP tetiklenen işlevi olan bir Java işlevleri projesi oluşturdunuz, yerel makinenizde çalıştırın ve Azure'a dağıttınız. Şimdi, fonksiyonunuzu genişletin ...

> [!div class="nextstepaction"]
> [Azure Depolama sıra çıktısı bağlama ekleme](functions-add-output-binding-storage-queue-java.md)


[Azure CLI]: /cli/azure
[Azure portalında]: https://portal.azure.com
