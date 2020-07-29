---
title: İzleme seçenekleri-Azure ayrılmış HSM | Microsoft Docs
description: Azure adanmış HSM izleme seçeneklerine ve izleme sorumluluklarına genel bakış
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: mbaldwin
ms.openlocfilehash: 3fde577a6b0efb7584e1c9efd57c95583ebe4ec9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "70881428"
---
# <a name="azure-dedicated-hsm-monitoring"></a>Azure adanmış HSM izleme

Azure ayrılmış HSM hizmeti, tek müşteri kullanımı için tüm yönetim denetimi ve yönetim sorumluluğuna sahip fiziksel bir cihaz sağlar. Kullanılabilir cihaz, bir [Gemalto SafeNet Luna 7 HSM modeli A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/).  Microsoft, fiziksel seri bağlantı noktası ekinin ötesinde bir izleme rolü olarak bir müşteri tarafından sağlandıktan sonra yönetim erişimi olmayacaktır. Sonuç olarak, müşteriler kapsamlı izleme ve günlük analizi dahil olmak üzere tipik işletimsel etkinliklerden sorumludur.
Müşteriler, HSM 'leri kullanan uygulamalardan tamamen sorumludur ve destek ya da danışmanlık yardımı için Gemalto ile birlikte çalışmalıdır. İşlemsel Hygiene 'ın müşteri sahipliğinin uzatması nedeniyle Microsoft 'un bu hizmet için herhangi bir tür yüksek kullanılabilirlik garantisi sunabileceği için bu mümkün değildir. Yüksek kullanılabilirlik elde etmek için uygulamalarının doğru şekilde yapılandırıldığından emin olunması müşterinin sorumluluğundadır. Microsoft, cihaz sistem durumu ve ağ bağlantısını izler ve korur.

## <a name="microsoft-monitoring"></a>Microsoft Monitoring

Kullanımdaki Gemalto SafeNet cihazı, cihazı izlemeye yönelik seçenekler olarak varsayılan SNMP ve seri bağlantı noktasıdır. Microsoft, seri bağlantı noktası bağlantısını, cihaz durumu üzerinde temel telemetri almak için cihaza bağlanmak üzere fiziksel bir yol olarak kullandı. Bu, sıcaklık ve güç kaynakları ve fanları gibi bileşen durumu gibi öğeleri içerir.
Bu ulaşmak için, Microsoft, Gemalto cihazında ayarlanmış yönetici olmayan bir "izleyici" rolü kullanır. Bu rol, Telemetriyi alma, ancak yönetim görevi açısından cihaza veya şifreleme bilgilerini görüntüleyerek herhangi bir şekilde erişim vermemenize olanak tanır. Müşterilerimiz, hassas şifreleme anahtarı depolaması için yönetim, yönetme ve kullanma gibi cihazların gerçekten kendi kendine ait olmasını garanti edebilir. Müşterilerin, temel sistem durumu izleme için bu minimum erişimle karşılanmadığı durumlarda, izleme hesabını devre dışı bırakma seçeneği vardır. Bunun belirgin sonucu, Microsoft 'un hiçbir bilgi sahibi olmaması ve bu nedenle cihaz sistem durumu sorunlarının öngörülü bildirimini sağlamabilmesidir. Bu durumda, müşteri, cihazın sistem durumuyla sorumludur.
İzleyici işlevinin kendisi, sistem durumu verilerinin alınması için her 10 dakikada bir cihaz yoklamak üzere ayarlanır. Seri iletişimin ortaya çıkan hatası nedeniyle, yalnızca bir saatlik süre içinde birden çok negatif sistem durumu göstergeleriyle sonra bir uyarı tetiklenir. Bu uyarı, sonunda sorunu bildiren proaktif bir müşteri iletişimine yol açabilir.
Sorunun doğasına bağlı olarak, etkiyi azaltmak ve düşük riskli düzeltme sağlamak için uygun eylem kursu alınacaktır. Örneğin, bir güç kaynağı hatası, en düşük etki ve işlem açısından en az risk ile gerçekleştirilebilecek bir etkin düzeltme olayı olmayan bir Hot-Swap yordamdır. Diğer yordamlar, bir cihazın sıfır dışı olması ve müşteriye yönelik herhangi bir güvenlik riskini en aza indirmek için sağlanması gerekebilir. Bu durumda, bir müşterinin alternatif bir cihaz sağlaması, daha sonra cihaz eşitlemesini tetiklemesini sağlamak için yüksek kullanılabilirlik eşleştirmesine yeniden katılın. Normal işlem, minimum kesinti ve en düşük güvenlik riskine sahip en kısa sürede sürdürülür.  

## <a name="customer-monitoring"></a>Müşteri izleme

Adanmış HSM hizmeti 'nin bir değer teklifi, müşterinin cihazı aldığı ve özellikle de bulut teslim edilen bir cihaz olduğu denetimdir. Bu denetimin bir sonucu, cihazın sistem durumunu izleme ve yönetme sorumluluğudur. Gemalto SafeNet cihazı SNMP ve Syslog uygulamasına yönelik kılavuzlarla birlikte gelir. Microsoft Monitor hesabı etkin olmaya devam eder ve Microsoft Monitor hesabını devre dışı bıraktıklarında bu özelliği zorunlu kabul etmelidir, adanmış HSM hizmeti müşterilerinin kullanılması önerilir.
Her iki yöntem de bir müşterinin sorunları belirlemesini ve uygun düzeltme işlerini başlatması için Microsoft desteği 'ni çağırmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Hizmetin yüksek kullanılabilirlik ve güvenlik gibi tüm temel kavramlarının herhangi bir cihaz sağlama ve uygulama tasarımı ya da dağıtımdan önce iyi anlaşıldığından emin olmanız önerilir. Daha fazla kavram düzeyi konuları:

* [Yüksek kullanılabilirlik](high-availability.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirlik](supportability.md)
