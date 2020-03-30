---
title: Azure portalında yönetilen bir kimliğin hizmet ilkesini görüntüleme - Azure AD
description: Azure portalında yönetilen bir kimliğin hizmet ilkesini görüntülemek için adım adım yönergeler.
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
ms.openlocfilehash: c12f15cc79d5329d028239ade4e18a853000bf01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79298606"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>Azure portalında yönetilen bir kimliğin hizmet ilkesini görüntüleme

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure portalını kullanarak yönetilen bir kimliğin hizmet ilkesini nasıl görüntüleyebilirsiniz.

 > [!NOTE] 
 > Hizmet ilkeleri Kurumsal Uygulamalar'dır. 

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın.
- Zaten bir Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun.](https://azure.microsoft.com/free/)
- [Sanal bir makinede](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) veya [uygulamada](/azure/app-service/overview-managed-identity#add-a-system-assigned-identity)sistem atanan kimliği etkinleştirin.

## <a name="view-the-service-principal"></a>Hizmet ilkesini görüntüleme

Bu yordam, sistem atanmış kimlik etkin bir VM hizmet ilkesinasıl görüntülenebilir gösterir (aynı adımlar bir uygulama için geçerlidir).

1. **Azure Etkin Dizini'ni** tıklatın ve ardından **Kurumsal uygulamaları**tıklatın.
2. **Uygulama Türü**altında, **Tüm Uygulamaları** seçin ve sonra **Uygula'yı**tıklatın.
3. Arama filtresi kutusuna, kimliği etkinleştirilen VM veya uygulamanın adını yazın veya sunulan listeden seçin.

   ![Yönetilen kimlik hizmeti ilkesini portalda görüntüleme](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure kaynakları için yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview)

