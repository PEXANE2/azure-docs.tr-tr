---
title: include dosyası
description: include dosyası
services: iot-hub
author: chrissie926
ms.service: iot-hub
ms.topic: include
ms.date: 08/07/2019
ms.author: menchi
ms.custom: include file
ms.openlocfilehash: a5c1ddd085ae65b9920d73f50f993f4646785a69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68883814"
---
## <a name="create-a-module-identity"></a>Modül kimliği oluşturma

Bu bölümde, hub'ınızdaki kimlik kayıt defterinde bir aygıt kimliği ve modül kimliği oluşturan bir .NET konsol uygulaması oluşturursunuz. Bir aygıt veya modül, kimlik kayıt defterinde bir giriş olmadığı sürece hub'a bağlayamaz. Daha fazla bilgi [için, IoT Hub geliştirici kılavuzunun Kimlik Kayıt Defteri bölümüne](../articles/iot-hub/iot-hub-devguide-identity-registry.md)bakın.

Bu konsol uygulamasını çalıştırdığınızda, hem cihaz hem de modül için benzersiz bir kimlik ve anahtar oluşturur. Aygıtınız ve modülünüz, IoT Hub'a aygıttan buluta iletiler gönderirken kendilerini tanımlamak için bu değerleri kullanır. Kimlikler büyük/küçük harfe duyarlıdır.

1. Visual Studio'u açın ve **yeni bir proje oluştur'u**seçin.

1. **Yeni bir proje oluştur'da**Konsol **Uygulaması'nı (.NET Framework)** seçin.

1. **Yeni projenizi Yapılandırma'yı**açmak için **İleri'yi** seçin. Projeye *CreateIdentities* ve çözüme *IoTHubGetStarted* adını verin. .NET Framework sürümünün 4.6.1 veya sonraki bir sürüm olduğundan emin olun.

    ![Visual Studio çözümünüz için ad ve çerçeve girin](./media/iot-hub-get-started-create-module-identity-csharp/configure-createidentities-project.png)

1. Visual Studio'da, Open **Tools** > **NuGet Package Manager** > **Çözüm için NuGet Paketlerini Yönetin.** **Gözat** sekmesini seçin.

1. **Microsoft.Azure.Devices'ı**arayın. Seçin ve sonra **Yükle'yi**seçin.

    ![Azure IoT Hub .NET hizmeti SDK geçerli sürümünü yükleme](./media/iot-hub-get-started-create-module-identity-csharp/install-service-sdk.png)

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

   ```csharp
   using Microsoft.Azure.Devices;
   using Microsoft.Azure.Devices.Common.Exceptions;
   ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.

   ```csharp
   const string connectionString = "<replace_with_iothub_connection_string>";
   const string deviceID = "myFirstDevice";
   const string moduleID = "myFirstModule";
   ```

1. **Ana** sınıfa aşağıdaki kodu ekleyin.

   ```csharp
   static void Main(string[] args)
   {
       AddDeviceAsync().Wait();
       AddModuleAsync().Wait();
   }
   ```

1. **Program** sınıfına aşağıdaki yöntemleri ekleyin:

    ```csharp
    private static async Task AddDeviceAsync()
    {
       RegistryManager registryManager = 
         RegistryManager.CreateFromConnectionString(connectionString);
       Device device;

       try
       {
           device = await registryManager.AddDeviceAsync(new Device(deviceID));
       }
       catch (DeviceAlreadyExistsException)
        {
            device = await registryManager.GetDeviceAsync(deviceID);
        }

        Console.WriteLine("Generated device key: {0}", 
          device.Authentication.SymmetricKey.PrimaryKey);
    }

    private static async Task AddModuleAsync()
    {
        RegistryManager registryManager = 
          RegistryManager.CreateFromConnectionString(connectionString);
        Module module;

        try
        {
            module = 
              await registryManager.AddModuleAsync(new Module(deviceID, moduleID));
        }
        catch (ModuleAlreadyExistsException)
        {
            module = await registryManager.GetModuleAsync(deviceID, moduleID);
        }

        Console.WriteLine("Generated module key: {0}", module.Authentication.SymmetricKey.PrimaryKey);
    }
    ```

    Yöntem `AddDeviceAsync` **kimliği myFirstDevice**ile bir aygıt kimliği oluşturur. Bu aygıt kimliği kimlik kayıt defterinde zaten varsa, kod yalnızca varolan aygıt bilgilerini alır. Bu durumda uygulama, bu kimliğin birincil anahtarını görüntüler. Hub'ınıza bağlanmak için bu anahtarı simüle edilen aygıt uygulamasında kullanırsınız.

    Yöntem `AddModuleAsync` **cihaz myFirstDevice**altında kimliği **myFirstModule** ile bir modül kimliği oluşturur. Bu modül kimliği kimlik kayıt defterinde zaten varsa, kod yalnızca varolan modül bilgilerini alır. Bu durumda uygulama, bu kimliğin birincil anahtarını görüntüler. Hub'ınıza bağlanmak için simüle edilmiş modül uygulamasında bu anahtarı kullanırsınız.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. Bu uygulamayı çalıştırın ve aygıt anahtarı ve modül anahtarını not edin.

> [!NOTE]
> IoT Hub kimlik kayıt defteri, hub'a güvenli erişim sağlamak için yalnızca aygıt ve modül kimliklerini depolar. Kimlik kayıt defteri, cihaz kimliklerini ve anahtarlarını güvenlik kimlik bilgileri olarak kullanmak için depolar. Kimlik kayıt defterinin her cihaz için depoladığı etkin/devre dışı bayrağını kullanarak, ilgili cihaza erişimi devre dışı bırakabilirsiniz. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa, uygulamaya özgü bir mağaza kullanmalıdır. Modül kimlikleri için etkin/devre dışı bayrağı yoktur. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
