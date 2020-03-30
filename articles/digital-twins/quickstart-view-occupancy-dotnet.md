---
title: 'Quickstart: Uygun oda bulun - Azure Digital Twins | Microsoft Dokümanlar'
description: Bu hızlı başlangıçta iki .NET Core örnek uygulaması çalıştırarak bir Azure Digital Twins alanına sanal hareket ve karbondioksit telemetri verileri göndereceksiniz. Burada hedefiniz, veriler bulutta işlendikten sonra Yönetim API'leriyle temiz havaya sahip olan uygun odaları bulmak olacak.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc seodec18
ms.date: 01/10/2020
ms.openlocfilehash: de8611e53339d38a03836bc9272ad42ff88a59b8
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79371435"
---
# <a name="quickstart-find-available-rooms-by-using-azure-digital-twins"></a>Hızlı başlangıç: Azure Digital Twins kullanarak kullanılabilir odaları bulun

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

Azure Digital Twins hizmeti, fiziksel ortamınızın dijital görüntüsünü yeniden oluşturmanıza olanak tanır. Bu işlemin ardından ortamınızdaki olaylarla ilgili bildirimler alabilir ve verdiğiniz yanıtları özelleştirebilirsiniz.

Bu hızlı başlangıç, hayali bir ofis binasını dijitalleştirmek için [bir çift .NET örneği](https://github.com/Azure-Samples/digital-twins-samples-csharp) kullanır. O binada nasıl uygun oda bulabileceğinizi gösteriyor. Digital Twins ile birçok sensörü ortamınızla ilişkilendirebilirsiniz. Ayrıca, mevcut odanızın hava kalitesinin karbondioksit için simüle edilmiş bir sensör yardımıyla en uygun olup olmadığını da öğrenebilirsiniz. Örnek uygulamalardan biri, bu senaryoyu görselleştirmenize yardımcı olmak için rasgele sensör verileri oluşturur.

Aşağıdaki video hızlı başlatma kurulum özetler:

>[!VIDEO https://www.youtube.com/embed/1izK266tbMI]

## <a name="prerequisites"></a>Ön koşullar

1. Azure hesabınız yoksa, başlamadan önce [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

1. Bu hızlı başlatmada çalıştırdığınız iki konsol uygulaması C# kullanılarak yazılır. [.NET Core SDK sürüm 2.1.403 ve üzerini](https://www.microsoft.com/net/download) geliştirme makinenize yükleyin. .NET Core SDK yüklüyse, geliştirme makinenizde C#'nin geçerli sürümünü doğrulayın. Komut `dotnet --version` isteminde çalıştırın.

1. Örnek [C# projesini](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip)indirin. Dijital ikizler-örnekler-csharp-master.zip arşivini ayıklayın.

## <a name="create-a-digital-twins-instance"></a>Digital Twins örneği oluşturma

Bu bölümdeki adımları izleyerek [portalda](https://portal.azure.com) yeni bir Dijital İkizler örneği oluşturun.

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="set-permissions-for-your-app"></a>Uygulamanızın izinlerini ayarlama

Bu bölüm, Dijital İkizler örneğinize erişebilmek için örnek uygulamanızı Azure Etkin Dizin'e (Azure AD) kaydeder. Azure AD uygulama kaydınız zaten varsa, örneğiniz için yeniden kullanın. Bu bölümde açıklandığı gibi yapılandırıldığından emin olun.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="build-application"></a>Uygulama oluşturma

Bu adımları izleyerek doluluk uygulamasını oluşturun.

1. Bir komut istemi açın. Dosyalarınızın `digital-twins-samples-csharp-master.zip` ayıklandığı klasöre gidin.
1. `cd occupancy-quickstart/src` öğesini çalıştırın.
1. `dotnet restore` öğesini çalıştırın.
1. [appSettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/appSettings.json) dosyasını düzenleyerek aşağıdaki değişkenleri güncelleştirin:
    - **ClientId**: Önceki bölümde belirtilen Azure AD uygulama kaydınızın Uygulama Kimliğini girin.
    - **Kiracı**: Azure AD kiracınızın dizin kimliğini girin, önceki bölümde de belirtin.
    - **BaseUrl**: Dijital İkizler örneğinizin Yönetim API `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`URL'si formatındadır. Bu URL'deki yer tutucuları önceki bölümdeki örneğinize ait değerlerle değiştirin.

    Güncelleştirilen dosyayı kaydedin.

## <a name="provision-graph"></a>Grafı sağlama

Bu adım, Dijital İkizler mekansal grafiğinizi aşağıdakilerle birlikte karşılar:

- Birkaç boşluk.
- Bir cihaz.
- İki sensör.
- Özel bir işlev.
- Bir rol görevi.

Uzamsal grafik [provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) dosyası kullanılarak sağlanır.

1. `dotnet run ProvisionSample` öğesini çalıştırın.

    >[!NOTE]
    >Aygıt Girişi Azure CLI aracı, kullanıcının azure AD kimliğine doğrulanması için kullanılır. Kullanıcı, [Microsoft oturum açma](https://microsoft.com/devicelogin) sayfasını kullanarak kimlik doğrulaması için belirli bir kod girmelidir. Kod girilen sonra, kimlik doğrulaması için adımları izleyin. Araç çalışırken kullanıcının kimlik doğrulaması yapması gerekir.

    >[!TIP]
    > Bu adımı çalıştırdığınızda, aşağıdaki hata iletisi görüntülendiğinde değişkenlerinizin doğru kopyalandığından emin olun:`EXIT: Unexpected error: The input is not a valid Base-64 string ...`

1. Sağlama adımı birkaç dakika sürebilir. Ayrıca, Dijital İkizler örneğiniz içinde bir IoT Hub'ı da karşılar. IoT Hub Durum=`Running`gösterene kadar döngüler.

    [![Örneği sağlama - Durum=Çalıştırma](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-provision-sample.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-provision-sample.png#lightbox)

1. Yürütmenin sonunda, aygıt simülatörü örneğinde kullanılmak üzere aygıtın kopyasını kopyalayın. `ConnectionString` Yalnızca bu resimde özetlenen dizekopyalayın.

    [![Bağlantı dizesini kopyalayın](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-connection-string.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-connection-string.png#lightbox)

    >[!TIP]
    > [Azure Dijital İkizler Grafik Görüntüleyici'yi](https://github.com/Azure/azure-digital-twins-graph-viewer)kullanarak uzamsal grafiğinizi görüntüleyebilir ve değiştirebilirsiniz.

Konsol penceresini daha sonra tekrar kullanılmak üzere açık tutun.

## <a name="send-sensor-data"></a>Sensör verilerini gönderme

Bu adımları izleyerek sensör simülatörü cihaz uygulamasını oluşturun ve çalıştırın.

1. Yeni bir komut istemi açın. `digital-twins-samples-csharp-master` Klasörde indirdiğiniz projeye gidin.
1. `cd device-connectivity` öğesini çalıştırın.
1. `dotnet restore` öğesini çalıştırın.
1. **DeviceConnectionString'i** önceki `ConnectionString`yle güncelleştirmek için [appsettings.json'u](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json) güncelleştirin. Güncelleştirilen dosyayı kaydedin.
1. Sensör `dotnet run` verilerini göndermeye başlamak için çalıştırın. Aşağıdaki resimde gösterildiği gibi Azure Digital Twins'e gönderilir.

     [![Cihaz Bağlantısı](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-device-connectivity.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-device-connectivity.png#lightbox)

1. Bir sonraki adım eylemiyle sonuçları yan yana görüntüleyebilmeniz için bu simülatörün çalışmasına izin verin. Bu pencere, Digital Twins'e gönderilen simüle edilmiş sensör verilerini gösterir. Bir sonraki adım, temiz hava içeren mevcut odaları bulmak için gerçek zamanlı olarak sorgular.

    >[!TIP]
    > Bu adımı çalıştırdığınızda, `DeviceConnectionString` aşağıdaki hata iletisi görünüyorsa doğru şekilde kopyalandığından emin olun:`EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Temiz havaya sahip olan odaları bulma

Sensör örneği iki sensör için rastgele veri değerlerini simüle eder. Hareket ve karbondioksit. Temiz hava içeren mevcut alanlar, odada bulunmadan numunede tanımlanır. Ayrıca 1000 ppm'in altındaki karbondioksit seviyesiyle de tanımlanırlar. Durum yerine getirilmezse, alan mevcut değildir veya hava kalitesi düşüktür.

1. Sağlama adımını çalıştırmak için kullandığınız komut istemini daha önce açın.
1. `dotnet run GetAvailableAndFreshSpaces` öğesini çalıştırın.
1. Bu komut istemi ve sensör veri komutu yan yana istemi bak.

    Sensör veri komutu komutu, dijital ikizlere her beş saniyede bir simüle hareket ve karbondioksit verisi gönderir. Diğer komut istemi, rasgele simüle edilmiş verilere dayalı temiz hava içeren mevcut odaları bulmak için grafiği gerçek zamanlı olarak okur. En son gönderilen sensör verilerine dayanarak bu koşullardan birini neredeyse gerçek zamanlı olarak görüntüler:
   - `Room is available and air is fresh`
   - `Room is not available or air quality is poor`

     [![Temiz havaya sahip olan odaları alma](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-get-available.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-get-available.png#lightbox)

Bu hızlı başlatmada ne olduğunu ve API'lerin ne olarak adlandırıldığını anlamak `digital-twins-samples-csharp`için, visual studio [code](https://code.visualstudio.com/Download) project'te bulunan kod çalışma alanı projesini açın. Aşağıdaki komutu kullanın:

```cmd
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

Öğreticiler kodun derinliklerine iner. Yapılandırma verilerini nasıl değiştireceğimiz ve API'lerin ne adlar adlandırılmasını öğretirler. Yönetim API'leri hakkında daha fazla bilgi için Digital Twins Swagger sayfanıza gidin:

```URL
https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/swagger
```

| Adı | Şununla değiştir |
| --- | --- |
| YOUR_INSTANCE_NAME | Dijital İkizler örneğinizin adı |
| YOUR_LOCATION | Örneğinizin barındırıldığı sunucu bölgesi |

Veya kolaylık sağlamak için, [Digital Twins Swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger)göz atın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiler nasıl hakkında ayrıntılı olarak gidin:

- Tesis yöneticileri için yolcu verimliliğini artırmak için bir uygulama oluşturun.
- Binayı daha verimli bir şekilde işletin.

Öğreticilere devam etmek için, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi düşünmüyorsanız, bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin.

1. Örnek deposunu karşıdan yüklediğinizde oluşturulan klasörü silin.
1. [Azure portalında](https://portal.azure.com)soldaki menüden **Tüm kaynakları**seçin. Ardından Dijital İkizler kaynağınızı seçin. **Tüm kaynaklar** bölmesinin üst kısmında **Sil'i**seçin.

    > [!TIP]
    > Dijital İkizler örneğini silmekonusunda daha önce sorun yaşadıysanız, düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma sunuldu. Lütfen örneğini yeniden silmeyi deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç, Dijital İkizler'in iyi çalışma koşullarına sahip odaları bulmak için nasıl kullanılabileceğini göstermek için basit bir senaryo ve örnek uygulamalar kullandı. Bu senaryonun ayrıntılı analizi için şu öğreticiyi okuyun:

>[!div class="nextstepaction"]
>[Öğretici: Azure Digital Twins'i dağıtma ve uzamsal graf yapılandırma](tutorial-facilities-setup.md)
