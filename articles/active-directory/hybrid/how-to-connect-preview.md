---
title: 'Azure AD Connect: Önizlemedeki özellikler | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect'te önizlemede olan özellikleri daha ayrıntılı olarak açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7def733a80aea1be77825bb9069217f5f43e003
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261287"
---
# <a name="more-details-about-features-in-preview"></a>Önizlemedeki özellikler hakkında daha fazla bilgi
Bu konu, şu anda önizlemede bulunan özelliklerin nasıl kullanılacağını açıklar.

## <a name="group-writeback"></a>Grup geri yazma
İsteğe bağlı özelliklerde grup geri yazma seçeneği, **Office 365 Gruplarını** Exchange yüklü bir ormana geri yazmanızı sağlar. Bu, bulutta her zaman hakim olan bir gruptur. Şirket içinde Exchange varsa, bu grupları şirket içi Exchange posta kutusuna sahip kullanıcıların bu gruplardan e-posta gönderip alabilmeleri için şirket içi olarak geri yazabilirsiniz.

Office 365 Grupları ve bunların nasıl kullanılacağı hakkında daha fazla bilgiyi [burada](https://aka.ms/O365g)bulabilirsiniz.

Office 365 grubu, şirket içi AD DS'de dağıtım grubu olarak temsil edilir. Şirket içi Exchange sunucunuz, bu yeni grup türünü tanımak için Exchange 2013 kümülatif güncelleştirme 8 (Mart 2015'te yayımlandı) veya Exchange 2016'da olmalıdır.

**Önizleme sırasında notlar**

* Adres defteri özniteliği şu anda önizlemede doldurulmadı. Bu öznitelik olmadan, grup GAL görünür değildir. Bu özelliği doldurmak için en kolay yolu Exchange PowerShell cmdlet `update-recipient`kullanmaktır.
* Sadece Exchange şeması olan ormanlar gruplar için geçerli hedeflerdir. Exchange algılanmadıysa, grup geri yazmayı etkinleştirmek mümkün değildir.
* Şu anda yalnızca tek ormanlı Exchange kuruluş dağıtımları desteklenir. Şirket içinde birden fazla Exchange kuruluşunuz varsa, bu grupların diğer ormanlarınızda görünmesi için şirket içi GALSync çözümüne ihtiyacınız vardır.
* Grup geri yazma özelliği güvenlik gruplarını veya dağıtım gruplarını işlemez.

> [!NOTE]
> Grup geri yazımı için Azure AD Premium aboneliği gereklidir.
> 
>

## <a name="user-writeback"></a>Kullanıcı geri yazma
> [!IMPORTANT]
> Kullanıcı geri yazma önizleme özelliği, Ağustos 2015 güncelleştirmesinde Azure AD Connect'e kaldırıldı. Etkinleştirdiyseniz, bu özelliği devre dışı bmelisiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Azure [AD Connect'in Özel yüklemenize devam edin.](how-to-connect-install-custom.md)

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
