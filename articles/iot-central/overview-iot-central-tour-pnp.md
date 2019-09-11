---
title: Azure IoT Central Kullanıcı Arabirimi turuna katılın | Microsoft Docs
description: Oluşturucu olarak, bir IoT çözümü oluşturmak için kullanabileceğiniz Azure IoT Central kullanıcı arabiriminin temel alanlarını tanıyın.
author: dominicbetts
ms.author: dobett
ms.date: 07/05/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 4a0c9d16474ddf032ff88382bc240713bc734ff8
ms.sourcegitcommit: 8fea78b4521921af36e240c8a92f16159294e10a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2019
ms.locfileid: "70211893"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui-preview-features"></a>Azure IoT Central kullanıcı arabirimine yönelik tura katılın (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu makalede, Microsoft Azure IoT Central kullanıcı arabirimi tanıtılmaktadır. Kullanıcı arabirimini kullanarak bir Azure IoT Central çözümü ile bağlı cihazlarını oluşturabilir, yönetebilir ve kullanabilirsiniz.

_Oluşturucu_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü tanımlayabilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

* Çözümünüze bağlanan cihaz türlerini tanımlama.
* Cihazlarınız için kurallar ve eylemler yapılandırma.
* Kullanıcı arabirimini, çözümünüzü kullanan bir _operatör_ için özelleştirme.

_Operatör_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü yönetebilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

* Cihazlarınızı izleme.
* Cihazlarınızı yapılandırma.
* Cihazlarınızla ilgili sorunları giderme ve düzeltme.
* Yeni cihazları hazırlama.

## <a name="use-the-left-navigation-menu"></a>Sol gezinti menüsünü kullanma

Uygulamanın farklı bölümlerine erişmek için sol gezinti menüsünü kullanın. **<** **Veya>** öğesini seçerek gezinti çubuğunu genişletebilir veya daraltabilirsiniz:

:::row:::
  :::column span="":::
      ![Left navigation menu](media/overview-iot-central-tour-pnp/navigationbar.png)
  :::column-end:::
  :::column span="2":::

      **Dashboard** displays your application dashboard. As a builder, you can customize the dashboard for your operators. Users can also create their own  dashboards.
    
      **Devices** lists the simulated and real devices associated with each device template in the application. As an operator, you use the **Device Explorer** to manage your connected devices.
    
      **Device groups** lets you view and create device groups. As an operator, you can create device groups as a logical collections of devices specified by a query.

      **Rules** lets you edit rules that fire based on device telemetry and trigger customizable actions.
    
      **Analytics** shows analytics derived from device telemetry for devices and device groups. As an operator, you can create custom views on top of device data to derive insights from your application.
    
      **Jobs** enables bulk device management by having you create and run jobs to update your devices at scale.
    
      **Device templates** shows the tools a builder uses to create and manage device templates.
    
      **Data export** enables an administrator to configure a continuous export to other Azure services such as storage and queues.
    
      **Administration** shows the application administration pages where an administrator can manage application settings, users, and roles.
   :::column-end:::
:::row-end:::

## <a name="search-help-and-support"></a>Arama, yardım ve destek

Üstteki menü her sayfada görünür:

![Araç Çubuğu](media/overview-iot-central-tour-pnp/toolbar.png)

* Cihaz şablonlarını ve cihazlarını aramak için bir **arama** değeri girin.
* Kullanıcı arabirimi dilini veya temasını değiştirmek için **Ayarlar** simgesini seçin.
* Uygulamanın oturumunu kapatmak için **Hesap** simgesini seçin.
* Yardım ve destek almak için, kaynak listesinden **Yardım** açılır listesini seçin. Deneme uygulamasında, destek kaynakları [canlı sohbete](howto-show-hide-chat.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)erişim içerir.

Kullanıcı arabirimi için açık renkli tema veya koyu renkli temayı seçebilirsiniz:

![Kullanıcı arabirimi için tema seçme](media/overview-iot-central-tour-pnp/themes.png)

> [!NOTE]
> Yönetici, uygulama için özel bir tema yapılandırmışsa açık ve koyu temalar arasında seçim yapmak için kullanılabilir değildir.

## <a name="dashboard"></a>Pano

![Pano](media/overview-iot-central-tour-pnp/homepage.png)

* Pano, Azure IoT Central uygulamanızda oturum açtığınızda gördüğünüz ilk sayfasıdır. Bir Oluşturucu olarak, kutucuk ekleyerek uygulama panosunu diğer kullanıcılar için özelleştirebilirsiniz. Daha fazla bilgi edinmek için bkz. [cihaz şablonu ayarlama](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) öğreticisi.

* Bir operatör olarak, kişiselleştirilmiş panolar oluşturabilir ve bunlar arasında ve varsayılan pano arasında geçiş yapabilirsiniz. Daha fazla bilgi edinmek için kişisel panolar nasıl yapılır makalesi [oluşturun ve yönetin](howto-personalize-dashboard.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) bölümüne bakın.

## <a name="devices"></a>Cihazlar

![Cihazlar sayfası](media/overview-iot-central-tour-pnp/explorer.png)

Gezgin sayfası, Azure IoT Central uygulamanızda _cihaz şablonuna_göre gruplanmış _cihazları_ gösterir.

* Cihaz şablonu, uygulamanıza bağlanabilen bir cihaz türünü tanımlar. Daha fazla bilgi almak için bkz. [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).
* Cihaz, uygulamanızdaki gerçek veya sanal bir cihazı temsil eder. Daha fazla bilgi almak için bkz. [Azure IoT Central uygulamanıza yeni bir cihaz ekleme](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="device-groups"></a>Cihaz grupları

![Cihaz grupları sayfası](media/overview-iot-central-tour-pnp/devicesets.png)

_Cihaz grupları_ sayfası, Oluşturucu tarafından oluşturulan cihaz gruplarını gösterir. Bir cihaz grubu, ilişkili cihazların koleksiyonudur. Bir Oluşturucu bir cihaz grubuna dahil olan cihazları tanımlamak için bir sorgu tanımlar. Uygulamanızdaki Analizi özelleştirirken cihaz gruplarını kullanırsınız. Daha fazla bilgi edinmek için [Azure IoT Central uygulama makalesindeki cihaz gruplarını kullanma](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesine bakın.

## <a name="rules"></a>Kurallar

![Kurallar sayfası](media/overview-iot-central-tour-pnp/rules.png)

Kurallar sayfası telemetri, cihaz durumu veya cihaz olaylarını temel alan kurallar tanımlamanızı sağlar. Bir kural tetiklendiğinde, bir işlece e-posta gönderme gibi bir eylemi tetikleyebilirler. Oluşturucu, kuralları oluşturmak ve yönetmek için bu sayfayı kullanır. Daha fazla bilgi için bkz. [Azure 'da cihazlarınız için kuralları ve eylemleri yapılandırma IoT Central](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) öğreticisi.

## <a name="analytics"></a>Analiz

![Analiz sayfası](media/overview-iot-central-tour-pnp/analytics.png)

Analiz sayfasında, uygulamanıza bağlı cihazların nasıl davrandığını anlamanıza yardımcı olan grafikler gösterilir. Operatör bu sayfayı kullanarak bağlı cihazlarla ilgili sorunları izler ve araştırır. Oluşturucu, bu sayfada gösterilen grafikleri tanımlayabilir. Daha fazla bilgi almak için [Azure IoT Central uygulamanız için özel analizler oluşturma](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesine bakın.

## <a name="jobs"></a>İşler

![İşler sayfası](media/overview-iot-central-tour-pnp/jobs.png)

İşler sayfası cihazlarınızda toplu cihaz yönetimi işlemlerini çalıştırmanızı sağlar. Oluşturucu, cihaz özelliklerini, ayarları ve komutları güncelleştirmek için bu sayfayı kullanır. Daha fazla bilgi edinmek için [İş çalıştırma](howto-run-a-job.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesine bakın.

## <a name="device-templates"></a>Cihaz şablonları

![Cihaz şablonları sayfası](media/overview-iot-central-tour-pnp/templates.png)

Cihaz şablonları sayfası, bir oluşturucunun uygulamadaki cihaz şablonlarını oluşturduğu ve yönettiği yerdir. Cihaz şablonu, şöyle bir cihaz özelliği belirtir:

* Telemetri, durum ve olay ölçümleri.
* Özelliklerinin.
* Komut.

Oluşturucu Ayrıca, cihazları yönetmek için kullanılacak operatörler için formlar ve panolar oluşturabilir.

Daha fazla bilgi almak için [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) öğreticisine bakın.

## <a name="data-export"></a>Veri dışarı aktarma

![Veri dışa aktarma sayfası](media/overview-iot-central-tour-pnp/export.png)

Veri dışa aktarma sayfası, bir yöneticinin uygulama aracılığıyla telemetri gibi verilerin nasıl akışının tanımlanabileceğini tanımlar. Diğer hizmetler, dışarıya aktarılmış verileri saklayabilir veya analiz için kullanabilir. Daha fazla bilgi edinmek için bkz. [Azure 'da verilerinizi dışarı aktarma IoT Central](howto-export-data.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesi.

## <a name="administration"></a>Yönetim

![Yönetim sayfası](media/overview-iot-central-tour-pnp/administration.png)

Yönetim sayfası, yönetici tarafından uygulamadaki kullanıcıları ve rolleri tanımlama ve Kullanıcı arabirimini özelleştirme gibi kullanılan araçların bağlantılarını içerir. Daha fazla bilgi almak için [Azure IoT Central uygulamanızı yönetme](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central’a genel bir bakış elde ettiğinize ve kullanıcı arabiriminin düzenini tanıdığınıza göre önerilen sıradaki adım, [Azure IoT Central uygulaması oluşturma](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hızlı başlangıcının tamamlanmasıdır.
