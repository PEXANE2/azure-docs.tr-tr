---
title: Yüksek kullanılabilirlik - Azure Özel HSM | Microsoft Dokümanlar
description: Azure Özel HSM yüksek kullanılabilirlik örneği ve temel hususlar
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: mbaldwin
ms.openlocfilehash: 536ef62acad900090924598edfa45450b2a8c951
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70882256"
---
# <a name="azure-dedicated-hsm-high-availability"></a>Azure Özel HSM yüksek kullanılabilirlik

Azure Özel HSM, Microsoft'un yüksek oranda kullanılabilen veri merkezleri tarafından desteklenmektedir. Ancak, yüksek kullanılabilirlikteki herhangi bir veri merkezi yerelleştirilmiş hatalara ve olağanüstü durumlarda bölgesel düzey hatalarına karşı savunmasızdır. Microsoft, birden çok aygıtın sağlanmasının bu aygıtların tek bir rafı paylaşmasına yol açmamasını sağlamak için HSM aygıtlarını bir bölgedeki farklı veri merkezlerinde dağıtır. Gemalto HA Group özelliğini kullanarak bu HSM'leri bölgedeki veri merkezleri arasında eşleştirerek daha yüksek kullanılabilirlik düzeyi elde edilebilir. Ayrıca, bir olağanüstü durum kurtarma durumunda bölgesel başarısızlık ları gidermek için aygıtları bölgeler arasında eşleştirmek de mümkündür. Bu çok katmanlı yüksek kullanılabilirlik yapılandırması sayesinde, uygulamaların çalışmaya devam etmesi için herhangi bir aygıt hatası otomatik olarak giderilir. Tüm veri merkezlerinin yerinde yedek aygıtları ve bileşenleri de vardır, böylece başarısız olan aygıtlar zamanında değiştirilebilir.

## <a name="high-availability-example"></a>Yüksek kullanılabilirlik örneği

HSM aygıtlarının yazılım düzeyinde yüksek kullanılabilirlik için nasıl yapılandırılabilen bilgiler 'Gemalto Luna Network HSM Yönetim Kılavuzu'nda yer almaktadır. Bu belge [gemalto HSM Sayfasında](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/)mevcuttur.

Aşağıdaki diyagram, kullanılabilir bir mimari gösterir. Bölgede birden çok aygıt ve ayrı bir bölgede eşleştirilmiş birden çok aygıt kullanır. Bu mimari, en az dört HSM aygıtı ve sanal ağ bileşeni kullanır.

![Yüksek kullanılabilirlik diyagramı](media/high-availability/high-availability.png)

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ve güvenlik gibi hizmetin tüm temel kavramlarının, cihaz sağlama ve uygulama tasarımı veya dağıtımından önce iyi anlaşılması önerilir.
Diğer kavram düzeyi konuları:

* [Dağıtım Mimarisi](deployment-architecture.md)
* [Fiziksel Güvenlik](physical-security.md)
* [Ağ Oluşturma](networking.md)
* [Desteklenebilirlik](supportability.md)
* [İzleme](monitoring.md)

Yüksek kullanılabilirlik için HSM aygıtlarının yapılandırılmasıyla ilgili özel ayrıntılar için, lütfen Yönetici Kılavuzları için Gemalto Müşteri Destek Portalı'na bakın ve bölüm 6'ya bakın.
