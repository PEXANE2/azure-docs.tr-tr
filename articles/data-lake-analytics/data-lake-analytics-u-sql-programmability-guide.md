---
title: Azure Veri Gölü için U-SQL programlanabilirlik kılavuzu
description: Azure Veri Gölü Analytics'te bulut tabanlı büyük veri platformu oluşturmanıza olanak tanıyan hizmet kümesi hakkında bilgi edinin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 63be271e-7c44-4d19-9897-c2913ee9599d
ms.topic: conceptual
ms.date: 06/30/2017
ms.openlocfilehash: dc55615d7a5c6ae9a393ed4fd5f49cd92aedc0f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73162586"
---
# <a name="u-sql-programmability-guide"></a>U-SQL programlanabilirlik kılavuzu

U-SQL, büyük veri türü iş yükleri için tasarlanmış bir sorgu dilidir. U-SQL'in benzersiz özelliklerinden biri, C# tarafından sağlanan genişletilebilirlik ve programlanabilirlik ile SQL benzeri bildirimdilinin birleşimidir. Bu kılavuzda, C# tarafından etkinleştirilen U-SQL dilinin genişletilebilirliğine ve programlanabilirliğine odaklanırız.

## <a name="requirements"></a>Gereksinimler

[Visual Studio için Azure Veri Gölü Araçlarını](https://www.microsoft.com/download/details.aspx?id=49504)indirin ve yükleyin.

## <a name="get-started-with-u-sql"></a>U-SQL kullanmaya başlama  

Aşağıdaki U-SQL komut dosyasına bakın:

```
@a  = 
  SELECT * FROM 
    (VALUES
       ("Contoso",   1500.0, "2017-03-39"),
       ("Woodgrove", 2700.0, "2017-04-10")
    ) AS D( customer, amount, date );

@results =
  SELECT
    customer,
    amount,
    date
  FROM @a;    
```

Bu komut dosyası iki `@a` Satır `@results`Kümesini tanımlar: ve . RowSet `@results` `@a`tanımlanır.

## <a name="c-types-and-expressions-in-u-sql-script"></a>U-SQL komut dosyasında C# türleri ve ifadeleri

U-SQL İfadesi, U-SQL mantıksal işlemleri `AND` `OR`ile birlikte `NOT`bir C# ifadesidir. U-SQL İfadeleri SELECT, EXTRACT, WHERE, HAVING, GROUP BY ve DECLARE ile kullanılabilir. Örneğin, aşağıdaki komut dosyası bir dizeyi DateTime değeri olarak parslar.

```
@results =
  SELECT
    customer,
    amount,
    DateTime.Parse(date) AS date
  FROM @a;    
```

Aşağıdaki parçacık, DECLARE deyiminde datetime değeri olarak bir dize parses.

```
DECLARE @d = DateTime.Parse("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a>Veri türü dönüştürmeleri için C# ifadelerini kullanma

Aşağıdaki örnek, C# ifadelerini kullanarak tarih zamanı veri dönüştürmesini nasıl yapabileceğinizi göstermektedir. Bu özel senaryoda, string datetime verileri gece yarısı 00:00:00 saat gösterimi ile standart datetime dönüştürülür.

```
DECLARE @dt = "2016-07-06 10:23:15";

@rs1 =
  SELECT 
    Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
    dt AS olddt
  FROM @rs0;

OUTPUT @rs1 
  TO @output_file 
  USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a>Bugünün tarihi için C# ifadelerini kullanma

Bugünün tarihini çekmek için aşağıdaki C# ifadesini kullanabiliriz:`DateTime.Now.ToString("M/d/yyyy")`

Bu ifadenin komut dosyasında nasıl kullanılacağına bir örnek aşağıda verilmiştir:

```
@rs1 =
  SELECT
    MAX(guid) AS start_id,
    MIN(dt) AS start_time,
    MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
    MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
    DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
    user,
    des
  FROM @rs0
  GROUP BY user, des;
```
## <a name="using-net-assemblies"></a>.NET montajlarını kullanma

U-SQL'in genişletilebilirlik modeli, .NET derlemelerinden özel kod ekleme yeteneğine büyük ölçüde dayanır. 

### <a name="register-a-net-assembly"></a>Bir .NET derlemesi kaydedin

Bir `CREATE ASSEMBLY` U-SQL Veritabanına .NET derlemesi yerleştirmek için deyimi kullanın. Daha sonra, U-SQL komut dosyaları `REFERENCE ASSEMBLY` deyimini kullanarak bu derlemeleri kullanabilir. 

Aşağıdaki kod, bir derlemenin nasıl kaydedilebildiğini gösterir:

```
CREATE ASSEMBLY MyDB.[MyAssembly]
   FROM "/myassembly.dll";
```

Aşağıdaki kod, bir derlemeye nasıl başvurulsüreceğini gösterir:

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

Bu konuyu daha ayrıntılı olarak kapsayan [montaj kayıt yönergelerine](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) başvurun.


### <a name="use-assembly-versioning"></a>Montaj sürümünü kullanma
Şu anda U-SQL .NET Framework sürüm 4.5'i kullansın. Bu nedenle, kendi derlemelerinizin çalışma zamanının bu sürümüyle uyumlu olduğundan emin olun.

Daha önce de belirtildiği gibi, U-SQL 64 bit (x64) biçiminde kod çalışır. Bu nedenle kodunuzu x64 üzerinde çalışacak şekilde derlenmiş olduğundan emin olun. Aksi takdirde daha önce gösterilen yanlış biçim hatası olsun.

Farklı bir çalışma zamanı, yerel derleme veya config dosyası gibi yüklenen her yükleme li DLL ve kaynak dosyası en fazla 400 MB olabilir. Dağıtılan kaynakların toplam boyutu, YA DEPLOY RESOURCE aracılığıyla veya derlemelere ve ek dosyalarına yapılan başvurular yoluyla, 3 GB'ı geçemez.

Son olarak, her U-SQL veritabanının belirli bir derlemenin yalnızca bir sürümünü içerebileceğini unutmayın. Örneğin, NewtonSoft Json.NET kitaplığı sürüm 7 ve sürüm 8'e ihtiyacınız varsa, bunları iki farklı veritabanına kaydetmeniz gerekir. Ayrıca, her komut dosyası yalnızca belirli bir derleme DLL bir sürümü ne başvurulabilir. Bu bağlamda, U-SQL C# montaj yönetimini ve semantik lerini sürüm den izler.

## <a name="use-user-defined-functions-udf"></a>Kullanıcı tanımlı işlevleri kullanın: UDF
U-SQL kullanıcı tanımlı işlevler veya UDF, parametreleri kabul eden, bir eylem gerçekleştiren (karmaşık bir hesaplama gibi) ve bu eylemin sonucunu bir değer olarak döndüren programlama yordamlarıdır. UDF'nin geri dönüş değeri yalnızca tek bir skaler olabilir. U-SQL UDF, diğer C# skaler işlevleri gibi U-SQL baz komut dosyasında çağrılabilir.

U-SQL kullanıcı tanımlı işlevleri **nigenel** ve **statik**olarak başlatmanızı öneririz.

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

Önce bir UDF oluşturma basit bir örnek bakalım.

Bu kullanım durumu senaryosunda, belirli bir kullanıcı için ilk oturum açmanın mali üç aylık ve mali ayı da dahil olmak üzere mali dönemi belirlememiz gerekir. Senaryomuzda yılın ilk mali ayı Haziran'dır.

Mali dönemi hesaplamak için aşağıdaki C# fonksiyonunu tanıtıyoruz:

```
public static string GetFiscalPeriod(DateTime dt)
{
    int FiscalMonth=0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter=0;
    if (FiscalMonth >=1 && FiscalMonth<=3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return "Q" + FiscalQuarter.ToString() + ":P" + FiscalMonth.ToString();
}
```

Yalnızca mali ayı ve üç aylık dönemi hesaplar ve bir dize değeri döndürür. İlk mali çeyreğin ilk ayı olan Haziran ayında "Q1:P1"i kullanırız. Temmuz için ,biz "Q1:P2" ve benzeri kullanın.

Bu, U-SQL projemizde kullanacağımız normal bir C# fonksiyonudur.

Kod arkası bölümü bu senaryoda şu şekilde görünür:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        public static string GetFiscalPeriod(DateTime dt)
        {
            int FiscalMonth=0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter=0;
            if (FiscalMonth >=1 && FiscalMonth<=3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return "Q" + FiscalQuarter.ToString() + ":" + FiscalMonth.ToString();
        }
    }
}
```

Şimdi bu işlevi temel U-SQL komut dosyasından adlandıracağız. Bunu yapmak için, bu durumda NameSpace.Class.Function (parametre) olan ad alanı da dahil olmak üzere işlev için tam nitelikli bir ad sağlamamız gerekir.

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

Aşağıdaki gerçek U-SQL temel komut dosyası:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt DateTime,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");

@rs1 =
    SELECT
        MAX(guid) AS start_id,
    MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

Komut dosyası yürütme çıktı dosyası aşağıdadır:

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

Bu örnek, U-SQL'de satır içi UDF'nin basit bir kullanımını göstermektedir.

### <a name="keep-state-between-udf-invocations"></a>UDF çağrıları arasında durumu koruma
U-SQL C# programlanabilirlik nesneleri, kod arkasındaki global değişkenler aracılığıyla etkileşimi kullanarak daha karmaşık olabilir. Aşağıdaki iş kullanım örneği senaryosuna bakalım.

Büyük kuruluşlarda, kullanıcılar dahili uygulama çeşitleri arasında geçiş yapabilir. Bunlar Microsoft Dynamics CRM, PowerBI ve benzeri içerir. Müşteriler, kullanıcıların farklı uygulamalar arasında nasıl geçiş yaptıklarına, kullanım eğilimlerinin ne olduğuna ve benzeri konularda bir telemetri çözümlemesi uygulamak isteyebilir. İşletmenin amacı uygulama kullanımını optimize etmektir. Ayrıca, farklı uygulamaları veya belirli oturum açma yordamlarını birleştirmek isteyebilirler.

Bu amaca ulaşmak için, gerçekleşen son oturum arasındaki oturum adlarını ve gecikme süresini belirlememiz gerekir.

Önceki oturum açma yı bulmamız ve ardından bu oturuma aynı uygulamaya oluşturulan tüm oturumlara atamamız gerekir. İlk zorluk, U-SQL temel komut dosyasının LAG işlevine sahip zaten hesaplanmış sütunlar üzerinde hesaplamalar uygulamamıza izin vermemesidir. İkinci zorluk, tüm seanslar için belirli oturumu aynı zaman dilimi içinde tutmamız gerektiğidir.

Bu sorunu çözmek için, kod arkası bölümünde genel `static public string globalSession;`bir değişken kullanırız: .

Bu genel değişken, komut dosyası yürütmemiz sırasında tüm satır kümesine uygulanır.

U-SQL programımızın kod arkası bölümü aşağıdaverevettir:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQLApplication21
{
    public class UserSession
    {
        static public string globalSession;
        static public string StampUserSession(string eventTime, string PreviousRow, string Session)
        {

            if (!string.IsNullOrEmpty(PreviousRow))
            {
                double timeGap = Convert.ToDateTime(eventTime).Subtract(Convert.ToDateTime(PreviousRow)).TotalMinutes;
                if (timeGap <= 60) {return Session;}
                else {return Guid.NewGuid().ToString();}
            }
            else {return Guid.NewGuid().ToString();}

        }

        static public string getStampUserSession(string Session)
        {
            if (Session != globalSession && !string.IsNullOrEmpty(Session)) { globalSession = Session; }
            return globalSession;
        }

    }
}
```

Bu örnek, `static public string globalSession;` `getStampUserSession` işlev içinde kullanılan genel değişkeni gösterir ve Oturum parametresi her değiştirilince yeniden başharfe dönüştürülebilir.

U-SQL temel komut dosyası aşağıdaki gibidir:

```
DECLARE @in string = @"\UserSession\test1.tsv";
DECLARE @out1 string = @"\UserSession\Out1.csv";
DECLARE @out2 string = @"\UserSession\Out2.csv";
DECLARE @out3 string = @"\UserSession\Out3.csv";

@records =
    EXTRACT DataId string,
            EventDateTime string,           
            UserName string,
            UserSessionTimestamp string

    FROM @in
    USING Extractors.Tsv();

@rs1 =
    SELECT 
        EventDateTime,
        UserName,
    LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,          
        string.IsNullOrEmpty(LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,           
        USQLApplication21.UserSession.StampUserSession
           (
            EventDateTime,
            LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC),
            LAG(UserSessionTimestamp, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)
           ) AS UserSessionTimestamp
    FROM @records;

@rs2 =
    SELECT 
        EventDateTime,
        UserName,
        LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,
        string.IsNullOrEmpty( LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,
        USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp) AS UserSessionTimestamp
    FROM @rs1
    WHERE UserName != "UserName";

OUTPUT @rs2
    TO @out2
    ORDER BY UserName, EventDateTime ASC
    USING Outputters.Csv();
```

İşlev `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` burada ikinci bellek rowset hesaplama sırasında denir. Sütundan `UserSessionTimestamp` geçer ve değiştirilene `UserSessionTimestamp` kadar değeri döndürür.

Çıktı dosyası aşağıdaki gibidir:

```
"2016-02-19T07:32:36.8420000-08:00","User1",,True,"72a0660e-22df-428e-b672-e0977007177f"
"2016-02-17T11:52:43.6350000-08:00","User2",,True,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-17T11:59:08.8320000-08:00","User2","2016-02-17T11:52:43.6350000-08:00",False,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-11T07:04:17.2630000-08:00","User3",,True,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-11T07:10:33.9720000-08:00","User3","2016-02-11T07:04:17.2630000-08:00",False,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-15T21:27:41.8210000-08:00","User3","2016-02-11T07:10:33.9720000-08:00",False,"4d2bc48d-bdf3-4591-a9c1-7b15ceb8e074"
"2016-02-16T05:48:49.6360000-08:00","User3","2016-02-15T21:27:41.8210000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-16T06:22:43.6390000-08:00","User3","2016-02-16T05:48:49.6360000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-17T16:29:53.2280000-08:00","User3","2016-02-16T06:22:43.6390000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T16:39:07.2430000-08:00","User3","2016-02-17T16:29:53.2280000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T17:20:39.3220000-08:00","User3","2016-02-17T16:39:07.2430000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-19T05:23:54.5710000-08:00","User3","2016-02-17T17:20:39.3220000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T05:48:37.7510000-08:00","User3","2016-02-19T05:23:54.5710000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T06:40:27.4830000-08:00","User3","2016-02-19T05:48:37.7510000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T07:27:37.7550000-08:00","User3","2016-02-19T06:40:27.4830000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T19:35:40.9450000-08:00","User3","2016-02-19T07:27:37.7550000-08:00",False,"3f385f0b-3e68-4456-ac74-ff6cef093674"
"2016-02-20T00:07:37.8250000-08:00","User3","2016-02-19T19:35:40.9450000-08:00",False,"685f76d5-ca48-4c58-b77d-bd3a9ddb33da"
"2016-02-11T09:01:33.9720000-08:00","User4",,True,"9f0cf696-c8ba-449a-8d5f-1ca6ed8f2ee8"
"2016-02-17T06:30:38.6210000-08:00","User4","2016-02-11T09:01:33.9720000-08:00",False,"8b11fd2a-01bf-4a5e-a9af-3c92c4e4382a"
"2016-02-17T22:15:26.4020000-08:00","User4","2016-02-17T06:30:38.6210000-08:00",False,"4e1cb707-3b5f-49c1-90c7-9b33b86ca1f4"
"2016-02-18T14:37:27.6560000-08:00","User4","2016-02-17T22:15:26.4020000-08:00",False,"f4e44400-e837-40ed-8dfd-2ea264d4e338"
"2016-02-19T01:20:31.4800000-08:00","User4","2016-02-18T14:37:27.6560000-08:00",False,"2136f4cf-7c7d-43c1-8ae2-08f4ad6a6e08"
```

Bu örnek, tüm bellek satır kümesine uygulanan kod arkası bölümünde genel bir değişken kullandığımız daha karmaşık bir kullanım örneği senaryosunu göstermektedir.

## <a name="use-user-defined-types-udt"></a>Kullanıcı tanımlı türleri kullanın: UDT
Kullanıcı tanımlı türleri veya UDT, U-SQL'in başka bir programlanabilirlik özelliğidir. U-SQL UDT normal bir C# kullanıcı tanımlı türü gibi davranır. C# yerleşik ve özel kullanıcı tanımlı türlerin kullanımına izin veren güçlü bir şekilde yazılan bir dildir.

U-SQL, UDT satır kümeslerinde vertices arasında geçirildiğinde rasgele UDT'leri dolaylı olarak serileştiremez veya seriselleştiremez. Bu, kullanıcının IFormatter arabirimini kullanarak açık bir formatter sağlaması gerektiği anlamına gelir. Bu, UDT için serileştirme ve deserialize yöntemleri ile U-SQL sağlar.

> [!NOTE]
> U-SQL'in yerleşik çıkarıcıları ve çıktıları şu anda UDT verilerini IFormatter kümesinde bile dosyalara veya dosyalardan serihale edemez veya serileştiremez. Bu nedenle, OUTPUT deyimi içeren bir dosyaya UDT verilerini yazarken veya bir çıkarıcıyla okurken, bir dize veya bayt dizisi olarak geçirmeniz gerekir. Sonra serileştirme ve deserialization kodu (yani, UDT's ToString() yöntemi) açıkça diyoruz. Kullanıcı tanımlı çıkarıcılar ve çıktılar, diğer taraftan, okuyabilir ve UDTs yazabilirsiniz.

BURADA gösterildiği gibi, EXTRACTOR veya OUTPUTTER 'de (önceki SELECT dışında) UDT kullanmaya çalışırsak:

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

Aşağıdaki hatayı alıyoruz:

```
Error   1   E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used to output column myfield of type
MyNameSpace.Myfunction_Returning_UDT.

Description:

Outputters.Text only supports built-in types.

Resolution:

Implement a custom outputter that knows how to serialize this type, or call a serialization method on the type in
the preceding SELECT.   C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
USQL-Programmability\Types.usql 52  1   USQL-Programmability
```

Outputter udt ile çalışmak için, ya ToString() yöntemi ile dize serileştirmek veya özel bir çıktı oluşturabilirsiniz.

ŞU ANDA GRUP BY'de UDT'ler kullanılamaz. GROUP BY'de UDT kullanılıyorsa, aşağıdaki hata atılır:

```
Error   1   E_CSC_USER_INVALIDTYPEINCLAUSE: GROUP BY doesn't support type MyNameSpace.Myfunction_Returning_UDT
for column myfield

Description:

GROUP BY doesn't support UDT or Complex types.

Resolution:

Add a SELECT statement where you can project a scalar column that you want to use with GROUP BY.
C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\USQL-Programmability\Types.usql
62  5   USQL-Programmability
```

Bir UDT tanımlamak için şunları yapmamız gerekir:

* Aşağıdaki ad alanlarını ekleyin:

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* UDT `Microsoft.Analytics.Interfaces`arabirimleri için gerekli olan ekle. Buna ek `System.IO` olarak, IFormatter arabirimini tanımlamak için gerekli olabilir.

* SqlUserDefinedType özniteliği ile kullanılmış tanımlı bir tür tanımlayın.

**SqlUserDefinedType,** bir derlemedeki bir tür tanımını U-SQL'de kullanıcı tanımlı tür (UDT) olarak işaretlemek için kullanılır. Öznitelikteki özellikler UDT'nin fiziksel özelliklerini yansıtır. Bu sınıf devralınamaz.

SqlUserDefinedType UDT tanımı için gerekli bir özniteliktir.

Sınıfın oluşturucusu:  

* SqlUserDefinedTypeAttribute (type formatter)

* Formatter yazın: Bir UDT formatter tanımlamak için gerekli parametre - özellikle, `IFormatter` arabirim türü burada geçirilmelidir.

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* Tipik UDT, aşağıdaki örnekte gösterildiği gibi IFormatter arabiriminin tanımını da gerektirir:

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

Arabirim, `IFormatter` \<daktinomref adı="T" > kök türüne sahip bir nesne grafiğini serihale eder ve seri olarak devre dışı eder.

\<typeparam adı="T">Nesne grafiğinin serihale ve seriselleştirmesi için kök türü.

* **Deserialize**: Sağlanan akıştaki verileri deserialize eder ve nesnelerin grafiğini yeniden oluşturur.

* Serialize : Verilen kökle birlikte bir nesneyi veya nesnelerin grafiğini, sağlanan akışa **serileştirir.**

`MyType`örnek: Türü örneği.  
`IColumnWriter`yazar `IColumnReader` / okuyucu: Altta yatan sütun akışı.  
`ISerializationContext`bağlam: Serileştirme sırasında akışın kaynağını veya hedef bağlamını belirten bir bayrak kümesitanımlayan enum.

* **Orta**: Kaynak veya hedef bağlamın kalıcı bir depo olmadığını belirtir.

* **Kalıcılık**: Kaynak veya hedef bağlamın kalıcı bir depo olduğunu belirtir.

Normal bir C# türü olarak, U-SQL UDT tanımı +/======== gibi işleçler için geçersiz kılmalar içerebilir. Statik yöntemleri de içerebilir. Örneğin, bu UDT'yi U-SQL MIN toplam işlevine parametre olarak kullanacaksak, < işleci geçersiz kılmayı tanımlamamız gerekir.

Daha önce bu kılavuzda, biçimdeki `Qn:Pn (Q1:P10)`belirli tarihten mali dönem tanımlaması için bir örnek gösterdik. Aşağıdaki örnek, mali dönem değerleri için özel bir türün nasıl tanımlandığını gösterir.

Aşağıda özel UDT ve IFormatter arabirimi ile bir kod arkası bölümü bir örnektir:

```
[SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
public struct FiscalPeriod
{
    public int Quarter { get; private set; }

    public int Month { get; private set; }

    public FiscalPeriod(int quarter, int month):this()
    {
    this.Quarter = quarter;
    this.Month = month;
    }

    public override bool Equals(object obj)
    {
    if (ReferenceEquals(null, obj))
    {
        return false;
    }

    return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
    }

    public bool Equals(FiscalPeriod other)
    {
return this.Quarter.Equals(other.Quarter) && this.Month.Equals(other.Month);
    }

    public bool GreaterThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
    }

    public bool LessThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
    }

    public override int GetHashCode()
    {
    unchecked
    {
        return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
    }
    }

    public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
    {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
    }

    public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.Equals(c2);
    }

    public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
    {
    return !c1.Equals(c2);
    }
    public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.GreaterThan(c2);
    }
    public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.LessThan(c2);
    }
    public override string ToString()
    {
    return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
    }

}

public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
{
    public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
    {
    using (var binaryWriter = new BinaryWriter(writer.BaseStream))
    {
        binaryWriter.Write(instance.Quarter);
        binaryWriter.Write(instance.Month);
        binaryWriter.Flush();
    }
    }

    public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
    {
    using (var binaryReader = new BinaryReader(reader.BaseStream))
    {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
        return result;
    }
    }
}
```

Tanımlanan tür iki sayı içerir: üç aylık ve ay. Operatörler `==/!=/>/<` ve `ToString()` statik yöntem burada tanımlanır.

Daha önce de belirtildiği gibi, UDT SELECT ifadelerinde kullanılabilir, ancak özel serileştirme olmadan OUTPUTTER/EXTRACTOR'da kullanılamaz. Ya özel bir OUTPUTTER /EXTRACTOR ile `ToString()` bir dize olarak serihale veya kullanılması gerekir.

Şimdi UDT kullanımını tartışalım. Kod arkası bölümünde, GetFiscalPeriod işlevimizi aşağıdaki şekilde değiştirdik:

```
public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
{
    int FiscalMonth = 0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter = 0;
    if (FiscalMonth >= 1 && FiscalMonth <= 3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return new FiscalPeriod(FiscalQuarter, FiscalMonth);
}
```

Gördüğünüz gibi, Mali Dönem türümüzün değerini döndürür.

Burada u-SQL temel komut dosyasında daha fazla nasıl kullanılacağına bir örnek salıyoruz. Bu örnek, U-SQL komut dosyasından farklı UDT çağırma biçimlerini gösterir.

```
DECLARE @input_file string = @"c:\work\cosmos\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"c:\work\cosmos\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid string,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT 
        guid AS start_id,
        dt,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Quarter AS fiscalquarter,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Month AS fiscalmonth,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt) + new USQL_Programmability.CustomFunctions.FiscalPeriod(1,7) AS fiscalperiod_adjusted,
        user,
        des
    FROM @rs0;

@rs2 =
    SELECT 
           start_id,
           dt,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           fiscalquarter,
           fiscalmonth,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS fiscalperiod,

       // This user-defined type was created in the prior SELECT.  Passing the UDT to this subsequent SELECT would have failed if the UDT was not annotated with an IFormatter.
           fiscalperiod_adjusted.ToString() AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    TO @output_file 
    USING Outputters.Text();
```

Burada tam bir kod arkası bölümüne bir örnek verilmiştir:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.IO;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        static public DateTime? ToDateTime(string dt)
        {
            DateTime dtValue;

            if (!DateTime.TryParse(dt, out dtValue))
                return Convert.ToDateTime(dt);
            else
                return null;
        }

        public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
        {
            int FiscalMonth = 0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter = 0;
            if (FiscalMonth >= 1 && FiscalMonth <= 3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return new FiscalPeriod(FiscalQuarter, FiscalMonth);
        }



        [SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
        public struct FiscalPeriod
        {
            public int Quarter { get; private set; }

            public int Month { get; private set; }

            public FiscalPeriod(int quarter, int month):this()
            {
                this.Quarter = quarter;
                this.Month = month;
            }

            public override bool Equals(object obj)
            {
                if (ReferenceEquals(null, obj))
                {
                    return false;
                }

                return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
            }

            public bool Equals(FiscalPeriod other)
            {
return this.Quarter.Equals(other.Quarter) &&    this.Month.Equals(other.Month);
            }

            public bool GreaterThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
            }

            public bool LessThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
            }

            public override int GetHashCode()
            {
                unchecked
                {
                    return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
                }
            }

            public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
            {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
            }

            public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.Equals(c2);
            }

            public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
            {
                return !c1.Equals(c2);
            }
            public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.GreaterThan(c2);
            }
            public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.LessThan(c2);
            }
            public override string ToString()
            {
                return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
            }

        }

        public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
        {
public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
            {
                using (var binaryWriter = new BinaryWriter(writer.BaseStream))
                {
                    binaryWriter.Write(instance.Quarter);
                    binaryWriter.Write(instance.Month);
                    binaryWriter.Flush();
                }
            }

public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
            {
                using (var binaryReader = new BinaryReader(reader.BaseStream))
                {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
                    return result;
                }
            }
        }
    }
}
```

## <a name="use-user-defined-aggregates-udagg"></a>Kullanıcı tanımlı agregalar kullanın: UDAGG
Kullanıcı tanımlı agregalar, U-SQL ile kutudan dışarı gönderilmemiş toplama yla ilgili işlevlerdir. Örnek, özel matematik hesaplamaları, dize concatenations, dizeleri ile manipülasyonlar ve benzeri gerçekleştirmek için bir toplam olabilir.

Kullanıcı tanımlı toplam taban sınıf tanımı aşağıdaki gibidir:

```csharp
    [SqlUserDefinedAggregate]
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
    {
        protected IAggregate();

        public abstract void Accumulate(T1 t1, T2 t2);
        public abstract void Init();
        public abstract TResult Terminate();
    }
```

**SqlUserDefinedAggregate,** türün kullanıcı tanımlı bir toplam olarak kaydedilmesi gerektiğini belirtir. Bu sınıf devralınamaz.

SqlUserDefinedType özniteliği UDAGG tanımı için **isteğe bağlıdır.**


Taban sınıf üç soyut parametre yi geçirmenizi sağlar: iki giriş parametreleri ve bir sonuç olarak. Veri türleri değişkendir ve sınıf kalıtım sırasında tanımlanmalıdır.

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    { … }

    public override void Accumulate(string guid, string user)
    { … }

    public override string Terminate()
    { … }
}
```

* **Init,** hesaplama sırasında her grup için bir kez çağrır. Her toplama grubu için bir başlatma yordamı sağlar.  
* **Birikmesi** her değer için bir kez yürütülür. Toplama algoritması için ana işlevselliği sağlar. Sınıf devralma sırasında tanımlanan çeşitli veri türleri ile değerleri toplamak için kullanılabilir. Değişken veri türlerinin iki parametresini kabul edebilir.
* **Sonlandırma,** her grup için sonucu çıktıetmek için işleme sonunda toplama grubu başına bir kez yürütülür.

Doğru giriş ve çıktı veri türlerini bildirmek için sınıf tanımını aşağıdaki gibi kullanın:

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* T1: Biriken ilk parametre
* T2: Birikecek ikinci parametre
* TResult: Sonlandırma nın dönüş türü

Örnek:

```
public class GuidAggregate : IAggregate<string, int, int>
```

or

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a>U-SQL'de UDAGG'ı kullanma
UDAGG'ı kullanmak için, önce kod arkasında tanımlayın veya daha önce tartışıldığı gibi varolan programlanabilirlik DLL'den başvurun.

Ardından aşağıdaki sözdizimini kullanın:

```
AGG<UDAGG_functionname>(param1,param2)
```

İşte UDAGG bir örnektir:

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    {
        guid_agg = "";
    }

    public override void Accumulate(string guid, string user)
    {
        if (user.ToUpper()== "USER1")
        {
        guid_agg += "{" + guid + "}";
        }
    }

    public override string Terminate()
    {
        return guid_agg;
    }

}
```

Ve temel U-SQL komut dosyası:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @" \usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    SELECT
        user,
        AGG<USQL_Programmability.GuidAggregate>(guid,user) AS guid_list
    FROM @rs0
    GROUP BY user;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

Bu kullanım örneği senaryosunda, belirli kullanıcılar için sınıf GUID'leri birleştiririz.

## <a name="use-user-defined-objects-udo"></a>Kullanıcı tanımlı nesneleri kullanma: UDO
U-SQL, kullanıcı tanımlı nesneler veya UDO olarak adlandırılan özel programlanabilirlik nesneleri tanımlamanızı sağlar.

U-SQL'de UDO listesi aşağıdavevettir:

* Kullanıcı tanımlı çıkarıcılar
    * Satır satır ayıklama
    * Özel yapılandırılmış dosyalardan veri çıkarma uygulamak için kullanılır

* Kullanıcı tanımlı çıktılar
    * Satır satır çıktı
    * Özel veri türlerini veya özel dosya biçimlerini çıktılamak için kullanılır

* Kullanıcı tanımlı işlemciler
    * Bir satır alın ve bir satır üretin
    * Sütun sayısını azaltmak veya varolan bir sütun kümesinden türetilen değerlerle yeni sütunlar üretmek için kullanılır

* Kullanıcı tanımlı değerlendiriciler
    * Bir satır alın ve 0'dan n satıra kadar
    * DıŞ/ÇAPRAZ UYGULAMA ile kullanılır

* Kullanıcı tanımlı kombinler
    * Satır kümelerini birleştirir-- kullanıcı tanımlı JO'ları

* Kullanıcı tanımlı azaltıcılar
    * N satırlarını alın ve bir satır üretin
    * Satır sayısını azaltmak için kullanılır

UDO genellikle u-SQL komut dosyasında aşağıdaki U-SQL deyimlerinin bir parçası olarak açıkça çağrılır:

* AYIKLA
* Çıkış
* Işlem
* Birleştirmek
* Azaltmak

> [!NOTE]  
> UDO'lar 0,5 Gb bellek tüketmekle sınırlıdır.  Bu bellek sınırlaması yerel yürütmeler için geçerli değildir.

## <a name="use-user-defined-extractors"></a>Kullanıcı tanımlı çıkarıcılar kullanma
U-SQL, EXTRACT deyimi ni kullanarak dış veri almanızı sağlar. Bir EXTRACT deyimi yerleşik UDO çıkarıcılar kullanabilirsiniz:  

* *Extractors.Text()*: Farklı kodlamaların sınırlı metin dosyalarından çıkarma sağlar.

* *Extractors.Csv()*: Farklı kodlamaların virgülle ayrılmış değer (CSV) dosyalarından çıkarma sağlar.

* *Extractors.Tsv()*: Farklı kodlamaların sekme ayrılmış değer (TSV) dosyalarından çıkarma sağlar.

Özel bir çıkarıcı geliştirmek yararlı olabilir. Aşağıdaki görevlerden herhangi birini yapmak istersek, veri alma sırasında bu yararlı olabilir:

* Sütunları bölerek ve tek tek değerleri değiştirerek giriş verilerini değiştirin. PROCESSOR işlevi sütunları birleştirmek için daha iyidir.
* Web sayfaları ve e-postalar gibi yapılandırılmamış verileri veya XML/JSON gibi yarı yapılandırılmamış verileri ayrıştırın.
* Desteklenmeyen kodlamada verileri ayrıştın.

Kullanıcı tanımlı bir çıkarıcı veya UDE tanımlamak için `IExtractor` bir arabirim oluşturmamız gerekir. Sütun/satır sınır çözücüler ve kodlama gibi çıkarıcıya tüm giriş parametrelerinin sınıfın oluşturucusu olarak tanımlanması gerekir. Arabirim `IExtractor` de aşağıdaki gibi `IEnumerable<IRow>` geçersiz kılma için bir tanım içermelidir:

```
[SqlUserDefinedExtractor]
public class SampleExtractor : IExtractor
{
     public SampleExtractor(string row_delimiter, char col_delimiter)
     { … }

     public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
     { … }
}
```

**SqlUserDefinedExtractor** özniteliği, türün kullanıcı tanımlı bir çıkarıcı olarak kaydedilmesi gerektiğini belirtir. Bu sınıf devralınamaz.

SqlUserDefinedExtractor UDE tanımı için isteğe bağlı bir özniteliktir. UDE nesnesi için AtomicFileProcessing özelliğini tanımlamak için kullanılır.

* bool AtomicFileProcessing   

* **true** = Bu çıkarıcının atomik giriş dosyaları gerektirdiğini gösterir (JSON, XML, ...)
* **false** = Bu çıkarıcının bölünmüş / dağıtılmış dosyalarla başa çıkabileceğini gösterir (CSV, SEQ, ...)

Ana UDE programlanabilirlik nesneleri **giriş** ve **çıkış**vardır. Giriş nesnesi, giriş verilerini `IUnstructuredReader`. Çıktı nesnesi, ayıklayıcı etkinliğinin bir sonucu olarak çıktı verilerini ayarlamak için kullanılır.

Giriş verilerine ve `System.IO.Stream` . `System.IO.StreamReader`

Giriş sütunları numaralandırmaiçin, önce bir satır delimiter kullanarak giriş akışını bölüşürüz.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

Daha sonra, giriş satırLarını sütun bölümlerine daha fazla bölün.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

Çıktı verilerini ayarlamak için `output.Set` yöntemi kullanırız.

Özel çıkarıcının yalnızca çıktıyla tanımlanan sütunları ve değerleri çıkardığını anlamak önemlidir. Yöntem çağrısını ayarlayın.

```
output.Set<string>(count, part);
```

Gerçek çıkarıcı çıktısı çağırılarak `yield return output.AsReadOnly();`tetiklenir.

Aşağıdaki çıkarıcı örneği:

```
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

Bu kullanım örneği senaryosunda, çıkarıcı GUID'i "guid" sütunu için yeniden oluşturur ve "kullanıcı" sütununun değerlerini büyük harfe dönüştürür. Özel çıkarıcılar, giriş verilerini ayrıştarak ve değiştirerek daha karmaşık sonuçlar üretebilir.

Aşağıda, özel bir çıkarıcı kullanan temel U-SQL komut dosyası vereme vardır:

```
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

## <a name="use-user-defined-outputters"></a>Kullanıcı tanımlı çıktılayıcıları kullanma
Kullanıcı tanımlı outputter, yerleşik U-SQL işlevselliğini genişletmenize olanak tanıyan başka bir U-SQL UDO'sudur. Çıkarıcıya benzer şekilde, birkaç yerleşik çıktı vardır.

* *Outputters.Text()*: Farklı kodlamaların sınırlı metin dosyalarına veri yazar.
* *Outputters.Csv()*: Farklı kodlamaların virgülle ayrılmış değer (CSV) dosyalarına veri yazar.
* *Outputters.Tsv()*: Farklı kodlamaların sekme ayrılmış değer (TSV) dosyalarına veri yazar.

Özel çıktı, verileri özel tanımlı biçimde yazmanızı sağlar. Bu, aşağıdaki görevler için yararlı olabilir:

* Yarı yapılandırılmış veya yapılandırılmamış dosyalara veri yazma.
* Desteklenen verileri yazma.
* Çıktı verilerini değiştirme veya özel öznitelikler ekleme.

Kullanıcı tanımlı çıktıyı tanımlamak için `IOutputter` arabirimi oluşturmamız gerekir.

Temel `IOutputter` sınıf uygulaması aşağıdadır:

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

Sütun/satır sınır çözücüler, kodlama ve benzeri gibi çıktıcıya tüm giriş parametrelerinin sınıfın oluşturucuda tanımlanması gerekir. Arabirim, `IOutputter` geçersiz kılma `void Output` için bir tanım da içermelidir. Öznitelik `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` isteğe bağlı olarak atomik dosya işleme için ayarlanabilir. Daha fazla bilgi için aşağıdaki ayrıntılara bakın.

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class MyOutputter : IOutputter
{

    public MyOutputter(myparam1, myparam2)
    {
      …
    }

    public override void Close()
    {
      …
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
      …
    }
}
```

* `Output`her giriş satırı için çağrılır. Sıra kümesini `IUnstructuredWriter output` döndürür.
* Oluşturucu sınıfı, parametreleri kullanıcı tarafından tanımlanan çıktıcıya geçirmek için kullanılır.
* `Close`pahalı durumu serbest bırakmak veya son satırın ne zaman yazıldığını belirlemek için isteğe bağlı olarak geçersiz kılmak için kullanılır.

**SqlUserDefinedOutputter** özniteliği, türün kullanıcı tanımlı bir çıktıcı olarak kaydedilmesi gerektiğini belirtir. Bu sınıf devralınamaz.

SqlUserDefinedOutputter, kullanıcı tanımlı outputter tanımı için isteğe bağlı bir özelliktir. AtomicFileProcessing özelliğini tanımlamak için kullanılır.

* bool AtomicFileProcessing   

* **true** = Bu çıkışın atomik çıkış dosyaları gerektirdiğini gösterir (JSON, XML, ...)
* **false** = Bu çıktılayıcının bölünmüş / dağıtılmış dosyalarla başa çıkabileceğini gösterir (CSV, SEQ, ...)

Ana programlanabilirlik nesneleri **satır** ve **çıkış.** **Satır** nesnesi, çıktı verilerini arabirim `IRow` olarak sıralamak için kullanılır. **Çıktı,** çıktı verilerini hedef dosyaya ayarlamak için kullanılır.

Çıktı verilerine `IRow` arayüz üzerinden erişilir. Çıktı verileri bir defada bir satır geçirilir.

Tek tek değerler, IRow arabiriminin Get metodu çağırılarak numaralandırılır:

```
row.Get<string>("column_name")
```

Tek tek sütun adları `row.Schema`arayarak belirlenebilir:

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Bu yaklaşım, herhangi bir meta veri şeması için esnek bir çıktı oluşturabilmenizi sağlar.

Çıktı verileri kullanılarak `System.IO.StreamWriter`dosyaya yazılır. Akış `IUnstructuredWriter output`parametresi, `output.BaseStream` 'nin bir parçası olarak ayarlanır.

Her satır yinelemeden sonra veri arabelleği dosyaya floş önemli olduğunu unutmayın. Buna ek `StreamWriter` olarak, nesne Tek kullanımlık öznitelik etkin (varsayılan) ve **kullanılan** anahtar kelime ile kullanılmalıdır:

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

Aksi takdirde, her yinelemeden sonra Flush() yöntemini açıkça arayın. Bunu aşağıdaki örnekte gösteririz.

### <a name="set-headers-and-footers-for-user-defined-outputter"></a>Kullanıcı tanımlı çıktılayıcı için üstbilgi ve altbilgi ayarlama
Üstbilgi ayarlamak için tek yineleme yürütme akışını kullanın.

```
public override void Output(IRow row, IUnstructuredWriter output)
{
 …
if (isHeaderRow)
{
    …                
}

 …
if (isHeaderRow)
{
    isHeaderRow = false;
}
 …
}
}
```

İlk `if (isHeaderRow)` bloktaki kod yalnızca bir kez yürütülür.

Altbilgi için, `System.IO.Stream` nesne örneğine başvuru yu`output.BaseStream`kullanın ( ). Altbilgiyi arabirimin Kapanış() yöntemine `IOutputter` yazın.  (Daha fazla bilgi için aşağıdaki örneğe bakın.)

Aşağıda, kullanıcı tanımlı bir çıktılayıcı örneği verilmiştir:

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class HTMLOutputter : IOutputter
{
    // Local variables initialization
    private string row_delimiter;
    private char col_delimiter;
    private bool isHeaderRow;
    private Encoding encoding;
    private bool IsTableHeader = true;
    private Stream g_writer;

    // Parameters definition            
    public HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    this.isHeaderRow = isHeader;
    this.encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
    }

    // The Close method is used to write the footer to the file. It's executed only once, after all rows
    public override void Close()
    {
    //Reference to IO.Stream object - g_writer
    StreamWriter streamWriter = new StreamWriter(g_writer, this.encoding);
    streamWriter.Write("</table>");
    streamWriter.Flush();
    streamWriter.Close();
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
    System.IO.StreamWriter streamWriter = new StreamWriter(output.BaseStream, this.encoding);

    // Metadata schema initialization to enumerate column names
    ISchema schema = row.Schema;

    // This is a data-independent header--HTML table definition
    if (IsTableHeader)
    {
        streamWriter.Write("<table border=1>");
        IsTableHeader = false;
    }

    // HTML table attributes
    string header_wrapper_on = "<th>";
    string header_wrapper_off = "</th>";
    string data_wrapper_on = "<td>";
    string data_wrapper_off = "</td>";

    // Header row output--runs only once
    if (isHeaderRow)
    {
        streamWriter.Write("<tr>");
        for (int i = 0; i < schema.Count(); i++)
        {
        var col = schema[i];
        streamWriter.Write(header_wrapper_on + col.Name + header_wrapper_off);
        }
        streamWriter.Write("</tr>");
    }

    // Data row output
    streamWriter.Write("<tr>");                
    for (int i = 0; i < schema.Count(); i++)
    {
        var col = schema[i];
        string val = "";
        try
        {
        // Data type enumeration--required to match the distinct list of types from OUTPUT statement
        switch (col.Type.Name.ToString().ToLower())
        {
            case "string": val = row.Get<string>(col.Name).ToString(); break;
            case "guid": val = row.Get<Guid>(col.Name).ToString(); break;
            default: break;
        }
        }
        // Handling NULL values--keeping them empty
        catch (System.NullReferenceException)
        {
        }
        streamWriter.Write(data_wrapper_on + val + data_wrapper_off);
    }
    streamWriter.Write("</tr>");

    if (isHeaderRow)
    {
        isHeaderRow = false;
    }
    // Reference to the instance of the IO.Stream object for footer generation
    g_writer = output.BaseStream;
    streamWriter.Flush();
    }
}

// Define the factory classes
public static class Factory
{
    public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    return new HTMLOutputter(isHeader, encoding);
    }
}
```

Ve U-SQL temel komut dosyası:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.html";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
         FROM @input_file
         USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 
    TO @output_file 
    USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Bu, tablo verileri içeren bir HTML dosyası oluşturan bir HTML çıktısı oluşturucudur.

### <a name="call-outputter-from-u-sql-base-script"></a>U-SQL temel komut dosyasından arama çıkışı
Temel U-SQL komut dosyasından özel bir çıktıcı çağırmak için, outputter nesnesinin yeni örneğinin oluşturulması gerekir.

```sql
OUTPUT @rs0 TO @output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Temel komut dosyasında nesnenin bir örneğini oluşturmamak için, önceki örneğimizde gösterildiği gibi bir işlev sarıcı oluşturabiliriz:

```csharp
        // Define the factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

Bu durumda, özgün arama aşağıdaki gibi görünür:

```
OUTPUT @rs0 
TO @output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a>Kullanıcı tanımlı işlemcileri kullanma
Kullanıcı tanımlı işlemci veya UDP, programlanabilirlik özellikleri uygulayarak gelen satırları işlemenizi sağlayan bir U-SQL UDO türüdür. UDP, sütunları birleştirmenizi, değerleri değiştirmenizi ve gerekirse yeni sütunlar eklemenizi sağlar. Temel olarak, gerekli veri öğelerini üretmek için bir satır kümesini işlemeye yardımcı olur.

UdP tanımlamak için, UDP `IProcessor` için isteğe bağlı öznitelik içeren `SqlUserDefinedProcessor` bir arabirim oluşturmamız gerekir.

Bu arabirim, aşağıdaki `IRow` örnekte gösterildiği gibi arabirim satır geçersiz kılma tanımıiçermelidir:

```
[SqlUserDefinedProcessor]
public class MyProcessor: IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
        …
 }
}
```

**SqlUserDefinedProcessor,** türün kullanıcı tanımlı bir işlemci olarak kaydedilmesi gerektiğini belirtir. Bu sınıf devralınamaz.

SqlUserDefinedProcessor özniteliği UDP tanımı için **isteğe bağlıdır.**

Ana programlanabilirlik nesneleri **giriş** ve **çıktıdır.** Giriş nesnesi, giriş sütunlarını ve çıktılarını sıralamak ve işlemci etkinliği sonucunda çıktı verilerini ayarlamak için kullanılır.

Giriş sütunları numaralandırma için `input.Get` yöntemi kullanırız.

```
string column_name = input.Get<string>("column_name");
```

Yöntem için `input.Get` parametre, U-SQL temel komut `PRODUCE` dosyasının `PROCESS` deyiminin bir parçası olarak geçirilen bir sütundur. Burada doğru veri türünü kullanmalıyız.

Çıktı için `output.Set` yöntemi kullanın.

Özel üreticinin yalnızca `output.Set` yöntem çağrısıyla tanımlanan sütunları ve değerleri çıktıladığını unutmayın.

```
output.Set<string>("mycolumn", mycolumn);
```

Gerçek işlemci çıkışı' n. `return output.AsReadOnly();`

Aşağıdaki bir işlemci örneği:

```
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

Bu kullanım durumu senaryosunda, işlemci varolan sütunları birleştirerek "full_description" adlı yeni bir sütun oluşturur-- bu durumda, büyük harfle "kullanıcı" ve "des". Ayrıca bir GUID'i yeniler ve özgün ve yeni GUID değerlerini döndürür.

Önceki örnekten de görebileceğiniz gibi, yöntem çağrısı `output.Set` sırasında C# yöntemlerini arayabilirsiniz.

Aşağıda, özel bir işlemci kullanan temel U-SQL komut dosyası örneği verilmiştir:

```
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

## <a name="use-user-defined-appliers"></a>Kullanıcı tanımlı aplikeleri kullanma
U-SQL kullanıcı tanımlı bir uygulama, sorgunun dış tablo ifadesi yle döndürülen her satır için özel bir C# işlevini çağırmanızı sağlar. Sağ giriş, sol girişten her satır için değerlendirilir ve üretilen satırlar son çıktı için birleştirilir. APPLY işleci tarafından üretilen sütunlistesi, sol ve sağ girişteki sütun kümesinin birleşimidir.

USQL SELECT ifadesinin bir parçası olarak kullanıcı tanımlı applier çağrılmaktadır.

Kullanıcı tanımlı applier için tipik arama aşağıdaki gibi görünür:

```
SELECT …
FROM …
CROSS APPLYis used to pass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

SELECT ifadesinde appliers kullanma hakkında daha fazla bilgi için cross [APPLY ve OUTER APPLY'dan U-SQL SELECT Seçimi'ne](/u-sql/statements-and-expressions/select/from/select-selecting-from-cross-apply-and-outer-apply)bakın.

Kullanıcı tanımlı applier taban sınıf tanımı aşağıdaki gibidir:

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

Kullanıcı tanımlı bir apse tanımlamak için, `IApplier` kullanıcı tanımlı bir aplik tanımı için isteğe bağlı olan [`SqlUserDefinedApplier`] özniteliği ile arabirim oluşturmamız gerekir.

```
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

* Dış tablonun her satırı için uygula denir. Çıktı satır `IUpdatableRow` kümesini döndürür.
* Oluşturucu sınıfı, parametreleri kullanıcı tanımlı apilete geçirmek için kullanılır.

**SqlUserDefinedApplier,** türün kullanıcı tanımlı bir aplier olarak kaydedilmesi gerektiğini belirtir. Bu sınıf devralınamaz.

**SqlUserDefinedApplier,** kullanıcı tanımlı bir uygulama tanımı için **isteğe bağlıdır.**


Ana programlanabilirlik nesneleri aşağıdaki gibidir:

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

Giriş rowsets giriş `IRow` olarak geçirilir. Çıktı satırları çıkış arabirimi olarak `IUpdatableRow` oluşturulur.

Tek tek sütun adları `IRow` Şema yöntemi ni çağırarak belirlenebilir.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Gelen gerçek veri değerlerini `IRow`elde etmek için Get() arabirimi yöntemini `IRow` kullanırız.

```
mycolumn = row.Get<int>("mycolumn")
```

Ya da şema sütun adını kullanırız:

```
row.Get<int>(row.Schema[0].Name)
```

Çıktı değerleri çıktı ile `IUpdatableRow` ayarlanmalıdır:

```
output.Set<int>("mycolumn", mycolumn)
```

Özel uygulamacıların yalnızca yöntem çağrısıyla `output.Set` tanımlanan çıkış sütunlarını ve değerlerini anladığını anlamak önemlidir.

Gerçek çıktı çağırılarak `yield return output.AsReadOnly();`tetiklenir.

Kullanıcı tanımlı aplik parametreleri oluşturucuya iletilebilir. Applier, temel U-SQL Script'teki applier araması sırasında tanımlanması gereken değişken sayıda sütun döndürebilir.

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

Aşağıda, kullanıcı tanımlı uygulama örneği verilmiştir:

```
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

Aşağıdaki kullanıcı tanımlı bu applier için temel U-SQL komut dosyası:

```
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

Bu kullanım örneği senaryosunda, kullanıcı tanımlı applier araç filosu özellikleri için virgül-delimited değer parser olarak davranır. Giriş dosyası satırları aşağıdaki gibi görünür:

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Chevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

Bu, make ve model gibi araç özelliklerini içeren bir özellik sütunu olan tipik bir sekme-sınırlandırılmış TSV dosyasıdır. Bu özellikler tablo sütunlarına ayrıştırılmalıdır. Sağlanan applier, geçirilen parametreye bağlı olarak sonuç satır kümesinde dinamik bir özellik sayısı oluşturmanıza da olanak tanır. Yalnızca tüm özellikleri veya belirli bir özellik kümesi oluşturabilirsiniz.

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

Kullanıcı tanımlı applier applier nesnenin yeni bir örnek olarak çağrılabilir:

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: "value") AS alias([columns types]…);
```

Ya da bir sarma makinesi fabrikası yönteminin çağrılması ile:

```csharp
    CROSS APPLY MyNameSpace.MyApplier (parameter: "value") AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a>Kullanıcı tanımlı kombinleri kullanma
Kullanıcı tanımlı birleşimcisi veya UDC, özel mantığa bağlı olarak sol ve sağ satır kümelerinden satırları birleştirmenizi sağlar. Kullanıcı tanımlı birleşim aracı, BIRLEŞ" ifadesi ile kullanılır.

Birbirlatörü, hem giriş satır kümeleri, gruplandırma sütunları, beklenen sonuç şeması ve ek bilgiler hakkında gerekli bilgileri sağlayan BIRLEŞ tümleme ifadesi yle çağrılıyor.

Bir birbirliyi temel U-SQL komut dosyasında çağırmak için aşağıdaki sözdizimini kullanırız:

```
Combine_Expression :=
    'COMBINE' Combine_Input
    'WITH' Combine_Input
    Join_On_Clause
    Produce_Clause
    [Readonly_Clause]
    [Required_Clause]
    USING_Clause.
```

Daha fazla bilgi için [birleştirme İfadesi (U-SQL)](/u-sql/statements-and-expressions/combine-expression)bakın.

Kullanıcı tanımlı bir birbirleşim tanımlamak `ICombiner` için, kullanıcı`SqlUserDefinedCombiner`tanımlı Birleşim tanımı için isteğe bağlı olan [ ] özniteliği ile arabirim oluşturmamız gerekir.

Taban `ICombiner` sınıf tanımı:

```
public abstract class ICombiner : IUserDefinedOperator
{
protected ICombiner();
public virtual IEnumerable<IRow> Combine(List<IRowset> inputs,
       IUpdatableRow output);
public abstract IEnumerable<IRow> Combine(IRowset left, IRowset right,
       IUpdatableRow output);
}
```

Arabirimin `ICombiner` özel uygulaması, Birleştirme `IEnumerable<IRow>` geçersiz kılma tanımını içermelidir.

```
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

**SqlUserDefinedCombiner** özniteliği, türün kullanıcı tanımlı bir birleşimci olarak kaydedilmesi gerektiğini belirtir. Bu sınıf devralınamaz.

**SqlUserDefinedCombiner** Combiner mod özelliğini tanımlamak için kullanılır. Kullanıcı tanımlı bir birbirleşim tanımı için isteğe bağlı bir özniteliktir.

CombinerMode Modu

CombinerMode enum aşağıdaki değerleri alabilir:

* Tam (0) Her çıkış satırı potansiyel olarak aynı anahtar değeriile sol ve sağ tüm giriş satırları bağlıdır.

* Sol (1) Her çıkış satırı soldan tek bir giriş satırına bağlıdır (ve potansiyel olarak aynı anahtar değerine sahip sağdan tüm satırlar).

* Sağ (2) Her çıkış satırı sağdan tek bir giriş satırına bağlıdır (ve potansiyel olarak aynı anahtar değerine sahip soldan tüm satırlar).

* İç (3) Her çıkış satırı, aynı değere sahip, soldan ve soldan tek bir giriş satırına bağlıdır.

Örnek:`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`[ ]


Ana programlanabilirlik nesneleri şunlardır:

```csharp
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

Giriş sıra kümeleri, **sol** ve **sağ** `IRowset` arabirim türü olarak geçirilir. Her iki satır kümesi de işlenmek için numaralandırılmalıdır. Her arabirimi yalnızca bir kez sayısalatabilirsiniz, bu yüzden gerekirse sayısalolarak önbelleğe almak zorundayız.

Önbelleğe alma amacıyla, bir\<\> LINQ sorgu yürütmesi sonucu bir Liste T `IRow` bellek yapısı türü oluşturabilir, özellikle liste<>. Anonim veri türü numaralandırma sırasında da kullanılabilir.

Linq sorguları hakkında daha fazla bilgi için [LINQ Sorgularına Giriş (C#)](/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) ve İzlenebilir\<T\> arabirimi hakkında daha fazla bilgi için [Ayrılmaz\<T\> Arabirimi'ne](/dotnet/api/system.collections.generic.ienumerable-1) bakın.

Gelen gerçek veri değerlerini `IRowset`elde etmek için Get() arabirimi yöntemini `IRow` kullanırız.

```
mycolumn = row.Get<int>("mycolumn")
```

Tek tek sütun adları `IRow` Şema yöntemi ni çağırarak belirlenebilir.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Veya şema sütun adını kullanarak:

```
c# row.Get<int>(row.Schema[0].Name)
```

LINQ ile genel numaralandırma aşağıdaki gibi görünür:

```
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

Her iki satır kümesini de sıraladıktan sonra, tüm satırları döngüye sokacağız. Sol satır kümesindeki her satır için, birleşicimizin durumunu karşılayan tüm satırları bulacağız.

Çıktı değerleri çıktı ile `IUpdatableRow` ayarlanmalıdır.

```
output.Set<int>("mycolumn", mycolumn)
```

Gerçek `yield return output.AsReadOnly();`çıktı' ya çağrılarak tetiklenir.

Aşağıdaki bir birleşim örneği:

```
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

Bu kullanım durumu senaryosunda, perakendeci için bir analiz raporu oluşturuyoruz. Amaç fazla 20.000 $ maliyet ve belirli bir zaman dilimi içinde normal perakendeci aracılığıyla daha hızlı web sitesi üzerinden satmak tüm ürünleri bulmaktır.

Burada temel U-SQL komut dosyasıdır. Normal bir JOIN ile birbirlatörü arasındaki mantığı karşılaştırabilirsiniz:

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

Kullanıcı tanımlı bir birleşim aracı, applier nesnesinin yeni bir örneği olarak çağrılabilir:

```
USING new MyNameSpace.MyCombiner();
```


Ya da bir sarma makinesi fabrikası yönteminin çağrılması ile:

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a>Kullanıcı tanımlı azaltıcılar kullanma

U-SQL, kullanıcı tanımlı operatör genişletilebilirlik çerçevesini kullanarak ve bir IReducer arabirimi uygulayarak C# özel rowset indirgemecileryazmanızı sağlar.

Kullanıcı tanımlı azaltıcı veya UDR, veri ayıklama (alma) sırasında gereksiz satırları ortadan kaldırmak için kullanılabilir. Ayrıca, satır ve sütunları işlemek ve değerlendirmek için de kullanılabilir. Programlanabilirlik mantığına bağlı olarak, hangi satırların ayıklanması gerektiğini de tanımlayabilir.

UDR sınıfTanımlamak için isteğe bağlı `IReducer` `SqlUserDefinedReducer` bir öznitelik içeren bir arabirim oluşturmamız gerekir.

Bu sınıf arabirimi `IEnumerable` arabirim rowset geçersiz kılma için bir tanım içermelidir.

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
        …
    }

}
```

**SqlUserDefinedReducer** özniteliği, türün kullanıcı tanımlı bir indirgeme civarı olarak kaydedilmesi gerektiğini belirtir. Bu sınıf devralınamaz.
**SqlUserDefinedReducer,** kullanıcı tanımlı bir indirgemeci tanımı için isteğe bağlı bir özelliktir. Isrecursive özelliğitanımlamak için kullanılır.

* bool IsRecursive    
* **true** = Bu Azaltıcının uyumlu ve uyumlu olup olmadığını gösterir

Ana programlanabilirlik nesneleri **giriş** ve **çıktıdır.** Giriş nesnesi, giriş satırlarını sıralamak için kullanılır. Çıktı, etkinliği azaltmanın bir sonucu olarak çıktı satırlarını ayarlamak için kullanılır.

Giriş satırları numaralandırma için `Row.Get` yöntemi kullanırız.

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

Yöntemin `Row.Get` parametresi, U-SQL temel komut dosyasının `PRODUCE` `REDUCE` sınıfının bir parçası olarak geçirilen bir sütundur. Burada da doğru veri türünü kullanmalıyız.

Çıktı için `output.Set` yöntemi kullanın.

Özel azaltıcının yalnızca yöntem çağrısıyla tanımlanan çıktı değerlerinin `output.Set` olduğunu anlamak önemlidir.

```
output.Set<string>("mycolumn", guid);
```

Gerçek azaltıcı çıkışı çağırArak `yield return output.AsReadOnly();`tetiklenir.

Aşağıda bir indirgeyici örneği verilmiştir:

```
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

Bu kullanım örneği senaryosunda, indirici boş bir kullanıcı adı olan satırları atlıyor. Satır kümesindeki her satır için, gerekli her sütunu okur, ardından kullanıcı adının uzunluğunu değerlendirir. Yalnızca kullanıcı adı değeri uzunluğu 0'dan fazlaysa gerçek satırı çıkar.

Aşağıda özel bir indirgeci kullanan temel U-SQL komut dosyası verem:

```
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
