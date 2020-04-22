---
title: Azure Veri Gölü Depolama sorgu hızlandırma (önizleme) kullanarak verileri filtreleme | Microsoft Dokümanlar
description: Depolama hesabınızdan bir veri alt kümesi almak için sorgu hızlandırma (önizleme) kullanın.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: normesta
ms.reviewer: jamsbak
ms.openlocfilehash: ae3dfc7681ef0d8ce3fcf679bddbd0ff195f4e3b
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81771852"
---
# <a name="filter-data-by-using-azure-data-lake-storage-query-acceleration-preview"></a>Azure Veri Gölü Depolama sorgu hızlandırma (önizleme) kullanarak verileri filtreleme

Bu makalede, depolama hesabınızdan bir veri alt kümesi almak için sorgu hızlandırma (önizleme) nasıl kullanılacağını gösterir. 

Sorgu hızlandırma (önizleme), uygulamaların ve analiz çerçevelerinin yalnızca belirli bir işlemi gerçekleştirmek için gereksinim duydukları verileri alarak veri işlemeyi önemli ölçüde optimize etmesini sağlayan Azure Veri Gölü Depolaması için yeni bir özelliktir. Daha fazla bilgi için Azure [Veri Gölü Depolama Sorgusu Hızlandırma (önizleme) adresine](data-lake-storage-query-acceleration.md)bakın.

> [!NOTE]
> Sorgu hızlandırma özelliği genel önizlemededir ve Kanada Orta ve Fransa Orta bölgelerinde kullanılabilir. Sınırlamaları gözden geçirmek için [Bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın. Önizlemeye kaydolmak için [bu forma](https://aka.ms/adls/qa-preview-signup)bakın.  

## <a name="prerequisites"></a>Ön koşullar

### <a name="net"></a>[.NET](#tab/dotnet)

- Azure Depolama'ya erişmek için bir Azure aboneliğine ihtiyacınız olacak. Zaten aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

- **Genel amaçlı v2** depolama hesabı. bkz. [Bir depolama hesabı oluşturun.](../common/storage-quickstart-create-account.md)

- [.NET SDK](https://dotnet.microsoft.com/download). 

### <a name="java"></a>[Java](#tab/java)

- Azure Depolama'ya erişmek için bir Azure aboneliğine ihtiyacınız olacak. Zaten aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

- **Genel amaçlı v2** depolama hesabı. bkz. [Bir depolama hesabı oluşturun.](../common/storage-quickstart-create-account.md)

- [Java Geliştirme Kiti (JDK)](/java/azure/jdk/?view=azure-java-stable) sürüm 8 veya üzeri.

- [Apache Maven](https://maven.apache.org/download.cgi). 

  > [!NOTE] 
  > Bu makalede, Apache Maven kullanarak bir Java projesi oluşturduğunuz varsayar. Apache Maven kullanarak proje oluşturma nın bir örneği için [bkz.](storage-quickstart-blobs-java.md#setting-up)
  
---

## <a name="install-packages"></a>Paketleri yükleme 

### <a name="net"></a>[.NET](#tab/dotnet)

1. Sorgu hızlandırma paketlerini indirin. Bu bağlantıyı kullanarak bu paketleri içeren sıkıştırılmış bir .zip dosyası elde edebilirsiniz: [https://aka.ms/adls/qqsdk/.net](https://aka.ms/adls/qqsdk/.net). 

2. Bu dosyanın içeriğini proje dizininize ayıklayın.

3. Proje dosyanızı *(.csproj)* bir metin düzenleyicisinde açın ve \<bu\> paket başvurularını Proje öğesinin içine ekleyin.

   ```xml
   <ItemGroup>
       <PackageReference Include="Azure.Storage.Blobs" Version="12.5.0-preview.1" />
       <PackageReference Include="Azure.Storage.Common" Version="12.4.0-preview.1" />
       <PackageReference Include="Azure.Storage.QuickQuery" Version="12.0.0-preview.1" />
   </ItemGroup>
   ```

4. Önizleme SDK paketlerini geri yükleyin. Bu örnek `dotnet restore` komut, komutu kullanarak önizleme SDK paketlerini geri yükler. 

   ```console
   dotnet restore --source C:\Users\contoso\myProject
   ```

5. Ortak NuGet deposundan diğer tüm bağımlılıkları geri yükleyin.

   ```console
   dotnet restore
   ```

### <a name="java"></a>[Java](#tab/java)

1. Projenizin kökünde dizin oluşturun. Kök dizini **pom.xml** dosyasını içeren dizindir.

   > [!NOTE]
   > Bu makaledeki örnekler, dizinin adının **lib**olduğunu varsayar.

2. Sorgu hızlandırma paketlerini indirin. Bu bağlantıyı kullanarak bu paketleri içeren sıkıştırılmış bir .zip dosyası elde edebilirsiniz: [https://aka.ms/adls/qqsdk/java](https://aka.ms/adls/qqsdk/java). 

3. Bu .zip dosyasındaki dosyaları oluşturduğunuz dizine ayıklayın. Örneğimizde, bu dizini **lib**adlandırılır. 

4. Metin düzenleyicinizdeki *pom.xml* dosyasını açın. Bağımlılıklar grubuna aşağıdaki bağımlılık öğelerini ekleyin. 

   ```xml
   <!-- Request static dependencies from Maven -->
   <dependency>
       <groupId>com.azure</groupId>
       <artifactId>azure-core</artifactId>
       <version>1.3.0</version>
   </dependency>
   <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-core-http-netty</artifactId>
      <version>1.3.0</version>
   </dependency>
   <dependency>
      <groupId>org.apache.avro</groupId>
      <artifactId>avro</artifactId>
      <version>1.9.2</version>
   </dependency>
   <dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-csv</artifactId>
    <version>1.8</version>
   </dependency>
   <!-- Local dependencies -->
   <dependency>
       <groupId>com.azure</groupId>
       <artifactId>azure-storage-blob</artifactId>
       <version>12.5.0-beta.1</version>
       <scope>system</scope>
       <systemPath>${project.basedir}/lib/azure-storage-blob-12.5.0-beta.1.jar</systemPath>
   </dependency>
   <dependency>
       <groupId>com.azure</groupId>
       <artifactId>azure-storage-common</artifactId>
       <version>12.5.0-beta.1</version>
       <scope>system</scope>
       <systemPath>${project.basedir}/lib/azure-storage-common-12.5.0-beta.1.jar</systemPath>
   </dependency>
   <dependency>
       <groupId>com.azure</groupId>
       <artifactId>azure-storage-quickquery</artifactId>
       <version>12.0.0-beta.1</version>
       <scope>system</scope>
       <systemPath>${project.basedir}/lib/azure-storage-quickquery-12.0.0-beta.1.jar</systemPath>
   </dependency>
   ```

---

## <a name="add-statements"></a>Ekstre ekleme


### <a name="net"></a>[.NET](#tab/dotnet)

Bu `using` deyimleri kod dosyanızın en üstüne ekleyin.

```csharp
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
using Azure.Storage.Blobs.Specialized;
using Azure.Storage.QuickQuery;
using Azure.Storage.QuickQuery.Models;
```

Sorgu hızlandırma CSV ve Json biçimlendirilmiş verileri alır. Bu nedenle, kullanmayı seçtiğiniz csv veya Json ayrıştma kitaplıkları için ifadeler kullandığınızdan emin olun. Bu makalede görünen örnekler, NuGet'de bulunan [CsvHelper](https://www.nuget.org/packages/CsvHelper/) kitaplığını kullanarak bir CSV dosyasını ayrıştırır. Bu nedenle, bu `using` ifadeleri kod dosyasının en üstüne ekleriz.

```csharp
using CsvHelper;
using CsvHelper.Configuration;
```

Bu makalede sunulan örnekleri derlemek için bu `using` ifadeleri de eklemeniz gerekir.

```csharp
using System.Threading.Tasks;
using System.IO;
using System.Globalization;
using System.Threading;
using System.Linq;
```

### <a name="java"></a>[Java](#tab/java)

Bu `import` deyimleri kod dosyanızın en üstüne ekleyin.

```java
import com.azure.storage.blob.*;
import com.azure.storage.blob.models.*;
import com.azure.storage.common.*;
import com.azure.storage.quickquery.*;
import com.azure.storage.quickquery.models.*;
import java.io.*;
import org.apache.commons.csv.*;
```

---

## <a name="retrieve-data-by-using-a-filter"></a>Filtre kullanarak veri alma

Sorgu hızlandırma isteğinde satır filtresi yüklemlerini ve sütun projeksiyonlarını belirtmek için SQL'i kullanabilirsiniz. Aşağıdaki kod depolama alanında bir CSV dosyasını sorgular ve üçüncü sütunun `Hemingway, Ernest`değeriyle eşleştiği tüm veri satırlarını döndürür. 

- SQL sorgusunda, anahtar `BlobStorage` kelime sorgulanan dosyayı belirtmek için kullanılır.

- Sütun başvuruları ilk `_N` sütunun olduğu `_1`yer olarak belirtilir. Kaynak dosya bir üstbilgi satırı içeriyorsa, sütunlara üstbilgi satırında belirtilen ada göre başvurabilirsiniz. 

### <a name="net"></a>[.NET](#tab/dotnet)

Async yöntemi `BlobQuickQueryClient.QueryAsync` sorgu hızlandırma API'sine sorgu gönderir ve sonra sonuçları [akış](https://docs.microsoft.com/dotnet/api/system.io.stream?view=netframework-4.8) nesnesi olarak uygulamaya geri aktarır.

```cs
static async Task QueryHemingway(BlockBlobClient blob)
{
    string query = @"SELECT * FROM BlobStorage WHERE _3 = 'Hemingway, Ernest'";
    await DumpQueryCsv(blob, query, false);
}

private static async Task DumpQueryCsv(BlockBlobClient blob, string query, bool headers)
{
    try
    {
        using (var reader = new StreamReader((await blob.GetQuickQueryClient().QueryAsync(query,
                new CsvTextConfiguration() { HasHeaders = headers }, 
                new CsvTextConfiguration() { HasHeaders = false }, 
                new ErrorHandler(),
                new BlobRequestConditions(), 
                new ProgressHandler(),
                CancellationToken.None)).Value.Content))
        {
            using (var parser = new CsvReader(reader, new CsvConfiguration(CultureInfo.CurrentCulture) 
            { HasHeaderRecord = false }))
            {
                while (await parser.ReadAsync())
                {
                    parser.Context.Record.All(cell =>
                    {
                        Console.Out.Write(cell + "  ");
                        return true;
                    });
                    Console.Out.WriteLine();
                }
            }
        }
    }
    catch (Exception ex)
    {
        Console.Error.WriteLine("Exception: " + ex.ToString());
    }
}

class ErrorHandler : IBlobQueryErrorReceiver
{
    public void ReportError(BlobQueryError err)
    {
        Console.Error.WriteLine(String.Format("Error: {1}:{2}", err.Name, err.Description));
    }
}

class ProgressHandler : IProgress<long>
{
    public void Report(long value)
    {
        Console.Error.WriteLine("Bytes scanned: " + value.ToString());
    }
}

```

### <a name="java"></a>[Java](#tab/java)

Yöntem `BlobQuickQueryClient.openInputStream()` sorguyu sorgu hızlandırma API'sine gönderir ve ardından sonuçları diğer `InputStream` InputStream nesnesi gibi okunabilen bir nesne olarak uygulamaya geri aktarır.

```java
static void QueryHemingway(BlobClient blobClient) {
    String expression = "SELECT * FROM BlobStorage WHERE _3 = 'Hemingway, Ernest'";
    DumpQueryCsv(blobClient, expression, false);
}

static void DumpQueryCsv(BlobClient blobClient, String query, Boolean headers) {
    try {
    
        BlobQuickQueryDelimitedSerialization input = new BlobQuickQueryDelimitedSerialization()
            .setRecordSeparator('\n')
            .setColumnSeparator(',')
            .setHeadersPresent(headers)
            .setFieldQuote('\0')
            .setEscapeChar('\\');

        BlobQuickQueryDelimitedSerialization output = new BlobQuickQueryDelimitedSerialization()
            .setRecordSeparator('\n')
            .setColumnSeparator(',')
            .setHeadersPresent(false)
            .setFieldQuote('\0')
            .setEscapeChar('\n');
                
        BlobRequestConditions requestConditions = null;
        /* ErrorReceiver determines what to do on errors. */
        ErrorReceiver<BlobQuickQueryError> errorReceiver = System.out::println;

        /* ProgressReceiver details how to log progress*/
        com.azure.storage.common.ProgressReceiver progressReceiver = System.out::println;
    
        /* Create a query acceleration client to the blob. */
        BlobQuickQueryClient qqClient = new BlobQuickQueryClientBuilder(blobClient)
            .buildClient();
        /* Open the query input stream. */
        InputStream stream = qqClient.openInputStream(query, input, output, requestConditions, errorReceiver, progressReceiver);
            
        try (BufferedReader reader = new BufferedReader(new InputStreamReader(stream))) {
            /* Read from stream like you normally would. */
            for (CSVRecord record : CSVParser.parse(reader, CSVFormat.EXCEL)) {
                System.out.println(record.toString());
            }
        }
    } catch (Exception e) {
        System.err.println("Exception: " + e.toString());
    }
}
```

---

## <a name="retrieve-specific-columns"></a>Belirli sütunları alma

Sonuçlarınızı bir sütun alt kümesine göre görebilirsiniz. Bu şekilde yalnızca belirli bir hesaplama gerçekleştirmek için gereken sütunları alırsınız. Bu, uygulama performansını artırır ve ağ üzerinden daha az veri aktarıldığından maliyeti azaltır. 

Bu kod, yalnızca `PublicationYear` veri kümesindeki tüm kitapların sütununa alır. Ayrıca, sorgudaki sütunlara başvurmak için kaynak dosyadaki üstbilgi satırındaki bilgileri de kullanır.


### <a name="net"></a>[.NET](#tab/dotnet)

```cs
static async Task QueryPublishDates(BlockBlobClient blob)
{
    string query = @"SELECT PublicationYear FROM BlobStorage";
    await DumpQueryCsv(blob, query, true);
}
```

### <a name="java"></a>[Java](#tab/java)

```java
static void QueryPublishDates(BlobClient blobClient)
{
    String expression = "SELECT PublicationYear FROM BlobStorage";
    DumpQueryCsv(blobClient, expression, true);
}
```

---

Aşağıdaki kod, satır filtreleme ve sütun projeksiyonlarını aynı sorguda birleştirir. 

### <a name="net"></a>[.NET](#tab/dotnet)

```cs
static async Task QueryMysteryBooks(BlockBlobClient blob)
{
    string query = @"SELECT BibNum, Title, Author, ISBN, Publisher FROM BlobStorage WHERE Subjects LIKE '%Mystery%'";
    await DumpQueryCsv(blob, query, true);
}
```

### <a name="java"></a>[Java](#tab/java)

```java
static void QueryMysteryBooks(BlobClient blobClient)
{
    String expression = "SELECT BibNum, Title, Author, ISBN, Publisher FROM BlobStorage WHERE Subjects LIKE '%Mystery%'";
    DumpQueryCsv(blobClient, expression, true);
}
```

---

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu hızlandırma kayıt formu](https://aka.ms/adls/queryaccelerationpreview)    
- [Azure Veri Gölü Depolama sorgu hızlandırma (önizleme)](data-lake-storage-query-acceleration.md)
- [Sorgu hızlandırma SQL dil başvurusu (önizleme)](query-acceleration-sql-reference.md)
- Sorgu ivmesi REST API başvurusu
