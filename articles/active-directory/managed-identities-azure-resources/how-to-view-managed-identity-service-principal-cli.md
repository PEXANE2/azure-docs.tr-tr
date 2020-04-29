---
title: Yönetilen kimliğin hizmet sorumlusunu görüntüleme-Azure CLı-Azure AD
description: Azure CLı kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntülemek için adım adım yönergeler.
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
ms.date: 11/29/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fedef003c6d4143d3ad1814081d55b61d5ee020
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79298708"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-azure-cli"></a>Azure CLı kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntüleme

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory ' de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure CLı kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntülemeyi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın.
- Henüz bir Azure hesabınız yoksa [ücretsiz hesap için kaydolun](https://azure.microsoft.com/free/).
- [Bir sanal makine veya uygulama üzerinde sistem tarafından atanan kimliği](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) etkinleştirin. [application](/azure/app-service/overview-managed-identity#add-a-system-assigned-identity)
- CLı betiği örneklerini çalıştırmak için üç seçeneğiniz vardır:
    - Azure portal [Azure Cloud Shell](../../cloud-shell/overview.md) kullanın (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell her kod bloğunun sağ üst köşesinde bulunan "dene" düğmesini kullanarak kullanın.
    - Yerel bir CLı konsolu kullanmayı tercih ediyorsanız ve kullanarak Azure 'da oturum açmak isterseniz [Azure CLI 'nın en son sürümünü yükleyebilirsiniz](https://docs.microsoft.com/cli/azure/install-azure-cli) .`az login`
 
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="view-the-service-principal"></a>Hizmet sorumlusunu görüntüleme

Aşağıdaki komut, yönetilen kimlik etkin bir VM veya uygulamanın hizmet sorumlusunu görüntülemeyi gösterir. Kendi `<VM or application name>` değerlerinizle değiştirin. 

```azurecli-interactive
az ad sp list --display-name <VM or application name>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı kullanarak Azure AD hizmet sorumlularını yönetme hakkında daha fazla bilgi için, bkz. [az ad SP](/cli/azure/ad/sp).


