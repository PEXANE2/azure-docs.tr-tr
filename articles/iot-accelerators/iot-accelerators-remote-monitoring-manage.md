---
title: Uzaktan Izleme çözümünde cihazları Yapılandırma-Azure | Microsoft Docs
description: Bu öğreticide, uzaktan Izleme çözüm hızlandırıcısına bağlı cihazların nasıl yapılandırılacağı gösterilir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: e4236952bd41c4955e337813ff6d706263b8ef47
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "73890911"
---
# <a name="tutorial-configure-devices-connected-to-your-monitoring-solution"></a>Öğretici: izleme çözümünüze bağlı cihazları yapılandırma

Bu öğreticide bağlı IoT cihazlarınızı yapılandırmak ve yönetmek için Uzaktan İzleme çözümü hızlandırıcısını kullanacaksınız. Çözüm hızlandırıcısına yeni bir cihaz ekler ve cihazı yapılandırırsınız.

Contoso, tesislerinden birini genişletmek için yeni makineler sipariş etmiştir. Yeni makinelerin teslim edilmesini beklerken çözümünüzün davranışını test etme amacıyla bir simülasyon çalıştırmak istiyorsunuz. Benzetimi çalıştırmak için, uzaktan Izleme çözüm hızlandırıcısına yeni bir sanal altyapı cihazı eklersiniz ve bu sanal cihazın yapılandırma güncelleştirmelerine doğru şekilde yanıt verdiğini test edersiniz. Bu öğretici sanal cihazları kullanırken, bir cihaz geliştiricisi, [Uzaktan izleme çözüm hızlandırıcısına bağlı gerçek bir cihaza](iot-accelerators-connecting-devices.md)doğrudan Yöntemler uygulayabilir.

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Sanal cihaz sağlama.
> * Sanal cihazı test etme.
> * Cihazı yeniden yapılandırma.
> * Cihazlarınızı düzenleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="add-a-simulated-device"></a>Sanal cihaz ekleme

Çözümdeki **Device Explorer** sayfasına gidin ve **+ yeni cihaz**:

[![Sanal cihaz sağlama](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-expanded.png#lightbox)

**New device** (Yeni cihaz) panelinde **Simulated** (Sanal) öğesini seçin, sağlanacak cihaz sayısını **1** olarak bırakın, **Faulty Engine** (Arızalı Motor) cihaz modelini seçin ve **Apply** (Uygula) öğesine tıklayarak sanal cihazı oluşturun:

[![Sanal altyapı cihazı sağlama](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-expanded.png#lightbox)

## <a name="test-the-simulated-device"></a>Sanal cihazı test etme

Sanal motor cihazınızı test etmek telemetri ve raporlama özellik değerlerini gönderiyor, **Device Explorer** sayfasındaki cihaz listesinden seçin. Motorunuzla ilgili canlı bilgiler **Device Details** (Cihaz Ayrıntıları) panelinde görüntülenir:

[![Yeni sanal altyapı cihazını görüntüleme](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-expanded.png#lightbox)

**Device Details** (Cihaz Ayrıntıları) sayfasında yeni cihazınızın telemetri verileri gönderdiğini doğrulayın. Cihazınızdan gelen titreşim telemetrisi akışını görüntülemek için **Vibration** (Titreşim) öğesine tıklayın:

[![Görüntülenecek telemetri akışını seçin](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-expanded.png#lightbox)

**Device Details** (Cihaz Ayrıntıları) panelinde etiket değerleri, desteklediği metotlar ve cihaz tarafından bildirilen özellikler gibi cihaz hakkındaki diğer bilgiler görüntülenir.

Ayrıntılı tanılama bilgilerini görüntülemek için **Device Details** (Cihaz Ayrıntıları) paneline inerek **Diagnostics** (Tanılama) bölümünü görüntüleyin.

## <a name="reconfigure-a-device"></a>Cihazı yeniden yapılandırma

Altyapıyı, altyapının yapılandırma özelliklerini güncelleştirebilmeniz için **Device Explorer** sayfasındaki cihaz listesinden seçin. Ardından **işler**' e tıklayın ve ardından **Özellikler**' i seçin. İşler panelinde seçilen cihaz için güncelleştirilebilecek özellik değerleri gösterilir:

[![Bir cihazı yeniden yapılandırma](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-expanded.png#lightbox)

Motorun konumunu güncelleştirmek için iş adını **UpdateEngineLocation**, boylamı **-122.15**, konumu **Factory 2**, enlemi **47.62** olarak ayarlayıp **Apply** (Uygula) öğesine tıklayın:

[![Cihaz özellik değerini güncelleştirme](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-expanded.png#lightbox)

İşin durumunu izlemek için **iş durumunu görüntüle**' ye tıklayın:

[![Cihaz özellik değerini güncelleştirme](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-expanded.png#lightbox)

İş tamamlandıktan sonra **Dashboard** (Pano) sayfasına gidin. Motor cihazı haritada yeni konumunda görüntülenir:

[![Altyapı konumunu görüntüle](./media/iot-accelerators-remote-monitoring-manage/enginelocation-inline.png)](./media/iot-accelerators-remote-monitoring-manage/enginelocation-expanded.png#lightbox)

## <a name="organize-your-devices"></a>Cihazlarınızı düzenleme

Operatör olarak cihazlarınızı düzenlemeyi ve yönetmeyi kolaylaştırmak için takım adıyla etiketleyebilirsiniz. Contoso'da saha hizmeti etkinlikleri için iki farklı takım vardır:

* Smart Vehicle takımı tırları ve prototip cihazlarını yönetmektedir.
* Smart Building takımı ise soğutucuları, asansörleri ve motorları yönetmektedir.

Tüm cihazlarınızı göstermek için **Device Explorer** sayfasına gidin ve **tüm cihazlar** filtresini seçin:

[![Tüm cihazları göster](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-expanded.png#lightbox)

### <a name="add-tags"></a>Etiket ekleme

**Trucks** (Tırlar) ve **Prototyping** (Prototip) cihazlarını seçin. Ardından **Jobs** (İşler) öğesine tıklayın.

**Jobs** (İşler) panelinde **Tag** (Etiket) öğesini seçin, iş adını **AddConnectedVehicleTag** olarak değiştirip **FieldService** adlı bir metin etiketi ekleyip **ConnectedVehicle** değerini verin. Ardından **Apply** (Uygula) öğesine tıklayın:

[![Prototip ve kamyon cihazlarına etiket ekle](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-expanded.png#lightbox)

Cihaz sayfasında tüm **Chiller** (Soğutucu), **Elevator** (Asansör) ve **Engine** (Motor) cihazlarını seçin. Ardından **Jobs** (İşler) öğesine tıklayın.

**Jobs** (İşler) panelinde **Tag** (Etiket) öğesini seçin, iş adını **AddSmartBuildingTag** olarak değiştirip **FieldService** adlı bir metin etiketi ekleyip **SmartBuilding** değerini verin. Ardından **Apply** (Uygula) öğesine tıklayın:

[![Chil, Asansör ve motor cihazlarına etiket ekleme](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-expanded.png#lightbox)

### <a name="create-filters"></a>Filtre oluşturma

Artık bu etiket değerlerini kullanarak filtre oluşturabilirsiniz. **Device Explorer** sayfasında, **cihaz gruplarını yönet**' e tıklayın:

[![Cihaz gruplarını yönetme](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-expanded.png#lightbox)

**FieldService** etiket adını ve koşul olarak **SmartBuilding** değerini kullanan bir metin filtresi oluşturun. Filtreyi **Smart Building** adıyla kaydedin:

[![Akıllı bina Filtresi Oluştur](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-expanded.png#lightbox)

**FieldService** etiket adını ve koşul olarak **ConnectedVehicle** değerini kullanan bir metin filtresi oluşturun. Filtreyi **Connected Vehicle** adıyla kaydedin.

[![Bağlı araç Filtresi Oluştur](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-expanded.png#lightbox)

Contoso operatörü artık operasyon ekibine göre cihazları sorgulayabilir:

[![Bağlı araç Filtresi Oluştur](./media/iot-accelerators-remote-monitoring-manage/filterinaction-inline.png)](./media/iot-accelerators-remote-monitoring-manage/filterinaction-expanded.png#lightbox)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Uzaktan İzleme çözümü hızlandırıcısına bağlı cihazların nasıl yapılandırılacağı ve yönetileceği gösterilmektedir. Çözüm hızlandırıcısının beklenmeyen bir uyarıda kök neden analizi gerçekleştirmek için nasıl kullanılacağını öğrenmek için sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Bir uyarıda kök neden analizi yürütme](iot-accelerators-remote-monitoring-root-cause-analysis.md)
