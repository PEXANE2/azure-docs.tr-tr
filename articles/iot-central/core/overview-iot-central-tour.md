---
title: Azure IoT Central Kullanıcı Arabirimi turuna katılın | Microsoft Docs
description: IoT çözümünüzü oluşturmak, yönetmek ve kullanmak için kullandığınız Azure IoT Merkezi UI'nin önemli alanlarını yakından tanımaya.
author: lmasieri
ms.author: lmasieri
ms.date: 12/09/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: b905b1e86810b25c4c94072d6cd414b993e2a883
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77426188"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui"></a>Azure IoT Central Kullanıcı Arabirimi turuna katılın



Bu makalede, Microsoft Azure IoT Central kullanıcı arabirimi tanıtılmaktadır. Kullanıcı arabirimini kullanarak bir Azure IoT Central çözümü ile bağlı cihazlarını oluşturabilir, yönetebilir ve kullanabilirsiniz.

Çözüm _oluşturucu_olarak, Azure IoT Merkezi çözümünüzü tanımlamak için Azure IoT Merkezi UI'yi kullanırsınız. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

* Çözümünüze bağlanan cihaz türlerini tanımlama.
* Cihazlarınız için kurallar ve eylemler yapılandırma. 
* Kullanıcı arabirimini, çözümünüzü kullanan bir _operatör_ için özelleştirme.

_Operatör_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü yönetebilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

* Cihazlarınızı izleme.
* Cihazlarınızı yapılandırma.
* Cihazlarınızla ilgili sorunları giderme ve düzeltme.
* Yeni cihazları hazırlama.

## <a name="iot-central-homepage"></a>IoT Merkez ana sayfası

[IoT Central ana sayfası, IoT](https://aka.ms/iotcentral-get-started) Central'da bulunan en son haberler ve özellikler hakkında daha fazla bilgi edinebileceğiniz, yeni uygulamalar oluşturabileceğiniz ve mevcut uygulamanızı görebileceğiniz ve başlatabileceğiniz yerdir.

> [!div class="mx-imgBorder"]
> ![IoT Merkez ana sayfası](media/overview-iot-central-tour/iot-central-homepage-pnp.png)

### <a name="create-an-application"></a>Uygulama oluşturma

Yapı bölümünde, hızlı bir şekilde başlamanıza yardımcı olmak için sektörle alakalı IoT Central şablonlarının listesine göz atabilir veya Özel uygulama şablonu kullanarak sıfırdan başlayabilirsiniz.  
> [!div class="mx-imgBorder"]
> ![IoT Merkezi yapı sayfası](media/overview-iot-central-tour/iot-central-build-pnp.png)

Daha fazla bilgi edinmek için [Azure IoT Central uygulaması oluşturma uygulamasına](quick-deploy-iot-central.md) bakın.

### <a name="launch-your-application"></a>Uygulamanızı başlatın

IoT Central uygulamanızı, uygulama oluşturma sırasında sizin veya çözüm oluşturucunuzun seçtiği URL'ye giderek başlatabilirsiniz. Ayrıca, [IoT Central uygulama yöneticisinde](https://aka.ms/iotcentral-apps)erişebildiğiniz tüm uygulamaların listesini de görebilirsiniz.

> [!div class="mx-imgBorder"]
> ![IoT Merkez uygulama yöneticisi](media/overview-iot-central-tour/app-manager-pnp.png)

## <a name="navigate-your-application"></a>Uygulamanızda gezinme

IoT uygulamanıza girdikten sonra, farklı alanlara erişmek için sol bölmeyi kullanın. Bölmenin üstündeki üç çizgili simgeyi seçerek sol bölmeyi genişletebilir veya daraltabilirsiniz:

> [!NOTE]
> Sol bölmede gördüğünüz öğeler kullanıcı rolünüze bağlıdır. [Kullanıcıları ve rolleri yönetme](howto-manage-users-roles.md)hakkında daha fazla bilgi edinin. 

:::row:::
  :::column span="":::
      > [!div class="mx-imgBorder"]
      > ![sol bölme](media/overview-iot-central-tour/navigationbar-pnp.png)
  :::column-end:::
  :::column span="2":::
     **Pano,** uygulama panonuzu görüntüler. Bir *çözüm oluşturucu olarak,* operatörleriniz için genel gösterge panelini özelleştirebilirsiniz. Operatörler, kullanıcı rollerine bağlı olarak kendi kişisel panolarını da oluşturabilir.
     
     **Aygıtlar** bağlı cihazlarınızı gerçek ve simüle olarak yönetmenize olanak tanır.

     **Aygıt grupları,** sorgu tarafından belirtilen aygıtların mantıksal koleksiyonlarını görüntülemenize ve oluşturmanıza olanak tanır. Bu sorguyu kaydedebilir ve toplu işlemleri gerçekleştirmek için uygulama aracılığıyla aygıt gruplarını kullanabilirsiniz.

     **Kurallar,** aygıtlarınızı izlemek için kurallar oluşturmanıza ve denetlemenize olanak tanır. Kurallar, aygıt telemetrisi ve tetiklenebilir eylemlere göre değerlendirilir.

     **Analytics,** uygulamanızdan öngörüler elde etmek için cihaz verilerinin üstünde özel görünümler oluşturmanıza olanak tanır.

     **İşler,** toplu işlemler çalıştırarak cihazlarınızı ölçekte yönetmenize olanak tanır.

     **Aygıt şablonları,** uygulamanıza bağlanan aygıtların özelliklerini oluşturduğunuz ve yönettiğiniz yerdir.

     **Veri dışa aktarma,** depolama ve kuyruklar gibi dış hizmetlere sürekli bir dış dış aktarım yapılandırmanızı sağlar.

     **Yönetim,** uygulamanızın ayarlarını, özelleştirmeyi, faturalandırmayı, kullanıcıları ve rolleri yönetebileceğiniz yerdir.

     **IoT Central,** *yöneticilerin* IoT Central'ın uygulama yöneticisine geri dönmelerini sağlar.
     
   :::column-end:::
:::row-end:::

### <a name="search-help-theme-and-support"></a>Arama, yardım, tema ve destek

Üstteki menü her sayfada görünür:

> [!div class="mx-imgBorder"]
> ![Araç Çubuğu](media/overview-iot-central-tour/toolbar-pnp.png)

* Aygıt şablonlarını ve aygıtlarını aramak için bir **Arama** değeri girin.
* UI dilini veya temasını değiştirmek için **Ayarlar** simgesini seçin. [Uygulama tercihlerinizi yönetme](howto-manage-preferences.md) hakkında daha fazla bilgi edinin
* Uygulamadan çıkmak için **Hesap** simgesini seçin.
* Yardım ve destek almak için, kaynak listesinden **Yardım** açılır listesini seçin. Ücretsiz fiyatlandırma planı ndaki bir uygulamada, destek kaynakları [canlı sohbete](howto-show-hide-chat.md)erişimi içerir.

Kullanıcı arabirimi için açık renkli tema veya koyu renkli temayı seçebilirsiniz:

> [!NOTE]
> Yöneticiniz uygulama için özel bir tema yapılandıysa, açık ve koyu renk temalar arasında seçim yapma seçeneği kullanılamaz.

> [!div class="mx-imgBorder"]
> ![Kullanıcı arabirimi için tema seçme](media/overview-iot-central-tour/themes-pnp.png)

### <a name="dashboard"></a>Pano
> [!div class="mx-imgBorder"]
> ![Pano](media/overview-iot-central-tour/dashboard-pnp.png)

* Pano, Azure IoT Merkezi uygulamanızda oturum açken gördüğünüz ilk sayfadır. Bir *çözüm oluşturucu olarak,* diğer kullanıcılar için birden çok genel uygulama panosu oluşturabilir ve özelleştirebilirsiniz. [Panonuza kutucuk ekleme](howto-add-tiles-to-your-dashboard.md) hakkında daha fazla bilgi edinin

* Bir *operatör*olarak, kullanıcı rolünüz buna izin veriyorsa, önemsediğiniz şeyi izlemek için kişisel panolar oluşturabilirsiniz. Daha fazla bilgi edinmek için [Azure IoT Merkezi'ni Oluştur kişisel panoları](howto-create-personal-dashboards.md) nasıl yapılsın makalesine bakın.

### <a name="devices"></a>Cihazlar

> [!div class="mx-imgBorder"]
> ![Cihazlar sayfası](media/overview-iot-central-tour/devices-pnp.png)

Explorer sayfası, Azure IoT Merkezi uygulamanızda _aygıt şablonuna_göre gruplanmış _aygıtları_ gösterir. 

* Cihaz şablonu, uygulamanıza bağlanabilen bir cihaz türünü tanımlar.
* Cihaz, uygulamanızdaki gerçek veya sanal bir cihazı temsil eder.

Daha fazla bilgi edinmek için aygıtlarınızın hızla [başlatılamasını Monitör'e](./quick-monitor-devices.md) bakın. 

### <a name="device-groups"></a>Device groups

> [!div class="mx-imgBorder"]
> ![Cihaz grupları sayfası](media/overview-iot-central-tour/device-groups-pnp.png)

Aygıt grubu ilgili aygıtların bir koleksiyonudur. *Çözüm oluşturucu,* aygıt grubuna dahil edilen aygıtları tanımlamak için bir sorgu tanımlar. Uygulamanızda toplu işlemler gerçekleştirmek için aygıt gruplarını kullanırsınız. Daha fazla bilgi edinmek için [Azure IoT Central uygulama makalenizde Cihaz Kullan gruplarına](tutorial-use-device-groups.md) bakın.

### <a name="rules"></a>Kurallar
> [!div class="mx-imgBorder"]
> ![Rules (Kurallar) Sayfası](media/overview-iot-central-tour/rules-pnp.png)

Kurallar sayfası, kuralların telemetrisini, durumunu veya olaylarını temel alan kuralları tanımlamanızı sağlar. Bir kural çıktığında, e-posta göndermek, webhook uyarıları vb. aracılığıyla harici bir sisteme bildirimde bulunmak gibi bir veya daha fazla eylemi tetikleyebilir. Öğrenmek için [Yapılandırma kuralları](tutorial-create-telemetry-rules.md) öğreticisine bakın. 

### <a name="analytics"></a>Analiz

> [!div class="mx-imgBorder"]
> ![Analiz sayfası](media/overview-iot-central-tour/analytics-pnp.png)

Analiz, uygulamanızdan öngörüler elde etmek için cihaz verilerinin üstünde özel görünümler oluşturmanıza olanak tanır. Daha fazla bilgi edinmek [için Azure IoT Central uygulama makaleniz için Analiz Oluştur'a](howto-create-analytics.md) bakın.

### <a name="jobs"></a>İşler

> [!div class="mx-imgBorder"]
> ![İşler sayfası](media/overview-iot-central-tour/jobs-pnp.png)

İş işleri sayfası, aygıtlarınızda toplu aygıt yönetimi işlemleri çalıştırmanızı sağlar. Aygıt özelliklerini, ayarlarını güncelleştirebilir ve komutları aygıt gruplarına karşı yürütebilirsiniz. Daha fazla bilgi edinmek için [İş çalıştırma](howto-run-a-job.md) makalesine bakın.

### <a name="device-templates"></a>Aygıt şablonları

> [!div class="mx-imgBorder"]
> ![Aygıt şablonları sayfası](media/overview-iot-central-tour/templates-pnp.png)

Aygıt şablonları sayfası, bir oluşturucunun uygulamadaki aygıt şablonlarını oluşturduğu ve yönettiği sayfadır. Aygıt şablonu aşağıdaki gibi aygıtlar özelliklerini belirtir:

* Telemetri, durum ve olay ölçümleri
* Özellikler
* Komutlar
* Görünümler

*Çözüm oluşturucu,* operatörlerin cihazları yönetmek için kullanmaları için formlar ve panolar da oluşturabilir.

Daha fazla bilgi almak için [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](howto-set-up-template.md) öğreticisine bakın. 

### <a name="data-export"></a>Veri dışarı aktarma
> [!div class="mx-imgBorder"]
> ![Veri verme sayfası](media/overview-iot-central-tour/export-pnp.png)

Veri dışa aktarma, uygulamadan harici sistemlere telemetri gibi veri akışları ayarlamanızı sağlar. Daha fazla bilgi edinmek için [Azure IoT Central makalesinde verilerinizi Dışa](./howto-export-data.md) Aktar'a bakın.

### <a name="administration"></a>Yönetim
> [!div class="mx-imgBorder"]
> ![Yönetim sayfası](media/overview-iot-central-tour/administration-pnp.png)

Yönetim sayfası, IoT Central uygulamanızı yapılandırmanızı ve özelleştirmenizi sağlar. Burada uygulama adınızı, URL'nizi, temalınızı değiştirebilir, kullanıcıları ve rolleri yönetebilir, API belirteçleri oluşturabilir ve uygulamanızı dışa aktarabilirsiniz. Daha fazla bilgi almak için [Azure IoT Central uygulamanızı yönetme](howto-administer.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central’a genel bir bakış elde ettiğinize ve kullanıcı arabiriminin düzenini tanıdığınıza göre önerilen sıradaki adım, [Azure IoT Central uygulaması oluşturma](quick-deploy-iot-central.md) hızlı başlangıcının tamamlanmasıdır.
