---
title: Azure portal kullanarak Dayanıklı İşlevler oluşturun
description: Portal geliştirme için Azure Işlevleri Dayanıklı İşlevler uzantısını yüklemeyi öğrenin.
ms.topic: conceptual
ms.date: 04/10/2020
ms.reviewer: azfuncdf
ms.openlocfilehash: 7eee3c36620d0cc9f5906e355b76e7418c61b477
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85807970"
---
# <a name="create-durable-functions-using-the-azure-portal"></a>Azure portal kullanarak Dayanıklı İşlevler oluşturun

Azure Işlevleri için [dayanıklı işlevler](durable-functions-overview.md) uzantısı, [Microsoft. Azure. WebJobs. Extensions. durabletask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask)NuGet paketinde sunulmaktadır. Bu uzantının işlev uygulamanızda yüklü olması gerekir. Bu makalede, Azure portal dayanıklı işlevler geliştirebilmeniz için bu paketin nasıl yükleneceği gösterilmektedir.

> [!NOTE]
> 
> * C# dilinde dayanıklı işlevler geliştiriyorsanız, bunun yerine [Visual Studio 2019 geliştirmeyi](durable-functions-create-first-csharp.md)göz önünde bulundurmanız gerekir.
> * JavaScript 'te dayanıklı işlevler geliştiriyorsanız bunun yerine [Visual Studio Code geliştirmeyi](./quickstart-js-vscode.md)göz önünde bulundurmanız gerekir.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Herhangi bir işlevin yürütülmesini barındırmak için bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, kaynakların daha kolay yönetilmesi, dağıtılması, ölçeklendirilmesi ve paylaşılması için işlevlerinizi bir mantıksal birim olarak gruplandırmanıza olanak tanır. .NET veya JavaScript uygulaması oluşturabilirsiniz.

[!INCLUDE [Create function app Azure portal](../../../includes/functions-create-function-app-portal.md)]

Oluşturulan işlev uygulaması, varsayılan olarak Azure Işlevleri çalışma zamanının 2. x sürümünü kullanır. Dayanıklı İşlevler uzantısı, C# ' de Azure Işlevleri çalışma zamanının 1. x ve 2. x sürümlerinde ve JavaScript 'te sürüm 2. x üzerinde çalışır. Ancak, şablonlar yalnızca seçili dilden bağımsız olarak çalışma zamanının 2. x 'i hedeflenirken kullanılabilir.

## <a name="install-the-durable-functions-npm-package-javascript-only"></a>Dayanıklı işlevler NPM paketini (yalnızca JavaScript) yükler

JavaScript Dayanıklı İşlevler oluşturuyorsanız [ `durable-functions` NPM paketini](https://www.npmjs.com/package/durable-functions)yüklemeniz gerekir:

1. İşlev uygulamanızın sayfasından sol bölmedeki **geliştirme araçları** altında **Gelişmiş Araçlar** ' ı seçin.

   :::image type="content" source="./media/durable-functions-create-portal/function-app-platform-features-choose-kudu.png" alt-text="İşlevler platform özellikleri kudu seçme":::

2. **Gelişmiş Araçlar** sayfasında **Git**' i seçin.

3. Kudu konsolunun içinde **Hata Ayıkla Konsolu**ve sonra **cmd**' yi seçin.

   :::image type="content" source="./media/durable-functions-create-portal/kudu-choose-debug-console.png" alt-text="Kudu hata ayıklama konsolu":::

3. İşlev uygulamanızın dosya dizini yapısı görüntülenmelidir. `site/wwwroot` klasörüne gidin. Buradan dosya `package.json` dizini penceresine sürükleyip bırakarak bir dosyayı karşıya yükleyebilirsiniz. Örnek `package.json` aşağıda verilmiştir:

    ```json
    {
      "dependencies": {
        "durable-functions": "^1.3.1"
      }
    }
    ```

   :::image type="content" source="./media/durable-functions-create-portal/kudu-choose-debug-console.png" alt-text="Kudu karşıya yükleme package.js":::

4. Karşıya yüklendikten sonra `package.json` `npm install` kudu uzaktan yürütme konsolundan komutunu çalıştırın.

   ![Kudu çalıştırma NPM yüklemesi](./media/durable-functions-create-portal/kudu-npm-install.png)
   
5. Son olarak, değerine sahip bir uygulama ayarı ekleyerek [Uyumluluk modunu etkinleştirin](https://docs.microsoft.com/azure/azure-functions/durable/quickstart-js-vscode#enable-compatibility-mode-1) `FUNCTIONS_V2_COMPATIBILITY_MODE` `true` .

## <a name="create-an-orchestrator-function"></a>Orchestrator işlevi oluşturma

1. İşlev uygulamanızda sol bölmeden **işlevler** ' i seçin ve ardından üst menüden **Ekle** ' yi seçin. 

1. **Yeni işlev** sayfasının arama alanına girin `durable` ve ardından **dayanıklı işlevler http başlangıç** şablonu ' nu seçin.

   :::image type="content" source="./media/durable-functions-create-portal/durable-functions-http-starter-template.png" alt-text="Dayanıklı İşlevler HTTP Starter 'ı seçin":::

1. **Yeni işlev** adı için girin `HttpStart` ve ardından **işlev oluştur**' u seçin.

   Oluşturulan işlev Düzenleme başlatmak için kullanılır.

1. İşlev uygulamasında, bu kez **dayanıklı işlevler Orchestrator** şablonunu kullanarak başka bir işlev oluşturun. Yeni düzenleme işlevinizi adlandırın `HelloSequence` .

1. `Hello` **Dayanıklı işlevler etkinlik** şablonunu kullanarak adlı üçüncü bir işlev oluşturun.

## <a name="test-the-durable-function-orchestration"></a>Dayanıklı işlev düzenlemesini test etme

1. **Httpstart** işlevine dönün, **Işlev URL 'sini al**' ı seçin ve **Panoya Kopyala** simgesini seçerek URL 'yi kopyalayın. **Merhaba sıra** işlevini başlatmak IÇIN bu URL 'yi kullanın.

1. Kopyaladığınız URL 'ye bir POST isteği göndermek için Postman veya kıvrık gibi bir HTTP aracını kullanın. Aşağıdaki örnek, dayanıklı işleve POST isteği gönderen bir kıvrımlı komuttur:

    ```bash
    curl -X POST https://{your-function-app-name}.azurewebsites.net/api/orchestrators/HelloSequence --header "Content-Length: 0"
    ```

    Bu örnekte, `{your-function-app-name}` işlev uygulamanızın adı olan etki alanıdır. Yanıt iletisi, aşağıdaki örneğe benzeyen ve yürütmeyi izleyip yönetmenizi sağlamak için kullanabileceğiniz bir dizi URI uç noktasını içerir:

    ```json
    {  
       "id":"10585834a930427195479de25e0b952d",
       "statusQueryGetUri":"https://...",
       "sendEventPostUri":"https://...",
       "terminatePostUri":"https://...",
       "rewindPostUri":"https://..."
    }
    ```

1. `statusQueryGetUri`Uç nokta URI 'sini çağırın ve dayanıklı işlevin geçerli durumunu görürsünüz ve bu örnek aşağıdaki gibi görünebilir:

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

1. `statusQueryGetUri`Durum **tamamlanana**kadar, bitiş noktasını çağırmaya devam edin ve aşağıdaki örnekte olduğu gibi bir yanıt görürsünüz:

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

İlk dayanıklı işleviniz artık Azure 'da çalışıyor ve çalışıyor.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ortak dayanıklı işlev desenleri hakkında bilgi edinin](durable-functions-overview.md#application-patterns)
