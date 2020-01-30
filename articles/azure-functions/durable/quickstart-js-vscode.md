---
title: JavaScript kullanarak Azure 'da ilk dayanıklı işlevinizi oluşturma
description: Visual Studio Code kullanarak bir Azure dayanıklı Işlevi oluşturun ve yayımlayın.
author: ColbyTresness
ms.topic: quickstart
ms.date: 11/07/2018
ms.reviewer: azfuncdf, cotresne
ms.openlocfilehash: b0a1d1a9305f6de2a072ee1ded310d8de174436b
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76845714"
---
# <a name="create-your-first-durable-function-in-javascript"></a>JavaScript 'te ilk dayanıklı işlevinizi oluşturma

*Dayanıklı işlevler* , bir sunucusuz ortamda durum bilgisi olan işlevler yazmanıza olanak sağlayan bir [Azure işlevleri](../functions-overview.md) uzantısıdır. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir.

[!INCLUDE [v1-note](../../../includes/functions-durable-v1-tutorial-note.md)]

Bu makalede, bir "Hello World" dayanıklı işlevini yerel olarak oluşturmak ve test etmek için Visual Studio Code Azure Işlevleri uzantısının nasıl kullanılacağını öğrenirsiniz.  Bu işlev, diğer işlevlere yönelik çağrıları düzenler ve birlikte zincirler. Ardından işlev kodunu Azure’da yayımlayacaksınız.

![Azure 'da dayanıklı işlevi çalıştırma](./media/quickstart-js-vscode/functions-vs-code-complete.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio Code](https://code.visualstudio.com/download)'i yükler.

* [Azure Functions Core Tools](../functions-run-local.md)en son sürümüne sahip olduğunuzdan emin olun.

* Bir Windows bilgisayarında, [Azure Storage öykünücüsü](../../storage/common/storage-use-emulator.md) ' nin yüklü ve çalışır olduğunu doğrulayın. Bir Mac veya Linux bilgisayarda gerçek bir Azure depolama hesabı kullanmanız gerekir.

* Sürüm 8,0 veya daha sonraki bir [Node. js](https://nodejs.org/) sürümünün yüklü olduğundan emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../../includes/functions-install-vs-code-extension.md)]

## <a name="create-an-azure-functions-project"></a>Yerel projenizi oluşturma 

Bu bölümde, yerel bir Azure Işlevleri projesi oluşturmak için Visual Studio Code kullanırsınız. 

1. Visual Studio Code ' de, F1 tuşuna basarak komut paletini açın. Komut paletinde `Azure Functions: Create new project...`arayıp seçin.

1. Proje çalışma alanınız için bir dizin konumu seçin ve **Seç**' i seçin.

    > [!NOTE]
    > Bu adımlar, bir çalışma alanının dışında tamamlanacak şekilde tasarlandı. Bu örnekte, bir çalışma alanının parçası olan bir proje klasörünü seçmeyin.

1. İstemlerinizi izleyerek, istediğiniz dil için aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşlev uygulaması projeniz için bir dil seçin | JavaScript | Yerel Node. js Işlevleri projesi oluşturun. |
    | Sürüm seçin | Azure Işlevleri v2 | Bu seçeneği yalnızca temel araçlar yüklü değilse görürsünüz. Bu durumda, uygulamayı ilk kez çalıştırdığınızda çekirdek araçlar yüklenir. |
    | Projenizin ilk işlevi için bir şablon seçin | HTTP tetikleyicisi | Yeni işlev uygulamasında HTTP ile tetiklenen bir işlev oluşturun. |
    | Bir işlev adı girin | HttpTrigger | Varsayılan adı kullanmak için ENTER tuşuna basın. |
    | Yetkilendirme düzeyi | İşlev | `function` Yetkilendirme düzeyi, işlevinizin HTTP uç noktasını çağırırken bir erişim anahtarı vermenizi gerektirir. Bu, güvenli olmayan bir uç noktaya erişmeyi daha zorlaştırır. Daha fazla bilgi için bkz. [Yetkilendirme anahtarları](../functions-bindings-http-webhook.md#authorization-keys).  |
    | Projenizi nasıl açmak istediğinizi seçin | Çalışma alanına Ekle | Geçerli çalışma alanında işlev uygulaması oluşturur. |

Gerekirse Azure Functions Core Tools Visual Studio Code yüklenir. Ayrıca yeni bir çalışma alanında işlev uygulaması projesi oluşturur. Bu proje [Host. JSON](../functions-host-json.md) ve [Local. Settings. JSON](../functions-run-local.md#local-settings-file) yapılandırma dosyalarını içerir. Ayrıca, işlev kodunu içeren bir Node. js dosyası olan [function. JSON tanım dosyasını](../functions-reference-node.md#folder-structure) ve [index. js dosyasını](../functions-reference-node.md#exporting-a-function)içeren bir httpexample klasörü oluşturur.

Kök klasörde bir Package. JSON dosyası da oluşturulur.

## <a name="install-the-durable-functions-npm-package"></a>Dayanıklı İşlevler NPM paketini yükler

1. İşlev uygulamasının kök dizininde `npm install durable-functions` çalıştırarak NPM paketini `durable-functions`.

## <a name="creating-your-functions"></a>İşlevleriniz oluşturma

Artık Dayanıklı İşlevler kullanmaya başlamak için ihtiyacınız olan üç işlevi oluşturacağız: bir HTTP Starter, Orchestrator ve bir etkinlik işlevi. HTTP Starter tüm çözümünüzü başlatacak ve Orchestrator işleri çeşitli etkinlik işlevlerine gönderir.

### <a name="http-starter"></a>HTTP başlangıç

İlk olarak, dayanıklı bir işlev düzenlemesi Başlatan HTTP ile tetiklenen bir işlev oluşturun.

1. *Azure: işlevler*' de **işlev oluştur** simgesini seçin.

    ![İşlev oluşturma](./media/quickstart-js-vscode/create-function.png)

2. İşlev uygulaması projenizin bulunduğu klasörü seçin ve **DAYANıKLı işlevler http Starter** işlev şablonunu seçin.

    ![HTTP başlangıç şablonunu seçin](./media/quickstart-js-vscode/create-function-choose-template.png)

3. Olarak varsayılan adı bırakın `DurableFunctionsHttpStart` basın ** **Enter** ardından **anonim** kimlik doğrulaması.

    ![Anonim kimlik doğrulamasını seçin](./media/quickstart-js-vscode/create-function-anonymous-auth.png)

Şimdi dayanıklı Işlevimize bir giriş noktası oluşturduk. Şimdi bir Orchestrator ekleyelim.

### <a name="orchestrator"></a>Orchestrator

Şimdi, etkinlik işlevlerini koordine etmek için bir Orchestrator oluşturacağız.

1. *Azure: işlevler*' de **işlev oluştur** simgesini seçin.

    ![İşlev oluşturma](./media/quickstart-js-vscode/create-function.png)

2. İşlev uygulaması projenizin bulunduğu klasörü seçin ve **dayanıklı işlevler Orchestrator** işlev şablonunu seçin. Adı varsayılan "DurableFunctionsOrchestrator" olarak bırakın

    ![Orchestrator şablonunu seçin](./media/quickstart-js-vscode/create-function-choose-template.png)

Etkinlik işlevlerine koordine etmek için bir Orchestrator ekledik. Şimdi başvurulan etkinlik işlevini ekleyelim.

### <a name="activity"></a>Etkinlik

Şimdi, çözümün işini gerçekten yürütmek için bir etkinlik işlevi oluşturacağız.

1. *Azure: işlevler*' de **işlev oluştur** simgesini seçin.

    ![İşlev oluşturma](./media/quickstart-js-vscode/create-function.png)

2. İşlev uygulaması projenizin bulunduğu klasörü seçin ve **dayanıklı işlevler etkinlik** işlevi şablonunu seçin. Adı varsayılan "Merhaba" olarak bırakın.

    ![Etkinlik şablonunu seçin](./media/quickstart-js-vscode/create-function-choose-template.png)

Artık bir düzenleme ve zincir oluşturma etkinlik işlevlerini başlatmak için gereken tüm bileşenleri ekledik.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio Code'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.

1. Bir Windows bilgisayarında, Azure Storage öykünücüsü ' nü başlatın ve *Local. Settings. JSON* ' ın **AzureWebJobsStorage** özelliğinin `UseDevelopmentStorage=true`olarak ayarlandığından emin olun.

    Depolama öykünücüsü 5,8 için, Local. Settings. JSON **AzureWebJobsSecretStorageType** özelliğinin `files`olarak ayarlandığından emin olun. Mac veya Linux bilgisayarda, **AzureWebJobsStorage** özelliğini mevcut bir Azure depolama hesabının bağlantı dizesine ayarlamanız gerekir. Bu makalede daha sonra bir depolama hesabı oluşturursunuz.

2. İşlevinizi test etmek için işlev kodunda bir kesme noktası ayarlayın ve işlev uygulaması projesini başlatmak için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir. Dayanıklı İşlevler ilk kez kullanıyorsanız, Dayanıklı İşlevler uzantısı yüklenir ve derleme birkaç saniye sürebilir.

    > [!NOTE]
    > JavaScript Dayanıklı İşlevler, **Microsoft. Azure. WebJobs. Extensions. DurableTask** uzantısının sürüm **1.7.0** veya üstünü gerektirir. Dayanıklı İşlevler uzantısını yüklemek için Azure Işlevleri uygulamanızın kök klasöründen aşağıdaki komutu çalıştırın `func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.7.0`

3. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](../media/functions-create-first-function-vs-code/functions-vscode-f5.png)

4. `{functionName}` yerine `DurableFunctionsOrchestrator` yazın.

5. [Postman](https://www.getpostman.com/) veya [kıvrık](https://curl.haxx.se/)gibi BIR araç kullanarak, URL uç noktasına bir http post isteği gönderin.

   Yanıt, HTTP işlevinin ilk sonucudur ve dayanıklı düzenleme 'nin başarıyla başlatıldığını öğrenmemize olanak tanıyor. Orchestration 'un nihai sonucu henüz değildir. Yanıt birkaç yararlı URL içerir. Şimdilik düzenleme durumunu sorgulayalım.

6. `statusQueryGetUri` URL değerini kopyalayın ve tarayıcının adres çubuğuna yapıştırın ve isteği yürütün. Alternatif olarak, GET isteğini vermek için Postman kullanmaya de devam edebilirsiniz.

   İstek, bu durum için düzenleme örneğini sorgular. Örneğin, örneğin tamamlandığını gösteren ve dayanıklı işlevin çıkışlarını veya sonuçlarını içeren nihai bir yanıt almanız gerekir. Şöyle görünür: 

    ```json
    {
        "instanceId": "d495cb0ac10d4e13b22729c37e335190",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-11-08T07:07:40Z",
        "lastUpdatedTime": "2018-11-08T07:07:52Z"
    }
    ```

7. Hata ayıklamayı durdurmak için VS Code **Shift + F5** tuşlarına basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

[!INCLUDE [functions-create-function-app-vs-code](../../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../../includes/functions-publish-project-vscode.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. **Çıktı** panelinden HTTP tetikleyicisinin URL’sini kopyalayın. HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

        http://<functionappname>.azurewebsites.net/orchestrators/<functionname>

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Yayımlanan uygulamayı kullanmadan önce aynı durum yanıtını almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

JavaScript dayanıklı bir işlev uygulaması oluşturmak ve yayımlamak için Visual Studio Code kullandınız.

> [!div class="nextstepaction"]
> [Ortak dayanıklı işlev desenleri hakkında bilgi edinin](durable-functions-overview.md#application-patterns)