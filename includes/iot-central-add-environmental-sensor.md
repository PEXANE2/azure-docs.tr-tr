---
title: include dosyası
description: include dosyası
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 03/12/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 9618dc1cef8d04cc5906579af0ef372694aeaaa9
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673988"
---
## <a name="create-a-device-template"></a>Cihaz şablonu oluşturma

Yerel makinenizde `environmental-sensor` çağrılan bir klasör oluşturun.

Çevre [sensörü yetenek modeli](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json) JSON dosyasını `environmental-sensor` indirin ve klasöre kaydedin.

İndirdiğiniz `EnvironmentalSensorInline.capabilitymodel.json` dosyadaki iki örneği şirket `{YOUR_COMPANY_NAME_HERE}` adınız ile değiştirmek için bir metin düzenleyicisi kullanın.

Azure IoT Merkezi uygulamanızda, aygıt *Environmental sensor* yetenek modeli dosyasını `EnvironmentalSensorInline.capabilitymodel.json` içe aktararak Çevre sensörü adı verilen bir aygıt şablonu oluşturun:

![İçe aktarılan aygıt yetenek modeline sahip aygıt şablonu](./media/iot-central-add-environmental-sensor/device-template.png)

Cihaz özelliği modeli iki arabirim içerir: standart **Aygıt Bilgileri** arabirimi ve özel **Çevre Sensörü** arabirimi. **Çevre Sensörü** arabirimi aşağıdaki yetenekleri tanımlar:

| Tür | Görünen Ad | Açıklama |
| ---- | ------------ | ----------- |
| Özellik | Cihaz Durumu     | Aygıtın durumu. Çevrimiçi/çevrimdışı iki durum mevcuttur. |
| Özellik (yazılabilir) | Müşteri Adı    | Aygıtı şu anda çalıştıran müşterinin adı. |
| Özellik (yazılabilir) | Parlaklık Düzeyi | Cihazdaki ışığın parlaklık düzeyi. 1 (yüksek), 2 (orta), 3 (düşük) olarak belirtilebilir. |
| Telemetri | Sıcaklık | Aygıt tarafından algılanan geçerli sıcaklık. |
| Telemetri | Nem oranı    | Cihaz tarafından algılanan akım nemi. |
| Komut | Yanıp          | Belirli bir zaman aralığı için cihazdaki LED'i yanıp sönmeye başlayın. |
| Komut | turnon         | Cihazın LED'ini açın. |
| Komut | kapatma        | Cihazın LED'ini kapatın. |
| Komut | rundiagnostics | Bu eşzamanlı komut, aygıtta bir tanılama çalışmasını başlatır. |

**IoT** Central uygulamanızda Aygıt Durumu özelliğinin nasıl görüntülenebildiğini özelleştirmek için aygıt şablonunda **Özelleştir'i** seçin. Aygıt **Durumu** girişini genişletin, **Doğru ad** olarak _Çevrimiçi'yi_ ve **False adı**olarak _Çevrimdışı_ girin. Ardından değişiklikleri kaydedin:

![Aygıt şablonu özelleştirme](./media/iot-central-add-environmental-sensor/customize-template.png)

## <a name="create-views"></a>Görünümler oluşturma

Görünümler, IoT Central uygulamanıza bağlı aygıtlarla etkileşim kurmanıza izin verir. Örneğin, telemetriyi görüntüleyen görünümler, özellikleri görüntüleyen görünümler ve yazılabilir ve bulut özelliklerini yeniden deletmenize izin veren görünümler olabilir. Görünümler aygıt şablonunun bir parçasıdır.

**Çevre sensörü** aygıt şablonuna bazı varsayılan görünümler eklemek için aygıt şablonunuza gidin, **Görünümler'i**seçin ve **Varsayılan Görünümler oluştur** döşemesini seçin. Genel **Bakış** ve **Hakkında'nın Açık**olduğundan emin olun ve ardından **varsayılan pano görünümünü oluştur'u(lar)** seçin. **About** Artık şablonunuzda tanımlanan iki varsayılan görünüm var.

**Çevre Sensörü** arabirimi iki yazılabilir özellik içerir - **Müşteri Adı** ve **Parlaklık Düzeyi.** Bir görünüm oluşturmak için, bu özellikleri yeniden kullanmak için kullanabilirsiniz:

1. **Görünümler'i** seçin ve ardından **Düzenleme aygıtını ve bulut veri** döşemesini seçin.

1. _Özellikler'i_ form adı olarak girin.

1. Parlaklık **Düzeyi** ve **Müşteri Adı** özelliklerini seçin. Ardından **Ekle bölümünü**seçin.

1. Yaptığınız değişiklikleri kaydedin.

![Özellik düzenlemeyi etkinleştirmek için görünüm ekleme](./media/iot-central-add-environmental-sensor/properties-view.png)

## <a name="publish-the-template"></a>Şablonu yayımlama

**Çevre sensörü** aygıtı şablonu kullanan bir aygıt eklemeden önce yayımlamanız gerekir.

Aygıt şablonunda **Yayımla'yı**seçin. Bu **aygıt şablonunu uygulama paneline** yayımla'da **Yayımla'yı**seçin.

Şablonun kullanıma hazır olup olmadığını kontrol etmek için, IoT Merkezi uygulamanızdaki **Cihazlar** sayfasına gidin. **Cihazlar** bölümünde, uygulamada yayınlanan aygıtların bir listesi gösterilmektedir:

![Aygıtlar sayfasında yayınlanan şablonlar](./media/iot-central-add-environmental-sensor/published-templates.png)

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IoT Merkezi uygulamanızda, önceki bölümde oluşturduğunuz aygıt şablonuna gerçek bir aygıt ekleyin:

1. **Cihazlar** sayfasında **Çevre sensörü** aygıtı şablonuna tıklayın.

    > [!TIP]
    > **+ Yeni'yi**seçmeden önce kullanılacak şablonu seçtiğinizden emin olun, aksi takdirde ilişkili olmayan bir aygıt oluşturursunuz.

1. Seçin **+ Yeni**.

1. **Benzetimkapalı** olduğundan **Off**emin olun. Ardından **Oluştur**’u seçin.

Aygıt adını tıklatın ve sonra **Bağlan'ı**seçin. **Aygıt Bağlantısı** sayfasındaki aygıt bağlantısı bilgilerini not edin - **kimlik kapsamı,** **Aygıt Kimliği**ve **Birincil anahtar**. Aygıt kodunuzu oluştururken bu değerlere ihtiyacınız vardır:

![Cihaz bağlantı bilgileri](./media/iot-central-add-environmental-sensor/device-connection.png)
