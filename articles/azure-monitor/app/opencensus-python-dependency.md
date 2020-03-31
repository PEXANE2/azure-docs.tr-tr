---
title: OpenCensus Python ile Azure Uygulama Öngörülerinde Bağımlılık İzleme | Microsoft Dokümanlar
description: OpenCensus Python aracılığıyla Python uygulamalarınız için bağımlılık çağrılarını izleyin.
ms.topic: conceptual
author: lzchen
ms.author: lechen
ms.date: 10/15/2019
ms.openlocfilehash: e400669fd96518adead74a81fc332767c5f9b23b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669939"
---
# <a name="track-dependencies-with-opencensus-python"></a>OpenCensus Python ile bağımlılıkları izleme

Bağımlılık, uygulamanız tarafından çağrılan harici bir bileşendir. Bağımlılık verileri OpenCensus Python ve çeşitli tümleştirmeleri kullanılarak toplanır. Veriler daha sonra Azure Monitor altında uygulama `dependencies` öngörüleri için telemetri olarak gönderilir.

İlk olarak, en son [OpenCensus Python SDK](../../azure-monitor/app/opencensus-python.md)ile Python uygulamanızı enstrüman.

## <a name="in-process-dependencies"></a>Süreç içi bağımlılıklar

Azure Monitor için OpenCensus Python SDK,"süreç içi" bağımlılık telemetrisi (uygulamanızda oluşan bilgi ve mantık) göndermenize olanak tanır. Süreç içi bağımlılıklar, `type` analitikte `INPROC` olduğu gibi alana sahip olacaktır.

```python
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracer

tracer = Tracer(exporter=AzureExporter(connection_string="InstrumentationKey=<your-ikey-here>"), sampler=ProbabilitySampler(1.0))

with tracer.span(name='foo'): # <-- A dependency telemetry item will be sent for this span "foo"
    print('Hello, World!')
```

## <a name="dependencies-with-requests-integration"></a>"İstek" tümleştirmesi ile bağımlılıklar

OpenCensus `requests` tümleştirmesi ile giden isteklerinizi izleyin.

`opencensus-ext-requests` [PyPI'den](https://pypi.org/project/opencensus-ext-requests/) indirin ve yükleyin ve izleme tümleştirmelerine ekleyin. Python istekleri kitaplığı kullanılarak gönderilen [istekler](https://pypi.org/project/requests/) izlenir.

```python
import requests
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace import config_integration
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracer

config_integration.trace_integrations(['requests'])  # <-- this line enables the requests integration

tracer = Tracer(exporter=AzureExporter(connection_string="InstrumentationKey=<your-ikey-here>"), sampler=ProbabilitySampler(1.0))

with tracer.span(name='parent'):
    response = requests.get(url='https://www.wikipedia.org/wiki/Rabbit') # <-- this request will be tracked
```

## <a name="dependencies-with-httplib-integration"></a>"httplib" tümleştirmesi ile bağımlılıklar

OpenCensus `httplib` tümleştirmesi ile giden isteklerinizi izleyin.

`opencensus-ext-httplib` [PyPI'den](https://pypi.org/project/opencensus-ext-httplib/) indirin ve yükleyin ve izleme tümleştirmelerine ekleyin. Python3 için [http.client](https://docs.python.org/3.7/library/http.client.html) veya Python2 için [http.client](https://docs.python.org/2/library/httplib.html) kullanılarak gönderilen istekler izlenir.

```python
import http.client as httplib
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace import config_integration
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracer

config_integration.trace_integrations(['httplib'])
conn = httplib.HTTPConnection("www.python.org")

tracer = Tracer(
    exporter=AzureExporter(),
    sampler=ProbabilitySampler(1.0)
)

conn.request("GET", "http://www.python.org", "", {})
response = conn.getresponse()
conn.close()
```

## <a name="dependencies-with-django-integration"></a>"django" entegrasyonu ile bağımlılıklar

OpenCensus `django` entegrasyonu ile giden Django isteklerinizi izleyin.

[PyPI'den](https://pypi.org/project/opencensus-ext-django/) indirin ve yükleyin `MIDDLEWARE` `settings.py` `opencensus-ext-django` ve Django dosyasındaki bölüme aşağıdaki satırı ekleyin.

```python
MIDDLEWARE = [
    ...
    'opencensus.ext.django.middleware.OpencensusMiddleware',
]
```

Ek yapılandırma sağlanabilir, tam bir başvuru için [özelleştirmeleri](https://github.com/census-instrumentation/opencensus-python#customization) okuyun.

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

## <a name="dependencies-with-mysql-integration"></a>"mysql" tümleştirmesi ile bağımlılıklar

OpenCensus `mysql` tümleştirmesi ile MYSQL bağımlılıklarınızı izleyin. Bu tümleştirme [mysql-bağlayıcı](https://pypi.org/project/mysql-connector-python/) kitaplığını destekler.

`opencensus-ext-mysql` [PyPI'den](https://pypi.org/project/opencensus-ext-mysql/) indirin ve yükleyin ve kodunuza aşağıdaki satırları ekleyin.

```python
from opencensus.trace import config_integration

config_integration.trace_integrations(['mysql'])
```

## <a name="dependencies-with-pymysql-integration"></a>"Pymysql" tümleştirmesi ile bağımlılıklar

OpenCensus `pymysql` tümleştirmesi ile PyMySQL bağımlılıklarınızı izleyin.

`opencensus-ext-pymysql` [PyPI'den](https://pypi.org/project/opencensus-ext-pymysql/) indirin ve yükleyin ve kodunuza aşağıdaki satırları ekleyin.

```python
from opencensus.trace import config_integration

config_integration.trace_integrations(['pymysql'])
```

## <a name="dependencies-with-postgresql-integration"></a>"postgresql" tümleştirmesi ile bağımlılıklar

OpenCensus `postgresql` tümleştirmesi ile PostgreSQL bağımlılıklarınızı izleyin. Bu tümleştirme [psycopg2](https://pypi.org/project/psycopg2/) kitaplığını destekler.

`opencensus-ext-postgresql` [PyPI'den](https://pypi.org/project/opencensus-ext-postgresql/) indirin ve yükleyin ve kodunuza aşağıdaki satırları ekleyin.

```python
from opencensus.trace import config_integration

config_integration.trace_integrations(['postgresql'])
```

## <a name="dependencies-with-pymongo-integration"></a>"Pymongo" entegrasyonu ile bağımlılıklar

OpenCensus `pymongo` tümleştirmesi ile MongoDB bağımlılıklarınızı izleyin. Bu tümleştirme [pymongo](https://pypi.org/project/pymongo/) kitaplığını destekler.

`opencensus-ext-pymongo` [PyPI'den](https://pypi.org/project/opencensus-ext-pymongo/) indirin ve yükleyin ve kodunuza aşağıdaki satırları ekleyin.

```python
from opencensus.trace import config_integration

config_integration.trace_integrations(['pymongo'])
```

### <a name="dependencies-with-sqlalchemy-integration"></a>"sqlalchemy" tümleştirmesi ile bağımlılıklar

OpenCensus `sqlalchemy` tümleştirmesini kullanarak SQLAlchemy'yi kullanarak bağımlılıklarınızı izleyin. Bu tümleştirme, temel veritabanından bağımsız olarak [sqlalchemy](https://pypi.org/project/SQLAlchemy/) paketinin kullanımını izler.

```python
from opencensus.trace import config_integration

config_integration.trace_integrations(['sqlalchemy'])
```

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulama Eşlemesi](../../azure-monitor/app/app-map.md)
* [Kullanılabilir -lik](../../azure-monitor/app/monitor-web-app-availability.md)
* [Arama](../../azure-monitor/app/diagnostic-search.md)
* [Günlük (Analytics) sorgusu](../../azure-monitor/log-query/log-query-overview.md)
* [İşlem tanılama](../../azure-monitor/app/transaction-diagnostics.md)
