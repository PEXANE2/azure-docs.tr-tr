---
title: Azure Active Directory B2C 'de uzantılar uygulaması | Microsoft Docs
description: B2C-Extensions-App ' i geri yükleme.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/06/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e5d38d8d791c6b932d8a28a898f2e2b80caff7ac
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846971"
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: uzantılar uygulaması

Azure AD B2C bir dizin oluşturulduğunda `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` adlı bir uygulama yeni dizin içinde otomatik olarak oluşturulur. **B2C-Extensions-App**olarak anılan bu uygulama *uygulama kayıtları*görünür. Kullanıcılar ve özel öznitelikler hakkında bilgi depolamak için Azure AD B2C hizmeti tarafından kullanılır. Uygulama silinirse, Azure AD B2C düzgün çalışmayacaktır ve üretim ortamınız etkilenecektir.

> [!IMPORTANT]
> Kiracınızı hemen silmeyi planlamadığınız müddetçe B2C-Extensions-App ' i silmeyin. Uygulama 30 günden uzun bir süre sonra silinirse, Kullanıcı bilgileri kalıcı olarak kaybedilir.

## <a name="verifying-that-the-extensions-app-is-present"></a>Uzantı uygulamasının mevcut olduğu doğrulanıyor

B2C-Extensions-uygulamasının mevcut olduğunu doğrulamak için:

1. Azure AD B2C kiracınızda, sol taraftaki gezinti menüsünde **tüm hizmetler** ' e tıklayın.
1. **Uygulama kayıtları**arayın ve açın.
1. **B2C-Extensions-App** ile başlayan bir uygulama arayın

## <a name="recover-the-extensions-app"></a>Uzantılar uygulamasını kurtarma

B2C-Extensions-App ' i yanlışlıkla sildiyseniz, bu uygulamayı kurtarmak için 30 gününüz vardır. Graph API kullanarak uygulamayı geri yükleyebilirsiniz:

1. [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/)gidin.
1. İçin silinen uygulamayı geri yüklemek istediğiniz Azure AD B2C dizin için genel yönetici olarak sitede oturum açın. Bu genel yöneticinin aşağıdakine benzer bir e-posta adresi olmalıdır: `username@{yourTenant}.onmicrosoft.com`.
1. API-Version = 1.6 ile `https://graph.windows.net/myorganization/deletedApplications` URL 'ye karşı HTTP GET verme. Bu işlem, son 30 gün içinde silinmiş tüm uygulamaların listesini alacak.
1. Adın ' B2C-Extension-App ' ile başladığı listede uygulamayı bulun ve `objectid` özellik değerini kopyalayın.
1. URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`için bir HTTP GÖNDERISI yayınlayın. URL 'nin `{OBJECTID}` bölümünü önceki adımdaki `objectid` değiştirin.

Artık Azure portal [geri yüklenen uygulamayı görebilmelisiniz](#verifying-that-the-extensions-app-is-present) .

> [!NOTE]
> Bir uygulama yalnızca son 30 gün içinde siliniyorsa geri yüklenebilir. 30 günden daha uzun bir süredir, veriler kalıcı olarak kaybedilir. Daha fazla yardım için bir destek bileti dosyası yapın.
