---
title: "Azure'da ilk dayanıklı işlevinizi C kullanarak oluşturun #"
description: Visual Studio veya Visual Studio Code'u kullanarak bir Azure Dayanıklı İşlev oluşturun ve yayımlayın.
author: jeffhollan
ms.topic: quickstart
ms.date: 03/18/2020
ms.author: azfuncdf
zone_pivot_groups: code-editors-set-one
ms.openlocfilehash: eda3afdf8deb3336cd0c5293c2422e694caa69c8
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80132731"
---
# <a name="create-your-first-durable-function-in-c"></a>C'de ilk dayanıklı işlevinizi oluşturun\#

*Dayanıklı Işlevler,* sunucusuz bir ortamda durum lu işlevleri yazmanıza olanak tanıyan [Azure İşlevlerinin](../functions-overview.md) bir uzantısıdır. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir.

::: zone pivot="code-editor-vscode"

Bu makalede, yerel bir "merhaba dünya" dayanıklı işlevi oluşturmak ve test etmek için Visual Studio Code nasıl kullanılacağını öğrenirler.  Bu işlev, diğer işlevlere çağrı ve zincirler birlikte çağırır. Ardından işlev kodunu Azure’da yayımlayacaksınız. Bu araçlar, VS Kodu [Azure İşlevleri uzantısının](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)bir parçası olarak kullanılabilir.

![Azure'da dayanıklı işlevi çalıştırma](./media/durable-functions-create-first-csharp/functions-vscode-complete.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio Kodunu](https://code.visualstudio.com/download)Yükleyin.

* Aşağıdaki VS Kodu uzantılarını yükleyin:
    - [Azure İşlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
    - [C #](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)

* [Azure İşlevler Temel Araçları'nın](../functions-run-local.md)en son sürümüne sahip olduğundan emin olun.

* Dayanıklı Işlevler bir Azure depolama hesabı gerektirir. Azure aboneliğine ihtiyacınız var.

* .NET [Core SDK'nın](https://dotnet.microsoft.com/download) sürüm 3.1 veya daha sonraki bir sürümü ne olduğundan emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-your-local-project"></a><a name="create-an-azure-functions-project"></a>Yerel projenizi oluşturun 

Bu bölümde, yerel bir Azure İşlevler projesi oluşturmak için Visual Studio Code'u kullanırsınız. 

1. Visual Studio Code'da komut paletini açmak için F1 (veya Ctrl/Cmd+Shift+P) tuşuna basın. Komut paletinde, 'yi `Azure Functions: Create New Project...`arayın ve seçin.

    ![İşlev projesi oluşturma](media/durable-functions-create-first-csharp/functions-vscode-create-project.png)

1. Projeniz için boş bir klasör konumu seçin ve **Seç'i**seçin.

1. İstemleri izleyerek, aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşlev uygulama projeniz için bir dil seçin | C# | Yerel bir C# İşlevleri projesi oluşturun. |
    | Sürüm seçin | Azure Fonksiyonları v3 | Bu seçeneği yalnızca Çekirdek Araçları zaten yüklenmemişken görürsünüz. Bu durumda, Uygulamayı ilk çalıştırdığınızda Çekirdek Araçlar yüklenir. |
    | Projenizin ilk işlevi için bir şablon seçin | Şimdilik atla | |
    | Projenizi nasıl açmak istediğinizi seçin | Geçerli pencerede aç | Seçtiğiniz klasörde VS Kodunu yeniden açar. |

Visual Studio Code gerekirse Azure İşlevler Temel Araçlarını yükler. Ayrıca bir klasörde bir işlev uygulaması projesi oluşturur. Bu proje [host.json](../functions-host-json.md) ve [local.settings.json](../functions-run-local.md#local-settings-file) yapılandırma dosyalarını içerir.

## <a name="add-functions-to-the-app"></a>Uygulamaya fonksiyon ekleme

Aşağıdaki adımlar, projenizdeki dayanıklı işlev kodunu oluşturmak için bir şablon kullanır.

1. Komut paletinde, 'yi `Azure Functions: Create Function...`arayın ve seçin.

1. İstemleri izleyerek, aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | İşleviniz için bir şablon seçin | Dayanıklı FonksiyonlarOrkestrasyon | Dayanıklı Fonksiyonlar düzenleme oluşturma |
    | Bir işlev adı sağlama | Merhaba Orkestrasyon | Işlevlerin oluşturulduğu sınıfın adı |
    | Ad alanı sağlama | Şirket.Fonksiyon | Oluşturulan sınıf için ad alanı |

1. VS Kodu bir depolama hesabı seçmenizi istediğinde, **depolama hesabı seçin'i**seçin. İstemleri takiben, Azure'da yeni bir depolama hesabı oluşturmak için aşağıdaki bilgileri sağlayın.

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | Abonelik seçme | *aboneliğinizin adı* | Azure aboneliğinizi seçin |
    | Depolama hesabı seçin | Yeni depolama hesabı oluşturma |  |
    | Yeni depolama hesabının adını girin | *benzersiz isim* | Oluşturulacak depolama hesabının adı |
    | Kaynak grubu seçme | *benzersiz isim* | Oluşturulacak kaynak grubunun adı |
    | Bir konum seçin | *region* | Size yakın bir bölge seçin |

Projeye yeni işlevleri içeren bir sınıf eklenir. VS Code ayrıca *local.settings.json'a* depolama hesabı bağlantı [`Microsoft.Azure.WebJobs.Extensions.DurableTask`](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) dizesini ve *.csproj* proje dosyasına NuGet paketine bir başvuru ekler.

İçeriği görüntülemek için yeni *HelloOrchestration.cs* dosyasını açın. Bu dayanıklı işlev aşağıdaki yöntemlerle basit bir fonksiyon zincirleme örneğidir:  

| Yöntem | Fonksiyon Adı | Açıklama |
| -----  | ------------ | ----------- |
| **`RunOrchestrator`** | `HelloOrchestration` | Dayanıklı orkestrasyonu yönetir. Bu durumda, orkestrasyon başlar, bir liste oluşturur ve listeye üç işlev çağrısının sonucunu ekler.  Üç işlev çağrısı tamamlandığında, listeyi döndürür. |
| **`SayHello`** | `HelloOrchestration_Hello` | Fonksiyon bir merhaba döndürür. Bu, düzenlenmektedir iş mantığı içeren işlevdir. |
| **`HttpStart`** | `HelloOrchestration_HttpStart` | Düzenlemenin bir örneğini başlatan ve bir denetim durumu yanıtını döndüren [HTTP tetiklenen](../functions-bindings-http-webhook.md) bir işlev. |

Artık işlev projenizi ve dayanıklı bir işlevi oluşturduğunuza göre, bunu yerel bilgisayarınızda sınayabilirsiniz.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio Code'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.

1. İşlevinizi sınamak için etkinlik `SayHello` işlev kodunda bir kesme noktası ayarlayın ve işlev uygulaması projesini başlatmak için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

    > [!NOTE]
    > Hata ayıklama hakkında daha fazla bilgi için [Dayanıklı İşlevler Tanılama](durable-functions-diagnostics.md#debugging) bölümüne bakın.

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](media/durable-functions-create-first-csharp/functions-vscode-f5.png)

1. [Postacı](https://www.getpostman.com/) veya [cURL](https://curl.haxx.se/)gibi bir aracı kullanarak, URL bitiş noktasına bir HTTP POST isteği gönderin.

   Yanıt, HTTP işlevinin bize dayanıklı orkestrasyonun başarıyla başladığını bildiren ilk sonucudur. Henüz orkestrasyonun sonucu değil. Yanıt birkaç yararlı URL'ler içerir. Şimdilik, orkestrasyonun durumunu sorgulayalım.

1. URL değerini kopyalayın `statusQueryGetUri` ve tarayıcının adres çubuğuna yapıştırın ve isteği çalıştırın. Alternatif olarak, GET isteğini vermek için Postacı'yı kullanmaya devam edebilirsiniz.

   İstek, durum için düzenleme örneğini sorgular. Bize örneğin tamamlandığını gösteren ve dayanıklı işlevin çıktılarını veya sonuçlarını içeren nihai bir yanıt almalısınız. Bu gibi görünüyor: 

    ```json
    {
        "name": "HelloOrchestration",
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

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. **Çıktı** panelinden HTTP tetikleyicisinin URL’sini kopyalayın. HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

        https://<functionappname>.azurewebsites.net/api/HelloOrchestration_HttpStart

1. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Yayınlanan uygulamayı kullanırken öncekiyle aynı durum yanıtını almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

C# dayanıklı işlev uygulaması oluşturmak ve yayınlamak için Visual Studio Code'u kullandınız.

> [!div class="nextstepaction"]
> [Yaygın dayanıklı işlev desenleri hakkında bilgi edinin](durable-functions-overview.md#application-patterns)

::: zone-end

::: zone pivot="code-editor-visualstudio"

Bu makalede Visual Studio 2019'un "merhaba dünya" dayanıklı bir işlev oluşturmayı ve test etmeyi öğreniyorsunuz.  Bu işlev, diğer işlevlere çağrı ve zincirler birlikte çağırır. Ardından işlev kodunu Azure’da yayımlayacaksınız. Bu araçlar Visual Studio 2019'da Azure geliştirme iş yükünün bir parçası olarak kullanılabilir.

![Azure'da dayanıklı işlevi çalıştırma](./media/durable-functions-create-first-csharp/functions-vs-complete.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/)’u yükleyin. **Azure geliştirme** iş yükünün de yüklü olduğundan emin olun. Visual Studio 2017 aynı zamanda Dayanıklı Fonksiyonlar geliştirmeyi de destekler, ancak ui ve adımlar farklıdır.

* Azure Depolama [Emülatörü'nün](../../storage/common/storage-use-emulator.md) yüklü ve çalışır durumda olduğunu doğrulayın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-function-app-project"></a>İşlev uygulaması projesi oluşturma

Azure İşlevler şablonu, Azure'daki bir işlev uygulamasında yayımlanabilen bir proje oluşturur. İşlev uygulaması, işlevleri daha kolay yönetim, dağıtım, ölçekleme ve kaynakların paylaşımı için mantıksal bir birim olarak gruplandırmanıza olanak tanır.

1. Visual Studio'da **Dosya** menüsünden **Yeni** > **Proje'yi** seçin.

1. Yeni **proje oluştur** iletişim kutusunda, `functions` **Azure İşlevleri** şablonunu arayın ve **İleri'yi**seçin. 

    ![Visual Studio'da işlev oluşturmaya yönelik yeni proje iletişim kutusu](./media/durable-functions-create-first-csharp/functions-vs-new-project.png)

1. Projeniz için bir **Proje adı** yazın ve **Tamam'ı**seçin. Proje adı C# ad alanı olarak geçerli olmalıdır, bu nedenle alt çizgi, tire veya diğer alfasayısal olmayan karakterleri kullanmayın.

1. **Yeni bir Azure İşlevler Uygulaması Oluştur'da,** görüntüyü izleyen tabloda belirtilen ayarları kullanın.

    ![Visual Studio'da yeni bir Azure İşlevleri Uygulaması iletişim kutusu oluşturma](./media/durable-functions-create-first-csharp/functions-vs-new-function.png)

    | Ayar      | Önerilen değer  | Açıklama                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Sürüm** | Azure Fonksiyonları 3.0 <br />(.NET Core) | .NET Core 3.1'i destekleyen Azure İşlevlerinin 3.0 sürümünü kullanan bir işlev projesi oluşturur. Daha fazla bilgi için bkz. [Azure İşlevleri çalışma zamanı sürümünün hedefini belirleme](../functions-versions.md).   |
    | **Şablon** | Boş | Boş bir işlev uygulaması oluşturur. |
    | **Depolama hesabı**  | Depolama Öykünücüsü | Dayanıklı işlev durumu yönetimi için bir depolama hesabı gereklidir. |

4. Boş bir işlev projesi oluşturmak için **Oluştur'u** seçin. Bu proje, işlevlerinizi çalıştırmak için gereken temel yapılandırma dosyalarına sahiptir.

## <a name="add-functions-to-the-app"></a>Uygulamaya fonksiyon ekleme

Aşağıdaki adımlar, projenizdeki dayanıklı işlev kodunu oluşturmak için bir şablon kullanır.

1. Visual Studio'da projeye sağ tıklayın ve**Yeni Azure İşi Ekle'yi** **Add** > seçin.

    ![Yeni işlev ekleme](./media/durable-functions-create-first-csharp/functions-vs-add-function.png)

1. Ekle menüsünden **Azure İşi'ni** doğrulayın, C# dosyanız için bir ad yazın ve sonra **Ekle'yi**seçin.

1. Dayanıklı **Fonksiyonlar Düzenleme** şablonu seçin ve ardından **Tamam'ı** seçin

    ![Dayanıklı şablonu seçin](./media/durable-functions-create-first-csharp/functions-vs-select-template.png)

Uygulamaya yeni bir dayanıklı işlev eklenir.  İçeriği görüntülemek için yeni .cs dosyasını açın. Bu dayanıklı işlev aşağıdaki yöntemlerle basit bir fonksiyon zincirleme örneğidir:  

| Yöntem | Fonksiyon Adı | Açıklama |
| -----  | ------------ | ----------- |
| **`RunOrchestrator`** | `<file-name>` | Dayanıklı orkestrasyonu yönetir. Bu durumda, orkestrasyon başlar, bir liste oluşturur ve listeye üç işlev çağrısının sonucunu ekler.  Üç işlev çağrısı tamamlandığında, listeyi döndürür. |
| **`SayHello`** | `<file-name>_Hello` | Fonksiyon bir merhaba döndürür. Bu, düzenlenmektedir iş mantığı içeren işlevdir. |
| **`HttpStart`** | `<file-name>_HttpStart` | Düzenlemenin bir örneğini başlatan ve bir denetim durumu yanıtını döndüren [HTTP tetiklenen](../functions-bindings-http-webhook.md) bir işlev. |

Artık işlev projenizi ve dayanıklı bir işlevi oluşturduğunuza göre, bunu yerel bilgisayarınızda sınayabilirsiniz.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.

1. İşlevinizi test etmek için F5’e basın. İstenirse Visual Studio'dan gelen Azure İşlevleri Temel (CLI) araçlarını indirme ve yükleme isteğini kabul edin. Aracın HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

2. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![Azure yerel çalışma zamanı](./media/durable-functions-create-first-csharp/functions-vs-debugging.png)

3. HTTP isteğinin URL'sini tarayıcınızın adres çubuğuna yapıştırın ve isteği çalıştırın. İşlevin döndürdüğü yerel GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

    ![Tarayıcıdaki işlev localhost yanıtı](./media/durable-functions-create-first-csharp/functions-vs-status.png)

    Yanıt, HTTP işlevinin bize dayanıklı orkestrasyonun başarıyla başladığını bildiren ilk sonucudur.  Henüz orkestrasyonun sonucu değil.  Yanıt birkaç yararlı URL'ler içerir.  Şimdilik, orkestrasyonun durumunu sorgulayalım.

4. URL değerini kopyalayın `statusQueryGetUri` ve tarayıcının adres çubuğuna yapıştırın ve isteği çalıştırın.

    İstek, durum için düzenleme örneğini sorgular. Aşağıdaki gibi görünen nihai bir yanıt almalısınız.  Bu çıktı bize örneğin tamamlandığını gösterir ve dayanıklı işlevin çıktılarını veya sonuçlarını içerir.

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
        "createdTime": "2019-11-02T07:07:40Z",
        "lastUpdatedTime": "2019-11-02T07:07:52Z"
    }
    ```

5. Hata ayıklamayı durdurmak için **Shift + F5** tuşuna basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Projenizi yayımlayabilmeniz için önce Azure aboneliğinizde bir işlev uygulamanızın olması gerekir. Visual Studio'dan bir işlev uygulaması oluşturabilirsiniz.

[!INCLUDE [Publish the project to Azure](../../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. Yayımlama profili sayfasından işlev uygulamasının temel URL'sini kopyalayın. İşlevi yerel olarak test ederken kullandığınız URL’nin `localhost:port` kısmını, yeni temel URL ile değiştirin.

    Dayanıklı işlevihttp tetikleyici çağıran URL aşağıdaki biçiminde olmalıdır:

        https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>_HttpStart

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Yayınlanan uygulamayı kullanırken öncekiyle aynı durum yanıtını almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

C# dayanıklı fonksiyon uygulaması oluşturmak ve yayınlamak için Visual Studio'yı kullandınız.

> [!div class="nextstepaction"]
> [Yaygın dayanıklı işlev desenleri hakkında bilgi edinin](durable-functions-overview.md#application-patterns)

::: zone-end
