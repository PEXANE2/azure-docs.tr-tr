---
title: Azure Kaynak taşıyıcısı 'nde PowerShell 'i kullanarak bölgeler arasında kaynakları taşıma
description: Azure Kaynak taşıyıcısı 'nde PowerShell kullanarak kaynakları bölgeler arasında taşımayı öğrenin.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: 3236e0a95c6a4b4f57ac38ed067011c3d6848b5a
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89670797"
---
# <a name="move-resources-across-regions-in-powershell"></a>PowerShell 'deki bölgeler arasında kaynakları taşıma

Azure Kaynak taşıyıcısı 'nde PowerShell kullanarak Azure kaynaklarını farklı bir bölgeye taşımayı öğrenin. 

> [!NOTE]
> Azure Kaynak taşıyıcısı Şu anda önizleme aşamasındadır.



## <a name="before-you-start"></a>Başlamadan önce

- Azure aboneliğinizin kaynak taşıyıcısı erişimi olmalıdır ve abonelik için [sahip](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) veya [Kullanıcı erişimi Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) izinlerine sahip olmanız gerekir.
- Kaynak taşıyıcısı değişiklikleri ve yükseltmeleri izlemediği için, devam etmeden önce kaynaklarda gerekli değişiklikleri yapın.
- Kaynakları PowerShell ile taşıdığınızda, şu anda herhangi bir hedef bölge ayarını düzenleyemezsiniz. Bu ayarları doğrudan portalda değiştirin.
- Bir taşıma koleksiyonuna kaynak eklediğinizde, bunları başka bir bölgeye taşımaya hazırlanarak, taşıma hakkındaki meta veriler amaç için oluşturulan bir kaynak grubunda depolanır. Şu anda bu kaynak grubu Doğu ABD 2 veya Kuzey Avrupa bölgelerinde bulunabilir. Azure kaynakları, bu bölgelerden birinde bulunan meta veriler kullanılarak tüm ortak bölgeler arasında taşınabilir.

## <a name="sample-values"></a>Örnek değerler

Bu değerleri betiğimizde kullanıyoruz:

**Ayar** | **Değer** 
--- | ---
Abonelik Kimliği | abonelik kimliği
Kaynak taşıyıcısı hizmet konumu | Doğu ABD 2
Meta veri kaynağı grubu | RegionMoveRG-merkezde ABD-westcentralus
Meta veri kaynağı grubu konumu | Doğu ABD 2
Koleksiyon adını taşı | MoveCollection-merkezde US-westcentralus
Kaynak bölge |  centralus
Kaynak kaynak grubu | Psdemorz
Hedef bölge | Orta Batı ABD
Hedef kaynak grubu | PSDemoRMtgt
Taşınacak VM | PSDemoVM

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure aboneliğinizde oturum açın.

```azurepowershell-interactive
# Sign in to an Azure subscription
Connect-AzAccount – Subscription "<subscription-id>"
```

## <a name="create-the-resource-group-for-metadata"></a>Meta veriler için kaynak grubu oluşturma

Taşıma koleksiyonu meta verilerini ve yapılandırma bilgilerini tutmak için bir kaynak grubu oluşturun.

```azurepowershell-interactive
# Sign in to an Azure subscription
New-AzResourceGroup -Name MoveCollection-centralus-westcentralus -Location "East US 2"
```

**Beklenen çıkış**:

![Kaynak Grubu oluşturulduktan sonra çıkış metni](./media/move-region-powershell/output-create-resource-group.png)

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

MoveCollection kaynağının oluşturulabilmesi için Microsoft. Migrate kaynak sağlayıcısını kaydedin.

```azurepowershell-interactive
# Register the provider
Register-AzResourceProvider -ProviderNamespace Microsoft.Migrate 

# Wait for registration
While(((Get-AzResourceProvider -ProviderNamespace Microsoft.Migrate)| where {$_.RegistrationState -eq "Registered" -and $_.ResourceTypes.ResourceTypeName -eq "moveCollections"}|measure).Count -eq 0)
  {
      Start-Sleep -Seconds 5
      Write-Output "Waiting for registration to complete."
  }
```

## <a name="create-the-move-collection"></a>Taşıma koleksiyonunu oluşturma

MoveCollection nesnesi, taşımak istediğiniz kaynaklarla ilgili meta verileri ve yapılandırma bilgilerini depolar.

- MoveCollection nesnesinin Azure Kaynak taşıyıcısı hizmetinin bulunduğu aboneliğe erişmesi için, abonelik tarafından güvenilen [sistem tarafından atanan bir yönetilen kimliğe](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) (eski adıyla yönetilen hizmet tanımlaması (MSI)) ihtiyacı vardır.
- Kimlik, katkıda bulunan veya kaynak abonelik için Kullanıcı Erişim Yöneticisi rolüne atanır.
- Kimliğe bir rol atamak için, abonelikte (sahip veya Kullanıcı erişimi Yöneticisi gibi) bu izinlerle bir rolün olması gerekir:
    -  Microsoft.Authorization/roleAssignments/write
    - Microsoft. Authorization/Roleatamaları/silme


```azurepowershell-interactive
# Create a MoveCollection object
New-AzResourceMoverMoveCollection -Name "MoveCollection-centralus-westcentralus " -ResourceGroupName "RegionMoveRG-centralus-westcentralus " -SubscriptionId "<subscription-id>" -SourceRegion "centralus" -TargetRegion "westcentralus" -Location  "East US 2" -IdentityType SystemAssigned
```
**Beklenen çıkış**:

![Taşıma koleksiyonu oluşturduktan sonra çıkış metni](./media/move-region-powershell/output-move-collection.png)


## <a name="assign-a-managed-identity"></a>Yönetilen kimlik atama 

Abonelik KIMLIĞINI ayarlayın, MoveCollection nesnesinin kimlik sorumlusunu alın ve Azure rollerini buna atayın.


```azurepowershell-interactive
# Set the subscriptionId
$subscriptionId = "<subscription-id>"

# Retrieve the principal managed identity of the MoveCollection
$moveCollection = Get-AzResourceMoverMoveCollection -SubscriptionId $subscriptionId -ResourceGroupName "RegionMoveRG-centralus-westcentralus " -Name "MoveCollection-centralus-westcentralus "

# Set the IdentityPrincipalID
$identityPrincipalId = $moveCollection.IdentityPrincipalId

# Assign the role to the IdentityPrincipalID
New-AzRoleAssignment -ObjectId $identityPrincipalId -RoleDefinitionName Contributor -Scope "/subscriptions/$subscriptionId"

New-AzRoleAssignment -ObjectId $identityPrincipalId -RoleDefinitionName "User Access Administrator" -Scope "/subscriptions/$subscriptionId"

```

## <a name="add-resources-to-the-move-collection"></a>Taşıma koleksiyonuna kaynak ekleme

Taşımak istediğiniz kaynak kaynağın KIMLIĞINI alın. Ardından, taşıma koleksiyonuna ekleyin.

```azurepowershell-interactive
# Retrieve the resource ID
Get-AzResource -Name PSDemoVM -ResourceGroupName PSDemoRM
```

**Beklenen çıkış**

![Kaynak KIMLIĞI alındıktan sonra çıkış metni](./media/move-region-powershell/output-retrieve-resource.png)


```azurepowershell-interactive
# Add the resource to the move collection
New-AzResourceMoverMoveResource -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -MoveCollectionName “MoveCollection-centralus-westcentralus ” -SourceId “/subscriptions/e80eb9fa-c996-4435-aa32-5af6f3d3077c/resourceGroups/PSDemoRM/providers/Microsoft.Compute/virtualMachines/PSDemoVM” -Name “PSDemoVM” -ResourceSettingResourceType “ Microsoft.Compute/virtualMachines” -ResourceSettingTargetResourceName “PSDemoVM”
```

**Beklenen çıkış** 
 ![ Kaynağı ekledikten sonra çıkış metni](./media/move-region-powershell/output-add-resource.png)

## <a name="resolve-dependencies"></a>Bağımlılıkları çözümle

Eklediğiniz kaynakları doğrulayın ve diğer kaynaklardaki bağımlılıkları çözün.

```azurepowershell-interactive
# Resolve dependencies
Resolve-AzResourceMoverMoveCollectionDependency -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus” -MoveCollectionName “MoveCollection-centralus-westcentralus”
```
**Bağımlılıklar mevcutsa çıkış**

Taşıma koleksiyonundaki kaynağın diğer kaynaklara bağımlılıkları varsa, başarısız bir ileti verilir.

![Bağımlılıkları denetledikten sonra çıkış metni](./media/move-region-powershell/dependency-error.png)

### <a name="retrieve-dependencies"></a>Bağımlılıkları al

Taşımak istediğiniz kaynağın diğer kaynaklara bağımlılıkları varsa, eksik bağımlılıklar hakkında bilgi alabilirsiniz.

```azurepowershell-interactive
# Identify dependencies
Get-AzResourceMoverUnresolvedDependency -MoveCollectionName “MoveCollection-centralus-westcentralus ” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -SubscriptionId  “<subscription-id>”
```

**Beklenen çıkış**

![Bağımlılıklar alındıktan sonra çıkış metni](./media/move-region-powershell/missing-dependencies.png)

Bu örnekte, iki bağımlılık eksik olarak tanımlanmıştır:

- Kaynak kaynak grubu: Psdemorz
- VM 'de NIC: PSDemoVM62

## <a name="add-dependencies-to-collection"></a>Koleksiyona bağımlılıklar ekleyin


Aldığınız kaynak kimliklerini kullanarak, eksik kaynakların adlarını belirleyip taşıma koleksiyonuna ekleyin.

### <a name="add-the-nic"></a>NIC 'yi ekleme

NIC adını alın ve yazın ve koleksiyonuna ekleyin.

```azurepowershell-interactive
# Get the NIC name and resource type
Get-AzResource -ResourceId “/subscriptions/ <subscription-id> /resourcegroups/psdemorm/providers/microsoft.network/networkinterfaces/psdemovm62”

```

**Beklenen çıkış**

![NIC alındıktan sonra çıkış metni](./media/move-region-powershell/output-retrieve-nic.png)

Şimdi, NIC 'ı taşıma koleksiyonuna ekleyin. Bu örnek, hedef NIC 'ye aynı adı verir. Ayarları ve büyük/küçük harf tutarlılığını koruyun.

```azurepowershell-interactive
# Add the NIC to the collection. 
New-AzResourceMoverMoveResource -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus” -MoveCollectionName “MoveCollection-centralus-westcentralus ” -SourceId “/subscriptions/<subscription-id>/resourceGroups/PSDemoRM/providers/Microsoft.Network/networkInterfaces/psdemovm62” -Name “psdemovm62” -ResourceSettingResourceType “Microsoft.Network/networkInterfaces” -ResourceSettingTargetResourceName “psdemovm62”
```

**Beklenen çıkış**

![NIC 'i koleksiyona ekledikten sonra çıkış metni](./media/move-region-powershell/add-nic.png)

## <a name="add-the-source-resource-group"></a>Kaynak kaynak grubunu ekleme

```azurepowershell-interactive
# Get the resource group name and resource type
Get-AzResource -ResourceId “/subscriptions/<subscription-id>/resourcegroups/psdemorm”

# Add the resource group to the collection. 
New-AzResourceMoverMoveResource -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus” -MoveCollectionName “MoveCollection-centralus-westcentralus” -SourceId “/subscriptions/<subscription-id>/resourcegroups/psdemorm” -Name “psdemorm” -ResourceSettingResourceType “resourcegroups” -ResourceSettingTargetResourceName “psdemorm”
```

**Beklenen çıkış**

![Kaynak grubu koleksiyona eklendikten sonra çıkış metni](./media/move-region-powershell/add-source-resource-group.png)

## <a name="recheck-dependencies"></a>Bağımlılıkları yeniden denetle

Eksik bağımlılıklar için yeniden kontrol edin.

```azurepowershell-interactive
Get-AzResourceMoverUnresolvedDependency -MoveCollectionName “MoveCollection-centralus-westcentralus ” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -SubscriptionId  “<subscription-id>”
```

**Çıktı**

Ek eksik bağımlılıklar var.

![Bağımlılıklar için yeniden denetlemeden sonra çıkış metni](./media/move-region-powershell/recheck-dependencies.png)

## <a name="retrieve-additional-dependencies"></a>Ek bağımlılıklar al

```azurepowershell-interactive
# Identify dependencies
Get-AzResourceMoverUnresolvedDependency -MoveCollectionName “MoveCollection-centralus-westcentralus ” -ResourceGroupName “RegionMoveRG-centralus-westcentralus” -SubscriptionId  “<subscription-id>”
```

**Beklenen çıkış**


![Ek bağımlılıklar alındıktan sonra çıkış metni](./media/move-region-powershell/additional-missing-dependencies.png)

Bu örnekte, eksik olan üç bağımlılık daha tanımlanmıştır:

- Genel IP adresi: psdemovm-IP
- Azure sanal ağı: psdemord-VNET
- Ağ güvenlik grubu (NSG): psdemovm-NSG

## <a name="add-additional-dependencies"></a>Ek bağımlılıklar ekleme

1. Bu kaynakları taşıma koleksiyonuna eklemek için [yönergeleri izleyin](#add-dependencies-to-collection) .
2. Kaynakları ekledikten sonra, tüm bağımlılıklar eklenene kadar tekrar doğrulayın.


## <a name="prepare-and-move-the-source-resource-group"></a>Kaynak kaynak grubunu hazırlama ve taşıma

Kaynak bölgede, kaynakları taşımadan önce hazırlarsınız. Hazırlama işlemi, taşıdığınız kaynaklara bağlıdır. Örneğin, durum bilgisiz kaynakları için hazırlama Azure Resource Manager (ARM) şablonu hazırlayabilir ve dışarı aktarabilir. Ya da durum bilgisi olan kaynaklar için çoğaltma kapatılabilir. 

Kaynak kaynakları hazırlayabilmeniz için önce kaynak kaynak grubunu hazırlamanız ve taşımanız gerekir.

### <a name="prepare"></a>Hazırlama

```azurepowershell-interactive
# Prepare the source resource group
Invoke-AzResourceMoverPrepare -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -MoveCollectionName “MoveCollection-centralus-westcentralus ” -MoveResource “PSDemoRM”
```

**Beklenen çıkış**

![Kaynak kaynak grubu hazırlandıktan sonra çıkış metni](./media/move-region-powershell/prepare-source-resource-group.png)

### <a name="initiate-move"></a>Taşımayı Başlat

```azurepowershell-interactive
# Initiate move
Invoke-AzResourceMoverInitiateMove -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -MoveCollectionName “MoveCollection-centralus-westcentralus ” -MoveResource “PSDemoRM”

**Expected output**

![Output text after initiating move of the source resource group](./media/move-region-powershell/initiate-move-resource-group.png)


```azurepowershell-interactive
# Commit move
Invoke-AzResourceMoverCommit -SubscriptionId “<subscription-id” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -MoveCollectionName “PS-centralus-
westcentralus-demoRM” -MoveResource “PSDemoRM”
```

**Beklenen çıkış**

![Kaynak kaynak grubu uygulandıktan sonra çıkış metni](./media/move-region-powershell/commit-move-resource-group.png)


## <a name="prepare-resources"></a>Kaynakları hazırlama

Kaynak kaynak grubu hedef bölgeye taşındıktan sonra, taşıma koleksiyonundaki kaynakların bir listesini alın ve bunları taşımaya hazırlanın.

```azurepowershell-interactive
# Retrieve resources in the collection
Get-AzResourceMoverMoveResource  -SubscriptionId “ <subscription-id> “ -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -MoveCollectionName “MoveCollection-centralus-westcentralus ”   | Where-Object {  $_.MoveStatusMoveState -eq “PreparePending” } | Select Name
```
**Beklenen çıkış**

![Koleksiyondaki kaynakları aldıktan sonra çıkış metni](./media/move-region-powershell/collection-resources.png)

Şimdi kaynakları hazırlayın.

```azurepowershell-interactive
# Prepare the resources, using the resource name
Invoke-AzResourceMoverPrepare -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus   -MoveResource $('psdemovm62', 'PSDemoVM-ip', 'PSDemoRM-vnet','PSDemoVM-nsg', ‘PSDemoVM’)
```
Alternatif olarak, kaynak KIMLIĞINI kullanarak kaynakları hazırlayabilir ve bu adı yerine taşıyabilirsiniz:

```azurepowershell-interactive
# Prepare the resources using the resource ID
Invoke-AzResourceMoverPrepare -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus  -MoveResourceInputType MoveResourceSourceId  -MoveResource $('/subscriptions/<subscription-id>/resourceGroups/PSDemoRM/providers/Microsoft.Network/networkSecurityGroups/PSDemoVM-nsg')
```

**Beklenen çıkış**

![Koleksiyondaki kaynakları hazırlarken çıkış metni](./media/move-region-powershell/collection-prepare.png)

## <a name="initiate-resource-move"></a>Kaynak taşımayı Başlat

Kaynakları hazırladıktan sonra taşımayı başlatın.

```azurepowershell-interactive
# Initiate the move 
Invoke-AzResourceMoverInitiateMove -SubscriptionId <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus   -MoveResource $('psdemovm62', 'PSDemoVM-ip', 'PSDemoRM-vnet','PSDemoVM-nsg', ‘PSDemoVM’)
```
**Beklenen çıkış** 
 ![ Kaynak taşımayı başlattıktan sonra çıkış metni](./media/move-region-powershell/initiate-resource-move.png)

## <a name="discard-or-commit-the-move"></a>Taşımayı atın veya işleyin

İlk taşıma işleminden sonra, taşımayı yürütmek mi yoksa atmak mi istediğinize karar verebilirsiniz. 

- **At**: test ediyorsanız bir taşıma atabilir ve kaynak kaynağı gerçekten taşımak istemezsiniz. Taşımayı atmak, kaynağı *başlatma bekleyen*bir durumuna döndürür. Ardından, gerekirse taşımayı yeniden başlatabilirsiniz.
- **COMMIT**: COMMIT, hedef bölgeye taşıma işlemini tamamlar. İşlem tamamlandıktan sonra kaynak kaynak, *bekleyen silme kaynağı*durumunda olur ve silmek istediğinize karar verebilirsiniz.

### <a name="discard"></a>Vazgeç

Taşımayı atmak için:

```azurepowershell-interactive
# Discard the move 
Invoke-AzResourceMoverDiscard -SubscriptionId  <subscription-id> `-ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus   -MoveResource $('psdemovm62', 'PSDemoVM-ip', 'PSDemoRM-vnet','PSDemoVM-nsg', ‘PSDemoVM’)
```
**Beklenen çıkış** 
 ![ Taşıma iptal edildikten sonra çıkış metni](./media/move-region-powershell/discard-move.png)


### <a name="commit"></a>İşleme

Taşımayı yürütmek için:

```azurepowershell-interactive
# Commit the move 
Invoke-AzResourceMoverCommit -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus   -MoveResource $('psdemovm62', 'PSDemoVM-ip', 'PSDemoRM-vnet','PSDemoVM-nsg', ‘PSDemoVM’)
```
**Beklenen çıkış**

![Taşıma işleminden sonra çıkış metni](./media/move-region-powershell/commit-move.png)

## <a name="delete-source-resources"></a>Kaynak kaynaklarını silme

Taşıma işleminden sonra ve kaynakların hedef bölgede beklendiği gibi çalıştığını doğruladıktan sonra, PowerShell veya [Azure CLI](../azure-resource-manager/management/manage-resources-cli.md#delete-resources) [kullanarak](../azure-resource-manager/management/manage-resources-powershell.md#delete-resources) [Azure Portal](../azure-resource-manager/management/manage-resources-portal.md#delete-resources)kaynak kaynaklarını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir taşıma koleksiyonundan kaynakları kaldırmayı [öğrenin](remove-move-resources.md) .