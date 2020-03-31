---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/12/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 95639caddfa627290f4e7153d50f9f7799a1f320
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75460567"
---
## <a name="protect-your-access-keys"></a>Erişim anahtarlarınızı koruyun

Depolama hesabı erişim anahtarlarınız, depolama hesabınız için bir kök parolaya benzer. Erişim anahtarlarınızı korumak için her zaman dikkatli olun. Anahtarlarınızı güvenli bir şekilde yönetmek ve döndürmek için Azure Key Vault'u kullanın. Erişim anahtarlarını diğer kullanıcılara dağıtmaktan, onları zor kodlamaktan veya başkalarının erişebileceği düz metinde herhangi bir yere kaydetmekten kaçının. Anahtarlarınızı tehlikeye atMış olabileceklerini düşünüyorsanız döndürün.

Mümkünse, Paylaşılan Anahtar yerine Blob ve Sıra depolama alanına istekleri yetkilendirmek için Azure Active Directory (Azure AD) kullanın. Azure AD, Paylaşılan Anahtar üzerinden üstün güvenlik ve kullanım kolaylığı sağlar. Azure AD ile verilere erişim yetkisi hakkında daha fazla bilgi için azure [etkin dizini kullanarak Azure blob'larına ve kuyruklarına erişimi yetkilendirme](../articles/storage/common/storage-auth-aad.md)'ye bakın.
