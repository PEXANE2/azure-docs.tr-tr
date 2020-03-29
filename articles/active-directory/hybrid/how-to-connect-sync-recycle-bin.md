---
title: Azure AD Connect eşitlemesini etkinleştirme AD geri dönüşüm kutusu | Microsoft Dokümanlar
description: Bu konu, Azure AD Connect ile AD Geri Dönüşüm Kutusu özelliğinin kullanılmasını önerir.
services: active-directory
keywords: AD Geri Dönüşüm Kutusu, yanlışlıkla silme, kaynak çapa
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/17/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fe7d3ea7d4f6d648438efc1a484d5909ade2f23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60382904"
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Azure AD Connect eşitlemesini etkinleştirme AD geri dönüşüm kutusu
Şirket içi Etkin Dizinleriniz için Azure AD ile senkronize edilen AD Geri Dönüşüm Kutusu özelliğini etkinleştirmeniz önerilir. 

Bir şirket içi AD kullanıcı nesnesini yanlışlıkla sildiyseniz ve özelliği kullanarak geri yüklediyseniz, Azure AD ilgili Azure AD kullanıcı nesnesini geri yükler.  AD Geri Dönüşüm Kutusu özelliği hakkında bilgi için, [Silinen Etkin Dizin Nesnelerini Geri Getirmek için](https://technet.microsoft.com/library/dd379542.aspx)Makale Senaryogenel Bakış'a bakın.

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>AD geri dönüşüm kutusu etkinleştirme yararları
Bu özellik, aşağıdakileri yaparak Azure AD kullanıcı nesnelerini geri yardımcı olur:

* Şirket içinde bir AD kullanıcı nesnesini yanlışlıkla sildiyseniz, ilgili Azure AD kullanıcı nesnesi bir sonraki eşitleme döngüsünde silinir. Varsayılan olarak, Azure AD silinen Azure AD kullanıcı nesnesini 30 gün boyunca yumuşak silinmiş durumda tutar.

* Şirket içinde AD Geri Dönüşüm Kutusu özelliği etkinse, silinen şirket içi AD kullanıcı nesnesini Kaynak Bağlantı değerini değiştirmeden geri yükleyebilirsiniz. Kurtarılan şirket içi AD kullanıcı nesnesi Azure AD ile eşitlendiğinde, Azure AD ilgili yumuşak silinmiş Azure AD kullanıcı nesnesini geri yükler. Kaynak Çapa özniteliği hakkında daha fazla bilgi için [Azure AD Connect: Tasarım kavramları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor)makalesine bakın.

* Şirket içi AD Geri Dönüşüm Kutusu özelliği etkinleştirmediyseniz, silinen nesneyi değiştirmek için bir AD kullanıcı nesnesi oluşturmanız gerekebilir. Azure AD Connect Eşitleme Hizmeti, Kaynak Bağlantı özelliği için sistem tarafından oluşturulan AD özniteliğini (ObjectGuid gibi) kullanacak şekilde yapılandırılırsa, yeni oluşturulan AD kullanıcı nesnesi silinen AD kullanıcı nesnesi ile aynı Kaynak Bağlantı değerine sahip olmaz. Yeni oluşturulan AD kullanıcı nesnesi Azure AD ile eşitlendiğinde, Azure AD yumuşak silinmiş Azure AD kullanıcı nesnesini geri getirmek yerine yeni bir Azure AD kullanıcı nesnesi oluşturur.

> [!NOTE]
> Varsayılan olarak, Azure AD silinmiş Azure AD kullanıcı nesnelerini kalıcı olarak silinmeden önce 30 gün boyunca yumuşak silinmiş durumda tutar. Ancak, yöneticiler bu tür nesnelerin silinmesini hızlandırabilir. Nesneler kalıcı olarak silindikten sonra, şirket içi AD Geri Dönüşüm Kutusu özelliği etkinleştirilse bile artık kurtarılamaz.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
