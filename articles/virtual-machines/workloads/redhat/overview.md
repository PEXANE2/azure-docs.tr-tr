---
title: Azure'a genel bakışta Red Hat iş yükleri | Microsoft Dokümanlar
description: Azure'da bulunan Red Hat ürün teklifleri hakkında bilgi edinin.
services: virtual-machines-linux
author: asinn826
manager: borisb2015
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: 718447e1dbf597af4349eab0be78a2bb544dec90
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78970167"
---
# <a name="red-hat-workloads-on-azure"></a>Azure'da Red Hat iş yükleri

Red Hat iş yükleri Azure'daki çeşitli tekliflerle desteklenir. Red Hat Enterprise Linux (RHEL) görüntüleri, Red Hat Update Infrastructure (RHUI) gibi RHEL iş yüklerinin merkezinde yer alır.

## <a name="red-hat-enterprise-linux-images"></a>Red Hat Enterprise Linux görüntüleri

Azure, Azure'da geniş bir RHEL görüntüsü sunar. Bu görüntüler iki farklı lisans modeli aracılığıyla kullanılabilir: pay-as-you-go ve bring-your-own-abonelik (BYOS). Azure'daki yeni RHEL görüntüleri, gerektiğinde yeni RHEL sürümleri yayımlandığında ve yaşam döngüleri boyunca güncelleştirildiğinde yayımlanır.

### <a name="pay-as-you-go-images"></a>Öde-as-you-go görüntüler

Azure, çeşitli RHEL öde görüntüleri sunar. Bu görüntüler RHEL için uygun şekilde sunulur ve bir güncelleştirme kaynağına (Red Hat Update Infrastructure) eklenir. Bu görüntüler, RHEL yetkilendirmesi ve güncellemeleri için bir prim ücreti alır. RHEL öde-as-you-go görüntü türevleri şunlardır:

* Standart RHEL.
* SAP için RHEL.
* Yüksek Kullanılabilirlik ve Güncelleme Hizmetleri ile SAP için RHEL.

Uygun sayıda abonelik için ayrı ayrı ödeme konusunda endişelenmek istemiyorsanız, kullandıkça öde görüntülerini kullanmak isteyebilirsiniz.

### <a name="red-hat-gold-images"></a>Kırmızı Şapka Altın Görüntüler

Azure ayrıca Red Hat`rhel-byos`Gold Images () sunar. Bu görüntüler, varolan Red Hat abonelikleri olan ve bunları Azure'da kullanmak isteyen müşteriler için yararlı olabilir. Azure'da kullanabilmeniz için Red Hat Cloud Access için mevcut Red Hat aboneliklerinizi etkinleştirmeniz gerekir. Red Hat abonelikleriniz Bulut Erişimi için etkinleştirildiğinde ve uygunluk gereksinimlerini karşıladığında bu resimlere erişim otomatik olarak verilir. Bu görüntülerin kullanılması, müşterinin kullandıkça öde görüntülerini kullanmaktan doğacak çift faturalandırmadan kaçınmasına olanak tanır.
* Azure ile Bulut Erişimi için Red Hat aboneliklerinizi nasıl [etkinleştirmenizi](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/con-enable-subs)öğrenin.
* [Azure portalında, Azure CLI'de veya PowerShell cmdlet'te Red Hat Gold Görüntülerini nasıl bulalışta bulunun.](./byos.md)

> [!NOTE]
> Bir kullanıcı RHEL abonelikleri için iki kez ödeme yaptığında çift faturalandırma yapılır. Bu senaryo genellikle bir müşteri RHEL pay-as-you-go VM bir hak eklemek için Red Hat Subscription-Manager kullandığında olur. Örneğin, RHEL'e iki kez ödeme yaptığı için, rhel ödemesi için bir ÜCRET olarak ödeme resmine SAP paketleri için bir yetki eklemek için Abonelik Yöneticisi'ni kullanan bir müşteri dolaylı olarak iki kez faturalandırılır. Bir kez ödeme-as-you-go prim ücreti ve bir kez SAP abonelik üzerinden ödeme. Bu senaryo BYOS görüntü kullanıcılarının başına gelmez.

### <a name="generation-2-images"></a>Nesil 2 görüntüler

Nesil 2 sanal makineler (VM' ler), Generation 1 VM'lere kıyasla bazı yeni özellikler sağlar. Daha fazla bilgi için [Nesil 2 belgelerine](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2)bakın. RHEL görüntü perspektifinden en önemli fark, Generation 2 VM'lerin BIOS firmware arabirimi yerine UEFI kullanmasıdır. Ayrıca önyükleme zamanında ana önyükleme kaydı (MBR) yerine GUID Partition Table (GPT) kullanırlar. GPT kullanımı, diğer şeylerin yanı sıra, 2 TB'den daha büyük işletim sistemi disk boyutlarına izin verir. Buna ek olarak, [Mv2 serisi VM'ler](../../mv2-series.md) yalnızca Generation 2 görüntülerinde çalışır.

RHEL Generation 2 görüntüleri Azure Marketi'nde kullanılabilir. Azure CLI'yi kullandığınızda görünen tüm görüntülerin listesinde sku görüntüsünde "gen2"yi arayın. Generation 2 VM dağıtmak için VM dağıtım işleminde **Gelişmiş** sekmesine gidin.

## <a name="red-hat-update-infrastructure"></a>Red Hat Güncelleştirme Altyapısı

Azure, Red Hat Update Infrastructure'ı yalnızca istediğiniz kadar öde RHEL VM'ler için sağlar. RHUI, Red Hat CDN'lerinin etkili bir aynasıdır, ancak yalnızca Azure'un istediğiniz kadar öde RHEL VM'leri tarafından erişilebilir. Dağıttığınız RHEL görüntüsüne bağlı olarak uygun paketlere erişebilirsiniz. Örneğin, SAP görüntüsü için bir RHEL, temel RHEL paketlerine ek olarak SAP paketlerine de erişebilir.

### <a name="rhui-update-behavior"></a>RHUI güncelleme davranışı

RHUI'ye bağlı RHEL görüntüleri, bir `yum update` çalıştırıldığında varsayılan olarak RHEL'in en son küçük sürümüne bağlanır. Bu davranış, bir `yum update` işlem çalıştırılırsa BIR RHEL 7.4 VM'nin RHEL 7.7'ye yükseltilebileceği anlamına gelir. Bu davranış RHUI için tasarım gereğidir. Bu yükseltme davranışını azaltmak için normal RHEL depolarından [Genişletilmiş Güncelleştirme Desteği depolarına](./redhat-rhui.md#rhel-eus-and-version-locking-rhel-vms)geçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'daki RHEL görüntüleri](./redhat-images.md)hakkında daha fazla bilgi edinin.
* [Red Hat Update Infrastructure](./redhat-rhui.md)hakkında daha fazla bilgi edinin.
* Red Hat [Gold Image (`rhel-byos`) teklifi](./byos.md)hakkında daha fazla bilgi edinin.
