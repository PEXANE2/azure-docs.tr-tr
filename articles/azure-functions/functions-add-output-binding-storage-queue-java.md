---
title: Java işlevinizi Azure Depolama'ya bağlayın
description: Bir Kuyruk depolama çıktısı bağlama kullanarak HTTP tarafından tetiklenen bir Java işlevini Azure Depolama'ya nasıl bağlayabilirsiniz öğrenin.
author: KarlErickson
ms.author: karler
ms.date: 10/14/2019
ms.topic: quickstart
zone_pivot_groups: java-build-tools-set
ms.openlocfilehash: d9815fd27a57acc8b418962e610d2ae1c106edde
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673273"
---
# <a name="connect-your-java-function-to-azure-storage"></a>Java işlevinizi Azure Depolama'ya bağlayın

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

Bu makalede, [önceki quickstart makalesinde](functions-create-first-java-maven.md) oluşturduğunuz işlevin Azure Depolama kuyruğuyla nasıl tümleştirilen ekidir. Bu işleve eklediğiniz çıktı bağlama, bir HTTP isteğindeki verileri kuyruktaki bir iletiye yazar.

Çoğu bağlama, Bağlı hizmete erişmek için Işlevlerin kullandığı depolanmış bir bağlantı dizesi gerektirir. Bu bağlantıyı kolaylaştırmak için, işlev uygulamanızla oluşturduğunuz Depolama hesabını kullanırsınız. Bu hesaba bağlantı zaten adlı `AzureWebJobsStorage`bir uygulama ayarında depolanır.  

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi başlatmadan önce, [Java quickstart bölüm 1](functions-create-first-java-maven.md)adımları tamamlayın.

## <a name="download-the-function-app-settings"></a>Fonksiyon uygulama ayarlarını indirin

[!INCLUDE [functions-app-settings-download-cli](../../includes/functions-app-settings-download-local-cli.md)]

## <a name="enable-extension-bundles"></a>Uzantı demetlerini etkinleştirme

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Artık projenize Depolama çıktısı bağlamayı ekleyebilirsiniz.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

[!INCLUDE [functions-add-output-binding-java-cli](../../includes/functions-add-output-binding-java-cli.md)]

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlaması kullanan kod ekleme

[!INCLUDE [functions-add-output-binding-java-code](../../includes/functions-add-output-binding-java-code.md)]

[!INCLUDE [functions-add-output-binding-java-test-cli](../../includes/functions-add-output-binding-java-test-cli.md)]

Artık yeni çıktı bağlamayı yerel olarak denemeye hazırsınız.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Daha önce olduğu gibi, projeyi oluşturmak ve Işlevleri yerel olarak çalıştırma zamanını başlatmak için aşağıdaki komutu kullanın:

# <a name="maven"></a>[Maven](#tab/maven)
```bash
mvn clean package 
mvn azure-functions:run
```
# <a name="gradle"></a>[Gradle](#tab/gradle) 
```bash
gradle jar --info
gradle azureFunctionsRun
```
---

> [!NOTE]  
> Host.json'da uzantı demetlerini etkinleştirdiğiniz için, [Depolama bağlama uzantısı](functions-bindings-storage-blob.md#add-to-your-functions-app) başlangıç sırasında diğer Microsoft bağlama uzantılarıyla birlikte karşıdan yüklendi ve sizin için yüklendi.

Daha önce olduğu gibi, yeni bir terminal penceresinde cURL kullanarak komut satırından işlevi tetikle:

```CMD
curl -w "\n" http://localhost:7071/api/HttpTrigger-Java --data AzureFunctions
```

Bu kez, çıktı bağlama depolama hesabınızda adlı `outqueue` bir sıra oluşturur ve aynı dize ile bir ileti ekler.

Ardından, yeni kuyruğa görüntülemek ve bir iletinin eklandığını doğrulamak için Azure CLI'yi kullanırsınız. Microsoft [Azure Depolama Gezgini'ni][Azure Storage Explorer] kullanarak veya [Azure portalında](https://portal.azure.com)da sıranızı görüntüleyebilirsiniz.

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

### <a name="redeploy-the-project"></a>Projeyi yeniden dağıtın 

Yayınlanan uygulamanızı güncellemek için aşağıdaki komutu yeniden çalıştırın:  

# <a name="maven"></a>[Maven](#tab/maven)  
```bash
mvn azure-functions:deploy
```
# <a name="gradle"></a>[Gradle](#tab/gradle)  
```bash
gradle azureFunctionsDeploy
```
---

Yine, dağıtılan işlevi sınamak için cURL kullanabilirsiniz. Daha önce olduğu `AzureFunctions` gibi, post isteğinin gövdesindeki değeri aşağıdaki örnekte olduğu gibi URL'ye iletin:

```bash
curl -w "\n" https://fabrikam-functions-20190929094703749.azurewebsites.net/api/HttpTrigger-Java?code=zYRohsTwBlZ68YF.... --data AzureFunctions
```

Çıktı bağlamanın beklendiği gibi kuyrukta yeni bir ileti oluşturduğunu doğrulamak için [Depolama sıra iletisini](#query-the-storage-queue) yeniden inceleyebilirsiniz.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Depolama kuyruğuna veri yazmak için HTTP tetiklenen işlevinizi güncellediniz. Java ile Azure İşlevleri geliştirme hakkında daha fazla bilgi edinmek için [Azure İşlevler Java geliştirici kılavuzuna](functions-reference-java.md) ve [Azure İşlevlerini tetikleyenlere ve bağlamalara](functions-triggers-bindings.md)bakın. Java'daki tam Fonksiyon projeleri örnekleri için [Java İşlevleri örneklerine](/samples/browse/?products=azure-functions&languages=Java)bakın. 

Ardından, işlev uygulamanız için Uygulama Öngörüleri izlemeetkinleştirmelisiniz:

> [!div class="nextstepaction"]
> [Application Insights tümleştirmesini etkinleştirme](functions-monitoring.md#manually-connect-an-app-insights-resource)


[Azure Storage Explorer]: https://storageexplorer.com/
