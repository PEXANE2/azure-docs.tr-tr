---
title: JSON verileriyle çalışma
description: Azure SQL Veritabanı, JavaScript Object Notation (JSON) gösteriminde verileri ayrıştırmanızı, sorgulamanızı ve biçimlendirmenizi sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
ms.date: 01/15/2019
ms.openlocfilehash: 958d937ad85fd62249c7ce3f0e0ab2f8cc1d1b80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73819932"
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Azure SQL Veritabanı'nda JSON özellikleriyle başlarken
Azure SQL Veritabanı, JavaScript Nesne Gösterimi [(JSON)](https://www.json.org/) biçiminde temsil edilen verileri ayrıştırmanızı ve sorgulamanızı ve ilişkisel verilerinizi JSON metni olarak dışa aktarmanızı sağlar. Aşağıdaki JSON senaryoları Azure SQL Veritabanı'nda kullanılabilir:
- [İlişkisel verileri JSON biçiminde](#formatting-relational-data-in-json-format) `FOR JSON` yan tümceyi kullanarak biçimlendirme.
- [JSON verileriyle çalışma](#working-with-json-data)
- JSON skaler işlevlerini kullanarak [JSON verilerini sorgulama.](#querying-json-data)
- Işlevi kullanarak `OPENJSON` [JSON'ı tabular biçime dönüştürme.](#transforming-json-into-tabular-format)

## <a name="formatting-relational-data-in-json-format"></a>İlişkisel verileri JSON biçiminde biçimlendirme
Veritabanı katmanından veri alan ve JSON biçiminde yanıt veren bir web hizmetiniz veya JSON olarak biçimlendirilmiş verileri kabul eden istemci tarafındaki JavaScript çerçeveleriniz veya kitaplıklarınız varsa, veritabanı içeriğinizi doğrudan bir SQL sorgusunda JSON olarak biçimlendirebilirsiniz. Artık Azure SQL Veritabanı'ndan sonuçları JSON olarak biçimlendiren uygulama kodu yazmanız veya tabular sorgu sonuçlarını dönüştürmek ve nesneleri JSON biçimine seri hale getirmek için bazı JSON serileştirme kitaplığı eklemeniz gerekmeyecektir. Bunun yerine, SQL sorgu sonuçlarını Azure SQL Veritabanı'nda JSON olarak biçimlendirmek ve doğrudan uygulamanızda kullanmak için FOR JSON yan tümcesini kullanabilirsiniz.

Aşağıdaki örnekte, Sales.Customer tablosundaki satırlar FOR JSON yan tümcesi kullanılarak JSON olarak biçimlendirilir:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

FOR JSON PATH yan tümcesi sorgu sonuçlarını JSON metni olarak biçimlendirin. Hücre değerleri JSON değerleri olarak oluşturulurken sütun adları anahtar olarak kullanılır:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Sonuç kümesi, her satırın ayrı bir JSON nesnesi olarak biçimlendiği bir JSON dizisi olarak biçimlendirilir.

PATH, sütun diğer adlarında nokta gösterimi kullanarak JSON sonucun çıktı biçimini özelleştirebileceğinizi gösterir. Aşağıdaki sorgu, çıkış JSON biçiminde "CustomerName" anahtarının adını değiştirir ve telefon ve faks numaralarını "İlgili Kişi" alt nesnesi olarak koyar:

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Bu sorgunun çıktısı aşağıdaki gibi görünür:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

Bu örnekte, [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) seçeneğini belirterek dizi yerine tek bir JSON nesnesini döndürmüştür. Sorgu sonucunda tek bir nesne döndürdeğinizi biliyorsanız bu seçeneği kullanabilirsiniz.

FOR JSON yan tümcesinin ana değeri, iç içe json nesneleri veya dizileri olarak biçimlendirilmiş veritabanınızdan karmaşık hiyerarşik verileri döndürmenize olanak tanır. Aşağıdaki örnek, `Orders` iç içe bir dizi `Customer` olarak ait tablodan satırları `Orders`nasıl dahil gösterir:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Müşteri verilerini almak ve ardından ilgili Siparişlerin listesini almak için ayrı sorgular göndermek yerine, aşağıdaki örnek çıktıda gösterildiği gibi, tek bir sorguyla gerekli tüm verileri alabilirsiniz:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>JSON verileriyle çalışma
Katı yapılı verileriniz yoksa, karmaşık alt nesneleriniz, dizileriniz veya hiyerarşik verileriniz varsa veya veri yapılarınızın zaman içinde evrimleşmesi durumunda, JSON biçimi karmaşık veri yapısını temsil etmenize yardımcı olabilir.

JSON, Azure SQL Veritabanı'ndaki diğer dize türü gibi kullanılabilen metin biçimidir. JSON verilerini standart bir NVARCHAR olarak gönderebilir veya depolayabilirsiniz:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

Bu örnekte kullanılan JSON verileri NVARCHAR(MAX) türü kullanılarak temsil edilir. JSON bu tabloya eklenebilir veya aşağıdaki örnekte gösterildiği gibi standart Transact-SQL sözdizimi kullanılarak depolanan yordamın bir bağımsız değişkeni olarak sağlanabilir:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Azure SQL Veritabanı'nda dize verileriyle çalışan istemci tarafındaki herhangi bir dil veya kitaplık da JSON verileriyle çalışır. JSON, Bellek için optimize edilmiş tablo veya Sistem sürümüyle yapılan tablo gibi NVARCHAR türünü destekleyen herhangi bir tabloda depolanabilir. JSON istemci tarafı kodunda veya veritabanı katmanında herhangi bir kısıtlama getirmez.

## <a name="querying-json-data"></a>JSON verilerini sorgulama
Azure SQL tablolarında depolanan JSON olarak biçimlendirilmiş verileriniz varsa, JSON işlevleri bu verileri herhangi bir SQL sorgusunda kullanmanıza izin verebiliyor.

Azure SQL veritabanında kullanılabilen JSON işlevleri, JSON olarak biçimlendirilmiş verileri diğer SQL veri türü olarak ele almanıza izin tanır. JSON metninden değerleri kolayca ayıklayabilir ve herhangi bir sorguda JSON verilerini kullanabilirsiniz:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

JSON_VALUE işlevi, Veri sütununda depolanan JSON metninden bir değer ayıklar. Bu işlev, json metinde ayıklamak için bir değer başvuruiçin JavaScript benzeri bir yol kullanır. Çıkarılan değer SQL sorgusunun herhangi bir bölümünde kullanılabilir.

JSON_QUERY işlevi JSON_VALUE benzer. JSON_VALUE aksine, bu işlev JSON metnine yerleştirilen diziler veya nesneler gibi karmaşık alt nesneyi ayıklar.

JSON_MODIFY işlevi, JSON metninde güncellenmesi gereken değerin yolunu ve eskisinin üzerine yazacak yeni bir değeri belirtmenizi sağlar. Bu şekilde, tüm yapıyı geri almadan JSON metnini kolayca güncelleştirebilirsiniz.

JSON standart bir metinde depolandığından, metin sütunlarında depolanan değerlerin düzgün biçimlendirilmiş olduğunun garantisi yoktur. JSON sütununda depolanan metnin standart Azure SQL Veritabanı denetim kısıtlamaları ve ISJSON işlevini kullanarak düzgün biçimlendirilmiş olduğunu doğrulayabilirsiniz:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Giriş metni json düzgün biçimlendirilmişise, ISJSON işlevi değeri 1 döndürür. JSON sütununun her ekleme veya güncelleştirmesinde, bu kısıtlama yeni metin değerinin bozuk JSON olmadığını doğrular.

## <a name="transforming-json-into-tabular-format"></a>JSON'un tabular formata dönüştürülmesi
Azure SQL Veritabanı ayrıca JSON koleksiyonlarını düz biçime dönüştürmenize ve JSON verilerini yüklemenize veya sorgulamanıza olanak tanır.

OPENJSON, JSON metnini ayrıştıran, json nesnelerinin bir dizisini bulunan, dizi öğelerini yineleyen ve dizinin her öğesi için çıktı sonucu bir satır döndüren bir tablo değeri işlevidir.

![JSON tabular](./media/sql-database-json-features/image_2.png)

Yukarıdaki örnekte, açılması gereken JSON dizininin nerede bulunacağını belirtebiliriz ($'da. Siparişler yolu), sonuç olarak hangi sütunların döndürülmesi gerektiği ve hücre olarak döndürülecek JSON değerlerinin nerede bulunacağı.

Değişkendeki bir JSON dizisini @orders bir satır kümesine dönüştürebilir, bu sonuç kümesini analiz edebilir veya satırları standart bir tabloya ekleyebiliriz:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    FROM OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
JSON dizisi olarak biçimlendirilmiş ve depolanan yordamiçin parametre olarak sağlanan siparişlerin toplanması ayrıştırılabilir ve Siparişler tablosuna eklenebilir.

## <a name="next-steps"></a>Sonraki adımlar
JSON'u uygulamanıza nasıl entegre edacağınızı öğrenmek için şu kaynaklara göz atın:

* [TechNet Blog](https://blogs.technet.microsoft.com/dataplatforminsider/20../../json-in-sql-server-2016-part-1-of-4/)
* [MSDN belgeleri](https://msdn.microsoft.com/library/dn921897.aspx)
* [Kanal 9 video](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

JSON'u uygulamanıza entegre etmek için çeşitli senaryolar hakkında bilgi edinmek için, bu [Kanal 9 videosundaki](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) demolara bakın veya [JSON blog gönderilerinde](https://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/)kullanım durumunuzla eşleşen bir senaryo bulun.

