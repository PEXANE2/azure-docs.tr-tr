---
title: Geliştirdiğiniz kimlik doğrulama ve yetkilendirme uygulamalarının esnekliği artırma
titleSuffix: Microsoft identity platform
description: Azure Active Directory ve Microsoft Identity platformu kullanarak uygulama geliştirmeye yönelik esnekliği kılavuzumuza genel bakış
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: knicholasa
ms.author: nichola
manager: martinco
ms.date: 11/23/2020
ms.openlocfilehash: c2c2f9d0ad7bfa50f543b57326b9fc8dab0069c6
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96029311"
---
# <a name="increase-resilience-of-authentication-and-authorization-applications-you-develop"></a>Geliştirdiğiniz kimlik doğrulama ve yetkilendirme uygulamalarının esnekliği artırma

Microsoft Identity modern, belirteç tabanlı kimlik doğrulama ve yetkilendirme kullanır. Bu, bir uygulamanın kullanıcı kimliğini doğrulamak ve uygulamayı korunan API 'Leri çağırmak üzere yetkilendirmek için bir kimlik sağlayıcısından belirteçleri aldığı anlamına gelir.

Belirteç, uygulamanın yeni bir süre önce edinilmeden önce belirli bir süre için geçerlidir. Nadiren, bir belirteç alma çağrısı ağ veya altyapı hatası ya da kimlik doğrulama hizmeti kesintisi gibi bir sorun nedeniyle başarısız olabilir. Bu belgede, bir belirteç alma hatası oluşursa, bir geliştiricinin uygulamalarında esnekliği arttırmak için atabileceğiniz adımları ana hatlarıyla sunuyoruz.

Bu makaleler, Microsoft Identity platform ve Azure Active Directory kullanarak uygulamalarda dayanıklılığı arttırmaya yönelik rehberlik sağlar. Hem oturum açmış bir kullanıcı adına hem de kendi adına çalışan arka plan uygulamalarına yönelik olarak çalışan istemci uygulamaları için rehberlik vardır. Bunlar, belirteçleri kullanmak ve kaynakları çağırmak için en iyi yöntemleri içerir.

- [Esnekliği, oturum açma kullanıcılarına yönelik uygulamalar oluşturun](resilience-client-app.md)
- [Kullanıcılar olmadan esnekliği uygulamaları oluşturma](resilience-daemon-app.md)
- [Kimlik ve erişim yönetimi altyapınızda esnekliği oluşturma](resilience-in-infrastructure.md)
- [Azure Active Directory B2C ile Müşteri Kimliğiniz ve erişim yönetiminde esnekliği oluşturun](resilience-b2c.md)
