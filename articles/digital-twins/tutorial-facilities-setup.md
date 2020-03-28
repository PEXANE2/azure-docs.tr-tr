---
title: 'Öğretici: Bir önizleme ortamı ve uzamsal grafik dağıtma - Azure Digital Twins| Microsoft Dokümanlar'
description: Bu öğreticideki adımları kullanarak Azure Dijital İkizler örneğini nasıl dağıtacağımız ve uzamsal kaynaklarınızı nasıl yapılandırılacaklarınızı öğrenin.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 01/10/2020
ms.openlocfilehash: 878b64fe6dd491adbb61c4c74cf4a5fc039858cd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79371418"
---
# <a name="tutorial-deploy-azure-digital-twins-preview-and-configure-a-spatial-graph"></a>Öğretici: Azure Dijital İkizler Önizlemesini dağıtın ve uzamsal bir grafik yapılandırın

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

Kişileri, yerleri ve cihazları tutarlı bir uzamsal sistemde bir araya getirmek için Azure Dijital İkizler Önizleme hizmetini kullanabilirsiniz. Bu öğretici serisi, en uygun sıcaklık ve hava kalitesi koşullarına sahip oda doluluklarını tespit etmek için Azure Digital Twins'in nasıl kullanılacağını göstermektedir. 

Bu öğreticiler, bir ofis binası senaryosu oluşturmak için bir .NET konsol uygulaması ile size yol verecektir. Binanın her katında birden fazla kat ve oda bulunmaktadır. Odalarda hareket, ortam sıcaklığı ve hava kalitesini algılayan bağlı sensörlere sahip cihazlar bulunur. 

Azure Dijital İkizler hizmetini kullanarak binadaki fiziksel alanları ve varlıkları dijital nesneler olarak nasıl kopyaladığınızı öğreneceksiniz. Başka bir konsol uygulaması kullanarak aygıt olaylarını simüle elabilirsiniz. Daha sonra, bu fiziksel alanlardan ve varlıklardan gelen olayları neredeyse gerçek zamanlı olarak nasıl izleyeceğinizi öğreneceksiniz. 

Ofis yöneticileri bu tür bilgileri kullanarak bu binada görevli bir çalışanın en uygun koşullara sahip toplantı odalarını ayırması konusunda yardımcı olabilir. Bir ofis tesisleri yöneticisi, odaların kullanım eğilimlerini elde etmek ve bakım amacıyla çalışma koşullarını izlemek için kurulumunuzu kullanabilir.

Bu serinin ilk öğreticisinde aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Dijital İkizler'i dağıtın.
> * Uygulamanıza izin ver.
> * Dijital İkizler örnek uygulamasını değiştirin.
> * Binanızı temin edin.

Bu öğreticilerde [uygun odaları bulma hızlı başlangıcındaki](quickstart-view-occupancy-dotnet.md) örnekler kullanılmakta, kavramlar daha ayrıntılı bir şekilde ele alınmaktadır.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure hesabınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

- .NET Çekirdek SDK. Bu öğreticilerde kullanılan Azure Dijital İkizler örnekleri C# olarak yazılır. Örneği oluşturmak ve çalıştırmak için [.NET Core SDK sürüm 2.1.403 veya daha sonra](https://www.microsoft.com/net/download) geliştirme makinenize yüklediğinizden emin olun. Komut penceresinde çalıştırarak `dotnet --version` makinenizde doğru sürümün yüklü olup olmadığını denetleyin.

- Örnek kodu incelemek için [Visual Studio Code](https://code.visualstudio.com/). 

## <a name="deploy-digital-twins"></a>Digital Twins'i dağıtma

Azure Dijital İkizler hizmetinin yeni bir örneğini oluşturmak için bu bölümdeki adımları kullanın. Abonelik başına yalnızca bir örnek oluşturulabilir. Zaten çalışan bir bölümünüz varsa bir sonraki bölüme atlayın. 

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="grant-permissions-to-your-app"></a>Uygulamanıza izin verme

Digital Twins, hizmete [okuma/yazma erişimini](../active-directory/develop/v2-permissions-and-consent.md) denetlemek için [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) kullanır. Dijital İkizler örneğinize bağlanması gereken tüm uygulamaların Azure AD'ye kaydedilmesi gerekir. Bu bölümde örnek uygulamanızı kaydetme adımları gösterilmektedir.

Uygulama kaydınız varsa örnek uygulama için bu kaydı kullanabilirsiniz. Ancak bu bölümdeki adımları izleyerek uygulama kaydınızın doğru yapılandırmaya sahip olduğundan emin olun.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="configure-the-digital-twins-sample"></a>Dijital İkizler örneğini yapılandırın

Bu bölüm, [Digital Twins REST API'leri](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index)ile iletişim kuran bir Azure Digital Twins uygulaması ile size yol açabilirsiniz. 

### <a name="download-the-sample"></a>Örneği indirme

[Uygun odaları bulma hızlı başlangıcı](quickstart-view-occupancy-dotnet.md) için bu örnekleri daha önce indirdiyseniz bu adımları atlayabilirsiniz.

1. Digital [Twins .NET örneklerini](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip)indirin.
2. Makinenizdeki zip klasörünün içeriğini ayıklayın.

### <a name="explore-the-sample"></a>Örneği keşfetme

Ayıklanan örnek klasöründeki **digital-twins-samples-csharp\digital-twins-samples.code-workspace** dosyasını Visual Studio Code ile açın. Bu dosyada iki proje bulunur:

* Bir [uzamsal zeka grafiğini](concepts-objectmodel-spatialgraph.md#digital-twins-object-models)yapılandırmak ve sağlamak için sağlama örnek **doluluk-quickstart** kullanabilirsiniz. Bu grafik, fiziksel alanlarınızın ve kaynakların sayısallaştırılmış görüntüsüdür. Akıllı bir bina için nesneleri tanımlayan bir [nesne modeli](concepts-objectmodel-spatialgraph.md#digital-twins-object-models)kullanır. Digital Twins nesnelerinin ve REST API'lerinin tam listesi için [bu REST API belgesini](https://docs.westcentralus.azuresmartspaces.net/management/swagger) veya [örneğiniz](#deploy-digital-twins) için oluşturulmuş olan Yönetim API'si URL'sini ziyaret edin.

   Dijital İkizler örneğinizle nasıl iletişim kurduğunu anlamak için örneği keşfetmek için **src\actions** klasörüyle başlayabilirsiniz. Bu klasördeki dosyalar, bu öğreticilerde kullanacağınız komutları uygular:
    - provisionSample.cs **provisionSample.cs** dosyası, uzamsal grafiğinizi nasıl sağlarak sağlarak gösterir.
    - **getSpaces.cs** dosyası, sağlanan alanlar hakkında bilgi alır.
    - **getAvailableAndFreshSpaces.cs** dosyası, kullanıcı tanımlı işlev adı verilen özel bir işlevin sonuçlarını alır.
    - **createEndpoints.cs** dosyası, diğer hizmetlerle etkileşim kurmak için uç noktalar oluşturur.

* Simülasyon örneği **cihaz bağlantısı** sensör verilerini simüle eder ve Dijital İkizler örneğiniz için sağlanan IoT hub'ına gönderir. Uzamsal grafiğinizi temin ettikten sonra bu örneği [bir sonraki öğreticide kullanacaksınız.](tutorial-facilities-udf.md#simulate-sensor-data) Bu örneği yapılandırmak için kullandığınız sensör ve aygıt tanımlayıcıları, grafiğinizi sağlamak için kullanacağınız aygıtla aynı olmalıdır.

### <a name="configure-the-provisioning-sample"></a>Sağlama örneğini yapılandırma

1. Komut penceresini açın ve indirilen örneğe gidin. Şu komutu çalıştırın:

    ```cmd/sh
    cd occupancy-quickstart/src
    ```

1. Şu komutu çalıştırarak bağımlılıkları örnek projeye geri yükleyin:

    ```cmd/sh
    dotnet restore
    ```

1. Visual Studio Code'da, **doluluk-hızlı başlatma** projesinde [appSettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/appSettings.json) dosyasını açın. Aşağıdaki değerleri güncelleştirin:
   * **ClientId**: Azure AD uygulama kaydınızın uygulama kimliğini girin. Bu kimliği [uygulama izinlerini ayarladığınız](#grant-permissions-to-your-app)bölümde kaydettiniz.
   * **Kiracı**: [Azure AD kiracınızın](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)dizin kimliğini girin. Ayrıca, uygulama izinlerini ayarladığınız bölümde bu kimliği de [kaydettiniz.](#grant-permissions-to-your-app)
   * **BaseUrl**: Digital Twins örneğinizin URL'sini girin. Bu URL'yi almak için şu URL'deki yer tutucuları örneğinize ait değerlerle değiştirin: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Dağıtım bölümünden Yönetim API URL'sini değiştirerek bu [URL'yi](#deploy-digital-twins)de alabilirsiniz. **Swagger/** ile **değiştirin/ api/v1.0/**.

1. Örneği kullanarak keşfedebileceğiniz Dijital İkizler özelliklerinin listesini inceleyin. Şu komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

## <a name="understand-the-provisioning-process"></a>Sağlama işlemini anlama

Bu bölümde örnekte binanın uzamsal grafının nasıl sağlandığı gösterilmektedir.

Visual Studio Code'da **doluluk-quickstart\src\actions** klasörüne göz atın ve dosyayı **provisionSample.cs**açın. Aşağıdaki işleve dikkat edin:

```csharp
public static async Task<IEnumerable<ProvisionResults.Space>> ProvisionSample(HttpClient httpClient, ILogger logger)
{
    IEnumerable<SpaceDescription> spaceCreateDescriptions;
    using (var r = new StreamReader("actions/provisionSample.yaml"))
    {
        spaceCreateDescriptions = await GetProvisionSampleTopology(r);
    }

    var results = await CreateSpaces(httpClient, logger, spaceCreateDescriptions, Guid.Empty);

    Console.WriteLine($"Completed Provisioning: {JsonConvert.SerializeObject(results, Formatting.Indented)}");

    return results;
}
```

Bu işlev [provisionSample.yaml'yi](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) aynı klasörde kullanır. Bu dosyayı açın ve bir ofis binasının hiyerarşisini not edin: *Yer,* *Kat,* *Alan*ve *Odalar.* Bu fiziksel alanların herhangi birinde *cihazlar* ve *sensörler* bulunabilir. Her giriş örneğin, `type` &mdash;Zemin, Oda için önceden tanımlanmış bir vardır.

Örnek **yaml** dosyası, Dijital İkizler `Default` nesne modelini kullanan uzamsal bir grafik gösterir. Bu model, çoğu tür için genel adlar sağlar. Genel adlar bir bina için yeterlidir. Örnekler SensorDataType için Sıcaklık ve SpaceBlobType için Haritadır. Örnek bir alan türü, FocusRoom, ConferenceRoom ve benzeri alt türleri olan Oda'dır. 

Fabrika gibi farklı bir mekan için uzamsal graf oluşturmanız gerekmesi durumunda başka bir nesne modeline ihtiyaç duyabilirsiniz. Sağlama örneği için komut satırındaki komutu `dotnet run GetOntologies` çalıştırarak hangi modellerin kullanılabilebileceğini öğrenebilirsiniz. 

Uzamsal grafikler ve nesne modelleri hakkında daha fazla bilgi [için, Anlama Dijital İkizler nesne modelleri ve uzamsal zeka grafiği okuyun.](concepts-objectmodel-spatialgraph.md)

### <a name="modify-the-sample-spatial-graph"></a>Örnek uzamsal grafiği değiştirme

**ProvisionSample.yaml** dosyası aşağıdaki düğümleri içerir:

- **kaynaklar**: `resources` Düğüm, kurulumunuzdaki aygıtlarla iletişim kurmak için bir Azure IoT Hub kaynağı oluşturur. Grafiğinizin kök düğümündeki bir IoT hub'ı, grafiğinizdeki tüm aygıtlar ve sensörlerle iletişim kurabilir.  

- **spaces**: Digital Twins nesne modelinde `spaces`, fiziksel konumları temsil eder. Her alan `Type` &mdash;örneğin, Bölge, Mekan veya&mdash;Müşteri `Name`ve bir dostu vardır. Boşluklar hiyerarşik bir yapı oluşturarak diğer boşluklara ait olabilir. provisionSample.yaml dosyasında hayali bir binanın mekansal grafiği vardır. Bir `Floor` zeminde, `Venue` `Room` bir alandaki `Area` düğümlerde, içindeki tür boşluklarının mantıksal iç içe geçmelerine dikkat edin. 

- **devices**: Alanlarda, bir dizi sensörü yöneten fiziksel veya sanal varlıklar olan `devices` öğeleri bulunabilir. Örneğin, bir aygıt kullanıcının telefonu, Raspberry Pi sensör bölmesi veya ağ geçidi olabilir. Örnekteki hayali binanın **Focus Room** adlı odasında bir **Raspberry Pi 3 A1** cihazı bulunmaktadır. Her cihaz düğümü, örneğe sabit kodlanmış benzersiz bir `hardwareId` değerine sahiptir. Bu örneği üretim amaçlı kullanım için yapılandırmak isterseniz bu değerleri kendi sisteminizdeki değerlerle değiştirmeniz gerekir.  

- **sensörler**: Bir cihaz `sensors`birden fazla . Sıcaklık, hareket ve pil seviyesi gibi fiziksel değişiklikleri algılayabilir ve kaydedebilirler. Her sensör düğümü, burada sabit kodlanmış `hardwareId` değeriyle benzersiz olarak tanımlanmıştır. Gerçek bir uygulama için, kurulumunuzdaki sensörlerin benzersiz tanımlayıcılarını kullanarak bunları değiştirin. provisionSample.yaml dosyasında biri *Motion* (Hareket) biri de *CarbonDioxide* (Karbondioksit) sensörü olmak üzere iki sensör vardır. CarbonDioxide sensörünün tanımlandığı satırların altına aşağıdaki satırları ekleyerek *Temperature* (Sıcaklık) kaydı yapacak yeni bir sensör ekleyin. Bunlar provisionSample.yaml'de yorum satırı olarak verilmiştir. Her satırın önündeki `#` karakteri kaldırarak yorumlarını kaldırabilirsiniz. 

    ```yaml
            - dataType: Temperature
              hardwareId: SAMPLE_SENSOR_TEMPERATURE
    ```
    > [!NOTE]
    > Anahtarların `hardwareId` `dataType` bu parçacığın üstündeki ifadelerle hizalandığından emin olun. Ayrıca düzenleyicinizin boşlukları sekmelerle değiştirmediğinden de emin olun. 

provisionSample.yaml dosyasını kaydedin ve kapatın. Bir sonraki öğreticide, bu dosyaya daha fazla bilgi ekleyecek ve ardından Azure Digital Twins örnek binanızı sağlarsınız.

> [!TIP]
> [Azure Dijital İkizler Grafik Görüntüleyici'yi](https://github.com/Azure/azure-digital-twins-graph-viewer)kullanarak uzamsal grafiğinizi görüntüleyebilir ve değiştirebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu noktada Azure Dijital İkizler'i keşfetmeyi durdurmak istiyorsanız, bu eğitimde oluşturulan kaynakları silmekte çekinmeyin:

1. [Azure portalının](https://portal.azure.com)sol menüsünden **Tüm kaynakları**seçin, Digital Twins kaynak grubunu seçin ve **Sil'i**seçin.

    > [!TIP]
    > Dijital İkizler örneğini silmede sorun yaşadıysanız, düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma sunuldu. Lütfen örneğini yeniden silmeyi deneyin.

1. Gerekirse, iş makinenizdeki örnek uygulamayı silin.

## <a name="next-steps"></a>Sonraki adımlar

Örnek binanızdaki koşulları izlemek için özel bir mantığı nasıl uygulayacağınızı öğrenmek için, serinin bir sonraki öğreticisine gidin: 
> [!div class="nextstepaction"]
> [Öğretici: Binanızı sağlama ve çalışma koşullarını izleme](tutorial-facilities-udf.md)