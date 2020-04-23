---
title: Şablon işlevleri - tarih
description: Tarihlerle çalışmak için Azure Kaynak Yöneticisi şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: 364b41e9e92cb248a7bd2fac5a41eb535adbf440
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084796"
---
# <a name="date-functions-for-arm-templates"></a>ARM şablonları için tarih işlevleri

Kaynak Yöneticisi, Azure Kaynak Yöneticisi (ARM) şablonlarınızdaki tarihlerle çalışmak için aşağıdaki işlevleri sağlar:

* [tarihTimeAdd](#datetimeadd)
* [utcŞimdi](#utcnow)

## <a name="datetimeadd"></a>tarihTimeAdd

`dateTimeAdd(base, duration, [format])`

Temel değere bir zaman süresi ekler. ISO 8601 formatı bekleniyor.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| base | Evet | string | Eklemeiçin başlangıç datetime değeri. [ISO 8601 zaman damgası biçimini](https://en.wikipedia.org/wiki/ISO_8601)kullanın. |
| süre | Evet | string | Taban eklemek için zaman değeri. Negatif bir değer olabilir. [ISO 8601 süre biçimini](https://en.wikipedia.org/wiki/ISO_8601#Durations)kullanın. |
| biçim | Hayır | string | Tarih saati sonucunun çıktı biçimi. Sağlanmadıysa, temel değerin biçimi kullanılır. Standart [biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings)kullanın. |

### <a name="return-value"></a>Döndürülen değer

Süre değerinin temel değere eklenmesinden kaynaklanan tarih saati değeri.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek şablon, zaman değerleri eklemenin farklı yollarını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "baseTime":{
            "type":"string",
            "defaultValue": "[utcNow('u')]"
        }
    },
    "variables": {
        "add3Years": "[dateTimeAdd(parameters('baseTime'), 'P3Y')]",
        "subtract9Days": "[dateTimeAdd(parameters('baseTime'), '-P9D')]",
        "add1Hour": "[dateTimeAdd(parameters('baseTime'), 'PT1H')]"
    },
    "resources": [],
    "outputs": {
        "add3Years": {
            "value": "[variables('add3Years')]",
            "type": "string"
        },
        "subtract9Days": {
            "value": "[variables('subtract9Days')]",
            "type": "string"
        },
        "add1Hour": {
            "value": "[variables('add1Hour')]",
            "type": "string"
        },
    }
}
```

Önceki şablon bir temel zaman ile `2020-04-07 14:53:14Z`dağıtıldığında, çıktı:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| add3Years | Dize | 07.07.2023 14:53:14 |
| çıkarma9Gün | Dize | 29.03.2020 14:53:14 |
| add1Hour | Dize | 07.07.2020 15:53:14 |

Sonraki örnek şablon, Otomasyon zamanlamasının başlangıç saatinin nasıl ayarlanır olduğunu gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "omsAutomationAccountName": {
            "type": "string",
            "defaultValue": "demoAutomation",
            "metadata": {
                "description": "Use an existing Automation account."
            }
        },
        "scheduleName": {
            "type": "string",
            "defaultValue": "demoSchedule1",
            "metadata": {
                "description": "Name of the new schedule."
            }
        },
        "baseTime":{
            "type":"string",
            "defaultValue": "[utcNow('u')]",
            "metadata": {
                "description": "Schedule will start one hour from this time."
            }
        }
    },
    "variables": {
        "startTime": "[dateTimeAdd(parameters('baseTime'), 'PT1H')]"
    },
    "resources": [
        {
            "name": "[concat(parameters('omsAutomationAccountName'), '/', parameters('scheduleName'))]",
            "type": "microsoft.automation/automationAccounts/schedules",
            "apiVersion": "2015-10-31",
            "location": "eastus2",
            "tags": {
            },
            "properties": {
                "description": "Demo Scheduler",
                "startTime": "[variables('startTime')]",
                "isEnabled": "true",
                "interval": 1,
                "frequency": "hour"
            }
        }
    ],
    "outputs": {
    }
}
```

## <a name="utcnow"></a>utcŞimdi

`utcNow(format)`

Geçerli (UTC) tarih saati değerini belirtilen biçimde verir. Format sağlanmadıysa, ISO 8601 (yyyyMddTHHmmssZ) biçimi kullanılır. **Bu işlev yalnızca bir parametre için varsayılan değerde kullanılabilir.**

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| biçim |Hayır |string |URI kodlanmış değeri bir dize dönüştürmek için. Standart [biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings)kullanın. |

### <a name="remarks"></a>Açıklamalar

Bu işlevi yalnızca bir parametrenin varsayılan değeri için bir ifade içinde kullanabilirsiniz. Bu işlevi şablonda başka bir yerde kullanmak bir hata döndürür. Her çağrıldığında farklı bir değer döndürdüğünden, işlev şablonun diğer bölümlerinde izin verilmez. Aynı şablonu aynı parametrelerle dağıtmak güvenilir bir şekilde aynı sonuçları üretmez.

Daha önceki [başarılı bir dağıtımı yeniden dağıtmak için bu seçeneği](rollback-on-error.md)kullanırsanız ve önceki dağıtım utcNow kullanan bir parametre içeriyorsa, parametre yeniden değerlendirilmez. Bunun yerine, önceki dağıtımdaki parametre değeri geri alma dağıtımında otomatik olarak yeniden kullanılır.

Varsayılan bir değer için utcNow işlevine dayanan bir şablonu yeniden dağıtılırken dikkatli olun. Yeniden dağıttığınızda ve parametre için bir değer sağlamadığınızda, işlev yeniden değerlendirilir. Yeni bir kaynak oluşturmak yerine varolan bir kaynağı güncelleştirmek istiyorsanız, önceki dağıtımdan parametre değerini geçirin.

### <a name="return-value"></a>Döndürülen değer

Geçerli UTC tarih saati değeri.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek şablon, tarih saati değeri için farklı biçimleri gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcValue": {
            "type": "string",
            "defaultValue": "[utcNow()]"
        },
        "utcShortValue": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "utcCustomValue": {
            "type": "string",
            "defaultValue": "[utcNow('M d')]"
        }
    },
    "resources": [
    ],
    "outputs": {
        "utcOutput": {
            "type": "string",
            "value": "[parameters('utcValue')]"
        },
        "utcShortOutput": {
            "type": "string",
            "value": "[parameters('utcShortValue')]"
        },
        "utcCustomOutput": {
            "type": "string",
            "value": "[parameters('utcCustomValue')]"
        }
    }
}
```

Önceki örnekteki çıktı her dağıtım için değişir, ancak benzer olacaktır:

| Adı | Tür | Değer |
| ---- | ---- | ----- |
| utcOutput | string | 20190305T175318Z |
| utcShortOutput | string | 03/05/2019 |
| utcCustomOutput | string | 3 5 |

Sonraki örnek, etiket değeri ayarlarken işlevden bir değerin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcShort": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "rgName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "name": "[parameters('rgName')]",
            "location": "westeurope",
            "tags":{
                "createdDate": "[parameters('utcShort')]"
            },
            "properties":{}
        }
    ],
    "outputs": {
        "utcShort": {
            "type": "string",
            "value": "[parameters('utcShort')]"
        }
    }
}
```