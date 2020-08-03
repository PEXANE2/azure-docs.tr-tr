---
author: baanders
description: Azure dijital TWINS kurulumu 'nda adımlara genel bakış için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: b8751685cfd273171cb8a5b500e4bed41ce2f59e
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407597"
---
>[!NOTE]
>Bu işlemlerin, Azure aboneliğinde *sahip* rolü olan bir kullanıcı tarafından tamamlanması amaçlanmıştır. Bu yükseltilmiş izin olmadan bazı parçalar tamamlanabilir, ancak kullanılabilir bir örneği tamamen ayarlamak için sahibin birlikte çalışması gerekecektir. [*Önkoşul: gerekli izinler*](#prerequisites-permission-requirements) bölümünde bunun hakkında daha fazla bilgi görüntüleyin.

Yeni bir Azure dijital TWINS örneği için tam kurulum üç bölümden oluşur:
1. **Örnek oluşturma**
2. **Kullanıcı erişim Izinlerini ayarlama**: Azure kullanıcılarının Azure dijital TWINS örneğinde, verileri ve verilerini yönetebilmek Için Azure *dijital TWINS sahibi (Önizleme)* rolünün olması gerekir. Bu adımda, Azure aboneliğindeki bir sahip olarak, Azure dijital TWINS örneğinizi yöneten kişiye bu rolü atayacaksınız. Bu, sizin veya kuruluşunuzdaki başka bir kullanıcı olabilir.
3. **İstemci uygulamaları için erişim Izinlerini ayarlama**: örneğinizle etkileşim kurmak için kullanılacak bir istemci uygulaması yazmak yaygındır. Bu istemci uygulamanın Azure dijital TWINS 'nize erişmesi için, istemci uygulamanın örnekte kimlik doğrulamak üzere kullanacağı [Azure Active Directory](../articles/active-directory/fundamentals/active-directory-whatis.md) bir *uygulama kaydı* ayarlamanız gerekir.

Devam etmek için bir Azure aboneliğine ihtiyacınız olacaktır. [Buradan](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)birini ücretsiz olarak ayarlayabilirsiniz.
