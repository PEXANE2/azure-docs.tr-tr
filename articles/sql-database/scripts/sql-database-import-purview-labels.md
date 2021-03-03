---
title: Azure purview etiketlerini kullanarak Azure SQL verilerinizi sınıflandırın
description: Azure SQL veritabanınızdaki ve Azure Synpade Analytics 'te Azure purview 'tan sınıflandırmanızı içeri aktarın
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: azurepowershell
ms.topic: sample
author: davidtrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 02/17/2021
ms.openlocfilehash: 2eab7c535ff0c68da772e8a45ead12420734279c
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101714910"
---
# <a name="classify-your-azure-sql-data-using-azure-purview-labels"></a>Azure purview etiketlerini kullanarak Azure SQL verilerinizi sınıflandırın
[!INCLUDE[appliesto-sqldb-asa](../../azure-sql/includes/appliesto-sqldb-asa.md)]

Bu belgede Azure SQL veritabanınıza ve Azure SYNAPSE Analytics 'e (eski adıyla SQL DW) Azure purview etiketlerinin nasıl ekleneceği açıklanmaktadır.

## <a name="create-an-application"></a>Uygulama oluşturma

1. Azure portal, **Azure Active Directory** açın.
2. **Yönet** altında, **uygulama kaydı**' nı seçin.
3. **Yeni uygulama**' yı seçerek yeni bir Azure Active Directory uygulaması oluşturun.
4. Uygulamanız için bir ad girin ve **Kaydet**' i seçin.
5. Uygulamanız oluşturulduktan sonra, **yönetici** altında **Sertifikalar & gizlilikler** ' ı açın.
6. **İstemci** gizli dizileri altında **yeni istemci parolası** ' nı seçerek yeni bir istemci gizli dizisi oluşturun.
7. İstemci gizli anahtarı için bir açıklama ekleyin, bir sona erme süresi seçin ve **Ekle**' yi seçin.
8. Daha sonra kullanmak için **değeri** saklayın.

   > [!NOTE]
   > Sayfayı kapattıktan sonra değer maskelenecek. Sayfaya geri giderseniz, istemci parolasının değerini alamazsınız. Yeni bir istemci parolası oluşturmanız gerekecektir.

9. Yeni oluşturulan uygulamanızın genel bakış sayfasına dönün ve ileride kullanılmak üzere aşağıdaki değerleri kopyalayın:
    1. Uygulama (istemci) kimliği
    1. Dizin (kiracı) kimliği

## <a name="provide-permissions-to-the-application"></a>Uygulamaya izinler sağlama

1. Azure portal, **purview hesaplarını** arayın.
2. SQL veritabanlarınızın ve SYNAPSE sınıflandırıldığı purview hesabını seçin.
3. **Erişim denetimi (IAM)** öğesini açın, **Ekle**' yi seçin.

4. **Rol ataması ekle**’yi seçin.
5. **Rol** bölümünde, **purview veri okuyucuyu** arayın ve seçin.
6. **Seç** bölümünde, önceden oluşturduğunuz uygulamayı arayın, seçin ve **Kaydet**' i tıklayın.

## <a name="extract-the-classification-from-azure-purview"></a>Azure purview 'tan sınıflandırmayı ayıklayın

1. Purview hesabınızı açın ve giriş sayfasında, etiketleri kopyalamak istediğiniz Azure SQL veritabanınızı veya Azure SYNAPSE Analytics 'i arayın.
2. **Özellikleri** altına QualifiedName kopyalayın ve ileride kullanılmak üzere saklayın.
3. PowerShell kabuğunu açın.

4. Aşağıdaki betiklerin birini SQL varlık türüne göre (Azure SQL veritabanı veya Azure SYNAPSE) kopyalayın.
5. Parametreleri yukarıda kopyaladığınız değerlerle doldurabilirsiniz:

   a. $TenantID: Bölüm 1, adım 9
   
   b. $ClientID: Bölüm 1, adım 9
   
   c. $SecretID: Bölüm 1, adım 8
   
   d. $purviewAccountName: Bölüm 2, adım 2
   
   e. $sqlDatabaseName: Bölüm 3, adım 2

6. İleride kullanılmak üzere betiğin çıkışını kopyalayın.

### <a name="for-azure-sql-database"></a>Azure SQL veritabanı için

```powershell
# Replace the values below with the relevant values for your environment
$TenantID = 'your_tenant_id'
$ClientID = 'your_client_id'
$SecretID = 'your_secret_id'
$purviewAccountName='purview_account_name'
$sqlDatabaseName="mssql://sql_server_name.database.windows.net/db_name"

###############################################################################
# Get an access accessToken, and build REST Request Header.
###############################################################################
$cmdletParameters = @{
  Method  = "POST"
  URI     = "https://login.microsoftonline.com/$TenantID/oauth2/token"
  Headers = @{
    "Content-Type" = 'application/x-www-form-urlencoded'
  }
  Body    = "client_id=$ClientID&client_secret=$SecretID&resource=https://purview.azure.net&grant_type=client_credentials"
}

$invokeResult = Invoke-RestMethod @cmdletParameters;
$accessToken = $invokeResult.access_token; 

$restRequestHeader = @{
  "authorization" = "Bearer $accessToken"
};

###############################################################################
# Get database entity.
###############################################################################
$getDatabaseEntityEntryEndpoint = "https://" + $purviewAccountName + ".catalog.purview.azure.com/api/atlas/v2/entity/uniqueAttribute/type/azure_sql_db?attr:qualifiedName=" + $sqlDatabaseName;
$cmdletParameters = @{
  Method      = "Get"
  Uri         = $getDatabaseEntityEntryEndpoint
  Headers     = $restRequestHeader
  ContentType = "application/json"        
};

$invokeResult =  Invoke-RestMethod @cmdletParameters;
$referredEntities = $invokeResult.referredEntities;
if ($null -eq $referredEntities) {
  Write-Output "No referred entities found under database entity!";
  exit;
}

###############################################################################
# Iterate database referred entities, find classified columns, and build T-SQL.
###############################################################################
foreach ($referredEntity in $referredEntities.psobject.Properties.GetEnumerator()) {
  $typeName = $referredEntity.Value.typeName;
  if ($null -eq $typeName -or $typeName -ne 'azure_sql_column') {
    continue;
  }
  
  $classifications = $referredEntity.Value.classifications;
  if ($null -eq $classifications) {
    continue;
  }
  
  foreach ($classification in $classifications.GetEnumerator())
  {
    if ($classification.typeName -notmatch 'Microsoft\.Label\.(?<labelId>.+)') { 
        continue;
    }

      $labelId = $Matches.labelId -replace "_", "-";

      $attributes = $referredEntity.Value.attributes;
      if ($null -eq $attributes) {
        continue;
      }
    
      $qualifiedName = $attributes.qualifiedName;
      if ($qualifiedName -notmatch '.+\.database\.windows\.net\/.+\/(?<schemaName>.+)\/(?<tableName>.+)#(?<columnName>.+)') {
        continue;
      }

      $schemaName = $Matches.schemaName;
      $tableName = $Matches.tableName;
      $columnName = $Matches.columnName;
      
      Write-Output "ADD SENSITIVITY CLASSIFICATION TO ${schemaName}.${tableName}.${columnName} WITH (LABEL='Azure Purview Label', LABEL_ID='${labelId}');";
  }
}
```

### <a name="for-azure-synapse-analytics"></a>Azure SYNAPSE Analytics için

```powershell
# Replace the values below with the relevant values for your environment
$TenantID = 'your_tenant_id'
$ClientID = 'your_client_id'
$SecretID = 'your_secret_id'
$purviewAccountName='purview_account_name'
$dwDatabaseName="mssql://dw_server_name.database.windows.net/dw_name"

###############################################################################
# Get an access accessToken, and build REST Request Header.
###############################################################################
$cmdletParameters = @{
  Method  = "POST"
  URI     = "https://login.microsoftonline.com/$TenantID/oauth2/token"
  Headers = @{
    "Content-Type" = 'application/x-www-form-urlencoded'
  }
  Body    = "client_id=$ClientID&client_secret=$SecretID&resource=https://purview.azure.net&grant_type=client_credentials"
}

$invokeResult = Invoke-RestMethod @cmdletParameters;
$accessToken = $invokeResult.access_token; 

$restRequestHeader = @{
  "authorization" = "Bearer $accessToken"
};

###############################################################################
# Get database entity.
###############################################################################
$getDatabaseEntityEntryEndpoint = "https://" + $purviewAccountName + ".catalog.purview.azure.com/api/atlas/v2/entity/uniqueAttribute/type/azure_sql_dw?attr:qualifiedName=" + $dwDatabaseName;
$cmdletParameters = @{
  Method      = "Get"
  Uri         = $getDatabaseEntityEntryEndpoint
  Headers     = $restRequestHeader
  ContentType = "application/json"        
};

$invokeResult =  Invoke-RestMethod @cmdletParameters;
$referredEntities = $invokeResult.referredEntities;
if ($null -eq $referredEntities) {
  Write-Output "No referred entities found under database entity!";
  exit;
}

###############################################################################
# Iterate database referred entities, find classified columns, and build T-SQL.
###############################################################################
foreach ($referredEntity in $referredEntities.psobject.Properties.GetEnumerator()) {
  $typeName = $referredEntity.Value.typeName;
  if ($null -eq $typeName -or $typeName -ne 'azure_sql_dw_column') {
    continue;
  }
  
  $classifications = $referredEntity.Value.classifications;
  if ($null -eq $classifications) {
    continue;
  }
  
  foreach ($classification in $classifications.GetEnumerator())
  {
    if ($classification.typeName -notmatch 'Microsoft\.Label\.(?<labelId>.+)') { 
        continue;
    }

      $labelId = $Matches.labelId -replace "_", "-";

      $attributes = $referredEntity.Value.attributes;
      if ($null -eq $attributes) {
        continue;
      }
    
      $qualifiedName = $attributes.qualifiedName;
      if ($qualifiedName -notmatch '.+\.database\.windows\.net\/.+\/(?<schemaName>.+)\/(?<tableName>.+)#(?<columnName>.+)') {
        continue;
      }

      $schemaName = $Matches.schemaName;
      $tableName = $Matches.tableName;
      $columnName = $Matches.columnName;
      
      Write-Output "ADD SENSITIVITY CLASSIFICATION TO ${schemaName}.${tableName}.${columnName} WITH (LABEL='Azure Purview Label', LABEL_ID='${labelId}');";
  }
}
```

## <a name="run-the-t-sql-command-on-your-sql-asset"></a>SQL varlıkınız üzerinde T-SQL komutunu çalıştırma

1. Tercih ettiğiniz aracı kullanarak Azure SQL veritabanınıza veya Azure SYNAPSE 'a bağlanın.
2. Önceki bölümden kopyaladığınız T-SQL komutunu çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).

Azure purview hakkında daha fazla bilgi için bkz. [Azure purview belgeleri](../../purview/index.yml).