---
title: include dosyası
description: ön koşulları olan dosyayı dahil et
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68426971"
---
> [!IMPORTANT]
> **Bu makaledeki görevleri tamamlamadan önce Azure AD Etki Alanı Hizmetleri'ne parola karma senkronizasyonunu etkinleştirin.**
>
> Azure AD dizininizdeki kullanıcı türüne bağlı olarak aşağıdaki yönergeleri izleyin. Azure AD dizininizde yalnızca bulut ve senkronize kullanıcı hesaplarının bir karışımı varsa her iki yönerge kümesini de tamamlayın. B2B Guest hesabı kullanmaya çalışıyorsanız aşağıdaki işlemleri gerçekleştiremeyebilirsiniz (örneğin, gmail'iniz veya MSA'nız farklı bir Kimlik sağlayıcısından izin verdiğimiz) çünkü bu kullanıcıların parolası yönetilen etki alanına senkronize edilemedi. dizindeki konuk hesapları. Parolaları da dahil olmak üzere bu hesaplarla ilgili tüm bilgiler Azure AD'nin dışında olacaktır ve bu bilgiler Azure AD'de olmadığı için yönetilen etki alanıyla eşitlenmez bile. 
> - [Yalnızca bulut kullanıcı hesapları için talimatlar](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync.md)
> - [Şirket içi dizinden senkronize edilen kullanıcı hesapları için talimatlar](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync-synced-tenant.md)
