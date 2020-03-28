---
title: 'Öğretici: IoT Central ile bir güneş ceza izleme uygulaması oluşturun'
description: 'Öğretici: Azure IoT Merkezi uygulama şablonlarını kullanarak güneş paneli uygulamasını nasıl oluşturabilirsiniz öğrenin.'
author: op-ravi
ms.author: omravi
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: d5ea3d3420cb598693ccaede7ee10d2f8c4fd839
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77025784"
---
# <a name="tutorial-create-and-walk-through-the-solar-panel-monitoring-app-template"></a>Öğretici: Oluşturma ve güneş paneli izleme uygulaması şablonu ile walk-through 



Bu öğretici, simüle edilmiş verilere sahip örnek bir cihaz modeli içeren güneş paneli izleme uygulaması oluşturma sürecinde size rehberlik eder. Bu öğreticide şunları öğreneceksiniz:


> [!div class="checklist"]
> * Güneş paneli uygulamasını ücretsiz oluşturun
> * Uygulama walk-through
> * Kaynakları temizleme


Aboneliğiniz yoksa, [ücretsiz deneme hesabı oluşturun](https://azure.microsoft.com/free)

## <a name="prerequisites"></a>Ön koşullar
- None
- Azure aboneliği önerilir, ancak denemek için gerekli değildir


## <a name="create-a-solar-panel-monitoring-app"></a>Güneş paneli izleme uygulaması oluşturma 

Bu uygulamayı üç basit adımda oluşturabilirsiniz:

1. [Azure IoT Merkezi giriş sayfasını](https://apps.azureiotcentral.com) açın ve yeni bir uygulama oluşturmak için **Oluştur'u** tıklatın. 

2. **Enerji** sekmesini seçin ve Güneş paneli izleme uygulama **döşemesi** altında **oluştur uygulamasını** tıklatın. 

    > [!div class="mx-imgBorder"]
    > ![Uygulama Oluştur](media/tutorial-iot-central-solar-panel/solar-panel-build.png)
  
3. **Create uygulaması** **Yeni başvuru** formu açılır. Aşağıdaki şekilde gösterildiği gibi istenen ayrıntıları doldurun:
    * **Başvuru adı**: IoT Central uygulamanız için bir ad seçin. 
    * **URL**: Bir IoT Merkezi URL'si seçin, platform benzersizliğini doğrular.
    * **7 günlük ücretsiz deneme**: Azure aboneliğiniz varsa varsayılan ayar önerilir. Azure aboneliğiniz yoksa, ücretsiz deneme sürümüyle başlayın.
    * **Fatura Bilgileri**: Uygulamanın kendisi ücretsizdir. Uygulamanızın kaynaklarını sağlamak için Dizin, Azure aboneliği ve Bölge ayrıntıları gereklidir.
    * Sayfanın altındaki **Oluştur** düğmesini tıklatın ve uygulamanız bir dakika içinde oluşturulacaktır.
        ![Yeni başvuru formu](media/tutorial-iot-central-solar-panel/solar-panel-create-app.png)
        
        ![Yeni başvuru formu fatura bilgileri](media/tutorial-iot-central-solar-panel/solar-panel-create-app-billinginfo.png)


### <a name="verify-the-application-and-simulated-data"></a>Uygulamayı ve simüle edilmiş verileri doğrulayın

Yeni oluşturulan güneş paneli uygulaması uygulamanızdır ve istediğiniz zaman değiştirebilirsiniz. Uygulamayı değiştirmeden önce uygulamanın dağıtılmış olduğundan ve beklendiği gibi çalıştığından emin olalım.

Uygulama oluşturma ve veri simülasyonu doğrulamak için **Pano'ya**gidin. Bazı verilerle kutucukları görebiliyorsanız, uygulama dağıtımınız başarılı oldu. Veri simülasyonu veri oluşturmak için birkaç dakika sürebilir, bu yüzden 1-2 dakika verin. 

## <a name="application-walk-through"></a>Uygulama walk-through
Uygulama şablonu başarıyla dağıtıldıktan sonra, örnek akıllı sayaç aygıtı, aygıt modeli ve pano ile birlikte gelir.

Adatum, güneş panellerini izleyen ve yöneten hayali bir enerji şirketidir. Güneş paneli izleme panosunda, güneş paneli özelliklerini, verilerini ve örnek komutlarını görürsünüz. Operatörlerin ve destek ekiplerinin destek olaylarına dönüşmeden önce aşağıdaki etkinlikleri proaktif bir şekilde gerçekleştirmelerine olanak tanır:
* Haritadaki en son panel bilgilerini ve yüklü konumunu gözden geçirin
* Panel durumunu ve bağlantı durumunu proaktif olarak kontrol edin
* Herhangi bir anormal desenleri yakalamak için enerji üretimi ve sıcaklık eğilimlerini gözden geçirin
* Planlama ve faturalandırma amacıyla toplam enerji üretimini izleme
* Paneli etkinleştirme ve firmware sürümünü güncelleme gibi komut ve kontrol işlemleri. Şablonda, komut düğmeleri olası işlevleri gösterir ve gerçek komutları göndermeyin.

> [!div class="mx-imgBorder"]
> ![Güneş paneli izleme panosu](media/tutorial-iot-central-solar-panel/solar-panel-dashboard.png)

### <a name="devices"></a>Cihazlar
Uygulama örnek bir güneş paneli cihazı ile birlikte geliyor. **Aygıtlar** sekmesine tıklayarak cihaz ayrıntılarını görebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Güneş paneli cihazları](media/tutorial-iot-central-solar-panel/solar-panel-device.png)


Cihaz ayrıntılarını görmek için örnek cihaz **SP0123456789** bağlantısına tıklayın. Özellikleri **Güncelleştir** sayfasında, aygıtın yazılabilir özelliklerini güncelleştirebilir ve panoda güncelleştirilen değerleri görselleştirebilirsiniz. 

> [!div class="mx-imgBorder"]
> ![Güneş paneli özellikleri](media/tutorial-iot-central-solar-panel/solar-panel-device-properties.png)


### <a name="device-template"></a>Aygıt Şablonu
Güneş paneli cihaz modelini görmek için **Aygıt şablonları** sekmesine tıklayın. Model, Veri, Özellik, Komutlar ve Görünümler için önceden tanımlı arabirime sahiptir.

> [!div class="mx-imgBorder"]
> ![Güneş paneli cihazları şablonu](media/tutorial-iot-central-solar-panel/solar-panel-device-templates.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu uygulamayı kullanmaya devam etmemeye karar verirseniz, aşağıdaki adımları kullanarak uygulamanızı silin:

1. Sol bölmeden, Yönetim sekmesini aç
2. Uygulama ayarlarını seçin ve sayfanın altındaki Sil düğmesini tıklatın. 

    > [!div class="mx-imgBorder"]
    > ![Uygulamayı silme](media/tutorial-iot-central-solar-panel/solar-panel-delete-app.png)


## <a name="next-steps"></a>Sonraki adımlar
* Güneş paneli uygulaması mimarisi hakkında bilgi edinin [konsept makaleye](https://docs.microsoft.com/azure/iot-central/energy/concept-iot-central-solar-panel-app) bakın
* Ücretsiz güneş paneli uygulama şablonları oluşturun: [güneş paneli uygulaması](https://apps.azureiotcentral.com/build/new/solar-panel-monitoring)
* IoT Central hakkında daha fazla bilgi edinin, [Bkz. IoT Central genel bakış](https://docs.microsoft.com/azure/iot-central/)

