---
title: 'Öğretici: Önizleme ortamı ve uzamsal grafik dağıtma-Azure dijital TWINS | Microsoft Docs'
description: Azure dijital TWINS örneğinizi dağıtmayı ve bu öğreticideki adımları kullanarak uzamsal kaynaklarınızı yapılandırmayı öğrenin.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 01/10/2020
ms.openlocfilehash: 878b64fe6dd491adbb61c4c74cf4a5fc039858cd
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79371418"
---
# <a name="tutorial-deploy-azure-digital-twins-preview-and-configure-a-spatial-graph"></a>Öğretici: Azure dijital TWINS önizlemesi dağıtma ve uzamsal bir grafik yapılandırma

[!INCLUDE [digital-twins-preview-limit-alert](../../includes/digital-twins-preview-limit-alert.md)]

Azure Digital TWINS önizleme hizmetini kullanarak, kişileri, yerleri ve cihazları tutarlı bir uzamsal sisteme bir araya getirebilirsiniz. Bu öğretici serisinde, sıcaklık ve hava kalitesinin en iyi koşullarına sahip oda dolışını algılamak için Azure dijital TWINS 'in nasıl kullanılacağı gösterilmektedir. 

Bu öğreticiler, bir Office binasının senaryosunu oluşturmak için bir .NET konsol uygulamasında size kılavuzluk eder. Binanın her bir tabandaki birden fazla kat ve oda vardır. Odalar, hareket, çevresel sıcaklık ve hava kalitesini algılayan eklenmiş sensörlerle cihazları içerir. 

Azure Digital TWINS hizmetini kullanarak bina Dijital nesnelerinde fiziksel alanların ve varlıkların nasıl çoğaltılacağını öğreneceksiniz. Başka bir konsol uygulaması kullanarak cihaz olaylarını benzetireceksiniz. Daha sonra, bu fiziksel alanlardan ve varlıklardan neredeyse gerçek zamanlı olarak gelen olayların nasıl izleneceğini öğreneceksiniz. 

Ofis yöneticileri bu tür bilgileri kullanarak bu binada görevli bir çalışanın en uygun koşullara sahip toplantı odalarını ayırması konusunda yardımcı olabilir. Office tesisler Yöneticisi, odaların kullanım eğilimlerini almak ve bakım amaçlarıyla çalışma koşullarını izlemek için kurulumunuzu kullanabilir.

Bu serinin ilk öğreticisinde aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Dijital TWINS dağıtın.
> * Uygulamanıza izin verin.
> * Dijital bir TWINS örnek uygulamasını değiştirme.
> * Binınızı sağlayın.

Bu öğreticilerde [uygun odaları bulma hızlı başlangıcındaki](quickstart-view-occupancy-dotnet.md) örnekler kullanılmakta, kavramlar daha ayrıntılı bir şekilde ele alınmaktadır.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure hesabınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

- .NET Core SDK. Bu öğreticilerde kullanılan Azure dijital TWINS örnekleri C# dilinde yazılmıştır. Örneği derlemek ve çalıştırmak için geliştirme makinenize [.NET Core SDK sürüm 2.1.403 veya sonraki bir sürümü](https://www.microsoft.com/net/download) yüklediğinizden emin olun. Bir komut penceresinde çalıştırarak `dotnet --version` doğru sürümün makinenizde yüklü olduğundan emin olun.

- Örnek kodu incelemek için [Visual Studio Code](https://code.visualstudio.com/). 

## <a name="deploy-digital-twins"></a>Digital Twins'i dağıtma

Azure Digital TWINS hizmetinin yeni bir örneğini oluşturmak için bu bölümdeki adımları kullanın. Her abonelik için yalnızca bir örnek oluşturulabilir. Zaten çalışır durumda olan bir sonraki bölüme atlayın. 

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="grant-permissions-to-your-app"></a>Uygulamanıza izin verme

Dijital TWINS, hizmete [okuma/yazma erişimini](../active-directory/develop/v2-permissions-and-consent.md) denetlemek için [Azure ACTIVE DIRECTORY](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) kullanır. Dijital TWINS örneğinizle bağlantı kurmak için gereken tüm uygulamaların Azure AD 'ye kayıtlı olması gerekir. Bu bölümde örnek uygulamanızı kaydetme adımları gösterilmektedir.

Uygulama kaydınız varsa örnek uygulama için bu kaydı kullanabilirsiniz. Ancak bu bölümdeki adımları izleyerek uygulama kaydınızın doğru yapılandırmaya sahip olduğundan emin olun.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="configure-the-digital-twins-sample"></a>Dijital TWINS örneğini yapılandırma

Bu bölümde, [dijital TWıNS REST API 'leri](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index)ile iletişim kuran bir Azure dijital TWINS uygulaması adım adım açıklanmaktadır. 

### <a name="download-the-sample"></a>Örneği indirme

[Uygun odaları bulma hızlı başlangıcı](quickstart-view-occupancy-dotnet.md) için bu örnekleri daha önce indirdiyseniz bu adımları atlayabilirsiniz.

1. [Dijital TWINS .net örneklerini](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip)indirin.
2. Makinenizde zip klasörünün içeriğini ayıklayın.

### <a name="explore-the-sample"></a>Örneği keşfetme

Ayıklanan örnek klasöründeki **digital-twins-samples-csharp\digital-twins-samples.code-workspace** dosyasını Visual Studio Code ile açın. Bu dosyada iki proje bulunur:

* Bir [uzamsal zeka grafiğini](concepts-objectmodel-spatialgraph.md#digital-twins-object-models)yapılandırmak ve sağlamak için örnek **ön hızlı başlangıç** sağlama örneğini kullanabilirsiniz. Bu grafik, fiziksel boşlukların ve içerdikleri kaynakların sayısal görüntüsüdür. Akıllı bir bina için nesneleri tanımlayan bir [nesne modeli](concepts-objectmodel-spatialgraph.md#digital-twins-object-models)kullanır. Digital Twins nesnelerinin ve REST API'lerinin tam listesi için [bu REST API belgesini](https://docs.westcentralus.azuresmartspaces.net/management/swagger) veya [örneğiniz](#deploy-digital-twins) için oluşturulmuş olan Yönetim API'si URL'sini ziyaret edin.

   Dijital TWINS örneğinizle nasıl iletişim kuracağını anlamak için örneği araştırmak üzere **src\actions** klasörüyle başlayabilirsiniz. Bu klasördeki dosyalar, bu öğreticilerde kullanacağınız komutları uygular:
    - **ProvisionSample.cs** dosyası, uzamsal grafınızı nasıl sağlayacağınızı gösterir.
    - **GetSpaces.cs** dosyası, sağlanan alanlarla ilgili bilgileri alır.
    - **GetAvailableAndFreshSpaces.cs** dosyası, Kullanıcı tanımlı işlev olarak adlandırılan özel bir işlevin sonuçlarını alır.
    - **CreateEndpoints.cs** dosyası diğer hizmetlerle etkileşimde bulunmak için uç noktalar oluşturur.

* Simülasyon örneği **cihaz bağlantısı** , algılayıcı verilerinin benzetimini yapar ve bunu dijital TWINS örneğiniz Için sağlanan IoT Hub 'ına gönderir. Bu örneği [, uzamsal grafınızı sağladığınızda sonraki öğreticide](tutorial-facilities-udf.md#simulate-sensor-data)kullanacaksınız. Bu örneği yapılandırmak için kullandığınız algılayıcı ve cihaz tanımlayıcıları, grafınızı sağlamak için kullanacağınız ile aynı olmalıdır.

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
   * **BaseUrl**: Digital Twins örneğinizin URL'sini girin. Bu URL'yi almak için şu URL'deki yer tutucuları örneğinize ait değerlerle değiştirin: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. [Dağıtım bölümünde](#deploy-digital-twins)yönetim API URL 'sini değiştirerek bu URL 'yi de alabilirsiniz. **Swagger/** , **api/v 1.0/** ile değiştirin.

1. Örneği kullanarak keşfedebileceğiniz dijital TWINS özelliklerinin listesini gözden geçirin. Şu komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

## <a name="understand-the-provisioning-process"></a>Sağlama sürecini anlayın

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

Bu işlev, aynı klasörde [Provisionsample. YAML](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) 'yi kullanır. Bu dosyayı açın ve Office oluşturma: *mekan*, *Floor*, *Area*ve *Odalar*hiyerarşisini göz önünde edin. Bu fiziksel alanların herhangi birinde *cihazlar* ve *sensörler* bulunabilir. Her giriş için önceden tanımlanmış `type` &mdash;bir örnek, kat, oda vardır.

**YAML** dosyası örneği, `Default` dijital TWINS nesne modelini kullanan bir uzamsal grafiği gösterir. Bu model çoğu tür için genel adlar sağlar. Genel adlar bir bina için yeterlidir. SensorDataType için sıcaklık ve SpaceBlobType için eşleme örnekleri. Örnek boşluk türü, alt tür Focusoda, ConferenceRoom vb. ile birlikte yer alır. 

Fabrika gibi farklı bir mekan için uzamsal graf oluşturmanız gerekmesi durumunda başka bir nesne modeline ihtiyaç duyabilirsiniz. Sağlama örneği için komut satırında komutunu `dotnet run GetOntologies` çalıştırarak hangi modellerin kullanılabilir olduğunu bulabilirsiniz. 

Uzamsal grafikler ve nesne modelleri hakkında daha fazla bilgi için bkz. [dijital TWINS nesne modellerini ve uzamsal zeka grafiğini anlama](concepts-objectmodel-spatialgraph.md).

### <a name="modify-the-sample-spatial-graph"></a>Örnek uzamsal grafiği değiştirme

**Provisionsample. YAML** dosyası aşağıdaki düğümleri içerir:

- **kaynaklar**: düğüm `resources` , kurulumlarınızdaki cihazlarla Iletişim kurmak için bir Azure IoT Hub kaynağı oluşturur. Grafiğinizin kök düğümündeki bir IoT Hub 'ı, grafiğinizde bulunan tüm cihazlar ve sensörlerle iletişim kurabilir.  

- **spaces**: Digital Twins nesne modelinde `spaces`, fiziksel konumları temsil eder. Her `Type` &mdash;alan bir örneğin, bölge, mekan veya müşteri&mdash;ve kolay `Name`bir şekilde bulunur. Boşluk, hiyerarşik bir yapı oluşturarak diğer alanlara ait olabilir. ProvisionSample. YAML dosyası, sanal bir binasının uzamsal bir grafiğine sahiptir. İçindeki `Floor` `Venue`türdeki boşlukların, `Area` bir zemin içindeki ve `Room` bir alandaki düğümlerin mantıksal iç içe geçme durumunu göz önünde bulundurmayın. 

- **devices**: Alanlarda, bir dizi sensörü yöneten fiziksel veya sanal varlıklar olan `devices` öğeleri bulunabilir. Örneğin, bir cihaz kullanıcının telefonu, Raspberry PI algılayıcısı Pod veya ağ geçidi olabilir. Örnekteki hayali binanın **Focus Room** adlı odasında bir **Raspberry Pi 3 A1** cihazı bulunmaktadır. Her cihaz düğümü, örneğe sabit kodlanmış benzersiz bir `hardwareId` değerine sahiptir. Bu örneği üretim amaçlı kullanım için yapılandırmak isterseniz bu değerleri kendi sisteminizdeki değerlerle değiştirmeniz gerekir.  

- **Algılayıcılar**: bir cihaz birden çok `sensors`içerebilir. Sıcaklık, hareket ve pil düzeyi gibi fiziksel değişiklikleri algılayıp kaydedebilirler. Her sensör düğümü, burada sabit kodlanmış `hardwareId` değeriyle benzersiz olarak tanımlanmıştır. Gerçek bir uygulama için, bu ayarları, kuruluminizdeki sensörların benzersiz tanımlayıcılarını kullanarak değiştirin. provisionSample.yaml dosyasında biri *Motion* (Hareket) biri de *CarbonDioxide* (Karbondioksit) sensörü olmak üzere iki sensör vardır. CarbonDioxide sensörünün tanımlandığı satırların altına aşağıdaki satırları ekleyerek *Temperature* (Sıcaklık) kaydı yapacak yeni bir sensör ekleyin. Bunlar, yorumlanan Sample. YAML 'de açıklamalı çizgiler olarak sağlanır. Her satırın önündeki `#` karakteri kaldırarak bunların açıklamasını kaldırabilirsiniz. 

    ```yaml
            - dataType: Temperature
              hardwareId: SAMPLE_SENSOR_TEMPERATURE
    ```
    > [!NOTE]
    > `dataType` Ve `hardwareId` anahtarlarının Bu kod parçacığının üzerindeki deyimlerle hizalanmasına dikkat edin. Ayrıca düzenleyicinizin boşlukları sekmelerle değiştirmediğinden de emin olun. 

provisionSample.yaml dosyasını kaydedin ve kapatın. Sonraki öğreticide bu dosyaya daha fazla bilgi ekleyecek ve ardından Azure dijital TWINS örnek binınızı sağlayacaksınız.

> [!TIP]
> [Azure dijital TWINS grafik Görüntüleyicisi 'ni](https://github.com/Azure/azure-digital-twins-graph-viewer)kullanarak uzamsal grafınızı görüntüleyebilir ve değiştirebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu noktada Azure dijital TWINS araştırmalarını durdurmak istiyorsanız bu öğreticide oluşturulan kaynakları silebilirsiniz:

1. [Azure Portal](https://portal.azure.com)sol menüden **tüm kaynaklar**' ı seçin, dijital TWINS kaynak grubunuzu seçin ve **Sil**' i seçin.

    > [!TIP]
    > Dijital TWINS örneğinizi silme konusunda sorun yaşıyorsanız, bu düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma alındı. Lütfen örneğinizi silmeyi yeniden deneyin.

1. Gerekirse, iş makinenizdeki örnek uygulamayı silin.

## <a name="next-steps"></a>Sonraki adımlar

Örnek binaınızdaki koşulları izlemek üzere özel bir mantık uygulamayı öğrenmek için, serideki bir sonraki Öğreticiye gidin: 
> [!div class="nextstepaction"]
> [Öğretici: Binanızı sağlama ve çalışma koşullarını izleme](tutorial-facilities-udf.md)