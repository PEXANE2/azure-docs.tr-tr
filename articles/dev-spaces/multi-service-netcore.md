---
title: .NET Core ve VS Code kullanarak birden çok bağımlı hizmet çalıştırma
services: azure-dev-spaces
ms.date: 11/21/2018
ms.topic: tutorial
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s
ms.openlocfilehash: 92e843b09b4ef8af3ea2ba103bb668d4f6549196
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74279876"
---
# <a name="multi-service-development-with-azure-dev-spaces"></a>Azure Dev Spaces ile çoklu hizmet geliştirme

Bu öğreticide, dev alanlarının sağladığı bazı avantajlarla birlikte Azure Dev Spaces kullanarak çok hizmet uygulamaları geliştirmeyi öğreneceksiniz.

## <a name="call-a-service-running-in-a-separate-container"></a>Ayrı kapsayıcıda çalıştırılan hizmeti çağırma

Bu bölümde, `mywebapi`ikinci bir hizmet oluşturacaksınız ve `webfrontend` bunu çağıracaksınız. Her hizmet ayrı kapsayıcılarda çalışır. Ardından her iki kapsayıcıda da hata ayıklayacaksınız.

![Birden çok kapsayıcı](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>*mywebapi* için örnek kod indirme
Zamandan kazanmak adına örnek kodu bir GitHub deposundan indirelim. [https://github.com/Azure/dev-spaces](https://github.com/Azure/dev-spaces) adresine gidip **Kopyala veya İndir**’i seçerek GitHub deposunu indirin. Bu bölümün kodu `samples/dotnetcore/getting-started/mywebapi` konumundadır.

### <a name="run-mywebapi"></a>*mywebapi* hizmetini çalıştırın
1. `mywebapi`Ayrı bir VS Code penceresinde klasörünü açın.
1. **Komut Paleti**'ni açın (**Görünüm | Komut Paleti** menüsünü kullanarak) ve otomatik tamamlama özelliğini kullanarak komutu yazın ve seçin: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Bu komut, projeyi dağıtım için yapılandıran `azds prep` komutuyla karıştırılmamalıdır.
1. F5'e bastıktan sonra hizmetin oluşturulup dağıtılmasını bekleyin. Uygulama başlatıldığında bu dosyanın size ait olduğunu bilirsiniz *. Kapatmak için CTRL + C tuşlarına basın.* ileti, hata ayıklama konsolunda görünür.
1. Uç nokta URL'si `http://localhost:<portnumber>` benzeri bir URL olacaktır. **İpucu: VS Code durum çubuğu turuncu olarak açılır ve tıklatılabilir bir URL görüntüler.** Kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte Azure’daki geliştirme ortamımızda çalışıyordur. Bunun localhost adresi olmasının nedeni `mywebapi` hizmetinin hiçbir genel uç nokta tanımlamamış olması ve buna yalnızca Kubernetes örneğinin içinden erişilebilmesidir. Size rahatlık sağlamak ve yerel makinenizden özel hizmetle etkileşimi kolaylaştırmak için, Azure Dev Spaces Azure'da çalıştırılan kapsayıcıya geçici bir SSH tüneli oluşturur.
1. `mywebapi` hazır olduğunda, tarayıcınızda localhost adresini açın. Varsayılan `/api/values` GET API’sini çağırmak için URL’ye `ValuesController` ekleyin.
1. Tüm adımları başarılı olduysa, `mywebapi` hizmetinden bir yanıt alındığını görebilmelisiniz.

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend*’den *mywebapi*’ye istek gönderme
Şimdi `webfrontend` uygulamasında `mywebapi` hizmetine istek gönderen bir kod yazalım.
1. `webfrontend` için VS Code penceresine geçin.
1. `HomeController.cs`içindeki hakkında yöntemi için kodu *değiştirin* :

    ```csharp
    public async Task<IActionResult> About()
    {
        ViewData["Message"] = "Hello from webfrontend";
        
        using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

        return View();
    }
    ```

Önceki kod örneğinde `azds-route-as` üst bilgisi gelen istekten giden isteğe iletilmektedir. Bunun daha sonra [işbirliğine dayalı geliştirme](team-development-netcore.md)ile takımlara nasıl yardımcı olduğunu göreceksiniz.

### <a name="debug-across-multiple-services"></a>Birden çok hizmette hata ayıklama
1. Bu noktada, `mywebapi` hizmetinin hata ayıklayıcısı ekli bir şekilde çalışmaya devam ediyor olması gerekir. Devam etmiyorsa, `mywebapi` projesinde F5'e basın.
1. `api/values/{id}` GET isteklerini işleyen `Get(int id)` yönteminin içinde bir kesme noktası ayarlayın. Bu, [ *Controllers/valuescontroller. cs* dosyasında 23. satırın](https://github.com/Azure/dev-spaces/blob/master/samples/dotnetcore/getting-started/mywebapi/Controllers/ValuesController.cs#L23)etrafında.
1. `webfrontend` projesinde, `mywebapi/api/values` konumuna GET isteği göndermeden hemen önce bir kesme noktası ayarlayın. Bu, önceki bölümde değiştirdiğiniz [ *Controllers/HomeController. cs* dosyasında](https://github.com/Azure/dev-spaces/blob/master/samples/dotnetcore/getting-started/webfrontend/Controllers/HomeController.cs) 32. satırda bulunur.
1. `webfrontend` projesinde F5'e basın.
1. Web uygulamasını çağırın ve her iki hizmette de kodun üzerinden geçin.
1. Web uygulamasında Hakkında sayfası iki hizmet tarafından birleştirilmiş bir ileti görüntüler: "Hello from webfrontend and Hello from mywebapi."

### <a name="well-done"></a>Bravo!
Artık her kapsayıcının ayrı ayrı geliştirilip dağıtılabileceği çok kapsayıcılı bir uygulamanız var.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Geliştirme alanlarında takım geliştirme hakkında bilgi edinin](team-development-netcore.md)
