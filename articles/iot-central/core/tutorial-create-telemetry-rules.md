---
title: Öğretici-Azure IoT Central uygulamanızda kurallar oluşturma ve yönetme
description: Bu öğreticide, Azure IoT Central kurallarının cihazınızı neredeyse gerçek zamanlı olarak izlemenize ve kural tetiklendiğinde e-posta gönderme gibi eylemleri otomatik olarak nasıl çağıracağına ilişkin yönergeler gösterilmektedir.
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: f61a41fa89c7006341db928472f6b20d272bc550
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77167491"
---
# <a name="tutorial-create-a-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Öğretici: Azure IoT Central uygulamanızda bir kural oluşturma ve bildirim ayarlama

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bağlı cihazlarınızı uzaktan izlemek için Azure IoT Central kullanabilirsiniz. Azure IoT Central kuralları, cihazlarınızı neredeyse gerçek zamanlı olarak izlemenizi sağlar ve e-posta gönder gibi eylemleri otomatik olarak çağırır. Yalnızca birkaç tıklamayla, cihazlarınızdan Telemetriyi izlemek ve ilgili bir eylemi yapılandırmak için bir koşul tanımlayabilirsiniz. Bu makalede, cihaz tarafından gönderilen Telemetriyi izlemek için kuralların nasıl oluşturulacağı açıklanmaktadır.

Cihazlar, cihazdan sayısal veri göndermek için telemetri kullanır. Seçilen cihaz telemetrisi belirtilen eşiği aştığında bir kural tetiklenir.

Bu öğreticide, sanal bir ortam algılayıcısı cihazındaki sıcaklık 70&deg; F 'yi aştığında e-posta göndermek için bir kural oluşturacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Kural oluşturma
> * E-posta eylemi ekleme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, [Azure IoT Central oluşturma uygulamasını](./quick-deploy-iot-central.md) tamamlayıp [IoT Central uygulamanıza sanal bir cihaz ekleyerek](./quick-create-pnp-device.md) , birlikte çalışmak üzere **mxyonga IoT devkit** cihaz şablonunu oluşturmanız gerekir.

## <a name="create-a-rule"></a>Kural oluşturma

Bir telemetri kuralı oluşturmak için, cihaz şablonunda en az bir telemetri ölçümü tanımlanmış olmalıdır. Bu öğretici, sıcaklık ve nem telemetrisi gönderen bir ortam algılayıcı cihazı kullanır. Bu cihaz şablonunu eklediniz ve sanal cihaz oluşturma [IoT Central uygulama](./quick-create-pnp-device.md) hızlı başlangıç bölümünde sanal cihaz oluşturdunuz. Kural, cihaz tarafından bildirilen sıcaklığın izler ve 70 derecenin üzerinde kaldığında bir e-posta gönderir.

1. Sol bölmede **kurallar**' ı seçin.

1. Henüz bir kural oluşturmadıysanız aşağıdaki ekranı görürsünüz:

    ![Henüz kural yok](media/tutorial-create-telemetry-rules/rules-landing-page1.png)

1. Yeni bir kural eklemek için **+** seçin.

1. Kuralı tanımlamak için _sıcaklık izleyicisini_ girin ve ENTER 'a basın.

1. **Mxyongaıot DevKit** cihaz şablonunu seçin. Varsayılan olarak, kural cihaz şablonuyla ilişkili tüm cihazlara otomatik olarak uygulanır. Cihazların bir alt kümesini filtrelemek için **+ filtre** ' yi seçin ve cihazları tanımlamak için cihaz özellikleri ' ni kullanın. Kuralı devre dışı bırakmak için, kural üstbilgisindeki **etkin/devre dışı** düğmesini değiştirin:

    ![Filtreler ve Etkinleştir](media/tutorial-create-telemetry-rules/device-filters.png)

### <a name="configure-the-rule-conditions"></a>Kural koşullarını yapılandırma

Koşullar kuralın izlediği kriterleri tanımlar. Bu öğreticide, sıcaklık 70&deg; F 'yi aştığında kullanılacak kuralı yapılandırırsınız.

1. **Telemetri** açılan menüsünde **sıcaklık** ' ı seçin.

1. Sonra, **işleç** **olarak büyüktür ' i seçin ve** **değer**olarak _70_ girin.

    ![Koşul](media/tutorial-create-telemetry-rules/condition-filled-out1.png)

1. İsteğe bağlı olarak, bir **zaman toplama**ayarlayabilirsiniz. Bir zaman toplamayı seçtiğinizde, toplama açılan listesinden ortalama veya toplam gibi bir toplama türü de seçmeniz gerekir.

    * Toplama olmadan kural, koşulu karşılayan her telemetri veri noktası için tetikler. Örneğin, kural sıcaklık 70 üzerinde olduğunda kural tetiklenecek şekilde yapılandırıldıysa, cihaz sıcaklık > 70 ' i raporladığında kural neredeyse anında tetiklenir.
    * Toplama ile kural, zaman penceresinde telemetri veri noktalarının toplam değeri koşulu karşılıyorsa tetiklenir. Örneğin, kural, sıcaklık 70 üzerinde olduğunda, zaman toplama 10 dakikaya ayarlandığında ve toplama türü ortalama olduğunda kural, cihaz bir ortalama sıcaklık > 70, 10 dakikalık bir süre içinde hesaplandığında tetiklenir aralığında.

     ![Toplama koşulu](media/tutorial-create-telemetry-rules/aggregate-condition-filled-out1.png)

**+ Koşul**' i seçerek bir kurala birden çok koşul ekleyebilirsiniz. Birden çok koşul belirtildiğinde kuralın tetiklenmesi için tüm koşulların karşılanması gerekir. Her koşul örtük bir `AND` yan tümcesiyle birleştirilir. Birden çok koşuldan oluşan zaman toplamayı kullanıyorsanız, tüm telemetri değerlerinin toplanması gerekir.

### <a name="configure-actions"></a>Eylemleri yapılandırma

Koşulu tanımladıktan sonra, kural tetiklendiğinde gerçekleştirilecek eylemleri ayarlarsınız. Kuralda belirtilen tüm koşulların doğru olarak değerlendirilmesi durumunda eylemler çağrılır.

1. **Eylemler** bölümünde **+ e-posta** ' yı seçin.

1. Eylem için görünen ad, **to** alanındaki e-posta adresiniz ve _cihazı denetlemeniz gerekir!_ e-postanın gövdesinde görünecek bir notun olması.

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

Etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. Kural kapsamındaki tüm cihazların kuralını etkinleştirmek veya devre dışı bırakmak için kuraldaki **Etkinleştir** veya **devre dışı bırak** düğmesini değiştirin.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Cihaz için bir kuralı etkinleştirme veya devre dışı bırakma

Etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. Belirli bir cihazı cihaz şablonuna dahil etmek veya hariç tutmak için **kapsamlar** bölümüne bir filtre ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Telemetri tabanlı kural oluşturma
* Eylem ekleme

Eşik tabanlı bir kural tanımladığınıza göre, önerilen sonraki adım şunları yapmayı öğrenmektedir:

> [!div class="nextstepaction"]
> [Sürekli veri vermeyi yapılandırma](./howto-export-data.md).
