---
title: Azure Veri Fabrikası'nda kimlik bilgilerini şifreleme
description: Kendi kendine barındırılan tümleştirme çalışma süresine sahip bir makinede şirket içi veri depolarınızın kimlik bilgilerini nasıl şifreleyip depoladığınızı öğrenin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: anandsub
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 8ce26360aca8d8408135cbe89aabff4f923013b4
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416360"
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Azure Veri Fabrikası'ndaki şirket içi veri depoları için kimlik bilgilerini şifreleme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Kendi kendine barındırılan tümleştirme çalışma süresine sahip bir makinede şirket içi veri depolarınızın (hassas bilgilere sahip bağlantılı hizmetler) kimlik bilgilerini şifreleyebilir ve depolayabilirsiniz. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Kimlik bilgilerini içeren bir JSON tanım dosyası <br/>[**Yeni-AzDataFactoryV2LinkedServiceEncryptedCredential**](/powershell/module/az.datafactory/New-AzDataFactoryV2LinkedServiceEncryptedCredential) cmdlet şifreli kimlik bilgileri ile bir çıkış JSON tanım dosyası üretmek için. Ardından, bağlı hizmetleri oluşturmak için güncelleştirilmiş JSON tanımını kullanın.

## <a name="author-sql-server-linked-service"></a>Author SQL Server bağlantılı hizmet
Aşağıdaki içeriğe sahip herhangi bir klasörde **SqlServerLinkedService.json** adında bir JSON dosyası oluşturun:  

Dosyayı kaydetmeden `<password>` önce SQL Server'ınız için değerler le değiştirin. `<servername>` `<databasename>` `<username>` Ve tümleştirme çalışma sürenizin adını değiştirin. `<integration runtime name>` 

```json
{
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
        },
        "connectVia": {
            "type": "integrationRuntimeReference",
            "referenceName": "<integration runtime name>"
        },
        "name": "SqlServerLinkedService"
    }
}
```

## <a name="encrypt-credentials"></a>Kimlik bilgilerini şifreleme
Şirket içi kendi kendine barındırılan tümleştirme çalışma zamanında JSON yükünden gelen hassas verileri şifrelemek için **New-AzDataFactoryV2LinkedServiceEncryptedCredential'ı**çalıştırın ve JSON yükünü aktarın. Bu cmdlet, kimlik bilgilerinin DPAPI kullanılarak şifrelenmesini ve kendi kendine barındırılan tümleştirme çalışma zamanı düğümünde yerel olarak depolanmasını sağlar. Kimlik bilgisine şifreli başvuruiçeren çıktı yükü başka bir JSON dosyasına yönlendirilebilir (bu durumda 'encryptedLinkedService.json').

```powershell
New-AzDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>JSON'u şifreli kimlik bilgileriyle kullanma
Şimdi, **SqlServerLinkedService**kurmak için şifreli kimlik bilgilerini içeren önceki komutun çıktı JSON dosyasını kullanın.

```powershell
Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Sonraki adımlar
Veri hareketi için güvenlik hususları hakkında bilgi için [bkz.](data-movement-security-considerations.md)

