---
title: Node. js web uygulaması oluşturma-Azure App Service
description: Azure App Service bir Node. js uygulaması dağıtma
author: msangapu
ms.author: msangapu
ms.date: 08/12/2019
ms.topic: quickstart
ms.service: app-service
ms.devlang: javascript
ms.openlocfilehash: b28e8e4dccf75d36b318e838e35de23d176c5c23
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2019
ms.locfileid: "71176813"
---
# <a name="create-a-nodejs-app-in-azure"></a>Azure 'da bir Node. js uygulaması oluşturma

Azure App Service, yüksek düzeyde ölçeklenebilir, kendini yayama bir Web barındırma hizmeti sağlar. Bu hızlı başlangıçta Azure App Service için bir Node. js uygulamasının nasıl dağıtılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Azure hesabınız yoksa, herhangi bir hizmet birleşimini denemek için bugün Azure kredilerine $200 ile ücretsiz bir hesap için [kaydolun](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension) .

Node. [js ve NPM](https://nodejs.org/en/download)için Node. js Paket Yöneticisi ile birlikte yüklü [Visual Studio Code](https://code.visualstudio.com/) gerekir.

Ayrıca, bir hizmet olarak Azure platformunda (PaaS) Linux Web Apps oluşturmak, yönetmek ve dağıtmak için kullanabileceğiniz [Azure App Service uzantısını](vscode:extension/ms-azuretools.vscode-azureappservice)yüklemeniz gerekir.

### <a name="sign-in"></a>Oturum aç

Uzantı yüklendikten sonra Azure hesabınızda oturum açın. Azure **uygulama hizmeti** Gezginini göstermek Için etkinlik çubuğunda Azure logosu ' na tıklayın. **Azure 'Da oturum aç** ' a tıklayın ve yönergeleri izleyin.

![Azure 'da oturum açın](./media/quickstart-nodejs/sign-in.png)

### <a name="troubleshooting"></a>Sorun giderme

**"Adı [ABONELIK kimliği] olan abonelik bulunamıyor"** hatasını görürseniz, bunun nedeni bir ara sunucu arkasında olduğunuzdan ve Azure API 'sine ulaşamamanıza kaynaklanabilir. `HTTPS_PROXY` Kullanarak `HTTP_PROXY` terminalinizdekiproxybilgileriniziledeğişkenleriyapılandırınve`export`ortam değişkenlerini kullanın.

```sh
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Ortam değişkenlerinin ayarlanması sorunu düzeltmezse, aşağıdaki **bir sorunla karşılaştım** düğmesine tıklayarak bizimle iletişim kurun.

### <a name="prerequisite-check"></a>Önkoşul denetimi

Devam etmeden önce tüm önkoşulların yüklü ve yapılandırılmış olduğundan emin olun.

VS Code, Azure e-posta adresinizi **Azure App SERVICE** Explorer 'Da durum çubuğunda ve aboneliğinizde görmeniz gerekir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=getting-started)

## <a name="create-your-nodejs-application"></a>Node. js uygulamanızı oluşturma

Ardından, buluta dağıtılabilecek bir Node. js uygulaması oluşturun. Bu hızlı başlangıç, uygulamayı bir terminalden hızlı bir şekilde genişletmek için bir uygulama Oluşturucu kullanır.

> [!TIP]
> [Node. js öğreticisini](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)zaten tamamladıysanız, [Web sitesini dağıtmaya](#deploy-the-website)devam edebilirsiniz.

### <a name="install-the-express-generator"></a>Hızlı oluşturucuyu yükler

[Express](https://www.expressjs.com) , Node. js uygulamaları oluşturmaya ve çalıştırmaya yönelik popüler bir çerçevedir. [Hızlı Oluşturucu](https://expressjs.com/en/starter/generator.html) aracını kullanarak yeni bir Express uygulaması oluşturabilirsiniz (oluşturma). Hızlı Oluşturucu bir NPM modülü olarak gönderilir ve NPM komut satırı aracı `npm`kullanılarak yüklenir.

```bash
npm install -g express-generator
```

`-g` Anahtar, her yerden çalıştırabilmeniz için hızlı oluşturucuyu makinenize küresel olarak yükleyebilir.

### <a name="scaffold-a-new-application"></a>Yeni bir uygulamayı yapı iskelesi

Sonra, şunu çalıştırarak yeni bir Express uygulaması `myExpressApp` olan yapı iskelesi:

```bash
express myExpressApp --view pug --git
```

Parametreler, oluşturucuya [Pug](https://pugjs.org/api/getting-started.html) şablon altyapısını (eski adıyla olarak `jade`bilinen) kullanmasını ve bir `.gitignore` dosya oluşturmayı söyler. `--view pug --git`

Uygulamanın tüm bağımlılıklarını yüklemek için yeni klasöre gidin ve çalıştırın `npm install`.

```bash
cd myExpressApp
npm install
```

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Sonra, uygulamanın çalıştığından emin olun. Terminalden, sunucuyu başlatmak için `npm start` komutunu kullanarak uygulamayı başlatın.

```bash
npm start
```

Şimdi tarayıcınızı açın ve [http://localhost:3000](http://localhost:3000)' a gidin. burada şöyle bir şey görmeniz gerekir:

![Express uygulaması çalıştırılıyor](./media/quickstart-nodejs/express.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-the-website"></a>Web sitesini dağıtma

Bu bölümde, VS Code ve Azure App Service uzantısını kullanarak Node. js web sitenizi dağıtırsınız. Bu hızlı başlangıç, uygulamanızın sıkıştırılması ve Linux üzerinde bir Azure Web uygulamasına dağıtıldığı en temel dağıtım modelini kullanır.

### <a name="deploy-using-azure-app-service"></a>Azure App Service kullanarak dağıtma

İlk olarak, uygulama klasörünüzü VS Code açın.

```bash
code .
```

**Azure App SERVICE** Gezgini ' nde, uygulamanızı Azure 'a dağıtmak için mavi yukarı ok simgesine tıklayın.

![Web uygulamasına dağıtma](./media/quickstart-nodejs/deploy.png)

> [!TIP]
> Ayrıca,  ****komut paletinden** (CTRL + SHIFT + P) ' Web uygulamasına Dağıt ' yazarak ve Azure App Service çalıştırarak dağıtım yapabilirsiniz: Web App** 'e dağıtın komutu.

1. Şu anda açık `myExpressApp`olan dizini seçin.

2. **Yeni Web uygulaması oluştur**öğesini seçin.

3. Web uygulamanız için genel olarak benzersiz bir ad yazın ve ENTER tuşuna basın. Uygulama adı için geçerli karakterler şunlardır ' a-z ', ' 0-9 ' ve '-'.

4. **Node. js sürümünüzü**seçin, LTS önerilir.

    Bildirim kanalı, uygulamanız için oluşturulmakta olan Azure kaynaklarını gösterir.

Yapılandırmanızı hedef sunucuda çalışacak `npm install` şekilde güncellemek isteyip istemediğiniz sorulduğunda Evet ' e tıklayın. Uygulamanız daha sonra dağıtılır.

![Yapılandırılmış dağıtım](./media/quickstart-nodejs/server-build.png)

Dağıtım başladığında, daha sonraki dağıtımlar aynı App Service Web uygulamasını otomatik olarak hedefleyecek şekilde çalışma alanınızı güncelleştirmeniz istenir. Değişikliklerinizin doğru uygulamaya dağıtıldığından emin olmak için **Evet** ' i seçin.

![Yapılandırılmış dağıtım](./media/quickstart-nodejs/save-configuration.png)

> [!TIP]
> Uygulamanızın bağlantı noktası ortam değişkeni tarafından belirtilen bağlantı noktasında dinleme yaptığını doğrulayın: `process.env.PORT`.

### <a name="browse-the-website"></a>Web sitesine Gözat

Dağıtım tamamlandıktan sonra, yeniden dağıtılan Web sitenizi görüntülemek için istemde **Web sitesine gözatıp** ' ye tıklayın.

### <a name="troubleshooting"></a>Sorun giderme

**"Bu dizini veya sayfayı görüntüleme izniniz yok"** hatasını görürseniz, büyük olasılıkla uygulama düzgün bir şekilde başlatılamadı. Sonraki bölüme gidin ve hatayı bulmak ve onarmak için günlük çıktısını görüntüleyin. Sorunu düzeltemedi, aşağıdan **bir sorunla karşılaştım** düğmesine tıklayarak bizimle iletişim kurun. Yardımımız duyuyoruz!

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="updating-the-website"></a>Web sitesi güncelleştiriliyor

Aynı işlemi kullanarak ve yeni bir uygulama oluşturmak yerine var olan uygulamayı seçerek bu uygulamaya yapılan değişiklikleri dağıtabilirsiniz.

## <a name="viewing-logs"></a>Günlükleri görüntüleme

Bu bölümde, çalışan Web sitesindeki günlüklerin nasıl görüntüleneceğini (veya "Tail") öğreneceksiniz. Sitede yapılan `console.log` tüm çağrılar Visual Studio Code çıkış penceresinde görüntülenir.

Uygulamayı **Azure App SERVICE** Explorer 'da bulun, uygulamaya sağ tıklayın ve **akış günlüklerini görüntüle**' yi seçin.

İstendiğinde, günlüğü etkinleştirmeyi ve uygulamayı yeniden başlatmayı seçin. Uygulama yeniden başlatıldıktan sonra VS Code çıkış penceresi, günlük akışı bağlantısıyla açılır.

![Akış günlüklerini görüntüle](./media/quickstart-nodejs/view-logs.png)

![Günlüğe kaydetmeyi etkinleştirme ve yeniden başlatma](./media/quickstart-nodejs/enable-restart.png)

Birkaç saniye sonra, günlük akış hizmetine bağlı olduğunuzu belirten bir ileti görürsünüz. Daha fazla etkinlik görmek için sayfayı birkaç kez yenileyin.

    ```bash
    2019-09-20 20:37:39.574 INFO  - Initiating warmup request to container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node
    2019-09-20 20:37:55.011 INFO  - Waiting for response to warmup request for container msdocs-vscode-node_2_00ac292a. Elapsed time = 15.4373071 sec
    2019-09-20 20:38:08.233 INFO  - Container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node initialized successfully and is ready to serve requests.
    2019-09-20T20:38:21  Startup Request, url: /Default.cshtml, method: GET, type: request, pid: 61,1,7, SCM_SKIP_SSL_VALIDATION: 0, SCM_BIN_PATH: /opt/Kudu/bin, ScmType: None
    ```

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=tailing-logs)

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, bu hızlı başlangıcı başarıyla tamamladınız!

Daha sonra diğer Azure uzantılarına göz atın.

* [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
* [Azure İşlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
* [Docker Araçları](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Azure CLı araçları](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
* [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

Ya da Azure Uzantı paketi [Için düğüm paketini](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) yükleyerek tümünü alın.
