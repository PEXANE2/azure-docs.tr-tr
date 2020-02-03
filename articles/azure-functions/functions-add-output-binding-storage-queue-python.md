---
title: Python işleviniz için bir Azure depolama kuyruğu bağlama ekleme
description: Bir Azure depolama kuyruğunu bir çıkış bağlaması kullanarak bir Python işleviyle tümleştirin.
ms.date: 01/15/2020
ms.topic: quickstart
ms.openlocfilehash: 14a381d13da052fd67679ed17bbb6b6711f7a0e6
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76715375"
---
# <a name="add-an-azure-storage-queue-binding-to-your-python-function"></a>Python işleviniz için bir Azure depolama kuyruğu bağlama ekleme

Bu makalede, bir Azure depolama kuyruğunu, [http tarafından tetiklenen bir Python Işlevi oluşturma](functions-create-first-function-python.md)bölümünde oluşturduğunuz işlev ve depolama hesabıyla tümleştirmeniz gerekir. Bu tümleştirmeyi, HTTP isteğinden kuyruktaki bir iletiye veri yazan bir *Çıkış bağlaması* kullanarak elde edersiniz. Bu makalenin tamamlanması, önceki hızlı başlangıç için birkaç USD 'nin ötesinde ek maliyet yoktur.

## <a name="prerequisites"></a>Önkoşullar

- Hızlı başlangıcı doldurun, [http tarafından tetiklenen bir Python Işlevi oluşturun](functions-create-first-function-python.md). Bu makalenin sonunda kaynakları zaten temizlediğinizde, Işlevler uygulamasını Azure 'da yeniden oluşturmak için adımları tekrar deneyin, ancak kaynakları yerinde bırakın.

## <a name="retrieve-the-azure-storage-connection-string"></a>Azure depolama bağlantı dizesini alma

Önceki hızlı başlangıçta Azure 'da bir işlev uygulaması oluşturduğunuzda, bir depolama hesabı da oluşturmuş olursunuz. Bu hesabın bağlantı dizesi, Azure 'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu ayarı *Local. Settings. JSON* dosyasına indirerek bu bağlantıyı, işlevi yerel olarak çalıştırırken aynı hesaptaki bir depolama kuyruğuna yazma işlemi kullanabilirsiniz. 

1. Projenin kökünden, aşağıdaki komutu çalıştırarak `<app_name>` önceki hızlı başlangıçta işlev uygulamanızın adıyla değiştirin. Bu komut, dosyadaki varolan tüm değerlerin üzerine yazar.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. *Local. Settings. JSON* ' ı açın ve depolama hesabı bağlantı dizesi olan `AzureWebJobsStorage`adlı değeri bulun. Ad `AzureWebJobsStorage` ve bağlantı dizesini Bu makalenin diğer bölümlerinde kullanırsınız.

> [!IMPORTANT]
> *Local. Settings. JSON* , Azure 'dan indirilen gizli dizileri içerdiğinden, bu dosyayı her zaman kaynak denetiminden dışlayın. Yerel işlevler projesiyle oluşturulan *. gitignore* dosyası, varsayılan olarak dosyayı dışlar.

## <a name="add-an-output-binding-to-functionjson"></a>Function. JSON öğesine çıkış bağlaması ekleyin

Bir işlev yalnızca bir tetikleyicisine sahip olsa da, özel tümleştirme kodu yazmadan diğer Azure hizmetlerine ve kaynaklarına bağlanmanızı sağlayan birden çok giriş ve çıkış bağlaması olabilir. İşlev için kullanmakta olduğunuz dile uygun şekilde işlev klasörünüzdeki *function. JSON* dosyasında bu bağlamaları bildirirsiniz.

Önceki hızlı başlangıçta, *Httpexample* klasöründeki *function. JSON* dosyanız `bindings` koleksiyonunda iki bağlama içerir:

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
    }
  ]
}
```

Her bağlamanın en az bir türü, yönü ve adı vardır. Yukarıdaki örnekte, ilk bağlama Direction `in``httpTrigger` türüdür. `in` yönü için, `name` tetikleyici tarafından çağrıldığında işleve gönderilen giriş parametresinin adını belirtir.

İkinci bağlama yön `out``http` türüdür, bu durumda `$return` özel `name`, bu bağlamanın bir giriş parametresi sağlamak yerine işlevin dönüş değerini kullandığını gösterir.

Bu işlevden bir Azure depolama kuyruğuna yazmak için, aşağıdaki kodda gösterildiği gibi, `queue` türünde bir `out` bağlama `msg`adını ekleyin:

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

Bu durumda, `msg` işleve çıkış bağımsız değişkeni olarak verilir. `queue` bir tür için, `queueName` sıranın adını da belirtmeniz ve `connection`' deki Azure depolama bağlantısı 'nın ( *Local. Settings. JSON*' dan) *adını* sağlamanız gerekir.

Bağlamaların ayrıntıları hakkında daha fazla bilgi için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md) ve [sıra çıkış yapılandırması](functions-bindings-storage-queue.md#output---configuration).

## <a name="add-code-to-use-the-output-binding"></a>Çıkış bağlamayı kullanmak için kod ekleme

*Function. JSON*' da belirtilen sıra bağlaması ile, artık işlevinizi `msg` çıktı parametresini alacak şekilde güncelleştirebilir ve iletileri kuyruğa yazabilirsiniz.

*Httpexample\\\_\_init\_\_. Kopyala* ' yı aşağıdaki kodla eşleşecek şekilde güncelleştirin, `msg` parametresini işlev tanımına ve `msg.set(name)` `if name:` deyimin altına ekleyin.

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

`msg` parametresi [`azure.functions.InputStream class`](/python/api/azure-functions/azure.functions.httprequest)bir örneğidir. `set` yöntemi sıraya bir dize iletisi yazar, bu durumda adı URL sorgu dizesinde işleve geçirilir.

Kimlik doğrulaması, kuyruk başvurusu alma veya veri yazma için kod *yazmanıza gerek olmadığını* gözlemleyin. Tüm bu tümleştirme görevleri, Azure Işlevleri çalışma zamanı ve sıra çıkış bağlamasında kolayca işlenir.

## <a name="run-and-test-the-function-locally"></a>İşlevi yerel olarak çalıştırma ve test etme

1. Bir Terminal veya komut isteminde, *Localfunctionproj*işlev proje klasörünüze gidin.

1. Aşağıdaki komutu kullanarak Azure Işlevleri çalışma zamanı konağını başlatın.

    ```
    func host start
    ```

1. Başlangıç tamamlandıktan sonra `HttpExample` uç noktasının URL 'sini görürsünüz, URL 'sini bir tarayıcıya kopyalayın ve sorgu dizesi `?name=<your-name>`ekleyerek `http://localhost:7071/api/HttpExample?name=Guido`gibi tam URL 'yi yapın. Tarayıcı, önceki makalede olduğu gibi `Hello Guido` gibi bir ileti görüntülemelidir.

    `HttpExample` uç noktası görünmüyorsa, ana bilgisayarı **Ctrl**+**C** ile durdurun ve hata için çıktıyı denetleyin. Örneğin, *function. JSON*dosyasında bir hata varsa, konak bitiş noktasını etkinleştirmez. Ayrıca, *Httpexample* klasöründen değil, işlevler proje klasöründen `func host start` çalıştırıp çalıştırmadığından emin olun.

1. İşiniz bittiğinde, **Ctrl**+**C**ile Konağı durdurun.

> [!TIP]
> Başlangıç sırasında, ana bilgisayar [depolama bağlama uzantısını](functions-bindings-storage-blob.md#packages---functions-2x-and-higher) ve diğer Microsoft bağlama uzantılarını indirir ve yükler. Bağlama uzantıları *Host. JSON* dosyasında aşağıdaki özelliklerle varsayılan olarak etkinleştirildiğinden, bu yükleme gerçekleşir:
>
> ```json
> {
>     "version": "2.0",
>     "extensionBundle": {
>         "id": "Microsoft.Azure.Functions.ExtensionBundle",
>         "version": "[1.*, 2.0.0)"
>     }
> }
> ```
>
> Bağlama uzantıları ile ilgili herhangi bir hatayla karşılaşırsanız, yukarıdaki özelliklerin *Host. JSON*içinde bulunduğundan emin olun.

## <a name="view-the-message-in-the-azure-storage-queue"></a>Azure depolama sırasındaki iletiyi görüntüleme

İşleviniz Web tarayıcısı için bir HTTP yanıtı oluşturduğunda, kuyruk bağlamasında belirtilen `outqueue`adlı bir Azure depolama kuyruğuna ileti yazan `msg.set(name)`de çağırır. Kuyruğu [Azure Portal](../storage/queues/storage-quickstart-queues-portal.md) veya [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)görüntüleyebilirsiniz. Ayrıca, aşağıdaki adımlarda açıklandığı gibi kuyruğu Azure CLı 'de de görüntüleyebilirsiniz:

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
    
    ```bash
    az storage queue list --output tsv
    ```
    
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    az storage queue list --output tsv
    ```
    
    # <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
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
    
## <a name="redeploy-the-project-to-azure"></a>Projeyi Azure 'a yeniden dağıtın

Artık işlevi yerel olarak test ettiğiniz ve Azure depolama kuyruğuna bir ileti yazdığınıza yönelik olarak, Azure üzerinde çalışan uç noktayı güncelleştirmek için projenizi yeniden dağıtabilirsiniz.

1. *Localfunctionsproj* klasöründe, projeyi yeniden dağıtmak için [`func azure functionapp publish`](functions-run-local.md#project-file-deployment) komutunu kullanın ve`<app_name>` uygulamanızın adıyla değiştirin.

    ```
    func azure functionapp publish <app_name>
    ```
    
1. Önceki hızlı başlangıçta olduğu gibi, yeniden dağıtılan işlevi sınamak için bir tarayıcı veya KıVRıMLı kullanın.

    # <a name="browsertabbrowser"></a>[Tarayıcı](#tab/browser)
    
    Yayımla komutunun çıktısında gösterilen tüm **çağırma URL** 'sini bir tarayıcı adres çubuğuna kopyalayın, `&name=Azure`sorgu parametresini ekleyin. , İşlevi yerel olarak çalıştırdığınızda tarayıcı benzer bir çıktı görüntülemelidir.

    ![Azure üzerinde çalışan işlevin çıktısı bir tarayıcıda](./media/functions-create-first-function-python/function-test-cloud-browser.png)

    # <a name="curltabcurl"></a>[kıvr](#tab/curl)
    
    `&name=Azure`parametresini ekleyerek **Invoke URL 'si**ile [kıvrımlı](https://curl.haxx.se/) çalıştırın. Komutun çıktısı, "Merhaba Azure" metni olmalıdır.
    
    ![İşlevin çıktısı, Azure 'da kıvrımlı kullanarak çalıştırıldı](./media/functions-create-first-function-python/function-test-cloud-curl.png)

    --- 

1. Depolama sırasını, önceki bölümde açıklandığı gibi bir kez inceleyerek sıraya yazılan yeni iletiyi içerdiğini doğrulayın.

    Kuyruğu incelemek için Azure CLı kullanıyorsanız, `az storage message peek` komutu yalnızca kuyruktaki ilk iletiyi gösterir. İletileri işlemenin benzetimini yapmak için aynı bağımsız değişkenlerle birlikte `az storage message get` kullanın. `get` komutu iletiyi döndürür ve sıradan kaldırır. Daha sonra, sıra boşalıncaya kadar aynı komutu tekrarlayabilirsiniz (ve komut bir hata verir).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki adıma devam ederseniz, [Application Insights tümleştirmeyi etkinleştirin](functions-monitoring.md#manually-connect-an-app-insights-resource), daha önce yapmış olduğunuz yerde oluşturacağınız gibi tüm kaynaklarınızı saklayın.

Aksi takdirde, daha fazla maliyet ödemeden kaçınmak için kaynak grubunu ve içerdiği tüm kaynakları silmek için aşağıdaki komutu kullanın.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Işlevleri uygulamasıyla Application Insights tümleştirmesini etkinleştirme](functions-monitoring.md#manually-connect-an-app-insights-resource)

Diğer kaynaklar:

- [Python 'da tüm işlev projelerinin örnekleri](/samples/browse/?products=azure-functions&languages=python).
- [Azure Işlevleri Python Geliştirici Kılavuzu](functions-reference-python.md)
- [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md).
- [İşlevler fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/)
- [Tüketim planı maliyetlerini tahmin](functions-consumption-costs.md) etme makalesi.
