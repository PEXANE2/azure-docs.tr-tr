---
title: Azure Uygulama Ağ Geçidi Giriş Denetleyicisi nedir?
description: Bu makalede, Uygulama Ağ Geçidi Giriş Denetleyicisi ne bir giriş sağlar.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: d6a63b6276c07b1fe6487b97f5c7fc255b6d3411
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335809"
---
# <a name="what-is-application-gateway-ingress-controller"></a>Uygulama Ağ Geçidi Giriş Denetleyicisi Nedir?
Uygulama Ağ Geçidi Giriş Denetleyicisi (AGIC), [Azure Kubernetes Hizmeti (AKS)](https://azure.microsoft.com/services/kubernetes-service/) müşterilerinin bulut yazılımını Internet'e çıkarmak için Azure'un yerel [Uygulama Ağ Geçidi](https://azure.microsoft.com/services/application-gateway/) L7 yük bakiyesini kullanmasını mümkün kılan bir Kubernetes uygulamasıdır. AGIC, barındırdığı Kubernetes kümesini izler ve seçili hizmetlerin Internet'e açık olması için bir Uygulama Ağ Geçidi'ni sürekli güncelleştirir.

Giriş Denetleyicisi müşterinin AKS üzerinde kendi bölmesinde çalışır. AGIC, değişiklikler için Kubernetes Kaynakları'nın bir alt kümesini izler. AKS kümesinin durumu Uygulama Ağ Geçidi'ne özgü yapılandırmaya çevrilir ve [Azure Kaynak Yöneticisi'ne (ARM)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)uygulanır.

## <a name="benefits-of-application-gateway-ingress-controller"></a>Uygulama Ağ Geçidi Denetleyicisinin Faydaları
AGIC, dağıtımınızın tek bir Uygulama Ağ Geçidi Giriş Denetleyicisi ile birden fazla AKS kümesini denetlemesine olanak tanır. AGIC ayrıca AKS kümesinin önünde başka bir yük dengeleyicisi/genel IP'ye sahip olması gereksinimini ortadan kaldırmaya yardımcı olur ve istekler AKS kümesine ulaşmadan önce veri yolunızda birden fazla atlamayı önler. Uygulama Ağ Geçidi, özel IP'lerini doğrudan kullanarak bölmelere konuşur ve NodePort veya KubeProxy hizmetlerini gerektirmez. Bu, dağıtımlarınıza daha iyi performans da getirir.

Giriş Denetleyicisi yalnızca Standard_v2 ve WAF_v2 SUS'lar tarafından desteklenir ve bu da size otomatik ölçekleme avantajları da sağlar. Uygulama Ağ Geçidi, AKS kümenizden herhangi bir kaynak tüketmeden trafik yükündeki artışa veya azalmaya tepki olarak tepki verebilir ve buna göre ölçeklenebilir.

AGIC'e ek olarak Uygulama Ağ Geçidi'ni kullanmak, TLS ilkesi ve Web Uygulaması Güvenlik Duvarı (WAF) işlevselliği sağlayarak AKS kümenizi korumaya da yardımcı olur.

![Azure Uygulama Ağ Geçidi + AKS](./media/application-gateway-ingress-controller-overview/architecture.png)

AGIC, Hizmet ve Dağıtımlar/Pod'larla birlikte Kubernetes [Ingress kaynağı](https://kubernetes.io/docs/user-guide/ingress/)üzerinden yapılandırılır. Azure'un yerel Application Gateway L7 yük dengeleyicisini yararlanarak bir dizi özellik sağlar. Birkaç isim:
  - URL yönlendirme
  - Tanımlama bilgisi tabanlı benzeşim
  - TLS sonlandırma
  - Uçuca TLS
  - Genel, özel ve hibrit web siteleri için destek
  - Entegre web uygulaması güvenlik duvarı

AGIC birden çok ad alanı işleyebilir ve Agic diğer mevcut arka uçları etkilemeden AKS kümeleri için özel Olarak Uygulama Ağ Geçidi yapılandırabilirsiniz anlamına gelir Yasak Hedefler vardır. 

## <a name="next-steps"></a>Sonraki Adımlar

- [**Greenfield Deployment**](ingress-controller-install-new.md): Boş alan altyapısına AGIC, AKS ve Application Gateway yükleme yönergeleri.
- [**Brownfield Dağıtım**](ingress-controller-install-existing.md): AGIC'i varolan bir AKS ve Uygulama Ağ Geçidine yükleyin.

