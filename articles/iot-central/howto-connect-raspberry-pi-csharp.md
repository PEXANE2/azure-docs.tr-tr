---
title: Azure IoT Central uygulamanıza bir Raspberry Pi bağlama (C#) | Microsoft Docs
description: Bir cihaz geliştiricisi olarak, kullanarak C#Raspberry Pi 'yi Azure IoT Central uygulamanıza bağlama.
author: viv-liu
ms.author: viviali
ms.date: 04/15/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 3feb0b2b50851903bbd6799f46d489879e62bf43
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876212"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-c"></a>Azure IoT Central uygulamanıza bir Raspberry PI bağlama (C#)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Bu makalede, bir cihaz geliştiricisi olarak, C# programlama dilini kullanarak bir Raspberry Pi 'yi Microsoft Azure IoT Central uygulamanıza nasıl bağlayabileceğinizi açıklar.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için aşağıdaki bileşenlere ihtiyacınız vardır:

* **Örnek Devkits** uygulama şablonundan oluşturulan bir Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* Raspbian işletim sistemini çalıştıran bir Raspberry PI cihazı. Raspberry PI, internet 'e bağlanabilmelidir. Daha fazla bilgi için bkz. [Raspberry PI 'Nizi ayarlama](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3).

## <a name="sample-devkits-application"></a>**Örnek Devkits** uygulaması

**Örnek Devkits** uygulama şablonundan oluşturulan bir uygulama, aşağıdaki özelliklere sahip bir **Raspberry PI** cihaz şablonu içerir:

- Telemetri, cihazın toplayacağı aşağıdaki ölçüleri içerir:
  - Nem
  - Sıcaklık
  - Basınç
  - Manyetik tometre (X, Y, Z)
  - İvme ölçer (X, Y, Z)
  - Jroscope (X, Y, Z)
- Ayarlar
  - Geril
  - Geçerli
  - Fan hızı
  - IR geçişi.
- Özellikler
  - Zar numarası cihaz özelliği
  - Konum bulutu özelliği

Cihaz şablonunun yapılandırmasının tam ayrıntıları için, [Raspberry PI cihaz şablonu ayrıntılarına](#raspberry-pi-device-template-details)bakın.

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IoT Central uygulamanızda, **Raspberry PI** cihaz şablonundan gerçek bir cihaz ekleyin. Cihaz bağlantısı ayrıntılarını (**kapsam kimliği**, **cihaz kimliği**ve **birincil anahtar**) bir yere unutmayın. Daha fazla bilgi için bkz. [Azure IoT Central uygulamanıza gerçek bir cihaz ekleme](tutorial-add-device.md).

### <a name="create-your-net-application"></a>.NET uygulamanızı oluşturma

Cihaz uygulamasını masaüstü makinenizde oluşturup test edersiniz.

Aşağıdaki adımları tamamlayabilmeniz için Visual Studio Code kullanabilirsiniz. Daha fazla bilgi için bkz. [Ile C#çalışma ](https://code.visualstudio.com/docs/languages/csharp).

> [!NOTE]
> İsterseniz, farklı bir kod Düzenleyicisi kullanarak aşağıdaki adımları tamamlayabilirsiniz.

1. .NET projenizi başlatmak ve gerekli NuGet paketlerini eklemek için aşağıdaki komutları çalıştırın:

   ```cmd/sh
   mkdir pisample
   cd pisample
   dotnet new console
   dotnet add package Microsoft.Azure.Devices.Client
   dotnet restore
   ```

1. `pisample` Visual Studio Code klasörü açın. Ardından **pisample. csproj** proje dosyasını açın. Aşağıdaki kod parçacığında gösterilen etiketiekleyin:`<RuntimeIdentifiers>`

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifiers>win-arm;linux-arm</RuntimeIdentifiers>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="Microsoft.Azure.Devices.Client" Version="1.19.0" />
      </ItemGroup>
    </Project>
    ```

    > [!NOTE]
    > **Microsoft. Azure. Devices. Client** Package sürüm numaranız gösterilenden daha yüksek olabilir.

1. **Pisample. csproj**öğesini kaydedin. Visual Studio Code restore komutunu yürütmenizi isterse, **geri yükle**' yi seçin.

1. **Program.cs** açın ve içeriğini şu kodla değiştirin:

    ```csharp
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;

    namespace pisample
    {
      class Program
      {
        static string DeviceConnectionString = "{your device connection string}";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();
        static CancellationTokenSource cts;
        static double baseTemperature = 60;
        static double basePressure = 500;
        static double baseHumidity = 50;
        static void Main(string[] args)
        {
          Console.WriteLine("Raspberry Pi Azure IoT Central example");

          try
          {
            InitClient();
            SendDeviceProperties();

            cts = new CancellationTokenSource();
            SendTelemetryAsync(cts.Token);

            Console.WriteLine("Wait for settings update...");
            Client.SetDesiredPropertyUpdateCallbackAsync(HandleSettingChanged, null).Wait();
            Console.ReadKey();
            cts.Cancel();
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        public static void InitClient()
        {
          try
          {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        public static async void SendDeviceProperties()
        {
          try
          {
            Console.WriteLine("Sending device properties:");
            Random random = new Random();
            TwinCollection telemetryConfig = new TwinCollection();
            reportedProperties["dieNumber"] = random.Next(1, 6);
            Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

            await Client.UpdateReportedPropertiesAsync(reportedProperties);
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        private static async void SendTelemetryAsync(CancellationToken token)
        {
          try
          {
            Random rand = new Random();

            while (true)
            {
              double currentTemperature = baseTemperature + rand.NextDouble() * 20;
              double currentPressure = basePressure + rand.NextDouble() * 100;
              double currentHumidity = baseHumidity + rand.NextDouble() * 20;

              var telemetryDataPoint = new
              {
                humidity = currentHumidity,
                pressure = currentPressure,
                temp = currentTemperature
              };
              var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
              var message = new Message(Encoding.ASCII.GetBytes(messageString));

              token.ThrowIfCancellationRequested();
              await Client.SendEventAsync(message);

              Console.WriteLine("{0} > Sending telemetry: {1}", DateTime.Now, messageString);

              await Task.Delay(1000);
            }
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Intentional shutdown: {0}", ex.Message);
          }
        }

        private static async Task HandleSettingChanged(TwinCollection desiredProperties, object userContext)
        {
          try
          {
            Console.WriteLine("Received settings change...");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            string setting = "fanSpeed";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "setVoltage";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "setCurrent";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "activateIR";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
          }

          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        private static void AcknowledgeSettingChange(TwinCollection desiredProperties, string setting)
        {
          reportedProperties[setting] = new
          {
            value = desiredProperties[setting]["value"],
            status = "completed",
            desiredVersion = desiredProperties["$version"],
            message = "Processed"
          };
        }
      }
    }
    ```

    > [!NOTE]
    > Bir sonraki adımda yer `{your device connection string}` tutucuyu güncelleştirebilirsiniz.

## <a name="run-your-net-application"></a>.NET uygulamanızı çalıştırma

Cihaza özel Bağlantı dizenizi, Azure IoT Central kimlik doğrulaması yapacak şekilde cihazın koduna ekleyin. Daha önce bir değişiklik yaptığınız **kapsam kimliği**, **cihaz kimliği**ve **birincil anahtarı** kullanarak [Cihaz bağlantı dizesi oluşturmak](howto-generate-connection-string.md) için bu yönergeleri izleyin.

1. Program.cs `{your device connection string}` dosyasında, oluşturduğunuz bağlantı dizesiyle değiştirin.

1. Komut satırı ortamınızda aşağıdaki komutu çalıştırın:

   ```cmd/sh
   dotnet restore
   dotnet publish -r linux-arm
   ```

1. `pisample\bin\Debug\netcoreapp2.1\linux-arm\publish` Klasörü Raspberry PI cihazınıza kopyalayın. Dosyaları kopyalamak için **SCP** komutunu kullanabilirsiniz, örneğin:

    ```cmd/sh
    scp -r publish pi@192.168.0.40:publish
    ```

    Daha fazla bilgi için bkz. [Raspberry PI uzaktan erişim](https://www.raspberrypi.org/documentation/remote-access/).

1. Raspberry PI cihazınızda oturum açın ve bir kabukta aşağıdaki komutları çalıştırın:

    ```cmd/sh
    sudo apt-get update
    sudo apt-get install libc6 libcurl3 libgcc1 libgssapi-krb5-2 liblttng-ust0 libstdc++6 libunwind8 libuuid1 zlib1g
    ```

1. Raspberry PI 'de aşağıdaki komutları çalıştırın:

    ```cmd/sh
    cd publish
    chmod 777 pisample
    ./pisample
    ```

    ![Program başlar](./media/howto-connect-raspberry-pi-csharp/device_begin.png)

1. Azure IoT Central uygulamanızda, Raspberry PI üzerinde çalışan kodun uygulamayla nasıl etkileşime gireceğini görebilirsiniz:

   * Gerçek cihazınızın **ölçümler** sayfasında telemetri ' i görebilirsiniz.
   * **Özellikler** sayfasında, bildirilen **zar numarası** özelliğinin değerini görebilirsiniz.
   * **Ayarlar** sayfasında, Raspberry Pi üzerinde voltaj ve fan hızı gibi çeşitli ayarları değiştirebilirsiniz.

     Aşağıdaki ekran görüntüsünde, ayar değişikliğini alan Raspberry PI gösterilmektedir:

     ![Raspberry PI ayar değişikliğini alır](./media/howto-connect-raspberry-pi-csharp/device_switch.png)

## <a name="raspberry-pi-device-template-details"></a>Raspberry PI cihaz şablonu ayrıntıları

**Örnek Devkits** uygulama şablonundan oluşturulan bir uygulama, aşağıdaki özelliklere sahip bir **Raspberry PI** cihaz şablonu içerir:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

| Alan adı     | Birimler  | Minimum | Maksimum | Ondalık basamak sayısı |
| -------------- | ------ | ------- | ------- | -------------- |
| Nem oranı       | %      | 0       | 100     | 0              |
| kopyalar           | 20     | -40     | 120     | 0              |
| basınç       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| Ivometerx | mg     | -2000   | 2000    | 0              |
| Iventery | mg     | -2000   | 2000    | 0              |
| Ivometerz | mg     | -2000   | 2000    | 0              |
| Jroscopex     | MDPS   | -2000   | 2000    | 0              |
| Jroscopey     | MDPS   | -2000   | 2000    | 0              |
| Jroscopez     | MDPS   | -2000   | 2000    | 0              |

### <a name="settings"></a>Ayarlar

Sayısal ayarlar

| Display name | Alan adı | Birimler | Ondalık basamak sayısı | Minimum | Maksimum | İlk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Geril      | Setvoltaj | Çalışmıyorken | 0              | 0       | 240     | 0       |
| Geçerli      | setCurrent | AMPS  | 0              | 0       | 100     | 0       |
| Fan hızı    | Fanın hızı   | RPM   | 0              | 0       | 1000    | 0       |

Ayarları aç

| Display name | Alan adı | Metinde | Kapalı metin | İlk |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | Activateır | AÇIK      | KAPALI      | Kapalı     |

### <a name="properties"></a>Özellikler

| Type            | Display name | Alan adı | Veri türü |
| --------------- | ------------ | ---------- | --------- |
| Cihaz özelliği | Zar numarası   | dieNumber  | numarası    |
| Text            | Location     | location   | Yok       |

## <a name="next-steps"></a>Sonraki adımlar

Artık bir Raspberry Pi 'yi Azure IoT Central uygulamanıza bağlamayı öğrendiğinize göre, önerilen sonraki adım, kendi IoT cihazınız için [özel bir cihaz şablonu ayarlamayı](howto-set-up-template.md) öğrenirsiniz.
