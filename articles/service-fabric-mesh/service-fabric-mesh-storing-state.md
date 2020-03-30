---
title: Azure Hizmet Kumaş Kafesi'nde durum depolama seçenekleri
description: Azure Service Fabric Mesh'te çalışan Service Fabric Mesh uygulamalarında durumu güvenilir bir şekilde depolama hakkında bilgi edinin.
author: dkkapur
ms.author: dekapur
ms.date: 11/27/2018
ms.topic: conceptual
ms.openlocfilehash: 0b0bd611fa86d155bb5bf2e07808e7365e28871c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259103"
---
# <a name="state-management-with-service-fabric"></a>Hizmet Kumaşı ile Devlet Yönetimi

Service Fabric, devlet depolaması için birçok farklı seçeneği destekler. Devlet yönetim kalıpları ve Hizmet Kumaşı kavramsal bir bakış için, [Hizmet Kumaş Kavramları bakın: Devlet](/azure/service-fabric/service-fabric-concepts-state). Tüm bu aynı kavramlar, hizmetleriniz Servis Kumaş Kafesi içinde veya dışında çalışır. 

Service Fabric Mesh ile yeni bir uygulamayı kolayca dağıtabilir ve Azure'da barındırılan mevcut bir veri deposuna bağlayabilirsiniz. Herhangi bir uzak veritabanı kullanmanın yanı sıra, hizmetin yerel veya uzak depolama yı isteyip istemediklerine bağlı olarak veri depolamak için çeşitli seçenekler vardır. 

## <a name="volumes"></a>Birimler

Kapsayıcılar genellikle geçici disklerden kullanır. Geçici diskler geçicidir, ancak, yeni bir geçici disk alırsınız ve bir kapsayıcı çöktürüldüğünde bilgileri kaybedersiniz. Geçici diskler hakkındaki bilgileri diğer kapsayıcılarla paylaşmak da zordur. Birimler, durumu sürdürmek için kullanabileceğiniz kapsayıcı örneklerinin içine monte edilmiş dizinlerdir. Birimler size genel amaçlı dosya depolama sağlar ve normal disk G/Ç dosya API'lerini kullanarak dosyaları okuma/yazmanızı sağlar. Birim kaynağı, dizin nasıl monte edilebildiğini ve hangi destek depolamaalanının kullanılacağını açıklar. Verileri depolamak için Azure Dosya depolama veya Hizmet Kumaş Hacmi diski seçebilirsiniz.

![Birimler][image3]

### <a name="service-fabric-reliable-volume"></a>Servis Kumaşı Güvenilir Hacim

Service Fabric Reliable Volume, yerel bir birimi konteynere monte etmek için kullanılan docker hacim sürücüsüdür. Okuma ve yazma yerel operasyonlar ve hızlıdır. Veriler ikincil düğümlere çoğaltılarak yüksek oranda kullanılabilir hale getirilir. Failover da hızlıdır. Bir kapsayıcı kilitlendiğinde, verilerinizin bir kopyasına zaten sahip olan bir düğüm üzerinde başarısız olur. Örneğin, Service [Fabric Reliable Volume ile bir uygulamanın nasıl dağıtılanabildiğini](service-fabric-mesh-howto-deploy-app-sfreliable-disk-volume.md)görün.

### <a name="azure-files-volume"></a>Azure Dosyaları Hacmi

Azure Dosyaları Birimi, Bir Azure Dosyaları paylaşımını kapsayıcıya monte etmek için kullanılan docker birim sürücüsüdür. Azure Files depolama ağı depolama kullanır, bu nedenle okur ve yazma ağı üzerinde gerçekleşir. Service Fabric Reliable Volume ile karşılaştırıldığında, Azure Dosyaları depolama daha az performans gösterir, ancak daha ucuz ve tamamen güvenilir bir veri seçeneği sağlar. Örneğin, Azure [Dosyaları Hacmi ile bir uygulamanın nasıl dağıtılanabildiğini](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama modeli hakkında bilgi için [Service Fabric kaynaklarına](service-fabric-mesh-service-fabric-resources.md) bakın

[image3]: ./media/service-fabric-mesh-storing-state/volumes.png
