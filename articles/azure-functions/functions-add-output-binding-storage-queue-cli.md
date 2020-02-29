---
title: Komut satırı araçlarını kullanarak Azure Işlevlerini Azure depolama 'ya bağlama
description: Komut satırı projenize bir çıkış bağlaması ekleyerek Azure Işlevlerini bir Azure depolama kuyruğuna bağlamayı öğrenin.
ms.date: 02/07/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: e3c37b368b723cc95302949baa8e85e2a8b621be
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78201930"
---
# <a name="connect-azure-functions-to-azure-storage-using-command-line-tools"></a>Komut satırı araçlarını kullanarak Azure Işlevlerini Azure depolama 'ya bağlama

Bu makalede, bir Azure depolama kuyruğunu [önceki hızlı](functions-create-first-azure-function-azure-cli.md)başlangıçta oluşturduğunuz işlev ve depolama hesabıyla tümleştirmeniz gerekir. Bu tümleştirmeyi, HTTP isteğinden kuyruktaki bir iletiye veri yazan bir *Çıkış bağlaması* kullanarak elde edersiniz. Bu makalenin tamamlanması, önceki hızlı başlangıç için birkaç USD 'nin ötesinde ek maliyet yoktur. Bağlamalar hakkında daha fazla bilgi edinmek için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırma

Başlamadan önce, [komut satırından hızlı başlangıç: Azure işlevleri projesi oluşturma](functions-create-first-azure-function-azure-cli.md)makalesini doldurmanız gerekir. Bu makalenin sonunda kaynakları zaten temizlediğinizde, işlev uygulamasını ve ilgili kaynakları Azure 'da yeniden oluşturmak için adımlara tekrar gidin.

## <a name="retrieve-the-azure-storage-connection-string"></a>Azure depolama bağlantı dizesini alma

Önceki hızlı başlangıçta Azure 'da bir işlev uygulaması oluşturduğunuzda, bir depolama hesabı da oluşturmuş olursunuz. Bu hesabın bağlantı dizesi, Azure 'daki uygulama ayarlarında güvenli bir şekilde depolanır. Bu ayarı *Local. Settings. JSON* dosyasına indirerek bu bağlantıyı, işlevi yerel olarak çalıştırırken aynı hesaptaki bir depolama kuyruğuna yazma işlemi kullanabilirsiniz. 

1. Projenin kökünden, aşağıdaki komutu çalıştırarak `<APP_NAME>` önceki hızlı başlangıçta işlev uygulamanızın adıyla değiştirin. Bu komut, dosyadaki varolan tüm değerlerin üzerine yazar.

    ```
    func azure functionapp fetch-app-settings <APP_NAME>
    ```
    
1. *Local. Settings. JSON* ' ı açın ve depolama hesabı bağlantı dizesi olan `AzureWebJobsStorage`adlı değeri bulun. Ad `AzureWebJobsStorage` ve bağlantı dizesini Bu makalenin diğer bölümlerinde kullanırsınız.

> [!IMPORTANT]
> *Local. Settings. JSON* , Azure 'dan indirilen gizli dizileri içerdiğinden, bu dosyayı her zaman kaynak denetiminden dışlayın. Yerel işlevler projesiyle oluşturulan *. gitignore* dosyası, varsayılan olarak dosyayı dışlar.

[!INCLUDE [functions-register-storage-binding-extension-csharp](../../includes/functions-register-storage-binding-extension-csharp.md)]

## <a name="add-an-output-binding-definition-to-the-function"></a>İşleve çıkış bağlama tanımı ekleme

Bir işlev yalnızca bir tetikleyicisine sahip olsa da, özel tümleştirme kodu yazmadan diğer Azure hizmetlerine ve kaynaklarına bağlanmanızı sağlayan birden çok giriş ve çıkış bağlaması olabilir. 

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Bu bağlamaları, işlev klasörünüzdeki *function. JSON* dosyasında bildirirsiniz. Önceki hızlı başlangıçta, *Httpexample* klasöründeki *function. JSON* dosyanız `bindings` koleksiyonunda iki bağlama içerir:  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-python"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-powershell"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json" range="2-18":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript, programming-language-powershell, programming-language-typescript"  
Her bağlamanın en az bir türü, yönü ve adı vardır. Yukarıdaki örnekte, ilk bağlama Direction `in``httpTrigger` türüdür. `in` yönü için, `name` tetikleyici tarafından çağrıldığında işleve gönderilen giriş parametresinin adını belirtir.  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
Koleksiyondaki ikinci bağlama `res`olarak adlandırılmıştır. Bu `http` bağlama, HTTP yanıtını yazmak için kullanılan bir çıkış bağlamadır (`out`). 

Bu işlevden bir Azure depolama kuyruğuna yazmak için, aşağıdaki kodda gösterildiği gibi, `queue` türünde bir `out` bağlama `msg`adını ekleyin:

:::code language="json" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python"  
Koleksiyondaki ikinci bağlama Direction `out``http` türündedir, bu durumda `$return` özel `name`, bu bağlamanın bir giriş parametresi sağlamak yerine işlevin dönüş değerini kullandığını gösterir.

Bu işlevden bir Azure depolama kuyruğuna yazmak için, aşağıdaki kodda gösterildiği gibi, `queue` türünde bir `out` bağlama `msg`adını ekleyin:

:::code language="json" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-powershell"  
Koleksiyondaki ikinci bağlama `res`olarak adlandırılmıştır. Bu `http` bağlama, HTTP yanıtını yazmak için kullanılan bir çıkış bağlamadır (`out`). 

Bu işlevden bir Azure depolama kuyruğuna yazmak için, aşağıdaki kodda gösterildiği gibi, `queue` türünde bir `out` bağlama `msg`adını ekleyin:

:::code language="json" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Bu durumda, `msg` işleve çıkış bağımsız değişkeni olarak verilir. `queue` bir tür için, `queueName` sıranın adını da belirtmeniz ve `connection`' deki Azure depolama bağlantısı 'nın ( *Local. Settings. JSON*' dan) *adını* sağlamanız gerekir. 
::: zone-end  

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]  
::: zone-end  

Bağlamaların ayrıntıları hakkında daha fazla bilgi için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md) ve [sıra çıkış yapılandırması](functions-bindings-storage-queue-output.md#configuration).

## <a name="add-code-to-use-the-output-binding"></a>Çıkış bağlamayı kullanmak için kod ekleme

*Function. JSON*' da belirtilen sıra bağlaması ile, artık işlevinizi `msg` çıktı parametresini alacak şekilde güncelleştirebilir ve iletileri kuyruğa yazabilirsiniz.

::: zone pivot="programming-language-python"     
[!INCLUDE [functions-add-output-binding-python](../../includes/functions-add-output-binding-python.md)]
::: zone-end  

::: zone pivot="programming-language-javascript"  
[!INCLUDE [functions-add-output-binding-js](../../includes/functions-add-output-binding-js.md)]
::: zone-end  

::: zone pivot="programming-language-typescript"  
[!INCLUDE [functions-add-output-binding-ts](../../includes/functions-add-output-binding-ts.md)]
::: zone-end  

::: zone pivot="programming-language-powershell"  
[!INCLUDE [functions-add-output-binding-powershell](../../includes/functions-add-output-binding-powershell.md)]  
::: zone-end

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]
::: zone-end 

Kimlik doğrulaması, kuyruk başvurusu alma veya veri yazma için kod *yazmanıza gerek olmadığını* gözlemleyin. Tüm bu tümleştirme görevleri, Azure Işlevleri çalışma zamanı ve sıra çıkış bağlamasında kolayca işlenir.

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

[!INCLUDE [functions-extension-bundles-info](../../includes/functions-extension-bundles-info.md)]

## <a name="view-the-message-in-the-azure-storage-queue"></a>Azure depolama sırasındaki iletiyi görüntüleme

Kuyruğu [Azure Portal](../storage/queues/storage-quickstart-queues-portal.md) veya [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)görüntüleyebilirsiniz. Ayrıca, aşağıdaki adımlarda açıklandığı gibi, kuyruğu Azure CLı 'de de görebilirsiniz:

1. İşlevin proje *yerel. Setting. JSON* dosyasını açın ve bağlantı dizesi değerini kopyalayın. Bir Terminal veya komut penceresinde, `<MY_CONNECTION_STRING>`yerine belirli bağlantı dizenizi yapıştırarak `AZURE_STORAGE_CONNECTION_STRING`adlı bir ortam değişkeni oluşturmak için aşağıdaki komutu çalıştırın. (Bu ortam değişkeni, `--connection-string` bağımsız değişkenini kullanarak sonraki her komuta bağlantı dizesi sağlamanız gerekmez.)

    # <a name="bash"></a>[Bash](#tab/bash)
    
    ```bash
    AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<MY_CONNECTION_STRING>"
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    set AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    ---
    
1. Seçim Hesabınızdaki depolama kuyruklarını görüntülemek için [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) komutunu kullanın. Bu komutun çıktısı, işlev bu kuyruğa ilk iletisini yazmışsa oluşturulan `outqueue`adlı bir sıra içermelidir.
    
    ```azure-cli
    az storage queue list --output tsv
    ```

1. Bu kuyruktan iletiyi okumak için [`az storage message get`](/cli/azure/storage/message#az-storage-message-get) komutunu kullanın. Bu, daha önce işlev test edilirken kullandığınız ilk ad olmalıdır. Komut kuyruktan ilk iletiyi okur ve kaldırır. 

    # <a name="bash"></a>[Bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershell"></a>[PowerShell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    az storage message get --queue-name outqueue -o tsv --query [].{Message:content} > %TEMP%out.b64 && certutil -decode -f %TEMP%out.b64 %TEMP%out.txt > NUL && type %TEMP%out.txt && del %TEMP%out.b64 %TEMP%out.txt /q
    ```

    Bu betik, yerel bir geçici dosyadan Base64 kodlamalı ileti koleksiyonunun kodunu çözmek için Certutil kullanır. Çıkış yoksa, bir hata olması durumunda certutil çıkışını gizlemeyi durdurmak için komut dosyasından `> NUL` kaldırmayı deneyin. 
    
    ---
    
    İleti gövdesi [Base64 kodlamalı](functions-bindings-storage-queue-trigger.md#encoding)depolandığından, iletinin görüntülenmeden önce kodu çözülemedi. `az storage message get`yürütmeden sonra ileti kuyruktan kaldırılır. `outqueue`içinde yalnızca bir ileti varsa, bu komutu ikinci kez çalıştırıp bir hata elde ettiğinizde bir ileti almazsınız.

## <a name="redeploy-the-project-to-azure"></a>Projeyi Azure 'a yeniden dağıtın

İşlevin Azure depolama kuyruğuna bir ileti yazdığını doğruladığınıza göre, Azure üzerinde çalışan uç noktayı güncelleştirmek için projenizi yeniden dağıtabilirsiniz.

1. *Localfunctionsproj* klasöründe, projeyi yeniden dağıtmak için [`func azure functionapp publish`](functions-run-local.md#project-file-deployment) komutunu kullanın ve`<APP_NAME>` uygulamanızın adıyla değiştirin.

    ```
    func azure functionapp publish <APP_NAME>
    ```
    
1. Önceki hızlı başlangıçta olduğu gibi, yeniden dağıtılan işlevi sınamak için bir tarayıcı veya KıVRıMLı kullanın.

    # <a name="browser"></a>[Tarayıcı](#tab/browser)
    
    Yayımla komutunun çıktısında gösterilen tüm **ÇAĞıRMA URL** 'sini bir tarayıcı adres çubuğuna kopyalayın, `&name=Functions`sorgu parametresini ekleyin. , İşlevi yerel olarak çalıştırdığınızda tarayıcı benzer bir çıktı görüntülemelidir.

    ![İşlevin çıktısı Azure üzerinde bir tarayıcıda çalışır](./media/functions-add-output-binding-storage-queue-cli/function-test-cloud-browser.png)

    # <a name="curl"></a>[kıvr](#tab/curl)
    
    [`curl`](https://curl.haxx.se/) `&name=Functions`parametresini ekleyerek **Invoke URL**'siyle çalıştırın. Komutun çıktısı, "Hello Functions" metni olmalıdır.
    
    ![İşlevin çıktısı, Azure 'da KıVRıMLı kullanılarak çalışır](./media/functions-add-output-binding-storage-queue-cli/function-test-cloud-curl.png)

    --- 

1. Depolama sırasını, önceki bölümde açıklandığı gibi bir kez inceleyerek sıraya yazılan yeni iletiyi içerdiğini doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde, daha fazla maliyet ödemeden kaçınmak için kaynak grubunu ve içerdiği tüm kaynakları silmek için aşağıdaki komutu kullanın.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen işlevinizi bir depolama kuyruğuna veri yazmak için güncelleştirdiniz. Artık çekirdek araçları ve Azure CLı kullanarak komut satırından Işlev geliştirme hakkında daha fazla bilgi edinebilirsiniz:

+ [Azure Functions Core Tools çalışın](functions-run-local.md)  

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
+ [Azure Işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)

+ [İşlevler fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/)

+ [Tüketim planı maliyetlerini tahmin etme](functions-consumption-costs.md) 
