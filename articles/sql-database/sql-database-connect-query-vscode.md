---
title: Bağlanmak ve sorgulamak için VS Kodunu kullanma
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
ms.openlocfilehash: 28b35f273783b2e4d0b8f59c5bc5be384b933ba2
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73826886"
---
# <a name="quickstart-use-visual-studio-code-to-connect-and-query-an-azure-sql-database"></a>Quickstart: Azure SQL Veritabanı'na bağlanmak ve sorgulamak için Visual Studio Kodunu kullanın

[Visual Studio Code,](https://code.visualstudio.com/docs) Linux, macOS ve Windows için bir grafik kod düzenleyicisidir. Microsoft SQL Server, Azure SQL Veritabanı ve SQL Veri Ambarı sorgulamaiçin [mssql uzantısı](https://aka.ms/mssql-marketplace) da dahil olmak üzere uzantıları destekler. Bu hızlı başlatmada, Visual Studio Code'u kullanarak Azure SQL veritabanına bağlanır ve ardından verileri sorgulamak, eklemek, güncelleştirmek ve silmek için Transact-SQL deyimlerini çalıştırırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure SQL veritabanı. Azure SQL Veritabanı'nda bir veritabanı oluşturmak ve yapılandırmak için bu hızlı başlangıçlardan birini kullanabilirsiniz:

  || Tek veritabanı | Yönetilen örnek |
  |:--- |:--- |:---|
  | Oluşturma| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [Powershell](scripts/sql-database-create-and-configure-database-powershell.md) | [Powershell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Yapılandırma | [Sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)| [VM'den bağlantı](sql-database-managed-instance-configure-vm.md)|
  |||[Siteden bağlantı](sql-database-managed-instance-configure-p2s.md)
  |Veri yükleme|Adventure Works quickstart başına yüklenen|[Geniş Dünya İthalatçıları Geri Yükleme](sql-database-managed-instance-get-started-restore.md)
  |||[GitHub'dan](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) [BACPAC](sql-database-import.md) dosyasından Adventure Works'ü geri yükleme veya alma|
  |||

  > [!IMPORTANT]
  > Bu makaledeki komut dosyaları Adventure Works veritabanını kullanmak için yazılmıştır. Yönetilen bir örnekle, Adventure Works veritabanını bir örnek veritabanına aktarmanız veya Geniş Dünya İthalatçılar veritabanını kullanmak için bu makaledeki komut dosyalarını değiştirmeniz gerekir.

## <a name="install-visual-studio-code"></a>Visual Studio Code'u yükleme

En son Visual Studio [Kodunu](https://code.visualstudio.com/Download) yüklediğinizden ve [mssql uzantısını](https://aka.ms/mssql-marketplace)yüklediğinizden emin olun. mssql uzantısını yükleme kılavuzu için Visual Studio Code için [VS Kodu yükle](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-and-start-visual-studio-code) ve [mssql'e ](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)bakın.

## <a name="configure-visual-studio-code"></a>Görsel Stüdyo Kodunu Yapılandır

### <a name="mac-os"></a>**Mac OS**

macOS için, mssql uzantısının kullandığı .NET Core'un ön koşulu olan OpenSSL'yi yüklemeniz gerekir. Terminalinizi açın ve **brew** ve **OpenSSL'i**yüklemek için aşağıdaki komutları girin.

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

## <a name="get-sql-server-connection-information"></a>SQL sunucu bağlantı bilgilerini alın

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Gelecek yordamlar için tam nitelikli sunucu adı veya ana bilgisayar adı, veritabanı adı ve giriş bilgilerine ihtiyacınız vardır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. **SQL veritabanlarına** veya **SQL yönetilen örnekler** sayfasına gidin.

3. Genel **Bakış** sayfasında, tek bir veritabanı için **Sunucu adının** yanındaki tam nitelikli sunucu adını veya yönetilen bir örnek için **Host'un** yanındaki tam nitelikli sunucu adını gözden geçirin. Sunucu adını veya ana bilgisayar adını kopyalamak için üzerine tıklayın ve **Kopyasimgesini** seçin.

## <a name="set-language-mode-to-sql"></a>Dili modunu SQL’e ayarlama

Visual Studio Code'da, mssql komutlarını ve T-SQL IntelliSense'i etkinleştirmek için dil modunu **SQL** olarak ayarlayın.

1. Yeni bir Visual Studio Code penceresi açın.

2. **Ctrl**+**N**tuşuna basın. Yeni bir düz metin dosyası açılır.

3. Durum çubuğunun sağ alt köşesinde **Düz Metin'i** seçin.

4. Açılan **dil modunu seç menüsünde** **SQL'i**seçin.

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

Visual Studio Code’u kullanarak Azure SQL Veritabanı sunucunuzla bağlantı kurun.

> [!IMPORTANT]
> Devam etmeden önce, sunucunuza sahip olduğunuzdan emin olun ve bilgileri hazır layın. Bağlantı profili bilgilerini girmeye başladıktan sonra, odak noktanızı Visual Studio Code'dan değiştirirseniz, profili oluşturmaya yeniden başlamanız gerekir.

1. Visual Studio Code'da Komut Paletini açmak için **Ctrl+Shift+P** (veya **F1)** tuşuna basın.

2. **MS SQL:Connect'i** seçin ve **Enter'u**seçin.

3. **Bağlantı Profili Oluştur'u**seçin.

4. Yeni profilin bağlantı özelliklerini belirtmek için istemleri izleyin. Her değeri belirttikten sonra devam etmek için **Enter'u** seçin.

   | Özellik       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Sunucu adı** | Tam sunucu adı | Gibi bir şey: **mynewserver20170313.database.windows.net**. |
   | **Veritabanı adı** | mySampleDatabase | Bağlanacak veritabanı. |
   | **Kimlik doğrulaması** | SQL Oturum Açma| Bu öğretici, SQL Kimlik Doğrulama'yı kullanır. |
   | **Kullanıcı adı** | Kullanıcı adı | Sunucu oluşturmak için kullanılan sunucu yöneticisi hesabının kullanıcı adı. |
   | **Parola (SQL Oturum Açma)** | Parola | Sunucu oluşturmak için kullanılan sunucu yöneticisi hesabının parolası. |
   | **Parola kaydedilsin mi?** | Evet veya Hayır | Parolayı her seferinde girmek istemiyorsanız **Evet'i** seçin. |
   | **Bu profil için bir ad girin** | **mySampleProfile** gibi bir profil adı | Kaydedilen profil, sonraki oturum açmalarda bağlantınızı hızlandırıyor. |

   Başarılı olursa, profilinizin oluşturulduğunu ve bağlanındığını belirten bir bildirim görüntülenir.

## <a name="query-data"></a>Verileri sorgulama

Kategoriye göre en iyi 20 ürünü sorgulamak için aşağıdaki [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL deyimini çalıştırın.

1. Düzenleyici penceresinde, aşağıdaki SQL sorgusunu yapıştırın.

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. Sorguyu çalıştırmak ve tabloların `Product` sonuçlarını görüntülemek için `ProductCategory` **Ctrl**+**Shift**+**E** tuşuna basın.

    ![2 tablodan veri almak için sorgu](./media/sql-database-connect-query-vscode/query.png)

## <a name="insert-data"></a>Veri ekleme

Tabloya yeni bir ürün eklemek için aşağıdaki [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL deyimini çalıştırın. `SalesLT.Product`

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

2. Tabloya yeni bir satır eklemek için `Product` **Ctrl**+**Shift**+**E** tuşuna basın.

## <a name="update-data"></a>Verileri güncelleştirme

Eklenen ürünü güncelleştirmek için aşağıdaki [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL deyimini çalıştırın.

1. Önceki sorguyu bununla değiştirin:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Tabloda belirtilen satırı güncelleştirmek için `Product` **Ctrl**+**Shift**+**E** tuşuna basın.

## <a name="delete-data"></a>Verileri silme

Yeni ürünü kaldırmak için aşağıdaki [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) Transact-SQL deyimini çalıştırın.

1. Önceki sorguyu bununla değiştirin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Tabloda belirtilen satırı silmek için `Product` **Ctrl**+**Shift**+**E** tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

- SQL Server Management Studio'ya bağlanmak ve sorgulamak [için Azure SQL Veritabanı'na bağlanmak ve verileri sorgulamak için Quickstart: SQL Server Management Studio'yı kullanın.](sql-database-connect-query-ssms.md)
- Azure portalını kullanarak bağlanmak ve sorgulamak [için, verileri bağlamak ve sorgulamak için Azure portalındaki SQL Sorgu düzenleyicisini kullanın.](sql-database-connect-query-portal.md)
- Visual Studio Code'u kullanmaya ilişkin MSDN dergisi makalesi için bkz. [MSSQL uzantısı blog gönderisinden yararlanarak veritabanı IDE'si oluşturma](https://msdn.microsoft.com/magazine/mt809115).
