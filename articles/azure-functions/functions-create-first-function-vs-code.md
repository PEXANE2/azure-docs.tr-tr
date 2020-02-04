---
title: Visual Studio Code’u kullanarak Azure’da ilk işlevinizi oluşturma
description: Visual Studio Code’daki Azure İşlevleri uzantısını kullanarak basit bir HTTP ile tetiklenen işlev oluşturun ve Azure’da yayımlayın.
ms.topic: quickstart
ms.date: 01/10/2020
ms.custom: mvc, devcenter
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 26313c68305f4d7e6411d31fa12366442ce4bd38
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964176"
---
# <a name="quickstart-create-an-azure-functions-project-using-visual-studio-code"></a>Hızlı başlangıç: Visual Studio Code kullanarak bir Azure Işlevleri projesi oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir işlev oluşturmak için Visual Studio Code kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur. 

Bu makalenin [CLI tabanlı bir sürümü](functions-create-first-azure-function-azure-cli.md) de vardır.

## <a name="configure-your-environment"></a>Ortamınızı yapılandırma

Başlamadan önce, aşağıdaki gereksinimlerin yerinde olduğundan emin olun:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

::: zone pivot="programming-language-csharp,programming-language-powershell,programming-language-python"  
+ [Node. js](https://nodejs.org/), NPM için Windows tarafından gereklidir. Yalnızca [ETKIN LTS ve bakım LTS sürümleri ](https://nodejs.org/about/releases/). Sürümünüzü denetlemek için `npm --version` komutunu kullanın.
    MacOS ve Linux 'ta yerel geliştirme için gerekli değildir.   
::: zone-end  
::: zone pivot="programming-language-javascript,programming-language-typescript"  
+ [Node. js](https://nodejs.org/), ETKIN LTS ve bakım LTS sürümleri (10.14.1 önerilir). Sürümünüzü denetlemek için `npm --version` komutunu kullanın.
::: zone-end 
::: zone pivot="programming-language-python"
+ Azure Işlevleri tarafından desteklenen [python 3,7](https://www.python.org/downloads/release/python-375/) veya [Python 3,6](https://www.python.org/downloads/release/python-368/). Python 3,8 henüz desteklenmiyor. 
::: zone-end   
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Core SDK 2.2 +](https://www.microsoft.com/net/download)  
::: zone-end  
+ [Desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms)birine [Visual Studio Code](https://code.visualstudio.com/) .  
::: zone pivot="programming-language-csharp"  
+ Visual Studio Code [ C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) .  
::: zone-end  
::: zone pivot="programming-language-python"
+ Visual Studio Code için [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python) .  
::: zone-end  
::: zone pivot="programming-language-powershell"
+ [Visual Studio Code Için PowerShell uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell).  
::: zone-end  

+ Visual Studio Code için [Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) . 

## <a name="create-an-azure-functions-project"></a>Yerel projenizi oluşturma 

Bu bölümde, seçtiğiniz dilde yerel bir Azure Işlevleri projesi oluşturmak için Visual Studio Code kullanırsınız. Bu makalenin ilerleyen kısımlarında, işlev kodunuzu Azure 'a yayımlayacaksınız. 

1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında **Yeni proje oluştur...** simgesini seçin.

    ![Yeni proje oluştur seçeneğini belirleyin](media/functions-create-first-function-vs-code/create-new-project.png)

1. Proje çalışma alanınız için bir dizin konumu seçin ve **Seç**' i seçin.

    > [!NOTE]
    > Bu adımlar, bir çalışma alanının dışında tamamlanacak şekilde tasarlandı. Bu örnekte, bir çalışma alanının parçası olan bir proje klasörünü seçmeyin.

1. İstemlere aşağıdaki bilgileri sağlayın:

    ::: zone pivot="programming-language-csharp"
    + **İşlev projeniz için bir dil seçin**: `C#`seçin.
    ::: zone-end
    ::: zone pivot="programming-language-javascript"
    + **İşlev projeniz için bir dil seçin**: `JavaScript`seçin.
    ::: zone-end
    ::: zone pivot="programming-language-typescript"
    + **İşlev projeniz için bir dil seçin**: `TypeScript`seçin.
    ::: zone-end
    ::: zone pivot="programming-language-powershell"
    + **İşlev projeniz için bir dil seçin**: `PowerShell`seçin.
    ::: zone-end
    ::: zone pivot="programming-language-python"
    + **İşlev projeniz için bir dil seçin**: `Python`seçin.

    + **Sanal ortam oluşturmak için bir Python diğer adı seçin**: Python yorumlayıcının konumunu seçin. Konum gösterilmemişse, Python ikilisinin tam yolunu yazın.  
    ::: zone-end

    + **Projenizin ilk işlevi için bir şablon seçin**: `HTTP trigger`seçin.
    
    + **Bir işlev adı girin**: tür `HttpExample`.
    
    ::: zone pivot="programming-language-csharp"
    + **Ad alanı girin**: tür `My.Functions`. 
    ::: zone-end

    + **Yetkilendirme düzeyi**: herkesin işlev uç noktanızı çağırmasını sağlayan `Anonymous`seçin. Yetkilendirme düzeyi hakkında bilgi edinmek için bkz. [Yetkilendirme anahtarları](functions-bindings-http-webhook.md#authorization-keys).

    + **Projenizi nasıl açmak Istediğinizi seçin**: `Add to workspace`seçin.

1. Bu bilgileri kullanarak, Visual Studio Code HTTP tetikleyicisiyle bir Azure Işlevleri projesi oluşturur. Yerel proje dosyalarını Gezgin içinde görüntüleyebilirsiniz. Oluşturulan dosyalar hakkında daha fazla bilgi edinmek için bkz. [oluşturulan proje dosyaları](functions-develop-vs-code.md#generated-project-files). 

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-python"

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

1. HTTP isteğinin bu URL 'sini tarayıcınızın adres çubuğuna yapıştırın, `name` sorgu dizesini bu URL 'nin sonuna `?name=Functions` olarak ekleyin ve sonra isteği yürütün. HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

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
