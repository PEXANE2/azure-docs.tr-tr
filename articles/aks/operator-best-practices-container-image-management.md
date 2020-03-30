---
title: Operatör en iyi uygulamaları - Azure Kubernetes Hizmetleri'nde (AKS) Konteyner görüntü yönetimi
description: Azure Kubernetes Hizmeti'nde (AKS) kapsayıcı görüntülerini yönetme ve güvenli hale getirmek için küme operatörünün en iyi uygulamalarını öğrenin
services: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: efe72157f598c336248e407c57bce92fe87da23a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594758"
---
# <a name="best-practices-for-container-image-management-and-security-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) konteyner görüntü yönetimi ve güvenliği için en iyi uygulamalar

Azure Kubernetes Hizmeti'nde (AKS) uygulama geliştirip çalıştırdığınızda, kapsayıcılarınızın ve konteyner görüntülerinizin güvenliği önemli bir husustur. Güncel olmayan temel görüntüleri veya yamasız uygulama çalışma sürelerini içeren kapsayıcılar bir güvenlik riski ve olası saldırı vektörü sunar. Bu riskleri en aza indirmek için, yapı zamanında ve çalışma zamanında ilgili sorunları tarayan ve düzelten araçları tümleştirmeniz gerekir. İşlemde güvenlik açığı veya güncel olmayan temel görüntü ne kadar erken yakalanırsa, küme o kadar güvenli olur. Bu makalede, *kapsayıcılar* hem kapsayıcı kayıt defterinde depolanan kapsayıcı görüntüleri ve çalışan kapsayıcılar anlamına gelir.

Bu makalede, AKS'deki kaplarınızın nasıl güvenli hale edilebildiğini odaklanın. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Görüntü açıklarını tarayıp düzeltin
> * Temel görüntü güncelleştirildiğinde kapsayıcı görüntülerini otomatik olarak tetikleyin ve yeniden dağıtın

Küme güvenliği ve [pod][best-practices-pod-security] [güvenliği][best-practices-cluster-security] için en iyi uygulamaları da okuyabilirsiniz.

Güvenlik [Merkezi'ndeki Kapsayıcı güvenliğini,][security-center-containers] ekseklerinizi güvenlik açıklarına karşı taramaya yardımcı olmak için de kullanabilirsiniz.  Resimlerinizi ve kayıt [defterinizi][security-center-acr] güvenlik açıklarından korumaya yardımcı olmak için Güvenlik Merkezi ile Azure Konteyner Kayıt Defteri entegrasyonu da vardır.

## <a name="secure-the-images-and-run-time"></a>Görüntüleri güvenli hale ve çalışma süresini

**En iyi uygulama kılavuzu** - Kapsayıcı görüntülerinizi güvenlik açıklarına karşı tarar ve yalnızca doğrulamayı geçmiş görüntüleri dağıtın. Temel görüntüleri ve uygulama çalışma zamanını düzenli olarak güncelleştirin ve ardından AKS kümesindeki iş yüklerini yeniden dağıtın.

Kapsayıcı tabanlı iş yüklerinin benimsenmesiyle ilgili endişelerden biri, kendi uygulamalarınızı oluşturmak için kullanılan görüntülerin ve çalışma zamanının güvenliğini doğrulamaktır. Dağıtımlarınıza güvenlik açıkları getirmediğinden nasıl emin olabilirsiniz? Dağıtım iş akışınız, [Twistlock][twistlock] veya [Aqua][aqua]gibi araçları kullanarak kapsayıcı görüntülerini tarayabilir ve ardından yalnızca doğrulanmış görüntülerin dağıtılmasına izin vermelidir.

![Kapsayıcı görüntülerini tarayıp düzeltin, doğrulayın ve dağıtın](media/operator-best-practices-container-security/scan-container-images-simplified.png)

Gerçek bir örnekte, görüntü taramalarını, doğrulamayı ve dağıtımları otomatikleştirmek için sürekli tümleştirme ve sürekli dağıtım (CI/CD) ardışık etki alanı kullanabilirsiniz. Azure Kapsayıcı Kayıt Defteri, bu güvenlik açıklarını tarama özelliklerini içerir.

## <a name="automatically-build-new-images-on-base-image-update"></a>Temel görüntü güncelleştirmesi üzerinde otomatik olarak yeni görüntüler oluşturun

**En iyi uygulama kılavuzu** - Uygulama görüntüleri için temel görüntüleri kullanırken, temel görüntü güncelleştirildiğinde yeni görüntüler oluşturmak için otomasyonu kullanın. Bu temel görüntüler genellikle güvenlik düzeltmeleri içerdiğinden, alt akış uygulama kapsayıcısı görüntülerini güncelleştirin.

Bir temel görüntü her güncelleştirilse, herhangi bir alt kapsayıcı görüntüsü de güncelleştirilmelidir. Bu yapı işlemi, [Azure Ardışık Hatları][azure-pipelines] veya Jenkins gibi doğrulama ve dağıtım ardışık hatlarına entegre edilmelidir. Bu ardışık hatlar, uygulamalarınızın güncelleştirilmiş tabanlı görüntülerde çalışmaya devam etmesini sağlar. Uygulama kapsayıcıgörüntüleri doğrulandıktan sonra, AKS dağıtımları en son güvenli görüntüleri çalıştırmak için güncellenebilir.

Azure Kapsayıcı Kayıt Defteri Görevleri, temel görüntü güncelleştirildiğinde kapsayıcı görüntülerini otomatik olarak güncelleyebilir. Bu özellik, az sayıda temel görüntü oluşturmanıza ve bunları düzenli olarak hata ve güvenlik düzeltmeleriyle güncel tutmanıza olanak tanır.

Temel resim güncelleştirmeleri hakkında daha fazla bilgi için Azure [Kapsayıcı Kayıt Defteri Görevleri ile temel resim güncelleştirmesi üzerinde resim yapılarını Otomatikleştir'e][acr-base-image-update]bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kapsayıcılarınızın nasıl güvenli hale alındığı üzerinde durulu. Bu alanlardan bazılarını uygulamak için aşağıdaki makalelere bakın:

* [Azure Kapsayıcı Kayıt Defteri Görevleri ile temel görüntü güncelleştirmesi üzerinde resim oluşturmayı otomatikleştirin][acr-base-image-update]

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