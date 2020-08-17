---
title: PowerShell kullanarak Azure Işlevlerinde ilk dayanıklı işlevinizi oluşturma
description: Visual Studio Code kullanarak bir Azure dayanıklı Işlevi oluşturun ve yayımlayın.
author: anthonychu
ms.topic: quickstart
ms.date: 08/10/2020
ms.reviewer: azfuncdf, antchu
ms.openlocfilehash: 11ae695a898e011fd287730d894394eff013a0ca
ms.sourcegitcommit: 2bab7c1cd1792ec389a488c6190e4d90f8ca503b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88273408"
---
# <a name="create-your-first-durable-function-in-powershell"></a>PowerShell 'de ilk dayanıklı işlevinizi oluşturma

*Dayanıklı işlevler* , bir sunucusuz ortamda durum bilgisi olan işlevler yazmanıza olanak sağlayan bir [Azure işlevleri](../functions-overview.md) uzantısıdır. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir.

Bu makalede, bir "Hello World" dayanıklı işlevini yerel olarak oluşturmak ve test etmek için Visual Studio Code Azure Işlevleri uzantısının nasıl kullanılacağını öğrenirsiniz.  Bu işlev, diğer işlevlere yönelik çağrıları düzenler ve birlikte zincirler. Ardından işlev kodunu Azure’da yayımlayacaksınız.

![Azure 'da dayanıklı işlevi çalıştırma](./media/quickstart-js-vscode/functions-vs-code-complete.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio Code](https://code.visualstudio.com/download)'i yükler.

* [Azure işlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) vs Code uzantısını yükler

* [Azure Functions Core Tools](../functions-run-local.md)en son sürümüne sahip olduğunuzdan emin olun.

* Dayanıklı İşlevler bir Azure depolama hesabı gerektirir. Bir Azure aboneliğine ihtiyacınız vardır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-your-local-project"></a><a name="create-an-azure-functions-project"></a>Yerel projenizi oluşturma 

Bu bölümde, yerel bir Azure Işlevleri projesi oluşturmak için Visual Studio Code kullanırsınız. 

1. Visual Studio Code, komut paletini açmak için F1 (veya Ctrl/Cmd + SHIFT + P) tuşuna basın. Komut paletinde, araması yapın ve seçin `Azure Functions: Create New Project...` .

    ![Create işlevi](media/quickstart-js-vscode/functions-create-project.png)

1. Projeniz için boş bir klasör konumu seçin ve **Seç**' i seçin.

1. İstemlerin ardından aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşlev uygulaması projeniz için bir dil seçin | PowerShell | Yerel bir PowerShell Işlevleri projesi oluşturun. |
    | Sürüm seçin | Azure İşlevleri v3 | Bu seçeneği yalnızca temel araçlar yüklü değilse görürsünüz. Bu durumda, uygulamayı ilk kez çalıştırdığınızda çekirdek araçlar yüklenir. |
    | Projenizin ilk işlevi için bir şablon seçin | Şimdilik atla | |
    | Projenizi nasıl açmak istediğinizi seçin | Geçerli pencerede aç | VS Code seçtiğiniz klasörde yeniden açılır. |

Gerekirse Azure Functions Core Tools Visual Studio Code yüklenir. Ayrıca, bir klasörde işlev uygulaması projesi oluşturur. Bu proje yapılandırma dosyalarında [host.js](../functions-host-json.md) ve [local.settings.js](../functions-run-local.md#local-settings-file) içerir.

Kök klasörde bir package.jsdosyası da oluşturulur.

### <a name="configure-function-app-to-use-powershell-7"></a>İşlev uygulamasını PowerShell 7 kullanacak şekilde yapılandırma

Dosyasında *local.settings.js* açın ve adlı bir ayarın `FUNCTIONS_WORKER_RUNTIME_VERSION` olarak ayarlandığını onaylayın `~7` . Eksik veya başka bir değere ayarlandıysa, dosyanın içeriğini güncelleştirin.

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "powershell",
    "FUNCTIONS_WORKER_RUNTIME_VERSION" : "~7"
  }
}
```

## <a name="create-your-functions"></a>İşlevlerinizi oluşturma

En temel Dayanıklı İşlevler uygulaması üç işlev içerir:

* *Orchestrator işlevi* -diğer işlevleri düzenleyen bir iş akışını açıklar.
* *Etkinlik işlevi* -Orchestrator işlevi tarafından çağırılır, iş gerçekleştirir ve isteğe bağlı olarak bir değer döndürür.
* *Client işlevi* -bir Orchestrator işlevi Başlatan normal bir Azure işlevi. Bu örnek, HTTP ile tetiklenen bir işlev kullanır.

### <a name="orchestrator-function"></a>Orchestrator işlevi

Projenizde dayanıklı işlev kodu oluşturmak için bir şablon kullanın.

1. Komut paletinde, araması yapın ve seçin `Azure Functions: Create Function...` .

1. İstemlerin ardından aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşleviniz için bir şablon seçin | Dayanıklı İşlevler Orchestrator (Önizleme) | Dayanıklı İşlevler düzenleme oluşturma |
    | Bir işlev adı girin | Merhaba Orchestrator | Dayanıklı işlevinizin adı |

Etkinlik işlevlerini koordine etmek için bir Orchestrator eklediniz. Orchestrator işlevini görmek için *helloorchestrator/run.ps1* açın. Cmdlet 'inin her çağrısı `Invoke-ActivityFunction` adlı bir etkinlik işlevini çağırır `Hello` .

Ardından, başvurulan `Hello` etkinlik işlevini eklersiniz.

### <a name="activity-function"></a>Activity işlevi

1. Komut paletinde, araması yapın ve seçin `Azure Functions: Create Function...` .

1. İstemlerin ardından aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşleviniz için bir şablon seçin | Dayanıklı İşlevler etkinliği (Önizleme) | Etkinlik işlevi oluşturma |
    | Bir işlev adı girin | Hello | Etkinlik işlevinizin adı |

`Hello`Orchestrator tarafından çağrılan etkinlik işlevini eklediniz. Giriş olarak bir ad ve bir tebrik döndürme olduğunu görmek için *Merhaba/run.ps1* açın. Bir etkinlik işlevi, veritabanı çağrısı yapma veya hesaplama gerçekleştirme gibi eylemleri gerçekleştirebileceğiniz yerdir.

Son olarak, Orchestration 'u Başlatan bir HTTP tetiklenen işlev eklersiniz.

### <a name="client-function-http-starter"></a>İstemci işlevi (HTTP başlangıç)

1. Komut paletinde, araması yapın ve seçin `Azure Functions: Create Function...` .

1. İstemlerin ardından aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşleviniz için bir şablon seçin | Dayanıklı İşlevler HTTP Starter (Önizleme) | HTTP başlangıç işlevi oluşturma |
    | Bir işlev adı girin | HttpStart | Etkinlik işlevinizin adı |
    | Yetkilendirme düzeyi | Anonim | Tanıtım amacıyla, işlevin kimlik doğrulaması olmadan çağrılmasına izin ver |

Orchestration Başlatan bir HTTP tetiklenen işlev eklediniz. Yeni bir düzenleme başlatmak için cmdlet 'ini kullandığını görmek için *Httpstart/run.ps1* öğesini açın `Start-NewOrchestration` . Ardından `New-OrchestrationCheckStatusResponse` cmdlet 'ini kullanarak yeni Orchestration izlemek ve yönetmek için kullanılabilecek URL 'leri içeren BIR http yanıtı döndürür.

Artık yerel olarak çalıştırılabilecek ve Azure 'a dağılabilecek bir Dayanıklı İşlevler uygulaması var.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio Code bir işlev uygulamasını ilk kez başlattığınızda bu araçları yüklemeniz istenir.

1. İşlevinizi test etmek için `Hello` etkinlik işlev kodunda (*merhaba/run.ps1*) bir kesme noktası ayarlayın. `Debug: Start Debugging`İşlev uygulaması projesini başlatmak Için F5 tuşuna basın veya komut paletinden seçim yapın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

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

1. Tarayıcınızı veya [Postman](https://www.getpostman.com/) veya [kıvrık](https://curl.haxx.se/)gıbı bir aracı kullanarak URL uç noktasına bir http post isteği gönderin. Son segmenti Orchestrator işlevinin adı () ile değiştirin `HelloOrchestrator` . URL şuna benzer olmalıdır `http://localhost:7071/api/orchestrators/HelloOrchestrator` .

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

## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Bu bölümde, Azure aboneliğinizde bir işlev uygulaması ve ilgili kaynaklar oluşturup kodunuzu dağıtırsınız. 

> [!IMPORTANT]
> Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır. 


1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında, **işlev uygulamasına dağıt uygulama...** düğmesini seçin.

    ![Projenizi Azure 'da yayımlayın](../../../includes/media/functions-publish-project-vscode/function-app-publish-project.png)

1. İstemlere aşağıdaki bilgileri sağlayın:

    + **Klasör seçin**: çalışma alanınızdan bir klasör seçin veya işlev uygulamanızı içeren bir klasöre gidin. Zaten açılmış geçerli bir işlev uygulamanız varsa bunu göremezsiniz.

    + **Abonelik seçin**: kullanılacak aboneliği seçin. Yalnızca bir aboneliğiniz varsa bunu göremezsiniz.

    + **Azure 'da işlev uygulaması seçin**: Seç `+ Create new Function App` . ( `Advanced` Bu makalede kapsanmayan seçeneğini seçmeyin.)
      
    + **İşlev uygulaması için genel olarak benzersiz bir ad girin**: URL yolunda geçerli olan bir ad yazın. Yazdığınız ad, Azure Işlevlerinde benzersiz olduğundan emin olmak için onaylanır. 

    + **Çalışma zamanı seçin**: yerel olarak çalıştırdığınız PowerShell sürümünü seçin. `pwsh -version`Sürümünüzü denetlemek için komutunu kullanabilirsiniz.

        > [!NOTE]
        > Azure Işlevleri VS Code uzantısı henüz PowerShell 7 ' yi desteklemiyor olabilir. PowerShell 7 bir seçenek olarak kullanılamıyorsa, şimdilik PowerShell 6. x ' i seçin ve işlev uygulaması oluşturulduktan sonra [sürümü el ile güncelleştirin](#update-function-app-ps7) .

    + **Yeni kaynaklar için bir konum seçin**: daha iyi performans için, size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. 
    
1.  Tamamlandığında, aşağıdaki Azure kaynakları, işlev uygulamanızın adına göre adlar kullanılarak aboneliğinizde oluşturulur:
    
    + İlgili kaynaklar için mantıksal kapsayıcı olan bir kaynak grubu.
    + Projeleriniz hakkında durum ve diğer bilgileri tutan standart bir Azure depolama hesabı.
    + Sunucusuz işlev uygulamanız için temel Konağı tanımlayan bir tüketim planı. 
    + İşlev kodunuzu yürütmek için ortam sağlayan bir işlev uygulaması. İşlev uygulaması, aynı barındırma planı içindeki kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.
    + Sunucusuz işlevinizin kullanımını izleyen işlev uygulamasına bağlı bir Application Insights örneği.

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır.

1. <a name="update-function-app-ps7"></a>İşlev uygulamasını oluştururken daha önce *PowerShell 7* ' yi seçmedik, komut paletini açmak için F1 tuşuna basın (veya Ctrl/Cmd + SHIFT + P). Komut paletinde, araması yapın ve seçin `Azure Functions: Upload Local Settings...` . Oluşturduğunuz işlev uygulamasını seçmek için istemleri izleyin. Mevcut ayarların üzerine yazılması istenirse, *Tümüne Hayır*' ı seçin.
    
1. Oluşturduğunuz Azure kaynakları dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **çıktıyı görüntüle** ' yi seçin. Bildirimi kaçırırsanız, sağ alt köşedeki zil simgesini seçerek tekrar görüntüleyin.

    ![Tüm bildirimi oluştur](../../../includes/media/functions-publish-project-vscode/function-create-notifications.png)


## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. **Çıktı** panelinden HTTP tetikleyicisinin URL’sini kopyalayın. HTTP ile tetiklenen işlevinizi çağıran URL şu biçimde olmalıdır: `http://<functionappname>.azurewebsites.net/api/orchestrators/HelloOrchestrator`

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Yayımlanan uygulamayı kullanmadan önce aynı durum yanıtını almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell dayanıklı işlev uygulaması oluşturmak ve yayımlamak için Visual Studio Code kullandınız.

> [!div class="nextstepaction"]
> [Ortak dayanıklı işlev desenleri hakkında bilgi edinin](durable-functions-overview.md#application-patterns)
