---
title: Visual Studio Code’u kullanarak Azure’da ilk işlevinizi oluşturma
description: Visual Studio Code’daki Azure İşlevleri uzantısını kullanarak basit bir HTTP ile tetiklenen işlev oluşturun ve Azure’da yayımlayın.
ms.topic: quickstart
ms.date: 01/10/2020
ms.custom: mvc, devcenter, seo
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 64c2e813743a772692efcb1d966c8ab7b52cc66d
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82628206"
---
# <a name="quickstart-create-a-function-in-azure-using-visual-studio-code"></a>Hızlı başlangıç: Visual Studio Code kullanarak Azure 'da bir işlev oluşturma

::: zone pivot="programming-language-csharp"  
Bu makalede, HTTP isteklerine yanıt veren bir C# sınıf kitaplığı tabanlı işlev oluşturmak için Visual Studio Code kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end  
::: zone pivot="programming-language-javascript"
Bu makalede, HTTP isteklerine yanıt veren bir JavaScript işlevi oluşturmak için Visual Studio Code kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end
::: zone pivot="programming-language-typescript"
Bu makalede, HTTP isteklerine yanıt veren bir TypeScript işlevi oluşturmak için Visual Studio Code kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end   
::: zone pivot="programming-language-powershell"
Bu makalede, HTTP isteklerine yanıt veren bir PowerShell işlevi oluşturmak için Visual Studio Code kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end  
::: zone pivot="programming-language-python" 
Bu makalede, HTTP isteklerine yanıt veren bir Python işlevi oluşturmak için Visual Studio Code kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end  
::: zone pivot="programming-language-java" 
Bu makalede, HTTP isteklerine yanıt veren bir Java işlevi oluşturmak için Visual Studio Code kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. 
::: zone-end

Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur. 

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"
Bu makalenin [CLI tabanlı bir sürümü](functions-create-first-azure-function-azure-cli.md) de vardır.
::: zone-end
::: zone pivot="programming-language-java"  
> [!NOTE]
> Tercih edilmemiş geliştirme aracınız Visual Studio Code, [Maven](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java), [Gradle](/azure/azure-functions/functions-create-first-java-gradle) ve [IntelliJ fikrini](/azure/developer/java/toolkit-for-intellij/quickstart-functions)kullanarak Java geliştiricileri için benzer öğreticilerimize göz atın.
::: zone-end  

## <a name="configure-your-environment"></a>Ortamınızı yapılandırma

Başlamadan önce, aşağıdaki gereksinimlerin yerinde olduğundan emin olun:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="programming-language-csharp,programming-language-powershell,programming-language-python"  
+ [Node. js](https://nodejs.org/), NPM için Windows tarafından gereklidir. Yalnızca [ETKIN LTS ve bakım LTS sürümleri](https://nodejs.org/about/releases/). Sürümünüzü denetlemek `node --version` için komutunu kullanın.
    MacOS ve Linux 'ta yerel geliştirme için gerekli değildir.   
::: zone-end  
::: zone pivot="programming-language-javascript,programming-language-typescript"  
+ [Node. js](https://nodejs.org/), ETKIN LTS ve bakım LTS sürümleri (10.14.1 önerilir). Sürümünüzü denetlemek `node --version` için komutunu kullanın.
::: zone-end 
::: zone pivot="programming-language-python"
+ Python [3,8](https://www.python.org/downloads/release/python-381/), [Python 3,7](https://www.python.org/downloads/release/python-375/), Python [3,6](https://www.python.org/downloads/release/python-368/) , Azure işlevleri (x64) tarafından desteklenir.
::: zone-end   
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Core SDK 2.2 +](https://www.microsoft.com/net/download)  
::: zone-end  
::: zone pivot="programming-language-java"  
+ [Java geliştirici seti](https://aka.ms/azure-jdks), sürüm 8.

+ [Apache Maven](https://maven.apache.org), sürüm 3,0 veya üzeri.
::: zone-end  
+ [Desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms)birine [Visual Studio Code](https://code.visualstudio.com/) .  
::: zone pivot="programming-language-csharp"  
+ Visual Studio Code için [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) .  
::: zone-end  
::: zone pivot="programming-language-python"
+ Visual Studio Code için [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python) .  
::: zone-end  
::: zone pivot="programming-language-powershell"
+ [Visual Studio Code Için PowerShell uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell). 
::: zone-end  
::: zone pivot="programming-language-java"  
+ [Java Uzantı paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
::: zone-end  

+ Visual Studio Code için [Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) . 

## <a name="create-your-local-project"></a><a name="create-an-azure-functions-project"></a>Yerel projenizi oluşturma 

Bu bölümde, seçtiğiniz dilde yerel bir Azure Işlevleri projesi oluşturmak için Visual Studio Code kullanırsınız. Bu makalenin ilerleyen kısımlarında, işlev kodunuzu Azure 'a yayımlayacaksınız. 

1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında **Yeni proje oluştur...** simgesini seçin.

    ![Yeni proje oluştur seçeneğini belirleyin](media/functions-create-first-function-vs-code/create-new-project.png)

1. Proje çalışma alanınız için bir dizin konumu seçin ve **Seç**' i seçin.

    > [!NOTE]
    > Bu adımlar, bir çalışma alanının dışında tamamlanacak şekilde tasarlandı. Bu örnekte, bir çalışma alanının parçası olan bir proje klasörünü seçmeyin.

1. İstemlere aşağıdaki bilgileri sağlayın:

    ::: zone pivot="programming-language-csharp"
    + **İşlev projeniz için bir dil seçin**: seçin `C#`.
    ::: zone-end
    ::: zone pivot="programming-language-javascript"
    + **İşlev projeniz için bir dil seçin**: seçin `JavaScript`.
    ::: zone-end
    ::: zone pivot="programming-language-typescript"
    + **İşlev projeniz için bir dil seçin**: seçin `TypeScript`.
    ::: zone-end
    ::: zone pivot="programming-language-powershell"
    + **İşlev projeniz için bir dil seçin**: seçin `PowerShell`.
    ::: zone-end
    ::: zone pivot="programming-language-python"
    + **İşlev projeniz için bir dil seçin**: seçin `Python`.

    + **Sanal ortam oluşturmak için bir Python diğer adı seçin**: Python yorumlayıcının konumunu seçin. Konum gösterilmemişse, Python ikilisinin tam yolunu yazın.  
    ::: zone-end

    ::: zone pivot="programming-language-java"  
    + **İşlev projeniz için bir dil seçin**: seçin `Java`.

    + **Bir grup kimliği sağlayın**: Seç `com.function`.

    + **YAPıT kimliği sağlayın**: seçin `myFunction`.

    + **Bir sürüm belirtin**: Seç `1.0-SNAPSHOT`.

    + **Bir paket adı belirtin**: Seç `com.function`.

    + **Bir uygulama adı belirtin**: Seç `myFunction-12345`.
    ::: zone-end  
    ::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"
    + **Projenizin ilk işlevi için bir şablon seçin**: seçin `HTTP trigger`.
    
    + **Bir işlev adı girin**: tür `HttpExample`.
    ::: zone-end  
    ::: zone pivot="programming-language-csharp"
    + **Ad alanı belirtin**: tür `My.Functions`. 
    ::: zone-end  
    ::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python"
    + **Yetkilendirme düzeyi**: herhangi `Anonymous`bir kişinin işlev uç noktanızı çağırmasını sağlayan öğesini seçin. Yetkilendirme düzeyi hakkında bilgi edinmek için bkz. [Yetkilendirme anahtarları](functions-bindings-http-webhook-trigger.md#authorization-keys).
    ::: zone-end  
    + **Projenizi nasıl açmak Istediğinizi seçin**: seçin `Add to workspace`.

1. Bu bilgileri kullanarak, Visual Studio Code HTTP tetikleyicisiyle bir Azure Işlevleri projesi oluşturur. Yerel proje dosyalarını Gezgin içinde görüntüleyebilirsiniz. Oluşturulan dosyalar hakkında daha fazla bilgi edinmek için bkz. [oluşturulan proje dosyaları](functions-develop-vs-code.md#generated-project-files). 

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-python,programming-language-java"

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

::: zone-end

::: zone pivot="programming-language-powershell"

[!INCLUDE [functions-run-function-test-local-vs-code-ps](../../includes/functions-run-function-test-local-vs-code-ps.md)]

::: zone-end

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi doğrudan Azure 'da yayımlamak için Visual Studio Code kullanma zamanı vardır. 

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="run-the-function-in-azure"></a>İşlevi Azure 'da çalıştırma

1. Yan çubuktaki **Azure: Functions** bölümüne geri döndüğünüzde, aboneliğinizin altındaki yeni işlev uygulamasını genişletin. **İşlevler**' i genişletin, **httpexample**üzerinde sağ tıklayın (Windows) veya CTRL + tıklama (MacOS) seçeneğini belirleyin ve ardından **işlev URL 'sini Kopyala**' yı seçin.

    ![Yeni HTTP tetikleyicisinin işlev URL 'sini kopyalayın](./media/functions-create-first-function-vs-code/function-copy-endpoint-url.png)

1. HTTP isteğinin bu URL 'sini tarayıcınızın adres çubuğuna yapıştırın, `name` sorgu DIZESINI `?name=Functions` bu URL 'nin sonuna ekleyin ve sonra isteği yürütün. HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

        http://<functionappname>.azurewebsites.net/api/httpexample?name=Functions 
        
    Aşağıdaki örnek, bu işlev tarafından döndürülen uzak GET isteğine tarayıcıda yapılan yanıtı gösterir: 

    ![Tarayıcıdaki işlev yanıtı](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki adıma devam ettiğinizde, [işleviniz için bir Azure depolama kuyruğu bağlaması ekleyin](functions-add-output-binding-storage-queue-vs-code.md), daha önce yapmış olduğunuz bir yerde oluşturmak için tüm kaynaklarınızı saklamanız gerekir.

Aksi takdirde, daha fazla maliyet ödemeden kaçınmak için işlev uygulamasını ve ilgili kaynaklarını silmek için aşağıdaki adımları kullanabilirsiniz.

[!INCLUDE [functions-cleanup-resources-vs-code.md](../../includes/functions-cleanup-resources-vs-code.md)]

Işlev maliyetleri hakkında daha fazla bilgi edinmek için bkz. [Tüketim planı maliyetlerini tahmin](functions-consumption-costs.md)etme.

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak bir işlev uygulaması oluşturmak için Visual Studio Code’u kullandınız. Sonraki makalede, bir çıkış bağlaması ekleyerek bu işlevi genişletmelisiniz. Bu bağlama, HTTP isteğinden dizeyi bir Azure kuyruk depolama kuyruğundaki bir iletiye yazar. 

> [!div class="nextstepaction"]
> [İşleviniz için bir Azure depolama kuyruğu bağlama ekleme](functions-add-output-binding-storage-queue-vs-code.md)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
