---
title: 'Tutorial: Build policies to enforce compliance'
description: In this tutorial, you use policies to enforce standards, control costs, maintain security, and impose enterprise wide design principles.
ms.date: 11/25/2019
ms.topic: tutorial
ms.openlocfilehash: 75a1d892a88f2b5bbdbec2a1b8d525245bb1e86f
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74482328"
---
# <a name="tutorial-create-and-manage-policies-to-enforce-compliance"></a>Tutorial: Create and manage policies to enforce compliance

Kurumsal standartlarınıza ve hizmet düzeyi sözleşmelerinize uyumluluğu korumak için Azure'da ilkelerin nasıl oluşturulduğunu ve yönetildiğini anlamak önemlidir. Bu öğreticide, Azure İlkesi'ni kullanarak kuruluşunuz genelinde ilkeler oluşturma, atama ve yönetmeyle ilgili en yaygın görevlerden bazılarını yerine getirmeyi öğreneceksiniz; örneğin:

> [!div class="checklist"]
> - Gelecekte oluşturacağınız kaynaklarda bir koşulu zorunlu tutmak için ilke atama
> - Birden çok kaynağın uyumluluğunu izlemek için girişim tanımı oluşturma ve atama
> - Uyumlu olmayan veya reddedilen kaynakları çözümleme
> - Kuruluş genelinde yeni bir ilke uygulama

Mevcut kaynaklarınızın geçerli uyumluluk durumunu tanımlamak için bir ilke atamak isterseniz, hızlı başlangıç makalelerinde bunun nasıl yapıldığı açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="assign-a-policy"></a>İlke atama

Azure İlkesi ile uyumluluğu zorlamanın ilk adımı bir ilke tanımı atamaktır. İlke tanımında, bir ilkenin hangi koşullar altında zorlanacağını ve hangi etkinin uygulanacağı tanımlanır. Bu örnekte, uyumluluk için tüm SQL Server veritabanlarının v12.0 sürümünde olması koşulunu zorunlu tutan *SQL Server sürüm 12.0 gerektir* adlı yerleşik bir ilke tanımı atayacaksınız.

1. Go to the Azure portal to assign policies. Search for and select **Policy**.

   ![Search for Policy in the search bar](../media/create-and-manage/search-policy.png)

1. Azure İlkesi sayfasının sol tarafından **Atamalar**'ı seçin. Atama, belirli bir kapsamda gerçekleşmesi için atanmış olan bir ilkedir.

   ![Select Assignments from Policy Overview page](../media/create-and-manage/select-assignments.png)

1. **İlke - Atamalar** sayfasının üst kısmından **İlke Ata**'yı seçin.

   ![Assign a policy definition from Assignments page](../media/create-and-manage/select-assign-policy.png)

1. On the **Assign Policy** page, select the **Scope** by selecting the ellipsis and selecting either a management group or subscription. İsterseniz bir kaynak grubu seçin. Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Then select **Select** at the bottom of the **Scope** page.

   Bu örnekte **Contoso** aboneliği kullanılır. Sizin aboneliğiniz farklı olacaktır.

1. Kaynaklar **Kapsam**’a göre dışlanabilir. **Dışlamalar**, **Kapsam**’dan bir düzey aşağıda başlatılır. **Dışlamalar** isteğe bağlıdır, bu yüzden şimdilik boş bırakın.

1. **İlke tanımı** üç nokta öğesini seçerek kullanılabilen tanımların listesini açın. **Tür** değeri *Yerleşik* olan ilke tanımlarını filtreleyerek bunların tümünü görüntüleyebilir ve açıklamalarını okuyabilirsiniz.

1. **SQL Server sürüm 12.0 gerektir**'i seçin. If you can't find it right away, type **require sql server** into the search box and then press ENTER or select out of the search box. Select **Select** at the bottom of the **Available Definitions** page once you have found and selected the policy definition.

   ![Use search filter to locate a policy](../media/create-and-manage/select-available-definition.png)

1. **Atama adı** otomatik olarak seçtiğiniz ilke adıyla doldurulur, ancak bunu değiştirebilirsiniz. Bu örnekte, *SQL Server sürüm 12.0 gerektir* ayarını değiştirmeyin. İsteğe bağlı bir **Açıklama** da ekleyebilirsiniz. Açıklama, bu ilke atamasıyla ilgili ayrıntıları sağlar.
   **Assigned by** is automatically filled based on who is logged in. Bu alan isteğe bağlı olduğu için özel değerler girilebilir.

1. **Yönetilen Kimlik Oluşturun** seçeneğini işaretsiz bırakın. This box _must_ be checked when the policy or initiative being assigned includes a policy with the [deployIfNotExists](../concepts/effects.md#deployifnotexists) effect. As the policy used for this tutorial doesn't, leave it blank. Daha fazla bilgi için [yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md) ve [düzeltme güvenliğinin işleyişi](../how-to/remediate-resources.md#how-remediation-security-works) bölümlerine bakın.

1. **Ata**'yı seçin.

## <a name="implement-a-new-custom-policy"></a>Yeni bir özel ilke uygulama

Artık bir yerleşik ilke tanımı atadığınıza göre, Azure İlkesi'yle daha fazlasını da yapabilirsiniz. Next, create a new custom policy to save costs by validating that VMs created in your environment can't be in the G series. Bu yolla, kuruluşunuzdaki kullanıcılar G serisinden bir sanal makine oluşturmayı her denediğinde istekleri reddedilir.

1. Azure İlkesi sayfasının sol tarafındaki **Yazma** bölümünden **Tanımlar**’ı seçin.

   ![Definition page under Authoring group](../media/create-and-manage/definition-under-authoring.png)

1. Sayfanın üst kısmındaki **+ İlke tanımı** seçeneğini belirleyin. This button opens to the **Policy definition** page.

1. Aşağıdaki bilgileri girin:

   - İlke tanımının kayıtlı olduğu yönetim grubu veya abonelik. **Tanım konumu**’ndaki üç noktayı kullanarak seçin.

     > [!NOTE]
     > Bu ilke tanımını birden çok aboneliğe uygulamayı planlıyorsanız, konumun ilkeyi atayacağınız abonelikleri içeren yönetim grubu olması gerekir. Bir girişim tanımı için de aynısı geçerlidir.

   - İlke tanımının adı - *Sanal makine SKU'larının G serisinden küçük olmasını gerektir*
   - İlke tanımının yapması beklenen işlemin açıklaması – *Bu ilke tanımı, maliyeti düşürmek için bu kapsamda oluşturulan tüm sanal makinelerin SKU'larının G serisinden küçük olmasını zorunlu tutar.*
   - Mevcut seçenekler arasından seçim yapın (_İşlem_ gibi) veya bu ilke tanımı için yeni bir kategori oluşturun.
   - Aşağıdaki JSON kodunu kopyalayın ve ardından gereksinimlerinize uygun olarak aşağıdaki öğelerle güncelleştirin:
      - İlke parametreleri.
      - İlke kuralları/koşulları; bu örnekte, G serisine eşit VM SKU boyutu
      - İlkenin etkisi, bu örnekte **Reddet**.

   JSON aşağıdakine benzer olmalıdır. Düzeltilmiş kodunuzu Azure Portal'a yapıştırın.

   ```json
   {
       "policyRule": {
           "if": {
               "allOf": [{
                       "field": "type",
                       "equals": "Microsoft.Compute/virtualMachines"
                   },
                   {
                       "field": "Microsoft.Compute/virtualMachines/sku.name",
                       "like": "Standard_G*"
                   }
               ]
           },
           "then": {
               "effect": "deny"
           }
       }
   }
   ```

   The *field* property in the policy rule must be one of the following values: Name, Type, Location, Tags, or an alias. Diğer adlara örnek olarak `"Microsoft.Compute/VirtualMachines/Size"` verilebilir.

   Diğer Azure ilkesi örneklerini görüntülemek için bkz. [Azure İlkesi örnekleri](../samples/index.md).

1. **Kaydet**’i seçin.

## <a name="create-a-policy-definition-with-rest-api"></a>REST API ile ilke tanımı oluşturma

You can create a policy with the REST API for Azure Policy Definitions. REST API, ilke tanımlarını oluşturmanıza ve silmenize, ayrıca mevcut tanımlar hakkında bilgi almanıza olanak tanır. İlke tanımı oluşturmak için aşağıdaki örneği kullanın:

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Aşağıdaki örnekte gösterilene benzer bir istek gövdesi ekleyin:

```json
{
    "properties": {
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                }
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>PowerShell ile ilke tanımı oluşturma

Before proceeding with the PowerShell example, make sure you've installed the latest version of the Azure PowerShell Az module.

`New-AzPolicyDefinition` cmdlet'ini kullanarak ilke tanımı oluşturabilirsiniz.

Bir dosyadan ilke tanımı oluşturmak için, dosyanın yolunu geçirin. Dış dosya için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
    -Name 'denyCoolTiering' `
    -DisplayName 'Deny cool access tiering for storage' `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Yerel dosya kullanımı için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
    -Name 'denyCoolTiering' `
    -Description 'Deny cool access tiering for storage' `
    -Policy 'c:\policies\coolAccessTier.json'
```

Satır içi kuralla ilke tanımı oluşturmak için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$definition = New-AzPolicyDefinition -Name 'denyCoolTiering' -Description 'Deny cool access tiering for storage' -Policy '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/accessTier",
                "equals": "cool"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

Çıkış bir `$definition` nesnesinde depolanır ve bu nesne ilke ataması sırasında kullanılır. Aşağıdaki örnekte, parametreler içeren bir ilke tanımı oluşturulur:

```azurepowershell-interactive
$policy = '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of locations that can be specified when deploying storage accounts.",
            "strongType": "location",
            "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzPolicyDefinition -Name 'storageLocations' -Description 'Policy to specify locations for storage accounts.' -Policy $policy -Parameter $parameters
```

### <a name="view-policy-definitions-with-powershell"></a>İlke tanımlarını PowerShell ile görüntüleme

Aboneliğinizdeki tüm ilke tanımlarını görmek için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzPolicyDefinition
```

Yerleşik ilkeler de dahil olmak üzere tüm kullanılabilir ilke tanımlarını döndürür. Her ilke şu biçimde döndürülür:

```output
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>Azure CLI ile ilke tanımı oluşturma

İlke tanımı komutuyla Azure CLI'yi kullanarak ilke tanımı oluşturabilirsiniz. Satır içi kuralla ilke tanımı oluşturmak için aşağıdaki örneği kullanın:

```azurecli-interactive
az policy definition create --name 'denyCoolTiering' --description 'Deny cool access tiering for storage' --rules '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/accessTier",
                "equals": "cool"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

### <a name="view-policy-definitions-with-azure-cli"></a>İlke tanımlarını Azure CLI ile görüntüleme

Aboneliğinizdeki tüm ilke tanımlarını görmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az policy definition list
```

Yerleşik ilkeler de dahil olmak üzere tüm kullanılabilir ilke tanımlarını döndürür. Her ilke şu biçimde döndürülür:

```json
{
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",
    "displayName": "Allowed locations",
    "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "policyRule": {
        "if": {
            "not": {
                "field": "location",
                "in": "[parameters('listOfAllowedLocations')]"
            }
        },
        "then": {
            "effect": "Deny"
        }
    },
    "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>Girişim tanımı oluşturma ve atama

Girişim tanımıyla, çeşitli ilke tanımlarını gruplandırıp kapsamlı bir hedefe ulaşabilirsiniz. An initiative evaluates resources within scope of the assignment for compliance to the included policies. Girişim tanımları hakkında daha fazla bilgi için bkz. [Azure İlkesine genel bakış](../overview.md).

### <a name="create-an-initiative-definition"></a>Girişim tanımı oluşturma

1. Azure İlkesi sayfasının sol tarafındaki **Yazma** bölümünden **Tanımlar**’ı seçin.

   ![Select definition from the Definitions page](../media/create-and-manage/definition-under-authoring.png)

1. Sayfanın üst kısmından **+ Giriş Tanımı**'nı seçerek **Girişim Tanımı** sayfasını açın.

   ![Review initiative definition page](../media/create-and-manage/initiative-definition.png)

1. Tanımın depolanacağı bir yönetim grubu veya abonelik seçmek için **Tanım konumu** üç nokta simgesini kullanın. Önceki sayfanın kapsamı tek bir yönetim grubu veya abonelik olduğunda, **Tanım konumu** otomatik olarak doldurulur.

1. Girişimin **Adını** ve **Açıklamasını** girin.

   This example validates that resources are in compliance with policy definitions about getting secure. Girişimi **Güvenliği Sağlama** olarak adlandırın ve şöyle bir açıklama yazın: **Bu girişim kaynakların güvenliğini sağlamakla ilişkili tüm ilke tanımlarını işlemek için oluşturuldu**.

1. **Kategori** için mevcut seçenekler arasından seçim yapın veya yeni bir kategori oluşturun.

1. **Kullanılabilir Tanımlar** (**Girişim tanımı** sayfasının sağ yarısı) listesine göz atın ve bu girişime eklemek istediğiniz ilke tanımlarını seçin. For the **Get secure** initiative, add the following built-in policy definitions by selecting the **+** next to the policy definition information or selecting a policy definition row and then the **+ Add** option in the details page:

   - SQL Server sürüm 12.0 gerektir
   - [Preview]: Monitor unprotected web applications in Security Center.
   - [Preview]: Monitor permissive network across in Security Center.
   - [Preview]: Monitor possible app Whitelisting in Security Center.
   - [Preview]: Monitor unencrypted VM Disks in Security Center.

   After selecting the policy definition from the list, it's added under **Policies and Parameters**.

   ![Review initiative definition parameters](../media/create-and-manage/initiative-definition-2.png)

1. If a policy definition being added to the initiative has parameters, they're shown under the policy name in the **Policies and Parameters** area. _Değer_'i, "Değer ata" (bu girişimin tüm atamaları için sabit kodlanmıştır) veya "Girişim Parametresini Kullan" (her girişim ataması sırasında ayarlanır) olarak ayarlayabilirsiniz. If 'Set value' is selected, the drop-down to the right of _Values_ allows entering or selecting the value(s). 'Girişim Parametresini Kullan' seçildiğinde ise girişim ataması sırasında ayarlanan parametreyi tanımlamanıza olanak sağlayan yeni bir **Giriş parametreleri** bölümü görüntülenir. Bu girişim parametresinde izin verilen değerler, girişim ataması sırasında ayarlanabilecek değerleri daha fazla kısıtlayabilir.

   ![Change initiative definition parameters from allowed values](../media/create-and-manage/initiative-definition-3.png)

   > [!NOTE]
   > Bazı `strongType` parametrelerinde değer listesi otomatik olarak belirlenebilir. Böyle durumlarda parametre satırının sağ tarafında üç nokta simgesi görünür. Selecting it opens the 'Parameter scope (&lt;parameter name&gt;)' page. Bu sayfada, değer seçeneklerini sağlamak için kullanılacak aboneliği seçin. Bu parametre kapsamı yalnızca girişim tanımı oluşturma işlemi sırasında kullanılır ve atandığında, ilke değerlendirmesi veya girişim kapsamı üzerinde herhangi bir etkisi olmaz.

1. **Kaydet**’i seçin.

### <a name="assign-an-initiative-definition"></a>Girişim tanımını atama

1. Azure İlkesi sayfasının sol tarafındaki **Yazma** bölümünden **Tanımlar**’ı seçin.

1. Daha önce oluşturduğunuz **Güvenliği Sağlama** adlı girişim tanımını bulup seçin. Sayfanın üst kısmından **Ata**’yı seçerek **Güvenliği Sağlama: Girişimi atama** sayfasını açın.

   ![Assign a definition from Initiative definition page](../media/create-and-manage/assign-definition.png)

   You can also right-click on the selected row or select the ellipsis at the end of the row for a contextual menu. Sonra da **Ata**’yı seçebilirsiniz.

   ![Alternative options for an initiative](../media/create-and-manage/select-right-click.png)

1. Aşağıdaki örnek bilgileri girerek **Güvenliği Sağlama: Girişimi Ata** sayfasını doldurun. Kendi bilgilerinizi de kullanabilirsiniz.

   - Kapsam: Girişimi kaydettiğiniz yönetim grubu veya abonelik, varsayılan kapsam olur.
     Kapsamı değiştirerek girişimi kayıt konumundaki bir aboneliğe veya kaynak grubuna atayabilirsiniz.
   - Dışlamalar: Kapsam dahilinde yer alan ancak girişim atamasının uygulanmasını önlemek istediğiniz kaynaklar varsa bunları yapılandırın.
   - Girişim tanımı ve Atama adı: Güvenliği Sağlama (atanan girişimin adı olarak önceden oluşturulur).
   - Açıklama: Bu girişim ataması, bu ilke tanımları grubunu zorunlu tutmak için uyarlanmıştır.
   - Atayan: Oturum açmış kişiye göre otomatik olarak doldurulur. Bu alan isteğe bağlı olduğu için özel değerler girilebilir.

1. **Yönetilen Kimlik Oluşturun** seçeneğini işaretsiz bırakın. This box _must_ be checked when the policy or initiative being assigned includes a policy with the [deployIfNotExists](../concepts/effects.md#deployifnotexists) effect. As the policy used for this tutorial doesn't, leave it blank. Daha fazla bilgi için [yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md) ve [düzeltme güvenliğinin işleyişi](../how-to/remediate-resources.md#how-remediation-security-works) bölümlerine bakın.

1. **Ata**'yı seçin.

## <a name="check-initial-compliance"></a>İlk uyumluluğu denetleme

1. Azure İlkesi sayfasının sol tarafından **Uyumluluklar**'ı seçin.

1. Locate the **Get Secure** initiative. It's likely still in _Compliance state_ of **Not started**.
   Select the initiative to get full details on the progress of the assignment.

   ![Initiative compliance page - evaluations not started](../media/create-and-manage/compliance-status-not-started.png)

1. Girişim ataması tamamlandıktan sonra, uyumluluk sayfası güncelleştirilerek _Uyumluluk durumu_ değeri **Uyumlu** olur.

   ![Initiative compliance page- resources compliant](../media/create-and-manage/compliance-status-compliant.png)

1. Selecting on any policy on the initiative compliance page opens the compliance details page for the policy. Bu sayfada uyumluluk için kaynak düzeyinde ayrıntılar sağlanır.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Dışlama'yı kullanarak uyumlu olmayan veya reddedilen kaynağı hariç tutma

Yukarıdaki örneği izleyerek, SQL Server sürüm 12.0'ı gerektirecek bir ilke tanımı atadıktan sonra 12.0’dan farklı bir sürümde oluşturulan SQL Server reddedilebilir. In this section, you walk through resolving a denied request to create a SQL server by creating an exclusion on a single resource group. Dışlama, ilkenin (veya girişimin) ilgili kaynağa uygulanmasını engeller.
Aşağıdaki örnekte, tek bir kaynak grubunda tüm SQL Server sürümlerine izin verilir. Aboneliğe, kaynak grubuna dışlama uygulanabilir veya dışlamayı tek tek kaynakları içerecek şekilde daraltabilirsiniz.

A deployment prevented by an assigned policy or initiative can be viewed in two locations:

- On the resource group targeted by the deployment: Select **Deployments** in the left side of the page, then select the **Deployment Name** of the failed deployment. Reddedilen kaynak, _Yasaklandı_ durum bilgisiyle listelenir. To determine the policy or initiative and assignment that denied the resource, select **Failed. Click here for details ->** on the Deployment Overview page.
  Sayfanın sağ tarafında hata bilgilerini içeren bir pencere açılır. Under **Error Details** are the GUIDs of the related policy objects.

  ![Dağıtım ilke ataması tarafından reddedildi](../media/create-and-manage/rg-deployment-denied.png)

- On the Azure Policy page: Select **Compliance** in the left side of the page and select the **Require SQL Server version 12.0** policy. Açılan sayfada **Ret** sayısında bir artış görürsünüz. Under the **Events** tab, you would also see who tried the deployment that was denied by the policy.

  ![Atanan bir ilkenin uyumluluğuna genel bakış](../media/create-and-manage/compliance-overview.png)

In this example, Trent Baker, one of Contoso's Sr. Virtualization specialists, was doing required work. We need to grant Trent an exception, but we don't want the non-version 12.0 SQL servers in just any resource group. **SQLServers_Excluded** adlı yeni bir kaynak grubu oluşturduk ve bu gruba bu ilke ataması için özel durum izni vereceğiz.

### <a name="update-assignment-with-exclusion"></a>Atamayı özel durumla güncelleştirme

1. Azure İlkesi sayfasının sol tarafındaki **Yazma** bölümünden **Atamalar**’ı seçin.

1. Tüm ilke atamalarına göz atın ve *SQL Server sürüm 12.0 gerektir* atamasını açın.

1. Set the **Exclusion** by selecting the ellipsis and selecting the resource group to exclude, *SQLServers_Excluded* in this example.

   ![Add an excluded resource group to the policy assignment](../media/create-and-manage/request-exclusion.png)

   > [!NOTE]
   > İlkeye ve sahip olduğu etkiye bağlı olarak atama kapsamı içindeki bir kaynak grubunda yer alan belirli kaynaklara da dışlama izni verilebilir. Bu öğreticide bir **Ret** etkisi kullanıldığından, zaten mevcut olan belirli bir kaynakta dışlama ayarlanması mantıklı değildir.

1. Select **Select** and then select **Save**.

In this section, you resolved the denied request by creating an exclusion on a single resource group.

## <a name="clean-up-resources"></a>Kaynakları temizleme

If you're done working with resources from this tutorial, use the following steps to delete any of the assignments or definitions created above:

1. Select **Definitions** (or **Assignments** if you're trying to delete an assignment) under **Authoring** in the left side of the Azure Policy page.

1. Kaldırmak istediğiniz yeni girişim veya tanımını (ya da atamayı) arayın.

1. Satıra sağ tıklayın ya da tanımın (veya atamanın) sonundaki üç noktayı seçip **Tanımı sil** (veya **Atamayı sil**) öğesini seçin.

## <a name="review"></a>Gözden Geçir

Bu öğreticide, aşağıdaki görevleri başarıyla gerçekleştirdiniz:

> [!div class="checklist"]
> - Gelecekte oluşturacağınız kaynaklarda bir koşulu zorunlu tutmak için ilke atandı
> - Birden çok kaynağın uyumluluğunu izlemek için girişim tanımı oluşturuldu ve atandı
> - Uyumlu olmayan veya reddedilen kaynak sorunu çözüldü
> - Kuruluş genelinde yeni bir ilke uygulandı

## <a name="next-steps"></a>Sonraki adımlar

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için şu makaleyi gözden geçirin:

> [!div class="nextstepaction"]
> [Azure İlkesi tanım yapısı](../concepts/definition-structure.md)