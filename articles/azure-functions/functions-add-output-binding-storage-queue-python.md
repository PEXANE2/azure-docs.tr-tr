---
title: Python işleviniz için bir Azure depolama kuyruğu bağlama ekleme
description: Azure CLı ve Işlevleri temel araçlarını kullanarak Python işlevinizin Azure depolama kuyruğu çıkışı bağlamayı nasıl ekleyeceğinizi öğrenin.
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
ms.openlocfilehash: 34ec7c678410b2e0814f8dbb7a69257886cb891d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639130"
---
# <a name="add-an-azure-storage-queue-binding-to-your-python-function"></a>Python işleviniz için bir Azure depolama kuyruğu bağlama ekleme

Azure Işlevleri, kendi tümleştirme kodunuzu yazmak zorunda kalmadan Azure hizmetlerini ve diğer kaynakları işlevlere bağlamanıza olanak tanır. Hem giriş hem de çıktıyı temsil eden bu *bağlamalar*, işlev tanımı içinde bildirilmiştir. Bağlamalardan alınan veriler işleve parametre olarak sağlanır. *Tetikleyici* özel bir giriş bağlama türüdür. Bir işlevde yalnızca bir tetikleyici olsa da, birden çok giriş ve çıkış bağlaması olabilir. Daha fazla bilgi için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

Bu makalede, [önceki hızlı başlangıç makalesinde](functions-create-first-function-python.md) oluşturduğunuz Işlevi bir Azure depolama kuyruğu ile tümleştirme işlemi gösterilmektedir. Bu işleve eklediğiniz çıkış bağlaması, verileri bir HTTP isteğinden kuyruktaki bir iletiye yazar.

Çoğu bağlamanın, bağlı hizmete erişmek için kullandığı depolanan bir bağlantı dizesi gerekir. Bu bağlantıyı daha kolay hale getirmek için, işlev uygulamanızla oluşturduğunuz depolama hesabını kullanırsınız. Bu hesap bağlantısı zaten adlı `AzureWebJobsStorage`bir uygulama ayarında depolanıyor.  

## <a name="prerequisites"></a>Önkoşullar

Bu makaleye başlamadan önce, [Python hızlı](functions-create-first-function-python.md)başlangıcının 1. bölümündeki adımları uygulayın.

## <a name="download-the-function-app-settings"></a>İşlev uygulaması ayarlarını indirin

Önceki hızlı başlangıç makalesinde, Azure 'da gerekli depolama hesabıyla birlikte bir işlev uygulaması oluşturdunuz. Bu hesabın bağlantı dizesi, Azure 'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki bir depolama kuyruğuna iletiler yazarsınız. İşlevi yerel olarak çalıştırırken depolama hesabınıza bağlanmak için, uygulama ayarlarını yerel. Settings. JSON dosyasına indirmeniz gerekir. Ayarları yerel. Settings. json ' a indirmek için, önceki makaledeki işlev uygulamanızın adıyla `<APP_NAME>` değiştirerek aşağıdaki Azure Functions Core Tools komutunu çalıştırın:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Azure hesabınızda oturum açmanız gerekebilir.

> [!IMPORTANT]  
> Gizli dizi içerdiğinden, Local. Settings. JSON dosyası asla yayımlanmaz ve kaynak denetiminden dışlanmalıdır.

Depolama hesabı bağlantı dizesi `AzureWebJobsStorage`olan değere ihtiyacınız vardır. Çıkış bağlamasının beklendiği gibi çalıştığını doğrulamak için bu bağlantıyı kullanın.

## <a name="enable-extension-bundles"></a>Uzantı paketlerini etkinleştir

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Artık, depolama çıkış bağlamasını projenize ekleyebilirsiniz.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

İşlevlerde, her bağlama türü, function. json dosyasında `direction`bir, `type`a ve UNIQUE `name` tanımlanmasını gerektirir. Bağlama türüne bağlı olarak ek özellikler gerekli olabilir. [Kuyruk çıkış yapılandırması](functions-bindings-storage-queue.md#output---configuration) , bir Azure depolama kuyruğu bağlaması için gereken alanları açıklar.

Bir bağlama oluşturmak için, function. JSON dosyasına bir bağlama yapılandırma nesnesi eklersiniz. Bu özelliklere sahip `bindings` diziye bir nesne eklemek için httptrigger klasörünüzdeki function. json dosyasını düzenleyin:

| Özellik | Value | Açıklama |
| -------- | ----- | ----------- |
| **`name`** | `msg` | Kodunuzda başvurulan bağlama parametresini tanımlayan ad. |
| **`type`** | `queue` | Bağlama bir Azure depolama kuyruğu bağlamadır. |
| **`direction`** | `out` | Bağlama bir çıkış bağlamadır. |
| **`queueName`** | `outqueue` | Bağlamanın yazdığı kuyruğun adı. `queueName` Yoksa, bağlama ilk kullanımda onu oluşturur. |
| **`connection`** | `AzureWebJobsStorage` | Depolama hesabı için bağlantı dizesini içeren bir uygulama ayarının adı. `AzureWebJobsStorage` Ayar, işlev uygulamasıyla oluşturduğunuz depolama hesabı için bağlantı dizesini içerir. |

Function. JSON dosyanız Şu örneğe benzer şekilde görünmelidir:

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

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlaması kullanan kod ekleme

`name` Yapılandırıldıktan sonra, işlev imzasında bir yöntem özniteliği olarak bağlamaya erişmek için kullanmaya başlayabilirsiniz. Aşağıdaki örnekte `msg` , bir örneğidir [`azure.functions.InputStream class`](/python/api/azure-functions/azure.functions.httprequest).

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

Çıkış bağlamayı kullandığınızda, kimlik doğrulaması, kuyruk başvurusu alma veya veri yazma için Azure depolama SDK kodunu kullanmanız gerekmez. Işlevler çalışma zamanı ve kuyruk çıkış bağlaması bu görevleri sizin için işler.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Daha önce olduğu gibi, Işlevleri çalışma zamanını yerel olarak başlatmak için aşağıdaki komutu kullanın:

```bash
func host start
```

> [!NOTE]  
> Önceki hızlı başlangıçta ana bilgisayar. JSON içindeki uzantı paketlerinin etkinleştirilmesi nedeniyle, [depolama bağlama uzantısı](functions-bindings-storage-blob.md#packages---functions-2x) , başlangıç sırasında diğer Microsoft bağlama uzantılarıyla birlikte indirilip sizin için yüklendi.

Çalışma zamanı çıktısından `HttpTrigger` işlevinizin URL’sini kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. Sorgu dizesini `?name=<yourname>` bu URL 'ye ekleyin ve isteği çalıştırın. Önceki makalede yaptığınız gibi tarayıcıda aynı yanıtı görmeniz gerekir.

Bu kez, çıkış bağlaması aynı zamanda depolama hesabınızda adlı `outqueue` bir kuyruk oluşturur ve bu dizeyi içeren bir ileti ekler.

Ardından, yeni kuyruğu görüntülemek ve bir iletinin eklendiğini doğrulamak için Azure CLı 'yi kullanırsınız. Kuyruğunuzu, [Microsoft Azure Depolama Gezgini][Azure Storage Explorer] veya [Azure Portal](https://portal.azure.com)kullanarak da görüntüleyebilirsiniz.

### <a name="set-the-storage-account-connection"></a>Depolama hesabı bağlantısını ayarla

Local. Settings. json dosyasını açın ve depolama hesabı bağlantı dizesi olan `AzureWebJobsStorage`değerini kopyalayın. Aşağıdaki Bash komutunu kullanarak ortamdeğişkeninibağlantıdizesineayarlayın:`AZURE_STORAGE_CONNECTION_STRING`

```azurecli-interactive
export AZURE_STORAGE_CONNECTION_STRING=<STORAGE_CONNECTION_STRING>
```

`AZURE_STORAGE_CONNECTION_STRING` Ortam değişkeninde bağlantı dizesini ayarladığınızda, her seferinde kimlik doğrulaması sağlamak zorunda kalmadan depolama hesabınıza erişebilirsiniz.

### <a name="query-the-storage-queue"></a>Depolama kuyruğunu sorgulama

Aşağıdaki örnekte olduğu gibi [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) , hesabınızdaki depolama kuyruklarını görüntülemek için komutunu kullanabilirsiniz:

```azurecli-interactive
az storage queue list --output tsv
```

Bu komutun çıktısı, işlev çalıştırıldığında oluşturulan kuyruk olan `outqueue`adlı bir kuyruk içerir.

Sonra, bu kuyruktaki [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek) iletileri görüntülemek için komutunu kullanın:

```azurecli-interactive
echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
```

Döndürülen dize, işlevi test etmek için gönderdiğiniz iletiyle aynı olmalıdır.

> [!NOTE]  
> Önceki örnek, Base64 olan döndürülen dizenin kodunu çözer. Bunun nedeni, kuyruk depolama bağlamalarının Azure depolama 'ya [Base64 dizeleri](functions-bindings-storage-queue.md#encoding)olarak yazma ve okuma.

Şimdi güncelleştirilmiş işlev uygulamasını Azure 'da yeniden yayımlamanız zaman.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

Yine, dağıtılan işlevi test etmek için kıvrımlı veya tarayıcı kullanabilirsiniz. Daha önce olduğu gibi, aşağıdaki örnekte `&name=<yourname>` olduğu gibi sorgu dizesini URL 'ye ekleyin:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

Çıktı bağlamasının sırada yeni bir ileti üretdiğini doğrulamak için [depolama kuyruğu iletisini inceleyebilirsiniz](#query-the-storage-queue) .

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen işlevinizi bir depolama kuyruğuna veri yazmak için güncelleştirdiniz. Python ile Azure Işlevleri geliştirme hakkında daha fazla bilgi için bkz. [Azure Işlevleri Python Geliştirici Kılavuzu](functions-reference-python.md) ve [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md).

Sonra, işlev uygulamanız için Application Insights izlemeyi etkinleştirmelisiniz:

> [!div class="nextstepaction"]
> [Application Insights tümleştirmeyi etkinleştir](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Storage Explorer]: https://storageexplorer.com/