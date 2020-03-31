---
title: Azure IoT gezginini yükleyin ve kullanın | Microsoft Dokümanlar
description: Azure IoT gezgini aracını yükleyin ve IoT hub'ıma bağlı IoT Tak ve Çalıştır Önizleme aygıtlarıyla etkileşim kurmak için kullanın.
author: miagdp
ms.author: miag
ms.date: 12/27/2019
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 963421fa1ef06599448c9a4197f0d7a6ad2e142d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159192"
---
# <a name="install-and-use-azure-iot-explorer"></a>Azure IoT gezginini yükleme ve kullanma

Azure IoT gezgini, IoT Tak ve Çalıştır Önizleme aygıtlarınızla etkileşim kurmak ve test etmek için kullanılan bir grafik aracıdır. Aracı yerel makinenize yükledikten sonra, bir aygıta bağlanmak için kullanabilirsiniz. Aracı, aygıtın gönderdiği telemetriyi görüntülemek, aygıt özellikleriyle çalışmak ve komutları aramak için kullanabilirsiniz.

Bu makale, şunları nasıl yapacağınızı gösterir:

- Azure IoT gezgini aracını yükleyin ve yapılandırır.
- Aygıtlarınızla etkileşimkurmak ve test etmek için aracı kullanın.

## <a name="prerequisites"></a>Ön koşullar

Azure IoT gezgini aracını kullanmak için şunları yapmanız gerekir:

- Azure IoT hub'ı. [Azure CLI'yi kullanarak IoT hub'ı oluşturma](../iot-hub/iot-hub-create-using-cli.md)gibi Azure aboneliğinize IoT hub'ı eklemenin birçok yolu vardır. Azure IoT gezgini aracını çalıştırmak için IoT hub bağlantı dizesine ihtiyacınız vardır. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.
- IoT hub'ınıza kayıtlı bir aygıt. Bir aygıtı kaydetmek için aşağıdaki Azure CLI komutunu kullanabilirsiniz. Yer tutucuları `{YourIoTHubName}` değerlerinizle değiştirdiğinizden emin olun: `{YourDeviceID}`

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

## <a name="install-azure-iot-explorer"></a>Azure IoT gezginini yükleme

Azure [IoT gezgini sürümlerine](https://github.com/Azure/azure-iot-explorer/releases) gidin ve en son sürüm için varlık listesini genişletin. Uygulamanın en son sürümünü indirin ve yükleyin.

## <a name="use-azure-iot-explorer"></a>Azure IoT gezginini kullanma

Bir aygıt için, kendi aygıtınızı bağlayabilir veya örnek simüle edilmiş aygıtlarımızdan birini kullanabilirsiniz. Benzetimli aygıt [örneğini](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/iothub_client/samples) çalıştırmak için bu yönergeleri izleyin.

### <a name="connect-to-your-hub"></a>Hub'ınıza bağlanma

Azure IoT gezginini ilk çalıştırdığınızda, IoT hub'ınızın bağlantı dizesi istenir. Bağlantı dizesini ekledikten sonra **Bağlan'ı**seçin. Bağlantı dizesini güncelleyerek başka bir IoT hub'ına geçmek için aracın ayarlarını kullanabilirsiniz.

IoT Tak ve Çalıştır aygıtının model tanımı, genel depoda, şirket deposunda veya bağlı aygıtınızda depolanır. Varsayılan olarak, araç ortak model deposunda ve bağlı aygıtınızda model tanımınızı arar. Kaynak ekleyebilir ve kaldırabilir veya **Ayarlar'daki**kaynakların önceliğini yapılandırabilirsiniz:

Kaynak eklemek için:

1. **Ayarlar'a**gidin.
1. **Yeni'yi** seçin ve kaynağınızı seçin.
1. Şirket model deponuzu ekliyorsanız, bağlantı dizesini sağlayın.

Bir kaynağı kaldırmak için:

1. **Ayarlar'a**gidin.
1. Kaldırmak istediğiniz kaynağı bulun.
1. Kaldırmak için **X'i** seçin. Ortak arabirim tanımları bu depodan geldiğinden, ortak model deposunu kaldıramazsınız.

Kaynak önceliklerini değiştirin:

Model tanım kaynaklarından birini listedeki farklı bir sıralamaya sürükleyip bırakabilirsiniz. Bir çakışma varsa, daha yüksek sıralamalara sahip tanım kaynakları daha düşük sıralamalara sahip kaynakları geçersiz kılar.

### <a name="view-devices"></a>Cihazları görüntüleme

Araç IoT hub'ınıza bağlandıktan sonra, IoT hub'ınıza kayıtlı aygıt kimliklerini listeleyen **Cihazlar** listesi sayfasını görüntüler. Daha fazla bilgi görmek için listedeki herhangi bir girişi genişletebilirsiniz.

**Cihazlar** listesi sayfasında şunları yapabilirsiniz:

- Hub'ınıza yeni bir aygıt kaydettirmek için **Ekle'yi** seçin. Ardından bir aygıt kimliği girin. Kimlik doğrulama anahtarlarını otomatik olarak oluşturmak ve hub'ınıza bağlantıyı etkinleştirmek için varsayılan ayarları kullanın.
- Bir aygıt seçin ve ardından aygıt kimliğini silmek için **Sil'i** seçin. Doğru aygıt kimliğini silediğinizden emin olmak için bu eylemi tamamlamadan önce aygıt ayrıntılarını gözden geçirin.
- Tarafından `capabilityID` sorgula ve `interfaceID`. Aygıtlarınızı `capabilityID` sorgulamak için parametrenizi veya `interfaceID` parametrenizi ekleyin.

## <a name="interact-with-a-device"></a>Bir aygıtla etkileşim

**Cihazlar** listesi sayfasında, kayıtlı aygıtın ayrıntı sayfasını görüntülemek için **Aygıt Kimliği** sütunundaki bir değer seçin. Her cihaz için iki bölüm vardır: **Aygıt** ve **Dijital İkiz**.

### <a name="device"></a>Cihaz

Bu bölümde **Aygıt Kimliği,** **Aygıt İkiz,** **Telemetri,** **Doğrudan yöntem** ve **Buluttan cihaza ileti** sekmeleri bulunur.

- **Aygıt kimlik** sekmesinde [aygıt kimlik](../iot-hub/iot-hub-devguide-identity-registry.md) bilgilerini görüntüleyebilir ve güncelleştirebilirsiniz.
- **Aygıt İkiz** sekmesinden [aygıta ikiz](../iot-hub/iot-hub-devguide-device-twins.md) bilgilere erişebilirsiniz.
- Bir aygıt bağlıysa ve etkin olarak veri gönderiyorsa, **Telemetri** sekmesinde [telemetriyi](../iot-hub/iot-hub-devguide-messages-read-builtin.md) görüntüleyebilirsiniz.
- **Doğrudan yöntem** sekmesinde cihazda doğrudan bir [yöntem](../iot-hub/iot-hub-devguide-direct-methods.md) çağırabilirsiniz.
- **Buluttan cihaza iletiler** sekmesinde [buluttan cihaza ileti](../iot-hub/iot-hub-devguide-messages-c2d.md) gönderebilirsiniz.

### <a name="digital-twin"></a>Dijital ikiz

Aracı, aygıtın dijital ikiz örneğini görüntülemek için kullanabilirsiniz. Bir IoT Tak ve Çalıştır aygıtı için, aygıt yetenek modeliyle ilişkili tüm arabirimler aracın bu bölümünde görüntülenir. İlgili [IoT Tak'ını genişletmek ve ilkelleri oynatmak](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL)için bir arayüz seçin.

### <a name="interface"></a>Arabirim

**Arabirim** sayfasında, arabirimin JSON tanımını görüntüleyebilirsiniz.

#### <a name="properties"></a>Özellikler

**Yazılamaz özellikler** sayfasında bir arabirimde tanımlanan salt okunur özelliklerini görüntüleyebilirsiniz. **Yazılabilir özellikler** sayfasındaki bir arabirimde tanımlanan yazılabilir özellikleri güncelleştirebilirsiniz:

1. **Writable özellikleri** sayfasına gidin.
1. Güncelleştirmek istediğiniz özelliği tıklatın.
1. Özellik için yeni değeri girin.
1. Aygıta gönderilmek üzere yükü önizleyin.
1. Değişikliği gönderin.

Bir değişiklik gönderdikten sonra güncelleştirme durumunu izleyebilirsiniz: **synching,** **başarı**veya **hata**. Sözleç tamamlandığında, **Bildirilen Özellik** sütununda mülkünüzün yeni değerini görürsünüz. Synching tamamlanmadan önce diğer sayfalara gidin, araç güncelleştirme tamamlandığında yine de sizi haber verirken. Bildirim geçmişini görmek için aracın bildirim merkezini de kullanabilirsiniz.

#### <a name="commands"></a>Komutlar

Bir aygıta komut göndermek için **Komutlar** sayfasına gidin:

1. Komutlar listesinde, tetiklemek istediğiniz komutu genişletin.
1. Komut için gerekli değerleri girin.
1. Aygıta gönderilmek üzere yükü önizleyin.
1. Komutu gönderin.

#### <a name="telemetry"></a>Telemetri

Seçili arabirimin telemetrisini görüntülemek için **Telemetri** sayfasına gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılabilir makalesinde, IoT Tak ve Çalıştır aygıtlarınizle etkileşim kurmak için Azure IoT gezginini nasıl yükleyip kullanacağınızı öğrendiniz. Önerilen bir sonraki adım, [Azure CLI uzantısını nasıl yükleyip kullanacağınızı](./howto-install-pnp-cli.md)öğrenmektir.
