---
title: Visual Studio Kodunu kullanarak Azure Fonksiyonlarını Azure Depolama'ya bağlayın
description: Visual Studio Code projenize bir çıktı bağlama ekleyerek Azure İşlevlerini Azure Depolama kuyruğuna nasıl bağlayabilirsiniz öğrenin.
ms.date: 02/07/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 22f7df52e90a35a3ed9a26a7672f8354efc173e3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241335"
---
# <a name="connect-azure-functions-to-azure-storage-using-visual-studio-code"></a>Visual Studio Kodunu kullanarak Azure Fonksiyonlarını Azure Depolama'ya bağlayın

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

Bu makalede, [önceki hızlı başlangıç makalesinde](functions-create-first-function-vs-code.md) oluşturduğunuz işlevi Azure Depolama'ya bağlamak için Visual Studio Code'u nasıl kullanacağınızı gösterir. Bu işleve eklediğiniz çıktı bağlama, HTTP isteğindeki verileri Azure Kuyruk depolama kuyruğundaki iletiye yazar. 

Çoğu bağlama, Bağlı hizmete erişmek için Işlevlerin kullandığı depolanmış bir bağlantı dizesi gerektirir. Bunu kolaylaştırmak için, işlev uygulamanızla oluşturduğunuz Depolama hesabını kullanırsınız. Bu hesaba bağlantı zaten adlı `AzureWebJobsStorage`bir uygulama ayarında depolanır.  

## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırın

Bu makaleye başlamadan önce aşağıdaki gereksinimleri karşılamanız gerekir:

* Visual [Studio Code için Azure Depolama uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage)yükleyin.

* [Azure Depolama Gezgini'ni](https://storageexplorer.com/)yükleyin. Depolama Gezgini, çıktı bağlamanız tarafından oluşturulan sıra iletilerini incelemek için kullanacağınız bir araçtır. Depolama Gezgini macOS, Windows ve Linux tabanlı işletim sistemlerinde desteklenir.

::: zone pivot="programming-language-csharp"
* [.NET Core CLI araçlarını](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)yükleyin.
::: zone-end

* Visual Studio [Code quickstart bölüm 1](functions-create-first-function-vs-code.md)adımları tamamlayın. 

Bu makalede, Visual Studio Code'tan Azure aboneliğinizde zaten oturum açmış olduğunuz varsayar. Komut paletinden `Azure: Sign In` koşarak oturum açabilirsiniz. 

## <a name="download-the-function-app-settings"></a>Fonksiyon uygulama ayarlarını indirin

Önceki [hızlı başlangıç makalesinde,](functions-create-first-function-vs-code.md)gerekli Depolama hesabıyla birlikte Azure'da bir işlev uygulaması oluşturdunuz. Bu hesabın bağlantı dizesi Azure'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki depolama kuyruğuna iletiler yazarsınız. İşlevi yerel olarak çalıştırırken Depolama hesabınıza bağlanmak için uygulama ayarlarını local.settings.json dosyasına indirmeniz gerekir. 

1. Komut paletini açmak için F1 tuşuna basın, `Azure Functions: Download Remote Settings....`ardından aramayı ve komutu çalıştırın. 

1. Önceki makalede oluşturduğunuz işlev uygulamasını seçin. Varolan yerel ayarların üzerine yazmak için **tümüne Evet'i** seçin. 

    > [!IMPORTANT]  
    > Sırlar içerdiğinden, local.settings.json dosyası asla yayınlanmaz ve kaynak denetiminin dışında tutulur.

1. Depolama hesabı `AzureWebJobsStorage`bağlantı dizesi değerinin anahtarı olan değeri kopyalayın. Çıktı bağlamanın beklendiği gibi çalıştığını doğrulamak için bu bağlantıyı kullanırsınız.

## <a name="register-binding-extensions"></a>Bağlama uzantılarını kaydetme

Sıra depolama çıktısı bağlama kullanıyorsanız, projeyi çalıştırmadan önce Depolama bağlayıcıları uzantısıyüklü olmalıdır. 

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

::: zone-end

::: zone pivot="programming-language-csharp"

HTTP ve zamanlayıcı tetikleyicileri dışında, bağlamalar uzantı paketleri olarak uygulanır. Projenize Depolama uzantısı paketini eklemek için Terminal penceresinde aşağıdaki [dotnet ekle paket](/dotnet/core/tools/dotnet-add-package) komutunu çalıştırın.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

::: zone-end

Artık, depolama çıktısını projenize bağlayabilirsiniz.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

İşlevler'de, her `direction`bağlama `type`türü, `name` function.json dosyasında tanımlanması gereken bir , ve benzersiz bir bağlama gerektirir. Bu öznitelikleri tanımlama şekliniz işlev uygulamanızın diline bağlıdır.

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell"

[!INCLUDE [functions-add-output-binding-json](../../includes/functions-add-output-binding-json.md)]

::: zone-end

::: zone pivot="programming-language-csharp"

[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]

::: zone-end

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlaması kullanan kod ekleme

Bağlama tanımlandıktan sonra, işlev `name` imzasında bir öznitelik olarak erişmek için bağlama kullanabilirsiniz. Çıktı bağlama kullanarak, kimlik doğrulama, sıra başvurusu alma veya veri yazmak için Azure Depolama SDK kodunu kullanmanız gerekmez. İşlevler çalışma zamanı ve sıra çıktısı bağlama bu görevleri sizin için yapar.

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

Çıktı bağlama ilk kullanıldığında, Işlevler çalışma zamanı tarafından depolama hesabınızda **outqueue** adlı yeni bir kuyruk oluşturulur. Sıranın yeni iletiyle birlikte oluşturulduğunu doğrulamak için Depolama Gezgini'ni kullanırsınız.

### <a name="connect-storage-explorer-to-your-account"></a>Depolama Gezgini’ni hesabınıza bağlama

Azure Depolama Gezgini'ni zaten yüklediyseniz ve Azure hesabınıza bağladıysanız bu bölümü atlayın.

1. [Azure Depolama Gezgini] aracını çalıştırın, soldaki bağlantı simgesini seçin ve **hesap ekle'yi**seçin.

    ![Microsoft Azure Depolama Gezgini'ne Azure hesabı ekleme](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-add-account.png)

1. **Bağlan** iletişim kutusunda, **Azure hesabı**ekle'yi, **Azure ortamınızı**seçin ve **Oturum Aç'ı seçin...** seçeneğini belirleyin. 

    ![Azure hesabınızda oturum açma](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-connect-azure-account.png)

Hesabınızda başarılı bir şekilde oturum imzaladıktan sonra, hesabınızla ilişkili tüm Azure aboneliklerini görürsünüz.

### <a name="examine-the-output-queue"></a>Çıkış kuyruğunu inceleme

1. Visual Studio Code'da komut paletini açmak için F1 tuşuna `Azure Storage: Open in Storage Explorer` basın, ardından komutu arayın ve çalıştırın ve Depolama hesabı adınızı seçin. Depolama hesabınız Azure Depolama Gezgini'nde açılır.  

1. **Kuyruklar** düğümünü genişletin ve sonra **outqueue** adlı kuyruğu seçin. 

   Kuyruk, HTTP ile tetiklenen işlevi çalıştırdığınızda kuyruk çıkış bağlamasının oluşturduğu iletiyi içerir. İşlevi varsayılan `name`*Azure* değeri ile çağırdıysanız, kuyruk iletisi *İşleve geçirilen ad: Azure* şeklinde olur.

    ![Azure Depolama Gezgini'nde gösterilen sıra iletisi](./media/functions-add-output-binding-storage-queue-vs-code/function-queue-storage-output-view-queue.png)

1. İşlevi yeniden çalıştırın, başka bir istek gönderin ve kuyrukta yeni bir ileti nin göründüğünü görürsünüz.  

Şimdi, güncelleştirilmiş işlev uygulamasını Azure'da yeniden yayımlama zamanı.

## <a name="redeploy-and-verify-the-updated-app"></a>Güncellenen uygulamayı yeniden dağıtın ve doğrulayın

1. Visual Studio Code'da komut paletini açmak için F1 tuşuna basın. Komut paletinde, 'yi `Azure Functions: Deploy to function app...`arayın ve seçin.

1. İlk makalede oluşturduğunuz işlev uygulamasını seçin. Projenizi aynı uygulamaya yeniden dağıttığınızdan, dosyaların üzerine yazma yla ilgili uyarıyı kapatmak için **Dağıt'ı** seçin.

1. Dağıtım tamamlandıktan sonra, yeniden dağıtılan işlevi test etmek için cURL'yi veya tarayıcıyı yeniden kullanabilirsiniz. Daha önce olduğu gibi, `&name=<yourname>` aşağıdaki örnekte olduğu gibi, sorgu dizesini URL'ye ekleyin:

    ```bash
    curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
    ```

1. Çıktı bağlamanın kuyrukta yeni bir ileti oluşturduğunu doğrulamak için [depolama kuyruğundaki iletiyi](#examine-the-output-queue) yeniden görüntüleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure’da *Kaynaklar*; işlev uygulamalarını, işlevleri, depolama hesaplarını ve benzeri öğeleri ifade eder. *Kaynak grupları* halinde gruplandırılırlar ve bir grubu silerek içindeki her şeyi silebilirsiniz.

Bu hızlı başlangıçları tamamlamak için kaynaklar oluşturdunuz. [Hesap durumunuza](https://azure.microsoft.com/account/) ve [hizmet fiyatlandırmanıza](https://azure.microsoft.com/pricing/) bağlı olarak size bu kaynakların ücretleri yansıtılabilir. Kaynaklara artık ihtiyacınız yoksa, şunları yaparak silebilirsiniz:

[!INCLUDE [functions-cleanup-resources-vs-code.md](../../includes/functions-cleanup-resources-vs-code.md)]

## <a name="next-steps"></a>Sonraki adımlar

Depolama kuyruğuna veri yazmak için HTTP tetiklenen işlevinizi güncellediniz. Artık Visual Studio Code'u kullanarak Fonksiyonlar geliştirme hakkında daha fazla bilgi edinebilirsiniz:

+ [Visual Studio Kodunu Kullanarak Azure Fonksiyonları Geliştirme](functions-develop-vs-code.md)
::: zone pivot="programming-language-csharp"  
+ [C# 'daki komple Fonksiyon projelerine örnekler.](/samples/browse/?products=azure-functions&languages=csharp)
+ [Azure İşlevler C# geliştirici başvurusu](functions-dotnet-class-library.md)  
::: zone-end 
::: zone pivot="programming-language-javascript"  
+ [JavaScript'teki tam Fonksiyon projelerine örnekler.](/samples/browse/?products=azure-functions&languages=javascript)
+ [Azure İşlevler JavaScript geliştirici kılavuzu](functions-reference-node.md)  
::: zone-end  
::: zone pivot="programming-language-typescript"  
+ [TypeScript'teki komple Fonksiyon projelerine örnekler.](/samples/browse/?products=azure-functions&languages=typescript)
+ [Azure İşlevler TypeScript geliştirici kılavuzu](functions-reference-node.md#typescript)  
::: zone-end  
::: zone pivot="programming-language-python"  
+ [Python'daki komple Fonksiyon projelerine örnekler.](/samples/browse/?products=azure-functions&languages=python)
+ [Azure İşlevler Python geliştirici kılavuzu](functions-reference-python.md)  
::: zone-end  
::: zone pivot="programming-language-powershell"  
+ [PowerShell'deki komple Fonksiyon projelerine örnekler.](/samples/browse/?products=azure-functions&languages=azurepowershell)
+ [Azure İşgücü Shell geliştirici kılavuzu](functions-reference-powershell.md) 
::: zone-end
+ [Azure İşlevleri tetikler ve bağlamalar.](functions-triggers-bindings.md)
+ [Fonksiyonlar fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/)
+ [Tüketim planı maliyetlerinin tahmini](functions-consumption-costs.md) makalesi.
