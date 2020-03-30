---
title: 'Quickstart: Bir Düğüm.js web uygulaması oluşturma'
description: İlk Düğüm.js Hello World'ünüzü birkaç dakika içinde Azure Uygulama Hizmetine dağıtın. Uygulama Hizmeti'ne dağıtmanın birçok yollarından biri olan Visual Studio Code'u kullanarak dağıtAbilirsiniz.
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 03/04/2020
ms.custom: mvc, devcenter, seodec18
experimental: false
experiment_id: a231f2b4-2625-4d
ms.openlocfilehash: bcb0ae2457175e7511c3539187d72d48e80baa5b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80047128"
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Azure App Service'te Node.js web uygulaması oluşturma 

Visual Studio Code kullanarak yerel olarak bir Node.js/Express uygulaması oluşturup uygulamayı buluta dağıtarak Azure Uygulama Hizmeti'ne başlayın. Ücretsiz bir Uygulama Hizmeti katmanı kullandığınızdan, bu hızlı başlangıcı tamamlamak için hiçbir ücrete tabi olmazsınız.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension)
- [Node.js ve npm](https://nodejs.org). Düğüm.js yüklü olduğunu doğrulamak için komutu `node --version` çalıştırın.
- [Görsel Stüdyo Kodu](https://code.visualstudio.com/).
- Visual Studio Code için [Azure Uygulama Hizmeti uzantısı.](vscode:extension/ms-azuretools.vscode-azureappservice)

## <a name="clone-and-run-a-local-nodejs-application"></a>Yerel bir Düğüm.js uygulamasını klonla ve çalıştırın

1. Yerel bilgisayarınızda bir terminal açın ve örnek deposunu kopyala:

    ```bash
    git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
    ```

1. Yeni uygulama klasörüne gidin:

    ```bash
    cd nodejs-docs-hello-world
    ```

1. Uygulamayı yerel olarak test etmek için başlatın:

    ```bash
    npm start
    ```
    
1. Tarayıcınızı açın ve [http://localhost:1337](http://localhost:1337)' ye gidin. Tarayıcı "Hello World!" görüntülemelidir.

1. Sunucuyu durdurmak için terminaldeki **Ctrl**+**C** tuşuna basın.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-the-app-to-azure"></a>Uygulamayı Azure’da dağıtma

Bu bölümde, Node.js uygulamanızı VS Kodu ve Azure Uygulama Hizmeti uzantısını kullanarak Azure'a dağıtabilirsiniz.

1. Terminalde, *nodejs-docs-hello-world* klasöründe olduğunuzdan emin olun ve ardından Aşağıdaki komutla Visual Studio Code'u başlatın:

    ```bash
    code .
    ```

1. VS Kodu etkinlik çubuğunda, AZURE APP **SERVICE** gezginini göstermek için Azure logosunu seçin. **Azure'da Oturum Aç'ı seçin...** ve yönergeleri izleyin. (Hatalarla karşılaştıysanız aşağıdaki [Sorun Giderme Azure oturum](#troubleshooting-azure-sign-in) açma'ya bakın.) Oturum açtıktan sonra gezgin, Azure aboneliğinizin adını göstermelidir.

    ![Azure'da oturum açma](containers/media/quickstart-nodejs/sign-in.png)

1. VS Code'un **AZURE APP SERVICE** gezgininde, uygulamanızı Azure'a dağıtmak için mavi yukarı ok simgesini seçin. (Ayrıca **Komut Paleti'nden** **(Ctrl**+**Shift**+**P)**'web uygulamasına dağıt' yazarak ve Azure Uygulama Hizmeti'ni seçerek aynı komutu çağırabilirsiniz: **Web Uygulamasına Dağıt).**

    ![Web Uygulamasına Dağıt](containers/media/quickstart-nodejs/deploy.png)
        
1. *Nodejs-docs-hello-world* klasörünü seçin.

1. Dağıtmak istediğiniz işletim sistemini temel alan bir oluşturma seçeneği seçin:

    - Linux: **Yeni Web Uygulaması Oluştur'u** seçin
    - Windows: **Yeni Web Uygulaması Oluştur'u seçin... Gelişmiş**

1. Web uygulamanız için benzersiz bir ad yazın ve **Enter**tuşuna basın. Ad tüm Azure'da benzersiz olmalı ve yalnızca alfasayısal karakterler ('A-Z', 'a-z', ve '0-9') ve tireler ('-') kullanmalıdır.

1. Linux'u hedefliyorsanız, istendiğinde bir Düğüm.js sürümü seçin. **LTS** sürümü önerilir.

1. Windows'u hedefliyorsanız, ek istemleri izleyin:
    1. **Yeni bir kaynak grubu oluştur'u**seçin, ardından kaynak `AppServiceQS-rg`grubu için bir ad girin.
    1. İşletim sistemi için **Windows'u** seçin.
    1. **Yeni Uygulama Hizmeti Planı Oluştur'u**seçin, ardından `AppServiceQS-plan`plan için bir ad girin (örneğin), ardından fiyatlandırma katmanı için **F1 Free'yi** seçin.
    1. Uygulama Öngörüleri hakkında istendiğinde **şimdilik Atla'yı** seçin.
    1. Size yakın veya erişmek istediğiniz kaynaklara yakın bir bölge seçin.

1. Tüm istemlere yanıt verdikten sonra VS Code, bildirim açılır penceresinde uygulamanız için oluşturulan Azure kaynaklarını gösterir.

    Linux'a dağıtım yaparken, hedef Linux sunucusunda çalışacak `npm install` şekilde yapılandırmanızı güncellemesi istendiğinde **Evet'i** seçin.

    ![Hedef Linux sunucusundaki yapılandırmayı güncelleştirme](containers/media/quickstart-nodejs/server-build.png)

1. Her zaman çalışma **alanı "nodejs-docs-hello-world" (uygulama adı)" dağıtmak**ile istendiğinde **Evet'i** seçin. **Evet'i** seçmek, VS Kodu'na, sonraki dağıtımlarla aynı Uygulama Hizmeti Web Uygulamasını otomatik olarak hedeflemesini söyler.

1. Linux'a dağıtım yapacaksanız, dağıtım tamamlandıktan sonra yeni dağıtılan web uygulamanızı görüntülemek için anında **Web Sitesine Gözat'ı** seçin. Tarayıcı "Merhaba Dünya!" göstermelidir

1. Windows'a dağıtım yapacaksanız, öncelikle web uygulaması için Düğüm.js sürüm numarasını ayarlamanız gerekir:

    1. VS Code'da, yeni uygulama hizmeti için düğümü genişletin, **Uygulama Ayarları'nı**sağ tıklatın ve **Yeni Ayar Ekle'yi seçin...**

        ![Uygulama ayarı komutu ekleme](containers/media/quickstart-nodejs/add-setting.png)

    1. Ayar `WEBSITE_NODE_DEFAULT_VERSION` anahtarı için girin.
    1. Ayar `10.15.2` değeri için girin.
    1. Uygulama hizmeti için düğüme sağ tıklayın ve **Yeniden Başlat'ı** seçin

        ![Uygulama hizmeti komutunu yeniden başlatın](containers/media/quickstart-nodejs/restart.png)

    1. Uygulama hizmeti için düğüme bir kez daha sağ tıklayın ve **Web Sitesine Gözat'ı**seçin.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="troubleshooting-azure-sign-in"></a>Azure oturum açma sorunu

Azure'da oturum açken **"Ad [abonelik kimliği] ile abonelik bulamıyor"** hatasını görürseniz, bunun nedeni bir proxy'nin arkasında olduğunuz ve Azure API'sine erişemediğiniz olabilir. Terminalinizdeki proxy bilgilerinizle yapılandırılan `HTTP_PROXY` ve `HTTPS_PROXY` `export`ortam değişkenlerini kullanarak yapılandırın.

```bash
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Ortam değişkenlerini ayarlamak sorunu düzeltmiyorsa, yukarıdaki **soruna göre "I"** düğmesini seçerek bize ulaşın.

### <a name="update-the-app"></a>Uygulamayı güncelleştirin

VS Code'da düzenleme yaparak, dosyalarınızı kaydederek ve yeni bir uygulama oluşturmak yerine mevcut uygulamayı seçmek yerine öncekiyle aynı işlemi kullanarak bu uygulamada değişiklikler dağıtabilirsiniz.

## <a name="viewing-logs"></a>Günlükleri Görüntüleme

Doğrudan VS Kodu çıkış `console.log`penceresinde uygulamadan günlük çıktısını (aramalar) görüntüleyebilirsiniz.

1. AZURE **APP SERVICE** explorer'da uygulama düğümüne sağ tıklayın ve **Akış Günlüklerini Başlat'ı**seçin.

    ![Akış Günlüklerini Başlat](containers/media/quickstart-nodejs/view-logs.png)

1. İstendiğinde, günlüğe kaydetmeyi etkinleştirmeyi ve uygulamayı yeniden başlatmayı seçin. Uygulama yeniden başlatıldıktan sonra, log akışına bağlantı yla VS Kodu çıkış penceresi açılır. 

    ![Günlük Kaydetme ve Yeniden Başlatmayı Etkinleştir](containers/media/quickstart-nodejs/enable-restart.png)

1. Birkaç saniye sonra, çıkış penceresi günlük akışı hizmetine bağlı olduğunuzu belirten bir ileti gösterir. Tarayıcıdaki sayfayı yenileyerek daha fazla çıktı etkinliği oluşturabilirsiniz.

    <pre>
    Connecting to log stream...
    2020-03-04T19:29:44  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours.
    Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).    
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
