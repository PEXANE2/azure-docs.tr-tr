---
title: 'Öğretici: Uyumluluğu zorlamak için ilkeler oluşturun'
description: Bu öğreticide, standartları uygulamak, maliyetleri denetlemek, güvenliği sağlamak ve kurumsal geniş tasarım ilkelerini uygulamak için ilkeler kullanırsınız.
ms.date: 03/24/2020
ms.topic: tutorial
ms.openlocfilehash: 15a6c3df26938332d42ecbcfff43f958577062c4
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239950"
---
# <a name="tutorial-create-and-manage-policies-to-enforce-compliance"></a>Öğretici: Uyumluluğu uygulamak için ilkeler oluşturma ve yönetme

Kurumsal standartlarınıza ve hizmet düzeyi sözleşmelerinize uyumluluğu korumak için Azure'da ilkelerin nasıl oluşturulduğunu ve yönetildiğini anlamak önemlidir. Bu öğreticide, Azure İlkesi'ni kullanarak kuruluşunuz genelinde ilkeler oluşturma, atama ve yönetmeyle ilgili en yaygın görevlerden bazılarını yerine getirmeyi öğreneceksiniz; örneğin:

> [!div class="checklist"]
> - Gelecekte oluşturacağınız kaynaklarda bir koşulu zorunlu tutmak için ilke atama
> - Birden çok kaynağın uyumluluğunu izlemek için girişim tanımı oluşturma ve atama
> - Uyumlu olmayan veya reddedilen kaynakları çözümleme
> - Kuruluş genelinde yeni bir ilke uygulama

Mevcut kaynaklarınızın geçerli uyumluluk durumunu tanımlamak için bir ilke atamak isterseniz, hızlı başlangıç makalelerinde bunun nasıl yapıldığı açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="assign-a-policy"></a>İlke atama

Azure İlkesi ile uyumluluğu zorlamanın ilk adımı bir ilke tanımı atamaktır. İlke tanımında, bir ilkenin hangi koşullar altında zorlanacağını ve hangi etkinin uygulanacağı tanımlanır. Bu örnekte, ana kaynak grubundan değeri yle birlikte belirtilen etiketi etiketi etiketi eksik olan yeni veya güncelleştirilmiş kaynaklara eklemek için _eksikse kaynak grubundan bir etiketi devral_ adlı yerleşik ilke tanımını atayın.

1. İlkeler atamak için Azure portalına gidin. **Politika'yı**arayın ve seçin.

   :::image type="content" source="../media/create-and-manage/search-policy.png" alt-text="Arama çubuğunda İlke Ara" border="false":::

1. Azure İlkesi sayfasının sol tarafından **Atamalar**'ı seçin. Atama, belirli bir kapsamda gerçekleşmesi için atanmış olan bir ilkedir.

   :::image type="content" source="../media/create-and-manage/select-assignments.png" alt-text="İlke Genel Bakış sayfasından Atamaları Seçin" border="false":::

1. **İlke - Atamalar** sayfasının üst kısmından **İlke Ata**'yı seçin.

   :::image type="content" source="../media/create-and-manage/select-assign-policy.png" alt-text="Atamalar sayfasından ilke tanımı atama" border="false":::

1. **İlke Atla** ve **Temel İlkeler** sekmesinde, elipsleri seçerek ve bir yönetim grubu veya abonelik seçerek **Kapsam'ı** seçin. İsterseniz bir kaynak grubu seçin. Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler.
   Ardından **Kapsam** sayfasının en altında **Seç'i** seçin.

   Bu örnekte **Contoso** aboneliği kullanılır. Sizin aboneliğiniz farklı olacaktır.

1. Kaynaklar **Kapsam**’a göre dışlanabilir. **Dışlamalar**, **Kapsam**’dan bir düzey aşağıda başlatılır. **Dışlamalar** isteğe bağlıdır, bu yüzden şimdilik boş bırakın.

1. **İlke tanımı** üç nokta öğesini seçerek kullanılabilen tanımların listesini açın. **Tür** değeri _Yerleşik_ olan ilke tanımlarını filtreleyerek bunların tümünü görüntüleyebilir ve açıklamalarını okuyabilirsiniz.

1. **Eksikse kaynak grubundan bir etiketi devral'ı**seçin. Hemen bulamıyorsanız, arama kutusuna **bir etiket** yazın ve ardından ENTER tuşuna basın veya arama kutusundan seçim yapın.
   İlke tanımını bulduktan ve seçtikten sonra **Kullanılabilir Tanımlar** sayfasının en altında seç'i **seçin.**

   :::image type="content" source="../media/create-and-manage/select-available-definition.png" alt-text="İlke bulmak için arama filtresini kullanma":::

1. **Atama adı** otomatik olarak seçtiğiniz ilke adıyla doldurulur, ancak bunu değiştirebilirsiniz. Bu örnekte, _eksikse kaynak grubundan bir etiket devral_bırak. İsteğe bağlı bir **Açıklama** da ekleyebilirsiniz. Açıklama, bu ilke atamasıyla ilgili ayrıntıları sağlar.

1. **İlke zorlamayı** _Etkin_olarak bırakın. _Devre Dışı bırakıldığında,_ bu ayar efekti tetiklemeden ilkenin sonucunu sınamasına izin verir. Daha fazla bilgi için [zorlama moduna](../concepts/assignment-structure.md#enforcement-mode)bakın.

1. **Atanan kişi,** oturum açan kişi tarafından otomatik olarak doldurulur. Bu alan isteğe bağlı olduğu için özel değerler girilebilir.

1. Sihirbazın üst kısmındaki **Parametreler** sekmesini seçin.

1. **Etiket Adı**için , _Çevre_girin .

1. Sihirbazın **Remediation** üst kısmındadüzeltme sekmesini seçin.

1. Bir **düzeltme görevi oluşturmayı** işaretlenmeden bırak. Bu kutu, yeni veya güncelleştirilmiş kaynaklara ek olarak varolan kaynakları değiştirmek için bir görev oluşturmanıza olanak sağlar. Daha fazla bilgi için, [düzeltici kaynaklara](../how-to/remediate-resources.md)bakın.

1. Yönetilen **Kimlik Oluşturma,** bu ilke tanımı [değişiklik](../concepts/effects.md#modify) efektini kullandığından otomatik olarak denetlenir. **İzinler,** ilke tanımına göre otomatik olarak _Katılımcı_ olarak ayarlanır. Daha fazla bilgi için [yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md) ve [düzeltme güvenliğinin işleyişi](../how-to/remediate-resources.md#how-remediation-security-works) bölümlerine bakın.

1. Sihirbazın üst kısmında **Gözden Geçir + oluştur** sekmesini seçin.

1. Seçimlerinizi gözden geçirin ve sayfanın altındaki **Oluştur'u** seçin.

## <a name="implement-a-new-custom-policy"></a>Yeni bir özel ilke uygulama

Artık bir yerleşik ilke tanımı atadığınıza göre, Azure İlkesi'yle daha fazlasını da yapabilirsiniz. Ardından, ortamınızda oluşturulan VM'lerin G serisinde olmayacağını doğrulayarak maliyetlerden tasarruf etmek için yeni bir özel ilke oluşturun. Bu yolla, kuruluşunuzdaki kullanıcılar G serisinden bir sanal makine oluşturmayı her denediğinde istekleri reddedilir.

1. Azure İlkesi sayfasının sol tarafında **Yazma** altında **Tanımlar'ı** seçin.

   :::image type="content" source="../media/create-and-manage/definition-under-authoring.png" alt-text="Yazma grubu altında tanım sayfası" border="false":::

1. Sayfanın üst kısmındaki **+ İlke tanımı** seçeneğini belirleyin. Bu düğme **İlke tanımı** sayfasına açılır.

1. Aşağıdaki bilgileri girin:

   - İlke tanımının kayıtlı olduğu yönetim grubu veya abonelik. **Tanım konumu**’ndaki üç noktayı kullanarak seçin.

     > [!NOTE]
     > Bu ilke tanımını birden çok aboneliğe uygulamayı planlıyorsanız, konumun ilkeyi atayacağınız abonelikleri içeren yönetim grubu olması gerekir. Bir girişim tanımı için de aynısı geçerlidir.

   - İlke tanımının adı - _*_G serisinden daha küçük VM SUS'lar gerektirir_
   - İlke tanımının ne yapmak amaçlandığı açıklaması – _Bu ilke tanımı, bu kapsamda oluşturulan tüm VM'lerin maliyeti azaltmak için G serisinden daha küçük SNU'lara sahip olmasını zorunlu kılmıştır._
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

   İlke kuralındaki _alan_ özelliği desteklenen bir değer olmalıdır. [İlke tanımı yapı alanlarında](../concepts/definition-structure.md#fields)değerlerin tam listesi bulunur. Diğer adlara örnek olarak `"Microsoft.Compute/VirtualMachines/Size"` verilebilir.

   Diğer Azure ilkesi örneklerini görüntülemek için bkz. [Azure İlkesi örnekleri](../samples/index.md).

1. **Kaydet'i**seçin.

## <a name="create-a-policy-definition-with-rest-api"></a>REST API ile ilke tanımı oluşturma

Azure İlkesi Tanımları için REST API'si ile bir ilke oluşturabilirsiniz. REST API, ilke tanımlarını oluşturmanıza ve silmenize, ayrıca mevcut tanımlar hakkında bilgi almanıza olanak tanır. İlke tanımı oluşturmak için aşağıdaki örneği kullanın:

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

PowerShell örneğine geçmeden önce Azure PowerShell Az modülünün en son sürümünü yüklediğinizden emin olun.

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

`az policy definition` Komutu ile Azure CLI kullanarak bir ilke tanımı oluşturabilirsiniz. Satır içi kuralla ilke tanımı oluşturmak için aşağıdaki örneği kullanın:

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

Girişim tanımıyla, çeşitli ilke tanımlarını gruplandırıp kapsamlı bir hedefe ulaşabilirsiniz. Bir girişim, atanan ilkelere uygunluk için atama kapsamındaki kaynakları değerlendirir. Girişim tanımları hakkında daha fazla bilgi için bkz. [Azure İlkesine genel bakış](../overview.md).

### <a name="create-an-initiative-definition"></a>Girişim tanımı oluşturma

1. Azure İlkesi sayfasının sol tarafında **Yazma** altında **Tanımlar'ı** seçin.

   :::image type="content" source="../media/create-and-manage/definition-under-authoring.png" alt-text="Tanımlar sayfasından tanım seçin" border="false":::

1. Sayfanın üst kısmından **+ Giriş Tanımı**'nı seçerek **Girişim Tanımı** sayfasını açın.

   :::image type="content" source="../media/create-and-manage/initiative-definition.png" alt-text="İnisiyatif tanım sayfasını gözden geçirin" border="false":::

1. Tanımın depolanacağı bir yönetim grubu veya abonelik seçmek için **Tanım konumu** üç nokta simgesini kullanın. Önceki sayfanın kapsamı tek bir yönetim grubu veya abonelik olduğunda, **Tanım konumu** otomatik olarak doldurulur. Seçildikten sonra **Kullanılabilir Tanımlar** doldurulur.

1. Girişimin **Adını** ve **Açıklamasını** girin.

   Bu örnek, kaynakların güvenli alma yla ilgili ilke tanımlarıyla uyumlu olduğunu doğrular. Girişimi **Güvenliği Sağlama** olarak adlandırın ve şöyle bir açıklama yazın: **Bu girişim kaynakların güvenliğini sağlamakla ilişkili tüm ilke tanımlarını işlemek için oluşturuldu**.

1. **Kategori** için mevcut seçenekler arasından seçim yapın veya yeni bir kategori oluşturun.

1. **Kullanılabilir Tanımlar** (**Girişim tanımı** sayfasının sağ yarısı) listesine göz atın ve bu girişime eklemek istediğiniz ilke tanımlarını seçin. Güvenli **Alma** girişimi için, ilke tanımı bilgilerinin **+** yanındaki ni seçerek veya bir ilke tanımı satırı seçerek aşağıdaki yerleşik ilke tanımlarını ekleyin ve ardından ayrıntılar sayfasında **+ Ekle** seçeneğini ekleyin:

   - İzin verilen konumlar
   - Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme
   - Internet'in sanal makinelerle karşı karşıya olması için Ağ Güvenliği Grubu Kuralları sertleştirilmiş olmalıdır
   - Sanal Makineler için Azure Yedekleme etkinleştirilmelidir
   - Disk şifreleme sanal makinelerde uygulanmalıdır

   Listeden ilke tanımı nı seçtikten sonra, her biri **Kategori'nin**altına eklenir.

   :::image type="content" source="../media/create-and-manage/initiative-definition-2.png" alt-text="İnisiyatif tanım parametrelerini gözden geçirme" border="false":::

1. Girişime eklenen bir ilke tanımının parametreleri varsa, bunlar **Kategori** alanı altındaki alandaki ilke adı altında gösterilir. _Değer_'i, "Değer ata" (bu girişimin tüm atamaları için sabit kodlanmıştır) veya "Girişim Parametresini Kullan" (her girişim ataması sırasında ayarlanır) olarak ayarlayabilirsiniz. 'Değer ayarla' seçilirse, _Değer(ler)_ sağına açılan düşüş, değerin (ler) girilmesine veya seçilmesine izin verir. 'Girişim Parametresini Kullan' seçildiğinde ise girişim ataması sırasında ayarlanan parametreyi tanımlamanıza olanak sağlayan yeni bir **Giriş parametreleri** bölümü görüntülenir. Bu girişim parametresinde izin verilen değerler, girişim ataması sırasında ayarlanabilecek değerleri daha fazla kısıtlayabilir.

   :::image type="content" source="../media/create-and-manage/initiative-definition-3.png" alt-text="Girişim tanım parametrelerini izin verilen değerlerden değiştirme" border="false":::

   > [!NOTE]
   > Bazı `strongType` parametrelerinde değer listesi otomatik olarak belirlenebilir. Böyle durumlarda parametre satırının sağ tarafında üç nokta simgesi görünür. Seçili olarak 'Parametre kapsamı&lt;(&gt;parametre adı )' sayfasını açar. Bu sayfada, değer seçeneklerini sağlamak için kullanılacak aboneliği seçin. Bu parametre kapsamı yalnızca girişim tanımı oluşturma işlemi sırasında kullanılır ve atandığında, ilke değerlendirmesi veya girişim kapsamı üzerinde herhangi bir etkisi olmaz.

   'İzin Verilen konumlar' parametresini 'Doğu ABD 2' olarak ayarlayın ve diğerlerini varsayılan 'AuditifNotExists' olarak bırakın.

1. **Kaydet'i**seçin.

#### <a name="create-a-policy-initiative-definition-with-azure-cli"></a>Azure CLI ile bir ilke girişimi tanımı oluşturma

`az policy set-definition` Komutu kullanarak Azure CLI'yi kullanarak bir ilke girişimi tanımı oluşturabilirsiniz. Varolan bir ilke tanımına sahip bir ilke girişimi tanımı oluşturmak için aşağıdaki örneği kullanın:

```azurecli-interactive
az policy set-definition create -n readOnlyStorage --definitions '[
        {
            "policyDefinitionId": "/subscriptions/mySubId/providers/Microsoft.Authorization/policyDefinitions/storagePolicy",
            "parameters": { "storageSku": { "value": "[parameters(\"requiredSku\")]" } }
        }
    ]' \
    --params '{ "requiredSku": { "type": "String" } }'
```

#### <a name="create-a-policy-initiative-definition-with-azure-powershell"></a>Azure PowerShell ile bir ilke girişimi tanımı oluşturma

`New-AzPolicySetDefinition` Cmdlet ile Azure PowerShell'i kullanarak bir ilke girişimi tanımı oluşturabilirsiniz. Varolan bir ilke tanımına sahip bir ilke girişimi `VMPolicySet.json`tanımı oluşturmak için aşağıdaki ilke girişimi tanım dosyasını şu şekilde kullanın:

```json
[
    {
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
        "parameters": {
            "tagName": {
                "value": "Business Unit"
            },
            "tagValue": {
                "value": "Finance"
            }
        }
    },
    {
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/464dbb85-3d5f-4a1d-bb09-95a9b5dd19cf"
    }
]
```

```azurepowershell-interactive
New-AzPolicySetDefinition -Name 'VMPolicySetDefinition' -Metadata '{"category":"Virtual Machine"}' -PolicyDefinition C:\VMPolicySet.json
```

### <a name="assign-an-initiative-definition"></a>Girişim tanımını atama

1. Azure İlkesi sayfasının sol tarafında **Yazma** altında **Tanımlar'ı** seçin.

1. Daha önce oluşturduğunuz **Güvenliği Sağlama** adlı girişim tanımını bulup seçin. **Güvenli Ol: Girişim** sayfasını atamak için sayfanın üst kısmında **Atla'yı** seçin.

   :::image type="content" source="../media/create-and-manage/assign-definition.png" alt-text="Girişim tanım sayfasından tanım atama" border="false":::

   Ayrıca, seçili satıra sağ tıklayabilir veya bağlamsal bir menü için satırın sonundaki elipsleri seçebilirsiniz. Sonra da **Ata**’yı seçebilirsiniz.

   :::image type="content" source="../media/create-and-manage/select-right-click.png" alt-text="Bir girişim için alternatif seçenekler" border="false":::

1. Aşağıdaki örnek bilgileri girerek **Güvenliği Sağlama: Girişimi Ata** sayfasını doldurun. Kendi bilgilerinizi de kullanabilirsiniz.

   - Kapsam: Girişimi kaydettiğiniz yönetim grubu veya abonelik, varsayılan kapsam olur.
     Kapsamı değiştirerek girişimi kayıt konumundaki bir aboneliğe veya kaynak grubuna atayabilirsiniz.
   - Dışlamalar: Kapsam dahilinde yer alan ancak girişim atamasının uygulanmasını önlemek istediğiniz kaynaklar varsa bunları yapılandırın.
   - Girişim tanımı ve Atama adı: Güvenliği Sağlama (atanan girişimin adı olarak önceden oluşturulur).
   - Açıklama: Bu girişim ataması, bu ilke tanımları grubunu zorunlu tutmak için uyarlanmıştır.
   - İlke zorlama: Varsayılan _Etkin_olarak bırakın.
   - Atayan: Oturum açmış kişiye göre otomatik olarak doldurulur. Bu alan isteğe bağlı olduğu için özel değerler girilebilir.

1. Sihirbazın üst kısmındaki **Parametreler** sekmesini seçin. Önceki adımlarda bir girişim parametresi yapılandıysanız, burada bir değer ayarlayın.

1. Sihirbazın **Remediation** üst kısmındadüzeltme sekmesini seçin. **Yönetilen Kimlik Oluşturun** seçeneğini işaretsiz bırakın. Atanmakta olan ilke veya girişim [deployIfNotExists](../concepts/effects.md#deployifnotexists) veya [değişiklik](../concepts/effects.md#modify) efektleri ile bir ilke içerdiğinde bu kutu _işaretlenmelidir._ Bu öğretici için kullanılan ilke olmadığı için, boş bırakın. Daha fazla bilgi için [yönetilen kimlikler](../../../active-directory/managed-identities-azure-resources/overview.md) ve [düzeltme güvenliğinin işleyişi](../how-to/remediate-resources.md#how-remediation-security-works) bölümlerine bakın.

1. Sihirbazın üst kısmında **Gözden Geçir + oluştur** sekmesini seçin.

1. Seçimlerinizi gözden geçirin ve sayfanın altındaki **Oluştur'u** seçin.

## <a name="check-initial-compliance"></a>İlk uyumluluğu denetleme

1. Azure İlkesi sayfasının sol tarafından **Uyumluluklar**'ı seçin.

1. Güvenli **Ol** girişimini bulun. Büyük olasılıkla hala **Başlatılmayan** _Uyumluluk durumundadır._
   Atamanın ilerlemesi hakkında ayrıntılı bilgi almak için girişimi seçin.

   :::image type="content" source="../media/create-and-manage/compliance-status-not-started.png" alt-text="Girişim uyum sayfası - değerlendirmeler başlamadı" border="false":::

1. Girişim ataması tamamlandıktan sonra, uyumluluk sayfası güncelleştirilerek _Uyumluluk durumu_ değeri **Uyumlu** olur.

   :::image type="content" source="../media/create-and-manage/compliance-status-compliant.png" alt-text="Girişim uyumluluk sayfası- kaynaklar uyumlu" border="false":::

1. Girişim uyumluluk sayfasındaki herhangi bir ilkenin seçilmesi, bu ilkeiçin uyumluluk ayrıntıları sayfasını açar. Bu sayfada uyumluluk için kaynak düzeyinde ayrıntılar sağlanır.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Dışlama'yı kullanarak uyumlu olmayan veya reddedilen kaynağı hariç tutma

Belirli bir konumu gerektirecek bir ilke girişimi atadıktan sonra, farklı bir konumda oluşturulan tüm kaynaklar reddedilir. Bu bölümde, tek bir kaynak grubunda dışlama oluşturarak kaynak oluşturmak için reddedilen bir isteği çözme de yürürsiniz. Dışlama, bu kaynak grubundaki ilkenin (veya girişimin) uygulanmasını engeller. Aşağıdaki örnekte, dışlanan kaynak grubunda herhangi bir konuma izin verilir. Dışlama, bir abonelik, kaynak grubu veya tek bir kaynak için geçerli olabilir.

Atanmış bir ilke veya girişim tarafından engellenen dağıtımlar, dağıtım tarafından hedeflenen kaynak grubunda görüntülenebilir: Sayfanın sol tarafında **ki Dağıtımları** seçin ve başarısız dağıtımın **Dağıtım Adını** seçin. Reddedilen kaynak, _Yasaklandı_ durum bilgisiyle listelenir. Kaynağı reddeden ilke veya girişimi ve atamayı belirlemek için **Başarısız'ı seçin. **Dağıtıma Genel Bakış sayfasındaki ayrıntılar ->için buraya tıklayın. Sayfanın sağ tarafında hata bilgilerini içeren bir pencere açılır. **Hata Ayrıntıları** altında ilgili ilke nesnelerin GUIDs vardır.

:::image type="content" source="../media/create-and-manage/rg-deployment-denied.png" alt-text="Dağıtım ilke ataması tarafından reddedildi" border="false":::

Azure İlkesi sayfasında: Sayfanın sol tarafında **Uyumluluk'u** seçin ve **Güvenli Olsun** ilkesi girişimini seçin. Bu sayfada, engellenen kaynaklar için **Reddet** sayısında bir artış vardır. **Olaylar** sekmesinin altında, ilke tanımı tarafından reddedilen kaynağı kim ler oluşturmaya veya dağıtmaya çalıştığına ilişkin ayrıntılar yer alıyor.

:::image type="content" source="../media/create-and-manage/compliance-overview.png" alt-text="Atanan bir ilkenin uyumluluğuna genel bakış" border="false":::

Bu örnekte, Contoso'nun Sr. Sanallaştırma uzmanlarından biri olan Trent Baker gerekli işi yapıyordu. Trent'e bir istisna için yer vermeliyiz. Yeni bir kaynak grubu oluşturdu, **LocationsExcluded**ve sonraki bu ilke atama için bir özel durum verir.

### <a name="update-assignment-with-exclusion"></a>Atamayı özel durumla güncelleştirme

1. Azure İlkesi sayfasının sol tarafında **Yazma** altında **Atamalar'ı** seçin.

1. Tüm ilke atamalarına göz atın ve _Güvenli Olsun_ ilke atamasını açın.

1. Elipsleri seçerek ve dışlanması gereken kaynak grubunu seçerek **Dışlama'yı** ayarlayın, Bu örnekte _Dışlanmış Yerler._ **Seçili Kapsama Ekle'yi** seçin ve ardından **Kaydet'i**seçin.

   :::image type="content" source="../media/create-and-manage/request-exclusion.png" alt-text="İlke atamasına dışlanmış bir kaynak grubu ekleme" border="false":::

   > [!NOTE]
   > İlke tanımına ve etkisine bağlı olarak, dışlama atama kapsamındaki bir kaynak grubu içindeki belirli kaynaklara da verilebilir. Bu öğreticide Bir **İnkar** efekti kullanıldığından, dışlamayı zaten var olan belirli bir kaynağa ayarlamak mantıklı olmaz.

1. **Gözden Geçir + kaydet'i** seçin ve ardından **Kaydet'i**seçin.

Bu bölümde, reddedilen isteği tek bir kaynak grubunda dışlama oluşturarak çözümlediniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticideki kaynaklarla çalışmayı bitirdiyseniz, yukarıda oluşturulan ilke atamalarından veya tanımlarından herhangi birini silmek için aşağıdaki adımları kullanın:

1. Azure İlkesi sayfasının sol tarafında **Yazma** altında **Tanımlar** 'ı (veya bir atamayı silmeye çalışıyorsanız **Atamalar)** seçin.

1. Kaldırmak istediğiniz yeni girişim veya tanımını (ya da atamayı) arayın.

1. Satıra sağ tıklayın ya da tanımın (veya atamanın) sonundaki üç noktayı seçip **Tanımı sil** (veya **Atamayı sil**) öğesini seçin.

## <a name="review"></a>Gözden geçirme

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