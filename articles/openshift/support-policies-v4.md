---
title: Azure Red Hat OpenShift 4 küme destek ilkesi
description: Red Hat OpenShift 4 için destek ilkesi gereksinimlerini anlayın.
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: ec27d054055866c72148ad6eb024d4324f063ce8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83774402"
---
# <a name="azure-red-hat-openshift-support-policy"></a>Azure Red Hat OpenShift destek ilkesi

Azure Red Hat OpenShift 4 kümelerine yönelik belirli yapılandırmalarda, kümenizin desteklenebilirliği etkilenebilir. Azure Red Hat OpenShift 4, küme yöneticilerinin iç küme bileşenlerinde değişiklik yapmasına izin verir, ancak tüm değişiklikler desteklenmez. Aşağıdaki destek ilkesi, hangi değişikliklerin ilkeyi ihlal edeceğini ve Microsoft ve Red Hat 'in void desteğini paylaşır.

> [!NOTE]
> OpenShift kapsayıcı platformunda teknoloji önizlemesi işaretli özellikler Azure Red Hat OpenShift 'te desteklenmez.

## <a name="cluster-configuration-requirements"></a>Küme yapılandırma gereksinimleri

* Tüm OpenShift kümesi işleçleri yönetilen bir durumda kalmalıdır. Küme işleçleri listesi, çalıştırılarak döndürülür `oc get clusteroperators` .
* Prometheus ve Alertmanager hizmetlerini kaldırmayın veya değiştirmeyin.
* Service Alertmanager kurallarını kaldırmayın.
* OpenShift küme sürümünü değiştirmeyin.
* Azure Red Hat OpenShift hizmet günlüğü 'nü (MDSD pods) kaldırmayın veya değiştirmeyin.
* ' Arosvc.azurecr.io ' küme çekme parolasını kaldırmayın veya değiştirmeyin.
* Tüm küme sanal makinelerinin, en azından Azure Resource Manager (ARM) ve hizmet günlüğü (Genfiliz) uç noktalarına doğrudan giden internet erişimi olması gerekir.  HTTPS proxy için hiçbir form desteklenmez.
* Kümenin sanal ağının DNS yapılandırmasını değiştirmeyin. Varsayılan Azure DNS Çözümleyicisinin kullanılması gerekir.
* Kümenin MachineConfig nesnelerinden hiçbirini (örneğin, kubelet yapılandırması) herhangi bir şekilde geçersiz kılmayın.
* Azure Red Hat OpenShift hizmeti, kümenize özel bağlantı hizmeti aracılığıyla erişir.  Hizmet erişimini kaldırmayın veya değiştirmeyin.
* RHCOS olmayan işlem düğümleri desteklenmez. Örneğin, bir RHEL işlem düğümü kullanamazsınız.

## <a name="supported-virtual-machine-sizes"></a>Desteklenen sanal makine boyutları

Azure Red Hat OpenShift 4, aşağıdaki sanal makine boyutlarında çalışan düğümü örneklerini destekler:

### <a name="general-purpose"></a>Genel amaçlı

|Seriler|Boyut|Sanal işlemci|Bellek: GiB|
|-|-|-|-|
|Dasv4|Standard_D4as_v4|4|16|
|Dasv4|Standard_D8as_v4|8|32|
|Dasv4|Standard_D16as_v4|16|64|
|Dasv4|Standard_D32as_v4|32|128|
|Dsv3|Standard_D4s_v3|4|16|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|

### <a name="memory-optimized"></a>Bellek için iyileştirilmiş

|Seriler|Boyut|Sanal işlemci|Bellek: GiB|
|-|-|-|-|
|Esv3|Standard_E4s_v3|4|32|
|Esv3|Standard_E8s_v3|8|64|
|Esv3|Standard_E16s_v3|16|128|
|Esv3|Standard_E32s_v3|32|256|

### <a name="compute-optimized"></a>İşlem için iyileştirilmiş

|Seriler|Boyut|Sanal işlemci|Bellek: GiB|
|-|-|-|-|
|Fsv2|Standard_F4s_v2|4|8|
|Fsv2|Standard_F8s_v2|8|16|
|Fsv2|Standard_F16s_v2|16|32|
|Fsv2|Standard_F32s_v2|32|64|

### <a name="master-nodes"></a>Ana düğümler

|Seriler|Boyut|Sanal işlemci|Bellek: GiB|
|-|-|-|-|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|
