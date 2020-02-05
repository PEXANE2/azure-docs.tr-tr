---
title: Mikro-Karşılama Merkezi uygulama şablonu öğreticisi | Microsoft Docs
description: IoT Central için mikro Karşılama Merkezi uygulama şablonu öğreticisi
author: avneet723
ms.author: avneets
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 01/09/2020
ms.openlocfilehash: 2ed6f37bc3b00397fa6331a501e1b16c8d622b5f
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77027793"
---
# <a name="tutorial-deploy-and-walk-through-a-micro-fulfillment-center-application-template"></a>Öğretici: mikro Karşılama Merkezi uygulama şablonunu dağıtma ve gözden geçir

Bu öğreticide, Azure IoT Central ***Micro-Karşılama Merkezi*** uygulama şablonuyla bir perakende çözümü oluşturma hakkında daha fazla yararlanacağız. MFC şablonunun nasıl dağıtılacağını, kutudan hangilerinin ekleneceğini ve ileri bir sonraki ne yapmak isteyebileceğiniz hakkında bilgi edineceksiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 
> [!div class="checklist"]
> * Perakende uygulaması oluşturmak için Azure IoT Central **mikro Karşılama Merkezi** şablonunu kullanın
> * Uygulamayı gözden geçir 

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici serisini tamamlayabilmeniz için şunlar gerekir:
* Azure aboneliği. İsteğe bağlı olarak 7 günlük ücretsiz deneme kullanabilirsiniz. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir tane oluşturabilirsiniz.

## <a name="create-an-application"></a>Uygulama oluşturma 
Bu bölümde, bir şablondan yeni bir Azure IoT Central uygulaması oluşturacaksınız. Bu uygulamayı, tüm bir çözüm oluşturmak için öğretici serisi boyunca kullanacaksınız.

Yeni bir Azure IoT Central uygulaması oluşturmak için:

1. [Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) Web sitesine gidin.
1. Azure aboneliğiniz varsa, ona erişmek için kullandığınız kimlik bilgileriyle oturum açın, aksi takdirde Microsoft hesabı kullanarak oturum açın:

   ![Kuruluş hesabınızı girin](./media/tutorial-in-store-analytics-create-app-pnp/sign-in.png)

1. Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni Uygulama**'yı seçin.

1. **Perakende**' ı seçin.  Perakende sayfasında çeşitli perakende uygulama şablonları görüntülenir.

Önizleme özelliklerini kullanan yeni bir mikro Karşılama Merkezi uygulaması oluşturmak için:  
1. **Mikro Karşılama Merkezi** uygulama şablonunu seçin. Bu şablon, öğreticide kullanılan tüm cihazların cihaz şablonlarını içerir. Şablon Ayrıca, karşılama merkezinizdeki koşulların yanı sıra robot taşıyıcılar için koşullar için de bir operatör panosu sağlar. 

    > [!div class="mx-imgBorder"]
    > ![mikro-uygulama türü](./media/tutorial-micro-fulfillment-center-app-pnp/iotc-retail-homepage-mfc.png)
    
1. İsteğe bağlı olarak, kolay bir **uygulama adı**seçin.  Uygulama şablonu kurgusal şirket Northwind Traders ' i temel alır. 

    > [!NOTE]
    > Kolay bir **uygulama adı**kullanırsanız, uygulama **URL 'si**için yine de benzersiz bir değer kullanmanız gerekir.

1. Azure aboneliğiniz varsa *dizininizi, Azure aboneliğinizi ve bölgenizi*girin. Aboneliğiniz yoksa, **7 günlük ücretsiz denemeyi** etkinleştirebilir ve gerekli iletişim bilgilerini tamamlayabilirsiniz.  

    Dizinler ve abonelikler hakkında daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](../preview/quick-deploy-iot-central.md).

1. **Oluştur**'u seçin.

> [!div class="mx-imgBorder"]
> ![mikro karşılama uygulaması oluşturma](./media/tutorial-micro-fulfillment-center-app-pnp/iotc-retail-create-app-mfc.png)

## <a name="walk-through-the-application"></a>Uygulamayı gözden geçir 

### <a name="dashboard"></a>Pano 

Uygulama şablonunu başarıyla dağıttıktan sonra, önce **Northwind Traders Micro-Karşılama Merkezi panosu**' na gidecaksınız. Northwind Trader, bu IoT Central uygulamasında yönetilen bir mikro karşılama merkezinin bulunduğu kurgusal bir satıcıdır. Bu operatör panosunda, bu şablondaki cihazlarla ilgili bilgileri ve telemetrisi, gerçekleştirebileceğiniz bir komut, iş ve eylem kümesiyle birlikte görürsünüz. Pano, mantıksal olarak iki bölüme ayrılır (sol ve sağ). Sol tarafta, sağlama yapısı içinde çevresel koşulları izleyebilir ve sağ tarafta, bir robot taşıyıcının sistem durumunu tesis içinde izleyebilirsiniz.  

Panodan şunları yapabilirsiniz:
   * Bkz. çekme sayısı, işlenen sipariş sayısı ve yapı sistem durumu gibi özellikler gibi cihaz telemetrisi.  
   * Karşılama yapısında robot taşıyıcılar 'nın **kat planını** ve konumunu görüntüleyin.
   * Denetim sistemini sıfırlama, taşıyıcının bellenimini güncelleştirme, ağı yeniden yapılandırma vb. gibi komutları tetikleyin.

> [!div class="mx-imgBorder"]
> ![mikro-Karşılama Merkezi panosu](./media/tutorial-micro-fulfillment-center-app-pnp/mfc-dashboard1.png)
   * Bir işlecin, Karşılama Merkezi içindeki koşulları izlemek için yararlanabilen bir pano örneği görürsünüz. 
   * Karşılama Merkezi içindeki ağ geçidi cihazında çalışan yüklerin sistem durumunu izleyin.    

> [!div class="mx-imgBorder"]
> ![mikro-Karşılama Merkezi panosu](./media/tutorial-micro-fulfillment-center-app-pnp/mfc-dashboard2.png)

## <a name="device-template"></a>Cihaz şablonu
Cihaz şablonları sekmesine tıklarsanız, şablonun bir parçası olan iki farklı cihaz türü olduğunu görürsünüz: 
   * **Robot taşıyıcı**: Bu cihaz şablonu, karşılama yapısında dağıtılan ve uygun depolama ve alma işlemlerini gerçekleştiren, çalışan bir robot taşıyıcının tanımını temsil eder. Şablona tıkladığınızda, robot 'un sıcaklık, eksen konumu ve robot taşıyıcı durumu gibi özellikler gibi cihaz verilerini gönderdiğini görürsünüz. 
   * **Yapı koşulu izleme**: Bu cihaz şablonu, çalışma merkezinizi desteklemek üzere çeşitli Edge iş yüklerini barındıran ağ geçidi cihazını ve ortam koşulunun izlenmesini sağlayan bir cihaz koleksiyonunu temsil eder. Cihaz, ortamınızda çalışan işlem iş yüklerinin durumu ve durumunun yanı sıra sıcaklık, çekme sayısı, sipariş sayısı gibi telemetri verilerini de gönderir. 

> [!div class="mx-imgBorder"]
> ![mikro-Karşılama Merkezi cihaz şablonları](./media/tutorial-micro-fulfillment-center-app-pnp/device-templates.png)

Cihaz grupları sekmesine tıklarsanız, bu cihaz şablonlarının kendileri için cihaz gruplarının otomatik olarak oluşturulduğunu da görürsünüz.

## <a name="rules"></a>Kurallar
Kurallar sekmesine atladıktan sonra, robot taşıyıcının sıcaklık koşullarını izlemek için uygulama şablonunda bulunan örnek bir kural görürsünüz. Bu kuralı, Tesisdeki belirli bir robot aşırı Isıtma ve bakım için çevrimdışına alınması gerekiyorsa işleci uyarmak için kullanabilirsiniz. 

Lütfen iş işlevleriniz için daha uygun olan kuralları tanımlamak üzere örnek kuraldan yararlanın.

   - **Robot taşıyıcı çok sıcak**: robot taşıyıcı bir süre boyunca bir sıcaklık eşiğine ulaşırsa, bu kural tetiklenir. 

> [!div class="mx-imgBorder"]
> ![mikro-Karşılama Merkezi kuralları](./media/tutorial-micro-fulfillment-center-app-pnp/rules.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam edemeyecekiniz, **yönetim** > **uygulama ayarlarını** ziyaret ederek uygulama şablonunu silin ve **Sil**' e tıklayın.

> [!div class="mx-imgBorder"]
> ![mikro-Karşılama Merkezi uygulaması temizleme](./media/tutorial-micro-fulfillment-center-app-pnp/delete.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Mikro Karşılama Merkezi Çözüm mimarisi](./architecture-micro-fulfillment-center-pnp.md) hakkında daha fazla bilgi edinin
* Diğer [IoT Central perakende şablonları](./overview-iot-central-retail-pnp.md) hakkında daha fazla bilgi edinin
* IoT Central hakkında daha fazla bilgi edinin [IoT Central genel bakış](../preview/overview-iot-central.md)