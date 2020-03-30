---
title: Azure portalından modüldağıtma - Azure IoT Edge
description: IoT Edge modülünüzü IoT Hub'ınızdan ioT Edge cihazınıza bir dağıtım bildirimi yle yapılandırılan IoT Edge cihazınıza itmek için Azure portalındaki IoT Hub'ınızı kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/30/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5c44561895bc1905328ec0eb357bee1c68a8eb55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271453"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Azure portalından Azure IoT Edge modüllerini dağıtma

İş mantığınızla IoT Edge modülleri oluşturduktan sonra, bunları kenarda çalışması için aygıtlarınıza dağıtmak istersiniz. Verileri toplamak ve işlemek için birlikte çalışan birden çok modülüz varsa, bunları aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirebilirsiniz.

Bu makalede, Azure portalının bir dağıtım bildirimi oluşturma ve dağıtımı bir IoT Edge aygıtına itme konusunda size nasıl rehberlik edemediği gösterilmektedir. Paylaşılan etiketlerine göre birden çok aygıtı hedefleyen bir dağıtım oluşturma hakkında bilgi [için](how-to-deploy-monitor.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğinizde bir [IoT hub'ı.](../iot-hub/iot-hub-create-through-portal.md)
* IoT Edge çalışma zamanı yüklü bir [IoT Edge aygıtı.](how-to-register-device.md#register-in-the-azure-portal)

## <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimini yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılabildiğini, modüller arasında verilerin nasıl aktığını ve modülün istenilen özelliklerini açıklayan bir JSON belgesidir. Dağıtım bildirimlerinin nasıl çalıştığı ve bunların nasıl oluşturulabileceği hakkında daha fazla bilgi için [bkz.](module-composition.md)

Azure portalında, JSON belgesini el ile oluşturmak yerine dağıtım bildirimini oluşturmada size yol gösteren bir sihirbaz vardır. Üç adımı vardır: **Modülekleme,** **Rotalar belirt**ve **dağıtımı gözden geçirin.**

### <a name="select-device-and-add-modules"></a>Cihazı seçin ve modül ekleyin

1. [Azure portalında](https://portal.azure.com) oturum açın ve IoT hub'ınıza gidin.
1. Sol bölmede menüden **IoT Edge'i** seçin.
1. Aygıtlar listesinden hedef aygıtın kimliğine tıklayın.
1. Üst çubuğunda **Modülleri Ayarla'yı**seçin.
1. Sayfanın **Konteyner Kayıt Defteri Ayarları** bölümünde, modül resimlerinizi içeren özel konteyner kayıtlarına erişmek için kimlik bilgilerini sağlayın.
1. Sayfanın **IoT Edge Modülleri** bölümünde **Ekle'yi**seçin.
1. Açılan menüden modül türlerine bakın:

   * **IoT Edge Modülü** - Modül adını ve konteyner görüntüsünü URI sağlarsınız. Örneğin, örnek Için görüntü URI SimüleTemperatureSensor `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0`modülü . Modül görüntüsü özel bir kapsayıcı kayıt defterinde depolanmışsa, resme erişmek için bu sayfadaki kimlik bilgilerini ekleyin.
   * **Market Modülü** - Azure Marketi'nde barındırılan modüller. Bazı pazar yeri modülleri ek yapılandırma gerektirir, bu nedenle [Azure Marketi IoT Edge Modülleri](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) listesindeki modül ayrıntılarını gözden geçirin.
   * **Azure Akış Analizi Modülü** - Bir Azure Akış Analizi iş yükünden oluşturulan modüller.

1. Modül ekledikten sonra, modül ayarlarını açmak için listeden modül adını seçin. Gerekirse isteğe bağlı alanları doldurun. Kapsayıcı oluşturma seçenekleri hakkında daha fazla bilgi için, ilkeyi yeniden başlatın ve istenen durum [edgeagent istenen özelliklere](module-edgeagent-edgehub.md#edgeagent-desired-properties)bakın. Modül ikizi hakkında daha fazla bilgi [için](module-composition.md#define-or-update-desired-properties)bkz.
1. Gerekirse, dağıtımınıza ek modüller eklemek için 5 ile 8 adımlarını yineleyin.
1. **Sonraki'ni seçin: Rotalar** bölümüne devam edecek rotalar.

### <a name="specify-routes"></a>Rotaları belirtin

**Rotalar** sekmesinde, iletilerin modüller ve IoT Hub'ı arasında nasıl geçirildiğini tanımlarsınız. İletiler ad/değer çiftleri kullanılarak oluşturulur. Varsayılan olarak bir rota **rota** olarak adlandırılır ve **FROM\* /iletiler/ INTO $upstream**olarak tanımlanır, bu da herhangi bir modül tarafından herhangi bir ileti çıkışının IoT hub'ınıza gönderildiği anlamına gelir.  

[Rotaları Bildir rotalarından](module-composition.md#declare-routes)gelen bilgilerle ekleyin veya güncelleştirin, ardından **Sonraki: Gözden Geçir +** sihirbazın bir sonraki adımına devam etmek için oluşturun'u seçin.

### <a name="review-deployment"></a>Dağıtımı gözden geçirme

İnceleme bölümü, önceki iki bölümdeki seçimlerinize göre oluşturulan JSON dağıtım bildirimini gösterir. Eklemediğinizi beyan eden iki modül olduğunu unutmayın: **$edgeAgent** ve **$edgeHub.** Bu iki modül [IoT Edge çalışma süresini](iot-edge-runtime.md) oluşturan ve her dağıtımda varsayılan olarak gereklidir.

Dağıtım bilgilerinizi gözden geçirin ve ardından **Oluştur'u**seçin.

## <a name="view-modules-on-your-device"></a>Cihazınızdaki modülleri görüntüleme

Modülleri cihazınıza dağıttıktan sonra, bunların tümlerini IoT Hub'ınızın aygıt ayrıntıları sayfasında görüntüleyebilirsiniz. Bu sayfa, dağıtılan her modülün adının yanı sıra dağıtım durumu ve çıkış kodu gibi yararlı bilgileri görüntüler.

## <a name="deploy-modules-from-azure-marketplace"></a>Azure Marketi'nden modüldağıtma

[Azure Marketi,](https://azuremarketplace.microsoft.com/) [IoT Edge modülleri](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)de dahil olmak üzere Azure'da çalışacak şekilde onaylı ve optimize edilmiş çok çeşitli kurumsal uygulamalara ve çözümlere göz atabileceğiniz çevrimiçi bir uygulama ve hizmet pazarıdır.

Azure Marketi'nden ve IoT Hub'ınızdan bir IoT Edge modülü dağıtabilirsiniz.

### <a name="deploy-from-azure-marketplace"></a>Azure Marketinden Dağıtma

IoT Edge modüllerini pazaryerinde incele ve istediğinizi bulduğunuzda **şimdi Oluştur** veya **Al'ı**seçerek dağıtabilirsiniz. Seçtiğiniz IoT Edge modülüne bağlı olarak değişebilen dağıtım sihirbazı adımlarını devam edin:

1. **Devam'ı**seçerek sağlayıcının kullanım koşullarını ve gizlilik politikasını kabul edin. Öncelikle iletişim bilgilerini vermeniz gerekebilir.
1. Aboneliğinizi ve hedef aygıtın bağlı olduğu IoT Hub'ını seçin.
1. **Aygıta Dağıt'ı**seçin.
1. Aygıtın adını girin veya hub'a kayıtlı aygıtlar arasında gezinmek için **Cihazı Bul'u** seçin.
1. İstenirse diğer modülleri ekleme de dahil olmak üzere, dağıtım bildirimini yapılandırmanın standart işlemini devam etmek için **Oluştur'u** seçin. Görüntü URI, oluşturma seçenekleri ve istenen özellikler gibi yeni modülün ayrıntıları önceden tanımlanır, ancak değiştirilebilir.

Modülün Azure portalındaki IoT Hub'ınızda dağıtılmış olduğundan doğrulayın. Cihazınızı seçin, **Modülleri Ayarla'yı** seçin ve modül **IoT Edge Modülleri** bölümünde listelenmelidir.

### <a name="deploy-from-azure-iot-hub"></a>Azure IoT Hub'ından dağıtma

Azure portalındaki IoT Hub'ınızdaki Azure Marketi'nden bir modülü cihazınıza hızla dağıtabilirsiniz.

1. Azure portalında IoT Hub'ınıza gidin.
1. Sol bölmede, **Otomatik Aygıt Yönetimi** **altında, IoT Edge'i**seçin.
1. Dağıtımı alacak olan IoT Edge aygıtını seçin.
1. Üst çubuğunda **Modülleri Ayarla'yı**seçin.
1. **IoT Edge Modülleri** bölümünde, **Ekle'yi**tıklatın ve açılan menüden **Market Modülü'nü** seçin.

![IoT Hub'ına modül ekleme](./media/how-to-deploy-modules-portal/iothub-add-module.png)

**IoT Edge Modül Pazarı** sayfasından bir modül seçin. Seçtiğiniz modül aboneliğiniz, kaynak grubunuz ve aygıtınız için otomatik olarak yapılandırılır. Daha sonra IoT Edge modülleri listenizde görünür. Bazı modüller ek yapılandırma gerektirebilir.

> [!TIP]
> Azure IoT Hub'Daki IoT Edge modülleri hakkındaki bilgiler sınırlıdır. Azure Marketi'ndeki [IoT Edge modülleri](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) hakkında daha fazla bilgi edinebilirsiniz.

**Sonraki'ni seçin: Rotaları** seçin ve bu makalenin başlarında [rotaları belirt](#specify-routes) ve [Dağıtımı Gözden Geçir'de](#review-deployment) açıklandığı şekilde dağıtıma devam edin.

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge modüllerini ölçekte nasıl dağıtılayacağım ve izleyeceğiz](how-to-deploy-monitor.md)
