---
title: 'Hızlı başlangıç: kullanılabilir odaları bulma-Azure dijital TWINS'
description: Bu hızlı başlangıçta iki .NET Core örnek uygulaması çalıştırarak bir Azure Digital Twins alanına sanal hareket ve karbondioksit telemetri verileri göndereceksiniz. Burada hedefiniz, veriler bulutta işlendikten sonra Yönetim API'leriyle temiz havaya sahip olan uygun odaları bulmak olacak.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc seodec18
ms.date: 11/08/2019
ms.openlocfilehash: efa5ede4937a2a7f48f9d64f06b78c6343345ad6
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903896"
---
# <a name="quickstart-find-available-rooms-by-using-azure-digital-twins"></a>Hızlı başlangıç: Azure dijital TWINS kullanarak kullanılabilir odaları bulma

Azure dijital TWINS hizmeti, fiziksel ortamınızın dijital görüntüsünü yeniden oluşturmanızı sağlar. Bu işlemin ardından ortamınızdaki olaylarla ilgili bildirimler alabilir ve verdiğiniz yanıtları özelleştirebilirsiniz.

Bu hızlı başlangıç, bir sanal Office oluşturmayı dijital olarak oluşturmak için [bir çift .net örneği](https://github.com/Azure-Samples/digital-twins-samples-csharp) kullanır. Bu, bu binadaki kullanılabilir odaların nasıl bulunacağını gösterir. Dijital TWINS sayesinde, ortamınızdaki birçok sensöri ilişkilendirebilirsiniz. Ayrıca, kullanılabilir odanın hava kalitesi ' nin, karbon dilü için bir sanal algılayıcı yardımıyla en uygun olup olmadığını da öğrenebilirsiniz. Örnek uygulamalardan biri, bu senaryoyu görselleştirmenize yardımcı olmak için rastgele algılayıcı verileri oluşturur.

Aşağıdaki videoda hızlı başlangıç ayarları özetlenir:

>[!VIDEO https://www.youtube.com/embed/1izK266tbMI]

## <a name="prerequisites"></a>Önkoşullar

1. Azure hesabınız yoksa, başlamadan önce [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

1. Bu hızlı başlangıçta çalıştırdığınız iki konsol uygulaması kullanılarak C#yazılmıştır. Geliştirme makinenize [.NET Core SDK sürüm 2.1.403 veya üstünü](https://www.microsoft.com/net/download) yükler. .NET Core SDK yüklüyse, geliştirme makinenizde geçerli sürümünü C# doğrulayın. Bir komut isteminde `dotnet --version` çalıştırın.

1. [Örnek C# projeyi](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip)indirin. Digital-Twins-Samples-CSharp-Master. zip arşivini ayıklayın.

## <a name="create-a-digital-twins-instance"></a>Digital Twins örneği oluşturma

Bu bölümdeki adımları izleyerek [portalda](https://portal.azure.com) dijital TWINS 'in yeni bir örneğini oluşturun.

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="set-permissions-for-your-app"></a>Uygulamanızın izinlerini ayarlama

Bu bölüm, dijital TWINS örneğinizi erişebilmeleri için örnek uygulamanızı Azure Active Directory (Azure AD) olarak kaydeder. Zaten bir Azure AD uygulama kaydınız varsa, örnek için onu yeniden kullanın. Bu bölümde açıklandığı gibi yapılandırıldığından emin olun.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="build-application"></a>Uygulama oluşturma

Bu adımları izleyerek, öğrenci uygulamasını oluşturun.

1. Bir komut istemi açın. `digital-twins-samples-csharp-master.zip` dosyalarınızın ayıklandığı klasöre gidin.
1. `cd occupancy-quickstart/src` öğesini çalıştırın.
1. `dotnet restore` öğesini çalıştırın.
1. [appSettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/appSettings.json) dosyasını düzenleyerek aşağıdaki değişkenleri güncelleştirin:
    - **ClientID**: önceki bölümde BELIRTILEN Azure AD uygulama KAYDLARıNıZıN uygulama kimliğini girin.
    - **Kiracı**: önceki bölümde da BELIRTILEN Azure AD kiracınızın dizin kimliğini girin.
    - **BaseUrl**: dijital TWINS ÖRNEĞINIZIN yönetim API URL 'si, `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`biçimindedir. Bu URL 'deki yer tutucuları, önceki bölümde örneğinizin değerleriyle değiştirin.

    Güncelleştirilmiş dosyayı kaydedin.

## <a name="provision-graph"></a>Grafı sağlama

Bu adım, dijital TWINS uzamsal grafiklerinizi şunları sağlar:

- Birkaç boşluk.
- Bir cihaz.
- İki algılayıcı.
- Özel bir işlev.
- Tek bir rol ataması.

Uzamsal grafik, [Provisionsample. YAML](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) dosyası kullanılarak sağlanır.

1. `dotnet run ProvisionSample` öğesini çalıştırın.

    >[!NOTE]
    >Azure AD 'de kullanıcının kimliğini doğrulamak için cihaz oturumu açma Azure CLı aracı kullanılır. Kullanıcı, [Microsoft oturum açma](https://microsoft.com/devicelogin) sayfasını kullanarak kimlik doğrulamak için belirli bir kod girmelidir. Kod girildikten sonra, kimlik doğrulaması için adımları izleyin. Araç çalışırken kullanıcının kimlik doğrulaması gerekir.

    >[!TIP]
    > Bu adımı çalıştırdığınızda, aşağıdaki hata iletisi görünürse değişkenlerinizin doğru şekilde kopyalandığından emin olun: `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

1. Sağlama adımı birkaç dakika sürebilir. Ayrıca, dijital TWINS örneğiniz içinde bir IoT Hub sağlar. IoT Hub durum =`Running`gösterene kadar döngüsü geçer.

    [![sağlama örneği](media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample1.png)](media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample1.png#lightbox)

1. Yürütmenin sonunda cihazın `ConnectionString` cihaz simülatör örneğinde kullanmak üzere kopyalayın. Yalnızca bu görüntüde özetlenen dizeyi kopyalayın.

    [bağlantı dizesini kopyalamak ![](media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample.png)](media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample.png#lightbox)

    >[!TIP]
    > [Azure dijital TWINS grafik Görüntüleyicisi 'ni](https://github.com/Azure/azure-digital-twins-graph-viewer)kullanarak uzamsal grafınızı görüntüleyebilir ve değiştirebilirsiniz.

Daha sonra kullanmak üzere konsol penceresini açık tutun.

## <a name="send-sensor-data"></a>Sensör verilerini gönderme

Aşağıdaki adımları izleyerek algılayıcı simülatörü cihaz uygulamasını derleyin ve çalıştırın.

1. Yeni bir komut istemi açın. `digital-twins-samples-csharp-master` klasörüne indirdiğiniz projeye gidin.
1. `cd device-connectivity` öğesini çalıştırın.
1. `dotnet restore` öğesini çalıştırın.
1. **Deviceconnectionstring** öğesini önceki `ConnectionString`güncelleştirmek için [appSettings. JSON](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json) ' i düzenleyin. Güncelleştirilmiş dosyayı kaydedin.
1. Algılayıcı verileri göndermeye başlamak için `dotnet run` çalıştırın. Aşağıdaki görüntüde gösterildiği gibi dijital TWINS 'e gönderildiğini görürsünüz.

     [![cihaz bağlantısı](media/quickstart-view-occupancy-dotnet/digital-twins-device-connectivity.png)](media/quickstart-view-occupancy-dotnet/digital-twins-device-connectivity.png#lightbox)

1. Sonraki adım eylemiyle sonuçları yan yana görüntüleyebilmeniz için bu simülatör çalışmasına izin verin. Bu pencere, dijital TWINS 'e gönderilen sanal algılayıcı verilerini gösterir. Sonraki adım, yeni uçak ile kullanılabilir odaları bulmak için gerçek zamanlı olarak sorgular.

    >[!TIP]
    > Bu adımı çalıştırdığınızda, aşağıdaki hata iletisi görünürse `DeviceConnectionString` düzgün şekilde kopyalandığından emin olun: `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Temiz havaya sahip olan odaları bulma

Algılayıcı örneği, iki sensör için rastgele veri değerlerinin benzetimini yapar. Bunlar hareket ve karbon dioksit. Yeni AIR ile kullanılabilen boşluklar, odada bulunmayan bir şekilde örnekte tanımlanmıştır. Ayrıca, 1.000 ppm altında bir karbon dioksit düzeyi ile de tanımlanırlar. Koşul karşılanmazsa, alan kullanılamaz veya hava kalitesi zayıf olur.

1. Daha önce sağlama adımını çalıştırmak için kullandığınız komut istemi ' ni açın.
1. `dotnet run GetAvailableAndFreshSpaces` öğesini çalıştırın.
1. Bu komut istemine ve algılayıcı verileri komut istemine yan yana bakın.

    Algılayıcı verileri komut istemi, her beş saniyede bir dijital TWINS 'e benzetimli hareket ve karbon dioksit verileri gönderir. Diğer komut istemi, rastgele sanal verilere göre yeni uçak ile kullanılabilir Odalar bulmak için grafı gerçek zamanlı olarak okur. Bu koşullardan birini, son gönderilen algılayıcı verilerine göre neredeyse gerçek zamanlı olarak görüntüler:
   - Temiz havaya sahip olan uygun odalar.
   - Dolu veya hava kalitesi düzeyi düşük olan odalar.

     [![yeni hava ile kullanılabilir alanlar Al](media/quickstart-view-occupancy-dotnet/digital-twins-get-available.png)](media/quickstart-view-occupancy-dotnet/digital-twins-get-available.png#lightbox)

Bu hızlı başlangıçta ne olduğunu ve hangi API 'Lerin çağrıldığını anlamak için, `digital-twins-samples-csharp`' de bulunan kod çalışma alanı projesiyle [Visual Studio Code](https://code.visualstudio.com/Download) açın. Aşağıdaki komutu kullanın:

```plaintext
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

Öğreticiler koda göre daha derin gider. Yapılandırma verilerini ve API 'Leri nasıl değiştireceğiniz hakkında öğretir. Yönetim API 'Leri hakkında daha fazla bilgi için dijital TWINS Swagger sayfanıza gidin:

```plaintext
https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/swagger
```

| Ad | Şununla değiştir |
| --- | --- |
| YOUR_INSTANCE_NAME | Dijital TWINS örneğinizin adı |
| YOUR_LOCATION | Örneğinizin barındırıldığı sunucu bölgesi |

Veya kolaylık sağlaması için, [dijital TWINS Swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger)öğesine gidin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiler şu şekilde bir ayrıntıya gider:

- Tesis yöneticileri için, işant üretkenliğini artırmak üzere bir uygulama oluşturun.
- Oluşturmayı daha verimli bir şekilde çalıştırın.

Öğreticilere devam etmek için, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin.

1. Örnek depoyu indirdiğiniz sırada oluşturulan klasörü silin.
1. [Azure Portal](https://portal.azure.com)sol taraftaki menüden **tüm kaynaklar**' ı seçin. Ardından dijital TWINS kaynağını seçin. **Tüm kaynaklar** bölmesinin en üstünde **Sil**' i seçin.

    > [!TIP]
    > Daha önce dijital TWINS örneğinizi silme konusunda sorun yaşıyorsanız, bu, düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma alındı. Lütfen örneğinizi silmeyi yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç, dijital TWINS 'in iyi çalışma koşullarına sahip odaları bulmak için nasıl kullanılabileceğini göstermek üzere basit bir senaryo ve örnek uygulamalar kullandı. Bu senaryonun derinlemesine çözümlenmesi için Bu öğreticiye bakın:

>[!div class="nextstepaction"]
>[Öğretici: Azure Digital Twins'i dağıtma ve uzamsal graf yapılandırma](tutorial-facilities-setup.md)
