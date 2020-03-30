---
title: HBase .NET SDK'yı kullanın - Azure HDInsight
description: Tablolar oluşturmak ve silmek ve verileri okumak ve yazmak için HBase .NET SDK'yı kullanın.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/02/2019
ms.openlocfilehash: eba7d7ad009b2ef0442a916983489489eb5cceb8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74806669"
---
# <a name="use-the-net-sdk-for-apache-hbase"></a>Apache HBase için .NET SDK'yı kullanın

[Apache HBase](apache-hbase-overview.md) verilerinizle çalışmak için iki birincil seçenek sağlar: [Apache Hive sorguları ve HBase'in RESTful API'sine çağrılar.](apache-hbase-tutorial-get-started-linux.md) `curl` Komutu veya benzer bir yardımcı programı kullanarak doğrudan REST API ile çalışabilirsiniz.

C# ve .NET uygulamaları [için .NET için Microsoft HBase REST İstemci Kitaplığı,](https://www.nuget.org/packages/Microsoft.HBase.Client/) HBase REST API'sinin üstünde bir istemci kitaplığı sağlar.

## <a name="install-the-sdk"></a>SDK yükle

HBase .NET SDK, Visual Studio **NuGet Paket Yöneticisi Konsolundan** aşağıdaki komutla yüklenebilen bir NuGet paketi olarak sağlanır:

    Install-Package Microsoft.HBase.Client

## <a name="instantiate-a-new-hbaseclient-object"></a>Yeni bir HBaseClient nesnesi anında

SDK'yı kullanmak için, kümenize `HBaseClient` ve Hadoop kullanıcı adı ve parolanızdan `ClusterCredentials` oluşan yeni bir `Uri` nesneyi anında anons edin.

```csharp
var credentials = new ClusterCredentials(new Uri("https://CLUSTERNAME.azurehdinsight.net"), "USERNAME", "PASSWORD");
client = new HBaseClient(credentials);
```

CLUSTERNAME'yi HDInsight HBase küme adınız ve KULLANıCı Adı ve PAROLA ile küme oluşturmada belirtilen Apache Hadoop kimlik bilgileriyle değiştirin. Varsayılan Hadoop kullanıcı adı **yöneticidir.**

## <a name="create-a-new-table"></a>Yeni bir tablo oluşturma

HBase verileri tablolarda depolar. Tablo, satır *anahtarı,* birincil anahtar ve *sütun aileleri*olarak adlandırılan bir veya daha fazla sütun grubundan oluşur. Her tablodaki veriler, Rowkey aralığı tarafından *bölgelere*yatay olarak dağıtılır. Her bölgenin bir başlangıç ve bitiş anahtarı vardır. Bir tabloda bir veya daha fazla bölge olabilir. Tablodaki veriler büyüdükçe, HBase büyük bölgeleri daha küçük bölgelere böler. Bölgeler, bir bölge sunucusunun birden çok bölgeyi depolayabildiği *bölge sunucularında*depolanır.

Veriler fiziksel olarak *HFiles'da*depolanır. Tek bir HFile, bir tablo, bir bölge ve bir sütun ailesi için veri içerir. HFile'daki satırlar Rowkey'de sıralanır şekilde depolanır. Her HFile satırların hızlı alınması için bir *B + Ağaç* dizin vardır.

Yeni bir tablo oluşturmak `TableSchema` için a ve sütunları belirtin. Aşağıdaki kod, 'RestSDKTable' tablosunun zaten var olup olmadığını denetler - değilse, tablo oluşturulur.

```csharp
if (!client.ListTablesAsync().Result.name.Contains("RestSDKTable"))
{
    // Create the table
    var newTableSchema = new TableSchema {name = "RestSDKTable" };
    newTableSchema.columns.Add(new ColumnSchema() {name = "t1"});
    newTableSchema.columns.Add(new ColumnSchema() {name = "t2"});

    await client.CreateTableAsync(newTableSchema);
}
```

Bu yeni tablo, t1 ve t2 olmak üzere iki sütunlu ailelere sahiptir. Sütun aileleri farklı HFiles'da ayrı olarak depolandığından, sık sorgulanan veriler için ayrı bir sütun ailesine sahip olmak mantıklıdır. Aşağıdaki [Veri Ekle](#insert-data) örneğinde, t1 sütun ailesine sütunlar eklenir.

## <a name="delete-a-table"></a>Bir tablo silme

Bir tabloyu silmek için:

```csharp
await client.DeleteTableAsync("RestSDKTable");
```

## <a name="insert-data"></a>Veri ekleme

Veri eklemek için, satır tanımlayıcısı olarak benzersiz bir satır anahtarı belirtirsiniz. Tüm veriler bir `byte[]` dizide depolanır. Aşağıdaki kod, t1 `title`sütun `director`ailesine , ve `release_date` sütunları tanımlar ve ekler, çünkü bu sütunlar en sık erişilenlerdir. Ve `description` `tagline` sütunlar t2 sütun ailesine eklenir. Verilerinizi gerektiğinde sütun ailelerine bölümlere aktarabilirsiniz.

```csharp
var key = "fifth_element";
var row = new CellSet.Row { key = Encoding.UTF8.GetBytes(key) };
var value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:title"),
    data = Encoding.UTF8.GetBytes("The Fifth Element")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:director"),
    data = Encoding.UTF8.GetBytes("Luc Besson")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:release_date"),
    data = Encoding.UTF8.GetBytes("1997")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t2:description"),
    data = Encoding.UTF8.GetBytes("In the colorful future, a cab driver unwittingly becomes the central figure in the search for a legendary cosmic weapon to keep Evil and Mr Zorg at bay.")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t2:tagline"),
    data = Encoding.UTF8.GetBytes("The Fifth is life")
};
row.values.Add(value);

var set = new CellSet();
set.rows.Add(row);

await client.StoreCellsAsync("RestSDKTable", set);
```

HBase [Cloud BigTable](https://cloud.google.com/bigtable/)uygular, böylece veri biçimi aşağıdaki görüntü gibi görünür:

![Apache HBase örnek veri çıktısı](./media/apache-hbase-rest-sdk/hdinsight-table-roles.png)

## <a name="select-data"></a>Verileri seçme

HBase tablosundaki verileri okumak için, tablo adını ve `GetCellsAsync` satır anahtarını döndürecek yönteme `CellSet`geçirin.

```csharp
var key = "fifth_element";

var cells = await client.GetCellsAsync("RestSDKTable", key);
// Get the first value from the row.
Console.WriteLine(Encoding.UTF8.GetString(cells.rows[0].values[0].data));
// Get the value for t1:title
Console.WriteLine(Encoding.UTF8.GetString(cells.rows[0].values
    .Find(c => Encoding.UTF8.GetString(c.column) == "t1:title").data));
// With the previous insert, it should yield: "The Fifth Element"
```

Bu durumda, benzersiz bir anahtar için yalnızca bir satır olması gerektiğinden, kod yalnızca ilk eşleşen satırı döndürür. Döndürülen değer `string` `byte[]` diziden biçime dönüştürülür. Ayrıca, değeri filmin çıkış tarihi için bir tamsayı gibi diğer türlere de dönüştürebilirsiniz:

```csharp
var releaseDateField = cells.rows[0].values
    .Find(c => Encoding.UTF8.GetString(c.column) == "t1:release_date");
int releaseDate = 0;

if (releaseDateField != null)
{
    releaseDate = Convert.ToInt32(Encoding.UTF8.GetString(releaseDateField.data));
}
Console.WriteLine(releaseDate);
// Should return 1997
```

## <a name="scan-over-rows"></a>Satırlar üzerinde tay

HBase `scan` bir veya daha fazla satır almak için kullanır. Bu örnek, 10'lu gruplar halinde birden çok satır ister ve anahtar değerleri 25 ile 35 arasında olan verileri alır. Tüm satırları aldıktan sonra, kaynakları temizlemek için tarayıcıyı silin.

```csharp
var tableName = "mytablename";

// Assume the table has integer keys and we want data between keys 25 and 35
var scanSettings = new Scanner()
{
    batch = 10,
    startRow = BitConverter.GetBytes(25),
    endRow = BitConverter.GetBytes(35)
};
RequestOptions scanOptions = RequestOptions.GetDefaultOptions();
scanOptions.AlternativeEndpoint = "hbaserest0/";
ScannerInformation scannerInfo = null;
try
{
    scannerInfo = await client.CreateScannerAsync(tableName, scanSettings, scanOptions);
    CellSet next = null;
    while ((next = client.ScannerGetNextAsync(scannerInfo, scanOptions).Result) != null)
    {
        foreach (var row in next.rows)
        {
            // ... read the rows
        }
    }
}
finally
{
    if (scannerInfo != null)
    {
        await client.DeleteScannerAsync(tableName, scannerInfo, scanOptions);
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta Apache HBase örneğiyle çalışmaya başlama](apache-hbase-tutorial-get-started-linux.md)
* [Apache HBase ile gerçek zamanlı Twitter duyarlılığını analiz edin ile](../hdinsight-hbase-analyze-twitter-sentiment.md) uça doğru bir uygulama oluşturun
