---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 8840e6649058837207c75dbdf7baa5c0b11ef387
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75468552"
---
Cihazınız, Azure'daki verileriniz için hedef olarak kullanılan bir depolama hesabıyla ilişkilidir. Depolama hesabına erişim abonelik ve bu depolama hesabıyla ilişkili iki adet 512 bit depolama erişim anahtarı tarafından denetlenir.

Anahtarlardan biri, Veri Kutusu Kenarı aygıtı depolama hesabına eriştiğinde kimlik doğrulama için kullanılır. Diğer anahtar yedekte tutulur, böylece tuşları periyodik olarak döndürebilirsiniz.

Güvenlik nedenleriyle, birçok veri merkezi anahtar döndürme gerektirir. Anahtar döndürme için aşağıdaki en iyi uygulamaları izlemenizi öneririz:

- Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Hesap anahtarınızı dikkatlice koruyun. Parolayı diğer kullanıcılara dağıtmayın, sabit kodlamayın veya başkalarının erişebileceği düz metin de herhangi bir yere kaydetmeyin.
- Hesap anahtarınızı Azure portalı üzerinden yeniden oluşturabilirsiniz. Daha fazla bilgi için [bkz.](../articles/storage/common/storage-account-keys-manage.md)
- Azure yöneticiniz, depolama hesabına doğrudan erişmek için Azure portalının Depolama bölümünü kullanarak birincil veya ikincil anahtarı düzenli aralıklarla değiştirmeli veya yeniden oluşturmalıdır.