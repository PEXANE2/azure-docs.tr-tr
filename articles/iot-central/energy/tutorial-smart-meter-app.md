---
title: 'Öğretici: IoT Central ile akıllı sayaç analiz uygulaması oluşturun'
description: 'Öğretici: Azure IoT Central uygulama şablonlarını kullanarak akıllı sayaç izleme uygulamasını nasıl oluşturabilirsiniz öğrenin.'
author: op-ravi
ms.author: omravi
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: 9e954e9c1a7efa43a19849b1c5b40284ec84eeed
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77016009"
---
# <a name="tutorial-create-and-walk-through-the-smart-meter-monitoring-app-template"></a>Öğretici: Akıllı sayaç izleme uygulaması şablonu oluşturun ve gözden geçirin 



Bu öğretici, simüle edilmiş verilere sahip örnek bir aygıt modeli içeren akıllı sayaç izleme uygulamasını oluşturma sürecinde size yol gösteriş sağlar. Bu öğreticide şunları öğreneceksiniz:

> [!div class="checklist"]
> * Akıllı Sayaç Uygulamasını ücretsiz oluşturun
> * Uygulama walk-through
> * Kaynakları temizleme


Aboneliğiniz yoksa, [ücretsiz deneme hesabı oluşturun](https://azure.microsoft.com/free)

## <a name="prerequisites"></a>Ön koşullar
- None
- Azure aboneliği önerilir, ancak denemek için gerekli değildir

## <a name="create-a-smart-meter-monitoring-app"></a>Akıllı sayaç izleme uygulaması oluşturma 

Bu uygulamayı üç basit adımda oluşturabilirsiniz:

1. [Azure IoT Merkezi giriş sayfasını](https://apps.azureiotcentral.com) açın ve yeni bir uygulama oluşturmak için **Oluştur'u** tıklatın. 

2. **Enerji** sekmesini seçin ve **Akıllı sayaç izleme** uygulama döşemesi altında Oluştur **uygulamasını** tıklatın.

    > [!div class="mx-imgBorder"]
    > ![Uygulama Oluştur](media/tutorial-iot-central-smart-meter/smart-meter-build.png)
    

3. **Create uygulaması** **Yeni başvuru** formunu açacaktır. Aşağıdaki şekilde gösterildiği gibi istenen ayrıntıları doldurun:
    * **Başvuru adı**: IoT Central uygulamanız için bir ad seçin. 
    * **URL**: Bir IoT Merkezi URL'si seçin, platform benzersizliğini doğrular.
    * **7 günlük ücretsiz deneme**: Azure aboneliğiniz varsa varsayılan ayar önerilir. Azure aboneliğiniz yoksa, ücretsiz deneme sürümüyle başlayın.
    * **Fatura Bilgileri**: Uygulamanın kendisi ücretsizdir. Uygulamanızın kaynaklarını sağlamak için Dizin, Azure aboneliği ve Bölge ayrıntıları gereklidir.
    * Sayfanın altındaki **Oluştur** düğmesini tıklatın ve uygulamanız bir dakika içinde oluşturulacaktır.

        ![Yeni başvuru formu](media/tutorial-iot-central-smart-meter/smart-meter-create-new-app.png)

        ![Yeni başvuru formu fatura bilgileri](media/tutorial-iot-central-smart-meter/smart-meter-create-new-app-billinginfo.png)


### <a name="verify-the-application-and-simulated-data"></a>Uygulamayı ve simüle edilmiş verileri doğrulayın

Yeni oluşturulan akıllı sayaç uygulaması sizin uygulamanızdır ve istediğiniz zaman değiştirebilirsiniz. Uygulamayı değiştirmeden önce uygulamanın dağıtılmış olduğundan ve beklendiği gibi çalıştığından emin olalım.

Uygulama oluşturma ve veri simülasyonu doğrulamak için **Pano'ya**gidin. Bazı verilerle kutucukları görebiliyorsanız, uygulama dağıtımınız başarılı oldu. Veri simülasyonu veri oluşturmak için birkaç dakika sürebilir, bu yüzden 1-2 dakika verin. 

## <a name="application-walk-through"></a>Uygulama walk-through
Uygulama şablonunu başarıyla dağıttıktan sonra, örnek akıllı sayaç aygıtı, aygıt modeli ve bir pano yla birlikte gelir. 

Adatum, akıllı sayaçları izleyen ve yöneten hayali bir enerji şirketidir. Akıllı sayaç izleme panosunda akıllı sayaç özelliklerini, verileri ve örnek komutları görürsünüz. Operatörlerin ve destek ekiplerinin destek olaylarına dönüşmeden önce aşağıdaki etkinlikleri proaktif bir şekilde gerçekleştirmelerine olanak tanır: 
* Haritadaki en son sayaç bilgilerini ve yüklü konumunu gözden geçirin
* Sayaç ağını ve bağlantı durumunu proaktif olarak kontrol edin 
* Ağ sağlığı için Min ve Max voltaj okumalarını izleyin 
* Herhangi bir anormal desenleri yakalamak için enerji, güç ve voltaj eğilimlerini gözden geçirin 
* Planlama ve faturalandırma amacıyla toplam enerji tüketimini izleme
* Yeniden bağlama sayacı ve firmware sürümünü güncelleme gibi komut ve kontrol işlemleri. Şablonda, komut düğmeleri olası işlevleri gösterir ve gerçek komutları göndermeyin. 

> [!div class="mx-imgBorder"]
> ![Akıllı sayaç izleme panosu](media/tutorial-iot-central-smart-meter/smart-meter-dashboard.png)

### <a name="devices"></a>Cihazlar
Uygulama örnek bir akıllı sayaç cihazı ile birlikte gelir. **Aygıtlar** sekmesine tıklayarak cihaz ayrıntılarını görebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Akıllı sayaç cihazları](media/tutorial-iot-central-smart-meter/smart-meter-devices.png)

Cihaz ayrıntılarını görmek için örnek cihaz **SM0123456789** bağlantısına tıklayın. Aygıtın yazılabilir özelliklerini **Özellikleri Güncelleştir** sayfasında güncelleştirebilir ve panoda güncelleştirilen değerleri görselleştirebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Akıllı sayaç özellikleri](media/tutorial-iot-central-smart-meter/smart-meter-device-properties.png)

### <a name="device-template"></a>Aygıt Şablonu
Akıllı sayaç aygıtı modelini görmek için **Aygıt şablonları** sekmesine tıklayın. Model, Veri, Özellik, Komutlar ve Görünümler için önceden tanımlı arabirime sahiptir.

> [!div class="mx-imgBorder"]
> ![Akıllı sayaç cihaz şablonları](media/tutorial-iot-central-smart-meter/smart-meter-device-template.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu uygulamayı kullanmaya devam etmemeye karar verirseniz, aşağıdaki adımları kullanarak uygulamanızı silin:

1. Sol bölmeden, Yönetim sekmesini aç
2. Uygulama ayarlarını seçin ve sayfanın altındaki Sil düğmesini tıklatın. 

    > [!div class="mx-imgBorder"]
    > ![Uygulamayı silme](media/tutorial-iot-central-smart-meter/smart-meter-delete-app.png)


## <a name="next-steps"></a>Sonraki adımlar
* Akıllı sayaç uygulaması mimarisi hakkında bilgi edinin [konsept makaleye](https://docs.microsoft.com/azure/iot-central/energy/concept-iot-central-smart-meter-app) bakın
* Akıllı sayaç uygulama şablonlarını ücretsiz oluşturun: [akıllı sayaç uygulaması](https://apps.azureiotcentral.com/build/new/smart-meter-monitoring)
* IoT Central hakkında daha fazla bilgi edinin, [Bkz. IoT Central genel bakış](https://docs.microsoft.com/azure/iot-central/)
