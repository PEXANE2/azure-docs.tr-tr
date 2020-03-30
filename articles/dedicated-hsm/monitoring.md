---
title: İzleme seçenekleri - Azure Özel HSM | Microsoft Dokümanlar
description: Azure Özel HSM izleme seçeneklerine ve izleme sorumluluklarına genel bakış
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70881428"
---
# <a name="azure-dedicated-hsm-monitoring"></a>Azure Özel HSM izleme

Azure Özel HSM Hizmeti, tüm yönetim kontrolü ve yönetim sorumluluğuyla tek müşteri kullanımı için fiziksel bir aygıt sağlar. Cihaz kullanılabilir hale [bir Gemalto SafeNet Luna 7 HSM modeli A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/).  Microsoft, bir izleme rolü olarak fiziksel seri bağlantı noktası eki dışında, bir müşteri tarafından sağlanan bir kez hiçbir yönetim erişimine sahip olacaktır. Sonuç olarak, müşteriler kapsamlı izleme ve günlük analizi de dahil olmak üzere tipik operasyonel faaliyetlerden sorumludur.
Müşteriler HSM'leri kullanan uygulamalardan tamamen sorumludur ve destek veya danışmanlık yardımı için Gemalto ile birlikte çalışmalıdır. Operasyonel hijyenin müşteri sahipliğinin kapsamı nedeniyle, Microsoft'un bu hizmet için herhangi bir yüksek kullanılabilirlik garantisi sunması mümkün değildir. Yüksek kullanılabilirlik elde etmek için uygulamalarının doğru şekilde yapılandırıldığından emin olmak müşterinin sorumluluğundadır. Microsoft, aygıt durumunu ve ağ bağlantısını izleyecek ve koruyacaktır.

## <a name="microsoft-monitoring"></a>Microsoft izleme

Kullanılan Gemalto SafeNet aygıtı, varsayılan olarak snmp ve seri bağlantı noktasına aygıtı izlemek için seçenekler olarak sahiptir. Microsoft, aygıt durumu temel telemetri almak için aygıta bağlanmak için fiziksel bir araç olarak seri bağlantı noktası bağlantısı kullanmıştır. Bu, güç kaynakları ve fanlar gibi sıcaklık ve bileşen durumu gibi öğeleri içerir.
Bunu başarmak için Microsoft, Gemalto aygıtında ayarlanan yönetim dışı bir "monitör" rolünü kullanır. Bu rol, telemetriyi alma olanağı sağlar, ancak yönetim görevi veya şifreleme bilgilerini görüntüleme açısından aygıta herhangi bir erişim sağlamaz. Müşterilerimiz, hassas şifreleme anahtar depolamasını yönetmek, yönetmek ve kullanmak için cihazlarının gerçekten kendilerine ait olduğundan emin olabilir. Herhangi bir müşterinin temel sistem durumu izleme için bu minimum erişimden memnun olmaması durumunda, izleme hesabını devre dışı bırakma seçeneğine sahiptir. Bunun bariz sonucu, Microsoft'un hiçbir bilgiye sahip olmaması ve dolayısıyla cihaz sağlık sorunları hakkında herhangi bir proaktif bildirim sağlama olanağı nın olmamasıdır. Bu durumda, müşteri cihazın sağlığından sorumludur.
Monitör işlevi, sistem durumu verilerini almak için aygıtı her 10 dakikada bir yoklamak üzere ayarlanmıştır. Seri iletişimin hataya yatkın doğası nedeniyle, yalnızca bir saatlik bir süre içinde birden fazla negatif sağlık göstergesinden sonra bir uyarı yükseltilir. Bu uyarı, sonuçta sorunu bildiren proaktif bir müşteri iletişimine yol açacak.
Sorunun niteliğine bağlı olarak, etkiyi azaltmak ve düşük riskli düzeltmeyi sağlamak için uygun eylem rotası alınacaktır. Örneğin, güç kaynağı arızası, düşük etki ve çalışma riski ile gerçekleştirilebildiği için, hiçbir sonucu kurcalama olayı olmayan sıcak takas işlemidir. Diğer yordamlar, müşteri için herhangi bir güvenlik riskini en aza indirmek için aygıtın sıfırlatılmasını ve hükmün bozulmasını gerektirebilir. Bu durumda, bir müşteri alternatif bir aygıt sağlar, yüksek kullanılabilirlik eşleştirmesi ile aygıt eşitlemi tetikler. Normal çalışma en az sürede, en az kesinti ve en düşük güvenlik riski ile devam edeceğini.  

## <a name="customer-monitoring"></a>Müşteri izleme

Özel HSM hizmetinin bir değer teklifi, müşterinin aygıtın aldığı denetimdir, özellikle de bulut teslim edilen bir aygıt olduğunu göz önünde bulundurarak. Bu kontrolün bir sonucu, cihazın durumunu izlemek ve yönetmekle yükümlüdür. Gemalto SafeNet cihazı, SNMP ve Syslog uygulaması için kılavuzla birlikte gelir. Özel HSM hizmetinin müşterileri, Microsoft monitör hesabı etkin kalsa bile bunu kullanmaları ve Microsoft monitör hesabını devre dışı kılırsa bunu zorunlu kabul etmeleri önerilir.
Kullanılabilir her iki teknik de, müşterinin sorunları tanımlamasına ve uygun düzeltme çalışmasını başlatmak için Microsoft desteğini aramasına olanak sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Örneğin yüksek kullanılabilirlik ve güvenlik gibi hizmetin tüm temel kavramlarının, herhangi bir cihaz sağlama ve uygulama tasarımı veya dağıtımından önce iyi anlaşılması önerilir. Diğer kavram düzeyi konuları:

* [Yüksek Kullanılabilirlik](high-availability.md)
* [Fiziksel Güvenlik](physical-security.md)
* [Ağ Oluşturma](networking.md)
* [Desteklenebilirlik](supportability.md)
