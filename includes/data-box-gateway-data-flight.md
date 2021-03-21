---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 10/15/2020
ms.author: alkohli
ms.openlocfilehash: e177fabdad7a1517a8371fe1d3483cb5c9310d2c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96583046"
---
Uçuş 'daki veriler için:

- Standart TLS 1,2, cihaz ile Azure arasında taşınan veriler için kullanılır. TLS 1,1 ve önceki bir sürümü için geri dönüş yoktur. TLS 1,2 desteklenmiyorsa, aracı iletişimi engellenir. Ayrıca, Portal ve SDK yönetimi için TLS 1,2 de gereklidir.
- İstemciler, bir tarayıcının yerel Web Kullanıcı arabirimi aracılığıyla cihazınıza erişebilince, varsayılan güvenli protokol olarak Standart TLS 1,2 kullanılır.

    - En iyi uygulama, tarayıcınızı TLS 1,2 kullanacak şekilde yapılandırmaktır.
    - Tarayıcı TLS 1,2 ' i desteklemiyorsa, TLS 1,1 veya TLS 1,0 ' i kullanabilirsiniz.
- Veri sunucularınızdan verileri korumak için şifreleme ile SMB 3,0 kullanmanızı öneririz.
