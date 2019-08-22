---
title: Azure Portal - Azure IOT Edge modüllerini dağıtmak | Microsoft Docs
description: Modüller IOT Edge cihazına dağıtmak için Azure portalını kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/25/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: f27484fd1d47a2e29aa5083a7d440e5c7dba11c1
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68839653"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Azure portalından Azure IOT Edge modüllerini dağıtmak

IOT Edge modülleri, iş mantığı ile oluşturduktan sonra bunları ucuna çalışılacak cihazlarınıza dağıtmak istiyorsanız. Toplamak ve veri işlemek için birlikte çalışan birden çok modül varsa, bunları tamamını aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirin.

Bu makalede, nasıl Azure portalında bir dağıtım bildirimi oluşturmak ve IOT Edge cihazına dağıtım gönderme sırasında size kılavuzluk eder gösterilmektedir. Birden fazla cihazda kendi paylaşılan etiketlere göre hedefleyen bir dağıtım oluşturma hakkında daha fazla bilgi için bkz. [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki.
* Bir [IOT Edge cihazı](how-to-register-device-portal.md) yüklü olan bir IOT Edge çalışma zamanı ile.

## <a name="select-your-device"></a>Cihazınızı seçin

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.
1. Seçin **IOT Edge** menüsünde.
1. Hedef cihazın cihazlar listesinden numarasını tıklayın.
1. **Modülleri Ayarlama**'yı seçin.

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Nasıl iş dağıtım bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).

Azure portalı, JSON belgesini el ile oluşturmak yerine dağıtım bildirimini oluşturmada size yol gösterir. bir sihirbaz vardır. Üç adım vardır: **Modüller ekleyin**, **rotalar belirtin**ve **dağıtımı gözden geçirin**.

### <a name="add-modules"></a>Modül Ekle

1. Sayfanın **Container Registry ayarları** bölümünde, modül görüntülerinizi içeren özel kapsayıcı kayıt defterlerine erişim için kimlik bilgilerini sağlayın.

1. Sayfanın **dağıtım modülleri** bölümünde **Ekle**' yi seçin.

1. Modüller, türler, aşağı açılan listeden bakın:

   * **IOT Edge Modülü** -varsayılan seçenek.
   * **Azure Stream Analytics Modülü** -yalnızca bir Azure Stream Analytics iş yükünden oluşturulan modüller.
   * Yalnızca **modül Azure Machine Learning** bir Azure Machine Learning çalışma alanından oluşturulan model görüntüleri.

1. Seçin **IOT Edge Modülü**.

1. Modül için bir ad belirtin ve ardından kapsayıcı görüntüsünü belirtin. Örneğin:

   * **Ad** -SimulatedTemperatureSensor
   * **Görüntü URI'si** -mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0

1. Gerekirse, isteğe bağlı alanları doldurun. Kapsayıcı hakkında daha fazla bilgi seçenekleri, yeniden başlatma ilkesi oluşturabilir ve istenen durumunu görmek için [EdgeAgent istenen özelliklerini](module-edgeagent-edgehub.md#edgeagent-desired-properties). Modül ikizi hakkında daha fazla bilgi için bkz. [tanımlayın veya güncelleştirme istenen özelliklerini](module-composition.md#define-or-update-desired-properties).

1. **Kaydet**’i seçin.

1. Dağıtımınız için ek modüller eklemek için 2-6 adımlarını yineleyin.

1. Seçin **sonraki** yollar bölüme geçmek için.

### <a name="specify-routes"></a>Rota belirtme

Varsayılan olarak sihirbaz size bir yol olarak adlandırılan **rota** ve tanımlanmış olarak **FROM /\* Yukarı Akış $**, modüllerin tarafından çıkış iletileri IOT hub'ına gönderilen anlamına gelir.  

Ekleme veya yolları alınan bilgilerle güncelleştirme [bildirmek yollar](module-composition.md#declare-routes), ardından **sonraki** gözden geçirme bölüme geçmek için.

### <a name="review-deployment"></a>Dağıtım gözden geçirin

Oluşturulan gözden geçirme bölümü gösterir, JSON dağıtım bildirimi önceki iki bölümlerde seçimlerinize göre. İki modül bildirildi, eklemediğim olduğunu unutmayın: **$edgeAgent** ve **$edgeHub**. Bu iki modül oluşturan [IOT Edge çalışma zamanı](iot-edge-runtime.md) ve her dağıtımda gerekli varsayılanlardır.

Dağıtım bilgilerinizi gözden geçirin ve ardından **Gönder**.

## <a name="view-modules-on-your-device"></a>Cihazınızda modülleri görüntüleme

Cihazınıza modülleri dağıttıktan sonra bunların tümünün görüntüleyebilirsiniz **cihaz ayrıntıları** portal sayfası. Bu sayfa, her dağıtılan modülü yanı sıra dağıtım durumu ve çıkış kodu gibi yararlı bilgiler adını görüntüler.

## <a name="deploy-modules-from-azure-marketplace"></a>Azure Marketi 'nden modüller dağıtma

Azure Marketi, [IoT Edge modüller](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)dahil olmak üzere Azure 'da çalışmak üzere sertifikalı ve iyileştirilmiş çok sayıda kurumsal uygulama ve çözüm arasında dolaşabileceğiniz çevrimiçi bir uygulamalar ve hizmetler marketi. Azure Market 'Te **kaynak oluştur**altında Azure Portal aracılığıyla da erişilebilir.

IoT Edge modülünü Azure Market 'ten veya Azure portal yükleyebilirsiniz:

1. Bir modül bulun ve dağıtım işlemine başlayın.

   * Azure portal: Bir modül bulun ve **Oluştur**' u seçin.

   * Azure Marketi:

     1. Bir modül bulun ve **Şimdi al**' ı seçin.
     1. Devam ' i seçerek sağlayıcının kullanım koşullarını ve gizlilik ilkesini kabul **edin**.

1. Aboneliğinizi ve hedef cihazın eklendiği IoT Hub seçin.

1. **Cihaza dağıt**' ı seçin.

1. Cihazın adını girin veya hub 'a kayıtlı cihazlar arasında gezinmek için **Cihazı bul** ' u seçin.

1. İsterseniz başka modüller ekleme dahil olmak üzere, bir dağıtım bildirimini yapılandırma işlemi için standart işleme devam etmek için **Oluştur** ' u seçin. Görüntü URI 'SI, oluşturma seçenekleri ve istenen özellikler gibi yeni modülün ayrıntıları önceden tanımlanmıştır ancak değiştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md)
