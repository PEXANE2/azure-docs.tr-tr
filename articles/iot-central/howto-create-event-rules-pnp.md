---
title: Azure IoT Central uygulamanızda olay kuralları oluşturma ve yönetme | Microsoft Docs
description: Azure IoT Central olay kuralları, cihazlarınızı neredeyse gerçek zamanlı olarak izlemenizi ve kural tetiklendiğinde e-posta gönderme gibi eylemleri otomatik olarak çağırmayı sağlar.
author: dominicbetts
ms.author: dobett
ms.date: 07/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 1a7fc2b38e8354fb29255bb7f9d6b2c03ed53725
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879961"
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application-preview-features"></a>Azure IoT Central uygulamanızda bir olay kuralı oluşturma ve bildirim ayarlama (Önizleme özellikleri)

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bağlı cihazlarınızı uzaktan izlemek için Azure IoT Central kullanabilirsiniz. Azure IoT Central kuralları, cihazlarınızı neredeyse gerçek zamanlı olarak izlemenizi sağlar ve e-posta gönder gibi eylemleri otomatik olarak çağırır. Yalnızca birkaç tıklamayla, cihaz verilerinizi izleyecek koşulu tanımlayabilir ve ilgili eylemi yapılandırabilirsiniz. Bu makalede, cihaz tarafından gönderilen olayları izlemek için kuralların nasıl oluşturulacağı açıklanmaktadır.

Cihazlar, önemli veya bilgilendirici cihaz olayları göndermek için olay ölçümünü kullanabilir. Bir olay kuralı, seçilen cihaz olayı cihaz tarafından bildirildiğinde tetiklenir.

## <a name="create-an-event-rule"></a>Olay kuralı oluşturma

Bir olay kuralı oluşturmak için, cihaz şablonunda en az bir olay ölçümü tanımlanmış olmalıdır. Bu örnek, bir fan motoru hata olayını raporlayan soğutma yapan bir havalandırma makinesi cihazını kullanır. Kural, cihaz tarafından bildirilen olayı izler ve olay bildirildiğinde bir e-posta gönderir.

1. **Kurallar** sayfasına gidin.

1. Henüz bir kural oluşturmadıysanız aşağıdaki ekranı görürsünüz:

   ![Henüz kural yok](media/howto-create-event-rules-pnp/rules-landing-page1.png)

1. Oluşturabileceğiniz kural türlerini görmek için **+ Yeni kural** ' ı seçin.

1. Olay izleme kuralı oluşturmak için **olay** ' ı seçin.

   ![Kural türleri](media/howto-create-event-rules-pnp/rule-types1.png)

1. Kuralı tanımlamanızı ve ENTER tuşuna basmanıza yardımcı olacak bir ad girin.

1. Bu kuralın kapsamını **kapsamlar** bölümünde olacak cihaz tanımını seçin. Bu ekran Ayrıca, **+ filtresi**kullanarak kuralın uygulandığı cihazları filtreleyebileceğiniz yerdir. Kural, cihaz şablonu altındaki tüm cihazlara otomatik olarak uygulanır. Kuralı devre dışı bırakmak için başlıktaki **devre dışı bırak** düğmesini seçin.

### <a name="configure-the-rule-conditions"></a>Kural koşullarını yapılandırma

Koşul, kural tarafından izlenen kriterleri tanımlar.

1. Toplamayı açık veya kapalı olarak **ayarlamak** isteyip istemediğinizi seçin.

   - Toplama olmadan kural, koşulu karşılayan her bir olay veri noktası için tetikler. Örneğin, bir **fan motoru hata** olayı gerçekleştiğinde kuralın koşulunu tetiklenecek şekilde yapılandırırsanız, cihaz bu olayı raporladığında kural neredeyse anında tetiklenir.
   - **Sayım** bir toplama işlevi olarak kullanılırsa, koşulun değerlendirilmesi gereken bir **değer** ve **zaman penceresi** sağlamanız gerekir. Bu durumda, olay sayısı toplanır ve kural yalnızca toplanmış olay sayısı değerle eşleşiyorsa tetiklenir.

1. **Ölçüm** açılan listesinden izlemek istediğiniz olayı seçin. Bu örnekte, **fan motoru hata** olayı seçilmiştir.

1. İsteğe bağlı olarak, **sayımı** **toplama** olarak ayarlayabilir ve kuralın tetikleyeceği değeri sağlayabilirsiniz.

     Örneğin, 5 dakika içinde üçten fazla cihaz olayı olduğunda uyarı vermek istiyorsanız, olayı seçin ve toplama işlevini **sayı**, büyüktür ve **değeri** 3 olarak ayarlayın. **Zaman penceresini** **5 dakika**olarak ayarlayın. Kural, 5 dakika içinde cihaz tarafından üçten fazla olay gönderildiğinde tetiklenir. Kural değerlendirmesi sıklığı **zaman penceresiyle**aynıdır, yani bu örnekte kural her 5 dakikada bir değerlendirilir.

 ![Koşul](media/howto-create-event-rules-pnp/aggregate-condition-filled-out1.png)

> [!NOTE]
> **Koşul**altında birden fazla olay ölçümü eklenebilir. Birden çok koşul belirtildiğinde kuralın tetiklenmesi için tüm koşulların karşılanması gerekir. Her koşul örtülü olarak bir ' ve ' yan tümcesi ile birleştirilir. Toplama kullanılırken, her ölçüm toplanmalıdır.

### <a name="configure-actions"></a>Eylemleri yapılandırma

Bu bölümde, kural tetiklendiğinde yapılacak eylemlerin nasıl ayarlanacağı gösterilmektedir. Kuralda belirtilen tüm koşulların doğru olarak değerlendirilmesi durumunda eylemler çağrılır.

1. **Eylem** bölümlerindeki **+ eylemine** tıklayın. Burada, kullanılabilir eylemlerin listesini görürsünüz.  

   ![Eylem Ekle](media/howto-create-event-rules-pnp/add-action1.png)

1. **E-posta** eylemini seçin, eylem için bir görünen ad, **to** alanına geçerli bir e-posta adresi girin ve kural tetiklendiğinde e-posta gövdesinde görüntülenecek bir notun sağlayın.

   > [!NOTE]
   > E-postalar yalnızca uygulamaya eklenmiş olan ve en az bir kez oturum açan kullanıcılara gönderilir. Azure IoT Central 'de [Kullanıcı yönetimi](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hakkında daha fazla bilgi edinin.

   ![Eylemi Yapılandır](media/howto-create-event-rules-pnp/configure-action1.png)

1. Eylemi kaydetmek için **bitti**' yi seçin.

1. Kuralı kaydetmek için **Kaydet**' i seçin. Kural birkaç dakika içinde canlı olarak geçer ve uygulamanıza gönderilen izleme olaylarını başlatır. Kuralda belirtilen koşul karşılandığında, kural yapılandırılan e-posta eylemini tetikler.

## <a name="parameterize-the-rule"></a>Kuralı Parametreleştirme

Kurallar, **cihaz özelliklerinden** belirli özellikleri parametre olarak türetebilir. Parametre kullanımı, farklı cihazlarda olay eşiklerinin değişebileceği senaryolarda yararlıdır. Kuralı oluşturduğunuzda, 3 olay gibi mutlak bir değer sağlamak yerine, eşiği belirten bir cihaz özelliğini ( **en fazla Ideal eşik**gibi) seçin. Kural yürütüldüğünde, Device özelliğinde ayarlanan değer ile cihaz olaylarıyla eşleşir.

Parametrelerin kullanılması, yönetilecek kural sayısını azaltmak için etkili bir yoldur.

Eylemler, bir parametre olarak **Device özelliği** kullanılarak da yapılandırılabilir. Bir e-posta adresi bir cihaz özelliği olarak depolanıyorsa,, adresini tanımlarken kullanabilirsiniz.

## <a name="delete-a-rule"></a>Kuralı silme

Artık bir kurala ihtiyacınız yoksa, kuralı açıp **Sil**' i seçerek silin. Kural silindiğinde cihaz şablonundan ve ilişkili tüm cihazlardan kaldırılır.

## <a name="enable-or-disable-a-rule"></a>Kuralı etkinleştirme veya devre dışı bırakma

Etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. Kuralın kapsamında olan tüm cihazların kuralını etkinleştirmek veya devre dışı bırakmak için kuraldaki **Etkinleştir** veya **devre dışı bırak** düğmesini değiştirin.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Cihaz için bir kuralı etkinleştirme veya devre dışı bırakma

Etkinleştirmek veya devre dışı bırakmak istediğiniz kuralı seçin. Belirli bir cihazı cihaz şablonuna dahil etmek veya hariç tutmak için **kapsamlar** bölümüne bir filtre ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda kuralların nasıl oluşturulduğunu öğrendiğinize göre, önerilen sonraki adım [cihazlarınızın nasıl yönetileceğini](howto-manage-devices-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) öğrenirsiniz.
