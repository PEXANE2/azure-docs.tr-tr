---
title: Yeni bir Hizmet Kumaşı kümesi için yönetilen kimlik desteğini yapılandırma
description: 'Yeni Bir Azure Hizmet Kumaşı kümesinde yönetilen kimlik desteğini şu şekilde etkinleştirebilirsiniz:'
ms.topic: article
ms.date: 12/09/2019
ms.custom: sfrev
ms.openlocfilehash: dd0cbd3251185a7831852ead47ca0b120126cf55
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415663"
---
# <a name="configure-managed-identity-support-for-a-new-service-fabric-cluster"></a>Yeni bir Hizmet Kumaşı kümesi için yönetilen kimlik desteğini yapılandırma

Hizmet Kumaşı uygulamalarınızda [Azure kaynakları için Yönetilen kimlikleri](../active-directory/managed-identities-azure-resources/overview.md) kullanmak için, önce kümedeki Yönetilen Kimlik *Belirteç Hizmeti'ni* etkinleştirin. Bu hizmet, Yönetilen kimliklerini kullanarak Service Fabric uygulamalarının kimlik doğrulamalarından ve onlar adına erişim belirteçleri almaktan sorumludur. Hizmet etkinleştirildikten sonra, sol bölmedeki **Sistem** bölümünün altında, diğer sistem hizmetlerinin yanında **kumaş:/System/ManagedIdentityTokenService** adı altında çalışan Service Fabric Explorer'da görebilirsiniz.

> [!NOTE]
> **Yönetilen Kimlik Belirteç Hizmeti**etkinleştirmek için Service Fabric runtime sürüm 6.5.658.9590 veya daha yüksek gereklidir.  

## <a name="enable-the-managed-identity-token-service"></a>Yönetilen Kimlik Belirteç Hizmetini Etkinleştirme

Küme oluşturma zamanında Yönetilen Kimlik Belirteç Hizmeti'ni etkinleştirmek için, küme Azure Kaynak Yöneticisi şablonunuza aşağıdaki snippet'i ekleyin:

```json
"fabricSettings": [
    {
        "name": "ManagedIdentityTokenService",
        "parameters": [
            {
                "name": "IsEnabled",
                "value": "true"
            }
        ]
    }
]
```

## <a name="errors"></a>Hatalar

Dağıtım bu iletiyle başarısız olursa, kümenin gerekli Hizmet Kumaşı sürümünde olmadığı anlamına gelir (desteklenen minimum çalışma süresi 6,5 CU2'dir):


```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="related-articles"></a>İlgili Makaleler

* Azure Hizmet Kumaşı'nda [yönetilen kimlik desteğini](./concepts-managed-identity.md) gözden geçirme

* [Varolan bir Azure Hizmet Kumaşı kümesinde yönetilen kimlik desteğini etkinleştirme](./configure-existing-cluster-enable-managed-identity-token-service.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Sistem tarafından atanmış yönetilen bir kimliğe sahip bir Azure Hizmet Kumaşı uygulamasını dağıtma](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Kullanıcı tarafından atanan yönetilen bir kimliğe sahip bir Azure Hizmet Kumaşı uygulaması dağıtma](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Hizmet kodundan Hizmet Kumaşı uygulamasının yönetilen kimliğinden yararlanma](./how-to-managed-identity-service-fabric-app-code.md)
* [Azure Hizmet Kumaşı uygulamasına diğer Azure kaynaklarına erişim hakkı verme](./how-to-grant-access-other-resources.md)