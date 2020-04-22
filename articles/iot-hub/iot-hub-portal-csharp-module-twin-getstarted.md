---
title: Azure IoT Hub modülü kimlik & modülü ikiz (portal ve .NET)
description: Portal ve .NET kullanarak modül kimliği oluşturmayı ve modülü güncelleştirmeyi öğrenin.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/20/2019
ms.custom: amqp
ms.openlocfilehash: a3258de2ed7269ab50e6feca3c421d55de5a9d91
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759772"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-the-portal-and-net-device"></a>Portal ve .NET cihazını kullanarak IoT Hub modül kimliğini ve modül ikizini kullanmaya başlama

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [Modül kimlikleri ve modül ikizleri](iot-hub-devguide-module-twins.md), Azure IoT Hub cihaz kimliğine ve cihaz ikizine benzer, ancak daha hassas ayrıntı düzeyi sağlar. Azure IoT Hub aygıt kimliği ve aygıt ikizi, arka uç uygulamasının bir aygıtı yapılandırmasını ve aygıtın koşullarında görünürlük sağlamasını sağlarken, modül kimliği ve modül ikizi bu özellikleri bir aygıtın tek tek bileşenleri için sağlar. İşletim sistemi tabanlı aygıtlar veya firmware aygıtları gibi birden fazla bileşene sahip yetenekli cihazlarda, modül kimlikleri ve modül ikizleri her bileşen için yalıtılmış yapılandırma ve koşullarsağlar.
>

Bu öğreticide şunları öğreneceksiniz:

* Portalda bir modül kimliği nasıl oluşturulur.

* Modülü aygıtınızdan ikiz olarak güncellemek için bir .NET cihazı SDK nasıl kullanılır?

> [!NOTE]
> Her iki uygulamayı da cihazlarda çalıştırmak için kullanabileceğiniz Azure IoT SK'ları hakkında bilgi için Azure [IoT SDK'larına](iot-hub-devguide-sdks.md)bakın.
>

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio.

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

## <a name="create-a-hub"></a>Hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-hub"></a>Hub'a yeni bir aygıt kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-module-identity-in-the-portal"></a>Portalda bir modül kimliği oluşturma

Tek bir cihaz kimliği içinde en fazla 20 modül kimliği oluşturabilirsiniz. Kimlik eklemek için aşağıdaki adımları izleyin:

1. Önceki bölümde oluşturduğunuz aygıt için, ilk modül kimliğinizi oluşturmak için **Modül Kimliği Ekle'yi** seçin.

1. *MyFirstModule*adını girin. Modül kimliğinizi kaydedin.

    ![Modül kimliği ekleme](./media/iot-hub-portal-csharp-module-twin-getstarted/add-module-identity.png)

    Yeni modül kimliğiniz ekranın alt kısmında görünür. Modül kimlik ayrıntılarını görmek için seçin.

    ![Modül kimlik bilgilerini görme](./media/iot-hub-portal-csharp-module-twin-getstarted/module-identity-details.png)

Connect **dizesini**kaydet - birincil anahtar . Modülünüzü cihaza ayarlamak için bir sonraki bölümde kullanırsınız.

## <a name="update-the-module-twin-using-net-device-sdk"></a>.NET cihaz SDK’sını kullanarak modül ikizini güncelleştirme

IoT Hub’ınızda modül kimliğini başarıyla oluşturdunuz. Simülasyon cihazınızdan bulutla iletişim kurmayı deneyelim. Bir modül kimliği oluşturulduktan sonra, IoT Hub’ında örtük olarak bir modül ikizi oluşturulur. Bu bölümde, modül ikizi tarafından raporlanan özelliklerini güncelleştiren simülasyon cihazınızda bir .NET konsol uygulaması oluşturacaksınız.

### <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

Modül ikiz bildirilen özelliklerini güncelleyen bir uygulama oluşturmak için aşağıdaki adımları izleyin:

1. Visual Studio'da **yeni bir proje oluştur'u**seçin, ardından Konsol **Uygulaması'nı (.NET Framework)** seçin ve **İleri'yi**seçin.

1. **Yeni projenizi yapılandırın,** **Proje adı**olarak *UpdateModuleTwinReportedProperties* girin. Devam etmek için **Oluştur'u** seçin.

    ![Görsel bir stüdyo projesi olduğunuzu yapılandırın](./media/iot-hub-portal-csharp-module-twin-getstarted/configure-twins-project.png)

### <a name="install-the-latest-azure-iot-hub-net-device-sdk"></a>En son Azure IoT Hub .NET cihazı SDK'yı yükleyin

Modül kimliği ve modül ikizi genel önizlemede. Yalnızca IoT Hub ön sürüm aygıtı SDK'larda kullanılabilir. Yüklemek için aşağıdaki adımları izleyin:

1. Visual Studio'da, Open **Tools** > **NuGet Package Manager** > **Çözüm için NuGet Paketlerini Yönetin.**

1. **Gözat'ı**seçin ve ardından **ön sürüm ekle'yi**seçin. *Microsoft.Azure.Devices.Client'ı*arayın. En son sürümü seçin ve yükleyin.

    ![Azure IoT Hub .NET hizmeti SDK önizlemesini yükleme](./media/iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png)

    Şimdi tüm modül özelliklerine erişiminiz vardır.

### <a name="get-your-module-connection-string"></a>Modül bağlantı dizenizi alın

Konsol uygulamanız için modül bağlantı dizesi gerekir. Şu adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. IoT hub'ınıza gidin ve **IoT Aygıtlarını**seçin. **MyFirstDevice'ı** açın ve **myFirstModule'in** başarıyla oluşturulduğunu görün.

1. **Modül Kimlikleri**altında **myFirstModule'i** seçin. **Modül Kimlik Ayrıntıları'nda** **Bağlantı dizesini (birincil anahtar)** kopyalayın.

    ![Azure portalı modül ayrıntısı](./media/iot-hub-portal-csharp-module-twin-getstarted/module-identity-details.png)

### <a name="create-updatemoduletwinreportedproperties-console-app"></a>Güncelleme ModülüOluşturmaTwinReportedProperties konsol uygulaması

Uygulamanızı oluşturmak için aşağıdaki adımları izleyin:

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

  ```csharp
  using Microsoft.Azure.Devices.Client;
  using Microsoft.Azure.Devices.Shared;
  using Newtonsoft.Json;
  ```

2. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, modül bağlantı dizesiyle değiştirin.

  ```csharp
  private const string ModuleConnectionString = "<Your module connection string>";
  private static ModuleClient Client = null;
  ```

3. Aşağıdaki **OnDesiredPropertyChanged** yöntemini **Program** sınıfına ekleyin:

  ```csharp
  private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
      {
          Console.WriteLine("desired property change:");
          Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
          Console.WriteLine("Sending current time as reported property");
          TwinCollection reportedProperties = new TwinCollection
          {
              ["DateTimeLastDesiredPropertyChangeReceived"] = DateTime.Now
          };
  
          await Client.UpdateReportedPropertiesAsync(reportedProperties).ConfigureAwait(false);
      }
  ```

4. Son olarak, **Ana** yöntemi aşağıdaki kodla değiştirin:

  ```csharp
  static void Main(string[] args)
  {
      Microsoft.Azure.Devices.Client.TransportType transport = Microsoft.Azure.Devices.Client.TransportType.Amqp;
  
      try
      {
          Client = ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
          Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
          Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();
  
          Console.WriteLine("Retrieving twin");
          var twinTask = Client.GetTwinAsync();
          twinTask.Wait();
          var twin = twinTask.Result;
          Console.WriteLine(JsonConvert.SerializeObject(twin));
  
          Console.WriteLine("Sending app start time as reported property");
          TwinCollection reportedProperties = new TwinCollection();
          reportedProperties["DateTimeLastAppLaunch"] = DateTime.Now;
  
          Client.UpdateReportedPropertiesAsync(reportedProperties);
      }
      catch (AggregateException ex)
      {
          Console.WriteLine("Error in sample: {0}", ex);
      }
  
      Console.WriteLine("Waiting for Events.  Press enter to exit...");
      Console.ReadKey();
      Client.CloseAsync().Wait();
  }
  
  private static void ConnectionStatusChangeHandler(ConnectionStatus status, ConnectionStatusChangeReason reason)
  {
      Console.WriteLine($"Status {status} changed: {reason}");
  }
  ```
  
  **F5**kullanarak bu uygulamayı oluşturabilir ve çalıştırabilirsiniz.

Bu kod örneği, AMQP protokolüyle raporlanan özellikleri güncelleştirme ve modül ikizini alma işlemini nasıl yapacağınızı gösterir. Genel önizleme aşamasında, modül ikizi işlemleri için yalnızca AMQP’yi destekleriz.

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [.NET yedekleme ve .NET cihazını kullanarak IoT Hub modül kimliğini ve modül ikizini kullanmaya başlama](iot-hub-csharp-csharp-module-twin-getstarted.md)

* [IoT Edge ile çalışmaya başlama](../iot-edge/tutorial-simulate-device-linux.md)
