---
title: Azure Dev Spaces kümenizi Miğfer 3 'ü kullanacak şekilde yapılandırın (önizleme)
services: azure-dev-spaces
ms.date: 02/28/2020
ms.topic: conceptual
description: Miğfer 3'ü kullanmak için Dev Spaces kümenizi nasıl yapılandırılasınız öğrenin
keywords: Azure Dev Alanlar, Dev Alanlar, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler
ms.openlocfilehash: dbccb2618fd5a27805261d60e7891d920e0bc372
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454304"
---
# <a name="configure-your-azure-dev-spaces-cluster-to-use-helm-3-preview"></a>Azure Dev Spaces kümenizi Miğfer 3 'ü kullanacak şekilde yapılandırın (önizleme)

Azure Dev Spaces, AKS kümenizde kullanıcı hizmetlerini dev alanlarda yüklemek için varsayılan olarak Helm 2'yi kullanır. Azure Dev Spaces'in, geliştirme alanlarında kullanıcı hizmetlerini yükleyen Helm 2 yerine Miğfer 3'ü kullanmasını etkinleştirebilirsiniz. Miğfer Azure Dev Spaces'in kullanıcı hizmetlerini yüklemek için kullandığı sürümden bağımsız olarak, aynı kümede kendi sürümlerinizi yönetmek için Helm 2 veya 3 istemcisini kullanmaya devam edebilirsiniz.

Mik 3'ü etkinleştirdiğinizde, Azure Geliştirme Alanları kullanıcı hizmetlerini geliştirme alanlarında aşağıdaki şekillerde yüklerken farklı şekilde davranmaktadır:

* Çapa makinesi artık *azds* ad alanında kümenize dağıtılamaz.
* Helm, *azds* ad alanı yerine bir hizmetin yüklü olduğu ad alanında bilgi yayımlar.
* Miğfer 3 sürüm bilgileri, denetleyici silindikten sonra bir hizmetin yüklendiği ad alanında kalır.
* Miğfer 3 istemcisini kullanarak kümenizdeki Azure Dev Spaces tarafından yönetilen tüm sürümlerle doğrudan etkileşim kurabilirsiniz.

Bu kılavuzda, Azure Geliştirme Alanları için Miğfer 3'ün kullanıcı hizmetlerini dev alanlara yüklemesini nasıl etkinleştireceğinizi öğreneceksiniz.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="before-you-begin"></a>Başlamadan önce

### <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
* [Yüklü Azure CLI][azure-cli].

### <a name="register-the-helm3preview-preview-feature"></a>Helm3Preview önizleme özelliğini kaydedin

Azure Dev Spaces'in kullanıcı hizmetlerini geliştirme alanlarında yüklemek için Miğfer 3'ü kullanmasını sağlamak için, öncelikle *az özellik kayıt* komutunu kullanarak aboneliğinizde *Helm3Preview* özellik bayrağını etkinleştirin:

> [!WARNING]
> *Helm3Preview* özellik bayrağıyla Azure Dev Spaces'i etkinleştirdiğiniz herhangi bir AKS kümesi bu önizleme deneyimini kullanır. AKS kümelerinde tam destekli Azure Dev Alanları'nı etkinleştirmeye devam etmek için, üretim aboneliklerinde önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek için ayrı bir test veya geliştirme Azure aboneliği kullanın.

```azure-cli
az feature register --namespace Microsoft.DevSpaces --name Helm3Preview
```

Kaydın tamamlanması birkaç dakika sürer. *az özelliği göster* komutunu kullanarak kayıt durumunu kontrol edin:

```azure-cli
az feature show --namespace Microsoft.DevSpaces --name Helm3Preview
```

*Devlet* *Kaydedildiğinde,* az sağlayıcı kaydını kullanarak *Microsoft.DevSpaces* *kaydını yenileyin:*

```azure-cli
az provider register --namespace Microsoft.DevSpaces
```

### <a name="limitations"></a>Sınırlamalar

Bu özellik önizlemedeyken aşağıdaki sınırlamalar geçerlidir:

* Varolan iş yüklerine sahip AKS kümelerinde bu özelliği kullanamazsınız. Yeni bir AKS kümesi oluşturmanız gerekir.

## <a name="create-your-cluster"></a>Kümenizi oluşturun

Bu önizleme özelliğine sahip bir bölgede yeni bir AKS kümesi oluşturun. Aşağıdaki komutlar *MyResourceGroup* adında bir kaynak grubu ve *MyAKS*adında yeni bir AKS kümesi oluşturur:

```azure-cli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces"></a>Azure Dev Alanlarını Etkinleştir

AKS kümenizde Dev Spaces'i etkinleştirmek ve istemleri izlemek için *kullanım-dev-spaces* komutunu kullanın. Aşağıdaki komut, *MyResourceGroup* grubunda *MyAKS* kümesinde Dev Spaces'i etkinleştirive varsayılan bir dev alanı oluşturur.

```cmd
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

## <a name="verify-dev-spaces-is-running-helm-3"></a>Dev Spaces'in Miğfer 3'ü çalıştırdan doğrula

Çapa makinesinin *azds* ad alanında dağıtımları listeleyerek çalışmadığını doğrulayın:

```cmd
kubectl get deployment -n azds
```

*Çapa makinesi dağıtmanın* azds ad alanında çalışmadığını onaylayın. Örnek:

```console
$ kubectl get deployments -n azds
NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
azds-webhook-deployment   2/2     2            2           39m
traefik                   1/1     1            1           39m
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Geliştirme Alanları'nın birden çok kapsayıcıda daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda farklı sürümlerle veya kod dallarıyla çalışarak ortak geliştirmeyi nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Geliştirme Alanlarında ekip geliştirme][team-quickstart]


[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[team-quickstart]: ../quickstart-team-development.md