---
title: PowerShell ile atamaları yönetme
description: Resmi Azure Blueprints PowerShell modülü Az.Blueprint ile plan atamalarını nasıl yönettemize edin.
ms.date: 09/30/2019
ms.topic: how-to
ms.openlocfilehash: 0868e5e207202511c1981a930870bfdc68a77a8f
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80677436"
---
# <a name="how-to-manage-assignments-with-powershell"></a>PowerShell ile atamaları yönetme

Bir plan ataması **Az.Blueprint** Azure PowerShell modülü kullanılarak yönetilebilir. Modül, atamaların alınmasını, oluşturulmasını, güncellenmesini ve kaldırılmasını destekler. Modül ayrıca varolan plan tanımları hakkında bilgi alabilir. Bu makalede, modülün nasıl yüklenir ve kullanmaya başlamak için kapsar.

## <a name="add-the-azblueprint-module"></a>Az.Blueprint modülekle

Azure PowerShell'in plan atamalarını yönetmesini sağlamak için modülün eklenmesi gerekir. Bu modül yerel olarak yüklenen PowerShell ile, [Azure Cloud Shell](https://shell.azure.com)ile veya Azure [PowerShell Docker görüntüsüyle](https://hub.docker.com/r/azuresdk/azure-powershell/)kullanılabilir.

### <a name="base-requirements"></a>Temel gereksinimler

Azure Blueprints modülü aşağıdaki yazılımı gerektirir:

- Azure PowerShell 1.5.0 veya üzeri. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/azure/install-az-ps) izleyin.
- PowerShellGet 2.0.1 veya üzeri. Henüz yüklenmiş ve güncellenmiş değilse, [bu yönergeleri](/powershell/scripting/gallery/installing-psget) izleyin.

### <a name="install-the-module"></a>Modülü yükleyin

PowerShell için Azure Planları modülü **Az.Blueprint'tir.**

1. **İdari** powershell isteminden aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   # Install the Azure Blueprints module from PowerShell Gallery
   Install-Module -Name Az.Blueprint
   ```

   > [!NOTE]
   > **Az.Accounts** zaten yüklüyse, yüklemeyi zorlamak `-AllowClobber` için kullanmanız gerekebilir.

1. Modülün içe aktarıldığını ve doğru sürüm olduğunu doğrulayın (0.2.6):

   ```azurepowershell-interactive
   # Get a list of commands for the imported Az.Blueprint module
   Get-Command -Module 'Az.Blueprint' -CommandType 'Cmdlet'
   ```

## <a name="get-blueprint-definitions"></a>Plan tanımlarını alın

Bir atamayla çalışmanın ilk adımı genellikle bir plan tanımına başvuru almaktır.
Cmdlet `Get-AzBlueprint` bir veya daha fazla plan tanımları alır. Cmdlet ile bir yönetim grubundan veya `-ManagementGroupId {mgId}` bir abonelik `-SubscriptionId {subId}`ile plan tanımları alabilirsiniz . **Ad** parametresi bir plan tanımı alır, ancak **ManagementGroupId** veya **SubscriptionId**ile kullanılmalıdır. **Sürüm,** hangi plan tanımının döndürüldüğü konusunda daha açık olmak için **Ad** ile birlikte kullanılabilir. **Sürüm**yerine, anahtar `-LatestPublished` en son yayınlanan sürümü kapmak.

Aşağıdaki örnek, `Get-AzBlueprint` '101-blueprints-definition-subscription' adlı bir plan tanımının tüm sürümlerini `{subId}`belirli bir abonelikten almak için kullanır:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get all versions of the blueprint definition in the specified subscription
$blueprints = Get-AzBlueprint -SubscriptionId '{subId}' -Name '101-blueprints-definition-subscription'

# Display the blueprint definition object
$blueprints
```

Birden çok sürümü olan bir plan tanımı için örnek çıktı aşağıdaki gibi görünür:

```output
Name                 : 101-blueprints-definition-subscription
Id                   : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprints/101
                       -blueprints-definition-subscription
DefinitionLocationId : {subId}
Versions             : {1.0, 1.1}
TimeCreated          : 2019-02-25
TargetScope          : Subscription
Parameters           : {storageAccount_storageAccountType, storageAccount_location,
                       allowedlocations_listOfAllowedLocations, [Usergrouporapplicationname]:Reader_RoleAssignmentName}
ResourceGroups       : ResourceGroup
```

Plan tanımındaki [plan parametreleri](../concepts/parameters.md#blueprint-parameters) daha fazla bilgi sağlamak için genişletilebilir.

```azurepowershell-interactive
$blueprints.Parameters
```

```output
Key                                                    Value
---                                                    -----
storageAccount_storageAccountType                      Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
storageAccount_location                                Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
allowedlocations_listOfAllowedLocations                Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
[Usergrouporapplicationname]:Reader_RoleAssignmentName Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
```

## <a name="get-blueprint-assignments"></a>Plan atamaları alın

Plan ataması zaten varsa, `Get-AzBlueprintAssignment` cmdlet ile bir referans alabilirsiniz. Cmdlet isteğe bağlı parametreler olarak **SubscriptionId** ve **Ad** alır. **SubscriptionId** belirtilmemişse, geçerli abonelik bağlamı kullanılır.

Aşağıdaki örnek, `Get-AzBlueprintAssignment` belirli bir abonelikten 'Atama-kilit-kaynak grupları' adlı tek bir `{subId}`plan ataması almak için kullanır:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the blueprint assignment in the specified subscription
$blueprintAssignment = Get-AzBlueprintAssignment -SubscriptionId '{subId}' -Name 'Assignment-lock-resource-groups'

# Display the blueprint assignment object
$blueprintAssignment
```

Plan ataması için örnek çıktı aşağıdaki gibi görünür:

```output
Name              : Assignment-lock-resource-groups
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssignme
                    nts/Assignment-lock-resource-groups
Scope             : /subscriptions/{subId}
LastModified      : 2019-02-19
LockMode          : AllResourcesReadOnly
ProvisioningState : Succeeded
Parameters        :
ResourceGroups    : ResourceGroup
```

## <a name="create-blueprint-assignments"></a>Plan atamaları oluşturma

Plan ataması henüz yoksa, `New-AzBlueprintAssignment` cmdlet ile oluşturabilirsiniz. Bu cmdlet aşağıdaki parametreleri kullanır:

- **Ad** [gerekli]
  - Plan atamasının adını belirtir
  - Benzersiz olmalı ve **SubscriptionId'de** zaten mevcut olmamalı
- **Plan** [gerekli]
  - Atamak için plan tanımını belirtir
  - Başvuru `Get-AzBlueprint` nesnesini almak için kullanın
- **Konum** [gerekli]
  - Oluşturulacak sistem atanmış yönetilen kimlik ve abonelik dağıtım nesnesi için bölgeyi belirtir
- **Abonelik** (isteğe bağlı)
  - Atamanın dağıtılan aboneliği belirtir
  - Sağlanmadıysa, geçerli abonelik bağlamında varsayılan
- **Kilitle** (isteğe bağlı)
  - Dağıtılan kaynaklar için kullanılacak [plan kaynağı kilitlemeyi](../concepts/resource-locking.md) tanımlar
  - Desteklenen seçenekler: _Yok_, _AllResourcesReadOnly_, _AllResourcesDoNotDelete_
  - Sağlanmazsa, _varsayılan_ olarak Yok
- **SystemAssignedIdentity** (isteğe bağlı)
  - Atama için sistem tarafından atanmış yönetilen bir kimlik oluşturmak ve kaynakları dağıtmak için seçin
  - "Kimlik" parametre kümesi için varsayılan
  - **UserAssignedIdentity** ile kullanılamaz
- **UserAssignedIdentity** (isteğe bağlı)
  - Atama için kullanmak ve kaynakları dağıtmak için kullanıcı tarafından atanan yönetilen kimliği belirtir
  - "Kimlik" parametre kümesinin bir parçası
  - **SystemAssignedIdentity** ile kullanılamaz
- **Parametre** (isteğe bağlı)
  - Plan ataması üzerinde [dinamik parametreleri](../concepts/parameters.md#dynamic-parameters) ayarlamak için anahtar/değer çiftlerinin [karma tablosu](/powershell/module/microsoft.powershell.core/about/about_hash_tables)
  - Dinamik bir parametre için varsayılan değer, tanımdaki **varsayılan Değerdir**
  - Bir parametre sağlanmadıysa ve **varsayılan Değeri**yoksa, parametre isteğe bağlı değildir

    > [!NOTE]
    > **Parametre** secureStrings'i desteklemez.

- **ResourceGroupParameter** (isteğe bağlı)
  - Kaynak grubu yapıtlarının [karma tablosu](/powershell/module/microsoft.powershell.core/about/about_hash_tables)
  - Her kaynak grubu yapı yı, bu kaynak grubu artifakında **Ad** ve **Konum'u** dinamik olarak ayarlamak için anahtar/değer çiftleri vardır
  - Kaynak grubu parametresi sağlanmadıysa ve **varsayılan Değeri**yoksa, kaynak grubu parametresi isteğe bağlı değildir
- **Atama Dosyası** (isteğe bağlı)
  - Bir plan atamasının JSON dosya gösterimine giden yol
  - Bu parametre, yalnızca **Ad**, **Plan**ve **SubscriptionId**ve ortak parametreleri içeren powershell parametre kümesinin bir parçasıdır.

### <a name="example-1-provide-parameters"></a>Örnek 1: Parametreleri sağlama

Aşağıdaki örnek, 'my-blueprint' plan tanımıile `Get-AzBlueprint`getirilen '1.1' sürümünün yeni bir atamasını oluşturur, yönetilen kimlik ve atama nesnesi konumunu 'westus2'ye ayarlar, kaynakları _AllResourcesReadOnly_ile kilitler ve belirli abonelikte hem **Parametre** hem de **ResourceGroupParameter** için karma tabloları `{subId}`ayarlar:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'

# Create the hash table for Parameters
$bpParameters = @{storageAccount_storageAccountType='Standard_GRS'}

# Create the hash table for ResourceGroupParameters
# ResourceGroup is the resource group artifact placeholder name
$bpRGParameters = @{ResourceGroup=@{name='storage_rg';location='westus2'}}

# Create the new blueprint assignment
$bpAssignment = New-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Location 'westus2' -Lock AllResourcesReadOnly `
    -Parameter $bpParameters -ResourceGroupParameter $bpRGParameters
```

Plan ataması oluşturmak için örnek çıktı aşağıdaki gibi görünür:

```output
Name              : my-blueprint-assignment
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssi
                    gnments/my-blueprint-assignment
Scope             : /subscriptions/{subId}
LastModified      : 2019-03-13
LockMode          : AllResourcesReadOnly
ProvisioningState : Creating
Parameters        : {storageAccount_storageAccountType}
ResourceGroups    : ResourceGroup
```

### <a name="example-2-use-a-json-assignment-definition-file"></a>Örnek 2: JSON atama tanım dosyası kullanma

Aşağıdaki örnek, [Örnek 1](#example-1-provide-parameters)ile hemen hemen aynı atamayı oluşturur.
Örnek, parametreleri cmdlet'e geçirmek yerine, JSON atama tanım dosyasının ve **AssignmentFile** parametresinin kullanımını gösterir. Ayrıca, **dışlanan Principals** özelliği **kilitlerin**bir parçası olarak yapılandırılır. **Dışlanan Principals** için bir PowerShell parametresi yoktur ve özellik yalnızca JSON atama tanım dosyası üzerinden ayarlayarak yapılandırılabilir.

```json
{
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "westus2",
  "properties": {
    "description": "Assignment of the 101-blueprint-definition-subscription",
    "blueprintId": "/subscriptions/{subId}/providers/Microsoft.Blueprint/blueprints/101-blueprints-definition-subscription",
    "locks": {
      "mode": "AllResourcesReadOnly",
      "excludedPrincipals": [
          "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
          "38833b56-194d-420b-90ce-cff578296714"
      ]
    },
    "parameters": {
      "storageAccount_storageAccountType": {
        "value": "Standard_GRS"
      }
    },
    "resourceGroups": {
      "ResourceGroup": {
        "name": "storage_rg",
        "location": "westus2"
      }
    }
  }
}
```

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Create the new blueprint assignment
$bpAssignment = New-AzBlueprintAssignment -Name 'my-blueprint-assignment' -SubscriptionId '{subId}' `
    -AssignmentFile '.\assignment.json'
```

Kullanıcı tarafından atanan yönetilen bir kimlik için JSON atama tanım dosyasının bir örneği için, Örnek: REST API için [kullanıcı tarafından atanan yönetilen kimlikle atama](/rest/api/blueprints/assignments/createorupdate#examples) daki istek gövdesine bakın.

## <a name="update-blueprint-assignments"></a>Plan atamalarını güncelleştirme

Bazen zaten oluşturulmuş bir plan ataması güncelleştirmek için gereklidir. Cmdlet `Set-AzBlueprintAssignment` bu eylemi yönetir. Cmdlet, `New-AzBlueprintAssignment` atamada ayarlanan her şeyin güncellenmesiiçin cmdlet'in yaptığı parametrelerin çoğunu alır. İstisnalar _Adı_, _Blueprint_ve _SubscriptionId_vardır. Yalnızca sağlanan değerler güncelleştirilir.

Plan atamasını güncellerken ne olduğunu anlamak [için atamaları güncelleştirme kurallarına](./update-existing-assignments.md#rules-for-updating-assignments)bakın.

- **Ad** [gerekli]
  - Güncelleştirilen plan atamasının adını belirtir
  - Atamayı değiştirmek için değil, güncelleştirmek için atamayı bulmak için kullanılır
- **Plan** [gerekli]
  - Plan atamasının plan tanımını belirtir
  - Başvuru `Get-AzBlueprint` nesnesini almak için kullanın
  - Atamayı değiştirmek için değil, güncelleştirmek için atamayı bulmak için kullanılır
- **Konum** (isteğe bağlı)
  - Oluşturulacak sistem atanmış yönetilen kimlik ve abonelik dağıtım nesnesi için bölgeyi belirtir
- **Abonelik** (isteğe bağlı)
  - Atamanın dağıtılan aboneliği belirtir
  - Sağlanmadıysa, geçerli abonelik bağlamında varsayılan
  - Atamayı değiştirmek için değil, güncelleştirmek için atamayı bulmak için kullanılır
- **Kilitle** (isteğe bağlı)
  - Dağıtılan kaynaklar için kullanılacak [plan kaynağı kilitlemeyi](../concepts/resource-locking.md) tanımlar
  - Desteklenen seçenekler: _Yok_, _AllResourcesReadOnly_, _AllResourcesDoNotDelete_
- **SystemAssignedIdentity** (isteğe bağlı)
  - Atama için sistem tarafından atanmış yönetilen bir kimlik oluşturmak ve kaynakları dağıtmak için seçin
  - "Kimlik" parametre kümesi için varsayılan
  - **UserAssignedIdentity** ile kullanılamaz
- **UserAssignedIdentity** (isteğe bağlı)
  - Atama için kullanmak ve kaynakları dağıtmak için kullanıcı tarafından atanan yönetilen kimliği belirtir
  - "Kimlik" parametre kümesinin bir parçası
  - **SystemAssignedIdentity** ile kullanılamaz
- **Parametre** (isteğe bağlı)
  - Plan ataması üzerinde [dinamik parametreleri](../concepts/parameters.md#dynamic-parameters) ayarlamak için anahtar/değer çiftlerinin [karma tablosu](/powershell/module/microsoft.powershell.core/about/about_hash_tables)
  - Dinamik bir parametre için varsayılan değer, tanımdaki **varsayılan Değerdir**
  - Bir parametre sağlanmadıysa ve **varsayılan Değeri**yoksa, parametre isteğe bağlı değildir

    > [!NOTE]
    > **Parametre** secureStrings'i desteklemez.

- **ResourceGroupParameter** (isteğe bağlı)
  - Kaynak grubu yapıtlarının [karma tablosu](/powershell/module/microsoft.powershell.core/about/about_hash_tables)
  - Her kaynak grubu yapı yı, bu kaynak grubu artifakında **Ad** ve **Konum'u** dinamik olarak ayarlamak için anahtar/değer çiftleri vardır
  - Kaynak grubu parametresi sağlanmadıysa ve **varsayılan Değeri**yoksa, kaynak grubu parametresi isteğe bağlı değildir

Aşağıdaki örnek, kilit modunu `Get-AzBlueprint` değiştirerek getirilen 'benim planım' plan tanımının '1.1' sürümünün atasını güncelleştirir:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'

# Update the existing blueprint assignment
$bpAssignment = Set-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Lock AllResourcesDoNotDelete
```

Plan ataması oluşturmak için örnek çıktı aşağıdaki gibi görünür:

```output
Name              : my-blueprint-assignment
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssi
                    gnments/my-blueprint-assignment
Scope             : /subscriptions/{subId}
LastModified      : 2019-03-13
LockMode          : AllResourcesDoNotDelete
ProvisioningState : Updating
Parameters        : {storageAccount_storageAccountType}
ResourceGroups    : ResourceGroup
```

## <a name="remove-blueprint-assignments"></a>Plan atamalarını kaldırma

Bir plan atamasının kaldırılması zamanı geldiğinde, `Remove-AzBlueprintAssignment` cmdlet bu eylemi işler. Cmdlet, hangi plan atamasının kaldırılacaklarını belirtmek için **Ad** veya **InputObject'i** alır. **SubscriptionId** _gereklidir_ ve her durumda sağlanmalıdır.

Aşağıdaki örnek, varolan bir `Get-AzBlueprintAssignment` plan atamasını ile birlikte getirir ve `{subId}`aşağıdaki gibi temsil edilen belirli abonelikten kaldırır:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the blueprint assignment in the specified subscription
$blueprintAssignment = Get-AzBlueprintAssignment -Name 'Assignment-lock-resource-groups'

# Remove the existing blueprint assignment
Remove-AzBlueprintAssignment -InputObject $blueprintAssignment -SubscriptionId '{subId}'
```

## <a name="end-to-end-code-example"></a>Uçuça kod örneği

Tüm adımları bir araya getirerek, aşağıdaki örnek plan tanımı alır, sonra oluşturur, güncelleştirmeler ve `{subId}`belirli bir abonelik olarak temsil edilen bir plan atama kaldırır:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

#region GetBlueprint
# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'
#endregion

#region CreateAssignment
# Create the hash table for Parameters
$bpParameters = @{storageAccount_storageAccountType='Standard_GRS'}

# Create the hash table for ResourceGroupParameters
# ResourceGroup is the resource group artifact placeholder name
$bpRGParameters = @{ResourceGroup=@{name='storage_rg';location='westus2'}}

# Create the new blueprint assignment
$bpAssignment = New-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Location 'westus2' -Lock AllResourcesReadOnly `
    -Parameter $bpParameters -ResourceGroupParameter $bpRGParameters
#endregion CreateAssignment

# Wait for the blueprint assignment to finish deployment prior to the next steps

#region UpdateAssignment
# Update the existing blueprint assignment
$bpAssignment = Set-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Lock AllResourcesDoNotDelete
#endregion UpdateAssignment

# Wait for the blueprint assignment to finish deployment prior to the next steps

#region RemoveAssignment
# Remove the existing blueprint assignment
Remove-AzBlueprintAssignment -InputObject $bpAssignment -SubscriptionId '{subId}'
#endregion
```

## <a name="next-steps"></a>Sonraki adımlar

- [Plan yaşam döngüsü](../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.