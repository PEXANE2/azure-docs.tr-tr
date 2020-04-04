---
title: Synapse SQL havuzu (Azure PowerShell) için ölçek hesaplama
description: Azure PowerShell'i kullanarak Synapse SQL havuzu (veri ambarı) için bilgi işlem ölçeklendirebilirsiniz.
services: synapse-analytics
author: Antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: e3038617c6270acf9af295c910e9fd5c7dae2043
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633791"
---
# <a name="quickstart-scale-compute-for-synapse-sql-pool-with-azure-powershell"></a>Quickstart: Azure PowerShell ile Synapse SQL havuzu için hesaplamayı ölçeklendirin

Azure PowerShell'i kullanarak Synapse SQL havuzu (veri ambarı) için bilgi işlem ölçeklendirebilirsiniz. Daha iyi performans için [işlemin ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu sağlamak için işlemin ölçeğini geri daraltın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu hızlı başlatma, ölçeklendirebileceğiniz bir SQL havuzuna sahip olduğunuzu varsayar. Bir tane oluşturmanız gerekiyorsa, **mySampleDataWarehouse**adlı bir SQL havuzu oluşturmak için [Oluştur ve Bağla - portalı](create-data-warehouse-portal.md) kullanın.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Hangi aboneliği kullandığınızı görmek için [Get-AzSubscription'ı](/powershell/module/az.accounts/get-azsubscription?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)çalıştırın.

```powershell
Get-AzSubscription
```

Varsayılan dan farklı bir abonelik kullanmanız gerekiyorsa, [Set-AzContext'ı](/powershell/module/az.accounts/set-azcontext?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)çalıştırın.

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Veri ambarı bilgilerini arama

Duraklatmayı ve sürdürmeyi planladığınız veri ambarı için veritabanı adını, sunucu adını ve kaynak grubunu bulun.

Veri ambarınız için konum bilgilerini bulmak amacıyla aşağıdaki adımları uygulayın.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Azure portalının sol navigasyon sayfasında **Azure Synapse Analytics'i (eski adıyla SQL DW)** tıklatın.
3. Veri ambarını açmak için **Azure Synapse Analytics (eski adıyla SQL DW)** sayfasından **mySampleDataWarehouse'u** seçin.

    ![Sunucu adı ve kaynak grubu](./media/quickstart-scale-compute-powershell/locate-data-warehouse-information.png)

4. Veritabanı adı olarak kullanılacak olan veri ambarı adını not alın. Veri ambarının tek bir veritabanı türü olduğunu unutmayın. Ayrıca sunucu adını ve kaynak grubunu da not alın. Duraklatma ve devam komutlarında sunucu adını ve kaynak grup adını kullanırsınız.
5. PowerShell cmdlets sunucu adının yalnızca ilk bölümünü kullanın. Önceki resimde, tam sunucu adı sqlpoolservername.database.windows.net. PowerShell cmdlet'te sunucu adı olarak **sqlpoolservername** kullanıyoruz.

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

SQL havuzunda, veri ambarı birimlerini ayarlayarak bilgi işlem kaynaklarını artırabilir veya azaltabilirsiniz. [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünde **mySampleDataWarehouse** oluşturuldu ve 400 DWU ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) PowerShell cmdlet'i kullanın. Aşağıdaki örnek, sunucu **sqlpoolservername'deki**Kaynak grubu **kaynak grubunda** barındırılan **mySampleDataWarehouse**veritabanı için veri ambarı birimlerini DW300c olarak ayarlar.

```Powershell
Set-AzSqlDatabase -ResourceGroupName "resourcegroupname" -DatabaseName "mySampleDataWarehouse" -ServerName "sqlpoolservername" -RequestedServiceObjectiveName "DW300c"
```

## <a name="check-data-warehouse-state"></a>Veri ambarı durumunu denetleme

Veri ambarının geçerli durumunu görmek için [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) PowerShell cmdlet'ini kullanın. Bu cmdlet, ResourceGroup **kaynak grubu adı** ve sunucu **sqlpoolservername.database.windows.net** **mySampleDataWarehouse** veritabanının durumunu gösterir.

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

Şimdi SQL havuzu için hesaplamayı ölçeklendirmeyi öğrendiniz. SQL havuzu hakkında daha fazla bilgi edinmek için veri yükleme eğitimine devam edin.

> [!div class="nextstepaction"]
>[Verileri SQL havuzuna yükleme](load-data-from-azure-blob-storage-using-polybase.md)
