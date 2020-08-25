---
title: Öğretici-Azure IoT Central uygulamanızda kurallar oluşturma ve yönetme
description: Bu öğreticide, Azure IoT Central kurallarının cihazınızı neredeyse gerçek zamanlı olarak izlemenize ve kural tetiklendiğinde e-posta gönderme gibi eylemleri otomatik olarak nasıl çağıracağına ilişkin yönergeler gösterilmektedir.
author: dominicbetts
ms.author: dobett
ms.date: 04/06/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 555da74da65f3b1897a276cf819a263334cfa053
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "80999055"
---
# <a name="tutorial-create-a-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Öğretici: Azure IoT Central uygulamanızda bir kural oluşturma ve bildirim ayarlama

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bağlı cihazlarınızı uzaktan izlemek için Azure IoT Central kullanabilirsiniz. Azure IoT Central kuralları cihazlarınızın neredeyse gerçek zamanlı olarak izlenmesini sağlar ve e-posta gönderme gibi eylemleri otomatik olarak çağırır. Bu makalede, cihazlarınızın gönderdikleri Telemetriyi izlemek için kuralların nasıl oluşturulacağı açıklanmaktadır.

Cihazlar, cihazdan sayısal veri göndermek için telemetri kullanır. Seçilen cihaz telemetrisi belirtilen eşiği aştığında bir kural tetiklenir.

Bu öğreticide, sanal bir ortam algılayıcısı cihazındaki sıcaklık 70 F 'yi aştığında bir e-posta göndermek için bir kural oluşturacaksınız &deg; .

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
>
> * Kural oluşturma
> * E-posta eylemi ekleme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce [Azure IoT Central oluşturma uygulamasını](./quick-deploy-iot-central.md) tamamlayıp [IoT Central uygulamanıza sanal bir cihaz ekleyerek](./quick-create-simulated-device.md) , birlikte çalışmak üzere **mxyonga IoT devkit** cihaz şablonunu oluşturun.

## <a name="create-a-rule"></a>Kural oluşturma

Bir telemetri kuralı oluşturmak için, cihaz şablonu en az bir telemetri değeri içermelidir. Bu öğreticide, sıcaklık ve nem telemetrisi gönderen bir sanal **Mxyonga IoT DevKit** cihazı kullanılmaktadır. Bu cihaz şablonunu eklediniz ve sanal cihaz oluşturma [IoT Central uygulama](./quick-create-simulated-device.md) hızlı başlangıç bölümünde sanal cihaz oluşturdunuz. Kural, cihaz tarafından bildirilen sıcaklığın izler ve 70 derecenin üzerinde kaldığında bir e-posta gönderir.

1. Sol bölmede **kurallar**' ı seçin.

1. Henüz bir kural oluşturmadıysanız aşağıdaki ekranı görürsünüz:

    ![Henüz kural yok](media/tutorial-create-telemetry-rules/rules-landing-page1.png)

1. **+** Yeni bir kural eklemek için seçin.

1. Kuralı tanımlamak için _sıcaklık izleyicisini_ girin ve ENTER 'a basın.

1. **Mxyongaıot DevKit** cihaz şablonunu seçin. Varsayılan olarak, kural cihaz şablonuyla ilişkili tüm cihazlara otomatik olarak uygulanır. Cihazların bir alt kümesini filtrelemek için **+ filtre** ' yi seçin ve cihazları tanımlamak için cihaz özellikleri ' ni kullanın. Kuralı devre dışı bırakmak için, kural üstbilgisindeki **etkin/devre dışı** düğmesini değiştirin:

    ![Filtreler ve Etkinleştir](media/tutorial-create-telemetry-rules/device-filters.png)

### <a name="configure-the-rule-conditions"></a>Kural koşullarını yapılandırma

Koşullar kuralın izlediği kriterleri tanımlar. Bu öğreticide, sıcaklığın 70 F 'yi aşması durumunda tetiklemesi kuralını yapılandırırsınız &deg; .

1. **Telemetri** açılan menüsünde **sıcaklık** ' ı seçin.

1. Sonra, **işleç** **olarak büyüktür ' i seçin ve** **değer**olarak _70_ girin.

    ![Koşul](media/tutorial-create-telemetry-rules/condition-filled-out1.png)

1. İsteğe bağlı olarak, bir **zaman toplama**ayarlayabilirsiniz. Bir zaman toplamayı seçtiğinizde, toplama açılan listesinden ortalama veya toplam gibi bir toplama türü de seçmeniz gerekir.

    * Toplama olmadan kural, koşulu karşılayan her telemetri veri noktası için tetikler. Örneğin, sıcaklık 70 ' den fazla olduğunda tetiklenecek kuralı yapılandırırsanız, cihaz sıcaklığı bu değeri aştığında kural neredeyse anında tetiklenir.
    * Toplama ile kural, zaman penceresinde telemetri veri noktalarının toplam değeri koşulu karşılıyorsa tetiklenir. Örneğin, sıcaklık 70 ' den fazla olduğunda ve 10 dakikalık ortalama bir süre toplanmasına sahip olan kuralı tetiklemek üzere yapılandırırsanız kural, cihaz, 10 dakikalık bir aralıkta hesaplanan, 70 ' den büyük bir ortalama sıcaklık raporladığında tetiklenir.

     ![Toplama koşulu](media/tutorial-create-telemetry-rules/aggregate-condition-filled-out1.png)

**+ Koşul**' i seçerek bir kurala birden çok koşul ekleyebilirsiniz. Birden çok koşul belirtildiğinde kuralın tetiklenmesi için tüm koşulların karşılanması gerekir. Her koşul örtük bir `AND` yan tümce ile birleştirilir. Birden çok koşuldan oluşan zaman toplamayı kullanıyorsanız, tüm telemetri değerlerinin toplanması gerekir.

### <a name="configure-actions"></a>Eylemleri yapılandırma

Koşulu tanımladıktan sonra, kural tetiklendiğinde gerçekleştirilecek eylemleri ayarlarsınız. Kuralda belirtilen tüm koşulların doğru olarak değerlendirilmesi durumunda eylemler çağrılır.

1. **Eylemler** bölümünde **+ e-posta** ' yı seçin.

1. Eylem _Temperature warning_ için görünen ad, **to** alanındaki e-posta adresiniz ve _cihazı denetlemeniz gerekir!_ e-postanın gövdesinde görünecek bir notun olması.

    > [!NOTE]
    > E-postalar yalnızca uygulamaya eklenmiş olan ve en az bir kez oturum açan kullanıcılara gönderilir. Azure IoT Central 'de [Kullanıcı yönetimi](howto-administer.md) hakkında daha fazla bilgi edinin.

   ![Eylemi Yapılandır](media/tutorial-create-telemetry-rules/configure-action1.png)

1. Eylemi kaydetmek için **bitti**' yi seçin. Bir kurala birden çok eylem ekleyebilirsiniz.

1. Kuralı kaydetmek için **Kaydet**' i seçin. Kural birkaç dakika içinde canlı olarak geçer ve uygulamanıza gönderilen izleme Telemetriyi başlatır. Kuralda belirtilen koşul karşılandığında, kural yapılandırılan e-posta eylemini tetikler.

Bir süre sonra, kural tetiklendiğinde bir e-posta iletisi alırsınız:

![Örnek e-posta](media/tutorial-create-telemetry-rules/email.png)

## <a name="delete-a-rule"></a>Kuralı silme

Artık bir kurala ihtiyacınız yoksa, kuralı açıp **Sil**' i seçerek silin.

## <a name="enable-or-disable-a-rule"></a>Kuralı etkinleştirme veya devre dışı bırakma

Etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. Kuralda kapsamı belirlenmiş tüm cihazların kuralını etkinleştirmek veya devre dışı bırakmak için kuraldaki **etkin/devre dışı** düğmesini değiştirin.

## <a name="enable-or-disable-a-rule-for-specific-devices"></a>Belirli cihazlar için bir kuralı etkinleştirme veya devre dışı bırakma

Özelleştirmek istediğiniz kuralı seçin. Kuralın kapsamını izlemek istediğiniz cihazlara daraltmak için **hedef cihazlar** bölümünde bir veya daha fazla filtre kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Telemetri tabanlı kural oluşturma
* Eylem ekleme

Eşik tabanlı bir kural tanımladığınıza göre, önerilen sonraki adım şunları yapmayı öğrenmektedir:

> [!div class="nextstepaction"]
> [Sürekli veri vermeyi yapılandırma](./howto-export-data.md).
