---
title: Soyut kapsayıcı güvenliği Microsoft Azure
description: Microsoft Azure teknik incelemesindeki kapsayıcı güvenliği için Özet.
author: TomShinder
ms.author: TomSh
ms.date: 09/05/2018
ms.topic: article
ms.service: security
ms.subservice: security-fundamentals
ms.openlocfilehash: b70744f403c483448a844d3f3decf6ce26f48f06
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727791"
---
# <a name="container-security-in-microsoft-azure"></a>Microsoft Azure Kapsayıcı güvenliği
## <a name="abstract"></a>Özet

Kapsayıcı teknolojisi, bulut bilgi işlem dünyasında yapısal değişikliğe neden oluyor. Kapsayıcılar, bir uygulamanın birden çok örneğini bir işletim sisteminin tek bir örneğinde çalıştırmanızı sağlar, böylece kaynakları daha verimli bir şekilde kullanmaktır. Kapsayıcılar, kuruluşların tutarlılığını ve esnekliğini sağlar. Uygulama, bir sanal makinede test edilmiş, bir sanal makinede test edildiğinden ve daha sonra bulutta üretime dağıtılabileceğinden sürekli dağıtımı etkinleştirir. Kapsayıcılar, kaynak iyileştirmesi nedeniyle çeviklik, kolaylaştırılmış işlemler, ölçeklenebilirlik ve düşük maliyetler sağlar.

Kapsayıcı teknolojisi görece yeni olduğundan, çoğu BT uzmanı, bir üretim ortamında görünürlük ve kullanım eksikliği hakkında güvenlik kaygıları vardır. Geliştirme ekipleri genellikle en iyi güvenlik uygulamaları hakkında farkında değildir. Bu Teknik İnceleme, güvenlik işlemleri ekiplerine ve geliştiricilere Microsoft Azure platformunda kapsayıcı geliştirme ve dağıtımların güvenliğini sağlamaya yönelik yaklaşımlar seçme konusunda yardımcı olabilir.

Bu raporda kapsayıcılar, kapsayıcı dağıtımı ve yönetimi ile yerel platform hizmetleri açıklanmaktadır. Ayrıca, Azure platformunda kapsayıcılar kullanımıyla ilgili çalışma zamanı güvenlik sorunlarını açıklar. Şekil ve örneklerde, bu kağıt kapsayıcı Düzenleyicisi olarak Docker ve Kubernetes 'e odaklanır. Çoğu güvenlik önerisi, Azure platformunda Microsoft iş ortaklarının diğer kapsayıcı modelleriyle de geçerlidir.

[Teknik incelemeyi indirin](https://azure.microsoft.com/mediahandler/files/resourcefiles/container-security-in-microsoft-azure/Open%20Container%20Security%20in%20Microsoft%20Azure.pdf)