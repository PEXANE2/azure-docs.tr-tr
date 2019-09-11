---
title: Azure 'da HTTP ile tetiklenen bir işlev oluşturma
description: Azure işlevleri çekirdek araçları ve Azure CLI kullanarak Azure'da ilk Python uygulamanızı oluşturma konusunda bilgi edinin.
author: ggailey777
ms.author: glenga
ms.date: 04/24/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: gwallace
ms.openlocfilehash: 28169bfb8dead65c543a3752a709f33487854e60
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70844738"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Azure 'da HTTP ile tetiklenen bir işlev oluşturma

Bu makalede, Azure Işlevleri 'nde çalışan bir Python projesi oluşturmak için komut satırı araçlarının nasıl kullanılacağı gösterilmektedir. Oluşturduğunuz işlev HTTP istekleri tarafından tetiklenir. Son olarak, projenizi Azure 'da [sunucusuz bir işlev](functions-scale.md#consumption-plan) olarak çalışacak şekilde yayımlayabilirsiniz.

Bu makale, Azure Işlevleri için iki hızlı başlangıçın birincsahiptir. Bu makaleyi tamamladıktan sonra, işlevize [bir Azure depolama kuyruğu çıkışı bağlaması eklersiniz](functions-add-output-binding-storage-queue-python.md) .

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilere sahip olmanız gerekir:

+ [Python 3.6. x](https://www.python.org/downloads/)'i yükler.

+ [Azure Functions Core Tools](./functions-run-local.md#v2) sürüm 2.7.1575 veya sonraki bir sürümü yükler.

+ [Azure CLI](/cli/azure/install-azure-cli) sürüm 2. x veya sonraki bir sürümünü yükler.

+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment-optional"></a>Sanal ortam oluşturma ve etkinleştirme (isteğe bağlı)

Python işlevlerini yerel olarak geliştirmek ve test etmek için Python 3,6 ortamının kullanılması önerilir. Oluşturma ve adlı bir sanal ortam etkinleştirmek için aşağıdaki komutları çalıştırın `.venv`. 

> [!NOTE]
> Python, Linux dağıtımına venv yüklemediyse, aşağıdaki komutu kullanarak yükleyebilirsiniz:
> ```command
> sudo apt-get install python3-venv
>

### <a name="bash"></a>Bash

```bash
python3.6 -m venv .venv
source .venv/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell veya bir Windows komut istemi:

```powershell
py -3.6 -m venv .venv
.venv\scripts\activate
```

Kalan komutlar sanal ortamın içinde çalıştırılır.

## <a name="create-a-local-functions-project"></a>Bir yerel işlevler projesi oluşturma

Işlevler projesi, Azure 'daki bir işlev uygulamasının eşdeğeridir. Hepsi aynı yerel ve barındırma yapılandırmalarının paylaştığı birden çok işleve sahip olabilir.

Sanal ortamda, çalışan çalışma zamanı olarak **Python** ' ı seçerek aşağıdaki komutu çalıştırın.

```console
func init MyFunctionProj
```

Aşağıdaki üç dosyayı içeren _Myfunctionproj_ adlı bir klasör oluşturulur:

* `local.settings.json`Yerel olarak çalışırken uygulama ayarlarını ve bağlantı dizelerini depolamak için kullanılır. Bu dosya Azure 'da yayınlanmıyor.
* `requirements.txt`Azure 'a yayımlarken yüklenecek paketlerin listesini içerir.
* `host.json`bir işlev uygulamasındaki tüm işlevleri etkileyen genel yapılandırma seçeneklerini içerir. Bu dosya Azure 'da yayımlanır.

Yeni MyFunctionProj klasörüne gidin:

```console
cd MyFunctionProj
```

## <a name="create-a-function"></a>İşlev oluşturma

Projenize bir işlev eklemek için aşağıdaki komutu çalıştırın:

```console
func new
```

**Http tetikleyici** şablonunu seçin, işlev için `HttpTrigger` ad olarak yazın ve ENTER tuşuna basın.

Aşağıdaki dosyaları içeren _Httptrigger_ adlı bir alt klasör oluşturulur:

* **function. JSON**: işlevi, tetikleyiciyi ve diğer bağlamaları tanımlayan yapılandırma dosyası. Bu dosyayı gözden geçirin ve değerini, işlev içeren `scriptFile` dosyaya işaret eden değeri, çağırma tetikleyicisi ve bağlamaları `bindings` dizide tanımlandığından görüntüleyin.

  Her bağlama bir yön, tür ve benzersiz bir ad gerektirir. HTTP tetikleyicisinin tür [`httpTrigger`](functions-bindings-http-webhook.md#trigger) ve çıkış bağlaması [`http`](functions-bindings-http-webhook.md#output)türünde bir giriş bağlaması vardır.

* **init.Kopyala\_: http ile tetiklenen işleviniz olan betik dosyası.\_ \_ \_** Bu betiği inceleyin ve varsayılan bir değer `main()`içerip içertiğine bakın. Tetikleyiciden gelen http verileri, `req` adlandırılmış bağlama parametresi kullanılarak bu işleve geçirilir. Function. JSON içinde tanımlanan, `req` [Azure. Functions. HttpRequest sınıfının](/python/api/azure-functions/azure.functions.httprequest)bir örneğidir. 

    Function. JSON içinde olarak `$return` tanımlanan Return nesnesi, [Azure. Functions. HttpResponse sınıfının](/python/api/azure-functions/azure.functions.httpresponse)bir örneğidir. Daha fazla bilgi için bkz. [Azure IŞLEVLERI http Tetikleyicileri ve bağlamaları](functions-bindings-http-webhook.md).

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Aşağıdaki komut, Azure 'daki aynı Azure Işlevleri çalışma zamanını kullanarak yerel olarak çalışan işlev uygulamasını başlatır.

```console
func host start
```

Işlevlerin ana bilgisayarı başlatıldığında, okunabilirlik için kesilmiş olan aşağıdaki çıktıya benzer bir şey yazar:

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

Çalışma zamanı çıktısından `HttpTrigger` işlevinizin URL’sini kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. Yerel işlevin döndürdüğü GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

![Tarayıcıda yerel olarak test etme](./media/functions-create-first-function-python/function-test-local-browser.png)

İşlevinizi yerel olarak çalıştırdığınıza göre, işlev uygulamasını ve gerekli diğer kaynakları Azure’da oluşturabilirsiniz.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Azure 'da bir işlev uygulaması oluşturma

İşlev uygulaması, işlev kodunuzun yürütülmesi için bir ortam sağlar. Bu sayede daha kolay yönetilmesi, dağıtım ve kaynakların paylaşımı için bir mantıksal birim olarak gruplandırmanıza işlevleri.

Bir yerine benzersiz işlev uygulamanızın adını kullanarak aşağıdaki komutu çalıştırarak `<APP_NAME>` yer tutucu ve depolama hesabı adı için `<STORAGE_NAME>`. `<APP_NAME>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. Bu ad Azure'daki tüm uygulamalar arasında benzersiz olmalıdır.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```
> [!NOTE]
> Linux ve Windows uygulamaları aynı kaynak grubunda barındırılamaz. Bir Windows işlev uygulaması veya Web uygulaması ile `myResourceGroup` adlandırılmış bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.

Bu komut Ayrıca, günlükleri izlemek ve görüntülemek için kullanılabilecek aynı kaynak grubunda ilişkili bir Azure Application Insights örneği sağlayacak.

Artık yerel işlevler projenizi Azure 'daki işlev uygulamasına yayımlamaya hazırsınız.

## <a name="deploy-the-function-app-project-to-azure"></a>İşlev uygulaması projesini Azure'a dağıtma

İşlev uygulaması Azure 'da oluşturulduktan sonra, proje kodunuzu Azure 'a dağıtmak için [`func azure functionapp publish`](functions-run-local.md#project-file-deployment) çekirdek Araçlar komutunu kullanabilirsiniz. Bu örneklerde, önceki adımda `<APP_NAME>` uygulamanızın adıyla değiştirin.

```command
func azure functionapp publish <APP_NAME> --build remote
```

Seçeneği `--build remote` , Python projenizi dağıtım paketindeki dosyalardan Azure 'da uzaktan oluşturur. 

Aşağıdakine benzer bir çıktı görürsünüz ve bu, okunabilirlik için kesildi:

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

Yalnızca Azure 'da işlevinizi `HttpTrigger`test etmek için kullanabileceğiniz değerinikopyalayın.`Invoke url` URL, işlev anahtarınız `code` olan bir sorgu dizesi değeri içeriyor. Bu anahtar başkalarının Azure 'da HTTP tetikleyici uç noktanızı aramasını zorlaştırır.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

> [!NOTE]
> Yayımlanmış bir Python uygulaması için neredeyse gerçek zamanlı günlükleri görüntülemek için Application Insights kullanmanızı öneririz [canlı ölçüm akışı](functions-monitoring.md#streaming-logs)

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen bir işlev içeren bir Python işlevleri projesi oluşturdunuz, yerel makinenizde çalıştırın ve Azure 'a dağıttınız. Şimdi, işlevinizi genişletin...

> [!div class="nextstepaction"]
> [Azure depolama kuyruğu çıkış bağlaması ekleme](functions-add-output-binding-storage-queue-python.md)
