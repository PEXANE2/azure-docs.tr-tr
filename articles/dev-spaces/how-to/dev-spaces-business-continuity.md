---
title: Azure Dev Spaces 'de iş sürekliliği ve olağanüstü durum kurtarma
services: azure-dev-spaces
author: lisaguthrie
ms.author: lcozzens
ms.date: 01/28/2019
ms.topic: conceptual
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
manager: gwallace
ms.openlocfilehash: f2c2767d23a99644ee4ecb4e1040162c58a72b1a
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280082"
---
# <a name="business-continuity-and-disaster-recovery-in-azure-dev-spaces"></a>Azure Dev Spaces 'de iş sürekliliği ve olağanüstü durum kurtarma

## <a name="review-disaster-recovery-guidance-for-azure-kubernetes-service-aks"></a>Azure Kubernetes hizmeti (AKS) için olağanüstü durum kurtarma kılavuzunu gözden geçirin

Azure Dev Spaces, Azure Kubernetes hizmeti 'nin (AKS) bir özelliğidir. AKS 'de olağanüstü durum kurtarma yönergelerinden haberdar olmanız ve dev alanları için kullandığınız AKS kümelerine uygulanıp uygulanmadığını göz önünde bulundurmanız gerekir. Daha fazla bilgi için lütfen [Azure Kubernetes Service (AKS) ' de iş sürekliliği ve olağanüstü durum kurtarma Için en iyi yöntemlere](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region) başvurun

## <a name="enable-dev-spaces-on-aks-clusters-in-different-regions"></a>Farklı bölgelerde AKS kümelerinde dev alanlarını etkinleştirme

Farklı bölgelerdeki AKS kümelerinde dev alanlarını etkinleştirmek, Azure bölge hatasından hemen sonra dev alanlarını kullanmayı sürdürmeye olanak tanır.

AKS 'nin çok bölgeli dağıtımları hakkında genel bilgi için bkz. [çok bölgeli dağıtım Için plan](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region#plan-for-multiregion-deployment)

Azure Dev Spaces ile uyumlu bir AKS kümesini dağıtma hakkında daha fazla bilgi için bkz. [Azure Cloud Shell kullanarak Kubernetes kümesi oluşturma](https://docs.microsoft.com/azure/dev-spaces/how-to/create-cluster-cloud-shell)

### <a name="enable-dev-spaces-via-the-azure-portal"></a>Azure portal aracılığıyla dev alanlarını etkinleştirme

Azure portal her kümenin özellikleri altında **dev Spaces** gezinti öğesine tıklayın. Ardından dev alanlarını etkinleştirme seçeneğini belirleyin.

![Azure portal aracılığıyla dev alanlarını etkinleştirme](../media/common/enable-dev-spaces.jpg)

Her küme için bu işlemi tekrarlayın.

### <a name="enable-dev-spaces-via-the-azure-cli"></a>Azure CLı aracılığıyla dev alanlarını etkinleştirme

Ayrıca, komut satırında dev alanlarını etkinleştirebilirsiniz:

```cmd
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

## <a name="deploy-your-teams-baseline-to-each-cluster"></a>Takımınızın temelini her kümeye dağıtma

Geliştirme alanları ile çalışırken, genellikle tüm uygulamayı Kubernetes kümenizdeki bir üst dev alanına dağıtırsınız. Varsayılan olarak `default` alanı kullanılır. İlk dağıtım, tüm hizmetlerin yanı sıra bu hizmetlerin bağımlı olduğu dış kaynakları ve örneğin veritabanları ya da kuyrukları içerir. Bu, *taban çizgisi*olarak bilinir. Üst dev alanında bir taban çizgisi ayarladıktan sonra, alt dev alanları içinde tek tek Hizmetleri yineleyebilir ve hata ayıklaması yapabilirsiniz.

Ana hat hizmet kümesinin en son sürümlerini birden çok bölgedeki kümelere dağıtmanız gerekir. Taban çizgisi hizmetlerinizin bu şekilde güncelleştirilmesi, bir Azure bölgesi arızası varsa geliştirme alanlarını kullanmaya devam edebilmenizi sağlar. Örneğin, taban çizgisini bir CI/CD işlem hattı aracılığıyla dağıtırsanız, işlem hattını farklı bölgelerde birden çok kümeye dağıtılacak şekilde değiştirin.

## <a name="select-the-correct-aks-cluster-to-use-for-dev-spaces"></a>Geliştirme alanları için kullanılacak doğru AKS kümesini seçin

Takımınızın temelini çalıştıran bir yedekleme kümesini düzgün bir şekilde yapılandırdıktan sonra dilediğiniz zaman yedekleme kümesine hızlıca geçiş yapabilirsiniz. Daha sonra, üzerinde çalıştığınız hizmetleri geliştirme alanlarında yeniden çalıştırabilirsiniz.

Aşağıdaki CLı komutuyla farklı bir küme seçin:

```cmd
az aks use-dev-spaces -g <new resource group name> -n <new cluster name>
```

Yeni kümede kullanılabilir dev alanlarını aşağıdaki komutla listeleyebilirsiniz:

```cmd
azds space list
```

Üzerinde çalışmak için yeni bir geliştirme alanı oluşturabilir veya var olan bir geliştirme alanını aşağıdaki komutla seçebilirsiniz:

```cmd
azds space select -n <space name>
```

Bu komutları çalıştırdıktan sonra, seçilen küme ve dev alanı sonraki CLı işlemleri için ve Azure Dev Spaces için Visual Studio Code uzantısı kullanılarak proje hata ayıklama için kullanılacaktır.

Visual Studio kullanıyorsanız, mevcut bir proje tarafından kullanılan kümeyi aşağıdaki adımlarla değiştirebilirsiniz:

1. Projenizi Visual Studio 'da açın.
1. Çözüm Gezgini ' de proje adına sağ tıklayın ve **Özellikler** ' e tıklayın.
1. Sol bölmede **Hata Ayıkla** ' ya tıklayın.
1. Hata ayıklama özellikleri sayfasında, **profil** açılır listesine tıklayın ve **Azure dev Spaces**' yi seçin.
1. **Değiştir** düğmesine tıklayın.
1. Görüntülenen iletişim kutusunda, kullanmak istediğiniz AKS kümesini seçin. İsterseniz, oturum açmak için farklı bir geliştirme alanı seçin veya **boşluk** açılır listesinden uygun seçeneği belirleyerek yeni bir geliştirme alanı oluşturun.

Doğru kümeyi ve boşluğu seçtikten sonra, geliştirme alanlarında hizmeti çalıştırmak için F5 'e basabilirsiniz.

Özgün kümeyi kullanmak üzere yapılandırılmış diğer tüm projeler için bu adımları tekrarlayın.

## <a name="access-a-service-on-a-backup-cluster"></a>Bir yedekleme kümesindeki bir hizmete erişme

Hizmetinizi ortak bir DNS adı kullanacak şekilde yapılandırdıysanız, bir yedekleme kümesinde çalıştırırsanız hizmetin farklı bir URL 'SI olur. Ortak DNS adları her zaman `<space name>.s.<root space name>.<service name>.<cluster GUID>.<region>.azds.io`biçimindedir. Farklı bir kümeye geçerseniz, küme GUID 'SI ve muhtemelen bölge değişir.

Geliştirme alanları her zaman `azds up`çalıştırılırken hizmetin doğru URL 'sini veya **Azure dev SPACES**altında Visual Studio 'daki çıkış penceresinde gösterir.

Ayrıca, `azds list-uris` komutunu çalıştırarak URL 'YI bulabilirsiniz:
```
$ azds list-uris
Uri                                                     Status
------------------------------------------------------  ---------
http://default.mywebapi.d05afe7e006a4fddb73c.eus.azds.io/  Available
```

Hizmete erişirken bu URL 'YI kullanın.
