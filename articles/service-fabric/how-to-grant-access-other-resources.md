---
title: Diğer Azure kaynaklarına uygulama erişimi verme
description: Bu makalede, yönetilen kimlik etkin Hizmet Dokusu uygulamanızın Azure Etkin Dizin tabanlı kimlik doğrulamasını destekleyen diğer Azure kaynaklarına nasıl erişilebildiğini açıklanmaktadır.
ms.topic: article
ms.date: 12/09/2019
ms.openlocfilehash: 3b1feab1e67e993df771564a1a7c1aba4236b2c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614802"
---
# <a name="granting-a-service-fabric-applications-managed-identity-access-to-azure-resources-preview"></a>Hizmet Kumaşı uygulamasının Azure kaynaklarına yönetilen kimlik erişimini verme (önizleme)

Uygulamanın yönetilen kimliğini diğer kaynaklara erişmek için kullanabilmesi için, erişilen korumalı Azure kaynağında bu kimliğe izin verilmesi gerekir. İzin vermek genellikle Azure hizmetinin 'denetim düzleminde' azure kaynak yöneticisi aracılığıyla yönlendirilen ve geçerli rol tabanlı erişim denetimini zorunlu kılarak yönlendirilen korumalı kaynağa sahip bir yönetim eylemidir.

Adımların tam sırası, erişilen Azure kaynağının türüne ve izin vermek için kullanılan dil/istemciye bağlıdır. Makalenin geri kalanı, uygulamaya atanan kullanıcı tarafından atanmış bir kimliği varsayar ve sizin rahatınız için birkaç tipik örnek içerir, ancak bu konu için hiçbir şekilde kapsamlı bir başvuru değildir; izinlerin verilmesiyle ilgili güncel talimatlar için ilgili Azure hizmetlerinin belgelerine başvurun.  

## <a name="granting-access-to-azure-storage"></a>Azure Depolama'ya erişim izni verme
Bir Azure depolama blob verileri almak için Hizmet Kumaş ı uygulamasının yönetilen kimliğini (bu durumda kullanıcı tarafından atanmış) kullanabilirsiniz. Azure portalında gerekli izinleri aşağıdaki adımlarla kimliğinizi ver:

1. Depolama hesabına gidin
2. Sol bölmedeki Erişim denetimi (IAM) bağlantısına tıklayın.
3. (isteğe bağlı) Varolan erişimi denetleyin: 'Bul' denetiminde Sistem veya Kullanıcı tarafından atanan yönetilen kimliği seçin; takip eden sonuç listesinden uygun kimliği seçin
4. Uygulamanın kimliği için yeni bir rol ataması eklemek için sayfanın üstüne + rol ataması ekleyin'i tıklatın.
Role altında, açılır açılır yerden Storage Blob Data Reader'ı seçin.
5. Bir sonraki açılır da, Access'i `User assigned managed identity`atayın altında, seçin.
6. Ardından, uygun aboneliğin Abonelik’te listelendiğinden emin olun ve sonra Kaynak Grubu’nu Tüm kaynak grupları olarak ayarlayın.
7. Select'in altında, Hizmet Kumaşı uygulamasına karşılık gelen UAI'yi seçin ve ardından Kaydet'i tıklatın.

Sistem tarafından atanan Hizmet Dokusu yönetilen kimlikler için destek, Azure portalında tümleştirmeyi içermez; Uygulamanız sistem tarafından atanmış bir kimlik kullanıyorsa, önce uygulamanın kimliğinin istemci kimliğini bulmanız ve ardından `Azure AD user, group, or service principal` yukarıdaki adımları yinelemeniz ancak Bul denetimindeki seçeneği seçmeniz gerekir.

## <a name="granting-access-to-azure-key-vault"></a>Azure Anahtar Kasası'na erişim izni verme
Benzer şekilde depolama alanına erişileyerek, Bir Azure anahtar kasasına erişmek için Hizmet Kumaşı uygulamasının yönetilen kimliğinden yararlanabilirsiniz. Azure portalına erişim izni verme adımları yukarıda listelenenlere benzer ve burada tekrarlanmaz. Farklılıklar için aşağıdaki resme bakın.

![Key Vault erişim ilkesi](../key-vault/media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

Aşağıdaki örnek, şablon dağıtımı yoluyla kasaya erişim izni vermeyi göstermektedir; şablon `resources` unsuru altında başka bir giriş olarak aşağıdaki snippet(s) ekleyin. Örnek, sırasıyla hem kullanıcı tarafından atanan hem de sistem tarafından atanan kimlik türleri için erişim izni ni gösterir - geçerli olanı seçin.

```json
    # under 'variables':
  "variables": {
        "userAssignedIdentityResourceId" : "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName'))]",
    }
    # under 'resources':
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
Ve sistem tarafından atanan yönetilen kimlikler için:
```json
    # under 'variables':
  "variables": {
        "sfAppSystemAssignedIdentityResourceId": "[concat(resourceId('Microsoft.ServiceFabric/clusters/applications/', parameters('clusterName'), parameters('applicationName')), '/providers/Microsoft.ManagedIdentity/Identities/default')]"
    }
    # under 'resources':
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2018-02-14",
        "properties": {
            "accessPolicies": [
            {
                    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
                    "tenantId": "[reference(variables('sfAppSystemAssignedIdentityResourceId'), '2018-11-30').tenantId]",
                    "objectId": "[reference(variables('sfAppSystemAssignedIdentityResourceId'), '2018-11-30').principalId]",
                    "dependsOn": [
                        "[variables('sfAppSystemAssignedIdentityResourceId')]"
                    ],
                    "permissions": {
                        "secrets": [
                            "get",
                            "list"
                        ],
                        "certificates": 
                        [
                            "get", 
                            "list"
                        ]
                    }
            },
        ]
        }
    }
```

Daha fazla bilgi için lütfen [Vaults - Update Access Policy](https://docs.microsoft.com/rest/api/keyvault/vaults/updateaccesspolicy)'ye bakın.

## <a name="next-steps"></a>Sonraki adımlar
* [Sistem tarafından atanmış yönetilen bir kimliğe sahip bir Azure Hizmet Kumaşı uygulamasını dağıtma](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Kullanıcı tarafından atanan yönetilen bir kimliğe sahip bir Azure Hizmet Kumaşı uygulaması dağıtma](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)