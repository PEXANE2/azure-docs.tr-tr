---
title: PowerShell'i kullanarak şirket içi verileri şirket içinde Azure'a kopyalama
description: Bu PowerShell komut dosyası, şirket içi BIR SQL Server veritabanından verileri başka bir Azure Blob Depolama alanına kopyalar.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.author: jingwang
author: linda33wj
manager: shwang
ms.custom: seo-lt-2019
ms.date: 10/31/2017
ms.openlocfilehash: 10555defc4888af66bb88d19190b6543aa8ae0c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75974700"
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-from-on-premises-to-azure"></a>Şirket içi verileri azure'a kopyalamak için bir veri fabrikası ardışık hattı oluşturmak için PowerShell'i kullanın

Bu örnek PowerShell komut dosyası, Azure Veri Fabrikası'nda şirket içi BIR SQL Server veritabanından Azure Blob Depolamasına verileri kopyalayan bir ardışık kaynak oluşturur.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- **SQL Server**. Bu örnekte **kaynak** veri deposu olarak şirket içi BIR SQL Server veritabanı kullanırsınız.
- **Azure Depolama hesabı.** Azure blob depolama alanını bu örnekte **hedef/lavabo** veri deposu olarak kullanırsınız. Azure depolama hesabınız yoksa, bir tane oluşturma adımları için [bir depolama hesabı oluştur](../../storage/common/storage-account-create.md) makalesine bakın.
- **Kendi kendine barındırılan tümleştirme çalışma zamanı.** MsI dosyasını [indirme merkezinden](https://www.microsoft.com/download/details.aspx?id=39717) indirin ve makinenize kendi kendine barındırılan bir tümleştirme çalışma süresi yüklemek için çalıştırın.  

### <a name="create-sample-database-in-sql-server"></a>SQL Server'da örnek veritabanı oluşturma
1. Şirket içi SQL Server veritabanında, aşağıdaki SQL komut dosyasını kullanarak **emp** adlı bir tablo oluşturun:

   ```sql   
     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
         CONSTRAINT PK_emp PRIMARY KEY (ID)
     )
     GO
   ```

2. Tabloya bazı örnek verileri ekleyin:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```

## <a name="sample-script"></a>Örnek betik

> [!IMPORTANT]
> Bu komut dosyası, c:\ 'daki sabit diskinizde Veri Fabrikası varlıklarını (bağlantılı hizmet, veri kümesi ve ardışık) tanımlayan JSON dosyaları oluşturur: \ Klasör.

[!code-powershell[main](../../../powershell_scripts/data-factory/copy-from-onprem-sql-server-to-azure-blob/copy-from-onprem-sql-server-to-azure-blob.ps1 "Copy from on-premises SQL Server -> Azure Blob Storage")]


## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek komut dosyasını çalıştırdıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanabilirsiniz:

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```
Veri fabrikasını kaynak grubundan kaldırmak için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Set-AzDataFactoryV2](/powershell/module/az.datafactory/set-Azdatafactoryv2) | Veri fabrikası oluşturma. |
| [Yeni-AzDataFactoryV2LinkedServiceEncryptCredential](/powershell/module/az.datafactory/new-Azdatafactoryv2linkedserviceencryptedcredential) | Bağlı bir hizmetteki kimlik bilgilerini şifreler ve şifrelenmiş kimlik bilgileriyle yeni bir bağlantılı hizmet tanımı oluşturur.
| [Set-AzDataFactoryV2LinkedService](/powershell/module/az.datafactory/Set-Azdatafactoryv2linkedservice) | Veri fabrikasında bağlantılı bir hizmet oluşturur. Bağlantılı bir hizmet, bir veri deposuveya bilgi işlem bağlantısını bir veri fabrikasına bağlar. |
| [Set-AzDataFactoryV2Dataset](/powershell/module/az.datafactory/Set-Azdatafactoryv2dataset) | Veri fabrikasında bir veri kümesi oluşturur. Veri kümesi, bir ardışık ardışık alandaki bir etkinlik için giriş/çıktıyı temsil eder. |
| [Set-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/Set-Azdatafactoryv2pipeline) | Veri fabrikasında bir ardışık hat lar oluşturur. Bir ardışık işlem, belirli bir işlemi gerçekleştiren bir veya daha fazla etkinlik içerir. Bu ardışık işlemde, bir kopya etkinliği verileri bir konumdan başka bir konuma bir Azure Blob Depolama'da kopyalar. |
| [Invoke-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/Invoke-Azdatafactoryv2pipeline) | Boru hattı için bir çalışma oluşturur. Başka bir deyişle, boru hattını çalıştırın. |
| [Get-AzDataFactoryV2ActivityRun](/powershell/module/az.datafactory/get-Azdatafactoryv2activityrun) | Ardışık ardışık ardışık alanda etkinlik (etkinlik çalışması) çalıştırıla ilgili ayrıntıları alır.
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Veri Fabrikası PowerShell komut dosyası örnekleri [Azure Veri Fabrikası PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
