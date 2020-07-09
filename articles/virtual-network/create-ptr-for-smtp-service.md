---
title: Bir SMTP başlık denetimi için ters arama bölgelerini yapılandırma
titlesuffix: Azure Virtual Network
description: Azure 'da bir SMTP başlık denetimi için geriye doğru arama bölgelerinin nasıl yapılandırılacağını açıklar.
services: virtual-network
documentationcenter: virtual-network
author: genlin
manager: dcscontentpm
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: c8ffadb8d54db0c2a99dc12e45b5990155a0505e
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135054"
---
# <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Bir SMTP başlık denetimi için ters arama bölgelerini yapılandırma

Bu makalede, Azure DNS ' de ters bir bölgenin nasıl kullanılacağı ve SMTP başlık denetimi için ters DNS (PTR) kaydı oluşturma açıklanır.

## <a name="symptom"></a>Belirti

Microsoft Azure bir SMTP sunucusu barındırdığınızda, uzak posta sunucularından bir ileti gönderdiğinizde veya alırken aşağıdaki hata iletisini alabilirsiniz:

**554: PTR kaydı yok**

## <a name="solution"></a>Çözüm

Azure 'daki bir sanal IP adresi için, ters kayıtlar özel etki alanı bölgelerinde değil, Microsoft 'un sahip olduğu etki alanı bölgelerinde oluşturulur.

Microsoft 'a ait bölgelerde PTR kayıtlarını yapılandırmak için, Publicıpaddress kaynağında-Smarfqdn özelliğini kullanın. Daha fazla bilgi için bkz. [Azure 'da barındırılan hizmetler için ters DNS yapılandırma](../dns/dns-reverse-dns-for-azure-services.md).

PTR kayıtlarını yapılandırırken, IP adresinin ve ters FQDN 'nin aboneliğe ait olduğundan emin olun. Aboneliğe ait olmayan bir ters FQDN ayarlamaya çalışırsanız aşağıdaki hata iletisini alırsınız:

```output
Set-AzPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership:
                    
1) ReverseFqdn matches fqdn of any public ip resource under the subscription;
2) ReverseFqdn resolves to the fqdn (through CName records chain) of any public ip resource under the subscription;
3) It resolves to the ip address (through CName and A records chain) of a static public ip resource under the subscription.
```

SMTP başlığını varsayılan ters FQDN 'umuza uyacak şekilde el ile değiştirirseniz, uzak posta sunucusu, SMTP başlık konağının etki alanı için MX kaydıyla eşleşmesini beklediği için yine de başarısız olabilir.
