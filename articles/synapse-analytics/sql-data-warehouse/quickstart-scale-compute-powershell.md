---
title: 'Hızlı başlangıç: adanmış SQL Havuzu (eski adıyla SQL DW) için ölçek işlem (Azure PowerShell)'
description: Azure PowerShell kullanarak adanmış SQL havuzunun (eski adıyla SQL DW) işlem ölçeğini ölçeklendirebilirsiniz.
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
ms.openlocfilehash: 87e10740e6081431bad96daa930f61238ca495bd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96921899"
---
# <a name="quickstart-scale-compute-for-dedicated-sql-pool-formerly-sql-dw-with-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell ile adanmış SQL Havuzu (eski adıyla SQL DW) için işlem ölçekleme

Azure PowerShell kullanarak adanmış SQL havuzunun (eski adıyla SQL DW) işlem ölçeğini ölçeklendirebilirsiniz. Daha iyi performans için [işlemin ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu sağlamak için işlemin ölçeğini geri daraltın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu hızlı başlangıçta ölçeklendirebildiğiniz ayrılmış bir SQL havuzuna (eski adıyla SQL DW) sahip olduğunuz varsayılır. Bir tane oluşturmanız gerekiyorsa, **Mysampledatawarehouse** adlı özel bir SQL Havuzu (eskı ADıYLA SQL DW) oluşturmak için [Oluştur ve Bağlan-Portal](create-data-warehouse-portal.md) ' ı kullanın.

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

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Azure portal sol gezinti sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
3. Veri ambarını açmak için **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin.

    ![Sunucu adı ve kaynak grubu](./media/quickstart-scale-compute-powershell/locate-data-warehouse-information.png)

4. Veritabanı adı olarak kullanılacak olan veri ambarı adını not alın. Veri ambarının tek bir veritabanı türü olduğunu unutmayın. Ayrıca sunucu adını ve kaynak grubunu da not alın. Pause ve Resume komutlarında sunucu adını ve kaynak grubu adını kullanacaksınız.
5. PowerShell cmdlet 'lerinde sunucu adının yalnızca ilk kısmını kullanın. Yukarıdaki görüntüde, tam sunucu adı sqlpoolservername.database.windows.net ' dir. PowerShell cmdlet 'inde sunucu adı olarak **sqlpoolservername** kullanıyoruz.

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

Adanmış SQL havuzunda (eski adıyla SQL DW), veri ambarı birimlerini ayarlayarak işlem kaynaklarını artırabilir veya azaltabilirsiniz. [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünde **mySampleDataWarehouse** oluşturuldu ve 400 DWU ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için [set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) PowerShell cmdlet 'ini kullanın. Aşağıdaki örnek, **sqlpoolservername** sunucusunda **resourcegroupname** kaynak grubunda barındırılan, **mysampledatawarehouse** veritabanı için veri ambarı birimlerini DW300c olarak ayarlar.

```Powershell
Set-AzSqlDatabase -ResourceGroupName "resourcegroupname" -DatabaseName "mySampleDataWarehouse" -ServerName "sqlpoolservername" -RequestedServiceObjectiveName "DW300c"
```

## <a name="check-data-warehouse-state"></a>Veri ambarı durumunu denetleme

Veri ambarının geçerli durumunu görmek için [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) PowerShell cmdlet 'ini kullanın. Bu cmdlet, ResourceGroup **resourcegroupname** ve Server **Sqlpoolservername.Database.Windows.net** içindeki **mysampledatawarehouse** veritabanının durumunu gösterir.

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

Artık adanmış SQL Havuzu (eski adıyla SQL DW) için işlem ölçeklendirmeyi öğrendiniz. Adanmış SQL Havuzu (eski adıyla SQL DW) hakkında daha fazla bilgi edinmek için veri yükleme öğreticisine geçin.

> [!div class="nextstepaction"]
>[Özel bir SQL havuzuna veri yükleme](load-data-from-azure-blob-storage-using-copy.md)
