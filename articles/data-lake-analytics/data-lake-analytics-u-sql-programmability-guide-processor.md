---
title: Azure Data Lake için U-SQL Kullanıcı tanımlı işlemci Programlama Kılavuzu
description: U-SQL UıDO Programlama Kılavuzu-Kullanıcı tanımlı işlemcisi hakkında bilgi edinin.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: 6ff45c577e94a8c63bd7cb1e6603e4d5519af5c6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96512693"
---
# <a name="use-user-defined-processor"></a>Kullanıcı tarafından tanımlanan işlemciyi kullanma

## <a name="u-sql-udo-user-defined-processor"></a>U-SQL UDO: Kullanıcı tanımlı işlemci
Kullanıcı tanımlı işlemci veya UDP, programlama özellikleri uygulayarak gelen satırları işlemenizi sağlayan bir U-SQL UDO türüdür. UDP sütunları birleştirip, değerleri değiştirmenize ve gerekirse yeni sütunlar eklemenize olanak sağlar. Temel olarak, gerekli veri öğelerini oluşturmak için bir satır kümesini işlemeye yardımcı olur.

## <a name="how-to-define-and-use-user-defined-processor"></a>Kullanıcı tanımlı işlemciyi tanımlama ve kullanma
UDP tanımlamak için, özniteliğine sahip bir arabirim oluşturmanız gerekir `IProcessor` `SqlUserDefinedProcessor` , bu, UDP için isteğe bağlıdır.

Bu arabirim, `IRow` Aşağıdaki örnekte gösterildiği gibi, arabirim satır kümesi geçersiz kılma tanımını içermelidir:

```csharp
[SqlUserDefinedProcessor]
public class MyProcessor: IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
        …
 }
}
```

**Sqluserdefinedprocessor** , türün Kullanıcı tanımlı bir işlemci olarak kaydedilmesi gerektiğini gösterir. Bu sınıf devralınamaz.

SqlUserDefinedProcessor özniteliği UDP tanımı için **isteğe bağlıdır** .

Ana programlama nesneleri **giriş** ve **Çıkış**' dır. Giriş nesnesi, giriş sütunlarını ve çıkışını numaralandırmak ve çıkış verilerini işlemci etkinliğinin sonucu olarak ayarlamak için kullanılır.

Giriş sütunları numaralandırması için `input.Get` yöntemini kullanırız.

```csharp
string column_name = input.Get<string>("column_name");
```

Yöntemi parametresi, `input.Get` `PRODUCE` `PROCESS` U-SQL temel betiği ifadesinin bir parçası olarak geçirilen bir sütundur. Burada doğru veri türünü kullandık.

Çıktı için `output.Set` yöntemini kullanın.

Özel üreticinin yalnızca yöntem çağrısıyla tanımlanan sütunları ve değerleri çıktı olarak gösterdiğine dikkat edin `output.Set` .

```csharp
output.Set<string>("mycolumn", mycolumn);
```

Gerçek işlemci çıkışı çağırarak tetiklenir `return output.AsReadOnly();` .

Aşağıda bir işlemci örneği verilmiştir:

```csharp
[SqlUserDefinedProcessor]
public class FullDescriptionProcessor : IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
     string user = input.Get<string>("user");
     string des = input.Get<string>("des");
     string full_description = user.ToUpper() + "=>" + des;
     output.Set<string>("dt", input.Get<string>("dt"));
     output.Set<string>("full_description", full_description);
     output.Set<Guid>("new_guid", Guid.NewGuid());
     output.Set<Guid>("guid", input.Get<Guid>("guid"));
     return output.AsReadOnly();
 }
}
```

Bu kullanım örneği senaryosunda, işlemci mevcut sütunları birleştirerek "full_description" adlı yeni bir sütun oluşturuyor (Bu durumda, büyük harfle "user" ve "des"). Ayrıca bir GUID 'YI yeniden oluşturur ve özgün ve yeni GUID değerlerini döndürür.

Önceki örnekte görebileceğiniz gibi, yöntem çağrısı sırasında C# yöntemlerini çağırabilirsiniz `output.Set` .

Aşağıda, özel bir işlemci kullanan bir Base U-SQL betiğinin örneği verilmiştir:

```usql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
     PROCESS @rs0
     PRODUCE dt String,
             full_description String,
             guid Guid,
             new_guid Guid
     USING new USQL_Programmability.FullDescriptionProcessor();

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```


## <a name="next-steps"></a>Sonraki adımlar
* [U-SQL programlama kılavuzu-genel bakış](data-lake-analytics-u-sql-programmability-guide.md)
* [U-SQL programlama kılavuzu-UDT ve UDADGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)