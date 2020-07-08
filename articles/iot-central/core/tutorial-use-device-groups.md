---
title: Azure IoT Central uygulamanızda cihaz gruplarını kullanma | Microsoft Docs
description: Bir operatör olarak, Azure IoT Central uygulamanızdaki cihazlardan telemetri çözümlemek için cihaz gruplarını nasıl kullanacağınızı öğrenin.
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 58fc71ab05c34e8acd252e7a1984c55996d1b3a7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80999031"
---
# <a name="tutorial-use-device-groups-to-analyze-device-telemetry"></a>Öğretici: cihaz telemetrisini çözümlemek için cihaz gruplarını kullanma

Bu makalede, Azure IoT Central uygulamanızda cihaz telemetrisini çözümlemek üzere cihaz gruplarını kullanarak nasıl bir operatör olarak kullanılacağı açıklanmaktadır.

Bir cihaz grubu, belirtilen bazı ölçütlerle eşleştiğinden, birlikte gruplanmış cihazların bir listesidir. Cihaz grupları cihazları daha küçük ve mantıksal gruplara gruplandırarak, cihazları ölçekli olarak yönetmenize, görselleştirmenize ve çözümlemenize yardımcı olur. Örneğin, bir teknisyenin sorumlu oldukları cihazları bulmasını sağlamak için Seattle 'daki tüm AIR klimaları cihazlarını listelemek üzere bir cihaz grubu oluşturabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz grubu oluşturma
> * Cihaz telemetrisini çözümlemek için bir cihaz grubu kullanma

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, [Azure IoT Central oluşturma uygulamasını](./quick-deploy-iot-central.md) tamamlayıp [IoT Central uygulamanıza sanal bir cihaz ekleyerek](./quick-create-simulated-device.md) , birlikte çalışmak üzere **mxyonga IoT devkit** cihaz şablonunu oluşturmanız gerekir.

## <a name="create-simulated-devices"></a>Sanal cihazlar oluşturma

Bir cihaz grubu oluşturmadan önce, bu öğreticide kullanmak üzere **Mxyonga IoT DevKit** cihaz şablonundan en az beş sanal cihaz ekleyin:

![Beş sanal algılayıcı cihazı](./media/tutorial-use-device-groups/simulated-devices.png)

Sanal algılayıcı aygıtlarından oluşan dört için, müşteri adını *contoso*olarak ayarlamak üzere **cihaz yönetme** görünümünü kullanın:

![Müşteri adını contoso olarak ayarla](./media/tutorial-use-device-groups/customer-name.png)

## <a name="create-a-device-group"></a>Cihaz grubu oluşturma

Bir cihaz grubu oluşturmak için:

1. Sol bölmedeki **cihaz grupları** ' nı seçin.

1. Şunu seçin **+** :

    ![Yeni cihaz grubu](media/tutorial-use-device-groups/image1.png)

1. Cihaz grubunuza *contoso cihazları*adı verin. Ayrıca, bir açıklama ekleyebilirsiniz. Bir cihaz grubu yalnızca tek bir cihaz şablonundan cihaz içerebilir. Bu grup için kullanılacak **Mxyongaıot DevKit** cihaz şablonunu seçin.

1. Cihaz grubunu yalnızca **contoso**'ya ait olan cihazları içerecek şekilde özelleştirmek Için **+ filtre**' yi seçin. **Müşteri adı** özelliğini, **eşittir** karşılaştırma işlecini ve **contoso** değerini seçin. Birden çok filtre ekleyebilirsiniz ve **Tüm** filtre ölçütlerini karşılayan cihazlar cihaz grubuna yerleştirilir. Oluşturduğunuz cihaz grubuna, uygulamaya erişimi olan herkes tarafından erişilebilir, böylece herkes cihaz grubunu görüntüleyebilir, değiştirebilir veya silebilir:

    ![Cihaz grubu sorgusu](media/tutorial-use-device-groups/image2.png)

    > [!TIP]
    > Cihaz grubu dinamik bir sorgudur. Cihazların listesini her görüntülediğinizde, listede farklı cihazlar olabilir. Liste, şu anda sorgu ölçütlerine uyan cihazlara bağlıdır.

1. **Kaydet**'i seçin.

> [!NOTE]
> Azure IoT Edge cihazlar için bir cihaz grubu oluşturmak üzere Azure IoT Edge Şablonlar ' ı seçin.

## <a name="analytics"></a>Analiz

Gruptaki cihazlardan telemetri çözümlemek için bir cihaz grubuyla **Analytics** kullanabilirsiniz. Örneğin, tüm contoso ortam sensörleri tarafından raporlanan ortalama sıcaklığın çizebilirsiniz.

Bir cihaz grubunun telemetrisini çözümlemek için:

1. Sol bölmedeki **analiz** ' ı seçin.

1. Oluşturduğunuz **contoso cihazları** cihaz grubunu seçin. Ardından hem **sıcaklık** hem de **nem** telemetrisi türlerini ekleyin:

    ![Analiz oluştur](./media/tutorial-use-device-groups/create-analysis.png)

    Bir toplama türü seçmek için telemetri türlerinin yanındaki dişli tekerleği simgesini kullanın. Varsayılan değer **Average**' dir. Toplama verilerinin nasıl gösterileceğini değiştirmek için **Böl** ' ü kullanın. Örneğin, cihaz KIMLIĞINE göre böldüğünüz takdirde, **Çözümle**' yi seçtiğinizde her bir cihaz için bir çizim görürsünüz.

1. Ortalama telemetri değerlerini görüntülemek için **Çözümle** ' yi seçin:

    ![Analizi görüntüle](./media/tutorial-use-device-groups/view-analysis.png)

    Görünümü özelleştirebilir, gösterilen zaman dilimini değiştirebilir ve verileri dışarı aktarabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanızda cihaz gruplarını nasıl kullanacağınızı öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Telemetri kuralları oluşturma](tutorial-create-telemetry-rules.md)
