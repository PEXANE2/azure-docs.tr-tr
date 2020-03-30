---
title: Mikro-fulfillment merkezi uygulama şablonu öğretici | Microsoft Dokümanlar
description: Azure IoT Central için mikro-gerçekleştirme merkezi uygulama şablonu hakkında bir öğretici
author: avneet723
ms.author: avneets
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 01/09/2020
ms.openlocfilehash: 93906f582f1edc351088f8b4c453bf9ebda54f83
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77369870"
---
# <a name="tutorial-deploy-and-walk-through-a-micro-fulfillment-center-application-template"></a>Öğretici: Dağıtma ve bir mikro-yerine getirme merkezi uygulama şablonu ile yürümek

Bu eğitimde, bir perakende çözümü oluşturmak için Azure IoT Merkezi mikro yerine getirme merkezi uygulama şablonunu kullanırsınız. Şablonu nasıl dağıtabileceğinizi, şablona nelerin ekolduğunu ve bundan sonra ne yapmak isteyebileceğinizi öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici seriyi tamamlamak için bir Azure aboneliğine ihtiyacınız var. İsteğe bağlı olarak 7 günlük ücretsiz deneme sürümü kullanabilirsiniz. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir abonelik oluşturabilirsiniz.

## <a name="create-an-application"></a>Uygulama oluşturma 
Bu bölümde, şablondan yeni bir Azure IoT Merkezi uygulaması oluşturursunuz. Tam bir çözüm oluşturmak için öğretici serisi boyunca bu uygulamayı kullanacağız.

Yeni bir Azure IoT Central uygulaması oluşturmak için:

1. [Azure IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesine gidin.
1. Azure aboneliğiniz varsa, bu aboneye erişmek için kullandığınız kimlik bilgileriyle oturum açın. Aksi takdirde, bir Microsoft hesabı kullanarak oturum açın:

   ![Microsoft hesap oturum açma iletişim kutusunun ekran görüntüsü](./media/tutorial-in-store-analytics-create-app-pnp/sign-in.png)

1. Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni Uygulama**'yı seçin.

1. **Perakende'yi**seçin.  Perakende sayfası birkaç perakende uygulama şablonu görüntüler.

Önizleme özelliklerini kullanan yeni bir mikro karşılama merkezi uygulaması oluşturmak için:  
1. **Mikro-karşılama merkezi** uygulama şablonu'nu seçin. Bu şablon, öğreticide kullanılan tüm aygıtlar için aygıt şablonlarını içerir. Şablon ayrıca, yerine getirme merkezinizdeki izleme koşullarının yanı sıra robot taşıyıcılarınızın koşullarını izlemek için bir operatör panosu da sağlar. 

    ![Azure IoT Central Ekran Görüntüsü IoT uygulama sayfanızı oluşturun](./media/tutorial-micro-fulfillment-center-app-pnp/iotc-retail-homepage-mfc.png)
    
1. İsteğe bağlı olarak, dostça bir **Uygulama adı**seçin. Uygulama şablonu kurgusal şirket Northwind Traders dayanmaktadır. 

    >[!NOTE]
    >Kolay bir uygulama adı kullanıyorsanız, uygulama URL'si için yine de benzersiz bir değer kullanmanız gerekir.

1. Azure aboneliğiniz varsa, dizininizi, Azure aboneliğinizi ve bölgenizi girin. Aboneliğiniz yoksa, 7 günlük ücretsiz deneme sürümünü etkinleştirebilir ve gerekli iletişim bilgilerini tamamlayabilirsiniz.  

    Dizinler ve abonelikler hakkında daha fazla bilgi için uygulama hızlı [başlat'ı oluştur'a](../preview/quick-deploy-iot-central.md) bakın.

1. **Oluştur'u**seçin.

    ![Azure IoT Merkezi Yeni uygulama sayfasının ekran görüntüsü](./media/tutorial-micro-fulfillment-center-app-pnp/iotc-retail-create-app-mfc.png)

## <a name="walk-through-the-application"></a>Uygulama da yürüyün 

Uygulama şablonu başarıyla dağıtıldıktan sonra, **Northwind Traders mikro-yerine getirme merkezi panosunu**görürsünüz. Northwind Traders, bu Azure IoT Central uygulamasında yönetilen bir mikro-gerçekleştirme merkezine sahip hayali bir perakendecidir. Bu operatör panosunda, bu şablondaki aygıtlar hakkında bilgi ve telemetrinin yanı sıra yapabileceğiniz bir dizi komut, iş ve eylem görürsünüz. Pano mantıksal olarak iki bölüme ayrılmıştır. Solda, yerine getirme yapısı içindeki çevre koşullarını izleyebilir, sağda ise tesis içindeki robotik taşıyıcının sağlığını izleyebilirsiniz.  

Panodan şunları yapabilirsiniz:
   * Çekme sayısı, işlenen sipariş sayısı ve yapı sistemi durumu gibi özellikler gibi aygıt telemetrisi bölümüne bakın.  
   * Yerine getirme yapısı içinde kat planı ve robotik taşıyıcıların konumunu görüntüleyin.
   * Denetim sistemini sıfırlama, taşıyıcının yazılımını güncelleştirme ve ağı yeniden yapılandırma gibi tetikleyici komutlar.

     ![Northwind Traders mikro-yerine getirme merkezi panosu ekran görüntüsü](./media/tutorial-micro-fulfillment-center-app-pnp/mfc-dashboard1.png)
   * Bir operatörün yerine getirme merkezindeki koşulları izlemek için kullanabileceği pano örneğine bakın. 
   * Karşılama merkezi içinde ağ geçidi aygıtında çalışan yüklerin durumunu izleyin.    

     ![Northwind Traders mikro-yerine getirme merkezi panosu ekran görüntüsü](./media/tutorial-micro-fulfillment-center-app-pnp/mfc-dashboard2.png)

## <a name="device-template"></a>Aygıt şablonu
Aygıt şablonları sekmesini seçerseniz, şablonun bir parçası olan iki farklı aygıt türü olduğunu görürsünüz: 
   * **Robotik Taşıyıcı**: Bu cihaz şablonu, yerine getirme yapısında konuşlandırılmış ve uygun depolama ve alma işlemlerini gerçekleştiren işleyen bir robot taşıyıcının tanımını temsil eder. Şablonu seçerseniz, robotun sıcaklık ve eksen konumu gibi aygıt verilerini ve robot taşıyıcı durumu gibi özellikleri gönderdiğini görürsünüz. 
   * **Yapı Durumu İzleme**: Bu aygıt şablonu, ortam durumunu izlemenize olanak tanıyan bir aygıt koleksiyonunun yanı sıra yerine getirme merkezinize güç sağlamak için çeşitli kenar iş yüklerini barındıran ağ geçidi aygıtını temsil eder. Cihaz sıcaklık, çekme sayısı ve sipariş sayısı gibi telemetri verileri gönderir. Ayrıca, ortamınızda çalışan bilgi işlem iş yüklerinin durumu ve durumu hakkında da bilgi gönderir. 

     ![Mikro-yerine getirme Merkezi Cihaz Şablonları](./media/tutorial-micro-fulfillment-center-app-pnp/device-templates.png)

Aygıt grupları sekmesini seçerseniz, bu aygıt şablonlarının kendileri için otomatik olarak aygıt grupları oluşturulduğunu da görürsünüz.

## <a name="rules"></a>Kurallar
**Kurallar** sekmesinde, robot taşıyıcının sıcaklık koşullarını izlemek için uygulama şablonunda bulunan bir örnek kuralı görürsünüz. Tesisteki belirli bir robot aşırı ısınıyorsa ve servis için çevrimdışı olması gerekiyorsa, operatörü uyarmak için bu kuralı kullanabilirsiniz. 

İş işlevleriniz için daha uygun olan kuralları tanımlamak için örnek kuralını ilham kaynağı olarak kullanın.

![Kurallar sekmesinin ekran görüntüsü](./media/tutorial-micro-fulfillment-center-app-pnp/rules.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, uygulama şablonunu silin. **Yönetim** > **Uygulaması ayarlarına**gidin ve **Sil'i**seçin.

![Mikro-dolum merkezi Uygulama ayarları sayfasının ekran görüntüsü](./media/tutorial-micro-fulfillment-center-app-pnp/delete.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Mikro-yerine getirme merkezi çözüm mimarisi](./architecture-micro-fulfillment-center-pnp.md)hakkında daha fazla bilgi edinin.
* Diğer Azure [IoT Merkezi perakende satış şablonları](./overview-iot-central-retail-pnp.md)hakkında daha fazla bilgi edinin.
* Azure [IoT Merkezi genel görünümünü](../preview/overview-iot-central.md)okuyun.
