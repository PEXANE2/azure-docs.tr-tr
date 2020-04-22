---
title: Tek bir veritabanı oluşturma
description: Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak Bir Azure SQL Veritabanı tek veritabanı oluşturun. Veritabanını Azure portalında Sorgu Düzenleyicisi ile sorgula.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: ninarn
ms.reviewer: carlrab, sstein, vanto
ms.date: 04/19/2020
ms.openlocfilehash: b06c255f94b4d539c7c85e318238b10629cbb907
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685129"
---
# <a name="quickstart-create-an-azure-sql-database-single-database"></a>Quickstart: Azure SQL Veritabanı tek veritabanı oluşturma

Bu hızlı başlangıçta, Azure SQL Veritabanı tek bir veritabanı oluşturmak için Azure portalını, PowerShell komut dosyasını veya Azure CLI komut dosyasını kullanırsınız. Daha sonra Azure portalındaki **Sorgu düzenleyicisini** kullanarak veritabanını sorgularsınız. 

[Tek bir veritabanı,](sql-database-single-database.md) Azure SQL Veritabanı için en hızlı ve en basit dağıtım seçeneğidir. Belirli bir Azure bölgesindeki bir [Azure kaynak grubunun](../azure-resource-manager/management/overview.md) içinde bulunan bir SQL Veritabanı [sunucusunda](sql-database-servers.md)tek bir veritabanını yönetirsiniz. Bu hızlı başlatmada, yeni veritabanı için yeni bir kaynak grubu ve SQL sunucusu oluşturursunuz.

*Sağlanan* veya *sunucusuz* bilgi işlem katmanında tek bir veritabanı oluşturabilirsiniz. Sağlanan bir veritabanı, CPU ve bellek de dahil olmak üzere sabit miktarda bilgi işlem kaynağı önceden tahsis edilir ve iki [satın alma modelinden](sql-database-purchase-models.md)birini kullanır. Bu hızlı başlatma [vCore tabanlı](sql-database-service-tiers-vcore.md) satın alma modelini kullanarak uygun bir veritabanı oluşturur, ancak [DTU tabanlı](sql-database-service-tiers-DTU.md) bir model de seçebilirsiniz. 

Sunucusuz bilgi işlem katmanı yalnızca vCore tabanlı satın alma modelinde kullanılabilir ve CPU ve bellek de dahil olmak üzere otomatik ölçeklenmiş bir bilgi işlem kaynağı aralığına sahiptir. Sunucusuz bilgi işlem katmanında tek bir veritabanı oluşturmak için [bkz.](sql-database-serverless.md#create-new-database-in-serverless-compute-tier)

## <a name="prerequisite"></a>Önkoşul

- Etkin bir Azure aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

## <a name="create-a-single-database"></a>Tek bir veritabanı oluşturma

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="query-the-database"></a>Veritabanını sorgula

Veritabanınız oluşturulduktan sonra, veritabanına bağlanmak ve verileri sorgulamak için Azure portalındaki yerleşik **Sorgu düzenleyicisini** kullanabilirsiniz.

1. Portalda, **SQL veritabanlarını**arayın ve seçin ve ardından listeden veritabanınızı seçin.
1. Veritabanınızın **SQL Veritabanı** sayfasında, sol menüde Sorgu düzenleyicisi **(önizleme)** seçeneğini belirleyin.
1. Sunucu yönetici giriş bilgilerinizi girin ve **Tamam'ı**seçin.
   
   ![Sorgu düzenleyicisinde oturum açma](./media/sql-database-single-database-get-started/query-editor-login.png)

1. **Sorgu düzenleyicisi** bölmesine aşağıdaki sorguyu girin.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

1. **Çalıştır'ı**seçin ve ardından **Sonuçlar** bölmesinde sorgu sonuçlarını gözden geçirin.

   ![Sorgu düzenleyicisi sonuçları](./media/sql-database-single-database-get-started/query-editor-results.png)

1. Sorgu **düzenleyicisi** sayfasını kapatın ve kaydedilmemiş düzenleyicilerinizi atması istendiğinde **Tamam'ı** seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu, sunucuyu ve tek veritabanını sonraki adımlara devam etmek için tutun ve veritabanınızı farklı yöntemlerle nasıl bağlayıp sorgulayacağız öğrenin.

Bu kaynakları kullanmayı bitirdiğinizde, oluşturduğunuz kaynak grubunu silebilir ve bu grup içindeki sunucuyu ve tek veritabanını da silebilir.

### <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak **MyResourceGroup'umu** ve tüm kaynaklarını silmek için:

1. Portalda, **Kaynak gruplarını**arayın ve seçin ve ardından listeden **myResourceGroup'u** seçin.
1. Kaynak grubu sayfasında **kaynak grubunu sil'i**seçin.
1. **Kaynak grup adı altında,** *myResourceGroup*girin ve sonra **Sil'i**seçin.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Kaynak grubunu ve tüm kaynaklarını silmek için kaynak grubunuzun adını kullanarak aşağıdaki Azure CLI komutunu çalıştırın:

```azurecli-interactive
az group delete --name <your resource group>
```

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Kaynak grubunu ve tüm kaynaklarını silmek için kaynak grubunuzun adını kullanarak aşağıdaki PowerShell cmdlet'i çalıştırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group>
```

---

## <a name="next-steps"></a>Sonraki adımlar

Farklı araçları ve dilleri kullanarak veritabanınızı bağlayın ve [sorgulayın:](sql-database-connect-query.md)
> [!div class="nextstepaction"]
> [SQL Server Management Studio kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md)
> 
> [Azure Data Studio kullanarak bağlanma ve sorgulama](/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
