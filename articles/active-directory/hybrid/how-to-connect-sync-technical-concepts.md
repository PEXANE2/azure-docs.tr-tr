---
title: 'Azure AD Connect eşitleme: teknik kavramlar | Microsoft Docs'
description: Azure AD Connect eşitleme 'nin teknik kavramlarını açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 731cfeb3-beaf-4d02-aef4-b02a8f99fd11
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8ec4a6100cfbb4419d7e30f4b97589113b88939
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "60347587"
---
# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect Eşitleme: Teknik Kavramlar
Bu makale, [mimariyi anlama](how-to-connect-sync-technical-concepts.md)konusunun bir özetidir.

Azure AD Connect derlemeleri sağlam bir Metadirectory eşitleme platformunda eşitler.
Aşağıdaki bölümlerde Metadirectory eşitlemesine yönelik kavramlar tanıtılmaktadır.
MıSıS, ıLA ve FIM üzerine derleme Azure Active Directory Eşitleme Hizmetleri, veri kaynaklarına bağlanmak, veri kaynakları arasında verilerin eşitlenmesi ve kimliklerin sağlanması ve sağlanması için bir sonraki platformu sağlar.

![Teknik Kavramlar](./media/how-to-connect-sync-technical-concepts/scenario.png)

Aşağıdaki bölümlerde FIM eşitleme hizmetinin aşağıdaki yönleri hakkında daha fazla bilgi sağlanmaktadır:

* Bağlayıcı
* Öznitelik akışı
* Bağlayıcı alanı
* Meta veri deposu
* Sağlama

## <a name="connector"></a>Bağlayıcı
Bağlı bir dizinle iletişim kurmak için kullanılan kod modüllerine bağlayıcılar (eski adıyla yönetim aracıları (MAs)) denir.

Bunlar, Azure AD Connect Sync çalıştıran bilgisayara yüklenir. Bağlayıcılar, özel aracıların dağıtımına güvenmek yerine uzak sistem protokollerini kullanarak, aracısız bir özellik sunar. Bu, özellikle kritik uygulamalar ve sistemlerle ilgilenirken risk ve dağıtım sürelerinin düşürüldüğü anlamına gelir.

Yukarıdaki resimde bağlayıcı, bağlayıcı alanıyla eşanlamlıdır ancak dış sistemle tüm iletişimi kapsar.

Bağlayıcı, sisteme tüm içeri ve dışarı aktarma işlevlerinden sorumludur ve geliştiricilerin veri dönüştürmelerini özelleştirmek için bildirim temelli sağlama kullanırken her bir sisteme yerel olarak nasıl bağlanacağınızı anlamalarına gerek duymasını sağlar.

İçeri aktarmalar ve dışarı aktarmalar yalnızca zamanlandığı zaman oluşur ve değişiklikler otomatik olarak bağlı veri kaynağına yayılmaz. Ayrıca, geliştiriciler neredeyse tüm veri kaynaklarına bağlanmak için kendi bağlayıcılarını da oluşturabilir.

## <a name="attribute-flow"></a>Öznitelik akışı
Meta veri deposu, komşu bağlayıcı alanlarında bulunan tüm birleştirilmiş kimliklerin birleştirilmiş görünümüdür. Yukarıdaki şekilde, öznitelik akışı hem gelen hem de giden akış için ok uçları olan satırlara göre gösterilmiştir. Öznitelik akışı, verileri bir sistemden diğerine ve tüm öznitelik akışlarına (gelen veya giden) kopyalama veya dönüştürme işlemidir.

Eşitleme (tam veya Delta) işlemleri çalıştırılmak üzere zamanlandığında bağlayıcı alanı ve meta veri deposu arasında öznitelik akışı oluşur.

Öznitelik akışı yalnızca bu eşitlemeler çalıştırıldığında oluşur. Öznitelik akışları eşitleme kurallarında tanımlanmıştır. Bunlar gelen (yukarıdaki resimde ıSR) veya giden (yukarıdaki resimde OSR) olabilir.

## <a name="connected-system"></a>Bağlı sistem
Bağlı sistem (diğer adıyla bağlı dizin), uzak sistem Azure AD Connect eşitleme 'nin kimlik verilerini ve üzerinden bağlanıp okuyup okumasını sağlar.

## <a name="connector-space"></a>Bağlayıcı alanı
Her bağlı veri kaynağı, bağlayıcı alanındaki nesnelerin ve özniteliklerin filtrelenmiş bir alt kümesi olarak temsil edilir.
Bu, Eşitleme hizmetinin nesneleri eşitlerken uzak sistemle iletişim kurmaya gerek olmadan yerel olarak çalışmasını sağlar ve etkileşimi yalnızca içeri ve dışarı aktarmaları ile sınırlar.

Veri kaynağı ve bağlayıcının bir değişiklikler listesi (bir Delta içeri aktarma) sağlayabilme özelliği varsa, işlemsel verimlilik önemli ölçüde, yalnızca son yoklama döngüsünden sonra yapılan değişiklikler olarak artar. Bağlayıcı alanı, bağlayıcı zamanlaması içeri ve dışarı aktarmaları gerekmeden, bağlı veri kaynağını otomatik olarak yayılan değişikliklerden yalıtılmış olarak düzenler. Bu eklenen sigorta, test etme, önizleme veya sonraki güncelleştirmeyi onaylama sırasında size rahat bir fikir verir.

## <a name="metaverse"></a>Meta veri deposu
Meta veri deposu, komşu bağlayıcı alanlarında bulunan tüm birleştirilmiş kimliklerin birleştirilmiş görünümüdür.

Kimlikler birlikte bağlanır ve içeri aktarma akış eşlemeleri aracılığıyla çeşitli öznitelikler için yetkili atandığında, merkezi metadize nesnesi birden fazla sistemden bilgi toplamaya başlar. Bu nesne özniteliği akışından, eşlemeler giden sistemlere bilgi taşır.

Bir yetkili sistem tarafından meta veri deposunda projeler oluşturulduğunda nesneler oluşturulur. Tüm bağlantılar kaldırıldıktan hemen sonra metadize nesnesi silinir.

Meta veri deposundaki nesneler doğrudan düzenlenemez. Nesnedeki tüm verilere öznitelik akışı aracılığıyla katkıda bulunulmalıdır. Metadize her bağlayıcı alanıyla kalıcı bağlayıcılar tutar. Bu bağlayıcılar her eşitleme çalıştırması için yeniden değerlendirme gerektirmez. Bu, Azure AD Connect eşitlemenin her seferinde eşleşen uzak nesneyi bulması gerekmediği anlamına gelir. Bu, pahalı aracıların, nesnelerin bağıntıdan sorumlu olması için normalde sorumlu olabilecek özniteliklerde değişiklik yapılmasını önler.

Eşitlenmesi gereken daha önceden varolan nesneleri bulunan yeni veri kaynaklarını keşfederken Azure AD Connect eşitleme, bir bağlantı kuracak potansiyel adayları değerlendirmek için JOIN kuralı adlı bir işlem kullanır.
Bağlantı kurulduktan sonra, bu değerlendirme yeniden gerçekleşmez ve uzak bağlantılı veri kaynağı ile meta veri deposu arasında normal öznitelik akışı gerçekleşemez.

## <a name="provisioning"></a>Sağlama
Bir yetkili Kaynak, meta veri deposuna yeni bir nesne, bir aşağı akış bağlantılı veri kaynağını temsil eden başka bir bağlayıcıda oluşturulabilir.

Bu, kendiliğinden bir bağlantı oluşturur ve öznitelik akışı çift yönlü olarak devam edebilir.

Her kural, yeni bir bağlayıcı alanı nesnesinin oluşturulması gerektiğini belirlediğinde, sağlama olarak adlandırılır. Ancak, bu işlem yalnızca bağlayıcı alanı içinde gerçekleştiğinden, bir dışa aktarma gerçekleşene kadar bağlı veri kaynağına taşınmaz.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure AD Connect eşitleme: eşitleme seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
