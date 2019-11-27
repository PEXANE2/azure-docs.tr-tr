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
ms.openlocfilehash: 6d915145e64a5f1a097f38cf79b19426c3acbaf2
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457435"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Azure portalından Azure IOT Edge modüllerini dağıtmak

IOT Edge modülleri, iş mantığı ile oluşturduktan sonra bunları ucuna çalışılacak cihazlarınıza dağıtmak istiyorsanız. Toplamak ve veri işlemek için birlikte çalışan birden çok modül varsa, bunları tamamını aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirin.

Bu makalede, nasıl Azure portalında bir dağıtım bildirimi oluşturmak ve IOT Edge cihazına dağıtım gönderme sırasında size kılavuzluk eder gösterilmektedir. Paylaşılan etiketlerine göre birden çok cihazı hedefleyen bir dağıtım oluşturma hakkında bilgi için bkz. [IoT Edge modüllerini ölçeklendirerek dağıtma ve izleme](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğinizdeki bir [IoT Hub 'ı](../iot-hub/iot-hub-create-through-portal.md) .
* IoT Edge çalışma zamanı yüklü [IoT Edge bir cihaz](how-to-register-device.md#register-in-the-azure-portal) .

## <a name="select-your-device"></a>Cihazınızı seçin

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub 'ınıza gidin.
1. Menüden **IoT Edge** seçin.
1. Hedef cihazın cihazlar listesinden numarasını tıklayın.
1. **Modülleri Ayarlama**'yı seçin.

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Dağıtım bildirimlerinin nasıl çalıştığı ve nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [IoT Edge modüllerinin nasıl kullanılabileceğini, yapılandırılacağını ve yeniden kullanıldığını anlayın](module-composition.md).

Azure portalı, JSON belgesini el ile oluşturmak yerine dağıtım bildirimini oluşturmada size yol gösterir. bir sihirbaz vardır. Üç adım vardır: **modüller ekleme**, **rotalar belirtme**ve **dağıtımı İnceleme**.

### <a name="add-modules"></a>Modül Ekle

1. Sayfanın **Container Registry ayarları** bölümünde, modül görüntülerinizi içeren özel kapsayıcı kayıt defterlerine erişim için kimlik bilgilerini sağlayın.

1. Sayfanın **dağıtım modülleri** bölümünde **Ekle**' yi seçin.

1. Modüller, türler, aşağı açılan listeden bakın:

   * **IoT Edge Module** -varsayılan seçenek.
   * Yalnızca **modül Azure Stream Analytics** bir Azure Stream Analytics iş yüküyle oluşturulan modüller.
   * Yalnızca **modül Azure Machine Learning** bir Azure Machine Learning çalışma alanından oluşturulan model görüntüleri.

1. **IoT Edge modülünü**seçin.

1. Modül için bir ad belirtin ve ardından kapsayıcı görüntüsünü belirtin. Örneğin:

   * **Ad** -SimulatedTemperatureSensor
   * **Görüntü URI 'si** -MCR.Microsoft.com/azureiotedge-Simulated-Temperature-Sensor:1.0

1. Gerekirse, isteğe bağlı alanları doldurun. Kapsayıcı oluşturma seçenekleri hakkında daha fazla bilgi için, ilkeyi yeniden başlatın ve istenen durum bkz. [Edgeagent istenen özellikler](module-edgeagent-edgehub.md#edgeagent-desired-properties). Modül ikizi hakkında daha fazla bilgi için bkz. [istenen özellikleri tanımlama veya güncelleştirme](module-composition.md#define-or-update-desired-properties).

1. **Kaydet**’i seçin.

1. Dağıtımınız için ek modüller eklemek için 2-6 adımlarını yineleyin.

1. Rotalar bölümüne devam etmek için **İleri ' yi** seçin.

### <a name="specify-routes"></a>Rota belirtme

Varsayılan olarak, sihirbaz size **route** adlı bir yol sağlar ve **FROM/* to $upstream * * olarak tanımlanır, bu da herhangi bir modülle giden tüm iletilerin IoT Hub 'ınıza gönderilmesi anlamına gelir.  

Bilgileri [Declare rotalarındaki](module-composition.md#declare-routes)bilgilerle ekleyin veya güncelleştirin, ardından inceleme bölümüne devam etmek için **İleri** ' yi seçin.

### <a name="review-deployment"></a>Dağıtım gözden geçirin

Oluşturulan gözden geçirme bölümü gösterir, JSON dağıtım bildirimi önceki iki bölümlerde seçimlerinize göre. **$EdgeAgent** ve **$edgeHub**eklemediğiniz iki modül olduğunu unutmayın. Bu iki modül [IoT Edge çalışma zamanını](iot-edge-runtime.md) yapar ve her dağıtımda gerekli varsayılanlar olur.

Dağıtım bilgilerinizi gözden geçirin ve ardından **Gönder**' i seçin.

## <a name="view-modules-on-your-device"></a>Cihazınızda modülleri görüntüleme

Bir modülü cihazınıza dağıttıktan sonra, bunların tümünü portalın **cihaz ayrıntıları** sayfasında görüntüleyebilirsiniz. Bu sayfa, her dağıtılan modülü yanı sıra dağıtım durumu ve çıkış kodu gibi yararlı bilgiler adını görüntüler.

## <a name="deploy-modules-from-azure-marketplace"></a>Azure Marketi 'nden modüller dağıtma

Azure Marketi, [IoT Edge modüller](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)dahil olmak üzere Azure 'da çalışmak üzere sertifikalı ve iyileştirilmiş çok sayıda kurumsal uygulama ve çözüm arasında dolaşabileceğiniz çevrimiçi bir uygulamalar ve hizmetler marketi. Azure Market 'Te **kaynak oluştur**altında Azure Portal aracılığıyla da erişilebilir.

IoT Edge modülünü Azure Market 'ten veya Azure portal yükleyebilirsiniz:

1. Bir modül bulun ve dağıtım işlemine başlayın.

   * Azure portal: bir modül bulun ve **Oluştur**' u seçin.

   * Azure Marketi:

     1. Bir modül bulun ve **Şimdi al**' ı seçin.
     1. Devam ' i seçerek sağlayıcının kullanım koşullarını ve gizlilik ilkesini kabul **edin**.

1. Aboneliğinizi ve hedef cihazın eklendiği IoT Hub seçin.

1. **Cihaza dağıt**' ı seçin.

1. Cihazın adını girin veya hub 'a kayıtlı cihazlar arasında gezinmek için **Cihazı bul** ' u seçin.

1. İsterseniz başka modüller ekleme dahil olmak üzere, bir dağıtım bildirimini yapılandırma işlemi için standart işleme devam etmek için **Oluştur** ' u seçin. Görüntü URI 'SI, oluşturma seçenekleri ve istenen özellikler gibi yeni modülün ayrıntıları önceden tanımlanmıştır ancak değiştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge modüllerini ölçekli olarak dağıtmayı ve izlemeyi](how-to-deploy-monitor.md) öğrenin
