---
title: Visual Studio Code kullanarak işlevleri Azure depolama 'ya bağlama
description: Visual Studio Code kullanarak işlevlerinizi bir Azure depolama kuyruğuna bağlamak için çıkış bağlamayı nasıl ekleyeceğinizi öğrenin.
author: ggailey777
ms.author: glenga
ms.date: 06/25/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
manager: jeconnoc
ms.openlocfilehash: 63065c918a6f78510b4908c5e2ae80df67665b40
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2019
ms.locfileid: "71672611"
---
# <a name="connect-functions-to-azure-storage-using-visual-studio-code"></a>Visual Studio Code kullanarak işlevleri Azure depolama 'ya bağlama

Azure Işlevleri, kendi tümleştirme kodunuzu yazmak zorunda kalmadan işlevleri Azure hizmetlerine ve diğer kaynaklara bağlamanıza olanak tanır. Hem giriş hem de çıktıyı temsil eden bu *bağlamalar*, işlev tanımı içinde bildirilmiştir. Bağlamalardan alınan veriler işleve parametre olarak sağlanır. Tetikleyici özel bir giriş bağlama türüdür. Bir işlev yalnızca bir tetikleyicisine sahip olsa da, birden çok giriş ve çıkış bağlaması olabilir. Daha fazla bilgi için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

Bu makalede, [önceki hızlı başlangıç makalesinde](functions-create-first-function-vs-code.md) oluşturduğunuz Işlevi Azure Storage 'a bağlamak için Visual Studio Code nasıl kullanılacağı gösterilmektedir. Bu işleve eklediğiniz çıkış bağlaması, HTTP isteğinden verileri bir Azure kuyruk depolama kuyruğundaki bir iletiye yazar. 

Çoğu bağlamanın, bağlı hizmete erişmek için kullandığı depolanan bir bağlantı dizesi gerekir. Daha kolay hale getirmek için, işlev uygulamanız ile oluşturduğunuz depolama hesabını kullanırsınız. Bu hesap bağlantısı zaten `AzureWebJobsStorage` adlı bir uygulama ayarında depolanıyor.  

## <a name="prerequisites"></a>Önkoşullar

Bu makaleye başlamadan önce, aşağıdaki gereksinimleri karşılamanız gerekir:

* [Visual Studio Code Için Azure Storage uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage)yükler.
* [Azure Depolama Gezgini](https://storageexplorer.com/)'i yükler. Depolama Gezgini, çıkış bağlamalarınız tarafından oluşturulan sıra iletilerini incelemek için kullanacağınız bir araçtır. Depolama Gezgini, macOS, Windows ve Linux tabanlı işletim sistemlerinde desteklenir.
* [.NET Core CLI araçları](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x) 'nı (C# yalnızca projeler) yükler.
* [Visual Studio Code hızlı başlangıcı ' nın 1. bölümündeki](functions-create-first-function-vs-code.md)adımları uygulayın. 

Bu makalede, Visual Studio Code Azure aboneliğinizde zaten oturum açmış olduğunuz varsayılmaktadır. Komut paletinden `Azure: Sign In` çalıştırarak oturum açabilirsiniz. 

## <a name="download-the-function-app-settings"></a>İşlev uygulaması ayarlarını indirin

[Önceki hızlı başlangıç makalesinde](functions-create-first-function-vs-code.md), Azure 'Da gerekli depolama hesabıyla birlikte bir işlev uygulaması oluşturdunuz. Bu hesabın bağlantı dizesi, Azure 'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki bir depolama kuyruğuna iletiler yazarsınız. İşlevi yerel olarak çalıştırırken depolama hesabınıza bağlanmak için, uygulama ayarlarını yerel. Settings. JSON dosyasına indirmeniz gerekir. 

1. F1 tuşuna basarak komut paletini açın, sonra `Azure Functions: Download Remote Settings....` komutunu arayıp çalıştırın. 

1. Önceki makalede oluşturduğunuz işlev uygulamasını seçin. Var olan yerel ayarların üzerine yazmak için **Tümüne Evet** ' i seçin. 

    > [!IMPORTANT]  
    > Gizli dizi içerdiğinden, Local. Settings. JSON dosyası hiçbir şekilde yayımlanmaz ve kaynak denetiminden çıkarılır.

1. Depolama hesabı bağlantı dizesi değeri için anahtar olan `AzureWebJobsStorage` değerini kopyalayın. Çıkış bağlamasının beklendiği gibi çalıştığını doğrulamak için bu bağlantıyı kullanın.

## <a name="register-binding-extensions"></a>Bağlama uzantılarını kaydetme

Kuyruk depolama çıkış bağlaması kullandığınızdan, projeyi çalıştırmadan önce depolama bağlamaları uzantısının yüklü olması gerekir. 

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

HTTP ve Zamanlayıcı Tetikleyicileri dışında, bağlamalar uzantı paketleri olarak uygulanır. Depolama uzantısı paketini projenize eklemek için Terminal penceresinde aşağıdaki [DotNet paket Ekle](/dotnet/core/tools/dotnet-add-package) komutunu çalıştırın.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```
---
Şimdi, depolama çıkış bağlamasını projenize ekleyebilirsiniz.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

Işlevlerde, her bağlama türü, function. json dosyasında tanımlanacak bir `direction`, `type` ve benzersiz bir `name` gerektirir. Bu öznitelikleri tanımlama yöntemi, işlev uygulamanızın diline bağlıdır.

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

Bağlama öznitelikleri doğrudan function. json dosyasında tanımlanır. Bağlama türüne bağlı olarak ek özellikler gerekli olabilir. [Kuyruk çıkış yapılandırması](functions-bindings-storage-queue.md#output---configuration) , bir Azure depolama kuyruğu bağlaması için gereken alanları açıklar. Uzantı, function. JSON dosyasına bağlama eklemeyi kolaylaştırır. 

Bir bağlama oluşturmak için, HttpTrigger klasörünüzdeki `function.json` dosyasını sağ tıklatın (macOS üzerinde CTRL + tıklama) ve **bağlama Ekle...** öğesini seçin. Yeni bağlama yönelik aşağıdaki bağlama özelliklerini tanımlamak için istemleri izleyin:

| İstem | Value | Açıklama |
| -------- | ----- | ----------- |
| **Bağlama yönünü seçin** | `out` | Bağlama bir çıkış bağlamadır. |
| **Yön içeren bağlamayı seçin...** | `Azure Queue Storage` | Bağlama bir Azure depolama kuyruğu bağlamadır. |
| **Kodunuzda bu bağlamayı tanımlamak için kullanılan ad** | `msg` | Kodunuzda başvurulan bağlama parametresini tanımlayan ad. |
| **İletinin gönderileceği kuyruk** | `outqueue` | Bağlamanın yazdığı kuyruğun adı. *SıraAdı* mevcut olmadığında, bağlama ilk kullanımda oluşturulur. |
| **"Yerel. Setting. JSON" ayarını seçin** | `AzureWebJobsStorage` | Depolama hesabı için bağlantı dizesini içeren bir uygulama ayarının adı. @No__t-0 ayarı, işlev uygulamasıyla oluşturduğunuz depolama hesabı için bağlantı dizesini içerir. |

Function. JSON dosyanızdaki `bindings` dizisine bir bağlama eklenir ve bundan sonra aşağıdaki örnekte olduğu gibi görünmelidir:

```json
{
   ...

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

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]

---

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlaması kullanan kod ekleme

Bağlama tanımlandıktan sonra, işlev imzasında bir öznitelik olarak erişmek için bağlamanın `name` ' ı kullanabilirsiniz. Bir çıkış bağlaması kullanarak kimlik doğrulaması, kuyruk başvurusu alma veya veri yazma için Azure depolama SDK kodunu kullanmanız gerekmez. Işlevler çalışma zamanı ve kuyruk çıkış bağlaması bu görevleri sizin için işler.

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

Bir kuyruk iletisi oluşturmak için `context.bindings` üzerinde `msg` çıkış bağlama nesnesini kullanan kodu ekleyin. Bu kodu `context.res` deyiminden önce ekleyin.

```javascript
// Add a message to the Storage queue.
context.bindings.msg = "Name passed to the function: " + 
(req.query.name || req.body.name);
```

Bu noktada, işleviniz aşağıdaki gibi görünmelidir:

```javascript
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

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]

---

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Çıkış bağlaması ilk kullanıldığında, depolama hesabınızda Işlevler çalışma zamanına göre **outqueue** adlı yeni bir kuyruk oluşturulur. Depolama Gezgini, sıranın yeni iletiyle birlikte oluşturulduğunu doğrulamak için kullanacaksınız.

### <a name="connect-storage-explorer-to-your-account"></a>Depolama Gezgini’ni hesabınıza bağlama

Azure Depolama Gezgini zaten yüklediyseniz ve Azure hesabınıza bağladıysanız bu bölümü atlayın.

1. [Azure Depolama Gezgini] aracını çalıştırın, sol taraftaki Bağlan simgesini seçin ve **Hesap Ekle**' yi seçin.

    ![Microsoft Azure Depolama Gezgini bir Azure hesabı ekleyin](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-add-account.png)

1. **Bağlan** iletişim kutusunda, **Azure hesabı ekle**' yi seçin, **Azure ortamınızı**seçin ve oturum aç ' ı seçin. **..** 

    ![Azure hesabınızda oturum açma](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-connect-azure-account.png)

Hesabınızda başarıyla oturum açtıktan sonra hesabınızla ilişkili tüm Azure aboneliklerini görürsünüz.

### <a name="examine-the-output-queue"></a>Çıkış kuyruğunu inceleme

1. Visual Studio Code ' de, F1 tuşuna basarak komut paletini açın, sonra `Azure Storage: Open in Storage Explorer` komutunu arayıp çalıştırın ve depolama hesabınızın adını seçin. Depolama Hesabınız Azure Depolama Gezgini açılır.  

1. **Kuyruklar** düğümünü genişletin ve sonra **outqueue** adlı kuyruğu seçin. 

   Kuyruk, HTTP ile tetiklenen işlevi çalıştırdığınızda kuyruk çıkış bağlamasının oluşturduğu iletiyi içerir. İşlevi *Azure*'un varsayılan `name` değeriyle çağırdıysanız, kuyruk iletisi işleve *name olarak geçirilir: Azure @ no__t-0.

    ![Azure Depolama Gezgini gösterilen kuyruk iletisi](./media/functions-add-output-binding-storage-queue-vs-code/function-queue-storage-output-view-queue.png)

1. İşlevi yeniden çalıştırın, başka bir istek gönderin ve kuyrukta yeni bir ileti göründüğünü görürsünüz.  

Şimdi, güncelleştirilmiş işlev uygulamasını Azure 'da yeniden yayımlamanız zaman atalım.

## <a name="redeploy-and-verify-the-updated-app"></a>Güncelleştirilmiş uygulamayı yeniden dağıtın ve doğrulayın

1. Visual Studio Code ' de, F1 tuşuna basarak komut paletini açın. Komut paletinde, `Azure Functions: Deploy to function app...` ' ı arayıp seçin.

1. İlk makalede oluşturduğunuz işlev uygulamasını seçin. Projenizi aynı uygulamaya yeniden dağıtmakta olduğunuzdan, dosyaların üzerine yazma uyarısını kapatmak için **Dağıt** ' ı seçin.

1. Dağıtım tamamlandıktan sonra yeniden dağıtılan işlevi test etmek için kıvrımlı veya bir tarayıcı kullanabilirsiniz. Daha önce olduğu gibi, aşağıdaki örnekte olduğu gibi `&name=<yourname>` sorgu dizesini URL 'ye ekleyin:

    ```bash
    curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
    ```

1. Çıktı bağlamasının sırada yeni bir ileti oluşturmadığını doğrulamak için, [depolama sırasındaki iletiyi yeniden görüntüleyin](#examine-the-output-queue) .

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure’da *Kaynaklar*; işlev uygulamalarını, işlevleri, depolama hesaplarını ve benzeri öğeleri ifade eder. *Kaynak grupları* halinde gruplandırılırlar ve bir grubu silerek içindeki her şeyi silebilirsiniz.

Bu hızlı başlangıçları tamamlamak için kaynaklar oluşturdunuz. [Hesap durumunuza](https://azure.microsoft.com/account/) ve [hizmet fiyatlandırmanıza](https://azure.microsoft.com/pricing/) bağlı olarak size bu kaynakların ücretleri yansıtılabilir. Kaynaklara artık ihtiyacınız yoksa, şunları yaparak silebilirsiniz:

1. Visual Studio Code ' de, F1 tuşuna basarak komut paletini açın. Komut paletinde, `Azure Functions: Open in portal` ' ı arayıp seçin.

1. İşlev uygulamanızı seçin ve ENTER tuşuna basın. İşlev uygulaması sayfası [Azure Portal](https://portal.azure.com)açılır.

1. **Genel bakış** sekmesinde **kaynak grubu**altında adlandırılan bağlantıyı seçin.

    ![İşlev uygulaması sayfasından silinecek kaynak grubunu seçin.](./media/functions-add-output-binding-storage-queue-vs-code/functions-app-delete-resource-group.png)

1. **Kaynak grubu** sayfasında, dahil edilen kaynakların listesini gözden geçirin ve silmek istediğiniz kaynakların bunlar olduğunu doğrulayın.
 
1. **Kaynak grubunu sil**’i seçin ve yönergeleri izleyin.

   Silme işlemi birkaç dakika sürebilir. İşlem tamamlandığında, birkaç saniye boyunca bir bildirim görüntülenir. Bildirimi görüntülemek için sayfanın üst kısmındaki zil simgesini de seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen işlevinizi bir depolama kuyruğuna veri yazmak için güncelleştirdiniz. Işlevleri geliştirme hakkında daha fazla bilgi için bkz. [Visual Studio Code kullanarak Azure Işlevleri geliştirme](functions-develop-vs-code.md).

Sonra, işlev uygulamanız için Application Insights izlemeyi etkinleştirmelisiniz:

> [!div class="nextstepaction"]
> [Application Insights tümleştirmesini etkinleştirme](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Depolama Gezgini]: https://storageexplorer.com/
