---
title: Azure IoT Merkezi uygulamanızda aygıt gruplarını kullanma | Microsoft Dokümanlar
description: Operatör olarak, Azure IoT Merkezi uygulamanızdaki aygıtlardan telemetrianaliz etmek için aygıt gruplarını nasıl kullanacağınızı öğrenin.
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 758ac037fcd224d02f62239b3408b41b50390147
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77167292"
---
# <a name="tutorial-use-device-groups-to-analyze-device-telemetry"></a>Öğretici: Cihaz telemetrisini analiz etmek için aygıt gruplarını kullanma

Bu makalede, bir operatör olarak Azure IoT Merkezi uygulamanızda aygıt telemetrisini analiz etmek için aygıt gruplarını nasıl kullanacağı açıklanmaktadır.

Aygıt grubu, belirli ölçütler ile eşleştikleri için birlikte gruplanan aygıtların listesidir. Aygıt grupları, aygıtları daha küçük, mantıksal gruplar halinde gruplayarak aygıtları ölçekte yönetmenize, görselleştirmenize ve çözümlemenize yardımcı olur. Örneğin, seattle'daki tüm klima aygıtlarını listeleyen bir aygıt grubu oluşturarak bir teknisyenin sorumlu oldukları cihazları bulmasını sağlayabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Cihaz grubu oluşturma
> * Cihaz telemetrisini analiz etmek için bir aygıt grubu kullanma

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce [Azure IoT Merkezi oluştur uygulamasını](./quick-deploy-iot-central.md) tamamlamalı ve [IoT Central uygulamanıza simüle edilmiş bir aygıt eklemeniz](./quick-create-pnp-device.md) gerekmektedir ve çalışmak için **MXChip IoT DevKit** aygıt şablonunu oluşturmaya hızlı bir şekilde başlamalısınız.

## <a name="create-simulated-devices"></a>Benzetimli aygıtlar oluşturma

Bir aygıt grubu oluşturmadan önce, bu öğreticide kullanmak üzere **MXChip IoT DevKit** aygıt şablonundan en az beş simüle edilmiş aygıt ekleyin:

![Beş simüle sensör cihazı](./media/tutorial-use-device-groups/simulated-devices.png)

Simüle edilen sensör aygıtlarından dördü için, müşteri adını *Contoso*olarak ayarlamak için **Yönet aygıtı** görünümünü kullanın:

![Müşteri adını Contoso olarak ayarlama](./media/tutorial-use-device-groups/customer-name.png)

## <a name="create-a-device-group"></a>Cihaz grubu oluşturma

Aygıt grubu oluşturmak için:

1. Sol bölmedeki **Aygıt gruplarını** seçin.

1. Seçin **+**:

    ![Yeni cihaz grubu](media/tutorial-use-device-groups/image1.png)

1. Cihaz grubunuza *Contoso aygıtları*adını verin. Ayrıca bir açıklama da ekleyebilirsiniz. Aygıt grubu yalnızca tek bir aygıt şablonundaki aygıtları içerebilir. Bu grup için kullanılacak **MXChip IoT DevKit** aygıt şablonunu seçin.

1. Aygıt grubunu yalnızca **Contoso'ya**ait cihazları içerecek şekilde özelleştirmek için **+ Filtre'yi**seçin. Değer olarak **Müşteri Adı** özelliğini, **Eşittir karşılaştırma** işlecini ve **Contoso'yu** seçin. Birden çok filtre ekleyebilirsiniz ve **tüm** filtre ölçütlerini karşılayan aygıtlar aygıt grubuna yerleştirilir. Oluşturduğunuz aygıt grubuna, uygulamaya erişimi olan herkes erişebilir, böylece herkes aygıt grubunu görüntüleyebilir, değiştirebilir veya silebilir:

    ![Aygıt grubu sorgusu](media/tutorial-use-device-groups/image2.png)

    > [!TIP]
    > Aygıt grubu dinamik bir sorgudur. Aygıtlistesini her görüntülediğiniz de listede farklı aygıtlar olabilir. Liste, hangi aygıtların şu anda sorgu ölçütlerini karşıladığına bağlıdır.

1. **Kaydet**'i seçin.

> [!NOTE]
> Azure IoT Edge aygıtları için, aygıt grubu oluşturmak için Azure IoT Edge şablonlarını seçin.

## <a name="analytics"></a>Analiz

Gruptaki aygıtlardan gelen telemetriyi analiz etmek için **Analytics'i** bir aygıt grubuyla kullanabilirsiniz. Örneğin, tüm Contoso çevre sensörleri tarafından bildirilen ortalama sıcaklığı çizebilirsiniz.

Bir aygıt grubu için telemetriyi analiz etmek için:

1. Sol bölmede **Analytics'i** seçin.

1. Oluşturduğunuz **Contoso aygıt** grubunu seçin. Daha sonra hem **Sıcaklık** hem de **Nem** telemetri türlerini ekleyin:

    ![Analitik oluşturma](./media/tutorial-use-device-groups/create-analysis.png)

    Bir toplama türünü seçmek için telemetri türlerinin yanındaki dişli çarkı simgelerini kullanın. Varsayılan **ortalamadır.** Toplu verilerin gösterilme şeklini değiştirmek için **Split'i** kullanın. Örneğin, aygıt kimliğine göre bölünürseniz, **Çözümle'yi**seçtiğinizde her cihaz için bir çizim görürsünüz.

1. Ortalama telemetri değerlerini görüntülemek için **Analiz** et'i seçin:

    ![Analizi görüntüle](./media/tutorial-use-device-groups/view-analysis.png)

    Görünümü özelleştirebilir, gösterilen süreyi değiştirebilir ve verileri dışa aktarabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Merkezi uygulamanızda aygıt gruplarını nasıl kullanacağınızı öğrendiğinize göre, önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [Telemetri kuralları nasıl oluşturulur?](tutorial-create-telemetry-rules.md)
