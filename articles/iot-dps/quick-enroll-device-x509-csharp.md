---
title: 'X.509 cihazını C kullanarak Azure Aygıt Sağlama Hizmetine kaydedin #'
description: Bu hızlı başlangıçta grup kayıtları kullanılmaktadır. Bu hızlı başlangıçta, X.509 aygıtlarını C# kullanarak Azure IoT Hub Aygıt Sağlama Hizmetine (DPS) kaydedin.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: csharp
ms.custom: mvc
ms.openlocfilehash: 64bc3921a606ab3211173b46b268ded53952c8bb
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75434655"
---
# <a name="quickstart-enroll-x509-devices-to-the-device-provisioning-service-using-c"></a>Hızlı başlangıç: C# kullanarak X.509 cihazlarını Cihaz Sağlama Hizmeti'ne kaydetme

[!INCLUDE [iot-dps-selector-quick-enroll-device-x509](../../includes/iot-dps-selector-quick-enroll-device-x509.md)]

Bu hızlı başlangıçta C# programlama özelliklerini kullanarak ara veya kök CA X.509 sertifikalarını kullanan bir [Kayıt grubu](concepts-service.md#enrollment-group) oluşturmayı öğreneceksiniz. Kayıt grubu, [.NET için Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-csharp) ve örnek c# .NET Core uygulaması kullanılarak oluşturulur. Kayıt grubu, sertifika zincirlerinde ortak imzalama sertifikasını paylaşan cihazlar için sağlama hizmetine erişimi denetler. Daha fazla bilgi edinmek için bkz. [X.509 sertifikalarıyla sağlama hizmetine cihaz erişimini denetleme](./concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates). Azure IoT Hub ve Cihaz Sağlama Hizmeti ile X.509 sertifikası tabanlı Ortak Anahtar Altyapısı'nı (PKI) kullanma hakkında daha fazla bilgi için bkz. [X.509 CA sertifikası güvenliğine genel bakış](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

Bu hızlı başlangıç, zaten bir IoT hub'ı ve Aygıt Sağlama Hizmeti örneği oluşturmanızı bekler. Bu kaynakları henüz oluşturmadıysanız, bu makaleye devam etmeden önce [Azure portalı ile IoT Hub Aygıt Sağlama Hizmeti](./quick-setup-auto-provision.md) Ayarla'yı tamamlayın.

Bu makaledeki adımlar hem Windows hem de Linux bilgisayarlarda çalışsa da, bu makalede bir Windows geliştirme bilgisayarı kullanır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2019](https://www.visualstudio.com/vs/)’u yükleyin.
* Yükleyin [.NET Çekirdek SDK](https://www.microsoft.com/net/download/windows).
* [Git'i yükleyin.](https://git-scm.com/download/)

## <a name="prepare-test-certificates"></a>Test sertifikalarını hazırlama

Bu hızlı başlangıç için ara veya kök CA X.509 sertifikasının ortak bölümünü içeren bir .pem veya .cer dosyasına sahip olmanız gerekir. Bu sertifikanın sağlama hizmetinize yüklenmesi ve hizmet tarafından doğrulanması gerekir.

[Azure IoT C SDK,](https://github.com/Azure/azure-iot-sdk-c) bir X.509 sertifika zinciri oluşturmanıza, bu zincirden bir kök veya ara sertifika yüklemenize ve sertifikayı doğrulamak için hizmetle birlikte bulundurma kanıtı yapmanıza yardımcı olabilecek test aracı içerir.

> [!CAUTION]
> Yalnızca geliştirme sınayı için SDK aracıyla oluşturulan sertifikaları kullanın.
> Bu sertifikaları üretimde kullanmayın.
> Bunlar, 30 gün sonra süresi dolan *1234*gibi sabit kodlu parolalar içerir.
> Üretim ortamında kullanıma uygun sertifikalar almayı öğrenmek için, Azure IoT Hub belgelerinde [X.509 CA sertifikası alma](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) konusuna bakın.
>

Sertifika oluşturmak için bu test aracını kullanmak için aşağıdaki adımları yapın:

1. Azure IoT C SDK'nın [en son sürümü](https://github.com/Azure/azure-iot-sdk-c/releases/latest) için etiket adını bulun.

2. Komut istemi veya Git Bash kabuğu açın ve makinenizdeki çalışma klasörüne geçin. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunun en son sürümünde klonlamak için aşağıdaki komutları çalıştırın. Önceki adımda bulduğunuz etiketi `-b` parametre nin değeri olarak kullanın:

    ```cmd/sh
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

   Test araçları kopyaladığınız deponun *azure-iot-sdk-c/tools/CACertificates* dizininde bulunur.

3. [Örnekler ve öğreticiler için test amaçlı CA sertifikalarını yönetme](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) adımlarını izleyin.

C SDK'daki takıma ek olarak, *.NET için Microsoft Azure IoT SDK'daki* [Grup sertifikası doğrulama örneği,](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/provisioning/Samples/service/GroupCertificateVerificationSample) varolan bir X.509 ara veya kök CA sertifikasıyla C#'da sahip olma kanıtının nasıl yapılacağını gösterir.

## <a name="get-the-connection-string-for-your-provisioning-service"></a>Sağlama hizmetinizin bağlantı dizesini alma

Bu hızlı başlangıçtaki örnek için sağlama hizmetinizin bağlantı dizesine ihtiyacınız vardır.

1. Azure portalında oturum açın, **Tüm kaynakları**ve ardından Cihaz Sağlama Hizmeti'ni seçin.

1. **Paylaşılan erişim ilkelerini**seçin ve özelliklerini açmak için kullanmak istediğiniz erişim ilkesini seçin. **Access İlkesi'nde**birincil anahtar bağlantı dizesini kopyalayın ve kaydedin.

    ![Portaldan sağlama hizmeti bağlantı dizesini alma](media/quick-enroll-device-x509-csharp/get-service-connection-string-vs2019.png)

## <a name="create-the-enrollment-group-sample"></a>Kayıt grubu örneğini oluşturma 

Bu bölümde, sağlama hizmetinize bir kayıt grubu ekleyen bir .NET Core konsol uygulamasının nasıl oluşturulutur. Biraz değişiklikle, kayıt grubunu eklemek üzere bir [Windows IoT Core](https://developer.microsoft.com/en-us/windows/iot) konsol uygulaması oluşturmak için de bu adımları izleyebilirsiniz. IoT Core ile geliştirme hakkında daha fazla bilgi edinmek için bkz. [Windows IoT Core geliştirici belgeleri](https://docs.microsoft.com/windows/iot-core/).

1. Visual Studio'u açın ve **yeni bir proje oluştur'u**seçin. **Yeni bir proje oluştur'da,** C# proje şablonu için Konsol **Uygulaması'nı (.NET Core)** seçin ve **İleri'yi**seçin.

1. Project *CreateEnrollmentGroup'u*adlandırın ve ardından **Oluştur'a**basın.

    ![Visual C# Windows Classic Desktop projesini yapılandır](media//quick-enroll-device-x509-csharp/configure-app-vs2019.png)

1. Çözüm Visual Studio'da, **Çözüm Gezgini** bölmesinde açıldığında **CreateEnrollmentGroup** projesini sağ tıklatın ve ardından **NuGet Paketlerini Yönet'i**seçin.

1. **NuGet Package Manager'da** **Gözat'ı,** Aramayı ve **Microsoft.Azure.Devices.Provisioning.Service'i**seçin ve ardından **Yükle'ye**basın.

    ![NuGet Paket Yöneticisi penceresi](media//quick-enroll-device-x509-csharp/add-nuget.png)

   Bu adım, [Azure IoT Sağlama Hizmeti İstemi SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) NuGet paketini ve bağımlılıklarını karşıdan yükler, yükler ve ekler.

1. Aşağıdaki `using` ifadeleri en üstteki diğer `using` ifadelerden sonra `Program.cs`ekleyin:

   ```csharp
   using System.Security.Cryptography.X509Certificates;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Provisioning.Service;
   ```

1. Aşağıdaki alanları `Program` sınıfa ekleyin ve listelenen değişiklikleri yapın.  

   ```csharp
   private static string ProvisioningConnectionString = "{ProvisioningServiceConnectionString}";
   private static string EnrollmentGroupId = "enrollmentgrouptest";
   private static string X509RootCertPath = @"{Path to a .cer or .pem file for a verified root CA or intermediate CA X.509 certificate}";
   ```

   * `ProvisioningServiceConnectionString` Yer tutucu değerini, kaydı oluşturmak istediğiniz sağlama hizmetinin bağlantı dizesiyle değiştirin.

   * `X509RootCertPath` Yer tutucu değerini .pem veya .cer dosyasına giden yol ile değiştirin. Bu dosya, daha önce yüklenen ve sağlama hizmetiile doğrulanmış bir ara veya kök CA X.509 sertifikasının ortak bölümünü temsil eder.

   * İsteğe bağlı olarak `EnrollmentGroupId` değeri değiştirebilirsiniz. Dize yalnızca küçük harflerden ve kısa çizgilerden oluşabilir.

   > [!IMPORTANT]
   > Üretim kodunda, güvenlikle ilgili aşağıdaki noktalara dikkat edin:
   >
   > * Sağlama hizmeti yöneticisi için bağlantı dizesinin sabit kodlanması en iyi güvenlik yöntemlerine uygun değildir. Bunun yerine, bağlantı dizesi güvenli bir şekilde, örneğin güvenli yapılandırma dosyasının içinde veya kayıt defterinin içinde tutulmalıdır.
   > * İmzalama sertifikasının yalnızca ortak bölümünü karşıya yüklediğinizden emin olun. Özel anahtarları içeren .pfx (PKCS12) veya .pem dosyalarını asla sağlama hizmetine yüklemeyin.

1. `Program` Sınıfa aşağıdaki yöntemi ekleyin. Bu kod bir kayıt grubu girişi `CreateOrUpdateEnrollmentGroupAsync` oluşturur `ProvisioningServiceClient` ve ardından kayıt grubunu sağlama hizmetine eklemek için yöntemi çağırır.

   ```csharp
   public static async Task RunSample()
   {
       Console.WriteLine("Starting sample...");
 
       using (ProvisioningServiceClient provisioningServiceClient =
               ProvisioningServiceClient.CreateFromConnectionString(ProvisioningConnectionString))
       {
           #region Create a new enrollmentGroup config
           Console.WriteLine("\nCreating a new enrollmentGroup...");
           var certificate = new X509Certificate2(X509RootCertPath);
           Attestation attestation = X509Attestation.CreateFromRootCertificates(certificate);
           EnrollmentGroup enrollmentGroup =
                   new EnrollmentGroup(
                           EnrollmentGroupId,
                           attestation)
                   {
                       ProvisioningStatus = ProvisioningStatus.Enabled
                   };
           Console.WriteLine(enrollmentGroup);
           #endregion
 
           #region Create the enrollmentGroup
           Console.WriteLine("\nAdding new enrollmentGroup...");
           EnrollmentGroup enrollmentGroupResult =
               await provisioningServiceClient.CreateOrUpdateEnrollmentGroupAsync(enrollmentGroup).ConfigureAwait(false);
           Console.WriteLine("\nEnrollmentGroup created with success.");
           Console.WriteLine(enrollmentGroupResult);
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

## <a name="run-the-enrollment-group-sample"></a>Kayıt grubu örneğini çalıştırma
  
Örneği Visual Studio'da çalıştırarak kayıt grubunu oluşturun. Komut İstemi penceresi görüntülenir ve onay iletilerini göstermeye başlar. Başarılı oluşturmada, Komut İstemi penceresi yeni kayıt grubunun özelliklerini görüntüler.

Kayıt grubunun oluşturulduğunu doğrulayabilirsiniz. Aygıt Sağlama Hizmeti özetine gidin ve **kayıtları Yönet'i**seçin ve ardından **Kayıt Grupları'nı**seçin. Örnekte kullandığınız kayıt kimliğine karşılık gelen yeni bir kayıt girdisi görmelisiniz.

![Portaldaki kayıt özellikleri](media/quick-enroll-device-x509-csharp/verify-enrollment-portal-vs2019.png)

Sertifika parmak izini ve girişin diğer özelliklerini doğrulamak için girişi seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

C# hizmet örneğini keşfetmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Aksi takdirde, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Bilgisayarınızdaki C# örnek çıkış penceresini kapatın.

1. Azure portalında Cihaz Sağlama hizmetinize gidin, **kayıtları Yönet'i**seçin ve ardından **Kayıt Grupları'nı**seçin. Bu hızlı başlatı kullanarak oluşturduğunuz kayıt girişi için *Kayıt Kimliği'ni* seçin ve **Sil'e**basın.

1. Azure portalındaki Aygıt Sağlama hizmetinizden, **Sertifikalar'ı**seçin, bu hızlı başlangıç için yüklediğiniz sertifikayı seçin ve **Sertifika Ayrıntıları'nın**en üstündeki **Sil'e** basın.  

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure IoT Hub Aygıt Sağlama Hizmeti'ni kullanarak X.509 ara veya kök CA sertifikası için bir kayıt grubu oluşturdunuz. Cihaz sağlama hakkında ayrıntılı bilgi edinmek için Azure portalında Cihaz Sağlama Hizmeti ayarları öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure IoT Hub Cihazı Sağlama Hizmeti öğreticileri](./tutorial-set-up-cloud.md)
