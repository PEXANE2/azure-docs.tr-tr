---
title: REST kullanarak kullanıcı tarafından atanan yönetilen kimlikleri yönetme - Azure AD
description: REST API aramaları yapmak için kullanıcı tarafından atanan yönetilen bir kimliğin nasıl oluşturulacağına, listelenene ve silinmeye ilişkin adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/26/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39e108451e4c19e77e01b5bcc5d8dd21e86ad73a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74547422"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-rest-api-calls"></a>REST API çağrılarını kullanarak kullanıcı tarafından atanan yönetilen bir kimlik oluşturma, listele veya silme

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için yönetilen kimlikler, Azure hizmetlerine kodunuzda kimlik bilgilerine gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlere kimlik doğrulama olanağı sağlar. 

Bu makalede, REST API aramaları yapmak için CURL kullanarak kullanıcı tarafından atanan yönetilen bir kimliği nasıl oluşturabileceğinizi, listelemeyi ve silmeyi öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Windows kullanıyorsanız, Linux [için Windows Alt Sistemini](https://msdn.microsoft.com/commandline/wsl/about) yükleyin veya Azure portalında [Azure Bulut Kabuğu'nu](../../cloud-shell/overview.md) kullanın.
- Linux için [Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Linux dağıtım işletim sistemi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest)kullanıyorsanız, [Azure CLI yerel konsoluna yükleyin.](/cli/azure/install-azure-cli)
- Azure CLI yerel konsolu kullanıyorsanız, kullanıcı `az login` tarafından atanan yönetilen kimlik bilgilerini dağıtmak veya almak istediğiniz Azure aboneliğiyle ilişkili bir hesapla Azure'da oturum açın.
- Aşağıdaki kullanıcı tarafından atanan `az account get-access-token` yönetilen kimlik işlemlerini gerçekleştirmek için kullanarak bir Taşıyıcı erişim jetonu alın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için, hesabınızın [Yönetilen Kimlik Oluşturici](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```

**İstek üstbilgi**

|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        |

**İstek gövdesi**

|Adı  |Açıklama  |
|---------|---------|
|location     | Gereklidir. Kaynak konumu.        |

## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listele

Kullanıcı tarafından atanan yönetilen bir kimliği listelemek/okumak için hesabınızın [Yönetilen Kimlik Operatörü'ne](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya Yönetilen [Kimlik Katkıda Bulunan](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview HTTP/1.1
```

|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        |

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliği silme

Kullanıcı tarafından atanan yönetilen bir kimliği silmek için, hesabınızın [Yönetilen Kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

> [!NOTE]
> Kullanıcı tarafından atanan yönetilen bir kimliğisiletmek, başvuruyu atandığı herhangi bir kaynaktan kaldırmaz. CURL kullanarak kullanıcı tarafından atanan yönetilen bir kimliği [VM'den](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm)kaldırmak için bkz.

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
DELETE https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```
|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*İçerik Türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gereklidir. Geçerli `Bearer` bir erişim jetonuna ayarlayın.        |

## <a name="next-steps"></a>Sonraki adımlar

Curl see kullanarak bir Azure VM/VMSS'e kullanıcı tarafından atanan yönetilen bir kimliğin nasıl atayılabildiğini öğrenmek için, [REST API çağrılarını kullanarak Azure VM'deki Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-rest-vm.md#user-assigned-managed-identity) ve REST [API çağrılarını kullanarak sanal makine ölçeğinde Azure kaynakları için yönetilen kimlikleri yapılandırma.](qs-configure-rest-vmss.md#user-assigned-managed-identity)
