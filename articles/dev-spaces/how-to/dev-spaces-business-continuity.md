---
title: İş sürekliliği ve olağanüstü durum kurtarma
services: azure-dev-spaces
author: lisaguthrie
ms.author: lcozzens
ms.date: 01/28/2019
ms.topic: conceptual
description: İş sürekliliği sağlamak ve olağanüstü durum kurtarma çalışmalarına hazırlanmak için Azure Geliştirme Alanları ve Azure Kubernetes Hizmetlerini kullanmayı öğrenin
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s '
manager: gwallace
ms.openlocfilehash: 37c0048bfa7e72b25eb56603fc027045eba25cea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78295836"
---
# <a name="business-continuity-and-disaster-recovery-in-azure-dev-spaces"></a>Azure Dev Spaces'te iş sürekliliği ve olağanüstü durum kurtarma

## <a name="review-disaster-recovery-guidance-for-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) için olağanüstü durum kurtarma kılavuzunu gözden geçirin

Azure Geliştirme Alanları, Azure Kubernetes Hizmetinin (AKS) bir özelliğidir. AKS'de olağanüstü durum kurtarma yönergelerine dikkat etmeli ve bunların Dev Spaces için kullandığınız AKS kümeleri için geçerli olup olmadığını göz önünde bulundurmalısınız. Daha fazla bilgi için lütfen [Azure Kubernetes Hizmeti'nde (AKS) iş sürekliliği ve olağanüstü durum kurtarma için en iyi uygulamalara](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region) başvurun

## <a name="enable-dev-spaces-on-aks-clusters-in-different-regions"></a>Farklı bölgelerdeki AKS kümelerinde Dev Boşlukları etkinleştirme

Farklı bölgelerdeki AKS kümelerinde Dev Spaces'i etkinleştirmek, Azure bölgesi hatasından hemen sonra Dev Spaces'i kullanmaya devam etmenizi sağlar.

AKS'nin çok bölgeli dağıtımları hakkında genel bilgi için, [çok bölgeli dağıtım planı'na](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region#plan-for-multiregion-deployment) bakın

### <a name="enable-dev-spaces-via-the-azure-portal"></a>Azure portalı üzerinden Dev Spaces'i etkinleştirme

Azure portalındaki her kümenin ayarlarının altındaki **Dev Spaces** menü öğesini seçin. Ardından Dev Spaces'i etkinleştirme ve kaydetme seçeneğini belirleyin.

![Azure portalı üzerinden Dev Spaces'i etkinleştirme](../media/common/enable-dev-spaces.jpg)

Her küme için bu işlemi yineleyin.

### <a name="enable-dev-spaces-via-the-azure-cli"></a>Azure CLI ile Dev Boşlukları Etkinleştirme

Komut satırında Dev Spaces'i de etkinleştirebilirsiniz:

```azurecli
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

## <a name="deploy-your-teams-baseline-to-each-cluster"></a>Ekibinizin taban çizgisini her kümeye dağıtma

Dev Spaces ile çalışırken, genellikle tüm uygulamayı Kubernetes kümenizdeki bir üst dev alana dağıtırsınız. Varsayılan olarak, `default` alan kullanılır. İlk dağıtım, veritabanları veya kuyruklar gibi bu hizmetlerin bağlı olduğu tüm hizmetlerin yanı sıra dış kaynakları da içerir. Bu *taban çizgisi*olarak bilinir. Üst dev alanında bir taban çizgisi ayarladıktan sonra, alt geliştirme alanlarında tek tek hizmetleri yineler ve hata ayıklarsınız.

Temel hizmet kümenizin en son sürümlerini birden çok bölgede kümelere dağıtmanız gerekir. Temel hizmetlerinizi bu şekilde güncelleştirmek, Azure bölgesi hatası olduğunda Dev Spaces'i kullanmaya devam edebilmenizi sağlar. Örneğin, taban çizginizi bir CI/CD ardışık noktası üzerinden dağıtırsanız, ardışık lığı farklı bölgelerdeki birden çok kümeye dağıtacak şekilde değiştirin.

## <a name="select-the-correct-aks-cluster-to-use-for-dev-spaces"></a>Dev Spaces için kullanılacak doğru AKS kümesini seçin

Takımınızın taban çizgisini çalıştıran bir yedekleme kümesini düzgün şekilde yapılandırdıktan sonra, istediğiniz zaman yedekleme kümesine hızlı bir şekilde geçebilirsiniz. Daha sonra çocuk geliştirme alanlarında üzerinde çalıştığınız bireysel hizmetleri yeniden çalıştırabilirsiniz.

Aşağıdaki CLI komutuyla farklı bir küme seçin:

```azurecli
az aks use-dev-spaces -g <new resource group name> -n <new cluster name>
```

Yeni kümedeki kullanılabilir dev boşluklarını aşağıdaki komutla listeleyebilirsiniz:

```cmd
azds space list
```

Çalışmak için yeni bir dev alanı oluşturabilir veya aşağıdaki komutla varolan bir geliştirme alanı seçebilirsiniz:

```cmd
azds space select -n <space name>
```

Bu komutları çalıştırdıktan sonra, seçili küme ve dev alanı sonraki CLI işlemleri ve Azure Dev Spaces için Visual Studio Code uzantısını kullanarak projeleri hata ayıklama için kullanılır.

Visual Studio kullanıyorsanız, varolan bir proje tarafından kullanılan kümeyi aşağıdaki adımlarla değiştirebilirsiniz:

1. Görsel Stüdyo'da projenizi açın.
1. Çözüm Gezgini'nde proje adını sağ tıklatın ve **Özellikler'i** tıklatın
1. Sol bölmede Hata **Ayıklama'yı** tıklatın
1. Hata Ayıklama özellikleri sayfasında, **Profil** açılır listesini tıklatın ve **Azure Dev Alanları'nı**seçin.
1. **Değiştir** düğmesini tıklatın.
1. Görünen iletişim kutusunda, kullanmak istediğiniz AKS kümesini seçin. İstenirse, **Uzay** açılır listesinden uygun seçeneği seçerek çalışmak için farklı bir geliştirme alanı seçin veya yeni bir dev alanı oluşturun.

Doğru küme ve alanı seçtikten sonra, Hizmeti Dev Spaces'te çalıştırmak için F5 tuşuna basabilirsiniz.

Özgün kümeyi kullanmak üzere yapılandırılan diğer projeler için bu adımları yineleyin.

## <a name="access-a-service-on-a-backup-cluster"></a>Yedekleme kümesindeki bir hizmete erişin

Hizmetinizi ortak bir DNS adı kullanacak şekilde yapılandırıldıysanız, yedek kümede çalıştırırsanız hizmetin farklı bir URL'si olacaktır. Genel DNS adları her `<space name>.s.<root space name>.<service name>.<cluster GUID>.<region>.azds.io`zaman biçimindedir. Farklı bir kümeye geçerseniz, GUID kümesi ve büyük olasılıkla bölge değişecektir.

Dev Spaces, çalışırken `azds up`hizmet için her zaman doğru URL'yi gösterir veya Azure Dev **Spaces**altında Visual Studio'daki Çıktı penceresinde.

Ayrıca komutu çalıştırarak URL'yi `azds list-uris` de bulabilirsiniz:
```
$ azds list-uris
Uri                                                     Status
------------------------------------------------------  ---------
http://default.mywebapi.d05afe7e006a4fddb73c.eus.azds.io/  Available
```

Hizmete erişirken bu URL'yi kullanın.
