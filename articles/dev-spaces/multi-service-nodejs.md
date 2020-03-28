---
title: 'Birden çok bağımlı hizmet çalıştırma: Node.js & Visual Studio Code'
services: azure-dev-spaces
ms.date: 11/21/2018
ms.topic: tutorial
description: Bu öğretici, Azure Kubernetes Hizmeti'nde çok hizmetli bir Düğüm düğüm uygulamasını hata ayıklamak için Azure Dev Spaces ve Visual Studio Kodunu nasıl kullanacağınızı gösterir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
ms.openlocfilehash: a5fa0aae3a966dd96ee95e6bcafc3b2eec4e6837
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75438302"
---
# <a name="running-multiple-dependent-services-nodejs-and-visual-studio-code-with-azure-dev-spaces"></a>Birden çok bağımlı hizmet çalıştırma: Node.js ve Azure Dev Spaces ile Visual Studio Kodu

Bu eğitimde, Dev Spaces'in sağladığı bazı ek avantajlarla birlikte Azure Dev Spaces'i kullanarak çok hizmetli uygulamaların nasıl geliştireceğinizi öğreneceksiniz.

## <a name="call-a-service-running-in-a-separate-container"></a>Ayrı kapsayıcıda çalıştırılan hizmeti çağırma

Bu bölümde `mywebapi` adlı ikinci bir hizmet oluşturacak ve bu hizmetin `webfrontend` tarafından çağrılmasını sağlayacaksınız. Her hizmet ayrı kapsayıcılarda çalışır. Ardından her iki kapsayıcıda da hata ayıklayacaksınız.

![](media/common/multi-container.png)

### <a name="open-sample-code-for-mywebapi"></a>*mywebapi* için örnek kodu açma
`mywebapi` Bu kılavuz için örnek kodu zaten adlı `samples` bir klasörün altında https://github.com/Azure/dev-spaces olmalıdır (değilse, GitHub deposunu indirmek için **Clone veya Download'a** gidin veya indirin'i seçin.) Bu bölümün `samples/nodejs/getting-started/mywebapi`kodu .

### <a name="run-mywebapi"></a>*mywebapi* hizmetini çalıştırın
1. *Ayrı bir VS Code penceresinde*`mywebapi` klasörünü açın.
1. **Komut Paleti**'ni açın (**Görünüm | Komut Paleti** menüsünü kullanarak) ve otomatik tamamlama özelliğini kullanarak komutu yazın ve seçin: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Bu komut, projeyi dağıtım için yapılandıran `azds prep` komutuyla karıştırılmamalıdır.
1. F5'e bastıktan sonra hizmetin oluşturulup dağıtılmasını bekleyin. Hata ayıklama konsolunda dinleme *bağlantı noktası 80* iletisi göründüğünde hazır olduğunu anlarsınız.
1. Uç nokta URL'sini not alın; `http://localhost:<portnumber>` benzeri bir URL olacaktır. **İpucu: VS Kodu durum çubuğu turuncuya döner ve tıklanabilir bir URL görüntüler.** Kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte Azure’daki geliştirme ortamınızda çalışıyordur. Bunun localhost adresi olmasının nedeni `mywebapi` hizmetinin hiçbir genel uç nokta tanımlamamış olması ve buna yalnızca Kubernetes örneğinin içinden erişilebilmesidir. Size rahatlık sağlamak ve yerel makinenizden özel hizmetle etkileşimi kolaylaştırmak için, Azure Dev Spaces Azure'da çalıştırılan kapsayıcıya geçici bir SSH tüneli oluşturur.
1. `mywebapi` hazır olduğunda, tarayıcınızda localhost adresini açın. `mywebapi` hizmetinden bir yanıt görmeniz gerekir ("Hello from mywebapi").


### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend*’den *mywebapi*’ye istek gönderme
Şimdi `webfrontend` uygulamasında `mywebapi` hizmetine istek gönderen bir kod yazalım.
1. `webfrontend` için VS Code penceresine geçin.
1. Şu kod satırlarını `server.js` dosyasının en üstüne ekleyin:
    ```javascript
    var request = require('request');
    ```

3. `/api` GET işleyicisinin kodunu *değiştirin*. İsteği işlerken, o da `mywebapi` hizmetine bir çağrı yapar ve her iki hizmetten de sonuçları döndürür.

    ```javascript
    app.get('/api', function (req, res) {
       request({
          uri: 'http://mywebapi',
          headers: {
             /* propagate the dev space routing header */
             'azds-route-as': req.headers['azds-route-as']
          }
       }, function (error, response, body) {
           res.send('Hello from webfrontend and ' + body);
       });
    });
    ```
   1. `server.js` sonundaki `server.close()` satırını *kaldırma*

Önceki kod örneğinde `azds-route-as` üst bilgisi gelen istekten giden isteğe iletilmektedir. Daha sonra bunun takımlara işbirliğine dayalı geliştirme açısından nasıl yardımcı olduğunu göreceksiniz.

### <a name="debug-across-multiple-services"></a>Birden çok hizmette hata ayıklama
1. Bu noktada, `mywebapi` hizmetinin hata ayıklayıcısı ekli bir şekilde çalışmaya devam ediyor olması gerekir. Devam etmiyorsa, `mywebapi` projesinde F5'e basın.
1. 8. [satırında `server.js` ](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/mywebapi/server.js#L8)varsayılan `/` GET işleyicisinin içinde bir kesme noktası ayarlama.
1. `webfrontend` projesinde, `http://mywebapi` konumuna GET isteği göndermeden hemen önce bir kesme noktası ayarlayın.
1. `webfrontend` projesinde F5'e basın.
1. Web uygulamasını açın ve her iki hizmette de kodun üzerinden geçin. Web uygulamasında iki hizmet tarafından birleştirilmiş bir ileti görüntüleniyor olmalıdır: "Hello from webfrontend and Hello from mywebapi."

### <a name="well-done"></a>Bravo!
Artık her kapsayıcının ayrı ayrı geliştirilip dağıtılabileceği çok kapsayıcılı bir uygulamanız var.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dev Spaces'te ekip geliştirme hakkında bilgi edinin](team-development-nodejs.md)
