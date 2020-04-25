---
title: Azure IoT Edge modül SKU 'Ları | Azure Marketi
description: IoT Edge modülü için SKU 'Lar oluşturun.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 0a394c14842fe0c4eb4baa18cbb13acb8ca3170a
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82142451"
---
# <a name="iot-edge-module-skus-tab"></a>IoT Edge modülü SKU 'Ları sekmesi

>[!Important]
>13 Nisan 2020 ' den itibaren, IoT Edge modülü tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilmiş tekliflerinizi yönetmek için [IoT Edge modülü oluşturma teklifi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-iot-edge-module-creation) bölümündeki yönergeleri izleyin.

**Yeni teklif** sayfasının **SKU 'ları** sekmesi bir veya daha fazla SKU oluşturmanıza ve bunları yeni teklifinizle ilişkilendirmenize olanak sağlar.  Bir çözümü Özellik kümelerine, faturalandırma modellerine veya başka bir özelliğe göre ayırt etmek için farklı SKU 'Lar kullanabilirsiniz.


## <a name="sku-settings"></a>SKU ayarları

Yeni bir teklif oluşturmaya başladığınızda, teklifle ilişkili hiçbir SKU yoktur. Yeni bir SKU oluşturmak için aşağıdaki adımları izleyin:

- **IoT Edge modüller > yeni teklif** sayfasında **SKU 'lar** sekmesini seçin.
- SKU 'Lar altında **+ yenı SKU** ' yı seçerek bir iletişim kutusu açın.

  ![IoT Edge modülleri için yeni teklif sekmesindeki yeni SKU düğmesi](./media/iot-edge-module-skus-tab-new-sku.png)

- **Yenı SKU** ILETIŞIM kutusunda SKU için bir tanımlayıcı girip **Tamam**' ı seçin.
(Aşağıdaki tabloda tanımlayıcı adlandırma kuralları verilmiştir.)

**SKU 'lar** sekmesi yenilenir ve SKU 'yu yapılandırmak için düzenlediğiniz alanları görüntüler. Alan adının sonuna bir yıldız işareti (*), gerekli olduğunu gösterir.

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **SKU KIMLIĞI\***       | Bu SKU için tanımlayıcı. Bu ad, küçük harfli alfasayısal karakterler veya kısa çizgilerden (-) oluşan en fazla 50 karakter içerir, ancak kısa çizgi ile bitemez. **Note:** Teklifin yayımlandıktan sonra bu adı değiştiremezsiniz. Ad, ürün URL 'Lerinde herkese açık bir şekilde görünür. |
|  |  |


## <a name="sku-details"></a>SKU ayrıntıları

SKU 'nuzun Azure Marketi 'nde ve Azure portal web sitelerinde nasıl görüntüleneceğini tanımlamak için **SKU ayrıntılarını** yapılandırın.

![IoT Edge modülü SKU meta verileri](media/iot-edge-module-skus-tab-metadata.png)

Aşağıdaki tabloda, **SKU ayrıntıları**altındaki alanlar için amaç, içerik ve biçimlendirme açıklanmaktadır. Gerekli alanlar bir yıldız işareti (*) ile tanımlanır.

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **Başlık\***        | Bu SKU için başlık. En fazla 50 karakter uzunluğunda. <br/> Azure portal gösterilir ve dağıtıldığında varsayılan bir modül adı (boşluklar ve özel karakterler olmadan) olarak kullanılacaktır. Tam olarak bu alanın nerede görüntülendiğini görmek için aşağıdaki resimlere bakın.|
| **Özet\***      | Bu SKU 'nun kısa özeti. En fazla 100 karakter uzunluğunda. Teklifi **özetleme,** yalnızca SKU.  Bu Özet Azure Marketi 'nde gösterilir. Tam olarak bu alanın nerede görüntülendiğini görmek için aşağıdaki resimlere bakın.|
| **Açıklama\***  | Bu SKU 'nun kısa açıklaması. En fazla 3000 karakter uzunluğunda. Teklifi tanımlama, ancak yalnızca bu SKU. Azure Marketi 'nde ve Azure portal görüntülenir. Azure portal, Market sekmesinde tanımlanan teklifi açıklayan Market açıklamasına eklenecektir.  SKU Özeti ile aynı olabilir. Tam olarak bu alanın nerede görüntülendiğini görmek için aşağıdaki resimlere bakın.|
| **Bu SKU 'YU gizle\*** | **Hayır**olan varsayılan ayarı koruyun. |
|  |  |


### <a name="sku-example"></a>SKU örneği

 Aşağıdaki örneklerde SKU **başlığı**, **Özeti**ve **Açıklama** alanlarının farklı görünümlerde nasıl göründüğü gösterilmektedir.
 

#### <a name="on-the-azure-marketplace-website"></a>Azure Marketi Web sitesinde:

- SKU ayrıntıları aranırken:

    ![Azure Market web sitesinde SKU 'Ları nasıl görüntülenir](media/iot-edge-module-ampdotcom-pdp-plans.png)


#### <a name="on-the-azure-portal-website"></a>Azure portal web sitesinde:

- SKU 'Lara gözatarken:

    ![IoT Edge modülün #1 Azure portal gözatarken nasıl gösterdiği](media/iot-edge-module-portal-browse.png)

    ![IoT Edge modülün #2 Azure portal gözatarken nasıl gösterdiği](media/iot-edge-module-portal-product-picker.png)

- SKU 'Lar aranırken:

    ![IoT Edge modülün Azure portal ararken nasıl gösterdiği](media/iot-edge-module-portal-search.png)

- SKU ayrıntıları aranırken:

    ![Portalda ürün ayrıntılarını ararken IoT Edge modülünün nasıl gösterdiği](./media/iot-edge-module-portal-pdp.png)

- Modülün dağıtımı sırasında:
    
    ![IoT Edge modülün dağıtıldığında nasıl gösterdiği](./media/iot-edge-module-deployment.png)


## <a name="sku-content"></a>SKU içeriği

**Edge modülü görüntüleri**altında IoT Edge modülünüzü karşıya yüklemek için ihtiyacımız olan bilgileri girin.

IoT Edge modül görüntünüzü içeren [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR) için bize erişim sağlayın ve bu sayede dosyayı onaylabiliriz. Yayımlandıktan sonra, IoT Edge modülünüzün Azure Marketi tarafından barındırılan ortak bir kapsayıcı kayıt defteri kullanılarak kopyalanıp dağıtılması gerekir.

Birden çok platformu hedefleyebilir ve Etiketler aracılığıyla çeşitli sürümler sağlayabilirsiniz. ["IoT Edge modülü teknik varlıklarınızı hazırlama" bölümünde Etiketler ve sürüm oluşturma](./cpp-create-technical-assets.md)hakkında daha fazla bilgi edinin.

![Modül görüntülerini IoT Edge](./media/iot-edge-module-skus-tab-acr.png)

Aşağıdaki tabloda, **görüntü deposu ayrıntıları** ve **görüntü sürümü**bölümlerine ilişkin alanların amacı, içerikleri ve biçimlendirmesi açıklanmaktadır.  Gerekli alanlar bir yıldız işareti (*) ile tanımlanır.


|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
|  |  ***Görüntü deposu ayrıntıları***    |
| **Abonelik KIMLIĞI\***        | ACR 'nizin Azure abonelik KIMLIĞI.|
| **Kaynak grubu adı\***      | ACR 'nizin kaynak grubu adı.|
| **Kayıt defteri adı\***  | ACR kayıt defteriniz adı. Yalnızca kayıt defteri adını, oturum açma sunucusu adını DEĞIL (örneğin, olmadan `azurecr.io`) kopyalayın |
| **Depo adı\***  | IoT Edge modülünüzü içeren ACR 'nizin Depo adı. **Note:** Ad ayarlandıktan sonra, daha sonra değiştirilemez. Hesabınızda başka hiçbir teklifin aynı ada sahip olmamasını sağlamak için benzersiz bir ad kullanın. |
| **Nitelen\*** | ACR 'iyle ilişkilendirilen Kullanıcı adı (Yönetici Kullanıcı adı). |
| **Parolayı\*** | ACR 'iyle ilişkilendirilen parola. |
|    |  ***Görüntü sürümü***   |
| **Resim etiketi veya Özeti\*** | En azından bir `latest` etiket ve sürüm etiketi içermelidir (örneğin, xx bir sayı olduğu için başlangıç `xx.xx.xx-` ). Birden çok platformu hedeflemek için [bildirim etiketleri](https://github.com/estesp/manifest-tool) olmaları gerekir. Bir bildirim etiketi tarafından başvurulan tüm Etiketler de karşıya yüklenebilmemiz için eklenmelidir. Etiketleri kullanarak bir IoT Edge modülünün birkaç sürümünü ekleyebilirsiniz. Tüm bildirim etiketleri (except `latest`), ya da `X.Y-` `X.Y.Z-` X, Y, Z tamsayılarla başlamalıdır. ["IoT Edge modülü teknik varlıklarınızı hazırlama" bölümünde Etiketler ve sürüm oluşturma](./cpp-create-technical-assets.md)hakkında daha fazla bilgi edinin. <br/> Örneğin `latest` , bir etiket `1.0.1-linux-x64`, `1.0.1-linux-arm32`,, ve `1.0.1-windows-arm32`' a işaret ediyorsa, bu 6 etiketlerinin buraya eklenmesi gerekir. |
|  |  |


### <a name="help-your-customers-launch-by-using-default-settings"></a>Varsayılan ayarları kullanarak müşterilerinizin başlatma yardımına yardımcı olma

IoT Edge modülünüzü dağıtmak için en yaygın ayarları tanımlayın. Bu varsayılan değerlerle IoT Edge modülünüzü kullanıma hazır hale getirerek müşteri dağıtımlarını iyileştirin.

![Dağıtım sırasında modül varsayılan ayarlarını IoT Edge](./media/iot-edge-module-skus-tab-iot-edge-defaults.png)

Aşağıdaki tabloda **Varsayılan rotalar**için alanların amacı, içeriği ve biçimlendirmesi, varsayılan **ikizi istenen özellikler**, varsayılan **ortam değişkenleri**ve **varsayılan createOptions**açıklanmaktadır.

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **Varsayılan yollar**        | Her varsayılan yol adı ve değeri 512 karakterden kısa olmalıdır. En fazla 5 varsayılan yol tanımlayabilirsiniz. Rota değerde doğru bir [yol sözdizimi](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) kullandığınızdan emin olun. Modülünüzü ifade etmek için varsayılan modül adını kullanın, bu, boşluk ve özel karakterler olmadan **SKU başlığınız** olacaktır. Henüz bilinmeyen diğer modüllere başvurmak için, müşterilerinizin bu bilgileri güncelleştirmesi `<FROM_MODULE_NAME>` gerektiğini bilmesini sağlamak için kuralını kullanın. [IoT Edge yollar](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes)hakkında daha fazla bilgi edinin. <br/> Örneğin, modül `ContosoModule` üzerinde `ContosoInput` giriş ve çıkış verilerini dinler `ContosoOutput`, aşağıdaki 2 varsayılan yolları tanımlamak mantıklı olur:<br/>-Ad #1:`ToContosoModule`<br/>-Değer #1:`FROM /messages/modules/<FROM_MODULE_NAME>/outputs/* INTO BrokeredEndpoint("/modules/ContosoModule/inputs/ContosoInput")`<br/>-Ad #2:`FromContosoModuleToCloud`<br/>-Değer #2:`FROM /messages/modules/ContonsoModule/outputs/ContosoOutput INTO $upstream`<br/>  |
| **Varsayılan ikizi istenen özellikler**      | Her varsayılan ikizi istenen özellikler adı ve değeri 512 karakterden kısa olmalıdır. En fazla 5 ad/değer ikizi istenen özellikleri tanımlayabilirsiniz. İkizi Desired özelliklerinin değerleri, diziler olmadan ve en fazla iç içe geçmiş 4 ' ü içeren geçerli bir JSON, kaçışsız olmalıdır. [İkizi istenen özellikler](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties)hakkında daha fazla bilgi edinin. <br/> Örneğin, bir modül ikizi istenen özellikler aracılığıyla dinamik olarak yapılandırılabilir yenileme oranını destekliyorsa, aşağıdaki varsayılan ikizi istenen özelliği tanımlamak mantıklı olur:<br/> -Ad #1:`RefreshRate`<br/>-Değer #1:`60`|
| **Varsayılan ortam değişkenleri**  | Her varsayılan ortam değişkeni adı ve değeri 512 karakterden kısa olmalıdır. En fazla 5 ad/değer ortam değişkeni tanımlayabilirsiniz. <br/>Örneğin, bir modülün başlamadan önce kullanım koşullarını kabul etmesi gerekiyorsa, aşağıdaki ortam değişkenini tanımlayabilirsiniz:<br/> -Ad #1:`ACCEPT_EULA`<br/>-Değer #1:`Y`|
| **Varsayılan createOptions**  | CreateOptions 512 karakterden kısa olmalıdır. Geçerli bir JSON, kaçışsız olmalıdır. [CreateOptions](https://docs.microsoft.com/azure/iot-edge/module-composition#configure-modules)hakkında daha fazla bilgi edinin. <br/> Örneğin, bir modül bağlantı noktası bağlamayı gerektiriyorsa, aşağıdaki createOptions tanımlayabilirsiniz:<br/>  `"HostConfig":{"PortBindings":{"5012/tcp":[{"HostPort":"5012"}]}`|
|   |   |

SKU ayarlarınızı kaydetmek için **Kaydet** ' i seçin. 


## <a name="next-steps"></a>Sonraki adımlar

Teklifiniz için Market açıklaması oluşturmak üzere [Market sekmesini](./cpp-marketplace-tab.md) kullanın.
