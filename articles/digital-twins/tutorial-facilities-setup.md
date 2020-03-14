---
title: 'Öğretici: Önizleme ortamı ve uzamsal grafik dağıtma-Azure dijital TWINS | Microsoft Docs'
description: Azure dijital İkizlerini örneğinizi dağıtma ve bu öğreticideki adımları kullanarak uzamsal kaynaklarınızı yapılandırmak hakkında bilgi edinin.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 01/10/2020
ms.openlocfilehash: 878b64fe6dd491adbb61c4c74cf4a5fc039858cd
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79371418"
---
# <a name="tutorial-deploy-azure-digital-twins-preview-and-configure-a-spatial-graph"></a>Öğretici: Azure dijital TWINS önizlemesi dağıtma ve uzamsal bir grafik yapılandırma

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

Azure Digital TWINS önizleme hizmetini kullanarak, kişileri, yerleri ve cihazları tutarlı bir uzamsal sisteme bir araya getirebilirsiniz. Bu öğretici serisinde, Azure dijital İkizlerini odası doluluk sıcaklık ve Uzaktan kalite en uygun koşullarla algılamak için nasıl kullanılacağını gösterir. 

Bu öğreticiler bir ofis binasındaki bir senaryo oluşturmak için bir .NET konsol uygulaması size yol gösterir. Yapı, her zemin içinde birden çok Katlar ve odaları sahiptir. Odaları ile ortam sıcaklığı, hareket algılayan ve kalite hava, bağlı sensörlerden cihazları içerir. 

Azure dijital İkizlerini hizmetini kullanarak varlıkları dijital nesneleri oluşturma ve fiziksel alanları çoğaltmak öğreneceksiniz. Başka bir konsol uygulaması kullanarak cihaz olaylarının benzetimini yapma. Ardından, bu fiziksel alanları ve varlıkları neredeyse gerçek zamanlı olarak gelen olayları izlemek nasıl öğreneceksiniz. 

Ofis yöneticileri bu tür bilgileri kullanarak bu binada görevli bir çalışanın en uygun koşullara sahip toplantı odalarını ayırması konusunda yardımcı olabilir. Bir office tesis Yöneticisi kurulumunuzu odaları kullanım eğilimlerini almak ve bakım amacıyla çalışma koşullarına izlemek için kullanabilirsiniz.

Bu serinin ilk öğreticisinde aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Dijital İkizlerini dağıtın.
> * Uygulamanıza izinler verir.
> * Dijital İkizlerini örnek bir uygulama değiştirin.
> * Yapı sağlayın.

Bu öğreticilerde [uygun odaları bulma hızlı başlangıcındaki](quickstart-view-occupancy-dotnet.md) örnekler kullanılmakta, kavramlar daha ayrıntılı bir şekilde ele alınmaktadır.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure hesabınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

- .NET Core SDK. Aşağıdaki öğreticilerde kullanılan dijital İkizlerini Azure örnekleri yazılan C#. Örneği derlemek ve çalıştırmak için geliştirme makinenize [.NET Core SDK sürüm 2.1.403 veya sonraki bir sürümü](https://www.microsoft.com/net/download) yüklediğinizden emin olun. Bir komut penceresinde `dotnet --version` çalıştırarak, makinenizde doğru sürümün yüklü olduğundan emin olun.

- Örnek kodu incelemek için [Visual Studio Code](https://code.visualstudio.com/). 

## <a name="deploy-digital-twins"></a>Digital Twins'i dağıtma

Azure dijital İkizlerini hizmetinin yeni bir örneğini oluşturmak için bu bölümdeki adımları kullanın. Abonelik başına yalnızca bir örneği oluşturulabilir. Bir çalıştırma zaten varsa, sonraki bölüme atlayın. 

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="grant-permissions-to-your-app"></a>Uygulamanıza izin verme

Dijital TWINS, hizmete [okuma/yazma erişimini](../active-directory/develop/v2-permissions-and-consent.md) denetlemek için [Azure ACTIVE DIRECTORY](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) kullanır. Dijital İkizlerini Örneğinize bağlanmak için gereken herhangi bir uygulamadan Azure AD'ye kayıtlı olması gerekir. Bu bölümde örnek uygulamanızı kaydetme adımları gösterilmektedir.

Bir uygulama kaydı zaten varsa, Örneğiniz için yeniden kullanabilirsiniz. Ancak bu bölümdeki adımları izleyerek uygulama kaydınızın doğru yapılandırmaya sahip olduğundan emin olun.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="configure-the-digital-twins-sample"></a>Dijital İkizlerini örnek yapılandırma

Bu bölümde, [dijital TWıNS REST API 'leri](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index)ile iletişim kuran bir Azure dijital TWINS uygulaması adım adım açıklanmaktadır. 

### <a name="download-the-sample"></a>Örneği indirme

[Uygun odaları bulma hızlı başlangıcı](quickstart-view-occupancy-dotnet.md) için bu örnekleri daha önce indirdiyseniz bu adımları atlayabilirsiniz.

1. [Dijital TWINS .net örneklerini](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip)indirin.
2. Makinenizde posta klasörünün içeriğini ayıklayın.

### <a name="explore-the-sample"></a>Örneği keşfetme

Ayıklanan örnek klasöründe, **Digital-Twins-Samples-csharp\digital-Twins-Samples.Code-Workspace** dosyasını Visual Studio Code açın. Bu dosyada iki proje bulunur:

* Bir [uzamsal zeka grafiğini](concepts-objectmodel-spatialgraph.md#digital-twins-object-models)yapılandırmak ve sağlamak için örnek **ön hızlı başlangıç** sağlama örneğini kullanabilirsiniz. Bu grafik, fiziksel alanları ve bunları kaynakları sayısal görüntüsüdür. Akıllı bir bina için nesneleri tanımlayan bir [nesne modeli](concepts-objectmodel-spatialgraph.md#digital-twins-object-models)kullanır. Dijital TWINS nesnelerinin ve REST API 'lerinin tüm listesi için, [bu REST API belgelerini](https://docs.westcentralus.azuresmartspaces.net/management/swagger) veya [örneğiniz](#deploy-digital-twins)için oluşturulmuş yönetim API URL 'sini ziyaret edin.

   Dijital TWINS örneğinizle nasıl iletişim kuracağını anlamak için örneği araştırmak üzere **src\actions** klasörüyle başlayabilirsiniz. Bu klasördeki dosyalar bu öğreticilerde kullanacağınız komutları uygulayın:
    - **ProvisionSample.cs** dosyası, uzamsal grafınızı nasıl sağlayacağınızı gösterir.
    - **GetSpaces.cs** dosyası, sağlanan alanlarla ilgili bilgileri alır.
    - **GetAvailableAndFreshSpaces.cs** dosyası, Kullanıcı tanımlı işlev olarak adlandırılan özel bir işlevin sonuçlarını alır.
    - **CreateEndpoints.cs** dosyası diğer hizmetlerle etkileşimde bulunmak için uç noktalar oluşturur.

* Simülasyon örneği **cihaz bağlantısı** , algılayıcı verilerinin benzetimini yapar ve bunu dijital TWINS örneğiniz Için sağlanan IoT Hub 'ına gönderir. Bu örneği [, uzamsal grafınızı sağladığınızda sonraki öğreticide](tutorial-facilities-udf.md#simulate-sensor-data)kullanacaksınız. Bu örneği yapılandırmak için kullandığınız sensör ve cihaz tanımlayıcıları grafınızı sağlamak için kullanacaksınız ile aynı olması gerekir.

### <a name="configure-the-provisioning-sample"></a>Sağlama örneğini yapılandırma

1. Bir komut penceresi açın ve indirilen örneğe gidin. Şu komutu çalıştırın:

    ```cmd/sh
    cd occupancy-quickstart/src
    ```

1. Şu komutu çalıştırarak bağımlılıkları örnek projeye geri yükleyin:

    ```cmd/sh
    dotnet restore
    ```

1. Visual Studio Code, **is-QuickStart** projesinde [appSettings. JSON](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/appSettings.json) dosyasını açın. Aşağıdaki değerleri güncelleştirin:
   * **ClientID**: Azure AD uygulama kaydlarınızın uygulama kimliğini girin. [Uygulama izinlerini ayarladığınız](#grant-permissions-to-your-app)bölümde bu kimliği not etmiştiniz.
   * **Kiracı**: [Azure AD KIRACıNıZıN](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)dizin kimliğini girin. Ayrıca, bu KIMLIĞI [uygulama izinlerini ayarladığınız](#grant-permissions-to-your-app)bölümde de not etmiştiniz.
   * **BaseUrl**: Digital Twins örneğinizin URL'sini girin. Bu URL 'YI almak için, bu URL 'deki yer tutucuları örneğinizin değerleriyle değiştirin: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. [Dağıtım bölümünde](#deploy-digital-twins)yönetim API URL 'sini değiştirerek bu URL 'yi de alabilirsiniz. **Swagger/** , **api/v 1.0/** ile değiştirin.

1. Örneği kullanarak keşfedebileceğiniz dijital TWINS özelliklerinin listesini gözden geçirin. Şu komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

## <a name="understand-the-provisioning-process"></a>Sağlama işlemini anlama

Bu bölümde örnekte binanın uzamsal grafının nasıl sağlandığı gösterilmektedir.

Visual Studio Code ' de **Occupancy-quickstart\src\actions** klasörüne gidin ve **provisionSample.cs**dosyasını açın. Aşağıdaki işleve dikkat edin:

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

Bu işlev, aynı klasörde [Provisionsample. YAML](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) 'yi kullanır. Bu dosyayı açın ve Office oluşturma: *mekan*, *Floor*, *Area*ve *Odalar*hiyerarşisini göz önünde edin. Bu fiziksel alanların herhangi birinde *cihazlar* ve *sensörler* bulunabilir. Her giriş için önceden tanımlanmış bir `type`&mdash;(örneğin, kat, oda) vardır.

**YAML** dosyası örnek, `Default` Digital TWINS nesne modelini kullanan bir uzamsal grafiği gösterir. Bu model türlerinin çoğu için genel adlar sağlar. Genel adlar bir yapı için yeterlidir. Örnek SensorDataType için sıcaklık ve için SpaceBlobType eşleyin. Bir örnek alanı subtypes FocusRoom oda, ConferenceRoom ve benzeri türüdür. 

Fabrika gibi farklı bir mekan için uzamsal graf oluşturmanız gerekmesi durumunda başka bir nesne modeline ihtiyaç duyabilirsiniz. Sağlama örneği için komut satırında `dotnet run GetOntologies` komutunu çalıştırarak hangi modellerin kullanılabilir olduğunu bulabilirsiniz. 

Uzamsal grafikler ve nesne modelleri hakkında daha fazla bilgi için bkz. [dijital TWINS nesne modellerini ve uzamsal zeka grafiğini anlama](concepts-objectmodel-spatialgraph.md).

### <a name="modify-the-sample-spatial-graph"></a>Örnek uzamsal grafiğe değiştirme

**Provisionsample. YAML** dosyası aşağıdaki düğümleri içerir:

- **kaynaklar**: `resources` düğümü, kurulumlarınızdaki cihazlarla iletişim kurmak Için bir Azure IoT Hub kaynağı oluşturur. IOT hub'ı, grafiğin kök düğümde, tüm cihazlardan ve sensörlerden grafınızı ile iletişim kurabilir.  

- **spaces**: Digital Twins nesne modelinde `spaces`, fiziksel konumları temsil eder. Her bir alanda `Type`&mdash;, bölge, mekan veya müşteri&mdash;ve kolay bir `Name`bulunur. Alanları, hiyerarşik bir yapıyı başka alanları için ait olabilir. Sanal bir yapı uzamsal grafiğini provisionSample.yaml dosyası vardır. `Floor` türündeki boşlukların mantıksal iç içe geçme `Venue`, bir zemin içinde `Area` ve bir alandaki `Room` düğümlere göz önünde bulundurmayın. 

- **devices**: Alanlarda, bir dizi sensörü yöneten fiziksel veya sanal varlıklar olan `devices` öğeleri bulunabilir. Örneğin, bir cihaz kullanıcının telefonu, Raspberry PI algılayıcısı Pod veya ağ geçidi olabilir. Örnekteki hayali binanın **Focus Room** adlı odasında bir **Raspberry Pi 3 A1** cihazı bulunmaktadır. Her cihaz düğümü, örneğe sabit kodlanmış benzersiz bir `hardwareId` değerine sahiptir. Bu örneği üretim amaçlı kullanım için yapılandırmak isterseniz bu değerleri kendi sisteminizdeki değerlerle değiştirmeniz gerekir.  

- **Algılayıcılar**: bir cihazda birden çok `sensors`bulunabilir. Bunlar algılayabilir ve sıcaklık, hareket ve pil düzeyi kayıt fiziksel değişiklikleri ister. Her sensör düğümü, burada sabit kodlanmış `hardwareId` değeriyle benzersiz olarak tanımlanmıştır. Gerçek bir uygulama için bu kurulumda sensörlerden öğesinin benzersiz tanımlayıcıları kullanarak değiştirin. ProvisionSample. YAML dosyasının *hareketi* ve *Carbondioksit*kayıt işlemini yapmak için iki sensöri vardır. CarbonDioxide sensörünün tanımlandığı satırların altına aşağıdaki satırları ekleyerek *Temperature* (Sıcaklık) kaydı yapacak yeni bir sensör ekleyin. Bunlar, yorumlanan Sample. YAML 'de açıklamalı çizgiler olarak sağlanır. Her satırın önündeki `#` karakterini kaldırarak bunların açıklamasını kaldırabilirsiniz. 

    ```yaml
            - dataType: Temperature
              hardwareId: SAMPLE_SENSOR_TEMPERATURE
    ```
    > [!NOTE]
    > `dataType` ve `hardwareId` anahtarlarının Bu kod parçacığının üzerindeki deyimlerle hizalanmasına dikkat edin. Ayrıca düzenleyicinizin boşlukları sekmelerle değiştirmediğinden de emin olun. 

ProvisionSample.yaml dosyasını kaydedip kapatın. Sonraki öğreticide, daha fazla bilgi bu dosyaya ekleyin ve ardından, Azure dijital İkizlerini örnek yapı sağlamak.

> [!TIP]
> [Azure dijital TWINS grafik Görüntüleyicisi 'ni](https://github.com/Azure/azure-digital-twins-graph-viewer)kullanarak uzamsal grafınızı görüntüleyebilir ve değiştirebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu noktada Azure dijital İkizlerini keşfetmeye durdurmak istiyorsanız, bu öğreticide oluşturulan kaynakları silmek çekinmeyin:

1. [Azure Portal](https://portal.azure.com)sol menüden **tüm kaynaklar**' ı seçin, dijital TWINS kaynak grubunuzu seçin ve **Sil**' i seçin.

    > [!TIP]
    > Dijital İkizlerini örneğinizin silme sorun olduysa, bir hizmet güncelleştirmesi düzeltme alındı. Örneğiniz silme yeniden deneyin.

1. Gerekirse, iş makinenizde örnek uygulamayı silin.

## <a name="next-steps"></a>Sonraki adımlar

Koşullar oluşturma Örneğinizdeki izlemek için özel bir mantıksal uygulama hakkında bilgi edinmek için serideki sonraki öğretici gidin: 
> [!div class="nextstepaction"]
> [Öğretici: Binanızı sağlama ve çalışma koşullarını izleme](tutorial-facilities-udf.md)