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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76118067"
---
## <a name="about-the-user-delegation-sas"></a>Kullanıcı delegasyonu Hakkında SAS

Bir kapsayıcıya veya blob'a erişim için bir SAS belirteci, Azure AD kimlik bilgileri veya hesap anahtarı kullanılarak güvenli hale alınabilir. SAS'ı imzalamak için kullanılan OAuth 2.0 belirteci kullanıcı adına istendiğinden, Azure AD kimlik bilgileriyle güvenli bir SAS kullanıcı delegasyonu Nasibine Kullanıcı delegasyonu denir.

Microsoft, hesap anahtarını kullanmak yerine, daha kolay tehlikeye girebilecek bir güvenlik uygulaması olarak mümkün olduğunda Azure AD kimlik bilgilerini kullanmanızı önerir. Uygulama tasarımınız paylaşılan erişim imzaları gerektiriyorsa, üstün güvenlik için bir kullanıcı delegasyonu SAS oluşturmak için Azure AD kimlik bilgilerini kullanın. Kullanıcı delegasyonu SAS hakkında daha fazla bilgi için [bkz.](/rest/api/storageservices/create-user-delegation-sas)

> [!CAUTION]
> Geçerli bir SAS'a sahip olan tüm istemciler, depolama hesabınızdaki verilere, bu SAS'ın izin verdiği şekilde erişebilir. Bir SAS'ı kötü amaçlı veya istenmeyen kullanımlardan korumak önemlidir. Bir SAS dağıtmak için takdir kullanın ve tehlikeye sas iptal etmek için bir plan var.

Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni](../articles/storage/common/storage-sas-overview.md)ver.
