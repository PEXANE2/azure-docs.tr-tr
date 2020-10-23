---
author: baanders
description: Azure dijital TWINS kurulumu 'nda adımlara genel bakış için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 10/14/2020
ms.author: baanders
ms.openlocfilehash: 5ab6812d144122c61018ad740892db869a7ba43d
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92205512"
---
>[!NOTE]
>Bu işlemler, Azure aboneliğindeki kaynakları ve Kullanıcı erişimini yönetmek için bir kullanıcı tarafından bir kullanıcı tarafından tamamlanacak şekilde tasarlanmıştır. Bazı adımlar daha düşük izinlerle tamamlanabilse de, bu izinlere sahip birisinin ortak işlemi, kullanılabilir bir örneği tamamen ayarlamak için gerekli olacaktır. [*Önkoşul: gerekli izinler*](#prerequisites-permission-requirements) bölümünde bunun hakkında daha fazla bilgi görüntüleyin.

Yeni bir Azure dijital TWINS örneği için tam kurulum iki bölümden oluşur:
1. **Örnek oluşturma**
2. **Kullanıcı erişim Izinlerini ayarlama**: Azure kullanıcılarının Azure dijital TWINS örneğinde, verileri ve verilerini yönetebilmek Için Azure *dijital TWINS sahibi (Önizleme)* rolünün olması gerekir. Bu adımda, Azure aboneliğinin sahibi/Yöneticisi, bu rolü Azure dijital TWINS örneğinizi yöneten kişiye atayacaktır. Bu, sizin veya kuruluşunuzdaki başka bir kullanıcı olabilir.