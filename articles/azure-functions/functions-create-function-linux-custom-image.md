---
title: Özel bir görüntü kullanarak Linux'ta Azure Fonksiyonları oluşturma
description: Özel bir Linux görüntüsü üzerinde çalışan Azure İşlevleri oluşturmayı öğrenin.
ms.date: 03/30/2020
ms.topic: tutorial
ms.custom: mvc
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 44ca8f721967b90be283f867f8656344ec3f1906
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673406"
---
# <a name="create-a-function-on-linux-using-a-custom-container"></a>Özel bir kapsayıcı kullanarak Linux üzerinde işlev oluşturma

Bu öğreticide, Linux temel görüntüsünü kullanarak Python kodunu Azure Fonksiyonları'na özel bir Docker kapsayıcısı olarak oluşturur ve dağıtabilirsiniz. İşlevleriniz belirli bir dil sürümü gerektirdiğinde veya yerleşik görüntü tarafından sağlanmayan belirli bir bağımlılığa veya yapılandırmaya sahipse genellikle özel bir görüntü kullanırsınız.

[Linux'ta barındırılan ilk işlevinizi oluştur'da](functions-create-first-azure-function-azure-cli-linux.md)açıklandığı gibi varsayılan bir Azure Uygulama Hizmeti kapsayıcısı da kullanabilirsiniz. Azure İşlevleri için desteklenen temel [görüntüler, Azure İşlevleri temel resimleri repo'da](https://hub.docker.com/_/microsoft-azure-functions-base)bulunur.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure İşlevler Temel Araçlarını kullanarak bir işlev uygulaması ve Dockerfile oluşturun.
> * Docker kullanarak özel bir görüntü oluşturun.
> * Özel görüntüyü bir kapsayıcı kayıt defterinde yayımlayın.
> * İşlev uygulaması için Azure'da destek kaynakları oluşturma
> * Docker Hub’dan bir işlev uygulaması dağıtın.
> * Uygulama ayarlarını işlevi uygulamasına ekleyin.
> * Sürekli dağıtımı etkinleştirin.
> * Kapsayıcıya SSH bağlantılarını etkinleştirin.
> * Sıra depolama çıktısı bağlama ekleyin. 

Bu öğreticiyi Windows, macOS veya Linux çalıştıran herhangi bir bilgisayarda takip edebilirsiniz. Öğreticinin tamamlanması, Azure hesabınızda birkaç ABD doları maliyetine neden olur.

[!INCLUDE [functions-requirements-cli](../../includes/functions-requirements-cli.md)]

<!---Requirements specific to Docker --->
+ [Docker](https://docs.docker.com/install/)  

+ [Docker Kimliği](https://hub.docker.com/signup)

[!INCLUDE [functions-cli-verify-prereqs](../../includes/functions-cli-verify-prereqs.md)]

+ `docker login` Docker'a imza at. Docker çalışmıyorsa bu komut başarısız olur, bu durumda docker'ı başlatın ve komutu yeniden deneyin.

[!INCLUDE [functions-cli-create-venv](../../includes/functions-cli-create-venv.md)]

## <a name="create-and-test-the-local-functions-project"></a>Yerel işlevler projesini oluşturma ve test edin

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
Bir terminal veya komut isteminde, seçtiğiniz dil için aşağıdaki komutu çalıştırın. `LocalFunctionsProject`  
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

# <a name="bash"></a>[bash](#tab/bash)
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

Maven, dağıtımı sırasında projeoluşturmayı bitirmek için gereken değerleri sizden sorar.   
İstendiğinde aşağıdaki değerleri sağlayın:

| İstem | Değer | Açıklama |
| ------ | ----- | ----------- |
| **groupId** | `com.fabrikam` | Java için [paket adlandırma kurallarını](https://docs.oracle.com/javase/specs/jls/se6/html/packages.html#7.7) izleyerek, projenizi tüm projelerde benzersiz olarak tanımlayan bir değer. |
| **Artifactıd** | `fabrikam-functions` | Bir sürüm numarası olmayan kavanozun adı olan bir değer. |
| **Sürüm** | `1.0-SNAPSHOT` | Varsayılan değeri seçin. |
| **Paket** | `com.fabrikam.functions` | Oluşturulan işlev kodu için Java paketi olan bir değer. Varsayılan değeri kullanın. |

Onaylamak `Y` için Enter yazın veya enter tuşuna basın.

Maven, bu örnekte yer alan _artifactId_adında yeni bir klasörde proje dosyalarını `fabrikam-functions`oluşturur. 
::: zone-end
Bu `--docker` seçenek, `Dockerfile` Azure İşlevleri ve seçili çalışma zamanı ile kullanılmak üzere uygun bir özel kapsayıcı tanımlayan proje için bir seçenek oluşturur.

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
`--name` Bağımsız değişkenin işlevinizin benzersiz adı olduğu ve `--template` bağımsız değişkenin işlevin tetikleyicisini belirttiği aşağıdaki komutu kullanarak projenize bir işlev ekleyin. `func new`projenin seçilen diline uygun bir kod dosyası ve *function.json*adlı yapılandırma dosyası içeren işlev adı ile eşleşen bir alt klasör oluşturun.

```
func new --name HttpExample --template "HTTP trigger"
```
::: zone-end  
İşlevi yerel olarak sınamak için, proje klasörünün kökünde yerel Azure İşlevler çalışma zamanı ana bilgisayarını başlatın: 
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
Çıktıda bitiş `HttpExample` noktasının göründüğünü gördüğünüzde, [`http://localhost:7071/api/HttpExample?name=Functions`](http://localhost:7071/api/HttpExample?name=Functions)'ye gidin. Tarayıcı, sorgu parametresi için sağlanan `Functions`değeri geri yankılanan `name` bir "merhaba" iletisi göstermelidir.

Ana bilgisayarı durdurmak için **Ctrl**-**C'yi** kullanın.

## <a name="build-the-container-image-and-test-locally"></a>Kapsayıcı görüntüsünü oluşturun ve yerel olarak test edin

(İsteğe bağlı) Proje klasörünün kökündeki *Dockerfile'ı inceleyin. Dockerfile, işlev uygulamasını Linux üzerinde çalıştırmak için gerekli ortamı açıklar.  Azure İşlevleri için desteklenen temel görüntülerin tam listesini [Azure İşlevleri temel resim sayfasında](https://hub.docker.com/_/microsoft-azure-functions-base)bulabilirsiniz.
    
Kök proje klasöründe, [docker build](https://docs.docker.com/engine/reference/commandline/build/) komutunu çalıştırın `azurefunctionsimage`ve bir `v1.0.0`ad ve etiket sağlayın. `<DOCKER_ID>` değerini Docker Hub hesabınızın kimliğiyle değiştirin. Bu komut, kapsayıcı için Docker görüntüsünü derler.

```
docker build --tag <DOCKER_ID>/azurefunctionsimage:v1.0.0 .
```

Komut tamamlandığında, yeni kapsayıcıyı yerel olarak çalıştırabilirsiniz.
    
Yapıyı test etmek için, docker [run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanarak görüntüyü `<DOCKER_ID` yerel bir kapsayıcıda çalıştırın, `-p 8080:80`Docker Kimliğinizi yeniden değiştirin ve bağlantı noktaları bağımsız değişkenini ekleyerek:

```
docker run -p 8080:80 -it <docker_id>/azurefunctionsimage:v1.0.0
```

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"  
Görüntü yerel bir kapsayıcıda çalışırken, aşağıda `http://localhost:8080`gösterilen yer tutucu görüntüsünü görüntülemek için bir tarayıcı açın. İşleviniz Azure'da olduğu gibi yerel kapsayıcıda çalıştığından, görüntü bu noktada görünür, bu da `"authLevel": "function"` özellik ile birlikte *function.json'da* tanımlandığı gibi bir erişim anahtarı tarafından korunduğu anlamına gelir. Ancak kapsayıcı henüz Azure'daki bir işlev uygulamasında yayınlanmamıştır, bu nedenle anahtar henüz kullanılamamıştır. Yerel kapsayıcıya karşı test etmek istiyorsanız, docker'ı `"authLevel": "anonymous"`durdurun, yetkilendirme özelliğini değiştirin, görüntüyü yeniden oluşturun ve docker'ı yeniden başlatın. Sonra `"authLevel": "function"` *function.json*sıfırlamak . Daha fazla bilgi için [yetkilendirme anahtarlarına](functions-bindings-http-webhook-trigger.md#authorization-keys)bakın.

![Konteynerin yerel olarak çalıştığını belirten yer tutucu görüntüsü](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

::: zone-end
::: zone pivot="programming-language-java"  
Görüntü yerel bir kapsayıcıda çalışırken, eskisi [`http://localhost:8080/api/HttpExample?name=Functions`](http://localhost:8080/api/HttpExample?name=Functions)gibi aynı "merhaba" iletisini görüntülemesi gereken "merhaba" iletisine göz atın. Maven arketipi anonim yetkilendirme kullanan bir HTTP tetikleme işlevi oluşturduğundan, kapsayıcıda çalışıyor olsa bile işlevi çağırabilirsiniz. 
::: zone-end  

Kapsayıcıdaki işlev uygulamasını doğruladıktan sonra **Ctrl**+**C**ile docker'ı durdurun.

## <a name="push-the-image-to-docker-hub"></a>Görüntüyü Docker Hub'a itin

Docker Hub, görüntüleri barındıran, görüntü ve kapsayıcı hizmetleri sağlayan bir kapsayıcı kayıt defteridir. Azure'a dağıtmayı içeren resminizi paylaşmak için, resmi nizi bir kayıt defterine itmeniz gerekir.

1. Docker ile daha önce oturum açmadıysanız, docker [giriş](https://docs.docker.com/engine/reference/commandline/login/) komutuyla `<docker_id>` docker kimliğinizi değiştirin. Bu komut, kullanıcı adınız ve şifreniz için sizi ister. "Oturum Açma Başarılı Oldu" iletisi oturum açolduğunuzu onaylar.

    ```
    docker login
    ```
    
1. Oturum açtıktan sonra, [docker push](https://docs.docker.com/engine/reference/commandline/push/) komutunu kullanarak resmi Docker Kimliğinizi `<docker_id>` yeniden değiştirerek Docker Hub'a itin.

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.0
    ```

1. Ağ hızınıza bağlı olarak, görüntüyü ilk kez itme birkaç dakika sürebilir (sonraki değişiklikleri zorlamak çok daha hızlıdır). Beklerken, bir sonraki bölüme geçebilir ve başka bir terminalde Azure kaynakları oluşturabilirsiniz.

## <a name="create-supporting-azure-resources-for-your-function"></a>İşleviniz için destekleyici Azure kaynakları oluşturun

İşlev kodunuzu Azure'a dağıtmak için üç kaynak oluşturmanız gerekir:

- İlgili kaynaklar için mantıksal bir kapsayıcı olan kaynak grubu.
- Projelerinizle ilgili durumu ve diğer bilgileri koruyan bir Azure Depolama hesabı.
- İşlev kodunuzu yürütmek için ortam sağlayan bir Azure işlevleri uygulaması. İşlev uygulaması yerel işlev projenizi eşler ve daha kolay yönetim, dağıtım ve kaynakların paylaşımı için işlevleri mantıksal bir birim olarak gruplandırmanızı sağlar.

Bu öğeleri oluşturmak için Azure CLI komutlarını kullanırsınız. Her komut tamamlandıktan sonra JSON çıkışı sağlar.

1. Az giriş komutuyla Azure'da oturum [açın:](/cli/azure/reference-index#az-login)

    ```azurecli
    az login
    ```
    
1. [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, `AzureFunctionsContainers-rg` `westeurope` bölgede adlı bir kaynak grubu oluşturur. (Genellikle `az account list-locations` kaynak grubunuzu ve kaynaklarınızı komuttan kullanılabilir bir bölgeyi kullanarak yakınınızdaki bir bölgede oluşturursunuz.)

    ```azurecli
    az group create --name AzureFunctionsContainers-rg --location westeurope
    ```
    
    > [!NOTE]
    > Linux ve Windows uygulamalarını aynı kaynak grubunda barındıramaz. Windows işlev uygulaması uygulaması `AzureFunctionsContainers-rg` veya web uygulamasıyla birlikte adında varolan bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.
    
1. Az depolama hesabı oluşturma komutunu kullanarak kaynak grubunuzda ve bölgenizde genel amaçlı bir [depolama hesabı oluşturun.](/cli/azure/storage/account#az-storage-account-create) Aşağıdaki örnekte, `<storage_name>` size uygun genel olarak benzersiz bir ad la değiştirin. Adlar yalnızca üç ila 24 karakter lisi ve küçük harf içermelidir. `Standard_LRS`tipik bir genel amaçlı hesap belirtir.

    ```azurecli
    az storage account create --name <storage_name> --location westeurope --resource-group AzureFunctionsContainers-rg --sku Standard_LRS
    ```
    
    Depolama hesabı bu öğretici için sadece birkaç USD sent tabi.
    
1. **Komutu kullanarak,** Batı Avrupa bölgesinde (veya size yakın uygun`--sku EP1`bir bölge kullanın)`-location westeurope`ve Bir Linux kapsayıcısında () Elastik`--is-linux`Premium 1 fiyatlandırma katmanında () adı verilen `myPremiumPlan` Azure İşlevleri için bir Premium planı oluşturmak için kullanın.

    ```azurecli
    az functionapp plan create --resource-group AzureFunctionsContainers-rg --name myPremiumPlan --location westeurope --number-of-workers 1 --sku EP1 --is-linux
    ```   

    Özel fonksiyonlar için Linux barındırma kapsayıcıları [Özel (App Service) planları](functions-scale.md#app-service-plan) ve [Premium planları](functions-premium-plan.md#features)desteklenir. Gerektiğinde ölçeklendirilebilen Premium planını burada kullanıyoruz. Barındırma hakkında daha fazla bilgi edinmek için, bkz. [Azure İşlevleri barındırma planları karşılaştırması](functions-scale.md). Maliyetleri hesaplamak [için, Fonksiyonlar fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/functions/)bakın.

    Komut ayrıca, işlev uygulamanızı izleyip günlüklerinizi görüntüleyebileceğiniz ilişkili bir Azure Uygulama Öngörüleri örneğini de aynı kaynak grubunda karşılar. Daha fazla bilgi için [bkz.](functions-monitoring.md) Örneği etkinleştirene kadar hiçbir ücret etüt etmez.

## <a name="create-and-configure-a-function-app-on-azure-with-the-image"></a>Azure'da görüntüyle bir işlev uygulaması oluşturma ve yapılandırma

Azure'daki bir işlev uygulaması, barındırma planınızda işlevlerinizin yürütülmesini yönetir. Bu bölümde, Docker Hub'daki bir resimden bir işlev uygulaması oluşturmak ve Azure Depolama'ya bağlantı dizesi ile yapılandırmak için önceki bölümdeki Azure kaynaklarını kullanırsınız.

1. [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak Fonksiyonlar uygulamasını oluşturun. Aşağıdaki örnekte, `<storage_name>` depolama hesabı için önceki bölümde kullandığınız adla değiştirin. Ayrıca, `<app_name>` size uygun olan genel olarak `<docker_id>` benzersiz bir ad ve Docker Kimliğinizle değiştirin.

    ```azurecli
    az functionapp create --name <app_name> --storage-account <storage_name> --resource-group AzureFunctionsContainers-rg --plan myPremiumPlan --deployment-container-image-name <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
    *Dağıtım kapsayıcısı-görüntü adı* parametresi, işlev uygulaması için kullanılacak görüntüyü belirtir. Dağıtım için kullanılan görüntü hakkındaki bilgileri görüntülemek için [az functionapp config container show](/cli/azure/functionapp/config/container#az-functionapp-config-container-show) komutunu kullanabilirsiniz. Ayrıca farklı bir görüntü dağıtmak için [az functionapp config konteyner kümesi](/cli/azure/functionapp/config/container#az-functionapp-config-container-set) komutunu kullanabilirsiniz.

1. [Az depolama hesabı show-connection-string](/cli/azure/storage/account) komutunu kullanarak oluşturduğunuz depolama hesabının bağlantı dizesini `storageConnectionString`bir kabuk değişkenine atayarak alın:

    ```azurecli
    az storage account show-connection-string --resource-group AzureFunctionsContainers-rg --name <storage_name> --query connectionString --output tsv
    ```
    
1. [Az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) komutunu kullanarak bu ayarı işlev uygulamasına ekleyin. Aşağıdaki komutta, `<app_name>` işlev uygulamanızın adı ile değiştirin ve önceki adımdaki bağlantı dizesiyle değiştirin `<connection_string>` ("DefaultEndpointProtocol=" ile başlayan uzun kodlanmış bir dize):
 
    ```azurecli
    az functionapp config appsettings set --name <app_name> --resource-group AzureFunctionsContainers-rg --settings AzureWebJobsStorage=<connection_string>
    ```

1. İşlev artık depolama hesabına erişmek için bu bağlantı dizesini kullanabilir.

    > [!TIP]
    > Bash'te, panoyerine bağlantı dizesini yakalamak için bir kabuk değişkeni kullanabilirsiniz. İlk olarak, bağlantı dizeli bir değişken oluşturmak için aşağıdaki komutu kullanın:
    > 
    > ```bash
    > storageConnectionString=$(az storage account show-connection-string --resource-group AzureFunctionsContainers-rg --name <storage_name> --query connectionString --output tsv)
    > ```
    > 
    > Daha sonra ikinci komuttaki değişkene bakın:
    > 
    > ```azurecli
    > az functionapp config appsettings set --name <app_name> --resource-group AzureFunctionsContainers-rg --settings AzureWebJobsStorage=$storageConnectionString
    > ```

> [!NOTE]    
> Özel resminizi özel bir kapsayıcı hesabına yayımlarsanız, bağlantı dizesi için Dockerfile'daki ortam değişkenlerini kullanmanız gerekir. Daha fazla bilgi için [ENV talimatına](https://docs.docker.com/engine/reference/builder/#env)bakın. Ayrıca değişkenleri `DOCKER_REGISTRY_SERVER_USERNAME` ve `DOCKER_REGISTRY_SERVER_PASSWORD`. Değerleri kullanmak için görüntüyü yeniden oluşturmanız, resmi kayıt defterine itmeniz ve ardından Azure'daki işlev uygulamasını yeniden başlatmanız gerekir.

## <a name="verify-your-functions-on-azure"></a>Azure'da işlevlerinizi doğrulayın

Görüntü Azure'daki işlev uygulamasına dağıtıldığınızda, artık işlevi HTTP istekleri aracılığıyla çağırabilirsiniz. *function.json* tanımı özelliği `"authLevel": "function"`içerdiğinden, önce erişim anahtarını ("işlev anahtarı" olarak da adlandırılır) edinmeniz ve bitiş noktasına gelen tüm isteklere URL parametresi olarak eklemeniz gerekir.

1. Azure portalını kullanarak veya komutla Azure CLI'yi kullanarak erişim (işlev) `az rest` anahtarıyla işlev URL'sini alın.)

    # <a name="portal"></a>[Portal](#tab/portal)

    1. Azure portalında oturum açın ve sayfanın üst kısmındaki **Arama** kutusuna işlev uygulama adınızı girerek işlev uygulamanızı bulun. Sonuçlarda, Uygulama **Hizmeti** kaynağını seçin.

    1. Sol daki gezinti panelinde, **Fonksiyonlar (Salt okunur)** altında, işlevinizin adını seçin.

    1. Ayrıntılar panelinde, **</> İşlev URL'sini**seçin:
    
        ![Azure portalında işlev URL komutunu al komutu](./media/functions-create-function-linux-custom-image/functions-portal-get-url-key.png)   

    1. Açılır pencerede **varsayılan (İşlev tuşu) seçeneğini** belirleyin ve ardından **Kopyala'yı seçin.** Anahtar aşağıdaki `?code=`karakter dizesidir.

        ![Azure portalından işlev URL'sini kopyalama](./media/functions-create-function-linux-custom-image/functions-portal-get-url-key-popup.png)   

    > [!NOTE]  
    > İşlev uygulamanız kapsayıcı olarak dağıtıldığı için, portaldaki işlev kodunuzda değişiklik yapamazsınız. Bunun yerine projeyi yerel görüntüde güncelleştirmeniz, resmi yeniden kayıt defterine itmeniz ve ardından Azure'a yeniden dağıtmanız gerekir. Daha sonraki bir bölümde sürekli dağıtım ayarlayabilirsiniz.
    
    # <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

    1. Azure abonelik kimliğiniz, işlev uygulamanızın `<resource_group>`kaynak `<app_name>` grubu ve işlev uygulamanızın adını sırasıyla değiştirerek `<subscription_id>`aşağıdaki biçimde bir URL dizesi oluşturun:

        ```
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Web/sites/<app_name>/host/default/listKeys?api-version=2018-11-01"
        ```

        Örneğin, URL aşağıdaki adrese görünebilir:

        ```
        "/subscriptions/1234aaf4-1234-abcd-a79a-245ed34eabcd/resourceGroups/AzureFunctionsContainers-rg/providers/Microsoft.Web/sites/msdocsfunctionscontainer/host/default/listKeys?api-version=2018-11-01"
        ```

        > [!TIP]
        > Kolaylık sağlamak için URL'yi bir ortam değişkenine atayabilir ve komutta `az rest` kullanabilirsiniz.
    
    1. Aşağıdaki `az rest` komutu çalıştırın (Azure CLI sürümü 2.0.77 `<uri>` ve sonraki sürümde kullanılabilir), tırnak işaretleri de dahil olmak üzere son adımdaki URI dizesini değiştirerek:

        ```azurecli
        az rest --method post --uri <uri> --query functionKeys.default --output tsv
        ```

    1. Komutun çıktısı işlev anahtarıdır. Tam işlevLI URL `https://<app_name>.azurewebsites.net/api/<function_name>?code=<key>`sonra `<app_name>`, `<function_name>`değiştirme `<key>` , ve belirli değerleri ile.
    
        > [!NOTE]
        > Burada alınan anahtar, işlevler uygulamasındaki tüm işlevler için çalışan *ana bilgisayar* anahtarıdır; portal için gösterilen yöntem yalnızca tek işlev için anahtarı alır.

    ---

1. Bu URL'nin sonuna parametre `&name=Azure` ekleyerek işlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. Tarayıcıda "Hello Azure" gibi metinler görünmelidir.

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-function-linux-custom-image/function-app-browser-testing.png)

1. Yetkilendirmeyi test etmek için, kod= parametresini URL'den kaldırın ve işlevden yanıt almadığınızı doğrulayın.


## <a name="enable-continuous-deployment-to-azure"></a>Azure'da sürekli dağıtımı etkinleştirme

Kayıt defterindeki resmi her güncellediğinizde Azure Fonksiyonları'nın görüntü dağıtımınızı otomatik olarak güncelleştirmesini etkinleştirebilirsiniz.

1. [Az functionapp dağıtım kapsayıcı config](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-config) komutunu kullanarak, işlev uygulamanızın adını değiştirerek `<app_name>` sürekli dağıtımı etkinleştirin:

    ```azurecli
    az functionapp deployment container config --enable-cd --query CI_CD_URL --output tsv --name <app_name> --resource-group AzureFunctionsContainers-rg
    ```
    
    Bu komut sürekli dağıtım sağlar ve dağıtım webhook URL'sini döndürür. (Bu URL'yi daha sonra az [functionapp deployment container show-cd-url](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-show-cd-url) komutunu kullanarak alabilirsiniz.)

1. Dağıtım webhook URL'sini panoya kopyalayın.

1. [Docker Hub'ı](https://hub.docker.com/)açın, oturum açın ve gezinme çubuğundaki **Depoları** seçin. Resmi bulun ve seçin, **Webhooks** sekmesini seçin, Bir **Webhook adı**belirtin, URL'nizi **Webhook URL'ye**yapıştırın ve ardından **Oluştur'u**seçin:

    ![DockerHub repo'nuza webhook'u ekleyin](./media/functions-create-function-linux-custom-image/dockerhub-set-continuous-webhook.png)  

1. Azure Fonksiyonları, web hook seti yle resminizi Docker Hub'da her güncellediğinizde yeniden dağılar.

## <a name="enable-ssh-connections"></a>SSH bağlantılarını etkinleştirme

SSH, kapsayıcı ile istemci arasında güvenli iletişime olanak tanır. SSH etkinken, Uygulama Hizmeti Gelişmiş Araçları (Kudu) kullanarak kabınıza bağlanabilirsiniz. Azure İşlevler, SSH kullanarak kapsayıcınıza bağlanmayı kolaylaştırmak için, SSH'yi zaten etkinleştirmiş bir temel görüntü sağlar. Yalnızca Dockerfile'ınızı yeniden, ardından görüntüyü yeniden oluşturmanız ve yeniden dağıtmanız gerekir. Daha sonra Gelişmiş Araçlar (Kudu) aracılığıyla konteyner bağlanabilirsiniz

1. Dockerfile'nizde, dizeyi `-appservice` talimatınızdaki `FROM` temel görüntüye ekleyin:

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

    Temel görüntüler arasındaki farklar App [Services açıklanmıştır - Özel docker görüntüleri öğretici](../app-service/containers/tutorial-custom-docker-image.md#enable-ssh-connections).

1. Docker ID'niz `docker build` ile değiştirerek `<docker_id>` komutu yeniden kullanarak görüntüyü yeniden oluşturun:

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.0 .
    ```
    
1. Güncelleştirilmiş görüntüyü Docker Hub'a itin, bu da yalnızca görüntünün güncelleştirilmiş bölümlerinin yüklenmesi gereken ilk itmeden çok daha az zaman almalıdır.

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
1. Azure İşlevler görüntüyü otomatik olarak işlevler uygulamanıza yeniden dağır; işlem bir dakikadan kısa bir süre içinde gerçekleşir.

1. Bir tarayıcıda, `https://<app_name>.scm.azurewebsites.net/`açık `<app_name>` , benzersiz adınızı ile değiştirerek. Bu URL, işlev uygulama kapsayıcınız için Gelişmiş Araçlar (Kudu) bitiş noktasıdır.

1. Azure hesabınızda oturum açın ve kapsayıcıyla bağlantı kurmak için **SSH'yi** seçin. Azure kapsayıcı görüntüsünü hala güncelliyorsa bağlanma birkaç dakika sürebilir.

1. Kapsayıcınızla bir bağlantı kurulduktan sonra, `top` şu anda çalışan işlemleri görüntülemek için komutu çalıştırın. 

    ![SSH oturumunda çalışan Linux üst komutu](media/functions-create-function-linux-custom-image/linux-custom-kudu-ssh-top.png)

## <a name="write-to-an-azure-storage-queue"></a>Azure Depolama kuyruğuna yazma

Azure İşlevleri, işlevlerinizi kendi tümleştirme kodunuzu yazmak zorunda olan diğer Azure hizmetlerine ve kaynaklarına bağlamanızı sağlar. Hem giriş hem de çıktıyı temsil eden bu *bağlamalar*işlev tanımı içinde bildirilir. Bağlamalardan alınan veriler işleve parametre olarak sağlanır. *Tetikleyici,* özel bir giriş bağlama türüdür. Bir işlevin yalnızca bir tetikleyicisi olmasına rağmen, birden çok giriş ve çıkış bağlaması olabilir. Daha fazla bilgi edinmek için Azure [İşlevlerini tetikler ve bağlama kavramlarını](functions-triggers-bindings.md)öğrenin.

Bu bölümde, işlevinizi bir Azure Depolama kuyruğuyla nasıl tümleştiracağınızı gösterir. Bu işleve eklediğiniz çıktı bağlama, bir HTTP isteğindeki verileri kuyruktaki bir iletiye yazar.

[!INCLUDE [functions-cli-get-storage-connection](../../includes/functions-cli-get-storage-connection.md)]

[!INCLUDE [functions-register-storage-binding-extension-csharp](../../includes/functions-register-storage-binding-extension-csharp.md)]

[!INCLUDE [functions-add-output-binding-cli](../../includes/functions-add-output-binding-cli.md)]

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]  
::: zone-end  
::: zone pivot="programming-language-java" 
[!INCLUDE [functions-add-output-binding-java-cli](../../includes/functions-add-output-binding-java-cli.md)]
::: zone-end  

## <a name="add-code-to-use-the-output-binding"></a>Çıktı bağlamayı kullanmak için kod ekleme

Tanımlanan sıra bağlama ile, artık `msg` çıktı parametresini almak ve sıraya iletiyazmak için işlevinizi güncelleştirebilirsiniz.

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

### <a name="update-the-image-in-the-registry"></a>Kayıt defterindeki resmi güncelleştirme

1. Kök klasöründe, `docker build` yeniden çalıştırın ve bu kez etiketteki sürümü `v1.0.1`güncelleştirin. Daha önce `<docker_id>` olduğu gibi, Docker Hub hesap kimliğinizi değiştirin:

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.1
    ```
    
1. Güncellenen görüntüyü depoya: `docker push`

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.1
    ```

1. Sürekli teslimatı yapılandırdığınız için, kayıt defterindeki resmi yeniden güncelleyerek Azure'daki işlev uygulamanızı otomatik olarak güncellersiniz.

## <a name="view-the-message-in-the-azure-storage-queue"></a>Azure Depolama kuyruğundaki iletiyi görüntüleme

Bir tarayıcıda, işlevinizi çağırmak için önceki URL'yi kullanın. İşlev kodunun bu bölümünü değiştirmediğiniz için tarayıcı öncekiyle aynı yanıtı görüntülemelidir. Ancak eklenen kod, `name` `outqueue` depolama kuyruğuna URL parametresini kullanarak bir ileti yazdı.

[!INCLUDE [functions-add-output-binding-view-queue-cli](../../includes/functions-add-output-binding-view-queue-cli.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz kaynakları kullanarak Azure İşlevi ile çalışmaya devam etmek istiyorsanız, tüm bu kaynakları yerinde bırakabilirsiniz. Azure İşlevler için bir Premium Plan oluşturduğunuz için, devam eden maliyetlerde günde bir veya iki USD ödeme niz olur.

Devam eden maliyetleri önlemek `AzureFunctionsContainer-rg` için, bu gruptaki tüm kaynakları temizlemek için kaynak grubunu silin: 

```azurecli
az group delete --name AzureFunctionsContainer-rg
```

## <a name="next-steps"></a>Sonraki adımlar

+ [İzleme fonksiyonları](functions-monitoring.md)
+ [Ölçeklendirme ve barındırma seçenekleri](functions-scale.md)
+ [Kubernetes tabanlı sunucusuz barındırma](functions-kubernetes-keda.md)
