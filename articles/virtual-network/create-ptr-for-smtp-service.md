---
title: SMTP banner denetimi için ters arama bölgelerini yapılandırma
titlesuffix: Azure Virtual Network
description: Azure'da Bir SMTP banner denetimi için ters arama bölgelerini nasıl yapılandırıştırılabildiğini açıklar
services: virtual-network
documentationcenter: virtual-network
author: genlin
manager: dcscontentpm
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 6c37ba61005c9936e6421d06369d1f52b93ac264
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77201705"
---
# <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>SMTP banner denetimi için ters arama bölgelerini yapılandırma

Bu makalede, Azure DNS'de ters bölge nasıl kullanılacağı ve SMTP Banner Denetimi için bir Ters DNS (PTR) kaydının nasıl oluşturuluyorum.

## <a name="symptom"></a>Belirti

Microsoft Azure'da bir SMTP sunucusu barındırıyorsanız, uzak posta sunucularından ileti gönderirken veya aldığınızda aşağıdaki hata iletisini alabilirsiniz:

**554: PTR Kaydı Yok**

## <a name="solution"></a>Çözüm

Azure'daki sanal bir IP adresi için ters kayıtlar özel etki alanı bölgelerinde değil, Microsoft'a ait etki alanı bölgelerinde oluşturulur.

Microsoft'a ait bölgelerdeki PTR kayıtlarını yapılandırmak için PublicIpAddress kaynağındaki -ReverseFqdn özelliğini kullanın. Daha fazla bilgi için [Azure'da barındırılan hizmetler için ters DNS'yi yapılandır' 'a](../dns/dns-reverse-dns-for-azure-services.md)bakın.

PTR kayıtlarını yapılandırdığınızda, IP adresinin ve ters FQDN'nin aboneye ait olduğundan emin olun. Aboneğe ait olmayan ters bir FQDN ayarlamaya çalışırsanız, aşağıdaki hata iletisini alırsınız:

    Set-AzPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership:
                        
    1) ReverseFqdn abonelik altında herhangi bir kamu ip kaynağıfqdn eşleşir;
    2) ReverseFqdn abonelik altında herhangi bir kamu ip kaynağının fqdn (CName kayıt zinciri üzerinden) giderir;
    3) Abonelik altında statik bir kamu ip kaynağının ip adresine (CName ve A kayıt zinciri aracılığıyla) giderir.

SMTP banner'ınızı varsayılan ters FQDN ile eşleşecek şekilde el ile değiştirirseniz, Uzak posta sunucusu yine de başarısız olabilir, çünkü SMTP banner ana bilgisayarının etki alanının MX kaydıyla eşleşmesini bekleyebilir.
