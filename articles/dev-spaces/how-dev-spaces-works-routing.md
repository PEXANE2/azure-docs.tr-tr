---
title: Azure Dev Spaces ile yönlendirme nasıl çalışır?
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Azure Dev Spaces'e güç veren işlemleri ve yönlendirmenin nasıl çalıştığını açıklar
keywords: Azure Dev Alanlar, Dev Alanlar, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler
ms.openlocfilehash: e9bc1875c053335da6a8e2603406bcdb34a6dd04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241393"
---
# <a name="how-routing-works-with-azure-dev-spaces"></a>Azure Dev Spaces ile yönlendirme nasıl çalışır?

Azure Geliştirme Alanları, Kubernetes uygulamalarını hızla tekrarlayıp hata ayıklamanın ve bir Azure Kubernetes Hizmeti (AKS) kümesinde ekibinizle işbirliği yapmanın birden çok yolunu sağlar. Projeniz bir geliştirme alanında çalıştırDıktan sonra, Azure Geliştirme Alanları projeniz için ek ağ ve yönlendirme özellikleri sağlar.

Bu makalede, Yönlendirme Dev Spaces ile nasıl çalıştığını açıklamaktadır.

## <a name="how-routing-works"></a>Yönlendirme nasıl çalışır?

Bir dev alanı AKS üstüne inşa edilmiş ve aynı [ağ kavramları](../aks/concepts-network.md)kullanır. Azure Dev Spaces ayrıca merkezi bir *ingressmanager* hizmetine sahiptir ve kendi Giriş Denetleyicisini AKS kümesine dağıtır. *Ingressmanager* hizmeti, AKS kümelerini dev boşluklarla izler ve uygulama bölmelerine yönlendirme için Giriş nesneleriyle kümedeki Azure Dev Spaces Giriş Denetleyicisini genişletir. Her bölmedeki devspaces-proxy kapsayıcısı, URL'ye dayalı bir dev alana HTTP trafiği için bir `azds-route-as` HTTP üstbilgisi ekler. Örneğin, URL'ye *http://azureuser.s.default.serviceA.fedcba09...azds.io* yapılan bir istek, `azds-route-as: azureuser`'' ile bir HTTP üstbilgisi alır. Devspaces-proxy kapsayıcı biri zaten `azds-route-as` varsa bir üstbilgi eklemez.

Kümenin dışından bir hizmete BIR HTTP isteği yapıldığında, istek Giriş denetleyicisine gider. Giriş denetleyicisi, Giriş nesneleri ve kurallarına göre isteği doğrudan uygun bölmeye yönlendirir. Bölmedeki devspaces-proxy kapsayıcısı isteği alır, `azds-route-as` üstbilgi URL'ye göre ekler ve isteği uygulama kapsayıcısına yönlendirir.

Küme içindeki başka bir hizmetten bir hizmete HTTP isteği yapıldığında, istek ilk olarak arama hizmetinin devspaces-proxy kapsayıcısından geçer. Devspaces-proxy kapsayıcı http isteği bakar ve `azds-route-as` üstbilgi denetler. Üstbilgi temel alınarak, devspaces-proxy kapsayıcı üstbilgi değeri ile ilişkili hizmetin IP adresi arar. Bir IP adresi bulunursa, devspaces-proxy kapsayıcı isteği bu IP adresine yeniden yönlendirir. BIR IP adresi bulunamazsa, devspaces-proxy kapsayıcıüst uygulama kapsayıcısı isteği yönlendirir.

Örneğin, uygulamalar *serviceA* ve *serviceB* *varsayılan*olarak adlandırılan bir üst dev alana dağıtılır. *serviceA* *serviceB'e* güvenir ve http çağrıları yapar. Azure Kullanıcı, *azureuser*adı verilen *varsayılan* alanı temel alan bir alt dev alanı oluşturur. Azure Kullanıcı ayrıca kendi *serviceA* sürümünü kendi alt alanına dağıtır. Bir istek *http://azureuser.s.default.serviceA.fedcba09...azds.io*yapıldığında:

![Azure Dev Alanlar yönlendirme](media/how-dev-spaces-works/routing.svg)

1. Giriş denetleyicisi, *serviceA.azureuser*olan URL ile ilişkili bölmenin IP'sini arar.
1. Giriş denetleyicisi, Azure Kullanıcı'nın geliştirme alanında bölmenin IP'sini bulur ve isteği *serviceA.azureuser* pod'una yönlendirir.
1. *serviceA.azureuser* bölmesindeki devspaces-proxy kapsayıcısı `azds-route-as: azureuser` isteği alır ve http üstbilgi olarak ekler.
1. *serviceA.azureuser* bölmesindeki devspaces-proxy kapsayıcısı isteği *serviceA.azureuser* bölmesindeki *serviceA* uygulama kapsayıcısına yönlendirir.
1. *serviceA.azureuser* bölmesindeki *serviceA* uygulaması *serviceB'i*arar. *ServiceA* uygulaması da bu durumda `azds-route-as` varolan üstbilgi, korumak `azds-route-as: azureuser`için kod içerir.
1. *serviceA.azureuser* bamasındaki devspaces-proxy kapsayıcısı isteği alır ve `azds-route-as` üstbilginin değerine göre *serviceB'nin* IP'sini arar.
1. *serviceA.azureuser* baçısından devspaces-proxy kapsayıcı *serviceB.azureuser*için bir IP bulamaz.
1. *serviceA.azureuser* bamasındaki devspaces-proxy kapsayıcısı, *serviceB.default*olan üst alandaki *serviceB* için IP'yi arar.
1. *serviceA.azureuser* bamasındaki devspaces-proxy kapsayıcısı *serviceB.default* için IP'yi bulur ve isteği *serviceB.default* bölmesine yönlendirir.
1. *serviceB.default* pod'daki devspaces-proxy kapsayıcısı isteği alır ve isteği *serviceB.default* pod'undaki *serviceB* uygulama kapsayıcısına yönlendirir.
1. *serviceB.default* pod'daki *serviceB* uygulaması *serviceA.azureuser* bölmesi'ne bir yanıt verir.
1. *serviceA.azureuser* bamasındaki devspaces-proxy kapsayıcısı yanıtı alır ve yanıtı *serviceA.azureuser* bölmesinde *serviceA* uygulama kapsayıcısına yönlendirir.
1. *serviceA* uygulaması yanıtı alır ve sonra kendi yanıtını döndürür.
1. *serviceA.azureuser* bama'sındaki devspaces-proxy kapsayıcısı *serviceA* uygulama kapsayıcısından yanıtı alır ve yanıtı kümenin dışındaki özgün arayana yönlendirir.

HTTP olmayan diğer tüm TCP trafiği Giriş denetleyicisi ve devspaces-proxy kapsayıcıları değiştirilmeden geçer.

## <a name="sharing-a-dev-space"></a>Dev alanını paylaşma

Bir ekiple çalışırken, [tüm takım genelinde bir dev alanı paylaşabilir](how-to/share-dev-spaces.md) ve türetilmiş dev alanları oluşturabilirsiniz. Dev alanı, geliştirme alanının kaynak grubuna katkıda bulunan herkes tarafından kullanılabilir.

Ayrıca, başka bir dev alanından türetilen yeni bir dev alanı da oluşturabilirsiniz. Türetilmiş bir dev alanı oluşturduğunuzda, *azds.io/parent-space=PARENT-SPACE-NAME* etiketi türemiş dev alanının ad alanına eklenir. Ayrıca, üst dev alanı tüm uygulamalar türetilmiş geliştirme alanı ile paylaşılır. Bir uygulamanın güncelleştirilmiş bir sürümünü türetilmiş dev alanına dağıtırsanız, bu yalnızca türetilmiş geliştirme alanında bulunur ve üst dev alanı etkilenmez. Türetilmiş dev alanları veya *büyükanne ve büyükbaba* alanları en fazla üç düzeyde olabilir.

Türetilen dev alanı da akıllıca kendi uygulamaları ve üst paylaşılan uygulamalar arasında istekleri yönlendirmek olacaktır. Yönlendirme, isteği türetilmiş geliştirme alanında bir uygulamaya yönlendirmeye çalışarak ve üst dev alanından paylaşılan uygulamaya geri dönerek çalışır. Yönlendirme, uygulama ana alanda değilse, büyükanne ve büyükbaba alanındaki paylaşılan uygulamaya geri döner.

Örnek:
* Dev alanı *varsayılan* uygulamaları *serviceA* ve *serviceB*vardır.
* Dev alanı *azureuser* *varsayılan*türetilmiştir.
* *ServiceA'nın* güncelleştirilmiş bir sürümü *azureuser'a*dağıtılır.

*azureuser*kullanırken, *serviceA'ya* yapılan tüm istekler *azureuser'de*güncelleştirilmiş sürüme yönlendirilecektir. *ServiceB'e* bir istek öncelikle *serviceB'nin* *azureuser* sürümüne yönlendirilmeye çalışacaktır. Var olmadığından, *serviceB'nin* *varsayılan* sürümüne yönlendirilecektir. *serviceA'nın* *azureuser* sürümü *kaldırılırsa, serviceA'ya* yapılan tüm istekler *serviceA'nın* *varsayılan* sürümünü kullanmaya geri döner.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces'in hızlı yineleme ve geliştirme sağlamak için yönlendirmeyi nasıl kullandığına dair bazı örnekleri görmek için geliştirme [makinenizi geliştirme alanınıza bağlamanın nasıl çalıştığını][how-it-works-connect], [Azure Dev Spaces ile kodunuzu uzaktan hata ayıklamanın nasıl çalıştığını][how-it-works-remote-debugging]ve Azure [Kubernetes Hizmeti & GitHub Eylemleri'ni][pr-flow]görün.

Ekip geliştirme için Azure Dev Spaces ile yönlendirmeyi kullanmaya başlamak için [Azure Dev Spaces'teki ekip geliştirme çalışmasına][quickstart-team] bakın.

[helm-upgrade]: https://helm.sh/docs/intro/using_helm/#helm-upgrade-and-helm-rollback-upgrading-a-release-and-recovering-on-failure
[how-it-works-connect]: how-dev-spaces-works-connect.md
[how-it-works-remote-debugging]: how-dev-spaces-works-remote-debugging.md
[pr-flow]: how-to/github-actions.md
[quickstart-team]: quickstart-team-development.md
[troubleshooting]: troubleshooting.md