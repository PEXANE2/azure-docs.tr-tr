---
title: Visual Studio Code’u kullanarak Azure’da ilk işlevinizi oluşturma
description: Visual Studio Code’daki Azure İşlevleri uzantısını kullanarak basit bir HTTP ile tetiklenen işlev oluşturun ve Azure’da yayımlayın.
ms.topic: quickstart
ms.date: 01/10/2020
ms.custom: mvc, devcenter
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 0540c7b01d693975f34515c7d13f0477ac74d4a1
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842265"
---
# <a name="quickstart-create-an-azure-functions-project-using-visual-studio-code"></a>Hızlı başlangıç: Visual Studio Code kullanarak bir Azure Işlevleri projesi oluşturma

Bu makalede, HTTP isteklerine yanıt veren bir işlev oluşturmak için Visual Studio Code kullanırsınız. Kodu yerel olarak test ettikten sonra, Azure Işlevlerinin sunucusuz ortamına dağıtırsınız. Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur. 

Bu makalenin [CLI tabanlı bir sürümü](functions-create-first-azure-function-azure-cli.md) de vardır.

## <a name="prerequisites"></a>Ön koşullar

+ [Desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms)birine [Visual Studio Code](https://code.visualstudio.com/) . 
::: zone pivot="programming-language-csharp"
+ Visual Studio Code [ C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) .
::: zone-end
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node. js](https://nodejs.org/), ETKIN LTS ve bakım LTS sürümleri (8.11.1 ve 10.14.1 önerilir).
::: zone-end
::: zone pivot="programming-language-python"
+ Azure Işlevleri tarafından desteklenen [python 3,7](https://www.python.org/downloads/release/python-375/) veya [Python 3,6](https://www.python.org/downloads/release/python-368/). Python 3,8 henüz desteklenmiyor. 

+ Visual Studio Code için [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python) .
::: zone-end
::: zone pivot="programming-language-powershell"
+ [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Core SDK 2.2 +](https://www.microsoft.com/net/download)

+ [Visual Studio Code Için PowerShell uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell). 
::: zone-end

+ Visual Studio Code için [Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) . 

+ Etkin aboneliği olan bir [Azure hesabı](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) . Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

## <a name="create-an-azure-functions-project"></a>Yerel projenizi oluşturma 

Bu bölümde, seçtiğiniz dilde yerel bir Azure Işlevleri projesi oluşturmak için Visual Studio Code kullanırsınız. Bu makalenin ilerleyen kısımlarında, işlev kodunuzu Azure 'a yayımlayacaksınız. 

1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında **Yeni proje oluştur...** simgesini seçin.

    ![Yeni proje oluştur seçeneğini belirleyin](media/functions-create-first-function-vs-code/create-new-project.png)

1. Proje çalışma alanınız için bir dizin konumu seçin ve **Seç**' i seçin.

    > [!NOTE]
    > Bu adımlar, bir çalışma alanının dışında tamamlanacak şekilde tasarlandı. Bu örnekte, bir çalışma alanının parçası olan bir proje klasörünü seçmeyin.

1. İstemlere aşağıdaki bilgileri sağlayın:

    ::: zone pivot="programming-language-csharp"

    | İstem | Değer | 
    | ------ | ----- | 
    | İşlev projeniz için bir dil seçin | C# |
    | Sürüm seçin | Azure Işlevleri v2 | 
    | Projenizin ilk işlevi için bir şablon seçin | HTTP tetikleyicisi | 
    | Bir işlev adı girin | HttpExample | 
    | Ad alanı belirtin | My. Functions | 
    | Yetkilendirme düzeyi | Anonim | 
    | Projenizi nasıl açmak istediğinizi seçin | Çalışma alanına Ekle |

    ::: zone-end

    ::: zone pivot="programming-language-javascript"

    | İstem | Değer | 
    | ------ | ----- | 
    | İşlev projeniz için bir dil seçin | JavaScript | 
    | Sürüm seçin | Azure Işlevleri v2 | 
    | Projenizin ilk işlevi için bir şablon seçin | HTTP tetikleyicisi | 
    | Bir işlev adı girin | HttpExample | 
    | Yetkilendirme düzeyi | Anonim | 
    | Projenizi nasıl açmak istediğinizi seçin | Çalışma alanına Ekle |

    ::: zone-end

    ::: zone pivot="programming-language-typescript"

    | İstem | Değer | 
    | ------ | ----- | 
    | İşlev projeniz için bir dil seçin | TypeScript | 
    | Sürüm seçin | Azure Işlevleri v2 | 
    | Projenizin ilk işlevi için bir şablon seçin | HTTP tetikleyicisi | 
    | Bir işlev adı girin | HttpExample | 
    | Yetkilendirme düzeyi | Anonim | 
    | Projenizi nasıl açmak istediğinizi seçin | Çalışma alanına Ekle |

    ::: zone-end

    ::: zone pivot="programming-language-powershell"

    | İstem | Değer | 
    | ------ | ----- | 
    | İşlev projeniz için bir dil seçin | PowerShell | 
    | Sürüm seçin | Azure Işlevleri v2 | 
    | Projenizin ilk işlevi için bir şablon seçin | HTTP tetikleyicisi | 
    | Bir işlev adı girin | HttpExample | 
    | Yetkilendirme düzeyi | Anonim | 
    | Projenizi nasıl açmak istediğinizi seçin | Çalışma alanına Ekle |

    ::: zone-end

    ::: zone pivot="programming-language-python"

    | İstem | Değer | 
    | ------ | ----- | 
    | İşlev projeniz için bir dil seçin | Python | 
    | Sürüm seçin | Azure Işlevleri v2 | 
    | Sanal ortam oluşturmak için bir Python diğer adı seçin | Python diğer adı | 
    | Projenizin ilk işlevi için bir şablon seçin | HTTP tetikleyicisi | 
    | Bir işlev adı girin | HttpExample | 
    | Yetkilendirme düzeyi | Anonim | 
    | Projenizi nasıl açmak istediğinizi seçin | Çalışma alanına Ekle | 

    ::: zone-end

1. Şunları Visual Studio Code:

    + [Host. JSON](functions-host-json.md) ve [Local. Settings. JSON](functions-run-local.md#local-settings-file) yapılandırma dosyalarını içeren yeni bir çalışma alanında Azure işlevleri projesi oluşturur. 

    ::: zone pivot="programming-language-csharp"

    + İşlevi uygulayan bir [HttpExample.cs sınıf kitaplığı dosyası](functions-dotnet-class-library.md#functions-class-library-project) oluşturur.

    ::: zone-end
        
    ::: zone pivot="programming-language-javascript"
    
    + Kök klasörde bir Package. JSON dosyası oluşturur.

    + İşlev kodunu içeren bir Node. js dosyası olan [function. JSON tanım dosyasını](functions-reference-node.md#folder-structure) ve [index. js dosyasını](functions-reference-node.md#exporting-a-function)içeren bir httpexample klasörü oluşturur.
    
    ::: zone-end
    
    ::: zone pivot="programming-language-typescript"
    
    + Kök klasörde bir Package. JSON dosyası ve tsconfig. JSON dosyası oluşturur.

    + İşlev kodunu içeren bir TypeScript dosyası olan [function. JSON tanım dosyasını](functions-reference-node.md#folder-structure) ve [index. TS dosyasını](functions-reference-node.md#typescript)içeren bir httpexample klasörü oluşturur.
    
    ::: zone-end
    
    ::: zone pivot="programming-language-powershell"
    
    + Function [. JSON tanım dosyasını](functions-reference-python.md#programming-model) ve işlev kodunu içeren Run. ps1 dosyasını Içeren bir httpexample klasörü oluşturur.
    
    ::: zone-end
    
    ::: zone pivot="programming-language-python"
    
    + Işlevleri için gereken paketleri listeleyen proje düzeyi requirements. txt dosyası oluşturur.
    
    + Function [. JSON tanım dosyasını](functions-reference-python.md#programming-model) ve işlev kodunu içeren \_\_init\_\_. Kopyala dosyasını Içeren bir httpexample klasörü oluşturur.
    
    ::: zone-end

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

1. **Çıktı** panelinden HTTP tetikleyicisinin URL’sini kopyalayın. Yeniden, bu URL 'nin sonuna `?name=<yourname>` `name` sorgu dizesini ekleyin ve isteği yürütün.

    HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

        http://<functionappname>.azurewebsites.net/api/httpexample?name=<yourname> 

1. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki örnek, bu işlev tarafından döndürülen uzak GET isteğine tarayıcıda yapılan yanıtı gösterir: 

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
