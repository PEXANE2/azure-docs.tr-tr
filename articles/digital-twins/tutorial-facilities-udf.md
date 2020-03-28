---
title: 'Öğretici: Bir IoT cihaz alanını izleyin - Azure Digital Twins| Microsoft Dokümanlar'
description: Bu öğreticideki adımları kullanarak Azure Digital Twins ile mekansal kaynaklarınızı nasıl sağlayıp çalışma koşullarını izleyeceğinizi öğrenin.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 01/10/2020
ms.openlocfilehash: 6cf6a8f7de181a81d60028e33ba2631815c8ca04
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75895348"
---
# <a name="tutorial-provision-your-building-and-monitor-working-conditions-with-azure-digital-twins-preview"></a>Öğretici: Azure Digital Twins Preview ile binanızı sağlama ve çalışma koşullarını izleme

Bu öğretici, istediğiniz sıcaklık koşulları ve konfor düzeyi için alanınızı izlemek için Azure Digital Twins Preview'un nasıl kullanılacağını gösterir. Örnek [binanızı yapılandırdıktan](tutorial-facilities-setup.md)sonra, bu öğreticideki adımları kullanarak binanızı sağlayabilir ve sensör verileriniz üzerinde özel işlevler çalıştırabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * İzlenecek koşulları tanımlayın.
> * Kullanıcı tanımlı bir işlev (UDF) oluşturun.
> * Sensör verilerini simüle edin.
> * Kullanıcı tanımlı bir işlevin sonuçlarını alın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, Azure [Digital Twins kurulumunuzu tamamladığınızı](tutorial-facilities-setup.md)varsayar. Devam etmeden önce aşağıdakilere sahip olduğunuzdan emin olun:

- Bir [Azure hesabı.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Çalışan bir Digital Twins örneği. 
- Çalışma makinenize indirilmiş ve ayıklanmış [Digital Twins C# örnekleri](https://github.com/Azure-Samples/digital-twins-samples-csharp). 
- [.NET Core SDK sürüm 2.1.403 veya daha sonra](https://www.microsoft.com/net/download) geliştirme makinenizde örnek oluşturmak ve çalıştırmak için. Doğru `dotnet --version` sürümün yüklü olduğunu doğrulamak için çalıştırın. 
- Örnek kodu incelemek için [Visual Studio Code](https://code.visualstudio.com/). 

>[!TIP]
> Yeni bir örnek sağlıyorsanız benzersiz bir Dijital İkizler örnek adý kullanın.

## <a name="define-conditions-to-monitor"></a>İzleme koşullarını tanımlama

*Eşleştirme*adı verilen aygıt veya sensör verilerinde izlemek için belirli koşullar kümesi tanımlayabilirsiniz. Daha sonra kullanıcı *tanımlı işlevler*olarak adlandırılan işlevleri tanımlayabilirsiniz. Kullanıcı tanımlı işlevler, eşleştiriciler tarafından belirtilen koşullar oluştuğunda, boşluklarınızdan ve aygıtlarınızdan gelen veriler üzerinde özel mantık uygular. Daha fazla bilgi için [Veri işleme ve kullanıcı tanımlı işlevleri](concepts-user-defined-functions.md)okuyun. 

**occupancy-quickstart** örnek projesinin **src\actions\provisionSample.yaml** adlı dosyasını Visual Studio Code'da açın. **matchers** türü ile başlayan bölümü bulun. Bu tür deki her giriş, belirtilen **Adı**taşıyan bir eşleyici oluşturur. Eşleştirici tip **dataTypeValue**bir sensör izleyecek. Birkaç sensör içeren bir **aygıt** düğümü olan *Focus Room A1*adlı alanla nasıl ilişkili olduğuna dikkat edin. Bu sensörlerden birini izleyecek bir eşleştirme sağlamak için, **veriTypeValue** sensörün **dataType**eşleşir emin olun. 

Aşağıdaki eşleştirmeyi varolan eşleştiricilerin altına ekleyin. Tuşların hizalandığından ve boşlukların sekmelerle değiştirilmediğinden emin olun. Bu satırlar, *provisionSample.yaml* dosyasında yorum satırı olarak da mevcuttur. Her satırın `#` önündeki karakteri kaldırarak yorumlarını kaldırabilirsiniz.

```yaml
      - name: Matcher Temperature
        dataTypeValue: Temperature
```

Bu `SAMPLE_SENSOR_TEMPERATURE` [eşleştirme, ilk öğreticide](tutorial-facilities-setup.md)eklediğiniz sensörü izleyecek. 

## <a name="create-a-user-defined-function"></a>Kullanıcı tanımlı işlev oluşturma

Sensör verilerinizin işlenmesini özelleştirmek için kullanıcı tanımlı işlevleri kullanabilirsiniz. Bunlar, eşleştiriciler tarafından açıklandığı gibi belirli koşullar oluştuğunda Azure Dijital İkizler örneğiniz içinde çalıştırılabilen özel JavaScript kodudur. İzlemek istediğiniz her sensör için farklı eşleştiriciler ve kullanıcı tanımlı işlevler oluşturabilirsiniz. Daha fazla bilgi için [Veri işleme ve kullanıcı tanımlı işlevleri](concepts-user-defined-functions.md)okuyun. 

Örnek *provisionSample.yaml* dosyasında, **kullanıcı tanımlı fonksiyonlarla**başlayan bir bölüm arayın. Bu bölümde, kullanıcı tarafından tanımlanan bir işlevi belirli bir **Ad**ile birlikte hükümler. Bu UDF **matcherNames**altında eşleşenler listesinde hareket eder. UDF için kendi JavaScript dosyanızı **script** bölümünde sağlayabilirsiniz.

Ayrıca **roleassignments** adlı bölüme de dikkat edin. Space Administrator rolünü kullanıcı tanımlı işleve atar. Bu rol, sağlanan alanlardan herhangi birinden gelen olaylara erişmesine olanak tanır. 

1. *provisionSample.yaml* adlı dosyanın `matcherNames` düğümüne aşağıdaki satırı ekleyerek veya var olan satırın açıklamasını kaldırarak UDF'yi sıcaklık eşleştiricisini içerecek şekilde yapılandırın:

    ```yaml
            - Matcher Temperature
    ```

1. **src\actions\userDefinedFunctions\availability.js** dosyasını düzenleyicinizde açın. Bu *provisionSample.yaml* **komut öğesibaşvurulan** dosyadır. Bu dosyadaki kullanıcı tanımlı işlev, odada hareket algılanmadığında ve karbondioksit düzeylerinin 1.000 ppm'nin altında olduğu koşulları arar. 

   Sıcaklığı ve diğer koşulları izlemek için JavaScript dosyasını değiştirin. Odada hareket algılanmadı, karbondioksit seviyesi 1.000 ppm'nin altında ve sıcaklık 78 fahrenhaytın altında yken koşulları aramak için aşağıdaki kod satırlarını ekleyin.

   >[!NOTE]
   > Bu bölümde *src\actions\userDefinedFunctions\availability.js* dosyasını değiştirir, böylece kullanıcı tanımlı bir işlev yazmanın bir yolunu ayrıntılı olarak öğrenebilirsiniz. Ancak, kurulumunuzda [doğrudan src\actions\userDefinedFunctions\availabilityForTutorial.js](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) dosyasını kullanmayı seçebilirsiniz. Bu dosya, öğretici için gerekli olan tüm değişikliklere sahiptir. Bunun yerine bu dosyayı kullanıyorsanız, [src\actions\provisionSample.yaml'deki](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) **komut dosyası** anahtarı için doğru dosya adını kullandığınızdan emin olun.

    a. Dosyanın en üstünde, `// Add your sensor type here` açıklamasının altına sıcaklık için şu satırları ekleyin:

    ```JavaScript
        var temperatureType = "Temperature";
        var temperatureThreshold = 78;
    ```

    b. Açıklamanın `var motionSensor` `// Add your sensor variable here`altında , aşağıdaki ifadeleri tanımlayan aşağıdaki satırları ekleyin:

     ```JavaScript
        var temperatureSensor = otherSensors.find(function(element) {
            return element.DataType === temperatureType;
        });
    ```

    c. Açıklamanın `var carbonDioxideValue` `// Add your sensor latest value here`altında , aşağıdaki satırı tanımlayan ifadeden sonra ekleyin:

    ```JavaScript
        var temperatureValue = getFloatValue(temperatureSensor.Value().Value);
    ```

    d. Şu kod satırlarını `// Modify this line to monitor your sensor value` açıklamasının altından kaldırın:

     ```JavaScript
        if(carbonDioxideValue === null || motionValue === null) {
            sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide or motion are null, returning");
            return;
        }
    ```

    Bunları şu satırlarla değiştirin:

    ```JavaScript
        if(carbonDioxideValue === null || motionValue === null || temperatureValue === null){
            sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide, motion, or temperature are null, returning");
            return;
        }
    ```

    e. Şu kod satırlarını `// Modify these lines as per your sensor` açıklamasının altından kaldırın:

    ```JavaScript
        var availableFresh = "Room is available and air is fresh";
        var noAvailableOrFresh = "Room is not available or air quality is poor";
    ```

    Bunları şu satırlarla değiştirin:

    ```JavaScript
        var alert = "Room with fresh air and comfortable temperature is available.";
        var noAlert = "Either room is occupied, or working conditions are not right.";
    ```

    f. Şu *if-else* kod bloğunu `// Modify this code block for your sensor` açıklamasının altından kaldırın:

    ```JavaScript
        // If carbonDioxide less than threshold and no presence in the room => log, notify and set parent space computed value
        if(carbonDioxideValue < carbonDioxideThreshold && !presence) {
            log(`${availableFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
            setSpaceValue(parentSpace.Id, spaceAvailFresh, availableFresh);
        }
        else {
            log(`${noAvailableOrFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
            setSpaceValue(parentSpace.Id, spaceAvailFresh, noAvailableOrFresh);

            // Set up custom notification for poor air quality
            parentSpace.Notify(JSON.stringify(noAvailableOrFresh));
        }
    ```

    Sonrasında şu *if-else* bloğuyla değiştirin:

    ```JavaScript
        // If sensor values are within range and room is available
        if(carbonDioxideValue < carbonDioxideThreshold && temperatureValue < temperatureThreshold && !presence) {
            log(`${alert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

            // log, notify and set parent space computed value
            setSpaceValue(parentSpace.Id, spaceAvailFresh, alert);

            // Set up notification for this alert
            parentSpace.Notify(JSON.stringify(alert));
        }
        else {
            log(`${noAlert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

            // log, notify and set parent space computed value
            setSpaceValue(parentSpace.Id, spaceAvailFresh, noAlert);
        }
    ```

    Değiştirilen UDF, bir odanın kullanılabilir durumda olması, karbondioksit ve sıcaklık sınırlarının kabul edilen sınırlar içinde olması durumunu izler. Bu koşul yerine getirildiğinde `parentSpace.Notify(JSON.stringify(alert));` deyimiyle bir bildirim oluşturur. Sağlanan koşuldan bağımsız olarak izlenen alanın değerini ayarlayacak ve aşağıdaki iletiyi görüntüleyecektir.

    g. Dosyayı kaydedin.

1. Bir komut penceresi açın ve klasöre git **doluluk-quickstart\src**. Uzamsal zeka grafiğinizi ve kullanıcı tanımlı işlevinizi sağlamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dotnet run ProvisionSample
    ```

   >[!IMPORTANT]
   > Dijital İkizler Yönetimi API'nize yetkisiz erişimi önlemek **için, doluluk hızlı başlatma** uygulaması, Azure hesap kimlik bilgilerinizle oturum açmanızı gerektirir. Kimlik bilgilerinizi kısa bir süre için kaydeder, bu nedenle her çalıştırdığınızda oturum açmanız gerekmeyebilir. Bu program ilk kez çalıştığında ve bundan sonra kaydedilmiş kimlik bilgilerinizin süresi dolduğunda, uygulama sizi oturum açma sayfasına yönlendirir ve oturuma özel bir kod verir. Azure hesabınızda oturum açmak için yönergeleri izleyin.

1. Hesabınız doğrulandıktan sonra, uygulama *provisionSample.yaml'de*yapılandırılan bir örnek uzamsal grafik oluşturmaya başlar. Sağlama bitene kadar bekleyin. Birkaç dakika sürer. Bundan sonra, komut penceresindeki iletileri gözlemleyin ve uzamsal grafiğinizin nasıl oluşturulduğuna dikkat edin. Uygulamanın kök düğümünde veya `Venue`.

1. Komut penceresindeki çıktıdan, `ConnectionString` `Devices` bölümün altındaki değeri panonuza kopyalayın. Bir sonraki bölümdeki aygıt bağlantısını simüle etmek için bu değere ihtiyacınız vardır.

    [![Provizyon örneği](./media/tutorial-facilities-udf/run-provision-sample.png)](./media/tutorial-facilities-udf/run-provision-sample.png#lightbox)

>[!TIP]
> Hükmün ortasındaki "G/Ç işlemi, iş parçacığı çıkışı veya uygulama isteği nedeniyle iptal edildi" gibi bir hata iletisi alırsanız, komutu yeniden çalıştırmayı deneyin. BU, HTTP istemcisi bir ağ sorunundan zaman lanmışsa ortaya çıkabilir.

## <a name="simulate-sensor-data"></a>Sensör verilerinin simülasyonunu yapma

Bu bölümde, örnekte *aygıt bağlantısı* adlı projeyi kullanırsınız. Hareket, sıcaklık ve karbondioksit algılamak için sensör verilerini simüle edeceksiniz. Bu proje, sensörler için rastgele değerler oluşturur ve bunları cihaz bağlantı dizesini kullanarak IoT hub'a gönderir.

1. Ayrı bir komut penceresinde, Azure Dijital İkizler örneğine ve ardından **aygıt bağlantısı** klasörüne gidin.

1. Projenizin bağımlılıklarının doğru olduğundan emin olmak için şu komutu çalıştırın:

    ```cmd/sh
    dotnet restore
    ```

1. Düzenleyicinizde [appsettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json) dosyasını açın ve aşağıdaki değerleri düzeltin:

   a. **DeviceConnectionString**: Bir önceki bölümde çıktı penceresinde görünen `ConnectionString` değerini atayın. Simülatörin IoT hub'ına düzgün bağlanabilmesi için bu dizeyi tırnak içinde tamamen kopyalayın.

   b. **Sensors** dizisinde **DonanımId:** Azure Digital Twins örneğinize sağlanan sensörlerden olayları simüle ettiğiniz için, donanım kimliği ve bu `sensors` dosyadaki sensörlerin adları *provisionSample.yaml* dosyasının düğümüyle eşleşmelidir.

      Sıcaklık sensörü için yeni bir giriş ekleyin. *appsettings.json'daki* **Sensörler** düğümü aşağıdaki gibi görünmelidir:

      ```JSON
      "Sensors": [{
        "DataType": "Motion",
        "HardwareId": "SAMPLE_SENSOR_MOTION"
      },{
        "DataType": "CarbonDioxide",
        "HardwareId": "SAMPLE_SENSOR_CARBONDIOXIDE"
      },{
        "DataType": "Temperature",
        "HardwareId": "SAMPLE_SENSOR_TEMPERATURE"
      }]
      ```

1. Sıcaklık, hareket ve karbondioksit için cihaz olayı simülasyonunu başlatmak üzere şu komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

   >[!NOTE]
   > Simülasyon örneği Dijital İkizler örneğinizle doğrudan iletişim kurmadığından, kimlik doğrulamanızı gerektirmez.

## <a name="get-results-of-the-user-defined-function"></a>Kullanıcı tanımlı işlevin sonuçlarını alma

Örneğiniz cihaz ve sensör verilerini her aldığında kullanıcı tanımlı işlev çalışır. Bu bölümde, kullanıcı tanımlı işlevin sonuçlarını almak için Azure Dijital İkizler örneğiniz sorgulanmaktadır. Neredeyse gerçek zamanlı olarak, bir oda müsait olduğunda, havanın taze ve sıcaklığın doğru olduğu size bildirilir. 

1. Örneği veya yeni bir komut penceresini sağlamak için kullandığınız komut penceresini açın ve örneğin **doluluk-quickstart\src** klasörüne tekrar gidin.

1. Aşağıdaki komutu çalıştırın ve istendiğinde oturum açın:

    ```cmd/sh
    dotnet run GetAvailableAndFreshSpaces
    ```

Çıktı penceresi, kullanıcı tanımlı işlevin nasıl çalıştığını ve aygıt simülasyonundaki olayları nasıl engellediğini gösterir. 

   [![UDF için çıktı](./media/tutorial-facilities-udf/adt-tutorial-udf-running.png)](./media/tutorial-facilities-udf/adt-tutorial-udf-running.png#lightbox)

İzlenen koşul karşılanırsa, kullanıcı tanımlı işlev daha [önce](#create-a-user-defined-function)gördüğümüz gibi, ilgili ileti ile boşluğun değerini ayarlar. İşlev `GetAvailableAndFreshSpaces` konsoldaki iletiyi yazdırır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu noktada Azure Dijital İkizler'i keşfetmeyi durdurmak istiyorsanız, bu eğitimde oluşturulan kaynakları silmekte çekinmeyin:

1. [Azure portalının](https://portal.azure.com)sol menüsünden **Tüm kaynakları**seçin, Digital Twins kaynak grubunu seçin ve **Sil'i**seçin.

    >[!TIP]
    > Dijital İkizler örneğini silmede sorun yaşadıysanız, düzeltmeyle birlikte bir hizmet güncelleştirmesi kullanıma sunuldu. Lütfen örneğini yeniden silmeyi deneyin.

2. Gerekirse, iş makinenizdeki örnek uygulamaları silin.

## <a name="next-steps"></a>Sonraki adımlar

Artık alanlarınızı oluşturduğunuza ve özel bildirimleri tetiklemek için bir çerçeve oluşturduğunuza göre, aşağıdaki öğreticilerden herhangi birine gidebilirsiniz:

> [!div class="nextstepaction"]
> [Öğretici: Logic Apps'i kullanarak Azure Digital Twins alanlarınızdan bildirim alma](tutorial-facilities-events.md)

> [!div class="nextstepaction"]
> [Öğretici: Time Series Insights'ı kullanarak Azure Digital Twins alanlarınızdan gelen olayları görselleştirme ve analiz etme](tutorial-facilities-analyze.md)
