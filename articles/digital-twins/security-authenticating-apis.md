---
title: API kimlik doğrulamasını anlama - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins'i kullanarak API'lere nasıl bağlanıp kimlik doğrulaması yapacaklarını öğrenin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: d950d41186d578702343645875dd7c565002d5a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76513017"
---
# <a name="connect-to-and-authenticate-with-apis"></a>API'lere bağlanma ve kimlik doğrulaması

Azure Digital Twins, kullanıcıların kimliğini doğrulamak ve uygulamaları korumak için Azure Active Directory (Azure AD) kullanır. Azure AD, çeşitli modern mimariler için kimlik doğrulamayı destekler. Bunların hepsi endüstri standardı protokolleri OAuth 2.0 veya OpenID Connect dayanmaktadır. Ayrıca, geliştiriciler tek kiracılı ve iş çizgisi (LOB) uygulamaları oluşturmak için Azure AD'yi kullanabilir. Geliştiriciler ayrıca [çok kiracılı uygulamalar](how-to-multitenant-applications.md)geliştirmek için Azure AD'yi de kullanabilir.

Azure AD'ye genel bir bakış için, adım adım kılavuzlar, kavramlar ve hızlı başlangıçlar için [temel ler sayfasını](https://docs.microsoft.com/azure/active-directory/fundamentals/) ziyaret edin.

> [!TIP]
> Azure Digital Twins örnek uygulamasını ayarlamak ve çalıştırmak için [Öğretici'yi](tutorial-facilities-setup.md) izleyin.

Bir uygulama veya hizmeti Azure AD ile tümleştirmek için geliştiricilerin önce uygulamayı Azure AD'ye kaydetmesi gerekir. Ayrıntılı talimatlar ve ekran görüntüleri için [bu quickstart'ı](../active-directory/develop/quickstart-register-app.md)okuyun.

[Beş birincil uygulama senaryosu](../active-directory/develop/v2-app-types.md) Azure AD tarafından desteklenir:

* Tek sayfalı uygulama (SPA): Bir kullanıcının Azure AD tarafından güvenli tek sayfalı bir uygulamada oturum açması gerekir.
* Web tarayıcısı ve web uygulaması: Bir kullanıcının Azure AD tarafından güvenli bir web uygulamasında oturum açması gerekir.
* Web API'ye yerel uygulama: Telefonda, tablette veya bilgisayarda çalışan yerel bir uygulamanın, Azure AD tarafından güvenli bir web API'sinden kaynak elde etmek için kullanıcının kimliğini doğrulaması gerekir.
* Web api'sine web uygulaması: Bir web uygulamasının Azure AD tarafından güvenli bir web API'sinden kaynak alması gerekir.
* Web API'sine daemon veya sunucu uygulaması: Bir daemon uygulaması veya web kullanıcı arabirimi olmayan bir sunucu uygulamasının Azure AD tarafından güvenli bir web API'sinden kaynak alması gerekir.

> [!IMPORTANT]
> Azure Digital Twins, aşağıdaki kimlik doğrulama kitaplıklarının her ikisini de destekler:
> * Daha yeni [Microsoft Kimlik Doğrulama Kitaplığı (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview)
> * [Azure Etkin Dizin Kimlik Doğrulama Kitaplığı (ADAL)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Orta katmanlı web API'sinden Dijital İkizler'i arayın

Geliştiriciler Digital Twins çözümlerini mimar ettiklerinde, genellikle bir orta katman uygulaması veya API oluştururlar. Uygulama veya API daha sonra Digital Twins API'yi aşağı akım olarak adlandırır. Bu standart web çözüm mimarisini desteklemek için, önce kullanıcıların şularını

1. Orta katman uygulamasıyla kimlik doğrulaması

1. Kimlik doğrulama sırasında Bir OAuth 2.0 Adına-Token elde edilir

1. Edinilen belirteç, Daha sonra On-Behalf-Of akışını kullanarak daha aşağıda olan API'leri doğrulamak veya aramak için kullanılır

Akış adına nasıl düzenleneneciyi anlatan talimatlar için [OAuth 2.0 On-Behalf-Of akışını](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow)okuyun. Ayrıca, [bir downstream web API'yi arama](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof)da kod örneklerini görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

OAuth 2.0 örtülü hibe akışını kullanarak Azure Digital Twins'i yapılandırmak ve test etmek için [Postacıyı Yapılandırma'yı](./how-to-configure-postman.md)okuyun.

Azure Digital Twins güvenliği hakkında bilgi edinmek için [rol atamaları oluştur ve yönet'](./security-create-manage-role-assignments.md)i okuyun.
