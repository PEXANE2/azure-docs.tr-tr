---
author: baanders
description: Azure Digital TWINS kurulumu 'nda erişim izinleri için dosya ekleme adımı
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 18aa039977beeff3f6b16249822fbd98370ca404
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408357"
---
Bir Azure dijital TWINS örneği ayarladıktan sonra, bir istemci uygulaması aracılığıyla bu örnekle etkileşimde bulunmak yaygındır. Çalışan bir istemci uygulaması oluşturmak için, istemci uygulamanın Azure dijital ikikine karşı kimlik doğrulaması yapabildiğinizden emin olmanız gerekir. Bu işlem, istemci uygulamanızın kullanması için bir [Azure Active Directory (Azure AD)](../articles/active-directory/fundamentals/active-directory-whatis.md) **uygulama kaydı** ayarlanarak yapılır.

Bu uygulama kaydı, [Azure dijital TWINS API 'leri](../articles/digital-twins/how-to-use-apis-sdks.md)için erişim izinlerini yapılandırdığınız yerdir. Daha sonra, istemci uygulaması uygulama kaydında kimlik doğrulaması yapacak ve bu nedenle API 'lere yapılandırılmış erişim izinleri verilmeyecektir.

>[!TIP]
> Abonelik sahibi olarak, her yeni Azure dijital TWINS örneği için yeni bir uygulama kaydı ayarlamayı *veya* bunu yalnızca bir kez yapmayı tercih edebilirsiniz. Bu, abonelikteki tüm Azure dijital TWINS örnekleri arasında paylaşılacak tek bir uygulama kaydı oluşturur.

### <a name="create-the-registration"></a>Kayıt oluşturma