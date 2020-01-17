---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/15/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a0a4a10797109cb3363027e2445259d06d9aa071
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76118067"
---
## <a name="about-the-user-delegation-sas"></a>Kullanıcı temsili SAS hakkında

Bir kapsayıcıya veya bloba erişim için bir SAS belirteci, Azure AD kimlik bilgileri veya hesap anahtarı kullanılarak güvenliği sağlanmış olabilir. Azure AD kimlik bilgileriyle güvenliği sağlanmış bir SAS, Kullanıcı adına SAS imzalamak için kullanılan OAuth 2,0 belirtecinin istendiği için Kullanıcı temsili SAS olarak adlandırılır.

Microsoft, Azure AD kimlik bilgilerini, daha kolay tehlikeye giren hesap anahtarını kullanmak yerine en iyi güvenlik uygulaması olarak mümkün olduğunca kullanmanızı önerir. Uygulama tasarımınız paylaşılan erişim imzaları gerektirdiğinde, üstün güvenlik için Kullanıcı temsili SAS oluşturmak için Azure AD kimlik bilgilerini kullanın. Kullanıcı temsili SAS hakkında daha fazla bilgi için bkz. [Kullanıcı TEMSILI SAS oluşturma](/rest/api/storageservices/create-user-delegation-sas).

> [!CAUTION]
> Geçerli bir SAS sahibi olan tüm istemciler, bu SAS tarafından izin verilen Depolama hesabınızdaki verilere erişebilir. Bir SAS 'ın kötü amaçlı veya istenmeyen bir kullanım açısından korunması önemlidir. SAS dağıtmanın yanı sıra güvenliği aşılmış SAS iptal etmek için bir plana sahip olmak için bir plan kullanın.

Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../articles/storage/common/storage-sas-overview.md).
