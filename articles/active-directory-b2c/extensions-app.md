---
title: Azure Active Directory B2C 'de uzantılar uygulaması | Microsoft Docs
description: B2C-Extensions-App ' i geri yükleme.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/06/2017
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 547b625996a65999c32c1b73699e3b408be01de3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78188605"
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: uzantılar uygulaması

Azure AD B2C bir dizin oluşturulduğunda, adlı `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` bir uygulama yeni dizin içinde otomatik olarak oluşturulur. **B2C-Extensions-App**olarak anılan bu uygulama *uygulama kayıtları*görünür. Kullanıcılar ve özel öznitelikler hakkında bilgi depolamak için Azure AD B2C hizmeti tarafından kullanılır. Uygulama silinirse, Azure AD B2C düzgün çalışmayacaktır ve üretim ortamınız etkilenecektir.

> [!IMPORTANT]
> Kiracınızı hemen silmeyi planlamadığınız müddetçe B2C-Extensions-App ' i silmeyin. Uygulama 30 günden uzun bir süre sonra silinirse, Kullanıcı bilgileri kalıcı olarak kaybedilir.

## <a name="verifying-that-the-extensions-app-is-present"></a>Uzantı uygulamasının mevcut olduğu doğrulanıyor

B2C-Extensions-uygulamasının mevcut olduğunu doğrulamak için:

1. Azure AD B2C kiracınızda, sol taraftaki gezinti menüsünde **tüm hizmetler** ' e tıklayın.
1. **Uygulama kayıtları**arayın ve açın.
1. **B2C-Extensions-App** ile başlayan bir uygulama arayın

## <a name="recover-the-extensions-app"></a>Uzantılar uygulamasını kurtarma

B2C-Extensions-App ' i yanlışlıkla sildiyseniz, bu uygulamayı kurtarmak için 30 gününüz vardır. Graph API kullanarak uygulamayı geri yükleyebilirsiniz:

1. Öğesine [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/)gidin.
1. İçin silinen uygulamayı geri yüklemek istediğiniz Azure AD B2C dizin için genel yönetici olarak sitede oturum açın. Bu genel yöneticinin aşağıdakine benzer bir e-posta adresi olmalıdır: `username@{yourTenant}.onmicrosoft.com`.
1. Api-Version = 1.6 ile URL `https://graph.windows.net/myorganization/deletedApplications` 'ye YÖNELIK BIR http get verme. Bu işlem, son 30 gün içinde silinmiş tüm uygulamaların listesini alacak.
1. Adın ' B2C-Extension-App ' ile başladığı listede uygulamayı bulun ve `objectid` özellik değerini kopyalayın.
1. URL 'ye `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`KARŞı BIR http post verme. URL 'nin `{OBJECTID}` bölümünü önceki adımdan bulunan ile `objectid` değiştirin.

Artık Azure portal [geri yüklenen uygulamayı görebilmelisiniz](#verifying-that-the-extensions-app-is-present) .

> [!NOTE]
> Bir uygulama yalnızca son 30 gün içinde siliniyorsa geri yüklenebilir. 30 günden daha uzun bir süredir, veriler kalıcı olarak kaybedilir. Daha fazla yardım için bir destek bileti dosyası yapın.
