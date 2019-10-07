---
title: Uyumlu olmayan kaynakları düzeltme
description: Bu kılavuzda, Azure Ilkesindeki ilkelerle uyumlu olmayan kaynakların düzeltilme adımları gösterilmektedir.
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/09/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: 219a3c56f9e4e4c9e132fa759b017fac63ade766
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71977982"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Azure Ilkesiyle uyumlu olmayan kaynakları düzelt

Bir **Deployifnotexists** veya **MODIFY** Policy ile uyumlu olmayan kaynaklar, **Düzeltme**aracılığıyla uyumlu bir duruma yerleştirilebilir. Düzeltme, Azure Ilkesini, mevcut kaynaklarınız üzerinde atanan ilkenin **Deployifnotexists** efektini veya etiket **işlemlerini** çalıştırmak üzere karşılaştırarak gerçekleştirilir. Bu makalede, Azure Ilkesini anlamak ve düzeltmeyi gerçekleştirmek için gereken adımlar gösterilir.

## <a name="how-remediation-security-works"></a>Düzeltme güvenliğinin nasıl çalıştığı

Azure Ilkesi, şablonu **Deployifnotexists** ilke tanımında çalıştırdığında, bu, [yönetilen bir kimlik](../../../active-directory/managed-identities-azure-resources/overview.md)kullanılarak yapılır.
Azure Ilkesi, her atama için yönetilen bir kimlik oluşturur, ancak yönetilen kimliğe hangi rollerin verilmek üzere ayrıntıları içermelidir. Yönetilen kimliğin rolleri yoksa, ilke veya girişim ataması sırasında bu hata görüntülenir. Portal kullanılırken, Azure Ilkesi, atama başlatıldıktan sonra yönetilen kimliğe listelenen roller için otomatik olarak izin verir.

![Yönetilen Kimlik-eksik rol](../media/remediate-resources/missing-role.png)

> [!IMPORTANT]
> **Deployifnotexists** veya **MODIFY** tarafından değiştirilen bir kaynak ilke atamasının kapsamı dışındaysa veya şablon, ilke atamasının kapsamı dışında kaynaklardaki özelliklere erişirse, atamanın yönetilen kimliği olmalıdır [ el ile erişim izni verilmesi](#manually-configure-the-managed-identity) veya düzeltme dağıtımı başarısız olur.

## <a name="configure-policy-definition"></a>İlke tanımını Yapılandır

İlk adım, dahil edilen şablonunuzun içeriğini başarılı bir şekilde dağıtmak için, **dağıtım** ve ilke tanımında **yapılan ihtiyaçları karşılayan** rolleri tanımlamaktır. **Ayrıntılar** özelliği altında, bir **roledefinitionıds** özelliği ekleyin. Bu özellik, ortamınızdaki rollerle eşleşen dizelerin bir dizisidir. Tam bir örnek için, [Deployifnotexists örneğine](../concepts/effects.md#deployifnotexists-example) veya [değiştirme örneklerine](../concepts/effects.md#modify-examples)bakın.

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

**Roledefinitionıds** özelliği, tam kaynak tanımlayıcıyı kullanır ve rolün kısa **roleName** özelliğini almaz. Ortamınızdaki ' katkıda bulunan ' rolünün KIMLIĞINI almak için aşağıdaki kodu kullanın:

```azurecli-interactive
az role definition list --name 'Contributor'
```

## <a name="manually-configure-the-managed-identity"></a>Yönetilen kimliği el ile yapılandır

Portalı kullanarak bir atama oluştururken, Azure Ilkesi yönetilen kimliği oluşturur ve rol **Definitionıds**içinde tanımlanan rollere izin verir. Aşağıdaki koşullarda, yönetilen kimlik oluşturma ve bu izinleri atama adımlarının el ile yapılması gerekir:

- SDK kullanılırken (örneğin, Azure PowerShell)
- Atama kapsamı dışında bir kaynak şablon tarafından değiştirildiğinde
- Atama kapsamı dışında bir kaynak, şablon tarafından okunmasıyla

> [!NOTE]
> Azure PowerShell ve .NET şu anda bu özelliği destekleyen tek SDK 'lardır.

### <a name="create-managed-identity-with-powershell"></a>PowerShell ile yönetilen kimlik oluşturma

İlke ataması sırasında yönetilen bir kimlik oluşturmak için, **konumun** tanımlanması ve **atamadan kimlik** kullanılması gerekir. Aşağıdaki örnek, **SQL veritabanı saydam veri şifrelemesi dağıtma**, hedef kaynak grubunu ayarlayan ve sonra atamayı oluşturan yerleşik ilke tanımını alır.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the built-in "Deploy SQL DB transparent data encryption" policy definition
$policyDef = Get-AzPolicyDefinition -Id '/providers/Microsoft.Authorization/policyDefinitions/86a912f6-9a06-4e26-b447-11b16ba8659f'

# Get the reference to the resource group
$resourceGroup = Get-AzResourceGroup -Name 'MyResourceGroup'

# Create the assignment using the -Location and -AssignIdentity properties
$assignment = New-AzPolicyAssignment -Name 'sqlDbTDE' -DisplayName 'Deploy SQL DB transparent data encryption' -Scope $resourceGroup.ResourceId -PolicyDefinition $policyDef -Location 'westus' -AssignIdentity
```

@No__t-0 değişkeni, bir ilke ataması oluştururken döndürülen standart değerlerle birlikte yönetilen kimliğin asıl KIMLIĞINI de içerir. @No__t-0 üzerinden erişilebilir.

### <a name="grant-defined-roles-with-powershell"></a>PowerShell ile tanımlı roller verme

Yeni yönetilen kimliğin, gerekli rollere verilebilmesi için Azure Active Directory aracılığıyla çoğaltmayı tamamlaması gerekir. Çoğaltma tamamlandıktan sonra aşağıdaki örnek, **Roledefinitionıds** için `$policyDef` ' daki ilke tanımını yineler ve yeni yönetilen kimliğe roller vermek için [New-azroleatama](/powershell/module/az.resources/new-azroleassignment) kullanır.

```azurepowershell-interactive
# Use the $policyDef to get to the roleDefinitionIds array
$roleDefinitionIds = $policyDef.Properties.policyRule.then.details.roleDefinitionIds

if ($roleDefinitionIds.Count -gt 0)
{
    $roleDefinitionIds | ForEach-Object {
        $roleDefId = $_.Split("/") | Select-Object -Last 1
        New-AzRoleAssignment -Scope $resourceGroup.ResourceId -ObjectId $assignment.Identity.PrincipalId -RoleDefinitionId $roleDefId
    }
}
```

### <a name="grant-defined-roles-through-portal"></a>Portal aracılığıyla tanımlı roller verme

**Erişim denetimi (IAM)** kullanarak veya ilkeyi veya girişim atamasını düzenleyerek ve **Kaydet**' e tıklayarak, bir atamanın yönetilen kimliğine portalı kullanarak tanımlanmış rolleri vermenin iki yolu vardır.

Atamanın yönetilen kimliğine bir rol eklemek için aşağıdaki adımları izleyin:

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

1. Azure İlkesi sayfasının sol tarafından **Atamalar**'ı seçin.

1. Yönetilen bir kimliğe sahip olan atamayı bulun ve ada tıklayın.

1. Düzenleme sayfasında **atama kimliği** özelliğini bulun. Atama KIMLIĞI şöyle bir şey olacaktır:

   ```output
   /subscriptions/{subscriptionId}/resourceGroups/PolicyTarget/providers/Microsoft.Authorization/policyAssignments/2802056bfc094dfb95d4d7a5
   ```

   Yönetilen kimliğin adı, bu örnekte `2802056bfc094dfb95d4d7a5` olan atama kaynak KIMLIĞININ son bölümüdür. Atama kaynak KIMLIĞININ bu kısmını kopyalayın.

1. Rol tanımı el ile eklenmiş olması gereken kaynağa veya kaynak üst kapsayıcısına (kaynak grubu, abonelik, yönetim grubu) gidin.

1. Kaynaklar sayfasındaki **erişim denetimi (IAM)** bağlantısına tıklayın ve erişim denetimi sayfasının en üstünde **+ rol ataması Ekle** ' ye tıklayın.

1. İlke tanımındaki bir **Roledefinitionıds** ile eşleşen uygun rolü seçin.
   ' Azure AD Kullanıcı, Grup veya uygulama ' için varsayılan değer olarak ayarlanan **ata erişimini** bırak. **Seç** kutusunda, daha önce bulunan atama kaynağı kimliğinin bölümünü yapıştırın veya yazın. Arama tamamlandığında, KIMLIK ' i seçmek için aynı ada sahip nesneye tıklayın ve **Kaydet**' e tıklayın.

## <a name="create-a-remediation-task"></a>Düzeltme görevi oluşturma

### <a name="create-a-remediation-task-through-portal"></a>Portal aracılığıyla düzeltme görevi oluşturma

Değerlendirme sırasında, **Deployifnotexists** veya **değişiklik** efektlerine sahip ilke ataması uyumlu olmayan kaynaklar olup olmadığını belirler. Uyumlu olmayan kaynaklar bulunduğunda, Ayrıntılar **Düzeltme** sayfasında sağlanır. Uyumlu olmayan kaynaklara sahip ilkelerin listesi ile birlikte bir **Düzeltme görevi**tetikleme seçeneği vardır. Bu seçenek, **Deployifnotexists** şablonundan veya **değiştirme** işlemlerinden bir dağıtım oluşturur.

Bir **Düzeltme görevi**oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

   ![Tüm hizmetlerde Ilke ara](../media/remediate-resources/search-policy.png)

1. Azure Ilkesi sayfasının sol tarafındaki **Düzeltme** ' yi seçin.

   ![Ilke sayfasında düzeltme ' yi seçin](../media/remediate-resources/select-remediation.png)

1. Tüm **Deployifnotexists** ve uyumlu olmayan kaynaklarla ilke atamalarını **değiştirme** , sekme ve veri tablosunu düzeltme **ilkelerine** dahildir. Uyumlu olmayan kaynaklarla bir ilkeye tıklayın. **Yeni düzeltme görev** sayfası açılır.

   > [!NOTE]
   > **Düzeltme görevi** sayfasını açmak için alternatif bir yol, **Uyumluluk** sayfasında Ilkeyi bulup tıklattıktan sonra **Düzeltme görevi oluştur** düğmesine tıklamanız gerekir.

1. **Yeni düzeltme görevi** sayfasında, ilkenin atandığı alt kaynakları (tek tek kaynak nesneleri dahil) seçmek için **kapsam** üç noktayı kullanarak düzeltmek üzere kaynakları filtreleyin. Ayrıca, kaynakları daha fazla filtrelemek için **konumlar açılan konumlarını** kullanın. Yalnızca tabloda listelenen kaynaklar düzeltilmeyecektir.

   ![Düzelt-hangi kaynakların düzeltileceği seçin](../media/remediate-resources/select-resources.png)

1. Kaynaklar filtrelendikten sonra düzeltme görevini başlatmak için **Düzelt**'e tıklayın. İlke uyumluluk sayfası, görev ilerleme durumunun durumunu göstermek için **Düzeltme görevleri** sekmesine açılır.

   ![Düzeltme görevlerinin ilerlemesini düzelt](../media/remediate-resources/task-progress.png)

1. İlerleme hakkındaki ayrıntıları almak için ilke uyumluluğu sayfasından **Düzeltme görevi** ' ne tıklayın. Görev için kullanılan filtreleme, düzeltilen kaynakların bir listesi ile birlikte gösterilir.

1. Düzeltme **görevi** sayfasında, düzeltme görevinin dağıtımını ya da kaynağını görüntülemek için bir kaynağa sağ tıklayın. Bir hata iletisi gibi ayrıntıları görmek için satırın sonunda **ilgili olaylar** ' a tıklayın.

   ![Düzelt-kaynak görev bağlam menüsü](../media/remediate-resources/resource-task-context-menu.png)

Bir **Düzeltme görevi** aracılığıyla dağıtılan kaynaklar, ilke uyumluluğu sayfasındaki **dağıtılan kaynaklar** sekmesine eklenir.

### <a name="create-a-remediation-task-through-azure-cli"></a>Azure CLı aracılığıyla bir düzeltme görevi oluşturma

Azure CLı ile bir **Düzeltme görevi** oluşturmak için `az policy remediation` komutlarını kullanın. @No__t-0 ' yı abonelik KIMLIĞINIZLE ve `{myAssignmentId}` ' i, **Deployifnotexists** ile değiştirin ya da Ilke atama kimliğini **değiştirin** .

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Create a remediation for a specific assignment
az policy remediation create --name myRemediation --policy-assignment '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Diğer düzeltme komutları ve örnekleri için, [az Policy düzeltme](/cli/azure/policy/remediation) komutlarına bakın.

### <a name="create-a-remediation-task-through-azure-powershell"></a>Azure PowerShell aracılığıyla düzeltme görevi oluşturma

Azure PowerShell bir **Düzeltme görevi** oluşturmak için `Start-AzPolicyRemediation` komutlarını kullanın. @No__t-0 ' yı abonelik KIMLIĞINIZLE ve `{myAssignmentId}` ' i, **Deployifnotexists** ile değiştirin ya da Ilke atama kimliğini **değiştirin** .

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Create a remediation for a specific assignment
Start-AzPolicyRemediation -Name 'myRemedation' -PolicyAssignmentId '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Diğer düzeltme cmdlet 'leri ve örnekleri için bkz. [az. Policınsıghts](/powershell/module/az.policyinsights/#policy_insights) modülü.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](getting-compliance-data.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.