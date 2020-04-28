---
title: Linux üzerinde özel görüntü kullanarak Azure Işlevleri oluşturma
description: Özel bir Linux görüntüsü üzerinde çalışan Azure İşlevleri oluşturmayı öğrenin.
ms.date: 03/30/2020
ms.topic: tutorial
ms.custom: mvc
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: fee4e16bd77664e541eeb36cb807a77d13191899
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "82165731"
---
# <a name="create-a-function-on-linux-using-a-custom-container"></a>Linux üzerinde özel kapsayıcı kullanarak bir işlev oluşturma

Bu öğreticide, bir Linux temel görüntüsü kullanarak kodunuzu oluşturup Azure Işlevlerine özel bir Docker kapsayıcısı olarak dağıtırsınız. İşlevleriniz belirli bir dil sürümü gerektirdiğinde veya yerleşik görüntü tarafından sağlanmayan belirli bir bağımlılığı ya da yapılandırmaya sahip olduğunda genellikle özel bir görüntü kullanırsınız.

[Linux üzerinde barındırılan ilk işlevinizi oluşturma](functions-create-first-azure-function-azure-cli-linux.md)konusunda açıklandığı gibi varsayılan bir Azure App Service kapsayıcısını de kullanabilirsiniz. Azure Işlevleri için desteklenen temel görüntüler, [Azure işlevleri temel görüntüler](https://hub.docker.com/_/microsoft-azure-functions-base)deposunda bulunur.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Functions Core Tools kullanarak bir işlev uygulaması ve Dockerfile oluşturun.
> * Docker kullanarak özel bir görüntü oluşturun.
> * Özel görüntüyü bir kapsayıcı kayıt defterinde yayımlayın.
> * İşlev uygulaması için Azure 'da destekleyici kaynaklar oluşturma
> * Docker Hub’dan bir işlev uygulaması dağıtın.
> * Uygulama ayarlarını işlevi uygulamasına ekleyin.
> * Sürekli dağıtımı etkinleştirin.
> * Kapsayıcıya SSH bağlantılarını etkinleştirin.
> * Kuyruk depolama çıkış bağlaması ekleyin. 

Bu öğreticiyi Windows, macOS veya Linux çalıştıran herhangi bir bilgisayarda takip edebilirsiniz. Öğreticiyi tamamlamak, Azure hesabınızda birkaç ABD Doları maliyetlerinden oluşur.

[!INCLUDE [functions-requirements-cli](../../includes/functions-requirements-cli.md)]

<!---Requirements specific to Docker --->
+ [Docker](https://docs.docker.com/install/)  

+ Bir [Docker kimliği](https://hub.docker.com/signup)

[!INCLUDE [functions-cli-verify-prereqs](../../includes/functions-cli-verify-prereqs.md)]

+ Docker 'da oturum açmak için ' i çalıştırın `docker login` . Docker çalışmıyorsa bu komut başarısız olur ve bu durumda Docker başlatılır ve komutu yeniden deneyin.

[!INCLUDE [functions-cli-create-venv](../../includes/functions-cli-create-venv.md)]

## <a name="create-and-test-the-local-functions-project"></a>Yerel işlevler projesi oluşturma ve test etme

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
Bir Terminal veya komut isteminde, adlı `LocalFunctionsProject`bir klasörde işlev uygulaması projesi oluşturmak için seçtiğiniz dil için aşağıdaki komutu çalıştırın.  
::: zone-end  
::: zone pivot="programming-language-csharp"  
```
func init LocalFunctionsProject --worker-runtime dotnet --docker
```
::: zone-end  
::: zone pivot="programming-language-javascript"  
```
func init LocalFunctionsProject --worker-runtime node --language javascript --docker
```
::: zone-end  
::: zone pivot="programming-language-powershell"  
```
func init LocalFunctionsProject --worker-runtime powershell --docker
```
::: zone-end  
::: zone pivot="programming-language-python"  
```
func init LocalFunctionsProject --worker-runtime python --docker
```
::: zone-end  
::: zone pivot="programming-language-typescript"  
```
func init LocalFunctionsProject --worker-runtime node --language typescript --docker
```
::: zone-end
::: zone pivot="programming-language-java"  
İşlevler projesini bir [Maven arketipinden](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) oluşturmak için boş bir klasörde aşağıdaki komutu çalıştırın.

# <a name="bash"></a>[Bash](#tab/bash)
```bash
mvn archetype:generate -DarchetypeGroupId=com.microsoft.azure -DarchetypeArtifactId=azure-functions-archetype -Ddocker
```
# <a name="powershell"></a>[PowerShell](#tab/powershell)
```powershell
mvn archetype:generate "-DarchetypeGroupId=com.microsoft.azure" "-DarchetypeArtifactId=azure-functions-archetype" "-Ddocker"
```
# <a name="cmd"></a>[Cmd](#tab/cmd)
```cmd
mvn archetype:generate "-DarchetypeGroupId=com.microsoft.azure" "-DarchetypeArtifactId=azure-functions-archetype" "-Ddocker"
```
---

Maven, dağıtımda projenin oluşturulmasını tamamlaması için gereken değerleri ister.   
İstendiğinde aşağıdaki değerleri sağlayın:

| İstem | Değer | Açıklama |
| ------ | ----- | ----------- |
| **groupId** | `com.fabrikam` | Java için [paket adlandırma kurallarını](https://docs.oracle.com/javase/specs/jls/se6/html/packages.html#7.7) takip eden tüm projelerde projenizi benzersiz bir şekilde tanımlayan bir değer. |
| **ArtifactId** | `fabrikam-functions` | Bir sürüm numarası olmadan jar adı olan bir değer. |
| **Sürüm** | `1.0-SNAPSHOT` | Varsayılan değeri seçin. |
| **leyebilir** | `com.fabrikam.functions` | Oluşturulan işlev kodu için Java paketi olan bir değer. Varsayılan değeri kullanın. |

Onaylamak `Y` için yazın veya ENTER tuşuna basın.

Maven, proje dosyalarını, bu örnekte olduğu `fabrikam-functions`gibi, _ArtifactId_adında yeni bir klasörde oluşturur. 
::: zone-end
`--docker` Seçeneği, Azure işlevleri `Dockerfile` ve seçilen çalışma zamanı ile kullanılmak üzere uygun bir özel kapsayıcıyı tanımlayan proje için bir oluşturur.

Proje klasörüne gidin:
::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
```
cd LocalFunctionsProject
```
::: zone-end  
::: zone pivot="programming-language-java"  
```
cd fabrikam-functions
```
::: zone-end  
::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python" 
Aşağıdaki komutu kullanarak projenize bir işlev ekleyin; burada `--name` bağımsız değişken işlevinizin benzersiz adıdır ve `--template` bağımsız değişken işlevin tetikleyicisini belirtir. `func new`Projenin seçtiği dile uygun bir kod dosyası ve *function. JSON*adlı bir yapılandırma dosyası içeren işlev adıyla eşleşen bir alt klasör oluşturun.

```
func new --name HttpExample --template "HTTP trigger"
```
::: zone-end  
İşlevi yerel olarak test etmek için, proje klasörünün kökündeki yerel Azure Işlevleri çalışma zamanı konağını başlatın: 
::: zone pivot="programming-language-csharp"  
```
func start --build  
```
::: zone-end  
::: zone pivot="programming-language-javascript,programming-language-powershell,programming-language-python"   
```
func start  
```
::: zone-end  
::: zone pivot="programming-language-typescript"  
```
npm install
npm start
```
::: zone-end  
::: zone pivot="programming-language-java"  
```
mvn clean package  
mvn azure-functions:run
```
::: zone-end
`HttpExample` Uç noktanın çıktıda göründüğünü gördüğünüzde öğesine `http://localhost:7071/api/HttpExample?name=Functions`gidin. Tarayıcı `Functions`, `name` sorgu parametresine sağlanan değeri gösteren bir "Merhaba" iletisi görüntülemelidir.

Konağı durdurmak için **CTRL**-**C** 'yi kullanın.

## <a name="build-the-container-image-and-test-locally"></a>Kapsayıcı görüntüsünü oluşturma ve yerel olarak test etme

Seçim Proje klasörünün kökündeki * Dockerfile "bölümünü inceleyin. Dockerfile, Linux üzerinde işlev uygulamasını çalıştırmak için gerekli ortamı açıklar.  Azure Işlevleri için desteklenen temel görüntülerin tüm listesi, [Azure işlevleri temel görüntü sayfasında](https://hub.docker.com/_/microsoft-azure-functions-base)bulunabilir.
    
Kök proje klasöründe [Docker Build](https://docs.docker.com/engine/reference/commandline/build/) komutunu çalıştırın ve bir ad, `azurefunctionsimage`, ve etiketi belirtin. `v1.0.0` `<DOCKER_ID>` değerini Docker Hub hesabınızın kimliğiyle değiştirin. Bu komut, kapsayıcı için Docker görüntüsünü derler.

```
docker build --tag <DOCKER_ID>/azurefunctionsimage:v1.0.0 .
```

Komut tamamlandığında, yeni kapsayıcıyı yerel olarak çalıştırabilirsiniz.
    
Derlemeyi test etmek için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanarak görüntüyü yerel bir kapsayıcıda çalıştırın, DOCKER Kimliğiniz ile yeniden `<DOCKER_ID` değiştirin ve bağlantı noktası bağımsız değişkenini ekleyin: `-p 8080:80`

```
docker run -p 8080:80 -it <docker_id>/azurefunctionsimage:v1.0.0
```

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
Görüntü yerel kapsayıcıda çalışmaya başladıktan sonra, bir tarayıcı açın `http://localhost:8080`ve aşağıda gösterilen yer tutucu görüntüsünü görüntülemesi gerekir. İşleviniz, Azure 'da olduğu gibi yerel kapsayıcıda çalıştığı için görüntü bu noktada görünür, yani bu, `"authLevel": "function"` özelliği ile birlikte *function. JSON* içinde tanımlanan bir erişim anahtarı tarafından korunur. Kapsayıcı henüz Azure 'da bir işlev uygulamasına yayımlanmadı, bu nedenle anahtar henüz kullanılamıyor. Yerel kapsayıcıya karşı test etmek istiyorsanız Docker 'ı durdurun, yetkilendirme özelliğini olarak `"authLevel": "anonymous"`değiştirin, görüntüyü yeniden derleyin ve Docker 'ı yeniden başlatın. Sonra `"authLevel": "function"` *function. JSON*' da sıfırlayın. Daha fazla bilgi için bkz. [Yetkilendirme anahtarları](functions-bindings-http-webhook-trigger.md#authorization-keys).

![Kapsayıcının yerel olarak çalıştığını gösteren yer tutucu resim](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

::: zone-end
::: zone pivot="programming-language-java"  
Görüntü yerel kapsayıcıda çalışmaya başladıktan sonra, daha önce olduğu gibi aynı `http://localhost:8080/api/HttpExample?name=Functions`"Merhaba" iletisini görüntülemesi gereken öğesine gidin. Maven arşiv ETYPE, anonim yetkilendirme kullanan bir HTTP ile tetiklenen bir işlev oluşturduğundan, kapsayıcıda çalışıyor olsa bile işlevi çağırabilirsiniz. 
::: zone-end  

Kapsayıcıda işlev uygulamasını doğruladıktan sonra, **CTRL**+**C**ile Docker 'ı durdurun.

## <a name="push-the-image-to-docker-hub"></a>Görüntüyü Docker Hub 'a gönderme

Docker Hub, görüntüleri barındıran ve görüntü ve kapsayıcı hizmetleri sağlayan bir kapsayıcı kayıt defteridir. Azure 'a dağıtma da dahil olmak üzere görüntünüzü paylaşmak için bir kayıt defterine göndermeniz gerekir.

1. Docker 'da henüz oturum açmadıysanız [, DOCKER](https://docs.docker.com/engine/reference/commandline/login/) ID ile değiştirin `<docker_id>` . Bu komut sizden Kullanıcı adı ve parola girmenizi ister. "Oturum açma başarılı oldu" iletisi oturum açtığınızdan emin olur.

    ```
    docker login
    ```
    
1. Oturum açtıktan sonra Docker [Push](https://docs.docker.com/engine/reference/commandline/push/) komutunu kullanarak görüntüyü Docker Hub 'a gönderin, yeniden Docker Kimliğiniz ile değiştirin `<docker_id>` .

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.0
    ```

1. Ağ hızınıza bağlı olarak, görüntüyü ilk kez göndermek birkaç dakika sürebilir (sonraki değişiklikler çok daha hızlıdır). Beklerken, bir sonraki bölüme devam edebilir ve Azure kaynaklarını başka bir terminalde oluşturabilirsiniz.

## <a name="create-supporting-azure-resources-for-your-function"></a>İşleviniz için destekleyici Azure kaynakları oluşturma

İşlev kodunuzu Azure 'a dağıtmak için üç kaynak oluşturmanız gerekir:

- İlgili kaynaklar için mantıksal kapsayıcı olan bir kaynak grubu.
- Projelerinizle ilgili durumu ve diğer bilgileri tutan bir Azure depolama hesabı.
- İşlev kodunuzun yürütülmesi için ortam sağlayan bir Azure işlevleri uygulaması. Bir işlev uygulaması yerel işlev projenize eşlenir ve kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanızı sağlar.

Bu öğeleri oluşturmak için Azure CLı komutlarını kullanırsınız. Her komut, tamamlandıktan sonra JSON çıktısı sağlar.

1. [Az Login](/cli/azure/reference-index#az-login) komutuyla Azure 'da oturum açın:

    ```azurecli
    az login
    ```
    
1. [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, `AzureFunctionsContainers-rg` `westeurope` bölgesinde adlı bir kaynak grubu oluşturur. (Bir bölgede kullanılabilir bir bölge `az account list-locations` kullanarak, genellikle kaynak grubunuzu ve kaynaklarınızı size yakın bir bölgede oluşturursunuz.)

    ```azurecli
    az group create --name AzureFunctionsContainers-rg --location westeurope
    ```
    
    > [!NOTE]
    > Linux ve Windows uygulamalarını aynı kaynak grubunda barındıralamazsınız. Bir Windows işlev uygulaması veya Web uygulaması ile `AzureFunctionsContainers-rg` adlandırılmış bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.
    
1. [Az Storage Account Create](/cli/azure/storage/account#az-storage-account-create) komutunu kullanarak kaynak grubunuzda ve bölgenizde genel amaçlı bir depolama hesabı oluşturun. Aşağıdaki örnekte, öğesini sizin için `<storage_name>` uygun olan bir genel benzersiz adla değiştirin. Adlar yalnızca üç ile 24 karakter arasında ve küçük harflerden oluşmalıdır. `Standard_LRS`tipik bir genel amaçlı hesabı belirtir.

    ```azurecli
    az storage account create --name <storage_name> --location westeurope --resource-group AzureFunctionsContainers-rg --sku Standard_LRS
    ```
    
    Depolama hesabı, bu öğretici için yalnızca birkaç USD ilay.
    
1. Bu komutu kullanarak, **elastik Premium 1** fiyatlandırma katmanında (`--sku EP1`), Batı Avrupa bölgesinde (`-location westeurope`veya size yakın uygun bir bölge kullanın) ve bir Linux kapsayıcısında (`--is-linux`) adlı `myPremiumPlan` Azure işlevleri için Premium bir plan oluşturun.

    ```azurecli
    az functionapp plan create --resource-group AzureFunctionsContainers-rg --name myPremiumPlan --location westeurope --number-of-workers 1 --sku EP1 --is-linux
    ```   

    Özel işlevler kapsayıcıları için Linux barındırma, [adanmış (App Service) planlar](functions-scale.md#app-service-plan) ve [Premium planlarda](functions-premium-plan.md#features)desteklenir. Burada, gerektiğinde ölçeklenebilen Premium planı kullanıyoruz. Barındırma hakkında daha fazla bilgi edinmek için, bkz. [Azure İşlevleri barındırma planları karşılaştırması](functions-scale.md). Maliyetleri hesaplamak için bkz. [işlevler fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/).

    Bu komut Ayrıca, aynı kaynak grubunda, işlev uygulamanızı izleyebilmeniz ve günlükleri görüntüleyebileceğiniz ilişkili bir Azure Application Insights örneğini sağlar. Daha fazla bilgi için bkz. [Azure Işlevlerini izleme](functions-monitoring.md). Örnek, siz etkinleştirene kadar hiçbir maliyet vermez.

## <a name="create-and-configure-a-function-app-on-azure-with-the-image"></a>Azure 'da görüntüyle bir işlev uygulaması oluşturma ve yapılandırma

Azure 'daki bir işlev uygulaması, barındırma planınızdaki işlevlerinizin yürütülmesini yönetir. Bu bölümde, Docker Hub 'daki görüntüden bir işlev uygulaması oluşturmak ve bunu Azure depolama 'ya bir bağlantı dizesiyle yapılandırmak için önceki bölümde yer alan Azure kaynaklarını kullanırsınız.

1. [Az functionapp Create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak işlevler uygulamasını oluşturun. Aşağıdaki örnekte, değerini depolama hesabı `<storage_name>` için önceki bölümde kullandığınız adla değiştirin. Ayrıca, `<app_name>` sizin için uygun bir genel benzersiz adla ve `<docker_id>` Docker Kimliğiniz ile değiştirin.

    ```azurecli
    az functionapp create --name <app_name> --storage-account <storage_name> --resource-group AzureFunctionsContainers-rg --plan myPremiumPlan --deployment-container-image-name <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
    *Dağıtım-kapsayıcı-görüntü-adı* parametresi, işlev uygulaması için kullanılacak resmi belirtir. Dağıtım için kullanılan görüntü hakkındaki bilgileri görüntülemek için [az functionapp config Container Show](/cli/azure/functionapp/config/container#az-functionapp-config-container-show) komutunu kullanabilirsiniz. Farklı bir görüntüden dağıtmak için [az functionapp config Container set](/cli/azure/functionapp/config/container#az-functionapp-config-container-set) komutunu da kullanabilirsiniz.

1. [Az Storage Account Show-Connection-String](/cli/azure/storage/account) komutunu kullanarak oluşturduğunuz depolama hesabı için bağlantı dizesini bir Shell değişkenine `storageConnectionString`atayarak alın:

    ```azurecli
    az storage account show-connection-string --resource-group AzureFunctionsContainers-rg --name <storage_name> --query connectionString --output tsv
    ```
    
1. Bu ayarı, [az functionapp config appSettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) komutunu kullanarak işlev uygulamasına ekleyin. Aşağıdaki komutta, öğesini işlev uygulamanızın `<app_name>` adıyla değiştirin ve önceki adımdaki bağlantı dizesiyle değiştirin `<connection_string>` ("defaultendpointprotocol =" ile başlayan uzun kodlanmış bir dize):
 
    ```azurecli
    az functionapp config appsettings set --name <app_name> --resource-group AzureFunctionsContainers-rg --settings AzureWebJobsStorage=<connection_string>
    ```

1. Bu işlev artık depolama hesabına erişmek için bu bağlantı dizesini kullanabilir.

    > [!TIP]
    > Bash 'de, pano kullanmak yerine bağlantı dizesini yakalamak için bir Shell değişkeni kullanabilirsiniz. İlk olarak, bağlantı dizesiyle bir değişken oluşturmak için aşağıdaki komutu kullanın:
    > 
    > ```bash
    > storageConnectionString=$(az storage account show-connection-string --resource-group AzureFunctionsContainers-rg --name <storage_name> --query connectionString --output tsv)
    > ```
    > 
    > Ardından ikinci komutta değişkenine başvurun:
    > 
    > ```azurecli
    > az functionapp config appsettings set --name <app_name> --resource-group AzureFunctionsContainers-rg --settings AzureWebJobsStorage=$storageConnectionString
    > ```

> [!NOTE]    
> Özel görüntünüzü özel bir kapsayıcı hesabına yayımlarsanız, bunun yerine bağlantı dizesinin Dockerfile içindeki ortam değişkenlerini kullanmanız gerekir. Daha fazla bilgi için bkz. [env yönergesi](https://docs.docker.com/engine/reference/builder/#env). Ayrıca, ve `DOCKER_REGISTRY_SERVER_USERNAME` `DOCKER_REGISTRY_SERVER_PASSWORD`değişkenlerini de ayarlamanız gerekir. Değerleri kullanmak için, görüntüyü yeniden oluşturmanız, görüntüyü kayıt defterine göndermeniz ve sonra işlev uygulamasını Azure 'da yeniden başlatmanız gerekir.

## <a name="verify-your-functions-on-azure"></a>Azure 'da işlevlerinizi doğrulama

Azure 'da işlev uygulamasına dağıtılan görüntü ile, artık işlevi HTTP istekleri aracılığıyla çalıştırabilirsiniz. *Function. JSON* tanımı özelliğini `"authLevel": "function"`içerdiğinden, önce erişim anahtarını edinmeniz ("işlev anahtarı" olarak da bilinir) ve bunu uç noktaya herhangi bir istekte URL parametresi olarak eklemeniz gerekir.

1. Azure portal kullanarak veya `az rest` komutuyla Azure CLI kullanarak işlev URL 'sini erişim (işlev) anahtarıyla alın.)

    # <a name="portal"></a>[Portal](#tab/portal)

    1. Azure portal oturum açın ve ardından sayfanın üst kısmındaki **arama** kutusuna işlev uygulamanızın adını girerek işlev uygulamanızı bulun. Sonuçlarda **App Service** kaynağını seçin.

    1. Sol gezinti panelinde, **işlevler (salt okuma)** altında, işlevinizin adını seçin.

    1. Ayrıntılar panelinde **</> işlev URL 'Sini al**' ı seçin:
    
        ![Azure portal işlev URL 'sini Al komutu](./media/functions-create-function-linux-custom-image/functions-portal-get-url-key.png)   

    1. Açılan pencerede **varsayılan (işlev anahtarı)** öğesini seçin ve sonra **kopyalayın**. Anahtar, aşağıdaki `?code=`karakter dizesidir.

        ![Azure portal işlev URL 'SI kopyalanıyor](./media/functions-create-function-linux-custom-image/functions-portal-get-url-key-popup.png)   

    > [!NOTE]  
    > İşlev uygulamanız bir kapsayıcı olarak dağıtıldığından, portalda işlev kodunuzda değişiklik yapamazsınız. Bunun yerine yerel görüntüde projeyi güncelleştirmeniz, görüntüyü kayıt defterine yeniden göndermeniz ve sonra Azure 'a yeniden dağıtmanız gerekir. Sonraki bölümde sürekli dağıtım ayarlayabilirsiniz.
    
    # <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

    1. Aşağıdaki biçimde bir URL dizesi oluşturun,, ve `<subscription_id>` `<resource_group>` `<app_name>` yerine Azure abonelik kimliğinizi, işlev uygulamanızın kaynak grubunu ve işlev uygulamanızın adını sırasıyla değiştirin:

        ```
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Web/sites/<app_name>/host/default/listKeys?api-version=2018-11-01"
        ```

        Örneğin, URL şu adrese benzeyebilir:

        ```
        "/subscriptions/1234aaf4-1234-abcd-a79a-245ed34eabcd/resourceGroups/AzureFunctionsContainers-rg/providers/Microsoft.Web/sites/msdocsfunctionscontainer/host/default/listKeys?api-version=2018-11-01"
        ```

        > [!TIP]
        > Bunun için, URL 'YI bir ortam değişkenine atayıp `az rest` komutta kullanabilirsiniz.
    
    1. Aşağıdaki `az rest` komutu çalıştırın (Azure CLI sürüm 2.0.77 ve üzeri sürümlerde bulunur), tırnak işaretleri dahil `<uri>` olmak üzere son adımdaki URI dizesiyle değiştirin:

        ```azurecli
        az rest --method post --uri <uri> --query functionKeys.default --output tsv
        ```

    1. Komutun çıktısı, işlev anahtarıdır. Tam işlev URL `https://<app_name>.azurewebsites.net/api/<function_name>?code=<key>`'si,,, ve `<app_name>` `<function_name>` `<key>` değerlerini belirli değerlerinizle değiştirir.
    
        > [!NOTE]
        > Burada alınan anahtar, işlevler uygulamasındaki tüm işlevler için çalışır olan *ana bilgisayar* anahtarıdır; Portal için gösterilen yöntem yalnızca bir işlev için anahtarı alır.

    ---

1. Bu URL 'nin sonuna parametreyi `&name=Azure` ekleyerek, işlev URL 'sini tarayıcınızın adres çubuğuna yapıştırın. "Hello Azure" gibi metinler tarayıcıda görünmelidir.

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-function-linux-custom-image/function-app-browser-testing.png)

1. Yetkilendirmeyi test etmek için, URL 'den Code = parametresini kaldırın ve işlevden yanıt bulunmadığını doğrulayın.


## <a name="enable-continuous-deployment-to-azure"></a>Azure 'a sürekli dağıtımı etkinleştir

Kayıt defterinde görüntüyü güncelleştirdiğinizde bir görüntü dağıtımınızı otomatik olarak güncelleştirmek için Azure Işlevleri 'ni etkinleştirebilirsiniz.

1. İşlev uygulamanızın adıyla değiştirerek `<app_name>` [az functionapp Deployment Container config](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-config) komutunu kullanarak sürekli dağıtımı etkinleştirin:

    ```azurecli
    az functionapp deployment container config --enable-cd --query CI_CD_URL --output tsv --name <app_name> --resource-group AzureFunctionsContainers-rg
    ```
    
    Bu komut sürekli dağıtımı etkinleştirilir ve dağıtım Web kancası URL 'sini döndürür. ( [Az functionapp Deployment Container show-CD-URL](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-show-cd-url) komutunu kullanarak bu URL 'yi daha sonra da alabilirsiniz.)

1. Dağıtım Web kancası URL 'sini panoya kopyalayın.

1. [Docker Hub 'ını](https://hub.docker.com/)açın, oturum açın ve gezinti çubuğunda **depolar** ' ı seçin. Görüntü bulun ve seçin, **Web kancaları** sekmesini seçin, **Web kancası adı**belirtin, URL 'nizi **Web kancası URL 'sine**yapıştırın ve sonra **Oluştur**' u seçin:

    ![Web kancasını DockerHub depoya ekleme](./media/functions-create-function-linux-custom-image/dockerhub-set-continuous-webhook.png)  

1. Web kancası ayarlandığında, Azure Işlevleri, Docker Hub 'ında her güncelleştirdiğinizde görüntünüzü yeniden dağıtır.

## <a name="enable-ssh-connections"></a>SSH bağlantılarını etkinleştir

SSH, kapsayıcı ile istemci arasında güvenli iletişime olanak tanır. SSH etkinken, App Service gelişmiş araçlar (kudu) kullanarak kapsayıcınıza bağlanabilirsiniz. Azure Işlevleri, SSH kullanarak kapsayıcınıza bağlanmayı kolaylaştırmak için SSH 'nin zaten etkinleştirilmiş olduğu bir temel görüntü sağlar. Yalnızca Dockerfile dosyanızı düzenlemeniz, ardından görüntüyü yeniden oluşturup yeniden dağıtmanız gerekir. Daha sonra, gelişmiş araçlar (kudu) aracılığıyla kapsayıcıya bağlanabilirsiniz

1. Dockerfile dosyanızda, `FROM` yönergedeki taban görüntüye `-appservice` dizeyi ekleyin:

    ::: zone pivot="programming-language-csharp"
    ```Dockerfile
    FROM microsoft/dotnet:2.2-sdk-appservice AS installer-env
    ```
    ::: zone-end

    ::: zone pivot="programming-language-javascript"
    ```Dockerfile
    FROM mcr.microsoft.com/azure-functions/node:2.0-appservice
    ```
    ::: zone-end

    ::: zone pivot="programming-language-powershell"
    ```Dockerfile
    FROM mcr.microsoft.com/azure-functions/powershell:2.0-appservice
    ```
    ::: zone-end

    ::: zone pivot="programming-language-python"
    ```Dockerfile
    FROM mcr.microsoft.com/azure-functions/python:2.0-python3.7-appservice
    ```
    ::: zone-end

    ::: zone pivot="programming-language-typescript"
    ```Dockerfile
    FROM mcr.microsoft.com/azure-functions/node:2.0-appservice
    ```
    ::: zone-end

    Temel görüntüler arasındaki farklılıklar, [App Services-özel Docker görüntüleri öğreticisinde](../app-service/containers/tutorial-custom-docker-image.md#enable-ssh-connections)açıklanmıştır.

1. `docker build` Komutu tekrar kullanarak, DOCKER Kimliğiniz ile değiştirerek `<docker_id>` görüntüyü yeniden oluşturun:

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.0 .
    ```
    
1. Güncelleştirilmiş görüntüyü Docker Hub 'a gönderin. Bu, ilk gönderiden yalnızca görüntünün güncelleştirilmiş segmentlerinin karşıya yüklenmesi gereken zamandan çok daha az zaman alır.

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
1. Azure Işlevleri otomatik olarak görüntüyü işlevler uygulamanıza yeniden dağıtır; işlem bir dakikadan kısa bir sürede gerçekleşir.

1. Bir tarayıcıda, öğesini açın `https://<app_name>.scm.azurewebsites.net/`, benzersiz `<app_name>` adınızla değiştirin. Bu URL, işlev uygulaması Kapsayıcınız için gelişmiş araçlar (kudu) uç noktasıdır.

1. Azure hesabınızda oturum açın ve kapsayıcıya bir bağlantı kurmak için **SSH** 'yi seçin. Azure 'un kapsayıcı görüntüsünü güncelleştirmesi devam ediyorsa, bağlanma işlemi birkaç dakika sürebilir.

1. Kapsayıcınıza bir bağlantı kurulduktan sonra, çalışmakta olan işlemi görüntülemek `top` için komutunu çalıştırın. 

    ![SSH oturumunda çalışan Linux üst komutu](media/functions-create-function-linux-custom-image/linux-custom-kudu-ssh-top.png)

## <a name="write-to-an-azure-storage-queue"></a>Azure depolama kuyruğuna yazma

Azure Işlevleri, işlevlerinizi kendi tümleştirme kodunuzu yazmak zorunda olan diğer Azure hizmetlerine ve kaynaklarına bağlamanıza olanak tanır. Hem giriş hem de çıktıyı temsil eden bu *bağlamalar*, işlev tanımı içinde bildirilmiştir. Bağlamalardan alınan veriler işleve parametre olarak sağlanır. *Tetikleyici* özel bir giriş bağlama türüdür. Bir işlevde yalnızca bir tetikleyici olsa da, birden çok giriş ve çıkış bağlaması olabilir. Daha fazla bilgi için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

Bu bölümde, işlevinizi bir Azure depolama kuyruğu ile tümleştirme işlemi gösterilmektedir. Bu işleve eklediğiniz çıkış bağlaması, verileri bir HTTP isteğinden kuyruktaki bir iletiye yazar.

[!INCLUDE [functions-cli-get-storage-connection](../../includes/functions-cli-get-storage-connection.md)]

[!INCLUDE [functions-register-storage-binding-extension-csharp](../../includes/functions-register-storage-binding-extension-csharp.md)]

[!INCLUDE [functions-add-output-binding-cli](../../includes/functions-add-output-binding-cli.md)]

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]  
::: zone-end  
::: zone pivot="programming-language-java" 
[!INCLUDE [functions-add-output-binding-java-cli](../../includes/functions-add-output-binding-java-cli.md)]
::: zone-end  

## <a name="add-code-to-use-the-output-binding"></a>Çıkış bağlamayı kullanmak için kod ekleme

Sıra bağlaması tanımlı ile, artık işlevinizi, `msg` çıkış parametresini alacak ve kuyruğa ileti yazacak şekilde güncelleştirebilirsiniz.

::: zone pivot="programming-language-python"     
[!INCLUDE [functions-add-output-binding-python](../../includes/functions-add-output-binding-python.md)]
::: zone-end  

::: zone pivot="programming-language-javascript"  
[!INCLUDE [functions-add-output-binding-js](../../includes/functions-add-output-binding-js.md)]
::: zone-end  

::: zone pivot="programming-language-typescript"  
[!INCLUDE [functions-add-output-binding-ts](../../includes/functions-add-output-binding-ts.md)]
::: zone-end  

::: zone pivot="programming-language-powershell"  
[!INCLUDE [functions-add-output-binding-powershell](../../includes/functions-add-output-binding-powershell.md)]  
::: zone-end

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]
::: zone-end 

::: zone pivot="programming-language-java"
[!INCLUDE [functions-add-output-binding-java-code](../../includes/functions-add-output-binding-java-code.md)]

[!INCLUDE [functions-add-output-binding-java-test-cli](../../includes/functions-add-output-binding-java-test-cli.md)]
::: zone-end

### <a name="update-the-image-in-the-registry"></a>Kayıt defterindeki görüntüyü güncelleştirme

1. Kök klasörde, yeniden çalıştırın `docker build` ve bu kez etiketteki sürümü olarak `v1.0.1`güncelleştirin. Daha önce olduğu gibi `<docker_id>` , Docker Hub hesabı Kimliğinizle değiştirin:

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.1
    ```
    
1. Güncelleştirilmiş görüntüyü şu ile `docker push`depoya geri gönderin:

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.1
    ```

1. Sürekli teslimi yapılandırdığınız için, kayıt defterindeki görüntüyü güncelleştirme işlemi otomatik olarak Azure 'daki işlev uygulamanızı güncelleştirir.

## <a name="view-the-message-in-the-azure-storage-queue"></a>Azure depolama sırasındaki iletiyi görüntüleme

Bir tarayıcıda, işlevinizi çağırmak için aynı URL 'YI kullanın. İşlev kodunun bu bölümünü değiştirmediğiniz için tarayıcı, önceki ile aynı yanıtı görüntülemelidir. Ancak eklenen kod, `name` `outqueue` depolama kuyruğuna URL parametresi kullanılarak bir ileti yazdı.

[!INCLUDE [functions-add-output-binding-view-queue-cli](../../includes/functions-add-output-binding-view-queue-cli.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz kaynakları kullanarak Azure Işleviyle çalışmaya devam etmek istiyorsanız, tüm bu kaynakları yerinde bırakabilirsiniz. Azure Işlevleri için Premium bir plan oluşturduğunuza göre, sürekli maliyetlerde günlük bir veya iki USD olacaktır.

Devam eden maliyetlerin önüne geçmek için, `AzureFunctionsContainer-rg` bu gruptaki tüm kaynakları temizlemek üzere kaynak grubunu silin: 

```azurecli
az group delete --name AzureFunctionsContainer-rg
```

## <a name="next-steps"></a>Sonraki adımlar

+ [İzleme işlevleri](functions-monitoring.md)
+ [Ölçek ve barındırma seçenekleri](functions-scale.md)
+ [Kubernetes tabanlı sunucusuz barındırma](functions-kubernetes-keda.md)
