---
title: IoT Tak ve Kullan köprüsü oluşturma, dağıtma ve genişletme | Microsoft Docs
description: IoT Tak ve Kullan köprü bileşenlerini belirler. Köprüyü genişletmeyi ve IoT cihazlarında, ağ geçitlerinde ve bir IoT Edge modülü olarak nasıl çalıştırılacağını öğrenin.
author: usivagna
ms.author: ugans
ms.date: 12/11/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: ece9f62e64eb64b1f34af46b42d57ec583f8f214
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97675938"
---
# <a name="build-deploy-and-extend-the-iot-plug-and-play-bridge"></a>IoT Tak ve Kullan köprüsü oluşturma, dağıtma ve genişletme

IoT Tak ve Kullan Köprüsü, bir ağ geçidine bağlı mevcut Cihazları IoT Hub 'ınıza bağlamanıza olanak tanır. IoT Tak ve Kullan arabirimlerini iliştirilmiş cihazlara eşlemek için köprü kullanırsınız. IoT Tak ve Kullan arabirimi, bir cihazın gönderdiği Telemetriyi, cihaz ile bulut arasında eşitlenen özellikleri ve cihazın yanıt verdiği komutları tanımlar. Açık kaynaklı köprü uygulamasını Windows veya Linux ağ geçitlerine yükleyebilir ve yapılandırabilirsiniz.

Bu makalede nasıl yapılacağını ayrıntılı olarak açıklanmaktadır:

- Köprü yapılandırın.
- Yeni bağdaştırıcılar oluşturarak bir köprüyü genişletin.
- Farklı ortamlarda köprü oluşturma ve çalıştırma.

Köprünün nasıl kullanılacağını gösteren basit bir örnek için bkz. [Linux veya Windows üzerinde çalışan ıot Tak ve Kullan köprüsü örneğini IoT Hub olarak bağlama](howto-use-iot-pnp-bridge.md).

Bu makaledeki kılavuz ve örnekler, [Azure dijital TWINS](../digital-twins/overview.md) ve [IoT Tak ve kullan](overview-iot-plug-and-play.md)temel duyumuzu varsayar.

## <a name="general-prerequisites"></a>Genel önkoşullar

### <a name="supported-os-platforms"></a>Desteklenen işletim sistemi platformları

Aşağıdaki işletim sistemi platformları ve sürümleri desteklenir:

|Platform  |Desteklenen Sürümler  |
|---------|---------|
|Windows 10 |     Tüm Windows SKU 'Ları desteklenir. Örneğin: IoT Enterprise, Server, Desktop, IoT Core. *Kamera sistem durumu izleme işlevselliği için 20H1 veya üzeri derleme önerilir. Tüm Windows 10 Derlemeleriyle tüm diğer işlevler kullanılabilir.*  |
|Linux     |Ubuntu 18,04 üzerinde sınanmış ve desteklenmiş, diğer dağıtımların işlevselliği sınanmamıştır.         |
||

### <a name="hardware"></a>Donanım

- Yukarıdaki işletim sistemi SKU 'Larını ve sürümlerini destekleyen herhangi bir donanım platformu.
- Seri, USB, Bluetooth ve kamera çevreörleri ve algılayıcılar yerel olarak desteklenir. IoT Tak ve Kullan Köprüsü, herhangi bir özel çevre birimi veya algılayıcısı destekleyecek şekilde genişletilebilir.

### <a name="azure-iot-products-and-tools"></a>Azure IoT ürünleri ve araçları

- **Azure IoT Hub** -Cihazınızı bağlamak için Azure aboneliğinizde bir [Azure IoT Hub](../iot-hub/index.yml) 'ınız olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/). Bir IoT Hub 'ınız yoksa, [oluşturmak için bu yönergeleri izleyin](../iot-hub/iot-hub-create-using-cli.md). IoT Tak ve Kullan desteği temel katman IoT Hub 'larına dahil değildir.
- **Azure IoT Explorer** -IoT Tak ve kullan cihazlarınızla etkileşim kurmak Için Azure IoT gezgin aracını kullanabilirsiniz. İşletim sisteminiz için [Azure IoT Explorer 'ın en son sürümünü indirin ve yükleyin](./howto-use-iot-explorer.md) . Azure IoT gezgin aracını IoT Hub 'ınıza bağlanacak şekilde yapılandırın ve kopyaladığınız **IoT-plug ve-Play-Bridge** içindeki *pnpbridge\docs\schemas* klasöründe model tanımlarını arayın.

## <a name="configure-a-bridge"></a>Köprü yapılandırma

Köprü yapılandırmak için iki yol vardır:

- Köprü bir IoT cihazında veya ağ geçidinde yerel olarak çalışıyorsa, yapılandırma dosyasını kullanın. Bu senaryo, bir Linux veya Windows makinesi kullanabilir.
- Köprü, IoT Edge çalışma zamanında bir IoT Edge modülü olarak çalışıyorsa, Module ikizi Desired özelliğini kullanın. Bu senaryoda IoT Edge çalışma zamanının bir Linux ortamında barındırıldığı varsayılır.

### <a name="configuration-file"></a>Yapılandırma dosyası

Bir IoT cihazında veya ağ geçidinde Tak ve Kullan köprüsü yerel olarak çalıştığında, aşağıdakileri yapmak için bir yapılandırma dosyası kullanır:

- IoT Central veya IoT Hub bağlantı bilgilerini alın.
- IoT Tak ve Kullan arabirimlerinin yayımlandığı cihazları yapılandırın.

Yapılandırma dosyasını köprüye sağlamak için aşağıdaki seçeneklerden birini kullanın:

- Yapılandırma dosyasının yolunu köprü yürütülebilir dosyasına bir komut satırı parametresi olarak geçirin.
- *Pnpköprü\cmake\ pnpbridge_x86 \src\pnpköprü\samples\console* klasöründe var olan *config.js* dosyasını kullanın.

[Yapılandırma dosyası şeması](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/src/pnpbridge/src/pnpbridge_config_schema.json) GitHub deposunda mevcuttur.

> [!TIP]
> VS Code ' de bir köprü yapılandırma dosyası düzenlerseniz, dosyayı doğrulamak için bu şema dosyasını kullanabilirsiniz.

### <a name="iot-edge-module-configuration"></a>IoT Edge modülü yapılandırması

Köprü, bir IoT Edge çalışma zamanı üzerinde IoT Edge modülü olarak çalıştırıldığında, yapılandırma dosyası buluttan istenen özelliğe yönelik bir güncelleştirme olarak gönderilir `PnpBridgeConfig` . Köprü, bağdaştırıcılar ve bileşenleri yapılandırmadan önce bu özellik güncelleştirmesini bekler.

## <a name="extend-the-bridge"></a>Köprüyü genişletme

Köprünün yeteneklerini genişletmek için kendi köprü bağdaştırıcılarınızı yazabilirsiniz.

Köprü bağdaştırıcıları şu amaçlarla kullanır:

- Bir cihaz ile bulut arasında bağlantı kurun.
- Bir cihaz ile bulut arasında veri akışını etkinleştirin.
- Cihaz yönetimini buluttan etkinleştirin.

Her köprü bağdaştırıcısında şunları yapmanız gerekir:

- Dijital bir TWINS arabirimi oluşturun.
- Cihaz tarafı işlevselliğini telemetri, Özellikler ve komutlar gibi bulut tabanlı yeteneklere bağlamak için arabirimini kullanın.
- Cihaz donanımı veya üretici yazılımıyla denetim ve veri iletişimi kurun.

Her köprü bağdaştırıcısı, bağdaştırıcının cihaza bağlanıp cihazla etkileşime girdiği belirli bir cihaz türü ile etkileşime girer. Bir cihazla iletişim, bir anlaşma Protokolü kullandığından bile, bir köprü bağdaştırıcısında verileri cihazdan yorumlayacağınız birden çok yol olabilir. Bu senaryoda, Köprü bağdaştırıcısı bağdaştırıcının verileri ayrıştırmak için kullanması gereken *arabirim yapılandırmasını* belirleyebilmek için yapılandırma dosyasındaki bağdaştırıcının bilgilerini kullanır.

Bir köprü bağdaştırıcısı, cihazla etkileşim kurmak için cihaz ve API 'Ler tarafından desteklenen bir iletişim protokolü ve temel alınan işletim sistemi ya da cihaz satıcısı tarafından kullanılır.

Bir köprü bağdaştırıcısı, bulutla etkileşimde bulunmak için Azure IoT cihaz C SDK 'Sı tarafından telemetri göndermek, dijital ikizi arabirimleri oluşturmak, özellik güncelleştirmeleri göndermek ve özellik güncelleştirmeleri ve komutları için geri çağırma işlevleri oluşturmak için tarafından sunulan API 'Leri kullanır.

### <a name="create-a-bridge-adapter"></a>Köprü bağdaştırıcısı oluşturma

Köprü, [_PNP_ADAPTER](https://github.com/Azure/iot-plug-and-play-bridge/blob/9964f7f9f77ecbf4db3b60960b69af57fd83a871/pnpbridge/src/pnpbridge/inc/pnpadapter_api.h#L296) arabiriminde tanımlanan API 'leri uygulamak için bir köprü bağdaştırıcısı bekler:

```c
typedef struct _PNP_ADAPTER {
  // Identity of the IoT Plug and Play adapter that is retrieved from the config
  const char* identity;

  PNPBRIDGE_ADAPTER_CREATE createAdapter;
  PNPBRIDGE_COMPONENT_CREATE createPnpComponent;
  PNPBRIDGE_COMPONENT_START startPnpComponent;
  PNPBRIDGE_COMPONENT_STOP stopPnpComponent;
  PNPBRIDGE_COMPONENT_DESTROY destroyPnpComponent;
  PNPBRIDGE_ADAPTER_DESTOY destroyAdapter;
} PNP_ADAPTER, * PPNP_ADAPTER;
```

Bu arabirimde:

- `PNPBRIDGE_ADAPTER_CREATE` bağdaştırıcıyı oluşturur ve arabirim Yönetimi kaynaklarını ayarlar. Bağdaştırıcı ayrıca bağdaştırıcı oluşturmak için genel bağdaştırıcı parametrelerine de sahip olabilir. Bu işlev, tek bir bağdaştırıcı için bir kez çağrılır.
- `PNPBRIDGE_COMPONENT_CREATE` Dijital ikizi istemci arabirimlerini oluşturur ve geri çağırma işlevlerini bağlar. Bağdaştırıcı, cihaza iletişim kanalını başlatır. Bağdaştırıcı, telemetri akışını etkinleştirmek için kaynakları ayarlayabilir, ancak çağrılana kadar raporlama telemetrisi başlatamaz `PNPBRIDGE_COMPONENT_START` . Bu işlev, yapılandırma dosyasındaki her arabirim bileşeni için bir kez çağrılır.
- `PNPBRIDGE_COMPONENT_START` Köprü bağdaştırıcısının, cihazdan dijital ikizi istemcisine telemetri iletmeye başlamasını sağlamak için çağrılır. Bu işlev, yapılandırma dosyasındaki her arabirim bileşeni için bir kez çağrılır.
- `PNPBRIDGE_COMPONENT_STOP` Telemetri akışını sonlandırır.
- `PNPBRIDGE_COMPONENT_DESTROY` Digital ikizi istemcisini ve ilişkili arabirim kaynaklarını yok eder. Bu işlev, köprü bozuk olduğunda veya önemli bir hata oluştuğunda yapılandırma dosyasındaki her arabirim bileşeni için bir kez çağrılır.
- `PNPBRIDGE_ADAPTER_DESTROY` Köprü bağdaştırıcısı kaynaklarını temizler.

### <a name="bridge-core-interaction-with-bridge-adapters"></a>Köprü bağdaştırıcıları ile Köprü çekirdek etkileşimi

Aşağıdaki listede köprü başlatıldığında ne olacağı özetlenmektedir.

1. Köprü başlatıldığında, köprü bağdaştırıcı Yöneticisi yapılandırma dosyasında tanımlanan her arabirim bileşenini arar ve `PNPBRIDGE_ADAPTER_CREATE` uygun bağdaştırıcı üzerinde çağırır. Bağdaştırıcı, çeşitli *arabirim yapılandırmalarını* destekleyecek kaynakları ayarlamak için genel bağdaştırıcı yapılandırma parametrelerini kullanabilir.
1. Yapılandırma dosyasındaki her cihaz için, köprü Yöneticisi uygun köprü bağdaştırıcısını çağırarak arabirim oluşturmayı başlatır `PNPBRIDGE_COMPONENT_CREATE` .
1. Bağdaştırıcı, arabirim bileşeni için tüm isteğe bağlı bağdaştırıcı yapılandırma ayarlarını alır ve bu bilgileri cihazla bağlantı kurmak için kullanır.
1. Bağdaştırıcı, dijital ikizi istemci arabirimlerini oluşturur ve özellik güncelleştirmeleri ve komutları için geri çağırma işlevlerini bağlar. Cihaz bağlantıları kurulması, dijital ikizi arabirimi oluşturma başarılı olduktan sonra geri çağırmaların döndürülmesini engellemez. Etkin cihaz bağlantısı, köprünün oluşturduğu etkin arabirim istemcisinden bağımsızdır. Bağlantı başarısız olursa, bağdaştırıcı cihazın etkin olmadığını varsayar. Köprü bağdaştırıcısı bu bağlantıyı yapmayı yeniden denemeyi tercih edebilir.
1. Köprü bağdaştırıcısı Yöneticisi, yapılandırma dosyasında belirtilen tüm arabirim bileşenlerini oluşturduktan sonra, tüm arabirimleri Azure IoT Hub kaydeder. Kayıt, engelleyici, zaman uyumsuz bir çağrıdır. Arama tamamlandığında, köprü bağdaştırıcısında, buluttan özellik ve komut geri çağırmaları işlemeye başlayabileceğiniz bir geri çağırma tetikler.
1. Daha sonra köprü bağdaştırıcısı Yöneticisi `PNPBRIDGE_INTERFACE_START` her bileşeni çağırır ve Köprü bağdaştırıcısı, dijital ikizi istemcisine rapor Telemetriyi başlatır.

### <a name="design-guidelines"></a>Tasarım yönergeleri

Yeni bir köprü bağdaştırıcısı geliştirirken aşağıdaki yönergeleri izleyin:

- Hangi cihaz yeteneklerinin desteklendiğini ve bu bağdaştırıcıyı kullanan bileşenlerin arabirim tanımının nasıl göründüğünü saptayın.
- Bağdaştırıcınızda yapılandırma dosyasında tanımlanan arabirim ve genel parametreleri saptayın.
- Bileşen özelliklerini ve komutlarını desteklemek için gereken alt düzey cihaz iletişimini belirler.
- Bağdaştırıcının ham verileri cihazdan nasıl ayrıştıracağını ve IoT Tak ve Kullan arabirim tanımının belirttiği telemetri türlerine nasıl dönüştürüleceğini belirler.
- Daha önce açıklanan Köprü bağdaştırıcısı arabirimini uygulayın.
- Yeni bağdaştırıcıyı bağdaştırıcı bildirimine ekleyin ve köprü oluşturun.

### <a name="enable-a-new-bridge-adapter"></a>Yeni bir köprü bağdaştırıcısını etkinleştir

[Adapter_manifest. c](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/src/adapters/src/shared/adapter_manifest.c)' de bir başvuru ekleyerek, köprüdeki bağdaştırıcıları etkinleştirebilirsiniz:

```c
  extern PNP_ADAPTER MyPnpAdapter;
  PPNP_ADAPTER PNP_ADAPTER_MANIFEST[] = {
    .
    .
    &MyPnpAdapter
  }
```

> [!IMPORTANT]
> Köprü bağdaştırıcısı geri çağırmaları ardışık olarak çağrılır. Bu, köprü çekirdeğin ilerleme yapmasını önlediği için, bir bağdaştırıcı geri çağırma işlemini engelmemelidir.

### <a name="sample-camera-adapter"></a>Örnek kamera bağdaştırıcısı

[Kamera bağdaştırıcısı Benioku dosyası](https://github.com/Azure/iot-plug-and-play-bridge/blob/master/pnpbridge/src/adapters/src/Camera/readme.md) , etkinleştirebileceğiniz bir örnek kamera bağdaştırıcısı tanımlar.

## <a name="build-and-run-the-bridge-on-an-iot-device-or-gateway"></a>Bir IoT cihazında veya ağ geçidinde köprü oluşturma ve çalıştırma

| Platform | Desteklenir |
| :-----------: | :-----------: |
| Windows |  Yes |
| Linux | Yes |

### <a name="prerequisites"></a>Önkoşullar

Bu bölümü gerçekleştirmek için, yerel makinenize aşağıdaki yazılımı yüklemeniz gerekir:

- [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/) gibi c++ derlemesini destekleyen bir geliştirme ortamı-Visual Studio 'Yu yüklerken NuGet Paket Yöneticisi bileşenini ve c++ iş yüküyle **Masaüstü geliştirmeyi** eklediğinizden emin olun.
- [CMake](https://cmake.org/download/) -CMake 'i yüklediğinizde, **sistem yoluna CMake Ekle** seçeneğini belirleyin.
- Windows üzerinde oluşturuyorsanız, [windows 17763 SDK 'sını](https://developer.microsoft.com/windows/downloads/windows-10-sdk)de indirmeniz gerekir.

### <a name="source-code"></a>Kaynak kod

[Iot Tak ve kullan köprü](https://github.com/Azure/iot-plug-and-play-bridge) deposunu yerel makinenize kopyalayın:

```cmd/sh
git clone https://github.com/Azure/iot-plug-and-play-bridge.git

cd iot-plug-and-play-bridge

git submodule update --init --recursive
```

Önceki komutun çalıştırılması birkaç dakika sürer.

> [!NOTE]
> `git submodule update`Windows 'da başarısız olan sorunlarla karşılaşırsanız, sorunu çözmek için aşağıdaki komutu deneyin: `git config --system core.longpaths true` .

### <a name="build-the-bridge-on-windows"></a>Windows 'da köprü oluşturma

**VS 2019 için geliştirici komut istemi** açın ve Klonladığınız depoyu içeren klasöre gidin ve aşağıdaki komutları çalıştırın:

```cmd
cd pnpbridge\scripts\windows

build.cmd
```

Önceki komutun çalıştırılması birkaç dakika sürer.

İsteğe bağlı olarak, kodu düzenlemek, yeniden oluşturmak ve hatalarını ayıklamak için Visual Studio 'da üretilen *pnpköprü\cmake\ pnpbridge_x86 \ azure_iot_pnp_bridge. sln* çözüm dosyasını açabilirsiniz.

> [!TIP]
> Bu proje, proje dosyalarını oluşturmak için CMAKE kullanır. Visual Studio 'da projede yaptığınız tüm değişiklikler, uygun CMAKE dosyaları güncelleştirilemeyebilir kaybolabilir.

### <a name="build-the-bridge-on-linux"></a>Linux 'ta köprü oluşturma

Bash kabuğunu kullanarak, Klonladığınız depoyu içeren klasöre gidin ve aşağıdaki komutları çalıştırın:

```bash
cd scripts/linux

./setup.sh

./build.sh
```

### <a name="configure-the-generic-sensors"></a>Genel algılayıcıları yapılandırma

`pnp_bridge_connection_parameters`Windows 'daki *iot-Plug-and-Play-bridge\pnpbridge\cmake\ pnpbridge_x86 \src\pnpvbrk\, \ konsol* klasörü veya Windows 'daki *IoT-plug-ve-Play-Bridge/pnpbridge/CMake/pnpbridge_linux \ src/pnpbridge/Samples/konsol* klasörü içindeki *config.js* içindeki düğüm altında bulunan aşağıdaki parametreleri değiştirin:

IoT Hub 'ınıza bağlanmak için bir bağlantı dizesi kullanıyorsanız:

```JSON
{
  "connection_parameters": {
    "connection_type" : "connection_string",
    "connection_string" : "dtmi:com:example:RootPnpBridgeSampleDevice;1",
    "root_interface_model_id": "[To fill in]",
    "auth_parameters": {
      "auth_type": "symmetric_key",
      "symmetric_key": "[To fill in]"
    }
  }
}
```

> [!TIP]
> `symmetric_key`Değer bağlantı DIZESINDEKI SAS anahtarıyla aynı olmalıdır.

IoT Hub 'ınıza veya IoT Central uygulamasına bağlanmak için DPS kullanıyorsanız:

```JSON
{
  "connection_parameters": {
    "connection_type" : "dps",
    "root_interface_model_id": "dtmi:com:example:RootPnpBridgeSampleDevice;1",
    "auth_parameters" : {
      "auth_type" : "symmetric_key",
      "symmetric_key" : "[DEVICE KEY]"
    },
    "dps_parameters" : {
      "global_prov_uri" : "[GLOBAL PROVISIONING URI] - typically it is global.azure-devices-provisioning.net",
      "id_scope": "[IoT Central / IoT Hub ID SCOPE]",
      "device_id": "[DEVICE ID]"
    }
  }
}
```

Bu örnekte hangi arabirim bileşenlerinin ve genel parametrelerin yapılandırıldığını görmek için yapılandırma dosyasının geri kalanını gözden geçirin.

### <a name="start-the-bridge-in-windows"></a>Windows 'da köprü başlatma

Köprüyü komut isteminde çalıştırarak başlatın:

```cmd
cd iot-plug-and-play-bridge\pnpbridge\cmake\pnpbridge_x86\src\pnpbridge\samples\console

Debug\pnpbridge_bin.exe
```

> [!TIP]
> Farklı bir konumdan bir yapılandırma dosyası kullanmak için, köprüyü çalıştırdığınızda yolunu komut satırı parametresi olarak geçirin.
  
> [!TIP]
> Bilgisayarınıza bağlı bir yerleşik kameranıza veya USB kameranıza sahipseniz, yerleşik **Kamera** uygulaması gibi kamera kullanan bir uygulamayı başlatabilirsiniz. **Kamera** uygulaması çalışırken, Köprü Konsolu çıktısı, IoT Hub 'ınıza dijital ikizi arabirimi aracılığıyla bildirilen izleme istatistiklerini ve kare oranını gösterir.

## <a name="build-and-run-the-bridge-as-an-iot-edge-module"></a>Köprüyü IoT Edge modülü olarak derleyin ve çalıştırın

| Platform | Desteklenir |
| :-----------: | :-----------: |
| Windows |  No |
| Linux | Yes |

### <a name="prerequisites"></a>Önkoşullar

Bu bölümü gerçekleştirmek için ücretsiz veya Standart katmanlı bir Azure IoT Hub 'ına ihtiyacınız vardır. IoT Hub oluşturma hakkında bilgi almak için bkz. [IoT Hub oluşturma](../iot-hub/iot-hub-create-through-portal.md).

Bu bölümdeki adımlarda, bir Windows 10 makinesinde aşağıdaki geliştirme ortamının olduğunu varsaymaktadır. Bu araçlar IoT Edge cihazınıza bir IoT Edge modülü oluşturmanıza ve dağıtmanıza olanak tanır:

- Linux için Windows alt sistemi (WSL) 2 Ubuntu 18,04 LTS çalıştırıyor. Daha fazla bilgi edinmek için bkz. [Windows 10 Linux Için Windows alt sistemi yükleme kılavuzu](https://docs.microsoft.com/windows/wsl/install-win10).
- WSL 2 kullanacak şekilde yapılandırılmış Windows için Docker Desktop. Daha fazla bilgi için bkz. [Docker Desktop WSL 2 arka ucu](https://docs.docker.com/docker-for-windows/wsl/).
- Aşağıdaki üç uzantı yüklü olan [Windows ortamınızda yüklü Visual Studio Code](https://code.visualstudio.com/docs/setup/windows) :

  - [Uzaktan geliştirme Uzantı paketi](https://aka.ms/vscode-remote/download/extension) -bu uzantı, WSL 2 ' de kod oluşturmanızı ve çalıştırmanızı sağlar.
  - [Visual Studio Code Için Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) -bu uzantının vs Code **WSL: Ubuntu-18,04** ortamında etkinleştirilmesi gerekir.
  - [Visual Studio Code Için Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) -bu uzantının vs Code **WSL: Ubuntu-18,04** ortamında etkinleştirilmesi gerekir.

- Gerekli derleme araçlarını yüklemek için aşağıdaki komutları WSL 2 ortamınızda çalıştırın:

  ```bash
  sudo apt-get update
  sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
  ```

- Azure, Azure kaynaklarınızı yönetmek için WSL 2 ortamınızda yüklü olan [Azure CLI](/cli/azure/install-azure-cli-apt?view=azure-cli-latest&preserve-view=true) .

  > [!TIP]
  > Tercih ederseniz, `az` CLI 'nın önceden yüklendiği [Azure Cloud Shell](https://shell.azure.com/) komutları çalıştırabilirsiniz.

### <a name="create-an-iot-edge-device"></a>IoT Edge cihazı oluşturma

Buradaki komutlar bir Azure sanal makinesinde çalışan IoT Edge bir cihaz oluşturur. Gerçek bir cihaza erişiminiz yoksa, bir IoT Edge cihazının bir sanal makinede çalıştırılması, dağıtımınızı test etmek için kullanışlıdır.

IoT Hub 'ınızda IoT Edge cihaz kaydı oluşturmak için, WSL 2 ortamınızda aşağıdaki komutları çalıştırın. `az login`Azure aboneliğinizde oturum açmak için komutunu kullanın:

```bash
az iot hub device-identity create --device-id bridge-edge-device --edge-enabled true --hub-name {your IoT hub name}
```

IoT Edge çalışma zamanı yüklü bir Azure sanal makinesi oluşturmak için aşağıdaki komutları çalıştırın. Yer tutucuları uygun değerlerle güncelleştirin:

```bash
az group create --name bridge-edge-resources --location eastus
az deployment group create \
--resource-group bridge-edge-resources \
--template-uri "https://aka.ms/iotedge-vm-deploy" \
--parameters dnsLabelPrefix='{unique DNS name for virtual machine}' \
--parameters adminUsername='{admin user name}' \
--parameters deviceConnectionString=$(az iot hub device-identity connection-string show --device-id bridge-edge-device --hub-name {your IoT hub name} -o tsv) \
--parameters authenticationType='password' \
--parameters adminPasswordOrKey="{admin password for virtual machine}"
```

Artık bir sanal makinede çalışan IoT Edge çalışma zamanına sahipsiniz. **$EdgeAgent** ve **$edgeHub** cihazda çalıştığından emin olmak için aşağıdaki komutu kullanabilirsiniz:

```bash
az iot hub module-identity list --device-id bridge-edge-device -o table --hub-name {your IoT hub name}
```

> [!CAUTION]
> Bu sanal makine çalışırken faturalandırılırsınız. Kullanmadığınız sırada kapatmayı ve uygulamayı tamamladıktan sonra kaldırmayı unutmayın ve bu uygulamayı kapatın.

### <a name="download-the-source-code"></a>Kaynak kodunu indirin

Aşağıdaki adımlarda, WSL 2 ortamınızda Docker görüntüsünü oluşturacaksınız. **IoT-Plug-Play-Bridge** deposunu kopyalamak için, uygun bir klasörde bir WSL 2 bash kabuğu 'nda aşağıdaki komutları çalıştırın:

```bash
git clone https://github.com/Azure/iot-plug-and-play-bridge.git

cd iot-plug-and-play-bridge

git submodule update --init --recursive
```

Önceki komutun çalıştırılması birkaç dakika sürer.

### <a name="update-the-dockerfile"></a>*Dockerfile* 'ı güncelleştirme

VS Code başlatın, komut paletini açın, *WSL 'de uzak WSL: Open klasörünü* girin ve ardından klonlanmış depoyu içeren *IoT-Plug-Play-Bridge* klasörünü açın.

*Pnpköprü\dockerfile.exe* dosyasını açın. Ortam değişkeni tanımlarını aşağıdaki gibi düzenleyin:

```dockerfile
ENV IOTHUB_DEVICE_CONNECTION_STRING="{Add your device connection string here}"
ENV PNP_BRIDGE_ROOT_MODEL_ID="dtmi:com:example:RootPnpBridgeSampleDevice;1"
ENV PNP_BRIDGE_HUB_TRACING_ENABLED="false"
ENV IOTEDGE_WORKLOADURI="something"
```

> [!TIP]
> Cihaz bağlantı dizesini almak için aşağıdaki komutu kullanabilirsiniz: `az iot hub device-identity connection-string show --device-id bridge-edge-device --hub-name {your IoT hub name}`

Diğer mimarilere yönelik örnek *Dockerfiles* vardır.

### <a name="build-the-iot-plug-and-play-bridge-module-image"></a>IoT Tak ve Kullan köprü modülü görüntüsünü oluşturma

VS Code, komut paletini açın, *Docker kayıt defterleri: DOCKER CLI ' da oturum açın* ve Azure aboneliğinizi ve kapsayıcı kayıt defterinizi seçin. Bu komut Docker 'ın kapsayıcı Kayıt defterinize bağlanmasını ve modül görüntüsünü karşıya yüklemeyi sağlar.

Dosya *üzerinde pnpbridge/module.js* açın. `{your container registry name}.azurecr.io/iotpnpbridge`Kapsayıcı görüntü deposu olarak ve sürümü olarak ekleyin `v1` .

VS Code, **Gezgin** görünümünde dosya *üzerinde pnpbridge/module.js* öğesine sağ tıklayın, **derleme ve gönderme IoT Edge modülü görüntüsünü** seçin ve platform olarak **AMD64** ' i seçin.

VS Code, **Docker** görünümünde, artık **ıotpnpbridge: v1-AMD64** modül görüntüsünü içeren kapsayıcı kayıt defterinizin içeriğine gözatabilmeniz gerekir.

### <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bir IoT Edge cihaz modül görüntülerini bir kapsayıcı kayıt defterinden indirir. Bu örnek, bir Azure Container kayıt defteri kullanır.

**Köprü uç kaynakları** kaynak grubunda bir Azure Container Registry oluşturun. Ardından kapsayıcı Kayıt defterinize yönetici erişimini etkinleştirin ve IoT Edge cihazınızın modül görüntülerini indirmek için ihtiyaç duyacağı kimlik bilgilerini alın:

```bash
az acr create -g bridge-edge-resources --sku Basic -n {your container registry name}
az acr update --admin-enabled true -n {your container registry name}
az acr credential show -n {your container registry name}
```

### <a name="create-the-deployment-manifest"></a>Dağıtım bildirimini oluşturma

IoT Edge dağıtım bildirimi, bir IoT Edge cihazına dağıtılacak modülleri ve yapılandırma değerlerini belirtir.

VS Code ' de, dosya *üzerinde pnpbridge/deployment.template.js* açın:

- `registryCredentials`Önceki komuttan gelen değerlerle güncelleştirin. `address`Değer gibi görünür `{your container registry name}.azurecr.io` .
- `image`İçin değerini güncelleştirin `ModulePnpBridge` . Modül görüntüsü şöyle görünür: `{your container registry}.azurecr.io/iotpnpbridge:v1-amd64` .
- `PnPBridgeConfig`CO2 algılama bağdaştırıcısını yapılandırmak için AŞAĞıDAKI JSON ile değiştirin:

  ```json
  "PnpBridgeConfig": {
    "pnp_bridge_interface_components": [
      {
        "_comment": "Component 1 - Modbus Device",
        "pnp_bridge_component_name": "Co2Detector1",
        "pnp_bridge_adapter_id": "modbus-pnp-interface",
        "pnp_bridge_adapter_config": {
          "unit_id": 1,
          "tcp": {
            "host": "10.159.29.2",
            "port": 502
          },
          "modbus_identity": "DL679"
        }
      }
    ],
    "pnp_bridge_adapter_global_configs": {
      "modbus-pnp-interface": {
        "DL679": {
          "telemetry": {
            "co2": {
              "startAddress": "40001",
              "length": 1,
              "dataType": "integer",
              "defaultFrequency": 5000,
              "conversionCoefficient": 1
            },
            "temperature": {
              "startAddress": "40003",
              "length": 1,
              "dataType": "decimal",
              "defaultFrequency": 5000,
              "conversionCoefficient": 0.01
            }
          },
          "properties": {
            "firmwareVersion": {
              "startAddress": "40482",
              "length": 1,
              "dataType": "hexstring",
              "defaultFrequency": 60000,
              "conversionCoefficient": 1,
              "access": 1
            },
            "modelName": {
              "startAddress": "40483",
              "length": 2,
              "dataType": "string",
              "defaultFrequency": 60000,
              "conversionCoefficient": 1,
              "access": 1
            },
            "alarmStatus_co2": {
              "startAddress": "00305",
              "length": 1,
              "dataType": "boolean",
              "defaultFrequency": 1000,
              "conversionCoefficient": 1,
              "access": 1
            },
            "alarmThreshold_co2": {
              "startAddress": "40225",
              "length": 1,
              "dataType": "integer",
              "defaultFrequency": 30000,
              "conversionCoefficient": 1,
              "access": 2
            }
          },
          "commands": {
            "clearAlarm_co2": {
              "startAddress": "00305",
              "length": 1,
              "dataType": "flag",
              "conversionCoefficient": 1
            }
          }
        }
      }
    }
  }
  ```

  Değişiklikleri kaydedin.

VS Code, **Gezgin** görünümünde dosya *üzerinde pnpbridge/deployment.template.js* öğesine sağ tıklayın, **IoT Edge dağıtım bildirimi oluştur**' u seçin. Bu komut, dağıtım bildiriminde *pnpbridge/config/deployment.amd64.js* oluşturur.

### <a name="deploy-the-bridge-to-your-iot-edge-device"></a>IoT Edge cihazınıza köprü dağıtma

VS Code, komut paletini açın, *Azure IoT Hub girin: IoT Hub*' yı seçin, ardından aboneliğinizi ve IoT Hub 'ınızı seçin.

VS Code ' de, **Gezgin** görünümündeki dosya *üzerinde pnpbridge/config/deployment.amd64.js* öğesine sağ tıklayın, **tek cihaz için dağıtım oluştur**' u seçin ve **Bridge-Edge-Device**' ı seçin.

Cihazınızdaki modüllerin durumunu görüntülemek için aşağıdaki komutu çalıştırın:

```bash
az iot hub module-identity list --device-id bridge-edge-device -o table --hub-name {your IoT hub name}
```

Çalışan modüllerin listesi artık CO2 algılama bağdaştırıcısını kullanacak şekilde yapılandırılmış olan **Modulepnpbridge** modülünü içerir.

### <a name="tidy-up"></a>Tidy yukarı

Sanal makineyi ve kapsayıcı kayıt defterini Azure aboneliğinizden kaldırmak için aşağıdaki komutu çalıştırın:

```bash
az group delete -n bridge-edge-resources
```

## <a name="folder-structure"></a>Klasör yapısı

*pnpbridge\deps\azure-iot-SDK-c-PnP*: c SDK Için Azure ıOT cihaz SDK 'sını içeren git alt modülleri.

*pnpköprü\ KomutDosyaları*: derleme betikleri.

*pnpköprü\src*: IoT Tak ve kullan köprü çekirdeği için kaynak kodu.

*pnpköprü\src\bağdaştırıcıları*: çeşitli IoT Tak ve kullan köprü bağdaştırıcıları için kaynak kodu.

## <a name="next-steps"></a>Sonraki adımlar

IoT Tak ve Kullan köprüsü hakkında daha fazla bilgi edinmek için [ıot Tak ve Kullan köprüsü](https://github.com/Azure/iot-plug-and-play-bridge) GitHub deposunu ziyaret edin.
