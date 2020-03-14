---
title: 'Azure AD Connect: Önizlemedeki Özellikler | Microsoft Docs'
description: Bu konuda, Azure AD Connect önizleme aşamasında olan daha ayrıntılı özellikler açıklanmaktadır.
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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79261287"
---
# <a name="more-details-about-features-in-preview"></a>Önizlemedeki özellikler hakkında daha fazla ayrıntı
Bu konuda, şu anda önizleme aşamasında olan özelliklerin nasıl kullanılacağı açıklanmaktadır.

## <a name="group-writeback"></a>Grup geri yazma
İsteğe bağlı özelliklerde grup geri yazma seçeneği, Exchange yüklü bir ormana **Office 365 gruplarını** geri almanıza olanak sağlar. Bu, bulutta her zaman ana kopyalı bir gruptur. Şirket içi Exchange kullanıyorsanız, şirket içi Exchange posta kutusu olan kullanıcıların bu gruplardan e-posta gönderip alabilmesi için bu grupları şirket içinde geri yazabilirsiniz.

Office 365 grupları ve bunların nasıl kullanılacağı hakkında daha fazla bilgi [burada](https://aka.ms/O365g)bulunabilir.

Office 365 Grubu, şirket içi AD DS dağıtım grubu olarak temsil edilir. Şirket içi Exchange Server, bu yeni grup türünü tanımak için Exchange 2013 toplu güncelleştirme 8 ' de (2015 Mart 'ta yayımlanmıştır) veya Exchange 2016 ' de olmalıdır.

**Önizleme sırasında Notlar**

* Adres defteri özniteliği şu anda önizlemede doldurulmamış. Bu öznitelik olmadan grup, GAL 'nda görünmez. Bu özniteliği doldurmanın en kolay yolu, `update-recipient`Exchange PowerShell cmdlet 'ini kullanmaktır.
* Yalnızca Exchange şemasına sahip ormanlar gruplar için geçerli hedeflerdir. Exchange algılanmadığında, grup geri yazma özelliğinin etkinleştirilmesi mümkün değildir.
* Şu anda yalnızca tek ormanlı Exchange kuruluşu dağıtımları desteklenmektedir. Şirket içinde birden fazla Exchange kuruluşunuz varsa, bu grupların diğer ormanlarınızdan görünmesi için bir şirket içi GALSync çözümüne ihtiyacınız vardır.
* Grup geri yazma özelliği güvenlik gruplarını veya dağıtım gruplarını işlemez.

> [!NOTE]
> Grup geri yazma için Azure AD Premium bir abonelik gereklidir.
> 
>

## <a name="user-writeback"></a>Kullanıcı geri yazma
> [!IMPORTANT]
> Kullanıcı geri yazma önizleme özelliği Azure AD Connect için Ağustos 2015 güncelleştirmesinde kaldırılmıştır. Bunu etkinleştirdiyseniz, bu özelliği devre dışı bırakmanız gerekir.
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Connect özel yüklemenize](how-to-connect-install-custom.md)devam edin.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
