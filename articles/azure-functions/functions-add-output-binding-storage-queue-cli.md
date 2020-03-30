---
title: Komut satırı araçlarını kullanarak Azure Fonksiyonlarını Azure Depolama'ya bağlayın
description: Komut satırı projenize bir çıktı bağlama ekleyerek Azure İşlevlerini Azure Depolama kuyruğuna nasıl bağlayabilirsiniz öğrenin.
ms.date: 02/07/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: 9181caf516d5c2003cfe99b125d2921732cbbb9d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79473396"
---
# <a name="connect-azure-functions-to-azure-storage-using-command-line-tools"></a>Komut satırı araçlarını kullanarak Azure Fonksiyonlarını Azure Depolama'ya bağlayın

Bu makalede, bir Azure Depolama [kuyruğunu önceki quickstart'ta](functions-create-first-azure-function-azure-cli.md)oluşturduğunuz işlev ve depolama hesabıyla tümleştirirsiniz. Bu tümleştirmeyi, bir HTTP isteğinden kuyruktaki bir iletiye veri yazan bir *çıktı bağlama* kullanarak elde elabilirsiniz. Bu makalenin tamamlanması, önceki hızlı başlangıç birkaç USD sent ötesinde hiçbir ek maliyete tabi değildir. Bağlamalar hakkında daha fazla bilgi edinmek için [Azure İşlevlerini tetikleyen ve bağlama kavramlarına](functions-triggers-bindings.md)bakın.

## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırın

Başlamadan önce, makaleyi tamamlamanız gerekir, [Hızlı Başlat: Komut satırından bir Azure İşlevleri projesi oluşturun.](functions-create-first-azure-function-azure-cli.md) Bu makalenin sonunda kaynakları zaten temizlediyseniz, Azure'daki işlev uygulamasını ve ilgili kaynakları yeniden oluşturmak için adımları yeniden gözden geçirin.

## <a name="retrieve-the-azure-storage-connection-string"></a>Azure Depolama bağlantı dizesini alın

Önceki hızlı başlangıçta Azure'da bir işlev uygulaması oluşturduğunuzda, bir Depolama hesabı da oluşturdunuz. Bu hesabın bağlantı dizesi Azure'daki uygulama ayarlarında güvenli bir şekilde depolanır. Ayarı *local.settings.json* dosyasına indirerek, işlevi yerel olarak çalıştırırken bu bağlantıyı aynı hesaptaki Depolama kuyruğuna yazma'yı kullanabilirsiniz. 

1. Projenin kökünden, önceki hızlı başlangıçtaki işlev `<APP_NAME>` uygulamanızın adını değiştirerek aşağıdaki komutu çalıştırın. Bu komut, dosyadaki varolan değerlerin üzerine yazılır.

    ```
    func azure functionapp fetch-app-settings <APP_NAME>
    ```
    
1. *local.settings.json'u* açın ve `AzureWebJobsStorage`Depolama hesabı bağlantı dizesi olan adlı değeri bulun. Bu makalenin `AzureWebJobsStorage` diğer bölümlerinde adı ve bağlantı dizesini kullanırsınız.

> [!IMPORTANT]
> *local.settings.json* Azure'dan indirilen sırları içerdiğinden, bu dosyayı her zaman kaynak denetiminden hariç tutar. Yerel işlevler projesiyle oluşturulan *.gitignore* dosyası varsayılan olarak dosyayı dışlar.

[!INCLUDE [functions-register-storage-binding-extension-csharp](../../includes/functions-register-storage-binding-extension-csharp.md)]

## <a name="add-an-output-binding-definition-to-the-function"></a>İşleviçin çıktı bağlama tanımı ekleme

Bir işlevin yalnızca bir tetikleyicisi olsa da, özel tümleştirme kodu yazmadan diğer Azure hizmetlerine ve kaynaklarına bağlanmanızı sağlayan birden çok giriş ve çıktı bağlaması olabilir. 

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Bu bağlamaları işlev klasörünüzde *işlev.json* dosyasında bildirirsiniz. Önceki hızlı başlangıçtan itibaren, *HttpExample* klasöründeki `bindings` *function.json* dosyanız koleksiyonda iki bağlama içerir:  
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
Her bağlamanın en az bir türü, yönü ve adı vardır. Yukarıdaki örnekte, ilk bağlama yönü `httpTrigger` `in`ile türüdür. `in` Yön için, `name` tetikleyici tarafından çağrıldığınızda işleve gönderilen bir giriş parametresinin adını belirtir.  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
Koleksiyondaki ikinci bağlama adı `res`. Bu `http` bağlama, HTTP`out`yanıtını yazmak için kullanılan bir çıktı bağlama ( ) 

Bu işlevden bir Azure Depolama kuyruğuna `out` yazmak `queue` için, `msg`aşağıdaki kodda gösterildiği gibi, adında bir tür bağlama ekleyin:

:::code language="json" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python"  
Koleksiyondaki ikinci `http` bağlama yönü `out`ile tür , bu durumda `name` `$return` özel bu bağlama bir giriş parametresi sağlamak yerine işlevin dönüş değerini kullandığını gösterir.

Bu işlevden bir Azure Depolama kuyruğuna `out` yazmak `queue` için, `msg`aşağıdaki kodda gösterildiği gibi, adında bir tür bağlama ekleyin:

:::code language="json" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-powershell"  
Koleksiyondaki ikinci bağlama adı `res`. Bu `http` bağlama, HTTP`out`yanıtını yazmak için kullanılan bir çıktı bağlama ( ) 

Bu işlevden bir Azure Depolama kuyruğuna `out` yazmak `queue` için, `msg`aşağıdaki kodda gösterildiği gibi, adında bir tür bağlama ekleyin:

:::code language="json" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Bu durumda, `msg` bir çıktı bağımsız değişkeni olarak işlev verilir. Bir `queue` tür için, sıranın adını belirtmeniz `queueName` ve Azure Depolama bağlantısının *adını* *(local.settings.json'dan)*.'da `connection`sağlamanız gerekir. 
::: zone-end  

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]  
::: zone-end  

Bağlamaayrıntıları hakkında daha fazla bilgi için Azure [İşlevlerini tetikler ve bağlamalar kavramları ve](functions-triggers-bindings.md) [sıra çıktıyapılandırması'na](functions-bindings-storage-queue-output.md#configuration)bakın.

## <a name="add-code-to-use-the-output-binding"></a>Çıktı bağlamayı kullanmak için kod ekleme

*function.json'da*belirtilen sıra bağlama ile artık çıktı parametresini `msg` almak ve sıraya ileti yazmak için işlevinizi güncelleştirebilirsiniz.

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

Kimlik doğrulama, sıra başvurusu alma veya veri yazma için kod yazmanız *gerekmesin.* Tüm bu tümleştirme görevleri Azure İşlevleri çalışma zamanında ve sıra çıktısına bağlamada rahatlıkla işlenir.

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

[!INCLUDE [functions-extension-bundles-info](../../includes/functions-extension-bundles-info.md)]

## <a name="view-the-message-in-the-azure-storage-queue"></a>Azure Depolama kuyruğundaki iletiyi görüntüleme

[Kuyruğa Azure portalında](../storage/queues/storage-quickstart-queues-portal.md) veya [Microsoft Azure Depolama Gezgini'nde](https://storageexplorer.com/)görüntüleyebilirsiniz. Aşağıdaki adımlarda açıklandığı gibi, Azure CLI'deki sırayı da görüntüleyebilirsiniz:

1. İşlev projesinin *yerel.setting.json* dosyasını açın ve bağlantı dize değerini kopyalayın. Bir terminal veya komut penceresinde, aşağıdaki komutu çalıştırarak bir ortam değişkeni oluşturmak için , `AZURE_STORAGE_CONNECTION_STRING`'yerine belirli bağlantı dizesi yapıştırma `<MY_CONNECTION_STRING>`. (Bu ortam değişkeni, `--connection-string` bağımsız değişkeni kullanarak sonraki her komuta bağlantı dizesi sağlamanız gerekolmadığı anlamına gelir.)

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    # <a name="powershell"></a>[Powershell](#tab/powershell)
    
    ```powershell
    $env:AZURE_STORAGE_CONNECTION_STRING = "<MY_CONNECTION_STRING>"
    ```
    
    # <a name="azure-cli"></a>[Azure CLI](#tab/cmd)
    
    ```azurecli
    set AZURE_STORAGE_CONNECTION_STRING="<MY_CONNECTION_STRING>"
    ```
    
    ---
    
1. (İsteğe bağlı) Hesabınızdaki [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) Depolama kuyruklarını görüntülemek için komutu kullanın. Bu komutun çıktısı, işlev `outqueue`bu kuyruğa ilk iletisini yazdığında oluşturulan , adlı bir sıra içermelidir.
    
    ```azurecli
    az storage queue list --output tsv
    ```

1. İşlevdaha önce test ederken kullandığınız ilk ad olmalıdır bu kuyruktaki iletiyi okumak için [`az storage message get`](/cli/azure/storage/message#az-storage-message-get) komutu kullanın. Komut ilk iletiyi sıradan okur ve kaldırır. 

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    echo `echo $(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
    ```
    
    # <a name="powershell"></a>[Powershell](#tab/powershell)
    
    ```powershell
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(az storage message get --queue-name outqueue -o tsv --query '[].{Message:content}')))
    ```
    
    # <a name="azure-cli"></a>[Azure CLI](#tab/cmd)
    
    ```azurecli
    az storage message get --queue-name outqueue -o tsv --query [].{Message:content} > %TEMP%out.b64 && certutil -decode -f %TEMP%out.b64 %TEMP%out.txt > NUL && type %TEMP%out.txt && del %TEMP%out.b64 %TEMP%out.txt /q
    ```

    Bu komut dosyası, yerel bir geçici dosyadan base64 kodlanmış ileti koleksiyonunu çözmek için certutil kullanır. Çıktı yoksa, bir hata olması `> NUL` durumunda certutil çıktısını bastırmayı durdurmak için komut dosyasından kaldırmayı deneyin. 
    
    ---
    
    İleti gövdesi [base64 kodlu](functions-bindings-storage-queue-trigger.md#encoding)olarak depolandığı için, ileti görüntülenmeden önce deşifre edilmelidir. Yürütmeden `az storage message get`sonra ileti kuyruktan kaldırılır. Yalnızca bir ileti `outqueue`varsa, bu komutu ikinci kez çalıştırdığınızda bir ileti almazsınız ve bunun yerine bir hata alırsınız.

## <a name="redeploy-the-project-to-azure"></a>Projeyi Azure'a yeniden dağıtma

İşlevin Azure Depolama kuyruğuna bir ileti yazdığını yerel olarak doğruladığınıza göre, Projenizi Azure'da çalışan bitiş noktasını güncelleştirmek için yeniden dağıtabilirsiniz.

1. *LocalFunctionsProj* klasöründe, uygulamanızın adını değiştirerek [`func azure functionapp publish`](functions-run-local.md#project-file-deployment) `<APP_NAME>` projeyi yeniden dağıtmak için komutu kullanın.

    ```
    func azure functionapp publish <APP_NAME>
    ```
    
1. Önceki hızlı başlatmada olduğu gibi, yeniden dağıtılan işlevi sınamak için bir tarayıcı veya CURL kullanın.

    # <a name="browser"></a>[Tarayıcı](#tab/browser)
    
    Yayımlama komutunun çıktısında gösterilen invoke **URL'sinin** tamamını bir tarayıcı adresi `&name=Functions`çubuğuna kopyalayın ve sorgu parametresini ekler. Tarayıcı, işlevi yerel olarak çalıştırdığınızda olduğu gibi benzer çıktıları görüntülemelidir.

    ![İşlevin çıktısı bir tarayıcıda Azure'da çalışır](./media/functions-add-output-binding-storage-queue-cli/function-test-cloud-browser.png)

    # <a name="curl"></a>[Curl](#tab/curl)
    
    Invoke [`curl`](https://curl.haxx.se/) **URL'si**ile çalıştırın, parametreyi `&name=Functions`ekle. Komutun çıktısı "Merhaba İşlevler" metni olmalıdır.
    
    ![İşlevin çıktısı CURL kullanarak Azure'da çalışır](./media/functions-add-output-binding-storage-queue-cli/function-test-cloud-curl.png)

    --- 

1. Sıraya yazılan yeni iletiyi içerdiğini doğrulamak için önceki bölümde açıklandığı gibi Depolama sırasını yeniden inceleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bitirdikten sonra, daha fazla maliyete yol alamamak için kaynak grubunu ve içerdiği tüm kaynakları silmek için aşağıdaki komutu kullanın.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Sonraki adımlar

Depolama kuyruğuna veri yazmak için HTTP tetiklenen işlevinizi güncellediniz. Artık Temel Araçlar ve Azure CLI'yi kullanarak komut satırından İşlevler geliştirme hakkında daha fazla bilgi edinebilirsiniz:

+ [Azure İşlevleri Temel Araçlarıyla Çalışma](functions-run-local.md)  

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
+ [Azure Fonksiyonları tetikler ve bağlamalar](functions-triggers-bindings.md)

+ [Fonksiyonlar fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/)

+ [Tüketim planı maliyetlerinin tahmini](functions-consumption-costs.md) 
