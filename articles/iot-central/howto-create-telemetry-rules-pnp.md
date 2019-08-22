---
title: Azure IoT Central uygulamanızda telemetri kuralları oluşturma ve yönetme | Microsoft Docs
description: Azure IoT Central telemetri kuralları, cihazlarınızı neredeyse gerçek zamanlı olarak izlemenizi ve kural tetiklendiğinde e-posta gönderme gibi eylemleri otomatik olarak çağırmayı sağlar.
author: ankitgupta
ms.author: ankitgup
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: d6deecf558105701be591c1f08923eca2c1e3bbd
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879948"
---
# <a name="create-a-telemetry-rule-and-set-up-notifications-in-your-azure-iot-central-application-preview-features"></a>Azure IoT Central uygulamanızda bir telemetri kuralı oluşturma ve bildirim ayarlama (Önizleme özellikleri)

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bağlı cihazlarınızı uzaktan izlemek için Azure IoT Central kullanabilirsiniz. Azure IoT Central kuralları, cihazlarınızı neredeyse gerçek zamanlı olarak izlemenizi ve e-posta veya Microsoft Flow tetikleyici Gönder gibi eylemleri otomatik olarak çağırmayı sağlar. Yalnızca birkaç tıklamayla, cihaz verilerinizi izleyecek koşulu tanımlayabilir ve ilgili eylemi yapılandırabilirsiniz. Bu makalede, cihaz tarafından gönderilen Telemetriyi izlemek için kuralların nasıl oluşturulacağı açıklanmaktadır.

Cihazlar, cihazdan sayısal veri göndermek için telemetri ölçümü kullanabilir. Bir telemetri kuralı, seçilen cihaz telemetrisi belirtilen eşiği aştığında tetikler.

## <a name="create-a-telemetry-rule"></a>Telemetri kuralı oluşturma

Bir telemetri kuralı oluşturmak için, cihaz tanımında en az bir telemetri ölçümü tanımlanmış olmalıdır. Bu örnek, sıcaklık ve nem telemetrisi gönderen bir soğutma makinesi cihazı kullanır. Kural, cihaz tarafından bildirilen sıcaklığın izler ve 80 derecenin üzerinde kaldığında bir e-posta gönderir.

1. **Kurallar** sayfasına gidin.

1. Henüz bir kural oluşturmadıysanız aşağıdaki ekranı görürsünüz:

    ![Henüz kural yok](media/howto-create-telemetry-rules-pnp/rules-landing-page1.png)

1. Oluşturabileceğiniz kural türlerini görmek için **+ Yeni kural** ' ı seçin.

1. Cihaz telemetrisini izlemek için bir kural oluşturmak üzere **telemetri** ' i seçin.

    ![Kural türleri](media/howto-create-telemetry-rules-pnp/rule-types1.png)


1. Kuralı tanımlamanızı ve ENTER tuşuna basmanıza yardımcı olacak bir ad girin.

1. Kapsamlar bölümünde bu kuralın kapsamını belirlemek istediğiniz cihaz tanımını seçin. Bu ekran Ayrıca, **+ filtresi**kullanarak kuralın uygulandığı cihazları filtreleyebileceğiniz yerdir. Kural, cihaz şablonu altındaki tüm cihazlara otomatik olarak uygulanır. Kuralı devre dışı bırakmak için başlıktaki **devre dışı bırak** düğmesini seçin.

### <a name="configure-the-rule-conditions"></a>Kural koşullarını yapılandırma

Koşul, kural tarafından izlenen kriterleri tanımlar.

1. Toplamayı açık veya kapalı olarak **ayarlamak** isteyip istemediğinizi seçin.

      - Toplama isteğe bağlıdır. Toplama olmadan kural, koşulu karşılayan her telemetri veri noktası için tetikler. Örneğin, kural sıcaklık 80 üzerinde olduğunda kural tetiklenecek şekilde yapılandırıldıysa, cihaz sıcaklık > 80 ' i raporladığında kural neredeyse anında tetiklenir.
      - Ortalama, min, Max, Count gibi bir toplama işlevi seçilirse, kullanıcının koşulun değerlendirilmesi gereken bir **zaman penceresi** sağlaması gerekir. Örneğin, dönemi "5 dakika" olarak ayarlarsanız ve kuralınız 80 üzerinde ortalama sıcaklık alıyorsa, kural ortalama sıcaklık en az 5 dakika boyunca 80 ' in üzerinde olduğunda tetiklenir. Kural değerlendirmesi sıklığı **zaman penceresiyle**aynıdır, yani bu örnekte kural her 5 dakikada bir değerlendirilir.

1. **Ölçüm** açılan listesinden izlemek istediğiniz Telemetriyi seçin.

1. Sonra bir **işleç** seçin ve bir **değer**sağlayın.

     ![Koşul](media/howto-create-telemetry-rules-pnp/aggregate-condition-filled-out1.png)


>[!NOTE]
>**+ Condition**seçimi yaparak birden fazla telemetri ölçümü eklenebilir. Birden çok koşul belirtildiğinde kuralın tetiklenmesi için tüm koşulların karşılanması gerekir. Her koşul örtülü olarak bir ' ve ' yan tümcesi ile birleştirilir. Toplama kullanılırken, her ölçüm toplanmalıdır.

### <a name="configure-actions"></a>Eylemleri yapılandırma

Bu bölümde, kural tetiklendiğinde yapılacak eylemlerin nasıl ayarlanacağı gösterilmektedir. Kuralda belirtilen tüm koşulların doğru olarak değerlendirilmesi durumunda eylemler çağrılır.

1. **Eylem** bölümlerindeki **+ eylemine** tıklayın. Burada, kullanılabilir eylemlerin listesini görürsünüz.  

    ![Eylem Ekle](media/howto-create-telemetry-rules-pnp/add-action1.png)


1. **E-posta** eylemini seçin, eylem için bir görünen ad, **to** alanına geçerli bir e-posta adresi girin ve kural tetiklendiğinde e-posta gövdesinde görüntülenecek bir notun sağlayın.

    > [!NOTE]
    > E-postalar yalnızca uygulamaya eklenmiş olan ve en az bir kez oturum açan kullanıcılara gönderilir. Azure IoT Central 'de [Kullanıcı yönetimi](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hakkında daha fazla bilgi edinin.

   ![Eylemi Yapılandır](media/howto-create-telemetry-rules-pnp/configure-action1.png)


1. Eylemi kaydetmek için **bitti**' yi seçin.

1. Kuralı kaydetmek için **Kaydet**' i seçin. Kural birkaç dakika içinde canlı olarak geçer ve uygulamanıza gönderilen izleme Telemetriyi başlatır. Kuralda belirtilen koşul karşılandığında, kural yapılandırılan e-posta eylemini tetikler.

## <a name="parameterize-the-rule"></a>Kuralı Parametreleştirme

Kurallar, **cihaz özelliklerinden** belirli özellikleri parametre olarak türetebilir. Farklı cihazlarda telemetri eşiklerinin değişebileceği senaryolarda, parametreleri kullanmak faydalıdır. Kural oluştururken, 80 derece gibi mutlak bir değer sağlamak yerine, eşiği belirten, **en yüksek Ideal eşik**gibi bir cihaz özelliği seçin. Kural yürütüldüğünde, Device özelliğinde ayarlanan değer ile cihaz telemetrisiyle eşleşir.

Parametrelerin kullanılması, yönetilecek kural sayısını azaltmak için etkili bir yoldur.

Eylemler, bir parametre olarak **Device özelliği** kullanılarak da yapılandırılabilir. Bir e-posta adresi özellik olarak depolanıyorsa,, adresini tanımlarken kullanabilirsiniz.

## <a name="delete-a-rule"></a>Kuralı silme

Artık bir kurala ihtiyacınız yoksa, kuralı açıp **Sil**' i seçerek silin. Kural silindiğinde cihaz şablonundan ve ilişkili tüm cihazlardan kaldırılır.

## <a name="enable-or-disable-a-rule"></a>Kuralı etkinleştirme veya devre dışı bırakma

Etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. Kural kapsamındaki tüm cihazların kuralını etkinleştirmek veya devre dışı bırakmak için kuraldaki **Etkinleştir** veya **devre dışı bırak** düğmesini değiştirin.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Cihaz için bir kuralı etkinleştirme veya devre dışı bırakma

Etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. Belirli bir cihazı cihaz şablonuna dahil etmek veya hariç tutmak için **kapsamlar** bölümüne bir filtre ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda kuralların nasıl oluşturulduğunu öğrendiğinize göre, önerilen sonraki adım [cihazlarınızın nasıl yönetileceğini](howto-manage-devices-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) öğrenirsiniz.
