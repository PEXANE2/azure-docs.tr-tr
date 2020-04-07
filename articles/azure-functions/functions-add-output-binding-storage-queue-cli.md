---
title: Komut satırı araçlarını kullanarak Azure Fonksiyonlarını Azure Depolama'ya bağlayın
description: Komut satırı projenize bir çıktı bağlama ekleyerek Azure İşlevlerini Azure Depolama kuyruğuna nasıl bağlayabilirsiniz öğrenin.
ms.date: 02/07/2020
ms.topic: quickstart
zone_pivot_groups: programming-languages-set-functions
ms.openlocfilehash: f9d9573523083b6355f423b7b3db94b795d8657f
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673383"
---
# <a name="connect-azure-functions-to-azure-storage-using-command-line-tools"></a>Komut satırı araçlarını kullanarak Azure Fonksiyonlarını Azure Depolama'ya bağlayın

Bu makalede, bir Azure Depolama [kuyruğunu önceki quickstart'ta](functions-create-first-azure-function-azure-cli.md)oluşturduğunuz işlev ve depolama hesabıyla tümleştirirsiniz. Bu tümleştirmeyi, bir HTTP isteğinden kuyruktaki bir iletiye veri yazan bir *çıktı bağlama* kullanarak elde elabilirsiniz. Bu makalenin tamamlanması, önceki hızlı başlangıç birkaç USD sent ötesinde hiçbir ek maliyete tabi değildir. Bağlamalar hakkında daha fazla bilgi edinmek için [Azure İşlevlerini tetikleyen ve bağlama kavramlarına](functions-triggers-bindings.md)bakın.

## <a name="configure-your-local-environment"></a>Yerel ortamınızı yapılandırın

Başlamadan önce, makaleyi tamamlamanız gerekir, [Hızlı Başlat: Komut satırından bir Azure İşlevleri projesi oluşturun.](functions-create-first-azure-function-azure-cli.md) Bu makalenin sonunda kaynakları zaten temizlediyseniz, Azure'daki işlev uygulamasını ve ilgili kaynakları yeniden oluşturmak için adımları yeniden gözden geçirin.

[!INCLUDE [functions-cli-get-storage-connection](../../includes/functions-cli-get-storage-connection.md)]

[!INCLUDE [functions-register-storage-binding-extension-csharp](../../includes/functions-register-storage-binding-extension-csharp.md)]

[!INCLUDE [functions-add-output-binding-cli](../../includes/functions-add-output-binding-cli.md)]

::: zone pivot="programming-language-csharp"  
[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]  
::: zone-end  
::: zone pivot="programming-language-java" 
[!INCLUDE [functions-add-output-binding-java-cli](../../includes/functions-add-output-binding-java-cli.md)]
::: zone-end   

Bağlamaayrıntıları hakkında daha fazla bilgi için Azure [İşlevlerini tetikler ve bağlamalar kavramları ve](functions-triggers-bindings.md) [sıra çıktıyapılandırması'na](functions-bindings-storage-queue-output.md#configuration)bakın.

## <a name="add-code-to-use-the-output-binding"></a>Çıktı bağlamayı kullanmak için kod ekleme

Tanımlanan sıra bağlama ile, artık `msg` çıktı parametresini almak ve sıraya iletiyazmak için işlevinizi güncelleştirebilirsiniz.

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

::: zone pivot="programming-language-java"
[!INCLUDE [functions-add-output-binding-java-code](../../includes/functions-add-output-binding-java-code.md)]

[!INCLUDE [functions-add-output-binding-java-test-cli](../../includes/functions-add-output-binding-java-test-cli.md)]
::: zone-end

Kimlik doğrulama, sıra başvurusu alma veya veri yazma için kod yazmanız *gerekmesin.* Tüm bu tümleştirme görevleri Azure İşlevleri çalışma zamanında ve sıra çıktısına bağlamada rahatlıkla işlenir.

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

[!INCLUDE [functions-extension-bundles-info](../../includes/functions-extension-bundles-info.md)]

## <a name="view-the-message-in-the-azure-storage-queue"></a>Azure Depolama kuyruğundaki iletiyi görüntüleme

[!INCLUDE [functions-add-output-binding-view-queue-cli](../../includes/functions-add-output-binding-view-queue-cli.md)]

## <a name="redeploy-the-project-to-azure"></a>Projeyi Azure'a yeniden dağıtma

İşlevin Azure Depolama kuyruğuna bir ileti yazdığını yerel olarak doğruladığınıza göre, Projenizi Azure'da çalışan bitiş noktasını güncelleştirmek için yeniden dağıtabilirsiniz.

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python,programming-language-powershell,programming-language-csharp" 
*LocalFunctionsProj* klasöründe, uygulamanızın adını değiştirerek [`func azure functionapp publish`](functions-run-local.md#project-file-deployment) `<APP_NAME>` projeyi yeniden dağıtmak için komutu kullanın.

```
func azure functionapp publish <APP_NAME>
```
::: zone-end  

::: zone pivot="programming-language-java" 

Yerel proje klasöründe, projenizi yeniden yayımlamak için aşağıdaki Maven komutunu kullanın:
```
mvn azure-functions:deploy
```
::: zone-end

## <a name="verify-in-azure"></a>Azure'da doğrula

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
