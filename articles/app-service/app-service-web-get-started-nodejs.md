---
title: 'Hızlı başlangıç: node. js web uygulaması oluşturma'
description: İlk Node. js Merhaba Dünya Azure App Service dakikalar içinde dağıtın. App Service dağıtmanın birçok yolu olan Visual Studio Code kullanarak dağıtın.
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 03/04/2020
ms.custom: seodec18
experimental: false
experiment_id: a231f2b4-2625-4d
ms.openlocfilehash: 1f105792a95115580d52444a617b3fc1678843ca
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78374050"
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Azure App Service'te Node.js web uygulaması oluşturma 

Visual Studio Code kullanarak yerel olarak bir Node. js/Express uygulaması oluşturup uygulamayı buluta dağıtarak Azure App Service kullanmaya başlayın. Ücretsiz bir App Service katmanı kullandığınız için, bu hızlı başlangıcı tamamlamaya yönelik bir ücret ödeirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension).
- [Node.js ve npm](https://nodejs.org). Node. js ' nin yüklü olduğunu doğrulamak için `node --version` komutunu çalıştırın.
- [Visual Studio Code](https://code.visualstudio.com/).
- Visual Studio Code için [Azure App Service uzantısı](vscode:extension/ms-azuretools.vscode-azureappservice) .

## <a name="clone-and-run-a-local-nodejs-application"></a>Yerel bir Node. js uygulamasını kopyalama ve çalıştırma

1. Yerel bilgisayarınızda bir Terminal açın ve örnek depoyu kopyalayın:

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
    
1. Tarayıcınızı açın ve [http://localhost:1337](http://localhost:1337)gidin. Tarayıcının "Merhaba Dünya!" görüntülemesi gerekir.

1. Sunucuyu durdurmak için terminalde **Ctrl**+**C** tuşlarına basın.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-the-app-to-azure"></a>Uygulamayı Azure’da dağıtma

Bu bölümde, VS Code ve Azure App Service uzantısını kullanarak Node. js uygulamanızı Azure 'a dağıtırsınız.

1. Terminalde *NodeJS-docs-Hello-World* klasöründe olduğunuzdan emin olun ve Visual Studio Code aşağıdaki komutla başlatın:

    ```bash
    code .
    ```

1. **Azure uygulama hizmeti** Gezginini göstermek için vs Code etkinlik çubuğunda Azure logosu ' nu seçin. **Azure 'Da oturum aç...** ' ı seçin ve yönergeleri izleyin. (Hata halinde çalıştırırsanız bkz. [Azure oturum açma sorunlarını giderme](#troubleshooting-azure-sign-in) .) Oturum açıldıktan sonra gezgin, Azure aboneliğinizin adını göstermelidir.

    ![Azure'da oturum açma](containers/media/quickstart-nodejs/sign-in.png)

1. VS Code **Azure App SERVICE** Gezgini ' nde, uygulamanızı Azure 'a dağıtmak için mavi yukarı ok simgesini seçin. (' Web uygulamasına Dağıt ' yazıp **Azure App Service: Web uygulamasına dağıt**) seçeneğini belirleyerek **komut paletinden** aynı komutu da çağırabilirsiniz (**CTRL**+**SHIFT**+**P**).

    ![Web uygulamasına dağıtma](containers/media/quickstart-nodejs/deploy.png)
        
1. *NodeJS-docs-Hello-World* klasörünü seçin.

1. Dağıtmak istediğiniz işletim sistemini temel alan bir oluşturma seçeneği belirleyin:

    - Linux: **Yeni Web uygulaması oluştur** seçeneğini belirleyin
    - Windows: **Yeni Web uygulaması oluştur öğesini seçin... Gelişmiş**

1. Web uygulamanız için genel olarak benzersiz bir ad yazın ve **ENTER**tuşuna basın. Ad tüm Azure genelinde benzersiz olmalıdır ve yalnızca alfasayısal karakterler (' A-Z ', ' a-z ' ve ' 0-9 ') ve tireler ('-') kullanılmalıdır.

1. Linux hedefleniyorsa, sorulduğunda bir Node. js sürümü seçin. **LTS** sürümü önerilir.

1. Windows 'u hedefliyorsanız, ek istemleri izleyin:
    1. **Yeni kaynak grubu oluştur**' u seçin ve ardından kaynak grubu için `AppServiceQS-rg`gibi bir ad girin.
    1. İşletim sistemi için **Windows** ' u seçin.
    1. **Yeni App Service planı oluştur**' u seçin, sonra plan için bir ad girin (örneğin `AppServiceQS-plan`) ve ardından fiyatlandırma katmanı Için **F1 ücretsiz** ' i seçin.
    1. Application Insights sorulduğunda **Şu anda atla** ' yı seçin.
    1. Size yakın bir bölge veya erişmek istediğiniz kaynakları seçin.

1. Tüm istemlere yanıt verdikten sonra, VS Code bildirim açılan penceresinde uygulamanız için oluşturulmakta olan Azure kaynaklarını gösterir.

    Linux 'a dağıtım yaparken, yapılandırmayı hedef Linux sunucusunda `npm install` çalıştıracak şekilde güncelleştirmeniz istendiğinde **Evet** ' i seçin.

    ![Hedef Linux sunucusunda yapılandırmayı güncelleştirmek için sor](containers/media/quickstart-nodejs/server-build.png)

1. **Her zaman "NodeJS-docs-Hello-World" çalışma alanını (uygulama adı) "olarak dağıtmak**isteyip Istemediğiniz sorulduğunda **Evet** ' i seçin. **Evet** seçildiğinde, sonraki dağıtımlarla aynı App Service Web uygulamasını otomatik olarak hedeflemek vs Code söyler.

1. Linux 'a dağıtıyorsanız, dağıtım tamamlandıktan sonra yeniden dağıtılan Web uygulamanızı görüntülemek için istemde **Web sitesine gözatıp** ' yi seçin. Tarayıcının "Merhaba Dünya!" görüntülemesi gerekir

1. Windows 'a dağıtıyorsanız, önce Web uygulaması için Node. js sürüm numarasını ayarlamanız gerekir:

    1. VS Code ' de, yeni App Service düğümünü genişletin, **uygulama ayarları**' na sağ tıklayın ve **yeni ayar Ekle...** ' yi seçin:

        ![Uygulama ayarı komutu Ekle](containers/media/quickstart-nodejs/add-setting.png)

    1. Ayar anahtarı için `WEBSITE_NODE_DEFAULT_VERSION` girin.
    1. Ayar değeri için `10.15.2` girin.
    1. App Service düğümüne sağ tıklayın ve **Yeniden Başlat** ' ı seçin.

        ![App Service komutunu yeniden Başlat](containers/media/quickstart-nodejs/restart.png)

    1. App Service 'e daha fazla düğüm sağ tıklayın ve **Web sitesine gidin**' i seçin.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="troubleshooting-azure-sign-in"></a>Azure oturum açma sorunlarını giderme

Azure 'da oturum açarken **"adı [ABONELIK kimliği] olan abonelik bulunamıyor"** hatasını görürseniz, bunun nedeni bir ara sunucu arkasında olduğunuzdan ve Azure API 'sine ulaşamamanıza kaynaklanabilir. `export`kullanarak terminalinizdeki proxy bilgilerinizi `HTTP_PROXY` ve `HTTPS_PROXY` ortam değişkenlerini yapılandırın.

```bash
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Ortam değişkenlerinin ayarlanması sorunu düzeltmezse Yukarıdaki **bir sorunla karşılaştım** düğmesini seçerek bizimle iletişim kurun.

### <a name="update-the-app"></a>Uygulamayı güncelleştirme

VS Code düzenleme yaparak, dosyalarınızı kaydederek ve sonra yalnızca var olan uygulamayı yeni bir tane oluşturmak yerine yalnızca seçmeden aynı işlemi kullanarak bu uygulamaya yapılan değişiklikleri dağıtabilirsiniz.

## <a name="viewing-logs"></a>Günlükleri görüntüleme

Uygulamadan doğrudan VS Code çıkış penceresinde günlük çıkışını (`console.log`çağrıları) görüntüleyebilirsiniz.

1. **Azure App SERVICE** Gezgini ' nde uygulama düğümüne sağ tıklayın ve **akış günlüklerini Başlat**' ı seçin.

    ![Akış günlüklerini başlatma](containers/media/quickstart-nodejs/view-logs.png)

1. İstendiğinde, günlüğü etkinleştirmeyi ve uygulamayı yeniden başlatmayı seçin. Uygulama yeniden başlatıldıktan sonra VS Code çıkış penceresi, günlük akışı bağlantısıyla açılır. 

    ![Günlüğe kaydetmeyi etkinleştirme ve yeniden başlatma](containers/media/quickstart-nodejs/enable-restart.png)

1. Birkaç saniye sonra çıkış penceresinde, günlük akış hizmetine bağlı olduğunuzu belirten bir ileti gösterilir. Tarayıcıdaki sayfayı yenileyerek daha fazla çıkış etkinliği oluşturabilirsiniz.

    <pre>
    Connecting to log stream...
    2020-03-04T19:29:44  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours.
    Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).    
    </pre>

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
