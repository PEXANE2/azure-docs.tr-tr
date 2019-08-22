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
ms.openlocfilehash: 6c06d789e6a005c0079f889cead6a75f4d0baf19
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69875707"
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

| Menü | Açıklama |
| ---- | ----------- |
| ![Sol gezinti menüsü](media/overview-iot-central-tour/navigationbar.png) | <ul><li>**Pano** düğmesi uygulama panonuzu görüntüler. Bir Oluşturucu olarak, operatörlerinizi panosunu özelleştirebilirsiniz. Kullanıcılar kendi panoları da oluşturabilir.</li><li>**Device Explorer** düğme, uygulamadaki her bir cihaz şablonuyla ilişkili olan sanal ve gerçek cihazları listeler. Operatör olarak, **Device Explorer**’ı kullanarak bağlı cihazlarınızı yönetebilirsiniz.</li><li>**Cihaz Kümeleri** düğmesi, cihaz kümelerini görüntüleyip oluşturmanızı sağlar. Operatör olarak, bir sorgu tarafından belirtilen cihazların mantıksal bir koleksiyonu olarak cihaz kümeleri oluşturabilirsiniz.</li><li>**Analiz** düğmesi, cihazlar ve cihaz kümeleri için cihaz telemetrisinden türetilen analizleri gösterir. Operatör olarak, uygulamanızdan içgörüler türetmek için cihaz verileri üzerinde özel görünümler oluşturabilirsiniz.</li><li>**İşler** düğmesi, uygun ölçekte güncelleştirmeler gerçekleştirmek için iş oluşturmanız ve çalıştırmanız gerekmeden toplu cihaz yönetimi sağlar.</li><li>**Cihaz şablonları** düğmesi, bir oluşturucunun cihaz şablonları oluşturmak ve yönetmek için kullandığı araçları gösterir.</li><li>**Sürekli veri dışa aktarma** düğmesi bir yöneticinin depolama ve kuyruklar gibi diğer Azure hizmetlerine sürekli bir dışarı aktarma yapılandırmasını sağlar.</li><li>**Yönetim** düğmesi, bir yöneticinin uygulama ayarlarını, kullanıcıları ve rolleri yönetebildiği uygulama yönetimi sayfalarını gösterir.</li></ul> |

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