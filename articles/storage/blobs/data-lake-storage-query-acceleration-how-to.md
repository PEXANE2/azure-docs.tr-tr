---
title: Azure Data Lake Storage sorgu hızlandırma (Önizleme) kullanarak verileri filtreleme | Microsoft Docs
description: Depolama hesabınızdan verilerin bir alt kümesini almak için sorgu hızlandırma (Önizleme) kullanın.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 04/21/2020
ms.author: normesta
ms.reviewer: jamsbak
ms.custom: devx-track-csharp
ms.openlocfilehash: 6de6661e5c970c7c3cbfc944b8539060b8844a36
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89005233"
---
# <a name="filter-data-by-using-azure-data-lake-storage-query-acceleration-preview"></a>Azure Data Lake Storage sorgu hızlandırma kullanarak verileri filtreleme (Önizleme)

Bu makalede, depolama hesabınızdan verilerin bir alt kümesini almak için sorgu hızlandırmasının (Önizleme) nasıl kullanılacağı gösterilir. 

Sorgu hızlandırma (Önizleme), uygulamaların ve analiz çerçevelerinin yalnızca belirli bir işlemi gerçekleştirmek için gereksinim duydukları verileri alarak veri işlemeyi önemli ölçüde iyileştirmesini sağlayan Azure Data Lake Storage yeni bir özelliğidir. Daha fazla bilgi için bkz. [Azure Data Lake Storage sorgu hızlandırma (Önizleme)](data-lake-storage-query-acceleration.md).

> [!NOTE]
> Sorgu hızlandırma özelliği genel önizlemededir ve Kanada Orta ve Fransa Orta bölgelerinde kullanılabilir. Sınırlamaları gözden geçirmek için, [bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın. Önizlemeye kaydolmak için [Bu forma](https://aka.ms/adls/qa-preview-signup)bakın.  

## <a name="prerequisites"></a>Önkoşullar

### <a name="net"></a>[.NET](#tab/dotnet)

- Azure depolama 'ya erişmek için bir Azure aboneliğine sahip olmanız gerekir. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- **Genel amaçlı v2** depolama hesabı. bkz. [depolama hesabı oluşturma](../common/storage-quickstart-create-account.md).

- [.NET SDK](https://dotnet.microsoft.com/download). 

### <a name="java"></a>[Java](#tab/java)

- Azure depolama 'ya erişmek için bir Azure aboneliğine sahip olmanız gerekir. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- **Genel amaçlı v2** depolama hesabı. bkz. [depolama hesabı oluşturma](../common/storage-quickstart-create-account.md).

- [Java Development Kit (JDK)](/java/azure/jdk/?view=azure-java-stable) sürüm 8 veya üzeri.

- [Apache Maven](https://maven.apache.org/download.cgi). 

  > [!NOTE] 
  > Bu makalede, Apache Maven kullanarak bir Java projesi oluşturduğunuzu varsayılmaktadır. Apache Maven kullanarak bir projenin nasıl oluşturulacağı hakkında bir örnek için bkz. [ayarlama](storage-quickstart-blobs-java.md#setting-up).
  
---

## <a name="install-packages"></a>Paketleri yükler 

### <a name="net"></a>[.NET](#tab/dotnet)

1. Sorgu hızlandırma paketlerini indirin. Bu bağlantıyı kullanarak bu paketleri içeren sıkıştırılmış bir. zip dosyası elde edebilirsiniz: [https://aka.ms/adls/qqsdk/.net](https://aka.ms/adls/qqsdk/.net) . 

2. Bu dosyanın içeriğini proje dizininize ayıklayın.

3. Proje dosyanızı (*. csproj*) bir metin düzenleyicisinde açın ve bu paket başvurularını öğesinin içine ekleyin \<Project\> .

   ```xml
   <ItemGroup>
       <PackageReference Include="Azure.Storage.Blobs" Version="12.5.0-preview.1" />
       <PackageReference Include="Azure.Storage.Common" Version="12.4.0-preview.1" />
       <PackageReference Include="Azure.Storage.QuickQuery" Version="12.0.0-preview.1" />
   </ItemGroup>
   ```

4. Önizleme SDK paketlerini geri yükleyin. Bu örnek komut, komutunu kullanarak önizleme SDK paketlerini geri yükler `dotnet restore` . 

   ```console
   dotnet restore --source C:\Users\contoso\myProject
   ```

5. Tüm diğer bağımlılıkları genel NuGet deposundan geri yükleyin.

   ```console
   dotnet restore
   ```

### <a name="java"></a>[Java](#tab/java)

1. Projenizin kökünde dizin oluşturun. Kök dizin, **pom.xml** dosyasını içeren dizindir.

   > [!NOTE]
   > Bu makaledeki örneklerde, Dizin adının **lib**olduğu varsayılır.

2. Sorgu hızlandırma paketlerini indirin. Bu bağlantıyı kullanarak bu paketleri içeren sıkıştırılmış bir. zip dosyası elde edebilirsiniz: [https://aka.ms/adls/qqsdk/java](https://aka.ms/adls/qqsdk/java) . 

3. Bu. zip dosyasındaki dosyaları oluşturduğunuz dizine ayıklayın. Örneğimizde, bu dizin **lib**olarak adlandırılmıştır. 

4. *pom.xml* dosyasını metin düzenleyicinizde açın. Aşağıdaki bağımlılık öğelerini bağımlılıklar grubuna ekleyin. 

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

## <a name="add-statements"></a>Deyim Ekle


### <a name="net"></a>[.NET](#tab/dotnet)

Bu `using` deyimleri, kod dosyanızın en üstüne ekleyin.

```csharp
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
using Azure.Storage.Blobs.Specialized;
using Azure.Storage.QuickQuery;
using Azure.Storage.QuickQuery.Models;
```

Sorgu hızlandırma CSV ve JSON biçimli verileri alır. Bu nedenle, kullanmayı seçtiğiniz CSV veya JSON ayrıştırma kitaplıklarının using deyimlerini eklediğinizden emin olun. Bu makalede görüntülenen örneklerde, NuGet 'de bulunan [Csvhelper](https://www.nuget.org/packages/CsvHelper/) kitaplığını kullanarak bir CSV dosyası ayrıştırılabilir. Bu nedenle, bu `using` deyimleri kod dosyasının en üstüne ekleyeceğiz.

```csharp
using CsvHelper;
using CsvHelper.Configuration;
```

Bu makalede sunulan örnekleri derlemek için de bu deyimleri de eklemeniz gerekir `using` .

```csharp
using System.Threading.Tasks;
using System.IO;
using System.Globalization;
using System.Threading;
using System.Linq;
```

### <a name="java"></a>[Java](#tab/java)

Bu `import` deyimleri, kod dosyanızın en üstüne ekleyin.

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

Bir sorgu hızlandırma isteğindeki satır filtresi koşullarını ve sütun projeksiyonlarını belirtmek için SQL 'i kullanabilirsiniz. Aşağıdaki kod, depolama alanındaki bir CSV dosyasını sorgular ve üçüncü sütunun değerle eşleştiği tüm veri satırlarını döndürür `Hemingway, Ernest` . 

- SQL sorgusunda, `BlobStorage` sorgulanmakta olan dosyayı belirtmek için anahtar sözcüğü kullanılır.

- Sütun başvuruları, `_N` ilk sütunun bulunduğu konum olarak belirtilir `_1` . Kaynak dosya bir başlık satırı içeriyorsa, üst bilgi satırında belirtilen ada göre sütunlara başvurabilirsiniz. 

### <a name="net"></a>[.NET](#tab/dotnet)

Async yöntemi sorguyu `BlobQuickQueryClient.QueryAsync` sorgu HıZLANDıRMA API 'sine gönderir ve ardından sonuçları bir [Stream](https://docs.microsoft.com/dotnet/api/system.io.stream?view=netframework-4.8) nesnesi olarak uygulamaya geri akış sağlar.

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
        Console.Error.WriteLine($"Error: {err.Name}:{ err.Description }");
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

Yöntemi sorguyu `BlobQuickQueryClient.openInputStream()` sorgu HıZLANDıRMA API 'sine gönderir ve ardından sonuçları `InputStream` başka bir InputStream nesnesi gibi okuyabilen bir nesne olarak uygulamaya geri gönderir.

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

## <a name="retrieve-specific-columns"></a>Belirli sütunları al

Sonuçlarınızı bir sütun alt kümesiyle kapsamını belirleyebilirsiniz. Bu şekilde, yalnızca belirli bir hesaplamayı gerçekleştirmek için gereken sütunları elde edersiniz. Bu, uygulama performansını geliştirir ve ağ üzerinden daha az veri aktarıldığından maliyeti azaltır. 

Bu kod, `PublicationYear` veri kümesindeki tüm kitaplar için yalnızca sütunu alır. Ayrıca, sorgudaki sütunlara başvurmak için kaynak dosyasındaki üst bilgi satırındaki bilgileri de kullanır.


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

Aşağıdaki kod, satır filtrelemeyi ve sütun projeksiyonlarını aynı sorguyla birleştirir. 

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

- [Sorgu hızlandırma kayıt formu](https://aka.ms/adls/qa-preview-signup)    
- [Azure Data Lake Storage sorgu hızlandırma (Önizleme)](data-lake-storage-query-acceleration.md)
- [Sorgu hızlandırma SQL dil başvurusu (Önizleme)](query-acceleration-sql-reference.md)
