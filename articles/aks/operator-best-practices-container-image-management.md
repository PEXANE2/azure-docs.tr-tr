---
title: Operatör en iyi yöntemleri-Azure Kubernetes hizmetlerinde kapsayıcı görüntüsü yönetimi (AKS)
description: Azure Kubernetes Service (AKS) ' de kapsayıcı görüntülerini yönetme ve güvenli hale getirme için küme operatörü en iyi uygulamalarını öğrenin
services: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: efe72157f598c336248e407c57bce92fe87da23a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77594758"
---
# <a name="best-practices-for-container-image-management-and-security-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içindeki kapsayıcı görüntüsü yönetimi ve güvenlik için en iyi uygulamalar

Azure Kubernetes Service (AKS) içinde uygulama geliştirip çalıştırırken, kapsayıcılarınızın ve kapsayıcı görüntülerinin güvenliği önemli bir konudur. Güncel temel görüntüler veya düzeltme eki yüklenmemiş uygulama çalışma zamanları içeren kapsayıcılar, güvenlik riski ve olası saldırı vektörü sunar. Bu riskleri en aza indirmek için, derleme zamanında ve çalışma zamanının yanı sıra kapsayıcılarınızdaki sorunları taradığı ve düzeltebileceğiniz araçları tümleştirmelisiniz. Daha önce güvenlik açığı veya güncel olmayan temel görüntü yakalanmışsa, kümenin daha güvenli hale getirin. Bu makalede, *kapsayıcılar* her ikisi de bir kapsayıcı kayıt defterinde depolanan kapsayıcı görüntüleri ve çalışan kapsayıcılar anlamına gelir.

Bu makalede, AKS 'de kapsayıcılarınızın güvenliğini sağlama konusu ele alınmaktadır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Görüntü için tarama ve düzeltme güvenlik açıkları
> * Temel görüntü güncelleştirilirken kapsayıcı görüntülerini otomatik olarak tetikleme ve yeniden dağıtma

[Küme güvenliği][best-practices-cluster-security] ve [Pod güvenliği][best-practices-pod-security]için en iyi yöntemleri de okuyabilirsiniz.

Kapsayıcılarınızın güvenlik açıklarına karşı taranmasına yardımcı olması için [Güvenlik Merkezi 'Nde kapsayıcı güvenliği][security-center-containers] de kullanabilirsiniz.  Ayrıca, güvenlik merkezi ile, görüntülerinizi ve kayıt defterinizi güvenlik açıklarına karşı korumaya yardımcı olmak için [Azure Container Registry tümleştirme][security-center-acr] de vardır.

## <a name="secure-the-images-and-run-time"></a>Görüntülerin ve çalışma zamanının güvenliğini sağlama

**En iyi Yöntem Kılavuzu** -kapsayıcı görüntülerinizi güvenlik açıklarına karşı tarayın ve yalnızca doğrulamayı geçen görüntüleri dağıtın. Temel görüntüleri ve uygulama çalışma zamanını düzenli olarak güncelleştirin ve ardından AKS kümesindeki iş yüklerini yeniden dağıtın.

Kapsayıcı tabanlı iş yüklerinin benimsenmesiyle ilgili bir sorun, kendi uygulamalarınızı oluşturmak için kullanılan görüntülerin ve çalışma zamanının güvenliğini doğrulamasıdır. Dağıtımlarınıza güvenlik açıklarına neden olmadığınızdan nasıl emin olabilirsiniz? Dağıtım iş akışınız, [twistlock][twistlock] veya [deniz mavisi][aqua]gibi araçları kullanarak kapsayıcı görüntülerini taramak için bir işlem içermelidir ve sonra yalnızca doğrulanmış görüntülerin dağıtılmasına izin verir.

![Kapsayıcı görüntülerini tarayın ve düzeltin, doğrulayın ve dağıtın](media/operator-best-practices-container-security/scan-container-images-simplified.png)

Gerçek dünyada bir örnekte, görüntü taramalarının, doğrulamanın ve dağıtımların otomatikleştirilmesi için sürekli tümleştirme ve sürekli dağıtım (CI/CD) işlem hattı kullanabilirsiniz. Azure Container Registry, bu güvenlik açıkları tarama özelliklerini içerir.

## <a name="automatically-build-new-images-on-base-image-update"></a>Temel görüntü güncelleştirmesinde otomatik olarak yeni görüntüler oluşturma

**En iyi Yöntem Kılavuzu** -uygulama görüntüleri için temel görüntüleri kullanırken, temel görüntü güncelleştirilirken yeni görüntüler oluşturmak için Otomasyon kullanın. Bu temel görüntüler genellikle güvenlik düzeltmelerini içerir, tüm aşağı akış uygulama kapsayıcısı görüntülerini güncelleştirin.

Bir temel görüntü her güncelleştirildiği zaman, tüm aşağı akış kapsayıcı görüntüleri de güncellenir. Bu derleme işlemi, [Azure Pipelines][azure-pipelines] veya Jenkins gibi doğrulama ve dağıtım işlem hatları ile tümleştirilebilmelidir. Bu işlem hatları, uygulamalarınızın güncelleştirilmiş tabanlı görüntülerde çalışmaya devam etmesini sağlar. Uygulama kapsayıcısı görüntüleriniz doğrulandıktan sonra, AKS dağıtımları, en son güvenli görüntüleri çalıştıracak şekilde güncellenebilir.

Azure Container Registry görevler, temel görüntü güncelleştirilirken kapsayıcı görüntülerini da otomatik olarak güncelleştirebilir. Bu özellik, az sayıda temel görüntü oluşturmanıza ve bunları düzenli olarak hata ve güvenlik düzeltmeleriyle güncelleştirmelerini sağlar.

Temel görüntü güncelleştirmeleri hakkında daha fazla bilgi için bkz. [Azure Container Registry görevlerle temel görüntü güncelleştirme üzerinde görüntü derlemelerini otomatikleştirme][acr-base-image-update].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kapsayıcılarınızın güvenliğini sağlama konusunda odaklanılmıştır. Bu alanlardan bazılarını uygulamak için aşağıdaki makalelere bakın:

* [Azure Container Registry görevlerle temel görüntü güncelleştirmesinde görüntü derlemelerini otomatikleştirin][acr-base-image-update]

<!-- EXTERNAL LINKS -->
[azure-pipelines]: /azure/devops/pipelines/?view=vsts
[twistlock]: https://www.twistlock.com/
[aqua]: https://www.aquasec.com/

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[acr-base-image-update]: ../container-registry/container-registry-tutorial-base-image-update.md
[security-center-containers]: /azure/security-center/container-security
[security-center-acr]: /azure/security-center/azure-container-registry-integration