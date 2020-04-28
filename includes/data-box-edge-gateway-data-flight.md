---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: acf1195616d45b155445604ef0204528e5f7ca03
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67189008"
---
Uçuş 'daki veriler için:

- Standart TLS 1,2, cihaz ile Azure arasında taşınan veriler için kullanılır. TLS 1,1 ve önceki bir sürümü için geri dönüş yoktur. TLS 1,2 desteklenmiyorsa, aracı iletişimi engellenir. Ayrıca, Portal ve SDK yönetimi için TLS 1,2 de gereklidir.
- İstemciler, bir tarayıcının yerel Web Kullanıcı arabirimi aracılığıyla cihazınıza erişebilince, varsayılan güvenli protokol olarak Standart TLS 1,2 kullanılır.

    - En iyi uygulama, tarayıcınızı TLS 1,2 kullanacak şekilde yapılandırmaktır.
    - Tarayıcı TLS 1,2 ' i desteklemiyorsa, TLS 1,1 veya TLS 1,0 ' i kullanabilirsiniz.
- Veri sunucularınızdan verileri korumak için şifreleme ile SMB 3,0 kullanmanızı öneririz.
