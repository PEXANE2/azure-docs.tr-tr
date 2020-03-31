---
title: Visual Studio Code kullanarak Azure İşlevlerini Geliştirin
description: Visual Studio Code için Azure İşlevler uzantısını kullanarak Azure İşlevlerini nasıl geliştirip test edebilirsiniz öğrenin.
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: 2d33b7dddf29d37d174bdb7734e9048bc1658840
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277173"
---
# <a name="develop-azure-functions-by-using-visual-studio-code"></a>Visual Studio Code kullanarak Azure İşlevlerini Geliştirin

[Visual Studio Code için Azure İşlevler uzantısı,] işlevleri yerel olarak geliştirmenize ve Azure'a dağıtmanıza olanak tanır. Bu deneyim Azure İşlevleri ile ilk deneyiminizse, [Azure İşlevlerini tanıtma](functions-overview.md)bölümünden daha fazla bilgi edinebilirsiniz.

Azure İşlevler uzantısı aşağıdaki avantajları sağlar:

* Yerel geliştirme bilgisayarınızdaki işlevleri edin, oluşturun ve çalıştırın.
* Azure İşlevler projenizi doğrudan Azure'da yayınlayın.
* Visual Studio Code'un avantajlarından yararlanırken işlevlerinizi çeşitli dillerde yazın.

Uzantı, sürüm 2.x ile başlayan Azure İşlevleri çalışma zamanı tarafından desteklenen aşağıdaki dillerde kullanılabilir:

* [C# derlenmiş](functions-dotnet-class-library.md)
* [C# komut dosyası](functions-reference-csharp.md)<sup>*</sup>
* [Javascript](functions-reference-node.md)
* [Java](functions-reference-java.md)
* [Powershell](functions-reference-powershell.md)
* [Python](functions-reference-python.md)

<sup>*</sup>[C# komut dosyanızı varsayılan proje diliniz olarak ayarlamanızı](#c-script-projects)gerektirir.

Bu makalede, örnekler şu anda yalnızca JavaScript (Node.js) ve C# sınıfı kitaplık işlevleri için kullanılabilir.  

Bu makalede, işlevleri geliştirmek ve Azure'da yayımlamak için Azure İşlevler uzantısının nasıl kullanılacağı hakkında ayrıntılı bilgi verilmektedir. Bu makaleyi okumadan önce Visual [Studio Code'u kullanarak ilk işlevinizi oluşturmalısınız.](functions-create-first-function-vs-code.md)

> [!IMPORTANT]
> Tek bir işlev uygulaması için yerel geliştirme ve portal geliştirmeyi karıştırmayın. Yerel bir projeden bir işlev uygulamasına yayımladığınızda, dağıtım işlemi portalda geliştirdiğiniz işlevlerin üzerine yazar.

## <a name="prerequisites"></a>Ön koşullar

Visual Studio Code için [Azure İşlevleri uzantısıAzure][İşlevler uzantısını]yüklemeden ve çalıştırmadan önce aşağıdaki gereksinimleri karşılamanız gerekir:

* [Desteklenen](https://code.visualstudio.com/) [platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms)birine Görsel Stüdyo Kodu yüklenir.

* Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[Visual Studio Code'u kullanarak yayımladığınızda,](#publish-to-azure)azure depolama hesabı gibi gereksinim duyduğunuz diğer kaynaklar aboneliğinizde oluşturulur.

> [!IMPORTANT]
> Yerel olarak işlevler geliştirebilir ve yerel olarak başlatmak ve çalıştırmak zorunda kalmadan bunları Azure'da yayımlayabilirsiniz. İşlevlerinizi yerel olarak çalıştırmak için Azure İşlevler Temel Araçları'nın otomatik olarak indirilmesi de dahil olmak üzere bazı ek gereksinimleri karşılamanız gerekir. Daha fazla bilgi edinmek [için, projeyi yerel olarak çalıştırmak için ek gereksinimlere](#additional-requirements-for-running-a-project-locally)bakın.

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

## <a name="create-an-azure-functions-project"></a>Azure İşlevleri projesi oluşturma

İşlevler uzantısı, ilk işlevinizle birlikte bir işlev uygulaması projesi oluşturmanıza olanak tanır. Aşağıdaki adımlar, yeni bir Işlevler projesinde HTTP tarafından tetiklenen bir işlevin nasıl oluşturultuğu gösteriştir. [HTTP tetikleyici](functions-bindings-http-webhook.md) göstermek için en basit işlev tetikleme şablonudur.

1. **Azure'dan: Fonksiyonlar,** **İşlev Oluştur** simgesini seçin:

    ![İşlev oluşturma](./media/functions-develop-vs-code/create-function.png)

1. İşlev uygulaması projeniz için klasörü seçin ve ardından **işlev projeniz için bir dil seçin.**

1. Çekirdek Araçlarını zaten yüklemediyseniz, yüklemek için Çekirdek Araçları'nın **bir sürümünü seçmeniz** istenir. Sürüm 2.x veya daha sonraki bir sürümü seçin. 

1. HTTP **tetikleyici** işlev şablonunu seçin veya işlevsiz bir proje oluşturmak **için şimdilik Atla'yı** seçebilirsiniz. Projenize her zaman daha sonra [bir işlev ekleyebilirsiniz.](#add-a-function-to-your-project)

    ![HTTP tetikleyicisi şablonunu seçin](./media/functions-develop-vs-code/create-function-choose-template.png)

1. İşlev adı için **HttpExample** yazın ve Enter'u seçin ve ardından **İşlev** yetkilendirmesini seçin. Bu yetkilendirme düzeyi, işlev bitiş noktasını aradiğinizde bir [işlev anahtarı](functions-bindings-http-webhook-trigger.md#authorization-keys) sağlamanızı gerektirir.

    ![İşlev yetkilendirmesi seçin](./media/functions-develop-vs-code/create-function-auth.png)

    Seçtiğiniz dilde ve HTTP tetiklenen bir işlev için şablonda bir işlev oluşturulur.

    ![Visual Studio Code'da HTTP tetiklenen işlev şablonu](./media/functions-develop-vs-code/new-function-full.png)

### <a name="generated-project-files"></a>Oluşturulan proje dosyaları

Proje şablonu seçtiğiniz dilde bir proje oluşturur ve gerekli bağımlılıkları yükler. Herhangi bir dil için, yeni proje şu dosyalara sahiptir:

* **host.json**: İşlevler ana bilgisayarını yapılandırmanızı sağlar. Bu ayarlar, işlevleri yerel olarak çalıştırırken ve Bunları Azure'da çalıştırırken geçerlidir. Daha fazla bilgi için [host.json referansına](functions-host-json.md)bakın.

* **local.settings.json**: Yerel olarak işlevleri çalıştırırken kullanılan ayarları korur. Bu ayarlar yalnızca işlevleri yerel olarak çalıştırırken kullanılır. Daha fazla bilgi için [Yerel ayarlar dosyasına](#local-settings-file)bakın.

    >[!IMPORTANT]
    >Local.settings.json dosyası sırlar içerebildiği için, dosyayı proje kaynak denetiminizden çıkarmanız gerekir.

Dilinize bağlı olarak, bu diğer dosyalar oluşturulur:

# <a name="c"></a>[C\#](#tab/csharp)

* İşlevin uygulanmasını [HttpExample.cs sınıf kitaplığı dosyası.](functions-dotnet-class-library.md#functions-class-library-project)

# <a name="javascript"></a>[Javascript](#tab/nodejs)

* Kök klasöründe bir package.json dosyası.

* [İşlev.json tanım dosyası](functions-reference-node.md#folder-structure) ve [dizin.js dosyasını](functions-reference-node.md#exporting-a-function)içeren bir HttpExample klasörü, işlev kodunu içeren bir Düğüm.js dosyası.

<!-- # [PowerShell](#tab/powershell)

* An HttpExample folder that contains the [function.json definition file](functions-reference-python.md#programming-model) and the run.ps1 file, which contains the function code.
 
# [Python](#tab/python)
    
* A project-level requirements.txt file that lists packages required by Functions.
    
* An HttpExample folder that contains the [function.json definition file](functions-reference-python.md#programming-model) and the \_\_init\_\_.py file, which contains the function code.
     -->
---

Bu noktada, [function.json dosyasını değiştirerek](#add-a-function-to-your-project) veya [C# sınıf kitaplık işlevine bir parametre ekleyerek](#add-a-function-to-your-project)işlevinize giriş ve çıkış bağlamaları ekleyebilirsiniz.

Ayrıca [projenize yeni bir işlev ekleyebilirsiniz.](#add-a-function-to-your-project)

## <a name="install-binding-extensions"></a>Bağlama uzantılarını yükleme

HTTP ve zamanlayıcı tetikleyicileri dışında, bağlamalar uzatma paketlerinde uygulanır. Tetikleyiciler ve bağlamalar için uzantı paketlerini yüklemeniz gerekir. Bağlama uzantılarını yükleme işlemi projenizin diline bağlıdır.

# <a name="c"></a>[C\#](#tab/csharp)

Projenizde ihtiyacınız olan uzantı paketlerini yüklemek için Terminal penceresinde [dotnet ekle paket](/dotnet/core/tools/dotnet-add-package) komutunu çalıştırın. Aşağıdaki komut, Blob, Queue ve Tablo depolama için bağlayıcılar uygulayan Azure Depolama uzantısını yükler.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

# <a name="javascript"></a>[Javascript](#tab/nodejs)

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

---

## <a name="add-a-function-to-your-project"></a>Projenize bir işlev ekleme

Önceden tanımlanmış İşlevler tetikleyici şablonlarından birini kullanarak varolan bir projeye yeni bir işlev ekleyebilirsiniz. Yeni bir işlev tetikleyicisi eklemek için komut paletini açmak için F1'i seçin ve ardından **Azure İşlevleri: İşlev Oluştur**komutunu arayıp çalıştırın. Tetikleyici türünü seçmek ve tetikleyicinin gerekli özniteliklerini tanımlamak için istemleri izleyin. Tetikleyiciniz bir hizmete bağlanmak için bir erişim anahtarı veya bağlantı dizesi gerektiriyorsa, işlev tetikleyicisini oluşturmadan önce hazırlayın.

Bu eylemin sonuçları projenizin diline bağlıdır:

# <a name="c"></a>[C\#](#tab/csharp)

Projenize yeni bir C# sınıfı kitaplığı (.cs) dosyası eklenir.

# <a name="javascript"></a>[Javascript](#tab/nodejs)

Projede yeni bir klasör oluşturulur. Klasör, yeni bir function.json dosyası ve yeni JavaScript kod dosyası içerir.

---

## <a name="add-input-and-output-bindings"></a>Giriş ve çıktı bağlamaları ekleme

Giriş ve çıktı bağlamaları ekleyerek işlevinizi genişletebilirsiniz. Bağlama ekleme işlemi projenizin diline bağlıdır. Bağlamalar hakkında daha fazla bilgi edinmek için [Azure İşlevlerini tetikleyen ve bağlama kavramlarına](functions-triggers-bindings.md)bakın.

Aşağıdaki örnekler, local.settings.json'daki `outqueue` `MyStorageConnection` uygulama ayarında depolama hesabının bağlantı dizesinin ayarlandığı , adlı depolama kuyruğuna bağlanır.

# <a name="c"></a>[C\#](#tab/csharp)

Yöntem tanımına aşağıdaki parametreyi eklemek `Run` için işlev yöntemini güncelleştirin:

```cs
[Queue("outqueue"),StorageAccount("MyStorageConnection")] ICollector<string> msg
```

Bu kod, aşağıdaki `using` deyimi eklemenizi gerektirir:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

`msg` Parametre, işlev `ICollector<T>` tamamlandığında çıktı bağlamasına yazılan iletiler koleksiyonunu temsil eden bir türdür. Koleksiyona bir veya daha fazla ileti eklersiniz. Bu iletiler işlev tamamlandığında kuyruğa gönderilir.

Daha fazla bilgi edinmek için [Sıra depolama çıktısına bağlama](functions-bindings-storage-queue-output.md) belgelerine bakın.

# <a name="javascript"></a>[Javascript](#tab/nodejs)

Visual Studio Code, uygun bir istem kümesini izleyerek function.json dosyanıza bağlama eklemenizi sağlar. Bağlama oluşturmak için, işlev klasörünüzdeki **function.json** dosyasına sağ tıklayın (macOS'a Ctrl+tıklayın) ve **Bağlayıcı ekle'yi**seçin:

![Varolan bir JavaScript işlevine bağlama ekleme ](media/functions-develop-vs-code/function-add-binding.png)

Yeni bir depolama çıktısı bağlama tanımlamak için örnek istemleri şunlardır:

| İstem | Değer | Açıklama |
| -------- | ----- | ----------- |
| **Bağlama yönünü seçin** | `out` | Bağlama bir çıktı bağlayıcısidır. |
| **Yön ile bağlamayı seçin** | `Azure Queue Storage` | Bağlama, Azure Depolama sırasına bağlamadır. |
| **Kodunuzda bu bağlamayı tanımlamak için kullanılan ad** | `msg` | Kodunuzda başvurulan bağlama parametresini tanımlayan ad. |
| **İletinin gönderilecek sıra** | `outqueue` | Bağlamanın yazdığı sıranın adı. *QueueName* yoksa, bağlama ilk kullanımda oluşturur. |
| **"local.settings.json" adresinden ayarı seçin** | `MyStorageConnection` | Depolama hesabının bağlantı dizesini içeren bir uygulama ayarının adı. Ayar, `AzureWebJobsStorage` işlev uygulamasıyla oluşturduğunuz depolama hesabının bağlantı dizesini içerir. |

Bu örnekte, function.json `bindings` dosyanızdaki diziye aşağıdaki bağlama eklenir:

```javascript
{
    "type": "queue",
    "direction": "out",
    "name": "msg",
    "queueName": "outqueue",
    "connection": "MyStorageConnection"
}
```

Ayrıca, aynı bağlama tanımını doğrudan işlevinize ekleyebilirsiniz.json.

İşlev kodunuzda, `msg` bağlama, bu `context`örnekte olduğu gibi,

```javascript
context.bindings.msg = "Name passed to the function: " req.query.name;
```

Daha fazla bilgi edinmek için [Sıra depolama çıktısı bağlama](functions-bindings-storage-queue-output.md) başvurusuna bakın.

---

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

## <a name="publish-to-azure"></a>Azure’da Yayımlama

Visual Studio Code, İşlevprojenizi doğrudan Azure'da yayımlamanızı sağlar. Süreç kapsamında, Azure abonelik bir işlev uygulaması ve ilgili kaynakları oluşturursunuz. İşlev uygulaması, işlevlerinize ilişkin bir yürütme bağlamı sağlar. Proje, Azure aboneliğinizdeki yeni işlev uygulamasında paketlenir ve dağıtılır.

Visual Studio Code'tan Azure'da yeni bir işlev uygulamasına yayımladığınızda, hem hızlı işlev uygulaması oluşturma yolu hem de gelişmiş bir yol sunulur. 

Visual Studio Code'dan yayımladığınızda Zip [dağıtım](functions-deployment-technologies.md#zip-deploy) teknolojisinden yararlanırsınız. 

### <a name="quick-function-app-create"></a>Hızlı fonksiyon uygulaması oluşturma

**Azure'da + Yeni işlev uygulaması oluşturun'u seçtiğinizde...** uzantı, işlev uygulamanızın gerektirdiği Azure kaynakları için otomatik olarak değerler oluşturur. Bu değerler, seçtiğiniz işlev uygulama adını temel alıntır. Projenizi Azure'daki yeni bir işlev uygulamasında yayımlamak için varsayılanları kullanma örneği için [Visual Studio Code hızlı başlangıç makalesine](functions-create-first-function-vs-code.md#publish-the-project-to-azure)bakın.

Oluşturulan kaynaklar için açık adlar sağlamak istiyorsanız, gelişmiş oluşturma yolunu seçmeniz gerekir.

### <a name="publish-a-project-to-a-new-function-app-in-azure-by-using-advanced-options"></a><a name="enable-publishing-with-advanced-create-options"></a>Gelişmiş seçenekleri kullanarak Azure'da yeni bir işlev uygulamasına proje yayınlama

Aşağıdaki adımlar, projenizi gelişmiş oluşturma seçenekleriyle oluşturulan yeni bir işlev uygulamasında yayımlar:

1. **Azure: Fonksiyonlar** alanında, **İşlev Uygulaması simgesine dağıt'ı** seçin.

    ![İşlev uygulaması ayarları](./media/functions-develop-vs-code/function-app-publish-project.png)

1. Oturum açmadıysanız, **Azure'da Oturum Açmanız**istenir. Ayrıca **ücretsiz bir Azure hesabı oluşturabilirsiniz.** Tarayıcıdan oturum imzaladıktan sonra Visual Studio Code'a geri dön.

1. Birden çok aboneliğiniz varsa, işlev uygulaması için **bir abonelik seçin** ve ardından **Azure'da + Yeni İşlev Uygulaması Oluştur'u seçin... _Gelişmiş_**. Bu _Gelişmiş_ seçenek, Azure'da oluşturduğunuz kaynaklar üzerinde daha fazla denetim sağlar. 

1. İstemleri takip ederek şu bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | Azure'da işlev uygulamasını seçin | Azure'da Yeni İşlev Uygulaması Oluşturma | Bir sonraki istekte, yeni işlev uygulamanızı tanımlayan genel olarak benzersiz bir ad yazın ve ardından Enter'u seçin. İşlev uygulaması adına ilişkin geçerli karakterler `a-z`, `0-9` ve `-` işaretidir. |
    | İşletim Sistemi Seçin | Windows | İşlev uygulaması Windows'da çalışır. |
    | Barındırma planı seçin | Tüketim planı | Sunucusuz [Tüketim planı barındırma](functions-scale.md#consumption-plan) kullanılır. |
    | Yeni uygulamanız için çalışma zamanı seçin | Proje diliniz | Çalışma zamanı, yayımladığınız projeyle eşleşmelidir. |
    | Yeni kaynaklar için kaynak grubu seçme | Yeni Kaynak Grubu Oluşturma | Bir sonraki komut isteminde, bir `myResourceGroup`kaynak grubu adı yazın, gibi , ve sonra enter'u seçin. Varolan bir kaynak grubunu da seçebilirsiniz. |
    | Depolama hesabı seçin | Yeni depolama hesabı oluşturma | Bir sonraki istekte, işlev uygulamanız tarafından kullanılan yeni depolama hesabı için genel olarak benzersiz bir ad yazın ve ardından Enter'u seçin. Depolama hesabı adları 3 ile 24 karakter uzunluğunda olmalıdır ve yalnızca sayılar ve küçük harfler içerebilir. Varolan bir hesabı da seçebilirsiniz. |
    | Yeni kaynaklar için bir konum seçin | region | Yakınınızdaki bir [bölgede](https://azure.microsoft.com/regions/) veya işlevlerinizin eriştettiği diğer hizmetlerin yakınında bir konum seçin. |

    İşlev uygulamanız oluşturulduktan ve dağıtım paketi uygulandıktan sonra bir bildirim görüntülenir. Oluşturduğunuz Azure kaynakları da dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **Çıktıyı** Görüntüle'yi seçin.

## <a name="republish-project-files"></a>Proje dosyalarını yeniden yayımla

[Sürekli dağıtım](functions-continuous-deployment.md)ayarladığınızda, bağlı kaynak konumda kaynak dosyalar güncelleştirildiğinde Azure'daki işlev uygulamanız güncelleştirilir. Sürekli dağıtım öneririz, ancak proje dosyası güncelleştirmelerinizi Visual Studio Code'tan yeniden yayımlayabilirsiniz.

> [!IMPORTANT]
> Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır.

1. Visual Studio Code'da komut paletini açmak için F1'i seçin. Komut paletinde Azure Fonksiyonlarını arayın ve **seçin: İşlev uygulamasına dağıtın.**

1. Oturum açmadıysanız, **Azure'da Oturum Açmanız**istenir. Tarayıcıdan oturum kurduktan sonra Visual Studio Code'a geri dön. Birden çok aboneliğiniz varsa, işlev uygulamanızı içeren **bir abonelik seçin.**

1. Azure'da mevcut işlev uygulamanızı seçin. İşlev uygulamasındaki tüm dosyaların üzerine yazma konusunda uyarıldığınızda, uyarıyı kabul etmek ve devam etmek için **Dağıt'ı** seçin.

Proje yeniden oluşturulur, yeniden paketlenir ve Azure'a yüklenir. Varolan proje yeni paketle değiştirilir ve işlev uygulaması yeniden başlatılır.

## <a name="get-the-url-of-the-deployed-function"></a>Dağıtılan işlevin URL'sini alma

HTTP tarafından tetiklenen bir işlevi çağırmak için, işlev uygulamanıza dağıtıldığında işlevin URL'sine ihtiyacınız vardır. Bu URL gerekli [işlev tuşlarını](functions-bindings-http-webhook-trigger.md#authorization-keys)içerir. Uzantıyı, dağıtılan işlevleriniz için bu URL'leri almak için kullanabilirsiniz.

1. Komut paletini açmak için F1'i seçin ve ardından **Azure İşlevlerini arayıp çalıştırın: İşlev URL'sini kopyala.**

1. Azure'da işlev uygulamanızı seçmek için istemleri ve ardından çağırmak istediğiniz belirli HTTP tetikleyicisini izleyin.

İşlev URL'si, sorgu parametresi tarafından geçirilen `code` gerekli anahtarlarla birlikte panoya kopyalanır. POST isteklerini göndermek için bir HTTP aracı veya uzak işleve GET istekleri için bir tarayıcı kullanın.  

## <a name="run-functions-locally"></a>İşlevlerini yerel olarak çalıştırma

Azure İşlevler uzantısı, yerel geliştirme bilgisayarınızda bir İşlevler projesi çalıştırmanıza olanak tanır. Yerel çalışma zamanı, işlev uygulamanızı Azure'da barındıran çalışma zamanıile aynıdır. Yerel ayarlar [local.settings.json dosyasından](#local-settings-file)okunur.

### <a name="additional-requirements-for-running-a-project-locally"></a>Bir projeyi yerel olarak çalıştırmak için ek gereksinimler

İşlevler projenizi yerel olarak çalıştırmak için şu ek gereksinimleri karşılamanız gerekir:

* [Azure İşlevler Temel Araçları](functions-run-local.md#v2)sürümü 2.x veya daha sonra yükleyin. Core Tools paketi, projeye yerel olarak başladığınızda otomatik olarak yüklenir ve yüklenir. Temel Araçlar tüm Azure İşlevlerini çalışma süresini içerir, bu nedenle indirme ve yükleme biraz zaman alabilir.

* Seçtiğiniz dile özgü gereksinimleri yükleyin:

    | Dil | Gereksinim |
    | -------- | --------- |
    | **C #** | [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)<br/>[.NET Core CLI araçları](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)   |
    | **Java** | [Java uzantısı için Hata Ayıklama](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)<br/>[Java 8](https://aka.ms/azure-jdks)<br/>[Maven 3 veya sonrası](https://maven.apache.org/) |
    | **Javascript** | [Node.js](https://nodejs.org/)<sup>*</sup> |  
    | **Python** | [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python)<br/>[Python 3.6.8](https://www.python.org/downloads/) önerilir|

    <sup>*</sup>Aktif LTS ve Bakım LTS sürümleri (8.11.1 ve 10.14.1 önerilir).

### <a name="configure-the-project-to-run-locally"></a>Projeyi yerel olarak çalışacak şekilde yapılandırın

İşlevler çalışma süresi, HTTP ve webhooks dışındaki tüm tetikleyici türleri için dahili olarak bir Azure Depolama hesabı kullanır. Bu nedenle **Values.AzureWebJobsStorage** anahtarını geçerli bir Azure Depolama hesabı bağlantı dizesi olarak ayarlamanız gerekir.

Bu bölümde, depolama bağlantı dizesine bağlanmak ve almak için [Azure Depolama Gezgini](https://storageexplorer.com/) ile [Visual Studio Code için Azure Depolama uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) kullanır.

Depolama hesabı bağlantı dizesini ayarlamak için:

1. Visual Studio'da **Bulut Gezgini'ni**açın,**Depolama** **Hesabınızı** > Genişletin ve ardından **Özellikler'i** seçin ve **Birincil Bağlantı String** değerini kopyalayın.

2. Projenizde local.settings.json dosyasını açın ve kopyaladığınız bağlantı dizesinin **AzureWebJobsStorage** anahtarının değerini ayarlayın.

3. İşlevlerinizin gerektirdiği diğer bağlantılar için **Değerler** dizisine benzersiz anahtarlar eklemek için önceki adımı yineleyin.

Daha fazla bilgi için [Yerel ayarlar dosyasına](#local-settings-file)bakın.

### <a name="debugging-functions-locally"></a>Yerel hata ayıklama işlevleri  

İşlevlerinizi hata ayıklamak için F5'i seçin. [Core Tools][Azure İşlevleri Temel Araçlarını]indirmediyseniz, bunu yapmanız istenir. Core Tools yüklendiğinde ve çalıştırıldığında, çıktı Terminal'de gösterilir. Bu, Terminal'den `func host start` Çekirdek Araçları komutunu çalıştırmakla aynıdır, ancak ek yapı görevleri ve ekli hata ayıklama ile.  

Proje çalışırken, proje Azure'a dağıtıldığında işlevlerinizi tetikleyebilirsiniz. Proje hata ayıklama modunda çalışırken, beklendiği gibi Visual Studio Code'da kesme noktaları vurulur.

HTTP tetikleyicileri için istek URL'si Terminal'deki çıktıda görüntülenir. Bir proje yerel olarak çalışırken HTTP tetikleyicilerinin işlev anahtarları kullanılmaz. Daha fazla bilgi [için, kodunuzu Azure İşlevlerinde sınayın stratejileri'ne](functions-test-a-function.md)bakın.  

Daha fazla bilgi için bkz: [Azure İşlevler Temel Araçları Azure][İşletin Temel Araçları]ile Çalışma .

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

Varsayılan olarak, proje Azure'a yayımlandığında bu ayarlar otomatik olarak geçirilir. Bitişleri yayımladıktan sonra, ayarları local.settings.json'dan Azure'daki işlev uygulamanıza yayımlama seçeneği sunulur. Daha fazla bilgi için [bkz.](#publish-application-settings)

**ConnectionStrings'deki** değerler hiçbir zaman yayımlanmaz.

İşlev uygulama ayarları değerleri kodunuzda ortam değişkenleri olarak da okunabilir. Daha fazla bilgi için, bu dile özel başvuru makalelerinin Çevre değişkenleri bölümlerine bakın:

* [C# önceden derlenmiş](functions-dotnet-class-library.md#environment-variables)
* [C# betiği (.csx)](functions-reference-csharp.md#environment-variables)
* [Java](functions-reference-java.md#environment-variables)
* [Javascript](functions-reference-node.md#environment-variables)

## <a name="application-settings-in-azure"></a>Azure'da uygulama ayarları

Projenizdeki local.settings.json dosyasındaki ayarlar, Azure'daki işlev uygulamasındaki uygulama ayarlarıyla aynı olmalıdır. local.settings.json'a eklediğiniz tüm ayarlar Azure'daki işlev uygulamasına da eklenmelidir. Projeyi yayımladığınızda bu ayarlar otomatik olarak yüklenmiyor. Aynı şekilde, [portaldaki](functions-how-to-use-azure-function-app-settings.md#settings) işlev uygulamanızda oluşturduğunuz tüm ayarların yerel projenize indirilmesi gerekir.

### <a name="publish-application-settings"></a>Uygulama ayarlarını yayımlama

Gerekli ayarları Azure'da işlev uygulamanızda yayınlamanın en kolay yolu, projenizi yayımladıktan sonra görünen **Yükleme ayarları** bağlantısını kullanmaktır:

![Uygulama ayarlarını yükleme](./media/functions-develop-vs-code/upload-app-settings.png)

Ayarları, komut paletindeki **Azure İşlevlerini: Yerel Ayar Yükle** komutunu kullanarak da yayımlayabilirsiniz. **Azure İşlevlerini** kullanarak Azure'daki uygulama ayarlarına tek tek ayarlar ekleyebilirsiniz: Yeni Ayar Ekle komutu.

> [!TIP]
> Yayımlanmadan önce local.settings.json dosyanızı kaydettiğinizden emin olun.

Yerel dosya şifrelenirse, şifresi çözülür, yayımlanır ve yeniden şifrelenir. İki konumda çakışan değerlere sahip ayarlar varsa, nasıl devam edeceğinizi seçmeniz istenir.

Azure'daki mevcut uygulama ayarlarını görüntüleyin: Aboneliğinizi, işlev uygulamanızı ve **Uygulama Ayarlarınızı**genişleterek **işlevler** alanını görüntüleyin.

![Visual Studio Code'da işlev uygulama ayarlarını görüntüleme](./media/functions-develop-vs-code/view-app-settings.png)

### <a name="download-settings-from-azure"></a>Azure'dan indirme ayarları

Azure'da uygulama ayarları oluşturduysanız, **Azure İşlevlerini** kullanarak bunları local.settings.json dosyanıza indirebilirsiniz: Uzak Ayarları İndir komutunu indirin.

Yüklemede olduğu gibi, yerel dosya şifrelenirse, şifresi çözülür, güncellenir ve yeniden şifrelenir. İki konumda çakışan değerlere sahip ayarlar varsa, nasıl devam edeceğinizi seçmeniz istenir.

## <a name="monitoring-functions"></a>İzleme fonksiyonları

[İşlevleri yerel olarak çalıştırdığınızda,](#run-functions-locally)günlük verileri Terminal konsoluna aktarılır. İşlevler projeniz Azure'daki bir işlev uygulamasında çalışırken günlük verilerini de alabilirsiniz. Gerçek zamanlı günlük verilerini görmek için Azure'daki akış günlüklerine bağlanabilir veya işlev uygulamanızın nasıl davrandığını daha iyi anlamak için Uygulama Öngörüleri'ni etkinleştirebilirsiniz.

### <a name="streaming-logs"></a>Akış günlükleri

Bir uygulama geliştirirken, günlük bilgilerini neredeyse gerçek zamanlı olarak görmek genellikle yararlıdır. Işlevleriniz tarafından oluşturulan günlük dosyaları akışını görüntüleyebilirsiniz. Bu çıktı, HTTP tarafından tetiklenen bir işlev için bir istek için akış günlükleri bir örnektir:

![HTTP tetikleyicisi için akış günlükleri çıktısı](media/functions-develop-vs-code/streaming-logs-vscode-console.png)

Daha fazla bilgi edinmek için [Akış günlüklerine](functions-monitoring.md#streaming-logs)bakın.

[!INCLUDE [functions-enable-log-stream-vs-code](../../includes/functions-enable-log-stream-vs-code.md)]

> [!NOTE]
> Akış günlükleri, İşlevler ana bilgisayarının yalnızca tek bir örneğini destekler. Işleviniz birden çok örneğe ölçeklendirildiğinde, günlük akışında diğer örneklerden gelen veriler gösterilmez. Uygulama [Öngörüleri'ndeki Canlı Ölçümler Akışı,](../azure-monitor/app/live-stream.md) birden çok örneği destekler. Ayrıca neredeyse gerçek zamanlı olarak, akış analitiği [örneklenmiş verilere](functions-monitoring.md#configure-sampling)dayanır.

### <a name="application-insights"></a>Application Insights

İşlev uygulamanızı Application Insights ile tümleştirerek işlevlerinizin yürütülmesini izlemenizi öneririz. Azure portalında bir işlev uygulaması oluşturduğunuzda, bu tümleştirme varsayılan olarak gerçekleşir. Visual Studio yayıncılık sırasında işlev uygulamanızı oluştururken, Application Insights'ı kendiniz entegre etmeniz gerekir.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Daha fazla bilgi için Azure [İşlerini İzle'ye](functions-monitoring.md)bakın.

## <a name="c-script-projects"></a>C\# komut dosyası projeleri

Varsayılan olarak, tüm C# projeleri [C# derlenmiş sınıf kitaplığı projeleri](functions-dotnet-class-library.md)olarak oluşturulur. Bunun yerine C# komut dosyası projeleri ile çalışmayı tercih ederseniz, Azure İşlerİ uzantı ayarlarında varsayılan dil olarak C# komut dosyası seçeneğini seçmeniz gerekir:

1. **Dosya** > **Tercihleri** > **Ayarlarını**seçin.

1. Kullanıcı **Ayarları** > **Uzantıları** > **Azure İşlevlerini**gidin.

1. Azure İşlevi'nden **C#Script'i** **seçin: Proje Dili.**

Bu adımları tamamladıktan sonra, temel Çekirdek Araçları'na yapılan aramalar, C# komut dosyası (.csx) proje dosyalarını oluşturan ve yayımlayan `--csx` seçeneği içerir. Bu varsayılan dil belirtildiğinde, oluşturduğunuz tüm projeler varsayılan C# komut dosyası projeleri için. Varsayılan ayarlandığında proje dilini seçmeniz istenmez. Diğer dillerde proje oluşturmak için bu ayarı değiştirmeniz veya kullanıcı settings.json dosyasından kaldırmanız gerekir. Bu ayarı kaldırdıktan sonra, bir proje oluştururken dilinizi tekrar seçmeniz istenir.

## <a name="command-palette-reference"></a>Komut paleti başvurusu

Azure İşlevler uzantısı, Azure'daki işlev uygulamalarınızla etkileşim kurmak için bölgede yararlı bir grafik arabirimi sağlar. Komut paletinde (F1) komut olarak aynı işlevsellik de kullanılabilir. Bu Azure İşlevleri komutları kullanılabilir:

|Azure İşlevler komutu  | Açıklama  |
|---------|---------|
|**Yeni Ayarlar Ekle**  |  Azure'da yeni bir uygulama ayarı oluşturur. Daha fazla bilgi için [bkz.](#publish-application-settings) [Ayrıca bu ayarı yerel ayarlarınıza indirmeniz](#download-settings-from-azure)gerekebilir. |
| **Dağıtım Kaynağını Yapılandırma** | Azure'daki işlev uygulamanızı yerel bir Git deposuna bağlar. Daha fazla bilgi edinmek [için Azure İşlevleri için Sürekli dağıtım'a](functions-continuous-deployment.md)bakın. |
| **GitHub Deposuna Bağlan** | İşlev uygulamanızı github deposuna bağlar. |
| **Kopyalama Fonksiyonu URL'si** | Azure'da çalışan HTTP tetiklenen bir işlevin uzak URL'sini alır. Daha fazla bilgi için [bkz.](#get-the-url-of-the-deployed-function) |
| **Azure'da işlev uygulaması oluşturma** | Azure'da aboneliğinizde yeni bir işlev uygulaması oluşturur. Daha fazla bilgi edinmek için [Azure'da yeni bir işlev uygulamasında nasıl yayımlanılabilen bölüme](#publish-to-azure)bakın.        |
| **Ayarları Çözme** | Azure İşlevler tarafından şifrelenmiş [yerel ayarların](#local-settings-file) şifresini çözer: **Ayarları Şifrele.**  |
| **Fonksiyon Uygulamasını Sil** | Azure'daki aboneliğinizdeki bir işlev uygulamasını kaldırır. Uygulama Hizmeti planında başka uygulama yoksa, bunu da silme seçeneğine sahip siniz. Depolama hesapları ve kaynak grupları gibi diğer kaynaklar silinmez. Tüm kaynakları kaldırmak için [kaynak grubunu silmeniz](functions-add-output-binding-storage-queue-vs-code.md#clean-up-resources)gerekir. Yerel projeniz etkilenmez. |
|**İşlev silme**  | Varolan bir işlevi Azure'daki bir işlev uygulamasından kaldırır. Bu silme işlemi yerel projenizi etkilemediği için, bunun yerine işlevi yerel olarak kaldırmayı ve [ardından projenizi yeniden yayımlamayı](#republish-project-files)düşünün. |
| **Proxy'yi Sil** | Azure'daki işlev uygulamanızdan bir Azure İşleyişi proxy'si kaldırır. Yakınlıklar hakkında daha fazla bilgi edinmek için Azure [İşle İşle Çalışma](functions-proxies.md)'na bakın. |
| **Silme Ayarı** | Azure'da bir işlev uygulaması ayarını siler. Bu silme işlemi local.settings.json dosyanızdaki ayarları etkilemez. |
| **Repo ile bağlantıyı kesme**  | Azure'daki bir işlev uygulaması ile kaynak denetim deposu arasındaki [sürekli dağıtım](functions-continuous-deployment.md) bağlantısını kaldırır. |
| **Uzaktan Ayarları İndir** | Azure'da seçilen işlev uygulamasından ayarları yerel ayarlar.json dosyanıza indirir. Yerel dosya şifrelenirse, şifresi çözülür, güncellenir ve yeniden şifrelenir. İki konumda çakışan değerlere sahip ayarlar varsa, nasıl devam edeceğinizi seçmeniz istenir. Bu komutu çalıştırmadan önce local.settings.json dosyanızdaki değişiklikleri kaydettiğinizden emin olun. |
| **Ayarları değiştir** | Azure'da varolan bir işlev uygulaması ayarı değerini değiştirir. Bu komut yerel ayarlar.settings.json dosyanızdaki ayarları etkilemez.  |
| **Ayarları şifreleme** | Yerel `Values` [ayarlardaki](#local-settings-file)dizideki tek tek öğeleri şifreler. Bu dosyada, `IsEncrypted` yerel `true`çalışma zamanının ayarları kullanmadan önce şifresini çözeceğini belirten , aynı zamanda ayarlanır. Değerli bilgilerin sızdırılması riskini azaltmak için yerel ayarları şifreleyin. Azure'da uygulama ayarları her zaman şifreli olarak depolanır. |
| **İşlevİ Şimdi Yürüt** | Azure'da [zamanlayıcı tarafından tetiklenen](functions-bindings-timer.md) bir işlevi el ile başlatır. Bu komut sınama için kullanılır. Azure'da HTTP olmayan işlevleri tetikleme hakkında daha fazla bilgi edinmek için [bkz.](functions-manually-run-non-http.md) |
| **VS Kodu ile Kullanım Projesi'ni Başlatma** | Varolan bir İşlevler projesine gerekli Visual Studio Code proje dosyalarını ekler. Çekirdek Araçları'nı kullanarak oluşturduğunuz bir projeyle çalışmak için bu komutu kullanın. |
| **Azure İşlevlerini Temel Araçları Yükleme veya Güncelleştirme** | İşlevleri yerel olarak çalıştırmak için kullanılan [Azure İşlevler Temel Araçları'nı]yükler veya güncelleştirir. |
| **Yeniden dağıtım**  | Proje dosyalarını bağlı bir Git deposundan Azure'da belirli bir dağıtıma yeniden dağıtmanızı sağlar. Visual Studio Code'daki yerel güncelleştirmeleri yeniden yayımlamak için [projenizi yeniden yayımlayın.](#republish-project-files) |
| **Ayarları Yeniden Adlandır** | Azure'da varolan bir işlev uygulaması ayarının anahtar adını değiştirir. Bu komut yerel ayarlar.settings.json dosyanızdaki ayarları etkilemez. Azure'da ayarları yeniden adlandırdıktan sonra, [bu değişiklikleri yerel projede indirmeniz](#download-settings-from-azure)gerekir. |
| **Yeniden Başlatma** | Azure'daki işlev uygulamasını yeniden başlatın. Güncelleştirmeleri dağıtmak da işlev uygulamasını yeniden başlatır. |
| **AzureWebJobsStorage'ı Ayarlama**| `AzureWebJobsStorage` Uygulama ayarının değerini ayarlar. Bu ayar Azure İşlevler tarafından gereklidir. Azure'da bir işlev uygulaması oluşturulduğunda ayarlanır. |
| **Başlangıç** | Azure'da durdurulmuş bir işlev uygulaması başlatır. |
| **Akış Günlüklerini Başlat** | Azure'daki işlev uygulamasıiçin akış günlüklerini başlatır. Azure'da uzaktan sorun giderme sırasında günlükleri, günlük bilgilerini neredeyse gerçek zamanlı olarak görmeniz gerekiyorsa kullanın. Daha fazla bilgi edinmek için [Akış günlüklerine](#streaming-logs)bakın. |
| **Durdur** | Azure'da çalışan bir işlev uygulamasını durdurur. |
| **Akış Günlüklerini Durdur** | Azure'daki işlev uygulamasının akış günlüklerini durdurur. |
| **Yuva Ayarı olarak geçiş** | Etkinleştirildiğinde, belirli bir dağıtım yuvası için uygulama ayarının devam etmesini sağlar. |
| **Azure İşlevlerini Temel Araçları Kaldırma** | Uzantı tarafından gerekli olan Azure İşlevler Temel Araçlarını kaldırır. |
| **Yerel Ayarları Yükle** | Ayarları local.settings.json dosyanızdan Azure'da seçilen işlev uygulamasına yükler. Yerel dosya şifrelenirse, şifresi çözülür, yüklenir ve yeniden şifrelenir. İki konumda çakışan değerlere sahip ayarlar varsa, nasıl devam edeceğinizi seçmeniz istenir. Bu komutu çalıştırmadan önce local.settings.json dosyanızdaki değişiklikleri kaydettiğinizden emin olun. |
| **GitHub'da Commit'i görüntüle** | İşlev uygulamanız bir depoya bağlandığında belirli bir dağıtımdaki en son taahhüdü gösterir. |
| **Dağıtım Günlüklerini Görüntüle** | Azure'daki işlev uygulamasına belirli bir dağıtımın günlüklerini gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure İşlevler Temel Araçları hakkında daha fazla bilgi edinmek için azure [işlevleri temel araçlarıyla çalışma](functions-run-local.md)'na bakın.

.NET sınıf kitaplıkları gibi işlevler geliştirme hakkında daha fazla bilgi edinmek için [Azure İşlevler C# geliştirici başvurusuna](functions-dotnet-class-library.md)bakın. Bu makalede, Azure İşlevler tarafından desteklenen çeşitli bağlama türlerini bildirmek için özniteliklerin nasıl kullanılacağına iliş eden örneklere bağlantılar da verilmektedir.

[Visual Studio Code için Azure İşlevleri uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
[Azure İşlevler Temel Araçları]: functions-run-local.md
