---
title: Azure Veri Gezgini kullanarak Azure Izleyici 'den aktarılmış verileri sorgulama (Önizleme)
description: Log Analytics çalışma alanınızdan bir Azure depolama hesabına aktarılmış verileri sorgulamak için Azure Veri Gezgini kullanın.
ms.subservice: logs
author: orens
ms.author: bwren
ms.reviewer: bwren
ms.topic: conceptual
ms.date: 10/13/2020
ms.openlocfilehash: b3ab711f6d324c6d49eda0dccd88a3f2ac939eb5
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92461592"
---
# <a name="query-exported-data-from-azure-monitor-using-azure-data-explorer-preview"></a>Azure Veri Gezgini kullanarak Azure Izleyici 'den aktarılmış verileri sorgulama (Önizleme)
Azure Izleyici 'den bir Azure depolama hesabına veri aktarma, düşük maliyetli bekletme ve günlükleri farklı bölgelere yeniden tahsis etme olanağı sağlar. Log Analytics çalışma alanlarınızdan aktarılmış verileri sorgulamak için Azure Veri Gezgini kullanın. Yapılandırıldıktan sonra, çalışma alanlarınızdan bir Azure depolama hesabına gönderilen desteklenen tablolar Azure Veri Gezgini için bir veri kaynağı olarak kullanılabilir.

İşlem akışı aşağıdaki gibidir: 

1.  Log Analytics çalışma alanındaki verileri Azure depolama hesabına aktarın.
2.  Azure Veri Gezgini kümenizde dış tablo oluşturun ve veri türleri için eşleme yapın.
3.  Azure Veri Gezgini 'de verileri sorgulama.

:::image type="content" source="media/azure-data-explorer-query-storage/exported-data-query.png" alt-text="Azure Veri Gezgini veri sorgulama akışını verdi.":::



## <a name="send-data-to-azure-storage"></a>Azure Storage 'a veri gönderme
Azure Izleyici günlükleri, aşağıdaki seçeneklerden herhangi biri kullanılarak bir Azure depolama hesabına aktarılabilir.

- Log Analytics çalışma alanınızdan tüm verileri Azure depolama hesabına veya Olay Hub 'ına aktarmak için Azure Izleyici günlüklerinin Log Analytics çalışma alanı verileri dışarı aktarma özelliğini kullanın. Bkz. [Log Analytics çalışma alanı verilerini dışarı aktarma Azure izleyici (Önizleme)](logs-data-export.md)
- Mantıksal uygulama kullanılarak günlük sorgusundan zamanlanan dışarı aktarma. Bu, veri dışa aktarma özelliğine benzerdir, ancak filtrelenmiş veya toplanmış verileri Azure depolama 'ya göndermenizi sağlar. Bu yöntem, [günlük sorgusu sınırlarına](../service-limits.md#log-analytics-workspaces)  tabidir, [Logic App kullanarak Log Analytics çalışma alanındaki arşiv verilerini Azure depolama 'ya](logs-export-logic-app.md)görün.
- Bir mantıksal uygulama kullanarak bir kerelik dışarı aktarma. [Logic Apps ve güç otomatikleştirme için bkz. Azure Izleyici günlükleri Bağlayıcısı](logicapp-flow-connector.md).
- PowerShell betiği kullanılarak yerel makineye bir kerelik dışarı aktarma. Bkz. [Invoke-Azoperationalınsightsqueryexport](https://www.powershellgallery.com/packages/Invoke-AzOperationalInsightsQueryExport).

> [!TIP]
> Mevcut bir Azure Veri Gezgini kümesini kullanabilir veya gerekli yapılandırmalara sahip yeni bir adanmış küme oluşturabilirsiniz.

## <a name="create-an-external-table-located-in-azure-blob-storage"></a>Azure Blob depolamada bulunan dış tablo oluşturma
Azure Veri Gezgini Azure depolama hesabına bağlamak için [dış tabloları](/azure/data-explorer/kusto/query/schema-entities/externaltables) kullanın. Dış tablo, kusto veritabanı dışında depolanan verilere başvuran bir kusto şeması varlıktır. Tablolar gibi, dış tablo ise iyi tanımlanmış bir şemaya sahiptir. Tabloların aksine, veriler bir kusto kümesi dışında saklanır ve yönetilir. Önceki bölümden alınan veriler JSON satırlarına kaydedilir.

Bir başvuru oluşturmak için, dışarıya aktarılmış tablonun şemasını gerekir. Tablonun sütunlarını ve veri türlerini içeren bu bilgileri almak için Log Analytics [GetSchema](/azure/data-explorer/kusto/query/getschemaoperator) işlecini kullanın.

:::image type="content" source="media\azure-data-explorer-query-storage\exported-data-map-schema.jpg" alt-text="Azure Veri Gezgini veri sorgulama akışını verdi.":::

Artık çıktıyı, dış tablo oluşturmak için kusto sorgusu oluşturmak üzere kullanabilirsiniz.
[Azure depolama veya Azure Data Lake dış tablolar oluşturma ve değiştirme](/azure/data-explorer/kusto/management/external-tables-azurestorage-azuredatalake)bölümündeki yönergelerin ardından, JSON biçiminde bir dış tablo oluşturun ve ardından sorguyu Azure Veri Gezgini veritabanından çalıştırın.

>[!NOTE]
>Dış tablo oluşturma iki işlemlerden oluşturulmuştur. Birincisi dış tabloyu oluşturuyor, ikincisi ise eşlemeyi oluşturuyor.

Aşağıdaki PowerShell betiği tablo ve eşleme için [oluşturma](/azure/data-explorer/kusto/management/external-tables-azurestorage-azuredatalake#create-external-table-mapping) komutlarını oluşturacaktır.

```powershell
PARAM(
    $resourcegroupname, #The name of the Azure resource group
    $TableName, # The log lanlyics table you wish to convert to external table
    $MapName, # The name of the map
    $subscriptionId, #The ID of the subscription
    $WorkspaceId, # The log lanlyics WorkspaceId
    $WorkspaceName, # The log lanlyics workspace name
    $BlobURL, # The Blob URL where to save
    $ContainerAccessKey, # The blob container Access Key (Option to add a SAS url)
    $ExternalTableName = $null # The External Table name, null to use the same name
)

if($null -eq $ExternalTableName) {
    $ExternalTableName = $TableName
}

$query = $TableName + ' | getschema | project ColumnName, DataType'

$output = (Invoke-AzOperationalInsightsQuery -WorkspaceId $WorkspaceId -Query $query).Results

$FirstCommand = @()
$SecondCommand = @()

foreach ($record in $output) {
    if ($record.DataType -eq 'System.DateTime') {
        $dataType = 'datetime'
    } else {
        $dataType = 'string'
    }
    $FirstCommand += $record.ColumnName + ":" + "$dataType" + ","
    $SecondCommand += "{`"column`":" + "`"" + $record.ColumnName + "`"," + "`"datatype`":`"$dataType`",`"path`":`"$." + $record.ColumnName + "`"},"
}
$schema = ($FirstCommand -join '') -replace ',$'
$mapping = ($SecondCommand -join '') -replace ',$'

$CreateExternal = @'
.create external table {0} ({1})
kind=blob
partition by (TimeGeneratedPartition:datetime = bin(TimeGenerated, 1min))
pathformat = (datetime_pattern("'y='yyyy'/m='MM'/d='dd'/h='HH'/m='mm", TimeGeneratedPartition))
dataformat=multijson
(
   h@'{2}/WorkspaceResourceId=/subscriptions/{4}/resourcegroups/{6}/providers/microsoft.operationalinsights/workspaces/{5};{3}'
)
with
(
   docstring = "Docs",
   folder = "ExternalTables"
)
'@ -f $TableName, $schema, $BlobURL, $ContainerAccessKey, $subscriptionId, $WorkspaceName, $resourcegroupname,$WorkspaceId

$createMapping = @'
.create external table {0} json mapping "{1}" '[{2}]'
'@ -f $ExternalTableName, $MapName, $mapping

Write-Host -ForegroundColor Red 'Copy and run the following commands (one by one), on your Azure Data Explorer cluster query window to create the external table and mappings:'
write-host -ForegroundColor Green $CreateExternal
Write-Host -ForegroundColor Green $createMapping
```

Aşağıdaki görüntüde çıktının bir örneği gösterilir.

:::image type="content" source="media/azure-data-explorer-query-storage/external-table-create-command-output.png" alt-text="Azure Veri Gezgini veri sorgulama akışını verdi.":::

[![Örnek çıkış](media/azure-data-explorer-query-storage/external-table-create-command-output.png)](media/azure-data-explorer-query-storage/external-table-create-command-output.png#lightbox)

>[!TIP]
>Tablo ve eşleme oluşturmak için Azure Veri Gezgini istemci aracındaki betiğin çıktısını kopyalayın, yapıştırın ve çalıştırın.

## <a name="query-the-exported-data-from-azure-data-explorer"></a>Azure Veri Gezgini aktarılmış verileri sorgulama 

Eşlemeyi yapılandırdıktan sonra, Azure Veri Gezgini aktarılmış verileri sorgulayabilirsiniz. Sorgunuz aşağıdaki örnekte olduğu gibi [external_table](/azure/data-explorer/kusto/query/externaltablefunction) işleve gerek duyar.

```kusto
external_table("HBTest","map") | take 10000
```

[![Log Analytics aktarılmış verileri sorgula](media/azure-data-explorer-query-storage/external-table-query.png)](media/azure-data-explorer-query-storage/external-table-query.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Gezgini sorguları yazmayı](https://docs.microsoft.com/azure/data-explorer/write-queries) öğrenin