---
title: Azure 'da HTTP ile tetiklenen bir işlev oluşturma
description: Azure işlevleri çekirdek araçları ve Azure CLI kullanarak Azure'da ilk Python uygulamanızı oluşturma konusunda bilgi edinin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 04/24/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: 58f5cfd3718720cafc922bbd7b974a353e0d9d02
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68722791"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Azure 'da HTTP ile tetiklenen bir işlev oluşturma

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

Bu makalede, Azure Işlevleri 'nde çalışan bir Python projesi oluşturmak için komut satırı araçlarının nasıl kullanılacağı gösterilmektedir. Oluşturduğunuz işlev HTTP istekleri tarafından tetiklenir. Son olarak, projenizi Azure 'da [sunucusuz bir işlev](functions-scale.md#consumption-plan) olarak çalışacak şekilde yayımlayabilirsiniz.

Bu makale, Azure Işlevleri için iki hızlı başlangıçın birincsahiptir. Bu makaleyi tamamladıktan sonra, işlevize [bir Azure depolama kuyruğu çıkışı bağlaması eklersiniz](functions-add-output-binding-storage-queue-python.md) .

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilere sahip olmanız gerekir:

+ [Python 3,6](https://www.python.org/downloads/)' ü yükler.

+ [Azure Functions Core Tools](./functions-run-local.md#v2) sürüm 2.6.1071 veya sonraki bir sürümü yükler.

+ [Azure CLI](/cli/azure/install-azure-cli) sürüm 2. x veya sonraki bir sürümünü yükler.

+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Oluşturma ve bir sanal ortam etkinleştirin

Python işlevlerini yerel olarak geliştirmek ve test etmek için bir Python 3,6 ortamında çalışmanız gerekir. Oluşturma ve adlı bir sanal ortam etkinleştirmek için aşağıdaki komutları çalıştırın `.venv`.

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

Ardından, uzantı paketlerini etkinleştirmek için Host. json dosyasını güncelleştirin.  

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

```bash
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

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

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
> Azure Işlevleri, Linux için tüketim planı şu anda önizleme aşamasındadır ve yalnızca aşağıdaki bölgelerde kullanılabilir: Batı ABD, Doğu ABD, Batı Avrupa Doğu Asya. Ayrıca, Linux ve Windows uygulamaları aynı kaynak grubunda barındırılamaz. Bir Windows işlev uygulaması veya Web uygulaması ile `myResourceGroup` adlandırılmış bir kaynak grubunuz varsa, farklı bir kaynak grubu kullanmanız gerekir.

Artık yerel işlevler projenizi Azure 'daki işlev uygulamasına yayımlamaya hazırsınız.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen bir işlev içeren bir Python işlevleri projesi oluşturdunuz, yerel makinenizde çalıştırın ve Azure 'a dağıttınız. Şimdi, işlevinizi genişletin...

> [!div class="nextstepaction"]
> [Azure depolama kuyruğu çıkış bağlaması ekleme](functions-add-output-binding-storage-queue-python.md)
