---
title: Yüksek kullanılabilirlik ve olağanüstü durum kurtarma - Azure Toplu İş
description: Toplu İşlem uygulamanızı bölgesel bir kesinti için nasıl tasarlayın öğrenin. İş yükleri başka bir bölgeye devredilmeli veya iki veya daha fazla bölge arasında bölünmelidir.
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/29/2019
ms.author: labrenne
ms.openlocfilehash: 84b0cce9557b4ae05586579f175cd0f5db14fdfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77026090"
---
# <a name="design-your-application-for-high-availability"></a>Uygulamanızı yüksek kullanılabilirlik için tasarlama

Azure Toplu İşlem bölgesel bir hizmettir. Toplu iş tüm Azure bölgelerinde kullanılabilir, ancak toplu iş hesabı oluşturulduğunda bir bölgeyle ilişkilendirilmesi gerekir. Toplu İşlem hesabı için tüm işlemler daha sonra bu bölgeye uygulanır. Örneğin, havuzlar ve ilişkili sanal makineler (VM'ler) Toplu İşlem hesabıyla aynı bölgede oluşturulur.

Toplu Iş kullanan bir uygulama tasarlarken, Toplu İşlem'in bir bölgede kullanılamaması olasılığını göz önünde bulundurmanız gerekir. Bölge yle ilgili bir sorun, bölgedeki tüm Toplu İşlem hizmeti veya belirli Toplu İşlem hesabınızla ilgili bir sorun olduğu nadir bir durumla karşılaşmak mümkündür.

Toplu İşlem'i kullanan uygulama veya çözümün her zaman kullanılabilir olması gerekiyorsa, başka bir bölgeye geçemeyecek veya iş yükünün her zaman iki veya daha fazla bölge arasında bölünmesi için tasarlanmalıdır. Her iki yaklaşım da, her hesap farklı bir bölgede bulunan en az iki Toplu Işlem hesabı gerektirir.

## <a name="multiple-batch-accounts-in-multiple-regions"></a>Birden çok bölgede birden çok Toplu Işlem hesabı

Çeşitli bölgelerde birden çok Toplu İşlem hesabı kullanmak, başka bir bölgedeki toplu iş hesabı kullanılamıyorsa uygulamanızın çalışmaya devam etmesini sağlar. Uygulamanızın çok kullanılabilir olması gerekiyorsa, birden çok hesap kullanmak özellikle önemlidir.

Bazı durumlarda, bir uygulama her zaman iki veya daha fazla bölge kullanmak üzere tasarlanmış olabilir. Örneğin, önemli miktarda kapasiteye ihtiyacınız olduğunda, büyük ölçekli bir uygulamayı işlemek veya gelecekteki büyümeyi karşılamak için birden çok bölge kullanmak gerekebilir.

## <a name="design-considerations-for-providing-failover"></a>Başarısız lık sağlamak için tasarım konuları

Alternatif bir bölgeye başarısız olma olanağı sağlarken göz önünde bulundurulması gereken önemli bir nokta, bir çözümdeki tüm bileşenlerin dikkate alınması gerektiğidir; sadece ikinci bir Toplu hesap olması yeterli değildir. Örneğin, çoğu Toplu İşlem uygulamasında, depolama hesabı ve Toplu Iş hesabının kabul edilebilir performans için aynı bölgede olması gereken bir Azure depolama hesabı gereklidir.

Başarısız olabilecek bir çözüm tasarlarken aşağıdaki noktaları göz önünde bulundurun:

- Toplu Iş hesabı ve depolama hesabı gibi her bölgedeki gerekli tüm hesapları önceden oluşturun. Hesapların oluşturulması için genellikle herhangi bir ücret alınmaz, yalnızca depolanan veriler olduğunda veya hesap kullanıldığında.
- Kotaların hesaplarda önceden ayarlandığınızdan emin olun, böylece Toplu İş hesabını kullanarak gerekli çekirdek sayısını ayırabilirsiniz.
- Uygulamanın bir bölgede dağıtımını otomatikleştirmek için şablonları ve/veya komut dosyalarını kullanın.
- Uygulama ikililerini ve başvuru verilerini tüm bölgelerde güncel tutun. Güncel kalmak, dosyaların yüklenmesini ve dağıtılmasını beklemek zorunda kalmadan bölgenin hızlı bir şekilde çevrimiçi olmasını sağlayacaktır. Örneğin, havuz düğümlerine yüklenecek özel bir uygulama Toplu uygulama paketleri kullanılarak depolanır ve başvurulursa, uygulamanın yeni bir sürümü üretildiğinde, her Toplu Işlem hesabına yüklenmeli ve havuz yapılandırması tarafından başvurulmalıdır (veya yeni sürümü varsayılan sürüm yapın).
- Toplu Iş, depolama ve diğer hizmetleri çağıran uygulamada, istemciler veya farklı bölgeye yük kolayca geçiş.
- Bir başarısızlık başarılı olmasını sağlamak için en iyi uygulama, normal işlemin bir parçası olarak sık sık alternatif bir bölgeye geçmektir. Örneğin, ayrı bölgelerde ki iki dağıtımla, her ay alternatif bölgeye geçiş yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalı, Azure](batch-account-create-portal.md) [CLI,](cli-samples.md) [PowerShell](batch-powershell-cmdlets-get-started.md)veya [Toplu İşlem yönetimi API'si](batch-management-dotnet.md)ile Toplu Iş hesapları oluşturma hakkında daha fazla bilgi edinin.
- Varsayılan kotalar bir Toplu Iş hesabıyla ilişkilidir; [Bu makalede](batch-quota-limit.md) varsayılan kota değerleri ayrıntılı olarak açıklanır ve kotaların nasıl artırılabildiği açıklanır.
