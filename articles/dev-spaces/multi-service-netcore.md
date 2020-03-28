---
title: 'Birden fazla bağımlı hizmet çalıştırın: .NET Core & Visual Studio Code'
services: azure-dev-spaces
ms.date: 11/21/2018
ms.topic: tutorial
description: Bu öğretici, Azure Kubernetes Hizmetinde çok hizmetli bir .NET Core uygulamasını hata ayıklamak için Azure Dev Spaces ve Visual Studio Code'u nasıl kullanacağınızı gösterir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
ms.openlocfilehash: 0bbb1aefe517c45207160b83b89f7207e8909666
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75438309"
---
# <a name="running-multiple-dependent-services-net-core-and-visual-studio-code-with-azure-dev-spaces"></a>Birden çok bağımlı hizmet çalıştırma: .NET Core ve Visual Studio Code ile Azure Geliştirme Alanları

Bu eğitimde, Dev Spaces'in sağladığı bazı ek avantajlarla birlikte Azure Dev Spaces'i kullanarak çok hizmetli uygulamaların nasıl geliştireceğinizi öğreneceksiniz.

## <a name="call-a-service-running-in-a-separate-container"></a>Ayrı kapsayıcıda çalıştırılan hizmeti çağırma

Bu bölümde, ikinci bir hizmet `mywebapi`oluşturacak ve `webfrontend` onu çağırmış sınız. Her hizmet ayrı kapsayıcılarda çalışır. Ardından her iki kapsayıcıda da hata ayıklayacaksınız.

![Birden çok kapsayıcı](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>*mywebapi* için örnek kod indirme
Zamandan kazanmak adına örnek kodu bir GitHub deposundan indirelim. https://github.com/Azure/dev-spaces adresine gidip **Kopyala veya İndir**’i seçerek GitHub deposunu indirin. Bu bölümün kodu `samples/dotnetcore/getting-started/mywebapi` konumundadır.

### <a name="run-mywebapi"></a>*mywebapi* hizmetini çalıştırın
1. *Ayrı bir VS Code penceresinde*`mywebapi` klasörünü açın.
1. **Komut Paleti**'ni açın (**Görünüm | Komut Paleti** menüsünü kullanarak) ve otomatik tamamlama özelliğini kullanarak komutu yazın ve seçin: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Bu komut, projeyi dağıtım için yapılandıran `azds prep` komutuyla karıştırılmamalıdır.
1. F5'e bastıktan sonra hizmetin oluşturulup dağıtılmasını bekleyin. Uygulama başladığında hazır olduğunu *anlarsınız. Kapatmak için Ctrl+C tuşuna basın.* ileti hata ayıklama konsolunda görüntülenir.
1. Uç nokta URL'si `http://localhost:<portnumber>` benzeri bir URL olacaktır. **İpucu: VS Kodu durum çubuğu turuncuya döner ve tıklanabilir bir URL görüntüler.** Kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte Azure’daki geliştirme ortamımızda çalışıyordur. Bunun localhost adresi olmasının nedeni `mywebapi` hizmetinin hiçbir genel uç nokta tanımlamamış olması ve buna yalnızca Kubernetes örneğinin içinden erişilebilmesidir. Size rahatlık sağlamak ve yerel makinenizden özel hizmetle etkileşimi kolaylaştırmak için, Azure Dev Spaces Azure'da çalıştırılan kapsayıcıya geçici bir SSH tüneli oluşturur.
1. `mywebapi` hazır olduğunda, tarayıcınızda localhost adresini açın. Varsayılan `ValuesController` GET API’sini çağırmak için URL’ye `/api/values` ekleyin.
1. Tüm adımları başarılı olduysa, `mywebapi` hizmetinden bir yanıt alındığını görebilmelisiniz.

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend*’den *mywebapi*’ye istek gönderme
Şimdi `webfrontend` uygulamasında `mywebapi` hizmetine istek gönderen bir kod yazalım.
1. `webfrontend` için VS Code penceresine geçin.
1. About yöntemiiçin kodu `HomeController.cs` *değiştirin:*

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

Önceki kod örneğinde `azds-route-as` üst bilgisi gelen istekten giden isteğe iletilmektedir. Bunun ortak [geliştirme](team-development-netcore.md)ekiplerine nasıl yardımcı olduğunu daha sonra görürsünüz.

### <a name="debug-across-multiple-services"></a>Birden çok hizmette hata ayıklama
1. Bu noktada, `mywebapi` hizmetinin hata ayıklayıcısı ekli bir şekilde çalışmaya devam ediyor olması gerekir. Devam etmiyorsa, `mywebapi` projesinde F5'e basın.
1. GET isteklerini işleyen `Get(int id)` yöntemin içinde bir kesme noktası ayarlayın. `api/values/{id}` Bu, [ *Denetleyiciler/ValuesController.cs* dosyasındaki 23.](https://github.com/Azure/dev-spaces/blob/master/samples/dotnetcore/getting-started/mywebapi/Controllers/ValuesController.cs#L23)
1. `webfrontend` projesinde, `mywebapi/api/values` konumuna GET isteği göndermeden hemen önce bir kesme noktası ayarlayın. Bu, önceki bölümde değiştirdiğiniz [ *Denetleyiciler/HomeController.cs* dosyasındaki](https://github.com/Azure/dev-spaces/blob/master/samples/dotnetcore/getting-started/webfrontend/Controllers/HomeController.cs) 32 numaralı satır civarındadır.
1. `webfrontend` projesinde F5'e basın.
1. Web uygulamasını çağırın ve her iki hizmette de kodun üzerinden geçin.
1. Web uygulamasında Hakkında sayfası iki hizmet tarafından birleştirilmiş bir ileti görüntüler: "Hello from webfrontend and Hello from mywebapi."

### <a name="well-done"></a>Bravo!
Artık her kapsayıcının ayrı ayrı geliştirilip dağıtılabileceği çok kapsayıcılı bir uygulamanız var.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dev Spaces'te ekip geliştirme hakkında bilgi edinin](team-development-netcore.md)
