---
title: IoT iletilerine bağımonolasyon iahleri ekleme w/distributed izleme (önceden)
description: Çözümünüz tarafından kullanılan Azure hizmetlerinde IoT iletilerini izlemek için dağıtılmış izleme yeteneğini nasıl kullanacağınızı öğrenin.
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: jlian
ms.openlocfilehash: efee34ddfb2b2f6090d5dc8c43647c7ee1c53ce2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562437"
---
# <a name="trace-azure-iot-device-to-cloud-messages-with-distributed-tracing-preview"></a>Azure IoT aygıttan buluta iletileri dağıtılmış izleme (önizleme) ile izleme

Microsoft Azure IoT Hub şu anda bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)olarak dağıtılmış izleme destekler.

IoT Hub, dağıtılmış izlemeyi destekleyen ilk Azure hizmetlerinden biridir. Daha fazla Azure hizmeti dağıtılmış izlemeyi destekledikçe, çözümünüzde yer alan Azure hizmetlerinde IoT iletilerini izleyerek izlenebilirsiniz. Dağıtılmış izleme yle ilgili bir arka plan için [bkz.](../azure-monitor/app/distributed-tracing.md)

IoT Hub için dağıtılmış izlemeyi etkinleştirmek size şunları yapmanızı sağlar:

- [İz bağlamını](https://github.com/w3c/trace-context)kullanarak her iletinin akışını IoT Hub üzerinden tam olarak izleyin. Bu izleme bağlamı, bir bileşendeki olayları başka bir bileşenden gelen olaylarla ilişkilendirmenize olanak tanıyan korelasyon işleri içerir. Bir alt küme veya tüm IoT aygıt iletileri için [aygıt ikizi](iot-hub-devguide-device-twins.md)kullanılarak uygulanabilir.
- İzleme bağlamını Azure [Monitor tanı günlüklerine](iot-hub-monitor-resource-health.md)otomatik olarak kaydedin.
- Aygıtlardan IoT Hub'ına ve yönlendirme uç noktalarına ileti akışını ve gecikmesini ölçün ve anlayın.
- IoT çözümünüzde Azure olmayan hizmetler için dağıtılmış izlemeyi nasıl uygulamak istediğinizi düşünmeye başlayın.

Bu makalede, dağıtılmış izleme yle [C için Azure IoT aygıt SDK'sını](iot-hub-device-sdk-c-intro.md) kullanıyorsunuz. Diğer SDK'lar için dağıtılmış izleme desteği hala devam ediyor.

## <a name="prerequisites"></a>Ön koşullar

- Dağıtılmış izleme önizlemesi şu anda yalnızca aşağıdaki bölgelerde oluşturulan IoT Hub'ları için desteklenir:

  - **Kuzey Avrupa**
  - **Güneydoğu Asya**
  - **Batı ABD 2**

- Bu makalede, IoT hub'ınıza telemetri iletileri göndermeye aşina olduğunuzu varsayar. [Telemetri Gönder C Quickstart'ı](quickstart-send-telemetry-c.md)tamamladığınızdan emin olun.

- Aygıtı IoT hub'ınıza (her Quickstart'ta bulunan adımlar) kaydedin ve bağlantı dizesini not edin.

- En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

## <a name="configure-iot-hub"></a>IoT Hub'ı yapılandır

Bu bölümde, dağıtılmış izleme özniteliklerini (korelasyon işleri ve zaman damgaları) gününde gündeme kaydetmek için bir IoT Hub' ı yapımını sızDırırma

1. [Azure portalındaki](https://portal.azure.com/)IoT hub'ınıza gidin.

1. IoT hub'ınız için sol bölmede, **İzleme** bölümüne gidin ve **Tanılama ayarlarını**tıklatın.

1. Tanılama ayarları zaten açık değilse, **tanılamayı aç'ı**tıklatın. Tanılama ayarlarını zaten etkinleştirdiyseniz, **tanılama ayarını ekle'yi**tıklatın.

1. **Ad** alanına, yeni bir tanılama ayarı için bir ad girin. Örneğin, **DistributedTracingSettings**.

1. Günlüğe kaydetmenin nereye gönderileceğini belirleyen aşağıdaki seçeneklerden birini veya birkaçını seçin:

    - **Depolama hesabına arşivleme**: Günlük bilgilerini içerecek şekilde bir depolama hesabını yapılandırın.
    - **Olay merkezine akış**: Günlük bilgilerini içerecek şekilde bir olay hub'ı yapılandırın.
    - **Günlük Analizine Gönder**: Günlük analizi çalışma alanını günlük bilgilerini içerecek şekilde yapılandırın.

1. **Günlük** bölümünde, günlüğe kaydetme bilgilerini istediğiniz işlemleri seçin.

    **DistributedTracing'i**eklediğinizden ve **günlüğe** kaydetmenin kaç gün boyunca bekletinmesini istediğinizden emin olun. Günlük tutma depolama maliyetlerini etkiler.

    ![Dağıtılmış İzleme kategorisinin IoT tanı ayarları için nerede olduğunu gösteren ekran görüntüsü](./media/iot-hub-distributed-tracing/diag-logs.png)

1. Yeni ayar için **Kaydet'i** tıklatın.

1. (İsteğe bağlı) İletilerin farklı yerlere aktığını görmek için yönlendirme [kurallarını en az iki farklı uç noktaya](iot-hub-devguide-messages-d2c.md)ayarlayın.

Günlüğe kaydetme açık olduktan sonra, ioT Hub aşağıdaki durumlardan herhangi birinde geçerli izleme özellikleri içeren bir iletiyle karşılaşıldığında bir günlük kaydeder:

- İletiler IoT Hub'ın ağ geçidine uy.
- İleti IoT Hub tarafından işlenir.
- İleti özel uç noktalara yönlendirilir. Yönlendirme etkinleştirilmelidir.

Bu günlükler ve şemaları hakkında daha fazla bilgi edinmek için, [IoT Hub tanı günlüklerinde Dağıtılmış izleme](iot-hub-monitor-resource-health.md#distributed-tracing-preview)bakın.

## <a name="set-up-device"></a>Cihazı ayarlama

Bu bölümde, [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c)ile kullanılmak üzere bir geliştirme ortamı hazırlayın. Ardından, cihazınızın telemetri iletilerinde dağıtılmış izleme sağlamak için örneklerden birini değiştirirsiniz.

Bu yönergeler, windows'da örnek oluşturmak içindir. Diğer ortamlar [için, PlatformA ÖzgüL Geliştirme için](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#prepackaged-c-sdk-for-platform-specific-development) [C SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#compile) veya Önceden Paketlenmiş C SDK'yı derleme konusuna bakın.

### <a name="clone-the-source-code-and-initialize"></a>Kaynak kodu klonlayın ve başlatma

1. Visual Studio 2019 için ["C++'lı masaüstü geliştirme" iş yükünü](https://docs.microsoft.com/cpp/build/vscpp-step-0-installation?view=vs-2019) yükleyin. Visual Studio 2017 ve 2015 de desteklendi.

1. [CMake'i yükleyin.](https://cmake.org/) Komut isteminden yazarak `PATH` `cmake -version` sizin içinde olduğundan emin olun.

1. Komut istemini veya Git Bash kabuğunu açın. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunun en son sürümünde klonlamak için aşağıdaki komutları çalıştırın:

    ```cmd
    git clone -b public-preview https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

1. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. Dizinden aşağıdaki komutları `azure-iot-sdk-c` çalıştırın:

    ```cmd
    mkdir cmake
    cd cmake
    cmake ..
    ```

    C++ derleyicinizi `cmake` bulamıyorsanız, yukarıdaki komutu çalıştırırken yapı hataları alabilirsiniz. Bu durumda bu komutu [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) çalıştırmayı deneyin. 

    Derleme başarılı olduktan sonra, son birkaç çıkış satırı aşağıdaki çıkışa benzer olacaktır:

    ```cmd
    $ cmake ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```

### <a name="edit-the-send-telemetry-sample-to-enable-distributed-tracing"></a>Dağıtılmış izlemeyi etkinleştirmek için gönderme telemetri örneğini edin

> [!div class="button"]
> <a href="https://github.com/Azure-Samples/azure-iot-distributed-tracing-sample/blob/master/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c" target="_blank">Örneği GitHub'dan alın</a>

1. Kaynak dosyayı `azure-iot-sdk-c/iothub_client/samples/iothub_ll_telemetry_sample/iothub_ll_telemetry_sample.c` açmak için bir düzenleyici kullanın.

1. `connectionString` sabitinin bildirimini bulun:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_config&highlight=2)]

    `connectionString` [Telemetri C Quickstart Gönder'in](./quickstart-send-telemetry-c.md)bir aygıt bölümünde [not](./quickstart-send-telemetry-c.md#register-a-device) aldığınız aygıt bağlantı dizesiyle sabitin değerini değiştirin.

1. Defineyi `MESSAGE_COUNT` şu `5000`şekilde değiştirin:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_config&highlight=3)]

1. İleti döngüsünden önce `IoTHubDeviceClient_LL_SetConnectionStatusCallback` bağlantı durumu geri arama işlevini kaydetmek için çağıran kod satırını bulun. Aygıt için dağıtılmış izleme yi `IoTHubDeviceClient_LL_EnablePolicyConfiguration` etkinleştirmeyi çağırmak için aşağıdaki satırın altına kod ekleyin:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_tracing&highlight=5)]

    İşlev, `IoTHubDeviceClient_LL_EnablePolicyConfiguration` [aygıt ikizleri](./iot-hub-devguide-device-twins.md)aracılığıyla yapılandırılan belirli IoTHub özellikleri için ilkeler sağlar. Yukarıdaki `POLICY_CONFIGURATION_DISTRIBUTED_TRACING` kod satırı yla etkinleştirildiğinde, aygıtın izleme davranışı aygıt ikizinde yapılan dağıtılmış izleme değişikliklerini yansıtır.

1. Tüm kotanızı kullanmadan örnek uygulamanın çalışmasını sağlamak için, ileti gönderme döngüsünün sonuna bir saniyelik bir gecikme ekleyin:

    [!code-c[](~/samples-iot-distributed-tracing/iothub_ll_telemetry_sample-c/iothub_ll_telemetry_sample.c?name=snippet_sleep&highlight=8)]

### <a name="compile-and-run"></a>Derle ve çalıştır

1. Daha önce *iothub_ll_telemetry_sample* oluşturduğunuz CMake dizininden`azure-iot-sdk-c/cmake`iothub_ll_telemetry_sample proje dizinine gidin ve örneği derle:

    ```cmd
    cd iothub_client/samples/iothub_ll_telemetry_sample
    cmake --build . --target iothub_ll_telemetry_sample --config Debug
    ```

1. Uygulamayı çalıştırın. Cihaz dağıtılmış izleme destekleyen telemetri gönderir.

    ```cmd
    Debug/iothub_ll_telemetry_sample.exe
    ```

1. Uygulamayı çalışmaya devam edin. İsteğe bağlı olarak konsol penceresine bakarak IoT Hub'a gönderilen iletiyi gözlemleyin.

<!-- For a client app that can receive sampling decisions from the cloud, check out [this sample](https://aka.ms/iottracingCsample).  -->

### <a name="workaround-for-third-party-clients"></a>Üçüncü taraf istemciler için geçici çözüm

C SDK'yı kullanmadan dağıtılmış izleme özelliğini önizlemek **önemsiz değildir.** Bu nedenle, bu yaklaşım önerilmez.

İlk olarak, geliştirme kılavuzu Oluştur'u izleyerek [ve IoT Hub iletilerini okuyarak](iot-hub-devguide-messages-construct.md)iletilerinizdeki tüm IoT Hub protokol ilkellerini uygulamanız gerekir. Daha sonra, sistem `tracestate` **özelliği**olarak eklemek için MQTT/AMQP iletilerinde protokol özelliklerini düzenleyin. Daha ayrıntılı belirtmek gerekirse:

* MQTT için, `%24.tracestate=timestamp%3d1539243209` ileti konusuna `1539243209` unix zaman damgası biçiminde iletinin oluşturma süresiyle değiştirilmeye ekolarak ekleyin. Örnek olarak, [C SDK'daki](https://github.com/Azure/azure-iot-sdk-c/blob/6633c5b18710febf1af7713cf1a336fd38f623ed/iothub_client/src/iothubtransport_mqtt_common.c#L761) uygulamaya bakın
* AMQP için, `key("tracestate")` `value("timestamp=1539243209")` ekle ve ileti ek açıklama olarak. Başvuru uygulaması için [buraya](https://github.com/Azure/azure-iot-sdk-c/blob/6633c5b18710febf1af7713cf1a336fd38f623ed/iothub_client/src/uamqp_messaging.c#L527)bakın.

Bu özelliği içeren iletilerin yüzdesini denetlemek için, buluttarafından başlatılan olayları dinlemek için mantık uygulayın.

## <a name="update-sampling-options"></a>Örnekleme seçeneklerini güncelleştirme 

Buluttan izlenecek iletilerin yüzdesini değiştirmek için aygıt ikizini güncelleştirmeniz gerekir. Portaldaki JSON düzenleyicisi ve IoT Hub hizmeti SDK dahil olmak üzere bunu birden çok şekilde gerçekleştirebilirsiniz. Aşağıdaki alt bölümlerde örnekler verilmiştir.

### <a name="update-using-the-portal"></a>Portalı kullanarak güncelleştirin

1. [Azure portalındaki](https://portal.azure.com/)IoT hub'ınıza gidin ve **ardından IoT aygıtlarını**tıklatın.

1. Cihazınızı tıklatın.

1. **Dağıtılmış izlemeyi Etkinleştir'i (önizleme)** arayın, ardından **Etkinleştir'i**seçin.

    ![Azure portalında dağıtılmış izleme yi etkinleştirme](./media/iot-hub-distributed-tracing/azure-portal.png)

1. %0 ile %100 arasında bir **Örnekleme oranı** seçin.

1. **Kaydet**'e tıklayın.

1. Birkaç saniye bekleyin ve **Yenile'ye**dokunun, ardından aygıt tarafından başarıyla kabul edilirse, onay işareti olan bir eşitleme simgesi görüntülenir.

1. Telemetri iletisi uygulaması için konsol penceresine geri dön. Uygulama `tracestate` özelliklerinde iletilerin gönderildiğini görürsünüz.

    ![İzleme durumu](./media/iot-hub-distributed-tracing/MicrosoftTeams-image.png)

1. (İsteğe bağlı) Örnekleme oranını farklı bir değerle değiştirin ve iletilerin `tracestate` uygulama özelliklerine dahil olduğu sıklık değişikliğini gözlemleyin.

### <a name="update-using-azure-iot-hub-for-vs-code"></a>VS Kodu için Azure IoT Hub'ı kullanarak güncelleştirme

1. VS Kodu yükleyin, ardından VS Kodu için Azure IoT Hub'ın en son sürümünü [buradan](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)yükleyin.

1. VS Kodunu açın ve [IoT Hub bağlantı dizesini kurun.](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit#user-content-prerequisites)

1. Aygıtı genişletin ve **Dağıtılmış İzleme Ayarı 'nı arayın (Önizleme)**. Altında, alt düğümün **Dağıtılmış İzleme Ayarını (Önizleme) güncelleştir'i** tıklatın.

    ![Azure IoT Hub uzantısında dağıtılmış izleme yi etkinleştirme](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-1.png)

1. Açılan **pencerede, Örnekleme**'ni seçin ve örnekleme hızı olarak 100'u onaylamak için Enter tuşuna basın.

    ![Örnekleme modunu güncelleştir](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-2.png)

    ![Örnekleme oranını güncelleştir](./media/iot-hub-distributed-tracing/update-distributed-tracing-setting-3.png)

### <a name="bulk-update-for-multiple-devices"></a>Birden çok aygıt için toplu güncelleştirme

Birden çok aygıt için dağıtılmış izleme örnekleme yapılandırmasını güncelleştirmek için [otomatik aygıt yapılandırmasını](iot-hub-auto-device-config.md)kullanın. Bu ikiz şema takip emin olun:

```json
{
    "properties": {
        "desired": {
            "azureiot*com^dtracing^1": {
                "sampling_mode": 1,
                "sampling_rate": 100
            }
        }
    }
}
```

| Öğe adı | Gerekli | Tür | Açıklama |
|-----------------|----------|---------|-----------------------------------------------------|
| `sampling_mode` | Evet | Tamsayı | Örneklemeyi açıp kapatmak için şu anda iki mod değeri desteklenir. `1`açıktır ve `2` Kapalıdır. |
| `sampling_rate` | Evet | Tamsayı | Bu değer bir yüzdedir. Yalnızca `0` (dahil) değerlere `100` izin verilir.  |

## <a name="query-and-visualize"></a>Sorgula ve görselleştir

Bir IoT Hub tarafından günlüğe kaydedilen tüm izleri görmek için tanılama ayarlarında seçtiğiniz günlük deposunu sorgulayın. Bu bölümde birkaç farklı seçenek üzerinden yürür.

### <a name="query-using-log-analytics"></a>Günlük Analizini kullanarak sorgula

[Log Analytics'i tanılama günlükleriyle](../azure-monitor/platform/resource-logs-collect-storage.md)kurduysanız, `DistributedTracing` kategorideki günlükleri arayarak sorgulayın. Örneğin, bu sorgu günlüğe kaydedilen tüm izleri gösterir:

```Kusto
// All distributed traces 
AzureDiagnostics 
| where Category == "DistributedTracing" 
| project TimeGenerated, Category, OperationName, Level, CorrelationId, DurationMs, properties_s 
| order by TimeGenerated asc  
```

Log Analytics tarafından gösterildiği gibi örnek günlükler:

| TimeGenerated | ThrottledRequests | Kategori | Düzey | CorrelationId | Sürems | Özellikler |
|--------------------------|---------------|--------------------|---------------|---------------------------------------------------------|------------|------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-02-22T03:28:28.633Z | DiagnosticIoTHubD2C | DistributedTracing | Bilgilendirici | 00-8cd869a412459a25f5b4f31311223344-0144d2590acd909-01 |  | {"deviceId":"AZ3166","messageSize":"96","callerLocalTimeUtc":"2018-02-22T03:27:28.633Z","calleeLocalTimeUtc":"2018-02-22T03:27:28.687Z"} |
| 2018-02-22T03:28:38.633Z | DiagnosticIoTHubIngress | DistributedTracing | Bilgilendirici | 00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01 | 20 | {"isRoutingEnabled":"false","parentSpanid":"0144d2590aacd909"} |
| 2018-02-22T03:28:48.633Z | DiagnostikTHubEgress | DistributedTracing | Bilgilendirici | 00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01 | 23 | {"endpointType":"EventHub","endpointName":"myEventHub", "parentSpanid":"0144d2590aacd909"} |

Farklı günlük türlerini anlamak için [Azure IoT Hub tanı günlüklerine](iot-hub-monitor-resource-health.md#distributed-tracing-preview)bakın.

### <a name="application-map"></a>Uygulama Eşlemesi

IoT iletilerinin akışını görselleştirmek için Uygulama Haritası örnek uygulamasını ayarlayın. Örnek uygulama, dağıtılmış izleme günlüklerini Bir Azure İşlevi ve Etkinlik Hub'ı kullanarak [Uygulama Haritası'na](../application-insights/app-insights-app-map.md) gönderir.

> [!div class="button"]
> <a href="https://github.com/Azure-Samples/e2e-diagnostic-provision-cli" target="_blank">Örneği GitHub'dan alın</a>

Aşağıdaki resimde, üç yönlendirme uç noktası yla Uygulama Haritası'nda dağıtılmış izleme gösterilmektedir:

![IoT, Uygulama Haritası'nda dağıtılan izleme](./media/iot-hub-distributed-tracing/app-map.png)

## <a name="understand-azure-iot-distributed-tracing"></a>Azure IoT dağıtılmış izlemeyi anlama

### <a name="context"></a>Bağlam

Kendi [referans mimarimiz](https://aka.ms/iotrefarchitecture) de dahil olmak üzere birçok IoT çözümü (yalnızca İngilizce), genellikle [microservice mimarisinin](https://docs.microsoft.com/azure/architecture/microservices/)bir varyantını takip eder. Bir IoT çözümü daha karmaşık hale geldikçe, bir düzine veya daha fazla mikro hizmet kullanırsınız. Bu mikro hizmetler Azure'dan olabilir veya olmayabilir. IoT iletilerinin nereye düştüğünü veya yavaşladığını saptamak zor olabilir. Örneğin, 5 farklı Azure hizmeti ve 1500 etkin aygıt kullanan bir IoT çözümünüz vardır. Her cihaz 10 cihazdan buluta/saniye ileti gönderir (toplam 15.000 ileti/saniye için), ancak web uygulamanızın yalnızca 10.000 ileti/saniye gördüğünü fark esiniz. Sorun nerede? Suçluyu nasıl buldunuz?

### <a name="distributed-tracing-pattern-in-microservice-architecture"></a>Mikro hizmet mimarisinde dağıtılmış izleme deseni

Bir IoT iletisinin akışını farklı hizmetler arasında yeniden yapılandırmak için, her hizmet iletiyi benzersiz olarak tanımlayan bir *korelasyon kimliği* yaymalıdır. Merkezi bir sistemde toplandıktan sonra, korelasyon işletmesi ileti akdığını görmenizi sağlar. Bu yöntem, [dağıtılmış izleme deseni](https://docs.microsoft.com/azure/architecture/microservices/logging-monitoring#distributed-tracing)olarak adlandırılır.

Dağıtılmış izleme için daha geniş bir benimsemeyi desteklemek için Microsoft, [dağıtılmış izleme için W3C standart önerisine](https://w3c.github.io/trace-context/)katkıda bulunuyor.

### <a name="iot-hub-support"></a>IoT Hub desteği

Etkinleştirildiğinde, IoT Hub için dağıtılmış izleme desteği bu akışı takip edecektir:

1. IoT aygıtında bir ileti oluşturulur.
1. IoT aygıtı (bulutun yardımıyla) bu iletinin bir izleme bağlamıyla atanmasına karar verir.
1. SDK, ileti `tracestate` oluşturma zaman damgasını içeren ileti uygulaması özelliğine bir ek.
1. IoT aygıtı iletiyi IoT Hub'a gönderir.
1. İleti IoT hub ağ geçidine gelir.
1. IoT Hub `tracestate` ileti uygulaması özellikleri arar ve doğru biçimde olup olmadığını görmek için denetler.
1. Bu nedenle, IoT Hub ileti `trace-id` için genel olarak `span-id` benzersiz bir ,"hop" için bir genel olarak benzersiz `DiagnosticIoTHubD2C`oluşturur ve bunları işlem altında Azure Monitor tanı günlüklerine kaydeder.
1. İleti işleme tamamlandıktan sonra, IoT `span-id` Hub başka bir tane `trace-id` oluşturur ve `DiagnosticIoTHubIngress`işlem altında varolanla birlikte günlüğe kaydeder.
1. İleti için yönlendirme etkinse, IoT Hub bunu özel bitiş noktasına yazar `span-id` ve `trace-id` kategori `DiagnosticIoTHubEgress`altında aynı olan başka bir günlüğe kaydeder.
1. Yukarıdaki adımlar oluşturulan her ileti için yinelenir.

## <a name="public-preview-limits-and-considerations"></a>Genel önizleme sınırları ve dikkat edilmesi gerekenler

- W3C İzleme Bağlamı standardı önerisi şu anda çalışan bir taslaktır.
- Şu anda, istemci SDK tarafından desteklenen tek geliştirme dili C'dir.
- [IoT Hub temel katmanı](iot-hub-scaling.md#basic-and-standard-tiers)için bulut-aygıt çift yeteneği kullanılamaz. Ancak, IoT Hub düzgün oluşturulmuş bir izleme bağlam üstbilgisi görürse Azure Monitor'da oturum açmaya devam eder.
- Verimli çalışma sağlamak için, IoT Hub dağıtılmış izlemenin bir parçası olarak oluşabilecek günlük işlemine bir gaz uygular.

## <a name="next-steps"></a>Sonraki adımlar

- Mikro hizmetlerdeki genel dağıtılmış izleme deseni hakkında daha fazla bilgi edinmek için [Microservice mimari desenine bakın: dağıtılmış izleme.](https://microservices.io/patterns/observability/distributed-tracing.html)
- Dağıtılmış izleme ayarlarını çok sayıda aygıta uygulamak için yapılandırmayı ayarlamak için, [IoT aygıtlarını ölçekte yapılandır'ı ve izleyin.](iot-hub-auto-device-config.md)
- Azure Monitor hakkında daha fazla bilgi edinmek için Azure [Monitor nedir?](../azure-monitor/overview.md)
