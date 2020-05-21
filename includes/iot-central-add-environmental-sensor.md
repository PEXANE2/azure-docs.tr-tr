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
ms.openlocfilehash: 9717c76b42a63479c77f862057bfb141954eacff
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83673416"
---
## <a name="create-a-device-template"></a>Cihaz şablonu oluşturma

Yerel makinenizde adlı bir klasör oluşturun `environmental-sensor` .

[Ortam algılayıcısı yetenek modeli](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/9004219bff1e958b7cd6ff2a52209f4b7ae19396/samples/EnvironmentalSensorInline.capabilitymodel.json) json dosyasını indirin ve `environmental-sensor` klasöre kaydedin.

`{YOUR_COMPANY_NAME_HERE}`İndirdiğiniz dosyada şirket adınızla iki örneğini değiştirmek için bir metin düzenleyicisi kullanın `EnvironmentalSensorInline.capabilitymodel.json` .

Azure IoT Central uygulamanızda, cihaz yetenek modeli dosyasını içeri aktararak *çevresel algılayıcı* adlı bir cihaz şablonu oluşturun `EnvironmentalSensorInline.capabilitymodel.json` :

![İçeri aktarılan cihaz özelliği modeliyle cihaz şablonu](./media/iot-central-add-environmental-sensor/device-template.png)

Cihaz yetenek modeli iki arabirim içerir: standart **cihaz bilgileri** arabirimi ve özel **çevresel algılayıcı** arabirimi. **Çevresel algılayıcı** arabirimi aşağıdaki özellikleri tanımlar:

| Tür | Görünen Ad | Açıklama |
| ---- | ------------ | ----------- |
| Özellik | Cihaz Durumu     | Cihazın durumu. Çevrimiçi/çevrimdışı iki durum kullanılabilir. |
| Özellik (yazılabilir) | Müşteri Adı    | Müşterinin Şu anda cihazda çalışan adı. |
| Özellik (yazılabilir) | Parlaklık düzeyi | Cihazdaki ışığın parlaklık düzeyi. 1 (yüksek), 2 (orta), 3 (düşük) olarak belirtilebilir. |
| Telemetri | Sıcaklık | Cihaz tarafından algılanan geçerli sıcaklık. |
| Telemetri | Nem oranı    | Cihaz tarafından algılanan geçerli nem. |
| Komut | /          | Belirli bir zaman aralığı için cihazdaki yeniden yanıp sönmesini Başlat. |
| Komut | turnon         | Cihazın ıŞıĞıNı açın. |
| Komut | kapatma        | Cihazdaki ışığı devre dışı bırakın. |
| Komut | rundiagnostics | Bu zaman uyumsuz komut, cihazda bir tanılama çalıştırması başlatır. |

**Cihaz durumu** özelliğinin IoT Central uygulamanızda nasıl görüntüleneceğini özelleştirmek için cihaz şablonunda **Özelleştir** ' i seçin. **Cihaz durumu** girişini genişletin, **doğru** ad olarak _çevrimiçi_ ve **yanlış ad**olarak _çevrimdışı_ girin. Sonra değişiklikleri kaydedin:

![Cihaz şablonunu özelleştirme](./media/iot-central-add-environmental-sensor/customize-template.png)

## <a name="create-views"></a>Görünümler oluşturma

Görünümler, IoT Central uygulamanıza bağlı cihazlarla etkileşime geçmesini sağlar. Örneğin, Telemetriyi görüntüleyen görünümleriniz, özellikleri görüntüleyen görünümler ve yazılabilir ve bulut özelliklerini düzenlemenize olanak sağlayan görünümler bulunabilir. Görünümler bir cihaz şablonunun parçasıdır.

**Ortam algılayıcı** cihaz şablonunuza bazı varsayılan görünümler eklemek için cihaz şablonunuza gidin, **Görünümler**' i seçin ve **varsayılan görünümleri oluştur** kutucuğunu seçin. **Genel bakış** ve **hakkında** **olduğundan emin**olun ve ardından **varsayılan pano görünüm oluştur**' u seçin. Artık, şablonunuzda tanımlanmış iki varsayılan görünümünüz vardır.

**Çevresel algılayıcı** arabirimi iki yazılabilir Özellik Içerir- **müşteri adı** ve **parlaklık düzeyi**. Bir görünüm oluşturmak için bu özellikleri düzenlemek üzere kullanabilirsiniz:

1. **Görünümler** ' i seçin ve ardından **cihazı ve bulut verilerini Düzenle** kutucuğunu seçin.

1. _Özellikleri_ form adı olarak girin.

1. **Parlaklık düzeyini** ve **müşteri adı** özelliklerini seçin. Ardından **Bölüm Ekle**' yi seçin.

1. Yaptığınız değişiklikleri kaydedin.

![Özellik düzenlemesini etkinleştirmek için bir görünüm ekleyin](./media/iot-central-add-environmental-sensor/properties-view.png)

## <a name="publish-the-template"></a>Şablonu yayımlama

**Ortam algılayıcısı** cihaz şablonunu kullanan bir cihaz ekleyebilmeniz için önce onu yayımlamanız gerekir.

Cihaz şablonunda **Yayımla**' yı seçin. **Bu cihaz şablonunu uygulama panelinde Yayımla** ' da, **Yayımla**' yı seçin.

Şablonun kullanıma hazırsa emin olmak için IoT Central uygulamanızdaki **cihazlar** sayfasına gidin. **Cihazlar** bölümü, uygulamadaki yayınlanan cihazların bir listesini gösterir:

![Cihazlar sayfasında yayımlanan şablonlar](./media/iot-central-add-environmental-sensor/published-templates.png)

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IoT Central uygulamanızda, önceki bölümde oluşturduğunuz cihaz şablonuna gerçek bir cihaz ekleyin:

1. **Cihazlar** sayfasında, **çevresel algılayıcı** cihaz şablonunu seçin.

    > [!TIP]
    > **+ Yeni**' yi seçmeden önce kullanılacak şablonu seçtiğinizden emin olun, aksi takdirde ilişkilendirilmemiş bir cihaz oluşturacaksınız.

1. **+ Yeni**seçeneğini belirleyin.

1. **Benzetimli** 'un **kapalı**olduğundan emin olun. Ardından **Oluştur**’u seçin.

Cihaz adına tıklayın ve ardından **Bağlan**' ı seçin. Cihaz **bağlantısı** sayfa **kimliği KAPSAMı**, **cihaz kimliği**ve **birincil anahtar**üzerindeki cihaz bağlantı bilgilerini bir yere unutmayın. Cihaz kodunuzu oluştururken bu değerlere ihtiyacınız vardır:

![Cihaz bağlantı bilgileri](./media/iot-central-add-environmental-sensor/device-connection.png)
