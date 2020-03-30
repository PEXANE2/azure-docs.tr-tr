---
title: JavaScript'i kullanarak Azure'da ilk dayanıklı işlevinizi oluşturun
description: Visual Studio Code'u kullanarak bir Azure Dayanıklı İşlev oluşturun ve yayımlayın.
author: anthonychu
ms.topic: quickstart
ms.date: 03/24/2020
ms.reviewer: azfuncdf, antchu
ms.openlocfilehash: 55098daa69d3e878140b20095b0a3e08811269e1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80257657"
---
# <a name="create-your-first-durable-function-in-javascript"></a>JavaScript'te ilk dayanıklı işlevinizi oluşturma

*Dayanıklı Işlevler,* sunucusuz bir ortamda durum lu işlevleri yazmanıza olanak tanıyan [Azure İşlevlerinin](../functions-overview.md) bir uzantısıdır. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir.

[!INCLUDE [v1-note](../../../includes/functions-durable-v1-tutorial-note.md)]

Bu makalede, Visual Studio Code Azure İşlevler uzantısını kullanarak yerel olarak "merhaba dünya" dayanıklı bir işlev oluşturmayı ve test etmeyi öğrenirsiniz.  Bu işlev, diğer işlevlere çağrıları birlikte düzenleyecek ve zincirleyecektir. Ardından işlev kodunu Azure’da yayımlayacaksınız.

![Azure'da dayanıklı işlevi çalıştırma](./media/quickstart-js-vscode/functions-vs-code-complete.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio Kodunu](https://code.visualstudio.com/download)Yükleyin.

* Azure [İşlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) VS Kodu uzantısını yükleme

* [Azure İşlevler Temel Araçları'nın](../functions-run-local.md)en son sürümüne sahip olduğundan emin olun.

* Dayanıklı Işlevler bir Azure depolama hesabı gerektirir. Azure aboneliğine ihtiyacınız var.

* Sürüm 10.x veya 12.x [Node.js](https://nodejs.org/) yüklü olduğundan emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-your-local-project"></a><a name="create-an-azure-functions-project"></a>Yerel projenizi oluşturun 

Bu bölümde, yerel bir Azure İşlevler projesi oluşturmak için Visual Studio Code'u kullanırsınız. 

1. Visual Studio Code'da komut paletini açmak için F1 (veya Ctrl/Cmd+Shift+P) tuşuna basın. Komut paletinde, 'yi `Azure Functions: Create New Project...`arayın ve seçin.

    ![İşlev oluşturma](media/quickstart-js-vscode/functions-create-project.png)

1. Projeniz için boş bir klasör konumu seçin ve **Seç'i**seçin.

1. İstemleri izleyerek, aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşlev uygulama projeniz için bir dil seçin | JavaScript | Yerel bir Düğüm.js Fonksiyonları projesi oluşturun. |
    | Sürüm seçin | Azure Fonksiyonları v3 | Bu seçeneği yalnızca Çekirdek Araçları zaten yüklenmemişken görürsünüz. Bu durumda, Uygulamayı ilk çalıştırdığınızda Çekirdek Araçlar yüklenir. |
    | Projenizin ilk işlevi için bir şablon seçin | Şimdilik atla | |
    | Projenizi nasıl açmak istediğinizi seçin | Geçerli pencerede aç | Seçtiğiniz klasörde VS Kodunu yeniden açar. |

Visual Studio Code gerekirse Azure İşlevler Temel Araçlarını yükler. Ayrıca bir klasörde bir işlev uygulaması projesi oluşturur. Bu proje [host.json](../functions-host-json.md) ve [local.settings.json](../functions-run-local.md#local-settings-file) yapılandırma dosyalarını içerir.

Kök klasöründe bir package.json dosyası da oluşturulur.

### <a name="enable-azure-functions-v2-compatibility-mode"></a>Azure İşlerini Etkinleştir V2 uyumluluk modunu etkinleştirme

Şu anda, JavaScript Dayanıklı İşlevler etkinleştirilmesi için Azure İşlevleri V2 uyumluluk modu gerektirir.

1. Uygulamayı yerel olarak çalıştırırken kullanılan ayarları yeniden yapmak için *local.settings.json'u* açın.

1. 'nin değeri `FUNCTIONS_V2_COMPATIBILITY_MODE` yle adında bir ayar ekle `true`

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

## <a name="install-the-durable-functions-npm-package"></a>Dayanıklı Fonksiyonlar npm paketini yükleyin

Bir Düğüm.js işlev uygulamasında Dayanıklı Fonksiyonlar ile çalışmak `durable-functions`için, bir kitaplık kullanırsınız.

1. VS Code'da yeni bir terminal açmak için *Görünüm* menüsünü veya Ctrl+Shift+'ı kullanın.

1. Işlev `durable-functions` uygulamasının kök `npm install durable-functions` dizininde çalıştırarak npm paketini yükleyin.

## <a name="creating-your-functions"></a>İşlevlerinizi oluşturma

En temel Dayanıklı Fonksiyonlar uygulaması üç işlev içerir:

* *Orchestrator işlevi* - diğer işlevleri düzenleyen bir iş akışını açıklar.
* *Etkinlik işlevi* - orkestratör işlevi tarafından çağrılan, işi gerçekleştirir ve isteğe bağlı olarak bir değer döndürür.
* *İstemci işlevi* - bir orkestratör işlevi başlatan normal bir Azure İşlevi. Bu örnekte bir HTTP tetiklenen işlev kullanır.

### <a name="orchestrator-function"></a>Orkestratör fonksiyonu

Projenizdeki dayanıklı işlev kodunu oluşturmak için bir şablon kullanırsınız.

1. Komut paletinde, 'yi `Azure Functions: Create Function...`arayın ve seçin.

1. İstemleri izleyerek, aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşleviniz için bir şablon seçin | Dayanıklı Fonksiyonlar orkestratör | Dayanıklı Fonksiyonlar düzenleme oluşturma |
    | Bir işlev adı sağlama | MerhabaOrchestrator | Dayanıklı işlevinizin adı |

Etkinlik işlevlerini koordine etmek için bir orkestratör eklediniz. Orkestratör işlevini görmek için *HelloOrchestrator/index.js'yi* açın. Her çağrı `context.df.callActivity` adlı `Hello`bir etkinlik işlevi çağırır.

Ardından, başvurulan `Hello` etkinlik işlevini eklersiniz.

### <a name="activity-function"></a>Etkinlik fonksiyonu

1. Komut paletinde, 'yi `Azure Functions: Create Function...`arayın ve seçin.

1. İstemleri izleyerek, aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşleviniz için bir şablon seçin | Dayanıklı Fonksiyonlar etkinliği | Etkinlik işlevi oluşturma |
    | Bir işlev adı sağlama | Hello | Etkinlik işlevinizin adı |

Orkestratör tarafından `Hello` çağrılan etkinlik işlevini eklediniz. *Hello/index.js'yi* açarak bir adı girdi olarak aldığını ve bir karşılamayı döndürdüğünü görün. Etkinlik işlevi, veritabanı araması yapma veya hesaplama yapma gibi eylemleri gerçekleştireceğiniz yerdir.

Son olarak, orkestrasyonu başlatan bir HTTP tetikleme işlevi eklersiniz.

### <a name="client-function-http-starter"></a>İstemci işlevi (HTTP starter)

1. Komut paletinde, 'yi `Azure Functions: Create Function...`arayın ve seçin.

1. İstemleri izleyerek, aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşleviniz için bir şablon seçin | Dayanıklı Fonksiyonlar HTTP marş | HTTP başlatıcı işlevi oluşturma |
    | Bir işlev adı sağlama | Dayanıklı FonksiyonlarhttpBaşlat | Etkinlik işlevinizin adı |
    | Yetkilendirme düzeyi | Anonim | Demo amacıyla, işlevin kimlik doğrulaması olmadan çağrılmasını bekleyin |

Bir orkestrasyon başlatan bir HTTP tetikleme işlevi eklediniz. Yeni bir orkestrasyon başlatmak için kullandığını `client.startNew` görmek için Dayanıklı *İşlevlerHttpStart/index.js'yi* açın. Daha sonra, yeni orkestrasyonu izlemek ve yönetmek için kullanılabilecek URL'ler içeren bir HTTP yanıtı döndürmek için kullanılır. `client.createCheckStatusResponse`

Artık yerel olarak çalıştırılabilen ve Azure'a dağıtılabilen dayanıklı işlevler uygulamasına sahipsiniz.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio Code'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.

1. İşlevinizi sınamak için etkinlik `Hello` işlev kodunda bir kesme noktası ayarlayın *(Hello/index.js).* İşlev uygulaması `Debug: Start Debugging` projesini başlatmak için F5 tuşuna basın veya komut paletinden seçin. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

    > [!NOTE]
    > Hata ayıklama hakkında daha fazla bilgi için [Dayanıklı İşlevler Tanılama](durable-functions-diagnostics.md#debugging) bölümüne bakın.

1. Dayanıklı İşlevler çalıştırmak için bir Azure Depolama hesabı gerektirir. VS Kodu bir depolama hesabı seçmenizi istediğinde, **depolama hesabı seçin'i**seçin.

    ![Depolama hesabı oluştur](media/quickstart-js-vscode/functions-select-storage.png)

1. İstemleri takiben, Azure'da yeni bir depolama hesabı oluşturmak için aşağıdaki bilgileri sağlayın.

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | Abonelik seçme | *aboneliğinizin adı* | Azure aboneliğinizi seçin |
    | Depolama hesabı seçin | Yeni depolama hesabı oluşturma |  |
    | Yeni depolama hesabının adını girin | *benzersiz isim* | Oluşturulacak depolama hesabının adı |
    | Kaynak grubu seçme | *benzersiz isim* | Oluşturulacak kaynak grubunun adı |
    | Bir konum seçin | *region* | Size yakın bir bölge seçin |

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](media/quickstart-js-vscode/functions-f5.png)

1. [Postacı](https://www.getpostman.com/) veya [cURL](https://curl.haxx.se/)gibi bir aracı kullanarak, URL bitiş noktasına bir HTTP POST isteği gönderin. Son bölümü orkestratör işlevinin adı ile`HelloOrchestrator`değiştirin ( ). URL' nin `http://localhost:7071/api/orchestrators/HelloOrchestrator`' e benzer olması gerekir.

   Yanıt, HTTP işlevinin, dayanıklı orkestrasyonun başarıyla başladığını bildiren ilk sonucudur. Henüz orkestrasyonun sonucu değil. Yanıt birkaç yararlı URL'ler içerir. Şimdilik, orkestrasyonun durumunu sorgulayalım.

1. URL değerini kopyalayın `statusQueryGetUri` ve tarayıcının adres çubuğuna yapıştırın ve isteği çalıştırın. Alternatif olarak, GET isteğini vermek için Postacı'yı kullanmaya devam edebilirsiniz.

   İstek, durum için düzenleme örneğini sorgular. Bize örneğin tamamlandığını gösteren ve dayanıklı işlevin çıktılarını veya sonuçlarını içeren nihai bir yanıt almalısınız. Bu gibi görünüyor: 

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

1. Hata ayıklamayı durdurmak için VS Kodu'nda **Shift + F5** tuşuna basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

[!INCLUDE [functions-create-function-app-vs-code](../../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../../includes/functions-publish-project-vscode.md)]

### <a name="enable-azure-functions-v2-compatibility-mode"></a>Azure İşlerini Etkinleştir V2 uyumluluk modunu etkinleştirme

Yerel olarak etkinleştirdiğiniz aynı Azure İşlevler V2 uyumluluğunun Azure'daki uygulamada etkinleştirilmesi gerekir.

1. Komut paletini kullanarak, arama `Azure Functions: Edit Setting...`yapın ve seçin.

1. Azure aboneliğinizde işlev uygulamanızı bulmak için istemleri izleyin.

1. `Create new App Setting...` öğesini seçin.

1. Yeni bir ayar `FUNCTIONS_V2_COMPATIBILITY_MODE`anahtarı girin.

1. Ayar değerini `true`girin.

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. **Çıktı** panelinden HTTP tetikleyicisinin URL’sini kopyalayın. HTTP tarafından tetiklenen işlevinizi çağıran URL bu biçimde olmalıdır:`http://<functionappname>.azurewebsites.net/orchestrators/HelloOrchestrator`

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Yayınlanan uygulamayı kullanırken öncekiyle aynı durum yanıtını almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

Bir JavaScript dayanıklı işlev uygulaması oluşturmak ve yayınlamak için Visual Studio Code'u kullandınız.

> [!div class="nextstepaction"]
> [Yaygın dayanıklı işlev desenleri hakkında bilgi edinin](durable-functions-overview.md#application-patterns)