---
title: Azure Izleyici (Önizleme) ile Python uygulamalarını izleme | Microsoft Docs
description: Azure Izleyici ile OpenCensus Python 'ı bağlamak için yönergeler sağlar
ms.topic: conceptual
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6ef0675e3ae3f7a5da38138177f3033051723411
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79537117"
---
# <a name="set-up-azure-monitor-for-your-python-application"></a>Python uygulamanız için Azure Izleyicisini ayarlama

Azure Izleyici, [Opencensus](https://opencensus.io)ile tümleştirme yoluyla, Python uygulamalarının dağıtılmış izlemeyi, ölçüm toplamayı ve günlüğe kaydedilmesini destekler. Bu makale, Python için OpenCensus ayarlama ve izleme verilerinizi Azure Izleyici 'ye gönderme sürecinde size yol gösterecektir.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- Python yüklemesi. Bu makalede [Python 3.7.0](https://www.python.org/downloads/)kullanılmaktadır, ancak önceki sürümler büyük olasılıkla küçük değişikliklerle çalışacaktır.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-an-application-insights-resource-in-azure-monitor"></a>Azure Izleyici 'de Application Insights kaynağı oluşturma

İlk olarak Azure Izleyici 'de bir Application Insights kaynak oluşturmanız gerekir ve bu işlem bir izleme anahtarı (Ikey) oluşturur. Ikey daha sonra, Azure Izleyici 'ye telemetri verileri gönderecek şekilde OpenCensus SDK 'sını yapılandırmak için kullanılır.

1. **Kaynak** > oluştur**Geliştirici Araçları** > **Application Insights**seçin.

   ![Application Insights kaynağı ekleme](./media/opencensus-python/0001-create-resource.png)

1. Bir yapılandırma kutusu görünür. Giriş alanlarını doldurun için aşağıdaki tabloyu kullanın.

   | Ayar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Adı**      | Genel benzersiz değer | İzlemekte olduğunuz uygulamayı tanımlayan ad |
   | **Kaynak grubu**     | myResourceGroup      | Application Insights verileri barındıracak yeni kaynak grubunun adı |
   | **Konum** | Doğu ABD | Size yakın veya uygulamanızın barındırıldığı yerin yakınında bir konum |

1. **Oluştur**’u seçin.

## <a name="instrument-with-opencensus-python-sdk-for-azure-monitor"></a>Azure Izleyici için OpenCensus Python SDK 'Sı ile işaretleme

OpenCensus Azure Izleyici exporler 'i yükler:

```console
python -m pip install opencensus-ext-azure
```

Paketlerin ve tümleştirmelerin tam listesi için bkz. [Opencensus paketleri](https://docs.microsoft.com/azure/azure-monitor/app/nuget#common-packages-for-python-using-opencensus).

> [!NOTE]
> Komutu `python -m pip install opencensus-ext-azure` , Python yüklemeniz için ayarlanmış bir `PATH` ortam değişkeni olduğunu varsayar. Bu değişkeni yapılandırmadıysanız, Python yürütülebilirinizin bulunduğu konuma tam dizin yolu sağlamanız gerekir. Sonuç şöyle bir komuttur: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`.

SDK, Azure Izleyici 'ye farklı telemetri türleri göndermek için üç Azure Izleyici dışarı aktarmak kullanır: izleme, ölçümler ve Günlükler. Bu telemetri türleri hakkında daha fazla bilgi için bkz. [veri platformuna genel bakış](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform). Bu telemetri türlerini üç dışarı aktarmak yoluyla göndermek için aşağıdaki yönergeleri kullanın.

## <a name="telemetry-type-mappings"></a>Telemetri türü eşlemeleri

OpenCensus 'ın, Azure Izleyici 'de göreceğiniz telemetri türleriyle eşlendiğini sağlayan dışarı layıcılar aşağıda verilmiştir.

![OpenCensus 'den Azure Izleyici 'ye telemetri türlerinin eşlenmesinin ekran görüntüsü](./media/opencensus-python/0012-telemetry-types.png)

### <a name="trace"></a>İzleme

> [!NOTE]
> `Trace`OpenCensus ' de [Dağıtılmış izlemeyi](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing)ifade eder. Azure `AzureExporter` izleyici `requests` 'ye `dependency` gönderir ve telemetri sağlar.

1. İlk olarak, bazı izleme verilerini yerel olarak oluşturalım. Python boş veya istediğiniz Düzenleyicinizde aşağıdaki kodu girin.

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2. Kodu çalıştırmak, sürekli olarak bir değer girmenizi ister. Her giriş ile, değer kabuğa yazdırılır ve OpenCensus Python modülü buna karşılık gelen bir parçasını oluşturacaktır `SpanData`. OpenCensus projesi bir izlemeyi bir [yayılma ağacı olarak](https://opencensus.io/core-concepts/tracing/)tanımlar.
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

3. Değer girilmesi, sunum amacıyla yararlı olsa da, sonunda Azure Izleyici `SpanData` 'ye yayma yapmak istiyoruz. Bağlantı dizenizi doğrudan dışarı aktarılmasına geçirin veya bir ortam değişkeninde `APPLICATIONINSIGHTS_CONNECTION_STRING`belirtebilirsiniz. Aşağıdaki kod örneğine göre kodunuzu önceki adımdan değiştirin:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

4. Artık Python betiğini çalıştırdığınızda, yine de değer girmeniz istenir, ancak yalnızca değeri kabukta yazdırılıyor. Oluşturulan `SpanData` Azure izleyici 'ye gönderilir. Yayınlanan yayılmış verileri ' de bulabilirsiniz `dependencies`. Giden istekler hakkında daha fazla bilgi için bkz. OpenCensus Python [bağımlılıkları](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-dependency).
Gelen istekler hakkında daha fazla bilgi için bkz. OpenCensus Python [istekleri](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-request).

#### <a name="sampling"></a>Örnekleme

OpenCensus 'de örnekleme hakkında bilgi edinmek için, [opencensus 'de örneklemeye](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications)göz atın.

#### <a name="trace-correlation"></a>İzleme bağıntısı

İzleme verilerinizde telemetri bağıntısı hakkında ayrıntılı bilgi edinmek için, OpenCensus Python [telemetri bağıntı](https://docs.microsoft.com/azure/azure-monitor/app/correlation#telemetry-correlation-in-opencensus-python)' na göz atın.

#### <a name="modify-telemetry"></a>Telemetriyi değiştirme

İzlenen Telemetriyi Azure Izleyici 'ye gönderilmeden önce değiştirme hakkında daha fazla bilgi için bkz. OpenCensus Python [telemetri işlemcileri](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors).

### <a name="metrics"></a>Ölçümler

1. İlk olarak, bazı yerel ölçüm verileri oluşturalım. Kullanıcının Enter tuşuna bastığı süreyi izlemek için basit bir ölçüm oluşturacağız.

    ```python
    from datetime import datetime
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```
2. Kodu çalıştırmak, sürekli olarak ENTER tuşuna basmanız istenir. ENTER tuşuna basıldığı sayıyı izlemek için bir ölçüm oluşturulur. Her giriş ile, değer artırılır ve ölçüm bilgileri konsolda görüntülenir. Bu bilgiler, ölçüm güncellendiği zaman geçerli değeri ve geçerli zaman damgasını içerir.

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. Değer girilmesi, tanıtım amacıyla yararlı olsa da, sonunda ölçüm verilerini Azure Izleyici 'ye yayma istiyoruz. Bağlantı dizenizi doğrudan dışarı aktarılmasına geçirin veya bir ortam değişkeninde `APPLICATIONINSIGHTS_CONNECTION_STRING`belirtebilirsiniz. Aşağıdaki kod örneğine göre kodunuzu önceki adımdan değiştirin:

    ```python
    from datetime import datetime
    from opencensus.ext.azure import metrics_exporter
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    # TODO: replace the all-zero GUID with your instrumentation key.
    exporter = metrics_exporter.new_metrics_exporter(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')

    view_manager.register_exporter(exporter)

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```

4. Dışarı Aktarıcı, ölçüm verilerini Azure Izleyici 'ye sabit bir aralıkla gönderir. Varsayılan değer 15 saniyedir. Tek bir ölçümü izliyoruz, bu nedenle içerdiği değer ve zaman damgasıyla birlikte bu ölçüm verileri her aralığa gönderilir. Verileri altında `customMetrics`bulabilirsiniz.

#### <a name="standard-metrics"></a>Standart ölçümler

Varsayılan olarak, ölçüm Dışarı Aktarıcı Azure Izleyici 'ye bir dizi standart ölçüm gönderir. Bunu, ölçüm verme programı oluşturucusunda `enable_standard_metrics` bayrağını olarak `False` ayarlayarak devre dışı bırakabilirsiniz.

    ```python
    ...
    exporter = metrics_exporter.new_metrics_exporter(
      enable_standard_metrics=False,
      connection_string='InstrumentationKey=<your-instrumentation-key-here>')
    ...
    ```
Şu anda gönderilen standart ölçümlerin bir listesi aşağıda verilmiştir:

- Kullanılabilir bellek (bayt)
- CPU Işlemci zamanı (yüzde)
- Gelen Istek oranı (saniye başına)
- Gelen Istek ortalama yürütme süresi (milisaniye)
- Giden Istek hızı (saniye başına)
- İşlem CPU kullanımı (yüzde)
- İşlem özel baytları (bayt)

Bu ölçümleri ' de `performanceCounters`görebilmeniz gerekir. Gelen istek oranı altında `customMetrics`olur. Daha fazla bilgi için bkz. [performans sayaçları](https://docs.microsoft.com/azure/azure-monitor/app/performance-counters).

#### <a name="modify-telemetry"></a>Telemetriyi değiştirme

İzlenen Telemetriyi Azure Izleyici 'ye gönderilmeden önce değiştirme hakkında daha fazla bilgi için bkz. OpenCensus Python [telemetri işlemcileri](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors).

### <a name="logs"></a>Günlükler

1. İlk olarak, bazı yerel günlük verileri oluşturalım.

    ```python
    import logging

    logger = logging.getLogger(__name__)

    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2.  Kod sürekli olarak bir değer girilmesini ister. Her girilen değer için bir günlük girişi yayınlanır.

    ```
    Enter a value: 24
    24
    Enter a value: 55
    55
    Enter a value: 123
    123
    Enter a value: 90
    90
    ```

3. Değer girilmesi, tanıtım amacıyla yararlı olsa da, sonuçta günlük verilerini Azure Izleyici 'ye yaymak istiyoruz. Bağlantı dizenizi doğrudan dışarı aktarılmasına geçirin veya bir ortam değişkeninde `APPLICATIONINSIGHTS_CONNECTION_STRING`belirtebilirsiniz. Aşağıdaki kod örneğine göre kodunuzu önceki adımdan değiştirin:

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

4. Dışarı aktarma programı günlük verilerini Azure Izleyici 'ye gönderir. Verileri altında `traces`bulabilirsiniz. 

> [!NOTE]
> `traces`Bu bağlamda ile `Tracing`aynı değildir. `traces`kullanarak Azure Izleyici 'de göreceğiniz telemetri türünü ifade eder `AzureLogHandler`. `Tracing`OpenCensus içindeki bir kavramı ifade eder ve [Dağıtılmış izleme](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing)ile ilgilidir.

5. Günlük iletilerinizi biçimlendirmek için, yerleşik Python `formatters` [günlüğü API](https://docs.python.org/3/library/logging.html#formatter-objects)'sinde öğesini kullanabilirsiniz.

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    format_str = '%(asctime)s - %(levelname)-8s - %(message)s'
    date_format = '%Y-%m-%d %H:%M:%S'
    formatter = logging.Formatter(format_str, date_format)
    # TODO: replace the all-zero GUID with your instrumentation key.
    handler = AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    handler.setFormatter(formatter)
    logger.addHandler(handler)
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

6. Ayrıca, custom_dimensions alanını kullanarak *fazladan* anahtar sözcük bağımsız değişkenine günlük iletilerinize özel özellikler ekleyebilirsiniz. Bunlar, Azure Izleyici `customDimensions` 'de anahtar-değer çiftleri olarak görünür.
> [!NOTE]
> Bu özelliğin çalışması için custom_dimensions alana bir sözlük geçirmeniz gerekir. Diğer herhangi bir türün bağımsız değişkenlerini geçirirseniz, günlükçü bunları yoksayar.

    ```python
    import logging
    
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )

    properties = {'custom_dimensions': {'key_1': 'value_1', 'key_2': 'value_2'}}

    # Use properties in logging statements
    logger.warning('action', extra=properties)
    ```

#### <a name="sending-exceptions"></a>Özel durumlar gönderiliyor

OpenCensus Python, Telemetriyi otomatik olarak `exception` izlemez ve göndermez. Bunlar, `AzureLogHandler` Python günlüğü kitaplığı aracılığıyla özel durumlar kullanılarak üzerinden gönderilir. Normal günlüğe kaydetme ile tıpkı özel özellikler ekleyebilirsiniz.

    ```python
    import logging
    
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )

    properties = {'custom_dimensions': {'key_1': 'value_1', 'key_2': 'value_2'}}

    # Use properties in exception logs
    try:
        result = 1 / 0  # generate a ZeroDivisionError
    except Exception:
        logger.exception('Captured an exception.', extra=properties)
    ```
Özel durumları açıkça günlüğe yazmanız gerektiğinden, bu, işlenmemiş özel durumları nasıl günlüğe kaydetmek istediğinizdeki kullanıcıya kadar olur. OpenCensus, bir özel durum telemetrisini açık bir şekilde günlüğe kaydeden sürece kullanıcının bunu nasıl yapmak istediğini gösteren kısıtlamalar yerleştirmez.

#### <a name="sampling"></a>Örnekleme

OpenCensus 'de örnekleme hakkında bilgi edinmek için, [opencensus 'de örneklemeye](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications)göz atın.

#### <a name="log-correlation"></a>Günlük bağıntısı

Günlüklerinizi izleme bağlamı verileriyle zenginleştirme hakkında daha fazla bilgi için bkz. OpenCensus Python [günlükleri tümleştirmesi](https://docs.microsoft.com/azure/azure-monitor/app/correlation#log-correlation).

#### <a name="modify-telemetry"></a>Telemetriyi değiştirme

İzlenen Telemetriyi Azure Izleyici 'ye gönderilmeden önce değiştirme hakkında daha fazla bilgi için bkz. OpenCensus Python [telemetri işlemcileri](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors).

## <a name="view-your-data-with-queries"></a>Sorgular ile verilerinizi görüntüleme

Uygulamalarınızdan gönderilen telemetri verilerini **Günlükler (Analiz)** sekmesinden görüntüleyebilirsiniz.

![Kırmızı kutuda seçilen "Günlükler (Analiz)" ile genel bakış bölmesinin ekran görüntüsü](./media/opencensus-python/0010-logs-query.png)

**Etkin**altındaki listede:

- Azure Izleyici izleme dışarı aktarıcı ile gönderilen telemetri için, gelen istekler altında `requests`görünür. Giden veya işlem içi istekler altında `dependencies`görünür.
- Azure Izleyici ölçümleri dışarı aktarıcı ile gönderilen telemetri için, gönderilen ölçümler altında `customMetrics`görüntülenir.
- Azure Izleyici günlükleri dışarı aktarıcı ile gönderilen telemetri için Günlükler altında `traces`görüntülenir. Özel durumlar altında `exceptions`görünür.

Sorguları ve günlükleri kullanma hakkında daha ayrıntılı bilgi için bkz. [Azure izleyici 'de Günlükler](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs).

## <a name="learn-more-about-opencensus-for-python"></a>Python için OpenCensus hakkında daha fazla bilgi edinin

* [GitHub 'da OpenCensus Python](https://github.com/census-instrumentation/opencensus-python)
* [Özelleştirme](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [GitHub 'da Azure Izleyici dışarı vericiler](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)
* [OpenCensus tümleştirmeleri](https://github.com/census-instrumentation/opencensus-python#extensions)
* [Azure Izleyici örnek uygulamaları](https://github.com/Azure-Samples/azure-monitor-opencensus-python)

## <a name="next-steps"></a>Sonraki adımlar

* [Gelen istekleri izleme](./../../azure-monitor/app/opencensus-python-dependency.md)
* [Devam eden istekleri izleme](./../../azure-monitor/app/opencensus-python-request.md)
* [Uygulama eşlemesi](./../../azure-monitor/app/app-map.md)
* [Uçtan uca performans izleme](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>Uyarılar

* [Kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md): Sitenizin web’de görünür olduğundan emin olmaya yönelik testler oluşturun.
* [Akıllı tanılama](../../azure-monitor/app/proactive-diagnostics.md): Bu testler otomatik olarak çalıştığından, bunları ayarlamak için herhangi bir şey yapmanız gerekmez. Uygulamanızda olağan dışı oranda başarısız istek olup olmadığını bildirirler.
* [Ölçüm uyarıları](../../azure-monitor/app/alerts.md): bir ölçüm bir eşiği kesiştyolursa sizi uyarmak için Uyarılar ayarlayın. Bunları, uygulamanıza kodladığınız özel ölçümlerde ayarlayabilirsiniz.
