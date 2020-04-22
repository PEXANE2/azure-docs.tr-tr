---
title: Azure IoT Hub'ında X.509 güvenliği için öğretici | Microsoft Dokümanlar
description: Azure IoT hub'ınızdaki X.509 tabanlı güvenliğe benzetim ortamında başlayın.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.custom: amqp
ms.openlocfilehash: ac45cf42ed174d3e9423b4ea39cadf16b84897ef
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759641"
---
# <a name="set-up-x509-security-in-your-azure-iot-hub"></a>Azure IoT hub’ınızda X.509 güvenliğini ayarlayın

Bu öğretici, *X.509 Sertifika Kimlik Doğrulaması'nı*kullanarak Azure IoT hub'ınızı güvenli hale getirmek için gereken adımları gösterir. Çizim amacıyla, Windows makinenizde yerel olarak sertifika lar oluşturmak için open source aracı OpenSSL'i kullanırız. Bu öğreticiyi yalnızca test amacıyla kullanmanızı öneririz. Üretim ortamı için, sertifikaları *bir kök sertifika yetkilisinden (CA)* satın almalısınız.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, aşağıdaki kaynakların hazır olması gerektiğini gerektirir:

* Azure aboneliğinizi içeren bir IoT hub'ı oluşturdunuz. Bkz. Ayrıntılı adımlar için [portal aracılığıyla bir IoT hub'ı oluşturun.](iot-hub-create-through-portal.md)

* Visual [Studio 2017 veya Visual Studio 2019](https://www.visualstudio.com/vs/) yüklü.

## <a name="get-x509-ca-certificates"></a>X.509 CA sertifikaları alın

IoT Hub'daki X.509 sertifika tabanlı güvenlik, yaprak sertifikasına kadar kök sertifikanın yanı sıra ara sertifikaları da içeren bir [X.509 sertifika zinciriyle](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification)başlamanızı gerektirir.

Sertifikalarınızı almak için aşağıdaki yollardan birini seçebilirsiniz:

* *Bir kök sertifika yetkilisinden (CA)* X.509 sertifikaları satın alın. Bu yöntem üretim ortamları için önerilir.

* [OpenSSL](https://www.openssl.org/)gibi bir üçüncü taraf aracını kullanarak kendi X.509 sertifikalarınızı oluşturun. Bu teknik test ve geliştirme amacıyla iyidir. PowerShell veya Bash kullanarak test CA sertifikaları oluşturma hakkında bilgi için [örnekler ve öğreticiler için test CA sertifikalarını yönetmeye](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) bakın. Bu öğreticinin geri kalanı, [örnekler ve öğreticiler için test CA sertifikalarını yönetme](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)yönergelerini izleyerek oluşturulan test CA sertifikalarını kullanır.

* Varolan bir kök CA sertifikası tarafından imzalanmış bir [X.509 ara CA sertifikası](iot-hub-x509ca-overview.md#sign-devices-into-the-certificate-chain-of-trust) oluşturun ve hub'a yükleyin. Ara sertifika yüklendikten ve doğrulandıktan sonra, aşağıda belirtildiği gibi, aşağıda belirtilen kök CA sertifikası yerine kullanılabilir. OpenSSL ([openssl req](https://www.openssl.org/docs/man1.1.0/man1/req.html) ve [openssl ca)](https://www.openssl.org/docs/man1.1.0/man1/ca.html)gibi araçlar bir ara CA sertifikası oluşturmak ve imzalamak için kullanılabilir.

> [!NOTE]
> Üçüncü tarafın diğer müşterilerinin cihazlarını IoT Hub'ınıza bağlamasına olanak sağlayacağından, size özgü değilse üçüncü taraf kökünü yüklemeyin.

## <a name="register-x509-ca-certificates-to-your-iot-hub"></a>X.509 CA sertifikalarını IoT hub'ınıza kaydettirin

Bu adımlar, portal aracılığıyla IoT hub'ınıza nasıl yeni bir Sertifika Yetkilisi ekleyeceğinizgösterilmektedir.

1. Azure portalında, IoT hub'ınıza gidin ve hub için **Ayarlar** > **Sertifikaları'nı** seçin.

1. Yeni bir sertifika eklemek için **Ekle'yi** seçin.

1. **Sertifika**Adı'nda, dostça bir görüntü adı girin ve bilgisayarınızdan önceki bölümde oluşturduğunuz sertifika dosyasını seçin.

1. Sertifikanızın başarıyla yüklendiğine dair bir bildirim aldıktan sonra **Kaydet'i**seçin.

    ![Sertifikayı karşıya yükleme](./media/iot-hub-security-x509-get-started/iot-hub-add-cert.png)  

   Sertifikanız **doğrulanmamış**durumu olan sertifikalar listesinde görünür.

1. **Sertifika Ayrıntılarını**görüntülemek için eklediğiniz sertifikayı seçin ve ardından **Doğrulama Kodu Oluştur'u**seçin.

   ![Sertifikayı doğrula](./media/iot-hub-security-x509-get-started/copy-verification-code.png)  

1. Doğrulama **Kodunu** panoya kopyalayın. Sertifika sahipliğini doğrulamak için kullanırsınız.

1. Örnekler ve [öğreticiler için test CA sertifikalarını yönetmede](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)Adım 3'e uyun.  Bu işlem, doğrulama kodunuzu, imza üreten X.509 CA sertifikanızla özel anahtar ilişkilendirmesiyle imzalar. Bu imzalama işlemini gerçekleştirmek için kullanılabilir araçlar vardır( örneğin OpenSSL. Bu [işlem, sahip olma Kanıtı](https://tools.ietf.org/html/rfc5280#section-3.1)olarak bilinir.

1. **Sertifika Ayrıntıları'nda**, Doğrulama **Sertifikası .pem veya .cer dosyası**altında imza dosyasını bulun ve açın. Ardından **Doğrula'yı**seçin.

   Sertifikanızın durumu **Doğrulandı**olarak değişir. Sertifika otomatik olarak güncelleştirilmezse **Yenile'yi** seçin.

## <a name="create-an-x509-device-for-your-iot-hub"></a>IoT hub'ınız için bir X.509 cihazı oluşturma

1. Azure portalında, IoT hub'ınıza gidin ve ardından **Explorers** > **IoT aygıtlarını**seçin.

1. Yeni bir aygıt eklemek için **Yeni'yi** seçin.

1. **Aygıt**Kimliği'nde, dost bir ekran adı girin. **Kimlik Doğrulama türü için** **X.509 CA İmzalı'yı**seçin ve ardından **Kaydet'i**seçin.

   ![Portalda X.509 cihazı oluşturma](./media/iot-hub-security-x509-get-started/new-x509-device.png)

## <a name="authenticate-your-x509-device-with-the-x509-certificates"></a>X.509 sertifikalarıyla X.509 cihazınızı doğrulayın

X.509 cihazınızın kimliğini doğrulamak için önce aygıtı CA sertifikasıyla imzalamanız gerekir. Yaprak cihazların imzalanması normalde üretim araçlarının buna göre etkinleştirildiği üretim tesisinde yapılır. Aygıt bir üreticiden diğerine geçerken, her üreticinin imzalama eylemi zincir içinde bir ara sertifika olarak yakalanır. Sonuç, CA sertifikasından aygıtın yaprak sertifikasına kadar olan sertifika zinciridir. [Örnekler ve öğreticiler için test CA sertifikalarını yönetmedeki](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) adım 4, bir aygıt sertifikası oluşturur.

Daha sonra, IoT hub'ınız için kayıtlı X.509 aygıtını simüle etmek için nasıl bir C# uygulaması oluşturacağınızı göstereceğiz. Simüle edilen cihazdan hub'ınıza sıcaklık ve nem değerleri göndereceğiz. Bu eğitimde, yalnızca cihaz uygulamasını oluşturacağız. Bu simüle aygıt tarafından gönderilen olaylara yanıt gönderecek IoT Hub hizmet uygulaması oluşturmak için okuyuculara bir alıştırma olarak bırakılır. C# uygulaması, [örnekler ve öğreticiler için test CA sertifikalarını yönetme](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)adımlarını izlediğinizi varsayar.

1. Visual Studio'yu açın, **yeni bir proje oluştur'u**seçin ve ardından Konsol **Uygulaması (.NET Framework)** proje şablonu'nu seçin. **İleri**’yi seçin.

1. **Yeni projenizi yapılandırın,** *projeyi SimulateX509Device*olarak adlandırın ve ardından **Oluştur'u**seçin.

   ![Visual Studio'da X.509 cihaz projesi oluşturun](./media/iot-hub-security-x509-get-started/create-device-project-vs2019.png)

1. Çözüm Gezgini'nde, **SimulateX509Device** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet'i**seçin.

1. **NuGet Paket**Yöneticisi'nde, **Gözat'ı** seçin ve **Microsoft.Azure.Devices.Client'ı**arayın ve seçin. **Yükle**’yi seçin.

   ![Visual Studio'da cihaz SDK NuGet paketi ekleyin](./media/iot-hub-security-x509-get-started/device-sdk-nuget.png)

    Bu adım, Azure IoT aygıtı SDK NuGet paketini ve bağımlılıklarını karşıdan yükler, yükler ve ekler.

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using System.Security.Cryptography.X509Certificates;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin:

    ```csharp
        private static int MESSAGE_COUNT = 5;
        private const int TEMPERATURE_THRESHOLD = 30;
        private static String deviceId = "<your-device-id>";
        private static float temperature;
        private static float humidity;
        private static Random rnd = new Random();
    ```

    >your_device_id _<_ yerine bir önceki bölümde kullandığınız dostu cihaz adını kullanın.

1. Sıcaklık ve nem için rasgele sayılar oluşturmak ve bu değerleri hub'a göndermek için aşağıdaki işlevi ekleyin:

    ```csharp
    static async Task SendEvent(DeviceClient deviceClient)
    {
        string dataBuffer;
        Console.WriteLine("Device sending {0} messages to IoTHub...\n", MESSAGE_COUNT);

        for (int count = 0; count < MESSAGE_COUNT; count++)
        {
            temperature = rnd.Next(20, 35);
            humidity = rnd.Next(60, 80);
            dataBuffer = string.Format("{{\"deviceId\":\"{0}\",\"messageId\":{1},\"temperature\":{2},\"humidity\":{3}}}", deviceId, count, temperature, humidity);
            Message eventMessage = new Message(Encoding.UTF8.GetBytes(dataBuffer));
            eventMessage.Properties.Add("temperatureAlert", (temperature > TEMPERATURE_THRESHOLD) ? "true" : "false");
            Console.WriteLine("\t{0}> Sending message: {1}, Data: [{2}]", DateTime.Now.ToLocalTime(), count, dataBuffer);

            await deviceClient.SendEventAsync(eventMessage);
        }
    }
    ```

1. Son olarak, yer tutucular _aygıt-id,_ _your-iot-hub-name_ve _mutlak-yol-to-your-device-pfx-dosyası_ **yerine, Ana** işlevine aşağıdaki kod satırlarını ekleyin.

    ```csharp
    try
    {
        var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "1234");
        var auth = new DeviceAuthenticationWithX509Certificate("<device-id>", cert);
        var deviceClient = DeviceClient.Create("<your-iot-hub-name>.azure-devices.net", auth, TransportType.Amqp_Tcp_Only);

        if (deviceClient == null)
        {
            Console.WriteLine("Failed to create DeviceClient!");
        }
        else
        {
            Console.WriteLine("Successfully created DeviceClient!");
            SendEvent(deviceClient).Wait();
        }

        Console.WriteLine("Exiting...\n");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```

   Bu kod, X.509 aygıtınızın bağlantı dizesini oluşturarak IoT hub'ınıza bağlanır. Başarılı bir şekilde bağlandıktan sonra, hub'a sıcaklık ve nem olayları gönderir ve yanıtını bekler.

1. Uygulamayı çalıştırın. Bu uygulama bir *.pfx* dosyasına eriştiğinden, bu uygulamayı bir yönetici olarak yürütmeniz gerekebilir.

   1. Visual Studio çözümlerini oluşturun.

   1. **Çalıştır'ı yönetici olarak**kullanarak yeni bir Komut İstemi penceresi açın.  

   1. Çözümünüzü içeren klasöre gidin ve ardından çözüm klasörü içindeki *hata ayıklama* yoluna gidin.

   1. Komut isteminden **SimulateX509Device.exe** uygulamasını çalıştırın.

   Cihazınızın hub'a başarıyla bağlanıp olayları gönderdiğini görmeniz gerekir.

   ![Cihaz uygulamasını çalıştır](./media/iot-hub-security-x509-get-started/device-app-success.png)

## <a name="next-steps"></a>Sonraki adımlar

IoT çözümünüzü n için güvenli hale getirmek hakkında daha fazla bilgi edinmek için bkz:

* [IoT Güvenlik En İyi Uygulamalar](../iot-fundamentals/iot-security-best-practices.md)

* [IoT Güvenlik Mimarisi](../iot-fundamentals/iot-security-architecture.md)

* [IoT dağıtımınızın güvenliğini sağlama](../iot-fundamentals/iot-security-deployment.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
