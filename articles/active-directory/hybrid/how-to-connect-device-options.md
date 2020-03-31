---
title: 'Azure AD Connect: Aygıt seçenekleri | Microsoft Dokümanlar'
description: Bu belge, Azure AD Connect'te bulunan aygıt seçeneklerini ayrıntılarıyla birlikte
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
ms.topic: conceptual
ms.date: 09/13/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 96ddcdb67ef079cfa23902a1dcb03b0ec61077fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67109536"
---
# <a name="azure-ad-connect-device-options"></a>Azure AD Bağlantısı: Aygıt seçenekleri

Aşağıdaki belgeler, Azure AD Connect'te bulunan çeşitli aygıt seçenekleri hakkında bilgi sağlar. Aşağıdaki iki işlemi yapılandırmak için Azure AD Connect'i kullanabilirsiniz: 
* **Karma Azure AD join**: Ortamınızda şirket içi BIR AD ayak izi varsa ve Azure AD'nin avantajlarından yararlanmak istiyorsanız, karma Azure AD birleştirilmiş aygıtlar uygulayabilirsiniz. Bu aygıtlar hem şirket içi Active Dizininize hem de Azure Etkin Dizininize katılır.
* **Aygıt geri yazma**: Aygıt geri yazımı, aygıtlara dayalı Koşullu Erişimi AD FS (2012 R2 veya daha yüksek) korumalı aygıtlara etkinleştirmek için kullanılır

## <a name="configure-device-options-in-azure-ad-connect"></a>Azure AD Connect'te aygıt seçeneklerini yapılandırma

1.  Azure AD Connect'i çalıştırın. Ek **görevler** sayfasında, **aygıtı yapılandır seçeneklerini**seçin.  **İleri**'ye tıklayın.
    ![Aygıt seçeneklerini yapılandırma](./media/how-to-connect-device-options/deviceoptions.png) 

    **Genel Bakış** sayfası ayrıntıları görüntüler.
    ![Genel bakış](./media/how-to-connect-device-options/deviceoverview.png)

    >[!NOTE]
    > Yeni Yapılandırma aygıtı seçenekleri yalnızca sürüm 1.1.819.0 ve daha yeni sürümlerde kullanılabilir.

2.  Azure AD kimlik bilgilerini sağladıktan sonra, Aygıt seçenekleri sayfasında gerçekleştirilecek işlemi seçebilirsiniz.
    ![Cihaz işlemleri](./media/how-to-connect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Karma Azure AD birleştirme yapılandırma](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Aygıt yazmayı yapılandırma / devre dışı bırakın](how-to-connect-device-writeback.md)

