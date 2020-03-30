---
title: Azure İşlevleri Temel Araçlarıyla Çalışma
description: Azure İşlevlerinde çalıştırmadan önce Azure işlevlerini yerel bilgisayarınızdaki komut isteminden veya terminalden nasıl kodlayacağınızı ve test edin.
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.topic: conceptual
ms.date: 03/13/2019
ms.custom: 80e4ff38-5174-43
ms.openlocfilehash: 19691a654162ee3855cb257fd42e29d2e1fc0157
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276666"
---
# <a name="work-with-azure-functions-core-tools"></a>Azure İşlevleri Temel Araçlarıyla Çalışma

Azure İşlevler Temel Araçları, yerel bilgisayarınızdaki işlevlerinizi komut istemi veya terminalden geliştirmenize ve test etmenizi sağlar. Yerel işlevleriniz canlı Azure hizmetlerine bağlanabilir ve tam İşlevler çalışma süresini kullanarak yerel bilgisayarınızdaki işlevlerinizi hata ayıklayabilirsiniz. Azure aboneliğinize bir işlev uygulaması bile dağıtabilirsiniz.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Yerel bilgisayarınızda işlevler geliştirmek ve Bunları Temel Araçlar'ı kullanarak Azure'da yayımlamak aşağıdaki temel adımları izler:

> [!div class="checklist"]
> * [Temel Araçları ve bağımlılıkları yükleyin.](#v2)
> * [Dile özgü bir şablondan bir işlev uygulaması projesi oluşturun.](#create-a-local-functions-project)
> * [Tetikleyici ve bağlama uzantılarını kaydedin.](#register-extensions)
> * [Depolama ve diğer bağlantıları tanımlayın.](#local-settings-file)
> * [Tetikleyiciden ve dile özgü şablondan bir işlev oluşturun.](#create-func)
> * [İşlevi yerel olarak çalıştırın.](#start)
> * [Projeyi Azure'da yayımlayın.](#publish)

## <a name="core-tools-versions"></a>Core Tools sürümleri

Azure İşlevler Temel Araçları'nın üç sürümü vardır. Kullandığınız sürüm, yerel geliştirme ortamınıza, [dil seçiminize](supported-languages.md)ve gereken destek düzeyine bağlıdır:

+ **Sürüm 1.x**: Azure Fonksiyonları çalışma zamanının sürüm 1.x'ini destekler. Araçların bu sürümü yalnızca Windows bilgisayarlarda desteklenir ve bir [npm paketinden](https://www.npmjs.com/package/azure-functions-core-tools)yüklenir.

+ [**Sürüm 2.x/3.x**](#v2): [Azure Fonksiyonlarının 2.x sürümünü veya 3.x](functions-versions.md)sürümünü destekler. Bu sürümler [Windows,](/azure/azure-functions/functions-run-local?tabs=windows#v2) [macOS](/azure/azure-functions/functions-run-local?tabs=macos#v2)ve [Linux'u](/azure/azure-functions/functions-run-local?tabs=linux#v2) destekler ve platforma özel paket yöneticilerini veya yükleme için npm'yi kullanır.

Aksi belirtilmedikçe, bu makaledeki örnekler sürüm 3.x içindir.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools’u Yükleme

[Azure İşlevler Temel Araçları,] yerel geliştirme bilgisayarınızda çalıştırabileceğiniz Azure İşlevlerini çalıştıran aynı çalışma zamanının bir sürümünü içerir. Ayrıca işlevler oluşturmak, Azure'a bağlanmak ve işlev projelerini dağıtmak için komutlar da sağlar.

>[!IMPORTANT]
>Azure İşlevler Temel Araçları'ndan Azure'da yayımlayabilmek için [Azure CLI'yi](/cli/azure/install-azure-cli) yerel olarak yüklemiş olmalısınız.  

### <a name="version-2x-and-3x"></a><a name="v2"></a>Sürüm 2.x ve 3.x

Araçların sürüm 2.x/3.x'i ,.NET Core'da yerleşik olan Azure İşlevleri çalışma süresini kullanır. Bu sürüm [Windows,](/azure/azure-functions/functions-run-local?tabs=windows#v2) [macOS](/azure/azure-functions/functions-run-local?tabs=macos#v2)ve [Linux](/azure/azure-functions/functions-run-local?tabs=linux#v2)dahil olmak üzere tüm platformlarda desteklenir .NET Core destekler. 

> [!IMPORTANT]
> .NET Core SDK'yı yükleme gereksinimini [uzantı demetlerini]kullanarak atlayabilirsiniz.

# <a name="windows"></a>[Windows](#tab/windows)

Aşağıdaki adımlar, Windows'a Çekirdek Araçları yüklemek için npm'yi kullanır. Ayrıca [Chocolatey](https://chocolatey.org/)kullanabilirsiniz. Daha fazla bilgi [için, Çekirdek Araçları readme'ye](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows)bakın.

1. NPM içeren [Node.js]yükleyin.
    - Araçların sürüm 2.x'i için yalnızca Node.js 8.5 ve sonraki sürümler desteklenir.
    - Araçların sürüm 3.x'i için yalnızca Node.js 10 ve sonraki sürümler desteklenir.

1. Çekirdek Araçları paketini yükleyin:

    ##### <a name="v2x"></a>v2.x

    ```cmd
    npm install -g azure-functions-core-tools
    ```

    ##### <a name="v3x"></a>v3.x

    ```cmd
    npm install -g azure-functions-core-tools@3
    ```

   NPM'nin Core Tools paketini indirmesi ve yüklemesi birkaç dakika sürebilir.

1. [Uzantı demetleri]kullanmayı düşünmüyorsanız, Windows [için .NET Core 2.x SDK'yı](https://www.microsoft.com/net/download/windows)yükleyin.

# <a name="macos"></a>[Macos](#tab/macos)

Aşağıdaki adımlar, Core Tools'u macOS'a yüklemek için Homebrew'i kullanır.

1. Zaten yüklü değilse [Homebrew'ı](https://brew.sh/)yükleyin.

1. Çekirdek Araçları paketini yükleyin:

    ##### <a name="v2x"></a>v2.x

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools
    ```

    ##### <a name="v3x"></a>v3.x

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools@3
    # if upgrading on a machine that has 2.x installed
    brew link --overwrite azure-functions-core-tools@3
    ```

# <a name="linux"></a>[Linux](#tab/linux)

Aşağıdaki adımlar, Ubuntu/Debian Linux dağıtımınıza Çekirdek Araçları yüklemek için [APT'yi](https://wiki.debian.org/Apt) kullanır. Diğer Linux dağıtımları [için, Core Tools readme'ye](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux)bakın.

1. Paket bütünlüğünü doğrulamak için Microsoft paket deposu GPG anahtarını yükleyin:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

1. APT güncelleştirmesi yapmadan önce .NET geliştirme kaynak listesini ayarlayın.

   Ubuntu için APT kaynak listesini ayarlamak için şu komutu çalıştırın:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

   Debian için APT kaynak listesini ayarlamak için şu komutu çalıştırın:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/debian/$(lsb_release -rs | cut -d'.' -f 1)/prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

1. Aşağıda `/etc/apt/sources.list.d/dotnetdev.list` listelenen uygun Linux sürüm dizelerinden biri için dosyayı kontrol edin:

    | Linux dağıtımı | Sürüm |
    | --------------- | ----------- |
    | Debian 9 | `stretch` |
    | Debian 8 | `jessie` |
    | Ubuntu 18.10    | `cosmic`    |
    | Ubuntu 18.04    | `bionic`    |
    | Ubuntu 17.04    | `zesty`     |
    | Ubuntu 16.04/Linux Darphane 18    | `xenial`  |

1. APT kaynak güncelleştirmesini başlatın:

    ```bash
    sudo apt-get update
    ```

1. Çekirdek Araçları paketini yükleyin:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```

1. [Uzantı demetleri]kullanmayı planlamıyorsanız, [Linux için .NET Core 2.x SDK'yı](https://www.microsoft.com/net/download/linux)yükleyin.

---

## <a name="create-a-local-functions-project"></a>Yerel Bir İşlevler projesi oluşturma

İşlevler proje dizini, tek tek işlevler için kodu içeren alt klasörlerle birlikte [host.json](functions-host-json.md) ve [local.settings.json](#local-settings-file)dosyalarını içerir. Bu dizin, Azure'daki bir işlev uygulamasına eşdeğerdir. Fonksiyonlar klasörü yapısı hakkında daha fazla bilgi edinmek için [Azure İşlevleri geliştiricileri kılavuzuna](functions-reference.md#folder-structure)bakın.

Sürüm 2.x, projeniz için bir varsayılan dil seçmenizi gerektirir. Sürüm 2.x'te, eklenen tüm işlevler varsayılan dil şablonlarını kullanır. Sürüm 1.x'te, her işlev oluşturduğunuzda dili belirtirsiniz.

Terminal penceresinde veya komut isteminden, proje yi ve yerel Git deposunu oluşturmak için aşağıdaki komutu çalıştırın:

```
func init MyFunctionProj
```

Bir proje adı sağladığınızda, bu ada sahip yeni bir klasör oluşturulur ve başharfe aktarılır. Aksi takdirde, geçerli klasör başharfe aktarılır.  
Sürüm 2.x'te, komutu çalıştırdığınızda projeniz için bir çalışma zamanı seçmeniz gerekir. 

<pre>
Select a worker runtime:
dotnet
node
python 
powershell
</pre>

Bir dil seçmek için yukarı/aşağı ok tuşlarını kullanın ve ardından Enter tuşuna basın. JavaScript veya TypeScript işlevleri geliştirmeyi planlıyorsanız, **düğüm**seçin ve ardından dili seçin. TypeScript [bazı ek gereksinimleri](functions-reference-node.md#typescript)vardır. 

Çıktı, bir JavaScript projesi için aşağıdaki örnek gibi görünür:

<pre>
Select a worker runtime: node
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing C:\myfunctions\myMyFunctionProj\.vscode\extensions.json
Initialized empty Git repository in C:/myfunctions/myMyFunctionProj/.git/
</pre>

`func init`aksi belirtilmedikçe, yalnızca sürüm 2.x olan aşağıdaki seçenekleri destekler:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--csharp`**<br/> **`--dotnet`** | [C# sınıf kitaplığı (.cs) projesini](functions-dotnet-class-library.md)başlatır. |
| **`--csx`** | [C# komut dosyası (.csx) projesini](functions-reference-csharp.md)başlatır. Sonraki komutlarda belirtmeniz `--csx` gerekir. |
| **`--docker`** | Seçilen `--worker-runtime`temel görüntüyü kullanarak bir kapsayıcı için Dockerfile oluşturun. Özel bir Linux kapsayıcısına yayımlamayı planlarken bu seçeneği kullanın. |
| **`--docker-only`** |  Varolan bir projeye Dockerfile ekler. Local.settings.json'da belirtilmemişse veya ayarlanmamışsa, işçi çalıştırma zamanı için istemler. Varolan bir projeyi özel bir Linux kapsayıcısına yayımlamayı planlarken bu seçeneği kullanın. |
| **`--force`** | Projede varolan dosyalar olsa bile projeyi başlatma. Bu ayar, aynı ada sahip varolan dosyaların üzerine yazar. Proje klasöründeki diğer dosyalar etkilenmez. |
| **`--java`**  | Bir Java [projesini](functions-reference-java.md)başlatır. |
| **`--javascript`**<br/>**`--node`**  | Bir [JavaScript projesini](functions-reference-node.md)başlatır. |
| **`--no-source-control`**<br/>**`-n`** | Sürüm 1.x'te bir Git deposunun varsayılan oluşturulmasını önler. Sürüm 2.x'te git deposu varsayılan olarak oluşturulmaz. |
| **`--powershell`**  | Bir [PowerShell projesini başlatır.](functions-reference-powershell.md) |
| **`--python`**  | [Python projesini](functions-reference-python.md)başlatır. |
| **`--source-control`** | Git deposu oluşturulup oluşturulmadığını denetler. Varsayılan olarak, bir depo oluşturulmaz. Bir `true`depo oluşturulduğunda. |
| **`--typescript`**  | [Bir TypeScript projesini](functions-reference-node.md#typescript)başlatır. |
| **`--worker-runtime`** | Projenin dil çalışma saatini ayarlar. Desteklenen değerler şunlardır: `csharp`,`node` , `dotnet` `java` `javascript`, `powershell` `python`(JavaScript), , , ve `typescript`. Ayarlanmadığında, başlatma sırasında çalışma saatinizi seçmeniz istenir. |

> [!IMPORTANT]
> Varsayılan olarak, Core Tools sürümü 2.x [C# sınıfı projeler](functions-dotnet-class-library.md) (.csproj) olarak .NET çalışma zamanı için fonksiyon uygulaması projeleri oluşturur. Visual Studio veya Visual Studio Code ile kullanılabilen bu C# projeleri, test sırasında ve Azure'da yayımlanırken derlenir. Bunun yerine sürüm 1.x'te ve portalda oluşturulan aynı C# komut dosyası (.csx) dosyaları `--csx` oluşturmak ve bunlarla çalışmak istiyorsanız, işlevleri oluştururken ve dağıtırken parametreyi eklemeniz gerekir.

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

Varsayılan olarak, proje Azure'a yayımlandığında bu ayarlar otomatik olarak geçirilmeyecek. Bu `--publish-local-settings` ayarların Azure'daki işlev uygulamasına eklendiğinden emin olmak için [yayımlarken](#publish) anahtarı kullanın. **ConnectionStrings'teki** değerlerin hiçbir zaman yayınlanmadığını unutmayın.

İşlev uygulaması ayarları değerleri kodunuzda ortam değişkenleri olarak da okunabilir. Daha fazla bilgi için, şu dile özgü başvuru konularının Çevre değişkenleri bölümüne bakın:

* [C# önceden derlenmiş](functions-dotnet-class-library.md#environment-variables)
* [C# betiği (.csx)](functions-reference-csharp.md#environment-variables)
* [Java](functions-reference-java.md#environment-variables)
* [Javascript](functions-reference-node.md#environment-variables)

Geçerli bir depolama bağlantı dizesi ayarlanmadığında [`AzureWebJobsStorage`] ve emülatör kullanılmadığında, aşağıdaki hata iletisi gösterilir:

> AzureWebJobsStorage için local.settings.json'da eksik değer. Bu, HTTP dışındaki tüm tetikleyiciler için gereklidir. 'func azure functionapp fetch-app-settings \<functionAppName'\>çalıştırabilir veya local.settings.json'da bir bağlantı dizesi belirtebilirsiniz.

### <a name="get-your-storage-connection-strings"></a>Depolama bağlantı dizelerinizi alın

Geliştirme için Microsoft Azure Depolama Emülatörü'ni kullanırken bile gerçek bir depolama bağlantısıyla sınamak isteyebilirsiniz. Zaten bir [depolama hesabı oluşturduğunuzu](../storage/common/storage-create-storage-account.md)varsayarsak, geçerli bir depolama bağlantı dizesini aşağıdaki yollardan biriyle alabilirsiniz:

- Azure [portalından,] **Depolama hesaplarını**arayın ve seçin. 
  ![Azure portalından Depolama hesaplarını seçin](./media/functions-run-local/select-storage-accounts.png)
  
  Depolama hesabınızı seçin, **Ayarlar'da** **Access tuşlarını** seçin ve ardından **Bağlantı dize** değerlerinden birini kopyalayın.
  ![Azure portalından bağlantı dizelerini kopyalama](./media/functions-run-local/copy-storage-connection-portal.png)

- Azure hesabınıza bağlanmak için [Azure Depolama Gezgini'ni](https://storageexplorer.com/) kullanın. **Explorer'da**aboneliğinizi genişletin, **Depolama Hesaplarını**genişletin, depolama hesabınızı seçin ve birincil veya ikincil bağlantı dizesini kopyalayın.

  ![Bağlantı dizelerini Depolama Gezgini'nden kopyalama](./media/functions-run-local/storage-explorer.png)

+ Bağlantı dizesini Azure'dan aşağıdaki komutlardan biriyle indirmek için Çekirdek Araçları'nı kullanın:

  + Tüm ayarları mevcut bir işlev uygulamasından indirin:

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
  + Belirli bir depolama hesabı için Bağlantı dizesini alın:

    ```
    func azure storage fetch-connection-string <StorageAccountName>
    ```

    Azure'da zaten oturum açmış değilseniz, bunu yapmanız istenir.

## <a name="create-a-function"></a><a name="create-func"></a>İşlev oluşturma

Bir işlev oluşturmak için aşağıdaki komutu çalıştırın:

```
func new
```

Sürüm 2.x'te, `func new` çalıştırdığınızda işlev uygulamanızın varsayılan dilinde bir şablon seçmeniz istenir, ardından işleviniz için bir ad seçmeniz de istenir. Sürüm 1.x'te, dili seçmeniz de istenir.

<pre>
Select a language: Select a template:
Blob trigger
Cosmos DB trigger
Event Grid trigger
HTTP trigger
Queue trigger
SendGrid
Service Bus Queue trigger
Service Bus Topic trigger
Timer trigger
</pre>

İşlev kodu, aşağıdaki sıra tetikleyici çıkışında görebileceğiniz gibi, sağlanan işlev adı içeren bir alt klasörde oluşturulur:

<pre>
Select a language: Select a template: Queue trigger
Function name: [QueueTriggerJS] MyQueueTrigger
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\index.js
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\readme.md
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\sample.dat
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\function.json
</pre>

Aşağıdaki bağımsız değişkenleri kullanarak komutta bu seçenekleri de belirtebilirsiniz:

| Bağımsız Değişken     | Açıklama                            |
| ------------------------------------------ | -------------------------------------- |
| **`--csx`** | (Sürüm 2.x) Sürüm 1.x'te ve portalda kullanılan c# komut dosyası (.csx) şablonlarını oluşturur. |
| **`--language`**, **`-l`**| C#, F#veya JavaScript gibi şablon programlama dili. Bu seçenek sürüm 1.x'te gereklidir. Sürüm 2.x'te bu seçeneği kullanmayın veya alt çalışma zamanıyla eşleşen bir dil seçin. |
| **`--name`**, **`-n`** | Işlev adı. |
| **`--template`**, **`-t`** | Desteklenen `func templates list` her dil için kullanılabilir şablonların tam listesini görmek için komutu kullanın.   |

Örneğin, tek bir komutta bir JavaScript HTTP tetikleyicisi oluşturmak için çalıştırın:

```
func new --template "Http Trigger" --name MyHttpTrigger
```

Tek bir komutta sıra tetiklenen bir işlev oluşturmak için çalıştırın:

```
func new --template "Queue Trigger" --name QueueTriggerJS
```

## <a name="run-functions-locally"></a><a name="start"></a>İşlevlerini yerel olarak çalıştırma

Bir İşlevler projesini çalıştırmak için, İşlevler ana bilgisayarını çalıştırın. Ana bilgisayar, projedeki tüm işlevler için tetikleyicileri etkinleştirer. Başlangıç komutu, proje dilinize bağlı olarak değişir.

# <a name="c"></a>[C\#](#tab/csharp)

```
func start --build
```
# <a name="javascript"></a>[Javascript](#tab/node)

```
func start
```

# <a name="python"></a>[Python](#tab/python)

```
func start
```
Bu komut [sanal ortamda çalıştırılmalıdır.](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-python#create-venv)

# <a name="typescript"></a>[TypeScript](#tab/ts)

```
npm install
npm start     
```

---

>[!NOTE]  
> İşlevler çalışma süresisürüm 1.x aşağıdaki örnekte olduğu gibi `host` komut gerektirir:
>
> ```
> func host start
> ```

`func start`aşağıdaki seçenekleri destekler:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--no-build`** | Çalıştırmadan önce geçerli proje oluşturmayın. Sadece dotnet projeleri için. Varsayılan, false olarak ayarlanır. Sürüm 1.x için desteklenmez. |
| **`--cert`** | Özel anahtar içeren bir .pfx dosyasına giden yol. Sadece `--useHttps`. Sürüm 1.x için desteklenmez. |
| **`--cors-credentials`** | Sürüm 1.x için desteklenmeyen, başlangıçlar arası kimlik doğrulama isteklerine (örneğin çerezler ve Kimlik Doğrulama üstbilgisi) izin verin. |
| **`--cors`** | Hiçbir boşluk ile CORS kökenleri virgülle ayrılmış bir liste. |
| **`--language-worker`** | Dil işçisini yapılandırmak için bağımsız değişkenler. Örneğin, [hata ayıklama bağlantı noktası ve diğer gerekli bağımsız değişkenler](https://github.com/Azure/azure-functions-core-tools/wiki/Enable-Debugging-for-language-workers)sağlayarak dil çalışanı için hata ayıklama etkinleştirebilirsiniz. Sürüm 1.x için desteklenmez. |
| **`--nodeDebugPort`**, **`-n`** | Düğüm.js hata ayıklama için bağlantı noktası kullanılır. Varsayılan: launch.json veya 5858'den bir değer. Yalnızca Sürüm 1.x. |
| **`--password`** | Parola veya .pfx dosyasının parolasını içeren bir dosya. Sadece `--cert`. Sürüm 1.x için desteklenmez. |
| **`--port`**, **`-p`** | Dinleyecek yerel bağlantı noktası. Varsayılan değer: 7071. |
| **`--pause-on-error`** | İşlemden çıkmadan önce ek giriş için duraklatın. Yalnızca Core Tools'u entegre bir geliştirme ortamından (IDE) başlatırken kullanılır.|
| **`--script-root`**, **`--prefix`** | Çalıştırılacak veya dağıtılacak işlev uygulamasının köküne giden yolu belirtmek için kullanılır. Bu, proje dosyalarını bir alt klasöre oluşturan derlenmiş projeler için kullanılır. Örneğin, bir C# sınıfı kitaplık projesi oluşturduğunuzda, host.json, local.settings.json ve function.json dosyaları gibi `MyProject/bin/Debug/netstandard2.0`bir yolu olan bir *kök* alt klasöründe oluşturulur. Bu durumda önek 'i `--script-root MyProject/bin/Debug/netstandard2.0`. olarak ayarlayın Azure'da çalışırken işlev uygulamasının kökü budur. |
| **`--timeout`**, **`-t`** | İşlevler için zaman adedi saniyeler içinde başlayacak. Varsayılan: 20 saniye.|
| **`--useHttps`** | Bind `https://localhost:{port}` yerine `http://localhost:{port}`. Varsayılan olarak, bu seçenek bilgisayarınızda güvenilir bir sertifika oluşturur.|

İşlevler ana bilgisayar başladığında, HTTP tarafından tetiklenen işlevlerin URL'sini çıkar:

<pre>
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
</pre>

>[!IMPORTANT]
>Yerel olarak çalışırken, HTTP uç noktaları için yetkilendirme zorlanmaz. Bu, tüm yerel HTTP isteklerinin `authLevel = "anonymous"`. Daha fazla bilgi için [HTTP bağlayıcı makaleye](functions-bindings-http-webhook-trigger.md#authorization-keys)bakın.

### <a name="passing-test-data-to-a-function"></a>Test verilerini bir işleve geçirme

İşlevlerinizi yerel olarak sınamak [için, İşlevler ana bilgisayarını başlatın](#start) ve HTTP isteklerini kullanarak yerel sunucuda uç noktaları ararsınız. Aradığınız bitiş noktası işlevin türüne bağlıdır.

>[!NOTE]
> Bu konudaki örnekler, terminalden veya komut isteminden HTTP istekleri göndermek için cURL aracını kullanır. Yerel sunucuya HTTP isteklerigöndermek için seçtiğiniz bir aracı kullanabilirsiniz. CURL aracı varsayılan olarak Linux tabanlı sistemlerde kullanılabilir ve Windows 10 17063 ve sonrası bir yapıya sahiptir. Eski Windows'da, önce [cURL aracını](https://curl.haxx.se/)indirmeniz ve yüklemeniz gerekir.

Test işlevleri hakkında daha genel bilgi [için, kodunuzu Azure İşlevlerinde sınayın stratejileri'ne](functions-test-a-function.md)bakın.

#### <a name="http-and-webhook-triggered-functions"></a>HTTP ve webhook tetiklenen fonksiyonlar

Http ve webhook tetiklenen işlevleri yerel olarak çalıştırmak için aşağıdaki bitiş noktasını çağırırsınız:

    http://localhost:{port}/api/{function_name}

İşlevler ana bilgisayarının dinlediği sunucu adını ve bağlantı noktasını kullandığınızdan emin olun. İşlev ana bilgisayarını başlatırken oluşturulan çıktıda bunu görürsünüz. Tetikleyici tarafından desteklenen herhangi bir HTTP yöntemini kullanarak bu URL'yi arayabilirsiniz.

Aşağıdaki cURL komutu, `MyHttpTrigger` sorgu dizesinde geçirilen _ad_ parametresi ile GET isteğinden hızlı başlatma işlevini tetikler.

```
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```

Aşağıdaki örnek, posta isteği isteği nin istek gövdesinde _geçen addan_ çağrılan aynı işlevdir:

# <a name="bash"></a>[Bash](#tab/bash)
```bash
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```
# <a name="cmd"></a>[Cmd](#tab/cmd)
```cmd
curl --request POST http://localhost:7071/api/MyHttpTrigger --data "{'name':'Azure Rocks'}"
```
---

Sorgu dizesinde veri aktaran bir tarayıcıdan GET isteklerini gerçekleştirebilirsiniz. Diğer tüm HTTP yöntemleri için cURL, Fiddler, Postman veya benzer bir HTTP test aracı kullanmanız gerekir.

#### <a name="non-http-triggered-functions"></a>HTTP olmayan tetiklenen fonksiyonlar

HTTP tetikleyicileri ve webhooks ve Olay Izgara tetikleyicileri dışında işlevleri her türlü için, bir yönetim bitiş noktası çağırarak yerel olarak işlevlerinizi test edebilirsiniz. Bu bitiş noktasını yerel sunucuda bir HTTP POST isteğiyle çağırmak işlevi tetikler. 

Olay Izgarası tetiklenen işlevleri yerel olarak test etmek [için, görüntüleyen web uygulamasıyla yerel teste](functions-bindings-event-grid-trigger.md#local-testing-with-viewer-web-app)bakın.

Test verilerini isteğe bağlı olarak POST isteğinin gövdesindeki yürütmeye geçirebilirsiniz. Bu işlev, Azure portalındaki **Test** sekmesine benzer.

HTTP olmayan işlevleri tetiklemek için aşağıdaki yönetici bitiş noktasını çağırırsınız:

    http://localhost:{port}/admin/functions/{function_name}

Test verilerini bir işlevin yönetici bitiş noktasına geçirmek için, verileri BIR POST istek iletisinin gövdesinde sağlamanız gerekir. İleti gövdesinin aşağıdaki JSON biçimine sahip olması gerekir:

```JSON
{
    "input": "<trigger_input>"
}
```

Değer, `<trigger_input>` işlev tarafından beklenen biçimde veri içerir. Aşağıdaki cURL örneği bir `QueueTriggerJS` işlev için bir POST'tür. Bu durumda, giriş, kuyrukta bulunması beklenen iletiye eşdeğer bir dizedir.

# <a name="bash"></a>[Bash](#tab/bash)
```bash
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTrigger
```
# <a name="cmd"></a>[Cmd](#tab/cmd)
```bash
curl --request POST -H "Content-Type:application/json" --data "{'input':'sample queue data'}" http://localhost:7071/admin/functions/QueueTrigger
```
---

#### <a name="using-the-func-run-command-version-1x-only"></a>Komutu `func run` kullanma (yalnızca sürüm 1.x)

>[!IMPORTANT]
> Komut `func run` yalnızca araçların sürüm 1.x'inde desteklenir. Daha fazla bilgi için Azure [İşlevleri çalışma zamanı sürümlerini nasıl hedeflenizi](set-runtime-version.md)zedeten konuya bakın.

Sürüm 1.x'te, işlev için giriş `func run <FunctionName>` verilerini kullanarak ve sağlayarak doğrudan bir işlevi çağırabilirsiniz. Bu komut, Azure portalındaki **Test** sekmesini kullanarak bir işlev çalıştırmaya benzer.

`func run`aşağıdaki seçenekleri destekler:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--content`**, **`-c`** | Satır İçi içerik. |
| **`--debug`**, **`-d`** | İşlevçalıştırmadan önce ana bilgisayar işlemine bir hata ayıklama takın.|
| **`--timeout`**, **`-t`** | Yerel İşlevler ana bilgisayar hazır olana kadar bekleme süresi (saniye cinsinden).|
| **`--file`**, **`-f`** | İçerik olarak kullanılacak dosya adı.|
| **`--no-interactive`** | Giriş isteminde değildir. Otomasyon senaryoları için yararlıdır.|

Örneğin, HTTP tarafından tetiklenen bir işlevi çağırmak ve içerik gövdesini geçmek için aşağıdaki komutu çalıştırın:

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <a name="publish-to-azure"></a><a name="publish"></a>Azure’da Yayımlama

Azure İşlevler Temel Araçları iki tür dağıtımı destekler: işlev proje dosyalarını [Zip Dağıtımı](functions-deployment-technologies.md#zip-deploy) yoluyla doğrudan işlev uygulamanıza dağıtma ve özel bir [Docker kapsayıcısı dağıtma.](functions-deployment-technologies.md#docker-container) Azure aboneliğinizde kodunuzu dağıtacağınız [bir işlev uygulaması oluşturmuş](functions-cli-samples.md#create)olmalısınız. Derleme gerektiren projeler, ikili lerin dağıtılabilmeleri için oluşturulmalıdır.

>[!IMPORTANT]
>Azure'da Temel Araçlar'dan yayımlayabilmek için [Azure CLI'yi](/cli/azure/install-azure-cli) yerel olarak yüklemiş olmalısınız.  

Proje klasörü, yayımlanmaması gereken dile özgü dosyalar ve dizinler içerebilir. Dışlanan öğeler kök proje klasöründe bir .funcignore dosyasında listelenir.     

### <a name="deploy-project-files"></a><a name="project-file-deployment"></a>Proje dosyalarını dağıtma

Yerel kodunuzu Azure'daki bir işlev uygulamasında `publish` yayımlamak için aşağıdaki komutu kullanın:

```
func azure functionapp publish <FunctionAppName>
```

Bu komut, Azure'da varolan bir işlev uygulamasında yayımlanır. Aboneliğinizde bulunmayan bir `<FunctionAppName>` yerde yayımlamaya çalışırsanız hata alırsınız. Azure CLI'yi kullanarak komut istemi veya terminal penceresinden bir işlev uygulaması oluşturmayı öğrenmek [için](./scripts/functions-cli-create-serverless.md)bkz. Varsayılan olarak, bu komut [uzaktan yapı](functions-deployment-technologies.md#remote-build) kullanır ve [dağıtım paketinden çalıştırmak](run-functions-from-deployment-package.md)için uygulamanızı dağılar. Önerilen bu dağıtım modunu devre `--nozip` dışı kullanabilirsiniz.

>[!IMPORTANT]
> Azure portalında bir işlev uygulaması oluşturduğunuzda, varsayılan olarak İşlev çalışma zamanının 2.x sürümünü kullanır. İşlev uygulamasının çalışma zamanının 1.x sürümünü kullanmasını sağlamak için, [sürüm 1.x'te Çalıştır'daki](functions-versions.md#creating-1x-apps)yönergeleri izleyin.
> Varolan işlevleri olan bir işlev uygulamasının çalışma zamanı sürümünü değiştiremezsiniz.

Aşağıdaki yayımlama seçenekleri her iki sürüm için de geçerlidir, 1.x ve 2.x:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Ayarları local.settings.json to Azure'da yayımlayarak, ayar zaten varsa üzerine yazmak için gerekenleri yapın. Microsoft Azure Depolama Emülatörü kullanıyorsanız, önce uygulama ayarını gerçek bir [depolama bağlantısıyla](#get-your-storage-connection-strings)değiştirin. |
| **`--overwrite-settings -y`** | Kullanıldığında uygulama ayarlarını üzerine `--publish-local-settings -i` yazmak için istemi bastırın.|

Aşağıdaki yayımlama seçenekleri yalnızca sürüm 2.x'te desteklenir:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--publish-settings-only`**, **`-o`** |  Yalnızca ayarları yayımlayın ve içeriği atlayın. Varsayılan istektir. |
|**`--list-ignored-files`** | Yayımlama sırasında .funcignore dosyasını temel alan, yoksayılan dosyaların listesini görüntüler. |
| **`--list-included-files`** | .funcignore dosyasını temel alan, yayımlanan dosyaların listesini görüntüler. |
| **`--nozip`** | Varsayılan `Run-From-Package` modu kapatır. |
| **`--build-native-deps`** | Python işlev uygulamalarını yayınlarken .wheels klasörünü atlar. |
| **`--build`**, **`-b`** | Bir Linux işlev uygulamasına dağıtılırken yapı eylemi gerçekleştirir. Kabul `remote` eder: `local`ve . |
| **`--additional-packages`** | Yerel bağımlılıklar inşa ederken yüklenmesi gereken paketlerin listesi. Örneğin: `python3-dev libevent-dev`. |
| **`--force`** | Belirli senaryolarda yayımlama öncesi doğrulamayı yoksay. |
| **`--csx`** | C# komut dosyası (.csx) projesi yayımlayın. |
| **`--no-build`** | .NET sınıfı kitaplık işlevleri oluşturmayın. |
| **`--dotnet-cli-params`** | Derlenmiş C# (.csproj) işlevlerini yayımlarken, temel araçlar 'dotnet build --output bin/publish' adını alır. Buna geçirilen parametreler komut satırına eklenir. |

### <a name="deploy-custom-container"></a>Özel kapsayıcıdağıtma

Azure İşlevler, işlev projenizi özel bir [Docker kapsayıcısında](functions-deployment-technologies.md#docker-container)dağıtmanızı sağlar. Daha fazla bilgi için bkz: [Özel bir resim kullanarak Linux üzerinde işlev oluştur.](functions-create-function-linux-custom-image.md) Özel kapsayıcılarda Dockerdosyası olmalıdır. Dockerfile'li bir uygulama oluşturmak için dockerfile `func init`seçeneğini kullanın.

```
func deploy
```

Aşağıdaki özel kapsayıcı dağıtım seçenekleri kullanılabilir:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--registry`** | Geçerli kullanıcının oturum açmış olduğu Docker Registry'nin adı. |
| **`--platform`** | Fonksiyon uygulaması için barındırma platformu. Geçerli seçenekler,`kubernetes` |
| **`--name`** | İşlev uygulama adı. |
| **`--max`**  | İsteğe bağlı olarak, dağıtılabilmek için en fazla işlev uygulaması örneği sayısını ayarlar. |
| **`--min`**  | İsteğe bağlı olarak, dağıtılabilmek için en az işlev uygulaması örneği sayısını ayarlar. |
| **`--config`** | İsteğe bağlı dağıtım yapılandırma dosyası ayarlar. |

## <a name="monitoring-functions"></a>İzleme fonksiyonları

İşlevlerinizin yürütülmesini izlemenin önerilen yolu, Azure Uygulama Öngörüleri ile tümleştirmektir. Yürütme günlüklerini yerel bilgisayarınıza da aktarabilirsiniz. Daha fazla bilgi için Azure [İşlerini İzle'ye](functions-monitoring.md)bakın.

### <a name="application-insights-integration"></a>Uygulama Öngörüleri entegrasyonu

Azure'da işlev uygulamanızı oluştururken Uygulama Öngörüleri tümleştirmesi etkinleştirilmelidir. İşlev uygulamanız bir Uygulama Öngörüleri örneğine bağlı değilse, bu tümleştirmeyi Azure portalında kolayca yapabilirsiniz. 

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

### <a name="enable-streaming-logs"></a>Akış günlüklerini etkinleştirme

Yerel bilgisayarınızdaki bir komut satırı oturumunda işlevleriniz tarafından oluşturulan günlük dosyaları akışını görüntüleyebilirsiniz. 

#### <a name="native-streaming-logs"></a>Yerel akış günlükleri

[!INCLUDE [functions-streaming-logs-core-tools](../../includes/functions-streaming-logs-core-tools.md)]

Bu tür akış günlükleri, işlev uygulamanız için Application Insights tümleştirmesinin etkinleştirilmesini gerektirir.   


## <a name="next-steps"></a>Sonraki adımlar

Azure İşlevlerini Temel Araçlar Microsoft öğrenerek Azure İşlevlerini nasıl geliştirecek, test edin ve yayımlayacağınızı öğrenin [Microsoft, Azure](https://docs.microsoft.com/learn/modules/develop-test-deploy-azure-functions-with-core-tools/) İşlevleri Temel [Araçları'nın açık kaynak kodalır ve GitHub'da barındırılır.](https://github.com/azure/azure-functions-cli)  
Hata veya özellik isteği nde bulunmak için [GitHub sorununu açın.](https://github.com/azure/azure-functions-cli/issues)

<!-- LINKS -->

[Azure İşlevler Temel Araçları]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portalında]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
[`FUNCTIONS_WORKER_RUNTIME`]: functions-app-settings.md#functions_worker_runtime
['AzureWebJobsStorage']: functions-app-settings.md#azurewebjobsstorage
[uzatma paketleri]: functions-bindings-register.md#extension-bundles
