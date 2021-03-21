---
author: baanders
description: Azure dijital TWINS kurulumu 'nda adımlara genel bakış için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 10/14/2020
ms.author: baanders
ms.openlocfilehash: c1cfab8c9e68d9e6b0c3b84e8848645a50c24710
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100560798"
---
Yeni bir Azure dijital TWINS örneği için tam kurulum iki bölümden oluşur:
1. **Örnek oluşturma**
2. **Kullanıcı erişim Izinlerini ayarlama**: Azure kullanıcılarının Azure dijital TWINS örneğinde, verileri ve verilerini yönetebilmeleri Için Azure *Digital TWINS veri sahibi* rolüne sahip olması gerekir. Bu adımda, Azure aboneliğinin sahibi/Yöneticisi, bu rolü Azure dijital TWINS örneğinizi yöneten kişiye atayacaktır. Bu, sizin veya kuruluşunuzdaki başka bir kullanıcı olabilir.
 
>[!NOTE]
>Bu işlemler, Azure aboneliğindeki kaynakları ve Kullanıcı erişimini yönetmek için bir kullanıcı tarafından bir kullanıcı tarafından tamamlanacak şekilde tasarlanmıştır. Bazı adımlar daha düşük izinlerle tamamlanabilse de, bu izinlere sahip birisinin ortak işlemi, kullanılabilir bir örneği tamamen ayarlamak için gerekli olacaktır. [*Önkoşul: gerekli izinler*](#prerequisites-permission-requirements) bölümünde bunun hakkında daha fazla bilgi görüntüleyin.