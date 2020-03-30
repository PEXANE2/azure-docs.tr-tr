---
title: Yönetilen bir kimliğin hizmet ilkesini görüntüleme - Azure CLI - Azure AD
description: Azure CLI kullanarak yönetilen bir kimliğin hizmet ilkesini görüntülemek için adım adım yönergeler.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79298708"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-azure-cli"></a>Azure CLI'yi kullanarak yönetilen bir kimliğin hizmet ilkesini görüntüleme

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure CLI'yi kullanarak yönetilen bir kimliğin hizmet ilkesini nasıl görüntüleyişersiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın.
- Zaten bir Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun.](https://azure.microsoft.com/free/)
- [Sanal bir makinede](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) veya [uygulamada](/azure/app-service/overview-managed-identity#add-a-system-assigned-identity)sistem atanan kimliği etkinleştirin.
- CLI komut dosyası örneklerini çalıştırmak için üç seçeneğiniz var:
    - Azure portalından [Azure Bulut Shell'i](../../cloud-shell/overview.md) kullanın (sonraki bölüme bakın).
    - Her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesi aracılığıyla gömülü Azure Bulut Kabuğu'nu kullanın.
    - Yerel bir CLI konsolu kullanmayı ve Azure'da oturum açmayı tercih ediyorsanız [Azure CLI'nin en son sürümünü yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli)`az login`
 
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="view-the-service-principal"></a>Hizmet ilkesini görüntüleme

Bu aşağıdaki komut, yönetilen kimlik etkinleştirilmiş bir VM veya uygulamanın hizmet ilkesinin nasıl görüntülenebildiğini gösterir. Kendi `<VM or application name>` değerlerinizle değiştirin. 

```azurecli-interactive
az ad sp list --display-name <VM or application name>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI'yi kullanarak Azure AD hizmet ilkelerini yönetme hakkında daha fazla bilgi için [az reklam sp'ye](/cli/azure/ad/sp)bakın.


