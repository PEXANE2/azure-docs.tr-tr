---
title: Azure Veri Gezgini .NET Standard SDK ile veri alma (Önizleme)
description: Bu makalede .NET Standard SDK kullanarak Azure Veri Gezgini 'a veri alma (yükleme) hakkında bilgi edineceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 9b6eda60f0b0cb1b697560cccc2cffe719d58536
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79251784"
---
# <a name="ingest-data-using-the-azure-data-explorer-net-standard-sdk-preview"></a>Azure Veri Gezgini .NET Standard SDK 'sını kullanarak verileri alma (Önizleme)

Azure Veri Gezgini (ADX), günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir bir veri araştırma hizmetidir. ADX, .NET Standard için iki istemci kitaplığı sağlar: alma [kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Ingest.NETStandard) ve [veri kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Data.NETStandard). Bu kitaplıklar verileri bir kümeye almanıza (yüklemenize ve kodunuzdan verileri sorgulamanıza olanak tanır. Bu makalede, ilk olarak bir test kümesinde tablo ve veri eşlemesi oluşturacaksınız. Daha sonra kümeye bir alımı sıraya alın ve sonuçları doğrulayın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

* [Test kümesi ve veritabanı](create-cluster-database-portal.md)

## <a name="install-the-ingest-library"></a>Alma kitaplığını yüklemesi

```
Install-Package Microsoft.Azure.Kusto.Ingest.NETStandard
```

## <a name="authentication"></a>Kimlik Doğrulaması

Azure Veri Gezgini uygulamanın kimliğini doğrulamak için AAD kiracı kimliğinizi kullanır. Kiracı kimliğinizi bulmak için aşağıdaki URL'yi kullanın ve *YourDomain* yerine kendi etki alanınızı yazın.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Örneğin, etki alanınız *contoso.com* olduğunda URL şöyle olur: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Sonuçları görmek için bu URL'ye tıklayın; ilk satır aşağıdaki gibidir. 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

Bu örnekte kiracı kimliği `6babcaad-604b-40ac-a9d7-9fd97c0b779f` değeridir.

Bu örnek, kümeye erişmek için kimlik doğrulaması için bir AAD kullanıcısı ve parolası kullanır. AAD uygulama sertifikası ve AAD uygulama anahtarını da kullanabilirsiniz. Bu kodu çalıştırmadan önce `tenantId`, `user`ve `password` değerlerini ayarlayın.

```csharp
var tenantId = "<TenantId>";
var user = "<User>";
var password = "<Password>";
```

## <a name="construct-the-connection-string"></a>Bağlantı dizesini oluşturun
Şimdi bağlantı dizesini hazırlayın. Sonraki bir adımda hedef tabloyu ve eşlemeyi oluşturursunuz.

```csharp
var kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443/";
var database = "<DatabaseName>";

var kustoConnectionStringBuilder =
    new KustoConnectionStringBuilder(kustoUri)
    {
        FederatedSecurity = true,
        InitialCatalog = database,
        UserID = user,
        Password = password,
        Authority = tenantId
    };
```

## <a name="set-source-file-information"></a>Kaynak dosya bilgilerini ayarlama

Kaynak dosyanın yolunu ayarlayın. Bu örnekte, Azure Blob Depolama'da barındırılan bir örnek dosya kullanılır. **StormEvents** örnek veri kümesi, [Ulusal Çevre Bilgileri Merkezleri](https://www.ncdc.noaa.gov/stormevents/)'nden gelen hava durumu verilerini içerir.

```csharp
var blobPath = "https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
```

## <a name="create-a-table-on-your-test-cluster"></a>Test kümenizde tablo oluşturma
`StormEvents.csv` dosyasındaki verilerin şemasıyla eşleşen `StormEvents` adlı bir tablo oluşturun.

```csharp
var table = "StormEvents";
using (var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder))
{
    var command =
        CslCommandGenerator.GenerateTableCreateCommand(
            table,
            new[]
            {
                Tuple.Create("StartTime", "System.DateTime"),
                Tuple.Create("EndTime", "System.DateTime"),
                Tuple.Create("EpisodeId", "System.Int32"),
                Tuple.Create("EventId", "System.Int32"),
                Tuple.Create("State", "System.String"),
                Tuple.Create("EventType", "System.String"),
                Tuple.Create("InjuriesDirect", "System.Int32"),
                Tuple.Create("InjuriesIndirect", "System.Int32"),
                Tuple.Create("DeathsDirect", "System.Int32"),
                Tuple.Create("DeathsIndirect", "System.Int32"),
                Tuple.Create("DamageProperty", "System.Int32"),
                Tuple.Create("DamageCrops", "System.Int32"),
                Tuple.Create("Source", "System.String"),
                Tuple.Create("BeginLocation", "System.String"),
                Tuple.Create("EndLocation", "System.String"),
                Tuple.Create("BeginLat", "System.Double"),
                Tuple.Create("BeginLon", "System.Double"),
                Tuple.Create("EndLat", "System.Double"),
                Tuple.Create("EndLon", "System.Double"),
                Tuple.Create("EpisodeNarrative", "System.String"),
                Tuple.Create("EventNarrative", "System.String"),
                Tuple.Create("StormSummary", "System.Object"),
            });

    kustoClient.ExecuteControlCommand(command);
}
```

## <a name="define-ingestion-mapping"></a>Veri alımı eşlemesini tanımlama

Gelen CSV verilerini tablo oluştururken kullanılan sütun adlarıyla eşleyin.
Bu tabloda bir [CSV sütun eşleme nesnesi](/azure/kusto/management/create-ingestion-mapping-command) sağlama

```csharp
var tableMapping = "StormEvents_CSV_Mapping";
using (var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder))
{
    var command =
        CslCommandGenerator.GenerateTableCsvMappingCreateCommand(
            table,
            tableMapping,
            new[]
            {
                new CsvColumnMapping { ColumnName = "StartTime", Ordinal = 0 },
                new CsvColumnMapping { ColumnName = "EndTime", Ordinal = 1 },
                new CsvColumnMapping { ColumnName = "EpisodeId", Ordinal = 2 },
                new CsvColumnMapping { ColumnName = "EventId", Ordinal = 3 },
                new CsvColumnMapping { ColumnName = "State", Ordinal = 4 },
                new CsvColumnMapping { ColumnName = "EventType", Ordinal = 5 },
                new CsvColumnMapping { ColumnName = "InjuriesDirect", Ordinal = 6 },
                new CsvColumnMapping { ColumnName = "InjuriesIndirect", Ordinal = 7 },
                new CsvColumnMapping { ColumnName = "DeathsDirect", Ordinal = 8 },
                new CsvColumnMapping { ColumnName = "DeathsIndirect", Ordinal = 9 },
                new CsvColumnMapping { ColumnName = "DamageProperty", Ordinal = 10 },
                new CsvColumnMapping { ColumnName = "DamageCrops", Ordinal = 11 },
                new CsvColumnMapping { ColumnName = "Source", Ordinal = 12 },
                new CsvColumnMapping { ColumnName = "BeginLocation", Ordinal = 13 },
                new CsvColumnMapping { ColumnName = "EndLocation", Ordinal = 14 },
                new CsvColumnMapping { ColumnName = "BeginLat", Ordinal = 15 },
                new CsvColumnMapping { ColumnName = "BeginLon", Ordinal = 16 },
                new CsvColumnMapping { ColumnName = "EndLat", Ordinal = 17 },
                new CsvColumnMapping { ColumnName = "EndLon", Ordinal = 18 },
                new CsvColumnMapping { ColumnName = "EpisodeNarrative", Ordinal = 19 },
                new CsvColumnMapping { ColumnName = "EventNarrative", Ordinal = 20 },
                new CsvColumnMapping { ColumnName = "StormSummary", Ordinal = 21 },
            });

    kustoClient.ExecuteControlCommand(command);
}
```

## <a name="queue-a-message-for-ingestion"></a>Veri alımı için bir iletiyi kuyruğa alma

Blob depolamadan veri çekmek ve bu verilerin ADX 'e alınması için bir ileti kuyruğa alın.

```csharp
var ingestUri = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443/";
var ingestConnectionStringBuilder =
    new KustoConnectionStringBuilder(ingestUri)
    {
        FederatedSecurity = true,
        InitialCatalog = database,
        UserID = user,
        Password = password,
        Authority = tenantId
    };

using (var ingestClient = KustoIngestFactory.CreateQueuedIngestClient(ingestConnectionStringBuilder))
{
    var properties =
        new KustoQueuedIngestionProperties(database, table)
        {
            Format = DataSourceFormat.csv,
            CSVMappingReference = tableMapping,
            IgnoreFirstRecord = true
        };

    ingestClient.IngestFromSingleBlob(blobPath, deleteSourceOnSuccess: false, ingestionProperties: properties);
}
```

## <a name="validate-data-was-ingested-into-the-table"></a>Verileri doğrulama, tabloya alındı

Sıraya alınan alma işleminin alımı zamanlamasını ve verileri ADX ' e yüklemesi için beş ila on dakika bekleyin. Ardından aşağıdaki kodu çalıştırarak `StormEvents` tablosundaki kayıtların sayısını alın.

```csharp
using (var cslQueryProvider = KustoClientFactory.CreateCslQueryProvider(kustoConnectionStringBuilder))
{
    var query = $"{table} | count";

    var results = cslQueryProvider.ExecuteQuery<long>(query);
    Console.WriteLine(results.Single());
}
```

## <a name="run-troubleshooting-queries"></a>Sorun giderme sorguları çalıştırma

[https://dataexplorer.azure.com](https://dataexplorer.azure.com) adresinde oturum açın ve kümenize bağlanın. Son dört saatte hiç veri alımı hatası olup olmadığını görmek için veritabanınızda aşağıdaki komutu çalıştırın. Çalıştırmadan önce veritabanı adını değiştirin.

```Kusto
.show ingestion failures
| where FailedOn > ago(4h) and Database == "<DatabaseName>"
```

Son dört saatteki tüm veri alım işlemlerinin durumunu görüntülemek için aşağıdaki komutu çalıştırın. Çalıştırmadan önce veritabanı adını değiştirin.

```Kusto
.show operations
| where StartedOn > ago(4h) and Database == "<DatabaseName>" and Operation == "DataIngestPull"
| summarize arg_max(LastUpdatedOn, *) by OperationId
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer makalelerimizi izlemeyi planlıyorsanız oluşturduğunuz kaynakları saklayın. Aksi takdirde, veritabanınızda aşağıdaki komutu çalıştırarak `StormEvents` tablosunu temizleyin.

```Kusto
.drop table StormEvents
```

## <a name="next-steps"></a>Sonraki adımlar

* [Sorgu yazma](write-queries.md)
