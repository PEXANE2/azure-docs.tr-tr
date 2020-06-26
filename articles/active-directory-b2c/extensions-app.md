---
title: Azure Active Directory B2C 'de uzantılar uygulaması | Microsoft Docs
description: B2C-Extensions-App ' i geri yükleme.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/06/2017
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: accd32f376c6030900a9f9a3c29547118d0a1a7b
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85388519"
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: uzantılar uygulaması

Azure AD B2C bir dizin oluşturulduğunda, adlı bir uygulama `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` Yeni dizin içinde otomatik olarak oluşturulur. **B2C-Extensions-App**olarak anılan bu uygulama *uygulama kayıtları*görünür. Kullanıcılar ve özel öznitelikler hakkında bilgi depolamak için Azure AD B2C hizmeti tarafından kullanılır. Uygulama silinirse, Azure AD B2C düzgün çalışmayacaktır ve üretim ortamınız etkilenecektir.

> [!IMPORTANT]
> Kiracınızı hemen silmeyi planlamadığınız müddetçe B2C-Extensions-App ' i silmeyin. Uygulama 30 günden uzun bir süre sonra silinirse, Kullanıcı bilgileri kalıcı olarak kaybedilir.

## <a name="verifying-that-the-extensions-app-is-present"></a>Uzantı uygulamasının mevcut olduğu doğrulanıyor

B2C-Extensions-uygulamasının mevcut olduğunu doğrulamak için:

1. Azure AD B2C kiracınızda, sol taraftaki gezinti menüsünde **tüm hizmetler** ' e tıklayın.
1. **Uygulama kayıtları**arayın ve açın.
1. **B2C-Extensions-App** ile başlayan bir uygulama arayın

## <a name="recover-the-extensions-app"></a>Uzantılar uygulamasını kurtarma

B2C-Extensions-App ' i yanlışlıkla sildiyseniz, bu uygulamayı kurtarmak için 30 gününüz vardır. Graph API kullanarak uygulamayı geri yükleyebilirsiniz:

1. Öğesine gidin [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/) .
1. İçin silinen uygulamayı geri yüklemek istediğiniz Azure AD B2C dizin için genel yönetici olarak sitede oturum açın. Bu genel yöneticinin aşağıdakine benzer bir e-posta adresi olmalıdır: `username@{yourTenant}.onmicrosoft.com` .
1. `https://graph.windows.net/myorganization/deletedApplications`Api-Version = 1.6 Ile URL 'ye yönelik BIR http get verme. Bu işlem, son 30 gün içinde silinmiş tüm uygulamaların listesini alacak.
1. Adın ' B2C-Extension-App ' ile başladığı listede uygulamayı bulun ve `objectid` özellik değerini kopyalayın.
1. URL 'ye karşı bir HTTP POST verme `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore` . `{OBJECTID}`URL 'nin bölümünü önceki adımdan bulunan ile değiştirin `objectid` .

Artık Azure portal [geri yüklenen uygulamayı görebilmelisiniz](#verifying-that-the-extensions-app-is-present) .

> [!NOTE]
> Bir uygulama yalnızca son 30 gün içinde siliniyorsa geri yüklenebilir. 30 günden daha uzun bir süredir, veriler kalıcı olarak kaybedilir. Daha fazla yardım için bir destek bileti dosyası yapın.
