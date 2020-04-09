---
title: Azure portalındaki sorgu düzenleyicisini kullanarak SQL Veritabanı'nı sorgula
description: Bir Azure SQL Veritabanı'na karşı Transact-SQL (T-SQL) sorgularını çalıştırmak için Sorgu Düzenleyicisi'ni nasıl kullanacağınızı öğrenin.
keywords: sql veritabanına bağlanmak,query sql veritabanı, azure portal, portal, sorgu düzenleyicisi
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
ms.openlocfilehash: c072161db7a477b7973571a18d4f686b1b9c6202
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985686"
---
# <a name="quickstart-use-the-azure-portals-query-editor-to-query-a-sql-database"></a>Hızlı başlatma: SQL veritabanını sorgulamak için Azure portalının sorgu düzenleyicisini kullanma

Sorgu düzenleyicisi, Azure SQL veritabanınıza veya Azure SQL Veri Ambarı'na karşı SQL sorguları çalıştırmak için Azure portalında bir araçtır. 

Bu hızlı başlatmada, transact-SQL (T-SQL) sorgularını bir Azure SQL veritabanına karşı çalıştırmak için sorgu düzenleyicisini kullanırsınız.


## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlatmayı tamamlamak için AdventureWorksLT örnek veritabanı nı gerektirir. AdventureWorksLT SQL veritabanının çalışan bir kopyasına sahip değilseniz, aşağıdaki hızlı başlatma hızlı bir şekilde bir tane oluşturur:

- [Hızlı başlatma: Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak tek bir Azure SQL veritabanı oluşturun](sql-database-single-database-get-started.md) 

### <a name="configure-network-settings"></a>Ağ ayarlarını yapılandırma

Sorgu düzenleyicisinde aşağıdaki hatalardan birini alırsanız: *Yerel ağ ayarlarınız Sorgu Düzenleyicisi'nin sorgu vermesini engelliyor olabilir. Ağ ayarlarınızı nasıl yapılandıracağınıza*veya *sunucuya bağlantı kurulamazken nasıl yapılandırılabileceğine ilişkin talimatlar için lütfen buraya tıklayın. Bu, yerel güvenlik duvarı yapılandırmanızla veya ağ proxy ayarlarınızla ilgili bir sorunu gösterebilir,* aşağıdaki önemli bilgiler çözüme yardımcı olacaktır:

> [!IMPORTANT]
> Sorgu düzenleyicisi iletişim kurmak için 443 ve 1443 bağlantı noktalarını kullanır. Bu bağlantı noktalarında giden HTTPS trafiğini etkinleştirdiğinizden emin olun. Ayrıca, veritabanlarınıza ve veri ambarlarınıza erişmek [için giden IP adresinizi sunucunun izin verilen güvenlik duvarı kurallarına eklemeniz](sql-database-server-level-firewall-rule.md) gerekir.


## <a name="open-the-sql-database-query-editor"></a>SQL Veritabanı Sorgu Düzenleyicisini Aç

1. [Azure portalında](https://portal.azure.com/) oturum açın ve sorgulamak istediğiniz SQL veritabanını seçin.

2. SQL **veritabanı** menüsünde **Sorgu düzenleyicisini (önizleme)** seçin.

    ![sorgu düzenleyicisini bulma](./media/sql-database-connect-query-portal/find-query-editor.PNG)


## <a name="establish-a-connection-to-the-database"></a>Veritabanına bağlantı kurma

Portalda oturum açmış olsanız bile, SQL veritabanına erişmek için kimlik bilgileri sağlamanız gerekir. Veritabanınıza bağlanmak için SQL kimlik doğrulaması veya Azure Active Directory kullanarak bağlanabilirsiniz.

### <a name="connect-using-sql-authentication"></a>SQL Kimlik Doğrulaması kullanarak bağlanma

1. **Giriş** sayfasında, SQL **sunucu kimlik doğrulaması**altında, veritabanına erişimi olan bir kullanıcı için bir **Giriş** ve **Parola** girin. Emin değilseniz, veritabanı sunucusunun Sunucu yöneticisi için giriş ve parolayı kullanın.

    ![oturum aç](./media/sql-database-connect-query-portal/login-menu.png)

2. **Tamam'ı**seçin.


### <a name="connect-using-azure-active-directory"></a>Azure Etkin Dizini kullanarak bağlanma

Azure Etkin Dizin (Azure AD) yöneticisini yapılandırmak, Azure portalında ve SQL veritabanınızda oturum açabilmek için tek bir kimlik kullanmanıza olanak tanır. Azure AD'yi kullanarak veritabanınıza bağlanmak için, SQL sunucunuz için bir Azure AD yöneticisi ni yapılandırmak için aşağıdaki adımları izleyin.

> [!NOTE]
> * E-posta hesapları (örneğin, outlook.com, gmail.com, yahoo.com vb.) henüz Azure AD yöneticileri olarak desteklenmez. Azure AD'de yerel olarak oluşturulmuş veya Azure AD'de federe bir kullanıcı seçtiğinizden emin olun.
> * Azure AD yöneticisi oturum açma özelliği, 2 faktörlü kimlik doğrulaması etkinleştirilmiş hesaplarla çalışmaz.

#### <a name="set-an-active-directory-admin-for-the-database-server"></a>Veritabanı sunucusu için Active Directory yöneticisi ayarlama

1. Azure portalında SQL sunucunuzu seçin.

2. SQL **sunucu** menüsünde **Active Directory yöneticisini**seçin.

3. SQL sunucusu **Active Directory admin** sayfa araç çubuğunda, **yöneticiyi ayarla'yı** seçin ve Azure AD yöneticiniz olarak kullanıcıyı veya grubu seçin.

    ![active directory seçme](./media/sql-database-connect-query-portal/select-active-directory.png)

4. Yönetici **Ekle** sayfasında, arama kutusuna bir kullanıcı veya grup girin, onu yönetici olarak seçin ve sonra **Seç** düğmesini seçin.

5. SQL **sunucusuActive Directory admin** sayfası araç çubuğunda **Kaydet'i**seçin.

### <a name="connect-to-the-database"></a>Veritabanına bağlanın

6. SQL **sunucu** menüsünde, **SQL veritabanlarını**seçin ve ardından SQL veritabanınızı seçin.

7. SQL **veritabanı** menüsünde **Sorgu düzenleyicisini (önizleme)** seçin. Active **Directory kimlik doğrulama** etiketi altında **oturum açma** sayfasında, Azure AD yöneticisiyseniz oturum açmış olduğunuzu belirten bir ileti görüntülenir. Ardından * \<kullanıcınız olarak* **Continue** veya grup kimliği>düğmesini seçin. Sayfa, başarılı bir şekilde oturum açmadığınızı gösteriyorsa, sayfayı yenilemeniz gerekebilir.

## <a name="query-a-sql-database"></a>SQL veritabanını sorgula

Aşağıdaki örnek sorgular AdventureWorksLT örnek veritabanına karşı başarıyla çalıştırılmalıdır.

### <a name="run-a-select-query"></a>SELECT sorgusunu çalıştırma

1. Aşağıdaki sorguyu sorgu düzenleyicisine yapıştırın:

   ```sql
    SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
    FROM SalesLT.ProductCategory pc
    JOIN SalesLT.Product p
    ON pc.productcategoryid = p.productcategoryid;
   ```

2. **Çalıştır'ı** seçin ve **ardından Sonuçlar** bölmesindeki çıktıyı gözden geçirin.

   ![sorgu düzenleyicisi sonuçları](./media/sql-database-connect-query-portal/query-editor-results.png)

3. İsteğe bağlı olarak, sorguyu .sql dosyası olarak kaydedebilir veya döndürülen verileri .json, .csv veya .xml dosyası olarak dışa aktarabilirsiniz.

### <a name="run-an-insert-query"></a>INSERT sorgusunu çalıştırma

Tabloya yeni bir ürün eklemek için aşağıdaki [INSERT](/sql/t-sql/statements/insert-transact-sql/) T-SQL deyimini çalıştırın. `SalesLT.Product`

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


2. Tabloya yeni bir satır eklemek `Product` için **Çalıştır'ı** seçin. **İletiler** bölmesi **Sorgu'yu görüntüledi: Etkilenen satırlar: 1**.


### <a name="run-an-update-query"></a>UPDATE sorgusunu çalıştırma

Yeni ürününüzü değiştirmek için aşağıdaki [UPDATE](/sql/t-sql/queries/update-transact-sql/) T-SQL deyimini çalıştırın.

1. Önceki sorguyu bununla değiştirin.

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Tabloda belirtilen satırı güncelleştirmek `Product` için **Çalıştır'ı** seçin. **İletiler** bölmesi **Sorgu'yu görüntüledi: Etkilenen satırlar: 1**.

### <a name="run-a-delete-query"></a>DELETE sorgusunu çalıştırma

Yeni ürününüzü kaldırmak için aşağıdaki [DELETE](/sql/t-sql/statements/delete-transact-sql/) T-SQL deyimini çalıştırın.

1. Önceki sorguyu bununla değiştirin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Tabloda belirtilen satırı silmek `Product` için **Çalıştır'ı** seçin. **İletiler** bölmesi **Sorgu'yu görüntüledi: Etkilenen satırlar: 1**.


## <a name="query-editor-considerations"></a>Sorgu düzenleyicisi hususlar

Sorgu düzenleyicisi ile çalışırken bilmeniz gereken birkaç şey vardır.

* Sorgu düzenleyicisi iletişim kurmak için 443 ve 1443 bağlantı noktalarını kullanır. Bu bağlantı noktalarında giden HTTPS trafiğini etkinleştirdiğinizden emin olun. Ayrıca, veritabanlarınıza ve veri ambarlarınıza erişmek için giden IP adresinizi sunucunun izin verilen güvenlik duvarı kurallarına eklemeniz gerekir.

* Özel Bağlantı bağlantınız varsa, Sorgu Düzenleyicisi Istemci Ip adresini SQL Veritabanı güvenlik duvarına eklemeye gerek kalmadan çalışır

* **F5 tuşuna** basıldığında sorgu düzenleyicisi sayfası yenilenir ve üzerinde çalışılan tüm sorgu kaybolur.

* Sorgu düzenleyicisi veritabanına bağlanmayı desteklemez. `master`

* Sorgu yürütme için 5 dakikalık bir zaman dilimi vardır.

* Sorgu düzenleyicisi yalnızca coğrafya veri türleri için silindirik projeksiyonu destekler.

* Veritabanı tabloları ve görünümleri için IntelliSense desteği yoktur, ancak düzenleyici zaten yazılmış adlar üzerinde otomatik tamamlamayı destekler.




## <a name="next-steps"></a>Sonraki adımlar

Azure SQL veritabanlarında desteklenen Transact-SQL (T-SQL) hakkında daha fazla bilgi edinmek [için, SQL Veritabanı'na geçiş sırasında Transact-SQL farklılıklarını çözme'ye](sql-database-transact-sql-information.md)bakın.
