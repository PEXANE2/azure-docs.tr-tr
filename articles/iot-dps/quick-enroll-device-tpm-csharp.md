---
title: 'TPM aygıtını C kullanarak Azure Aygıt Sağlama Hizmetine kaydedin #'
description: Quickstart - C# hizmeti SDK'yı kullanarak TPM cihazını Azure IoT Hub Aygıt Sağlama Hizmetine (DPS) kaydedin. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: csharp
ms.custom: mvc
ms.openlocfilehash: ee1b803459e0c81b86021b617a29e0b29ee19909
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74976851"
---
# <a name="quickstart-enroll-tpm-device-to-iot-hub-device-provisioning-service-using-c-service-sdk"></a>Quickstart: C# hizmeti SDK kullanarak TPM cihazını IoT Hub Aygıt Sağlama Hizmetine kaydedin

[!INCLUDE [iot-dps-selector-quick-enroll-device-tpm](../../includes/iot-dps-selector-quick-enroll-device-tpm.md)]

Bu makalede, [C# Service SDK](https://github.com/Azure/azure-iot-sdk-csharp) ve örnek c# .NET Core uygulamasını kullanarak Azure IoT Hub Aygıt Sağlama Hizmeti'ndeki bir TPM aygıtı için tek bir kaydın nasıl programlanabilir bir şekilde oluşturulacak olduğu gösterilmektedir. Bu bireysel kayıt girişini kullanarak, simüle edilmiş bir TPM aygıtını sağlama hizmetine isteğe bağlı olarak kaydedebilirsiniz. Bu adımlar hem Windows hem de Linux bilgisayarlarda çalışsa da, bu makalede bir Windows geliştirme bilgisayarı kullanır.

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

1. Bilgisayarınızda [Visual Studio 2019](https://www.visualstudio.com/vs/) yüklü olduğunuzu doğrulayın.

1. Bilgisayarınızda [.NET Core SDK](https://www.microsoft.com/net/download/windows) yüklü olduğunuzu doğrulayın.

1. Devam etmeden önce [Azure portalıyla IoT Hub Aygıt Sağlama Hizmeti'ni ayarlama](./quick-setup-auto-provision.md) adımlarını tamamlayın.

1. (İsteğe bağlı) Bu hızlı başlatmanın sonunda simüle edilmiş bir aygıtı kaydetmek istiyorsanız, Oluştur'daki yordamı izleyin ve C# cihazı SDK'yı kullanarak aygıt için bir onay anahtarı aldığınız adıma kadar [simüle edilmiş bir TPM aygıtı nı uygulayın.](quick-create-simulated-device-tpm-csharp.md) Onay anahtarını, kayıt kimliğini ve isteğe bağlı olarak aygıt kimliğini kaydedin, çünkü bunları daha sonra bu hızlı başlangıçta kullanmanız gerekir.

   > [!NOTE]
   > Azure portalını kullanarak tek bir kayıt oluşturmak için adımları izlemeyin.

## <a name="get-the-connection-string-for-your-provisioning-service"></a>Sağlama hizmetinizin bağlantı dizesini alma

Bu hızlı başlangıçtaki örnek için sağlama hizmetinizin bağlantı dizesine ihtiyacınız vardır.

1. Azure portalında oturum açın, **Tüm kaynakları**ve ardından Cihaz Sağlama Hizmeti'ni seçin.

1. **Paylaşılan erişim ilkelerini**seçin, ardından özelliklerini açmak için kullanmak istediğiniz erişim ilkesini seçin. **Access İlkesi'nde**birincil anahtar bağlantı dizesini kopyalayın ve kaydedin.

    ![Portaldan sağlama hizmeti bağlantı dizesini alma](media/quick-enroll-device-tpm-csharp/get-service-connection-string-vs2019.png)

## <a name="create-the-individual-enrollment-sample"></a>Bireysel kayıt örneğini oluşturma

Bu bölümde, bir TPM aygıtı için tek tek bir kayıt ekleyen bir .NET Core konsol uygulamasının nasıl oluşturulutamamolduğu, sağlama hizmetinize nasıl eklenilir. Biraz değişiklikle, bireysel kayıt eklemek üzere bir [Windows IoT Core](https://developer.microsoft.com/en-us/windows/iot) konsol uygulaması oluşturmak için de bu adımları izleyebilirsiniz. IoT Core ile geliştirme hakkında daha fazla bilgi edinmek için [Windows IoT Core geliştirici belgelerine](https://docs.microsoft.com/windows/iot-core/)bakın.

1. Visual Studio'u açın ve **yeni bir proje oluştur'u**seçin. **Yeni bir proje oluştur'da,** C# için Konsol Uygulaması **(.NET Core)** proje şablonu seçin ve **İleri'yi**seçin.

1. *Project CreateTpmEnrollment'ı*adlandırın ve **Oluştur'a**basın.

    ![Visual C# Windows Classic Desktop projesini yapılandır](media/quick-enroll-device-tpm-csharp/configure-tpm-app-vs2019.png)

1. Çözüm Visual Studio'da, **Çözüm Gezgini** bölmesinde açıldığında **CreateTpmEnrollment** projesine sağ tıklayın. **NuGet Paketlerini Yönet'i**seçin.

1. **NuGet Package Manager'da** **Gözat'ı,** Aramayı ve **Microsoft.Azure.Devices.Provisioning.Service'i**seçin ve ardından **Yükle'ye**basın.

   ![NuGet Paket Yöneticisi penceresi](media//quick-enroll-device-tpm-csharp/add-nuget.png)

   Bu adım, [Azure IoT Sağlama Hizmeti İstemi SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) NuGet paketini ve bağımlılıklarını karşıdan yükler, yükler ve ekler.

1. Aşağıdaki `using` ifadeleri en üstteki diğer `using` ifadelerden sonra `Program.cs`ekleyin:
  
   ```csharp
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Provisioning.Service;
   ```

1. Aşağıdaki değişiklikleri yaparak `Program` sınıfa aşağıdaki alanları ekleyin.

   ```csharp
   private static string ProvisioningConnectionString = "{ProvisioningServiceConnectionString}";
   private const string RegistrationId = "sample-registrationid-csharp";
   private const string TpmEndorsementKey =
       "AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAxsj2gUS" +
       "cTk1UjuioeTlfGYZrrimExB+bScH75adUMRIi2UOMxG1kw4y+9RW/IVoMl4e620VxZad0ARX2gUqVjYO7KPVt3d" +
       "yKhZS3dkcvfBisBhP1XH9B33VqHG9SHnbnQXdBUaCgKAfxome8UmBKfe+naTsE5fkvjb/do3/dD6l4sGBwFCnKR" +
       "dln4XpM03zLpoHFao8zOwt8l/uP3qUIxmCYv9A7m69Ms+5/pCkTu/rK4mRDsfhZ0QLfbzVI6zQFOKF/rwsfBtFe" +
       "WlWtcuJMKlXdD8TXWElTzgh7JS4qhFzreL0c1mI0GCj+Aws0usZh7dLIVPnlgZcBhgy1SSDQMQ==";
       
   // Optional parameters
   private const string OptionalDeviceId = "myCSharpDevice";
   private const ProvisioningStatus OptionalProvisioningStatus = ProvisioningStatus.Enabled;
   ```

   * `ProvisioningServiceConnectionString` Yer tutucu değerini, kaydı oluşturmak istediğiniz sağlama hizmetinin bağlantı dizesiyle değiştirin.

   * İsteğe bağlı olarak kayıt kimliğini, onay anahtarını, cihaz kimliğini ve sağlama durumunu değiştirebilirsiniz.

   * Bu hızlı başlatmayı [C# cihazı SDK kullanarak simüle](quick-create-simulated-device-tpm-csharp.md) edilmiş bir TPM aygıtı nı oluşturarak simüle edilmiş bir tpm aygıtını kullanıyorsanız, onay anahtarını ve kayıt kimliğini bu hızlı başlatmada belirttiğiniz değerlerle değiştirin. Aygıt kimliğini bu hızlı başlatmada önerilen değerle değiştirebilir, kendi değerinizi kullanabilir veya bu örnekteki varsayılan değeri kullanabilirsiniz.

1. `Program` Sınıfa aşağıdaki yöntemi ekleyin.  Bu kod tek tek kayıt girişi `CreateOrUpdateIndividualEnrollmentAsync` oluşturur `ProvisioningServiceClient` ve daha sonra tek tek kaydı sağlama hizmetine eklemek için yöntem çağırır.

   ```csharp
   public static async Task RunSample()
   {
       Console.WriteLine("Starting sample...");

       using (ProvisioningServiceClient provisioningServiceClient =
               ProvisioningServiceClient.CreateFromConnectionString(ProvisioningConnectionString))
       {
           #region Create a new individualEnrollment config
           Console.WriteLine("\nCreating a new individualEnrollment...");
           Attestation attestation = new TpmAttestation(TpmEndorsementKey);
           IndividualEnrollment individualEnrollment =
                   new IndividualEnrollment(
                           RegistrationId,
                           attestation);

           // The following parameters are optional. Remove them if you don't need them.
           individualEnrollment.DeviceId = OptionalDeviceId;
           individualEnrollment.ProvisioningStatus = OptionalProvisioningStatus;
           #endregion

           #region Create the individualEnrollment
           Console.WriteLine("\nAdding new individualEnrollment...");
           IndividualEnrollment individualEnrollmentResult =
               await provisioningServiceClient.CreateOrUpdateIndividualEnrollmentAsync(individualEnrollment).ConfigureAwait(false);
           Console.WriteLine("\nIndividualEnrollment created with success.");
           Console.WriteLine(individualEnrollmentResult);
           #endregion
        
       }
   }
   ```

1. Son olarak, yöntemin `Main` gövdesini aşağıdaki satırlarla değiştirin:

   ```csharp
   RunSample().GetAwaiter().GetResult();
   Console.WriteLine("\nHit <Enter> to exit ...");
   Console.ReadLine();
   ```

1. Çözümü derleyin.

## <a name="run-the-individual-enrollment-sample"></a>Bireysel kayıt örneğini çalıştırma
  
Örneği Visual Studio'da çalıştırarak TPM cihazınızın bireysel kaydını oluşturun.

Komut İstemi penceresi görüntülenir ve onay iletilerini göstermeye başlar. Başarılı oluşturmada, Komut İstemi penceresi yeni tek tek kaydın özelliklerini görüntüler.

Tek tek kaydın oluşturulduğunu doğrulayabilirsiniz. Aygıt Sağlama Hizmeti özetine gidin ve **Kayıtları Yönet'i**seçin ve ardından **Bireysel Kayıtları**seçin. Örnekte kullandığınız kayıt kimliğine karşılık gelen yeni bir kayıt girdisi görmelisiniz.

![Portaldaki kayıt özellikleri](media/quick-enroll-device-tpm-csharp/verify-enrollment-portal-vs2019.png)

Girişiçin onay anahtarını ve diğer özellikleri doğrulamak için girişi seçin.

C# cihazı SDK hızlı başlat'ı [kullanarak simüle edilmiş bir TPM aygıtı oluşturma ve sağlama](quick-create-simulated-device-tpm-csharp.md) adımlarını takip ediyorsanız, simüle edilmiş aygıtınızı kaydetmek için bu hızlı başlatmada kalan adımları kullanmaya devam edebilirsiniz. Azure Portal'ı kullanarak bireysel kayıt oluşturma adımlarını atladığınızdan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

C# hizmet örneğini keşfetmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Aksi takdirde, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Bilgisayarınızdaki C# örnek çıkış penceresini kapatın.

1. Azure portalında Cihaz Sağlama hizmetinize gidin, **Kayıtları Yönet'i**seçin ve ardından **Bireysel Kayıtlar** sekmesini seçin. Bu hızlı başlangıcı kullanarak oluşturduğunuz kayıt girişi için *Kayıt Kimliği'nin* yanındaki onay kutusunu seçin ve bölmenin üst kısmındaki **Sil** düğmesine basın.

1. Benzetimli bir TPM aygıtı oluşturmak için [C# cihazı SDK'yı kullanarak simüle edilmiş bir TPM aygıtı oluşturma ve sağlama](quick-create-simulated-device-tpm-csharp.md) adımlarını izlediyseniz, aşağıdaki adımları yapın:

    1. TPM simülatör penceresini ve simülasyon cihazının örnek çıkış penceresini kapatın.

    1. Azure Portal'da, cihazınızın sağlandığı IoT Hub'ına gidin. **Explorers**altındaki menüde , **IoT aygıtlarını**seçin , bu hızlı başlatmada kaydettiğiniz aygıtın *AYGıT Kimliğinin* yanındaki onay kutusunu seçin ve ardından bölmenin üst kısmındaki **Sil** düğmesine basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir TPM aygıtı için programlı bir şekilde tek bir kayıt girişi oluşturdunuz. İsteğe bağlı olarak, bilgisayarınızda TPM simüle edilmiş bir aygıt oluşturdunuz ve Azure IoT Hub Aygıt Sağlama Hizmeti'ni kullanarak IoT hub'ınıza sokuldum. Cihaz sağlama hakkında ayrıntılı bilgi edinmek için Azure portalında Cihaz Sağlama Hizmeti ayarları öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure IoT Hub Cihazı Sağlama Hizmeti öğreticileri](./tutorial-set-up-cloud.md)
