---
title: Yönlendirmenin nasıl çalıştığı Azure Dev Spaces
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Güç Azure Dev Spaces ve yönlendirmenin nasıl çalıştığına ilişkin süreçler açıklanmaktadır
keywords: Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: 126a534cec2ee4b07aa3a127fb3f47f9931f0031
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84307427"
---
# <a name="how-routing-works-with-azure-dev-spaces"></a>Yönlendirmenin nasıl çalıştığı Azure Dev Spaces

Azure Dev Spaces, Kubernetes uygulamalarının hızla yinelenemez ve hata ayıklamanın yanı sıra Azure Kubernetes hizmeti (AKS) kümesinde ekibinizle işbirliği yapmak için birden çok yol sağlar. Projeniz bir geliştirme alanında çalışmaya başladıktan sonra, Azure Dev Spaces projeniz için ek ağ ve yönlendirme özellikleri sağlar.

Bu makalede, yönlendirmenin dev Spaces ile nasıl çalıştığı açıklanır.

## <a name="how-routing-works"></a>Yönlendirmenin nasıl çalıştığı

Geliştirici alanı AKS 'in üzerine kurulmuştur ve aynı [ağ kavramlarını](../aks/concepts-network.md)kullanır. Ayrıca Azure Dev Spaces merkezi bir *ingressmanager* hizmetine sahiptir ve kendi giriş denetleyiciyi aks kümesine dağıtır. *Inressmanager* hizmeti, aks kümelerini dev Spaces ile izler ve uygulama yığınlarında yönlendirme için giriş nesneleriyle kümedeki Azure dev Spaces giriş denetleyicisini iyileştirir. Her pod 'daki devspaces-proxy kapsayıcısı, `azds-route-as` URL 'yi temel alan bir geliştirme ALANıNA HTTP trafiği için bir http üst bilgisi ekler. Örneğin, URL 'ye yönelik bir istek *http://azureuser.s.default.serviceA.fedcba09...azds.io* ile BIR http üst bilgisi alır `azds-route-as: azureuser` . Devspaces-proxy kapsayıcısı zaten varsa, bir `azds-route-as` üst bilgi eklemez.

Küme dışından bir hizmete HTTP isteği yapıldığında, istek giriş denetleyicisine gider. Giriş denetleyicisi, isteği, giriş nesnelerine ve kurallarına göre doğrudan uygun Pod 'a yönlendirir. Pod 'daki devspaces-proxy kapsayıcısı isteği alır, `azds-route-as` URL 'yi temel alarak üst bilgiyi ekler ve sonra isteği uygulama kapsayıcısına yönlendirir.

Küme içindeki başka bir hizmetten bir hizmete HTTP isteği yapıldığında, istek ilk olarak çağıran hizmetin devspaces-proxy kapsayıcısından geçer. Devspaces-proxy kapsayıcısı HTTP isteğine bakar ve `azds-route-as` üstbilgiyi denetler. Üst bilgiye bağlı olarak, devspaces-proxy kapsayıcısı, üst bilgi değeriyle ilişkili hizmetin IP adresini arar. Bir IP adresi bulunursa, devspaces-proxy kapsayıcısı bu IP adresine isteği reroutes. Bir IP adresi bulunamazsa, devspaces-proxy kapsayıcısı, isteği üst uygulama kapsayıcısına yönlendirir.

Örneğin, *Servicea* ve *serviceb* uygulamaları *varsayılan*adlı bir üst geliştirme alanına dağıtılır. *Servicea* , *serviceb* 'YI kullanır ve buna http çağrıları yapar. Azure Kullanıcı, *azureuser*adlı *varsayılan* alana göre bir alt dev alanı oluşturur. Azure Kullanıcı aynı zamanda kendi *hizmet* sürümünü kendi alt alanına dağıtır. Bir istek yapıldığında *http://azureuser.s.default.serviceA.fedcba09...azds.io* :

![Azure Dev Spaces yönlendirme](media/how-dev-spaces-works/routing.svg)

1. Giriş denetleyicisi, *Servicea. azureuser*olan URL ile ilişkili pod için IP 'yi arar.
1. Giriş denetleyicisi, Azure kullanıcısının geliştirme alanında pod için IP 'yi bulur ve isteği *Servicea. azureuser* Pod 'a yönlendirir.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, isteği alır ve `azds-route-as: azureuser` bir http üst bilgisi olarak ekler.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, isteği *servicea. Azureuser* Pod içindeki *servicea* uygulama kapsayıcısına yönlendirir.
1. *Servicea. azureuser* Pod Içindeki *Servicea* uygulaması, *serviceb*'ye bir çağrı yapar. *Servicea* uygulaması aynı zamanda var olan üstbilgiyi koruyacak kodu de içerir `azds-route-as` , bu durumda `azds-route-as: azureuser` .
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, isteği alır ve üst bilgi değerine göre *serviceb* IP 'sini arar `azds-route-as` .
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı *serviceb. AZUREUSER*için bir IP bulamadı.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, ana alanda *SERVICEB* için IP 'yi arar. *varsayılan*.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, *serviceb. Default* için IP 'Yi bulur ve isteği *serviceb. Default* Pod öğesine yönlendirir.
1. *Serviceb. Default* Pod içindeki devspaces-proxy kapsayıcısı, isteği alır ve Isteği *serviceb. Default* Pod içindeki *serviceb* uygulama kapsayıcısına yönlendirir.
1. *Serviceb. Default* Pod Içindeki *serviceb* uygulaması *servicea. azureuser* Pod 'a bir yanıt döndürür.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı yanıtı alır ve yanıtı servicea *. Azureuser* Pod içindeki *servicea* uygulama kapsayıcısına yönlendirir.
1. *Servicea* uygulaması yanıtı alır ve kendi yanıtını döndürür.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, *servicea* uygulama kapsayıcısından gelen yanıtı alır ve yanıtı küme dışında özgün çağırana yönlendirir.

HTTP olmayan diğer tüm TCP trafiği, giriş denetleyicisi ve devspaces-proxy kapsayıcılarından değiştirilmemiş şekilde geçer.

## <a name="sharing-a-dev-space"></a>Geliştirme alanı paylaşma

Bir takımla çalışırken, bir [ekibin tamamında bir geliştirme alanı paylaşabilir](how-to/share-dev-spaces.md) ve türetilmiş dev alanları oluşturabilirsiniz. Geliştirici alanı, dev alanının kaynak grubuna katkıda bulunan erişimi olan herkes tarafından kullanılabilir.

Ayrıca, başka bir geliştirme alanından türetilmiş yeni bir geliştirme alanı da oluşturabilirsiniz. Türetilmiş bir geliştirme alanı oluşturduğunuzda, *azds.io/Parent-Space=Parent-Space-Name* etiketi türetilmiş geliştirme alanının ad alanına eklenir. Ayrıca, üst geliştirici alanındaki tüm uygulamalar, türetilmiş geliştirme alanıyla paylaşılır. Uygulamanın güncelleştirilmiş bir sürümünü türetilmiş geliştirme alanına dağıtırsanız, bu yalnızca türetilmiş dev alanında bulunur ve üst dev alanı etkilenmeden kalır. En fazla üç türetilmiş geliştirme alanı düzeyi veya en *üst* boşluk olabilir.

Türetilmiş geliştirme alanı Ayrıca, kendi uygulamaları ve üst öğesinden paylaşılan uygulamalar arasında istekleri de akıllıca yönlendirir. Yönlendirme, istek türetilmiş geliştirme alanındaki bir uygulamaya yönlendirilmeye çalışarak ve üst geliştirme alanından paylaşılan uygulamaya geri dönerek yapılır. Uygulama üst alanda değilse, yönlendirme, ana alanda bulunan paylaşılan uygulamaya geri döner.

Örneğin:
* Dev alanı *varsayılan* olarak, *Servicea* ve *serviceb*uygulamalarına sahiptir.
* *Azureuser* dev Space, *varsayılan*olarak türetilir.
* *Servicea* 'nın güncelleştirilmiş bir sürümü *azureuser*'e dağıtılır.

*Azureuser*kullanılırken, *servicea* 'ya yapılan tüm istekler *azureuser*içindeki güncelleştirilmiş sürüme yönlendirilir. *Serviceb* 'ye yönelik bir istek, önce *serviceb*'nin *azureuser* sürümüne yönlendirilmek üzere denenir. Mevcut olmadığından, bu hizmet, *serviceb*'nin *varsayılan* sürümüne yönlendirilir. *Servicea* 'nın *azureuser* sürümü kaldırılırsa, *servicea* 'Ya yapılan tüm istekler *varsayılan* *servicea*sürümünü kullanmaya geri döner.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces, Hızlı yineleme ve geliştirme sağlamak üzere yönlendirmeyi nasıl kullandığını görmek için, [Kubernetes Ile yerel Işlemin nasıl çalıştığı][how-it-works-local-process-kubernetes], [Azure dev Spaces ile kodunuzda uzaktan hata ayıklamanın][how-it-works-remote-debugging]yanı sıra [Azure Kubernetes hizmeti & GitHub eylemleriyle][pr-flow]ilgili bazı örnekler bulabilirsiniz.

Takım geliştirmesi için Azure Dev Spaces ile yönlendirmeyi kullanmaya başlamak için, Azure Dev Spaces hızlı başlangıçta [Takım geliştirme][quickstart-team] bölümüne bakın.

[helm-upgrade]: https://helm.sh/docs/intro/using_helm/#helm-upgrade-and-helm-rollback-upgrading-a-release-and-recovering-on-failure
[how-it-works-local-process-kubernetes]: how-dev-spaces-works-local-process-kubernetes.md
[how-it-works-remote-debugging]: how-dev-spaces-works-remote-debugging.md
[pr-flow]: how-to/github-actions.md
[quickstart-team]: quickstart-team-development.md
[troubleshooting]: troubleshooting.md