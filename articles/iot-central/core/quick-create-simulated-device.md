---
title: Hızlı başlangıç-Azure IoT Central sanal cihaz ekleme
description: Bu hızlı başlangıçta bir cihaz şablonu oluşturma ve IoT Central uygulamanıza sanal cihaz ekleme işlemleri gösterilmektedir.
author: dominicbetts
ms.author: dobett
ms.date: 11/16/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 488de7114d80e6a88cc619ba3b42f867c985ea11
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99833924"
---
# <a name="quickstart-add-a-simulated-device-to-your-iot-central-application"></a>Hızlı başlangıç: IoT Central uygulamanıza sanal cihaz ekleme

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bir cihaz şablonu, IoT Central uygulamanıza bağlanan bir cihazın yeteneklerini tanımlar. Özellikler cihaz gönderme, cihaz özellikleri ve bir cihazın yanıt verdiği komutları içerir. Bir cihaz şablonundan, bir Oluşturucu veya operatör bir uygulamaya hem gerçek hem de sanal cihaz ekleyebilir. Sanal cihazlar, gerçek cihazları bağlanmadan önce IoT Central uygulamanızın davranışını test etmek için kullanışlıdır.

Bu hızlı başlangıçta, bir ESP32-Azure IoT kit geliştirme panosu için bir cihaz şablonu ekler ve sanal bir cihaz oluşturursunuz. Bu hızlı başlangıcı tamamlayabilmeniz için gerçek bir cihaza ihtiyacınız yoktur, cihazın simülasyonu ile çalışırsınız. Bir ESP32 cihazı:

* Sıcaklık gibi telemetri gönderir.
* Cihazın yeniden başlatılmasından bu yana maksimum sıcaklık gibi cihaza özgü özellikleri raporlar.
* Yeniden başlatma gibi komutlara yanıt verir.
* Bellenim sürümü ve seri numarası gibi genel cihaz özelliklerini raporlar.

## <a name="prerequisites"></a>Önkoşullar

Özel uygulama **> özel uygulama** şablonunu kullanarak bir IoT Central uygulaması oluşturmak için [Azure IoT Central uygulaması oluşturma](./quick-deploy-iot-central.md) hızlı başlangıcı ' nı doldurun.

## <a name="create-a-device-template"></a>Cihaz şablonu oluşturma

Bir Oluşturucu olarak, IoT Central uygulamanızda cihaz şablonları oluşturabilir ve düzenleyebilirsiniz. Bir cihaz şablonu yayımladıktan sonra, cihaz şablonundan sanal cihaz oluşturabilir veya gerçek cihazları bağlayabilirsiniz. Sanal cihazlar, gerçek bir cihazı bağlanmadan önce uygulamanızın davranışını test etmenize olanak sağlar.

Uygulamanıza yeni bir cihaz şablonu eklemek için sol bölmedeki **cihaz şablonları** sekmesini seçin.

:::image type="content" source="media/quick-create-simulated-device/device-definitions.png" alt-text="Cihaz şablonlarının boş listesini gösteren ekran görüntüsü":::

Bir cihaz şablonu şunları tanımlayan bir cihaz modeli içerir:

* Cihazın gönderdiği telemetri.
* Cihaz özellikleri.
* Cihazın yanıt verdiği komutlar.

### <a name="add-a-device-template"></a>Cihaz şablonu ekleme

IoT Central uygulamanıza bir cihaz modeli eklemek için çeşitli seçenekler vardır. Sıfırdan bir model oluşturabilir, bir dosyadan model aktarabilir veya cihaz kataloğundan bir cihaz seçebilirsiniz. IoT Central Ayrıca, gerçek bir cihaz ilk kez bağlandığında uygulamanın bir depodan bir modeli otomatik olarak içeri aktardığı bir *Cihaz-ilk* yaklaşımı destekler.

Bu hızlı başlangıçta bir cihaz şablonu oluşturmak için cihaz kataloğundan bir cihaz seçersiniz.

Aşağıdaki adımlarda, bir **ESP32** cihazına ait modeli içeri aktarmak için cihaz kataloğunun nasıl kullanılacağı gösterilmektedir. Bu cihazlar, uygulamanıza sıcaklık gibi telemetri gönderir:

1. Yeni bir cihaz şablonu eklemek için **cihaz şablonları** sayfasında **+ Yeni** ' yi seçin.

1. **Tür Seç** sayfasında, **önceden yapılandırılmış bir cihaz şablonu kullan** bölümünde **ESP32-Azure IoT Kit** kutucuğunu bulana kadar aşağı kaydırın.

1. **ESP32-Azure IoT Kit** kutucuğunu seçin ve ardından **Ileri: gözden geçir**' i seçin.

1. **İnceleme** sayfasında **Oluştur**' u seçin.

1. Birkaç saniye sonra, yeni cihaz şablonunuzu görebilirsiniz:

    :::image type="content" source="media/quick-create-simulated-device/devkit-template.png" alt-text="ESP32 cihazının cihaz şablonunu gösteren ekran görüntüsü":::

    Şablonun adı **algılayıcı denetleyicisidir**. Model, **algılayıcı denetleyicisi**, **Sensortemp** ve **cihaz bilgileri arabirimi** gibi bileşenleri içerir. Bileşenler bir ESP32 cihazının yeteneklerini tanımlar. Özellikler telemetri, Özellikler ve komutları içerir.

### <a name="add-cloud-properties"></a>Bulut özelliklerini ekleme

Bir cihaz şablonu, bulut özellikleri içerebilir. Bulut özellikleri yalnızca IoT Central uygulamasında bulunur ve bir cihaza hiçbir şekilde gönderilmez veya buradan alınmaz. İki bulut özelliği eklemek için:

1. **Bulut özellikleri** ' ni seçin ve **+ bulut özelliği Ekle**' ye tıklayın. Cihaz şablonunuza iki bulut özelliği eklemek için aşağıdaki tablodaki bilgileri kullanın:

    | Görünen Ad      | Anlamsal tür | Şema |
    | ----------------- | ------------- | ------ |
    | Son Hizmet Tarihi | Yok          | Tarih   |
    | Müşteri Adı     | Yok          | Dize |

1. Değişikliklerinizi kaydetmek için **Kaydet** ' i seçin:

    :::image type="content" source="media/quick-create-simulated-device/cloud-properties.png" alt-text="İki bulut özelliğini gösteren ekran görüntüsü":::

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

1. **Cihazı yönetmek** için form adını değiştirin.

1. **Müşteri adı** ve **son hizmet tarihi** bulut özelliklerini ve **hedef sıcaklık** özelliğini seçin. Ardından **Bölüm Ekle**' yi seçin:

    :::image type="content" source="media/quick-create-simulated-device/new-form.png" alt-text="Cihaz şablonuna eklenen yeni formu gösteren ekran görüntüsü":::

1. Yeni formunuzu kaydetmek için **Kaydet** ' i seçin.

## <a name="publish-device-template"></a>Cihaz şablonunu Yayımla

Sanal bir cihaz oluşturmadan veya gerçek bir cihazı bağlayabilmeniz için önce cihaz şablonunuzu yayımlamanız gerekir. IoT Central şablonu ilk oluştururken yayımlamakla birlikte, güncelleştirilmiş sürümü yayımlamanız gerekir.

Bir cihaz şablonunu yayımlamak için:

1. **Cihaz şablonları** sayfasından **algılayıcı denetleyicisi** cihaz şablonunuza gidin.

1. **Yayımla**' yı seçin:

    :::image type="content" source="media/quick-create-simulated-device/published-model.png" alt-text="Yayımla simgesinin konumunu gösteren ekran görüntüsü":::

1. **Bu cihaz şablonunu uygulama** iletişim **kutusunda Yayımla ' yı seçin.**

Bir cihaz şablonunu yayımladıktan sonra, **cihazlar** sayfasında görünür. Yayımlanmış bir cihaz şablonunda, yeni bir sürüm oluşturmadan bir cihaz modelini düzenleyemezsiniz. Ancak, yayımlanmış bir cihaz şablonundaki bulut özelliklerini, özelleştirmeleri ve görünümleri sürüm oluşturma olmadan değiştirebilirsiniz. Herhangi bir değişiklik yaptıktan sonra, bu değişiklikleri işletmenizin dışına göndermek için **Yayımla**  ' yı seçin.

## <a name="add-a-simulated-device"></a>Sanal cihaz ekleme

Uygulamanıza sanal bir cihaz eklemek için, oluşturduğunuz **ESP32** cihaz şablonunu kullanırsınız.

1. Yeni bir cihazı operatör olarak eklemek için sol bölmedeki **cihazlar** ' ı seçin. **Cihazlar** SEKMESI, ESP32 cihazının **Tüm cihazlarını** ve **algılayıcı denetleyicisi** cihaz şablonunu gösterir. **Algılayıcı denetleyicisi**' ni seçin.

1. Sanal bir DevKit cihazı eklemek için **+ Yeni**' yi seçin. Önerilen **CIHAZ kimliğini** kullanın veya kendi kodunuzu girin. Bir cihaz KIMLIĞI harfler, rakamlar ve `-` karakteri içerebilir. Ayrıca, yeni cihazınız için bir ad girebilirsiniz. **Bu cihazın benzetimini yap** ' ın **Evet** olarak ayarlandığından emin olun ve ardından **Oluştur**' u seçin.

    :::image type="content" source="media/quick-create-simulated-device/simulated-device.png" alt-text="Sanal algılayıcı denetleyicisi cihazını gösteren ekran görüntüsü":::

Artık, benzetimli verileri kullanarak cihaz şablonu için Oluşturucu tarafından oluşturulan görünümlerle etkileşime geçebilirsiniz:

1. **Cihazlar** sayfasında sanal cihazınızı seçin

    * **Genel bakış** görünümünde, sanal Telemetriyi gösteren bir çizim gösterilmektedir:

        :::image type="content" source="media/quick-create-simulated-device/simulated-telemetry.png" alt-text="Sanal cihaz için genel bakış sayfasını gösteren ekran görüntüsü":::

    * **Hakkında** görünümü özellik değerlerini gösterir.

    * **Komutlar** görünümü cihazda **yeniden başlatma** gibi komutları çalıştırmanıza olanak sağlar.

    * **Cihazları yönet** görünümü, cihazı yönetmek için işleci için oluşturduğunuz formdur.

    * **Ham veri** görünümü, cihaz tarafından gönderilen ham telemetri ve özellik değerlerini görüntülemenize olanak sağlar. Bu görünüm cihazlarda hata ayıklama için yararlıdır.

## <a name="use-a-simulated-device-to-improve-views"></a>Görünümleri geliştirmek için sanal cihaz kullanma

Yeni bir sanal cihaz oluşturduktan sonra, Oluşturucu bu cihazı kullanarak cihaz şablonunun görünümlerini iyileştirmeye ve oluşturmaya devam edebilir.

1. Sol bölmedeki **cihaz şablonları** ' nı seçin ve **algılayıcı denetleyicisi** şablonunu seçin.

1. **Genel bakış** gibi düzenlemek istediğiniz görünümlerden birini seçin veya yeni bir görünüm oluşturun. **Önizleme cihazını Yapılandır**' ı seçin ve ardından **çalışan bir cihazdan öğesini seçin**. Burada, bir önizleme cihazı, test için yapılandırılmış gerçek bir cihaz ya da IoT Central eklemiş olduğunuz mevcut bir cihaz yok seçeneklerinden birini belirleyebilirsiniz.

1. Listeden sanal cihazınızı seçin. Sonra **Uygula**'yı seçin. Artık cihaz şablonu görünümlerini oluşturma deneyiminizdeki aynı sanal cihazı görebilirsiniz. Bu görünüm, grafikler ve diğer görselleştirmeler için kullanışlıdır.

    :::image type="content" source="media/quick-create-simulated-device/configure-preview.png" alt-text="Yapılandırılmış bir önizleme cihazını gösteren ekran görüntüsü":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-central-clean-up-resources](../../../includes/iot-central-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir ESP32 cihazı için bir **algılayıcı denetleyicisi** cihaz şablonu oluşturmayı ve uygulamanıza sanal bir cihaz eklemeyi öğrendiniz.

Uygulamanıza bağlı olan cihazları izleme hakkında daha fazla bilgi edinmek için hızlı başlangıç ile devam edin:

> [!div class="nextstepaction"]
> [Kuralları ve eylemleri yapılandırma](./quick-configure-rules.md)
