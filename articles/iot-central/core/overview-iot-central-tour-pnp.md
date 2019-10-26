---
title: Azure IoT Central Kullanıcı Arabirimi turuna katılın | Microsoft Docs
description: IoT çözümünüzü oluşturmak, yönetmek ve kullanmak için kullandığınız Azure IoT Central Kullanıcı arabiriminin temel alanlarıyla ilgili bilgi sahibi olun.
author: lmasieri
ms.author: lmasieri
ms.date: 10/21/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: 0bd078e8c920ca1ebe01767915036ca84650aadd
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72958199"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui-preview-features"></a>Azure IoT Central kullanıcı arabirimine yönelik tura katılın (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Bu makalede, Microsoft Azure IoT Central kullanıcı arabirimi tanıtılmaktadır. Kullanıcı arabirimini kullanarak bir Azure IoT Central çözümü ile bağlı cihazlarını oluşturabilir, yönetebilir ve kullanabilirsiniz.

Bir _çözüm Oluşturucusu_olarak Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü tanımlayabilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

* Çözümünüze bağlanan cihaz türlerini tanımlama.
* Cihazlarınız için kurallar ve eylemler yapılandırma. 
* Kullanıcı arabirimini, çözümünüzü kullanan bir _operatör_ için özelleştirme.

_Operatör_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü yönetebilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

* Cihazlarınızı izleme.
* Cihazlarınızı yapılandırma.
* Cihazlarınızla ilgili sorunları giderme ve düzeltme.
* Yeni cihazları hazırlama.

## <a name="iot-central-homepage"></a>IoT Central giriş sayfası

[IoT Central giriş](https://aka.ms/iotcentral-get-started) sayfası sayfası, IoT Central sunulan en son haberler ve özellikler hakkında daha fazla bilgi edinebilirsiniz, yeni uygulamalar oluşturabilir ve var olan uygulamanızı görebilir ve başlatabilir.

> [!div class="mx-imgBorder"]
> ![IoT Central giriş sayfası](media/overview-iot-central-tour-pnp/iot-central-homepage-pnp.png)

### <a name="create-an-application"></a>Uygulama oluşturma

Yapı bölümünde, sektörle ilgili IoT Central şablonlarının listesine giderek hızlı bir şekilde başlamanıza veya özel bir uygulama şablonu kullanarak sıfırdan başlamanıza yardımcı olabilirsiniz.  
> [!div class="mx-imgBorder"]
> ![IoT Central derleme sayfası](media/overview-iot-central-tour-pnp/iot-central-build-pnp.png)

Daha fazla bilgi edinmek için bkz. [Azure IoT Central uygulaması oluşturma](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hızlı başlangıcı.

### <a name="launch-your-application"></a>Uygulamanızı başlatın

Uygulama oluşturma sırasında sizin veya çözüm oluşturucunun seçim yaptığı URL 'ye giderek IoT Central uygulamanızı başlatabilirsiniz. Ayrıca, [IoT Central App Manager](https://aka.ms/iotcentral-apps)'da erişiminiz olan tüm uygulamaların listesini de görebilirsiniz.

> [!div class="mx-imgBorder"]
> ![IoT Central App Manager](media/overview-iot-central-tour-pnp/app-manager-pnp.png)

## <a name="navigate-your-application"></a>Uygulamanızda gezinin

IoT uygulamanızın içindeyken, farklı alanlara erişmek için sol bölmeyi kullanın. Gezinti çubuğunun üstündeki üç çizgili simgeyi seçerek gezinti çubuğunu genişletebilir veya daraltabilirsiniz:

> [!NOTE]
> Gezinti çubuğunda gördüğünüz öğeler, Kullanıcı rolünüze göre değişir. [Kullanıcıları ve rolleri yönetme](howto-manage-users-roles-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)hakkında daha fazla bilgi edinin. 

:::row:::
  :::column span="":::
      > [!div class="mx-imgBorder"]
      > ![sol bölme](media/overview-iot-central-tour-pnp/navigationbar-pnp.png)
  :::column-end:::
  :::column span="2":::
     **Pano** , uygulama panonuzu görüntüler. Bir *çözüm Oluşturucusu*olarak, işleçlerinizin genel panosunu özelleştirebilirsiniz. Kullanıcı rollerine bağlı olarak, işleçler kendi kişisel panolarını de oluşturabilir.
     
     **Cihazlar** , bağlı cihazlarınızı ve gerçek ve benzetimli cihazları yönetmenizi sağlar.

     **Cihaz grupları** , bir sorgu tarafından belirtilen cihazların mantıksal koleksiyonlarını görüntülemenizi ve oluşturmanızı sağlar. Toplu işlemleri gerçekleştirmek için bu sorguyu kaydedebilir ve uygulama aracılığıyla cihaz gruplarını kullanabilirsiniz.

     **Kurallar** , cihazlarınızı izlemek için kurallar oluşturmanızı ve düzenlemenizi sağlar. Kurallar cihaz telemetri temelinde değerlendirilir ve özelleştirilebilir eylemleri tetikler.

     **Analytics** , uygulamanızdan Öngörüler elde etmek için cihaz verilerinin üzerine özel görünümler oluşturmanızı sağlar.

     **İşler** , toplu işlemleri çalıştırarak cihazlarınızı ölçekli olarak yönetmenizi sağlar.

     **Cihaz şablonları** , uygulamanıza bağlanan cihazların özelliklerini oluşturduğunuz ve yönettiğiniz yerdir.

     **Veri dışa aktarma** , depolama ve kuyruklar gibi dış hizmetlere sürekli bir dışarı aktarma yapılandırmanıza olanak sağlar.

     **Yönetim** , uygulamanızın ayarlarını, özelleştirmesini, faturalandırmasını, kullanıcılarını ve rollerini yönetebileceğiniz yerdir.

     **IoT Central** , *yöneticilerin* IoT Central App Manager 'a geri atlamasını sağlar.
     
   :::column-end:::
:::row-end:::

### <a name="search-help-theme-and-support"></a>Arama, yardım, tema ve destek

Üstteki menü her sayfada görünür:

> [!div class="mx-imgBorder"]
> ![araç çubuğu](media/overview-iot-central-tour-pnp/toolbar-pnp.png)

* Cihaz şablonlarını ve cihazlarını aramak için bir **arama** değeri girin.
* Kullanıcı arabirimi dilini veya temasını değiştirmek için **Ayarlar** simgesini seçin. [Uygulama tercihlerinizi yönetme](howto-manage-preferences.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hakkında daha fazla bilgi edinin
* Uygulamanın oturumunu kapatmak için **Hesap** simgesini seçin.
* Yardım ve destek almak için, kaynak listesinden **Yardım** açılır listesini seçin. Deneme uygulamasında, destek kaynakları [canlı sohbete](howto-show-hide-chat.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)erişim içerir.

Kullanıcı arabirimi için açık renkli tema veya koyu renkli temayı seçebilirsiniz:

> [!NOTE]
> Yönetici, uygulama için özel bir tema yapılandırmışsa açık ve koyu temalar arasında seçim yapmak için kullanılabilir değildir.

> [!div class="mx-imgBorder"]
> ![Kullanıcı arabirimi için bir tema seçin](media/overview-iot-central-tour-pnp/themes-pnp.png)

### <a name="dashboard"></a>Pano
> [!div class="mx-imgBorder"]
> ![Pano](media/overview-iot-central-tour-pnp/dashboard-pnp.png)

* Pano, Azure IoT Central uygulamanızda oturum açtığınızda gördüğünüz ilk sayfasıdır. Bir *çözüm Oluşturucusu*olarak, diğer kullanıcılar için birden çok genel uygulama panosu oluşturabilir ve bunları özelleştirebilirsiniz. [Panonuza kutucuk ekleme](howto-add-tiles-to-your-dashboard.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hakkında daha fazla bilgi edinin

* Bir *operatör*olarak, kullanıcı rolünüzün izin veriyorsa, ilgilendiğiniz şeyi izlemek için kişisel panolar oluşturabilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure IoT Central kişisel panolar](howto-create-personal-dashboards.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) nasıl yapılır makalesi oluşturma.

### <a name="devices"></a>Cihazlar

> [!div class="mx-imgBorder"]
> ![cihazlar sayfası](media/overview-iot-central-tour-pnp/devices-pnp.png)

Gezgin sayfası, Azure IoT Central uygulamanızda _cihaz şablonuna_göre gruplanmış _cihazları_ gösterir. 

* Cihaz şablonu, uygulamanıza bağlanabilen bir cihaz türünü tanımlar.
* Cihaz, uygulamanızdaki gerçek veya sanal bir cihazı temsil eder.

Daha fazla bilgi edinmek için [cihazlarınızı izleme öğreticisine](tutorial-monitor-devices.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) bakın. 

### <a name="device-groups"></a>Cihaz grupları

> [!div class="mx-imgBorder"]
> ![cihaz grupları sayfası](media/overview-iot-central-tour-pnp/device-groups-pnp.png)

Cihaz grubu, ilişkili cihazların bir koleksiyonudur. *Çözüm Oluşturucu* bir cihaz grubuna dahil olan cihazları tanımlamak için bir sorgu tanımlar. Uygulamanızda toplu işlemler gerçekleştirmek için cihaz gruplarını kullanırsınız. Daha fazla bilgi edinmek için [Azure IoT Central uygulama makalesindeki cihaz gruplarını kullanma](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesine bakın.

### <a name="rules"></a>Kurallar
> [!div class="mx-imgBorder"]
> ![kuralları sayfası](media/overview-iot-central-tour-pnp/rules-pnp.png)

Kurallar sayfası, cihazların telemetri, durum veya olaylarını temel alan kurallar tanımlamanızı sağlar. Bir kural tetiklendiğinde, bir e-posta gönderme, Web kancası uyarıları aracılığıyla bir dış sisteme bildirme vb. gibi bir veya daha fazla eylemi tetikleyebilirler. Bilgi edinmek için bkz. [kuralları yapılandırma](tutorial-create-telemetry-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) öğreticisi. 

### <a name="analytics"></a>Analiz

> [!div class="mx-imgBorder"]
> ![Analytics sayfası](media/overview-iot-central-tour-pnp/analytics-pnp.png)

Analiz, uygulamanızdan Öngörüler elde etmek için cihaz verilerinin üzerine özel görünümler oluşturmanızı sağlar. Daha fazla bilgi almak için [Azure IoT Central uygulamanız için özel analizler oluşturma](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesine bakın.

### <a name="jobs"></a>İş

> [!div class="mx-imgBorder"]
> ![Işleri sayfası](media/overview-iot-central-tour-pnp/jobs-pnp.png)

İşler sayfası cihazlarınızda toplu cihaz yönetimi işlemlerini çalıştırmanızı sağlar. Cihaz özelliklerini, ayarlarını güncelleştirebilir ve komutları cihaz gruplarıyla karşı çalıştırabilirsiniz. Daha fazla bilgi edinmek için [İş çalıştırma](howto-run-a-job.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesine bakın.

### <a name="device-templates"></a>Cihaz şablonları

> [!div class="mx-imgBorder"]
> ![cihaz şablonları sayfası](media/overview-iot-central-tour-pnp/templates-pnp.png)

Cihaz şablonları sayfası, bir oluşturucunun uygulamadaki cihaz şablonlarını oluşturduğu ve yönettiği yerdir. Cihaz şablonu, şu gibi cihaz özelliklerini belirtir:

* Telemetri, durum ve olay ölçümleri
* Özellikler
* Komutlar
* Görünümler

*Çözüm Oluşturucu* Ayrıca, cihazları yönetmek için kullanılacak operatörler için formlar ve panolar oluşturabilir.

Daha fazla bilgi almak için [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](howto-set-up-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) öğreticisine bakın. 

### <a name="data-export"></a>Veri dışarı aktarma
> [!div class="mx-imgBorder"]
> ![veri dışa aktarma sayfası](media/overview-iot-central-tour-pnp/export-pnp.png)

Veri dışa aktarma, uygulama telemetri gibi veri akışlarını uygulamadan Harici sistemlere ayarlamanıza olanak sağlar. Daha fazla bilgi edinmek için bkz. [Azure 'da verilerinizi dışarı aktarma IoT Central](howto-export-data-blob-storage.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesi.

### <a name="administration"></a>Yönetim
> [!div class="mx-imgBorder"]
> ![yönetim sayfası](media/overview-iot-central-tour-pnp/administration-pnp.png)

Yönetim sayfası IoT Central uygulamanızı yapılandırmanıza ve özelleştirmenize olanak sağlar. Burada, uygulamanızın adını, URL 'sini, temalarını değiştirebilir, kullanıcıları ve rolleri yönetebilir, API belirteçleri oluşturabilir ve uygulamanızı dışarı aktarabilirsiniz. Daha fazla bilgi almak için [Azure IoT Central uygulamanızı yönetme](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central’a genel bir bakış elde ettiğinize ve kullanıcı arabiriminin düzenini tanıdığınıza göre önerilen sıradaki adım, [Azure IoT Central uygulaması oluşturma](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hızlı başlangıcının tamamlanmasıdır.
