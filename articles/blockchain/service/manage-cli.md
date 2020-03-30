---
title: Azure CLI'yi kullanarak Azure Blockchain Hizmetini yönetme
description: Azure CLI ile Azure Blockchain Hizmeti nasıl yönetilir?
ms.date: 11/22/2019
ms.topic: article
ms.reviewer: janders
ms.openlocfilehash: ac75be644877905c1517395c1c789b1ea16fd49c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74455589"
---
# <a name="manage-azure-blockchain-service-using-azure-cli"></a>Azure CLI'yi kullanarak Azure Blockchain Hizmetini yönetme

Azure portalına ek olarak, Azure Blockchain Hizmetiniz için blockchain üyelerini ve işlem düğümlerini yönetmek için Azure CLI'yi kullanabilirsiniz.

En son [Azure CLI'sini](https://docs.microsoft.com/cli/azure/install-azure-cli) yüklediğinizden ve bir Azure hesabına giriş yaptığınızdan emin `az login`olun.

Aşağıdaki örneklerde, örneği `<parameter names>` kendi değerlerinizle değiştirin.

## <a name="create-blockchain-member"></a>Blockchain üyesi oluşturma

Örnek, Azure Blockchain Hizmeti'nde, Yeni bir konsorsiyumda Quorum genel muhasebe protokolünü çalıştıran bir blockchain üyesi oluşturur.

```azurecli
az resource create \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --is-full-object \
                     --properties '{ "location":"<myBlockchainLocation>", "properties": {"password":"<myStrongPassword>", "protocol":"Quorum","consortium":"<myConsortiumName>", "consortiumManagementAccountPassword":"<myConsortiumManagementAccountPassword>", "firewallRules":[{"ruleName":"<myRuleName>","startIpAddress":"<myStartIpAddress>", "endIpAddress":"<myEndIpAddress>"}]}, "sku":{"name":"<skuName>"}}'
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının oluşturulduğu kaynak grubu adı. |
| **Adı** | Azure Blockchain Hizmeti blockchain üyenizi tanımlayan benzersiz bir ad. Ad, ortak bitiş noktası adresi için kullanılır. Örneğin, `myblockchainmember.blockchain.azure.com`. |
| **Konum** | Blockchain üyesinin oluşturulduğu Azure bölgesi. Örneğin, `eastus`. Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin. |
| **parola** | Üye hesabı parolası. Üye hesabı parolası, temel kimlik doğrulaması kullanarak blockchain üyesinin genel bitiş noktasına kimlik doğrulamak için kullanılır. Parola aşağıdaki dört gereksinimden üçünü karşılamalıdır: uzunluk 12 & 72 karakter, 1 küçük harf karakteri, 1 büyük harf karakteri, 1 sayı ve sayı işareti olmayan 1 özel karakter(#), yüzde/), virgül(,), yıldız(*), geri teklif(),çift\`tırnak("), tek teklif('), tire(-) ve yarı sütun(;)|
| **Protokolü** | Genel önizleme Quorum'u destekler. |
| **Konsorsiyumu** | Katılmak veya oluşturmak için konsorsiyumun adı. |
| **konsorsiyumManagementAccountPassword** | Konsorsiyum yönetim şifresi. Parola, bir konsorsiyuma katılmak için kullanılır. |
| **Rulename** | IP adres aralığını beyaz listeye almak için kural adı. Güvenlik duvarı kuralları için isteğe bağlı parametre.|
| **başlangıçIpAddress** | Beyaz liste için IP adres aralığının başlangıcı. Güvenlik duvarı kuralları için isteğe bağlı parametre. |
| **endIpAddress** | Beyaz liste için IP adres aralığının sonu. Güvenlik duvarı kuralları için isteğe bağlı parametre. |
| **skuName** | Katman türü. Temel için Standart ve B0 için S0 kullanın. |

## <a name="change-blockchain-member-password"></a>blockchain üye şifreni değiştir

Örnek, blockchain üyesinin parolayı değiştirir.

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.password='<myStrongPassword>' \
                     --remove properties.consortiumManagementAccountAddress
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının oluşturulduğu kaynak grubu adı. |
| **Adı** | Azure Blockchain Service üyenizi tanımlayan ad. |
| **parola** | Üye hesabı parolası. Parola aşağıdaki dört gereksinimden üçünü karşılamalıdır: uzunluk 12 & 72 karakter, 1 küçük harf karakteri, 1 büyük harf karakteri, 1 sayı ve sayı işareti olmayan 1 özel karakter(#), yüzde/), virgül(,), yıldız(*), geri teklif(),çift\`tırnak("), tek tırnak('), dash(-) ve yarı kolon (;)) arasında olmalıdır. |

## <a name="create-transaction-node"></a>İşlem düğümü oluşturma

Varolan bir blockchain üyesinin içinde bir hareket düğümü oluşturun. Hareket düğümleri ekleyerek, güvenlik yalıtımı artırabilir ve yük dağıtabilirsiniz. Örneğin, farklı istemci uygulamaları için bir işlem düğümü bitiş noktası olabilir.

```azurecli
az resource create \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName>/transactionNodes/<myTransactionNode> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --is-full-object \
                     --properties '{"location":"<myRegion>", "properties":{"password":"<myStrongPassword>", "firewallRules":[{"ruleName":"<myRuleName>", "startIpAddress":"<myStartIpAddress>", "endIpAddress":"<myEndIpAddress>"}]}}'
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının oluşturulduğu kaynak grubu adı. |
| **Adı** | Yeni işlem düğümü adını da içeren Azure Blockchain Hizmeti blockchain üyesinin adı. |
| **Konum** | Blockchain üyesinin oluşturulduğu Azure bölgesi. Örneğin, `eastus`. Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin. |
| **parola** | Hareket düğümü parolası. Parola aşağıdaki dört gereksinimden üçünü karşılamalıdır: uzunluk 12 & 72 karakter, 1 küçük harf karakteri, 1 büyük harf karakteri, 1 sayı ve sayı işareti olmayan 1 özel karakter(#), yüzde/), virgül(,), yıldız(*), geri teklif(),çift\`tırnak("), tek tırnak('), dash(-) ve yarı kolon (;)) arasında olmalıdır. |
| **Rulename** | IP adres aralığını beyaz listeye almak için kural adı. Güvenlik duvarı kuralları için isteğe bağlı parametre. |
| **başlangıçIpAddress** | Beyaz liste için IP adres aralığının başlangıcı. Güvenlik duvarı kuralları için isteğe bağlı parametre. |
| **endIpAddress** | Beyaz liste için IP adres aralığının sonu. Güvenlik duvarı kuralları için isteğe bağlı parametre.|

## <a name="change-transaction-node-password"></a>İşlem düğümü parolayı değiştirme

Örnek bir işlem düğümü parolası değiştirir.

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName>/transactionNodes/<myTransactionNode> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.password='<myStrongPassword>'
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **Adı** | Yeni işlem düğümü adını da içeren Azure Blockchain Hizmeti blockchain üyesinin adı. |
| **parola** | Hareket düğümü parolası. Parola aşağıdaki dört gereksinimden üçünü karşılamalıdır: uzunluk 12 & 72 karakter, 1 küçük harf karakteri, 1 büyük harf karakteri, 1 sayı ve sayı işareti olmayan 1 özel karakter(#), yüzde/), virgül(,), yıldız(*), geri teklif(),çift\`tırnak("), tek tırnak('), dash(-) ve yarı kolon (;)) arasında olmalıdır. |

## <a name="change-consortium-management-account-password"></a>Konsorsiyum yönetim hesap parolası değiştirme

Konsorsiyum yönetim hesabı konsorsiyum üyelik yönetimi için kullanılır. Her üye benzersiz bir konsorsiyum yönetim hesabı tarafından tanımlanır ve aşağıdaki komut ile bu hesabın şifresini değiştirebilirsiniz.

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.consortiumManagementAccountPassword='<myConsortiumManagementAccountPassword>' \
                     --remove properties.consortiumManagementAccountAddress
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının oluşturulduğu kaynak grubu adı. |
| **Adı** | Azure Blockchain Service üyenizi tanımlayan ad. |
| **konsorsiyumManagementAccountPassword** | Konsorsiyum yönetim hesap şifresi. Parola aşağıdaki dört gereksinimden üçünü karşılamalıdır: uzunluk 12 & 72 karakter, 1 küçük harf karakteri, 1 büyük harf karakteri, 1 sayı ve sayı işareti olmayan 1 özel karakter(#), yüzde/), virgül(,), yıldız(*), geri teklif(),çift\`tırnak("), tek tırnak('), dash(-) ve yarı kolon (;)) arasında olmalıdır. |
  
## <a name="update-firewall-rules"></a>Güvenlik duvarı kurallarını güncelleştirme

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.firewallRules='[{"ruleName":"<myRuleName>", "startIpAddress":"<myStartIpAddress>", "endIpAddress":"<myEndIpAddress>"}]' \
                     --remove properties.consortiumManagementAccountAddress
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **Adı** | Azure Blockchain Hizmeti blockchain üyesinin adı. |
| **Rulename** | IP adres aralığını beyaz listeye almak için kural adı. Güvenlik duvarı kuralları için isteğe bağlı parametre.|
| **başlangıçIpAddress** | Beyaz liste için IP adres aralığının başlangıcı. Güvenlik duvarı kuralları için isteğe bağlı parametre.|
| **endIpAddress** | Beyaz liste için IP adres aralığının sonu. Güvenlik duvarı kuralları için isteğe bağlı parametre.|

## <a name="list-api-keys"></a>API tuşlarını listele

API anahtarları, kullanıcı adı ve parolaya benzer düğüm erişimi için kullanılabilir. Anahtar döndürmeyi destekleyen iki API anahtarı vardır. API tuşlarınızı listelemek için aşağıdaki komutu kullanın.

```azurecli
az resource invoke-action \
                            --resource-group <myResourceGroup> \
                            --name <myMemberName>/transactionNodes/<myTransactionNode> \
                            --action "listApiKeys" \
                            --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **Adı** | Yeni işlem düğümü adını da içeren Azure Blockchain Hizmeti blockchain üyesinin adı. |

## <a name="regenerate-api-keys"></a>API tuşlarını yeniden oluşturma

API anahtarlarınızı yeniden oluşturmak için aşağıdaki komutu kullanın.

```azurecli
az resource invoke-action \
                            --resource-group <myResourceGroup> \
                            --name <myMemberName>/transactionNodes/<myTransactionNode> \
                            --action "regenerateApiKeys" \
                            --resource-type Microsoft.Blockchain/blockchainMembers \
                            --request-body '{"keyName":"<keyValue>"}'
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **Adı** | Yeni işlem düğümü adını da içeren Azure Blockchain Hizmeti blockchain üyesinin adı. |
| **Keyname** | keyValue'ı \<\> key1 veya key2 ile değiştirin. |

## <a name="delete-a-transaction-node"></a>Hareket düğümünü silme

Örnek bir blockchain üye hareket düğümü siler.

```azurecli
az resource delete \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName>/transactionNodes/<myTransactionNode> \
                     --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **Adı** | Silinecek işlem düğümü adını da içeren Azure Blockchain Hizmeti blockchain üyesinin adı. |

## <a name="delete-a-blockchain-member"></a>Blockchain üyesini silme

Örnek bir blockchain üyesini siler.

```azurecli
az resource delete \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **Adı** | Silinecek Azure Blockchain Hizmeti blockchain üyesinin adı. |

## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="grant-access-for-azure-ad-user"></a>Azure AD kullanıcısı için erişim izni verme

```azurecli
az role assignment create \
                            --role <role> \
                            --assignee <assignee> \
                            --scope /subscriptions/<subId>/resourceGroups/<groupName>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>
```

| Parametre | Açıklama |
|---------|-------------|
| **Rolü** | Azure AD rolünün adı. |
| **devrilen** | Azure AD kullanıcı kimliği. Örneğin, `user@contoso.com` |
| **Kapsam** | Rol atamasının kapsamı. Blockchain üyesi veya işlem düğümü olabilir. |

**Örnek:**

Azure AD kullanıcısının blockchain **üyesine**düğüm erişimi verme:

```azurecli
az role assignment create \
                            --role 'myRole' \
                            --assignee user@contoso.com \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

**Örnek:**

Azure AD kullanıcısının blockchain işlem düğümüne düğüm erişimi **nesle**izin verme:

```azurecli
az role assignment create \
                            --role 'MyRole' \
                            --assignee user@contoso.com \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1/transactionNodes/contosoTransactionNode1
```

### <a name="grant-node-access-for-azure-ad-group-or-application-role"></a>Azure AD grubu veya uygulama rolü için düğüm erişimi verme

```azurecli
az role assignment create \
                            --role <role> \
                            --assignee-object-id <assignee_object_id>
```

| Parametre | Açıklama |
|---------|-------------|
| **Rolü** | Azure AD rolünün adı. |
| **atanın-nesne-id** | Azure AD grup kimliği veya uygulama kimliği. |
| **Kapsam** | Rol atamasının kapsamı. Blockchain üyesi veya işlem düğümü olabilir. |

**Örnek:**

**Uygulama rolü** için düğüm erişimi verme

```azurecli
az role assignment create \
                            --role 'myRole' \
                            --assignee-object-id 22222222-2222-2222-2222-222222222222 \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

### <a name="remove-azure-ad-node-access"></a>Azure AD düğümü erişimini kaldırma

```azurecli
az role assignment delete \
                            --role <myRole> \
                            --assignee <assignee> \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/<myResourceGroup>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>/transactionNodes/<myTransactionNode>
```

| Parametre | Açıklama |
|---------|-------------|
| **Rolü** | Azure AD rolünün adı. |
| **devrilen** | Azure AD kullanıcı kimliği. Örneğin, `user@contoso.com` |
| **Kapsam** | Rol atamasının kapsamı. Blockchain üyesi veya işlem düğümü olabilir. |

## <a name="next-steps"></a>Sonraki adımlar

[Azure portalı ile Azure Blockchain Hizmet işlem düğümlerini nasıl yapılandırıştırmayı](configure-transaction-nodes.md)öğrenin.
