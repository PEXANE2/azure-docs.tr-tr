---
title: Yüksek kullanılabilirlik - Azure ayrılmış HSM | Microsoft Docs
description: Azure ayrılmış HSM yüksek kullanılabilirlik örneği ve temel konuları
services: dedicated-hsm
author: barclayn
manager: barbkess
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: barclayn
ms.openlocfilehash: bf55761bb58e309f919e08530fe047a137756323
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69656534"
---
# <a name="azure-dedicated-hsm-high-availability"></a>Azure ayrılmış HSM yüksek kullanılabilirlik

Azure ayrılmış HSM, Microsoft'un yüksek oranda kullanılabilir veri merkezleri tarafından underpinned. Bununla birlikte, yüksek oranda kullanılabilir herhangi bir veri merkezine yerelleştirilmiş hataları ve olağanüstü durumlarda, bölgesel düzeyindeki hatalar savunmasız durumdadır. Microsoft, HSM cihazlarına farklı veri merkezlerindeki birden çok cihaz sağlama tek bir rafa paylaşımı aygıtları götürmez emin olmak için bir bölge içinde dağıtır. Bu HSM 'ler, Gemalto HA Group özelliğini kullanarak bir bölgedeki veri merkezlerinde bu HSM 'leri eşleştirerek daha fazla yüksek kullanılabilirlik elde edilebilir. Ayrıca bir olağanüstü durum kurtarma durumda bölgesel yük devretme adres bölgeler arasında çifti cihazlara mümkündür. Bu çok katmanlı bir yüksek kullanılabilirlik yapılandırmasıyla, herhangi bir cihaz hata otomatik olarak çalışan uygulamaları korumak için getirilecektir. Başarısız olan herhangi bir CİHAZDAN vakitli şekilde değiştirilebilmesi tüm veri merkezlerinde de yedek aygıtlarının ve bileşenlerinin tesise var.

## <a name="high-availability-example"></a>Yüksek kullanılabilirlik örneği

Yüksek kullanılabilirlik için HSM cihazlarına yazılım düzeyinde yapılandırma konusunda bilgi 'Gemalto Luna ağ HSM Yönetim Kılavuzu'nda ' dir. Bu belge, [Gemalto HSM sayfasında](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/)bulunur.

Aşağıdaki diyagramda yüksek oranda kullanılabilir bir mimari gösterilmektedir. Birden çok bölgede hem de ayrı bir bölgede eşleştirilmiş birden çok cihazlarda kullanır. Bu mimari, en az dört HSM cihazlarına ve sanal ağ bileşenleri kullanır.

![Yüksek kullanılabilirlik diyagramı](media/high-availability/high-availability.png)

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ve güvenlik gibi hizmete tüm temel kavramlarını cihaz sağlama ve uygulama tasarım veya dağıtımınızın önce iyi anlaşıldığından önerilir.
Daha fazla kavramı düzey konular:

* [Dağıtım mimarisi](deployment-architecture.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirliği](supportability.md)
* [İzleme](monitoring.md)

HSM cihazlarına yüksek kullanılabilirlik için yapılandırma belirli Ayrıntılar için lütfen Yönetici kılavuzları için Gemalto müşteri desteği Portalı'na bakın ve 6 bölümüne bakın.
