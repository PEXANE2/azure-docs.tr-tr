---
title: Azure portal sorgu düzenleyicisini kullanarak bir SQL veritabanını sorgulama
description: Azure SQL veritabanı 'ndaki bir veritabanında Transact-SQL (T-SQL) sorgularını çalıştırmak için sorgu düzenleyicisini nasıl kullanacağınızı öğrenin.
titleSuffix: Azure SQL Database
keywords: SQL veritabanı 'na bağlanma, SQL veritabanı, Azure Portal, Portal, sorgu Düzenleyicisi 'ni sorgulama
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: quickstart
author: Ninarn
ms.author: ninarn
ms.reviewer: carlrab
ms.date: 05/29/2020
ms.openlocfilehash: 4dbbf8267b4a7a1ea280d16ee787c8ae6cd652c9
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84267367"
---
# <a name="quickstart-use-the-azure-portals-query-editor-to-query-an-azure-sql-database"></a>Hızlı başlangıç: Azure SQL veritabanını sorgulamak için Azure portal sorgu düzenleyicisini kullanma
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Sorgu Düzenleyicisi, Azure SQL veritabanı veya Azure SQL veri ambarı 'nda veri ambarı içindeki veritabanınıza SQL sorguları çalıştırmak için Azure portal bir araçtır. 

Bu hızlı başlangıçta, bir veritabanında Transact-SQL (T-SQL) sorgularını çalıştırmak için sorgu düzenleyicisini kullanacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak AdventureWorksLT örnek veritabanı gerektirir. SQL veritabanında AdventureWorksLT örnek veritabanının çalışan bir kopyasına sahip değilseniz aşağıdaki hızlı başlangıçta bir tane oluşturulur:

- [Hızlı başlangıç: Azure portal, PowerShell veya Azure CLı kullanarak Azure SQL veritabanı 'nda veritabanı oluşturma](single-database-create-quickstart.md) 

### <a name="configure-network-settings"></a>Ağ ayarlarını yapılandırma

Sorgu düzenleyicisinde aşağıdaki hatalardan birini alırsanız: *Yerel Ağ ayarlarınız sorgu düzenleyicisinin sorguları vermesini engelleyebilir. Ağ ayarlarınızı yapılandırma yönergeleri için lütfen buraya tıklayın*veya *sunucu bağlantısı kurulamadı. Bu, yerel güvenlik duvarı yapılandırmanızla veya ağ ara sunucu ayarlarınızda bir sorun olduğunu gösterebilir*ve aşağıdaki önemli bilgiler çözümlenmelidir:

> [!IMPORTANT]
> Sorgu Düzenleyicisi, iletişim kurmak için 443 ve 1443 bağlantı noktalarını kullanır. Bu bağlantı noktalarında giden HTTPS trafiğini etkinleştirmiş olduğunuzdan emin olun. Ayrıca, veritabanlarına ve veri ambarlarınıza erişmek için, [gıden IP adresinizi sunucunun izin verilen güvenlik duvarı kurallarına eklemeniz](firewall-create-server-level-portal-quickstart.md) gerekir.


## <a name="open-the-sql-database-query-editor"></a>SQL veritabanı sorgu düzenleyicisini açın

1. [Azure Portal](https://portal.azure.com/) oturum açın ve sorgulamak istediğiniz veritabanını seçin.

2. **SQL veritabanı** menüsünde **sorgu Düzenleyicisi 'ni (Önizleme)** seçin.

    ![sorgu düzenleyicisini bulma](./media/connect-query-portal/find-query-editor.PNG)


## <a name="establish-a-connection-to-the-database"></a>Veritabanıyla bağlantı kurun

Portalda oturum açmış olsanız da, veritabanına erişmek için kimlik bilgilerini sağlamanız gerekir. Veritabanınıza bağlanmak için, SQL kimlik doğrulaması veya Azure Active Directory kullanarak bağlanabilirsiniz.

### <a name="connect-using-sql-authentication"></a>SQL Kimlik Doğrulaması kullanarak bağlanma

1. **Oturum açma** sayfasında, **SQL Server kimlik doğrulaması**' nın altında, veritabanına erişimi olan bir Kullanıcı Için **oturum açma** ve **parola** girin. Emin değilseniz, veritabanının sunucusunun sunucu yöneticisi için oturum açma ve parolayı kullanın.

    ![oturum aç](./media/connect-query-portal/login-menu.png)

2. **Tamam**’ı seçin.


### <a name="connect-using-azure-active-directory"></a>Azure Active Directory kullanarak bağlanma

Azure Active Directory (Azure AD) Yöneticisi yapılandırmak, Azure portal ve veritabanınızda oturum açmak için tek bir kimlik kullanmanıza olanak sağlar. Azure AD 'yi kullanarak veritabanınıza bağlanmak için, SQL Server Örneğiniz için bir Azure AD yöneticisi yapılandırmak üzere aşağıdaki adımları izleyin.

> [!NOTE]
> * E-posta hesapları (örneğin, outlook.com, gmail.com, yahoo.com, vb.) henüz Azure AD yöneticileri olarak desteklenmez. Azure AD 'de yerel olarak oluşturulmuş veya Azure AD 'de Federasyon oluşturulmuş bir Kullanıcı seçtiğinizden emin olun.
> * Azure AD yönetici oturum açma özelliği, 2 öğeli kimlik doğrulaması etkinleştirilmiş hesaplarla çalışmaz.

#### <a name="set-an-active-directory-admin-for-the-server"></a>Sunucu için Active Directory yöneticisini ayarlama

1. Azure portal, SQL Server örneğinizi seçin.

2. **SQL Server** menüsünde **Active Directory yönetici**' yi seçin.

3. SQL Server **Active Directory yönetici** sayfası araç çubuğunda **yönetici ayarla** ' yı seçin ve KULLANıCıYı veya grubu Azure AD yöneticiniz olarak seçin.

    ![active directory seçme](./media/connect-query-portal/select-active-directory.png)

4. **Yönetici Ekle** sayfasında, ara kutusuna bulunacak Kullanıcı veya grubu girin, yönetici olarak seçin ve ardından **Seç** düğmesini seçin.

5. SQL Server **Active Directory yönetici** sayfası araç çubuğuna geri döndüğünüzde **Kaydet**' i seçin.

### <a name="connect-to-the-database"></a>Veritabanına bağlanın

6. **SQL Server** menüsünde **SQL veritabanları**' nı seçin ve ardından veritabanınızı seçin.

7. **SQL veritabanı** menüsünde **sorgu Düzenleyicisi 'ni (Önizleme)** seçin. **Oturum açma** sayfasında, **Active Directory kimlik doğrulama** etiketi altında, bir Azure AD yöneticisiyseniz oturum açmış olduğunuzu bildiren bir ileti görüntülenir. Sonra **farklı devam et** *\<your user or group ID>* düğmesini seçin. Sayfada başarıyla oturum açmadıysanız, sayfayı yenilemeniz gerekebilir.

## <a name="query-a-database-in-sql-database"></a>SQL veritabanında bir veritabanını sorgulama

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

   ![sorgu düzenleyicisi sonuçları](./media/connect-query-portal/query-editor-results.png)

3. İsteğe bağlı olarak, sorguyu bir. SQL dosyası olarak kaydedebilir veya döndürülen verileri bir. JSON,. csv veya. xml dosyası olarak dışarı aktarabilirsiniz.

### <a name="run-an-insert-query"></a>EKLEME sorgusu Çalıştır

Tabloya yeni bir ürün eklemek için aşağıdaki [Insert](/sql/t-sql/statements/insert-transact-sql/) T-SQL ifadesini çalıştırın `SalesLT.Product` .

1. Önceki sorguyu bununla değiştirin.

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


2. Tabloya yeni bir satır eklemek için **Çalıştır** ' ı seçin `Product` . **İletiler** bölmesi **sorgu başarılı: etkilenen satırlar: 1 ' i**görüntüler.


### <a name="run-an-update-query"></a>GÜNCELLEŞTIRME sorgusu çalıştırma

Yeni ürününüzü değiştirmek için aşağıdaki [Update](/sql/t-sql/queries/update-transact-sql/) T-SQL ifadesini çalıştırın.

1. Önceki sorguyu bununla değiştirin.

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Tablodaki belirtilen satırı güncelleştirmek için **Çalıştır** ' ı seçin `Product` . **İletiler** bölmesi **sorgu başarılı: etkilenen satırlar: 1 ' i**görüntüler.

### <a name="run-a-delete-query"></a>SILME sorgusu Çalıştır

Yeni ürününüzü kaldırmak için aşağıdaki [Delete](/sql/t-sql/statements/delete-transact-sql/) T-SQL ifadesini çalıştırın.

1. Önceki sorguyu bununla değiştirin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Tablodaki belirtilen satırı silmek için **Çalıştır** ' ı seçin `Product` . **İletiler** bölmesi **sorgu başarılı: etkilenen satırlar: 1 ' i**görüntüler.


## <a name="query-editor-considerations"></a>Sorgu Düzenleyicisi konuları

Sorgu Düzenleyicisi ile çalışırken bilmemiz gereken birkaç nokta vardır.

* Sorgu Düzenleyicisi, iletişim kurmak için 443 ve 1443 bağlantı noktalarını kullanır. Bu bağlantı noktalarında giden HTTPS trafiğini etkinleştirmiş olduğunuzdan emin olun. Ayrıca, veritabanlarına ve veri ambarlarınıza erişmek için giden IP adresinizi sunucunun izin verilen güvenlik duvarı kurallarına eklemeniz gerekir.

* Özel bir bağlantı bağlantınız varsa, sorgu Düzenleyicisi, Istemci IP adresini SQL veritabanı güvenlik duvarı 'na eklemeye gerek kalmadan çalışmaktadır.

* **F5** tuşuna basıldığında sorgu Düzenleyicisi sayfası yenilenir ve üzerinde çalışılan sorgular kaybolur.

* Sorgu Düzenleyicisi, veritabanına bağlanmayı desteklemiyor `master` .

* Sorgu yürütmesi için 5 dakikalık bir zaman aşımı vardır.

* Sorgu Düzenleyicisi, Coğrafya veri türleri için yalnızca silindir projeksiyonu destekler.

* Veritabanı tabloları ve görünümleri için IntelliSense desteği yoktur, ancak düzenleyici zaten girilmiş adlarla otomatik tamamlamayı destekler.




## <a name="next-steps"></a>Sonraki adımlar

Azure SQL veritabanı 'nda desteklenen Transact-SQL (T-SQL) hakkında daha fazla bilgi edinmek için bkz. [SQL veritabanına geçiş sırasında Transact-SQL farklılıklarını çözümleme](transact-sql-tsql-differences-sql-server.md).
