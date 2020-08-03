---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/09/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 612e7dc2f3d1ef6aa5d3598999aa214a2f377e1f
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2020
ms.locfileid: "87513204"
---
## <a name="overview-of-ssh-and-keys"></a>SSH ve anahtarlara genel bakış

[SSH](https://www.ssh.com/ssh/) , güvenli olmayan bağlantılar üzerinde güvenli oturum açma işlemleri sağlayan şifreli bir bağlantı protokolüdür. SSH, Azure 'da barındırılan Linux VM 'Leri için varsayılan bağlantı protokolüdür. SSH şifreli bir bağlantı sağlasa da, SSH bağlantılarıyla parolaların kullanılması VM 'nin deneme yanılma saldırılarına karşı savunmasız kalmasına neden olabilir. *SSH anahtarları*olarak da bilinen ortak özel anahtar ÇIFTINI kullanarak SSH ÜZERINDEN bir VM 'ye bağlanmanızı öneririz. 

- *Ortak anahtar* , Linux sanal makinenize yerleştirilir.

- *Özel anahtar* yerel sisteminizde kalır. Bu özel anahtarı koruyun. Özel anahtarı paylaşmayın.

Linux VM 'nize bağlanmak için bir SSH istemcisi kullandığınızda (ortak anahtara sahiptir), uzak VM, doğru özel anahtara sahip olduğundan emin olmak için istemciyi sınar. İstemci özel anahtara sahipse, VM 'ye erişim izni verilir. 

Kuruluşunuzun güvenlik ilkelerine bağlı olarak, birden fazla Azure VM ve hizmetine erişmek için tek bir genel özel anahtar çiftini yeniden kullanabilirsiniz. Erişmek istediğiniz her VM veya hizmet için ayrı bir anahtar çiftine gerek yoktur. 

Ortak anahtarınız herkese paylaşılabilir, ancak yalnızca sizin (veya yerel güvenlik altyapınız) özel anahtarınıza erişebilmelidir.