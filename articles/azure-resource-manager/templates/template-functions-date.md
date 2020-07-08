---
title: Şablon işlevleri-Tarih
description: Tarihlerle çalışmak için bir Azure Resource Manager şablonunda kullanılacak işlevleri açıklar.
ms.topic: conceptual
ms.date: 06/22/2020
ms.openlocfilehash: abdc88ce15279b90f8f9dc05a38a2ae236498f12
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86058053"
---
# <a name="date-functions-for-arm-templates"></a>ARM şablonları için tarih işlevleri

Kaynak Yöneticisi, Azure Resource Manager (ARM) şablonlarınızın tarihleri ile çalışmak için aşağıdaki işlevleri sağlar:

* [dateTimeAdd](#datetimeadd)
* [utcNow](#utcnow)

## <a name="datetimeadd"></a>dateTimeAdd

`dateTimeAdd(base, duration, [format])`

Temel değere bir zaman süresi ekler. ISO 8601 biçimi bekleniyor.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| base | Evet | string | Ekleme için başlangıç tarih saat değeri. [Iso 8601 zaman damgası biçimini](https://en.wikipedia.org/wiki/ISO_8601)kullanın. |
| süre | Evet | string | Temele eklenecek zaman değeri. Negatif bir değer olabilir. [Iso 8601 Duration biçimini](https://en.wikipedia.org/wiki/ISO_8601#Durations)kullanın. |
| biçim | Hayır | string | Tarih saat sonucu için çıkış biçimi. Sağlanmazsa, temel değerin biçimi kullanılır. [Standart biçim dizelerini](/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim dizelerini](/dotnet/standard/base-types/custom-date-and-time-format-strings)kullanın. |

### <a name="return-value"></a>Döndürülen değer

Temel değere Duration değeri eklemekten kaynaklanan tarih saat değeri.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek şablonda saat değerleri eklemenin farklı yolları gösterilmektedir.

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

Yukarıdaki şablon bir taban saati ile dağıtıldığında `2020-04-07 14:53:14Z` , çıkış şu şekilde olur:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| add3Years | Dize | 4/7/2023 2:53:14 PM |
| subtract9Days | Dize | 3/29/2020 2:53:14 PM |
| add1Hour | Dize | 4/7/2020 3:53:14 PM |

Sonraki örnek şablonda, bir Otomasyon zamanlaması için başlangıç zamanının nasıl ayarlanacağı gösterilmektedir.

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

## <a name="utcnow"></a>utcNow

`utcNow(format)`

Belirtilen biçimdeki geçerli (UTC) Tarih/saat değerini döndürür. Biçim sağlanmazsa ISO 8601 (yyyyMMddTHHmmssZ) biçimi kullanılır. **Bu işlev, yalnızca bir parametre için varsayılan değerde kullanılabilir.**

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| biçim |Hayır |string |Dizeye dönüştürülecek URI kodlamalı değer. [Standart biçim dizelerini](/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim dizelerini](/dotnet/standard/base-types/custom-date-and-time-format-strings)kullanın. |

### <a name="remarks"></a>Açıklamalar

Bu işlevi yalnızca bir parametresinin varsayılan değeri için bir ifade içinde kullanabilirsiniz. Bu işlevi bir şablonda başka bir yerde kullanmak bir hata döndürür. İşlevin her çağrılışında farklı bir değer döndürdüğünden, bu işleve şablon diğer bölümlerinde izin verilmiyor. Aynı parametrelerle aynı şablon dağıtmak, aynı sonuçları güvenilir bir şekilde üretmez.

Daha önceki başarılı bir dağıtıma [hata üzerinde geri alma seçeneğini](rollback-on-error.md) kullanırsanız ve önceki dağıtım artık utckullanan bir parametre içeriyorsa, parametre yeniden değerlendirilmez. Bunun yerine, önceki dağıtımdaki parametre değeri geri alma dağıtımında otomatik olarak yeniden kullanılır.

Varsayılan bir değer için utcNow işlevine dayanan bir şablonu yeniden dağıtmaya dikkat edin. Yeniden dağıtırken ve parametresi için bir değer sağlamazsanız, işlev yeniden değerlendirilerek yapılır. Yeni bir kaynak oluşturmak yerine var olan bir kaynağı güncelleştirmek istiyorsanız, önceki dağıtımdan parametre değerini geçirin.

### <a name="return-value"></a>Döndürülen değer

Geçerli UTC Tarih saat değeri.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek şablonda tarih saat değeri için farklı biçimler gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
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

Yukarıdaki örnekteki çıktı her dağıtım için farklılık gösterir, ancak şuna benzer olacaktır:

| Name | Tür | Değer |
| ---- | ---- | ----- |
| utcOutput | string | 20190305T175318Z |
| Utckısaltoutput | string | 03/05/2019 |
| utcCustomOutput | string | 3 5 |

Sonraki örnekte, bir etiket değeri ayarlanırken işlevden bir değerin nasıl kullanılacağı gösterilmektedir.

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

## <a name="next-steps"></a>Sonraki adımlar

* Azure Resource Manager şablonundaki bölümlerin açıklaması için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
