---
title: Azure Data Lake için U-SQL Kullanıcı tanımlı Extractor Programlama Kılavuzu
description: U-SQL UıDO Programlama Kılavuzu-Kullanıcı tanımlı ayıklayıcısı hakkında bilgi edinin.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: a3613c2b95f13e6bbaaf570f522ad1f7b7edd756
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96512681"
---
# <a name="use-user-defined-extractor"></a>Kullanıcı tanımlı ayıklayıcısı kullan

## <a name="u-sql-udo-user-defined-extractor"></a>U-SQL UDO: Kullanıcı tanımlı ayıklayıcısı
U-SQL, ayıklama ifadesini kullanarak dış verileri içeri aktarmanıza olanak tanır. EXTRACT ifadesinde yerleşik UıDO ayıklayıcıları kullanılabilir:  

* *Ayıklayıcıları. Text ()*: farklı kodlamalardan sınırlandırılmış metin dosyalarından ayıklama sağlar.

* *Extractors.Csv ()*: farklı kodlamalar için virgülle ayrılmış değer (CSV) dosyalarından ayıklama sağlar.

* *Ayıklayıcıları. TSV ()*: farklı kodlamalara ait sekmeyle ayrılmış değer (TSV) dosyalarından ayıklama sağlar.

Özel bir ayıklayıcısı geliştirmek yararlı olabilir. Aşağıdaki görevlerden herhangi birini yapmak istiyorsam, bu, veri içeri aktarma sırasında yararlı olabilir:

* Sütunları bölerek ve ayrı değerleri değiştirerek giriş verilerini değiştirin. IŞLEMCI işlevselliği sütunları birleştirmek için daha iyidir.
* Web sayfaları ve e-postalar gibi yapılandırılmamış verileri veya XML/JSON gibi yarı yapılandırılmamış verileri ayrıştırın.
* Desteklenmeyen Kodlamadaki verileri ayrıştırın.

## <a name="how-to-define-and-use-user-defined-extractor"></a>Kullanıcı tanımlı ayıklayıcısı tanımlama ve kullanma
Kullanıcı tanımlı bir ayıklayıcı veya NCESI tanımlamak için bir arabirim oluşturuyoruz `IExtractor` . Yok edilecek sütun/satır sınırlayıcıları ve kodlama gibi tüm giriş parametrelerinin, sınıfın oluşturucusunda tanımlanması gerekir. `IExtractor`Arabirim Ayrıca `IEnumerable<IRow>` geçersiz kılma için aşağıdaki gibi bir tanım içermelidir:

```csharp
[SqlUserDefinedExtractor]
public class SampleExtractor : IExtractor
{
     public SampleExtractor(string row_delimiter, char col_delimiter)
     { … }

     public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
     { … }
}
```

**SqlUserDefinedExtractor** özniteliği, türün Kullanıcı tanımlı bir ayıklayıcı olarak kaydedilmesi gerektiğini belirtir. Bu sınıf devralınamaz.

SqlUserDefinedExtractor, NCESI tanımı için isteğe bağlı bir özniteliktir. NCESI nesnesi için AtomicFileProcessing özelliğini tanımlamak üzere kullanılır.

* bool AtomicFileProcessing   

* **true** = bu ayıklayıcı atomik giriş dosyaları gerektirdiğini BELIRTIR (JSON, XML,...)
* **false** = bu Extractor, bölünmüş/dağıtılmış DOSYALARLA (CSV, seq,...) ilgilenebileceğini belirtir

Ana NCESI programlama nesneleri **giriş** ve **Çıkış**. Giriş nesnesi, giriş verilerini olarak sıralamak için kullanılır `IUnstructuredReader` . Çıkış nesnesi, Extractor etkinliğinin sonucu olarak çıkış verilerini ayarlamak için kullanılır.

Giriş verilerine ve aracılığıyla erişilir `System.IO.Stream` `System.IO.StreamReader` .

Giriş sütunları numaralandırması için, önce bir satır sınırlayıcısı kullanarak giriş akışını böleceğiz.

```csharp
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

Ardından, giriş satırını daha fazla sütun bölümüne bölebilirsiniz.

```csharp
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

Çıktı verilerini ayarlamak için `output.Set` yöntemini kullanıyoruz.

Özel ayıklayıcı 'nın yalnızca çıkış ile tanımlanan sütunları ve değerleri çıktı olarak öğrendiğinden emin olmak önemlidir. Yöntem çağrısını ayarla.

```csharp
output.Set<string>(count, part);
```

Gerçek ayıklayıcı çıktısı çağırarak tetiklenir `yield return output.AsReadOnly();` .

Aşağıda, Extractor örneği verilmiştir:

```csharp
[SqlUserDefinedExtractor(AtomicFileProcessing = true)]
public class FullDescriptionExtractor : IExtractor
{
     private Encoding _encoding;
     private byte[] _row_delim;
     private char _col_delim;

    public FullDescriptionExtractor(Encoding encoding, string row_delim = "\r\n", char col_delim = '\t')
    {
         this._encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
         this._row_delim = this._encoding.GetBytes(row_delim);
         this._col_delim = col_delim;

    }

    public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
    {
         string line;
         //Read the input line by line
         foreach (Stream current in input.Split(_encoding.GetBytes("\r\n")))
         {
        using (System.IO.StreamReader streamReader = new StreamReader(current, this._encoding))
         {
             line = streamReader.ReadToEnd().Trim();
             //Split the input by the column delimiter
             string[] parts = line.Split(this._col_delim);
             int count = 0; // start with first column
             foreach (string part in parts)
             {
    if (count == 0)
             {  // for column “guid”, re-generated guid
                 Guid new_guid = Guid.NewGuid();
                 output.Set<Guid>(count, new_guid);
             }
             else if (count == 2)
             {
                 // for column “user”, convert to UPPER case
                 output.Set<string>(count, part.ToUpper());

             }
             else
             {
                 // keep the rest of the columns as-is
                 output.Set<string>(count, part);
             }
             count += 1;
             }

         }
         yield return output.AsReadOnly();
         }
         yield break;
     }
}
```

Bu kullanım örneği senaryosunda, Extractor "GUID" sütunu için GUID 'ı yeniden oluşturur ve "user" sütununun değerlerini büyük harfe dönüştürür. Özel ayıklayıcıları, giriş verilerini ayrıştırarak ve işleyerek daha karmaşık sonuçlar üretebilir.

Aşağıda özel bir ayıklayıcısı kullanan temel U-SQL betiği verilmiştir:

```usql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file
        USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 TO @output_file USING Outputters.Text();
```


## <a name="next-steps"></a>Sonraki adımlar
* [U-SQL programlama kılavuzu-genel bakış](data-lake-analytics-u-sql-programmability-guide.md)
* [U-SQL programlama kılavuzu-UDT ve UDADGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)