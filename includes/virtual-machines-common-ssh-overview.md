---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 12/21/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 3f9b86dd3716a25ab95265cf46b616144f57163b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71168657"
---
## <a name="overview-of-ssh-and-keys"></a>SSH ve anahtarlara genel bakış

[SSH,](https://www.ssh.com/ssh/) güvenli olmayan bağlantılar üzerinden güvenli oturum açmalara olanak tanıyan şifreli bir bağlantı protokolüdür. SSH, Azure'da barındırılan Linux VM'leri için varsayılan bağlantı protokolüdür. SSH'nin kendisi şifreli bir bağlantı sağlasa da, SSH bağlantılarına sahip parolalar kullanmak VM'yi kaba kuvvet saldırılarına veya parola tahminlerine karşı savunmasız bırakır. SSH kullanarak bir VM'ye bağlanmanın daha güvenli ve tercih edilen yöntemi, *SSH anahtarları*olarak da bilinen genel-özel anahtar çifti kullanmaktır. 

* *Ortak anahtar,* Linux VM'nize veya ortak anahtar şifrelemesiyle kullanmak istediğiniz diğer hizmetlere yerleştirilir.

* *Özel anahtar* yerel sisteminizde kalır. Bu özel anahtarı koruyun. Özel anahtarı paylaşmayın.

Linux VM'nize bağlanmak için bir SSH istemcisi kullandığınızda (ortak anahtara sahip), uzaktan VM istemciyi özel anahtara sahip olduğundan emin olmak için sınar. İstemci özel anahtara sahipse, VM'ye erişim izni verilir. 

Kuruluşunuzun güvenlik ilkelerine bağlı olarak, birden çok Azure VM'sine ve hizmetine erişmek için tek bir ortak özel anahtar çiftini yeniden kullanabilirsiniz. Erişmek istediğiniz her VM veya hizmet için ayrı bir anahtar çiftine ihtiyacınız yoktur. 

Ortak anahtarınız herkesle paylaşılabilir, ancak özel anahtarınıza yalnızca siz (veya yerel güvenlik altyapınız) sahip olmalısınız.