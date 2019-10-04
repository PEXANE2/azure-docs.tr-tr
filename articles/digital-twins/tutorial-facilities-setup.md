---
title: 'Öğretici: Azure dijital TWINS dağıtma | Microsoft Docs'
description: Azure dijital TWINS örneğinizi dağıtmayı ve bu öğreticideki adımları kullanarak uzamsal kaynaklarınızı yapılandırmayı öğrenin.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 09/17/2019
ms.openlocfilehash: 54afe8ea67996562c88a2ade2ec16c4eaa89cdee
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949763"
---
# <a name="tutorial-deploy-azure-digital-twins-preview-and-configure-a-spatial-graph"></a>Öğretici: Azure dijital TWINS önizlemesi dağıtma ve uzamsal bir grafik yapılandırma

Azure Digital TWINS önizleme hizmetini kullanarak, kişileri, yerleri ve cihazları tutarlı bir uzamsal sisteme bir araya getirebilirsiniz. Bu öğretici serisinde, sıcaklık ve hava kalitesinin en iyi koşullarına sahip oda dolışını algılamak için Azure dijital TWINS 'in nasıl kullanılacağı gösterilmektedir. 

Bu öğreticiler, bir Office binasının senaryosunu oluşturmak için bir .NET konsol uygulamasında size kılavuzluk eder. Binanın her bir tabandaki birden fazla kat ve oda vardır. Odalar, hareket, çevresel sıcaklık ve hava kalitesini algılayan eklenmiş sensörlerle cihazları içerir. 

Azure Digital TWINS hizmetini kullanarak bina Dijital nesnelerinde fiziksel alanların ve varlıkların nasıl çoğaltılacağını öğreneceksiniz. Başka bir konsol uygulaması kullanarak cihaz olaylarını benzetireceksiniz. Daha sonra, bu fiziksel alanlardan ve varlıklardan neredeyse gerçek zamanlı olarak gelen olayların nasıl izleneceğini öğreneceksiniz. 

Office Yöneticisi bu bilgileri kullanarak, bu binada çalışan bir çalışanın en iyi koşullara sahip toplantı odalarını defterlerine yardımcı olur. Office tesisler Yöneticisi, odaların kullanım eğilimlerini almak ve bakım amaçlarıyla çalışma koşullarını izlemek için kurulumunuzu kullanabilir.

Bu serinin ilk öğreticisinde şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Dijital TWINS dağıtın.
> * Uygulamanıza izin verin.
> * Dijital bir TWINS örnek uygulamasını değiştirme.
> * Binınızı sağlayın.

Bu öğreticiler, kavramların daha ayrıntılı ve derinlemesine bir kapsamı için, [kullanılabilir odaları bulmak için hızlı başlangıç](quickstart-view-occupancy-dotnet.md) ile aynı örnekleri kullanır ve değiştirir.

## <a name="prerequisites"></a>Prerequisites

- Bir Azure aboneliği. Azure hesabınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

- .NET Core SDK. Bu öğreticilerde kullanılan Azure dijital TWINS örnekleri içinde C#yazılmıştır. Örneği derlemek ve çalıştırmak için geliştirme makinenize [.NET Core SDK sürüm 2.1.403 veya sonraki bir sürümü](https://www.microsoft.com/net/download) yüklediğinizden emin olun. Bir komut penceresinde `dotnet --version` çalıştırarak doğru sürümün makinenizde yüklü olduğundan emin olun.

- Örnek kodu araştırmak için [Visual Studio Code](https://code.visualstudio.com/) . 

## <a name="deploy-digital-twins"></a>Dijital TWINS dağıtma

Azure Digital TWINS hizmetinin yeni bir örneğini oluşturmak için bu bölümdeki adımları kullanın. Her abonelik için yalnızca bir örnek oluşturulabilir. Zaten çalışır durumda olan bir sonraki bölüme atlayın. 

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="grant-permissions-to-your-app"></a>Uygulamanıza izin verin

Dijital TWINS, hizmete [okuma/yazma erişimini](../active-directory/develop/v1-permissions-and-consent.md) denetlemek için [Azure ACTIVE DIRECTORY](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) kullanır. Dijital TWINS örneğinizle bağlantı kurmak için gereken tüm uygulamaların Azure AD 'ye kayıtlı olması gerekir. Bu bölümdeki adımlarda, örnek uygulamanızın nasıl kaydedileceği gösterilmektedir.

Zaten bir uygulama kaydınız varsa, örnek için onu yeniden kullanabilirsiniz. Ancak, uygulama kaydlarınızın doğru şekilde yapılandırıldığından emin olmak için bu bölüme göz atabilirsiniz.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="configure-the-digital-twins-sample"></a>Dijital TWINS örneğini yapılandırma

Bu bölümde, [dijital TWıNS REST API 'leri](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index)ile iletişim kuran bir Azure dijital TWINS uygulaması adım adım açıklanmaktadır. 

### <a name="download-the-sample"></a>Örneği indirin

Hızlı başlangıç için yüklenmiş örnekleri zaten varsa, [mevcut odaları bulmak için](quickstart-view-occupancy-dotnet.md)bu adımları atlayabilirsiniz.

1. [Dijital TWINS .net örneklerini](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip)indirin.
2. Makinenizde zip klasörünün içeriğini ayıklayın.

### <a name="explore-the-sample"></a>Örneği keşfet

Ayıklanan örnek klasöründe, **Digital-Twins-Samples-csharp\digital-Twins-Samples.Code-Workspace** dosyasını Visual Studio Code açın. İki proje içerir:

* Bir [uzamsal zeka grafiğini](concepts-objectmodel-spatialgraph.md#digital-twins-object-models)yapılandırmak ve sağlamak için örnek **ön hızlı başlangıç** sağlama örneğini kullanabilirsiniz. Bu grafik, fiziksel boşlukların ve içerdikleri kaynakların sayısal görüntüsüdür. Akıllı bir bina için nesneleri tanımlayan bir [nesne modeli](concepts-objectmodel-spatialgraph.md#digital-twins-object-models)kullanır. Dijital TWINS nesnelerinin ve REST API 'lerinin tüm listesi için, [bu REST API belgelerini](https://docs.westcentralus.azuresmartspaces.net/management/swagger) veya [örneğiniz](#deploy-digital-twins)için oluşturulmuş yönetim API URL 'sini ziyaret edin.

   Dijital TWINS örneğiniz ile nasıl iletişim kuracağını görmek üzere örneği araştırmak için **src\actions** klasörüyle başlayabilirsiniz. Bu klasördeki dosyalar, bu öğreticilerde kullanacağınız komutları uygular:
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
   * **BaseUrl**: dijital TWINS örneğinizin URL 'sini girin. Bu URL 'YI almak için, bu URL 'deki yer tutucuları örneğinizin değerleriyle değiştirin: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. [Dağıtım bölümünde](#deploy-digital-twins)yönetim API URL 'sini değiştirerek bu URL 'yi de alabilirsiniz. **Swagger/** , **api/v 1.0/** ile değiştirin.

1. Örneği kullanarak keşfedebileceğiniz dijital TWINS özelliklerinin listesini görüntüleyin. Şu komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

## <a name="understand-the-provisioning-process"></a>Sağlama sürecini anlayın

Bu bölümde, örneğin bir bina için uzamsal grafik sağlama şekli gösterilmektedir.

Visual Studio Code ' de **Occupancy-quickstart\src\actions** klasörüne gidin ve **provisionSample.cs**dosyasını açın. Aşağıdaki işlevi aklınızda edin:

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

Bu işlev, aynı klasörde [Provisionsample. YAML](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) 'yi kullanır. Bu dosyayı açın ve Office oluşturma: *mekan*, *Floor*, *Area*ve *Odalar*hiyerarşisini göz önünde edin. Bu fiziksel boşlukların herhangi biri, *cihazlar* ve *Algılayıcılar*içerebilir. Her giriş için önceden tanımlanmış bir @no__t vardır-0 @ no__t-1örneğin, Floor, oda.

Örnek **YAML** dosyası, `Default` dijital TWINS nesne modelini kullanan bir uzamsal grafiği gösterir. Bu model çoğu tür için genel adlar sağlar. Genel adlar bir bina için yeterlidir. SensorDataType için sıcaklık ve SpaceBlobType için eşleme örnekleri. Örnek boşluk türü, alt tür Focusoda, ConferenceRoom vb. ile birlikte yer alır. 

Fabrika gibi farklı tür bir mekan için uzamsal bir grafik oluşturmanız gerekiyordu, farklı bir nesne modeline ihtiyacınız vardır. Sağlama örneği için komut satırında `dotnet run GetOntologies` komutunu çalıştırarak hangi modellerin kullanılabilir olduğunu bulabilirsiniz. 

Uzamsal grafikler ve nesne modelleri hakkında daha fazla bilgi için bkz. [dijital TWINS nesne modellerini ve uzamsal zeka grafiğini anlama](concepts-objectmodel-spatialgraph.md).

### <a name="modify-the-sample-spatial-graph"></a>Örnek uzamsal grafiği değiştirme

**Provisionsample. YAML** dosyası aşağıdaki düğümleri içerir:

- **kaynaklar**: `resources` düğümü, kuruluminizdeki cihazlarla iletişim kurmak Için bir Azure IoT Hub kaynağı oluşturur. Grafiğinizin kök düğümündeki bir IoT Hub 'ı, grafiğinizde bulunan tüm cihazlar ve sensörlerle iletişim kurabilir.  

- **boşluk**: dijital TWINS nesne modelinde `spaces` fiziksel konumları temsil eder. Her bir boşluk `Type` @ no__t-1; Örneğin, bölge, mekan veya Customer @ no__t-2 ve kolay bir `Name` ' ü içerir. Boşluk, hiyerarşik bir yapı oluşturarak diğer alanlara ait olabilir. ProvisionSample. YAML dosyası, sanal bir binasının uzamsal bir grafiğine sahiptir. @No__t-1, `Area` içinde `Floor` türündeki boşlukların mantıksal iç içe geçme ve bir alanda @no__t 3 düğümleri olduğunu aklınızda bulundurmayın. 

- **cihazlar**: boşluklar, bir dizi sensöri yöneten fiziksel veya sanal varlıklar olan `devices` içerebilir. Örneğin, bir cihaz kullanıcının telefonu, Raspberry PI algılayıcısı Pod veya ağ geçidi olabilir. Örneğinizdeki sanal Bina içinde, **odak odası** adlı odanın bir **Raspberry Pi 3 a1** cihazı nasıl içerdiğini aklınızda tutun. Her cihaz düğümü, örnekte sabit olarak kodlanmış benzersiz bir @no__t (0) tarafından tanımlanır. Bu örneği gerçek bir üretim için yapılandırmak üzere, bunları kurulumdaki değerlerle değiştirin.  

- **Algılayıcılar**: bir cihaz birden çok @no__t içerebilir-1. Sıcaklık, hareket ve pil düzeyi gibi fiziksel değişiklikleri algılayıp kaydedebilirler. Her algılayıcı düğümü, burada bir `hardwareId` tarafından benzersiz şekilde tanımlanır. Gerçek bir uygulama için, bu ayarları, kuruluminizdeki sensörların benzersiz tanımlayıcılarını kullanarak değiştirin. ProvisionSample. YAML dosyasının *hareketi* ve *Carbondioksit*kayıt işlemini yapmak için iki sensöri vardır. Karbondioksit algılayıcı için satırların altına aşağıdaki satırları ekleyerek *sıcaklığını*kaydetmek için başka bir algılayıcı ekleyin. Bunların, yorumlanan Sample. YAML 'de açıklamalı çizgiler olarak sağlandığını unutmayın. Her satırın önünde `#` karakterini kaldırarak bu açıklamaları özelleştirebilirsiniz. 

    ```yaml
            - dataType: Temperature
              hardwareId: SAMPLE_SENSOR_TEMPERATURE
    ```
    > [!NOTE]
    > @No__t-0 ve `hardwareId` anahtarlarının Bu kod parçacığının üzerindeki deyimlerle hizalanmasına dikkat edin. Ayrıca, Düzenleyicinizde boşlukları sekmelerle değiştirmez emin olun. 

ProvisionSample. YAML dosyasını kaydedin ve kapatın. Sonraki öğreticide bu dosyaya daha fazla bilgi ekleyecek ve ardından Azure dijital TWINS örnek binınızı sağlayacaksınız.

> [!TIP]
> [Azure dijital TWINS grafik Görüntüleyicisi 'ni](https://github.com/Azure/azure-digital-twins-graph-viewer)kullanarak uzamsal grafınızı görüntüleyebilir ve değiştirebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Bu noktada Azure dijital TWINS araştırmalarını durdurmak istiyorsanız bu öğreticide oluşturulan kaynakları silebilirsiniz:

1. [Azure Portal](https://portal.azure.com)sol menüden **tüm kaynaklar**' ı seçin, dijital TWINS kaynak grubunuzu seçin ve **Sil**' i seçin.

    > [!TIP]
    > Dijital TWINS örneğinizi silme konusunda sorun yaşıyorsanız, bu düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma alındı. Lütfen örneğinizi silmeyi yeniden deneyin.

1. Gerekirse, iş makinenizdeki örnek uygulamayı silin.

## <a name="next-steps"></a>Sonraki adımlar

Örnek binaınızdaki koşulları izlemek üzere özel bir mantık uygulamayı öğrenmek için, serideki bir sonraki Öğreticiye gidin: 
> [!div class="nextstepaction"]
> [Öğretici: oluşturma ve izleme çalışma koşullarınızı sağlama](tutorial-facilities-udf.md)