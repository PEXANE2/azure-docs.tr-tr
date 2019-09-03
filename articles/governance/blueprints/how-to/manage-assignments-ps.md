---
title: PowerShell ile atamaları yönetme
description: Resmi Azure şemaları PowerShell modülü az. şema ile şema atamalarını yönetmeyi öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: beaa3f4c5ab272592e7fae5a95b40a9b586aaf65
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232895"
---
# <a name="how-to-manage-assignments-with-powershell"></a>PowerShell ile atamaları yönetme

Şema atama **az. şema** Azure PowerShell modülü kullanılarak yönetilebilir. Modül, atamaları getirmeyi, oluşturmayı, güncelleştirmeyi ve kaldırmayı destekler. Modül, var olan şema tanımlarında ayrıntılar da getirebilir. Bu makalede, modülün nasıl yükleneceği ve nasıl kullanılacağı ele alınmaktadır.

## <a name="add-the-azblueprint-module"></a>Az. Blueprint modülünü ekleyin

Şema atamalarını yönetmek için Azure PowerShell etkinleştirmek üzere modülün eklenmesi gerekir. Bu modül, [Azure Cloud Shell](https://shell.azure.com)ile veya [Azure PowerShell Docker görüntüsüyle](https://hub.docker.com/r/azuresdk/azure-powershell/)yerel olarak yüklü PowerShell ile kullanılabilir.

### <a name="base-requirements"></a>Temel gereksinimler

Azure şemaları modülü için aşağıdaki yazılımlar gereklidir:

- Azure PowerShell 1.5.0 veya üzeri. Henüz yüklenmiş değilse, [bu yönergeleri](/powershell/azure/install-az-ps) izleyin.
- PowerShellGet 2.0.1 veya üzeri. Henüz yüklenmiş ve güncellenmiş değilse, [bu yönergeleri](/powershell/gallery/installing-psget) izleyin.

### <a name="install-the-module"></a>Modülü yükler

PowerShell için planlar modülü **az. Blueprint**olur.

1. Bir **Yönetim** PowerShell isteminde aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   # Install the Blueprints module from PowerShell Gallery
   Install-Module -Name Az.Blueprint
   ```

   > [!NOTE]
   > **Az. Accounts** zaten yüklüyse, yüklemeyi zorlamak için kullanılması `-AllowClobber` gerekebilir.

1. Modülün içeri aktarıldığını ve doğru sürüm (0.1.0) olduğunu doğrulayın:

   ```azurepowershell-interactive
   # Get a list of commands for the imported Az.Blueprint module
   Get-Command -Module 'Az.Blueprint' -CommandType 'Cmdlet'
   ```

## <a name="get-blueprint-definitions"></a>Şema tanımlarını al

Atama ile çalışmanın ilk adımı, genellikle bir şema tanımına başvuru almaktır.
`Get-AzBlueprint` Cmdlet 'i bir veya daha fazla şema tanımı alır. Cmdlet 'i ile olan bir yönetim grubundan `-ManagementGroupId {mgId}` şema tanımlarını veya ile `-SubscriptionId {subId}`aboneliği alabilir. **Name** parametresi bir şema tanımı alır, ancak **ManagementGroupId** veya **SubscriptionID**ile birlikte kullanılmalıdır. **Sürüm** , **ad** ile birlikte kullanılabilir ve bu, hangi şema tanımının döndürüldüğünden daha açık olacaktır. **Sürüm**yerine, anahtar `-LatestPublished` en son yayımlanan sürümü de dönüştürür.

Aşağıdaki örnek, ' `Get-AzBlueprint` 101-planlar-Definition-Subscription ' adlı bir şema tanımının tüm sürümlerini şu şekilde `{subId}`gösterilen belirli bir abonelikten almak için kullanır:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get all versions of the blueprint definition in the specified subscription
$blueprints = Get-AzBlueprint -SubscriptionId '{subId}' -Name '101-blueprints-definition-subscription'

# Display the blueprint definition object
$blueprints
```

Birden çok sürümü olan bir şema tanımının örnek çıktısı şuna benzer:

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

Şema tanımındaki [şema parametreleri](../concepts/parameters.md#blueprint-parameters) daha fazla bilgi sağlamak için genişletilebilir.

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

## <a name="get-blueprint-assignments"></a>Şema atamalarını al

Şema ataması zaten varsa `Get-AzBlueprintAssignment` cmdlet 'ini kullanarak buna bir başvuru alabilirsiniz. Cmdlet 'i, **SubscriptionID** ve **adı** isteğe bağlı parametreler olarak alır. **SubscriptionID** belirtilmemişse, geçerli abonelik bağlamı kullanılır.

Aşağıdaki örnek, olarak `Get-AzBlueprintAssignment` `{subId}`belirtilen belirli bir aboneliğin ' atama-kilitleme-kaynak-gruplar ' adlı tek bir şema atamasını almak için kullanır:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the blueprint assignment in the specified subscription
$blueprintAssignment = Get-AzBlueprintAssignment -SubscriptionId '{subId}' -Name 'Assignment-lock-resource-groups'

# Display the blueprint assignment object
$blueprintAssignment
```

Bir şema atamasının örnek çıktısı şöyle görünür:

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

## <a name="create-blueprint-assignments"></a>Şema atamaları oluşturma

Şema ataması henüz yoksa `New-AzBlueprintAssignment` cmdlet 'ini kullanarak oluşturabilirsiniz. Bu cmdlet şu parametreleri kullanır:

- **Ad** istenir
  - Şema atamasının adını belirtir
  - Benzersiz olmalı ve **abonelik kimliği** zaten mevcut değil
- **Blueprint** istenir
  - Atanacak şema tanımını belirtir
  - Başvuru `Get-AzBlueprint` nesnesini almak için kullanın
- **Konum** istenir
  - Sistem tarafından atanan yönetilen kimlik ve abonelik dağıtım nesnesinin oluşturulacağı bölgeyi belirtir
- **Abonelik** seçim
  - Atamanın dağıtıldığı aboneliği belirtir
  - Sağlanmazsa, varsayılan olarak geçerli abonelik bağlamına
- **Kilitle** seçim
  - Dağıtılan kaynaklar için kullanılacak [şema kaynak kilitlemeyi](../concepts/resource-locking.md) tanımlar
  - Desteklenen seçenekler: _None_, _allresourcesreadonly_, _allresourcesdonotdelete_
  - Sağlanmazsa varsayılan değer _none_ olur
- **Systemassignedıdentity** seçim
  - Atama için sistem tarafından atanan yönetilen kimlik oluşturmak ve kaynakları dağıtmak için seçin
  - "Identity" parametre kümesi için varsayılan
  - **Useratandıdentity** ile kullanılamaz
- **Useratandıdentity** seçim
  - Atama ve kaynakları dağıtma için kullanılacak kullanıcı tarafından atanan yönetilen kimliği belirtir
  - "Identity" parametre kümesinin bir parçası
  - **Systemassignedıdentity** ile kullanılamaz
- **Parametre** seçim
  - Şema atamasında [dinamik parametreleri](../concepts/parameters.md#dynamic-parameters) ayarlamak için anahtar/değer çiftlerinin [karma tablosu](/powershell/module/microsoft.powershell.core/about/about_hash_tables)
  - Bir dinamik parametre için varsayılan değer, tanımdaki **DefaultValue** 'dir
  - Bir parametre sağlanmazsa ve **DefaultValue**yoksa, parametre isteğe bağlı değildir

    > [!NOTE]
    > **Parametre** securestrings 'i desteklemiyor.

- **Resourcegroupparameter** seçim
  - Kaynak grubu yapıtlarının [karma tablosu](/powershell/module/microsoft.powershell.core/about/about_hash_tables)
  - Her kaynak grubu yapıt yer tutucusu, bu kaynak grubu yapıtı üzerinde dinamik olarak Setting **adı** ve/veya **konumu** için bir anahtar/değer çiftine sahip olacaktır
  - Bir kaynak grubu parametresi sağlanmazsa ve **DefaultValue**yoksa, kaynak grubu parametresi isteğe bağlı değildir

Aşağıdaki örnek, ' My-şema ' şeması ile `Get-AzBlueprint`getirilen ' 1,1 ' sürümünün yeni bir atamasını oluşturur, yönetilen kimliği ve atama nesnesi konumunu ' westus2 ' olarak ayarlar, kaynakları _allresourcesreadonly ile kilitler_ ve belirli bir abonelikte hem **parametre** hem de **resourcegroupparameter** için karma tabloları şu şekilde gösterildiği gibi `{subId}`ayarlar:

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
    -SubscriptionId '{subId}' -Location 'westus2' -Lock AllResourcesReadyOnly `
    -Parameter $bpParameters -ResourceGroupParameter $bpRGParameters
```

Bir şema ataması oluşturmak için örnek çıktı şöyle görünür:

```output
Name              : my-blueprint-assignment
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssi
                    gnments/my-blueprint-assignment
Scope             : /subscriptions/{subId}
LastModified      : 2019-03-13
LockMode          : AllResourcesReadyOnly
ProvisioningState : Creating
Parameters        : {storageAccount_storageAccountType}
ResourceGroups    : ResourceGroup
```

## <a name="update-blueprint-assignments"></a>Şema atamalarını Güncelleştir

Bazen zaten oluşturulmuş bir şema atamasının güncelleştirilmesi gerekir. `Set-AzBlueprintAssignment` Cmdlet 'i bu eylemi işler. Cmdlet 'i `New-AzBlueprintAssignment` cmdlet 'in kullandığı parametrelerin çoğunu alır ve atamada ayarlanan her şeyin güncelleştirilmesine izin verir. Bunun özel durumları, _ad_, _Blueprint_ve _SubscriptionID_' dir. Yalnızca belirtilen değerler güncellenir.

Bir şema atamasını güncelleştirirken ne olduğunu anlamak için bkz. [atamaları güncelleştirme kuralları](./update-existing-assignments.md#rules-for-updating-assignments).

- **Ad** istenir
  - Güncelleştirilecek BLUEPRINT atamasının adını belirtir
  - Güncelleştirme atamasını bulmak için kullanılır, atamayı değiştirmez
- **Blueprint** istenir
  - Şema atamasının şema tanımını belirtir
  - Başvuru `Get-AzBlueprint` nesnesini almak için kullanın
  - Güncelleştirme atamasını bulmak için kullanılır, atamayı değiştirmez
- **Konum** seçim
  - Sistem tarafından atanan yönetilen kimlik ve abonelik dağıtım nesnesinin oluşturulacağı bölgeyi belirtir
- **Abonelik** seçim
  - Atamanın dağıtıldığı aboneliği belirtir
  - Sağlanmazsa, varsayılan olarak geçerli abonelik bağlamına
  - Güncelleştirme atamasını bulmak için kullanılır, atamayı değiştirmez
- **Kilitle** seçim
  - Dağıtılan kaynaklar için kullanılacak [şema kaynak kilitlemeyi](../concepts/resource-locking.md) tanımlar
  - Desteklenen seçenekler: _None_, _allresourcesreadonly_, _allresourcesdonotdelete_
- **Systemassignedıdentity** seçim
  - Atama için sistem tarafından atanan yönetilen kimlik oluşturmak ve kaynakları dağıtmak için seçin
  - "Identity" parametre kümesi için varsayılan
  - **Useratandıdentity** ile kullanılamaz
- **Useratandıdentity** seçim
  - Atama ve kaynakları dağıtma için kullanılacak kullanıcı tarafından atanan yönetilen kimliği belirtir
  - "Identity" parametre kümesinin bir parçası
  - **Systemassignedıdentity** ile kullanılamaz
- **Parametre** seçim
  - Şema atamasında [dinamik parametreleri](../concepts/parameters.md#dynamic-parameters) ayarlamak için anahtar/değer çiftlerinin [karma tablosu](/powershell/module/microsoft.powershell.core/about/about_hash_tables)
  - Bir dinamik parametre için varsayılan değer, tanımdaki **DefaultValue** 'dir
  - Bir parametre sağlanmazsa ve **DefaultValue**yoksa, parametre isteğe bağlı değildir

    > [!NOTE]
    > **Parametre** securestrings 'i desteklemiyor.

- **Resourcegroupparameter** seçim
  - Kaynak grubu yapıtlarının [karma tablosu](/powershell/module/microsoft.powershell.core/about/about_hash_tables)
  - Her kaynak grubu yapıt yer tutucusu, bu kaynak grubu yapıtı üzerinde dinamik olarak Setting **adı** ve/veya **konumu** için bir anahtar/değer çiftine sahip olacaktır
  - Bir kaynak grubu parametresi sağlanmazsa ve **DefaultValue**yoksa, kaynak grubu parametresi isteğe bağlı değildir

Aşağıdaki örnek, kilit modunu değiştirerek ' My-şema ' şema tanımının `Get-AzBlueprint` ' 1,1 ' sürümünün atamasını güncelleştirir:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'

# Update the existing blueprint assignment
$bpAssignment = Set-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Lock AllResourcesDoNotDelete
```

Bir şema ataması oluşturmak için örnek çıktı şöyle görünür:

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

## <a name="remove-blueprint-assignments"></a>Şema atamalarını kaldır

Bir şema atamasının kaldırılması sırasında, `Remove-AzBlueprintAssignment` cmdlet bu eylemi işler. Cmdlet 'i hangi şema atamasının kaldırılacağını belirtmek için **Name** ya da **InputObject** değerini alır. **SubscriptionID** _gereklidir_ ve her durumda belirtilmelidir.

Aşağıdaki örnek, ile `Get-AzBlueprintAssignment` var olan bir şema atamasını getirir ve bu atamayı şöyle `{subId}`temsil eden belirli bir abonelikten kaldırır:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the blueprint assignment in the specified subscription
$blueprintAssignment = Get-AzBlueprintAssignment -Name 'Assignment-lock-resource-groups'

# Remove the existing blueprint assignment
Remove-AzBlueprintAssignment -InputObject $blueprintAssignment -SubscriptionId '{subId}'
```

## <a name="end-to-end-code-example"></a>Uçtan uca kod örneği

Tüm adımları bir araya getiren aşağıdaki örnek, şema tanımını alır, ardından belirli bir abonelikte şu şekilde `{subId}`gösterilen bir şema atamasını oluşturur, güncelleştirir ve kaldırır:

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
    -SubscriptionId '{subId}' -Location 'westus2' -Lock AllResourcesReadyOnly `
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

- [Şema yaşam döngüsü](../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.