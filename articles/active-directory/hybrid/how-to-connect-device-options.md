---
title: 'Azure AD Connect: cihaz seçenekleri | Microsoft Docs'
description: Bu belge Azure AD Connect ' de kullanılabilen cihaz seçeneklerini ayrıntılarıyla
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/13/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 90d8cc39174f6392b0c532948305fa713535f0c9
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89279236"
---
# <a name="azure-ad-connect-device-options"></a>Azure AD Connect: cihaz seçenekleri

Aşağıdaki belgeler Azure AD Connect ' de kullanılabilen çeşitli cihaz seçenekleri hakkında bilgi sağlar. Aşağıdaki iki işlemi yapılandırmak için Azure AD Connect kullanabilirsiniz: 
* **Karma Azure AD katılımı**: ortamınızda şirket ıçı bir ad alanı varsa ve Azure AD 'nin avantajlarından yararlanmak istiyorsanız, karma Azure AD 'ye katılmış cihazları uygulayabilirsiniz. Bu cihazlar, hem şirket içi Active Directory hem de Azure Active Directory birleştirilir.
* **Cihaz geri yazma**: cihazların AD FS (2012 R2 veya üzeri) korunan cihazlara göre koşullu erişimi etkinleştirmek için cihaz geri yazma kullanılır

## <a name="configure-device-options-in-azure-ad-connect"></a>Azure AD Connect cihaz seçeneklerini yapılandırma

1.  Azure AD Connect çalıştırın. **Ek görevler** sayfasında **cihaz seçeneklerini yapılandır**' ı seçin.  **İleri**’ye tıklayın.
    ![Cihaz seçeneklerini yapılandır](./media/how-to-connect-device-options/deviceoptions.png) 

    **Genel bakış** sayfasında ayrıntılar görüntülenir.
    ![Genel bakış](./media/how-to-connect-device-options/deviceoverview.png)

    >[!NOTE]
    > Yeni cihaz yapılandırma seçenekleri yalnızca sürüm 1.1.819.0 ve daha yeni sürümlerde kullanılabilir.

2.  Azure AD kimlik bilgilerini sağladıktan sonra, cihaz seçenekleri sayfasında gerçekleştirilecek işlemi seçebilirsiniz.
    ![Cihaz işlemleri](./media/how-to-connect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Karma Azure AD birleştirmesini yapılandırma](../devices/hybrid-azuread-join-plan.md)
* [Cihaz geri yazmayı yapılandırma/devre dışı bırakma](how-to-connect-device-writeback.md)