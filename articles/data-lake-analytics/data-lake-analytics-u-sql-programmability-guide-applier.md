---
title: U-SQL Kullanıcı tanımlı uygulayıcı Azure Data Lake için Programlama Kılavuzu
description: U-SQL UıDO Programlama Kılavuzu-Kullanıcı tanımlı uygulayıcı hakkında bilgi edinin.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: 0842a2cfa021ef8ea45c19ec885c7dec371730de
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96512645"
---
# <a name="use-user-defined-applier"></a>Kullanıcı tarafından tanımlanan uygulayıcıyı kullanma 

## <a name="u-sql-udo-user-defined-applier"></a>U-SQL UDO: Kullanıcı tanımlı Uygulayıcı
U-SQL Kullanıcı tanımlı bir uygulayıcı, bir sorgunun dış tablo ifadesiyle döndürülen her satır için özel bir C# işlevini çağırmanızı sağlar. Doğru giriş, sol girdinin her bir satırı için değerlendirilir ve üretilen satırlar nihai çıkış için birleştirilir. APPLY işleci tarafından üretilen sütunların listesi, sol taraftaki sütun kümesinin ve doğru girişin birleşimidir.


## <a name="how-to-define-and-use-user-defined-applier"></a>Kullanıcı tanımlı uygulayıcı tanımlama ve kullanma
Kullanıcı tanımlı Uygulayıcı, USQL SELECT ifadesinin bir parçası olarak çağrılır.

Kullanıcı tanımlı Uygulayıcı için tipik çağrı aşağıdakine benzer:

```usql
SELECT …
FROM …
CROSS APPLYis used to pass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

Bir SELECT ifadesinde appliers kullanma hakkında daha fazla bilgi için, bkz. [U-SQL SELECT-Apply ve Outer Apply 'Tan seçme](/u-sql/statements-and-expressions/select/from/select-selecting-from-cross-apply-and-outer-apply).

Kullanıcı tanımlı uygulayıcı temel sınıf tanımı aşağıdaki gibidir:

```csharp
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

Kullanıcı tanımlı bir uygulayıcı tanımlamak için `IApplier` arabirimi, `SqlUserDefinedApplier` Kullanıcı tanımlı uygulayıcı tanımı için isteğe bağlı olan [] özniteliğiyle oluşturmanız gerekir.

```csharp
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
    public ParserApplier()
    {
        …
    }

    public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
    {
        …
    }
}
```

* Apply, dış tablonun her satırı için çağırılır. `IUpdatableRow`Çıkış satır kümesini döndürür.
* Oluşturucu sınıfı, parametreleri Kullanıcı tanımlı uygulayıcı geçirmek için kullanılır.

**Sqluserdefinedapp,** türün Kullanıcı tanımlı bir uygulayıcı olarak kaydedilmesi gerektiğini belirtir. Bu sınıf devralınamaz.

**Sqluserdefinedappfetıon** , Kullanıcı tanımlı bir uygulayıcı tanımı için **isteğe bağlıdır** .


Ana programlama nesneleri aşağıdaki gibidir:

```csharp
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

Giriş satır kümeleri girdi olarak geçirilir `IRow` . Çıkış satırları çıkış arabirimi olarak oluşturulur `IUpdatableRow` .

Tek tek sütun adları, şema yöntemi çağırarak belirlenebilir `IRow` .

```csharp
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Gelen gerçek veri değerlerini almak için `IRow` arabirimin Get () yöntemini kullanırız `IRow` .

```csharp
mycolumn = row.Get<int>("mycolumn")
```

Ya da şema sütun adını kullanıyoruz:

```csharp
row.Get<int>(row.Schema[0].Name)
```

Çıkış değerlerinin çıkış ile ayarlanması gerekir `IUpdatableRow` :

```csharp
output.Set<int>("mycolumn", mycolumn)
```

Özel appliers yalnızca çıkış sütunlarının ve yöntem çağrısıyla tanımlanmış değerlerin olduğunu anlamak önemlidir `output.Set` .

Gerçek çıkış, çağırarak tetiklenir `yield return output.AsReadOnly();` .

Kullanıcı tanımlı uygulayıcı parametreleri oluşturucuya geçirilebilir. Uygulayıcı, Base U-SQL betiğinde uygulayıcı çağrısı sırasında tanımlanması gereken sayıda sütun döndürebilir.

```csharp
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

Kullanıcı tanımlı uygulayıcı örneği aşağıda verilmiştir:

```csharp
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
private string parsingPart;

public ParserApplier(string parsingPart)
{
    if (parsingPart.ToUpper().Contains("ALL")
    || parsingPart.ToUpper().Contains("MAKE")
    || parsingPart.ToUpper().Contains("MODEL")
    || parsingPart.ToUpper().Contains("YEAR")
    || parsingPart.ToUpper().Contains("TYPE")
    || parsingPart.ToUpper().Contains("MILLAGE")
    )
    {
    this.parsingPart = parsingPart;
    }
    else
    {
    throw new ArgumentException("Incorrect parameter. Please use: 'ALL[MAKE|MODEL|TYPE|MILLAGE]'");
    }
}

public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
{

    string[] properties = input.Get<string>("properties").Split(',');

    //  only process with correct number of properties
    if (properties.Count() == 5)
    {

    string make = properties[0];
    string model = properties[1];
    string year = properties[2];
    string type = properties[3];
    int millage = -1;

    // Only return millage if it is number, otherwise, -1
    if (!int.TryParse(properties[4], out millage))
    {
        millage = -1;
    }

    if (parsingPart.ToUpper().Contains("MAKE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("make", make);
    if (parsingPart.ToUpper().Contains("MODEL") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("model", model);
    if (parsingPart.ToUpper().Contains("YEAR") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("year", year);
    if (parsingPart.ToUpper().Contains("TYPE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("type", type);
    if (parsingPart.ToUpper().Contains("MILLAGE") || parsingPart.ToUpper().Contains("ALL")) output.Set<int>("millage", millage);
    }
    yield return output.AsReadOnly();            
}
}
```

Aşağıda, bu kullanıcı tanımlı Uygulayıcı için temel U-SQL betiği verilmiştir:

```usql
DECLARE @input_file string = @"c:\usql-programmability\car_fleet.tsv";
DECLARE @output_file string = @"c:\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        stocknumber int,
        vin String,
        properties String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        r.stocknumber,
        r.vin,
        properties.make,
        properties.model,
        properties.year,
        properties.type,
        properties.millage
    FROM @rs0 AS r
    CROSS APPLY
    new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

Bu kullanım örneği senaryosunda, Kullanıcı tanımlı Uygulayıcı, otomobil özellikleri için virgülle ayrılmış bir değer ayrıştırıcısı işlevi görür. Giriş dosyası satırları aşağıdaki gibi görünür:

```text
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Chevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

Bu, oluşturma ve model gibi araba özelliklerini içeren bir özellikler sütunuyla sekmeyle ayrılmış tipik bir TSV dosyasıdır. Bu özellikler tablo sütunlarına ayrıştırılmalıdır. Ayrıca, verilen Uygulayıcı, sonuç satır kümesinde geçirilen parametreye göre dinamik bir dizi özellik oluşturmanıza olanak sağlar. Tüm özellikleri ya da yalnızca belirli bir özellikler kümesini oluşturabilirsiniz.

```text
...USQL_Programmability.ParserApplier ("all")
...USQL_Programmability.ParserApplier ("make")
...USQL_Programmability.ParserApplier ("make&model")
```

Kullanıcı tanımlı Uygulayıcı, uygulayıcı nesnesinin yeni bir örneği olarak çağrılabilir:

```usql
CROSS APPLY new MyNameSpace.MyApplier (parameter: "value") AS alias([columns types]…);
```

Ya da sarmalayıcı fabrikası yönteminin çağrılması:

```csharp
    CROSS APPLY MyNameSpace.MyApplier (parameter: "value") AS alias([columns types]…);
```


## <a name="next-steps"></a>Sonraki adımlar
* [U-SQL programlama kılavuzu-genel bakış](data-lake-analytics-u-sql-programmability-guide.md)
* [U-SQL programlama kılavuzu-UDT ve UDADGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)