---
title: Kalem Testi | Microsoft Dokümanlar
description: Makale, pentest testi (pentest) işlemine ve Azure altyapısında çalışan uygulamalarınız aleyhine pentest işlemine genel bir bakış sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/13/2018
ms.author: barclayn
ms.openlocfilehash: 3a2addce83862ef109089f1474330f3821daaed7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68726710"
---
# <a name="penetration-testing"></a>Penetrasyon Testi
Uygulama sınama ve dağıtım için Azure'u kullanmanın avantajlarından biri, ortamların hızla oluşturulmasını elde edebilirsiniz. Kendi şirket içi donanımınızı talep etme, satın alma ve "rafa kaldırma ve istifleme" konusunda endişelenmenize gerek yoktur.

Bu harika - ama yine de normal güvenlik durum tespiti gerçekleştirmek emin olmak gerekir. Yapmak isteyabileceğiniz şeylerden biri, Azure'da dağıttığınız uygulamaları penetrasyon testine girmektir.

Microsoft'un [Azure ortamımızın penetrasyon testini gerçekleştirdiğini](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)zaten biliyor olabilirsiniz. Bu, Azure geliştirmelerini artırmaya yardımcı olur.

Sizin için uygulamanızı test etmiyoruz, ancak kendi uygulamalarınızda test yapmak isteyeceğinizi ve yapmanız gerektiğini biliyoruz. Bu iyi bir şeydir, çünkü uygulamalarınızın güvenliğini artırdığınızda tüm Azure ekosisteminin daha güvenli hale getirin.

15 Haziran 2017 itibariyle Microsoft, Azure kaynaklarına karşı bir penetrasyon testi gerçekleştirmek için artık ön onay gerektirmez. Microsoft Azure'a karşı yaklaşan penetrasyon test etkileşimlerini resmi olarak belgelemek isteyen müşterilerin [Azure Hizmeti Neetmeni Test Bildirimi formunu](https://portal.msrc.microsoft.com/en-us/engage/pentest)doldurmaları tavsiye edilir. Bu işlem yalnızca Microsoft Azure ile ilgilidir ve diğer Microsoft Bulut Hizmeti için geçerli değildir.

>[!IMPORTANT]
>Kalem testi etkinliklerini Microsoft'a bildirmek artık gerekli olmasa da, müşterilerin [Microsoft Bulut Birleşik Penetrasyon Testi Etkileşim Kurallarına uymaları](https://technet.microsoft.com/mt784683)gerekmektedir.

Gerçekleştirebileceğiniz standart testler şunlardır:

* [Açık Web Uygulama Güvenliği Projesi'ni (OWASP) en iyi 10 güvenlik açığını](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) ortaya çıkarmak için uç noktalarınızda testler
* Uç noktalarınızın [bulanık testi](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/)
* Uç noktalarınızın [bağlantı noktası taraması](https://en.wikipedia.org/wiki/Port_scanner)

Gerçekleştiremediğiniz bir test türü, herhangi bir [Hizmet Reddi (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) saldırısıdır. Buna bir DoS saldırısı nın başlatılması veya herhangi bir DoS saldırısı türünü belirleyebilecek, gösterebilecek veya simüle edebilecek ilgili testler gerçekleştirme dahildir.

## <a name="next-steps"></a>Sonraki adımlar

- Microsoft Azure'da barındırılan uygulamalarınız için yaklaşan bir penetrasyon [testini](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=2) resmi olarak belgelemek istiyorsanız, Katılım A.Ş.'ye gidin ve test bildirim formunu doldurun.
