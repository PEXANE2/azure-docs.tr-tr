---
title: Azure IoT Central Kullanıcı Arabirimi turuna katılın | Microsoft Docs
description: Oluşturucu olarak, bir IoT çözümü oluşturmak için kullanabileceğiniz Azure IoT Central kullanıcı arabiriminin temel alanlarını tanıyın.
author: dominicbetts
ms.author: dobett
ms.date: 06/09/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: c4eba3e08e403243e76ac891d3bfcb59f12cad85
ms.sourcegitcommit: 8fea78b4521921af36e240c8a92f16159294e10a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2019
ms.locfileid: "70211912"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui"></a>Azure IoT Central Kullanıcı Arabirimi turuna katılın

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Bu makalede, Microsoft Azure IoT Central kullanıcı arabirimi tanıtılmaktadır. Kullanıcı arabirimini kullanarak bir Azure IoT Central çözümü ile bağlı cihazlarını oluşturabilir, yönetebilir ve kullanabilirsiniz.

_Oluşturucu_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü tanımlayabilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

- Çözümünüze bağlanan cihaz türlerini tanımlama.
- Cihazlarınız için kurallar ve eylemler yapılandırma.
- Kullanıcı arabirimini, çözümünüzü kullanan bir _operatör_ için özelleştirme.

_Operatör_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü yönetebilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

- Cihazlarınızı izleme.
- Cihazlarınızı yapılandırma.
- Cihazlarınızla ilgili sorunları giderme ve düzeltme.
- Yeni cihazları hazırlama.

## <a name="use-the-left-navigation-menu"></a>Sol gezinti menüsünü kullanma

Uygulamanın farklı bölümlerine erişmek için sol gezinti menüsünü kullanın. **<** **Veya>** öğesini seçerek gezinti çubuğunu genişletebilir veya daraltabilirsiniz:

:::row:::
  :::column span="":::
      ![Left navigation menu](media/overview-iot-central-tour/navigationbar.png)
  :::column-end:::
  :::column span="2":::

      **Dashboard** displays your application dashboard. As a builder, you can customize the dashboard for your operators. Users can also create their own  dashboards.
    
      **Device Explorer** lists the simulated and real devices associated with each device template in the application. As an operator, you use the **Device Explorer** to manage your connected devices.
    
      **Device Sets** enables you to view and create device sets. As an operator, you can create device sets as a logical collection of devices specified by a query.
    
      **Analytics** shows analytics derived from device telemetry for devices and device sets. As an operator, you can create custom views on top of device data to derive insights from your application.
    
      **Jobs** enables bulk device management by having you create and run jobs to perform updates at scale.
    
      **Device Templates** shows the tools a builder uses to create and manage device templates.
    
      **Continuous Data Export** enables an administrator to configure a continuous export to other Azure services such as storage and queues.
    
      **Administration** shows the application administration pages where an administrator can manage application settings, users, and roles.
   :::column-end:::
:::row-end:::

## <a name="search-help-and-support"></a>Arama, yardım ve destek

Üstteki menü her sayfada görünür:

![Araç Çubuğu](media/overview-iot-central-tour/toolbar.png)

- Cihaz şablonlarını ve cihazlarını aramak için bir **arama** değeri girin.
- Kullanıcı arabirimi dilini veya temasını değiştirmek için **Ayarlar** simgesini seçin.
- Uygulamanın oturumunu kapatmak için **Hesap** simgesini seçin.
- Yardım ve destek almak için, kaynak listesinden **Yardım** açılır listesini seçin. Deneme uygulamasında, destek kaynakları [canlı sohbete](howto-show-hide-chat.md)erişim içerir.

Kullanıcı arabirimi için açık renkli tema veya koyu renkli temayı seçebilirsiniz:

![Kullanıcı arabirimi için tema seçme](media/overview-iot-central-tour/themes.png)

> [!NOTE]
> Yönetici, uygulama için özel bir tema yapılandırmışsa açık ve koyu temalar arasında seçim yapmak için kullanılabilir değildir.

## <a name="dashboard"></a>Pano

![Pano](media/overview-iot-central-tour/homepage.png)

* Pano, Azure IoT Central uygulamanızda oturum açtığınızda gördüğünüz ilk sayfasıdır. Bir Oluşturucu olarak, kutucuk ekleyerek uygulama panosunu diğer kullanıcılar için özelleştirebilirsiniz. Daha fazla bilgi almak için [Azure IoT Central operatör görünümünü özelleştirme](tutorial-customize-operator.md) öğreticisine bakın.

* Bir operatör olarak, kişiselleştirilmiş panolar oluşturabilir ve bunlar arasında ve varsayılan pano arasında geçiş yapabilirsiniz. Daha fazla bilgi edinmek için kişisel panolar nasıl yapılır makalesi [oluşturun ve yönetin](howto-personalize-dashboard.md) bölümüne bakın.

## <a name="device-explorer"></a>Device Explorer

![Gezgin sayfası](media/overview-iot-central-tour/explorer.png)

Gezgin sayfası, Azure IoT Central uygulamanızda _cihaz şablonuna_göre gruplanmış _cihazları_ gösterir.

* Cihaz şablonu, uygulamanıza bağlanabilen bir cihaz türünü tanımlar. Daha fazla bilgi almak için bkz. [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md).
* Cihaz, uygulamanızdaki gerçek veya sanal bir cihazı temsil eder. Daha fazla bilgi almak için bkz. [Azure IoT Central uygulamanıza yeni bir cihaz ekleme](tutorial-add-device.md).

## <a name="device-sets"></a>Cihaz kümeleri

![Cihaz Kümeleri sayfası](media/overview-iot-central-tour/devicesets.png)

_Cihaz kümeleri_ sayfası, oluşturucu tarafından oluşturulan cihaz kümelerini gösterir. Cihaz kümesi, ilişkili cihazların bir koleksiyonudur. Oluşturucu, bir cihaz kümesine eklenen cihazları belirlemek üzere bir sorgu tanımlar. Uygulamanızdaki analizleri özelleştirirken cihaz kümelerini kullanırsınız. Daha fazla bilgi almak için [Azure IoT Central uygulamanızda cihaz kümelerini kullanma](howto-use-device-sets.md) makalesine bakın.

## <a name="analytics"></a>Analiz

![Analiz sayfası](media/overview-iot-central-tour/analytics.png)

Analiz sayfasında, uygulamanıza bağlı cihazların nasıl davrandığını anlamanıza yardımcı olan grafikler gösterilir. Operatör bu sayfayı kullanarak bağlı cihazlarla ilgili sorunları izler ve araştırır. Oluşturucu, bu sayfada gösterilen grafikleri tanımlayabilir. Daha fazla bilgi almak için [Azure IoT Central uygulamanız için özel analizler oluşturma](howto-use-device-sets.md) makalesine bakın.

## <a name="jobs"></a>İşler

![İşler sayfası](media/overview-iot-central-tour/jobs.png)

İşler sayfası cihazlarınızda toplu cihaz yönetimi işlemlerini çalıştırmanızı sağlar. Oluşturucu, cihaz özelliklerini, ayarları ve komutları güncelleştirmek için bu sayfayı kullanır. Daha fazla bilgi edinmek için [İş çalıştırma](howto-run-a-job.md) makalesine bakın.

## <a name="device-templates"></a>Cihaz şablonları

![Cihaz şablonları sayfası](media/overview-iot-central-tour/templates.png)

Cihaz şablonları sayfası, bir oluşturucunun uygulamadaki cihaz şablonlarını oluşturduğu ve yönettiği yerdir. Cihaz şablonu, şöyle bir cihaz özelliği belirtir:

- Telemetri, durum ve olay ölçümleri.
- Ayarlar ve Özellikler.
- Komut.
- Olayları veya telemetri değerlerini temel alan kurallar.

Daha fazla bilgi almak için [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md) öğreticisine bakın.

## <a name="continuous-data-export"></a>Verileri sürekli dışarı aktarma

![Sürekli veri dışa aktarma sayfası](media/overview-iot-central-tour/export.png)

Sürekli veri dışa aktarma sayfası, bir yöneticinin uygulama aracılığıyla telemetri gibi verilerin nasıl akışının tanımlanabileceğini tanımlar. Diğer hizmetler, dışarıya aktarılmış verileri saklayabilir veya analiz için kullanabilir. Daha fazla bilgi edinmek için bkz. [Azure 'da verilerinizi dışarı aktarma IoT Central](howto-export-data.md) makalesi.

## <a name="administration"></a>Yönetim

![Yönetim sayfası](media/overview-iot-central-tour/administration.png)

Yönetim sayfası, yönetici tarafından uygulamadaki kullanıcıları ve rolleri tanımlama ve Kullanıcı arabirimini özelleştirme gibi kullanılan araçların bağlantılarını içerir. Daha fazla bilgi almak için [Azure IoT Central uygulamanızı yönetme](howto-administer.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central’a genel bir bakış elde ettiğinize ve kullanıcı arabiriminin düzenini tanıdığınıza göre önerilen sıradaki adım, [Azure IoT Central uygulaması oluşturma](quick-deploy-iot-central.md) hızlı başlangıcının tamamlanmasıdır.