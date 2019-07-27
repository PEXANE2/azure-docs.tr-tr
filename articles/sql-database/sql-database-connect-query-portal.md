---
title: "Azure portal: Sorgu düzenleyicisini kullanarak Azure SQL veritabanı 'nı sorgulama | Microsoft Docs"
description: SQL Sorgu Düzenleyicisi'ni kullanarak Azure portalında SQL Veritabanına bağlanmayı öğrenin. Ardından, verileri sorgulamak ve düzenlemek için Transact-SQL (T-SQL) deyimleri çalıştırın.
keywords: sql veritabanına bağlanma,azure portalı, portal, sorgu düzenleyicisi
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: Ninarn
ms.author: ninarn
ms.reviewer: carlrab
ms.date: 06/28/2019
ms.openlocfilehash: 3702c88d0a5cdc7aa1f854f71e3aee8a42d9c22c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569177"
---
# <a name="quickstart-use-the-azure-portals-sql-query-editor-to-connect-and-query-data"></a>Hızlı Başlangıç: Verilere bağlanmak ve verileri sorgulamak için Azure portal SQL sorgu düzenleyicisini kullanın

SQL sorgu Düzenleyicisi, Azure SQL veritabanı veya Azure SQL veri ambarı üzerinde SQL sorguları yürütmek için kolay bir yol sağlayarak, Azure portal tarayıcı bir araçtır. Bu hızlı başlangıçta, sorgu düzenleyicisini kullanarak bir SQL veritabanına bağlanın ve ardından Transact-SQL deyimlerini çalıştırarak verileri sorgulama, ekleme, güncelleştirme ve silme.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Bir Azure SQL veritabanı. Azure SQL veritabanı 'nda bir veritabanı oluşturmak ve yapılandırmak için bu hızlı başlangıçlardan birini kullanabilirsiniz:

  || Tek veritabanı |
  |:--- |:--- |
  | Create| [Portal](sql-database-single-database-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) |
  | Yapılandırma | [Sunucu düzeyi IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)|
  |||

> [!NOTE]
> Sorgu Düzenleyicisi, iletişim kurmak için 443 ve 1443 bağlantı noktalarını kullanır.  Lütfen bu bağlantı noktalarında giden HTTPS trafiğini etkinleştirmiş olduğunuzdan emin olun. Ayrıca, veritabanlarına ve veri ambarlarınıza erişmek için giden IP adresinizi sunucunun izin verilen güvenlik duvarı kurallarına eklemeniz gerekir.

## <a name="sign-in-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="connect-using-sql-authentication"></a>SQL kimlik doğrulamasını kullanarak bağlanma

1. Seçin **SQL veritabanları** seçin ve sol taraftaki menüden **mySampleDatabase**.

2. Sol taraftaki menüde bulmak ve seçmek **sorgu Düzenleyicisi (Önizleme)** . **Oturum açma** sayfası görüntülenir.

    ![sorgu düzenleyicisini bulma](./media/sql-database-connect-query-portal/find-query-editor.PNG)

3. Gelen **Yetkilendirme türü** açılan menüsünde, select **SQL Server kimlik doğrulamasını** ve kullanıcı kimliği ve veritabanı oluşturmak için kullanılan sunucu yönetici hesabı parolasını girin.

    ![oturum aç](./media/sql-database-connect-query-portal/login-menu.png)

4. **Tamam**’ı seçin.


## <a name="connect-using-azure-active-directory"></a>Azure Active Directory kullanarak bağlan

Bir Active Directory (AD) yöneticisinin yapılandırılması, Azure portalı ve SQL veritabanınızda oturum açmak için tek bir kimlik kullanmanıza olanak sağlar. SQL server için bir AD Yöneticisi yapılandırmak için aşağıdaki adımları izleyin.

> [!NOTE]
> * E-posta hesapları (örneğin, outlook.com, gmail.com, yahoo.com vb.) AD yöneticileri olarak henüz desteklenmemektedir. Bir kullanıcı ya da yerel olarak Azure AD'de oluşturulmuş veya Azure AD ile Federasyon seçtiğinizden emin olun.
> * Azure AD Yöneticisi'ni açın 2 faktörlü kimlik doğrulaması etkin olan hesapları ile çalışmaz.

1. Seçin **tüm kaynakları** sol taraftaki menüden ve ardından SQL server'ınızı seçin.

2. SQL Server 'ın **Ayarlar** menüsünden **yönetici Active Directory**' yi seçin.

3. AD Yönetim sayfası araç çubuğundan seçin **yönetici Ayarla** ve kullanıcı veya grup, AD Yöneticisi olarak seçin

    ![active directory seçme](./media/sql-database-connect-query-portal/select-active-directory.png)

4. AD Yönetim sayfası araç çubuğundan seçin **Kaydet**.

5. Gidin **mySampleDatabase** veritabanı ve sol taraftaki menüden **sorgu Düzenleyicisi (Önizleme)** . **Oturum açma** sayfası görüntülenir. Bir AD yöneticisiyseniz, sağ taraftaki **Active Directory çoklu oturum açma**altında oturum açmış olduğunuzu bildiren bir ileti görüntülenir.

6. **Tamam**’ı seçin.


## <a name="view-data"></a>Verileri görüntüleme

1. Doğrulandıktan sonra ilk 20 ürünü kategoriye göre almak için sorgu Düzenleyicisi'nde aşağıdaki SQL yapıştırın.

   ```sql
    SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
    FROM SalesLT.ProductCategory pc
    JOIN SalesLT.Product p
    ON pc.productcategoryid = p.productcategoryid;
   ```

2. Araç çubuğunda **çalıştırma** ve ardından çıktıyı inceleyin **sonuçları** bölmesi.

![sorgu düzenleyicisi sonuçları](./media/sql-database-connect-query-portal/query-editor-results.png)

## <a name="insert-data"></a>Veri ekleme

`SalesLT.Product` Tabloya yeni bir ürün eklemek için aşağıdaki [Insert](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL ifadesini çalıştırın.

1. Önceki sorguyu Bununla değiştirin.

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```


2. `Product` Tabloya yeni bir satır eklemek için **Çalıştır** ' ı seçin. **İletiler** bölmesi sorgu başarılı **iletisini görüntülüyor: Etkilenen satırlar: 1**.


## <a name="update-data"></a>Verileri güncelleştirme

Yeni ürününüzü değiştirmek için aşağıdaki [Update](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL ifadesini çalıştırın.

1. Önceki sorguyu Bununla değiştirin.

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. `Product` Tablodaki belirtilen satırı güncelleştirmek için **Çalıştır** ' ı seçin. **İletiler** bölmesi sorgu başarılı **iletisini görüntülüyor: Etkilenen satırlar: 1**.

## <a name="delete-data"></a>Verileri silme

Yeni ürününüzü kaldırmak için aşağıdaki [Delete](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL ifadesini çalıştırın.

1. Önceki sorguyu Bununla değiştirin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. `Product` Tablodaki belirtilen satırı silmek için **Çalıştır** ' ı seçin. **İletiler** bölmesi sorgu başarılı **iletisini görüntülüyor: Etkilenen satırlar: 1**.


## <a name="query-editor-considerations"></a>Sorgu Düzenleyicisi konuları

Sorgu Düzenleyicisi ile çalışırken bilmeniz gereken birkaç nokta vardır.

* Sorgu Düzenleyicisi, iletişim kurmak için 443 ve 1443 bağlantı noktalarını kullanır.  Lütfen bu bağlantı noktalarında giden HTTPS trafiğini etkinleştirmiş olduğunuzdan emin olun. Ayrıca, veritabanlarına ve veri ambarlarınıza erişmek için giden IP adresinizi sunucunun izin verilen güvenlik duvarı kurallarına eklemeniz gerekir.

* F5 tuşuna basarak sorgu Düzenleyicisi sayfası yenilenir ve üzerinde çalışılmakta olan herhangi bir sorgu kaybolur.

* Sorgu Düzenleyicisi, `master` veritabanına bağlanmayı desteklemiyor.

* Sorgu yürütme için 5 dakikalık zaman aşımı yoktur.

* Sorgu Düzenleyicisi, yalnızca coğrafi veri türleri için Silindirik izdüşümü destekler.

* Veritabanı tabloları ve görünümleri için IntelliSense desteği yoktur. Ancak, düzenleyici zaten girilmiş adları otomatik tamamlama desteklemiyor.


## <a name="next-steps"></a>Sonraki adımlar

Azure SQL veritabanlarında desteklenen Transact-SQL hakkında daha fazla bilgi için bkz: [Transact-SQL farklılıklarını çözümleme SQL veritabanına geçiş sırasında](sql-database-transact-sql-information.md).
