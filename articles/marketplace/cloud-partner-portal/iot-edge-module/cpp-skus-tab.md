---
title: Azure IoT Edge modülü SKUs' ları | Azure Marketi
description: Bir IoT Edge modülü için SUS oluşturun.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: dsindona
ms.openlocfilehash: 49ae69263735d6ec35cd911a20100472b32c0b59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286566"
---
# <a name="iot-edge-module-skus-tab"></a>IoT Edge modülü SUs sekmesi

**Yeni Teklif** sayfasının **STU** sekmesi, bir veya daha fazla SNU oluşturmanıza ve bunları yeni teklifinize ilişkilendirmenize olanak tanır.  Bir çözümü özellik kümelerine, faturalandırma modellerine veya başka bir özelliğe göre ayırt etmek için farklı SCO'lar kullanabilirsiniz.


## <a name="sku-settings"></a>SKU Ayarları

Yeni bir teklif oluşturmaya başladığınızda, teklifle ilişkili herhangi bir STU yoktur. Yeni bir SKU oluşturmak için aşağıdaki adımları izleyin:

- **IoT Edge Modülleri > Yeni Teklif** sayfasında **SK'ler** sekmesini seçin.
- SKU'lar altında iletişim kutusunu açmak için **+ Yeni SKU'yu** seçin.

  ![IoT Edge modülleri için Yeni Teklif sekmesinde yeni SKU düğmesi](./media/iot-edge-module-skus-tab-new-sku.png)

- Yeni **SKU** iletişim kutusunda, SKU için bir tanımlayıcı girin ve ardından **Tamam'ı**seçin.
(Aşağıdaki tablo tanımlayıcı adlandırma kuralları verir.)

**SKU** sekmesi yenilenir ve SKU'yu yapılandırmak için ayarladığınız alanları görüntüler. Alan adına eklenen bir yıldız işareti (*) bunun gerekli olduğunu gösterir.

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **SKU KİmLİk\***       | Bu SKU için tanımlayıcı. Bu ad, küçük alfasayısal karakterler veya tirelerden (-) oluşan en fazla 50 karaktere sahiptir, ancak tire ile son olarak dönememez. **Not:** Teklif yayınlandıktan sonra bu adı değiştiremezsiniz. Ad, ürün URL'lerinde genel olarak görülebilir. |
|  |  |


## <a name="sku-details"></a>SKU Detayları

**SKU'nuzun** Azure Marketi ve Azure Portalı web sitelerinde nasıl görüntüleneceğini tanımlamak için SKU Ayrıntılarını yapılandırın.

![IoT Edge modülü sku meta data](media/iot-edge-module-skus-tab-metadata.png)

Aşağıdaki **tabloda, SKU Ayrıntıları**altındaki alanların amacı, içeriği ve biçimlendirmesi açıklanmaktadır. Gerekli alanlar yıldız işareti (*) tarafından suçlanır.

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **Başlık\***        | Bu SKU için başlık. Maksimum uzunluğu 50 karakter. <br/> Azure Portalı'nda gösterilir ve dağıtıldığında varsayılan modül adı (boşluk lar ve özel karakterler olmadan) olarak kullanılır. Bu alanın tam olarak nerede görüntülendiğini görmek için aşağıdaki resimlere bakın.|
| **Özet\***      | Bu SKU kısa özeti. Maksimum uzunluğu 100 karakter. **Teklifi özetlemeyin,** sadece SKU.  Bu özet Azure Marketi'nde gösterilir. Bu alanın tam olarak nerede görüntülendiğini görmek için aşağıdaki resimlere bakın.|
| **Açıklama\***  | Bu SKU kısa açıklaması. Maksimum uzunluğu 3000 karakter. Teklif ama sadece bu SKU tarif ETMEYIN. Azure marketinde ve Azure portalında gösterilir. Azure portalında, Market sekmesinde tanımlanan teklifi açıklayan Market Açıklaması'na eklenir.  SKU Özeti ile aynı olabilir. Bu alanın tam olarak nerede görüntülendiğini görmek için aşağıdaki resimlere bakın.|
| **Bu SKU'yı sakla\*** | Varsayılan ayarı tutun, hangi **Hayır**. |
|  |  |


### <a name="sku-example"></a>SKU örneği

 Aşağıdaki örnekler, SKU **Başlık**, **Özet**ve **Açıklama** alanlarının farklı görünümlerde nasıl göründüğünü gösterir.
 

#### <a name="on-the-azure-marketplace-website"></a>Azure Marketi web sitesinde:

- SKU detaylarına bakarken:

    ![Azure Marketi web sitesinde SK'ler nasıl görüntülenir?](media/iot-edge-module-ampdotcom-pdp-plans.png)


#### <a name="on-the-azure-portal-website"></a>Azure Portal web sitesinde:

- SK'lere göz atarken:

    ![Azure portalında gezinirken IoT Edge modülü nasıl #1](media/iot-edge-module-portal-browse.png)

    ![Azure portalında gezinirken IoT Edge modülü nasıl #2](media/iot-edge-module-portal-product-picker.png)

- SK'lar ararken:

    ![Azure portalında arama yaparken IoT Edge modülü nasıl açılır?](media/iot-edge-module-portal-search.png)

- SKU detaylarına bakarken:

    ![Portaldaki ürün ayrıntılarına bakarken IoT Edge modülü nasıl açılır?](./media/iot-edge-module-portal-pdp.png)

- Modülü dağıtırken:
    
    ![Dağıtılırken IoT Edge modülü nasıl açılır?](./media/iot-edge-module-deployment.png)


## <a name="sku-content"></a>SKU içeriği

**Edge Modül Görüntüleri**altında, IoT Edge modülünüzü yüklemek için ihtiyacımız olan bilgileri sağlayın.

IoT Edge modül resminizi içeren [Azure Konteyner Kayıt Defterinize](https://azure.microsoft.com/services/container-registry/) (ACR) erişmemize izin verin, böylece onu yükleyip sertifikalayabilelim. Yayımlandıktan sonra, IoT Edge modülünüz Azure Marketi tarafından barındırılan genel bir konteyner kayıt defteri kullanılarak kopyalanır ve dağıtılır.

Birden çok platformu hedefleyebilir ve etiketler aracılığıyla çeşitli sürümler sağlayabilirsiniz. Etiketler ve sürüm ler hakkında daha fazla bilgi için ["IoT Edge modülü teknik varlıklarınızı hazırlayın" adresinden](./cpp-create-technical-assets.md)edinebilirsiniz.

![IoT Edge Modül Görüntüleri](./media/iot-edge-module-skus-tab-acr.png)

Aşağıdaki tabloda, **Resim Deposu Ayrıntıları** ve Resim **Sürümü**bölümleri için alanların amacı, içeriği ve biçimlendirmesi açıklanmaktadır.  Gerekli alanlar yıldız işareti (*) tarafından suçlanır.


|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
|  |  ***Resim Deposu Ayrıntıları***    |
| **Abonelik Kimliği\***        | ACR'nizin Azure abonelik kimliği.|
| **Kaynak grubu adı\***      | ACR'nizin kaynak grubu adı.|
| **Kayıt defteri adı\***  | ACR kayıt adınız. Sadece kayıt defteri adını kopyalamak, DEĞIL oturum açma sunucusu `azurecr.io`adı (örneğin, olmadan .) |
| **Depo adı\***  | IoT Edge modülünüzü içeren ACR'nizin depo adı. **Not:** Ad ayarlandıktan sonra değiştirilemez. Hesabınızdaki başka hiçbir teklifin aynı ada sahip olmadığından emin olmak için benzersiz bir ad kullanın. |
| **Username\*** | ACR 'nizle ilişkili kullanıcı adı (yönetici kullanıcı adı). |
| **Parola\*** | ACR'nizle ilişkili parola. |
|    |  ***Resim Sürümü***   |
| **Resim Etiketi veya Özeti\*** | En azından bir `latest` etiket ve sürüm etiketi içermelidir `xx.xx.xx-` (örneğin, xx'in bir sayı olduğu yerden başlayarak). Bunlar birden çok platformu hedeflemek için [açık etiketler](https://github.com/estesp/manifest-tool) olmalıdır. Bir bildirim etiketiyle başvurulan tüm etiketlerde de eklenmelidir, böylece bunları yükleyebiliriz. Etiketleri kullanarak bir IoT Edge modülünün çeşitli sürümlerini ekleyebilirsiniz. Tüm bildirim etiketleri `latest`(hariç) `X.Y-` `X.Y.Z-` ya da X, Y, Z'nin tümseger olduğu yerde başlamalıdır. Etiketler ve sürüm ler hakkında daha fazla bilgi için ["IoT Edge modülü teknik varlıklarınızı hazırlayın" adresinden](./cpp-create-technical-assets.md)edinebilirsiniz. <br/> `latest` Örneğin, bir etiket bu işaret `1.0.1-linux-x64`, `1.0.1-linux-arm32`, `1.0.1-windows-arm32`, ve , bu 6 etiketleri burada eklenmesi gerekir. |
|  |  |


### <a name="help-your-customers-launch-by-using-default-settings"></a>Varsayılan ayarları kullanarak müşterilerinizin başlatmaya yardımcı olun

IoT Edge modülünüzü dağıtmak için en yaygın ayarları tanımlayın. Bu varsayılanlarla IoT Edge modülünüzü kullanıma sunmasına izin vererek müşteri dağıtımlarını optimize edin.

![Dağıtımda IoT Edge modülü varsayılan ayarları](./media/iot-edge-module-skus-tab-iot-edge-defaults.png)

Aşağıdaki tabloda **Varsayılan Yollar, Varsayılan** **ikiz istenen özellikler,** **Varsayılan ortam değişkenleri**ve Varsayılan **CreateOptions**için alanların amacı, içeriği ve biçimlendirmeaçıklanır.

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **Varsayılan yollar**        | Her varsayılan rota adı ve değeri 512 karakterden az olmalıdır. En fazla 5 varsayılan rota tanımlayabilirsiniz. Rota değerinizde doğru bir [rota sözdizimini](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) kullandığınızdan emin olun. Modülünüze başvurmak için, boşluk ve özel karakter içermeyen **SKU Unvanınız** olacak varsayılan modül adını kullanın. Henüz bilinmeyen diğer modüllere başvurmak `<FROM_MODULE_NAME>` için, müşterilerinize bu bilgileri güncelleştirmeleri gerektiğini bildirmek için kuralı kullanın. [IoT Edge rotaları](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes)hakkında daha fazla bilgi edinin. <br/> Örneğin, modül `ContosoModule` veri girişlerini `ContosoInput` ve çıktılarını `ContosoOutput`dinliyorsa, aşağıdaki 2 varsayılan yolu tanımlamak mantıklıdır:<br/>- Adı #1:`ToContosoModule`<br/>- Değer #1:`FROM /messages/modules/<FROM_MODULE_NAME>/outputs/* INTO BrokeredEndpoint("/modules/ContosoModule/inputs/ContosoInput")`<br/>- Ad #2:`FromContosoModuleToCloud`<br/>- Değer #2:`FROM /messages/modules/ContonsoModule/outputs/ContosoOutput INTO $upstream`<br/>  |
| **Varsayılan ikiz istenilen özellikler**      | Her varsayılan ikiz istenilen özellikleri adı ve değeri 512 karakterden az olmalıdır. En fazla 5 ad/değer ikizi istenen özellikleri tanımlayabilirsiniz. İkiz istenilen özelliklerin değerleri, diziler olmadan ve en fazla 4 iç içe hiyerarşisi olan json, kaçamayan geçerli olmalıdır. [İstenilen ikiz özellikler](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties)hakkında daha fazla bilgi edinin. <br/> Örneğin, bir modül istenen ikiz özellikler aracılığıyla dinamik olarak yapılandırılabilir yenileme hızını destekliyorsa, aşağıdaki varsayılan ikiz istenen özelliği tanımlamak mantıklıdır:<br/> - Adı #1:`RefreshRate`<br/>- Değer #1:`60`|
| **Varsayılan ortam değişkenleri**  | Her varsayılan ortam değişkenleri adı ve değeri 512 karakterden az olmalıdır. En fazla 5 ad/değer ortamı değişkeni tanımlayabilirsiniz. <br/>Örneğin, bir modül başlatılmadan önce kullanım koşullarını kabul etmeyi gerektiriyorsa, aşağıdaki ortam değişkenini tanımlayabilirsiniz:<br/> - Adı #1:`ACCEPT_EULA`<br/>- Değer #1:`Y`|
| **Varsayılan createOptions**  | createOptions 512 karakterden az olmalıdır. JSON geçerli olmalı, kaçamayan. [CreateOptions](https://docs.microsoft.com/azure/iot-edge/module-composition#configure-modules)hakkında daha fazla bilgi edinin. <br/> Örneğin, bir modül bağlantı noktasını bağlamayı gerektiriyorsa, aşağıdaki createOptions'ı tanımlayabilirsiniz:<br/>  `"HostConfig":{"PortBindings":{"5012/tcp":[{"HostPort":"5012"}]}`|
|   |   |

SKU ayarlarınızı kaydetmek için **Kaydet'i** seçin. 


## <a name="next-steps"></a>Sonraki adımlar

Teklifiniz için pazar yeri açıklaması oluşturmak için [Market sekmesini](./cpp-marketplace-tab.md) kullanın.
