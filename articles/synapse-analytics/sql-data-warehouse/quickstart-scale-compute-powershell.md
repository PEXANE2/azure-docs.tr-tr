---
title: SYNAPSE SQL havuzu için ölçek işlem (Azure PowerShell)
description: Azure PowerShell kullanarak SYNAPSE SQL havuzunun (veri ambarı) işlem ölçeğini ölçeklendirebilirsiniz.
services: synapse-analytics
author: Antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: igorstan
ms.custom: seo-lt-2019, devx-track-azurepowershell
ms.openlocfilehash: 044b1b145f95e81b2aa2474950d4961131c3c88a
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89079736"
---
# <a name="quickstart-scale-compute-for-synapse-sql-pool-with-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell ile SYNAPSE SQL havuzu için işlem ölçekleme

Azure PowerShell kullanarak SYNAPSE SQL havuzunun (veri ambarı) işlem ölçeğini ölçeklendirebilirsiniz. Daha iyi performans için [işlemin ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu sağlamak için işlemin ölçeğini geri daraltın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu hızlı başlangıçta ölçeklendirebildiğiniz bir SQL havuzu zaten var. Bir tane oluşturmanız gerekiyorsa, **Mysampledatawarehouse**ADLı bir SQL havuzu oluşturmak için [Create and Connect-Portal](create-data-warehouse-portal.md) ' ı kullanın.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Hangi aboneliğin kullandığınızı görmek için [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)komutunu çalıştırın.

```powershell
Get-AzSubscription
```

Varsayılandan farklı bir abonelik kullanmanız gerekiyorsa, [set-AzContext](/powershell/module/az.accounts/set-azcontext?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)komutunu çalıştırın.

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Veri ambarı bilgilerini arama

Duraklatmayı ve sürdürmeyi planladığınız veri ambarı için veritabanı adını, sunucu adını ve kaynak grubunu bulun.

Veri ambarınız için konum bilgilerini bulmak amacıyla aşağıdaki adımları uygulayın.

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.
2. Azure portal sol gezinti sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
3. Veri ambarını açmak için **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin.

    ![Sunucu adı ve kaynak grubu](./media/quickstart-scale-compute-powershell/locate-data-warehouse-information.png)

4. Veritabanı adı olarak kullanılacak olan veri ambarı adını not alın. Veri ambarının tek bir veritabanı türü olduğunu unutmayın. Ayrıca sunucu adını ve kaynak grubunu da not alın. Pause ve Resume komutlarında sunucu adını ve kaynak grubu adını kullanacaksınız.
5. PowerShell cmdlet 'lerinde sunucu adının yalnızca ilk kısmını kullanın. Yukarıdaki görüntüde, tam sunucu adı sqlpoolservername.database.windows.net ' dir. PowerShell cmdlet 'inde sunucu adı olarak **sqlpoolservername** kullanıyoruz.

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

SQL havuzunda, veri ambarı birimlerini ayarlayarak işlem kaynaklarını artırabilir veya azaltabilirsiniz. [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünde **mySampleDataWarehouse** oluşturuldu ve 400 DWU ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için [set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) PowerShell cmdlet 'ini kullanın. Aşağıdaki örnek, **sqlpoolservername**sunucusunda **resourcegroupname** kaynak grubunda barındırılan, **mysampledatawarehouse**veritabanı için veri ambarı birimlerini DW300c olarak ayarlar.

```Powershell
Set-AzSqlDatabase -ResourceGroupName "resourcegroupname" -DatabaseName "mySampleDataWarehouse" -ServerName "sqlpoolservername" -RequestedServiceObjectiveName "DW300c"
```

## <a name="check-data-warehouse-state"></a>Veri ambarı durumunu denetleme

Veri ambarının geçerli durumunu görmek için [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) PowerShell cmdlet 'ini kullanın. Bu cmdlet, ResourceGroup **resourcegroupname** ve Server **Sqlpoolservername.Database.Windows.net**içindeki **mysampledatawarehouse** veritabanının durumunu gösterir.

```powershell
$database = Get-AzSqlDatabase -ResourceGroupName resourcegroupname -ServerName sqlpoolservername -DatabaseName mySampleDataWarehouse
```

Bu da aşağıdaki gibi bir sonuç verir:

```powershell
ResourceGroupName             : resourcegroupname
ServerName                    : sqlpoolservername
DatabaseName                  : mySampleDataWarehouse
Location                      : North Europe
DatabaseId                    : 34d2ffb8-b70a-40b2-b4f9-b0a39833c974
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1_General_CP1_CI_AS
CatalogCollation              :
MaxSizeBytes                  : 263882790666240
Status                        : Online
CreationDate                  : 11/20/2017 9:18:12 PM
CurrentServiceObjectiveId     : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
CurrentServiceObjectiveName   : DW300c
RequestedServiceObjectiveId   : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           :
Tags                          :
ResourceId                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
                                resourceGroups/resourcegroupname/providers/Microsoft.Sql/servers/sqlpoolservername/databases/mySampleDataWarehouse
CreateMode                    :
ReadScale                     : Disabled
ZoneRedundant                 : False
```

Çıkışta veritabanının **Durumunu** görüntüleyebilirsiniz. Bu durumda, bu veritabanının çevrimiçi olduğunu görebilirsiniz.  Bu komutu çalıştırdığınızda, Çevrimiçi, Duraklatılıyor, Sürdürülüyor, Ölçeklendiriliyor veya Duraklatıldı Durum değerlerinden birini alırsınız.

Durumun kendisini görüntülemek için şu komutu kullanın:

```powershell
$database | Select-Object DatabaseName,Status
```

## <a name="next-steps"></a>Sonraki adımlar

Artık SQL havuzu için işlem ölçeklendirmeyi öğrendiniz. SQL havuzu hakkında daha fazla bilgi edinmek için veri yükleme öğreticisine geçin.

> [!div class="nextstepaction"]
>[Verileri bir SQL havuzuna yükleme](load-data-from-azure-blob-storage-using-polybase.md)
