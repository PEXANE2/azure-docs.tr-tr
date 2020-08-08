---
author: baanders
description: Azure Digital TWINS kurulumu 'nda erişim izinleri için dosya ekleme adımı
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 45f55e6c18d7cc551a6b96504ad2ce2ec3f84d86
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009689"
---
Bir Azure dijital TWINS örneği ayarladıktan sonra, bir istemci uygulaması aracılığıyla bu örnekle etkileşimde bulunmak yaygındır. Çalışan bir istemci uygulaması oluşturmak için, istemci uygulamanın Azure dijital ikikine karşı kimlik doğrulaması yapabildiğinizden emin olmanız gerekir. Bu işlem, istemci uygulamanızın kullanması için bir [Azure Active Directory (Azure AD)](../articles/active-directory/fundamentals/active-directory-whatis.md) **uygulama kaydı** ayarlanarak yapılır.

Bu uygulama kaydı, [Azure dijital TWINS API 'leri](../articles/digital-twins/how-to-use-apis-sdks.md)için erişim izinlerini yapılandırdığınız yerdir. Daha sonra, istemci uygulaması uygulama kaydında kimlik doğrulaması yapacak ve bu nedenle API 'lere yapılandırılmış erişim izinleri verilmeyecektir.

>[!TIP]
> Abonelik sahibi/yönetici olarak, her yeni Azure dijital TWINS örneği için yeni bir uygulama kaydı ayarlamayı *veya* bunu yalnızca bir kez yapmayı tercih edebilirsiniz. Bu işlem, abonelikteki tüm Azure dijital TWINS örnekleri arasında paylaşılacak tek bir uygulama kaydı oluşturabilir.

### <a name="create-the-registration"></a>Kayıt oluşturma