---
title: Azure AD Connect'te eşitleme kuralı nasıl özelleştirilir | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect'i yüklerken sorunları nasıl gidereceklerine yönelik adımlar sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a65d4c477d0e3aa9d5feea53e3e667ece651c83f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60351077"
---
# <a name="how-to-customize-a-synchronization-rule"></a>Eşitleme kuralını özelleştirme

## <a name="recommended-steps"></a>**Önerilen Adımlar**

Yeni bir eşitleme kuralını yönetmek veya oluşturmak için eşitleme kuralı düzenleyicisini kullanabilirsiniz. Eşitleme kurallarında değişiklik yapmak için gelişmiş bir kullanıcı olmanız gerekir. Herhangi bir yanlış değişiklik, nesnelerin hedef dizininizden silinmesine neden olabilir. Senkronizasyon kurallarında uzmanlık kazanmak için lütfen [Önerilen Belgeler'i](#recommended-documents) okuyun. Eşitleme kuralını değiştirmek için aşağıdaki adımlardan geçin:

* Masaüstündeki uygulama menüsünden senkronizasyon düzenleyicisini aşağıda gösterildiği gibi başlatın:

    ![Senkronizasyon Kural Düzenleyicisi Menüsü](media/how-to-connect-create-custom-sync-rule/how-to-connect-create-custom-sync-rule/syncruleeditormenu.png)

* Varsayılan eşitleme kuralını özelleştirmek için, standart varsayılan kuralın bir kopyasını oluşturacak ve devre dışı bırakan Eşitleme Kuralları Düzenleyicisi'ndeki "Düzenle" düğmesini tıklatarak varolan kuralı kopyalayın. Klonlanan kuralı 100'den az bir öncelikle kaydedin.  Öncelik, öznitelik akışı çakışması varsa, hangi kuralın (daha düşük sayısal değer) bir çakışma çözümlemesi kazanmasını belirler.

    ![Eşitleme Kuralı Düzenleyicisi](media/how-to-connect-create-custom-sync-rule/how-to-connect-create-custom-sync-rule/clonerule.png)

* Belirli bir özniteliği değiştirirken, ideal olarak yalnızca klonlanmış kuralda değişiklik özniteliği tutmanız gerekir.  Ardından, değiştirilmiş öznitelik klonlanmış kuraldan gelmesi ve diğer özniteliklerin varsayılan standart kuralından seçilmesi için varsayılan kuralı etkinleştirin. 

* Değiştirilen özniteliğin hesaplanan değerinin klonlanmış kuralınızda NULL olması ve varsayılan standart kuralında NULL olmaması durumunda, NULL olmayan değerin kazanacağını ve NULL değerinin yerini alacağı nızı lütfen unutmayın. NULL değerinin NULL olmayan bir değerle değiştirilmesini istemiyorsanız, klonlanmış kuralınıza YetkiliNull'u atayın.

* **Giden** kuralı değiştirmek için, eşitleme kuralı düzenleyicisinden filtreyi değiştirin.

## <a name="recommended-documents"></a>**Önerilen Belgeler**
* [Azure AD Connect eşitleme: Teknik Kavramlar](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-technical-concepts)
* [Azure AD Connect eşitlemi: Mimariyi anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)
* [Azure AD Connect eşitlemi: Bildirimsel Sağlama'yı Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning)
* [Azure AD Connect eşitlemi: Bildirimsel Sağlama İfadelerini Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning-expressions)
* [Azure AD Connect eşitleme: Varsayılan yapılandırmayı anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-default-configuration)
* [Azure AD Connect eşitleme: Kullanıcıları, Grupları ve Kişileri Anlama](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts)
* [Azure AD Connect eşitleme: Gölge öznitelikleri](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-syncservice-shadow-attributes)

## <a name="next-steps"></a>Sonraki Adımlar
- [Azure AD Connect eşitleme.](how-to-connect-sync-whatis.md)
- [Melez kimlik nedir?](whatis-hybrid-identity.md)
