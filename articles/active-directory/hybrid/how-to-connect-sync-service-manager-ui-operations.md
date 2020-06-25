---
title: Azure AD Connect Synchronization Service Manager Işlemleri | Microsoft Docs
description: Azure AD Connect için Synchronization Service Manager Işlemler sekmesini anlayın.
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
ms.topic: how-to
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64ab5901ad54e37fc05d7da757ce7f31d42fe3b6
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85357112"
---
# <a name="using-the-sync-service-manager-operations-tab"></a>Eşitleme Service Manager Işlemler sekmesini kullanma

![Eşitleme Service Manager](./media/how-to-connect-sync-service-manager-ui-operations/operations.png)

İşlemler sekmesi en son işlemlerin sonuçlarını gösterir. Bu sekme, sorunları anlamak ve sorunlarını gidermek için önemli bir anahtardır.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>İşlemler sekmesinde görünen bilgileri anlayın
Üstteki yarım, tüm çalıştırmaları kronolojik sırayla gösterir. Varsayılan olarak, işlem günlüğü son yedi güne ilişkin bilgileri tutar, ancak bu ayar [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md)ile değiştirilebilir. Başarı durumu gösterolmayan herhangi bir çalıştırmaya bakmak istiyorsunuz. Üst bilgiye tıklayarak sıralamayı değiştirebilirsiniz.

**Durum** sütunu en önemli bilgiler ve bir çalıştırma için en ciddi sorunu gösterir. Aşağıda, araştırma önceliği sırasına göre en yaygın durumların hızlı bir özeti verilmiştir (burada * birkaç olası hata dizesini gösterir).

| Durum | Yorum |
| --- | --- |
| durdurulacağını\* |Çalıştırma tamamlanamadı. Örneğin, uzak sistem kapalıysa ve bu bağlantı kurulamadığı takdirde. |
| durduruldu-hata-limit |5.000 'den fazla hata var. Çok sayıda hata nedeniyle çalıştırma otomatik olarak durduruldu. |
| tamamlandı- \* -hatalar |Çalıştırma tamamlandı, ancak Araştırılması gereken hatalar (5.000 'den az). |
| tamamlandı- \* -Uyarılar |Çalıştırma tamamlandı, ancak bazı veriler beklenen durumda değil. Hatalar varsa, bu ileti genellikle yalnızca bir belirtidir. Hataları giderene kadar uyarıları araştırmamalıdır. |
| başarılı |Sorun yok. |

Bir satırı seçtiğinizde, o çalıştırmanın ayrıntılarını göstermek için en alttaki güncelleştirmeler. En alttaki, **# adımını**belirten bir listeniz olabilir. Bu liste yalnızca, ormanınızda her etki alanının bir adımla temsil edildiği birden fazla etki alanınız varsa görüntülenir. Etki alanı adı, başlık **bölümünün**altında bulunabilir. **Eşitleme istatistikleri**altında, işlenen değişiklik sayısı hakkında daha fazla bilgi edinebilirsiniz. Değiştirilen nesnelerin bir listesini almak için bağlantılara tıklayabilirsiniz. Hatalı nesneler varsa, bu hatalar **Eşitleme hataları**altında görünür.

Daha fazla bilgi için bkz. eşitlenmeyen [bir nesneyle ilgili sorunları giderme](tshoot-connect-object-not-syncing.md)

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırması hakkında daha fazla bilgi edinin.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
