---
title: Visual Studio Code’u kullanarak Azure’da ilk işlevinizi oluşturma
description: Visual Studio Code’daki Azure İşlevleri uzantısını kullanarak basit bir HTTP ile tetiklenen işlev oluşturun ve Azure’da yayımlayın.
ms.topic: quickstart
ms.date: 01/10/2020
ms.custom: mvc, devcenter
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 08575164334db965a9e5592b761a06205208de9e
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732782"
---
# <a name="quickstart-create-an-azure-functions-project-using-visual-studio-code"></a>Hızlı başlatma: Visual Studio Kodunu kullanarak Azure İşlevleri projesi oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir işlev oluşturmak için Visual Studio Code'u kullanırsınız. Kodu yerel olarak sınandıktan sonra, kodu Azure İşlevlerinin sunucusuz ortamına dağıtirsınız. Bu hızlı başlangıcın tamamlanması, Azure hesabınızda birkaç USD sent veya daha az küçük bir maliyete neden olabilir. 

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python" 
Bu makalenin [CLI tabanlı](functions-create-first-azure-function-azure-cli.md) bir sürümü de vardır.
::: zone-end  

::: zone pivot="programming-language-java"  
> [!NOTE]
> VS Kodu tercih geliştirme aracı değilse, [Maven,](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java) [Gradle](/azure/azure-functions/functions-create-first-java-gradle) ve [IntelliJ IDEA](/azure/developer/java/toolkit-for-intellij/quickstart-functions)kullanarak Java geliştiricileri için benzer öğreticiler göz atın.
::: zone-end  

## <a name="configure-your-environment"></a>Ortamınızı yapılandırma

Başlamadan önce aşağıdaki gereksinimleri n istediğinizden emin olun:

+ Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

::: zone pivot="programming-language-csharp,programming-language-powershell,programming-language-python"  
+ [Node.js](https://nodejs.org/), Windows tarafından npm için gereklidir. Sadece [Aktif LTS ve Bakım LTS sürümleri.](https://nodejs.org/about/releases/) Sürümünüzü `node --version` kontrol etmek için komutu kullanın.
    macOS ve Linux'ta yerel geliştirme için gerekli değildir.   
::: zone-end  
::: zone pivot="programming-language-javascript,programming-language-typescript"  
+ [Node.js](https://nodejs.org/), Active LTS ve Maintenance LTS versiyonları (10.14.1 önerilir). Sürümünüzü `node --version` kontrol etmek için komutu kullanın.
::: zone-end 
::: zone pivot="programming-language-python"
+ [Python 3.8](https://www.python.org/downloads/release/python-381/), [Python 3.7](https://www.python.org/downloads/release/python-375/), [Python 3.6](https://www.python.org/downloads/release/python-368/) Azure Fonksiyonları (x64) tarafından desteklenir.
::: zone-end   
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Çekirdek SDK 2.2+](https://www.microsoft.com/net/download)  
::: zone-end  
::: zone pivot="programming-language-java"  
+ [Java Geliştirici Kiti](https://aka.ms/azure-jdks), sürüm 8.

+ [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri.
::: zone-end  
+ [Desteklenen](https://code.visualstudio.com/) [platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms)birinde Visual Studio Code .  
::: zone pivot="programming-language-csharp"  
+ Visual Studio Code için [C# uzantısı.](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)  
::: zone-end  
::: zone pivot="programming-language-python"
+ Visual Studio Code için [Python uzantısı.](https://marketplace.visualstudio.com/items?itemName=ms-python.python)  
::: zone-end  
::: zone pivot="programming-language-powershell"
+ [Visual Studio Code için PowerShell uzantısı.](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) 
::: zone-end  
::: zone pivot="programming-language-java"  
+ [Java uzantı paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
::: zone-end  

+ Visual Studio Code için [Azure İşlevler uzantısı.](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) 

## <a name="create-your-local-project"></a><a name="create-an-azure-functions-project"></a>Yerel projenizi oluşturun 

Bu bölümde, seçtiğiniz dilde yerel bir Azure İşlevler projesi oluşturmak için Visual Studio Code'u kullanırsınız. Bu makalenin ilerleyen saatlerinde, işlev kodunuzu Azure'da yayımlarsınız. 

1. Etkinlik çubuğundaki Azure simgesini, ardından **Azure: İşlevler** alanında yeni **proje oluştur** simgesini seçin.

    ![Yeni bir proje oluştur'u seçin](media/functions-create-first-function-vs-code/create-new-project.png)

1. Proje çalışma alanınız için bir dizin konumu seçin ve **Seç'i**seçin.

    > [!NOTE]
    > Bu adımlar, bir çalışma alanı dışında tamamlanacak şekilde tasarlanmıştır. Bu örnekte, bir çalışma alanının parçası olan bir proje klasörünü seçmeyin.

1. İstemlerde aşağıdaki bilgileri sağlayın:

    ::: zone pivot="programming-language-csharp"
    + **İşlev projeniz için**bir `C#`dil seçin : Seçin.
    ::: zone-end
    ::: zone pivot="programming-language-javascript"
    + **İşlev projeniz için**bir `JavaScript`dil seçin : Seçin.
    ::: zone-end
    ::: zone pivot="programming-language-typescript"
    + **İşlev projeniz için**bir `TypeScript`dil seçin : Seçin.
    ::: zone-end
    ::: zone pivot="programming-language-powershell"
    + **İşlev projeniz için**bir `PowerShell`dil seçin : Seçin.
    ::: zone-end
    ::: zone pivot="programming-language-python"
    + **İşlev projeniz için**bir `Python`dil seçin : Seçin.

    + **Sanal ortam oluşturmak için bir Python takma adı seçin**: Python yorumlayıcınızın konumunu seçin. Konum gösterinmiyorsa, Python ikilinize tam yolu yazın.  
    ::: zone-end

    ::: zone pivot="programming-language-java"  
    + **İşlev projeniz için**bir `Java`dil seçin : Seçin.

    + **Grup kimliği sağlayın** `com.function`: Seçin.

    + **Bir yapı kimliği** `myFunction`sağlayın : Seçin.

    + **Bir sürüm**sağlayın `1.0-SNAPSHOT`: Seçin.

    + **Paket adı ver** `com.function`: Seç .

    + **Bir uygulama adı** `myFunction-12345`sağlayın : Seçin.
    ::: zone-end  
    ::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"
    + **Projenizin ilk işlevi için bir** `HTTP trigger`şablon seçin : Seçin.
    
    + **Bir işlev adı** `HttpExample`sağlayın : Yazın.
    ::: zone-end  
    ::: zone pivot="programming-language-csharp"
    + **Ad alanı sağlayın** `My.Functions`: Yazın. 
    ::: zone-end  
    ::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"
    + **Yetkilendirme düzeyi** `Anonymous`: Herkesin işlev bitiş noktasını aramasını sağlayan , seçin. Yetkilendirme düzeyi hakkında bilgi edinmek için [Yetkilendirme tuşlarına](functions-bindings-http-webhook-trigger.md#authorization-keys)bakın.
    ::: zone-end  
    + **Projenizi nasıl açmak istediğinizi**seçin `Add to workspace`: Seçin.

1. Bu bilgileri kullanarak Visual Studio Code, HTTP tetikleyicisi olan bir Azure İşleyiş projesi oluşturur. Yerel proje dosyalarını Explorer'da görüntüleyebilirsiniz. Oluşturulan dosyalar hakkında daha fazla bilgi edinmek için [oluşturulan proje dosyalarına](functions-develop-vs-code.md#generated-project-files)bakın. 

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-python,programming-language-java"

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

::: zone-end

::: zone pivot="programming-language-powershell"

[!INCLUDE [functions-run-function-test-local-vs-code-ps](../../includes/functions-run-function-test-local-vs-code-ps.md)]

::: zone-end

İşlevin yerel bilgisayarınızda doğru çalıştığını doğruladıktan sonra, projeyi doğrudan Azure'da yayınlamak için Visual Studio Code'u kullanmanın zamanı gelir. 

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="run-the-function-in-azure"></a>İşlevi Azure'da çalıştırın

1. Azure'da: Yan çubuktaki **işlevler** alanında, aboneliğiniz altındaki yeni işlev uygulamasını genişletin. **İşlevleri**Genişlet , **httpExample'da**sağ tıkla (Windows) veya Ctrl + +click (macOS) ve ardından **Kopyala işlev URL'sini**seçin.

    ![Yeni HTTP tetikleyicisi için işlev URL'sini kopyalama](./media/functions-create-first-function-vs-code/function-copy-endpoint-url.png)

1. HTTP isteği için bu URL'yi tarayıcınızın adres çubuğuna yapıştırın, sorgu dizesini `name` `?name=Functions` bu URL'nin sonuna ekleyin ve isteği yürütün. HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

        http://<functionappname>.azurewebsites.net/api/httpexample?name=Functions 
        
    Aşağıdaki örnek, tarayıcıda işlev tarafından döndürülen uzaktan GET isteğine verilen yanıtı gösterir: 

    ![Tarayıcıdaki işlev yanıtı](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki adıma devam ettiğinizde, [işlevinize bir Azure Depolama kuyruğu ekleyin,](functions-add-output-binding-storage-queue-vs-code.md)daha önce yaptıklarınızı oluşturmak için tüm kaynaklarınızı yerinde tutmanız gerekir.

Aksi takdirde, daha fazla maliyete maruz kalmamak için işlev uygulamasını ve ilgili kaynaklarını silmek için aşağıdaki adımları kullanabilirsiniz.

[!INCLUDE [functions-cleanup-resources-vs-code.md](../../includes/functions-cleanup-resources-vs-code.md)]

İşlev maliyetleri hakkında daha fazla bilgi edinmek için [bkz.](functions-consumption-costs.md)

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak bir işlev uygulaması oluşturmak için Visual Studio Code’u kullandınız. Sonraki makalede, bir çıktı bağlama ekleyerek bu işlevi genişletin. Bu bağlama, string'i HTTP isteğinden Azure Sıra Depolama kuyruğundaki iletiye yazar. 

> [!div class="nextstepaction"]
> [İşlevinize Azure Depolama sırasına bağlama ekleme](functions-add-output-binding-storage-queue-vs-code.md)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
