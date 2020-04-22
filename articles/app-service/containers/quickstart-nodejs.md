---
title: 'Quickstart: Bir Linux Node.js uygulaması oluşturun'
description: İlk Node.js uygulamanızı App Service'teki bir Linux konteynerine dağıtarak Azure Uygulama Hizmeti'ndeki Linux uygulamalarıyla başlayın.
author: msangapu-msft
ms.author: msangapu
ms.date: 08/12/2019
ms.topic: quickstart
ms.devlang: javascript
ms.openlocfilehash: 08a246b07effb8d5f3e2473a4d7959882cf43235
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687950"
---
# <a name="create-a-nodejs-app-in-azure"></a>Azure'da Bir Düğüm.js uygulaması oluşturma

Azure App Service, yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu hızlı başlangıç, Bir Düğüm.js uygulamasını Azure Uygulama Hizmeti'ne nasıl dağıtılanın gösterir.

## <a name="prerequisites"></a>Ön koşullar

Azure hesabınız yoksa, herhangi bir hizmet kombinasyonunu denemek için 200 TL'lik Azure kredisi içeren ücretsiz bir hesap için [bugün kaydolun.](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension)

Visual [Studio Code](https://code.visualstudio.com/) [Node.js ve npm , Node.js](https://nodejs.org/en/download)paket yöneticisi ile birlikte yüklü gerekir.

Ayrıca, Azure Platformu'nda (PaaS) Linux Web Apps'ı oluşturmak, yönetmek ve dağıtmak için kullanabileceğiniz [Azure Uygulama Hizmeti uzantısını](vscode:extension/ms-azuretools.vscode-azureappservice)da yüklemeniz gerekir.

### <a name="sign-in"></a>Oturum açma

Uzantı yüklendikten sonra Azure hesabınıza giriş yapın. Etkinlik Çubuğu'nda, **AZURE APP SERVICE** gezginini göstermek için Azure logosunu seçin. **Azure'da Oturum Aç'ı seçin...** ve yönergeleri izleyin.

![Azure'da oturum açma](./media/quickstart-nodejs/sign-in.png)

### <a name="troubleshooting"></a>Sorun giderme

**"Ad [abonelik kimliği] ile abonelik bulamıyor"** hatasını görürseniz, bunun nedeni bir proxy'nin arkasında olduğunuz ve Azure API'sine erişemediğiniz olabilir. Terminalinizdeki proxy bilgilerinizle yapılandırılan `HTTP_PROXY` ve `HTTPS_PROXY` `export`ortam değişkenlerini kullanarak yapılandırın.

```sh
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Ortam değişkenlerini ayarlamak sorunu düzeltmiyorsa, aşağıdaki sorun **düğmesine giren I'yi** seçerek bize ulaşın.

### <a name="prerequisite-check"></a>Ön koşul kontrolü

Devam etmeden önce, tüm ön koşulların yüklendiğinden ve yapılandırıldığından emin olun.

VS Code'da, Durum Çubuğu'nda Azure e-posta adresinizi ve **AZURE APP SERVICE** explorer'da aboneliğinizi görmeniz gerekir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=getting-started)

## <a name="create-your-nodejs-application"></a>Düğüm.js uygulamanızı oluşturun

Ardından, Bulut'a dağıtılabilen bir Düğüm.js uygulaması oluşturun. Bu hızlı başlatma, uygulamayı bir terminalden hızlı bir şekilde çıkarmak için bir uygulama jeneratörü kullanır.

> [!TIP]
> [Node.js eğitimini](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)tamamladıysanız, [Azure'a Dağıt'a](#deploy-to-azure)geçebilirsiniz.

### <a name="scaffold-a-new-application-with-the-express-generator"></a>Express Jeneratör ile İskele yeni bir uygulama

[Express,](https://www.expressjs.com) Node.js uygulamaları oluşturmak ve çalıştırmak için popüler bir çerçevedir. [Express Generator](https://expressjs.com/en/starter/generator.html) aracını kullanarak yeni bir Express uygulamasını iskelede (oluşturabilirsiniz) yapabilirsiniz. Ekspres Jeneratör bir npm modülü olarak sevk edilir ve doğrudan (kurulum olmadan) npm komut satırı aracı `npx`kullanılarak çalıştırılabilir.

```bash
npx express-generator myExpressApp --view pug --git
```

Parametreler, `--view pug --git` jeneratöre [pug](https://pugjs.org/api/getting-started.html) şablonu altyapısını (eskiden `jade`bilinen) kullanmasını `.gitignore` ve bir dosya oluşturmasını söyler.

Uygulamanın tüm bağımlılıklarını yüklemek için yeni klasöre gidin `npm install`ve çalıştırın.

```bash
cd myExpressApp
npm install
```

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Ardından, uygulamanın çalıştığından emin olun. Terminalden, sunucuyu başlatmak `npm start` için komutu kullanarak uygulamayı başlatın.

```bash
npm start
```

Şimdi, tarayıcınızı açın [http://localhost:3000](http://localhost:3000)ve gitmek , burada böyle bir şey görmeniz gerekir:

![Çalışan Ekspres Uygulaması](./media/quickstart-nodejs/express.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu bölümde, Node.js uygulamanızı VS Kodu ve Azure Uygulama Hizmeti uzantısını kullanarak dağıtabilirsiniz. Bu hızlı başlatma, uygulamanızın sıkıştırıldığı ve Linux'taki bir Azure Web Uygulamasına dağıtıldığı en temel dağıtım modelini kullanır.

### <a name="deploy-using-azure-app-service"></a>Azure Uygulama Hizmetini Kullanarak Dağıtma

İlk olarak, VS Code'da uygulama klasörünüzü açın.

```bash
code .
```

AZURE **APP SERVICE** explorer'da, uygulamanızı Azure'a dağıtmak için mavi yukarı ok simgesini seçin.

![Web Uygulamasına Dağıt](./media/quickstart-nodejs/deploy.png)

> [!TIP]
> Komut **Paleti'nden** (CTRL + SHIFT + P) 'web uygulamasına dağıt' yazarak ve Azure Uygulama Hizmetini çalıştırarak da dağıtabilirsiniz: Web Uygulaması **komutuna dağıtın.**

1. Şu anda açık olan dizini seçin. `myExpressApp`

1. Varsayılan olarak Linux'ta App Service'e dağıtılan **yeni Web Uygulaması Oluştur'u**seçin.

1. Web Uygulamanız için benzersiz bir ad yazın ve ENTER tuşuna basın. Bir uygulama adı için geçerli karakterler 'a-z', '0-9', ve '-'dir.

1. **Node.js sürümünüzü**seçin, LTS önerilir.

    Bildirim kanalı, uygulamanız için oluşturulan Azure kaynaklarını gösterir.

1. Hedef sunucuda çalışacak `npm install` yapılandırmanızı güncelleştirmek istendiğinde **Evet'i** seçin. Uygulamanız daha sonra dağıtılır.

    ![Yapılandırılmış dağıtım](./media/quickstart-nodejs/server-build.png)

1. Dağıtım başladığında, daha sonraki dağıtımların aynı App Service Web Uygulamasını otomatik olarak hedeflemesi için çalışma alanınızı güncelleştirmeniz istenir. Değişikliklerinizin doğru uygulamaya dağıtıldığından emin olmak için **Evet'i** seçin.

    ![Yapılandırılmış dağıtım](./media/quickstart-nodejs/save-configuration.png)

> [!TIP]
> Uygulamanızın PORT ortamı değişkeni tarafından sağlanan bağlantı noktasında `process.env.PORT`niçin dinlediğinden emin olun: .

### <a name="browse-the-app-in-azure"></a>Azure'da uygulamaya göz atın

Dağıtım tamamlandıktan sonra, yeni dağıtılan web uygulamanızı görüntülemek için anında **Web Sitesine Gözat'ı** seçin.

### <a name="troubleshooting"></a>Sorun giderme

**"Bu dizini veya sayfayı görüntüleme izniniz yok"** hatasını görürseniz, uygulama büyük olasılıkla doğru başlatılamamıştır. Bir sonraki bölüme gidin ve hatayı bulmak ve düzeltmek için günlük çıktısını görüntüleyin. Eğer bunu düzeltmek mümkün değilse, aşağıdaki bir sorun düğmesine **koştu I** seçerek bize ulaşın. Yardım etmekten mutluluk duyarım!

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="update-the-app"></a>Uygulamayı güncelleştirin

Aynı işlemi kullanarak ve yeni bir uygulama oluşturmak yerine varolan uygulamayı seçerek bu uygulamada değişiklikler dağıtabilirsiniz.

## <a name="viewing-logs"></a>Günlükleri Görüntüleme

Bu bölümde, çalışan Uygulama Hizmeti uygulamasından günlükleri nasıl görüntülediğinizi (veya "kuyruk" olarak) öğrenirsiniz. Uygulamada yapılan `console.log` tüm aramalar Visual Studio Code'daki çıkış penceresinde görüntülenir.

Uygulamayı **AZURE APP SERVICE** explorer'da bulun, uygulamayı sağ tıklatın ve Akış **Günlüklerini Görüntüle'yi**seçin.

İstendiğinde, günlüğe kaydetmeyi etkinleştirmeyi ve uygulamayı yeniden başlatmayı seçin. Uygulama yeniden başlatıldıktan sonra, log akışına bağlantı yla VS Kodu çıkış penceresi açılır.

![Akış Günlüklerini Görüntüle](./media/quickstart-nodejs/view-logs.png)

![Günlük Kaydetme ve Yeniden Başlatmayı Etkinleştir](./media/quickstart-nodejs/enable-restart.png)

Birkaç saniye sonra, günlük akışı hizmetine bağlı olduğunuzu belirten bir ileti görürsünüz. Daha fazla etkinlik görmek için sayfayı birkaç kez yenileyin.

<pre>
2019-09-20 20:37:39.574 INFO  - Initiating warmup request to container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node
2019-09-20 20:37:55.011 INFO  - Waiting for response to warmup request for container msdocs-vscode-node_2_00ac292a. Elapsed time = 15.4373071 sec
2019-09-20 20:38:08.233 INFO  - Container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node initialized successfully and is ready to serve requests.
2019-09-20T20:38:21  Startup Request, url: /Default.cshtml, method: GET, type: request, pid: 61,1,7, SCM_SKIP_SSL_VALIDATION: 0, SCM_BIN_PATH: /opt/Kudu/bin, ScmType: None
</pre>

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=tailing-logs)

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, bu hızlı başlangıcı başarıyla tamamladınız!

Ardından, diğer Azure uzantılarına göz atın.

* [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
* [Azure İşlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
* [Docker Araçları](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Azure CLI Araçları](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
* [Azure Kaynak Yöneticisi Araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

Veya Azure için [Düğüm Paketi'ni](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) yükleyerek hepsini elde edin.
