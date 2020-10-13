---
title: Bir uygulama proxy uygulamasını PingAccess kullanacak şekilde yapılandırma
description: Uygulama proxy 'sinin, üst bilgi tabanlı kimlik doğrulaması kullanan uygulamalara sağladığı avantajları uzatmak için PingAccess 'i nasıl kullanacağınızı öğrenin
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/21/2018
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7cfbca2e7542315b59d741a9e8f6f9eb92af6ebf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "84760227"
---
# <a name="how-to-configure-an-application-proxy-application-to-use-pingaccess"></a>Bir uygulama proxy uygulamasını PingAccess kullanacak şekilde yapılandırma

PingAccess ile işbirliği yaptığımız, uygulama proxy 'sinin avantajlarını üst bilgi tabanlı kimlik doğrulaması kullanarak genişletmenizi sağlar. Uygulamalarınız üst bilgileri kullanmazlar, diğer seçeneklerle ilgili ayrıntılı bilgi için [tek Sign-On belgelerimize](application-proxy-configure-single-sign-on-with-kcd.md) bakın.

## <a name="overview-of-steps-and-recommended-documents"></a>Adımlara ve önerilen belgelere genel bakış

Bir uygulamayı PingAccess ile yapılandırmak için dört adım vardır:

1.  Uygulama proxy bağlayıcıları yapılandırma

2.  Azure AD Uygulama Ara Sunucusu uygulaması oluşturma

3.  PingAccess 'i indirme & yapılandırma

4.  PingAccess 'te uygulamaları yapılandırma

Bu adımların her biri hakkında daha fazla bilgi için, [üst bilgi belgeleriyle tek Sign-On](application-proxy-configure-single-sign-on-with-ping-access.md)bakın.
