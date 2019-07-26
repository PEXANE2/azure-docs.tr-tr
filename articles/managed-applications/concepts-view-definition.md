---
title: Azure yönetilen uygulamalarında görünüm tanımına genel bakış | Microsoft Docs
description: Azure yönetilen uygulamalar için Görünüm tanımı oluşturma kavramını açıklar.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 06/12/2019
ms.openlocfilehash: ff96bddef1b34f5a8bf743ccaaccba2da01534dc
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335102"
---
# <a name="view-definition-artifact-in-azure-managed-applications"></a>Azure yönetilen uygulamalarında tanım yapıtı görüntüleme

Görünüm tanımı, Azure yönetilen uygulamalarında isteğe bağlı bir yapıdır. Genel Bakış sayfasını özelleştirmeye ve ölçümler ve özel kaynaklar gibi daha fazla görünüm eklemenize olanak tanır.

Bu makale, görünüm tanımı yapıtı ve özelliklerine genel bir bakış sağlar.

## <a name="view-definition-artifact"></a>Tanım yapıtını görüntüleme

Görünüm tanımı yapıtı, **ViewDefinition. JSON** olarak adlandırılmalıdır ve yönetilen uygulama tanımı oluşturan. zip paketinde **createuıdefinition. JSON** ve **maintemplate. JSON** ile aynı düzeye yerleştirilmelidir. . Zip paketini oluşturma ve yönetilen uygulama tanımını yayımlama hakkında bilgi edinmek için bkz. [Azure yönetilen uygulama tanımı yayımlama](publish-managed-app-definition-quickstart.md)

## <a name="view-definition-schema"></a>Tanım şemasını görüntüle

**ViewDefinition. JSON** dosyası bir görünüm dizisi olan yalnızca bir `views` top Level özelliğine sahiptir. Her görünüm, yönetilen uygulama kullanıcı arabiriminde, içindekiler tablosunda ayrı bir menü öğesi olarak gösterilir. Her görünüm, görünümün `kind` türünü ayarlayan bir özelliğe sahiptir. Bu değerin aşağıdaki değerlerden birine ayarlanması gerekir: [Genel bakış](#overview), [ölçümler](#metrics), [customresources](#custom-resources). Daha fazla bilgi için bkz. [viewDefinition. JSON için geçerli JSON şeması](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#).

Görünüm tanımı için örnek JSON:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#",
    "contentVersion": "0.0.0.1",
    "views": [
        {
            "kind": "Overview",
            "properties": {
                "header": "Welcome to your Azure Managed Application",
                "description": "This managed application is for demo purposes only.",
                "commands": [
                    {
                        "displayName": "Test Action",
                        "path": "testAction"
                    }
                ]
            }
        },
        {
            "kind": "Metrics",
            "properties": {
                "displayName": "This is my metrics view",
                "version": "1.0.0",
                "charts": [
                    {
                        "displayName": "Sample chart",
                        "chartType": "Bar",
                        "metrics": [
                            {
                                "name": "Availability",
                                "aggregationType": "avg",
                                "resourceTagFilter": [ "tag1" ],
                                "resourceType": "Microsoft.Storage/storageAccounts",
                                "namespace": "Microsoft.Storage/storageAccounts"
                            }
                        ]
                    }
                ]
            }
        },
        {
            "kind": "CustomResources",
            "properties": {
                "displayName": "Test custom resource type",
                "version": "1.0.0",
                "resourceType": "testCustomResource",
                "createUIDefinition": { },
                "commands": [
                    {
                        "displayName": "Custom Test Action",
                        "path": "testAction"
                    },
                    {
                        "displayName": "Custom Context Action",
                        "path": "testCustomResource/testContextAction",
                        "icon": "Stop",
                        "createUIDefinition": { }
                    }
                ],
                "columns": [
                    {"key": "name", "displayName": "Name"},
                    {"key": "properties.myProperty1", "displayName": "Property 1"},
                    {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
                ]
            }
        }
    ]
}

```

## <a name="overview"></a>Genel Bakış

`"kind": "Overview"`

Bu görünümü **ViewDefinition. JSON**içinde sağladığınızda, yönetilen uygulamanızdaki varsayılan genel bakış sayfasını geçersiz kılar.

```json
{
    "kind": "Overview",
    "properties": {
        "header": "Welcome to your Azure Managed Application",
        "description": "This managed application is for demo purposes only.",
        "commands": [
            {
                "displayName": "Test Action",
                "path": "testAction"
            }
        ]
    }
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|üst bilgi|Hayır|Genel Bakış sayfasının üst bilgisi.|
|description|Hayır|Yönetilen uygulamanızın açıklaması.|
|komutlar|Hayır|Genel Bakış sayfasının ek araç çubuğu düğmelerinin dizisi, bkz. [Komutlar](#commands).|

![Genel Bakış](./media/view-definition/overview.png)

## <a name="metrics"></a>Ölçümler

`"kind": "Metrics"`

Ölçüm görünümü, [Azure Izleyici ölçümlerinde](../azure-monitor/platform/data-platform-metrics.md)yönetilen uygulama kaynaklarınızdan veri toplamanıza ve bunları toplamanızı sağlar.

```json
{
    "kind": "Metrics",
    "properties": {
        "displayName": "This is my metrics view",
        "version": "1.0.0",
        "charts": [
            {
                "displayName": "Sample chart",
                "chartType": "Bar",
                "metrics": [
                    {
                        "name": "Availability",
                        "aggregationType": "avg",
                        "resourceTagFilter": [ "tag1" ],
                        "resourceType": "Microsoft.Storage/storageAccounts",
                        "namespace": "Microsoft.Storage/storageAccounts"
                    }
                ]
            }
        ]
    }
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Hayır|Görünümün görüntülenmiş başlığı.|
|version|Hayır|Görünümü işlemek için kullanılan platformun sürümü.|
|grafik|Evet|Ölçümler sayfasının grafik dizisi.|

### <a name="chart"></a>Grafik

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Evet|Grafiğin görüntülenmekte olan başlığı.|
|chartType|Hayır|Bu grafik için kullanılacak görselleştirme. Varsayılan olarak, bir çizgi grafik kullanır. Desteklenen grafik türleri: `Bar, Line, Area, Scatter`.|
|ölçümler|Evet|Bu grafiğe çizilecek ölçüm dizisi. Azure portal desteklenen ölçümler hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici Ile desteklenen ölçümler](../azure-monitor/platform/metrics-supported.md)|

### <a name="metric"></a>Ölçüm

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|name|Evet|Ölçümün adı.|
|Toplamatürü|Evet|Bu ölçüm için kullanılacak toplama türü. Desteklenen toplama türleri:`none, sum, min, max, avg, unique, percentile, count`|
|ad alanı|Hayır|Doğru ölçüm sağlayıcısı belirlenirken kullanılacak ek bilgiler.|
|resourceTagFilter|Hayır|Ölçümlerin gösterileceği kaynak etiketleri dizisi (Word ile `or` ayrılacaktır). Kaynak türü filtresinin üzerine uygulanır.|
|resourceType|Evet|Ölçümlerin gösterileceği kaynak türü.|

![Ölçümler](./media/view-definition/metrics.png)

## <a name="custom-resources"></a>Özel kaynaklar

`"kind": "CustomResources"`

Bu türde birden çok görünüm tanımlayabilirsiniz. Her görünüm, **Maintemplate. JSON**içinde tanımladığınız özel sağlayıcıdan **benzersiz** bir özel kaynak türünü temsil eder. Özel sağlayıcılara giriş için bkz. [Azure özel sağlayıcılar önizlemeye genel bakış](custom-providers-overview.md).

Bu görünümde, özel kaynak türü için al, koy, SIL ve POST işlemleri gerçekleştirebilirsiniz. POST işlemleri genel özel eylemler veya özel eylemler özel kaynak türü bağlamında olabilir.

```json
{
    "kind": "CustomResources",
    "properties": {
        "displayName": "Test custom resource type",
        "version": "1.0.0",
        "resourceType": "testCustomResource",
        "createUIDefinition": { },
        "commands": [
            {
                "displayName": "Custom Test Action",
                "path": "testAction"
            },
            {
                "displayName": "Custom Context Action",
                "path": "testCustomResource/testContextAction",
                "icon": "Stop",
                "createUIDefinition": { },
            }
        ],
        "columns": [
            {"key": "name", "displayName": "Name"},
            {"key": "properties.myProperty1", "displayName": "Property 1"},
            {"key": "properties.myProperty2", "displayName": "Property 2", "optional": true}
        ]
    }
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Evet|Görünümün görüntülenmiş başlığı. Başlık, **ViewDefinition. JSON**'ınızdaki her customresources görünümü için **benzersiz** olmalıdır.|
|version|Hayır|Görünümü işlemek için kullanılan platformun sürümü.|
|resourceType|Evet|Özel kaynak türü. Özel sağlayıcılarınızın **benzersiz** bir özel kaynak türü olması gerekir.|
|createUIDefinition|Hayır|Özel kaynak Oluştur komutu için UI tanımı şeması oluşturun. UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md)|
|komutlar|Hayır|CustomResources görünümündeki ek araç çubuğu düğmelerinin dizisi, bkz. [Komutlar](#commands).|
|Sütunları|Hayır|Özel kaynağın sütun dizisi. Tanımlanmamışsa, `name` sütun varsayılan olarak gösterilir. Sütun `"key"` ve`"displayName"`içermelidir. Anahtar için, bir görünümde görüntülenecek özelliğin anahtarını sağlayın. İç içe ise, `"key": "name"` veya `"key": "properties.property1"`gibi nokta sınırlayıcı olarak kullanın. Görünen ad için, bir görünümde görüntülenecek özelliğin görünen adını sağlayın. Bir `"optional"` özellik de sağlayabilirsiniz. True olarak ayarlandığında, sütun varsayılan olarak bir görünümde gizlenir.|

![CustomResources](./media/view-definition/customresources.png)

## <a name="commands"></a>Komutlar

Komutlar, sayfada görüntülenen ek araç çubuğu düğmelerinin bir dizisidir. Her komut, **Maintemplate. JSON**' da tanımlanan Azure özel SAĞLAYıCıNıZDAN bir post eylemini temsil eder. Özel sağlayıcılara giriş için bkz. [Azure özel sağlayıcılarına genel bakış](custom-providers-overview.md).

```json
{
    "commands": [
        {
            "displayName": "Start Test Action",
            "path": "testAction",
            "icon": "Start",
            "createUIDefinition": { }
        },
    ]
}
```

|Özellik|Gerekli|Açıklama|
|---------|---------|---------|
|displayName|Evet|Komut düğmesinin görünen adı.|
|path|Evet|Özel sağlayıcı eylem adı. Eylemin **Maintemplate. JSON**içinde tanımlanması gerekir.|
|Simgesi|Hayır|Komut düğmesinin simgesi. Desteklenen simgelerin listesi [JSON şemasında](https://schema.management.azure.com/schemas/viewdefinition/0.0.1-preview/ViewDefinition.json#)tanımlandı.|
|createUIDefinition|Hayır|Komut için UI tanımı şeması oluştur. UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).|

## <a name="looking-for-help"></a>Yardım aranıyor

Azure yönetilen uygulamalar hakkında sorularınız varsa [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-managedapps)yapmayı deneyin. Benzer bir soru zaten istendi ve yanıtlamış olabilir, bu nedenle göndermeden önce kontrol edin. Hızlı bir yanıt `azure-managedapps` almak için etiketi ekleyin!

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](overview.md) konusunu inceleyin.
- Özel sağlayıcılara giriş için bkz. [Azure özel sağlayıcılarına genel bakış](custom-providers-overview.md).
- Azure özel sağlayıcılarıyla Azure yönetilen uygulaması oluşturmak için bkz [. Öğretici: Özel sağlayıcı eylemleri ve kaynak türleriyle yönetilen uygulama oluşturma](tutorial-create-managed-app-with-custom-provider.md)
