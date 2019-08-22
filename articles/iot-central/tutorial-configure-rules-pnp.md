---
title: Azure IoT Central’da kural ve eylem yapılandırma | Microsoft Docs
description: Bu öğreticide, bir oluşturucu olarak Azure IoT Central uygulamanızda telemetri tabanlı kural ve eylemleri nasıl yapılandıracağınız gösterilir.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: c2aa6edfe7ce9b670ea1015e2da72f3ecc48c9ec
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878155"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central-preview-features"></a>Öğretici: Azure IoT Central cihazınız için kuralları ve eylemleri yapılandırma (Önizleme özellikleri)

*Bu makale, işleçler, oluşturucular ve yöneticiler için geçerlidir.*

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu öğreticide, bir ortam algılayıcısı cihazındaki sıcaklık 90&deg; F 'yi aştığında bir e-posta gönderen bir kural oluşturacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Telemetri tabanlı kural oluşturma
> * Eylem ekle

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, ile birlikte çalışmak üzere **ortam algılayıcısı** cihaz şablonu oluşturmak için, uygulama öğreticisinde [Yeni bir cihaz türü tanımlama](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) işlemini tamamlamalısınız.

## <a name="create-a-telemetry-based-rule"></a>Telemetri tabanlı kural oluşturma

1. Uygulamanıza yeni bir telemetri tabanlı kural eklemek için sol gezinti menüsünde **kurallar**' ı seçin.

1. Yeni bir kural oluşturmak için **+ Yeni**' yi seçin. Ardından **telemetri**' i seçin.

1. Kural adı olarak **çevresel sıcaklık** girin.

1. **Kapsam** bölümünde, cihaz şablonu olarak **ortam algılayıcısı** ' nı seçin. Bu kuralın geçerli olduğu cihazların kapsamı. **+ Filtresi**kullanarak daha fazla filtre ölçütü ekleyebilirsiniz.

1. **Koşul** bölümünde, kuralınızı neyin tetikleyeceğini tanımlarsınız. Sıcaklık telemetrisine dayalı bir koşul tanımlamak için aşağıdaki bilgileri kullanın:

    | Alan                                        | Value                             |
    | -------------------------------------------- | ------------------------------    |
    | Ölçüm                                  | Sıcaklık                       |
    | Operator                                     | büyüktür                   |
    | Value                                        | 90                                |

    Daha fazla koşul eklemek için **+ koşul**' ı seçin.

    ![Kural koşulu oluştur](./media/tutorial-configure-rules-pnp/condition.png)

1. Kural tetiklendiğinde çalıştırılacak bir eylem eklemek için **+ eylem**' i seçin ve **e-posta**' yı seçin.

1. Eyleminizi tanımlamak için aşağıdaki tablodaki bilgileri kullanın:

    | Ayar   | Value                                             |
    | --------- | ------------------------------------------------- |
    | Bitiş        | E-posta adresiniz                                |
    | Notlar     | Çevresel sıcaklık eşiği aştı. |

    > [!NOTE]
    > Bir e-posta bildirimi almak için e-posta adresinin [uygulamadaki bir kullanıcı kimliği](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) olması ve bu kullanıcının uygulamada en az bir kez oturum açmış olması gerekir.

    ![Kural oluşturma eylemi](./media/tutorial-configure-rules-pnp/action.png)

1. **Kaydet**’i seçin. Kuralınız **kurallar** sayfasında listelenir.

## <a name="test-the-rule"></a>Kuralı test etme

Kural kaydedildikten kısa bir süre sonra dinamik olur. Kuralda tanımlanan koşullar karşılandığında, uygulamanız eylemde belirtilen e-posta adresine bir ileti gönderir.

> [!NOTE]
> Testiniz tamamlandıktan sonra, gelen kutunuzda uyarı almayı durdurmak için kuralı kapatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Telemetri tabanlı kural oluşturma
* Eylem ekle

Artık bir eşik tabanlı kural tanımladığınıza göre önerilen sonraki adım şunları yapmanız gerekir:

> [!div class="nextstepaction"]
> [Bir olay kuralı oluşturun ve bildirimleri ayarlayın](howto-create-event-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).
