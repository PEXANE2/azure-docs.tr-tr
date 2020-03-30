---
title: Azure Active Directory B2C'deki uzantıuygulaması | Microsoft Dokümanlar
description: B2c uzantıları-uygulamasını geri geri.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188605"
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: Uzantılar uygulaması

Azure AD B2C dizini oluşturulduğunda, `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` yeni dizinin içinde otomatik olarak bir uygulama oluşturulur. **B2c uzantıları-uygulaması**olarak adlandırılan bu uygulama, Uygulama *kayıtlarında*görülebilir. Azure AD B2C hizmeti tarafından kullanıcılar ve özel öznitelikler hakkında bilgi depolamak için kullanılır. Uygulama silinirse, Azure AD B2C düzgün çalışmaz ve üretim ortamınız etkilenir.

> [!IMPORTANT]
> Kiracınızı hemen silmeyi düşünmüyorsanız b2c uzantıları-uygulamasını silmeyin. Uygulama 30 günden uzun süre silinmiş kalırsa, kullanıcı bilgileri kalıcı olarak kaybolur.

## <a name="verifying-that-the-extensions-app-is-present"></a>Uzantıuygulamasının mevcut olduğunu doğrulama

B2c uzantıları-uygulamasının mevcut olduğunu doğrulamak için:

1. Azure AD B2C kiracınızın içinde, sol daki gezinme menüsündeki **Tüm hizmetlere** tıklayın.
1. **Uygulama kayıtlarını**arayın ve açın.
1. **b2c uzantıları-uygulaması** ile başlayan bir uygulama arayın

## <a name="recover-the-extensions-app"></a>Uzantıları uygulamasını kurtarma

B2c uzantıları-uygulamasını yanlışlıkla sildiyseniz, onu kurtarmak için 30 gününüz vardır. Grafik API'sini kullanarak uygulamayı geri yükleyebilirsiniz:

1. 'ye [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/)göz atın.
1. Silinen uygulamayı geri yüklemek istediğiniz Azure AD B2C dizininin global yöneticisi olarak sitede oturum açın. Bu genel yöneticiaşağıdakilere benzer bir e-posta adresine sahip olmalıdır: `username@{yourTenant}.onmicrosoft.com`.
1. Api-version=1.6 ile URL'ye `https://graph.windows.net/myorganization/deletedApplications` karşı bir HTTP GET sorunu. Bu işlem, son 30 gün içinde silinen tüm uygulamaları listeleyecektir.
1. Adın 'b2c-extension-app' ile başladığı listede uygulamayı bulun `objectid` ve özellik değerini kopyalayın.
1. URL'ye `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`karşı bir HTTP POST sorunu . URL'nin `{OBJECTID}` bir bölümünü `objectid` önceki adımdan değiştirin.

Artık [Azure portalında geri yüklenen uygulamayı görebilmelisin.](#verifying-that-the-extensions-app-is-present)

> [!NOTE]
> Bir uygulama yalnızca son 30 gün içinde silinmişse geri yüklenebilir. 30 günden fazla olduysa, veriler kalıcı olarak kaybolur. Daha fazla yardım için bir destek bileti gönderin.
