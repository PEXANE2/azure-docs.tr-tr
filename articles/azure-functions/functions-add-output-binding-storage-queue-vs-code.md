---
title: Azure Işlevlerini Visual Studio Code kullanarak Azure depolama 'ya bağlama
description: Visual Studio Code projenize bir çıkış bağlaması ekleyerek Azure Işlevlerini bir Azure depolama kuyruğuna bağlamayı öğrenin.
ms.date: 02/07/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 22f7df52e90a35a3ed9a26a7672f8354efc173e3
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79241335"
---
# <a name="connect-azure-functions-to-azure-storage-using-visual-studio-code"></a>Azure Işlevlerini Visual Studio Code kullanarak Azure depolama 'ya bağlama

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

Bu makalede, [önceki hızlı başlangıç makalesinde](functions-create-first-function-vs-code.md) oluşturduğunuz Işlevi Azure Storage 'a bağlamak için Visual Studio Code nasıl kullanılacağı gösterilmektedir. Bu işleve eklediğiniz çıkış bağlaması, HTTP isteğinden verileri bir Azure kuyruk depolama kuyruğundaki bir iletiye yazar. 

Çoğu bağlamanın, bağlı hizmete erişmek için kullandığı depolanan bir bağlantı dizesi gerekir. Daha kolay hale getirmek için, işlev uygulamanız ile oluşturduğunuz depolama hesabını kullanırsınız. Bu hesap bağlantısı zaten `AzureWebJobsStorage`adlı bir uygulama ayarında depolanıyor.  

## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırma

Bu makaleye başlamadan önce, aşağıdaki gereksinimleri karşılamanız gerekir:

* [Visual Studio Code Için Azure Storage uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage)yükler.

* [Azure Depolama Gezgini](https://storageexplorer.com/)'i yükler. Depolama Gezgini, çıkış bağlamalarınız tarafından oluşturulan sıra iletilerini incelemek için kullanacağınız bir araçtır. Depolama Gezgini, macOS, Windows ve Linux tabanlı işletim sistemlerinde desteklenir.

::: zone pivot="programming-language-csharp"
* [.NET Core CLI araçları](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)'nı yükler.
::: zone-end

* [Visual Studio Code hızlı başlangıcı ' nın 1. bölümündeki](functions-create-first-function-vs-code.md)adımları uygulayın. 

Bu makalede, Visual Studio Code Azure aboneliğinizde zaten oturum açmış olduğunuz varsayılmaktadır. Komut paletinden `Azure: Sign In` çalıştırarak oturum açabilirsiniz. 

## <a name="download-the-function-app-settings"></a>İşlev uygulaması ayarlarını indirin

[Önceki hızlı başlangıç makalesinde](functions-create-first-function-vs-code.md), Azure 'Da gerekli depolama hesabıyla birlikte bir işlev uygulaması oluşturdunuz. Bu hesabın bağlantı dizesi, Azure 'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki bir depolama kuyruğuna iletiler yazarsınız. İşlevi yerel olarak çalıştırırken depolama hesabınıza bağlanmak için, uygulama ayarlarını yerel. Settings. JSON dosyasına indirmeniz gerekir. 

1. F1 tuşuna basarak komut paletini açın ve ardından `Azure Functions: Download Remote Settings....`komutu arayıp çalıştırın. 

1. Önceki makalede oluşturduğunuz işlev uygulamasını seçin. Var olan yerel ayarların üzerine yazmak için **Tümüne Evet** ' i seçin. 

    > [!IMPORTANT]  
    > Gizli dizi içerdiğinden, Local. Settings. JSON dosyası hiçbir şekilde yayımlanmaz ve kaynak denetiminden çıkarılır.

1. Depolama hesabı bağlantı dizesi değeri için anahtar olan `AzureWebJobsStorage`değerini kopyalayın. Çıkış bağlamasının beklendiği gibi çalıştığını doğrulamak için bu bağlantıyı kullanın.

## <a name="register-binding-extensions"></a>Bağlama uzantılarını kaydetme

Kuyruk depolama çıkış bağlaması kullandığınızdan, projeyi çalıştırmadan önce depolama bağlamaları uzantısının yüklü olması gerekir. 

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

::: zone-end

::: zone pivot="programming-language-csharp"

HTTP ve Zamanlayıcı Tetikleyicileri dışında, bağlamalar uzantı paketleri olarak uygulanır. Depolama uzantısı paketini projenize eklemek için Terminal penceresinde aşağıdaki [DotNet paket Ekle](/dotnet/core/tools/dotnet-add-package) komutunu çalıştırın.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

::: zone-end

Şimdi, depolama çıkış bağlamasını projenize ekleyebilirsiniz.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

Işlevlerde, her bağlama türü, function. json dosyasında tanımlanması için `direction`, `type`ve benzersiz bir `name` gerektirir. Bu öznitelikleri tanımlama yöntemi, işlev uygulamanızın diline bağlıdır.

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"

[!INCLUDE [functions-add-output-binding-json](../../includes/functions-add-output-binding-json.md)]

::: zone-end

::: zone pivot="programming-language-csharp"

[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]

::: zone-end

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlaması kullanan kod ekleme

Bağlama tanımlandıktan sonra, işlev imzasında bir öznitelik olarak erişmek için bağlamanın `name` kullanabilirsiniz. Bir çıkış bağlaması kullanarak kimlik doğrulaması, kuyruk başvurusu alma veya veri yazma için Azure depolama SDK kodunu kullanmanız gerekmez. Işlevler çalışma zamanı ve kuyruk çıkış bağlaması bu görevleri sizin için işler.

::: zone pivot="programming-language-javascript"  
[!INCLUDE [functions-add-output-binding-js](../../includes/functions-add-output-binding-js.md)]
::: zone-end  

::: zone pivot="programming-language-typescript"  
[!INCLUDE [functions-add-output-binding-ts](../../includes/functions-add-output-binding-ts.md)]
::: zone-end  

::: zone pivot="programming-language-powershell"

[!INCLUDE [functions-add-output-binding-powershell](../../includes/functions-add-output-binding-powershell.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [functions-add-output-binding-python](../../includes/functions-add-output-binding-python.md)]

::: zone-end

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]
::: zone-end  

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-python"

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

::: zone-end

::: zone pivot="programming-language-powershell"

[!INCLUDE [functions-run-function-test-local-vs-code-ps](../../includes/functions-run-function-test-local-vs-code-ps.md)]

::: zone-end

Çıkış bağlaması ilk kullanıldığında, depolama hesabınızda Işlevler çalışma zamanına göre **outqueue** adlı yeni bir kuyruk oluşturulur. Depolama Gezgini, sıranın yeni iletiyle birlikte oluşturulduğunu doğrulamak için kullanacaksınız.

### <a name="connect-storage-explorer-to-your-account"></a>Depolama Gezgini’ni hesabınıza bağlama

Azure Depolama Gezgini zaten yüklediyseniz ve Azure hesabınıza bağladıysanız bu bölümü atlayın.

1. [Azure Depolama Gezgini] aracını çalıştırın, sol taraftaki Bağlan simgesini seçin ve **Hesap Ekle**' yi seçin.

    ![Microsoft Azure Depolama Gezgini bir Azure hesabı ekleyin](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-add-account.png)

1. **Bağlan** iletişim kutusunda, **Azure hesabı ekle**' yi seçin, **Azure ortamınızı**seçin ve oturum aç ' ı seçin. **..** 

    ![Azure hesabınızda oturum açma](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-connect-azure-account.png)

Hesabınızda başarıyla oturum açtıktan sonra hesabınızla ilişkili tüm Azure aboneliklerini görürsünüz.

### <a name="examine-the-output-queue"></a>Çıkış kuyruğunu inceleme

1. Visual Studio Code, F1 tuşuna basarak komut paletini açın, sonra komut `Azure Storage: Open in Storage Explorer` arayıp çalıştırın ve depolama hesabınızın adını seçin. Depolama Hesabınız Azure Depolama Gezgini açılır.  

1. **Kuyruklar** düğümünü genişletin ve sonra **outqueue** adlı kuyruğu seçin. 

   Kuyruk, HTTP ile tetiklenen işlevi çalıştırdığınızda kuyruk çıkış bağlamasının oluşturduğu iletiyi içerir. İşlevi varsayılan `name`*Azure* değeri ile çağırdıysanız, kuyruk iletisi *İşleve geçirilen ad: Azure* şeklinde olur.

    ![Azure Depolama Gezgini gösterilen kuyruk iletisi](./media/functions-add-output-binding-storage-queue-vs-code/function-queue-storage-output-view-queue.png)

1. İşlevi yeniden çalıştırın, başka bir istek gönderin ve kuyrukta yeni bir ileti göründüğünü görürsünüz.  

Şimdi, güncelleştirilmiş işlev uygulamasını Azure 'da yeniden yayımlamanız zaman atalım.

## <a name="redeploy-and-verify-the-updated-app"></a>Güncelleştirilmiş uygulamayı yeniden dağıtın ve doğrulayın

1. Visual Studio Code ' de, F1 tuşuna basarak komut paletini açın. Komut paletinde `Azure Functions: Deploy to function app...`arayıp seçin.

1. İlk makalede oluşturduğunuz işlev uygulamasını seçin. Projenizi aynı uygulamaya yeniden dağıtmakta olduğunuzdan, dosyaların üzerine yazma uyarısını kapatmak için **Dağıt** ' ı seçin.

1. Dağıtım tamamlandıktan sonra yeniden dağıtılan işlevi test etmek için kıvrımlı veya bir tarayıcı kullanabilirsiniz. Daha önce olduğu gibi, aşağıdaki örnekte olduğu gibi sorgu dizesini URL 'ye `&name=<yourname>` ekleyin:

    ```bash
    curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
    ```

1. Çıktı bağlamasının sırada yeni bir ileti oluşturmadığını doğrulamak için, [depolama sırasındaki iletiyi yeniden görüntüleyin](#examine-the-output-queue) .

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure’da *Kaynaklar*; işlev uygulamalarını, işlevleri, depolama hesaplarını ve benzeri öğeleri ifade eder. *Kaynak grupları* halinde gruplandırılırlar ve bir grubu silerek içindeki her şeyi silebilirsiniz.

Bu hızlı başlangıçları tamamlamak için kaynaklar oluşturdunuz. [Hesap durumunuza](https://azure.microsoft.com/account/) ve [hizmet fiyatlandırmanıza](https://azure.microsoft.com/pricing/) bağlı olarak size bu kaynakların ücretleri yansıtılabilir. Kaynaklara artık ihtiyacınız yoksa, şunları yaparak silebilirsiniz:

[!INCLUDE [functions-cleanup-resources-vs-code.md](../../includes/functions-cleanup-resources-vs-code.md)]

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen işlevinizi bir depolama kuyruğuna veri yazmak için güncelleştirdiniz. Artık Visual Studio Code kullanarak Işlevleri geliştirme hakkında daha fazla bilgi edinebilirsiniz:

+ [Visual Studio Code kullanarak Azure Işlevleri geliştirme](functions-develop-vs-code.md)
::: zone pivot="programming-language-csharp"  
+ [İçindeki C#tüm işlev projelerinin örnekleri ](/samples/browse/?products=azure-functions&languages=csharp).
+ [Azure Işlevleri C# geliştirici başvurusu](functions-dotnet-class-library.md)  
::: zone-end 
::: zone pivot="programming-language-javascript"  
+ [JavaScript 'teki bütün işlev projelerinin örnekleri](/samples/browse/?products=azure-functions&languages=javascript).
+ [Azure Işlevleri JavaScript Geliştirici Kılavuzu](functions-reference-node.md)  
::: zone-end  
::: zone pivot="programming-language-typescript"  
+ [TypeScript 'teki bütün işlev projelerinin örnekleri](/samples/browse/?products=azure-functions&languages=typescript).
+ [Azure Işlevleri TypeScript Geliştirici Kılavuzu](functions-reference-node.md#typescript)  
::: zone-end  
::: zone pivot="programming-language-python"  
+ [Python 'da tüm işlev projelerinin örnekleri](/samples/browse/?products=azure-functions&languages=python).
+ [Azure Işlevleri Python Geliştirici Kılavuzu](functions-reference-python.md)  
::: zone-end  
::: zone pivot="programming-language-powershell"  
+ [PowerShell 'de tüm işlev projelerinin örnekleri](/samples/browse/?products=azure-functions&languages=azurepowershell).
+ [Azure Işlevleri PowerShell Geliştirici Kılavuzu](functions-reference-powershell.md) 
::: zone-end
+ [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md).
+ [İşlevler fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/)
+ [Tüketim planı maliyetlerini tahmin](functions-consumption-costs.md) etme makalesi.
