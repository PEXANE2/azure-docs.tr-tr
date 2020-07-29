---
title: Java işlevinizi Azure depolama 'ya bağlama
description: Bir kuyruk depolama çıkış bağlaması kullanarak HTTP ile tetiklenen bir Java işlevini Azure depolama 'ya bağlamayı öğrenin.
author: KarlErickson
ms.custom: devx-track-java
ms.author: karler
ms.date: 10/14/2019
ms.topic: quickstart
zone_pivot_groups: java-build-tools-set
ms.openlocfilehash: 9f512e3bbf7947361fa9890e9514693610c9f99d
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87321964"
---
# <a name="connect-your-java-function-to-azure-storage"></a>Java işlevinizi Azure depolama 'ya bağlama

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

Bu makalede, [önceki hızlı başlangıç makalesinde](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-java&tabs=bash,browser) oluşturduğunuz Işlevi bir Azure depolama kuyruğu ile tümleştirme işlemi gösterilmektedir. Bu işleve eklediğiniz çıkış bağlaması, verileri bir HTTP isteğinden kuyruktaki bir iletiye yazar.

Çoğu bağlamanın, bağlı hizmete erişmek için kullandığı depolanan bir bağlantı dizesi gerekir. Bu bağlantıyı daha kolay hale getirmek için, işlev uygulamanızla oluşturduğunuz depolama hesabını kullanırsınız. Bu hesap bağlantısı zaten adlı bir uygulama ayarında depolanıyor `AzureWebJobsStorage` .  

## <a name="prerequisites"></a>Önkoşullar

Bu makaleye başlamadan önce, [Java hızlı](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-java&tabs=bash,browser)başlangıcının 1. bölümündeki adımları uygulayın.

## <a name="download-the-function-app-settings"></a>İşlev uygulaması ayarlarını indirin

[!INCLUDE [functions-app-settings-download-cli](../../includes/functions-app-settings-download-local-cli.md)]

## <a name="enable-extension-bundles"></a>Uzantı paketlerini etkinleştir

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Artık, depolama çıkış bağlamasını projenize ekleyebilirsiniz.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

[!INCLUDE [functions-add-output-binding-java-cli](../../includes/functions-add-output-binding-java-cli.md)]

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlaması kullanan kod ekleme

[!INCLUDE [functions-add-output-binding-java-code](../../includes/functions-add-output-binding-java-code.md)]

[!INCLUDE [functions-add-output-binding-java-test-cli](../../includes/functions-add-output-binding-java-test-cli.md)]

Artık yeni çıkış bağlamasını yerel olarak denemeye hazırsınız.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Daha önce olduğu gibi, projeyi derlemek ve Işlevler çalışma zamanını yerel olarak başlatmak için aşağıdaki komutu kullanın:

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
> host.jsuzantı paketlerinin etkinleştirildiğinden, [depolama bağlama uzantısı](functions-bindings-storage-blob.md#add-to-your-functions-app) diğer Microsoft bağlama uzantılarıyla birlikte başlangıç sırasında sizin için indirilir ve yüklenir.

Daha önce olduğu gibi, yeni bir Terminal penceresinde kıvrımlı kullanarak işlevi komut satırından tetikleyin:

```CMD
curl -w "\n" http://localhost:7071/api/HttpTrigger-Java --data AzureFunctions
```

Bu kez, çıkış bağlaması aynı zamanda depolama hesabınızda adlı bir kuyruk oluşturur `outqueue` ve bu dizeyi içeren bir ileti ekler.

Ardından, yeni kuyruğu görüntülemek ve bir iletinin eklendiğini doğrulamak için Azure CLı 'yi kullanırsınız. Kuyruğunuzu, [Microsoft Azure Depolama Gezgini][Azure Storage Explorer] veya [Azure Portal](https://portal.azure.com)kullanarak da görüntüleyebilirsiniz.

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

### <a name="redeploy-the-project"></a>Projeyi yeniden dağıtın 

Yayınlanan uygulamanızı güncelleştirmek için aşağıdaki komutu yeniden çalıştırın:  

# <a name="maven"></a>[Maven](#tab/maven)  
```bash
mvn azure-functions:deploy
```
# <a name="gradle"></a>[Gradle](#tab/gradle)  
```bash
gradle azureFunctionsDeploy
```
---

Yine, dağıtılan işlevi test etmek için kıvrımlı kullanabilirsiniz. Daha önce olduğu gibi, `AzureFunctions` Aşağıdaki örnekte olduğu gıbı POST isteğinin gövdesinde bulunan DEĞERI URL 'ye geçirin:

```bash
curl -w "\n" https://fabrikam-functions-20190929094703749.azurewebsites.net/api/HttpTrigger-Java?code=zYRohsTwBlZ68YF.... --data AzureFunctions
```

Çıkış bağlamasının, kuyrukta beklendiği gibi yeni bir ileti üretdiğini doğrulamak için [depolama kuyruğu iletisini yeniden inceleyebilirsiniz](#query-the-storage-queue) .

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen işlevinizi bir depolama kuyruğuna veri yazmak için güncelleştirdiniz. Java ile Azure Işlevleri geliştirme hakkında daha fazla bilgi edinmek için bkz. [Azure Işlevleri Java geliştirici kılavuzu](functions-reference-java.md) ve [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md). Java 'daki tüm Işlev projelerinin örnekleri için bkz. [Java işlevleri örnekleri](/samples/browse/?products=azure-functions&languages=Java). 

Sonra, işlev uygulamanız için Application Insights izlemeyi etkinleştirmelisiniz:

> [!div class="nextstepaction"]
> [Application Insights tümleştirmesini etkinleştirme](functions-monitoring.md#manually-connect-an-app-insights-resource)


[Azure Storage Explorer]: https://storageexplorer.com/
