---
title: OpenCensus Python ile Azure Application Insights gelen Istek Izleme | Microsoft Docs
description: OpenCensus Python aracılığıyla Python uygulamalarınızın istek çağrılarını izleyin.
ms.topic: conceptual
author: lzchen
ms.author: lechen
ms.date: 10/15/2019
ms.openlocfilehash: 0396bd8d150c6145a39f36e7be9e6e2dcacef2c4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77669956"
---
# <a name="track-incoming-requests-with-opencensus-python"></a>OpenCensus Python ile gelen istekleri izleme

Gelen istek verileri, OpenCensus Python ve çeşitli tümleştirmeler kullanılarak toplanır. Popüler Web çerçeveleri `django` `flask` ve `pyramid`üzerinde oluşturulmuş Web uygulamalarınıza gönderilen gelen istek verilerini izleyin. Veriler daha sonra Azure Izleyici altında telemetri olarak `requests` Application Insights gönderilir.

İlk olarak, Python uygulamanızı en son [Opencensus Python SDK 'sı](../../azure-monitor/app/opencensus-python.md)ile işaretleyin.

## <a name="tracking-django-applications"></a>Docgo uygulamalarını izleme

1. [Pypı](https://pypi.org/project/opencensus-ext-django/) 'den indirip yükleyin `django` `opencensus-ext-django` ve uygulamayı ara yazılım ile işaretleyin. `django` Uygulamanıza gönderilen gelen istekler izlenir.

2. `settings.py` Altına `opencensus.ext.django.middleware.OpencensusMiddleware` `MIDDLEWARE`dosyanıza ekleyin.

    ```python
    MIDDLEWARE = (
        ...
        'opencensus.ext.django.middleware.OpencensusMiddleware',
        ...
    )
    ```

3. AzureExporter 'in `settings.py` altında `OPENCENSUS`düzgün şekilde yapılandırıldığından emin olun.

    ```python
    OPENCENSUS = {
        'TRACE': {
            'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1)',
            'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                connection_string="InstrumentationKey=<your-ikey-here>"
            )''',
        }
    }
    ```

4. Ayrıca, izlemek istemediğiniz isteklerin `settings.py` altına `BLACKLIST_PATHS` URL 'leri ekleyebilirsiniz.

    ```python
    OPENCENSUS = {
        'TRACE': {
            'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=0.5)',
            'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                connection_string="InstrumentationKey=<your-ikey-here>",
            )''',
            'BLACKLIST_PATHS': ['https://example.com'],  <--- These sites will not be traced if a request is sent from it.
        }
    }
    ```

## <a name="tracking-flask-applications"></a>Flask uygulamalarını izleme

1. [Pypı](https://pypi.org/project/opencensus-ext-flask/) 'den indirip yükleyin `flask` `opencensus-ext-flask` ve uygulamayı ara yazılım ile işaretleyin. `flask` Uygulamanıza gönderilen gelen istekler izlenir.

    ```python
    
    from flask import Flask
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.ext.flask.flask_middleware import FlaskMiddleware
    from opencensus.trace.samplers import ProbabilitySampler
    
    app = Flask(__name__)
    middleware = FlaskMiddleware(
        app,
        exporter=AzureExporter(connection_string="InstrumentationKey=<your-ikey-here>"),
        sampler=ProbabilitySampler(rate=1.0),
    )
    
    @app.route('/')
    def hello():
        return 'Hello World!'
    
    if __name__ == '__main__':
        app.run(host='localhost', port=8080, threaded=True)
    
    ```

2. Ara ortamınızı `flask` doğrudan kodda yapılandırabilirsiniz. İzlemek istemediğiniz URL 'lerden gelen istekler için, onları öğesine `BLACKLIST_PATHS`ekleyin.

    ```python
    app.config['OPENCENSUS'] = {
        'TRACE': {
            'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1.0)',
            'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                connection_string="InstrumentationKey=<your-ikey-here>",
            )''',
            'BLACKLIST_PATHS': ['https://example.com'],  <--- These sites will not be traced if a request is sent to it.
        }
    }
    ```

## <a name="tracking-pyramid-applications"></a>Piramit uygulamalarını izleme

1. [Pypı](https://pypi.org/project/opencensus-ext-pyramid/) 'den indirip yükleyin `pyramid` `opencensus-ext-django` ve uygulamanızı ara ile işaretleyin. `pyramid` Uygulamanıza gönderilen gelen istekler izlenir.

    ```python
    def main(global_config, **settings):
        config = Configurator(settings=settings)
    
        config.add_tween('opencensus.ext.pyramid'
                         '.pyramid_middleware.OpenCensusTweenFactory')
    ```

2. `pyramid` Arayı doğrudan kodda yapılandırabilirsiniz. İzlemek istemediğiniz URL 'lerden gelen istekler için, onları öğesine `BLACKLIST_PATHS`ekleyin.

    ```python
    settings = {
        'OPENCENSUS': {
            'TRACE': {
                'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1.0)',
                'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                    connection_string="InstrumentationKey=<your-ikey-here>",
                )''',
                'BLACKLIST_PATHS': ['https://example.com'],  <--- These sites will not be traced if a request is sent to it.
            }
        }
    }
    config = Configurator(settings=settings)
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulama Eşlemesi](../../azure-monitor/app/app-map.md)
* [Kullanılabilirlik](../../azure-monitor/app/monitor-web-app-availability.md)
* [Arama](../../azure-monitor/app/diagnostic-search.md)
* [Log (Analytics) sorgusu](../../azure-monitor/log-query/log-query-overview.md)
* [İşlem tanılamaları](../../azure-monitor/app/transaction-diagnostics.md)
