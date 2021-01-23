---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 01/22/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7ddc0234accd9f434aad850d2404e2f3001c4bae
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98742699"
---
## <a name="examples"></a>Örnekler

### <a name="filter-on-system-properties"></a>Sistem özelliklerinde filtrele
Bir filtre içindeki bir sistem özelliğine başvurmak için şu biçimi kullanın: `sys.<system-property-name>` . 

```csharp
sys.Label LIKE '%bus%'`
sys.messageid = 'xxxx'
sys.correlationid like 'abc-%'
```

### <a name="filter-on-message-properties"></a>İleti özelliklerini filtrele
İleti özelliklerini bir filtrede kullanma örnekleri aşağıda verilmiştir. Yalnızca veya kullanarak ileti özelliklerine erişebilirsiniz `user.property-name` `property-name` .

```csharp
MessageProperty = 'A'
SuperHero like 'SuperMan%'
```

### <a name="filter-on-message-properties-with-special-characters"></a>İleti özelliklerini özel karakterlerle filtrele
İleti özelliğinin adı özel karakterler içeriyorsa, `"` özellik adını çevrelemek için Double tırnakları () kullanın. Örneğin, özellik adı ise, `"http://schemas.microsoft.com/xrm/2011/Claims/EntityLogicalName"` filtrede aşağıdaki sözdizimini kullanın. 

```csharp
"http://schemas.microsoft.com/xrm/2011/Claims/EntityLogicalName" = 'account'
```

### <a name="filter-on-message-properties-with-numeric-values"></a>İleti özelliklerini sayısal değerlerle filtrele
Aşağıdaki örneklerde, filtrelerdeki sayısal değerlerle özellikleri nasıl kullanabileceğiniz gösterilmektedir. 

```csharp
MessageProperty = 1
MessageProperty > 1
MessageProperty > 2.08
MessageProperty = 1 AND MessageProperty2 = 3
MessageProperty = 1 OR MessageProperty2 = 3
```

### <a name="parameter-based-filters"></a>Parametre tabanlı filtreler
Parametre tabanlı filtreleri kullanmanın birkaç örneği aşağıda verilmiştir. Bu örneklerde, `DataTimeMp` türünde bir ileti özelliği `DateTime` ve bir `@dtParam` nesne olarak filtreye geçirilen parametredir `DateTime` .

```csharp
DateTimeMp < @dtParam
DateTimeMp > @dtParam

(DateTimeMp2-DateTimeMp1) <= @timespan //@timespan is a parameter of type TimeSpan
DateTimeMp2-DateTimeMp1 <= @timespan
```

### <a name="using-in-and-not-in"></a>İçinde DEĞIL, içinde kullanma

```csharp
StoreId IN('Store1', 'Store2', 'Store3')"

sys.To IN ('Store5','Store6','Store7') OR StoreId = 'Store8'

sys.To NOT IN ('Store1','Store2','Store3','Store4','Store5','Store6','Store7','Store8') OR StoreId NOT IN ('Store1','Store2','Store3','Store4','Store5','Store6','Store7','Store8')
```

Bir C# örneği için bkz. [GitHub 'Da konu filtreleri örneği](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Azure.Messaging.ServiceBus/BasicSendReceiveTutorialwithFilters).


### <a name="set-rule-action-for-a-sql-filter"></a>SQL filtresi için kural eylemi ayarlama

```csharp
// instantiate the ManagementClient
this.mgmtClient = new ManagementClient(connectionString);

// create the SQL filter
var sqlFilter = new SqlFilter("source = @stringParam");

// assign value for the parameter
sqlFilter.Parameters.Add("@stringParam", "orders");

// instantiate the Rule = Filter + Action
var filterActionRule = new RuleDescription
{
    Name = "filterActionRule",
    Filter = sqlFilter,
    Action = new SqlRuleAction("SET source='routedOrders'")
};

// create the rule on Service Bus
await this.mgmtClient.CreateRuleAsync(topicName, subscriptionName, filterActionRule);
```

### <a name="correlation-filter-using-correlationid"></a>CorrelationId kullanan bağıntı filtresi

```csharp
new CorrelationFilter("Contoso");
```

Olarak ayarlandığı iletileri filtreler `CorrelationID` `Contoso` . 

### <a name="correlation-filter-using-system-and-user-properties"></a>Sistem ve kullanıcı özelliklerini kullanarak bağıntı filtresi

```csharp
var filter = new CorrelationFilter();
filter.Label = "Important";
filter.ReplyTo = "johndoe@contoso.com";
filter.Properties["color"] = "Red";
```

Eşittir: `sys.ReplyTo = 'johndoe@contoso.com' AND sys.Label = 'Important' AND color = 'Red'`

