---
title: (AmortismanA Uğradı) Azure Kapsayıcı Hizmeti için DC/OS aracı havuzları
description: Ortak ve özel aracı havuzları bir Azure Kapsayıcı Hizmeti DC/OS kümesiyle nasıl çalışır?
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 01/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bb9b33bf537ebd5a563f8e8a8afd45cd2e5b292d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278377"
---
# <a name="deprecated-dcos-agent-pools-for-azure-container-service"></a>(AmortismanA Uğradı) Azure Kapsayıcı Hizmeti için DC/OS aracı havuzları

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Azure Kapsayıcı Hizmeti'ndeki DC/OS kümeleri iki havuzda, ortak havuzda ve özel havuzda aracı düğümleri içerir. Bir uygulama her iki havuza da dağıtılabilir ve bu da konteyner hizmetinizdeki makineler arasındaki erişilebilirliği etkiler. Makineler internete (genel) maruz kalınabilir veya dahili (özel) tutulabilir. Bu makalede, neden genel ve özel havuzları vardır kısa bir genel bakış verir.


* **Özel ajanlar**: Özel ajan düğümleri routable olmayan bir ağ üzerinden çalıştırın. Bu ağa yalnızca yönetici bölgesinden veya ortak bölge kenar yönlendiricisinden erişilebilir. Varsayılan olarak, DC/OS özel aracı düğümleri üzerinde uygulamalar başlatıyor. 

* **Genel aracılar**: Genel aracı düğümleri DC/OS uygulamalarını ve hizmetlerini herkesin erişebileceği bir ağ üzerinden çalıştırır. 

DC/OS ağ güvenliği hakkında daha fazla bilgi için [DC/OS belgelerine](https://docs.mesosphere.com/)bakın.

## <a name="deploy-agent-pools"></a>Aracı havuzlarını dağıtma

Azure Kapsayıcı Hizmeti'ndeki DC/OS aracı havuzları aşağıdaki gibi oluşturulur:

* **Özel havuz,** [DC/OS kümesini dağıtırken](container-service-deployment.md)belirttiğiniz aracı düğümü sayısını içerir. 

* **Ortak havuz** başlangıçta önceden belirlenmiş sayıda aracı düğümü içerir. DC/OS kümesi sağlandığında bu havuz otomatik olarak eklenir.

Özel havuz ve ortak havuz Azure sanal makine ölçek kümeleridir. Dağıtımdan sonra bu havuzları yeniden boyutlandırabilirsiniz.

## <a name="use-agent-pools"></a>Aracı havuzlarını kullanma
Varsayılan olarak, **Marathon** herhangi bir yeni uygulamayı *özel* aracı düğümlerine dağır. Uygulamanın oluşturulması sırasında uygulamayı *açıkça ortak* düğümlere dağıtmanız gerekir. **İsteğe Bağlı** sekmesini seçin ve **Kabul Edilen Kaynak Rolleri** değeri için **slave_public** girin. Bu işlem [burada](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) ve [DC/OS](https://docs.mesosphere.com/1.7/administration/installing/oss/custom/create-public-agent/) belgelerinde belgelenmiştir.

## <a name="next-steps"></a>Sonraki adımlar
* [DC/OS kapsayıcılarınızı yönetme](container-service-mesos-marathon-ui.md)hakkında daha fazla bilgi edinin.

* DC/OS kapsayıcılarınıza genel erişime izin vermek için Azure tarafından sağlanan [güvenlik duvarını](container-service-enable-public-access.md) nasıl açacağınızı öğrenin.

