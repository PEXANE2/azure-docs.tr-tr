---
title: Quickstart - Azure IoT Central'da kuralları ve eylemleri yapılandırma
description: Bu hızlı başlangıç, bir oluşturucu olarak Azure IoT Central uygulamanızda telemetri tabanlı kuralları ve eylemleri nasıl yapılandırabileceğinizi gösterir.
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 66c3bd8650d1194d5d753c1dc967ec8e870c8748
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998974"
---
# <a name="quickstart-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Hızlı başlatma: Azure IoT Central'da cihazınız için kuralları ve eylemleri yapılandırın

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

Bu hızlı başlatmada, bir aygıt sensörü tarafından bildirilen sıcaklık 90&deg; F'yi aştığında e-posta gönderen bir kural oluşturursunuz.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, önceki iki hızlı başlatmayı tamamlamanız bir [Azure IoT Central uygulaması oluşturmalı](./quick-deploy-iot-central.md) ve çalışmak için **MXChip IoT DevKit** aygıt şablonunu oluşturmak için [IoT Central uygulamanıza simüle edilmiş bir aygıt eklemeniz](./quick-create-simulated-device.md) gerekir.

## <a name="create-a-telemetry-based-rule"></a>Telemetri tabanlı kural oluşturma

1. Uygulamanıza yeni bir telemetri tabanlı kural eklemek için, sol bölmede **Kurallar'ı**seçin.

1. Yeni bir kural oluşturmak **+** için.

1. Kural adı olarak **Çevre sıcaklığını** girin.

1. Hedef **aygıtlar** bölümünde, aygıt şablonu olarak **MXChip IoT DevKit'i** seçin. Bu seçenek, kuralın aygıt şablonu türüne göre uygulandığı aygıtları filtreler. **+ Filtre'yi**seçerek daha fazla filtre ölçütleri ekleyebilirsiniz.

1. **Koşullar** bölümünde, kuralınızı neyin tetiklediğini tanımlarsınız. Sıcaklık telemetrisine dayalı bir durumu tanımlamak için aşağıdaki bilgileri kullanın:

    | Alan        | Değer            |
    | ------------ | ---------------- |
    | Ölçüm  | Sıcaklık      |
    | İşleç     | büyüktür  |
    | Değer        | 90               |

    Daha fazla koşul eklemek için **+ Koşul'u**seçin.

    ![Kural koşulu oluşturma](./media/quick-configure-rules/condition.png)

1. Kural tetiklendiğinde çalıştırmak için bir e-posta eylemi eklemek için **+ E-posta'yı**seçin.

1. Eyleminizi tanımlamak için aşağıdaki tablodaki bilgileri kullanın ve ardından **Bitti'yi**seçin:

    | Ayar   | Değer                                             |
    | --------- | ------------------------------------------------- |
    | Görünen ad | Operatör e-posta eylemi                          |
    | Alıcı        | E-posta adresiniz                                |
    | Notlar     | Çevre sıcaklığı eşiği aştı. |

    > [!NOTE]
    > Bir e-posta bildirimi almak için e-posta adresinin [uygulamadaki bir kullanıcı kimliği](howto-administer.md) olması ve bu kullanıcının uygulamada en az bir kez oturum açmış olması gerekir.

    ![Kural eylemi oluşturma](./media/quick-configure-rules/action.png)

1. **Kaydet'i**seçin. Kuralınız **Kurallar** sayfasında listelenir.

## <a name="test-the-rule"></a>Kuralı test etme

Kural kaydedildikten kısa bir süre sonra dinamik olur. Kuralda tanımlanan koşullar karşılandığında, uygulamanız eylemde belirtilen e-posta adresine bir ileti gönderir.

> [!NOTE]
> Testiniz tamamlandıktan sonra gelen kutunuzda uyarı almayı durdurmak için kuralı kapatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Telemetri tabanlı kural oluşturma
* Eylem ekleme

Uygulamanıza bağlı izleme aygıtları hakkında daha fazla bilgi edinmek için hızlı bir şekilde çalışmaya devam edin:

> [!div class="nextstepaction"]
> [Aygıtlarınızı izlemek için Azure IoT Central'ı kullanın.](quick-monitor-devices.md)
