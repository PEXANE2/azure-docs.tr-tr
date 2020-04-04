---
title: Azure PowerShell ile Synapse SQL havuzunda hesaplamayı duraklatma ve devam ettirme
description: Synapse SQL havuzunu (veri ambarı) duraklatmak ve devam ettirmek için Azure PowerShell'i kullanabilirsiniz. kaynakları hesaplat.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 03/20/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: f257f3751e7a411015ca188d704b676950845a74
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633828"
---
# <a name="quickstart-pause-and-resume-compute-in-synapse-sql-pool-with-azure-powershell"></a>Quickstart: Azure PowerShell ile Synapse SQL havuzunda hesaplamayı duraklatma ve devam ettirme

Synapse SQL havuzu (veri ambarı) bilgi işlem kaynaklarını duraklatmak ve devam ettirmek için Azure PowerShell'i kullanabilirsiniz.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu hızlı başlatma, duraklatabileceğiniz ve devam ettirebileceğiniz bir SQL havuzunuz olduğunu varsayar. Bir tane oluşturmanız gerekiyorsa, **mySampleDataWarehouse**adlı bir SQL havuzu oluşturmak için [Oluştur ve Bağla - portalı](create-data-warehouse-portal.md) kullanabilirsiniz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?toc=/azure/synapse-analytics/sql-data-warehouse?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Hangi aboneliği kullandığınızı görmek için [Get-AzSubscription'ı](/powershell/module/az.accounts/get-azsubscription?toc=/azure/synapse-analytics/sql-data-warehouse?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)çalıştırın.

```powershell
Get-AzSubscription
```

Varsayılan dan farklı bir abonelik kullanmanız gerekiyorsa, [Set-AzContext'ı](/powershell/module/az.accounts/set-azcontext?toc=/azure/synapse-analytics/sql-data-warehouse?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)çalıştırın.

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-sql-pool-information"></a>SQL havuz bilgilerine bakın

Duraklatmak ve devam ettirmeyi planladığınız SQL havuzunun veritabanı adını, sunucu adını ve kaynak grubunu bulun.

SQL havuzunuz için konum bilgilerini bulmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Azure portalının sol sayfasında **Azure Synapse Analytics'i (eski adıyla SQL DW)** tıklatın.
1. **Azure Synapse Analytics (eski adıyla SQL DW)** sayfasından **mySampleDataWarehouse'u** seçin. SQL havuzu açılır.

    ![Sunucu adı ve kaynak grubu](./media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

1. Veritabanı adı olan SQL havuz adını yazın. Ayrıca sunucu adını ve kaynak grubunu da not alın.
1. PowerShell cmdlets sunucu adının yalnızca ilk bölümünü kullanın. Önceki resimde, tam sunucu adı sqlpoolservername.database.windows.net. PowerShell cmdlet'te sunucu adı olarak **sqlpoolservername** kullanıyoruz.

## <a name="pause-compute"></a>İşlemi duraklatma

Maliyetlerden tasarruf etmek için, kaynakları isteğe bağlı olarak duraklatabilir ve yeniden başlatabilirsiniz. Örneğin, veritabanını gece ve hafta sonları kullanmıyorsanız, bu saatlerde veritabanını duraklatabilir ve gün içinde devam ettirebilirsiniz.

>[!NOTE]
>Veritabanı duraklatılmışken hesaplama kaynakları için ücret alınmaz. Ancak, depolama için ücretlendirilmeye devam edin.

Bir veritabanını duraklatmak için [Askıya Alma-AzSqlVeritabanı](/powershell/module/az.sql/suspend-azsqldatabase?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) cmdlet'ini kullanın. Aşağıdaki örnek, **sqlpoolservername**adlı bir sunucuda barındırılan **mySampleDataWarehouse** adlı bir SQL havuzuduraklatıyor. Sunucu **myResourceGroup**adlı bir Azure kaynak grubundadır.

```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "nsqlpoolservername" –DatabaseName "mySampleDataWarehouse"
```

Aşağıdaki örnekveritabanını $database nesnesine alır. Daha sonra askıya [alma-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)nesne borular. Sonuçlar nesne sonucu Veritabanı'nda depolanır. Son komut sonuçları gösterir.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="resume-compute"></a>Özgeçmiş hesaplama

Veritabanı başlatmak için [Özgeçmiş-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) cmdlet'ini kullanın. Aşağıdaki örnek, **sqlpoolservername**adlı bir sunucuda barındırılan **mySampleDataWarehouse** adlı bir veritabanı nı başlatır. Sunucu **myResourceGroup**adlı bir Azure kaynak grubundadır.

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" -DatabaseName "mySampleDataWarehouse"
```

Sonraki örnek veritabanını $database nesnesine alır. Daha sonra nesneyi [Resume-AzSqlDatabase'e](/powershell/module/az.sql/resume-azsqldatabase) borular ve sonuçları $resultDatabase. Son komut sonuçları gösterir.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Resume-AzSqlDatabase
$resultDatabase
```

## <a name="check-status-of-your-sql-pool-operation"></a>SQL havuz işleminizin durumunu kontrol edin

SQL havuzunuzun durumunu kontrol etmek için [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/Get-AzSqlDatabaseActivity#description?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) cmdlet'i kullanın.

```Powershell
Get-AzSqlDatabaseActivity -ResourceGroupName "myResourceGroup" -ServerName "sqlpoolservername" -DatabaseName "mySampleDataWarehouse"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri için ücretlendirilirsiniz ve SQL havuzunuzda depolanan veriler. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depolamada tutmak istiyorsanız, bilgi işlem'i duraklatın.
- Gelecekteki ücretleri kaldırmak istiyorsanız, SQL havuzunu silebilirsiniz.

Kaynakları istediğiniz gibi temizlemek için bu adımları izleyin.

1. [Azure portalında](https://portal.azure.com)oturum açın ve SQL havuzuna tıklayın.

    ![Kaynakları temizleme](./media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. İşlemi duraklatmak için, **Duraklat** düğmesine tıklayın. SQL havuzu duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşlemi sürdürmek için **Başlat**’a tıklayın.

3. İşlem veya depolama için ücretlendirilmemek için SQL havuzunu kaldırmak için **Sil'i**tıklatın.

4. Oluşturduğunuz SQL sunucusunu kaldırmak için **sqlpoolservername.database.windows.net**ve **sil'i**tıklatın.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için, **myResourceGroup**’a tıklayıp daha sonra **Kaynak grubunu sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

SQL havuzu hakkında daha fazla bilgi edinmek [için, verileri SQL havuzu makalesine yüklemeye](load-data-from-azure-blob-storage-using-polybase.md) devam edin. Bilgi işlem yeteneklerini yönetme hakkında daha fazla bilgi için, [bilgi işlem genel bakış](sql-data-warehouse-manage-compute-overview.md) makalesini yönet'e bakın.
