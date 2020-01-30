---
title: Linux üzerinde özel görüntü kullanarak Azure Işlevleri oluşturma
description: Özel bir Linux görüntüsü üzerinde çalışan Azure İşlevleri oluşturmayı öğrenin.
ms.date: 01/15/2020
ms.topic: tutorial
ms.custom: mvc
zone_pivot_groups: programming-languages-set-functions01
ms.openlocfilehash: edb947f0748c186e146bce5f4dbe9d0b95a2568d
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846493"
---
# <a name="create-a-function-on-linux-using-a-custom-container"></a>Linux üzerinde özel kapsayıcı kullanarak bir işlev oluşturma

Bu öğreticide, bir Linux temel görüntüsü kullanarak özel bir Docker kapsayıcısı olarak Python kodu oluşturup Azure Işlevleri 'ne dağıtırsınız. İşlevleriniz belirli bir dil sürümü gerektirdiğinde veya yerleşik görüntü tarafından sağlanmayan belirli bir bağımlılığı ya da yapılandırmaya sahip olduğunda genellikle özel bir görüntü kullanırsınız.

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

Windows, Mac OS veya Linux çalıştıran tüm bilgisayarlarda bu öğreticiyi izleyebilirsiniz. Öğreticiyi tamamlamak, Azure hesabınızda birkaç ABD Doları maliyetlerinden oluşur.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure Functions Core Tools](./functions-run-local.md#v2) sürüm 2.7.1846 veya sonraki bir sürümü
- [Azure CLI](/cli/azure/install-azure-cli) sürüm 2.0.77 veya üzeri
- [Azure işlevleri 2. x çalışma zamanı](functions-versions.md)
- Aşağıdaki dil çalışma zamanı bileşenleri:
    ::: zone pivot="programming-language-csharp"
    - [.NET Core 2.2. x veya üzeri](https://dotnet.microsoft.com/download)
    ::: zone-end
    ::: zone pivot="programming-language-javascript"
    - [Node.js](https://nodejs.org/en/download/)
    ::: zone-end
    ::: zone pivot="programming-language-powershell"
    - [PowerShell](/powershell/scripting/install/installing-windows-powershell?view=powershell-7)
    ::: zone-end
    ::: zone pivot="programming-language-python"
    - [Python 3,6-64 bit](https://www.python.org/downloads/release/python-3610/) veya [Python 3,7-64 bit](https://www.python.org/downloads/release/python-376/)
    ::: zone-end
    ::: zone pivot="programming-language-typescript"
    - [Node.js](https://nodejs.org/en/download/)
    - [TypeScript](http://www.typescriptlang.org/#download-links)
    ::: zone-end
- [Docker](https://docs.docker.com/install/)
- Bir [Docker kimliği](https://hub.docker.com/signup)

### <a name="prerequisite-check"></a>Önkoşul denetimi

1. Bir Terminal veya komut penceresinde, Azure Functions Core Tools sürüm 2.7.1846 veya üzeri olduğunu denetlemek için `func --version` çalıştırın.
1. Azure CLı sürümünün 2.0.76 veya üzeri olduğunu denetlemek için `az --version` çalıştırın.
1. Azure 'da oturum açmak ve etkin bir aboneliği doğrulamak için `az login` çalıştırın.
1. Docker 'da oturum açmak için `docker login` çalıştırın. Docker çalışmıyorsa bu komut başarısız olur, bu durumda Docker başlatılır ve komutu yeniden deneyin.

## <a name="create-and-test-the-local-functions-project"></a>Yerel işlevler projesi oluşturma ve test etme

1. Bir Terminal veya komut isteminde, bu öğretici için uygun bir konumda bir klasör oluşturun ve ardından bu klasöre gidin.

1. Bu öğreticide kullanmak üzere sanal ortam oluşturma [ve etkinleştirme](functions-create-first-function-python.md#create-and-activate-a-virtual-environment) ile ilgili yönergeleri izleyin.

1. `LocalFunctionsProject`adlı bir klasörde işlev uygulaması projesi oluşturmak için seçtiğiniz dil için aşağıdaki komutu çalıştırın. `--docker` seçeneği, Azure Işlevleri ve seçilen çalışma zamanı ile kullanılmak üzere uygun bir özel kapsayıcıyı tanımlayan proje için bir `Dockerfile` oluşturur.

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
    
1. Aşağıdaki komutu kullanarak projenize bir işlev ekleyin; burada `--name` bağımsız değişkeni işlevinizin benzersiz adıdır ve `--template` bağımsız değişkeni işlevin tetikleyicisini belirtir. `func new` projenin seçilen diline ve *function. JSON*adlı yapılandırma dosyasına uygun bir kod dosyası içeren işlev adıyla eşleşen bir alt klasör oluşturun.

    ```
    func new --name HttpExample --template "HTTP trigger"
    ```

1. İşlevi yerel olarak test etmek için *Localfunctionsproject* klasöründe yerel Azure işlevleri çalışma zamanı konağını başlatın:
   
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

1. Çıktıda `HttpExample` uç noktası göründüğünü gördüğünüzde, `http://localhost:7071/api/HttpExample?name=Functions`' a gidin. Tarayıcı "Hello, Functions" (seçtiğiniz programlama diline bağlı olarak değişebilir) gibi bir ileti görüntülemelidir.

1. Ana bilgisayarı durdurmak için **Ctrl**-**C** kullanın.

## <a name="build-the-container-image-and-test-locally"></a>Kapsayıcı görüntüsünü oluşturma ve yerel olarak test etme

1. Seçim *Localfunctionsproj* klasöründe * dockerfile "öğesini inceleyin. Dockerfile, Linux üzerinde işlev uygulamasını çalıştırmak için gerekli ortamı açıklar: 

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
    > Azure Işlevleri için desteklenen temel görüntülerin tüm listesi, [Azure işlevleri temel görüntü sayfasında](https://hub.docker.com/_/microsoft-azure-functions-base)bulunabilir.
    
1. *Localfunctionsproject* klasöründe [Docker Build](https://docs.docker.com/engine/reference/commandline/build/) komutunu çalıştırın ve bir ad, `azurefunctionsimage`ve etiket sağlayın `v1.0.0`. `<docker_id>` değerini Docker Hub hesabınızın kimliğiyle değiştirin. Bu komut, kapsayıcı için Docker görüntüsünü derler.

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.0 .
    ```
    
    Komut tamamlandığında, yeni kapsayıcıyı yerel olarak çalıştırabilirsiniz.
    
1. Derlemeyi test etmek için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanarak görüntüyü yerel bir kapsayıcıda çalıştırın, yeniden `<docker_id>`, DOCKER Kimliğiniz ile değiştirin ve bağlantı noktası bağımsız değişkenini ekleyin `-p 8080:80`:

    ```
    docker run -p 8080:80 -it <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
1. Görüntü yerel kapsayıcıda çalışmaya başladıktan sonra, aşağıda gösterilen yer tutucu görüntüsünü görüntülemesi gereken `http://localhost:8080`için bir tarayıcı açın. Bu noktada, işleviniz Azure 'da olduğu gibi yerel kapsayıcıda çalıştığı ve bu, `"authLevel": "function"` özelliği ile *function. JSON* içinde tanımlanan bir erişim anahtarı tarafından korunduğu için görüntü bu noktada görünür. Kapsayıcı henüz Azure 'da bir işlev uygulamasına yayımlanmadı, bu nedenle anahtar henüz kullanılamıyor. Yerel olarak test etmek istiyorsanız Docker 'ı durdurun, yetkilendirme özelliğini `"authLevel": "anonymous"`olarak değiştirin, görüntüyü yeniden derleyin ve Docker 'ı yeniden başlatın. Sonra *function. JSON*içinde `"authLevel": "function"` sıfırlayın. Daha fazla bilgi için bkz. [Yetkilendirme anahtarları](functions-bindings-http-webhook.md#authorization-keys).

    ![Kapsayıcının yerel olarak çalıştığını gösteren yer tutucu resim](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

1. Kapsayıcıda işlev uygulamasını doğruladıktan sonra, **Ctrl**+**C**ile Docker 'ı durdurun.

## <a name="push-the-image-to-docker-hub"></a>Görüntüyü Docker Hub 'a gönderme

Docker Hub, görüntüleri barındıran ve görüntü ve kapsayıcı hizmetleri sağlayan bir kapsayıcı kayıt defteridir. Azure 'a dağıtma da dahil olmak üzere görüntünüzü paylaşmak için bir kayıt defterine göndermeniz gerekir.

1. Docker 'da henüz oturum açmadıysanız, Docker [login](https://docs.docker.com/engine/reference/commandline/login/) komutuyla `<docker_id>` değiştirerek bunu DOCKER kimliği ile değiştirin. Bu komut sizden Kullanıcı adı ve parola girmenizi ister. "Oturum açma başarılı oldu" iletisi oturum açtığınızdan emin olur.

    ```
    docker login
    ```
    
1. Oturum açtıktan sonra Docker [Push](https://docs.docker.com/engine/reference/commandline/push/) komutunu kullanarak görüntüyü Docker Hub 'a gönderin, tekrar `<docker_id>` DOCKER Kimliğiniz ile değiştirin.

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
    
1. [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, `westeurope` bölgesinde `AzureFunctionsContainers-rg` adlı bir kaynak grubu oluşturur. (`az account list-locations` komutundan kullanılabilir bir bölge kullanarak genellikle kaynak grubunuzu ve kaynaklarınızı size yakın bir bölgede oluşturursunuz.)

    ```azurecli
    az group create --name AzureFunctionsContainers-rg --location westeurope
    ```
    
    > [!NOTE]
    > Linux ve Windows uygulamalarını aynı kaynak grubunda barındıralamazsınız. Bir Windows işlev uygulaması veya Web uygulamasıyla `AzureFunctionsContainers-rg` adlı mevcut bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.
    
1. [Az Storage Account Create](/cli/azure/storage/account#az-storage-account-create) komutunu kullanarak kaynak grubunuzda ve bölgenizde genel amaçlı bir depolama hesabı oluşturun. Aşağıdaki örnekte, `<storage_name>`, sizin için uygun olan bir genel benzersiz adla değiştirin. Adlar yalnızca üç ile 24 karakter arasında ve küçük harflerden oluşmalıdır. `Standard_LRS` tipik bir genel amaçlı hesabı belirtir.

    ```azurecli
    az storage account create --name <storage_name> --location westeurope --resource-group AzureFunctionsContainers-rg --sku Standard_LRS
    ```
    
    Depolama hesabı, bu öğretici için yalnızca birkaç USD ilay.
    
1. **Elastik Premium 1** fiyatlandırma katmanında (`--sku EP1`Batı Avrupa) `myPremiumPlan` adlı Azure Işlevleri için Premium bir plan oluşturmak için komutunu kullanın (`-location westeurope`ya da yakınınızdaki uygun bir bölge kullanın) ve bir Linux kapsayıcısında (`--is-linux`).

    ```azurecli
    az functionapp plan create --resource-group AzureFunctionsContainers-rg --name myPremiumPlan --location westeurope --number-of-workers 1 --sku EP1 --is-linux
    ```   

    Özel işlevler kapsayıcıları için Linux barındırma, [adanmış (App Service) planlar](functions-scale.md#app-service-plan) ve [Premium planlarda](functions-premium-plan.md#features)desteklenir. Burada, gerektiğinde ölçeklenebilen Premium planı kullanıyoruz. Barındırma hakkında daha fazla bilgi edinmek için, bkz. [Azure İşlevleri barındırma planları karşılaştırması](functions-scale.md). Maliyetleri hesaplamak için bkz. [işlevler fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/).

    Bu komut Ayrıca, aynı kaynak grubunda, işlev uygulamanızı izleyebilmeniz ve günlükleri görüntüleyebileceğiniz ilişkili bir Azure Application Insights örneğini sağlar. Daha fazla bilgi için bkz. [Azure Işlevlerini izleme](functions-monitoring.md). Örnek, siz etkinleştirene kadar hiçbir maliyet vermez.

## <a name="create-and-configure-a-function-app-on-azure-with-the-image"></a>Azure 'da görüntüyle bir işlev uygulaması oluşturma ve yapılandırma

Azure 'daki bir işlev uygulaması, barındırma planınızdaki işlevlerinizin yürütülmesini yönetir. Bu bölümde, Docker Hub 'daki görüntüden bir işlev uygulaması oluşturmak ve bunu Azure depolama 'ya bir bağlantı dizesiyle yapılandırmak için önceki bölümde yer alan Azure kaynaklarını kullanırsınız.

1. [Az functionapp Create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak işlevler uygulamasını oluşturun. Aşağıdaki örnekte `<storage_name>` değerini depolama hesabı için önceki bölümde kullandığınız adla değiştirin. Ayrıca `<app_name>`, sizin için uygun bir genel benzersiz adla değiştirin ve Docker KIMLIĞINIZ ile `<docker_id>`.

    ```azurecli
    az functionapp create --name <app_name> --storage-account <storage_name> --resource-group AzureFunctionsContainers-rg --plan myPremiumPlan --deployment-container-image-name <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
    *Dağıtım-kapsayıcı-görüntü-adı* parametresi, işlev uygulaması için kullanılacak resmi belirtir. Dağıtım için kullanılan görüntü hakkındaki bilgileri görüntülemek için [az functionapp config Container Show](/cli/azure/functionapp/config/container#az-functionapp-config-container-show) komutunu kullanabilirsiniz. Farklı bir görüntüden dağıtmak için [az functionapp config Container set](/cli/azure/functionapp/config/container#az-functionapp-config-container-set) komutunu da kullanabilirsiniz.

1. [Az Storage Account Show-Connection-String](/cli/azure/storage/account) komutunu kullanarak oluşturduğunuz depolama hesabı için bağlantı dizesini alın, bunu bir Shell değişkenine atayarak `storageConnectionString`:

    ```azurecli
    az storage account show-connection-string --resource-group AzureFunctionsContainers-rg --name <storage_name> --query connectionString --output tsv
    
1. Add this setting to the function app by using the [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) command. In the following command, replace `<app_name>` with the name of your function app, and replace `<connection_string>` with the connection string from the previous step (a long encoded string that begins with "DefaultEndpointProtocol="):
 
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
> Özel görüntünüzü özel bir kapsayıcı hesabına yayımlarsanız, bunun yerine bağlantı dizesinin Dockerfile içindeki ortam değişkenlerini kullanmanız gerekir. Daha fazla bilgi için bkz. [env yönergesi](https://docs.docker.com/engine/reference/builder/#env). `DOCKER_REGISTRY_SERVER_USERNAME` ve `DOCKER_REGISTRY_SERVER_PASSWORD`değişkenlerini de ayarlamanız gerekir. Değerleri kullanmak için, görüntüyü yeniden oluşturmanız, görüntüyü kayıt defterine göndermeniz ve sonra işlev uygulamasını Azure 'da yeniden başlatmanız gerekir.

## <a name="verify-your-functions-on-azure"></a>Azure 'da işlevlerinizi doğrulama

Azure 'da işlev uygulamasına dağıtılan görüntü ile, artık işlevi HTTP istekleri aracılığıyla çalıştırabilirsiniz. *Function. JSON* tanımı `"authLevel": "function"`özelliğini içerdiğinden, önce erişim anahtarını edinmeniz ("işlev anahtarı" olarak da bilinir) ve bunu uç noktaya herhangi BIR istekte URL parametresi olarak eklemeniz gerekir.

1. Azure portal kullanarak veya `az rest` komutuyla Azure CLı kullanarak, işlev URL 'sini erişim (işlev) anahtarıyla alın.)

    # <a name="portaltabportal"></a>[Portal](#tab/portal)

    1. Azure portal oturum açın ve ardından sayfanın üst kısmındaki **arama** kutusuna işlev uygulamanızın adını girerek işlev uygulamanızı bulun. Sonuçlarda **App Service** kaynağını seçin.

    1. Sol gezinti panelinde, **işlevler (salt okuma)** altında, işlevinizin adını seçin.

    1. Ayrıntılar panelinde **</> işlev URL 'Sini al**' ı seçin:
    
        ![Azure portal işlev URL 'sini Al komutu](./media/functions-create-function-linux-custom-image/functions-portal-get-url-key.png)   

    1. Açılan pencerede **varsayılan (işlev anahtarı)** öğesini seçin ve sonra **kopyalayın**. Anahtar, `?code=`izleyen karakterlerin dizesidir.

        ![Azure portal işlev URL 'SI kopyalanıyor](./media/functions-create-function-linux-custom-image/functions-portal-get-url-key-popup.png)   

    > [!NOTE]  
    > İşlev uygulamanız bir kapsayıcı olarak dağıtıldığından, portalda işlev kodunuzda değişiklik yapamazsınız. Bunun yerine yerel görüntüde projeyi güncelleştirmeniz, görüntüyü kayıt defterine yeniden göndermeniz ve sonra Azure 'a yeniden dağıtmanız gerekir. Sonraki bölümde sürekli dağıtım ayarlayabilirsiniz.
    
    # <a name="azure-clitabazurecli"></a>[Azure CLI](#tab/azurecli)

    1. `<subscription_id>`, `<resource_group>`ve `<app_name>` Azure abonelik KIMLIĞINIZLE, işlev uygulamanızın kaynak grubuyla ve işlev uygulamanızın adını sırasıyla değiştirerek aşağıdaki biçimde bir URL dizesi oluşturun:

        ```
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Web/sites/<app_name>/host/default/listKeys?api-version=2018-11-01"
        ```

        Örneğin, URL şu adrese benzeyebilir:

        ```
        "/subscriptions/1234aaf4-1234-abcd-a79a-245ed34eabcd/resourceGroups/AzureFunctionsContainers-rg/providers/Microsoft.Web/sites/msdocsfunctionscontainer/host/default/listKeys?api-version=2018-11-01"
        ```

        > [!TIP]
        > Bunun için, URL 'YI bir ortam değişkenine atayıp `az rest` komutunda kullanabilirsiniz.
    
    1. Aşağıdaki `az rest` komutunu çalıştırın (Azure CLı sürümünde 2.0.77 ve üzeri sürümlerde bulunur), tırnak işaretleri dahil olmak üzere son adımdaki URI dizesiyle birlikte `<uri>` değiştirin:

        ```azurecli
        az rest --method post --uri <uri> --query functionKeys.default --output tsv
        ```

    1. Komutun çıktısı, işlev anahtarıdır. Tam işlev URL 'SI `https://<app_name>.azurewebsites.net/api/<function_name>?code=<key>``<app_name>`, `<function_name>`ve `<key>` belirli değerlerinizle değiştirin.
    
        > [!NOTE]
        > Burada alınan anahtar, işlevler uygulamasındaki tüm işlevler için çalışır olan *ana bilgisayar* anahtarıdır; Portal için gösterilen yöntem yalnızca bir işlev için anahtarı alır.

    ---

1. İşlev URL 'sini tarayıcınızın adres çubuğuna yapıştırın, bu URL 'nin sonuna `&name=Azure` parametresini ekleyin. "Hello Azure" gibi metinler tarayıcıda görünmelidir.

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-function-linux-custom-image/function-app-browser-testing.png)

1. Yetkilendirmeyi test etmek için, URL 'den Code = parametresini kaldırın ve işlevden yanıt bulunmadığını doğrulayın.


## <a name="enable-continuous-deployment-to-azure"></a>Azure 'a sürekli dağıtımı etkinleştir

Kayıt defterinde görüntüyü güncelleştirdiğinizde bir görüntü dağıtımınızı otomatik olarak güncelleştirmek için Azure Işlevleri 'ni etkinleştirebilirsiniz.

1. [Az functionapp Deployment Container config](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-config) komutunu kullanarak sürekli dağıtımı etkinleştirin ve `<app_name>` işlev uygulamanızın adıyla değiştirin:

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

1. Dockerfile 'da, `FROM` yönergesindeki `-appservice` dize öğesini temel görüntüye ekleyin:

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

1. `docker build` komutunu tekrar kullanarak görüntüyü yeniden oluşturun ve `<docker_id>` Docker KIMLIĞINIZ ile değiştirin:

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.0 .
    ```
    
1. Güncelleştirilmiş görüntüyü Docker Hub 'a gönderin. Bu, ilk gönderiden yalnızca görüntünün güncelleştirilmiş segmentlerinin karşıya yüklenmesi gereken zamandan çok daha az zaman alır.

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.0
    ```
    
1. Azure Işlevleri otomatik olarak görüntüyü işlevler uygulamanıza yeniden dağıtır; işlem bir dakikadan kısa bir sürede gerçekleşir.

1. Bir tarayıcıda, `<app_name>` `https://<app_name>.scm.azurewebsites.net/`açın ve benzersiz adınızla değiştirin. Bu URL, işlev uygulaması Kapsayıcınız için gelişmiş araçlar (kudu) uç noktasıdır.

1. Azure hesabınızda oturum açın ve kapsayıcıya bir bağlantı kurmak için **SSH** 'yi seçin. Azure 'un kapsayıcı görüntüsünü güncelleştirme işleminde hala devam ediyorsa, bağlanma işlemi birkaç dakika sürebilir.

1. Kapsayıcınıza bir bağlantı kurulduktan sonra, çalışmakta olan işlemlerin görüntülemek için `top` komutunu çalıştırın. 

    ![SSH oturumunda çalışan Linux üst komutu](media/functions-create-function-linux-custom-image/linux-custom-kudu-ssh-top.png)

## <a name="write-to-an-azure-storage-queue"></a>Azure depolama kuyruğuna yazma

Azure Işlevleri, işlevlerinizi kendi tümleştirme kodunuzu yazmak zorunda olan diğer Azure hizmetlerine ve kaynaklarına bağlamanıza olanak tanır. Hem giriş hem de çıktıyı temsil eden bu *bağlamalar*, işlev tanımı içinde bildirilmiştir. Bağlamalardan alınan veriler işleve parametre olarak sağlanır. *Tetikleyici* özel bir giriş bağlama türüdür. Bir işlevde yalnızca bir tetikleyici olsa da, birden çok giriş ve çıkış bağlaması olabilir. Daha fazla bilgi için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

Bu bölümde, işlevinizi bir Azure depolama kuyruğu ile tümleştirme işlemi gösterilmektedir. Bu işleve eklediğiniz çıkış bağlaması, verileri bir HTTP isteğinden kuyruktaki bir iletiye yazar.

## <a name="retrieve-the-azure-storage-connection-string"></a>Azure depolama bağlantı dizesini alma

Daha önce, işlev uygulaması tarafından kullanılmak üzere bir Azure depolama hesabı oluşturdunuz. Bu hesabın bağlantı dizesi, Azure 'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu ayarı *Local. Settings. JSON* dosyasına indirerek bu bağlantıyı, işlevi yerel olarak çalıştırırken aynı hesaptaki bir depolama kuyruğuna yazma işlemi kullanabilirsiniz. 

1. Projenin kökünden, aşağıdaki komutu çalıştırarak `<app_name>` önceki hızlı başlangıçta işlev uygulamanızın adıyla değiştirin. Bu komut, dosyadaki varolan tüm değerlerin üzerine yazar.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. *Local. Settings. JSON* ' ı açın ve depolama hesabı bağlantı dizesi olan `AzureWebJobsStorage`adlı değeri bulun. Ad `AzureWebJobsStorage` ve bağlantı dizesini Bu makalenin diğer bölümlerinde kullanırsınız.

> [!IMPORTANT]
> *Local. Settings. JSON* , Azure 'dan indirilen gizli dizileri içerdiğinden, bu dosyayı her zaman kaynak denetiminden dışlayın. Yerel işlevler projesiyle oluşturulan *. gitignore* dosyası, varsayılan olarak dosyayı dışlar.

### <a name="add-an-output-binding-to-functionjson"></a>Function. JSON öğesine çıkış bağlaması ekleyin

Azure Işlevlerinde her bağlama türü, *function. JSON* dosyasında tanımlanacak bir `direction`, `type`ve benzersiz bir `name` gerektirir. *İşleviniz. JSON* zaten "httptrigger" türü için bir giriş bağlaması ve http yanıtı için bir çıkış bağlaması içeriyor. Bir depolama kuyruğuna bağlama eklemek için, dosyayı aşağıdaki şekilde değiştirin. Bu, sıranın kodda `msg`adında bir giriş bağımsız değişkeni olarak göründüğü "Queue" türü için bir çıkış bağlaması ekler. Kuyruk bağlama Ayrıca kullanılacak kuyruğun adını, bu örnekte `outqueue`ve bağlantı dizesini tutan ayarların adını, bu örnekte `AzureWebJobStorage`gerektirir.

::: zone pivot="programming-language-csharp"

Bir C# sınıf kitaplığı projesinde bağlamalar, işlev yönteminde bağlama öznitelikleri olarak tanımlanır. *Function. JSON* dosyası daha sonra bu özniteliklere göre otomatik olarak oluşturulur.

1. Kuyruk bağlama için, depolama uzantısı paketini projenize eklemek üzere aşağıdaki [DotNet Package Add](/dotnet/core/tools/dotnet-add-package) komutunu çalıştırın.

    ```
    dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
    ```

1. *HttpTrigger.cs* dosyasını açın ve aşağıdaki `using` ifadesini ekleyin:

    ```cs
    using Microsoft.Azure.WebJobs.Extensions.Storage;
    ```
    
1. `Run` yöntemi tanımına aşağıdaki parametreyi ekleyin:
    
    ```csharp
    [Queue("outqueue"), StorageAccount("AzureWebJobsStorage")] ICollector<string> msg
    ```
    
    `Run` yöntemi tanımı şu kodla eşleşmelidir:
    
    ```csharp
    [FunctionName("HttpTrigger")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
        [Queue("outqueue"), StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
    ```

`msg` parametresi, işlev tamamlandığında çıkış bağlamaya yazılan bir ileti koleksiyonunu temsil eden bir `ICollector<T>` türüdür. Bu durumda, çıktı `outqueue`adlı bir depolama kuyruğudur. Depolama hesabının bağlantı dizesi `StorageAccountAttribute`tarafından ayarlanır. Bu öznitelik, depolama hesabı bağlantı dizesini içeren ayarı belirtir ve sınıfı, yöntemi veya parametre düzeyinde uygulanabilir. Bu durumda, zaten varsayılan depolama hesabını kullandığınızdan `StorageAccountAttribute` atlayabilirsiniz.

::: zone-end

::: zone pivot="programming-language-javascript"

HTTP bağlamadan sonra sıra bağlamayı ekleyerek, *function. JSON* öğesini aşağıdaki ile eşleşecek şekilde güncelleştirin:

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

HTTP bağlamadan sonra sıra bağlamayı ekleyerek, *function. JSON* öğesini aşağıdaki ile eşleşecek şekilde güncelleştirin:

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

HTTP bağlamadan sonra sıra bağlamayı ekleyerek, *function. JSON* öğesini aşağıdaki ile eşleşecek şekilde güncelleştirin:

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

HTTP bağlamadan sonra sıra bağlamayı ekleyerek, *function. JSON* öğesini aşağıdaki ile eşleşecek şekilde güncelleştirin:

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

## <a name="add-code-to-use-the-output-binding"></a>Çıkış bağlamayı kullanmak için kod ekleme

Bağlama tanımlandıktan sonra, bu durumda `msg`bağlamanın adı, işlev kodunda bağımsız değişken (veya JavaScript ve TypeScript içindeki `context` nesnesi) olarak görünür. Daha sonra bu değişkeni, kuyruğa ileti yazmak için kullanabilirsiniz. Kimlik doğrulaması, kuyruk başvurusu alma veya veri yazma için herhangi bir kod yazmanız gerekir. Tüm bu tümleştirme görevleri, Azure Işlevleri çalışma zamanı ve sıra çıkış bağlamasında kolayca işlenir.

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

### <a name="update-the-image-in-the-registry"></a>Kayıt defterindeki görüntüyü güncelleştirme

1. Kök klasörde `docker build` yeniden çalıştırın ve bu kez etiketteki sürümü `v1.0.1`olarak güncelleştirin. Daha önce olduğu gibi `<docker_id>` Docker Hub hesabı KIMLIĞINIZLE değiştirin:

    ```
    docker build --tag <docker_id>/azurefunctionsimage:v1.0.1
    ```
    
1. Güncelleştirilmiş görüntüyü `docker push`depoya geri gönderin:

    ```
    docker push <docker_id>/azurefunctionsimage:v1.0.1
    ```

1. Sürekli teslimi yapılandırdığınız için, kayıt defterindeki görüntüyü güncelleştirme işlemi otomatik olarak Azure 'daki işlev uygulamanızı güncelleştirir.

## <a name="view-the-message-in-the-azure-storage-queue"></a>Azure depolama sırasındaki iletiyi görüntüleme

Bir tarayıcıda, işlevinizi çağırmak için aynı URL 'YI kullanın. İşlev kodunun bu bölümünü değiştirmediğiniz için tarayıcı, önceki ile aynı yanıtı görüntülemelidir. Ancak eklenen kod, `outqueue` depolama kuyruğuna `name` URL parametresi kullanılarak bir ileti yazdı.

Kuyruğu [Azure Portal](../storage/queues/storage-quickstart-queues-portal.md) veya [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)görüntüleyebilirsiniz. Ayrıca, aşağıdaki adımlarda açıklandığı gibi kuyruğu Azure CLı 'de de görüntüleyebilirsiniz:

1. İşlevin proje *yerel. Setting. JSON* dosyasını açın ve bağlantı dizesi değerini kopyalayın. Bir Terminal veya komut penceresinde, `<connection_string>`yerine belirli bağlantı dizenizi yapıştırarak `AZURE_STORAGE_CONNECTION_STRING`adlı bir ortam değişkeni oluşturmak için aşağıdaki komutu çalıştırın. (Bu ortam değişkeni, `--connection-string` bağımsız değişkenini kullanarak sonraki her komuta bağlantı dizesi sağlamanız gerekmez.)

    # <a name="bashtabbash"></a>[Bash](#tab/bash)
    
    ```bash
    AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
    ```
    
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<connection_string>"
    ```
    
    # <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    set AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
    ```
    
    ---
    
1. Seçim Hesabınızdaki depolama kuyruklarını görüntülemek için [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) komutunu kullanın. Bu komutun çıktısı, işlev bu kuyruğa ilk iletisini yazmışsa oluşturulan `outqueue`adlı bir sıra içermelidir.
    
    # <a name="bashtabbash"></a>[Bash](#tab/bash)
    
    ```azurecli
    az storage queue list --output tsv
    ```
    
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
    
    ```azurecli
    az storage queue list --output tsv
    ```
    
    # <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)
    
    ```azurecli
    az storage queue list --output tsv
    ```
    
    ---

1. Bu kuyruktaki iletileri görüntülemek için [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek) komutunu kullanın. Bu, daha önce işlev test edilirken kullandığınız ilk ad olmalıdır. Komut, sıradaki ilk iletiyi [Base64 kodlamasında](functions-bindings-storage-queue.md#encoding)alır, bu nedenle metin olarak görüntülemek için iletinin kodunu da çözmelisiniz.

    # <a name="bashtabbash"></a>[Bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)
    
    İleti toplamayı başvuru yapmanız ve Base64 'den kod çözmelisiniz, PowerShell 'i çalıştırın ve PowerShell komutunu kullanın.

    ---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz kaynakları kullanarak Azure Işleviyle çalışmaya devam etmek istiyorsanız, tüm bu kaynakları yerinde bırakabilirsiniz. Azure Işlevleri için Premium bir plan oluşturduğunuza göre, sürekli maliyetlerde günlük bir veya iki USD olacaktır.

Devam eden maliyetlerin önüne geçmek için, bu gruptaki tüm kaynakları temizlemek üzere `AzureFunctionsContainer-rg` kaynak grubunu silin: 

```azurecli
az group delete --name AzureFunctionsContainer-rg
```

## <a name="next-steps"></a>Sonraki adımlar

+ [İzleme işlevleri](functions-monitoring.md)
+ [Ölçek ve barındırma seçenekleri](functions-scale.md)
+ [Kubernetes tabanlı sunucusuz barındırma](functions-kubernetes-keda.md)
