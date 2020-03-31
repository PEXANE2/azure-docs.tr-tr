---
title: Azure Akış Analizi'nde .NET özel deserializers kullanarak girişi herhangi bir formatta okuyun
description: Bu makalede, Azure Akış Analizi bulutu ve kenar işleri için özel .NET deserializers tanımlayan serileştirme biçimi ve arabirimleri açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 1/28/2020
ms.openlocfilehash: 270e9a31c28e7209cfe43ea8307b928ed3257a35
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76845266"
---
# <a name="read-input-in-any-format-using-net-custom-deserializers"></a>.NET özel deserializers kullanarak herhangi bir biçimde giriş okuyun

.NET özel deserializers Azure Stream Analytics iş üç [yerleşik veri biçimleri](stream-analytics-parsing-json.md)dışında biçimlerinden verileri okumak için izin verir. Bu makalede, Azure Akış Analizi bulutu ve kenar işleri için .NET özel deserializers tanımlayan serileştirme biçimi ve arabirimleri açıklanmaktadır. Protokol Arabelleği ve CSV biçimi için örnek deserializers da vardır.

## <a name="net-custom-deserializer"></a>.NET özel deserializer

Aşağıdaki kod örnekleri, özel deserializer'ı tanımlayan `StreamDeserializer<T>`ve uygulayan arabirimlerdir.

`UserDefinedOperator`tüm özel akış işleçleri için taban sınıftır. Deserializer'ınızla ilgili sorunları ayıklamanız gereken tanılamayı yayımlama mekanizması nı içeren bağlam ı sağlar. `StreamingContext`

```csharp
    public abstract class UserDefinedOperator
    {
        public abstract void Initialize(StreamingContext streamingContext);
    }
```

Aşağıdaki kod parçacığı veri akışı için deserialization olduğunu. 

Atlanabilir hatalar ''ın `IStreamingDiagnostics` Initialize `UserDefinedOperator`yöntemi ile geçirkullanılarak yayılmalıdır. Tüm özel durumlar hata olarak kabul edilir ve deserializer yeniden oluşturulur. Belirli sayıda hatadan sonra, iş başarısız bir duruma gider.

`StreamDeserializer<T>`bir akışı tür `T`nesnesine deserialize eder. Aşağıdaki koşullar yerine getirilmelidir:

1. T bir sınıf ya da bir yapıdır.
1. T'deki tüm ortak alanlar ya
    1. [Bayt, bayt, kısa, ushort, int, uint, long, DateTime, string, float, double] veya bunların nullable eşdeğerleri.
    1. Aynı kurallara uyarak başka bir yapı veya sınıf.
    1. Aynı kuralları `T2` izleyen tür dizisi.
    1. T2'nin aynı kurallara uyan iList.`T2`
    1. Özyinelemeli türleri yoktur.

Parametre `stream` serileştirilmiş nesneyi içeren akıştır. `Deserialize`örneklerin `T` bir koleksiyon döndürür.

```csharp
    public abstract class StreamDeserializer<T> : UserDefinedOperator
    {
        public abstract IEnumerable<T> Deserialize(Stream stream);
    }
```

`StreamingContext`kullanıcı operatörü için tanılama yayımlama mekanizması içeren bağlam sağlar.

```csharp
    public abstract class StreamingContext
    {
        public abstract StreamingDiagnostics Diagnostics { get; }
    }
```

`StreamingDiagnostics`serializer, deserializer ve kullanıcı tanımlı işlevleri de dahil olmak üzere kullanıcı tanımlı işleçler için tanılama olduğunu.

`WriteError`tanılama günlüklerine bir hata iletisi yazar ve hatayı tanılama için gönderir.

`briefMessage`kısa bir hata iletisidir. Bu ileti tanılamada gösterir ve ürün ekibi tarafından hata ayıklama amacıyla kullanılır. Hassas bilgiler eklemeyin ve iletiyi 200 karakterden daha az tutmayın

`detailedMessage`yalnızca depolama alanınızdaki tanılama günlüklerinize eklenen ayrıntılı bir hata iletisidir. Bu ileti 2000 karakterden az olmalıdır.

```csharp
    public abstract class StreamingDiagnostics
    {
        public abstract void WriteError(string briefMessage, string detailedMessage);
    }
```

## <a name="deserializer-examples"></a>Deserializer örnekleri

Bu bölümde, Protobuf ve CSV için özel deserializers yazmak nasıl gösterir. Olay Hub Yakalama için AVRO biçimi gibi ek örnekler için [GitHub'daki Azure Akış Analizi'ni](https://github.com/Azure/azure-stream-analytics/tree/master/CustomDeserializers)ziyaret edin.

### <a name="protocol-buffer-protobuf-format"></a>Protokol arabelleği (Protobuf) biçimi

Bu, protokol arabellek biçimini kullanan bir örnektir.

Aşağıdaki protokol arabellek tanımını varsayalım.

```proto
syntax = "proto3";
// protoc.exe from nuget "Google.Protobuf.Tools" is used to generate .cs file from this schema definition.
// Run below command to generate the csharp class
// protoc.exe --csharp_out=. MessageBodyProto.proto

package SimulatedTemperatureSensor;
message MessageBodyProto {
    message Ambient {
      double temperature = 1;
      int64 humidity = 2;
    }

    message Machine {
      double temperature = 1;
      double pressure = 2;
    }

    Machine machine = 1;
    Ambient ambient = 2;
    string timeCreated = 3;
}
```

`protoc.exe` **Google.Protobuf.Tools** NuGet'den çalışan bir .cs dosyası oluşturur. Oluşturulan dosya burada gösterilmez.

Aşağıdaki kod parçacığı, oluşturulan dosyanın projeye dahil edildiğini varsayarak deserializer uygulamasıdır. Bu uygulama, oluşturulan dosya üzerinde sadece ince bir sarıcı.

```csharp
    public class MessageBodyDeserializer : StreamDeserializer<SimulatedTemperatureSensor.MessageBodyProto>
    {
        public override IEnumerable<SimulatedTemperatureSensor.MessageBodyProto> Deserialize(Stream stream)
        {
            while (stream.Position < stream.Length)
            {
                yield return SimulatedTemperatureSensor.MessageBodyProto.Parser.ParseDelimitedFrom(stream);
            }
        }

        public override void Initialize(StreamingContext streamingContext)
        {
        }
    }
```

### <a name="csv"></a>CSV

Aşağıdaki kod snippet aynı zamanda yayılma hataları gösteren basit bir CSV deserializer olduğunu.

```csharp
using System.Collections.Generic;
using System.IO;

using Microsoft.Azure.StreamAnalytics;
using Microsoft.Azure.StreamAnalytics.Serialization;

namespace ExampleCustomCode.Serialization
{
    public class CustomCsvDeserializer : StreamDeserializer<CustomEvent>
    {
        private StreamingDiagnostics streamingDiagnostics;

        public override IEnumerable<CustomEvent> Deserialize(Stream stream)
        {
            using (var sr = new StreamReader(stream))
            {
                string line = sr.ReadLine();
                while (line != null)
                {
                    if (line.Length > 0 && !string.IsNullOrWhiteSpace(line))
                    {
                        string[] parts = line.Split(',');
                        if (parts.Length != 3)
                        {
                            streamingDiagnostics.WriteError("Did not get expected number of columns", $"Invalid line: {line}");
                        }
                        else
                        {
                            yield return new CustomEvent()
                            {
                                Column1 = parts[0],
                                Column2 = parts[1],
                                Column3 = parts[2]
                            };
                        }
                    }

                    line = sr.ReadLine();
                }
            }
        }

        public override void Initialize(StreamingContext streamingContext)
        {
            this.streamingDiagnostics = streamingContext.Diagnostics;
        }
    }

    public class CustomEvent
    {
        public string Column1 { get; set; }

        public string Column2 { get; set; }

        public string Column3 { get; set; }
    }
}

```

## <a name="serialization-format-for-rest-apis"></a>REST API'leri için serileştirme biçimi

Her Akış Analizi girişinin bir **serileştirme biçimi**vardır. Giriş seçenekleri hakkında daha fazla bilgi [için, Giriş REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-input) belgelerine bakın.

Aşağıdaki Javascript kodu, REST API'sını kullanırken .NET deserializer serileştirme biçimine bir örnektir:

```javascript
{    
   "properties":{    
      "type":"stream",  
      "serialization":{    
         "type":"CustomCLR",  
         "properties":{    
            "serializationDllPath":"<path to the dll inside UserCustomCode\CLR\ folder>", 
            "serializationClassName":"<Full name of the deserializer class name>" 
         }  
      }
   }  
}  
```

`serializationClassName`uygulayan bir sınıf `StreamDeserializer<T>`olmalıdır. Bu, aşağıdaki bölümde açıklanmıştır.

## <a name="region-support"></a>Bölge desteği

Bu özellik aşağıdaki bölgelerde kullanılabilir:

* Orta Batı ABD
* Kuzey Avrupa
* Doğu ABD
* Batı ABD
* Doğu ABD 2
* Batı Avrupa

Ek bölgeler için [destek isteyebilirsiniz.](https://aka.ms/ccodereqregion)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="when-will-this-feature-be-available-in-all-azure-regions"></a>Bu özellik tüm Azure bölgelerinde ne zaman kullanılabilir olacak?

Bu özellik [6 bölgede](https://docs.microsoft.com/azure/stream-analytics/custom-deserializer-examples#region-support)kullanılabilir. Bu işlevselliği başka bir bölgede kullanmak istiyorsanız, [bir istek gönderebilirsiniz.](https://aka.ms/ccodereqregion) Tüm Azure bölgeleri için destek yol haritasında yer almaktadır.

### <a name="can-i-access-metadatapropertyvalue-from-my-inputs-similar-to-getmetadatapropertyvalue-function"></a>GetMetadataPropertyValue işlevine benzer girişlerimden MetadataPropertyValue'a erişebilir miyim?

Bu işlevsellik desteklenmez. Bu yeteneğe ihtiyacınız varsa, [UserVoice'ta](https://feedback.azure.com/forums/270577-stream-analytics/suggestions/38779801-accessing-input-metadata-properties-in-custom-dese)bu isteğe oy verebilirsiniz.

### <a name="can-i-share-my-deserializer-implementation-with-the-community-so-that-others-can-benefit"></a>Başkalarının yararlanabilmesi için deserializer uygulamamı toplulukla paylaşabilir miyim?

Deserializer'ınızı uyguladıktan sonra, toplulukla paylaşarak başkalarına yardımcı olabilirsiniz. Kodunuzu Azure [Akışı Analytics GitHub repo'suna](https://github.com/Azure/azure-stream-analytics/tree/master/CustomDeserializers)gönderin.

## <a name="next-steps"></a>Sonraki Adımlar

* [.Azure Akış Analizi bulut işleri için .NET özel deserializers](custom-deserializer.md)
