---
title: JavaScript kullanarak Azure 'da ilk dayanıklı işlevinizi oluşturma
description: Visual Studio Code kullanarak bir Azure dayanıklı Işlevi oluşturun ve yayımlayın.
author: anthonychu
ms.topic: quickstart
ms.date: 03/24/2020
ms.reviewer: azfuncdf, antchu
ms.openlocfilehash: 55098daa69d3e878140b20095b0a3e08811269e1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80257657"
---
# <a name="create-your-first-durable-function-in-javascript"></a>JavaScript'te ilk dayanıklı işlevinizi oluşturma

*Dayanıklı işlevler* , bir sunucusuz ortamda durum bilgisi olan işlevler yazmanıza olanak sağlayan bir [Azure işlevleri](../functions-overview.md) uzantısıdır. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir.

[!INCLUDE [v1-note](../../../includes/functions-durable-v1-tutorial-note.md)]

Bu makalede, bir "Hello World" dayanıklı işlevini yerel olarak oluşturmak ve test etmek için Visual Studio Code Azure Işlevleri uzantısının nasıl kullanılacağını öğrenirsiniz.  Bu işlev, diğer işlevlere yönelik çağrıları düzenler ve birlikte zincirler. Ardından işlev kodunu Azure’da yayımlayacaksınız.

![Azure 'da dayanıklı işlevi çalıştırma](./media/quickstart-js-vscode/functions-vs-code-complete.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio Code](https://code.visualstudio.com/download)'i yükler.

* [Azure işlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) vs Code uzantısını yükler

* [Azure Functions Core Tools](../functions-run-local.md)en son sürümüne sahip olduğunuzdan emin olun.

* Dayanıklı İşlevler bir Azure depolama hesabı gerektirir. Bir Azure aboneliğine ihtiyacınız vardır.

* [Node. js](https://nodejs.org/) sürümünün 10. x veya 12. x sürümünün yüklü olduğundan emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-your-local-project"></a><a name="create-an-azure-functions-project"></a>Yerel projenizi oluşturma 

Bu bölümde, yerel bir Azure Işlevleri projesi oluşturmak için Visual Studio Code kullanırsınız. 

1. Visual Studio Code, komut paletini açmak için F1 (veya Ctrl/Cmd + SHIFT + P) tuşuna basın. Komut paletinde, araması yapın ve seçin `Azure Functions: Create New Project...`.

    ![Create işlevi](media/quickstart-js-vscode/functions-create-project.png)

1. Projeniz için boş bir klasör konumu seçin ve **Seç**' i seçin.

1. İstemlerin ardından aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşlev uygulaması projeniz için bir dil seçin | JavaScript | Yerel Node. js Işlevleri projesi oluşturun. |
    | Sürüm seçin | Azure Işlevleri v3 | Bu seçeneği yalnızca temel araçlar yüklü değilse görürsünüz. Bu durumda, uygulamayı ilk kez çalıştırdığınızda çekirdek araçlar yüklenir. |
    | Projenizin ilk işlevi için bir şablon seçin | Şimdilik atla | |
    | Projenizi nasıl açmak istediğinizi seçin | Geçerli pencerede aç | VS Code seçtiğiniz klasörde yeniden açılır. |

Gerekirse Azure Functions Core Tools Visual Studio Code yüklenir. Ayrıca, bir klasörde işlev uygulaması projesi oluşturur. Bu proje [Host. JSON](../functions-host-json.md) ve [Local. Settings. JSON](../functions-run-local.md#local-settings-file) yapılandırma dosyalarını içerir.

Kök klasörde bir Package. JSON dosyası da oluşturulur.

### <a name="enable-azure-functions-v2-compatibility-mode"></a>Azure Işlevleri v2 uyumluluk modunu etkinleştir

Şu anda JavaScript Dayanıklı İşlevler Azure Işlevleri v2 uyumluluk modunun etkinleştirilmesini gerektirir.

1. Uygulamayı yerel olarak çalıştırırken kullanılan ayarları düzenlemek için *Local. Settings. JSON* öğesini açın.

1. Değeri olan adlı `FUNCTIONS_V2_COMPATIBILITY_MODE` bir ayar ekleyin `true`.

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureWebJobsStorage": "",
            "FUNCTIONS_WORKER_RUNTIME": "node",
            "FUNCTIONS_V2_COMPATIBILITY_MODE": "true"
        }
    }
    ```

## <a name="install-the-durable-functions-npm-package"></a>Dayanıklı İşlevler NPM paketini yükler

Node. js işlev uygulamasındaki Dayanıklı İşlevler çalışmak için adlı `durable-functions`bir kitaplık kullanırsınız.

1. VS Code ' de yeni bir terminal açmak için *Görünüm* menüsünü veya CTRL + SHIFT + ' kullanın.

1. İşlev uygulamasının `durable-functions` kök dizininde çalıştırarak `npm install durable-functions` NPM paketini yükler.

## <a name="creating-your-functions"></a>İşlevleriniz oluşturma

En temel Dayanıklı İşlevler uygulaması üç işlev içerir:

* *Orchestrator işlevi* -diğer işlevleri düzenleyen bir iş akışını açıklar.
* *Etkinlik işlevi* -Orchestrator işlevi tarafından çağırılır, iş gerçekleştirir ve isteğe bağlı olarak bir değer döndürür.
* *Client işlevi* -bir Orchestrator işlevi Başlatan normal bir Azure işlevi. Bu örnek, HTTP ile tetiklenen bir işlev kullanır.

### <a name="orchestrator-function"></a>Orchestrator işlevi

Projenizde dayanıklı işlev kodu oluşturmak için bir şablon kullanın.

1. Komut paletinde, araması yapın ve seçin `Azure Functions: Create Function...`.

1. İstemlerin ardından aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşleviniz için bir şablon seçin | Dayanıklı İşlevler Orchestrator | Dayanıklı İşlevler düzenleme oluşturma |
    | Bir işlev adı girin | Merhaba Orchestrator | Dayanıklı işlevinizin adı |

Etkinlik işlevlerini koordine etmek için bir Orchestrator eklediniz. Orchestrator işlevini görmek için *helloorchestrator/index. js* ' i açın. Her çağrı, `context.df.callActivity` adlı `Hello`bir etkinlik işlevini çağırır.

Ardından, başvurulan `Hello` etkinlik işlevini eklersiniz.

### <a name="activity-function"></a>Activity işlevi

1. Komut paletinde, araması yapın ve seçin `Azure Functions: Create Function...`.

1. İstemlerin ardından aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşleviniz için bir şablon seçin | Dayanıklı İşlevler etkinliği | Etkinlik işlevi oluşturma |
    | Bir işlev adı girin | Hello | Etkinlik işlevinizin adı |

Orchestrator tarafından çağrılan `Hello` etkinlik işlevini eklediniz. Giriş olarak bir ad ve bir tebrik döndürme olduğunu görmek için *Merhaba/index. js* ' i açın. Bir etkinlik işlevi, veritabanı çağrısı yapma veya hesaplama gerçekleştirme gibi eylemleri gerçekleştirebileceğiniz yerdir.

Son olarak, Orchestration 'u Başlatan bir HTTP tetiklenen işlev eklersiniz.

### <a name="client-function-http-starter"></a>İstemci işlevi (HTTP başlangıç)

1. Komut paletinde, araması yapın ve seçin `Azure Functions: Create Function...`.

1. İstemlerin ardından aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşleviniz için bir şablon seçin | HTTP Starter Dayanıklı İşlevler | HTTP başlangıç işlevi oluşturma |
    | Bir işlev adı girin | DurableFunctionsHttpStart | Etkinlik işlevinizin adı |
    | Yetkilendirme düzeyi | Anonim | Tanıtım amacıyla, işlevin kimlik doğrulaması olmadan çağrılmasına izin ver |

Orchestration Başlatan bir HTTP tetiklenen işlev eklediniz. Yeni bir düzenleme başlatmak için kullandığını `client.startNew` görmek için *DurableFunctionsHttpStart/index. js* ' i açın. Ardından, yeni `client.createCheckStatusResponse` Orchestration izlemek ve yönetmek Için kullanılabilecek URL 'leri IÇEREN bir http yanıtı döndürmek için kullanır.

Artık yerel olarak çalıştırılabilecek ve Azure 'a dağılabilecek bir Dayanıklı İşlevler uygulaması var.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio Code'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.

1. İşlevinizi test etmek için `Hello` etkinlik işlev kodunda (*Hello/index. js*) bir kesme noktası ayarlayın. İşlev uygulaması projesini başlatmak `Debug: Start Debugging` için F5 tuşuna basın veya komut paletinden seçim yapın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

    > [!NOTE]
    > Hata ayıklama hakkında daha fazla bilgi için [dayanıklı işlevler tanılamayı](durable-functions-diagnostics.md#debugging) inceleyin.

1. Dayanıklı İşlevler, çalıştırmak için bir Azure depolama hesabı gerektirir. VS Code bir depolama hesabı seçmenizi isterse **depolama hesabı seç**' i seçin.

    ![Depolama hesabı oluştur](media/quickstart-js-vscode/functions-select-storage.png)

1. İstemlerin ardından, Azure 'da yeni bir depolama hesabı oluşturmak için aşağıdaki bilgileri sağlayın.

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | Abonelik seçme | *aboneliğinizin adı* | Azure aboneliğinizi seçin |
    | Bir depolama hesabı seçin | Yeni depolama hesabı oluşturma |  |
    | Yeni depolama hesabının adını girin | *benzersiz ad* | Oluşturulacak depolama hesabının adı |
    | Kaynak grubu seçme | *benzersiz ad* | Oluşturulacak kaynak grubunun adı |
    | Bir konum seçin | *geli* | Size yakın bir bölge seçin |

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](media/quickstart-js-vscode/functions-f5.png)

1. [Postman](https://www.getpostman.com/) veya [kıvrık](https://curl.haxx.se/)gibi BIR araç kullanarak, URL uç noktasına bir http post isteği gönderin. Son segmenti Orchestrator işlevinin adı (`HelloOrchestrator`) ile değiştirin. URL şuna benzer olmalıdır `http://localhost:7071/api/orchestrators/HelloOrchestrator`.

   Yanıt, HTTP işlevinin ilk sonucudur ve dayanıklı düzenleme 'nin başarıyla başlatıldığını bilmenizi sağlar. Orchestration 'un nihai sonucu henüz değildir. Yanıt birkaç yararlı URL içerir. Şimdilik düzenleme durumunu sorgulayalım.

1. URL değerini kopyalayın `statusQueryGetUri` ve tarayıcının adres çubuğuna yapıştırın ve isteği yürütün. Alternatif olarak, GET isteğini vermek için Postman kullanmaya de devam edebilirsiniz.

   İstek, bu durum için düzenleme örneğini sorgular. Örneğin, örneğin tamamlandığını gösteren ve dayanıklı işlevin çıkışlarını veya sonuçlarını içeren nihai bir yanıt almanız gerekir. Şöyle görünür: 

    ```json
    {
        "name": "HelloOrchestrator",
        "instanceId": "9a528a9e926f4b46b7d3deaa134b7e8a",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2020-03-18T21:54:49Z",
        "lastUpdatedTime": "2020-03-18T21:54:54Z"
    }
    ```

1. Hata ayıklamayı durdurmak için VS Code **Shift + F5** tuşlarına basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

[!INCLUDE [functions-create-function-app-vs-code](../../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../../includes/functions-publish-project-vscode.md)]

### <a name="enable-azure-functions-v2-compatibility-mode"></a>Azure Işlevleri v2 uyumluluk modunu etkinleştir

Yerel olarak etkinleştirdiğiniz Azure Işlevleri v2 uyumluluğu, Azure 'daki uygulamada etkinleştirilmelidir.

1. Komut paletini kullanarak araması yapın ve seçin `Azure Functions: Edit Setting...`.

1. Azure aboneliğinizde işlev uygulamanızı bulmak için istemleri izleyin.

1. `Create new App Setting...` öğesini seçin.

1. Yeni bir ayar anahtarı girin `FUNCTIONS_V2_COMPATIBILITY_MODE`.

1. Bir ayar değeri girin `true`.

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. **Çıktı** panelinden HTTP tetikleyicisinin URL’sini kopyalayın. HTTP ile tetiklenen işlevinizi çağıran URL şu biçimde olmalıdır:`http://<functionappname>.azurewebsites.net/orchestrators/HelloOrchestrator`

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Yayımlanan uygulamayı kullanmadan önce aynı durum yanıtını almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

JavaScript dayanıklı bir işlev uygulaması oluşturmak ve yayımlamak için Visual Studio Code kullandınız.

> [!div class="nextstepaction"]
> [Ortak dayanıklı işlev desenleri hakkında bilgi edinin](durable-functions-overview.md#application-patterns)