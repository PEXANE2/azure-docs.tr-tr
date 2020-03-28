---
title: Özel bir görüntü kullanarak Linux'ta Azure Fonksiyonları oluşturma
description: Özel bir Linux görüntüsü üzerinde çalışan Azure İşlevleri oluşturmayı öğrenin.
ms.date: 01/15/2020
ms.topic: tutorial
ms.custom: mvc
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 8c074c677c645dd03e3cf5288d82aa3e65720e8b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239633"
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

Bu öğreticiyi Windows, Mac OS veya Linux çalıştıran herhangi bir bilgisayarda takip edebilirsiniz. Öğreticinin tamamlanması, Azure hesabınızda birkaç ABD doları maliyetine neden olur.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Azure İşletme Temel Araçları](./functions-run-local.md#v2) sürümü 2.7.1846 veya daha sonra
- [Azure CLI](/cli/azure/install-azure-cli) sürümü 2.0.77 veya sonrası
- [Azure İşlevler 2.x çalışma süresi](functions-versions.md)
- Aşağıdaki dil çalışma zamanı bileşenleri:
    ::: zone pivot="programming-language-csharp"
    - [.NET Core 2.2.x veya sonrası](https://dotnet.microsoft.com/download)
    ::: zone-end
    ::: zone pivot="programming-language-javascript"
    - [Node.js](https://nodejs.org/en/download/)
    ::: zone-end
    ::: zone pivot="programming-language-powershell"
    - [Powershell](/powershell/scripting/install/installing-windows-powershell?view=powershell-7)
    ::: zone-end
    ::: zone pivot="programming-language-python"
    - [Python 3.6 - 64 bit](https://www.python.org/downloads/release/python-3610/) veya [Python 3.7 - 64 bit](https://www.python.org/downloads/release/python-376/)
    ::: zone-end
    ::: zone pivot="programming-language-typescript"
    - [Node.js](https://nodejs.org/en/download/)
    - [TypeScript](http://www.typescriptlang.org/#download-links)
    ::: zone-end
- [Docker](https://docs.docker.com/install/)
- [Docker Kimliği](https://hub.docker.com/signup)

### <a name="prerequisite-check"></a>Ön koşul kontrolü

1. Bir terminal veya komut `func --version` penceresinde, Azure İşlevler Temel Araçları'nın sürüm 2.7.1846 veya sonraki sürüm olup olmadığını denetlemek için çalıştırın.
1. Azure `az --version` CLI sürümünün 2.0.76 veya sonraki sürüm olup olmadığını kontrol etmek için çalıştırın.
1. Azure'da oturum açmak ve etkin bir aboneliği doğrulamak için çalıştırın. `az login`
1. `docker login` Docker'a imza at. Docker çalışmıyorsa bu komut başarısız olur, bu durumda docker'ı başlatın ve komutu yeniden deneyin.

## <a name="create-and-test-the-local-functions-project"></a>Yerel işlevler projesini oluşturma ve test edin

1. Bir terminal veya komut isteminde, uygun bir konumda bu öğretici için bir klasör oluşturun ve ardından bu klasöre gidin.

1. Bu öğreticiyle kullanılmak üzere sanal bir ortam oluşturmak için [Sanal ortam Oluştur'daki](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-python#create-venv) yönergeleri izleyin ve etkinleştirin.

1. Bir işlev uygulaması projesi oluşturmak için seçtiğiniz dil için `LocalFunctionsProject`aşağıdaki komutu çalıştırın. Bu `--docker` seçenek, `Dockerfile` Azure İşlevleri ve seçili çalışma zamanı ile kullanılmak üzere uygun bir özel kapsayıcı tanımlayan proje için bir seçenek oluşturur.

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
    
1. Proje klasörüne gidin:

    ```
    cd LocalFunctionsProject
    ```
    
1. `--name` Bağımsız değişkenin işlevinizin benzersiz adı olduğu ve `--template` bağımsız değişkenin işlevin tetikleyicisini belirttiği aşağıdaki komutu kullanarak projenize bir işlev ekleyin. `func new`projenin seçilen diline uygun bir kod dosyası ve *function.json*adlı yapılandırma dosyası içeren işlev adı ile eşleşen bir alt klasör oluşturun.

    ```
    func new --name HttpExample --template "HTTP trigger"
    ```

1. İşlevi yerel olarak sınamak *için, LocalFunctionsProject* klasöründe yerel Azure İşlevler çalışma zamanı ana bilgisayarını başlatın:
   
    ::: zone pivot="programming-language-csharp"
    ```
    func start --build
    ```
    ::: zone-end

    ::: zone pivot="programming-language-javascript"
    ```
    func start
    ```
    ::: zone-end

    ::: zone pivot="programming-language-powershell"
    ```
    func start
    ```
    ::: zone-end

    ::: zone pivot="programming-language-python"
    ```
    func start
    ```
    ::: zone-end    

    ::: zone pivot="programming-language-typescript"
    ```
    npm install
    ```

    ```
    npm start
    ```
    ::: zone-end

1. Çıktıda bitiş `HttpExample` noktasının göründüğünü gördüğünüzde, `http://localhost:7071/api/HttpExample?name=Functions`'ye gidin. Tarayıcı "Merhaba, Fonksiyonlar" gibi bir ileti görüntülemelidir (seçtiğiniz programlama diline bağlı olarak biraz değişir).

1. Ana bilgisayarı durdurmak için **Ctrl**-**C'yi** kullanın.

## <a name="build-the-container-image-and-test-locally"></a>Kapsayıcı görüntüsünü oluşturun ve yerel olarak test edin

1. (İsteğe bağlı) *LocalFunctionsProj* klasöründeki *Dockerfile'ı inceleyin. Dockerfile, işlev uygulamasını Linux üzerinde çalıştırmak için gerekli ortamı açıklar: 

    ::: zone pivot="programming-language-csharp"
    ```Dockerfile
    FROM microsoft/dotnet:2.2-sdk AS installer-env

    COPY . /src/dotnet-function-app
    RUN cd /src/dotnet-function-app && \
        mkdir -p /home/site/wwwroot && \
        dotnet publish *.csproj --output /home/site/wwwroot
    
    # To enable ssh & remote debugging on app service change the base image to the one below
    # FROM mcr.microsoft.com/azure-functions/dotnet:2.0-appservice 
    FROM mcr.microsoft.com/azure-functions/dotnet:2.0
    ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
        AzureFunctionsJobHost__Logging__Console__IsEnabled=true
    
    COPY --from=installer-env ["/home/site/wwwroot", "/home/site/wwwroot"]
    ```
    ::: zone-end

    ::: zone pivot="programming-language-javascript"
    ```Dockerfile
    # To enable ssh & remote debugging on app service change the base image to the one below
    # FROM mcr.microsoft.com/azure-functions/node:2.0-appservice
    FROM mcr.microsoft.com/azure-functions/node:2.0
    
    ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
        AzureFunctionsJobHost__Logging__Console__IsEnabled=true
    
    COPY . /home/site/wwwroot
    
    RUN cd /home/site/wwwroot && \
    npm install    
    ```
    ::: zone-end

    ::: zone pivot="programming-language-powershell"
    ```Dockerfile
    # To enable ssh & remote debugging on app service change the base image to the one below
    # FROM mcr.microsoft.com/azure-functions/powershell:2.0-appservice
    FROM mcr.microsoft.com/azure-functions/powershell:2.0
    ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
        AzureFunctionsJobHost__Logging__Console__IsEnabled=true
    
    COPY . /home/site/wwwroot    
    ```
    ::: zone-end

    ::: zone pivot="programming-language-python"
    ```Dockerfile
    # To enable ssh & remote debugging on app service change the base image to the one below
    # FROM mcr.microsoft.com/azure-functions/python:2.0-python3.7-appservice
    FROM mcr.microsoft.com/azure-functions/python:2.0-python3.7
    
    ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
        AzureFunctionsJobHost__Logging__Console__IsEnabled=true
    
    COPY requirements.txt /
    RUN pip install -r /requirements.txt
    
    COPY . /home/site/wwwroot    
    ```
    ::: zone-end

    ::: zone pivot="programming-language-typescript"
    ```Dockerfile
    # To enable ssh & remote debugging on app service change the base image to the one below
    # FROM mcr.microsoft.com/azure-functions/node:2.0-appservice
    FROM mcr.microsoft.com/azure-functions/node:2.0
    
    ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
        AzureFunctionsJobHost__Logging__Console__IsEnabled=true
    
    COPY . /home/site/wwwroot
    
    RUN cd /home/site/wwwroot && \
    npm install    
    ```
    ::: zone-end

    > [!NOTE]
    > Azure İşlevleri için desteklenen temel görüntülerin tam listesini [Azure İşlevleri temel resim sayfasında](https://hub.docker.com/_/microsoft-azure-functions-base)bulabilirsiniz.
    
1. *LocalFunctionsProject* klasöründe docker [build](https://docs.docker.com/engine/reference/commandline/build/) komutunu çalıştırın ve `azurefunctionsimage`bir ad `v1.0.0`ve etiket sağlayın. `<docker_id>` değerini Docker Hub hesabınızın kimliğiyle değiştirin. Bu komut, kapsayıcı için Docker görüntüsünü derler.

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.0 .
    ```
    
    Komut tamamlandığında, yeni kapsayıcıyı yerel olarak çalıştırabilirsiniz.
    
1. Yapıyı test etmek için, docker [run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanarak görüntüyü `<docker_id>` yerel bir kapsayıcıda çalıştırın, `-p 8080:80`Docker Kimliğinizi yeniden değiştirin ve bağlantı noktaları bağımsız değişkenini ekleyerek:

    ```
    docker run -p 8080:80 -it <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
1. Görüntü yerel bir kapsayıcıda çalışırken, aşağıda `http://localhost:8080`gösterilen yer tutucu görüntüsünü görüntülemek için bir tarayıcı açın. İşleviniz Azure'da olduğu gibi yerel kapsayıcıda çalıştığından, görüntü bu noktada görünür, bu da `"authLevel": "function"` özellik ile birlikte *function.json'da* tanımlandığı gibi bir erişim anahtarı tarafından korunduğu anlamına gelir. Ancak kapsayıcı henüz Azure'daki bir işlev uygulamasında yayınlanmamıştır, bu nedenle anahtar henüz kullanılamamıştır. Yerel olarak test etmek istiyorsanız, docker'ı durdurun, yetkilendirme özelliğini `"authLevel": "anonymous"`değiştirin, görüntüyü yeniden oluşturun ve docker'ı yeniden başlatın. Sonra `"authLevel": "function"` *function.json*sıfırlamak . Daha fazla bilgi için [yetkilendirme anahtarlarına](functions-bindings-http-webhook-trigger.md#authorization-keys)bakın.

    ![Konteynerin yerel olarak çalıştığını belirten yer tutucu görüntüsü](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

1. Kapsayıcıdaki fonksiyon uygulamasını doğruladıktan sonra **Ctrl**+**C**ile docker'ı durdurun.

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

1. Azure hesabınızda oturum açın ve kapsayıcıyla bağlantı kurmak için **SSH'yi** seçin. Azure kapsayıcı görüntüsünü güncelleştirme sürecindeyse bağlanma birkaç dakika sürebilir.

1. Kapsayıcınızla bir bağlantı kurulduktan sonra, `top` şu anda çalışan işlemleri görüntülemek için komutu çalıştırın. 

    ![SSH oturumunda çalışan Linux üst komutu](media/functions-create-function-linux-custom-image/linux-custom-kudu-ssh-top.png)

## <a name="write-to-an-azure-storage-queue"></a>Azure Depolama kuyruğuna yazma

Azure İşlevleri, işlevlerinizi kendi tümleştirme kodunuzu yazmak zorunda olan diğer Azure hizmetlerine ve kaynaklarına bağlamanızı sağlar. Hem giriş hem de çıktıyı temsil eden bu *bağlamalar*işlev tanımı içinde bildirilir. Bağlamalardan alınan veriler işleve parametre olarak sağlanır. *Tetikleyici,* özel bir giriş bağlama türüdür. Bir işlevin yalnızca bir tetikleyicisi olmasına rağmen, birden çok giriş ve çıkış bağlaması olabilir. Daha fazla bilgi edinmek için Azure [İşlevlerini tetikler ve bağlama kavramlarını](functions-triggers-bindings.md)öğrenin.

Bu bölümde, işlevinizi bir Azure Depolama kuyruğuyla nasıl tümleştiracağınızı gösterir. Bu işleve eklediğiniz çıktı bağlama, bir HTTP isteğindeki verileri kuyruktaki bir iletiye yazar.

## <a name="retrieve-the-azure-storage-connection-string"></a>Azure Depolama bağlantı dizesini alın

Daha önce, işlev uygulaması tarafından kullanılmak üzere bir Azure Depolama hesabı oluşturabilirsiniz. Bu hesabın bağlantı dizesi Azure'daki uygulama ayarlarında güvenli bir şekilde depolanır. Ayarı *local.settings.json* dosyasına indirerek, işlevi yerel olarak çalıştırırken bu bağlantıyı aynı hesaptaki Depolama kuyruğuna yazma'yı kullanabilirsiniz. 

1. Projenin kökünden, önceki hızlı başlangıçtaki işlev `<app_name>` uygulamanızın adını değiştirerek aşağıdaki komutu çalıştırın. Bu komut, dosyadaki varolan değerlerin üzerine yazılır.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. *local.settings.json'u* açın ve `AzureWebJobsStorage`Depolama hesabı bağlantı dizesi olan adlı değeri bulun. Bu makalenin `AzureWebJobsStorage` diğer bölümlerinde adı ve bağlantı dizesini kullanırsınız.

> [!IMPORTANT]
> *local.settings.json* Azure'dan indirilen sırları içerdiğinden, bu dosyayı her zaman kaynak denetiminden hariç tutar. Yerel işlevler projesiyle oluşturulan *.gitignore* dosyası varsayılan olarak dosyayı dışlar.

### <a name="add-an-output-binding-to-functionjson"></a>function.json'a çıktı bağlama ekleme

Azure İşlevlerinde, her bağlama `direction` `type` `name` *türü, function.json* dosyasında tanımlanması gereken bir , ve benzersiz bir bağlama gerektirir. *Function.json* zaten "httpTrigger" türü için bir giriş bağlama ve HTTP yanıtı için bir çıkış bağlama içerir. Depolama sırasına bağlama eklemek için, sıranın kodda bir giriş bağımsız değişkeni olarak `msg`göründüğü "sıra" türü için çıktı bağlayıcısı ekleyen dosyayı aşağıdaki gibi değiştirin. Sıra bağlama da kullanmak için sıranın adını gerektirir, bu durumda `outqueue`, ve bağlantı dizesini `AzureWebJobStorage`tutan ayarların adı, bu durumda .

::: zone pivot="programming-language-csharp"

C# sınıfı kitaplık projesinde, bağlamalar işlev yöntemine bağlama öznitelikleri olarak tanımlanır. *Function.json* dosyası daha sonra bu özniteliklere göre otomatik olarak oluşturulur.

1. Sıra bağlama için, projenize Depolama uzantısı paketini eklemek için aşağıdaki [dotnet ekle paket](/dotnet/core/tools/dotnet-add-package) komutunu çalıştırın.

    ```
    dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
    ```

1. *HttpTrigger.cs* dosyasını açın ve `using` aşağıdaki ifadeyi ekleyin:

    ```cs
    using Microsoft.Azure.WebJobs.Extensions.Storage;
    ```
    
1. Yöntem tanımına aşağıdaki `Run` parametreyi ekleyin:
    
    ```csharp
    [Queue("outqueue"), StorageAccount("AzureWebJobsStorage")] ICollector<string> msg
    ```
    
    `Run` Yöntem tanımı şimdi aşağıdaki kod la eşleşmelidir:
    
    ```csharp
    [FunctionName("HttpTrigger")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
        [Queue("outqueue"), StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
    ```

`msg` Parametre, işlev `ICollector<T>` tamamlandığında çıktı bağlamasına yazılan iletiler koleksiyonunu temsil eden bir türdür. Bu durumda, çıktı adlı `outqueue`bir depolama sırasıdır. Depolama hesabının `StorageAccountAttribute`bağlantı dizesi . Bu öznitelik, Depolama hesabı bağlantı dizesini içeren ve sınıf, yöntem veya parametre düzeyinde uygulanabilen ayarı gösterir. Bu durumda, varsayılan depolama `StorageAccountAttribute` hesabını zaten kullandığınıziçin atlayabilirsiniz.

::: zone-end

::: zone pivot="programming-language-javascript"

HTTP bağlama sonra sıra bağlama ekleyerek aşağıdaki leri eşleştirmek için *function.json* güncelleştirin:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```
::: zone-end

::: zone pivot="programming-language-powershell"

HTTP bağlama sonra sıra bağlama ekleyerek aşağıdaki leri eşleştirmek için *function.json* güncelleştirin:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "Request",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "Response"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```
::: zone-end

::: zone pivot="programming-language-python"

HTTP bağlama sonra sıra bağlama ekleyerek aşağıdaki leri eşleştirmek için *function.json* güncelleştirin:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```
::: zone-end

::: zone pivot="programming-language-typescript"

HTTP bağlama sonra sıra bağlama ekleyerek aşağıdaki leri eşleştirmek için *function.json* güncelleştirin:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "Request",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "Response"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```
::: zone-end

## <a name="add-code-to-use-the-output-binding"></a>Çıktı bağlamayı kullanmak için kod ekleme

Bağlama tanımlandıktan sonra, bağlama nın adı, `msg`bu durumda işlev kodunda bağımsız değişken olarak `context` (veya JavaScript ve TypeScript'teki nesnede) görünür. Daha sonra sıraya ileti yazmak için bu değişkeni kullanabilirsiniz. Kimlik doğrulama, sıra başvurusu alma veya veri yazma için herhangi bir kod yazmanız gerekir. Tüm bu tümleştirme görevleri Azure İşlevleri çalışma zamanında ve sıra çıktısına bağlamada rahatlıkla işlenir.

::: zone pivot="programming-language-csharp"
```csharp
[FunctionName("HttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [Queue("outqueue"), StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    if (!string.IsNullOrEmpty(name))
    {
        // Add a message to the output collection.
        msg.Add(string.Format("Name passed to the function: {0}", name));
    }
    
    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```
::: zone-end

::: zone pivot="programming-language-javascript"
```js
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    if (req.query.name || (req.body && req.body.name)) {
        // Add a message to the Storage queue.
        context.bindings.msg = "Name passed to the function: " +
            (req.query.name || req.body.name);

        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};
```
::: zone-end

::: zone pivot="programming-language-powershell"
```powershell
using namespace System.Net

# Input bindings are passed in via param block.
param($Request, $TriggerMetadata)

# Write to the Azure Functions log stream.
Write-Host "PowerShell HTTP trigger function processed a request."

# Interact with query parameters or the body of the request.
$name = $Request.Query.Name
if (-not $name) {
    $name = $Request.Body.Name
}

if ($name) {
    $outputMsg = "Name passed to the function: $name"
    Push-OutputBinding -name msg -Value $outputMsg

    $status = [HttpStatusCode]::OK
    $body = "Hello $name"
}
else {
    $status = [HttpStatusCode]::BadRequest
    $body = "Please pass a name on the query string or in the request body."
}

# Associate values to output bindings by calling 'Push-OutputBinding'.
Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = $status
    Body = $body
})
```
::: zone-end

::: zone pivot="programming-language-python"
```python
import logging

import azure.functions as func


def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> str:

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        msg.set(name)
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
```
::: zone-end

::: zone pivot="programming-language-typescript"
```typescript
import { AzureFunction, Context, HttpRequest } from "@azure/functions"

const httpTrigger: AzureFunction = async function (context: Context, req: HttpRequest): Promise<void> {
    context.log('HTTP trigger function processed a request.');
    const name = (req.query.name || (req.body && req.body.name));

    if (name) {
        // Add a message to the Storage queue.
        context.bindings.msg = "Name passed to the function: " +
            (req.query.name || req.body.name);
        
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};

export default httpTrigger;
```
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

[Kuyruğa Azure portalında](../storage/queues/storage-quickstart-queues-portal.md) veya [Microsoft Azure Depolama Gezgini'nde](https://storageexplorer.com/)görüntüleyebilirsiniz. Azure CLI'deki sırayı aşağıdaki adımlarda açıklandığı gibi de görüntüleyebilirsiniz:

1. İşlev projesinin *yerel.setting.json* dosyasını açın ve bağlantı dize değerini kopyalayın. Bir terminal veya komut penceresinde, aşağıdaki komutu çalıştırarak bir ortam değişkeni oluşturmak için , `AZURE_STORAGE_CONNECTION_STRING`'yerine belirli bağlantı dizesi yapıştırma `<connection_string>`. (Bu ortam değişkeni, `--connection-string` bağımsız değişkeni kullanarak sonraki her komuta bağlantı dizesi sağlamanız gerekolmadığı anlamına gelir.)

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
    ```
    
    # <a name="powershell"></a>[Powershell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<connection_string>"
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    set AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
    ```
    
    ---
    
1. (İsteğe bağlı) Hesabınızdaki [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) Depolama kuyruklarını görüntülemek için komutu kullanın. Bu komutun çıktısı, işlev `outqueue`bu kuyruğa ilk iletisini yazdığında oluşturulan , adlı bir sıra içermelidir.
    
    # <a name="bash"></a>[bash](#tab/bash)
    
    ```azurecli
    az storage queue list --output tsv
    ```
    
    # <a name="powershell"></a>[Powershell](#tab/powershell)
    
    ```azurecli
    az storage queue list --output tsv
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```azurecli
    az storage queue list --output tsv
    ```
    
    ---

1. İşlevdaha önce sınama sırasında kullandığınız ilk ad olmalıdır bu sırada iletileri görüntülemek için [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek) komutu kullanın. Komut, [base64 kodlamasında](functions-bindings-storage-queue-trigger.md#encoding)kuyruktaki ilk iletiyi alır, bu nedenle metin olarak görüntülemek için iletiyi deşifre etmeniz gerekir.

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershell"></a>[Powershell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    İleti koleksiyonunu dereference'ı deşifre edip base64'ten çözmeniz, PowerShell'i çalıştırmanız ve PowerShell komutunu kullanmanız gerektiğinden.

    ---

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
