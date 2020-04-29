---
title: include dosyası
description: ön koşul içeren dosyayı Ekle
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 66b5f8e2-e08d-43c8-b460-6204aecda8f7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.custom: include file
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: f7b1b294e9500ef9e0aadd24cfe3cd4e61fddda1
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "68426971"
---
> [!IMPORTANT]
> **Bu makaledeki görevleri tamamlamadan önce Azure AD Domain Services için Parola karması eşitlemesini etkinleştirin.**
>
> Azure AD dizininizde bulunan kullanıcıların türüne bağlı olarak aşağıdaki yönergeleri izleyin. Azure AD dizininizde yalnızca bulutta ve eşitlenmiş Kullanıcı hesaplarının bir karışımına sahipseniz her iki yönerge kümesini de doldurun. Bir B2B Konuk hesabı (örneğin, izin verdiğimiz farklı bir kimlik sağlayıcısından Gmail veya MSA) kullanmaya çalışıyorsanız aşağıdaki işlemleri gerçekleştiremeyebilirsiniz, çünkü bu kullanıcılar, dizinde Konuk hesaplardır ve bu, yönetilen etki alanı ile eşitlenen bu kullanıcılara yönelik parolaya sahip değildir. Bu hesaplarla ilgili olarak, parolaları da dahil olmak üzere tüm bilgiler Azure AD 'nin dışında ve bu bilgiler Azure AD 'de olmadığından, yönetilen etki alanıyla eşitlenmeyebilir. 
> - [Yalnızca bulut Kullanıcı hesapları için yönergeler](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync.md)
> - [Şirket içi dizinden eşitlenen Kullanıcı hesapları için yönergeler](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync-synced-tenant.md)
