---
title: PowerShell ile ithalat ve ihracat planları
description: Kod olarak plan tanımlarınızla nasıl çalışacağınızı öğrenin. Dışa aktarma ve alma komutlarını kullanarak bunları paylaşın, kaynak denetimi ve yönetme.
ms.date: 09/03/2019
ms.topic: how-to
ms.openlocfilehash: fc7b9818072665d79deaf8a456868943e8428730
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873208"
---
# <a name="import-and-export-blueprint-definitions-with-powershell"></a>PowerShell ile plan tanımlarını alma ve ihraç etme

Azure Planları, Azure portalı üzerinden tam olarak yönetilebilir. Kuruluşlar Blueprints'i kullanırken ilerledikçe, plan tanımlarını yönetilen kod olarak düşünmeye başlamalıdırlar. Bu kavram genellikle Kod (IaC) olarak Altyapı olarak adlandırılır. Plan tanımlarınızı kod olarak ele almanız, Azure portalının sunduğunun ötesinde ek avantajlar sunar. Bu avantajlar şunlardır:

- Plan tanımlarını paylaşma
- Plan tanımlarınızı yedekleme
- Farklı kiracılarda veya aboneliklerde plan tanımlarını yeniden kullanma
- Plan tanımlarını kaynak denetimine yerleştirme
  - Test ortamlarında plan tanımlarının otomatik test edilmesi
  - Sürekli entegrasyon ve sürekli dağıtım (CI/CD) boru hatlarının desteklenmesi

Nedenleriniz ne olursa olsun, plan tanımlarınızı kod olarak yönetmenin yararları vardır. Bu makalede, [Az.Blueprint](https://powershellgallery.com/packages/Az.Blueprint/) modülündeki komutların `Import-AzBlueprintWithArtifact` ve `Export-AzBlueprintWithArtifact` komutların nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, Azure Planları'nın ılımlı çalışma bilgisi varsayar. Henüz yapmadıysanız, aşağıdaki makaleler üzerinde çalışın:

- [Portalda bir plan oluşturma](../create-blueprint-portal.md)
- Dağıtım [aşamaları](../concepts/deployment-stages.md) ve [plan yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin
- PowerShell ile plan tanımları ve atamaları [oluşturma](../create-blueprint-powershell.md) ve [yönetme](./manage-assignments-ps.md)

Henüz yüklenmemişse, PowerShell Galerisi'nden **Az.Blueprint** modüllerini yüklemek ve doğrulamak için [Az.Blueprint modülekle modülündeki](./manage-assignments-ps.md#add-the-azblueprint-module) yönergeleri izleyin.

## <a name="folder-structure-of-a-blueprint-definition"></a>Plan tanımının klasör yapısı

Planları dışa aktarma ve alma öncesine bakmadan önce, plan tanımını oluşturan dosyaların nasıl yapılandırıldığına bakalım. Plan tanımı kendi klasöründe depolanmalıdır.

> [!IMPORTANT]
> Cmdlet'in Ad parametresine değer aktarılamazsa, plan tanımının depolanan klasörünün adı kullanılır. **Name** `Import-AzBlueprintWithArtifact`

Adlandırılması `blueprint.json`gereken plan tanımının yanı sıra, plan tanımının oluşturduğu yapılar dır. Her yapı, '. `artifacts`
Klasörlerde JSON dosyaları olarak plan tanımınızın yapısı aşağıdaki gibi görünmelidir:

```text
.
|
|- MyBlueprint/  _______________ # Root folder name becomes default name of blueprint definition
|  |- blueprint.json  __________ # The blueprint definition. Fixed name.
|
|  |- artifacts/  ______________ # Subfolder for all blueprint artifacts. Fixed name.
|     |- artifact.json  ________ # Blueprint artifact as JSON file. Artifact named from file.
|     |- ...
|     |- more-artifacts.json

```

## <a name="export-your-blueprint-definition"></a>Plan tanımınızı dışa aktarma

Plan tanımınızı dışa aktarma adımları basittir. Plan tanımının dışa aktarılması, kaynak denetimine paylaşmak, yedeklemek veya yerleştirmek için yararlı olabilir.

- **Plan** [gerekli]
  - Plan tanımını belirtir
  - Başvuru `Get-AzBlueprint` nesnesini almak için kullanın
- **OutputPath** [gerekli]
  - Plan tanımı JSON dosyaları kaydetmek için yol belirtir
  - Çıktı dosyaları, plan tanımının adını içeren bir alt klasörde
- **Sürüm** (isteğe bağlı)
  - **Blueprint** başvuru nesnesi birden fazla sürüme başvurular içeriyorsa, sürümü çıktıolarak belirtir.

1. Aşağıdaki şekilde `{subId}`temsil edilen abonelikten dışa aktarmak için plan tanımına bir referans alın:

   ```azurepowershell-interactive
   # Login first with Connect-AzAccount if not using Cloud Shell

   # Get version '1.1' of the blueprint definition in the specified subscription
   $bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'MyBlueprint' -Version '1.1'
   ```

1. Belirtilen `Export-AzBlueprintWithArtifact` plan tanımını dışa aktarmak için cmdlet kullanın:

   ```azurepowershell-interactive
   Export-AzBlueprintWithArtifact -Blueprint $bpDefinition -OutputPath 'C:\Blueprints'
   ```

## <a name="import-your-blueprint-definition"></a>Plan tanımınızı alma

[Dışa aktarılan](#export-your-blueprint-definition) bir plan tanımına sahip olduktan veya [gerekli klasör yapısında](#folder-structure-of-a-blueprint-definition)el ile oluşturulmuş bir plan tanımına sahip olduktan sonra, bu plan tanımını farklı bir yönetim grubuna veya aboneye aktarabilirsiniz.

Yerleşik plan tanımları örnekleri için [Azure Blueprint GitHub repo'ya](https://github.com/Azure/azure-blueprints/tree/master/samples/builtins)bakın.

- **Ad** [gerekli]
  - Yeni plan tanımının adını belirtir
- **InputPath** [gerekli]
  - Plan tanımını oluşturmak için yolu belirtir
  - [Gerekli klasör yapısıyla](#folder-structure-of-a-blueprint-definition) eşleşmeli
- **ManagementGroupId** (isteğe bağlı)
  - Geçerli bağlam varsayılan değilse plan tanımı kaydetmek için yönetim grubu kimliği
  - **ManagementGroupId** veya **SubscriptionId** belirtilmelidir
- **SubscriptionId** (isteğe bağlı)
  - Plan tanımını geçerli bağlam varsayılanına kaydetmek için abonelik kimliği
  - **ManagementGroupId** veya **SubscriptionId** belirtilmelidir

1. Belirtilen `Import-AzBlueprintWithArtifact` plan tanımını almak için cmdlet kullanın:

   ```azurepowershell-interactive
   # Login first with Connect-AzAccount if not using Cloud Shell

   Import-AzBlueprintWithArtifact -Name 'MyBlueprint' -ManagementGroupId 'DevMG' -InputPath 'C:\Blueprints\MyBlueprint'
   ```

Plan tanımı alındıktan sonra [PowerShell ile atayın.](./manage-assignments-ps.md#create-blueprint-assignments)

Gelişmiş plan tanımları oluşturma hakkında bilgi için aşağıdaki makalelere bakın:

- [Statik ve dinamik parametreleri](../concepts/parameters.md)kullanın.
- [Plan sıralama sırasını](../concepts/sequencing-order.md)özelleştirin.
- Blueprint kaynak [kilitleme](../concepts/resource-locking.md)ile dağıtımları koruyun.
- [Planları Kod Olarak Yönetin.](https://github.com/Azure/azure-blueprints/blob/master/README.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Plan yaşam döngüsü](../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.