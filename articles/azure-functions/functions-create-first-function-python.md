---
title: Azure 'da HTTP ile tetiklenen bir işlev oluşturma
description: Azure işlevleri çekirdek araçları ve Azure CLI kullanarak Azure'da ilk Python uygulamanızı oluşturma konusunda bilgi edinin.
author: ggailey777
ms.author: glenga
ms.date: 09/11/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: gwallace
ms.openlocfilehash: 03b8e12d63ba84b4e20d7263f1c2ecb8d912936d
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203180"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Azure 'da HTTP ile tetiklenen bir işlev oluşturma

Bu makalede, Azure Işlevleri 'nde çalışan bir Python projesi oluşturmak için komut satırı araçlarının nasıl kullanılacağı gösterilmektedir. Ayrıca, bir HTTP isteği tarafından tetiklenen bir işlev oluşturursunuz. Son olarak, projenizi Azure 'da [sunucusuz bir işlev](functions-scale.md#consumption-plan) olarak çalışacak şekilde yayımlayabilirsiniz.

Bu makale, Azure Işlevleri için iki Python hızlı başlangıçdan birincisi. Bu hızlı başlangıcı tamamladıktan sonra, işleviniz için [bir Azure depolama kuyruğu çıkışı bağlama ekleyebilirsiniz](functions-add-output-binding-storage-queue-python.md) .

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapmanız gerekir:

+ [Python 3.6. x](https://www.python.org/downloads/)'i yükler.

+ [Azure Functions Core Tools](./functions-run-local.md#v2) sürüm 2.7.1575 veya sonraki bir sürümü yükler.

+ [Azure CLI](/cli/azure/install-azure-cli) sürüm 2. x veya sonraki bir sürümünü yükler.

+ Etkin bir Azure aboneliğiniz olmalıdır.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment-optional"></a>Sanal ortam oluşturma ve etkinleştirme (isteğe bağlı)

Python işlevlerini yerel olarak geliştirmek için bir Python 3.6. x ortamı kullanmanız gerekir. Oluşturma ve adlı bir sanal ortam etkinleştirmek için aşağıdaki komutları çalıştırın `.venv`.

> [!NOTE]
> Python, Linux dağıtımına venv yüklemediyse, aşağıdaki komutu kullanarak yükleyebilirsiniz:
> ```command
> sudo apt-get install python3-venv

### <a name="bash"></a>Bash

```bash
python -m venv .venv
source .venv/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell veya bir Windows komut istemi:

```powershell
py -m venv .venv
.venv\scripts\activate
```

Artık sanal ortamı etkinleştirdikten sonra, içinde kalan komutları çalıştırın. Sanal ortamdan yararlanmak için öğesini çalıştırın `deactivate`.

## <a name="create-a-local-functions-project"></a>Yerel işlevler projesi oluşturma

İşlevler projesi, Azure 'daki bir işlev uygulamasının eşdeğeridir. Hepsi aynı yerel ve barındırma yapılandırmalarının paylaştığı birden çok işleve sahip olabilir.

1. Sanal ortamda aşağıdaki komutu çalıştırın:

    ```console
    func init MyFunctionProj
    ```

1. Çalışan çalışma zamanı olarak **Python** ' u seçin.

    Komut bir _Myfunctionproj_ klasörü oluşturur. Bu üç dosyayı içerir:

    * *Local. Settings. JSON*: yerel olarak çalışırken uygulama ayarlarını ve bağlantı dizelerini depolamak için kullanılır. Bu dosya Azure 'da yayınlanmıyor.
    * *requirements. txt*: sistemin Azure 'a yayımlamaya yüklenecek paketlerin listesini içerir.
    * *Host. JSON*: bir işlev uygulamasındaki tüm işlevleri etkileyen genel yapılandırma seçeneklerini içerir. Bu dosya Azure 'da yayımlanır.

1. New *Myfunctionproj* klasörüne gidin:

    ```console
    cd MyFunctionProj
    ```

## <a name="create-a-function"></a>İşlev oluşturma

Yeni projeye bir işlev ekleyin.

1. Projenize bir işlev eklemek için aşağıdaki komutu çalıştırın:

    ```console
    func new
    ```

1. **Http tetikleyici** şablonunu seçmek için aşağı oklarınızı kullanın.

1. Bir işlev adı girmeniz istendiğinde, *Httptrigger* yazın ve ardından ENTER tuşuna basın.

Bu komutlar _Httptrigger_adında bir alt klasör oluşturur. Aşağıdaki dosyaları içerir:

* *function. JSON*: işlevi, tetikleyiciyi ve diğer bağlamaları tanımlayan yapılandırma dosyası. Bu dosyada, için `scriptFile` değeri işlevi içeren dosyayı işaret ettiğini `bindings` ve dizi, çağırma tetikleyicisini ve bağlamalarını tanımladığını unutmayın.

    Her bağlama bir yön, tür ve benzersiz bir ad gerektirir. HTTP tetikleyicisinin tür [`httpTrigger`](functions-bindings-http-webhook.md#trigger) ve çıkış bağlaması [`http`](functions-bindings-http-webhook.md#output)türünde bir giriş bağlaması vardır.

* *init.Kopyala\_: http ile tetiklenen işleviniz olan betik dosyası.\_ \_ \_* Bu betiğin varsayılan `main()`olduğuna dikkat edin. Tetikleyiciden gelen http verileri, `req` adlandırılmış adı `binding parameter`kullanılarak işleve geçer. Function. json ' da tanımlanan, [Azure. Functions. HttpRequest sınıfının](/python/api/azure-functions/azure.functions.httprequest)bir örneğidir. `req` 

    `$return` *Function. JSON*içinde olarak tanımlanan Return nesnesi, [Azure. Functions. HttpResponse sınıfının](/python/api/azure-functions/azure.functions.httpresponse)bir örneğidir. Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](functions-bindings-http-webhook.md).

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

İşlevi, Azure Işlevleri çalışma zamanını kullanarak yerel olarak çalışır.

1. Bu komut, işlev uygulamasını başlatır:

    ```console
    func host start
    ```

    Azure Işlevleri ana bilgisayarı başlatıldığında, aşağıdaki çıktıya benzer bir şey yazar. Buradan daha iyi okuyabilmeniz için bu atılır:

    ```output
    
                      %%%%%%
                     %%%%%%
                @   %%%%%%    @
              @@   %%%%%%      @@
           @@@    %%%%%%%%%%%    @@@
         @@      %%%%%%%%%%        @@
           @@         %%%%       @@
             @@      %%%       @@
               @@    %%      @@
                    %%
                    %
    
    ...
    
    Content root path: C:\functions\MyFunctionProj
    Now listening on: http://0.0.0.0:7071
    Application started. Press Ctrl+C to shut down.
    
    ...
    
    Http Functions:
    
            HttpTrigger: http://localhost:7071/api/HttpTrigger
    
    [8/27/2018 10:38:27 PM] Host started (29486ms)
    [8/27/2018 10:38:27 PM] Job host started
    ```

1. Çalışma zamanı çıktısından `HttpTrigger` işlevinizin URL’sini kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın.

1. `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. Aşağıdaki ekran görüntüsünde, yerel işlevin tarayıcıya döndürdüğü GET isteğine olan yanıt gösterilmektedir:

    ![Tarayıcıda yerel olarak doğrula](./media/functions-create-first-function-python/function-test-local-browser.png)

1. İşlev uygulamanızı kapatmak için CTRL + C ' yi seçin.

İşlevinizi yerel olarak çalıştırdığınıza göre, işlev uygulamasını ve gerekli diğer kaynakları Azure’da oluşturabilirsiniz.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Azure 'da bir işlev uygulaması oluşturma

İşlev uygulaması, işlev kodunuzun yürütülmesi için bir ortam sağlar. Bu sayede daha kolay yönetilmesi, dağıtım ve kaynakların paylaşımı için bir mantıksal birim olarak gruplandırmanıza işlevleri.

Aşağıdaki komutu çalıştırın. Benzersiz `<APP_NAME>` bir işlev uygulama adıyla değiştirin. Bir `<STORAGE_NAME>` depolama hesabı adıyla değiştirin. `<APP_NAME>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. Bu ad Azure'daki tüm uygulamalar arasında benzersiz olmalıdır.

> [!NOTE]
> Linux ve Windows uygulamalarını aynı kaynak grubunda barındıralamazsınız. Bir Windows işlev uygulaması veya Web uygulaması ile `myResourceGroup` adlandırılmış bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```

Yukarıdaki komut aynı kaynak grubundaki ilişkili bir Azure Application Insights örneğini de sağlar. Bu örneği, işlev uygulamanızı izlemek ve günlükleri görüntülemek için kullanabilirsiniz.

Artık yerel işlevler projenizi Azure 'daki işlev uygulamasına yayımlamaya hazırsınız.

## <a name="deploy-the-function-app-project-to-azure"></a>İşlev uygulaması projesini Azure'a dağıtma

Azure 'da işlev uygulaması oluşturduktan sonra, proje kodunuzu Azure 'a dağıtmak için [Func Azure functionapp Publish](functions-run-local.md#project-file-deployment) Core araçları komutunu kullanabilirsiniz. Bu örnekte, değerini uygulamanızın `<APP_NAME>` adıyla değiştirin.

```console
func azure functionapp publish <APP_NAME> --build remote
```

Seçeneği `--build remote` , Python projenizi dağıtım paketindeki dosyalardan Azure 'da uzaktan oluşturur. 

Aşağıdaki iletiye benzer bir çıktı görürsünüz. Buradan daha iyi okuyabilmeniz için bu atılır:

```output
Getting site publishing info...
...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
Functions in myfunctionapp:
    HttpTrigger - [httpTrigger]
        Invoke url: https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....
```

İçin değeri`Invoke url` kopyalayabilir veAzure'daişlevinizidoğrulamakiçinkullanabilirsiniz.`HttpTrigger` URL, işlev anahtarınız `code` olan bir sorgu dizesi değeri içerir, bu da başkalarının Azure 'da http tetikleyici uç noktanızı aramasını zorlaştırır.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

> [!NOTE]
> Yayımlanmış bir Python uygulaması için neredeyse gerçek zamanlı günlükleri görüntülemek için [Application Insights canlı ölçüm akışı](functions-monitoring.md#streaming-logs)kullanın.

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen bir işlev içeren bir Python işlevleri projesi oluşturdunuz, yerel makinenizde çalıştırın ve Azure 'a dağıttınız. Şimdi, işlevinizi genişletin...

> [!div class="nextstepaction"]
> [Azure depolama kuyruğu çıkış bağlaması ekleme](functions-add-output-binding-storage-queue-python.md)
