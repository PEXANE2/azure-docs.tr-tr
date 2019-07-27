---
title: "VS Code: Azure SQL veritabanı 'nda verileri bağlama ve sorgulama | Microsoft Docs"
description: Visual Studio Code kullanarak SQL Veritabanına bağlanma hakkında bilgi edinin. Ardından, verileri sorgulamak ve düzenlemek için Transact-SQL (T-SQL) deyimleri çalıştırın.
keywords: sql veritabanına bağlanma
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/25/2019
ms.openlocfilehash: d8f12e699c17787d897a7f5ed23eccdbf3659921
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569128"
---
# <a name="quickstart-use-visual-studio-code-to-connect-and-query-an-azure-sql-database"></a>Hızlı Başlangıç: Azure SQL veritabanına bağlanmak ve veritabanını sorgulamak için Visual Studio Code kullanma

[Visual Studio Code](https://code.visualstudio.com/docs) Linux, macOS ve Windows için bir grafiksel kod düzenleyicisidir. Dahil olmak üzere uzantıları destekleyen [mssql uzantısı](https://aka.ms/mssql-marketplace) Microsoft SQL Server, Azure SQL veritabanı ve SQL veri ambarı'nı sorgulamak için. Bu hızlı başlangıçta, Azure SQL veritabanına bağlanmak ve ardından Transact-SQL deyimlerini kullanarak verileri sorgulamak, eklemek, güncelleştirmek ve silmek için Visual Studio Code kullanacaksınız.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure SQL veritabanı. Azure SQL veritabanı 'nda bir veritabanı oluşturmak ve yapılandırmak için bu hızlı başlangıçlardan birini kullanabilirsiniz:

  || Tek veritabanı | Yönetilen örnek |
  |:--- |:--- |:---|
  | Create| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Yapılandırma | [Sunucu düzeyi IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)| [Bir VM 'den bağlantı](sql-database-managed-instance-configure-vm.md)|
  |||[Siteden bağlantı](sql-database-managed-instance-configure-p2s.md)
  |Veri yükleme|Hızlı başlangıç başına yüklenen Adventure Works|[Geniş dünyada içeri aktarıcılar geri yükleme](sql-database-managed-instance-get-started-restore.md)
  |||[GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 'Dan [bacpac](sql-database-import.md) dosyasından Adventure Works 'ü geri yükleme veya içe aktarma|
  |||

  > [!IMPORTANT]
  > Bu makaledeki betikler, Adventure Works veritabanını kullanmak için yazılmıştır. Yönetilen bir örnek ile, Adventure Works veritabanını bir örnek veritabanına aktarmanız veya bu makaledeki betikleri Wide World Importers veritabanını kullanacak şekilde değiştirmeniz gerekir.

## <a name="install-visual-studio-code"></a>Visual Studio Kodu'nu yükle

En son yüklediğinizden emin olun [Visual Studio Code](https://code.visualstudio.com/Download) yüklenip yüklenmediğini [mssql uzantısı](https://aka.ms/mssql-marketplace). Mssql uzantısı yükleme hakkında yönergeler için bkz. [VS Code yükleme](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-and-start-visual-studio-code) ve [Visual Studio Code için mssql ](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql).

## <a name="configure-visual-studio-code"></a>Visual Studio Code'u yapılandırma

### <a name="mac-os"></a>**Mac OS**

MacOS için, MSSQL uzantısının kullandığı .NET Core için bir önkoşul olan OpenSSL 'yi yüklemeniz gerekir. **brew** ve **OpenSSL**’yi yüklemek için terminalinizi açın aşağıdaki komutları girin.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Hiçbir özel yapılandırma gerekmez.

### <a name="windows"></a>**Windows**

Hiçbir özel yapılandırma gerekmez.

## <a name="get-sql-server-connection-information"></a>SQL Server bağlantı bilgilerini al

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adı veya ana bilgisayar adı, veritabanı adı ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. **SQL veritabanları** veya **SQL yönetilen örnekler** sayfasına gidin.

3. **Genel bakış** sayfasında, tek bir veritabanı için **sunucu adı** ' nın yanında tam sunucu adını veya yönetilen örnek Için **ana bilgisayar ' ın** yanındaki tam sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine gelin ve **Kopyala** simgesini seçin.

## <a name="set-language-mode-to-sql"></a>Dili modunu SQL’e ayarlama

Dil modunu Visual Studio Code'da kümesine **SQL** mssql komutlarını ve T-SQL IntelliSense'i etkinleştirmek için.

1. Yeni bir Visual Studio Code penceresi açın.

2. **CTRL**+**N**tuşuna basın. Yeni bir düz metin dosyası açılır.

3. Seçin **düz metin** durum çubuğunun sağ alt köşesindeki içinde.

4. İçinde **dil modu Seç** açıldığında seçin açılan menüsü **SQL**.

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

Visual Studio Code’u kullanarak Azure SQL Veritabanı sunucunuzla bağlantı kurun.

> [!IMPORTANT]
> Devam etmeden önce, sunucunuzun ve oturum açma bilgilerinin hazırlanmasına sahip olduğunuzdan emin olun. Odağınızı Visual Studio Code'dan değiştirirseniz bağlantı profili bilgilerini girmeye başladıktan sonra profil oluşturma işlemini yeniden başlatmanız gerekir.

1. Visual Studio Code'da basın **Ctrl + Shift + P** (veya **F1**) komut paletini açın.

2. **MS SQL: Connect** ' i seçin ve **ENTER**' u seçin.

3. Seçin **bağlantı profili oluşturmak**.

4. Yeni profilinin bağlantı özelliklerini belirtmek için istemleri izleyin. Her bir değeri belirttikten sonra, devam etmek için **ENTER** ' ı seçin.

   | Özellik       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Sunucu adı** | Tam sunucu adı | Aşağıdakine benzer: **mynewserver20170313.database.windows.net**. |
   | **Veritabanı adı** | mySampleDatabase | Bağlanmak için veritabanı. |
   | **Kimlik doğrulaması** | SQL Oturum Açma| Bu öğreticide, SQL kimlik doğrulaması kullanır. |
   | **Kullanıcı adı** | Kullanıcı adı | Sunucu oluşturmak için kullanılan sunucu yönetici hesabı kullanıcı adı. |
   | **Parola (SQL Oturum Açma)** | Parola | Sunucu oluşturmak için kullanılan sunucu yönetici hesabı parolası. |
   | **Parola kaydedilsin mi?** | Evet veya Hayır | Seçin **Evet** her defasında parolayı girmek istemiyorsanız. |
   | **Bu profil için bir ad girin** | Gibi bir profil adı **mySampleProfile** | Kaydedilen bir profilin bağlantınızı sonraki oturum açma hızlandırır. |

   Başarılı olursa, profilin oluşturulup bağlandığını söyleyen bir bildirim görüntülenir.

## <a name="query-data"></a>Verileri sorgulama

Kategoriye göre ilk 20 ürünü sorgulamak için aşağıdaki [Select](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL ifadesini çalıştırın.

1. Düzenleyicisi penceresinde, aşağıdaki SQL sorgusunu yapıştırın.

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. Sorguyu +çalıştırmak ve `Product` ve+ tablolarından`ProductCategory` sonuçları göstermek için CTRL SHIFT**E** tuşlarına basın.

    ![2 tablolarından veri almak için sorgu](./media/sql-database-connect-query-vscode/query.png)

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

2. Tuşuna **Ctrl**+**Shift**+**E** yeni bir satır eklemek için `Product` tablo.

## <a name="update-data"></a>Verileri güncelleştirme

Eklenen ürünü güncelleştirmek için aşağıdaki [Update](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL ifadesini çalıştırın.

1. Önceki sorguyu Bununla değiştirin:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Tuşuna **Ctrl**+**Shift**+**E** belirtilen satırı güncelleştirmek için `Product` tablo.

## <a name="delete-data"></a>Verileri silme

Yeni ürünü kaldırmak için aşağıdaki [Delete](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) Transact-SQL ifadesini çalıştırın.

1. Önceki sorguyu Bununla değiştirin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Tuşuna **Ctrl**+**Shift**+**E** belirtilen satırı silmek için `Product` tablo.

## <a name="next-steps"></a>Sonraki adımlar

- SQL Server Management Studio kullanarak bağlanmak ve sorgulamak için bkz [. hızlı başlangıç: Azure SQL veritabanına bağlanmak ve verileri](sql-database-connect-query-ssms.md)sorgulamak için SQL Server Management Studio kullanın.
- Azure Portal kullanarak bağlanmak ve sorgulamak için bkz [. hızlı başlangıç: Bağlanmak ve veri](sql-database-connect-query-portal.md)sorgulamak Için Azure Portal SQL sorgu düzenleyicisini kullanın.
- Visual Studio Code'u kullanmaya ilişkin MSDN dergisi makalesi için bkz. [MSSQL uzantısı blog gönderisinden yararlanarak veritabanı IDE'si oluşturma](https://msdn.microsoft.com/magazine/mt809115).
