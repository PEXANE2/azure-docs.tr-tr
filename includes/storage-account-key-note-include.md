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
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75460567"
---
## <a name="protect-your-access-keys"></a>Erişim anahtarlarınızı koruyun

Depolama hesabı erişim anahtarlarınız, depolama hesabınız için bir kök parolaya benzer. Erişim anahtarlarınızı korumak için her zaman dikkatli olun. Anahtarlarınızı güvenli bir şekilde yönetmek ve döndürmek için Azure Key Vault kullanın. Erişim anahtarlarını diğer kullanıcılara dağıtmaktan, sabit kodlamadan veya başkalarının erişebileceği düz metin olarak herhangi bir yere kaydetmekten kaçının. Anahtarların tehlikede olduğunu düşünüyorsanız, anahtarlarınızı döndürün.

Mümkünse, istekleri blob ve kuyruk depolamaya yetkilendirmek için, paylaşılan anahtar yerine Azure Active Directory (Azure AD) kullanın. Azure AD, paylaşılan anahtar üzerinde üstün güvenlik ve kullanım kolaylığı sağlar. Azure AD ile verilere erişimi yetkilendirme hakkında daha fazla bilgi için bkz. [Azure Active Directory kullanarak Azure bloblarına ve kuyruklara erişim yetkisi verme](../articles/storage/common/storage-auth-aad.md).
