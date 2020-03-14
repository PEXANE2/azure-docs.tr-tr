---
title: Azure portal sorgu düzenleyicisini kullanarak bir SQL veritabanını sorgulama
description: Azure SQL veritabanında Transact-SQL (T-SQL) sorgularını çalıştırmak için sorgu düzenleyicisini nasıl kullanacağınızı öğrenin.
keywords: SQL veritabanı 'na bağlanma, SQL veritabanı, Azure Portal, Portal, sorgu Düzenleyicisi 'ni sorgulama
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: Ninarn
ms.author: ninarn
ms.reviewer: carlrab
ms.date: 03/12/2020
ms.openlocfilehash: 5847ef3033d257faef4831785b8abd864d54e835
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209594"
---
# <a name="quickstart-use-the-azure-portals-query-editor-to-query-a-sql-database"></a>Hızlı başlangıç: bir SQL veritabanını sorgulamak için Azure portal sorgu düzenleyicisini kullanma

Sorgu Düzenleyicisi, Azure SQL veritabanı veya Azure SQL veri Ambarınızda SQL sorguları çalıştırmak için Azure portal bir araçtır. 

Bu hızlı başlangıçta, Azure SQL veritabanında Transact-SQL (T-SQL) sorgularını çalıştırmak için sorgu düzenleyicisini kullanacaksınız.


## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak AdventureWorksLT örnek veritabanı gerektirir. AdventureWorksLT SQL veritabanının çalışan bir kopyasına sahip değilseniz aşağıdaki hızlı başlangıçta bir tane oluşturulur:

- [Hızlı başlangıç: Azure portal, PowerShell veya Azure CLı kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-single-database-get-started.md) 

### <a name="configure-network-settings"></a>Ağ ayarlarını yapılandırma

Sorgu düzenleyicisinde aşağıdaki hatalardan birini alırsanız: *Yerel Ağ ayarlarınız sorgu düzenleyicisinin sorguları vermesini engelleyebilir. Ağ ayarlarınızı yapılandırma yönergeleri için lütfen buraya tıklayın*veya *sunucu bağlantısı kurulamadı. Bu, yerel güvenlik duvarı yapılandırmanızla veya ağ ara sunucu ayarlarınızda bir sorun olduğunu gösterebilir*ve aşağıdaki önemli bilgiler çözümlenmelidir:

> [!IMPORTANT]
> Sorgu Düzenleyicisi, iletişim kurmak için 443 ve 1443 bağlantı noktalarını kullanır. Bu bağlantı noktalarında giden HTTPS trafiğini etkinleştirmiş olduğunuzdan emin olun. Ayrıca, veritabanlarına ve veri ambarlarınıza erişmek için, [gıden IP adresinizi sunucunun izin verilen güvenlik duvarı kurallarına eklemeniz](sql-database-server-level-firewall-rule.md) gerekir.


## <a name="open-the-sql-database-query-editor"></a>SQL veritabanı sorgu düzenleyicisini açın

1. [Azure Portal](https://portal.azure.com/) oturum açın ve sorgulamak istediğiniz SQL veritabanını seçin.

2. **SQL veritabanı** menüsünde **sorgu Düzenleyicisi 'ni (Önizleme)** seçin.

    ![sorgu düzenleyicisini bulma](./media/sql-database-connect-query-portal/find-query-editor.PNG)


## <a name="establish-a-connection-to-the-database"></a>Veritabanıyla bağlantı kurun

Portalda oturum açmış olsanız da, SQL veritabanına erişmek için kimlik bilgilerini sağlamanız gerekir. Veritabanınıza bağlanmak için, SQL kimlik doğrulaması veya Azure Active Directory kullanarak bağlanabilirsiniz.

### <a name="connect-using-sql-authentication"></a>SQL Kimlik Doğrulaması kullanarak bağlanma

1. **Oturum açma** sayfasında, **SQL Server kimlik doğrulaması**' nın altında, veritabanına erişimi olan bir Kullanıcı Için **oturum açma** ve **parola** girin. Emin değilseniz, veritabanının sunucusunun sunucu yöneticisi için oturum açma ve parolayı kullanın.

    ![oturum aç](./media/sql-database-connect-query-portal/login-menu.png)

2. **Tamam**’ı seçin.


### <a name="connect-using-azure-active-directory"></a>Azure Active Directory kullanarak bağlan

Azure Active Directory (Azure AD) Yöneticisi yapılandırmak, Azure portal ve SQL veritabanınızda oturum açmak için tek bir kimlik kullanmanıza olanak sağlar. Azure AD 'yi kullanarak veritabanınıza bağlanmak için, SQL Server için bir Azure AD yöneticisi yapılandırmak üzere aşağıdaki adımları izleyin.

> [!NOTE]
> * E-posta hesapları (örneğin, outlook.com, gmail.com, yahoo.com, vb.) henüz Azure AD yöneticileri olarak desteklenmez. Bir kullanıcı ya da yerel olarak Azure AD'de oluşturulmuş veya Azure AD ile Federasyon seçtiğinizden emin olun.
> * Azure AD Yöneticisi'ni açın 2 faktörlü kimlik doğrulaması etkin olan hesapları ile çalışmaz.

#### <a name="set-an-active-directory-admin-for-the-database-server"></a>Veritabanı sunucusu için Active Directory yöneticisini ayarlama

1. Azure portal, SQL Server 'nizi seçin.

2. **SQL Server** menüsünde **Active Directory yönetici**' yi seçin.

3. SQL Server **Active Directory yönetici** sayfası araç çubuğunda **yönetici ayarla** ' yı seçin ve Azure AD yöneticinizle Kullanıcı veya grup ' u seçin.

    ![active directory seçme](./media/sql-database-connect-query-portal/select-active-directory.png)

4. **Yönetici Ekle** sayfasında, ara kutusuna bulunacak Kullanıcı veya grubu girin, yönetici olarak seçin ve ardından **Seç** düğmesini seçin.

5. SQL Server **Active Directory yönetici** sayfası araç çubuğuna geri dönerek **Kaydet**' i seçin.

### <a name="connect-to-the-database"></a>Veritabanına bağlanın

6. **SQL Server** menüsünde **SQL veritabanları**' nı seçin ve ardından SQL veritabanınızı seçin.

7. **SQL veritabanı** menüsünde **sorgu Düzenleyicisi 'ni (Önizleme)** seçin. **Oturum açma** sayfasında, **Active Directory kimlik doğrulama** etiketi altında, bir Azure AD yöneticisiyseniz oturum açmış olduğunuzu bildiren bir ileti görüntülenir. Sonra, *Kullanıcı veya grup kimliği > düğmesini\<* **olarak devam et** ' i seçin. Sayfada başarıyla oturum açmadıysanız, sayfayı yenilemeniz gerekebilir.

## <a name="query-a-sql-database"></a>Bir SQL veritabanını sorgulama

Aşağıdaki örnek sorgular AdventureWorksLT örnek veritabanında başarıyla çalışmalıdır.

### <a name="run-a-select-query"></a>Seçme sorgusu çalıştırma

1. Aşağıdaki sorguyu sorgu düzenleyicisine yapıştırın:

   ```sql
    SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
    FROM SalesLT.ProductCategory pc
    JOIN SalesLT.Product p
    ON pc.productcategoryid = p.productcategoryid;
   ```

2. **Çalıştır** ' ı seçin ve ardından **sonuçlar** bölmesinde çıktıyı gözden geçirin.

   ![sorgu düzenleyicisi sonuçları](./media/sql-database-connect-query-portal/query-editor-results.png)

3. İsteğe bağlı olarak, sorguyu bir. SQL dosyası olarak kaydedebilir veya döndürülen verileri bir. JSON,. csv veya. xml dosyası olarak dışarı aktarabilirsiniz.

### <a name="run-an-insert-query"></a>EKLEME sorgusu Çalıştır

`SalesLT.Product` tablosuna yeni bir ürün eklemek için aşağıdaki [Insert](/sql/t-sql/statements/insert-transact-sql/) T-SQL ifadesini çalıştırın.

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


2. `Product` tablosuna yeni bir satır eklemek için **Çalıştır** ' ı seçin. **İletiler** bölmesi **sorgu başarılı: etkilenen satırlar: 1 ' i**görüntüler.


### <a name="run-an-update-query"></a>GÜNCELLEŞTIRME sorgusu çalıştırma

Yeni ürününüzü değiştirmek için aşağıdaki [Update](/sql/t-sql/queries/update-transact-sql/) T-SQL ifadesini çalıştırın.

1. Önceki sorguyu Bununla değiştirin.

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. `Product` tablosundaki belirtilen satırı güncelleştirmek için **Çalıştır** ' ı seçin. **İletiler** bölmesi **sorgu başarılı: etkilenen satırlar: 1 ' i**görüntüler.

### <a name="run-a-delete-query"></a>SILME sorgusu Çalıştır

Yeni ürününüzü kaldırmak için aşağıdaki [Delete](/sql/t-sql/statements/delete-transact-sql/) T-SQL ifadesini çalıştırın.

1. Önceki sorguyu Bununla değiştirin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. `Product` tablosundaki belirtilen satırı silmek için **Çalıştır** ' ı seçin. **İletiler** bölmesi **sorgu başarılı: etkilenen satırlar: 1 ' i**görüntüler.


## <a name="query-editor-considerations"></a>Sorgu Düzenleyicisi konuları

Sorgu Düzenleyicisi ile çalışırken bilmeniz gereken birkaç nokta vardır.

* Sorgu Düzenleyicisi, iletişim kurmak için 443 ve 1443 bağlantı noktalarını kullanır. Bu bağlantı noktalarında giden HTTPS trafiğini etkinleştirmiş olduğunuzdan emin olun. Ayrıca, veritabanlarına ve veri ambarlarınıza erişmek için giden IP adresinizi sunucunun izin verilen güvenlik duvarı kurallarına eklemeniz gerekir.

* Sorgu Düzenleyicisi, Istemci IP adresini SQL veritabanı güvenlik duvarı 'na eklemeye gerek kalmadan özel bağlantıyla birlikte çalışmaktadır

* **F5** tuşuna basıldığında sorgu Düzenleyicisi sayfası yenilenir ve üzerinde çalışılan sorgular kaybolur.

* Sorgu Düzenleyicisi `master` veritabanına bağlanmayı desteklemiyor.

* Sorgu yürütme için 5 dakikalık zaman aşımı yoktur.

* Sorgu Düzenleyicisi, yalnızca coğrafi veri türleri için Silindirik izdüşümü destekler.

* Veritabanı tabloları ve görünümleri için IntelliSense desteği yoktur, ancak düzenleyici zaten girilmiş adlarla otomatik tamamlamayı destekler.




## <a name="next-steps"></a>Sonraki adımlar

Azure SQL veritabanlarında desteklenen Transact-SQL (T-SQL) hakkında daha fazla bilgi edinmek için bkz. [SQL veritabanına geçiş sırasında Transact-SQL farklılıklarını çözümleme](sql-database-transact-sql-information.md).
