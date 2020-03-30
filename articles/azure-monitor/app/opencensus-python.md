---
title: Azure Monitor ile Python uygulamalarını izleme (önizleme) | Microsoft Dokümanlar
description: OpenCensus Python'u Azure Monitor ile kablolamak için yönergeler sağlar
ms.topic: conceptual
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6ef0675e3ae3f7a5da38138177f3033051723411
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537117"
---
# <a name="set-up-azure-monitor-for-your-python-application"></a>Python uygulamanız için Azure Monitör'ü ayarlama

Azure Monitor, [OpenCensus](https://opencensus.io)ile tümleştirme yoluyla Python uygulamalarının dağıtılmış izleme, metrik toplama ve günlüğe kaydetmeyi destekler. Bu makale, Python için OpenCensus'u ayarlama ve izleme verilerinizi Azure Monitor'a gönderme işleminde size yol açacaktır.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
- Python kurulumu. Bu makalede [Python 3.7.0](https://www.python.org/downloads/)kullanır, ancak önceki sürümleri büyük olasılıkla küçük değişiklikler ile çalışacaktır.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-an-application-insights-resource-in-azure-monitor"></a>Azure Monitör'de Uygulama Öngörüleri kaynağı oluşturma

Öncelikle Azure Monitor'da bir enstrümantasyon anahtarı (ikey) oluşturacak bir Uygulama Öngörüleri kaynağı oluşturmanız gerekir. Ikey daha sonra Azure Monitor'a telemetri verilerini göndermek için OpenCensus SDK'yı yapılandırmak için kullanılır.

1. **Kaynak** > **Oluştur Geliştirici araçları** > **Uygulama Öngörüleri'ni**seçin.

   ![Uygulama Öngörüleri kaynağı ekleme](./media/opencensus-python/0001-create-resource.png)

1. Bir yapılandırma kutusu görüntülenir. Giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

   | Ayar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Adı**      | Küresel olarak benzersiz değer | İzlediğiniz uygulamayı tanımlayan ad |
   | **Kaynak Grubu**     | myResourceGroup      | Application Insights verilerini barındıracak yeni kaynak grubunun adı |
   | **Konum** | Doğu ABD | Yakınınızdaki veya uygulamanızın barındırıldığı yerin yakınında bir konum |

1. **Oluştur'u**seçin.

## <a name="instrument-with-opencensus-python-sdk-for-azure-monitor"></a>Azure Monitör için OpenCensus Python SDK'lı araç

OpenCensus Azure Monitörü ihracatçılarını yükleyin:

```console
python -m pip install opencensus-ext-azure
```

Paketlerin ve tümleştirmelerin tam listesi için [OpenCensus paketlerine](https://docs.microsoft.com/azure/azure-monitor/app/nuget#common-packages-for-python-using-opencensus)bakın.

> [!NOTE]
> Komut, `python -m pip install opencensus-ext-azure` Python yüklemeniz `PATH` için ayarlanan bir ortam değişkeniniz olduğunu varsayar. Bu değişkeni yapılandırmadıysanız, Python çalıştırılabilir inizin bulunduğu yere tam dizin yolunu vermeniz gerekir. Sonuç şu gibi bir `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`komuttur: .

SDK, Azure Monitor'a farklı türde telemetri göndermek için üç Azure Monitor ihracatçısı kullanır: izleme, ölçümler ve günlükler. Bu telemetri türleri hakkında daha fazla bilgi için [veri platformuna genel bakış](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform)alabakın. Üç ihracatçılar aracılığıyla bu telemetri türleri göndermek için aşağıdaki talimatları kullanın.

## <a name="telemetry-type-mappings"></a>Telemetri türü eşlemeleri

OpenCensus'un sağladığı ihracatçılar, Azure Monitor'da göreceğiniz telemetri türlerinin eşlendiğini görebilirsiniz.

![OpenCensus'dan Azure Monitor'a telemetri türlerinin eşleme sinin ekran görüntüsü](./media/opencensus-python/0012-telemetry-types.png)

### <a name="trace"></a>İzleme

> [!NOTE]
> `Trace`[OpenCensus'da dağıtılmış izleme](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing)anlamına gelir. Azure `AzureExporter` `requests` Monitor'a göndermeler ve `dependency` telemetri.

1. İlk olarak, yerel olarak bazı izleme verileri oluşturalım. Python IDLE veya seçtiğiniz düzenleyicide aşağıdaki kodu girin.

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

2. Kodu çalıştırmak, bir değer girmenizi sürekli olarak ister. Her girişte, değer kabuk yazdırılır ve OpenCensus Python Modülü karşılık gelen `SpanData`bir parça üretecek. OpenCensus projesi bir [izlemeyi açıklıklar ağacı olarak](https://opencensus.io/core-concepts/tracing/)tanımlar.
    
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

3. Değerleri girmek gösteri amacıyla yararlı olsa da, sonuçta Azure `SpanData` Monitor'a vermek istiyoruz. Bağlantı dizenizi doğrudan ihracatçıya geçirin veya bir `APPLICATIONINSIGHTS_CONNECTION_STRING`ortam değişkeninde belirtebilirsiniz. Aşağıdaki kod örneğini temel alan kodunuzu önceki adımdan değiştirin:

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

4. Python komut dosyasını çalıştırdığınızda, yine de değerleri girmeniz istenir, ancak kabukta yalnızca değer yazdırılır. Oluşturulan `SpanData` Azure Monitor'a gönderilir. Yayılan yayılma verilerini ' nin `dependencies`altında bulabilirsiniz. Giden istekler hakkında daha fazla bilgi için OpenCensus Python [bağımlılıklarına](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-dependency)bakın.
Gelen istekler hakkında daha fazla bilgi için OpenCensus Python [isteklerine](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-request)bakın.

#### <a name="sampling"></a>Örnekleme

OpenCensus örnekleme hakkında bilgi için, [OpenCensus örnekleme](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications)bir göz atın.

#### <a name="trace-correlation"></a>İz korelasyon

İzleme verilerinizdeki telemetri korelasyonhakkında ayrıntılı bilgi için OpenCensus Python [telemetri korelasyonuna](https://docs.microsoft.com/azure/azure-monitor/app/correlation#telemetry-correlation-in-opencensus-python)bir göz atın.

#### <a name="modify-telemetry"></a>Telemetriyi değiştirin

İzlenen telemetrinin Azure Monitor'a gönderilmeden önce nasıl değiştirilebildiğini öğrenmek için OpenCensus Python [telemetri işlemcilerine](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors)bakın.

### <a name="metrics"></a>Ölçümler

1. İlk olarak, bazı yerel metrik veriler oluşturalım. Kullanıcının Enter tuşuna kaç kez basmasını izlemek için basit bir metrik oluştururuz.

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
2. Kodu çalıştırmak, tekrar tekrar Enter tuşuna basmanızı ister. Enter tuşuna basıldığında n için kaç kez basıldığını izlemek için bir metrik oluşturulur. Her girişte değer artımlı olur ve metrik bilgiler konsolda görüntülenir. Bilgiler, ölçüm güncelleştirildiğinde geçerli değeri ve geçerli zaman damgasını içerir.

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. Değerleri girmek gösterim amacıyla yararlı olsa da, sonuçta metrik verileri Azure Monitor'a yalamak istiyoruz. Bağlantı dizenizi doğrudan ihracatçıya geçirin veya bir `APPLICATIONINSIGHTS_CONNECTION_STRING`ortam değişkeninde belirtebilirsiniz. Aşağıdaki kod örneğini temel alan kodunuzu önceki adımdan değiştirin:

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

4. İhracatçı metrik verileri belirli bir aralıkta Azure Monitor'a gönderir. Varsayılan değer her 15 saniyede birdir. Tek bir metrik takip ediyoruz, bu yüzden bu metrik veriler, içerdiği değer ve zaman damgası ile her aralıkta gönderilecek. Verileri . `customMetrics`

#### <a name="standard-metrics"></a>Standart ölçümler

Varsayılan olarak, metrik ihracatçısı Azure Monitor'a bir dizi standart ölçüm gönderir. Bu, `enable_standard_metrics` bayrağı ölçüihracatçının `False` oluşturucusuna ayarlayarak devre dışı kullanabilirsiniz.

    ```python
    ...
    exporter = metrics_exporter.new_metrics_exporter(
      enable_standard_metrics=False,
      connection_string='InstrumentationKey=<your-instrumentation-key-here>')
    ...
    ```
Aşağıda, şu anda gönderilen standart ölçümlerin bir listesi verilmiştir:

- Kullanılabilir Bellek (bayt)
- CPU İşlemci Süresi (yüzde)
- Gelen İstek Oranı (saniyede)
- Gelen İstek Ortalama Yürütme Süresi (milisaniye)
- Giden İstek Oranı (saniyede)
- İşlem CPU Kullanımı (yüzde)
- İşlem Özel Bayt (bayt)

Bu ölçümleri `performanceCounters`. Gelen istek oranı altında `customMetrics`olacaktır. Daha fazla bilgi için [performans sayaçlarına](https://docs.microsoft.com/azure/azure-monitor/app/performance-counters)bakın.

#### <a name="modify-telemetry"></a>Telemetriyi değiştirin

İzlenen telemetrinin Azure Monitor'a gönderilmeden önce nasıl değiştirilebildiğini öğrenmek için OpenCensus Python [telemetri işlemcilerine](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors)bakın.

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

2.  Kod sürekli olarak bir değerin girilmesini ister. Girilen her değer için bir günlük girişi yayılır.

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

3. Değerleri girmek gösterim amacıyla yararlı olsa da, sonuçta günlük verilerini Azure Monitor'a yalamak istiyoruz. Bağlantı dizenizi doğrudan ihracatçıya geçirin veya bir `APPLICATIONINSIGHTS_CONNECTION_STRING`ortam değişkeninde belirtebilirsiniz. Aşağıdaki kod örneğini temel alan kodunuzu önceki adımdan değiştirin:

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

4. İhracatçı günlük verilerini Azure Monitor'a gönderir. Verileri . `traces` 

> [!NOTE]
> `traces`bu bağlamda aynı `Tracing`değildir. `traces`'yi kullanırken Azure Monitor'da göreceğiniz telemetri `AzureLogHandler`türünü ifade eder. `Tracing`OpenCensus bir kavram anlamına gelir ve [dağıtılmış izleme](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing)ile ilgilidir.

5. Günlük iletilerinizi biçimlendirmek için `formatters` yerleşik Python [günlük API'sini](https://docs.python.org/3/library/logging.html#formatter-objects)kullanabilirsiniz.

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

6. Ayrıca, custom_dimensions alanını kullanarak *ek* anahtar kelime bağımsız değişkeninde günlük iletilerinize özel özellikler ekleyebilirsiniz. Bunlar Azure Monitor'da `customDimensions` anahtar değer çiftleri olarak görünür.
> [!NOTE]
> Bu özelliğin çalışması için custom_dimensions alanına bir sözlük geçirmeniz gerekir. Başka bir türbağımsız değişkenler geçerseniz, kaydedici bunları yoksayacaktır.

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

#### <a name="sending-exceptions"></a>Özel durumlar gönderme

OpenCensus Python otomatik olarak izlemez ve telemetri göndermez. `exception` Python günlük kitaplığı aracılığıyla özel durumlar kullanılarak gönderilir. `AzureLogHandler` Normal günlüğe kaydetmede olduğu gibi özel özellikler ekleyebilirsiniz.

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
Özel durumları açıkça günlüğe kaydetmeniz gerektiğinden, işlenmemiş özel durumları nasıl günlüğe kaydetmek istedikleri kullanıcıya aittir. OpenCensus, bir özel durum telemetrisini açıkça günlüğe kaydettikleri sürece, kullanıcının bunu nasıl yapmak istediğine kısıtlama lar yerleştirmez.

#### <a name="sampling"></a>Örnekleme

OpenCensus örnekleme hakkında bilgi için, [OpenCensus örnekleme](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications)bir göz atın.

#### <a name="log-correlation"></a>Günlük korelasyon

İzleme bağlam verileriyle günlüklerinizi nasıl zenginleştirin izle ilgili ayrıntılar için OpenCensus Python [günlükleri tümleştirmesine](https://docs.microsoft.com/azure/azure-monitor/app/correlation#log-correlation)bakın.

#### <a name="modify-telemetry"></a>Telemetriyi değiştirin

İzlenen telemetrinin Azure Monitor'a gönderilmeden önce nasıl değiştirilebildiğini öğrenmek için OpenCensus Python [telemetri işlemcilerine](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors)bakın.

## <a name="view-your-data-with-queries"></a>Verilerinizi sorgularla görüntüleme

Uygulamanızdan gönderilen telemetri verilerini **Günlükler (Analytics)** sekmesinden görüntüleyebilirsiniz.

![Kırmızı bir kutuda seçilen "Günlükler (Analytics)" ile genel bakış bölmesinin ekran görüntüsü](./media/opencensus-python/0010-logs-query.png)

**Etkin**altındaki listede:

- Azure Monitor izleme ihracatçısı ile gönderilen telemetri `requests`için gelen istekler . Giden veya işlem içi istekler altında `dependencies`görünür.
- Azure Monitor ölçümleri ihracatçısı ile gönderilen telemetri `customMetrics`için gönderilen ölçümler ' in altında görünür.
- Azure Monitor günlükleri ihracatçısı ile gönderilen telemetri `traces`için günlükler . Özel durumlar `exceptions`altında görünür.

Sorguların ve günlüklerin nasıl kullanılacağı hakkında daha ayrıntılı bilgi için [Azure Monitor'daki Günlükler'e](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs)bakın.

## <a name="learn-more-about-opencensus-for-python"></a>Python için OpenCensus hakkında daha fazla bilgi edinin

* [GitHub'da OpenCensus Python](https://github.com/census-instrumentation/opencensus-python)
* [Özelleştirme](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Azure Monitör İhracatçıları GitHub'da](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)
* [OpenCensus Entegrasyonları](https://github.com/census-instrumentation/opencensus-python#extensions)
* [Azure Monitör Örnek Uygulamaları](https://github.com/Azure-Samples/azure-monitor-opencensus-python)

## <a name="next-steps"></a>Sonraki adımlar

* [Gelen istekleri izleme](./../../azure-monitor/app/opencensus-python-dependency.md)
* [Giden istekleri izleme](./../../azure-monitor/app/opencensus-python-request.md)
* [Uygulama haritası](./../../azure-monitor/app/app-map.md)
* [Uçuça performans izleme](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>Uyarılar

* [Kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md): Sitenizin web’de görünür olduğundan emin olmaya yönelik testler oluşturun.
* [Akıllı tanılama](../../azure-monitor/app/proactive-diagnostics.md): Bu testler otomatik olarak çalıştığından, bunları ayarlamak için herhangi bir şey yapmanız gerekmez. Uygulamanızda olağan dışı oranda başarısız istek olup olmadığını bildirirler.
* [Metrik uyarılar](../../azure-monitor/app/alerts.md): Bir metrik bir eşiği geçerse sizi uyarmak için uyarılar ayarlayın. Bunları, uygulamanıza kodladığınız özel ölçümlerde ayarlayabilirsiniz.
