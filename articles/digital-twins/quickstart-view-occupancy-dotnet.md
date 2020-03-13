---
title: 'Hızlı başlangıç: kullanılabilir odaları bulma-Azure dijital TWINS | Microsoft Docs'
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
ms.openlocfilehash: 6c9c5df27f4a361e534bac2fe21b2c470f8d0186
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79240691"
---
# <a name="quickstart-find-available-rooms-by-using-azure-digital-twins"></a>Hızlı Başlangıç: Azure dijital İkizlerini kullanarak kullanılabilir odaları bulun.

Azure dijital İkizlerini service, fiziksel ortamınızın dijital bir görüntüsünü yeniden oluşturmak sağlar. Bu işlemin ardından ortamınızdaki olaylarla ilgili bildirimler alabilir ve verdiğiniz yanıtları özelleştirebilirsiniz.

Bu hızlı başlangıç, bir sanal Office oluşturmayı dijital olarak oluşturmak için [bir çift .net örneği](https://github.com/Azure-Samples/digital-twins-samples-csharp) kullanır. Bu yapı içinde kullanılabilir odaları nasıl gösterir. Dijital İkizlerini kullanmaya birçok sensörlerden ortamınız ile ilişkilendirebilirsiniz. Bunu ayrıca kullanılabilir odanıza hava kalitesini sanal algılayıcı tasarruf edilen karbon dioksit için Yardım en uygun olup olmadığını öğrenebilirsiniz. Örnek uygulamalarından biridir, bu senaryo görselleştirmenize yardımcı olmak için rastgele sensör verilerini oluşturur.

Aşağıdaki video hızlı başlangıç kurulumunu özetler:

>[!VIDEO https://www.youtube.com/embed/1izK266tbMI]

## <a name="prerequisites"></a>Önkoşullar

1. Azure hesabınız yoksa, başlamadan önce [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

1. Bu hızlı başlangıçta çalıştırdığınız iki konsol uygulamaları kullanılarak yazılan C#. Geliştirme makinenize [.NET Core SDK sürüm 2.1.403 veya üstünü](https://www.microsoft.com/net/download) yükler. .NET Core SDK varsa, geçerli sürümünü doğrulama C# geliştirme makinenizde. Bir komut isteminde `dotnet --version` çalıştırın.

1. [Örnek C# projeyi](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip)indirin. Dijital-twins-samples-csharp-master.zip arşivini ayıklayın.

## <a name="create-a-digital-twins-instance"></a>Digital Twins örneği oluşturma

Bu bölümdeki adımları izleyerek [portalda](https://portal.azure.com) dijital TWINS 'in yeni bir örneğini oluşturun.

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="set-permissions-for-your-app"></a>Uygulamanızın izinlerini ayarlama

Dijital İkizlerini örneğinizin erişebilmesi için bu bölümde, örnek uygulamanızı Azure Active Directory (Azure AD) kaydeder. Bir Azure AD uygulama kaydı zaten varsa, Örneğiniz için yeniden kullanın. Bu bölümde açıklanan şekilde yapılandırıldığından emin olun.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="build-application"></a>Uygulama oluşturma

Sahiplik uygulamayı aşağıdaki adımları izleyerek oluşturun.

1. Bir komut istemi açın. `digital-twins-samples-csharp-master.zip` dosyalarınızın ayıklandığı klasöre gidin.
1. `cd occupancy-quickstart/src` öğesini çalıştırın.
1. `dotnet restore` öğesini çalıştırın.
1. [appSettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/appSettings.json) dosyasını düzenleyerek aşağıdaki değişkenleri güncelleştirin:
    - **ClientID**: önceki bölümde BELIRTILEN Azure AD uygulama KAYDLARıNıZıN uygulama kimliğini girin.
    - **Kiracı**: önceki bölümde da BELIRTILEN Azure AD kiracınızın dizin kimliğini girin.
    - **BaseUrl**: dijital TWINS ÖRNEĞINIZIN yönetim API URL 'si, `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`biçimindedir. Bu URL içindeki yer tutucuları önceki bölümde Örneğiniz için değerlerle değiştirin.

    Güncelleştirilen dosyayı kaydedin.

## <a name="provision-graph"></a>Grafı sağlama

Bu adım, dijital İkizlerini uzamsal grafik ile sağlar:

- Çeşitli alanları.
- Bir cihaz.
- İki algılayıcılar.
- Özel bir işlev.
- Bir rol ataması.

Uzamsal grafik, [Provisionsample. YAML](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) dosyası kullanılarak sağlanır.

1. `dotnet run ProvisionSample` öğesini çalıştırın.

    >[!NOTE]
    >Cihaz oturum açma Azure CLI aracını, Azure ad kullanıcının kimliğini doğrulamak için kullanılır. Kullanıcı, [Microsoft oturum açma](https://microsoft.com/devicelogin) sayfasını kullanarak kimlik doğrulamak için belirli bir kod girmelidir. Kod girildikten sonra kimlik doğrulaması için adımları izleyin. Aracı çalıştırırken, kullanıcının kimliğini doğrulaması gerekir.

    >[!TIP]
    > Bu adımı çalıştırdığınızda, aşağıdaki hata iletisi görünürse değişkenlerinizin doğru şekilde kopyalandığından emin olun: `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

1. Sağlama adım birkaç dakika sürebilir. Ayrıca, IOT hub'ı dijital İkizlerini örneğinizin içinde sağlar. IoT Hub durum =`Running`gösterene kadar döngüsü geçer.

    [örnek durum = çalışıyor ![sağlama](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-provision-sample.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-provision-sample.png#lightbox)

1. Yürütmenin sonunda cihazın `ConnectionString` cihaz simülatör örneğinde kullanmak üzere kopyalayın. Yalnızca bu görüntüde ana hatlarıyla belirtilen dizeyi kopyalayın.

    [bağlantı dizesini kopyalamak ![](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-connection-string.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-connection-string.png#lightbox)

    >[!TIP]
    > [Azure dijital TWINS grafik Görüntüleyicisi 'ni](https://github.com/Azure/azure-digital-twins-graph-viewer)kullanarak uzamsal grafınızı görüntüleyebilir ve değiştirebilirsiniz.

Daha sonra kullanmak üzere konsol penceresini açık tutun.

## <a name="send-sensor-data"></a>Sensör verilerini gönderme

Aşağıdaki adımları izleyerek algılayıcı simülatörü cihaz uygulamasını derleyin ve çalıştırın.

1. Yeni bir komut istemi açın. `digital-twins-samples-csharp-master` klasörüne indirdiğiniz projeye gidin.
1. `cd device-connectivity` öğesini çalıştırın.
1. `dotnet restore` öğesini çalıştırın.
1. **Deviceconnectionstring** öğesini önceki `ConnectionString`güncelleştirmek için [appSettings. JSON](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json) ' i düzenleyin. Güncelleştirilen dosyayı kaydedin.
1. Algılayıcı verileri göndermeye başlamak için `dotnet run` çalıştırın. Aşağıdaki görüntüde gösterildiği gibi Azure dijital TWINS 'e gönderilir.

     [![cihaz bağlantısı](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-device-connectivity.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-device-connectivity.png#lightbox)

1. Bu simülatörünü çalıştırın, böylece sonraki adım eylemi ile yan yana sonuçlarını görüntüleyebilirsiniz olanak tanır. Bu pencere dijital çiftleri için gönderilen sanal sensör verilerini gösterir. Sonraki adım sorgular gerçek zamanlı olarak güncel hava ile kullanılabilir odaları bulunacak.

    >[!TIP]
    > Bu adımı çalıştırdığınızda, aşağıdaki hata iletisi görünürse `DeviceConnectionString` düzgün şekilde kopyalandığından emin olun: `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Temiz havaya sahip olan odaları bulma

Sensör örnek iki algılayıcılar için rastgele veri değerleri benzetimini yapar. Bunlar, hareket ve tasarruf edilen karbon dioksit hedeflenmiştir. Kullanılabilir alanları ile yeni hava örnekte hiçbir durum odadaki tanımlanır. Ayrıca bir tasarruf edilen karbon dioksit düzeyi altında 1.000 ppm tanımlı. Koşul yerine değil, alanı mevcut değil veya uzaktan kalite düşük.

1. Daha önce sağlama adımını çalıştırmak için kullandığınız komut istemi ' ni açın.
1. `dotnet run GetAvailableAndFreshSpaces` öğesini çalıştırın.
1. Bu komut istemi ve sensör verilerini komut istemi yan yana bakın.

    Algılayıcı verileri komut istemi, her beş saniyede bir dijital TWINS 'e benzetimli hareket ve karbon dioksit verileri gönderir. Diğer komut istemi, rastgele sanal verilere göre yeni uçak ile kullanılabilir Odalar bulmak için grafı gerçek zamanlı olarak okur. Bu koşullardan biri son gönderilen algılayıcı verileri temel alan neredeyse gerçek zamanlı gösterir:
   - `Room is available and air is fresh`
   - `Room is not available or air quality is poor`

     [![yeni hava ile kullanılabilir alanlar Al](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-get-available.png)](media/quickstart-view-occupancy-dotnet/azure-digital-twins-quickstart-get-available.png#lightbox)

Bu hızlı başlangıçta ne olduğunu ve hangi API 'Lerin çağrıldığını anlamak için, `digital-twins-samples-csharp`' de bulunan kod çalışma alanı projesiyle [Visual Studio Code](https://code.visualstudio.com/Download) açın. Aşağıdaki komutu kullanın:

```cmd
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

Öğreticiler, derin koda gidin. Bunlar, yapılandırma verisini değiştirmesine nasıl ve hangi API'ler çağrıldığında öğretin. Yönetim API'leri hakkında daha fazla bilgi için dijital İkizlerini Swagger sayfanıza gidin:

```URL
https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/swagger
```

| Adı | Şununla değiştir |
| --- | --- |
| YOUR_INSTANCE_NAME | Dijital İkizlerini örneğinizin adı |
| YOUR_LOCATION | Örneğinizin barındırıldığı sunucu bölgesi |

Veya kolaylık sağlaması için, [dijital TWINS Swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger)öğesine gidin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiler kullanma hakkında daha fazla ayrıntıya gidin:

- Tesis yöneticilerinin occupant üretkenliği artırmak için bir uygulama oluşturun.
- Oluşturmaya çalışması daha verimli bir şekilde.

Öğreticilerine devam etmek için kaynakları oluşturulan temizlemeyin Bu hızlı başlangıçta. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç ile oluşturulan tüm kaynakları silin.

1. Örnek depoyu indirildiğinde oluşturduğunuz klasörü silin.
1. [Azure Portal](https://portal.azure.com)sol taraftaki menüden **tüm kaynaklar**' ı seçin. Ardından dijital İkizlerini kaynağınızı seçin. **Tüm kaynaklar** bölmesinin en üstünde **Sil**' i seçin.

    > [!TIP]
    > Daha önce dijital TWINS örneğinizi silme konusunda sorun yaşıyorsanız, bu, düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma alındı. Örneğiniz silme yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç, dijital TWINS 'in iyi çalışma koşullarına sahip odaları bulmak için nasıl kullanılabileceğini göstermek üzere basit bir senaryo ve örnek uygulamalar kullandı. Bu senaryonun derinlemesine çözümlenmesi için bu öğreticiyi okuyun:

>[!div class="nextstepaction"]
>[Öğretici: Azure Digital Twins'i dağıtma ve uzamsal graf yapılandırma](tutorial-facilities-setup.md)
