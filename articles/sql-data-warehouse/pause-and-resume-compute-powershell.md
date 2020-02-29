---
title: 'Hızlı başlangıç: duraklatma & devam eden işlem-PowerShell '
description: Maliyetleri kaydetmek için Azure SYNAPSE Analytics SQL havuzunda işlem duraklatmak için PowerShell 'i kullanın. Veri ambarını kullanmaya hazırsanız işlem işlemine da izin verilmez.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 03/20/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: ce183edef9e5895d7b3f702f5466c505956a869a
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78200575"
---
# <a name="quickstart-pause-and-resume-compute-in-azure-synapse-analytics-sql-pool-with-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell ile Azure SYNAPSE Analytics SQL havuzunda işlem duraklatma ve devam ettirme

Maliyetleri kaydetmek üzere SQL havuzu için işlem duraklatmak için Azure PowerShell kullanın. Veri ambarını kullanmaya hazırsanız [Işlem işlemine](sql-data-warehouse-manage-compute-overview.md) da izin verilmez.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu hızlı başlangıçta duraklatıp devam ettirebildiğiniz bir SQL havuzu zaten var. Bir tane oluşturmanız gerekiyorsa, **Mysampledatawarehouse**ADLı bir SQL havuzu oluşturmak için [Create and Connect-Portal](create-data-warehouse-portal.md) ' ı kullanabilirsiniz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Hangi aboneliğin kullandığınızı görmek için [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription)komutunu çalıştırın.

```powershell
Get-AzSubscription
```

Varsayılandan farklı bir abonelik kullanmanız gerekiyorsa, [set-AzContext](/powershell/module/az.accounts/set-azcontext)komutunu çalıştırın.

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Veri ambarı bilgilerini arama

Duraklamayı ve sürdürmeyi planladığınız SQL havuzunun veritabanı adını, sunucu adını ve kaynak grubunu bulun.

SQL havuzunuzun konum bilgilerini bulmak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Azure portal sol sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
1. **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin. Veri ambarı açılır.

    ![Sunucu adı ve kaynak grubu](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

1. Veritabanı adı olan veri ambarı adı ' nı yazın. Ayrıca sunucu adını ve kaynak grubunu da not alın.
1. PowerShell cmdlet 'lerinde sunucu adının yalnızca ilk kısmını kullanın. Yukarıdaki görüntüde, tam sunucu adı sqlpoolservername.database.windows.net ' dir. PowerShell cmdlet 'inde sunucu adı olarak **sqlpoolservername** kullanıyoruz.

## <a name="pause-compute"></a>İşlem Duraklat

Maliyetleri kaydetmek için, talep üzerine işlem kaynaklarını duraklatabilir ve sürdürebilirsiniz. Örneğin, gece ve hafta sonları sırasında veritabanını kullanmıyorsanız, bu zamanlarda duraklatıp gün içinde devam edebilirsiniz. Veritabanı duraklatıldığında işlem kaynakları için ücret alınmaz. Ancak, depolama alanı için ücretlendirilmeye devam edersiniz.

Bir veritabanını duraklatmak için, [askıya al-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase) cmdlet 'ini kullanın. Aşağıdaki örnekte, **sqlpoolservername**adlı bir sunucuda barındırılan **mysampledatawarehouse** adlı bir veri ambarı duraklatılır. Sunucu, **Myresourcegroup**adlı bir Azure Kaynak grubunda bulunur.


```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "nsqlpoolservername" –DatabaseName "mySampleDataWarehouse"
```

Bir çeşitleme, bu sonraki örnek veritabanını $database nesnesine alır. Sonra, nesneyi [askıya al-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase)' a yöneltin. Sonuçlar, nesne resultDatabase içinde depolanır. Son komut sonuçları gösterir.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="resume-compute"></a>İşlem işlemine geri dön

Bir veritabanını başlatmak için, [özgeçmişi-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase) cmdlet 'ini kullanın. Aşağıdaki örnek, **sqlpoolservername**adlı bir sunucuda barındırılan **mysampledatawarehouse** adlı bir veritabanını başlatır. Sunucu, **Myresourcegroup**adlı bir Azure Kaynak grubunda bulunur.

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" -DatabaseName "mySampleDataWarehouse"
```

Bir çeşitleme, bu sonraki örnek veritabanını $database nesnesine alır. Ardından, nesneyi [sürdürülecek-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase) ve sonuçları $resultDatabase depolar. Son komut sonuçları gösterir.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Resume-AzSqlDatabase
$resultDatabase
```

## <a name="check-status-of-your-data-warehouse-operation"></a>Veri ambarı işleminin durumunu denetleyin

Veri ambarınızın durumunu denetlemek için [Get-AzSqlDatabaseActivity](https://docs.microsoft.com/powershell/module/az.sql/Get-AzSqlDatabaseActivity#description) cmdlet 'ini kullanın.

```Powershell
Get-AzSqlDatabaseActivity -ResourceGroupName "myResourceGroup" -ServerName "sqlpoolservername" -DatabaseName "mySampleDataWarehouse"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri ve veri ambarınızda depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depolama alanında tutmak istiyorsanız, işlem ' i duraklatın.
- Gelecekteki ücretleri kaldırmak istiyorsanız, SQL havuzunu silebilirsiniz.

Kaynakları istediğiniz gibi temizlemek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com)oturum açın ve SQL havuzunuza tıklayın.

    ![Kaynakları temizleme](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. İşlemi duraklatmak için, **Duraklat** düğmesine tıklayın. SQL havuzu duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşlemi sürdürmek için **Başlat**’a tıklayın.

3. İşlem veya depolama için ücretlendirilmemek üzere SQL havuzunu kaldırmak için **Sil**' e tıklayın.

4. Oluşturduğunuz SQL Server 'ı kaldırmak için **sqlpoolservername.Database.Windows.net**ve ardından **Sil**' e tıklayın.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için, **myResourceGroup**’a tıklayıp daha sonra **Kaynak grubunu sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Artık SQL havuzunuz için işlemi duraklattı ve sürdürdü. SQL havuzu hakkında daha fazla bilgi edinmek için veri yükleme öğreticisine geçin.

> [!div class="nextstepaction"]
> [Verileri SQL havuzuna yükleme](load-data-from-azure-blob-storage-using-polybase.md)
