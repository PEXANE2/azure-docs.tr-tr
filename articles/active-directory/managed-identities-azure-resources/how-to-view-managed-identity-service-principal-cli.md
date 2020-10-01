---
title: Yönetilen kimliğin hizmet sorumlusunu görüntüleme-Azure CLı-Azure AD
description: Azure CLı kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntülemek için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/30/2020
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurecli
ms.openlocfilehash: 1c0f167a36ceff6a9b62e2a3ccf24ca6e13e6294
ms.sourcegitcommit: 06ba80dae4f4be9fdf86eb02b7bc71927d5671d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91611920"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-azure-cli"></a>Azure CLı kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntüleme

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory ' de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure CLı kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntülemeyi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın.
- Henüz bir Azure hesabınız yoksa [ücretsiz hesap için kaydolun](https://azure.microsoft.com/free/).
- [Bir sanal makine veya uygulama üzerinde sistem tarafından atanan kimliği](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) etkinleştirin. [application](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity)
- Örnek betikleri çalıştırmak için iki seçeneğiniz vardır:
    - Kod bloklarının sağ üst köşesindeki **It TRY** düğmesini kullanarak açabileceğiniz [Azure Cloud Shell](../../cloud-shell/overview.md)kullanın.
    - [Azure CLI](/cli/azure/install-azure-cli)'nın en son sürümünü yükleyerek betikleri yerel olarak çalıştırın, sonra [az Login](/cli/azure/reference-index#az-login)kullanarak Azure 'da oturum açın. Kaynak oluşturmak istediğiniz Azure aboneliğiyle ilişkili bir hesap kullanın.   

## <a name="view-the-service-principal"></a>Hizmet sorumlusunu görüntüleme

Aşağıdaki komut, yönetilen kimlik etkin bir VM veya uygulamanın hizmet sorumlusunu görüntülemeyi gösterir. `<Azure resource name>`Kendi değerlerinizle değiştirin.

```azurecli-interactive
az ad sp list --display-name <Azure resource name>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı kullanarak Azure AD hizmet sorumlularını yönetme hakkında daha fazla bilgi için, bkz. [az ad SP](/cli/azure/ad/sp).
