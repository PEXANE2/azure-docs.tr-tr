---
title: 'Azure AD Connect eşitlemi: Teknik kavramlar | Microsoft Dokümanlar'
description: Azure AD Connect eşitlemesinin teknik kavramlarını açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60347587"
---
# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect Eşitleme: Teknik Kavramlar
Bu makale, [anlama](how-to-connect-sync-technical-concepts.md)mimarisinin bir özetidir.

Azure AD Connect eşitleme, sağlam bir metadizisenkronizasyon platformu üzerine kuruludur.
Aşağıdaki bölümlerde metadirectory senkronizasyon kavramları tanıtın.
MIIS, ILM ve FIM'den oluşan Azure Active Directory Sync Services, veri kaynaklarına bağlanmak, veri kaynakları arasında verileri eşitlemenin yanı sıra kimliklerin sağlanması ve bunların kaldırılması için bir sonraki platformu sağlar.

![Teknik Kavramlar](./media/how-to-connect-sync-technical-concepts/scenario.png)

Aşağıdaki bölümler, FIM Eşitleme Hizmeti'nin aşağıdaki yönleri hakkında daha fazla ayrıntı sağlar:

* Bağlayıcı
* Öznitelik akışı
* Konektör alanı
* Metaverse
* Sağlama

## <a name="connector"></a>Bağlayıcı
Bağlı bir dizinle iletişim kurmak için kullanılan kod modüllerine bağlayıcılar (eski yönetim aracıları (MAs) olarak bilinir.

Bunlar, Azure AD Connect eşitlemi çalıştıran bilgisayara yüklenir. Bağlayıcılar, özel aracıların dağıtımına güvenmek yerine uzak sistem protokollerini kullanarak aracısız sohbet etme olanağı sağlar. Bu, özellikle kritik uygulamalar ve sistemlerle uğraşırken risk ve dağıtım sürelerinin azalması anlamına gelir.

Yukarıdaki resimde, bağlayıcı konektör alanı ile eşanlamlıdır ancak dış sistemle tüm iletişimi kapsar.

Bağlayıcı, sisteme tüm alma ve dışa aktarma işlevlerinden sorumludur ve geliştiricilerin veri dönüşümlerini özelleştirmek için bildirimsel sağlama yı kullanırken her sisteme nasıl yerel olarak bağlanacaklarını anlamaları gerektiğini anlamalarını sağlar.

İthalat ve dışa aktarımlar yalnızca zamanlandığında gerçekleşir ve değişiklikler bağlı veri kaynağına otomatik olarak yayılmadığından, sistem içinde meydana gelen değişikliklerden daha fazla yalıtım sağlar. Buna ek olarak, geliştiriciler hemen hemen her veri kaynağına bağlanmak için kendi bağlayıcılarını da oluşturabilirler.

## <a name="attribute-flow"></a>Öznitelik akışı
Metaverse komşu bağlayıcı alanlardan tüm birleştirilmiş kimliklerin konsolide görünümüdür. Yukarıdaki şekilde öznitelik akışı, hem gelen hem de giden akış için ok başları olan çizgilerle gösterilmiştir. Öznitelik akışı, verileri bir sistemden diğerine kopyalama veya dönüştürme işlemidir ve tüm öznitelik akışları (gelen veya giden) olur.

Eşitleme (tam veya delta) işlemleri çalıştırmak için zamanlandığında bağlayıcı boşluk ve metaverse çift yönlü arasında öznitelik akışı oluşur.

Öznitelik akışı yalnızca bu eşitlemeler çalıştırıldığında oluşur. Öznitelik akışları Eşitleme Kuralları'nda tanımlanır. Bunlar gelen (yukarıdaki resimde ISR) veya giden (yukarıdaki resimde OSR) olabilir.

## <a name="connected-system"></a>Bağlı sistem
Bağlı sistem (aka bağlı dizin) uzak sistem Azure AD Connect senkronizasyon bağlı ve okuma ve kimlik verileri ve gelen kimlik verileri yazıyor atıfta bulunuyor.

## <a name="connector-space"></a>Konektör alanı
Bağlı her veri kaynağı, bağlayıcı alanındaki nesnelerin ve özniteliklerin filtrelenmiş bir alt kümesi olarak temsil edilir.
Bu, nesneleri eşitlerken uzak sisteme dokunmaya gerek kalmadan eşitleme hizmetinin yerel olarak çalışmasını sağlar ve etkileşimi yalnızca içeri alma ve dışa aktarımlarla sınırlandırırlar.

Veri kaynağı ve bağlayıcı değişikliklerin bir listesini (delta alma) sağlama yeteneğine sahip olduğunda, son yoklama döngüsü değiş tokuş edildiğinden itibaren yalnızca değişiklikler arttıkça operasyonel verimlilik önemli ölçüde artar. Bağlayıcı alanı, bağlayıcının içeri alma ve dışa aktarma zamanlamasını gerektirerek bağlı veri kaynağını otomatik olarak yayılan değişikliklerden yalıtır. Bu ek sigorta, bir sonraki güncelleştirmeyi test ederken, önizlerken veya onaylarken size huzur verir.

## <a name="metaverse"></a>Metaverse
Metaverse komşu bağlayıcı alanlardan tüm birleştirilmiş kimliklerin konsolide görünümüdür.

Kimlikler birbirine bağlandıkça ve alma akışı eşlemeleri aracılığıyla çeşitli öznitelikler için yetki atandıkça, merkezi metaverse nesnebirden fazla sistemden bilgi toplama başlar. Bu nesne öznitelik akışından, eşlemeler giden sistemlere bilgi taşır.

Nesneler, yetkili bir sistem bunları metaverse'e dönüştürdüklerinde oluşturulur. Tüm bağlantılar kaldırılır kaldırılmaz, metaverse nesne silir.

Metaverse nesneleri doğrudan düzenlenemez. Nesnedeki tüm veriler öznitelik akışı yoluyla katkıda bulunulmalıdır. Metaverse her bağlayıcı alanı ile kalıcı konektörler tutar. Bu bağlayıcılar, her eşitleme çalışması için yeniden değerlendirme gerektirmez. Bu, Azure AD Connect eşitlemesi eşleşen uzak nesneyi her seferinde bulmak zorunda olmadığı anlamına gelir. Bu, normalde nesnelerin ilişkilendirilmesinden sorumlu olacak özniteliklerde değişiklikleri önlemek için pahalı aracılar için ihtiyaç önler.

Azure AD Connect eşitlenmesi, yönetilmesi gereken önceden varolan nesnelere sahip olabilecek yeni veri kaynakları keşfederken, bağlantı kuracak potansiyel adayları değerlendirmek için birleştirme kuralı adı verilen bir işlem kullanır.
Bağlantı kurulduktan sonra, bu değerlendirme yeniden oluşmaz ve uzak bağlı veri kaynağı ile metaverse arasında normal öznitelik akışı oluşabilir.

## <a name="provisioning"></a>Sağlama
Yetkili bir kaynak metaverse içine yeni bir nesne oluşturduğunda, akış aşağı bağlı bir veri kaynağını temsil eden başka bir Bağlayıcı'da yeni bir bağlayıcı uzay nesnesi oluşturulabilir.

Bu doğal olarak bir bağlantı kurar ve öznitelik akışı iki yönlü olarak devam edebilir.

Bir kural yeni bir bağlayıcı alanı nesnesi oluşturulması gerektiğini belirlerse, buna sağlama denir. Ancak, bu işlem yalnızca bağlayıcı alanı içinde gerçekleştiğinden, bir dışa aktarma gerçekleştirilir kadar bağlı veri kaynağına taşımaz.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure AD Connect Eşitleme: Senkronizasyon seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
