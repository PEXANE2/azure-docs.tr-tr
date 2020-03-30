---
title: Azure portalını kullanarak Dayanıklı Işlevler Oluşturma
description: Portal geliştirme için Azure Fonksiyonları için Dayanıklı İşlevler uzantısını nasıl yükleyin öğrenin.
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: azfuncdf
ms.openlocfilehash: 0060088acb100036c094406e01d0d736a4af88eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75769651"
---
# <a name="create-durable-functions-using-the-azure-portal"></a>Azure portalını kullanarak Dayanıklı Işlevler Oluşturma

Azure [İşlevler için Dayanıklı İşlevler](durable-functions-overview.md) uzantısı NuGet paketi [Microsoft.Azure.WebJobs.Extensions.DurableTask'ta](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask)sağlanır. Bu uzantı işlev uygulamanızda yüklenmiş olmalıdır. Bu makalede, Azure portalında dayanıklı işlevler geliştirebilmeniz için bu paketin nasıl yüklenirsiniz.

> [!NOTE]
> 
> * C#'da dayanıklı işlevler geliştiriyorsanız, [visual studio 2019 geliştirmeyi](durable-functions-create-first-csharp.md)göz önünde bulundurmalısınız.
> * JavaScript'te dayanıklı işlevler geliştiriyorsanız, bunun yerine [Visual Studio Code geliştirmeyi](./quickstart-js-vscode.md)düşünmelisiniz.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Herhangi bir işlevin yürütülmesini barındıracak bir işlev uygulamasına sahip olmalısınız. İşlev uygulaması, işlevlerinizi kaynakların daha kolay yönetimi, dağıtımı, ölçeklendirilmesi ve paylaşımı için mantıksal bir birim olarak gruplandırmanızı sağlar. Bir .NET veya JavaScript uygulaması oluşturabilirsiniz.

[!INCLUDE [Create function app Azure portal](../../../includes/functions-create-function-app-portal.md)]

Varsayılan olarak, oluşturulan işlev uygulaması Azure İşlevleri çalışma zamanının 2.x sürümünü kullanır. Dayanıklı Işlevler uzantısı, C#'daki Azure İşlevleri çalışma süresinin 1.x ve 2.x sürümlerinde ve JavaScript'te sürüm 2.x'te çalışır. Ancak şablonlar yalnızca seçilen dilden bağımsız olarak çalışma zamanının sürüm 2.x'ini hedeflerken kullanılabilir.

## <a name="install-the-durable-functions-npm-package-javascript-only"></a>Dayanıklı işlevler npm paketini yükleyin (yalnızca JavaScript)

JavaScript Dayanıklı Fonksiyonlar oluşturuyorsanız, [ `durable-functions` npm paketini](https://www.npmjs.com/package/durable-functions)yüklemeniz gerekir.

1. İşlev uygulamanızın adını seçin, ardından **Platform Özellikleri,** **Gelişmiş araçlar (Kudu)** izleyin.

   ![Fonksiyonlar platformu özellikleri Kudu seçin](./media/durable-functions-create-portal/function-app-platform-features-choose-kudu.png)

2. Kudu konsolu içinde Hata **Ayıklama konsolu** sonra **CMD**seçin.

   ![Kudu hata ayıklama konsolu](./media/durable-functions-create-portal/kudu-choose-debug-console.png)

3. İşlev uygulamanızın dosya dizini yapısı görüntülenmelidir. `site/wwwroot` klasörüne gidin. Buradan, dosyayı `package.json` sürükleyip dosya dizini penceresine bırakarak dosyayı yükleyebilirsiniz. Bir `package.json` örnek aşağıdadır:

    ```json
    {
      "dependencies": {
        "durable-functions": "^1.3.1"
      }
    }
    ```

   ![Kudu upload package.json](./media/durable-functions-create-portal/kudu-choose-debug-console.png)

4. Yüklemeniz `package.json` indikten sonra `npm install` Kudu Uzaktan Yürütme Konsolu'ndan komutu çalıştırın.

   ![Kudu çalıştır npm yüklemek](./media/durable-functions-create-portal/kudu-npm-install.png)

## <a name="create-an-orchestrator-function"></a>Orkestratör işlevi oluşturma

1. İşlev uygulamanızı genişletin ve **+** **Fonksiyonlar'ın**yanındaki düğmeyi tıklatın. Bu, işlev uygulamanızdaki ilk işlevse **Portalda**'yı ve ardından **Devam**'ı seçin. Aksi takdirde üçüncü adıma geçin.

   ![Azure portalındaki İşlevler hızlı başlangıç sayfası](./media/durable-functions-create-portal/function-app-quickstart-choose-portal.png)

1. **Diğer şablonlar**'ı ve ardından **Sonlandır ve şablonları görüntüle**'yi seçin.

    ![İşlevler hızlı başlangıcı diğer şablonlar](./media/durable-functions-create-portal/add-first-function.png)

1. Arama alanında, Dayanıklı `durable` **Fonksiyonlar HTTP başlangıç** şablonunu yazın ve seçin.

1. İstendiğinde, Azure Dayanıklı Görev uzantısını ve işlev uygulamasındaki tüm bağımlılıkları yüklemek için **Yükle'yi** seçin. Uzantıyı işlev uygulamasına yalnızca bir kez yüklemeniz gerekir. Yükleme başarılı olduktan sonra **Devam**'ı seçin.

    ![Bağlama uzantılarını yükleme](./media/durable-functions-create-portal/install-durabletask-extension.png)

1. Yükleme tamamlandıktan sonra, yeni `HttpStart` işlevi adlandırın ve **Oluştur'u**seçin. Oluşturulan işlev, orkestrasyonu başlatmak için kullanılır.

1. Bu kez **Dayanıklı Fonksiyonlar Orchestrator** şablonunu kullanarak işlev uygulamasında başka bir işlev oluşturun. Yeni orkestrasyon `HelloSequence`işlevinizi adlandırın.

1. **Dayanıklı İşlevler Etkinliği** şablonunu kullanarak adlı `Hello` üçüncü bir işlev oluşturun.

## <a name="test-the-durable-function-orchestration"></a>Dayanıklı fonksiyon orkestrasyonunu test edin

1. **HttpBaşlat** işlevine geri dön, **</> İşlev URL'sini al** ve URL'yi **kopyala'yı** seçin. **HelloSequence** işlevini başlatmak için bu URL'yi kullanırsınız.

1. Kopyaladığınız URL'ye POSTA isteği göndermek için Postman veya cURL gibi bir HTTP aracıkullanın. Aşağıdaki örnek, dayanıklı işleve POST isteği gönderen bir cURL komutudur:

    ```bash
    curl -X POST https://{your-function-app-name}.azurewebsites.net/api/orchestrators/HelloSequence
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

1. Bitiş `statusQueryGetUri` noktası URI'yi arayın ve bu örnek gibi görünen dayanıklı işlevin geçerli durumunu görürsünüz:

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

1. Durum `statusQueryGetUri` **Tamamlanan'da**değişene kadar bitiş noktasını aramaya devam edin ve aşağıdaki örnek gibi bir yanıt görürsünüz:

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

İlk dayanıklı işleviniz Azure'da çalışmaya başlıyor.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yaygın dayanıklı işlev desenleri hakkında bilgi edinin](durable-functions-overview.md#application-patterns)
