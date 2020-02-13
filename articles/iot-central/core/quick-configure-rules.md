---
title: Hızlı başlangıç-Azure IoT Central kuralları ve eylemleri yapılandırma
description: Bu hızlı başlangıçta, Azure IoT Central uygulamanızda telemetri tabanlı kuralların ve eylemlerin nasıl yapılandırılacağı hakkında bir Oluşturucu olarak gösterilir.
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 4bbf571d6b73a6f43c1c3b1ce261da6963a74183
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169510"
---
# <a name="quickstart-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Hızlı başlangıç: Azure IoT Central cihazınız için kuralları ve eylemleri yapılandırma

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bu hızlı başlangıçta, bir cihaz algılayıcısı tarafından bildirilen sıcaklık 90&deg; F 'yi aştığında bir e-posta gönderen bir kural oluşturacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, önceki iki hızlı başlangıçu [bir Azure IoT Central uygulaması oluşturun](./quick-deploy-iot-central.md) ve birlikte çalışmak üzere **Mxyongaıot devkit** cihaz şablonunu oluşturmak için [IoT Central uygulamanıza sanal bir cihaz ekleyin](./quick-create-pnp-device.md) .

## <a name="create-a-telemetry-based-rule"></a>Telemetri tabanlı kural oluşturma

1. Uygulamanıza yeni bir telemetri tabanlı kural eklemek için, sol bölmede **kurallar**' ı seçin.

1. Yeni bir kural oluşturmak için **+** ' yi seçin.

1. Kural adı olarak **çevresel sıcaklık** girin.

1. **Hedef cihazlar** bölümünde, cihaz şablonu olarak **Mxyongaıot devkit** ' i seçin. Bu seçenek, kural için geçerli olan cihazları cihaz şablonu türüne göre filtreler. **+ Filter**öğesini seçerek daha fazla filtre ölçütü ekleyebilirsiniz.

1. **Koşullar** bölümünde, kuralınızı neyin tetikleyeceğini tanımlarsınız. Sıcaklık telemetrisine dayalı bir koşul tanımlamak için aşağıdaki bilgileri kullanın:

    | Alan        | Değer            |
    | ------------ | ---------------- |
    | Ölçüm  | Sıcaklık      |
    | İşleç     | Şundan büyüktür  |
    | Değer        | 90               |

    Daha fazla koşul eklemek için **+ koşul**' ı seçin.

    ![Kural koşulu oluştur](./media/quick-configure-rules/condition.png)

1. Kural tetiklendiğinde çalıştırılacak bir e-posta eylemi eklemek için **+ e-posta**' yı seçin.

1. Eyleminizi tanımlamak için aşağıdaki tablodaki bilgileri kullanın ve ardından **bitti**' yi seçin:

    | Ayar   | Değer                                             |
    | --------- | ------------------------------------------------- |
    | Görünen ad | İşleç e-posta eylemi                          |
    | Alıcı        | E-posta adresiniz                                |
    | Notlar     | Çevresel sıcaklık eşiği aştı. |

    > [!NOTE]
    > Bir e-posta bildirimi almak için e-posta adresinin [uygulamadaki bir kullanıcı kimliği](howto-administer.md) olması ve bu kullanıcının uygulamada en az bir kez oturum açmış olması gerekir.

    ![Kural oluşturma eylemi](./media/quick-configure-rules/action.png)

1. **Kaydet**’i seçin. Kuralınız **kurallar** sayfasında listelenir.

## <a name="test-the-rule"></a>Kuralı test etme

Kural kaydedildikten kısa bir süre sonra dinamik olur. Kuralda tanımlanan koşullar karşılandığında, uygulamanız eylemde belirtilen e-posta adresine bir ileti gönderir.

> [!NOTE]
> Testiniz tamamlandıktan sonra, gelen kutunuzda uyarı almayı durdurmak için kuralı kapatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Telemetri tabanlı kural oluşturma
* Eylem ekleme

Uygulamanıza bağlı olan cihazları izleme hakkında daha fazla bilgi edinmek için hızlı başlangıç ile devam edin:

> [!div class="nextstepaction"]
> [Cihazlarınızı izlemek Için Azure IoT Central kullanın](quick-monitor-devices.md).
