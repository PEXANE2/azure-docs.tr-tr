---
title: Python'u kullanarak Azure Veri Gölü Analizini yönetme
description: Bu makalede, Veri Gölü Analizi hesaplarını, veri kaynaklarını, kullanıcıları, & işleri yönetmek için Python'un nasıl kullanılacağı açıklanmaktadır.
services: data-lake-analytics
ms.service: data-lake-analytics
author: matt1883
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: d4213a19-4d0f-49c9-871c-9cd6ed7cf731
ms.topic: conceptual
ms.date: 06/08/2018
ms.openlocfilehash: d40658e1510c9ae8a2e3e1f865df7ac95f61abfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68355986"
---
# <a name="manage-azure-data-lake-analytics-using-python"></a>Python'u kullanarak Azure Veri Gölü Analizini yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede, Python kullanarak Azure Veri Gölü Analizi hesaplarının, veri kaynaklarının, kullanıcıların ve işlerin nasıl yönetilenbir şekilde yönetilen.

## <a name="supported-python-versions"></a>Desteklenen Python sürümleri

* Python'un 64 bit sürümünü kullanın.
* **[Python.org indirmede](https://www.python.org/downloads/)** bulunan standart Python dağıtımını kullanabilirsiniz. 
* Birçok geliştirici **[Anaconda Python dağıtımını](https://www.anaconda.com/download/)** kullanmayı uygun bulur.  
* Bu makale, standart Python dağıtımından Python sürüm 3.6 kullanılarak yazılmıştır

## <a name="install-azure-python-sdk"></a>Azure Python SDK’yı yükleme

Aşağıdaki modülleri yükleyin:

* **Azure-mgmt-kaynak** modülü, Active Directory vb. için diğer Azure modüllerini içerir.
* **Azure veri gölü deposu** modülü, Azure Veri Gölü Deposu dosya sistemi işlemlerini içerir. 
* **Azure-mgmt-datalake-store** modülü, Azure Veri Gölü Deposu hesap yönetimi işlemlerini içerir.
* **Azure-mgmt-datalake-analytics** modülü, Azure Veri Gölü Analizi işlemlerini içerir. 

İlk olarak, aşağıdaki `pip` komutu çalıştırarak en son sahip emin olun:

```
python -m pip install --upgrade pip
```

Bu belge kullanılarak `pip version 9.0.1`yazılmıştır.

Komut satırından modülleri yüklemek için aşağıdaki `pip` komutları kullanın:

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a>Yeni bir Python komut dosyası oluşturma

Aşağıdaki kodu komut dosyasına yapıştırın:

```python
# Use this only for Azure AD service-to-service authentication
#from azure.common.credentials import ServicePrincipalCredentials

# Use this only for Azure AD end-user authentication
#from azure.common.credentials import UserPassCredentials

# Required for Azure Resource Manager
from azure.mgmt.resource.resources import ResourceManagementClient
from azure.mgmt.resource.resources.models import ResourceGroup

# Required for Azure Data Lake Store account management
from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
from azure.mgmt.datalake.store.models import DataLakeStoreAccount

# Required for Azure Data Lake Store filesystem management
from azure.datalake.store import core, lib, multithread

# Required for Azure Data Lake Analytics account management
from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInformation

# Required for Azure Data Lake Analytics job management
from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

# Required for Azure Data Lake Analytics catalog management
from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

# Use these as needed for your application
import logging
import getpass
import pprint
import uuid
import time
```

Modüllerin içe aktarılabilen olduğunu doğrulamak için bu komut dosyasını çalıştırın.

## <a name="authentication"></a>Kimlik doğrulaması

### <a name="interactive-user-authentication-with-a-pop-up"></a>Açılır pencere ile etkileşimli kullanıcı kimlik doğrulaması

Bu yöntem desteklenmez.

### <a name="interactive-user-authentication-with-a-device-code"></a>Aygıt koduyla etkileşimli kullanıcı kimlik doğrulaması

```python
user = input(
    'Enter the user to authenticate with that has permission to subscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a>SPI ve gizli ile etkileşimli olmayan kimlik doğrulaması

```python
credentials = ServicePrincipalCredentials(
    client_id='FILL-IN-HERE', secret='FILL-IN-HERE', tenant='FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a>API ve sertifika ile etkileşimli olmayan kimlik doğrulama

Bu yöntem desteklenmez.

## <a name="common-script-variables"></a>Ortak komut dosyası değişkenleri

Bu değişkenler örneklerde kullanılır.

```python
subid = '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>'  # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-the-clients"></a>İstemcileri oluşturma

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient(
    credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a>Azure Kaynak Grubu oluşturma

```python
armGroupResult = resourceClient.resource_groups.create_or_update(
    rg, ResourceGroup(location=location))
```

## <a name="create-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Önce bir mağaza hesabı oluşturun.

```python
adlsAcctResult = adlsAcctClient.account.create(
    rg,
    adls,
    DataLakeStoreAccount(
        location=location)
    )
).wait()
```
Ardından, bu mağazayı kullanan bir ADLA hesabı oluşturun.

```python
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInformation(name=adls)]
    )
).wait()
```

## <a name="submit-a-job"></a>İş gönderme

```python
script = """
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"""

jobId = str(uuid.uuid4())
jobResult = adlaJobClient.job.create(
    adla,
    jobId,
    JobInformation(
        name='Sample Job',
        type='USql',
        properties=USqlJobProperties(script=script)
    )
)
```

## <a name="wait-for-a-job-to-end"></a>Bir işin bitmesini bekleyin

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' +
          jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a>Ardışık hatları ve yinelemeleri listele
İşlerinizin ardışık veya yinelenen meta verisi olup olmadığına bağlı olarak, ardışık lıkları ve yinelemeleri listeleyebilirsiniz.

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a>Bilgi işlem ilkelerini yönetme

DataLakeAnalyticsAccountManagementClient nesnesi, Bir Data Lake Analytics hesabının bilgi işlem ilkelerini yönetmek için yöntemler sağlar.

### <a name="list-compute-policies"></a>Bilgi işlem ilkelerini listele

Aşağıdaki kod, Bir Data Lake Analytics hesabının bilgi işlem ilkelerinin listesini alır.

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' +
          p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a>Yeni bir işlem ilkesi oluşturma

Aşağıdaki kod, bir Data Lake Analytics hesabı için yeni bir bilgi işlem ilkesi oluşturarak, belirtilen kullanıcıiçin kullanılabilir maksimum A'yi 50'ye ve minimum iş önceliğini 250'ye ayarlar.

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(
    userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(
    rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a>Sonraki adımlar

- Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
- U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
- Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).

