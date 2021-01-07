---
title: Sızma testi | Microsoft Docs
description: Makale, sızma testi sürecine genel bir bakış ve Azure altyapısında çalışan uygulamanıza yönelik bir kalem testinin nasıl gerçekleştirileceğini açıklar.
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
ms.date: 08/24/2020
ms.author: terrylan
ms.openlocfilehash: 84229ce3562f9cdcc4b55704eaa4f3b891a43d75
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97968951"
---
# <a name="penetration-testing"></a>Sızma testi

Uygulama testi ve dağıtımı için Azure kullanmanın avantajlarından biri, oluşturulan ortamları hızlıca almanızı sağlar. Kendi şirket içi donanımlarınızı talep eden, alma ve "izleme ve yığınlama" konusunda endişelenmeniz gerekmez.

Ortamları hızlı bir şekilde oluşturmak harika, ancak yine de normal güvenlik süresi ile devam ettiğinden emin olmanız gerekir. Büyük olasılıkla yapmak istediğiniz şeylerden biri, Azure 'da dağıttığınız uygulamaları test ediyor.

Microsoft 'un [Azure ortamımızın sızma testi](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)gerçekleştirdiğini zaten biliyorbilirsiniz. Bu test Azure geliştirmelerini yönlendirmeye yardımcı olur.

Uygulamanızı sizin için sızma testi gerçekleştirmedik, ancak kendi uygulamalarınızda test yapmanız gerektiğini ve istediğinizi anladık. Bu iyi bir şeydir çünkü uygulamalarınızın güvenliğini geliştirmenizde tüm Azure ekosisteminin daha güvenli hale getirilmesine yardımcı olursunuz.

15 Haziran 2017'den başlayarak Microsoft artık Azure kaynakları üzerinde sızma testi yapmak için ön onay gerektirmez. Bu işlem yalnızca Microsoft Azure ile ilgilidir ve diğer Microsoft Bulut Hizmetleri için geçerli değildir.

>[!IMPORTANT]
>Pen test etkinliklerinin Microsoft 'a bildirimde bulunmak için artık gerekli müşterilerin, [katılımın Microsoft bulut birleştirilmiş test kuralları](https://technet.microsoft.com/mt784683)ile uyumlu olması gerekir.

Gerçekleştirebileceğiniz standart testler şunlardır:

* [Açık Web uygulaması güvenlik projesini (OWASP) ilk 10 güvenlik açığını açmak](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) için uç noktalarınızda test edin
* Uç noktalarınızın [fuzz testi](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/)
* Uç noktalarınızın [bağlantı noktası taraması](https://en.wikipedia.org/wiki/Port_scanner)

Gerçekleştiremiyoruz bir tür kalem testi, herhangi bir [hizmet reddi (DOS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) saldırısı türüdür. Bu test, bir DoS saldırılarının kendisini başlatmayı veya herhangi bir DoS saldırısı türünü tespit edebilir, gösterebilir veya benzetim yaparak ilgili testlerin gerçekleştirilmesini içerir.

>[!Note]
>Microsoft, simülasyon için DDoS koruması etkinleştirilmiş genel IP adreslerine karşı trafik oluşturabileceğiniz bir arabirim oluşturmak üzere BreakingPoint bulutu ile işbirliği yaptı. BreakingPoint bulut simülasyonu hakkında daha fazla bilgi edinmek için bkz. [Benzetim aracılığıyla test etme](../../ddos-protection/test-through-simulations.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Katılım testi kuralları](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=2)hakkında daha fazla bilgi edinin.
