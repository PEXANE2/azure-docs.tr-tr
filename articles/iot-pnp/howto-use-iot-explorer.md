---
title: Azure IoT Gezginini yüklemeyi ve kullanmayı | Microsoft Docs
description: Azure IoT gezgin aracını yükleyip IoT Hub 'ına bağlı IoT Tak ve Kullan önizleme cihazlarıyla etkileşim kurmak için bu aracı kullanın.
author: rido-min
ms.author: rmpablos
ms.date: 05/06/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 083dcde44e56af34f17d952c46e554b234818f27
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352368"
---
# <a name="install-and-use-azure-iot-explorer"></a>Azure IoT Gezginini yükleyip kullanma

Azure IoT Explorer, IoT Tak ve Kullan önizleme cihazlarınızı etkileşimli ve test etmeye yönelik bir grafik aracıdır. Aracı yerel makinenize yükledikten sonra, bir hub 'a bağlanmak için kullanabilirsiniz. Aracı, cihazların gönderdiği Telemetriyi görüntülemek, cihaz özellikleriyle çalışmak ve komutları çağırmak için kullanabilirsiniz.

Bu makale, şunları nasıl yapacağınızı gösterir:

- Azure IoT gezgin aracını yükleyip yapılandırın.
- Cihazlarınızla etkileşim kurmak ve bunları test etmek için aracını kullanın.

## <a name="prerequisites"></a>Önkoşullar

Azure IoT gezgin aracını kullanmak için şunlar gerekir:

- Azure IoT Hub 'ı. Azure aboneliğinize Azure [CLI kullanarak](../iot-hub/iot-hub-create-using-cli.md)IoT Hub 'ı oluşturma gibi bir IoT Hub 'ı eklemenin birçok yolu vardır. Azure IoT gezgin aracını çalıştırmak için IoT Hub bağlantı dizesine ihtiyacınız vardır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- IoT Hub 'ınıza kayıtlı bir cihaz. Bir cihazı kaydetmek için aşağıdaki Azure CLı komutunu kullanabilirsiniz. `{YourIoTHubName}`Ve `{YourDeviceID}` yer tutucuları değerlerinizle değiştirdiğinizden emin olun:

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

## <a name="install-azure-iot-explorer"></a>Azure IoT Explorer 'ı yükler

[Azure IoT Explorer sürümleri](https://github.com/Azure/azure-iot-explorer/releases) ' ne gidin ve en son sürümün varlık listesini genişletin. Uygulamanın en son sürümünü indirip yükleyin.

>[!Important]
>0.11.0 IoT Explorer sürümü yalnızca IoT Tak ve Kullan Mayıs 2020 Preview-Release ' i destekler. Önceki önizlemede bulunan özellikleri kullanmak için, Ağustos 2019 ' den yayın sürümünde bir 0.10. x sürümü yüklemelisiniz.

## <a name="use-azure-iot-explorer"></a>Azure IoT Gezginini Kullanma

Bir cihaz için kendi cihazınızı bağlanabilir veya örnek sanal cihazlarımızın birini kullanabilirsiniz. Sanal cihaz örneğini çalıştırmak için [Bu yönergeleri](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/iothub_client/samples) izleyin.

### <a name="connect-to-your-hub"></a>Hub 'ınıza bağlanın

Azure IoT Gezginini ilk kez çalıştırdığınızda IoT Hub 'ınızın bağlantı dizesi istenir. Bağlantı dizesini ekledikten sonra **Bağlan**' ı seçin. Bağlantı dizesini güncelleştirerek, aracın ayarlarını başka bir IoT Hub 'ına geçiş yapmak için kullanabilirsiniz.

IoT Tak ve Kullan cihazının model tanımı, genel depoda, bağlı cihazda veya yerel bir klasörde depolanır. Varsayılan olarak, araç genel depoda ve bağlı cihazınızda model tanımınızı arar. Kaynak ekleyebilir ve kaldırabilir veya **Ayarlar**' da kaynakların önceliğini yapılandırabilirsiniz:

Kaynak eklemek için:

1. **Giriş/ıot Tak ve Kullan ayarlarına** git
2. **Ekle** ' yi seçin ve bir depodan veya yerel klasörden kaynağınızı seçin.

Bir kaynağı kaldırmak için:

1. **Giriş/ıot Tak ve Kullan ayarlarına** git
2. Kaldırmak istediğiniz kaynağı bulun.
3. **X** seçimini kaldırmak için seçin.

Kaynak önceliklerini değiştirin:

Model tanımı kaynaklarından birini, listedeki farklı bir derecelendirmeye sürükleyip bırakabilirsiniz. 

### <a name="view-devices"></a>Cihazları görüntüleme

Araç IoT Hub 'ınıza bağlandıktan sonra, IoT Hub 'ınıza kayıtlı cihaz kimliklerini listeleyen **cihazlar** listesi sayfasını görüntüler. Daha fazla bilgi için listedeki herhangi bir girişi seçebilirsiniz.

**Cihazlar** listesi sayfasında şunları yapabilirsiniz:

- Hub 'ınıza yeni bir cihaz kaydetmek için **Yeni** ' yi seçin. Ardından bir cihaz KIMLIĞI girin. Otomatik olarak kimlik doğrulama anahtarları oluşturmak ve hub 'ınız ile bağlantıyı etkinleştirmek için varsayılan ayarları kullanın.
- Bir cihaz seçin ve ardından bir cihaz kimliğini silmek için **Sil** ' i seçin. Doğru cihaz kimliğini sildiğinizden emin olmak için bu eylemi tamamlamadan önce cihaz ayrıntılarını gözden geçirin.

## <a name="interact-with-a-device"></a>Cihazla etkileşim kurma

**CIHAZ** **kimliği** sütununda, kayıtlı cihazın ayrıntı sayfasını görüntülemek için bir değer seçin. Her bir cihaz için iki bölüm vardır: **Device** ve **Digital ikizi**.

### <a name="device"></a>Cihaz

Bu bölümde **cihaz kimliği**, **cihaz ikizi**, **telemetri**, **doğrudan yöntem**, **buluttan cihaza ileti**, **modül kimliği** sekmeleri bulunur.

- [Cihaz kimlik bilgilerini](../iot-hub/iot-hub-devguide-identity-registry.md) **cihaz kimliği** sekmesinden görüntüleyebilir ve güncelleştirebilirsiniz.
- Device [ikizi](../iot-hub/iot-hub-devguide-device-twins.md) bilgilerine **Device ikizi** sekmesinden erişebilirsiniz.
- Bir cihaz bağlıysa ve etkin bir şekilde veri [gönderiyorsanız telemetri '](../iot-hub/iot-hub-devguide-messages-read-builtin.md) i **telemetri** sekmesinden görüntüleyebilirsiniz.
- Doğrudan **Yöntem** sekmesinde cihazda [doğrudan yöntem](../iot-hub/iot-hub-devguide-direct-methods.md) çağırabilirsiniz.
- Buluttan cihaza **iletiler** sekmesinden [buluta cihaz iletisi](../iot-hub/iot-hub-devguide-messages-c2d.md) gönderebilirsiniz.
- [Module ikizi](../iot-hub/iot-hub-devguide-module-twins.md) bilgilerine erişebilirsiniz.

### <a name="iot-plug-and-play-components"></a>IoT Tak ve Kullan bileşenleri

Cihaz hub 'a bir **model kimliği**kullanarak bağlıysa araç, **model kimliğini**görebileceğiniz ' ' **IoT Tak ve kullan bileşenleri** sekmesini gösterir.

**Model kimliği** yapılandırılmış kaynaklardan birinde, genel depolarınızda veya yerel klasöründen kullanılabiliyorsa, bileşen listesi görüntülenir. Bir bileşeni seçtiğinizde, kullanılabilen özellikler, komutlar ve telemetri gösterilir.

**Bileşen** sayfasında salt okuma özelliklerini görüntüleyebilir, yazılabilir özellikleri güncelleştirebilir, komutları çağırabilir ve bu bileşen tarafından üretilen telemetri iletilerini görebilirsiniz.

:::image type="content" source="media/howto-use-iot-explorer/components-iot-explorer.png" alt-text="Azure IoT Explorer 'da bileşenleri görüntüleme":::

#### <a name="properties"></a>Özellikler

:::image type="content" source="media/howto-use-iot-explorer/properties-iot-explorer.png" alt-text="Azure IoT Explorer 'da özellikleri görüntüleme":::

**Özellikler (salt okuma)** sekmesindeki bir arabirimde tanımlanmış salt okuma özelliklerini görüntüleyebilirsiniz. **Özellikler (yazılabilir)** sekmesindeki bir arabirimde tanımlanan yazılabilir özellikleri güncelleştirebilirsiniz:

1. **Özellikler (yazılabilir)** sekmesine gidin.
1. Güncelleştirmek istediğiniz özelliğe tıklayın.
1. Özellik için yeni bir değer girin.
1. Cihaza gönderilecek yükün önizlemesini görüntüleyin.
1. Değişikliği gönder.

Bir değişiklik gönderdikten sonra, güncelleştirme durumunu takip edebilirsiniz: **eşitleniyor**, **başarılı**veya **hata**. Eşitleme tamamlandığında, **bildirilen özellik** sütununda, özelliğinin yeni değerini görürsünüz. Eşitleme tamamlanmadan önce diğer sayfalara gittiğinizde, güncelleştirme tamamlandığında araç sizi size bildirir. Ayrıca, bildirim geçmişini görmek için aracın bildirim merkezini de kullanabilirsiniz.

#### <a name="commands"></a>Komutlar

Bir cihaza komut göndermek için, **Komutlar** sekmesine gidin:

1. Komut listesinde, tetiklemek istediğiniz komutu genişletin.
1. Komut için gerekli değerleri girin.
1. Cihaza gönderilecek yükün önizlemesini görüntüleyin.
1. Komutunu gönder.

#### <a name="telemetry"></a>Telemetri

Seçili arabirime yönelik Telemetriyi görüntülemek için **telemetri** sekmesine gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde IoT Tak ve Kullan cihazlarınızla etkileşim kurmak için Azure IoT Gezginini yüklemeyi ve kullanmayı öğrendiniz. Önerilen bir sonraki adım, [Azure CLI ıot Tak ve kullan komutlarının](./howto-use-iot-pnp-cli.md)nasıl kullanılacağını öğrenirsiniz.
