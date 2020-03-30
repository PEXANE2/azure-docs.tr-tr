---
title: Azure DNS özel bölgelerinin sanal ağ bağlantısı alt kaynağı nedir
description: Sanal ağ bağlantısı alt kaynağına genel bakış bir Azure DNS özel bölgesi
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 9/24/2019
ms.author: rohink
ms.openlocfilehash: 9181ef93dfedbc28b297bef48a0bc37ba6d69798
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646770"
---
# <a name="what-is-a-virtual-network-link"></a>Sanal ağ bağlantısı nedir?

Azure'da özel bir DNS bölgesi oluşturduktan sonra, herhangi bir sanal ağdan hemen erişilemez. Bu ağda barındırılan bir VM özel DNS bölgesine erişebilmek için sanal ağa bağlamanız gerekir.
Özel bir DNS bölgesini sanal ağa bağlamak için, özel DNS bölgesi altında bir sanal ağ bağlantısı oluşturmanız gerekir. Her özel DNS bölgesi, sanal ağ bağlantısı alt kaynakları koleksiyonuna sahiptir. Bu kaynakların her biri sanal ağa bir bağlantı temsil eder.

Sanal ağı kayıt sanal ağı olarak veya çözümlü sanal ağ olarak özel bir DNS bölgesine bağlayabilirsiniz.

## <a name="registration-virtual-network"></a>Kayıt sanal ağ

Özel bir DNS bölgesi ile sanal ağ arasında [bir bağlantı oluşturduğunuzda,](https://docs.microsoft.com/azure/dns/private-dns-getstarted-portal#link-the-virtual-network) sanal makineler için DNS kayıtlarının otomatik [kaydını](./private-dns-autoregistration.md) açma seçeneğiniz vardır. Bu seçeneği seçerseniz, sanal ağ özel DNS bölgesi için bir kayıt sanal ağ olur. Ağda dağıttığınız sanal makineler için otomatik olarak bir DNS kaydı oluşturulur. DNS kayıtları, sanal ağda zaten dağıttığınız sanal makineler için oluşturulur. Sanal ağ açısından bakıldığında, özel DNS bölgesi bu sanal ağ için kayıt bölgesi olur.
Bir özel DNS bölgesi birden çok kayıt sanal ağına sahip olabilir, ancak her sanal ağ onunla ilişkili tam olarak bir kayıt bölgesine sahip olabilir.

## <a name="resolution-virtual-network"></a>Çözünürlük sanal ağ

Özel bir DNS bölgesi altında sanal ağ bağlantısı oluşturduğunuzda ve DNS kaydı otomatik kaydını etkinleştirmemeyi seçtiğinizde, sanal ağ yalnızca bir çözünürlük yalnızca sanal ağ olarak kabul edilir. Bu tür ağlarda dağıtılan sanal makinelerin DNS kayıtları, bağlantılı özel DNS bölgesinde otomatik olarak oluşturulmaz. Ancak, böyle bir ağda dağıtılan sanal makineler, özel DNS bölgesinden DNS kayıtlarını başarıyla sorgulayabilir. Bu kayıtlar sizin tarafından el ile oluşturulabilir veya özel DNS bölgesine kayıt ağı olarak bağlanan diğer sanal ağlardan doldurulabilir.
Bir özel DNS bölgesi birden çok çözünürlüklü sanal ağa sahip olabilir ve sanal ağ da ilişkili birden çok çözüm bölgesi ne olabilir.

## <a name="limits"></a>Sınırlar

Kaç kayıt ve çözüm ağı olduğunu anlamak için [Azure DNS Sınırları'nı](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-dns-limits) görebilirsiniz: özel DNS bölgelerine bağlanabilirsiniz

## <a name="other-considerations"></a>Diğer konular

* Klasik dağıtım modeli kullanılarak dağıtılan sanal ağlar desteklenmez.

* Özel bir DNS bölgesi ile sanal ağ arasında yalnızca bir bağlantı oluşturabilirsiniz.

* Özel bir DNS bölgesi altındaki her sanal ağ bağlantısı, özel DNS bölgesi bağlamında benzersiz bir ada sahip olmalıdır. Farklı özel DNS bölgelerinde aynı ada bağlantılar alabilirsiniz.

* Sanal ağ bağlantısı oluşturduktan sonra, sanal ağ bağlantısı kaynağının "Bağlantı Durumu" alanını kontrol edin. Sanal ağın boyutuna bağlı olarak, bağlantının çalışması ve Bağlantı *Durumu'nun Tamamlandı'ya*değiştirilmesi birkaç dakika sürebilir.

* Bir sanal ağı sildiğinizde, farklı özel DNS bölgelerinde onunla ilişkili tüm sanal ağ bağlantıları ve otomatik olarak kaydedilmiş DNS kayıtları otomatik olarak silinir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalLarını](https://docs.microsoft.com/azure/dns/private-dns-getstarted-portal#link-the-virtual-network) kullanarak sanal ağı özel bir DNS bölgesine nasıl bağlayacaklarınız öğrenin

* [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI'yi](./private-dns-getstarted-cli.md)kullanarak Azure DNS'de nasıl özel bir bölge oluşturabilirsiniz öğrenin.

* Azure DNS'deki özel bölgelerle gerçekleştirilebilecek bazı yaygın [özel bölge senaryoları](./private-dns-scenarios.md) hakkında bilgi edinin.

* Belirli işlem türleri için bekleyebileceğiniz belirli davranışlar da dahil olmak üzere Azure DNS'deki özel bölgelerle ilgili sık sorulan sorular ve yanıtlar için [Bkz.](./dns-faq-private.md)
