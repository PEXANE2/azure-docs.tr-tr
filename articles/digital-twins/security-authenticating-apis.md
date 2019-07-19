---
title: Azure dijital TWINS API kimlik doğrulamasını anlama | Microsoft Docs
description: Azure dijital TWINS kullanarak API 'lere bağlanma ve kimlik doğrulama
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: lyhughes
ms.openlocfilehash: 114edc072524552fab35e9cad6fc85573c4e8d0e
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846544"
---
# <a name="connect-and-authenticate-to-apis"></a>API 'lere bağlanma ve kimlik doğrulama

Azure dijital TWINS, kullanıcıların kimliğini doğrulamak ve uygulamaları korumak için Azure Active Directory (Azure AD) kullanır. Azure AD, çeşitli modern mimarilerin kimlik doğrulamasını destekler. Bunların hepsi, OAuth 2,0 veya OpenID Connect sektör standardı protokollerine dayanır. Ayrıca, geliştiriciler tek kiracılı ve iş kolu (LOB) uygulamaları oluşturmak için Azure AD 'yi kullanabilir. Geliştiriciler, çok kiracılı uygulamalar geliştirmek için Azure AD 'yi de kullanabilir.

Azure AD 'ye genel bakış için, adım adım kılavuzlar, kavramlar ve hızlı başlangıçların [temelleri sayfasını](https://docs.microsoft.com/azure/active-directory/fundamentals/index) ziyaret edin.

Bir uygulama veya hizmeti Azure AD ile tümleştirmek için geliştiricilerin önce uygulamayı Azure AD'ye kaydetmesi gerekir. Ayrıntılı yönergeler ve ekran görüntüleri için bkz. [Bu hızlı başlangıç](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

Azure AD tarafından [beş birincil uygulama senaryosu](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) desteklenir:

* Tek sayfalı uygulama (SPA): Bir kullanıcının Azure AD tarafından güvenliği sağlanmış tek sayfalı bir uygulamada oturum açması gerekir.
* Web uygulamasına Web tarayıcısı: Bir kullanıcının Azure AD tarafından güvenliği sağlanmış bir Web uygulamasında oturum açması gerekir.
* Web API 'sine yerel uygulama: Bir telefonda, tablette veya bılgısayarda çalışan yerel bir uygulamanın, Azure AD tarafından güvenliği sağlanan bir Web API 'sinden kaynakları almak için bir kullanıcının kimliğini doğrulaması gerekir.
* Web uygulamasından Web API 'sine: Bir Web uygulamasının Azure AD tarafından güvenliği sağlanmış bir Web API 'sinden kaynak alması gerekir.
* Web API 'ye yönelik Daemon veya sunucu uygulaması: Bir Web Kullanıcı arabirimi olmayan bir Daemon uygulamasının veya bir sunucu uygulamasının Azure AD tarafından güvenliği sağlanmış bir Web API 'sinden kaynak alması gerekir.

Windows Azure kimlik doğrulama kitaplığı Active Directory belirteçleri edinmenin birçok yolunu sunar. Kitaplık ve kod örnekleri hakkında daha fazla bilgi için [Bu makaleye](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki)bakın.

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Orta katman Web API 'sinden dijital TWINS çağırma

Geliştiriciler dijital TWINS çözümlerini mimarilerlerse, genellikle bir orta katman uygulaması veya API 'SI oluşturur. Uygulama veya API daha sonra dijital TWINS API 'sini aşağı akış olarak çağırır. Bu standart Web çözüm mimarisini desteklemek için, kullanıcıların önce şunları yaptığınızdan emin olun:

1. Orta katman uygulamasıyla kimlik doğrulama

1. Kimlik doğrulaması sırasında bir OAuth 2,0 adına belirteç alındı

1. Elde edilen belirteç daha sonra kimlik doğrulaması için veya şirket içi akış kullanarak daha fazla aşağı akış olan API 'Leri çağırmak için kullanılır

Şirket adına akışı düzenleme hakkında yönergeler için bkz. [OAuth 2,0 on-the Flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Ayrıca, [bir aşağı akış Web API 'Sini çağırarak](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/)kod örneklerini görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

OAuth 2,0 örtük verme akışını kullanarak Azure dijital TWINS yapılandırmak ve test etmek için [Postman yapılandırma](./how-to-configure-postman.md)makalesini okuyun.

Azure dijital TWINS güvenliği hakkında daha fazla bilgi edinmek için [rol atamaları oluşturma ve yönetme](./security-create-manage-role-assignments.md)konusunu okuyun.