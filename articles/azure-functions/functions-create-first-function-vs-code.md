---
title: Visual Studio Code’u kullanarak Azure’da ilk işlevinizi oluşturma
description: Visual Studio Code’daki Azure İşlevleri uzantısını kullanarak basit bir HTTP ile tetiklenen işlev oluşturun ve Azure’da yayımlayın.
ms.topic: quickstart
ms.date: 06/25/2019
ms.custom: mvc, devcenter
ms.openlocfilehash: b89e6d75cd82a65dfbce29b949c4b7d463582bb9
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230790"
---
# <a name="create-your-first-function-using-visual-studio-code"></a>Visual Studio Code kullanarak ilk işlevinizi oluşturma

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir ortamda yürütmenize olanak tanır.

Bu makalede Microsoft Visual Studio Code’u kullanarak yerel bilgisayarınızda bir “merhaba dünya” işlevi oluşturup test etmek üzere [Visual Studio Code için Azure İşlevleri uzantısı] kullanmayı öğreneceksiniz. Ardından işlev kodunu Visual Studio Code ile Azure’da yayımlayacaksınız.

![Visual Studio projesinde Azure İşlevleri kodu](./media/functions-create-first-function-vs-code/functions-vscode-intro.png)

The extension currently supports C#, JavaScript, Java, and Python functions. The steps in this article and the article that follows support only JavaScript and C# functions. To learn how to use Visual Studio Code to create and publish Python functions, see [Create and deploy serverless Azure Functions in Python with Visual Studio Code](/azure/python/tutorial-vs-code-serverless-python-01). To learn how to use Visual Studio Code to create and publish PowerShell functions, see [Create your first PowerShell function in Azure](functions-create-first-function-powershell.md). 

Uzantı şu an önizleme aşamasındadır. Daha fazla bilgi edinmek için [Visual Studio Code için Azure İşlevleri uzantısı] sayfasına bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için:

* [Desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms) birinde [Visual Studio Code](https://code.visualstudio.com/)’u yükleyin.

* Install version 2.x of the [Azure Functions Core Tools](functions-run-local.md#v2).

* Seçtiğiniz dile özgü gereksinimleri yükleyin:

    | Dil | Gereksinim |
    | -------- | --------- |
    | **C#** | [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)  |
    | **JavaScript** | [Node.js](https://nodejs.org/)<sup>*</sup> | 
 
    <sup>*</sup>Active LTS and Maintenance LTS versions (8.11.1 and 10.14.1 recommended).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

[!INCLUDE [functions-create-function-app-vs-code](../../includes/functions-create-function-app-vs-code.md)]

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="run-the-function-in-azure"></a>Run the function in Azure

1. **Çıktı** panelinden HTTP tetikleyicisinin URL’sini kopyalayın. This URL includes the function key, which is passed to the `code` query parameter. Daha önce olduğu gibi, `?name=<yourname>` sorgu dizesini bu URL’nin sonuna ekleyip isteği yürütmeyi unutmayın.

    HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

        http://<functionappname>.azurewebsites.net/api/<functionname>?code=<function_key>&name=<yourname> 

1. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. İşlevin döndürdüğü uzak GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir: 

    ![Tarayıcıdaki işlev yanıtı](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak bir işlev uygulaması oluşturmak için Visual Studio Code’u kullandınız. In the next article, you expand that function by adding an output binding. This binding writes the string from the HTTP request to a message in an Azure Queue Storage queue. The next article also shows you how to clean up these new Azure resources by removing the resource group you created.

> [!div class="nextstepaction"]
> [Add an Azure Storage queue binding to your function](functions-add-output-binding-storage-queue-vs-code.md)

[Azure Functions Core Tools]: functions-run-local.md
[Visual Studio Code için Azure İşlevleri uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
