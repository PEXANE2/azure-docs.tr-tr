---
title: Hızlı başlangıç-Azure IoT Central sanal cihaz ekleme
description: Bu hızlı başlangıçta bir cihaz şablonu oluşturma ve IoT Central uygulamanıza sanal cihaz ekleme işlemleri gösterilmektedir.
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 489bf81388c1bb889756d54957e791282054f0f7
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169550"
---
# <a name="quickstart-add-a-simulated-device-to-your-iot-central-application"></a>Hızlı başlangıç: IoT Central uygulamanıza sanal cihaz ekleme

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bir cihaz şablonu, IoT Central uygulamanıza bağlanan bir cihazın yeteneklerini tanımlar. Özellikler cihaz gönderme, cihaz özellikleri ve bir cihazın yanıt verdiği komutları içerir. Bir cihaz şablonundan, bir Oluşturucu veya operatör bir uygulamaya hem gerçek hem de sanal cihaz ekleyebilir. Sanal cihazlar, gerçek cihazları bağlanmadan önce IoT Central uygulamanızın davranışını test etmek için kullanışlıdır.

Bu hızlı başlangıçta, bir *Mxyonga IoT devkit* (devkit) panosu için bir cihaz şablonu eklersiniz ve sanal bir cihaz oluşturacaksınız. Bu hızlı başlangıcı tamamlayabilmeniz için gerçek bir cihaza ihtiyacınız yoktur, cihazın simülasyonu ile çalışırsınız. Bir DevKit cihazı:

* Sıcaklık gibi telemetri gönderir.
* , Parlaklık düzeyi gibi cihaza özgü özellikleri raporlar.
* Açma ve kapatma gibi komutlara yanıt verir.
* Bellenim sürümü ve seri numarası gibi genel cihaz özelliklerini raporlar.

## <a name="prerequisites"></a>Önkoşullar

Özel uygulama **> özel uygulama** şablonunu kullanarak bir IoT Central uygulaması oluşturmak için [Azure IoT Central uygulaması oluşturma](./quick-deploy-iot-central.md) hızlı başlangıcı ' nı doldurun.

## <a name="create-a-template"></a>Şablon oluşturma

Bir Oluşturucu olarak, IoT Central uygulamanızda cihaz şablonları oluşturabilir ve düzenleyebilirsiniz. Bir cihaz şablonu yayımladıktan sonra, cihaz şablonundan sanal cihaz oluşturabilir veya gerçek cihazları bağlayabilirsiniz. Sanal cihazlar, gerçek bir cihazı bağlanmadan önce uygulamanızın davranışını test etmenize olanak sağlar.

Uygulamanıza yeni bir cihaz şablonu eklemek için sol bölmedeki **cihaz şablonları** sekmesini seçin.

![Cihaz şablonları sayfası](./media/quick-create-pnp-device/devicedefinitions.png)

Bir cihaz şablonu, cihazın gönderdiği Telemetriyi, cihaz özelliklerini ve cihazın yanıt verdiği komutları tanımlayan bir cihaz yetenek modeli içerir.

### <a name="add-a-device-capability-model"></a>Cihaz yetenek modeli ekleme

IoT Central uygulamanıza cihaz yetenek modeli eklemek için çeşitli seçenekler vardır. Sıfırdan bir model oluşturabilir, bir dosyadan model aktarabilir veya cihaz kataloğundan bir cihaz seçebilirsiniz. IoT Central Ayrıca bir cihaz ilk kez bağlandığı zaman bir depodan bir modelden otomatik olarak bir model aktardığı bir *cihaz* yaklaşımını da destekler. Bu hızlı başlangıçta cihaz yetenek modelini içeri aktarmak için cihaz kataloğundan bir cihaz seçersiniz.

Aşağıdaki adımlarda, bir **Mxyonga IoT DevKit** cihazı için yetenek modelini içeri aktarmak üzere cihaz kataloğunun nasıl kullanılacağı gösterilmektedir. Bu cihazlar, uygulamanıza sıcaklık gibi telemetri gönderir:

1. Yeni bir cihaz şablonu eklemek için **cihaz şablonları** sayfasında **+** ' yi seçin.

1. **Şablon türü seç** sayfasında, **Mxyongaıot devkit** kutucuğunu bulana kadar aşağı kaydırın.

1. **Mxyongaıot DevKit** kutucuğunu seçin ve ardından **İleri: Özelleştir**' i seçin.

1. **İnceleme** sayfasında **Oluştur**' u seçin.

1. Birkaç saniye sonra, yeni cihaz şablonunuzu görebilirsiniz:

    ![Mxyongaıot DevKit cihaz şablonu](./media/quick-create-pnp-device/devkit-template.png)

    Mxyongaıot DevKit yetenek modeli, **mxchip_sensor**, **Mxchip_settings**ve **cihaz bilgileri**gibi arabirimleri içerir. Arabirimler bir Mxyongaıot DevKit cihazının yeteneklerini tanımlar. Özellikler, bir cihazın gönderdiği Telemetriyi, bir cihaz rapor özelliklerini ve bir cihazın yanıt verdiği komutları içerir.

### <a name="add-cloud-properties"></a>Bulut özellikleri ekle

Bir cihaz şablonu, bulut özellikleri içerebilir. Bulut özellikleri yalnızca IoT Central uygulamasında bulunur ve bir cihaza hiçbir şekilde gönderilmez veya buradan alınmaz.

1. **Bulut özellikleri** ' ni seçin ve **+ bulut özelliği Ekle**' ye tıklayın. Cihaz şablonunuza iki bulut özelliği eklemek için aşağıdaki tablodaki bilgileri kullanın:

    | Görünen Ad      | Anlamsal tür | Şema |
    | ----------------- | ------------- | ------ |
    | Son Hizmet Tarihi | Hiçbiri          | Tarih   |
    | Müşteri adı     | Hiçbiri          | Dize |

1. Değişikliklerinizi kaydetmek için **Kaydet** ' i seçin:

    ![Bulut özellikleri](media/quick-create-pnp-device/cloudproperties.png)

## <a name="views"></a>Görünümler

Bir Oluşturucu olarak, uygulamayı bir işlece cihazla ilgili bilgileri görüntüleyecek şekilde özelleştirebilirsiniz. Özelleştirmeleriniz, uygulamaya bağlı cihazları yönetmek için işlecini etkinleştirir. Cihazlarla etkileşim kurmak için kullanılacak bir operatör için iki tür görünüm oluşturabilirsiniz:

* Cihaz ve bulut özelliklerini görüntüleme ve düzenleme için formlar.
* Aygıtları, gönderdikleri telemetri dahil olmak üzere görselleştirmeye yönelik panolar.

### <a name="default-views"></a>Varsayılan görünümler

Varsayılan görünümler, önemli cihaz bilgilerinizin görselleştirilmesi ile çalışmaya başlamanız için hızlı bir yoldur. Cihaz şablonunuz için en fazla üç varsayılan görünüm oluşturmuş olabilirsiniz:

* **Komutlar** görünümü, operatörünüzün komutlarınıza erişmenizi sağlar.
* **Genel bakış** görünümü cihaz telemetrisini görüntülemek için grafikleri ve ölçümleri kullanır.
* **Hakkında** görünümü cihaz özelliklerini görüntüler.

Cihaz şablonundaki **Görünümler** düğümünü seçin. IoT Central, şablonu eklediğinizde bir **genel bakış** ve **hakkında** bir görünüm üretdiğine bakabilirsiniz.

Bir işlecin cihazı yönetmek için kullanabileceği yeni bir **Cihaz yönet** formu eklemek için:

1. **Görünümler** düğümünü seçin ve ardından yeni bir görünüm eklemek için **cihazı ve bulut verilerini Düzenle** kutucuğunu seçin.

1. **Cihazı yönetmek**için form adını değiştirin.

1. **Müşteri adı** ve **son hizmet tarihi** bulut özelliklerini ve **fan hızı** özelliğini seçin. Ardından **Bölüm Ekle**' yi seçin:

    ![Yeni form oluştur](media/quick-create-pnp-device/new-form.png)

1. Yeni formunuzu kaydetmek için **Kaydet** ' i seçin.

## <a name="publish-device-template"></a>Cihaz şablonunu Yayımla

Sanal bir cihaz oluşturmadan veya gerçek bir cihazı bağlayabilmeniz için önce cihaz şablonunuzu yayımlamanız gerekir. IoT Central şablonu ilk oluştururken yayımlamakla birlikte, güncelleştirilmiş sürümü yayımlamanız gerekir.

Bir cihaz şablonunu yayımlamak için:

1. Cihaz **şablonları** sayfasından cihaz şablonunuza gidin.

1. **Yayımla**' yı seçin:

    ![Yayımlanan model](media/quick-create-pnp-device/publishedmodel.png)

1. **Bu cihaz şablonunu uygulama** iletişim **kutusunda Yayımla ' yı seçin.** 

Bir cihaz şablonunu yayımladıktan sonra, **cihazlar** sayfasında görünür. Yayımlanmış bir cihaz şablonunda, yeni bir sürüm oluşturmadan bir cihaz yetenek modeli düzenleyemezsiniz. Ancak, bulut özellikleri, özelleştirmeler ve görünümlerde, sürüm oluşturmadan yayımlanmış bir cihaz şablonunda güncelleştirmeler yapabilirsiniz. Herhangi bir değişiklik yaptıktan sonra, bu değişiklikleri işletmenizin dışına göndermek için **Yayımla** ' yı seçin.

## <a name="add-a-simulated-device"></a>Sanal cihaz ekleme

Uygulamanıza sanal bir cihaz eklemek için, oluşturduğunuz **Mxyonga IoT DevKit** cihaz şablonunu kullanın.

1. Yeni bir cihazı operatör olarak eklemek için sol bölmedeki **cihazlar** ' ı seçin. **Cihazlar** sekmesinde **tüm cihazlar** ve **mxyonga IoT devkit** cihaz şablonu gösterilmektedir. **Mxyongaıot DevKit**' i seçin.

1. Sanal bir DevKit cihazı eklemek için **+** ' yi seçin. Önerilen **CIHAZ kimliğini** kullanın veya kendi küçük BIR **Cihaz Kimliğinizi**girin. Ayrıca, yeni cihazınız için bir ad girebilirsiniz. **Benzetimli** geçiş 'nin **Açık** olduğundan emin olun ve ardından **Oluştur**' u seçin.

    ![Sanal cihaz](./media/quick-create-pnp-device/simulated-device.png)

Artık, benzetimli verileri kullanarak cihaz şablonu için Oluşturucu tarafından oluşturulan görünümlerle etkileşime geçebilirsiniz:

1. **Cihazlar** sayfasında sanal cihazınızı seçin.

1. **Genel bakış** görünümünde, sanal Telemetriyi gösteren bir çizim gösterilmektedir:

    ![Genel bakış görünümü](./media/quick-create-pnp-device/simulated-telemetry.png)

1. **Hakkında** görünümü, görünüme eklediğiniz bulut özellikleri de dahil olmak üzere özellik değerlerini gösterir.

1. **Komutlar** görünümü, cihazda **yanıp sönme** gibi komutları çalıştırmanıza olanak sağlar.

1. **Cihazları yönet** görünümü, cihazı yönetmek için işleci için oluşturduğunuz formdur.

## <a name="use-a-simulated-device-to-improve-views"></a>Görünümleri geliştirmek için sanal cihaz kullanma

Yeni bir sanal cihaz oluşturduktan sonra, Oluşturucu bu cihazı kullanarak cihaz şablonunun görünümlerini iyileştirmeye ve oluşturmaya devam edebilir.

1. Sol bölmedeki **cihaz şablonları** ' nı seçin ve **Mxyonga IoT devkit** şablonunu seçin.

1. Düzenlemek istediğiniz görünümlerden birini seçin veya yeni bir görünüm oluşturun. **Önizleme cihazını Yapılandır**' ı seçin ve ardından **çalışan bir cihazdan öğesini seçin**. Burada, bir önizleme cihazı, test için yapılandırılmış gerçek bir cihaz ya da IoT Central eklemiş olduğunuz mevcut bir cihaz yok seçeneklerinden birini belirleyebilirsiniz.

1. Listeden sanal cihazınızı seçin. Ardından **Uygula**' yı seçin. Artık cihaz şablonu görünümlerini oluşturma deneyiminizdeki aynı sanal cihazı görebilirsiniz. Bu görünüm, grafikler ve diğer görselleştirmeler için kullanışlıdır.

    ![Önizleme cihazını Yapılandır](./media/quick-create-pnp-device/configure-preview.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir **Mxyonga IoT DevKit** cihaz şablonu oluşturmayı ve uygulamanıza sanal bir cihaz eklemeyi öğrendiniz.

Uygulamanıza bağlı olan cihazları izleme hakkında daha fazla bilgi edinmek için hızlı başlangıç ile devam edin:

> [!div class="nextstepaction"]
> [Kuralları ve eylemleri yapılandırma](./quick-configure-rules.md)
