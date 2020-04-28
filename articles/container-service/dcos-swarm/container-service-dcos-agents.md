---
title: Kullanım DıŞı Azure Container Service için DC/OS aracı havuzları
description: Ortak ve özel aracı havuzları Azure Container Service DC/OS kümesi ile nasıl çalışır?
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 01/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bb9b33bf537ebd5a563f8e8a8afd45cd2e5b292d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76278377"
---
# <a name="deprecated-dcos-agent-pools-for-azure-container-service"></a>Kullanım DıŞı Azure Container Service için DC/OS aracı havuzları

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Azure Container Service içindeki DC/OS kümeleri, genel bir havuz ve özel havuz olmak üzere iki havuzdaki aracı düğümleri içerir. Bir uygulama, kapsayıcı hizmetinizdeki makineler arasındaki erişilebilirliği etkileyen her iki havuza de dağıtılabilir. Makineler Internet 'e (genel) sunulabilir veya iç (özel) tutulur. Bu makale, genel ve özel havuzların neden olduğuna ilişkin kısa bir genel bakış sunar.


* **Özel aracılar**: özel aracı düğümleri yönlendirilemeyen bir ağ aracılığıyla çalışır. Bu ağa yalnızca yönetim bölgesinden veya genel bölge sınır yönlendiricisinden erişilebilir. Varsayılan olarak, DC/OS özel aracı düğümlerinde uygulamalar başlatır. 

* **Ortak aracılar**: genel aracı düğümleri, genel olarak erişilebilen bir ağ üzerinden DC/OS uygulamaları ve Hizmetleri çalıştırır. 

DC/OS ağ güvenliği hakkında daha fazla bilgi için bkz. [DC/OS belgeleri](https://docs.mesosphere.com/).

## <a name="deploy-agent-pools"></a>Aracı havuzlarını dağıtma

Azure Container Service Içindeki DC/OS aracı havuzları şu şekilde oluşturulur:

* **Özel havuz** , [DC/OS kümesini dağıtırken](container-service-deployment.md)belirttiğiniz aracı düğümlerinin sayısını içerir. 

* **Ortak havuz** başlangıçta önceden belirlenmiş sayıda aracı düğümü içerir. Bu havuz, DC/OS kümesi sağlandığında otomatik olarak eklenir.

Özel havuz ve genel havuz Azure sanal makine ölçek kümeleridir. Dağıtımdan sonra Bu havuzların boyutunu değiştirebilirsiniz.

## <a name="use-agent-pools"></a>Aracı havuzlarını kullanma
Varsayılan olarak, **Marathon** tüm yeni uygulamaları *özel* aracı düğümlerine dağıtır. Uygulamayı oluştururken uygulamayı *ortak* düğümlere açıkça dağıtmanız gerekir. **Isteğe bağlı** sekmesini seçin ve **kabul edilen kaynak rolleri** değeri için **slave_public** girin. Bu işlem [burada](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) ve [DC/OS](https://docs.mesosphere.com/1.7/administration/installing/oss/custom/create-public-agent/) belgelerinde belgelenmiştir.

## <a name="next-steps"></a>Sonraki adımlar
* [DC/OS Kapsayıcılarınızı yönetme](container-service-mesos-marathon-ui.md)hakkında daha fazla bilgi edinin.

* DC/OS kapsayıcılarınıza genel erişime izin vermek için Azure tarafından sunulan [güvenlik duvarının nasıl açılacağını](container-service-enable-public-access.md) öğrenin.

