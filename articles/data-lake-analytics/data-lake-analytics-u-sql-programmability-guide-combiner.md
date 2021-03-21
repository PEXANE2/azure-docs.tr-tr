---
title: U-SQL Kullanıcı tanımlı birleştirici Azure Data Lake için Programlama Kılavuzu
description: U-SQL UıDO Programlama Kılavuzu-Kullanıcı tanımlı birleştirici hakkında bilgi edinin.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: a6c560cf4ec11197183711656d69024591e7008c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96512694"
---
# <a name="use-user-defined-combiner"></a>Kullanıcı tarafından tanımlanan birleştiriciyi kullanma

## <a name="u-sql-udo-user-defined-combiner"></a>U-SQL UDO: Kullanıcı tanımlı birleştirici
Kullanıcı tanımlı Birleştirici veya UDC, satırları sol ve sağ satır kümelerine özel mantığa göre birleştirmenizi sağlar. Kullanıcı tanımlı birleştirici, BIRLEŞTIRME ifadesiyle birlikte kullanılır.

## <a name="how-to-define-and-use-user-defined-combiner"></a>Kullanıcı tanımlı birleştirici tanımlama ve kullanma

Bir birleştirici, hem giriş satır kümeleri, gruplama sütunları, beklenen sonuç şeması ve ek bilgiler hakkında gerekli bilgileri sağlayan BIRLEŞTIRME ifadesiyle çağrılır.

Bir temel U-SQL betiğinde bir birleştirici çağırmak için aşağıdaki sözdizimini kullanıyoruz:

```usql
Combine_Expression :=
    'COMBINE' Combine_Input
    'WITH' Combine_Input
    Join_On_Clause
    Produce_Clause
    [Readonly_Clause]
    [Required_Clause]
    USING_Clause.
```

Daha fazla bilgi için bkz. [birleştirme ifadesi (U-SQL)](/u-sql/statements-and-expressions/combine-expression).

Kullanıcı tanımlı bir birleştirici tanımlamak için `ICombiner` arabirimi, `SqlUserDefinedCombiner` Kullanıcı tanımlı birleştirici tanımı için isteğe bağlı olan [] özniteliğiyle oluşturmanız gerekir.

Temel `ICombiner` sınıf tanımı:

```csharp
public abstract class ICombiner : IUserDefinedOperator
{
protected ICombiner();
public virtual IEnumerable<IRow> Combine(List<IRowset> inputs,
       IUpdatableRow output);
public abstract IEnumerable<IRow> Combine(IRowset left, IRowset right,
       IUpdatableRow output);
}
```

Bir arabirimin özel uygulanması, `ICombiner` `IEnumerable<IRow>` birleştirme geçersiz kılma tanımını içermelidir.

```csharp
[SqlUserDefinedCombiner]
public class MyCombiner : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    …
}
}
```

**SqlUserDefinedCombiner** özniteliği, türün Kullanıcı tanımlı birleştirici olarak kaydedilmesi gerektiğini gösterir. Bu sınıf devralınamaz.

**SqlUserDefinedCombiner** , birleştirici Mode özelliğini tanımlamak için kullanılır. Kullanıcı tanımlı birleştirici tanımı için isteğe bağlı bir özniteliktir.

CombinerMode modu

CombinerMode numaralandırması aşağıdaki değerleri alabilir:

* Tam (0) her çıkış satırı büyük olasılıkla aynı anahtar değeriyle soldan ve sağda tüm giriş satırlarına bağlıdır.

* Sol (1) her çıkış satırı, sol taraftaki tek bir giriş satırına bağlıdır (ve potansiyel olarak aynı anahtar değeri ile sağdaki tüm satırlardan).

* Sağ (2) her çıkış satırı, sağdaki tek bir giriş satırına bağlıdır (ve potansiyel olarak aynı anahtar değeri ile soldan tüm satırlar).

* İç (3) her çıkış satırı, tek bir giriş satırına, aynı değer ile soldan ve sağdan bağlıdır.

Örnek: [ `SqlUserDefinedCombiner(Mode=CombinerMode.Left)` ]


Ana programlama nesneleri şunlardır:

```csharp
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

Giriş satır kümeleri, arabirimin **sol** ve **sağ** `IRowset` türü olarak geçirilir. Her iki satır kümesi de işlenmek üzere numaralandırılmalıdır. Her bir arabirimi yalnızca bir kez numaralandırabilirsiniz, bu nedenle gerekirse bunları listeletmemiz ve önbelleğe almanız gerekiyordu.

Önbelleğe alma amacıyla, bir \<T\> LINQ sorgu yürütmesinin sonucu olarak bir tür bellek yapısı oluşturuyoruz, özellikle de<`IRow`> listeleyin. Anonim veri türü ayrıca numaralandırma sırasında kullanılabilir.

LINQ sorguları hakkında daha fazla bilgi için bkz. [LINQ Sorgularına Giriş (C#)](/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) ve IEnumerable arabirimi hakkında daha fazla bilgi için [IEnumerable \<T\> arabirimi](/dotnet/api/system.collections.generic.ienumerable-1) \<T\> .

Gelen gerçek veri değerlerini almak için `IRowset` arabirimin Get () yöntemini kullanırız `IRow` .

```csharp
mycolumn = row.Get<int>("mycolumn")
```

Tek tek sütun adları, şema yöntemi çağırarak belirlenebilir `IRow` .

```csharp
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Ya da şema sütun adını kullanarak:

```csharp
c# row.Get<int>(row.Schema[0].Name)
```

LINQ ile genel sabit listesi aşağıdaki gibi görünür:

```csharp
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

Her iki satır kümesi numaralandırıldıktan sonra tüm satırlarda döngü ekleyeceğiz. Sol satır kümesindeki her satır için, birleştirici durumunu karşılayan tüm satırları bulacağız.

Çıkış değerlerinin çıkış ile ayarlanması gerekir `IUpdatableRow` .

```csharp
output.Set<int>("mycolumn", mycolumn)
```

Gerçek çıktı, öğesine çağırarak tetiklenir `yield return output.AsReadOnly();` .

Aşağıda bir birleştirici örneği verilmiştir:

```csharp
[SqlUserDefinedCombiner]
public class CombineSales : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    var internetSales =
    (from row in left.Rows
          select new
          {
              ProductKey = row.Get<int>("ProductKey"),
              OrderDateKey = row.Get<int>("OrderDateKey"),
              SalesAmount = row.Get<decimal>("SalesAmount"),
              TaxAmt = row.Get<decimal>("TaxAmt")
          }).ToList();

    var resellerSales =
    (from row in right.Rows
     select new
     {
     ProductKey = row.Get<int>("ProductKey"),
     OrderDateKey = row.Get<int>("OrderDateKey"),
     SalesAmount = row.Get<decimal>("SalesAmount"),
     TaxAmt = row.Get<decimal>("TaxAmt")
     }).ToList();

    foreach (var row_i in internetSales)
    {
    foreach (var row_r in resellerSales)
    {

        if (
        row_i.OrderDateKey > 0
        && row_i.OrderDateKey < row_r.OrderDateKey
        && row_i.OrderDateKey == 20010701
        && (row_r.SalesAmount + row_r.TaxAmt) > 20000)
        {
        output.Set<int>("OrderDateKey", row_i.OrderDateKey);
        output.Set<int>("ProductKey", row_i.ProductKey);
        output.Set<decimal>("Internet_Sales_Amount", row_i.SalesAmount + row_i.TaxAmt);
        output.Set<decimal>("Reseller_Sales_Amount", row_r.SalesAmount + row_r.TaxAmt);
        }

    }
    }
    yield return output.AsReadOnly();
}
}
```

Bu kullanım örneği senaryosunda, perakende için bir analiz raporu oluşturacağız. Amaç, $20.000 'den daha fazla maliyet sunan ve belirli bir zaman çerçevesinde normal satıcıdan daha hızlı satış yapan tüm ürünleri bulmaktır.

Temel U-SQL betiği aşağıda verilmiştir. Bir normal JOIN ve birleştirici arasındaki mantığı karşılaştırabilirsiniz:

```sql
DECLARE @LocalURI string = @"\usql-programmability\";

DECLARE @input_file_internet_sales string = @LocalURI+"FactInternetSales.txt";
DECLARE @input_file_reseller_sales string = @LocalURI+"FactResellerSales.txt";
DECLARE @output_file1 string = @LocalURI+"output_file1.tsv";
DECLARE @output_file2 string = @LocalURI+"output_file2.tsv";

@fact_internet_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    CustomerKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_internet_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@fact_reseller_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    ResellerKey int ,
    EmployeeKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_reseller_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@rs1 =
SELECT
    fis.OrderDateKey,
    fis.ProductKey,
    fis.SalesAmount+fis.TaxAmt AS Internet_Sales_Amount,
    frs.SalesAmount+frs.TaxAmt AS Reseller_Sales_Amount
FROM @fact_internet_sales AS fis
     INNER JOIN @fact_reseller_sales AS frs
     ON fis.ProductKey == frs.ProductKey
WHERE
    fis.OrderDateKey < frs.OrderDateKey
    AND fis.OrderDateKey == 20010701
    AND frs.SalesAmount+frs.TaxAmt > 20000;

@rs2 =
COMBINE @fact_internet_sales AS fis
WITH @fact_reseller_sales AS frs
ON fis.ProductKey == frs.ProductKey
PRODUCE OrderDateKey int,
        ProductKey int,
        Internet_Sales_Amount decimal,
        Reseller_Sales_Amount decimal
USING new USQL_Programmability.CombineSales();

OUTPUT @rs1 TO @output_file1 USING Outputters.Tsv();
OUTPUT @rs2 TO @output_file2 USING Outputters.Tsv();
```

Kullanıcı tanımlı bir birleştirici, uygulayıcı nesnesinin yeni bir örneği olarak çağrılabilir:

```csharp
USING new MyNameSpace.MyCombiner();
```


Ya da sarmalayıcı fabrikası yönteminin çağrılması:

```csharp
USING MyNameSpace.MyCombiner();
```

## <a name="next-steps"></a>Sonraki adımlar
* [U-SQL programlama kılavuzu-genel bakış](data-lake-analytics-u-sql-programmability-guide.md)
* [U-SQL programlama kılavuzu-UDT ve UDADGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)