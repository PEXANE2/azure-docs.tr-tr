---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: acf1195616d45b155445604ef0204528e5f7ca03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67189008"
---
Uçuş verileri için:

- Standart TLS 1.2, aygıt ve Azure arasında seyahat eden veriler için kullanılır. TLS 1.1 ve daha önceki bir geri dönüş yoktur. TLS 1.2 desteklenmezse aracı iletişimi engellenir. PORTAL ve SDK yönetimi için TLS 1.2 de gereklidir.
- İstemciler cihazınıza bir tarayıcının yerel web ui üzerinden eriştisinse, standart TLS 1.2 varsayılan güvenli protokol olarak kullanılır.

    - En iyi uygulama, tarayıcınızı TLS 1.2 kullanacak şekilde yapılandırmaktır.
    - Tarayıcı TLS 1.2'yi desteklemiyorsa, TLS 1.1 veya TLS 1.0 kullanabilirsiniz.
- Verileri veri sunucularınızdan kopyalarken korumak için SMB 3.0'ı şifreleme ile kullanmanızı öneririz.
