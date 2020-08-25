---
title: Öğretici-Azure IoT Central bir video analizi oluşturma-nesne ve hareket algılama uygulaması
description: Bu öğreticide, IoT Central bir video analizi uygulamasının nasıl oluşturulacağı gösterilmektedir. Oluşturun, özelleştirin ve diğer Azure hizmetlerine bağlayın.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
author: KishorIoT
ms.author: nandab
ms.date: 07/31/2020
ms.openlocfilehash: b98406984f2c9f2adfca030369a6ea3d47a786f3
ms.sourcegitcommit: e2b36c60a53904ecf3b99b3f1d36be00fbde24fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88762786"
---
# <a name="tutorial-create-a-video-analytics---object-and-motion-detection-application-in-azure-iot-central"></a>Öğretici: Azure IoT Central bir video Analytics-nesne ve hareket algılama uygulaması oluşturma

Bir çözüm Oluşturucusu olarak, IoT Central *video analizi-nesne ve hareket algılama* uygulama şablonu, Azure IoT Edge cihazları ve Azure Media Services ile bir video analizi uygulaması oluşturmayı öğrenin. Çözüm, güvenlik kameralarını izlemek için ortak iş ihtiyacını nasıl karşılakullanacağınızı göstermek için bir perakende mağaza kullanır. Çözüm, ilginç olayları hızlı bir şekilde tanımlamak ve bulmak için bir video akışında otomatik nesne algılamayı kullanır.

Örnek uygulama, iki sanal cihaz ve bir IoT Edge ağ geçidi içerir. Aşağıdaki öğreticiler, denemek ve ağ geçidinin yeteneklerini anlamak için iki yaklaşım gösterir:

* Azure VM 'de IoT Edge ağ geçidini oluşturun ve sanal bir kamerayı bağlayın.
* IoT Edge ağ geçidini Intel NUC gibi gerçek bir cihazda oluşturun ve gerçek bir kamerayı bağlayın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure IoT Central video analizi uygulama şablonunu kullanarak bir perakende mağaza uygulaması oluşturun
> * Uygulama ayarlarını özelleştirme
> * IoT Edge ağ geçidi cihazı için bir cihaz şablonu oluşturma
> * IoT Central uygulamanıza bir ağ geçidi cihazı ekleme

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici serisini tamamlayabilmeniz için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir tane oluşturabilirsiniz.
* Gerçek bir kamera kullanıyorsanız, IoT Edge cihaz ile kamera arasında bağlantınız olması gerekir ve **gerçek zamanlı akış protokolü** kanalının olması gerekir.

## <a name="initial-setup"></a>İlk kurulum

Bu öğreticilerde, çeşitli yapılandırma dosyalarını güncelleştirir ve kullanırsınız. Bu dosyaların ilk sürümleri [LVA-Gateway](https://github.com/Azure/live-video-analytics/tree/master/ref-apps/lva-edge-iot-central-gateway) GitHub deposunda bulunur. Depo Ayrıca, dağıttığınız hizmetlerden yapılandırma değerlerini kaydetmek için indirmeniz ve kullanmanız için bir karalama yazı metni dosyası içerir.

Bu dosyaların kopyalarını kaydetmek için yerel makinenizde *LVA-yapılandırma* adlı bir klasör oluşturun. Ardından, aşağıdaki bağlantıların her birine sağ tıklayın ve dosyayı *LVA-yapılandırma* klasörüne kaydetmek Için **farklı kaydet** ' i seçin:

- [Scratchpad.txt](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/Scratchpad.txt)
- [ Üzerindedeployment.amd64.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/deployment.amd64.json)
- [ ÜzerindeLvaEdgeGatewayDcm.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/LvaEdgeGatewayDcm.json)
- [ Üzerindestate.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/state.json)

> [!NOTE]
> GitHub deposu, **Lvaedgegatewaymodule** ve **lvaYolov3** IoT Edge modüllerinin kaynak kodunu da içerir. Kaynak kodla çalışma hakkında daha fazla bilgi için bkz. [LVA Gateway modüllerini oluşturma](tutorial-video-analytics-build-module.md).

## <a name="deploy-and-configure-azure-media-services"></a>Azure Media Services dağıtma ve yapılandırma

Çözüm, IoT Edge ağ geçidi cihazı tarafından yapılan nesne algılama video kliplerini depolamak için bir Azure Media Services hesabı kullanır.

Media Services hesabı oluşturduğunuzda:

- Hesap adı, Azure aboneliği, konum, kaynak grubu ve depolama hesabı sağlamanız gerekir. Media Services hesabını oluştururken varsayılan ayarları kullanarak yeni bir depolama hesabı oluşturun.

- Konumun **Doğu ABD** bölgesini seçin.

- Media Services ve depolama hesapları için **Doğu ABD** bölgesinde *LVA-RG* adlı yeni bir kaynak grubu oluşturun. Öğreticileri tamamladığınızda, *LVA-RG* kaynak grubunu silerek tüm kaynakları kolayca kaldırabilirsiniz.

[Media Services hesabı Azure Portal](https://portal.azure.com/?r=1#create/Microsoft.MediaService)oluşturun.

> [!TIP]
> Bu öğreticiler, tüm örneklerde **Doğu ABD** bölgesini kullanır. İsterseniz en yakın bölgenizi kullanabilirsiniz.

*scratchpad.txt* dosyasında **Media Services** hesabınızın adını bir yere göz önüne alın.

Dağıtım tamamlandığında **Media Services** hesabınızın **Özellikler** sayfasına gidin. *scratchpad.txt* DOSYASıNDAKI **kaynak kimliğini** bir yere getirin, bu değeri daha sonra IoT Edge modülünü yapılandırdığınızda kullanırsınız.

Sonra, Media Services kaynağınız için bir Azure Active Directory hizmet sorumlusu yapılandırın. **API erişimi** ' ni ve ardından **hizmet sorumlusu kimlik doğrulamasını**seçin. Media Services kaynağınız ile aynı ada sahip yeni bir Azure Active Directory uygulaması oluşturun ve bir açıklama *IoT Edge erişime*sahip bir gizli dizi oluşturun.

:::image type="content" source="./media/tutorial-video-analytics-create-app/media-service-authentication.png" alt-text="AMS için AAD uygulamasını yapılandırma":::

Gizli dizi oluşturulduğunda, **hizmet sorumlusu uygulamanızı bağlamak için kimlik bilgilerinizi kopyalama** bölümüne gidin. Ardından **JSON**' ı seçin. Tüm kimlik bilgisi bilgilerini buradan bir go içinde kopyalayabilirsiniz. *scratchpad.txt* dosyasında bu bilgileri bir yere, daha sonra IoT Edge cihazını yapılandırdığınızda kullanacağınızı görürsünüz.

> [!WARNING]
> Bu, gizli dizi görüntüleme ve kaydetme şansınız olur. Kaybederseniz, başka bir gizli dizi oluşturmanız gerekir.

## <a name="create-the-azure-iot-central-application"></a>Azure IoT Central uygulamasını oluşturma

Bu bölümde, bir şablondan yeni bir Azure IoT Central uygulaması oluşturacaksınız. Bu uygulamayı, tüm bir çözüm oluşturmak için öğretici serisi boyunca kullanırsınız.

Yeni bir Azure IoT Central uygulaması oluşturmak için:

1. [Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) Web sitesine gidin.

1. Azure aboneliğinize erişmek için kullandığınız kimlik bilgileriyle oturum açın.

1. Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yapı** sayfasında **Yeni uygulama** ' yı seçin.

1. **Perakende**' ı seçin. Perakende sayfasında çeşitli perakende uygulama şablonları görüntülenir.

Yeni bir video analizi uygulaması oluşturmak için:

1. **Video analizi-nesne ve hareket algılama** uygulama şablonunu seçin. Bu şablon, öğreticide kullanılan cihazların cihaz şablonlarını içerir. Şablon, operatörlerin, kameraları izleme ve yönetme gibi görevleri gerçekleştirmek için kullanabileceği örnek panolar içerir.

1. İsteğe bağlı olarak, kolay bir **uygulama adı**seçin. Bu uygulama, Northwind Traders adlı kurgusal bir perakende mağazayı temel alır. Öğretici, *Northwind Traders video analizi* **uygulama adını** kullanır.

    > [!NOTE]
    > Kolay bir **uygulama adı**kullanırsanız, uygulama **URL 'si**için yine de benzersiz bir değer kullanmanız gerekir.

1. Azure aboneliğiniz varsa, **konum**olarak **Dizin**, **Azure aboneliğiniz**ve **Birleşik Devletler** seçin. Aboneliğiniz yoksa, **7 günlük ücretsiz denemeyi** etkinleştirebilir ve gerekli iletişim bilgilerini tamamlayabilirsiniz. Bu öğretici üç cihaz (iki kamera ve bir IoT Edge cihaz kullanır. bu nedenle, ücretsiz deneme sürümünü kullanmıyorsanız kullanım için faturalandırılırsınız.

    Dizinler, abonelikler ve konumlar hakkında daha fazla bilgi için bkz. [uygulama oluşturma hızlı](../core/quick-deploy-iot-central.md)başlangıcı.

1. **Oluştur**’u seçin.

    :::image type="content" source="./media/tutorial-video-analytics-create-app/new-application.png" alt-text="Azure IoT Central Uygulama Oluştur sayfası":::

### <a name="retrieve-the-configuration-data"></a>Yapılandırma verilerini alma

Bu öğreticinin ilerleyen kısımlarında IoT Edge ağ geçidini yapılandırdığınızda IoT Central uygulamadan bazı yapılandırma verileri gerekir. IoT Edge cihaz, uygulamasına kaydolmak ve uygulamaya bağlanmak için bu bilgiye ihtiyaç duyuyor.

**Yönetim** bölümünde, **uygulamanızı** seçin ve *scratchpad.txt* DOSYADAKI uygulama **URL** 'sini ve **uygulama kimliğini** bir yere göz önünde oluşturun:

:::image type="content" source="./media/tutorial-video-analytics-create-app/administration.png" alt-text="Yönetim":::

**API belirteçlerini** seçin ve **Işleç** rolü Için **lvaedgetoken** adlı yeni bir belirteç oluşturun:

:::image type="content" source="./media/tutorial-video-analytics-create-app/token.png" alt-text="Belirteç oluştur":::

*scratchpad.txt* dosyasındaki belirteci daha sonra bir yere unutmayın. İletişim kutusu kapatıldıktan sonra belirteci yeniden görüntüleyemezsiniz.

**Yönetim** bölümünde **cihaz bağlantısı**' nı seçin ve ardından **SAS-IoT-cihazlar**' ı seçin.

*scratchpad.txt* dosyasındaki cihazların **birincil anahtarını** bir yere unutmayın. IoT Edge cihazı yapılandırırken bu *birincil grup paylaşılan erişim imzası belirtecini* daha sonra kullanırsınız.

## <a name="edit-the-deployment-manifest"></a>Dağıtım bildirimini düzenleme

Dağıtım bildirimi kullanarak bir IoT Edge modülünü dağıtırsınız. IoT Central bildirimi bir cihaz şablonu olarak içeri aktarabilir ve ardından modül dağıtımını IoT Central yönetmenizi sağlayabilirsiniz.

Dağıtım bildirimini hazırlamak için:

1. Bir metin düzenleyicisi kullanarak *LVA-yapılandırma* klasörüne kaydettiğiniz *deployment.amd64.jsdosya üzerinde* açın.

1. `LvaEdgeGatewayModule`Aşağıdaki kod parçacığında gösterildiği gibi ayarları bulun ve görüntü adını değiştirin:

    ```json
    "LvaEdgeGatewayModule": {
        "settings": {
            "image": "mcr.microsoft.com/lva-utilities/lva-edge-iotc-gateway:1.0-amd64",
    ```

1. Bölümündeki düğümüne Media Services hesabınızın adını ekleyin `env` `LvaEdgeGatewayModule` . *scratchpad.txt* dosyasında bu hesap adının bir örneğini yaptınız:

    ```json
    "env": {
        "lvaEdgeModuleId": {
            "value": "lvaEdge"
        },
        "amsAccountName": {
            "value": "<YOUR_AZURE_MEDIA_ACCOUNT_NAME>"
        }
    }
    ```

1. Şablon bu özellikleri IoT Central kullanıma sunmaz, bu nedenle Media Services yapılandırma değerlerini dağıtım bildirimine eklemeniz gerekir. Modülün yerini bulun `lvaEdge` ve Media Services hesabınızı oluştururken *scratchpad.txt* dosyasında bir Note yaptığınız değerlerle birlikte yer tutucuları değiştirin.

    , `azureMediaServicesArmId` Media Services hesabını oluştururken *scratchpad.txt* dosyasında bir Note oluşturdunuz **kaynak kimliğidir** .

    `aadTenantId` `aadServicePrincipalAppId` `aadServicePrincipalSecret` Media Services hesabınız için hizmet sorumlusu oluştururken *scratchpad.txt* dosyasında,, ve ' yi bir yere görürsünüz:

    ```json
    {
        "lvaEdge":{
        "properties.desired": {
            "applicationDataDirectory": "/var/lib/azuremediaservices",
            "azureMediaServicesArmId": "[Resource ID from scratchpad]",
            "aadTenantId": "[AADTenantID from scratchpad]",
            "aadServicePrincipalAppId": "[AadClientId from scratchpad]",
            "aadServicePrincipalSecret": "[AadSecret from scratchpad]",
            "aadEndpoint": "https://login.microsoftonline.com",
            "aadResourceId": "https://management.core.windows.net/",
            "armEndpoint": "https://management.azure.com/",
            "diagnosticsEventsOutputName": "AmsDiagnostics",
            "operationalMetricsOutputName": "AmsOperational",
            "logCategories": "Application,Event",
            "AllowUnsecuredEndpoints": "true",
            "TelemetryOptOut": false
            }
        }
    }
    ```

1. Değişiklikleri kaydedin.

İsteğe bağlı olarak, Yolov3 modülünü Intel 'teki donanım için iyileştirilmiş [Openvino &trade; model sunucusu – Edge AI uzantı modülü](https://github.com/openvinotoolkit/model_server/tree/ams/extras/ams_wrapper) ile değiştirebilirsiniz. [deployment.openvino.amd64.js](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/deployment.openvino.amd64.json)örnek dağıtım bildirimini indirebilirsiniz. Bu bildirim modülün yapılandırma ayarlarını `lvaYolov3` aşağıdaki yapılandırmayla değiştirir:

```json
"OpenVINOModelServerEdgeAIExtensionModule": {
    "settings": {
        "image": "marketplace.azurecr.io/intel_corporation/open_vino",
        "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"4000/tcp\":[{\"HostPort\":\"4000\"}]}},\"Cmd\":[\"/ams_wrapper/start_ams.py\",\"--ams_port=4000\",\"--ovms_port=9000\"]}"
    },
    "type": "docker",
    "version": "1.0",
    "status": "running",
    "restartPolicy": "always"
}
```

## <a name="create-the-azure-iot-edge-gateway-device"></a>Azure IoT Edge ağ geçidi cihazı oluşturma

Video analizi-nesne ve hareket algılama uygulaması bir **LVA Edge nesne algılayıcısı** cihaz şablonu ve **LVA Edge hareketi algılama** cihaz şablonu içerir. Bu bölümde, dağıtım bildirimini kullanarak bir ağ geçidi cihaz şablonu oluşturur ve ağ geçidi cihazını IoT Central uygulamanıza eklersiniz.

### <a name="create-a-device-template-for-the-lva-edge-gateway"></a>LVA Edge Gateway için bir cihaz şablonu oluşturma

Dağıtım bildirimini içeri aktarmak ve **LVA Edge Gateway** cihaz şablonunu oluşturmak için:

1. IoT Central uygulamanızda **cihaz şablonları**' na gidin ve **+ Yeni**' yi seçin.

1. **Şablon türü seç** sayfasında **Azure IoT Edge** kutucuğunu seçin. Ardından **İleri: Özelleştir**' i seçin.

1. **Azure IoT Edge dağıtım bildirimini karşıya yükle** sayfasında, *LVA Edge Gateway* 'i şablon adı olarak girin ve **ağ geçidi cihazını aşağı akış cihazlarıyla**denetleyin.

    Henüz dağıtım bildirimine gözamayın. Bunu yaparsanız, Dağıtım Sihirbazı her modül için bir arabirim bekler, ancak yalnızca **Lvaedgegatewaymodule**arabirimini kullanıma sunmalıdır. Daha sonraki bir adımda bildirimi karşıya yüklersiniz.

    :::image type="content" source="./media/tutorial-video-analytics-create-app/upload-deployment-manifest.png" alt-text="Dağıtım bildirimini karşıya yükleme":::

    Şunu seçin: **İleri: Gözden Geçir**.

1. **İnceleme** sayfasında **Oluştur**' u seçin.

### <a name="import-the-device-capability-model"></a>Cihaz yetenek modeli 'ni içeri aktarma

Cihaz şablonu bir cihaz yetenek modeli içermelidir. **LVA Edge Gateway** sayfasında **içeri aktarma yetenek modeli** kutucuğunu seçin. Daha önce oluşturduğunuz *LVA-yapılandırma* klasörüne gidin ve dosyadaki *LvaEdgeGatewayDcm.js* seçin.

**LVA Edge Gateway** cihaz şablonu artık **LVA Edge Gateway modülünü** üç arabirimle birlikte içermektedir: **cihaz bilgileri**, **LVA Edge Gateway ayarları**ve **LVA Edge Gateway arabirimi**.

### <a name="replace-the-manifest"></a>Bildirimi değiştirme

**LVA Edge Gateway** sayfasında **+ bildirimi Değiştir**' i seçin.

:::image type="content" source="./media/tutorial-video-analytics-create-app/replace-manifest.png" alt-text="Bildirimi Değiştir":::

*LVA-yapılandırma* klasörüne gidin ve daha önce düzenlediğiniz bildirim dosyasında *deployment.amd64.js* seçin. **Karşıya Yükle**’yi seçin. Doğrulama tamamlandığında **Değiştir**' i seçin.

### <a name="add-relationships"></a>İlişki Ekle

**LVA Edge Gateway** cihaz şablonunda, **modüller/LVA sınır ağ geçidi modülü**altında **ilişkiler**' i seçin. **+ Ilişki Ekle** ' yi seçin ve aşağıdaki iki ilişkiyi ekleyin:

|Görünen Ad               |Name          |Hedef |
|-------------------------- |------------- |------ |
|LVA Edge hareket algılayıcısı   |Varsayılanı kullan   |LVA Edge hareket algılayıcısı cihazı |
|LVA Edge nesne algılayıcısı   |Varsayılanı kullan   |LVA Edge nesne algılayıcısı cihazı |

Sonra **Kaydet**'i seçin.

:::image type="content" source="media/tutorial-video-analytics-create-app/relationships.png" alt-text="İlişki Ekle":::

### <a name="add-views"></a>Görünümler ekleme

**LVA Edge Gateway** cihaz şablonu hiçbir görünüm tanımı içermez.

Cihaz şablonuna bir görünüm eklemek için:

1. **LVA sınır ağ geçidi** cihaz şablonunda, **Görünümler** ' e gidin ve cihaz kutucuğunu **görselleştirmeyi** seçin.

1. *LVA Edge Gateway cihazını* görünüm adı olarak girin.

1. Görünüme aşağıdaki kutucukları ekleyin:

    * **Cihaz bilgileri** özelliklerine sahip bir kutucuk: **cihaz modeli**, **üretici**, **işletim sistemi**, **Işlemci mimarisi**, **yazılım sürümü**, **toplam bellek**ve **toplam depolama alanı**.
    * **Boş belleği** ve **sistem sinyal** telemetrisi değerlerini içeren bir çizgi grafik kutucuğu.
    * Şu olaylarla bir olay geçmişi kutucuğu: **Kamera oluşturma**, **kamerayı silme**, **modül yeniden başlatma**, modül **başlatıldı**, **Modül durdu**.
    * **IoT Central Istemci durumu** telemetrisini gösteren bir 2x1 bilinen son değer kutucuğu.
    * **Modül durumu** telemetrisini gösteren 2x1 bilinen son değer kutucuğu.
    * **Sistem sinyal** telemetrisini gösteren 1x1 bilinen son değer kutucuğu.
    * **Bağlı kameraları** Telemetriyi gösteren 1x1 bilinen son değer kutucuğu.

    :::image type="content" source="media/tutorial-video-analytics-create-app/gateway-dashboard.png" alt-text="Pano":::

1. **Kaydet**’i seçin.

### <a name="publish-the-device-template"></a>Cihaz şablonunu yayımlama

Uygulamaya bir cihaz ekleyebilmeniz için önce cihaz şablonunu yayımlamanız gerekir:

1. **LVA sınır ağ geçidi** cihaz şablonunda **Yayımla**' yı seçin.

1. **Bu cihaz şablonunu uygulama üzerinde Yayımla** sayfasında **Yayımla**' yı seçin.

**LVA Edge Gateway** artık, uygulamadaki **cihazlar** sayfasında kullanılacak cihaz türü olarak kullanılabilir.

## <a name="add-a-gateway-device"></a>Ağ geçidi cihazı ekleme

Uygulamaya **LVA Edge Gateway** cihazı eklemek için:

1. **Cihazlar** sayfasına gidin ve **LVA Edge Gateway** cihaz şablonunu seçin.

1. **+ Yeni** seçeneğini belirleyin.

1. **Yeni bir cihaz oluştur** iletişim kutusunda cihaz adını *LVA Gateway 001*olarak değiştirin ve cihaz kimliğini *LVA-Gateway-001*olarak değiştirin.

    > [!NOTE]
    > Cihaz KIMLIĞI uygulamada benzersiz olmalıdır.

1. **Oluştur**’u seçin.

Cihaz durumu **kayıtlı**.

### <a name="get-the-device-credentials"></a>Cihaz kimlik bilgilerini al

Cihazın IoT Central uygulamanıza bağlanmasına izin veren kimlik bilgileri gerekir. Cihaz kimlik bilgilerini al:

1. **Cihazlar** sayfasında, oluşturduğunuz **LVA-Gateway-001** cihazını seçin.

1. **Bağlan**’ı seçin.

1. **Cihaz bağlantısı** sayfasında, **kimlik KAPSAMı**, **cihaz kimliği**ve cihaz **birincil anahtarı** *scratchpad.txt* dosyasında bir Note olun. Bu değerleri daha sonra kullanırsınız.

1. Bağlantı yönteminin, **paylaşılan erişim imzası**olarak ayarlandığından emin olun.

1. **Kapat**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Artık **video analizi-nesne ve hareket algılama** uygulama şablonunu kullanarak bir IoT Central uygulaması oluşturdunuz, ağ geçidi cihazı için bir cihaz şablonu oluşturdunuz ve uygulamaya bir ağ geçidi cihazı eklediniz.

Video analizi-nesne ve hareket algılama uygulamasını, sanal bir sanal makineyi benzetimli video akışlarıyla çalıştıran IoT Edge modüller kullanarak denemek istiyorsanız:

> [!div class="nextstepaction"]
> [Video analizi için IoT Edge örneği oluşturma (Linux VM)](./tutorial-video-analytics-iot-edge-vm.md)

**Gerçek bir** cihaz ile gerçek zamanlı olarak çalışan IoT Edge modüller kullanarak video analizi-nesne ve hareket algılama uygulamasını denemek istiyorsanız:

> [!div class="nextstepaction"]
> [Video analizi için IoT Edge örneği oluşturma (Intel NUC)](./tutorial-video-analytics-iot-edge-nuc.md)
