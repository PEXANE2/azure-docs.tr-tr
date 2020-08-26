---
title: Bir kural hiyerarşisi tanımlamak için Azure Güvenlik Duvarı ilkesini kullanma
description: Bir kural hiyerarşisi tanımlamak ve uyumluluğu zorlamak için Azure Güvenlik duvarı ilkesi 'ni nasıl kullanacağınızı öğrenin.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: how-to
ms.date: 08/26/2020
ms.author: victorh
ms.openlocfilehash: c290904c9f4bc7dba70dad9351dc45b676e0c236
ms.sourcegitcommit: c6b9a46404120ae44c9f3468df14403bcd6686c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88893782"
---
# <a name="use-azure-firewall-policy-to-define-a-rule-hierarchy"></a>Bir kural hiyerarşisi tanımlamak için Azure Güvenlik Duvarı ilkesini kullanma

Güvenlik yöneticilerinin güvenlik duvarlarını yönetmesi ve şirket içi ve bulut dağıtımları arasında uyumluluk sağlamak gerekir. Anahtar bileşen, uygulama ekiplerini otomatik bir şekilde güvenlik duvarı kuralları oluşturmak için CI/CD işlem hatları uygulamak için esneklik sağlama olanağıdır.

Azure Güvenlik duvarı ilkesi, bir kural hiyerarşisi tanımlamanızı ve uyumluluğu zorlamanıza izin verir:

- Bir alt uygulama takım ilkesinin üst kısmında merkezi bir temel ilkenin kaplaması için hiyerarşik bir yapı sağlar. Temel ilkenin önceliği daha yüksektir ve alt ilkeden önce çalışır.
- Yanlışlıkla temel ilke kaldırmayı engellemek için özel bir rol tabanlı erişim denetimi (RBAC) tanımı kullanın ve bir abonelik veya kaynak grubu içindeki kural koleksiyonu gruplarına seçmeli erişim sağlayın. 

## <a name="solution-overview"></a>Çözüme genel bakış

Bu örnek için üst düzey adımlar şunlardır:

1. Güvenlik ekibi kaynak grubunda bir temel güvenlik duvarı ilkesi oluşturun. 
3. Temel ilkede BT güvenliğine özgü kuralları tanımlayın. Bu, trafiğe izin verme/reddetme için ortak bir kurallar kümesi ekler.
4. Temel ilkeyi devralacak uygulama takım ilkeleri oluşturun. 
5. İlkede uygulamaya özgü kuralları tanımlayın. Ayrıca, kuralları önceden varolan güvenlik duvarlarından geçirebilirsiniz.
6. Kural koleksiyonu grubuna ayrıntılı erişim sağlamak ve güvenlik duvarı Ilke kapsamındaki roller eklemek için Azure Active Directory özel roller oluşturun. Aşağıdaki örnekte, Sales ekibi üyeleri, Sales ekipleri güvenlik duvarı Ilkesi için kural koleksiyonu gruplarını düzenleyebilir. Aynı veritabanı ve mühendislik takımları için de geçerlidir.
7. İlkeyi ilgili güvenlik duvarıyla ilişkilendirin. Azure Güvenlik duvarında yalnızca bir atanmış ilke olabilir. Bu, her bir uygulama takımının kendi güvenlik duvarı olmasını gerektirir.



:::image type="content" source="media/rule-hierarchy/contoso-teams.png" alt-text="Takımlar ve gereksinimler" border="false":::

### <a name="create-the-firewall-policies"></a>Güvenlik Duvarı ilkelerini oluşturma

- Temel güvenlik duvarı ilkesi.

Uygulama takımlarının her biri için ilkeler oluşturun:

- Bir satış güvenlik duvarı ilkesi. Satış güvenlik duvarı ilkesi, temel güvenlik duvarı ilkesini devralır.
- Bir veritabanı güvenlik duvarı ilkesi. Veritabanı güvenlik duvarı ilkesi temel güvenlik duvarı ilkesini devralır.
- Mühendislik güvenlik duvarı ilkesi. Mühendislik güvenlik duvarı ilkesi de temel güvenlik duvarı ilkesini devralır.

:::image type="content" source="media/rule-hierarchy/policy-hierarchy.png" alt-text="İlke hiyerarşisi" border="false":::

### <a name="create-custom-roles-to-access-the-rule-collection-groups"></a>Kural koleksiyonu gruplarına erişmek için özel roller oluşturma 

Özel roller her bir uygulama takımı için tanımlanır. Rol, işlemleri ve kapsamı tanımlar. Uygulama ekiplerinin ilgili uygulamaları için kural koleksiyonu gruplarını düzenlemesine izin verilir.

Özel rolleri tanımlamak için aşağıdaki üst düzey prosedürü kullanın:

1. Aboneliği al:

   `Select-AzSubscription -SubscriptionId xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx`
2. Aşağıdaki komutu çalıştırın:

   `Get-AzProviderOperation "Microsoft.Support/*" | FT Operation, Description -AutoSize`
3. Reader rolünü JSON biçiminde çıkarmak için Get-AzRoleDefinition komutunu kullanın. 

   `Get-AzRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\CustomRoles\ReaderSupportRole.json`
4. Dosyada ReaderSupportRole.jsbir düzenleyicide açın.

   Aşağıdaki JSON çıktısı gösterilir. Farklı özellikler hakkında daha fazla bilgi için bkz. [Azure özel rolleri](../role-based-access-control/custom-roles.md).

```json
   { 
     "Name": "Reader", 
     "Id": "acdd72a7-3385-48ef-bd42-f606fba81ae7", 
     "IsCustom": false, 
     "Description": "Lets you view everything, but not make any changes.", 
     "Actions": [ 
      "*/read" 
     ], 
     "NotActions": [], 
     "DataActions": [], 
     "NotDataActions": [], 
     "AssignableScopes": [ 
       "/" 
     ] 
   } 
```
5. Şunu eklemek için JSON dosyasını düzenleyin 

   `*/read", "Microsoft.Network/*/read", "Microsoft.Network/firewallPolicies/ruleCollectionGroups/write` 

    **Eylemler**   özelliği için işlem. Okuma işleminden sonra virgül eklemeyi unutmayın. Bu eylem, kullanıcının kural koleksiyonu grupları oluşturmasına ve güncelleştirmesine izin verir.
6.  **Astifblescopes**, abonelik kimliğinizi şu biçimle ekleyin: 

   `/subscriptions/xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx`

   Açık abonelik kimliklerini girmeniz gerekir, aksi halde rolü aboneliğinize aktaramazsınız.
7.  **ID**   özellik satırını silin ve **IsCustom**   özelliğini true olarak değiştirin.
8.  **Ad**   ve **Açıklama**   özelliklerini *azfm kural koleksiyonu grubu yazarı* olarak değiştirme ve *Bu roldeki kullanıcılar güvenlik duvarı ilke kuralı koleksiyon gruplarını düzenleyebilir*

JSON dosyanız aşağıdaki örneğe benzer şekilde görünmelidir:

```
{ 

    "Name":  "AZFM Rule Collection Group Author", 
    "IsCustom":  true, 
    "Description":  "Users in this role can edit Firewall Policy rule collection groups", 
    "Actions":  [ 
                    "*/read", 
                    "Microsoft.Network/*/read", 
                     "Microsoft.Network/firewallPolicies/ruleCollectionGroups/write" 
                ], 
    "NotActions":  [ 
                   ], 
    "DataActions":  [ 
                    ], 
    "NotDataActions":  [ 
                       ], 
    "AssignableScopes":  [ 
                             "/subscriptions/xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxx"] 
} 
```
9. Yeni özel rolü oluşturmak için New-AzRoleDefinition komutunu kullanın ve JSON rol tanımı dosyasını belirtin. 

   `New-AzRoleDefinition -InputFile "C:\CustomRoles\RuleCollectionGroupRole.json`

### <a name="list-custom-roles"></a>Özel rolleri listeleme

Tüm özel rolleri listelemek için Get-AzRoleDefinition komutunu kullanabilirsiniz:

   `Get-AzRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom`

Ayrıca, Azure portal özel rolleri de görebilirsiniz. Aboneliğinize gidin, **erişim denetimi (IAM)**, **Roller**' i seçin.

:::image type="content" source="media/rule-hierarchy/sales-app-policy.png" alt-text="SalesAppPolicy":::

:::image type="content" source="media/rule-hierarchy/sales-app-policy-read.png" alt-text="SalesAppPolicy okuma izni":::

Daha fazla bilgi için bkz. [öğretici: Azure PowerShell kullanarak Azure özel rolü oluşturma](../role-based-access-control/tutorial-custom-role-powershell.md).

### <a name="add-users-to-the-custom-role"></a>Özel Role Kullanıcı ekleme

Portalda, AZFM kuralı koleksiyon grubu yazarları rolüne kullanıcı ekleyebilir ve güvenlik duvarı ilkelerine erişim sağlayabilirsiniz.

1. Portalda, uygulama ekibi güvenlik duvarı ilkesini seçin (örneğin, SalesAppPolicy).
2. **Access Control**seçin.
3. **Rol ataması ekle**’yi seçin.
4. Role Kullanıcı/Kullanıcı grupları (örneğin, satış ekibi) ekleyin.

Diğer güvenlik duvarı ilkeleri için bu yordamı tekrarlayın.

### <a name="summary"></a>Özet

Özel RBAC ile güvenlik duvarı Ilkesi artık güvenlik duvarı ilke kuralı koleksiyon gruplarına seçmeli erişim sağlar.

Kullanıcıların şunları yapmak için izinleri yoktur:
- Azure Güvenlik duvarını veya güvenlik duvarı ilkesini silin.
- Güvenlik duvarı ilkesi hiyerarşisini veya DNS ayarlarını ya da tehdit bilgilerini güncelleştirin.
- Güvenlik Duvarı ilkesini AZFM kural koleksiyonu grubu yazar grubunun üyesi olmadıkları yerde güncelleştirin.

Güvenlik yöneticileri, temel ilkeyi kullanarak kuruluş için gerekli olan belirli trafik türlerini (örneğin, ıCMP) uygulayabilir. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure Güvenlik duvarı ilkesi](policy-overview.md)hakkında daha fazla bilgi edinin.

