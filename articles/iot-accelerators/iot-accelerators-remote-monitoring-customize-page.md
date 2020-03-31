---
title: Uzaktan İzleme çözümü Kullanıcı Arabirimi'ne sayfa ekleme - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne nasıl yeni bir sayfa ekleyeceğimiz gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.openlocfilehash: 0228f317e2d3380f2387dd557a27203eb3abc4ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240259"
---
# <a name="add-a-custom-page-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir sayfa ekleme

Bu makalede, Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne nasıl yeni bir sayfa ekleyeceğimiz gösterilmektedir. Makalede şöyle anlatılmaktadır:

- Yerel bir kalkınma ortamı nasıl hazırlanır.
- Web Web Arama Hizmeti'ne yeni bir sayfa ekleme.

Diğer nasıl yapılan dır kılavuzları eklediğiniz sayfaya daha fazla özellik eklemek için bu senaryoyu genişletir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'la ilgili kılavuzdaki adımları tamamlamak için, yerel geliştirme makinenize aşağıdaki yazılımın yüklenmesi gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="prepare-a-local-development-environment-for-the-ui"></a>UI için yerel bir geliştirme ortamı hazırlama

Uzaktan İzleme çözüm hızlandırıcı kullanıcı arabirimi kodu [React](https://reactjs.org/) JavaScript çerçevesi kullanılarak uygulanır. Kaynak kodunu Uzaktan İzleme [WebUI](https://github.com/Azure/pcs-remote-monitoring-webui) GitHub deposunda bulabilirsiniz.

UI'de değişiklik yapmak ve test etmek için, yerel geliştirme makinenizde çalıştırabilirsiniz. İsteğe bağlı olarak, yerel kopya, gerçek veya simüle edilmiş aygıtlarınizle etkileşimkurmasını sağlamak için çözüm hızlandırıcının dağıtılmış bir örneğine bağlanabilir.

Yerel geliştirme ortamınızı hazırlamak için, [Uzaktan İzleme WebUI](https://github.com/Azure/pcs-remote-monitoring-webui) deposunu yerel makinenize klonlamak için Git'i kullanın:

```cmd/sh
git clone https://github.com/Azure/pcs-remote-monitoring-webui.git
```

## <a name="add-a-page"></a>Sayfa ekleme

Web Ara Birimi'ne bir sayfa eklemek için, sayfayı tanımlayan kaynak dosyaları eklemeniz ve web UI'nin yeni sayfadan haberdar olmasını sağlamak için varolan bazı dosyaları değiştirmeniz gerekir.

### <a name="add-the-new-files-that-define-the-page"></a>Sayfayı tanımlayan yeni dosyaları ekleme

Başlamak için **src/walkthrough/components/pages/basicPage** klasörü basit bir sayfayı tanımlayan dört dosya içerir:

**basicPage.container.js**

[!code-javascript[Page container source](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.container.js?name=container "Page container source")]

**temelPage.js**

[!code-javascript[Basic page](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.js?name=page "Basic page")]

**basicPage.scss**

[!code-javascript[Page styling](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.scss?name=styles "Page styling")]

**basicPage.test.js**

[!code-javascript[Test code for basic page](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.test.js?name=test "Test code for basic page")]

Yeni bir klasör **src/components/pages/example** oluşturun ve bu dört dosyayı kopyalayın.

### <a name="add-the-new-page-to-the-web-ui"></a>Web'e yeni sayfa ekleme

Web Arama Arabirimi'ne yeni sayfa eklemek için varolan dosyalarda aşağıdaki değişiklikleri yapın:

1. **Src/components/pages/index.js** dosyasına yeni sayfa kapsayıcısını ekleyin:

    ```js
    export * from './example/basicPage.container';
    ```

1. (İsteğe bağlı)  Yeni sayfa için bir SVG simgesi ekleyin. Daha fazla bilgi için [webui/src/utilities/README.md .](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/utilities/README.md) Varolan bir SVG dosyayı kullanabilirsiniz.

1. Çeviriler dosyasına sayfa adı ekleyin, **genel/yerel/en/translations.json**. Web UI uluslararasılaşma için [i18next](https://www.i18next.com/) kullanır.

    ```json
    "tabs": {
      "basicPage": "Example",
    },
    ```

1. Üst düzey uygulama sayfasını tanımlayan **src/components/app.js** dosyasını açın. Yeni sayfayı içeri alma listesine ekleyin:

    ```javascript
    // Page Components
    import  {
      //...
      BasicPageContainer
    } from './pages';
    ```

1. Aynı dosyada, `pagesConfig` diziye yeni sayfa ekleyin. Rotanın `to` adresini ayarlayın, daha önce eklenen SVG simgesine ve `component` çevirilere başvurun ve sayfanın kapsayıcısına ayarlayın:

    ```js
    const pagesConfig = [
      //...
      {
        to: '/basicpage',
        exact: true,
        svg: svgs.tabs.example,
        labelId: 'tabs.basicPage',
        component: BasicPageContainer
      },
      //...
    ];
    ```

1. `crumbsConfig` Diziye yeni kırıntılar ekleyin:

    ```js
    const crumbsConfig = [
      //...
      {
        path: '/basicpage', crumbs: [
          { to: '/basicpage', labelId: 'tabs.basicPage' }
        ]
      },
      //...
    ];
    ```

    Bu örnek sayfada yalnızca bir kırıntı vardır, ancak bazı sayfalarda daha fazla ekmek kırıntısı olabilir.

Yaptığınız tüm değişiklikleri kaydedin. Yeni sayfanız eklenince web UI'yi çalıştırmaya hazırsınız.

### <a name="test-the-new-page"></a>Yeni sayfayı test edin

Komut istemiyle deponun yerel kopyanızın köküne gidin ve gerekli kitaplıkları yüklemek ve web web ui'sini yerel olarak çalıştırmak için aşağıdaki komutları çalıştırın:

```cmd/sh
npm install
npm start
```

Önceki komut ui'yi yerel [http://localhost:3000/dashboard](http://localhost:3000/dashboard)olarak çalışır.

Web UI'nin yerel örneğini çözüm hızlandırıcısının dağıtılmış bir örneğine bağlamadan, panoda hatalar görürsünüz. Bu hatalar yeni sayfanızı test etme yeteneğinizi etkilemez.

Artık site yerel olarak çalışırken kodu güncelleyebilir ve web Kullanıcı Arabirimi güncelleştirmesini dinamik olarak görebilirsiniz.

## <a name="optional-connect-to-deployed-instance"></a>[İsteğe bağlı] Dağıtılan örne bağlanma

İsteğe bağlı olarak, web kullanıcı arabiriminin yerel çalışan kopyasını buluttaki Uzaktan İzleme çözüm hızlandırıcısına bağlayabilirsiniz:

1. **Pccs** CLI'yi kullanarak çözüm hızlandırıcının **temel** bir örneğini dağıtın. Dağıtımınızın adını ve sanal makine için sağladığınız kimlik bilgilerini not edin. Daha fazla bilgi için [CLI'yi kullanarak Dağıt'a](iot-accelerators-remote-monitoring-deploy-cli.md)bakın.

1. SSH'nin çözümünüzdeki mikro hizmetleri barındıran sanal makineye erişimini sağlamak için Azure portalını veya [az CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) kullanın. Örnek:

    ```azurecli
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    Yalnızca test ve geliştirme sırasında SSH erişimini etkinleştirmelisiniz. SSH'yi [etkinleştiriseniz, mümkün olan en kısa sürede yeniden devre dışı bırakmanız gerekir.](../security/fundamentals/network-best-practices.md)

1. Sanal makinenizin adını ve genel IP adresini bulmak için Azure portalını veya [az CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) kullanın. Örnek:

    ```azurecli
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. Önceki adımdaki IP adresini ve çözümü dağıtmak için **bilgisayarları** çalıştırdığınızda sağladığınız kimlik bilgilerini kullanarak sanal makinenize bağlanmak için SSH'yi kullanın.

1. Yerel UX'nin bağlanmasına izin vermek için, sanal makinedeki bash kabuğunda aşağıdaki komutları çalıştırın:

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. Komutun tamamlanıp web sitesinin başladığını göredikten sonra, sanal makineyle bağlantınızı kesebilirsiniz.

1. [Uzaktan İzleme WebUI](https://github.com/Azure/pcs-remote-monitoring-webui) deposunun yerel kopyasında, dağıtılmış çözümünuzun URL'sini eklemek için **.env** dosyasını edin:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uzaktan İzleme çözüm hızlandırıcısında web Kullanıcı Arabirimi'ni özelleştirmenize yardımcı olacak kaynakları öğrendiniz.

Şimdi bir sayfa tanımlamış, bir sonraki adım UI görüntülemek için veri alır [Uzaktan İzleme çözüm hızlandırıcı web UI özel bir hizmet eklemektir.](iot-accelerators-remote-monitoring-customize-service.md)

Uzaktan İzleme çözüm hızlandırıcısı hakkında daha fazla kavramsal bilgi için uzaktan [izleme mimarisine](iot-accelerators-remote-monitoring-sample-walkthrough.md)bakın.
