---
title: Quickstart - Azure IoT Central'a benzetilen bir aygıt ekleme
description: Bu hızlı başlatma, bir aygıt şablonu oluşturmanın ve IoT Merkezi uygulamanıza nasıl benzetimli bir aygıt ekleyeceğinigösterir.
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 489bf81388c1bb889756d54957e791282054f0f7
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77169550"
---
# <a name="quickstart-add-a-simulated-device-to-your-iot-central-application"></a>Hızlı başlatma: IoT Merkezi uygulamanıza simüle edilmiş bir aygıt ekleme

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Aygıt şablonu, IoT Merkezi uygulamanıza bağlanan bir aygıtın özelliklerini tanımlar. Özellikler, aygıtın gönderdiği telemetriyi, aygıt özelliklerini ve aygıtın yanıt olarak yanıtlatmasını içerir. Aygıt şablonundan, oluşturucu veya operatör bir uygulamaya hem gerçek hem de simüle edilmiş aygıtlar ekleyebilir. Simüle edilmiş aygıtlar, gerçek aygıtları bağlamadan önce IoT Merkezi uygulamanızın davranışını sınarmak için yararlıdır.

Bu hızlı başlatmada, *MXChip IoT DevKit* (DevKit) kartı için bir aygıt şablonu ekler ve simüle edilmiş bir aygıt oluşturursunuz. Bu hızlı başlatmayı tamamlamak için gerçek bir cihaza ihtiyacınız yoktur, aygıtın simülasyonu yla çalışırsınız. DevKit cihazı:

* Sıcaklık gibi telemetri gönderir.
* Parlaklık düzeyi gibi aygıta özgü özellikleri raporlar.
* Açma ve kapatma gibi komutlara yanıt verir.
* Firmware sürümü ve seri numarası gibi genel aygıt özelliklerini raporlar.

## <a name="prerequisites"></a>Ön koşullar

**Özel uygulama > Özel uygulama** şablonu kullanarak bir IoT Central uygulaması oluşturmak için Azure [IoT Central uygulamasını](./quick-deploy-iot-central.md) hızlı bir şekilde oluşturun'ı tamamlayın.

## <a name="create-a-template"></a>Şablon oluşturma

Oluşturucu olarak, IoT Central uygulamanızda aygıt şablonları oluşturabilir ve edinebilirsiniz. Aygıt şablonu yayımladıktan sonra, simüle edilmiş aygıt oluşturabilir veya aygıt şablonundan gerçek aygıtlar bağlayabilirsiniz. Benzetimli aygıtlar, gerçek bir aygıtı bağlamadan önce uygulamanızın davranışını sınamamanızı sağlar.

Uygulamanıza yeni bir aygıt şablonu eklemek için sol bölmedeki **Aygıt Şablonları** sekmesini seçin.

![Aygıt Şablonları sayfası](./media/quick-create-pnp-device/devicedefinitions.png)

Aygıt şablonu, aygıtın gönderdiği telemetriyi, aygıt özelliklerini ve aygıtın yanıt verdiği komutları tanımlayan bir aygıt yeteneği modeli içerir.

### <a name="add-a-device-capability-model"></a>Aygıt özelliği modeli ekleme

IoT Central uygulamanıza aygıt yetenek modeli eklemek için çeşitli seçenekler vardır. Sıfırdan bir model oluşturabilir, bir dosyadan bir model içe aktarabilir veya aygıt kataloğundan bir aygıt seçebilirsiniz. IoT Central, bir aygıt ilk kez bağlandığında bir modeli depodan otomatik olarak içe aktarmayaptığı *aygıta yönelik ilk* yaklaşımı da destekler. Bu hızlı başlatmada, aygıt yetenek modelini almak için aygıt kataloğundan bir aygıt seçersiniz.

Aşağıdaki adımlar, bir **MXChip IoT DevKit** aygıtının yetenek modelini almak için aygıt kataloğunu nasıl kullanacağınızı gösterir. Bu aygıtlar uygulamanıza sıcaklık gibi telemetri gönderir:

1. Yeni bir aygıt şablonu **+** eklemek için **Aygıt şablonları** sayfasından seçim yapmayı n için seçin.

1. Şablon **türünü seç** sayfasında, **MXChip IoT DevKit** döşemesini bulana kadar aşağı kaydırın.

1. **MXChip IoT DevKit** döşemesini seçin ve ardından **İleri: Özelleştir'i**seçin.

1. Gözden **Geçir** sayfasında **Oluştur'u**seçin.

1. Birkaç saniye sonra yeni cihaz şablonunuzu görebilirsiniz:

    ![MXChip IoT DevKit cihaz şablonu](./media/quick-create-pnp-device/devkit-template.png)

    MXChip IoT DevKit yetenek modeli **mxchip_sensor,** **mxchip_settings**ve **Aygıt Bilgileri**gibi arayüzleri içerir. Arayüzler, bir MXChip IoT DevKit aygıtının özelliklerini tanımlar. Özellikler arasında aygıtın gönderdiği telemetri, aygıtın bildirdiği özellikler ve aygıtın yanıt vermesi yer alıyor.

### <a name="add-cloud-properties"></a>Bulut özellikleri ekleme

Aygıt şablonu bulut özelliklerini içerebilir. Bulut özellikleri yalnızca IoT Merkezi uygulamasında bulunur ve hiçbir zaman bir aygıta gönderilmez veya bir aygıttan alınmaz.

1. **Bulut Özellikleri'ni** seçin ve ardından **+ Bulut özelliği ekle.** Aygıt şablonunuza iki bulut özelliği eklemek için aşağıdaki tablodaki bilgileri kullanın:

    | Görünen Ad      | Anlamsal Tip | Şema |
    | ----------------- | ------------- | ------ |
    | Son Hizmet Tarihi | None          | Tarih   |
    | Müşteri Adı     | None          | Dize |

1. Değişikliklerinizi kaydetmek için **Kaydet'i** seçin:

    ![Bulut özellikleri](media/quick-create-pnp-device/cloudproperties.png)

## <a name="views"></a>Görünümler

Oluşturucu olarak, aygıtla ilgili bilgileri bir operatöre görüntülemek için uygulamayı özelleştirebilirsiniz. Özelleştirmeleriniz, operatörün uygulamaya bağlı aygıtları yönetmesini sağlar. Aygıtlarla etkileşimde kalmak için bir işleç için iki tür görünüm oluşturabilirsiniz:

* Aygıt ve bulut özelliklerini görüntülemek ve görüntülemek için formlar.
* Gönderdikleri telemetri de dahil olmak üzere cihazları görselleştirmek için panolar.

### <a name="default-views"></a>Varsayılan görünümler

Varsayılan görünümler, önemli aygıt bilgilerinizi görselleştirmeye başlamanın hızlı bir yoludur. Aygıt şablonunuzun en fazla üç varsayılan görünümü oluşturulabilir:

* **Komutlar** görünümü, operatörünüzün cihazınıza komutlar göndermesini sağlar.
* **Genel Bakış** görünümü, aygıt telemetrisini görüntülemek için grafikleri ve ölçümleri kullanır.
* **About** görünümü aygıt özelliklerini görüntüler.

Aygıt şablonundaki **Görünümler** düğümünü seçin. Şablonu eklediğinizde IoT Central'ın sizin için bir **Genel Bakış** ve **Bir About** görünümü oluşturduğunu görebilirsiniz.

Bir operatörün aygıtı yönetmek için kullanabileceği yeni bir **Yönet aygıtı** formu eklemek için:

1. **Görünümler** düğümünü seçin ve ardından yeni bir görünüm eklemek için **Düzenleme aygıtını ve bulut veri** döşemesini seçin.

1. Aygıtı Yönet olarak form adını **değiştirin.**

1. Müşteri **Adı** ve **Son Hizmet Tarihi** bulut özelliklerini ve Fan **Speed** özelliğini seçin. Sonra **Ekle bölümünü**seçin:

    ![Yeni form oluşturma](media/quick-create-pnp-device/new-form.png)

1. Yeni formunuzu kaydetmek için **Kaydet'i** seçin.

## <a name="publish-device-template"></a>Aygıt şablonu yayımlama

Benzetimli bir aygıt oluşturmadan veya gerçek bir aygıta bağlanmadan önce aygıt şablonunuzu yayımlamanız gerekir. IoT Central şablonu ilk oluşturduğunuzda yayımlamasına rağmen, güncelleştirilmiş sürümü yayımlamanız gerekir.

Aygıt şablonu yayımlamak için:

1. **Aygıt Şablonları** sayfasından aygıt şablonunuza gidin.

1. **Yayınla'yı**Seçin :

    ![Yayınlanan model](media/quick-create-pnp-device/publishedmodel.png)

1. Bu **aygıt şablonunu uygulama** iletişim kutusuna yayımla'da **Yayımla'yı**seçin. 

Aygıt şablonu yayımladıktan sonra, **aygıtlar** sayfasında görünür. Yayımlanmış bir aygıt şablonunda, yeni bir sürüm oluşturmadan aygıt yetenek modelini düzenleme yapamazsınız. Ancak, sürüm oluşturmadan yayımlanmış bir aygıt şablonunda bulut özellikleri, özelleştirmeler ve görünümler güncelleştirmeleri yapabilirsiniz. Herhangi bir değişiklik **Publish** yaptıktan sonra, bu değişiklikleri operatörünüze iletmek için Yayımla'yı seçin.

## <a name="add-a-simulated-device"></a>Sanal cihaz ekleme

Uygulamanıza benzetilen bir aygıt eklemek için oluşturduğunuz **MXChip IoT DevKit** aygıt şablonunu kullanırsınız.

1. Operatör olarak yeni bir aygıt eklemek için sol bölmedeki **Aygıtları** seçin. **Cihazlar** sekmesi **tüm aygıtları** ve **MXChip IoT DevKit** aygıt şablonu gösterir. **MXChip IoT DevKit'i**seçin.

1. Benzetimli DevKit aygıtı eklemek **+** için . Önerilen **Aygıt Kimliğini** kullanın veya kendi küçük **Aygıt Kimliğinizi**girin. Ayrıca yeni cihazınız için bir ad da girebilirsiniz. **Benzetimli** geçişin **in de** olduğundan emin olun ve ardından **Oluştur'u**seçin.

    ![Sanal cihaz](./media/quick-create-pnp-device/simulated-device.png)

Artık benzetimli verileri kullanarak aygıt şablonu için oluşturucu tarafından oluşturulan görünümlerle etkileşim kurabilirsiniz:

1. **Cihazlar** sayfasında simüle edilmiş cihazınızı seçin.

1. **Genel Bakış** görünümü, benzetimli telemetrinin bir çizimini gösterir:

    ![Genel bakış görünümü](./media/quick-create-pnp-device/simulated-telemetry.png)

1. **About** görünümü, görünüme eklediğiniz bulut özellikleri de dahil olmak üzere özellik değerlerini gösterir.

1. **Komutlar** görünümü, aygıtta **yanıp sönme** gibi komutları çalıştırmanızı sağlar.

1. **Aygıtları Yönet** görünümü, operatörün aygıtı yönetmesi için oluşturduğunuz formdur.

## <a name="use-a-simulated-device-to-improve-views"></a>Görünümleri geliştirmek için benzetimli bir aygıt kullanma

Yeni bir benzetimli aygıt oluşturduktan sonra, oluşturucu bu aygıtı aygıt şablonu için görünümleri geliştirmeye ve geliştirmeye devam etmek için kullanabilir.

1. Sol bölmedeki **Aygıt şablonlarını** seçin ve **MXChip IoT DevKit** şablonunu seçin.

1. Görüntülemek istediğiniz görünümlerden herhangi birini seçin veya yeni bir görünüm oluşturun. **Önizleme aygıtını Yapılandır'ı**seçin, ardından **çalışan bir aygıttan seçin.** Burada önizleme aygıtı, test için yapılandırılan gerçek bir aygıt veya IoT Central'a eklediğiniz varolan bir aygıtı seçebilirsiniz.

1. Listede simüle edilmiş cihazınızı seçin. Ardından **Uygula**'yı seçin. Artık aynı simüle edilmiş aygıtı cihazınızşablongörünümlerinde bina deneyimi görebilirsiniz. Bu görünüm grafikler ve diğer görseller için yararlıdır.

    ![Önizleme aygıtını yapılandırma](./media/quick-create-pnp-device/configure-preview.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir **MXChip IoT DevKit** cihaz şablonu oluşturmayı ve uygulamanıza simüle edilmiş bir aygıt eklemeyi öğrendiniz.

Uygulamanıza bağlı izleme aygıtları hakkında daha fazla bilgi edinmek için hızlı bir şekilde çalışmaya devam edin:

> [!div class="nextstepaction"]
> [Kuralları ve eylemleri yapılandırma](./quick-configure-rules.md)
