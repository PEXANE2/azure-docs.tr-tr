---
title: VNet ve güvenlik duvarının arkasındaki depolama hesabına denetim
description: Sanal ağ ve güvenlik duvarının arkasındaki depolama hesabına veritabanı olayları yazmak için denetimi yapılandırma
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 03/19/2020
ms.custom: azure-synapse
ms.openlocfilehash: 6345d210e26747f921595039a2a3c8e11be11fda
ms.sourcegitcommit: d0fd35f4f0f3ec71159e9fb43fcd8e89d653f3f2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80387640"
---
# <a name="write-audit-to-a-storage-account-behind-vnet-and-firewall"></a>VNet ve güvenlik duvarının arkasındaki depolama hesabına denetim yazma

[Azure SQL Veritabanı](sql-database-technical-overview.md) ve Azure [Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) için denetim, sanal ağ ve güvenlik duvarının arkasındaki [Azure Depolama hesabına](../storage/common/storage-account-overview.md) veritabanı olayları yazmayı destekler. 

Bu makalede, bu seçenek için Azure SQL Server ve Azure depolama hesabı yapılandırmak için iki yol açıklanmaktadır. Birincisi Azure portalını, ikincisi REST'i kullanır.

### <a name="background"></a>Arka plan

[Azure Sanal Ağı (VNet),](../virtual-network/virtual-networks-overview.md) Azure'daki özel ağınızın temel yapı taşıdır. VNet, Azure Sanal Makineleri (VM) gibi birçok Azure kaynağının birbirleriyle, internetle ve şirket içi ağlarla güvenli bir şekilde iletişim kurmasını sağlar. VNet, kendi veri merkezinizdeki geleneksel ağa benzer, ancak ölçek, kullanılabilirlik ve yalıtım gibi Azure altyapısının ek avantajlarını da beraberinde getirir.

VNet kavramları, en iyi uygulamalar ve daha fazlası hakkında daha fazla bilgi edinmek için [Azure Sanal Ağı nedir'e](../virtual-network/virtual-networks-overview.md)bakın.

Sanal ağ oluşturma hakkında daha fazla bilgi edinmek için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak sanal ağ oluşturun.](../virtual-network/quick-create-portal.md)

## <a name="prerequisites"></a>Ön koşullar

Denetimin Bir VNet veya güvenlik duvarının arkasındaki depolama hesabına yazılabilmesi için aşağıdaki ön koşullar gereklidir:

> [!div class="checklist"]
> * Genel amaçlı v2 depolama hesabı. Genel amaçlı bir v1 veya blob depolama hesabınız varsa, [genel amaçlı v2 depolama hesabına yükseltin.](../storage/common/storage-account-upgrade.md) Daha fazla bilgi için [bkz.](../storage/common/storage-account-overview.md#types-of-storage-accounts)
> * Depolama hesabı, Azure SQL Veritabanı sunucusuyla aynı abonelikte ve aynı konumda olmalıdır. 
> * Azure Depolama hesabı `Allow trusted Microsoft services to access this storage account`gerektirir. Bunu Depolama Hesabı **Güvenlik Duvarları ve Sanal ağlarda**ayarlayın.
> * Seçili `Microsoft.Authorization/roleAssignments/write` depolama hesabında izne sahip olmalısınız. Daha fazla bilgi için [Azure yerleşik rolleri'ne](../role-based-access-control/built-in-roles.md)bakın.

## <a name="configure-in-azure-portal"></a>Azure portalında yapılandırma

Aboneliğinizle [Azure portalına](https://portal.azure.com) bağlanın. Kaynak grubuna ve Azure SQL veritabanı sunucusuna gidin.

1. Güvenlik başlığı altında **Denetim'e** tıklayın. **Üzerinde'ni**seçin.

2. **Depolama**’yı seçin. Günlüklerin kaydedilen depolama hesabını seçin. Depolama [hesabı, Önkoşullar'da](#prerequisites)listelenen gereksinimlere uygun olmalıdır.

3. Açık **Depolama ayrıntıları** 

  > [!NOTE]
  > Seçili Depolama hesabı VNet'in arkasındaysa, aşağıdaki iletiyi görürsünüz:
  >
  >`You have selected a storage account that is behind a firewall or in a virtual network. Using this storage: requires an Active Directory admin on the server; enables 'Allow trusted Microsoft services to access this storage account' on the storage account; and creates a server managed identity with 'storage blob data contributor' RBAC.`
  >
  >Bu iletiyi görmüyorsanız, depolama hesabı bir VNet'in arkasında değildir.

4. Bekletme dönemi için gün sayısını seçin. Ardından **Tamam**'a tıklayın. Bekletme döneminden eski günlükler silinir.

5. Denetim ayarlarınızda **Kaydet'i** seçin.

Denetimi, Bir VNet veya güvenlik duvarının arkasındaki depolama hesabına yazmak üzere başarıyla yapılandırdınız. 

## <a name="configure-with-rest-commands"></a>REST komutlarıyla yapılandırma

Azure portalını kullanmaya alternatif olarak, denetimi bir VNet ve Güvenlik Duvarı'nın arkasındaki depolama hesabına veritabanı olayları yazmak için yapılandırmak için REST komutlarını kullanabilirsiniz. 

Bu bölümdeki örnek komut dosyaları, komut dosyasını çalıştırmadan önce güncelleştirmenizi gerektirir. Komut dosyalarında aşağıdaki değerleri değiştirin:

|Örnek değer|Örnek açıklama|
|:-----|:-----|
|`<subscriptionId>`| Azure abonelik kimliği|
|`<resource group>`| Kaynak grubu|
|`<sql database server>`| Azure SQL veritabanı sunucu adı|
|`<administrator login>`| SQL veritabanı yöneticisi hesabı |
|`<complex password>`| Yönetici hesabı için karmaşık parola|

Sql Audit'i, bir VNet veya Güvenlik Duvarı'nın arkasındaki bir depolama hesabına olay yazmak için yapılandırmak için:

1. Azure SQL Veritabanı sunucunuzu Azure Etkin Dizini (Azure AD) ile kaydedin. PowerShell veya REST API'yi kullanın.

   **Powershell**
   
   ```powershell
   Connect-AzAccount
   Select-AzSubscription -SubscriptionId <subscriptionId>
   Set-AzSqlServer -ResourceGroupName <your resource group> -ServerName <azure server name> -AssignIdentity
   ```
   
   [**REST API**](https://docs.microsoft.com/rest/api/sql/servers/createorupdate):

   Örnek istek

   ```html
   PUT https://management.azure.com/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Sql/servers/<azure server name>?api-version=2015-05-01-preview
   ```

   İstek gövdesi

   ```json
   {
   "identity": {
              "type": "SystemAssigned",
              },
   "properties": {
     "fullyQualifiedDomainName": "<azure server name>.database.windows.net",
     "administratorLogin": "<administrator login>",
     "administratorLoginPassword": "<complex password>",
     "version": "12.0",
     "state": "Ready"
   }
   ```

2. [Azure portalını](https://portal.azure.com) açın. Depolama hesabınıza gidin. **Access Denetimi'ni (IAM)** bulun ve **rol ataması ekle'yi**tıklatın. Önceki adımda olduğu gibi Azure Active Directory (Azure AD) ile kaydolduğunuz Azure SQL veritabanınızı barındıran Azure SQL Sunucunuza **Depolama Blob Veri Katılımcısı** RBAC rolünü atayın.

   > [!NOTE]
   > Bu adımı yalnızca Sahip ayrıcalığına sahip üyeler gerçekleştirebilir. Azure kaynakları için çeşitli yerleşik roller için [Azure yerleşik rollerine](../role-based-access-control/built-in-roles.md)bakın.

3. Bir *storageAccountAccessKey*belirtmeden [Azure SQL sunucusunun blob denetim ilkesini](/rest/api/sql/server%20auditing%20settings/createorupdate)yapılandırın:

   Örnek istek

   ```html
   PUT https://management.azure.com/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Sql/servers/<azure server name>?api-version=2017-03-01-preview
   ```

   İstek gövdesi

   ```json
   {
     "properties": {
      "state": "Enabled",
      "storageEndpoint": "https://<storage account>.blob.core.windows.net"
     }
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal ağ hizmeti bitiş noktası ve ardından Azure SQL Veritabanı için sanal ağ kuralı oluşturmak için PowerShell'i kullanın.](sql-database-vnet-service-endpoint-rule-powershell.md)
- [Sanal Ağ Kuralları: REST API'leri ile işlemler](/rest/api/sql/virtualnetworkrules)
- [Veritabanı sunucuları için sanal ağ hizmeti uç noktalarını ve kurallarını kullanma](sql-database-vnet-service-endpoint-rule-overview.md)
