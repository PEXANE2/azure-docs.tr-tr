---
title: Azure Dev Spaces kümenizi Held 3 (Önizleme) kullanacak şekilde yapılandırma
services: azure-dev-spaces
ms.date: 02/28/2020
ms.topic: conceptual
description: Geliştirme alanları kümenizi hele 3 kullanacak şekilde yapılandırmayı öğrenin
keywords: Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: 9e3f3ff90d36215c386bf1d8b8ec1edd54ebfb6a
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78202266"
---
# <a name="configure-your-azure-dev-spaces-cluster-to-use-helm-3-preview"></a>Azure Dev Spaces kümenizi Held 3 (Önizleme) kullanacak şekilde yapılandırma

Azure Dev Spaces, Kullanıcı hizmetlerini AKS kümenizdeki dev Spaces 'a yüklemek için varsayılan olarak Held 2 kullanır. Geliştirme alanlarında Kullanıcı hizmetlerini yüklemek için, Azure Dev Spaces Held 2 yerine helm3 kullanmasını sağlayabilirsiniz. Helm Azure Dev Spaces 'ın Kullanıcı hizmetlerini yüklemek için kullandığı sürümden bağımsız olarak, aynı kümedeki kendi sürümlerinizi yönetmek için Helm 2 veya 3 istemcisini kullanmaya devam edebilirsiniz.

Helm 3 ' ü etkinleştirdiğinizde, Azure Dev Spaces aşağıdaki yollarla Kullanıcı hizmetlerini geliştirme alanlarına yüklerken farklı davranır:

* Tiller artık *azds* ad alanındaki kümenize dağıtılmadı.
* Held, sürüm bilgilerini, *azds* ad alanı yerine bir hizmetin yüklendiği ad alanında depolar.
* Helb 3 sürüm bilgileri, bir denetleyici silindikten sonra hizmetin yüklendiği ad alanında kalır.
* Heln 3 istemcisini kullanarak kümenizdeki Azure Dev Spaces tarafından yönetilen herhangi bir yayınla doğrudan etkileşim kurabilirsiniz.

Bu kılavuzda, geliştirme alanlarında Kullanıcı hizmetlerini yüklemek için Azure Dev Spaces Held 3 ' ü nasıl etkinleştireceğinizi öğreneceksiniz.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="before-you-begin"></a>Başlamadan önce

### <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
* [Yüklü Azure CLI][azure-cli].

### <a name="register-the-helm3preview-preview-feature"></a>Helm3Preview Preview özelliğini kaydetme

Geliştirme alanlarında Kullanıcı hizmetlerini yüklemek için Azure Dev Spaces Held 3 ' ü kullanmak üzere etkinleştirmek için, önce *az Feature Register* komutunu kullanarak aboneliğinizde *Helm3Preview* Özellik bayrağını etkinleştirin:

> [!WARNING]
> *Helm3Preview* özelliği bayrağıyla üzerinde Azure dev Spaces etkinleştirdiğiniz herhangi bir aks kümesi, bu önizleme deneyimini kullanacaktır. AKS kümelerinde tamamen desteklenen Azure Dev Spaces etkinleştirmeye devam etmek için, üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek için ayrı bir test veya geliştirme Azure aboneliği kullanın.

```azure-cli
az feature register --namespace Microsoft.DevSpaces --name Helm3Preview
```

Kaydın tamamlanabilmesi birkaç dakika sürer. *Az Feature Show* komutunu kullanarak kayıt durumunu denetleyin:

```azure-cli
az feature show --namespace Microsoft.DevSpaces --name Helm3Preview
```

*Durum* *kaydedildiğinde*, *az Provider Register*kullanarak *Microsoft. devspaces* kaydını yenileyin:

```azure-cli
az provider register --namespace Microsoft.DevSpaces
```

### <a name="limitations"></a>Sınırlamalar

Bu özellik önizlemedeyken aşağıdaki sınırlamalar geçerlidir:

* Bu özelliği, mevcut iş yükleriyle AKS kümelerinde kullanamazsınız. Yeni bir AKS kümesi oluşturmanız gerekir.

## <a name="create-your-cluster"></a>Kümenizi oluşturma

Bu Önizleme özelliğine sahip bir bölgede yeni bir AKS kümesi oluşturun. Aşağıdaki komutlar *Myresourcegroup* adlı bir kaynak grubu ve *Orta Güney ABD* bölgesinde *MYAKS* adlı yeni bir aks kümesi oluşturur:

```azure-cli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces"></a>Azure Dev Spaces etkinleştir

AKS kümenizde dev alanlarını etkinleştirmek ve istemleri izlemek için *Use-dev-Spaces* komutunu kullanın. Aşağıdaki komut *Myresourcegroup* grubundaki *myaks* kümesinde dev alanlarını etkinleştiriyor ve varsayılan bir dev alanı oluşturuyor.

```cmd
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

## <a name="verify-dev-spaces-is-running-helm-3"></a>Geliştirme alanlarının, Held 3 çalıştığını doğrulama

*Azds* ad alanındaki dağıtımları listeleyerek Tiller 'in çalışmadığını doğrulayın:

```cmd
kubectl get deployment -n azds
```

*Tiller-Deploy* komutunun azds ad alanında çalışmadığını onaylayın. Örnek:

```console
$ kubectl get deployments -n azds
NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
azds-webhook-deployment   2/2     2            2           39m
traefik                   1/1     1            1           39m
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces birden çok kapsayıcı genelinde daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda kodunuzun farklı sürümleriyle veya dallarıyla çalışarak işbirliğine dayalı geliştirmeyi nasıl kolaylaştırabileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Dev Spaces 'de takım geliştirme][team-quickstart]


[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[team-quickstart]: ../quickstart-team-development.md