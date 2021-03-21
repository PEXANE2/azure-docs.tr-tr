---
title: PowerShell kullanarak Azure Image Builder hizmet izinlerini yapılandırma
description: PowerShell kullanarak izinler ve ayrıcalıklar dahil olmak üzere Azure VM görüntü Oluşturucu hizmeti gereksinimlerini yapılandırın
author: danielsollondon
ms.author: danis
ms.date: 03/05/2021
ms.topic: article
ms.service: virtual-machines
ms.subservice: image-builder
ms.collection: linux
ms.openlocfilehash: 9f8793b6ea0ba454b66c525c2d53c1de2197d539
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102440216"
---
# <a name="configure-azure-image-builder-service-permissions-using-powershell"></a>PowerShell kullanarak Azure Image Builder hizmet izinlerini yapılandırma

(AıB) için kaydolmanız durumunda bu, AıB hizmetine bir hazırlama kaynak grubu (IT_ *) oluşturma, yönetme ve silme izni verir ve bu, görüntü derlemesi için gerekli olan kaynak ekleme haklarına sahip olur. Bu işlem, başarılı bir kayıt sırasında aboneliğinizde kullanılabilir hale getirilen bir AıB hizmet sorumlusu adı (SPN) tarafından yapılır.

Azure VM Image Builder 'ın görüntüleri yönetilen görüntülere veya paylaşılan bir görüntü galerisine dağıtmasını sağlamak için, görüntüleri okuma ve yazma izinlerine sahip Azure Kullanıcı tarafından atanan bir kimlik oluşturmanız gerekir. Azure depolama 'ya erişiyorsanız, bu durumda özel veya ortak kapsayıcıları okumak için izinler gerekir.

Bir görüntü oluşturmadan önce izinleri ve ayrıcalıkları ayarlamalısınız. Aşağıdaki bölümlerde, PowerShell kullanarak olası senaryoların nasıl yapılandırılacağı ayrıntılı olarak anlatılmaktadır.

> [!IMPORTANT]
> Azure görüntü Oluşturucu Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="register-the-features"></a>Özellikleri kaydetme

İlk olarak, Azure Image Builder hizmetine kaydolmanız gerekir. Kayıt, hizmet iznini hazırlama kaynak grubu oluşturma, yönetme ve silme izni verir. Ayrıca hizmet, görüntü derlemesi için gerekli olan grubu kaynak ekleme haklarına sahiptir.

```powershell-interactive
Register-AzProviderFeature -FeatureName VirtualMachineTemplatePreview -ProviderNamespace Microsoft.VirtualMachineImages
```

## <a name="create-an-azure-user-assigned-managed-identity"></a>Azure Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Azure Image Builder, [Azure Kullanıcı tarafından atanan bir yönetilen kimlik](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)oluşturmanızı gerektirir. Azure Image Builder, görüntüleri okumak, görüntüleri yazmak ve Azure depolama hesaplarına erişmek için Kullanıcı tarafından atanan yönetilen kimliği kullanır. Aboneliğinizde belirli işlemleri yapmak için kimlik iznini verirsiniz.

> [!NOTE]
> Daha önce Azure Image Builder, görüntü kaynak gruplarına izin vermek için Azure görüntü Oluşturucu hizmet asıl adı 'nı (SPN) kullandı. SPN 'nin kullanılması kullanım dışı olacaktır. Bunun yerine Kullanıcı tarafından atanan bir yönetilen kimlik kullanın.

Aşağıdaki örnekte, Azure Kullanıcı tarafından atanan bir yönetilen kimliğin nasıl oluşturulacağı gösterilmektedir. Değişkenlerinizi ayarlamak için yer tutucu ayarlarını değiştirin.

| Ayar | Açıklama |
|---------|-------------|
| \<Resource group\> | Kullanıcı tarafından atanan yönetilen kimliğin oluşturulacağı kaynak grubu. |

```powershell-interactive
## Add AZ PS module to support AzUserAssignedIdentity
Install-Module -Name Az.ManagedServiceIdentity

$parameters = @{
    Name = 'aibIdentity'
    ResourceGroupName = '<Resource group>'
}
# create identity
New-AzUserAssignedIdentity @parameters
```

Azure Kullanıcı tarafından atanan kimlikler hakkında daha fazla bilgi için bkz. kimlik oluşturma hakkında [Azure Kullanıcı tarafından atanan yönetilen kimlik](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) belgeleri.

## <a name="allow-image-builder-to-distribute-images"></a>Görüntü oluşturucunun görüntüleri dağıtmasını sağlar

Görüntüleri (yönetilen görüntüler/paylaşılan görüntü Galerisi) dağıtmak için Azure Image Builder 'da, Azure görüntü Oluşturucu hizmeti 'nin bu kaynak gruplarına görüntüleri eklemesine izin verilmelidir. Gerekli izinleri vermek için, Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmanız ve görüntünün oluşturulduğu kaynak grubunda bu haklara izin vermeniz gerekir. Azure görüntü oluşturucunun, abonelikteki diğer kaynak gruplarındaki kaynaklara erişme **izni yok.** Derlemelerinizin başarısız olmasına engel olmak için erişime izin vermek üzere açık eylemler gerçekleştirmeniz gerekir.

Görüntü dağıtmak için, kaynak grubunda Kullanıcı tarafından atanan yönetilen kimliğe katkıda bulunan haklara izin vermeniz gerekmez. Ancak, Kullanıcı tarafından atanan yönetilen kimliğin `Actions` dağıtım kaynak grubunda aşağıdaki Azure izinleri olması gerekir:

```Actions
Microsoft.Compute/images/write
Microsoft.Compute/images/read
Microsoft.Compute/images/delete
```

Paylaşılan bir görüntü galerisine dağıtıyorsanız şunları da yapmanız gerekir:

```Actions
Microsoft.Compute/galleries/read
Microsoft.Compute/galleries/images/read
Microsoft.Compute/galleries/images/versions/read
Microsoft.Compute/galleries/images/versions/write
```

## <a name="permission-to-customize-existing-images"></a>Mevcut görüntüleri özelleştirme izni

Kaynak özel görüntülerden (yönetilen görüntüler/paylaşılan görüntü Galerisi) görüntü oluşturmak için Azure Image Builder 'ın, bu kaynak grupları içindeki görüntüleri okumasına izin verilmelidir. Gerekli izinleri vermek için, Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmanız ve görüntünün bulunduğu kaynak grubunda bu haklara izin vermeniz gerekir.

Var olan bir özel görüntüden oluştur:

```Actions
Microsoft.Compute/galleries/read
```

Var olan bir paylaşılan görüntü Galerisi sürümünden oluştur:

```Actions
Microsoft.Compute/galleries/read
Microsoft.Compute/galleries/images/read
Microsoft.Compute/galleries/images/versions/read
```

## <a name="permission-to-customize-images-on-your-vnets"></a>Sanal ağlarınızdaki görüntüleri özelleştirme izni

Azure Image Builder, aboneliğinizde mevcut bir VNET 'i dağıtma ve kullanma özelliğine sahiptir ve bu sayede, bağlı kaynaklara özelleştirmelerin erişmesine izin verir.

Mevcut bir VNET 'e bir VM dağıtmak için, kaynak grubunda Kullanıcı tarafından atanan yönetilen kimliğe katkıda bulunan haklara izin vermeniz gerekmez. Ancak, Kullanıcı tarafından atanan yönetilen kimliğin `Actions` VNET kaynak grubunda aşağıdaki Azure izinlerine ihtiyacı vardır:

```Actions
Microsoft.Network/virtualNetworks/read
Microsoft.Network/virtualNetworks/subnets/join/action
```

## <a name="create-an-azure-role-definition"></a>Azure rol tanımı oluşturma

Aşağıdaki örneklerde, önceki bölümlerde açıklanan eylemlerden bir Azure rol tanımı oluşturulur. Örnekler, kaynak grubu düzeyinde uygulanır. Örneklerin gereksinimleriniz için yeterince ayrıntılı olup olmadığını değerlendirin ve test edin. Senaryonuz için, belirli bir paylaşılan görüntü galerisine iyileştirmeniz gerekebilir.

Görüntü eylemleri okuma ve yazma işlemlerine izin verir. Ortamınız için nelerin uygun olduğuna karar verin. Örneğin, Azure Image Builder 'ın, *örnek-RG-1* kaynak grubundaki görüntüleri okumasına izin veren bir rol oluşturun ve kaynak grubuna görüntüleri yazın *-RG-2*.

### <a name="custom-image-azure-role-example"></a>Özel görüntü Azure rolü örneği

Aşağıdaki örnek, bir kaynak özel görüntüsünü kullanmak ve dağıtmak için bir Azure rolü oluşturur. Daha sonra özel rolü Azure Image Builder için Kullanıcı tarafından atanan yönetilen kimliğe verirsiniz.

Örnekteki değerlerin değiştirilmesini basitleştirmek için, önce aşağıdaki değişkenleri ayarlayın. Değişkenlerinizi ayarlamak için yer tutucu ayarlarını değiştirin.

| Ayar | Açıklama |
|---------|-------------|
| \<Subscription ID\> | Azure abonelik KIMLIĞINIZ |
| \<Resource group\> | Özel görüntü için kaynak grubu |

```powershell-interactive
$sub_id = "<Subscription ID>"
# Resource group - For Preview, image builder will only support creating custom images in the same Resource Group as the source managed image.
$imageResourceGroup = "<Resource group>"
$identityName = "aibIdentity"

# Use a web request to download the sample JSON description
$sample_uri="https://raw.githubusercontent.com/azure/azvmimagebuilder/master/solutions/12_Creating_AIB_Security_Roles/aibRoleImageCreation.json"
$role_definition="aibRoleImageCreation.json"

Invoke-WebRequest -Uri $sample_uri -Outfile $role_definition -UseBasicParsing

# Create a unique role name to avoid clashes in the same Azure Active Directory domain
$timeInt=$(get-date -UFormat "%s")
$imageRoleDefName="Azure Image Builder Image Def"+$timeInt

# Update the JSON definition placeholders with variable values
((Get-Content -path $role_definition -Raw) -replace '<subscriptionID>',$sub_id) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace '<rgName>', $imageResourceGroup) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace 'Azure Image Builder Service Image Creation Role', $imageRoleDefName) | Set-Content -Path $role_definition

# Create a custom role from the aibRoleImageCreation.json description file. 
New-AzRoleDefinition -InputFile $role_definition

# Get the user-identity properties
$identityNameResourceId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).Id
$identityNamePrincipalId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).PrincipalId

# Grant the custom role to the user-assigned managed identity for Azure Image Builder.
$parameters = @{
    ObjectId = $identityNamePrincipalId
    RoleDefinitionName = $imageRoleDefName
    Scope = '/subscriptions/' + $sub_id + '/resourceGroups/' + $imageResourceGroup
}

New-AzRoleAssignment @parameters
```

### <a name="existing-vnet-azure-role-example"></a>Mevcut VNET Azure rolü örneği

Aşağıdaki örnek, mevcut bir VNET görüntüsünü kullanmak ve dağıtmak için bir Azure rolü oluşturur. Daha sonra özel rolü Azure Image Builder için Kullanıcı tarafından atanan yönetilen kimliğe verirsiniz.

Örnekteki değerlerin değiştirilmesini basitleştirmek için, önce aşağıdaki değişkenleri ayarlayın. Değişkenlerinizi ayarlamak için yer tutucu ayarlarını değiştirin.

| Ayar | Açıklama |
|---------|-------------|
| \<Subscription ID\> | Azure abonelik KIMLIĞINIZ |
| \<Resource group\> | VNET kaynak grubu |

```powershell-interactive
$sub_id = "<Subscription ID>"
$res_group = "<Resource group>"
$identityName = "aibIdentity"

# Use a web request to download the sample JSON description
$sample_uri="https://raw.githubusercontent.com/azure/azvmimagebuilder/master/solutions/12_Creating_AIB_Security_Roles/aibRoleNetworking.json"
$role_definition="aibRoleNetworking.json"

Invoke-WebRequest -Uri $sample_uri -Outfile $role_definition -UseBasicParsing

# Create a unique role name to avoid clashes in the same AAD domain
$timeInt=$(get-date -UFormat "%s")
$networkRoleDefName="Azure Image Builder Network Def"+$timeInt

# Update the JSON definition placeholders with variable values
((Get-Content -path $role_definition -Raw) -replace '<subscriptionID>',$sub_id) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace '<vnetRgName>', $res_group) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace 'Azure Image Builder Service Networking Role',$networkRoleDefName) | Set-Content -Path $role_definition

# Create a custom role from the aibRoleNetworking.json description file
New-AzRoleDefinition -InputFile $role_definition

# Get the user-identity properties
$identityNameResourceId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).Id
$identityNamePrincipalId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).PrincipalId

# Assign the custom role to the user-assigned managed identity for Azure Image Builder
$parameters = @{
    ObjectId = $identityNamePrincipalId
    RoleDefinitionName = $networkRoleDefName
    Scope = '/subscriptions/' + $sub_id + '/resourceGroups/' + $res_group
}

New-AzRoleAssignment @parameters
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure Image Builder 'a genel bakış](../image-builder-overview.md).
