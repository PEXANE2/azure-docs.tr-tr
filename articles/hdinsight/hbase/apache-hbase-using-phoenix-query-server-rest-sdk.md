---
title: Phoenix Query Server REST SDK - Azure HDInsight
description: Azure HDInsight'taki Phoenix Query Server için REST SDK'yı yükleyin ve kullanın.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/01/2020
ms.openlocfilehash: 84c2bad1004029fe61dcfc19321957a170284587
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75612266"
---
# <a name="apache-phoenix-query-server-rest-sdk"></a>Apache Phoenix Sorgu Sunucusu REST SDK

[Apache Phoenix](https://phoenix.apache.org/) [apache HBase](apache-hbase-overview.md)üstüne bir açık kaynak, kitlesel paralel ilişkisel veritabanı katmanıdır. Phoenix, [SQLLine](apache-hbase-query-with-phoenix.md)gibi SSH araçları aracılığıyla HBase ile SQL benzeri sorgular kullanmanıza olanak sağlar. Phoenix ayrıca, istemci iletişimi için iki aktarım mekanizmasını destekleyen ince bir istemci olan Phoenix Query Server (PQS) adlı bir HTTP sunucusu da sağlar: JSON ve Protokol Arabellekleri. Protokol Arabellekleri varsayılan mekanizmadır ve JSON'dan daha verimli iletişim sunar.

Bu makalede, tablolar oluşturmak, satırları tek tek ve toplu olarak yukarı eklemek ve SQL deyimlerini kullanarak verileri seçmek için PQS REST SDK'nın nasıl kullanılacağı açıklanmaktadır. Örnekler, [Apache Phoenix Query Server için Microsoft .NET sürücüsünü](https://www.nuget.org/packages/Microsoft.Phoenix.Client)kullanır. Bu SDK, serileştirme biçimi için yalnızca Protokol Arabelleklerini kullanan [Apache Calcite'in Avatica](https://calcite.apache.org/avatica/) API'leri üzerine kurulmuştur.

Daha fazla bilgi için [Apache Calcite Avatica Protokol Tamponlar Başvuru](https://calcite.apache.org/avatica/docs/protobuf_reference.html)bakın.

## <a name="install-the-sdk"></a>SDK yükle

Apache Phoenix Query Server için Microsoft .NET sürücüsü, Visual Studio **NuGet Package Manager Console'dan** aşağıdaki komutla yüklenebilen bir NuGet paketi olarak sağlanır:

    Install-Package Microsoft.Phoenix.Client

## <a name="instantiate-new-phoenixclient-object"></a>Yeni PhoenixClient nesnesi anında

Kitaplığı kullanmaya başlamak için, kümenize `PhoenixClient` ve kümenin Apache Hadoop kullanıcı adı ve parolasını `ClusterCredentials` içeren `Uri` yeni bir nesneyi anında başlatın.

```csharp
var credentials = new ClusterCredentials(new Uri("https://CLUSTERNAME.azurehdinsight.net/"), "USERNAME", "PASSWORD");
client = new PhoenixClient(credentials);
```

CLUSTERNAME'yi HDInsight HBase küme adınız ve KULLANıCı Adı ve PAROLA ile küme oluşturmada belirtilen Hadoop kimlik bilgileriyle değiştirin. Varsayılan Hadoop kullanıcı adı **yöneticidir.**

## <a name="generate-unique-connection-identifier"></a>Benzersiz bağlantı tanımlayıcısı oluşturma

PQS'ye bir veya daha fazla istek göndermek için, isteği(ler) bağlantıyla ilişkilendirmek için benzersiz bir bağlantı tanımlayıcısı eklemeniz gerekir.

```csharp
string connId = Guid.NewGuid().ToString();
```

Her örnek, benzersiz bağlantı `OpenConnectionRequestAsync` tanımlayıcısını geçerek önce yönteme bir çağrı yapar. Ardından, `ConnectionProperties` tanımlayın ve, `RequestOptions`bu nesneleri ve oluşturulan bağlantı tanımlayıcısını yönteme geçirin. `ConnectionSyncRequestAsync` PQS'nin `ConnectionSyncRequest` nesnesi, hem istemcinin hem de sunucunun veritabanı özelliklerinin tutarlı bir görünümüne sahip olmasını sağlamaya yardımcı olur.

## <a name="connectionsyncrequest-and-its-connectionproperties"></a>ConnectionSyncRequest ve Bağlantı Özellikleri

Aramak `ConnectionSyncRequestAsync`için, bir `ConnectionProperties` nesne geçmek.

```csharp
ConnectionProperties connProperties = new ConnectionProperties
{
    HasAutoCommit = true,
    AutoCommit = true,
    HasReadOnly = true,
    ReadOnly = false,
    TransactionIsolation = 0,
    Catalog = "",
    Schema = "",
    IsDirty = true
};
await client.ConnectionSyncRequestAsync(connId, connProperties, options);
```

İlgi çekici bazı özellikler şunlardır:

| Özellik | Açıklama |
| -- | -- |
| Autocommit | Phoenix işlemleri için `autoCommit` etkinleştirilip etkinleştirilemediğini belirten bir boolean. |
| ReadOnly | Bağlantının salt okunur olup olmadığını belirten bir boolean. |
| İşlem Yalıtımı | JDBC belirtimi başına işlem yalıtımı düzeyini gösteren bir sonda - aşağıdaki tabloya bakın.|
| Katalog | Bağlantı özelliklerini alırken kullanılacak kataloğun adı. |
| Şema | Bağlantı özelliklerini alırken kullanılacak şema adı. |
| ısdirty | Özelliklerin değiştirilip değiştirilmediğini gösteren bir boolean. |

İşte değerler `TransactionIsolation` şunlardır:

| İzolasyon değeri | Açıklama |
| -- | -- |
| 0 | Hareketler desteklenmez. |
| 1 | Kirli okumalar, tekraredilemez okumalar ve hayalet okumalar oluşabilir. |
| 2 | Kirli okumalar engellenir, ancak tekrarlanamayan okumalar ve hayalet okumalar oluşabilir. |
| 4 | Kirli okumalar ve tekrarlanamayan okumalar engellenir, ancak hayalet okumalar oluşabilir. |
| 8 | Kirli okumalar, tekrarlanamayan okumalar ve hayalet okumalar engellenir. |

## <a name="create-a-new-table"></a>Yeni bir tablo oluşturma

HBase, diğer RDBMS gibi, tablolarda veri depolar. Phoenix, birincil anahtar ve sütun türlerini tanımlarken yeni tablolar oluşturmak için standart SQL sorguları kullanır.

Bu örnek ve daha sonraki tüm örneklerde, instantiated `PhoenixClient` nesneolarak [Instantiate yeni bir PhoenixClient nesne sin](#instantiate-new-phoenixclient-object)tanımlanan kullanın.

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();

// You can set certain request options, such as timeout in milliseconds:
options.TimeoutMillis = 300000;

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
CreateStatementResponse createStatementResponse = null;
OpenConnectionResponse openConnResponse = null;

try
{
    // Opening connection
    var info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);

    // Create the statement
    createStatementResponse = client.CreateStatementRequestAsync(connId, options).Result;
    
    // Create the table if it does not exist
    string sql = "CREATE TABLE IF NOT EXISTS Customers (Id varchar(20) PRIMARY KEY, FirstName varchar(50), " +
        "LastName varchar(100), StateProvince char(2), Email varchar(255), Phone varchar(15))";
    await client.PrepareAndExecuteRequestAsync(connId, sql, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);

    Console.WriteLine($"Table \"Customers\" created.");
}
catch (Exception e)
{
    Console.WriteLine(e);
    throw;
}
finally
{
    if (createStatementResponse != null)
    {
        client.CloseStatementRequestAsync(connId, createStatementResponse.StatementId, options).Wait();
        createStatementResponse = null;
    }

    if (openConnResponse != null)
    {
        client.CloseConnectionRequestAsync(connId, options).Wait();
        openConnResponse = null;
    }
}
```

Önceki örnek seçeneği kullanarak `Customers` `IF NOT EXISTS` adlı yeni bir tablo oluşturur. Arama, `CreateStatementRequestAsync` Avitica (PQS) sunucusunda yeni bir deyim oluşturur. Blok `finally` döndürülen `CreateStatementResponse` ve `OpenConnectionResponse` nesneleri kapatır.

## <a name="insert-data-individually"></a>Verileri tek tek ekleme

Bu örnek, Amerikan devlet ve bölge `List<string>` kısaltmaları koleksiyonuna başvuran tek bir veri eklemeyi gösterir:

```csharp
var states = new List<string> { "AL", "AK", "AS", "AZ", "AR", "CA", "CO", "CT", "DE", "DC", "FM", "FL", "GA", "GU", "HI", "ID", "IL", "IN", "IA", "KS", "KY", "LA", "ME", "MH", "MD", "MA", "MI", "MN", "MS", "MO", "MT", "NE", "NV", "NH", "NJ", "NM", "NY", "NC", "ND", "MP", "OH", "OK", "OR", "PW", "PA", "PR", "RI", "SC", "SD", "TN", "TX", "UT", "VT", "VI", "VA", "WA", "WV", "WI", "WY" };
```

Tablonun `StateProvince` sütun değeri daha sonraki bir seçme işleminde kullanılır.

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();
options.TimeoutMillis = 300000;

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
OpenConnectionResponse openConnResponse = null;
StatementHandle statementHandle = null;
try
{
    // Opening connection
    pbc::MapField<string, string> info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);

    string sql = "UPSERT INTO Customers VALUES (?,?,?,?,?,?)";
    PrepareResponse prepareResponse = await client.PrepareRequestAsync(connId, sql, 100, options);
    statementHandle = prepareResponse.Statement;
    
    var r = new Random();

    // Insert 300 rows
    for (int i = 0; i < 300; i++)
    {
        var list = new pbc.RepeatedField<TypedValue>();
        var id = new TypedValue
        {
            StringValue = "id" + i,
            Type = Rep.String
        };
        var firstName = new TypedValue
        {
            StringValue = "first" + i,
            Type = Rep.String
        };
        var lastName = new TypedValue
        {
            StringValue = "last" + i,
            Type = Rep.String
        };
        var state = new TypedValue
        {
            StringValue = states.ElementAt(r.Next(0, 49)),
            Type = Rep.String
        };
        var email = new TypedValue
        {
            StringValue = $"email{1}@junkemail.com",
            Type = Rep.String
        };
        var phone = new TypedValue
        {
            StringValue = $"555-229-341{i.ToString().Substring(0,1)}",
            Type = Rep.String
        };
        list.Add(id);
        list.Add(firstName);
        list.Add(lastName);
        list.Add(state);
        list.Add(email);
        list.Add(phone);

        Console.WriteLine("Inserting customer " + i);

        await client.ExecuteRequestAsync(statementHandle, list, long.MaxValue, true, options);
    }

    await client.CommitRequestAsync(connId, options);

    Console.WriteLine("Upserted customer data");

}
catch (Exception ex)
{

}
finally
{
    if (statementHandle != null)
    {
        await client.CloseStatementRequestAsync(connId, statementHandle.Id, options);
        statementHandle = null;
    }
    if (openConnResponse != null)
    {
        await client.CloseConnectionRequestAsync(connId, options);
        openConnResponse = null;
    }
}
```

Ek sekle deyimini yürütme yapısı yeni bir tablo oluşturmaya benzer. `try` Bloğun sonunda, hareket açıkça işlenir. Bu örnek, bir ekleme işlemini 300 kez yineler. Aşağıdaki örnekte daha verimli bir toplu iş ekleme işlemi gösterilmektedir.

## <a name="batch-insert-data"></a>Toplu ekleme verileri

Aşağıdaki kod, verileri tek tek eklemek için kodla neredeyse aynıdır. Bu örnek, `UpdateBatch` hazır bir `ExecuteBatchRequestAsync`ifadeyle tekrar tekrar `ExecuteRequestAsync` aramak yerine, bir çağrıda nesneyi kullanır.

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();
options.TimeoutMillis = 300000;

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
OpenConnectionResponse openConnResponse = null;
CreateStatementResponse createStatementResponse = null;
try
{
    // Opening connection
    pbc::MapField<string, string> info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);

    // Creating statement
    createStatementResponse = await client.CreateStatementRequestAsync(connId, options);

    string sql = "UPSERT INTO Customers VALUES (?,?,?,?,?,?)";
    PrepareResponse prepareResponse = client.PrepareRequestAsync(connId, sql, long.MaxValue, options).Result;
    var statementHandle = prepareResponse.Statement;
    var updates = new pbc.RepeatedField<UpdateBatch>();

    var r = new Random();

    // Insert 300 rows
    for (int i = 300; i < 600; i++)
    {
        var list = new pbc.RepeatedField<TypedValue>();
        var id = new TypedValue
        {
            StringValue = "id" + i,
            Type = Rep.String
        };
        var firstName = new TypedValue
        {
            StringValue = "first" + i,
            Type = Rep.String
        };
        var lastName = new TypedValue
        {
            StringValue = "last" + i,
            Type = Rep.String
        };
        var state = new TypedValue
        {
            StringValue = states.ElementAt(r.Next(0, 49)),
            Type = Rep.String
        };
        var email = new TypedValue
        {
            StringValue = $"email{1}@junkemail.com",
            Type = Rep.String
        };
        var phone = new TypedValue
        {
            StringValue = $"555-229-341{i.ToString().Substring(0, 1)}",
            Type = Rep.String
        };
        list.Add(id);
        list.Add(firstName);
        list.Add(lastName);
        list.Add(state);
        list.Add(email);
        list.Add(phone);

        var batch = new UpdateBatch
        {
            ParameterValues = list
        };
        updates.Add(batch);

        Console.WriteLine($"Added customer {i} to batch");
    }

    var executeBatchResponse = await client.ExecuteBatchRequestAsync(connId, statementHandle.Id, updates, options);

    Console.WriteLine("Batch upserted customer data");

}
catch (Exception ex)
{

}
finally
{
    if (openConnResponse != null)
    {
        await client.CloseConnectionRequestAsync(connId, options);
        openConnResponse = null;
    }
}
```

Bir test ortamında, tek tek 300 yeni kayıt eklemek neredeyse 2 dakika sürdü. Buna karşılık, bir toplu iş olarak 300 kayıtları ekleyerek sadece 6 saniye gereklidir.

## <a name="select-data"></a>Verileri seçme

Bu örnek, birden çok sorgu yürütmek için bir bağlantının nasıl yeniden kullanılacağını gösterir:

1. Tüm kayıtları seçin ve varsayılan en fazla 100'ün döndürüldükten sonra kalan kayıtları getirin.
2. Tek skaler sonucu almak için toplam satır sayısı seçme deyimini kullanın.
3. Eyalet veya bölge başına toplam müşteri sayısını döndüren bir seçim deyimi yürütün.

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
OpenConnectionResponse openConnResponse = null;
StatementHandle statementHandle = null;

try
{
    // Opening connection
    pbc::MapField<string, string> info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);
    var createStatementResponse = await client.CreateStatementRequestAsync(connId, options);

    string sql = "SELECT * FROM Customers";
    ExecuteResponse executeResponse = await client.PrepareAndExecuteRequestAsync(connId, sql, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);

    pbc::RepeatedField<Row> rows = executeResponse.Results[0].FirstFrame.Rows;
    // Loop through all of the returned rows and display the first two columns
    for (int i = 0; i < rows.Count; i++)
    {
        Row row = rows[i];
        Console.WriteLine(row.Value[0].ScalarValue.StringValue + " " + row.Value[1].ScalarValue.StringValue);
    }
    
    // 100 is hard-coded on the server side as the default firstframe size
    // FetchRequestAsync is called to get any remaining rows
    Console.WriteLine("");
    Console.WriteLine($"Number of rows: {rows.Count}");

    // Fetch remaining rows, offset is not used, simply set to 0
    // When FetchResponse.Frame.Done is true, all rows were fetched
    FetchResponse fetchResponse = await client.FetchRequestAsync(connId, createStatementResponse.StatementId, 0, int.MaxValue, options);
    Console.WriteLine($"Frame row count: {fetchResponse.Frame.Rows.Count}");
    Console.WriteLine($"Fetch response is done: {fetchResponse.Frame.Done}");
    Console.WriteLine("");

    // Running query 2
    string sql2 = "select count(*) from Customers";
    ExecuteResponse countResponse = await client.PrepareAndExecuteRequestAsync(connId, sql2, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);
    long count = countResponse.Results[0].FirstFrame.Rows[0].Value[0].ScalarValue.NumberValue;

    Console.WriteLine($"Total customer records: {count}");
    Console.WriteLine("");

    // Running query 3
    string sql3 = "select StateProvince, count(*) as Number from Customers group by StateProvince order by Number desc";
    ExecuteResponse groupByResponse = await client.PrepareAndExecuteRequestAsync(connId, sql3, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);

    pbc::RepeatedField<Row> stateRows = groupByResponse.Results[0].FirstFrame.Rows;
    for (int i = 0; i < stateRows.Count; i++)
    {
        Row row = stateRows[i];
        Console.WriteLine(row.Value[0].ScalarValue.StringValue + ": " + row.Value[1].ScalarValue.NumberValue);
    }
}
catch (Exception ex)
{

}
finally
{
    if (statementHandle != null)
    {
        await client.CloseStatementRequestAsync(connId, statementHandle.Id, options);
        statementHandle = null;
    }
    if (openConnResponse != null)
    {
        await client.CloseConnectionRequestAsync(connId, options);
        openConnResponse = null;
    }
}
```

İfadelerin çıktısı `select` aşağıdaki sonuç olmalıdır:

```
id0 first0
id1 first1
id10 first10
id100 first100
id101 first101
id102 first102
. . .
id185 first185
id186 first186
id187 first187
id188 first188

Number of rows: 100
Frame row count: 500
Fetch response is done: True

Total customer records: 600

NJ: 21
CA: 19
GU: 17
NC: 16
IN: 16
MA: 16
AZ: 16
ME: 16
IL: 15
OR: 15
. . . 
MO: 10
HI: 10
GA: 10
DC: 9
NM: 9
MD: 9
MP: 9
SC: 7
AR: 7
MH: 6
FM: 5
```

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Phoenix](../hdinsight-phoenix-in-hdinsight.md)
* [Apache HBase REST SDK'yı kullanma](apache-hbase-rest-sdk.md)
