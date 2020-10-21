---
title: Service Fabric yönetilen kümeler hakkında sık sorulan sorular
description: Yetenekler, kullanım örnekleri ve yaygın senaryolar dahil Service Fabric yönetilen kümeler hakkında sık sorulan sorular.
ms.topic: troubleshooting
ms.author: pepogors
author: peterpogorski
ms.date: 09/28/2020
ms.custom: references_regions
ms.openlocfilehash: 4dc41d2c13c834657534971041440bb744cfca38
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92319833"
---
# <a name="service-fabric-managed-clusters-frequently-asked-questions"></a>Service Fabric yönetilen kümeler hakkında sık sorulan sorular

Service Fabric yönetilen kümelerin (Önizleme) neler yapabileceğini ve bunların nasıl kullanılacağını gösteren çok sık sorulan sorular vardır. Bu belge, yaygın soruların ve bunların yanıtlarının çoğunu içerir.

## <a name="general"></a>Genel

### <a name="what-are-service-fabric-managed-clusters"></a>Service Fabric yönetilen kümeler nelerdir?

Service Fabric yönetilen kümeler, kümeleri dağıtmayı ve yönetmeyi kolaylaştırmak için tasarlanan Service Fabric kümesi kaynak modelinin bir gelişmidir. Service Fabric yönetilen bir küme, bir kullanıcının bugün dağıtmaları gereken çok sayıda bağımsız kaynakla (sanal makine ölçek kümesi, Load Balancer, IP ve daha fazlası) yalnızca tek bir küme kaynağı tanımlayıp dağıtabilmesi için Azure Resource Manager kapsülleme modelini kullanır.

### <a name="what-regions-are-supported-in-the-preview"></a>Önizlemede hangi bölgeler destekleniyor?

Service Fabric yönetilen kümeler önizlemesi için desteklenen bölgeler,,,,, `centraluseuap` `eastus2euap` ve içerir `eastasia` `northeurope` `westcentralus` `eastus2` .

### <a name="can-i-do-an-in-place-migration-of-my-existing-service-fabric-cluster-to-a-managed-cluster-resource"></a>Mevcut Service Fabric Kümemin bir yönetilen küme kaynağına yerinde geçişini yapabilir miyim?

Bu sırada, yeni Service Fabric yönetilen küme kaynak türünü kullanmak için yeni bir Service Fabric küme kaynağı oluşturmanız gerekir.

### <a name="is-there-an-additional-cost-for-service-fabric-managed-clusters"></a>Service Fabric yönetilen kümeler için ek bir ücret var mı?

Hayır, küme için gereken temel işlem, depolama ve ağ kaynaklarının maliyetinin ötesinde Service Fabric yönetilen bir kümeyle ilişkili ek bir maliyet yoktur.

### <a name="is-there-a-new-sla-introduced-by-the-service-fabric-managed-cluster-resource"></a>Service Fabric yönetilen küme kaynağı tarafından tanıtılan yeni bir SLA var mı?

SLA geçerli Service Fabric kaynak modelinden değişiklik yapmaz.

### <a name="what-is-the-difference-between-a-basic-and-standard-sku-cluster"></a>Temel ve standart SKU kümesi arasındaki fark nedir?

Temel bir SKU kümesi, yapılandırmaların çoğunun Service Fabric kaynak sağlayıcısı tarafından sağlandığı anlamına gelir. Temel SKU kümelerinin test ve üretim öncesi ortamları için kullanılması amaçlanmıştır. Standart SKU kümesi, kullanıcıların, kendi ihtiyaçlarını karşılamak için kümeyi yapılandırmasına olanak sağlar. Daha fazla bilgi için bkz. [Service Fabric yönetilen küme SKU 'ları](./overview-managed-cluster.md#service-fabric-managed-cluster-skus) .

## <a name="cluster-deployment-and-management"></a>Küme dağıtımı ve yönetimi

### <a name="i-run-custom-script-extensions-on-my-virtual-machine-scale-set-can-i-continue-to-do-that-with-a-managed-service-fabric-resource"></a>Sanal makine ölçek kümesi 'nde özel Betik uzantıları çalıştırdım, bunu yönetilen bir Service Fabric kaynağıyla yapmaya devam edebilir miyim?

Evet ' de, bir düğüm türünde VM uzantıları belirtebilirsiniz. Daha fazla bilgi için bkz. daha fazla ayrıntı için düğüm türü uzantısı örneği.

### <a name="i-want-to-have-an-internal-only-load-balancer-is-that-possible"></a>Yalnızca iç yük dengeleyiciye sahip olmak istersem bu mümkün midir?

Şu anda yalnızca iç yük dengeleyiciye sahip olmak mümkün değildir. İstenmeyen gelen/giden trafiği engellemek için ağ güvenlik grubu kurallarını kilitlemeyi öneririz.

### <a name="can-i-autoscale-my-cluster"></a>Kümemi otomatik ölçeklendirme yapabilir miyim? 
Otomatik ölçeklendirme Şu anda önizlemede kullanılamıyor.

### <a name="can-i-deploy-my-cluster-across-availability-zones"></a>Kümemi kullanılabilirlik bölgeleri arasında dağıtabilir miyim? 
Çapraz kullanılabilirlik bölgesi kümeleri Şu anda önizlemede yok.

### <a name="can-i-select-between-automatic-and-manual-upgrades-for-my-cluster-runtime"></a>Küme çalışma zamanı için otomatik ve el ile yükseltme arasında seçim yapabilir miyim? 
Önizlemede, tüm çalışma zamanı yükseltmeleri otomatik olarak tamamlanır.

## <a name="applications"></a>Uygulamalar

### <a name="is-there-a-local-development-experience-for-service-fabric-managed-clusters"></a>Service Fabric yönetilen kümeler için yerel bir geliştirme deneyimi var mı?

Yerel geliştirme deneyimi, mevcut Service Fabric kümelerinden değişmeden kalır. Daha fazla bilgi için bkz. yerel geliştirme deneyimi hakkında daha fazla bilgi için bkz. [.NET uygulaması oluşturma](./service-fabric-quickstart-dotnet.md) .

### <a name="can-i-deploy-my-applications-as-an-azure-resource-manager-resource"></a>Uygulamalarımı bir Azure Resource Manager kaynağı olarak dağıtabilir miyim?

Önizlemede, uygulamaları Azure Resource Manager kaynak olarak dağıtamazsınız. Uygulamalar, PowerShell veya CLı aracılığıyla doğrudan kümeye bağlanarak dağıtılmalıdır. Bu işlev, Service Fabric kümeleri genel kullanıma girmeden önce eklenecektir.