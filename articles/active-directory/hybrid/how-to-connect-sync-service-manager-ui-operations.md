---
title: Azure AD Bağlama Senkronizasyon Servis Yöneticisi İşlemleri | Microsoft Dokümanlar
description: Azure AD Connect için Eşitleme Hizmet Yöneticisi'ndeki İşlemler sekmesini anlayın.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 474000d1d4d7e1358682d1421125d482e3782049
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60381422"
---
# <a name="using-the-sync-service-manager-operations-tab"></a>Eşitleme Hizmeti Yöneticisi İşlemleri sekmesini kullanma

![Eşitleme Servis Yöneticisi](./media/how-to-connect-sync-service-manager-ui-operations/operations.png)

İşlemler sekmesi, en son işlemlerin sonuçlarını gösterir. Bu sekme, sorunları anlamak ve sorun gidermenin anahtarıdır.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>İşlemler sekmesinde görünen bilgileri anlama
Üst yarısında tüm kronolojik sırada çalışır gösterir. Varsayılan olarak, işlem günlüğü son yedi gün hakkında bilgi tutar, ancak bu ayar [zamanlayıcı](how-to-connect-sync-feature-scheduler.md)ile değiştirilebilir. Başarı durumunu göstermeyen herhangi bir çalıştırmayı aramak istiyorsunuz. Üstbilgi tıklayarak sıralama değiştirebilirsiniz.

**Durum** sütunu en önemli bilgilerdir ve bir çalışma için en ciddi sorunu gösterir. Burada araştırmak için öncelik sırasına göre en yaygın durumların hızlı bir özeti (burada * birkaç olası hata dizeleri gösterir).

| Durum | Açıklama |
| --- | --- |
| durdu-\* |Çalışma tamamlayamadı. Örneğin, uzak sistem çöktüyse ve bağlantı kurulamıyorsa. |
| durmuş hata sınırı |5.000'den fazla hata var. Çok sayıda hata nedeniyle çalıştırma otomatik olarak durduruldu. |
| tamamlanmış-\*-hatalar |Çalışma tamamlandı, ancak araştırılması gereken hatalar (5.000'den az) vardır. |
| tamamlanmış\*uyarılar |Çalışma tamamlandı, ancak bazı veriler beklenen durumda değil. Hatalarınız varsa, bu ileti genellikle yalnızca bir belirtidir. Hataları ele alana kadar uyarıları araştırmamalısınız. |
| başarılı |Sorun yok. |

Bir satır seçtiğinizde, bu çalıştırmanın ayrıntılarını göstermek için alt güncelleştirmeler. En solunda **Adım #** yazan bir listen olabilir. Bu liste yalnızca ormanınızda her etki alanının bir adımla temsil edildiği birden çok etki alanınız varsa görünür. Etki alanı adı **Bölüm**başlığı altında bulunabilir. **Eşitleme İstatistikleri**altında, işlenen değişikliklerin sayısı hakkında daha fazla bilgi bulabilirsiniz. Değiştirilen nesnelerin listesini almak için bağlantıları tıklatabilirsiniz. Hataları olan nesnelerinizin varsa, bu hatalar **Eşitleme Hataları**altında gösteriş gösterir.

Daha fazla bilgi için, [eşitleme olmayan bir nesneyi giderme](tshoot-connect-object-not-syncing.md)

## <a name="next-steps"></a>Sonraki adımlar
Azure AD [Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırması hakkında daha fazla bilgi edinin.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
