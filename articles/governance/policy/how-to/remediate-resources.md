---
title: Uyumlu olmayan kaynakları düzeltme
description: Bu kılavuz, Azure İlkesi'ndeki ilkelere uymayan kaynakların düzeltilmesinde size yol gösterin.
ms.date: 02/26/2020
ms.topic: how-to
ms.openlocfilehash: 71af5c81e0dce4d5c0a0461534f634db36bd66a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471396"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Azure İlkesi ile uyumlu olmayan kaynakları düzeltin

**Bir deployIfNotExists** veya **değişiklik** ilkesine uygun olmayan kaynaklar **Düzeltme**yoluyla uyumlu bir duruma getirilebilir. Düzeltme, Azure İlkesi'ne, ister bir yönetim grubu, ister abonelik, kaynak grubu veya tek bir kaynağa olsun, varolan kaynaklarınızda atanan ilkenin **dağıtımIfNotExists** efektini veya atanan **ilkenin** etiket işlemlerini çalıştırmasını talimat vererek gerçekleştirilir. Bu makalede, Azure İlkesi ile düzeltmeyi anlamak ve gerçekleştirmek için gereken adımlar gösterilmektedir.

## <a name="how-remediation-security-works"></a>Düzeltme güvenliği nasıl çalışır?

Azure İlkesi şablonu **deployIfNotExists** ilke tanımında çalıştırdığında, [bunu yönetilen](../../../active-directory/managed-identities-azure-resources/overview.md)bir kimlik kullanarak yapar.
Azure İlkesi her atama için yönetilen bir kimlik oluşturur, ancak yönetilen kimliğe hangi roller verilen roller hakkında ayrıntılara sahip olmalıdır. Yönetilen kimlik rolleri eksikse, bu hata ilke veya girişim atama sırasında görüntülenir. Azure İlkesi, portalı kullanırken, atama başladıktan sonra yönetilen kimliğe otomatik olarak listelenen rolleri verir. Yönetilen kimliğin _konumu_ Azure İlkesi ile çalışmasını etkilemez.

![Yönetilen kimlik - eksik rol](../media/remediate-resources/missing-role.png)

> [!IMPORTANT]
> **DeployIfNotExists** veya **değiştirerek** değiştirilen bir kaynak ilke atamasının kapsamı dışındaysa veya şablon ilke atamasının kapsamı dışındaki kaynaklardaki özelliklere erişirilirse, atamanın yönetilen [kimliğine el ile erişim verilmesi](#manually-configure-the-managed-identity) veya düzeltme dağıtımının başarısız olması gerekir.

## <a name="configure-policy-definition"></a>İlke tanımını yapılandırma

İlk adım, dahil edilen şablonunuzun içeriğini başarıyla dağıtmak için ilke tanımındaki gereksinimleri **dağıtan** rolleri tanımlamak ve **değiştirmektir.** **Ayrıntılar** özelliği altında, bir **roleDefinitionIds** özelliği ekleyin. Bu özellik, ortamınızdaki rolleri eşleştirebilen bir dizi dizedir. Tam bir örnek [için, bkz: deployIfNotExists örneği](../concepts/effects.md#deployifnotexists-example) veya [değişiklik örnekleri.](../concepts/effects.md#modify-examples)

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

**RoleDefinitionIds** özelliği tam kaynak tanımlayıcısını kullanır ve rolün kısa **rol Adı** almaz. Ortamınızdaki 'Katılımcı' rolünün kimliğini almak için aşağıdaki kodu kullanın:

```azurecli-interactive
az role definition list --name 'Contributor'
```

## <a name="manually-configure-the-managed-identity"></a>Yönetilen kimliği el ile yapılandırma

Portalı kullanarak bir atama oluştururken, Azure İlkesi hem yönetilen kimliği oluşturur hem de **roleDefinitionIds'de**tanımlanan rolleri verir. Aşağıdaki koşullarda, yönetilen kimliği oluşturmak ve izinleri atamak için adımlar el ile yapılmalıdır:

- SDK kullanırken (Azure PowerShell gibi)
- Atama kapsamı dışındaki bir kaynak şablon tarafından değiştirildiğinde
- Atama kapsamı dışındaki bir kaynak şablon tarafından okunduğunda

> [!NOTE]
> Azure PowerShell ve .NET, şu anda bu özelliği destekleyen tek SDK'dır.

### <a name="create-managed-identity-with-powershell"></a>PowerShell ile yönetilen kimlik oluşturma

İlkenin atanması sırasında yönetilen bir kimlik oluşturmak için **Konum** tanımlanmalı ve **Kimlik Atama** kullanılır. Aşağıdaki örnek, yerleşik ilke dağıtma **SQL DB saydam veri şifreleme**tanımı alır, hedef kaynak grubunu ayarlar ve sonra atama oluşturur.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the built-in "Deploy SQL DB transparent data encryption" policy definition
$policyDef = Get-AzPolicyDefinition -Id '/providers/Microsoft.Authorization/policyDefinitions/86a912f6-9a06-4e26-b447-11b16ba8659f'

# Get the reference to the resource group
$resourceGroup = Get-AzResourceGroup -Name 'MyResourceGroup'

# Create the assignment using the -Location and -AssignIdentity properties
$assignment = New-AzPolicyAssignment -Name 'sqlDbTDE' -DisplayName 'Deploy SQL DB transparent data encryption' -Scope $resourceGroup.ResourceId -PolicyDefinition $policyDef -Location 'westus' -AssignIdentity
```

Değişken `$assignment` şimdi bir ilke ataması oluştururken döndürülen standart değerleri ile birlikte yönetilen kimliğin temel kimliğini içerir. Üzerinden `$assignment.Identity.PrincipalId`erişilebilir.

### <a name="grant-defined-roles-with-powershell"></a>PowerShell ile tanımlanmış rolleri hibe

Yeni yönetilen kimliğin, gerekli rollerin verilebilmeleri için Azure Etkin Dizini aracılığıyla çoğaltmaişlemini tamamlaması gerekir. Çoğaltma tamamlandıktan sonra, aşağıdaki örnek rol `$policyDef` **DefinitionIds** için ilke tanımını yineler ve yeni yönetilen kimliğe rolleri vermek için [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) kullanır.

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

### <a name="grant-defined-roles-through-portal"></a>Portal aracılığıyla tanımlanmış rolleri hibe

Bir atamanın yönetilen kimliğine portalı kullanarak, **Access denetimini (IAM)** kullanarak veya ilke veya girişim atamasını düzenleyerek ve **Kaydet'i**tıklatarak tanımlanan rolleri vermenin iki yolu vardır.

Atamanın yönetilen kimliğine bir rol eklemek için aşağıdaki adımları izleyin:

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

1. Azure İlkesi sayfasının sol tarafından **Atamalar**'ı seçin.

1. Yönetilen bir kimliğe sahip atamayı bulun ve adı tıklatın.

1. Atama **Kimliği** özelliğini edit sayfasında bulun. Atama kimliği gibi bir şey olacaktır:

   ```output
   /subscriptions/{subscriptionId}/resourceGroups/PolicyTarget/providers/Microsoft.Authorization/policyAssignments/2802056bfc094dfb95d4d7a5
   ```

   Yönetilen kimliğin adı, atama kaynak kimliğinin son bölümüdür ve bu örnekte yer alan bir addır. `2802056bfc094dfb95d4d7a5` Atama kaynak kimliğinin bu bölümünü kopyalayın.

1. El ile eklenen rol tanımına ihtiyaç duyduğu kaynak veya kaynak üst kapsayıcısına (kaynak grubu, abonelik, yönetim grubu) gidin.

1. Kaynaklar sayfasındaki **Access denetimi (IAM)** bağlantısını tıklatın ve erişim denetimi sayfasının üst **kısmındaki rol atamasını ekle'** yi tıklatın.

1. İlke tanımındaki **bir roleDefinitionIds** eşleşen uygun rolü seçin.
   'Azure AD kullanıcısı, grubu veya uygulaması' varsayılanına **ayarlı erişim atay'ı** bırakın. **Seç** kutusunda, atama kaynak kimliğinin daha önce bulunan bölümünü yapıştırın veya yazın. Arama tamamlandıktan sonra, kimliği seçmek için aynı ada sahip nesneyi tıklatın ve **Kaydet'i**tıklatın.

## <a name="create-a-remediation-task"></a>Düzeltme görevi oluşturma

### <a name="create-a-remediation-task-through-portal"></a>Portal üzerinden düzeltme görevi oluşturma

Değerlendirme sırasında, **deployIfNotExists** veya **değişiklik** efektleri ile ilke ataması, uyumlu olmayan kaynaklar olup olmadığını belirler. Uyumlu olmayan kaynaklar bulunduğunda, ayrıntılar **Düzeltme** sayfasında sağlanır. Uyumlu olmayan kaynaklara sahip ilkeler listesiile birlikte bir **düzeltme görevi**tetikleme seçeneğidir. Bu seçenek, **deployIfNotExists** şablonundan veya **değiştirme** işlemlerinden dağıtım oluşturan seçenektir.

Düzeltme **görevi**oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

   ![Tüm Hizmetlerde Politika Ara](../media/remediate-resources/search-policy.png)

1. Azure İlkesi sayfasının sol tarafında **Düzeltme'yi** seçin.

   ![İlke sayfasında Düzeltme'yi seçin](../media/remediate-resources/select-remediation.png)

1. Tüm **deployIfNotExists** ve uyumlu olmayan kaynaklarla ilke atamaları **değiştirmek** sekme ve veri tablosu **düzeltmek için İlkeler** dahildir. Uyumlu olmayan kaynaklara sahip bir ilkeyi tıklatın. **Yeni düzeltme görev** sayfası açılır.

   > [!NOTE]
   > **Düzeltme görev** sayfasını açmanın alternatif bir **yolu, Uyumluluk** sayfasından ilkeyi bulup tıklatmak ve ardından Düzeltme Görevi **Oluştur** düğmesini tıklatmaktır.

1. Yeni **düzeltme görev** sayfasında, iidiaatanan yerden alt kaynakları seçmek için **Kapsam** elipslerini kullanarak düzeltecek kaynakları filtreleyin (tek tek kaynak nesnelerine kadar). Ayrıca, kaynakları daha fazla filtrelemek için **Konumlar** açılır günlerini kullanın. Yalnızca tabloda listelenen kaynaklar düzeltilecektir.

   ![Düzeltin - hangi kaynakları düzeltecek lerini seçin](../media/remediate-resources/select-resources.png)

1. Kaynaklar **Remediate'yi**tıklatarak filtrelendikten sonra düzeltme görevini başlatın. İlke uyumluluk sayfası, görevlerin ilerlemesinin durumunu göstermek için **Düzeltme görevleri** sekmesine açılır. Düzeltme görevi tarafından oluşturulan dağıtımlar hemen başlar.

   ![Düzeltici - düzeltme görevlerinin ilerlemesi](../media/remediate-resources/task-progress.png)

1. İlerleme hakkında ayrıntılı bilgi almak için ilke uyumluluğu sayfasından **düzeltme görevine** tıklayın. Görev için kullanılan filtreleme, düzeltilen kaynakların listesiyle birlikte gösterilir.

1. Düzeltme **görev** sayfasından, düzeltme görevinin dağıtımını veya kaynağı görüntülemek için kaynağa sağ tıklayın. Satırın sonunda, hata iletisi gibi ayrıntıları görmek için **İlgili olayları** tıklatın.

   ![Düzeltici - kaynak görev bağlamı menüsü](../media/remediate-resources/resource-task-context-menu.png)

**Düzeltme görevi** aracılığıyla dağıtılan kaynaklar, ilke **uyumluluğu sayfasındaki Dağıtılmış Kaynaklar** sekmesine eklenir.

### <a name="create-a-remediation-task-through-azure-cli"></a>Azure CLI ile düzeltme görevi oluşturma

Azure CLI ile bir **düzeltme görevi** `az policy remediation` oluşturmak için komutları kullanın. Abonelik `{subscriptionId}` kimliğinizle `{myAssignmentId}` ve **deployIfNotExists** ile değiştirin veya ilke atama kimliğini **değiştirin.**

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Create a remediation for a specific assignment
az policy remediation create --name myRemediation --policy-assignment '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Diğer düzeltme komutları ve örnekleri için [az ilkesi düzeltme](/cli/azure/policy/remediation) komutlarına bakın.

### <a name="create-a-remediation-task-through-azure-powershell"></a>Azure PowerShell ile düzeltme görevi oluşturma

Azure PowerShell ile bir **düzeltme görevi** `Start-AzPolicyRemediation` oluşturmak için komutları kullanın. Abonelik `{subscriptionId}` kimliğinizle `{myAssignmentId}` ve **deployIfNotExists** ile değiştirin veya ilke atama kimliğini **değiştirin.**

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Create a remediation for a specific assignment
Start-AzPolicyRemediation -Name 'myRemedation' -PolicyAssignmentId '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Diğer düzeltme cmdlets ve örnekler [için, Az.PolicyInsights](/powershell/module/az.policyinsights/#policy_insights) modülüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](get-compliance-data.md)öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.
