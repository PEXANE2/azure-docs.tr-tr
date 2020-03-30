---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/20/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: aec1faa4de1149f08fb6fbc1cc5bf3aa2ab6becd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75371794"
---
Paylaşılan erişim imzası (SAS), depolama hesabınızdaki kapsayıcılara ve lekelere sınırlı erişim izni vermenizi sağlar. Bir SAS oluşturduğunuzda, istemcinin hangi Azure Depolama kaynaklarına erişmesine izin verildiği, bu kaynaklarda hangi izinlere sahip oldukları ve SAS'ın ne kadar süreyle geçerli olduğu gibi kısıtlamalarını belirtirsiniz.

Her SAS bir anahtar la imzalanır. SAS'yi iki şekilde imzalayabilirsiniz:

- Azure Etkin Dizin (Azure AD) kimlik bilgileri kullanılarak oluşturulan bir anahtarla. Azure AD kimlik bilgileriyle imzalanmış bir *SAS, kullanıcı delegasyonu* SAS'tır.
- Depolama hesabı anahtarıyla. Hem *hizmet SAS* hem de *bir hesap SAS* depolama hesabı anahtarı ile imzalanır.

Bir kullanıcı delegasyonu SAS, depolama hesabı anahtarıyla imzalanan bir SAS'a üstün güvenlik sunar. Microsoft, mümkün olduğunda bir kullanıcı delegasyonu SAS kullanmanızı önerir. Daha fazla bilgi için bkz: [Paylaşılan erişim imzaları (SAS) ile verilere sınırlı erişim izni.](../articles/storage/common/storage-sas-overview.md)
