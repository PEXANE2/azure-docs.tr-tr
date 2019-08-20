---
title: Azure Service Fabric-diğer Azure kaynaklarına Service Fabric uygulama erişimi verme | Microsoft Docs
description: Bu makalede, yönetilen kimlik özellikli Service Fabric uygulamasına Azure Active Directory tabanlı kimlik doğrulamasını destekleyen diğer Azure kaynaklarına erişim izni verme açıklanmaktadır.
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.date: 08/08/2019
ms.author: atsenthi
ms.openlocfilehash: 1facd85563a54de2b703bbc8c192976db92c30f7
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69624899"
---
# <a name="granting-a-service-fabric-applications-managed-identity-access-to-azure-resources-preview"></a>Service Fabric uygulamasının Azure kaynaklarına yönetilen kimlik erişimi verme (Önizleme)

Uygulamanın diğer kaynaklara erişmek için yönetilen kimliğini kullanabilmesi için önce bu kimliğe erişilmekte olan korumalı Azure kaynağında izin verilmesi gerekir. İzin verme, genellikle Azure hizmetinin ' denetim düzlemi ' üzerinde, Azure Resource Manager aracılığıyla yönlendirilen korunan kaynağa sahip olan bir yönetim eyleminden oluşur ve bu, uygun rol tabanlı erişim denetimi uygular.

Adımların tam sırası, erişilmekte olan Azure kaynağına ve izin vermek için kullanılan dil/istemci türüne bağlıdır. Makalenin geri kalanında, uygulamaya atanan kullanıcı tarafından atanan bir kimlik varsayılır ve kolaylık olması için birkaç tipik örnek vardır ancak bu konuya yönelik ayrıntılı bir başvuru yoktur; ilgili Azure hizmetlerinin, izinleri verme konusunda güncel yönergeler için belgelerine bakın.  

## <a name="granting-access-to-azure-storage"></a>Azure depolama 'ya erişim verme
Azure Storage blobundan verileri almak için Service Fabric uygulamasının yönetilen kimliğini (Bu durumda Kullanıcı tarafından atanan) kullanabilirsiniz. Aşağıdaki adımlarla Azure portal kimlik gerekli izinleri verin:

1. Depolama hesabına gidin
2. Sol paneldeki erişim denetimi (ıAM) bağlantısına tıklayın.
3. seçim Mevcut erişimi denetle: ' bul ' denetiminde sistem veya Kullanıcı tarafından atanan yönetilen kimlik ' i seçin; Sonuç listesinden uygun kimliği seçin
4. Uygulamanın kimliği için yeni bir rol ataması eklemek için sayfanın üstünde + rol ataması Ekle ' ye tıklayın.
Rol altında, açılan listeden Depolama Blobu veri okuyucu ' yı seçin.
5. İleri açılan menüsünde, erişim ata ' nın altında, öğesini `User assigned managed identity`seçin.
6. Daha sonra, abonelik açılan menüsünde doğru aboneliğin listelendiğinden emin olun ve kaynak grubunu tüm kaynak grupları olarak ayarlayın.
7. Seç ' in altında Service Fabric uygulamasına karşılık gelen UAı öğesini seçin ve Kaydet ' e tıklayın.

Sistem tarafından atanan Service Fabric Yönetilen kimlikler için destek, Azure portal tümleştirme içermez; uygulamanız sistem tarafından atanan bir kimlik kullanıyorsa, ilk olarak uygulamanın kimliğinin istemci kimliğini bulmanız ve ardından yukarıdaki adımları tekrarlayarak bulma denetimindeki `Azure AD user, group, or service principal` seçeneği seçmeniz gerekir.

## <a name="granting-access-to-azure-key-vault"></a>Azure Key Vault erişim verme
Benzer şekilde, depolama erişimi ile bir Azure anahtar kasasına erişmek için bir Service Fabric uygulamasının yönetilen kimliğinden yararlanabilirsiniz. Azure portal erişim verme adımları yukarıda listelenenlere benzerdir ve burada yinelenmez. Farklar için aşağıdaki görüntüye bakın.

![Key Vault erişim ilkesi](../key-vault/media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

Aşağıdaki örnek, bir, şablon dağıtımı aracılığıyla bir kasaya erişim verme işlemini göstermektedir. Aşağıdaki kod parçacığını, şablonun `resources` öğesi altında başka bir girdi olarak ekleyin.

```json
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2018-02-14",
        "properties": {
            "accessPolicies": [
                {
                    "tenantId": "[reference(variables('userAssignedIdentityResourceId'), '2018-11-30').tenantId]",
                    "objectId": "[reference(variables('userAssignedIdentityResourceId'), '2018-11-30').principalId]",
                    "dependsOn": [
                        "[variables('userAssignedIdentityResourceId')]"
                    ],
                    "permissions": {
                        "keys":         ["get", "list"],
                        "secrets":      ["get", "list"],
                        "certificates": ["get", "list"]
                    }
                }
            ]
        }
    },
```

Daha fazla ayrıntı için lütfen bkz. [kasa-güncelleştirme erişim ilkesi](https://docs.microsoft.com/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="next-steps"></a>Sonraki adımlar

* [Sistem tarafından atanan yönetilen kimlik ile Azure Service Fabric uygulaması dağıtma](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)

* [Kullanıcı tarafından atanan yönetilen kimlik ile bir Azure Service Fabric uygulaması dağıtma](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)

## <a name="related-articles"></a>İlgili makaleler

* Azure Service Fabric [yönetilen kimlik desteğini](./concepts-managed-identity.md) gözden geçirme

* [Yeni bir dağıtım](./configure-new-azure-service-fabric-enable-managed-identity.md) Yönetilen kimlik desteği olan Azure Service Fabric kümesi 

* Mevcut bir Azure Service Fabric kümesinde [yönetilen kimliği etkinleştirme](./configure-existing-cluster-enable-managed-identity-token-service.md)

* [Kaynak kodundaki Service Fabric uygulamasının yönetilen kimliğinden](./how-to-managed-identity-service-fabric-app-code.md) yararlanın

* [Azure AD kimlik doğrulamasını destekleyen Azure hizmetleri](../active-directory/managed-identities-azure-resources/services-support-msi.md) listesine başvurun
