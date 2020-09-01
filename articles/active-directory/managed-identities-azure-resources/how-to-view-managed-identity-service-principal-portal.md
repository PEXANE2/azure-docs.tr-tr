---
title: Azure portal yönetilen bir kimliğin hizmet sorumlusunu görüntüleme-Azure AD
description: Azure portal yönetilen bir kimliğin hizmet sorumlusunu görüntülemek için adım adım yönergeler.
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
ms.date: 11/29/2018
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: c88f3cdc880888162301ace5c896161f669d2ede
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89269208"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>Azure portal yönetilen bir kimliğin hizmet sorumlusunu görüntüleme

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory ' de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure portal kullanarak yönetilen bir kimliğin hizmet sorumlusunu görüntülemeyi öğreneceksiniz.

 > [!NOTE] 
 > Hizmet sorumluları kurumsal uygulamalardır. 

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın.
- Henüz bir Azure hesabınız yoksa [ücretsiz hesap için kaydolun](https://azure.microsoft.com/free/).
- [Bir sanal makine veya uygulama üzerinde sistem tarafından atanan kimliği](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) etkinleştirin. [application](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity)

## <a name="view-the-service-principal"></a>Hizmet sorumlusunu görüntüleme

Bu yordamda, sistem tarafından atanan kimlik etkin bir VM 'nin hizmet sorumlusu nasıl görüntüleneceği gösterilir (bir uygulama için aynı adımlar geçerlidir).

1. **Azure Active Directory** ' a ve ardından **Kurumsal uygulamalar**' a tıklayın.
2. **Uygulama türü**altında **tüm uygulamalar** ' ı seçin ve ardından **Uygula**' ya tıklayın.
3. Arama filtresi kutusunda, yönetilen kimliği etkin olan VM veya uygulamanın adını yazın veya sunulan listeden seçin.

   ![Portalda yönetilen kimlik hizmeti sorumlusunu görüntüleme](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure kaynakları için yönetilen kimlikler](./overview.md)