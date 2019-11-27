---
title: REST kullanarak Kullanıcı tarafından atanan yönetilen kimlikleri yönetme-Azure AD
description: REST API çağrısı yapmak için Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme ve silme konusunda adım adım yönergeler.
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
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74547422"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-rest-api-calls"></a>REST API çağrılarını kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme veya silme

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için Yönetilen kimlikler, Azure hizmetleri 'nin kodunuzda kimlik bilgilerine gerek duymadan Azure AD kimlik doğrulamasını destekleyen hizmetlere kimlik doğrulaması yapma yeteneği sağlar. 

Bu makalede, REST API çağrısı yapmak için KıVRıMLı kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturmayı, listeleyeceğinizi ve silmeyi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. **[Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Windows kullanıyorsanız, [Linux Için Windows alt sistemini](https://msdn.microsoft.com/commandline/wsl/about) yükledikten sonra Azure Portal [Azure Cloud Shell](../../cloud-shell/overview.md) kullanın.
- [Linux Için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Linux dağıtım Işletim SISTEMI](/cli/azure/install-azure-cli-apt?view=azure-cli-latest)kullanıyorsanız [Azure CLI yerel konsolunu yükleyebilirsiniz](/cli/azure/install-azure-cli).
- Azure CLı yerel Konsolu kullanıyorsanız, dağıtmak veya Kullanıcı tarafından atanan yönetilen kimlik bilgilerini almak istediğiniz Azure aboneliğiyle ilişkili bir hesapla `az login` kullanarak Azure 'da oturum açın.
- Aşağıdaki Kullanıcı tarafından atanan yönetilen kimlik işlemlerini gerçekleştirmek için `az account get-access-token` kullanarak bir taşıyıcı erişim belirteci alın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için hesabınıza [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması gerekir.

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

**İstek üst bilgileri**

|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*İçerik türü*     | Gerekli. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gerekli. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        |

**İstek gövdesi**

|Ad  |Açıklama  |
|---------|---------|
|location     | Gerekli. Kaynak konumu.        |

## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listeleme

Kullanıcı tarafından atanan yönetilen kimliği listelemek/okumak için hesabınızın [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü ataması gerekir.

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview HTTP/1.1
```

|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*İçerik türü*     | Gerekli. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gerekli. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        |

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliği silme

Kullanıcı tarafından atanan bir yönetilen kimliği silmek için hesabınıza [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması gerekir.

> [!NOTE]
> Kullanıcı tarafından atanan bir yönetilen kimliğin silinmesi, atandığı herhangi bir kaynaktaki başvuruyu kaldırmaz. Bir VM 'den Kullanıcı tarafından atanan yönetilen kimliği, KıVRıMLı kullanarak kaldırmak için bkz. [Azure VM 'den Kullanıcı tarafından atanan kimliği kaldırma](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
DELETE https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```
|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*İçerik türü*     | Gerekli. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gerekli. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        |

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı tarafından atanan yönetilen kimliği, bir Azure VM/VMSS 'yi kullanarak atama hakkında daha fazla bilgi için bkz. [bir Azure VM üzerinde Azure kaynakları için Yönetilen kimlikler yapılandırma REST API çağırır](qs-configure-rest-vm.md#user-assigned-managed-identity) ve [REST API çağrılarını kullanarak bir sanal makine ölçek kümesindeki Azure kaynakları için Yönetilen kimlikler](qs-configure-rest-vmss.md#user-assigned-managed-identity)yapılandırma.
