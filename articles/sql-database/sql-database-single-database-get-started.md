---
title: Tek bir veritabanı oluşturma
description: Azure portal, PowerShell veya Azure CLı kullanarak Azure SQL veritabanı tek veritabanı oluşturun. Azure portal veritabanını sorgu Düzenleyicisi ile sorgulayın.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: ninarn
ms.reviewer: carlrab, sstein, vanto
ms.date: 03/10/2020
ms.openlocfilehash: 638adaac699bb7aa2774f5cbd37dc8394a2baee3
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79240523"
---
# <a name="quickstart-create-an-azure-sql-database-single-database"></a>Hızlı başlangıç: Azure SQL veritabanı tek veritabanı oluşturma

Bu hızlı başlangıçta, Azure SQL veritabanı tek veritabanı oluşturmak için Azure portal, bir PowerShell betiği veya bir Azure CLı betiği kullanırsınız. Daha sonra Azure portal **sorgu düzenleyicisini** kullanarak veritabanını sorgulayın. 

[Tek bir veritabanı](sql-database-single-database.md) , Azure SQL veritabanı için en hızlı ve en basit dağıtım seçeneğidir. Tek bir veritabanını, belirtilen bir Azure bölgesindeki bir [Azure Kaynak grubu](../azure-resource-manager/management/overview.md) içinde bulunan bir [SQL veritabanı sunucusu](sql-database-servers.md)içinde yönetirsiniz. Bu hızlı başlangıçta yeni veritabanı için yeni bir kaynak grubu ve SQL Server oluşturacaksınız.

*Sağlanan* veya *sunucusuz* işlem katmanında tek bir veritabanı oluşturabilirsiniz. Sağlanan bir veritabanı, CPU ve bellek dahil olmak üzere sabit bir işlem kaynakları miktarına önceden ayrılır ve iki [satın alma modelinden](sql-database-purchase-models.md)birini kullanır. Bu hızlı başlangıç, [sanal çekirdek tabanlı](sql-database-service-tiers-vcore.md) satın alma modeli kullanılarak sağlanan bir veritabanı oluşturur, ancak [DTU tabanlı](sql-database-service-tiers-DTU.md) bir model de seçebilirsiniz. 

Sunucusuz işlem katmanı yalnızca sanal çekirdek tabanlı satın alma modelinde kullanılabilir ve CPU ve bellek dahil olmak üzere otomatik olarak ölçeklendirilmiş bir işlem kaynakları aralığına sahiptir. Sunucusuz işlem katmanında tek bir veritabanı oluşturmak için bkz. [sunucusuz veritabanı oluşturma](sql-database-serverless.md#create-new-database-in-serverless-compute-tier).

## <a name="prerequisite"></a>Önkoşul

- Etkin bir Azure aboneliği. Hesabınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

## <a name="create-a-single-database"></a>Tek bir veritabanı oluşturma

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="query-the-database"></a>Veritabanını sorgulama

Veritabanınız oluşturulduktan sonra, veritabanına bağlanmak ve verileri sorgulamak için Azure portal yerleşik **sorgu düzenleyicisini** kullanabilirsiniz.

1. Portalda **SQL veritabanları**' nı arayıp seçin ve ardından listeden veritabanınızı seçin.
1. Veritabanınızın **SQL veritabanı** sayfasında, soldaki menüden **sorgu Düzenleyicisi 'ni (Önizleme)** seçin.
1. Sunucu Yöneticisi oturum açma bilgilerinizi girip **Tamam**' ı seçin.
   
   ![Sorgu Düzenleyicisi 'nde oturum açın](./media/sql-database-single-database-get-started/query-editor-login.png)

1. **Sorgu Düzenleyicisi** bölmesine aşağıdaki sorguyu girin.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

1. **Çalıştır**' ı seçin ve ardından **sonuçlar** bölmesinde sorgu sonuçlarını gözden geçirin.

   ![Sorgu Düzenleyicisi sonuçları](./media/sql-database-single-database-get-started/query-editor-results.png)

1. **Sorgu Düzenleyicisi** sayfasını kapatın ve kaydedilmemiş düzenlemelerinizi atmak isteyip Istemediğiniz sorulduğunda **Tamam** ' ı seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu, sunucuyu ve tek veritabanını bir sonraki adımlara devam etmek için ve veritabanınızı bağlanıp farklı yöntemlerle sorgulamayı öğrenin.

Bu kaynakları kullanmayı tamamladığınızda, oluşturduğunuz kaynak grubunu silebilirsiniz, bu da sunucuyu ve içindeki tek veritabanını da siler.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak **Myresourcegroup** ve tüm kaynaklarını silmek için:

1. Portalda **kaynak grupları**' nı arayıp seçin ve listeden **myresourcegroup** ' ı seçin.
1. Kaynak grubu sayfasında, **kaynak grubunu sil**' i seçin.
1. **Kaynak grubu adını yazın**altında *myresourcegroup*yazın ve ardından **Sil**' i seçin.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Kaynak grubunu ve tüm kaynaklarını silmek için, kaynak grubunuzun adını kullanarak aşağıdaki Azure CLı komutunu çalıştırın:

```azurecli-interactive
az group delete --name <your resource group>
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Kaynak grubunu ve tüm kaynaklarını silmek için, kaynak grubunuzun adını kullanarak aşağıdaki PowerShell cmdlet 'ini çalıştırın:

 ```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group>
```

---
## <a name="next-steps"></a>Sonraki adımlar

Farklı araçlar ve diller kullanarak veritabanınızı [bağlayın ve sorgulayın](sql-database-connect-query.md) :
> [!div class="nextstepaction"]
> [SQL Server Management Studio kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md)
> 
> [Azure Data Studio kullanarak bağlanma ve sorgulama](/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
