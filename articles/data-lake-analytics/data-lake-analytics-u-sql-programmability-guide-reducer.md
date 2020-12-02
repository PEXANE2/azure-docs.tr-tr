---
title: U-SQL Kullanıcı tanımlı Reducer Azure Data Lake için Programlama Kılavuzu
description: U-SQL UıDO Programlama Kılavuzu-Kullanıcı tanımlı Reducer hakkında bilgi edinin.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: 52d44685678c3e89dc820042a7925d284500cef8
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96512633"
---
# <a name="use-user-defined-reducer"></a>Kullanıcı tanımlı Reducer kullanma

## <a name="u-sql-udo-user-defined-reducer"></a>U-SQL UDO: Kullanıcı tanımlı Reducer

U-SQL, Kullanıcı tanımlı operatör genişletilebilirlik çerçevesini kullanarak ve bir IReducer arabirimi uygulayarak C# dilinde özel satır kümesi azaltıcının yazmanızı sağlar.

Kullanıcı tanımlı Reducer veya UDR, veri ayıklama (içeri aktarma) sırasında gereksiz satırları ortadan kaldırmak için kullanılabilir. Ayrıca, satırları ve sütunları işlemek ve değerlendirmek için de kullanılabilir. Programlama mantığını temel alarak, hangi satırların ayıklanmak gerektiğini de tanımlayabilir.

## <a name="how-to-define-and-use-user-defined-reducer"></a>Kullanıcı tanımlı Reducer tanımlama ve kullanma
UDR sınıfını tanımlamak için `IReducer` isteğe bağlı bir özniteliğe sahip bir arabirim oluşturuyoruz `SqlUserDefinedReducer` .

Bu sınıf arabirimi, `IEnumerable` arabirim satır kümesi geçersiz kılma için bir tanım içermelidir.

```csharp
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
        …
    }

}
```

**SqlUserDefinedReducer** özniteliği, türün Kullanıcı tanımlı Reducer olarak kaydedilmesi gerektiğini gösterir. Bu sınıf devralınamaz.
**SqlUserDefinedReducer** , Kullanıcı tanımlı Reducer tanımı için isteğe bağlı bir özniteliktir. Isyinelemeli özelliği tanımlamak için kullanılır.

* bool Isyinelemeli    
* **true**  = bu Reducer ilişkilendirilebilir ve iletişim olup olmadığını belirtir

Ana programlama nesneleri **giriş** ve **Çıkış**' dır. Giriş nesnesi, giriş satırlarını numaralandırmak için kullanılır. Çıktı, etkinliğin azaltılmasının sonucu olarak çıktı satırlarını ayarlamak için kullanılır.

Giriş satırları numaralandırması için `Row.Get` yöntemini kullanırız.

```csharp
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

Yöntemi için parametresi, `Row.Get` `PRODUCE` `REDUCE` U-SQL temel betiği ifadesinin sınıfının bir parçası olarak geçirilmiş bir sütundur. Burada doğru veri türünü de kullanmanız gerekir.

Çıktı için `output.Set` yöntemini kullanın.

Özel Reducer yalnızca yöntem çağrısıyla tanımlanan değerlerin çıkış yaptığını anlamak önemlidir `output.Set` .

```csharp
output.Set<string>("mycolumn", guid);
```

Gerçek Reducer çıkışı çağırarak tetiklenir `yield return output.AsReadOnly();` .

Aşağıda bir Reducer örneği verilmiştir:

```csharp
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
    string guid;
    DateTime dt;
    string user;
    string des;

    foreach (IRow row in input.Rows)
    {
        guid = row.Get<string>("guid");
        dt = row.Get<DateTime>("dt");
        user = row.Get<string>("user");
        des = row.Get<string>("des");

        if (user.Length > 0)
        {
        output.Set<string>("guid", guid);
        output.Set<DateTime>("dt", dt);
        output.Set<string>("user", user);
        output.Set<string>("des", des);

        yield return output.AsReadOnly();
        }
    }
    }

}
```

Bu kullanım örneği senaryosunda, Reducer boş Kullanıcı adına sahip satırları atlıyor. Satır kümesindeki her satır için, gerekli her sütunu okur, ardından Kullanıcı adının uzunluğunu değerlendirir. Yalnızca Kullanıcı adı değerinin uzunluğu 0 ' dan büyükse gerçek satırı verir.

Aşağıda, özel bir Reducer kullanan temel U-SQL betiği verilmiştir:

```usql
DECLARE @input_file string = @"\usql-programmability\input_file_reducer.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    REDUCE @rs0 PRESORT guid
    ON guid  
    PRODUCE guid string, dt DateTime, user String, des String
    USING new USQL_Programmability.EmptyUserReducer();

@rs2 =
    SELECT guid AS start_id,
           dt AS start_time,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS start_fiscalperiod,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    TO @output_file 
    USING Outputters.Text();
```

## <a name="next-steps"></a>Sonraki adımlar
* [U-SQL programlama kılavuzu-genel bakış](data-lake-analytics-u-sql-programmability-guide.md)
* [U-SQL programlama kılavuzu-UDT ve UDADGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)