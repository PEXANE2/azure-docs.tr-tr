---
title: Azure Analiz Hizmetleri görevlerini hizmet ilkeleriyle otomatikleştirin | Microsoft Dokümanlar
description: Azure Çözümleme Hizmetleri yönetim görevlerini otomatikleştirmek için bir hizmet ilkesioluşturmayı öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: dc163de9a7fb46d62f4bc2983e040e68bbf9231c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266149"
---
# <a name="automation-with-service-principals"></a>Hizmet sorumlularıyla otomasyon

Hizmet sorumluları, katılımsız kaynak ve hizmet düzeyinde işlemler gerçekleştirmek için kiracınızın içinde oluşturduğunuz bir Azure Active Directory uygulama kaynağıdır. Uygulama kimliği ve parolası veya sertifikası olan benzersiz bir *kullanıcı kimliği* türüdür. Bir hizmet sorumlusu yalnızca atandığı roller ve izinler tarafından tanımlanan görevleri gerçekleştirmek için gereken izinlere sahiptir. 

Analiz Hizmetlerinde, hizmet ilkeleri Azure Otomasyonu, PowerShell katılımsız modu, özel istemci uygulamaları ve web uygulamalarıyla ortak görevleri otomatikleştirmek için kullanılır. Örneğin, sunucu sağlama, modelleri dağıtma, veri yenileme, ölçeklendirme/küçültme ve duraklatma/devam etme gibi tüm hizmetler ilkeleri kullanılarak otomatikleştirilebilir. İzinler, tıpkı normal Azure AD UPN hesapları gibi rol üyeliği yoluyla hizmet ilkelerine atanır.

Analiz Hizmetleri, hizmet ilkelerini kullanarak yönetilen kimlikler tarafından gerçekleştirilen işlemleri de destekler. Daha fazla bilgi edinmek için Azure [kaynakları için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md) ve [Azure AD kimlik doğrulamasını destekleyen Azure hizmetleri'ne](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-analysis-services)bakın.  

## <a name="create-service-principals"></a>Hizmet sorumlusu oluşturma
 
Hizmet ilkeleri Azure portalında veya PowerShell kullanılarak oluşturulabilir. Daha fazla bilgi için bkz:

[Hizmet sorumlusu oluşturma - Azure portalı](../active-directory/develop/howto-create-service-principal-portal.md)   
[Hizmet sorumlusu oluşturma - PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="store-credential-and-certificate-assets-in-azure-automation"></a>Azure Otomasyonu'nda kimlik ve sertifika varlıklarını depolama

Hizmet temel kimlik bilgileri ve sertifikalar, runbook işlemleri için Azure Otomasyonu'nda güvenli bir şekilde depolanabilir. Daha fazla bilgi için bkz:

[Azure Otomasyonu'nda kimlik bilgileri](../automation/automation-credentials.md)   
[Azure Otomasyonu'ndaki sertifika varlıkları](../automation/automation-certificates.md)

## <a name="add-service-principals-to-server-admin-role"></a>Sunucu yöneticisi rolüne hizmet ilkeleri ekleme

Çözümleme Hizmetleri sunucu yönetimi işlemleri için bir hizmet ilkesini kullanabilmek için önce, sunucu yöneticileri rolüne eklemeniz gerekir. Daha fazla bilgi için bkz. [sunucu yöneticisi rolüne bir hizmet sorumlusu ekle.](analysis-services-addservprinc-admins.md)

## <a name="service-principals-in-connection-strings"></a>Bağlantı dizelerinde hizmet ilkeleri

Hizmet temel appID ve parola veya sertifika bağlantı dizeleri çok bir UPN aynı kullanılabilir.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

#### <a name="using-azanalysisservices-module"></a><a name="azmodule" />Az.AnalysisServices modüllerini kullanma

[Az.AnalysisServices](/powershell/module/az.analysisservices) modülü ile kaynak yönetimi işlemleri için bir `Connect-AzAccount` hizmet prensibi kullanırken cmdlet kullanın. 

Aşağıdaki örnekte, yalnızca okunur yinelemelere eşitleme ve ölçeklendirme/çıkış için denetim düzlemi işlemleri gerçekleştirmek için appID ve parola kullanılır:

```powershell
Param (
        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force
$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

# Connect using Az module
Connect-AzAccount -Credential $Credential -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx"

# Syncronize a database for query scale out
Sync-AzAnalysisServicesInstance -Instance "asazure://westus.asazure.windows.net/testsvr" -Database "testdb"

# Scale up the server to an S1, set 2 read-only replicas, and remove the primary from the query pool. The new replicas will hydrate from the synchronized data.
Set-AzAnalysisServicesServer -Name "testsvr" -ResourceGroupName "testRG" -Sku "S1" -ReadonlyReplicaCount 2 -DefaultConnectionMode Readonly
```

#### <a name="using-sqlserver-module"></a>SQLServer modüllerini kullanma

Aşağıdaki örnekte, bir model veritabanı yenileme işlemi gerçekleştirmek için appID ve parola kullanılır:

```powershell
Param (
        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force

$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

Invoke-ProcessTable -Server "asazure://westcentralus.asazure.windows.net/myserver" -TableName "MyTable" -Database "MyDb" -RefreshType "Full" -ServicePrincipal -ApplicationId $AppId -TenantId $TenantId -Credential $Credential
```

### <a name="amo-and-adomd"></a>AMO ve ADOMD 

İstemci uygulamaları ve web uygulamaları yla bağlantı kurarken, [AMO ve ADOMD istemci kitaplıkları](analysis-services-data-providers.md) sürüm 15.0.2 ve nuget `app:AppID` destek `cert:thumbprint`hizmet ilkelerinden daha yüksek yüklenebilir paketler aşağıdaki sözdizimini kullanarak bağlantı dizeleri: ve şifre veya . 

Aşağıdaki örnekte `appID` ve `password` bir model veritabanı yenileme işlemi gerçekleştirmek için kullanılır:

```csharp
string appId = "xxx";
string authKey = "yyy";
string connString = $"Provider=MSOLAP;Data Source=asazure://westus.asazure.windows.net/<servername>;User ID=app:{appId};Password={authKey};";
Server server = new Server();
server.Connect(connString);
Database db = server.Databases.FindByName("adventureworks");
Table tbl = db.Model.Tables.Find("DimDate");
tbl.RequestRefresh(RefreshType.Full);
db.Model.SaveChanges();
```

## <a name="next-steps"></a>Sonraki adımlar
[Azure PowerShell ile oturum açın](https://docs.microsoft.com/powershell/azure/authenticate-azureps)   
[Sunucu yöneticisi rolüne bir hizmet ilkesi ekleme](analysis-services-addservprinc-admins.md)   
