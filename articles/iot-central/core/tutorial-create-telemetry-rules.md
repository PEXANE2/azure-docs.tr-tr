---
title: Öğretici - Azure IoT Merkezi uygulamanızda kurallar oluşturun ve yönetin
description: Bu öğretici, Azure IoT Merkezi kurallarının cihazlarınızı gerçek zamanlı olarak izlemenize ve kural tetiklendiğinde e-posta gönderme gibi eylemleri otomatik olarak başlatmanızı nasıl sağladığını gösterir.
author: dominicbetts
ms.author: dobett
ms.date: 04/06/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 555da74da65f3b1897a276cf819a263334cfa053
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80999055"
---
# <a name="tutorial-create-a-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Öğretici: Azure IoT Central uygulamanızda bir kural oluşturun ve bildirimler ayarlayın

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bağlı cihazlarınızı uzaktan izlemek için Azure IoT Central'ı kullanabilirsiniz. Azure IoT Merkezi kuralları, cihazlarınızı neredeyse gerçek zamanlı olarak izlemenize ve e-posta gönderme gibi eylemleri otomatik olarak çağırmanıza izin verir. Bu makalede, aygıtlarınızın gönderdiği telemetriyi izlemek için kuralların nasıl oluşturuluyorum.

Aygıtlar, aygıttan sayısal veri göndermek için telemetri kullanır. Seçili aygıt telemetrisi belirtilen eşiği geçtiğinde bir kural tetikler.

Bu öğreticide, simüle edilmiş bir çevre sensörü aygıtındaki sıcaklık 70&deg; F'yi aştığında e-posta göndermek için bir kural oluşturursunuz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Kural oluşturma
> * E-posta eylemi ekleme

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce Azure [IoT Merkezi oluştur uygulamasını](./quick-deploy-iot-central.md) tamamlayın ve [IoT Central uygulamanıza simüle edilmiş bir aygıt ekleyin](./quick-create-simulated-device.md) ve çalışmak için **MXChip IoT DevKit** aygıt şablonu oluşturmaya hızlı bir şekilde başlayın.

## <a name="create-a-rule"></a>Kural oluşturma

Bir telemetri kuralı oluşturmak için aygıt şablonunda en az bir telemetri değeri içermesi gerekir. Bu öğretici sıcaklık ve nem telemetri gönderir bir simüle **MXChip IoT DevKit** cihaz kullanır. Bu aygıt şablonunu eklediniz ve [IoT Central uygulama hızlı başlatmanıza benzetilen bir benzetim aygıtı oluşturdunuz.](./quick-create-simulated-device.md) Kural, cihaz tarafından bildirilen sıcaklığı izler ve 70 derecenin üzerine çıktığında bir e-posta gönderir.

1. Sol bölmede **Kurallar'ı**seçin.

1. Henüz herhangi bir kural oluşturmadıysanız, aşağıdaki ekranı görürsünüz:

    ![Henüz kural yok](media/tutorial-create-telemetry-rules/rules-landing-page1.png)

1. Yeni **+** bir kural eklemek için seçin.

1. Kuralı tanımlamak için _Sıcaklık monitörünü_ girin ve Enter tuşuna basın.

1. **MXChip IoT DevKit** aygıt şablonunu seçin. Varsayılan olarak, kural otomatik olarak aygıt şablonu ile ilişkili tüm aygıtlar için geçerlidir. Aygıtların bir alt kümesine filtre uygulayın, **+ Filtre'yi** seçin ve aygıtları tanımlamak için aygıt özelliklerini kullanın. Kuralı devre dışı katmak için, kural üstbilgisinde **Etkin/Devredışı Düğmesi'ni** değiştirin:

    ![Filtreler ve etkinleştirin](media/tutorial-create-telemetry-rules/device-filters.png)

### <a name="configure-the-rule-conditions"></a>Kural koşullarını yapılandırma

Koşullar, kuralın izlediği ölçütleri tanımlar. Bu öğreticide, sıcaklık 70&deg; F'yi aştığında kuralı ateşle göre yapılandırırsınız.

1. **Telemetri** açılır düşüşünde **Sıcaklık'ı** seçin.

1. Sonra, **Operatör** olarak **daha büyük** ve **Değer**olarak _70_ girin seçin.

    ![Koşul](media/tutorial-create-telemetry-rules/condition-filled-out1.png)

1. İsteğe bağlı olarak, bir **Zaman toplama**ayarlayabilirsiniz. Bir zaman toplama seçtiğinizde, toplama açılır bırakma ortalaması veya toplamı gibi bir toplama türü de seçmeniz gerekir.

    * Toplama olmadan, kural koşulu karşılayan her telemetri veri noktası için tetikler. Örneğin, kuralı sıcaklık 70'in üzerinde olduğunda tetikleecek şekilde yapılandırırsanız, aygıt sıcaklığı bu değeri aştığında kural neredeyse anında tetikler.
    * Toplama ile, zaman penceresinde telemetri veri noktalarının toplam değeri koşulu karşılıyorsa kural tetikler. Örneğin, kuralı sıcaklık 70'in üzerinde olduğunda ve ortalama 10 dakikalık bir zaman toplamasıyla tetikleecek şekilde yapılandırırsanız, kural, aygıtın 10 dakikalık bir aralıkla hesaplanan ortalama sıcaklığı 70'ten büyük bildirdiği zaman tetikler.

     ![Toplam durum](media/tutorial-create-telemetry-rules/aggregate-condition-filled-out1.png)

Bir kurala **+ Koşul'u**seçerek birden çok koşul ekleyebilirsiniz. Birden çok koşul belirtildiğinde, kuralın tetiklemesi için tüm koşulların karşılanması gerekir. Her koşul acıklı `AND` bir yan tümce ile birleştirilir. Birden çok koşulla zaman toplama kullanıyorsanız, tüm telemetri değerleri toplanmalıdır.

### <a name="configure-actions"></a>Eylemleri yapılandırma

Koşulu tanımladıktan sonra, kural çıktığında yapılacak eylemleri ayarlarsınız. Kuralda belirtilen tüm koşullar doğru olarak değerlendirildiğinde eylemler çağrılır.

1. **Eylemler** bölümünde **+ E-posta'yı** seçin.

1. Eylem için görüntü adı olarak _Sıcaklık uyarısı_ girin, E-posta **adresinizi Için** alanına ve cihazı kontrol _etmelisiniz!_ e-postanın gövdesinde görünecek bir not olarak.

    > [!NOTE]
    > E-postalar yalnızca uygulamaya eklenen ve en az bir kez oturum açmış olan kullanıcılara gönderilir. Azure IoT Central'da [kullanıcı yönetimi](howto-administer.md) hakkında daha fazla bilgi edinin.

   ![Eylemi Yapılandırma](media/tutorial-create-telemetry-rules/configure-action1.png)

1. Eylemi kaydetmek için **Bitti'yi**seçin. Bir kurala birden çok eylem ekleyebilirsiniz.

1. Kuralı kaydetmek için **Kaydet'i**seçin. Kural birkaç dakika içinde yayına girer ve uygulamanıza gönderilen telemetriyi izlemeye başlar. Kuralda belirtilen koşul karşılandığında, kural yapılandırılan e-posta eylemini tetikler.

Bir süre sonra, kural çıktığında bir e-posta iletisi alırsınız:

![Örnek e-posta](media/tutorial-create-telemetry-rules/email.png)

## <a name="delete-a-rule"></a>Kuralı silme

Artık bir kurala ihtiyacınız yoksa, kuralı açarak ve **Sil'i**seçerek kuralı silin.

## <a name="enable-or-disable-a-rule"></a>Bir kuralı etkinleştirme veya devre dışı

Etkinleştirmek veya devre dışı kalmak istediğiniz kuralı seçin. Kural kapsamında ki tüm aygıtlar için kuralı etkinleştirmek veya devre dışı katmak için kuraldaki **Etkin/Devreden** düğmesini değiştirin.

## <a name="enable-or-disable-a-rule-for-specific-devices"></a>Belirli aygıtlar için bir kuralı etkinleştirme veya devre dışı

Özelleştirmek istediğiniz kuralı seçin. Kuralın kapsamını izlemek istediğiniz aygıtlara daraltmak için **Hedef aygıtlar** bölümünde bir veya daha fazla filtre kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Telemetri tabanlı kural oluşturma
* Eylem ekleme

Artık bir eşik tabanlı kural tanımladığınıza göre önerilen bir sonraki adım nasıl yapılacağını öğrenmektir:

> [!div class="nextstepaction"]
> [Sürekli veri dışa aktarma yapılandırma.](./howto-export-data.md)
