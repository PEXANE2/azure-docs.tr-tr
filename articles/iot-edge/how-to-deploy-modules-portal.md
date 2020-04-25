---
title: Azure portal Azure IoT Edge 'dan modül dağıtma
description: Bir IoT Edge modülünü IoT Hub bir dağıtım bildirimiyle yapılandırıldığı şekilde IoT Edge cihazınıza iletmek için Azure portal IoT Hub kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/30/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 754c106db42f3f0695ad023e736993bee82e9757
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133931"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Azure portal Azure IoT Edge modüllerini dağıtma

İş mantığınızla IoT Edge modüller oluşturduktan sonra, bunları kenarda çalıştırmak için cihazlarınıza dağıtmak istersiniz. Verileri toplamak ve işlemek için birlikte çalışan birden fazla modülünüz varsa, bunları tek seferde dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirebilirsiniz.

Bu makalede, Azure portal dağıtım bildirimi oluşturma ve dağıtımı bir IoT Edge cihazına gönderme işlemlerinde nasıl kılavuzluk gösterilmektedir. Paylaşılan etiketlerine göre birden çok cihazı hedefleyen bir dağıtım oluşturma hakkında bilgi için, bkz. [IoT Edge modüllerini ölçeklendirerek dağıtma ve izleme](how-to-deploy-at-scale.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğinizdeki bir [IoT Hub 'ı](../iot-hub/iot-hub-create-through-portal.md) .
* IoT Edge çalışma zamanı yüklü [IoT Edge bir cihaz](how-to-register-device.md#register-in-the-azure-portal) .

## <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimi yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılacağını, modüller arasında verilerin nasıl akacağını ve modül TWINS 'in istenen özelliklerini tanımlayan bir JSON belgesidir. Dağıtım bildirimlerinin nasıl çalıştığı ve nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [IoT Edge modüllerinin nasıl kullanılabileceğini, yapılandırılacağını ve yeniden kullanıldığını anlayın](module-composition.md).

Azure portal, JSON belgesini el ile oluşturmak yerine dağıtım bildirimini oluşturma konusunda size yol gösteren bir sihirbaza sahiptir. Üç adım vardır: **modüller ekleme**, **rotalar belirtme**ve **dağıtımı İnceleme**.

### <a name="select-device-and-add-modules"></a>Cihaz seçin ve modül ekleyin

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub 'ınıza gidin.
1. Sol bölmede, menüden **IoT Edge** ' yi seçin.
1. Cihaz listesinden hedef cihazın KIMLIĞINE tıklayın.
1. Üstteki çubukta **modülleri ayarla**' yı seçin.
1. Sayfanın **Container Registry ayarları** bölümünde, modül görüntülerinizi içeren özel kapsayıcı kayıt defterlerine erişim için kimlik bilgilerini sağlayın.
1. Sayfanın **IoT Edge modüller** bölümünde **Ekle**' yi seçin.
1. Açılan menüden modül türlerine bakın:

   * **IoT Edge modülü** -modül adı ve KAPSAYıCı görüntüsü URI 'sini sağlarsınız. Örneğin, örnek SimulatedTemperatureSensor modülünün görüntü URI 'SI `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0`. Modül görüntüsü bir özel kapsayıcı kayıt defterinde depolanıyorsa, görüntüye erişmek için bu sayfadaki kimlik bilgilerini ekleyin.
   * **Market modülü** -Azure Marketi 'nde barındırılan modüller. Bazı Market modülleri ek yapılandırma gerektirir, bu nedenle [Azure marketi IoT Edge modüller](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) listesindeki modül ayrıntılarını gözden geçirin.
   * **Azure Stream Analytics modülü** -bir Azure Stream Analytics iş yüküyle oluşturulan modüller.

1. Modül eklendikten sonra, modül ayarlarını açmak için listeden modül adını seçin. Gerekirse isteğe bağlı alanları doldurun. Kapsayıcı oluşturma seçenekleri hakkında daha fazla bilgi için, ilkeyi yeniden başlatın ve istenen durum bkz. [Edgeagent istenen özellikler](module-edgeagent-edgehub.md#edgeagent-desired-properties). Modül ikizi hakkında daha fazla bilgi için bkz. [istenen özellikleri tanımlama veya güncelleştirme](module-composition.md#define-or-update-desired-properties).
1. Gerekirse, dağıtımınıza ek modüller eklemek için 5 ' ten 8 ' e kadar olan adımları yineleyin.
1. **İleri ' yi seçin:** rotalar bölümüne devam etmek için yollar.

### <a name="specify-routes"></a>Rotaları belirtin

**Rotalar** sekmesinde, iletilerin modüller arasında nasıl geçtiğini tanımlar ve IoT Hub. İletiler ad/değer çiftleri kullanılarak oluşturulur. Varsayılan olarak bir rota **route** olarak adlandırılır ve **/ileti/\* **olarak tanımlanır $upstream. Bu, herhangi bir modülle giden tüm iletilerin IoT Hub 'ınıza gönderildiği anlamına gelir.  

Bilgileri [bildirme rotalarındaki](module-composition.md#declare-routes)bilgilerle ekleyin veya güncelleştirin, ardından İleri ' yi seçin. sihirbazın sonraki adımına devam etmek için ileri ' ye tıklayın ve **Oluştur** ' a tıklayın.

### <a name="review-deployment"></a>Dağıtımı gözden geçir

İnceleme Bölümü, önceki iki bölümdeki seçimlerinize göre oluşturulan JSON dağıtım bildirimini gösterir. **$EdgeAgent** ve **$edgeHub**eklemediğiniz iki modül olduğunu unutmayın. Bu iki modül [IoT Edge çalışma zamanını](iot-edge-runtime.md) yapar ve her dağıtımda gerekli varsayılanlar olur.

Dağıtım bilgilerinizi gözden geçirin ve ardından **Oluştur**' u seçin.

## <a name="view-modules-on-your-device"></a>Cihazınızdaki modülleri görüntüleme

Modülleri cihazınıza dağıttıktan sonra, bunların tümünü IoT Hub cihaz ayrıntıları sayfasında görüntüleyebilirsiniz. Bu sayfada dağıtılan her modülün adı ve dağıtım durumu ve çıkış kodu gibi yararlı bilgiler görüntülenir.

## <a name="deploy-modules-from-azure-marketplace"></a>Azure Marketi 'nden modüller dağıtma

[Azure Marketi](https://azuremarketplace.microsoft.com/) , [IoT Edge modüller](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)dahil olmak üzere Azure 'da çalışmak üzere sertifikalı ve iyileştirilmiş çok sayıda kurumsal uygulama ve çözüm arasında dolaşabileceğiniz çevrimiçi bir uygulamalar ve hizmetler marketi.

Azure Marketi 'nden ve IoT Hub bir IoT Edge modülünü dağıtabilirsiniz.

### <a name="deploy-from-azure-marketplace"></a>Azure Marketi 'nden dağıtma

Market 'teki IoT Edge modüllerini kullanın ve istediğiniz bir dosyayı bularak **Oluştur** veya **hemen al**seçeneğini belirleyerek dağıtabilirsiniz. Seçtiğiniz IoT Edge modüle göre değişebilen Dağıtım Sihirbazı adımlarına devam edin:

1. Devam ' i seçerek sağlayıcının kullanım koşullarını ve gizlilik ilkesini kabul **edin**. Önce iletişim bilgilerini sağlamanız gerekebilir.
1. Aboneliğinizi ve hedef cihazın eklendiği IoT Hub seçin.
1. **Cihaza dağıt**' ı seçin.
1. Cihazın adını girin veya hub 'a kayıtlı cihazlar arasında gezinmek için **Cihazı bul** ' u seçin.
1. İsterseniz başka modüller ekleme dahil olmak üzere, bir dağıtım bildirimini yapılandırma işlemi için standart işleme devam etmek için **Oluştur** ' u seçin. Görüntü URI 'SI, oluşturma seçenekleri ve istenen özellikler gibi yeni modülün ayrıntıları önceden tanımlanmıştır ancak değiştirilebilir.

Modülün Azure portal IoT Hub dağıtıldığını doğrulayın. Cihazınızı seçin, **modülleri ayarla** ' yı seçin ve modülün **IoT Edge modüller** bölümünde listelenmesi gerekir.

### <a name="deploy-from-azure-iot-hub"></a>Azure IoT Hub dağıtma

Azure portal IoT Hub Azure Marketi 'nden cihazınıza hızlı bir şekilde bir modül dağıtabilirsiniz.

1. Azure portal IoT Hub gidin.
1. Sol bölmedeki **otomatik cihaz yönetimi**altında **IoT Edge**' yi seçin.
1. Dağıtımı alacak IoT Edge cihazını seçin.
1. Üstteki çubukta **modülleri ayarla**' yı seçin.
1. **IoT Edge modüller** bölümünde, **Ekle**' ye tıklayın ve açılan menüden **Market modülü** ' nü seçin.

![IoT Hub Modül Ekle](./media/how-to-deploy-modules-portal/iothub-add-module.png)

**IoT Edge Module marketi** sayfasından bir modül seçin. Seçtiğiniz modül, aboneliğiniz, kaynak grubunuz ve cihazınız için otomatik olarak yapılandırılır. Daha sonra IoT Edge modüller listenizde görünür. Bazı modüller için ek yapılandırma gerekebilir.

> [!TIP]
> Azure IoT Hub IoT Edge modülleriyle ilgili bilgiler sınırlıdır. Önce Azure Marketi 'nde [IoT Edge modüller](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) hakkında daha fazla bilgi edinebilirsiniz.

**İleri ' yi seçin:** rotalar ve bu makalenin önceki kısımlarında yer alarak dağıtımları [belirtin](#specify-routes) ve [dağıtımı gözden geçir](#review-deployment) bölümünde açıklandığı gibi dağıtıma devam edin.

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge modüllerini ölçekli olarak dağıtmayı ve izlemeyi](how-to-deploy-at-scale.md) öğrenin
