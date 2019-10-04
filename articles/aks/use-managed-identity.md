---
title: Azure Kubernetes hizmetinde Yönetilen kimlikler kullanma
description: Azure Kubernetes hizmeti 'nde (AKS) yönetilen kimlikleri nasıl kullanacağınızı öğrenin
services: container-service
author: saudas
manager: saudas
ms.service: container-service
ms.topic: article
ms.date: 09/11/2019
ms.author: saudas
ms.openlocfilehash: a5717d8ee44e4d2e086a6e7bc1b7c3d0deb614c8
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827536"
---
# <a name="preview---use-managed-identities-in-azure-kubernetes-service"></a>Önizleme-Azure Kubernetes hizmetinde Yönetilen kimlikler kullanma

Şu anda, kullanıcılar bir hizmet sorumlusu sağlamalıdır veya AKS kümesi için (özellikle Kubernetes bulut sağlayıcısı) Azure 'da yük dengeleyiciler ve yönetilen diskler gibi ek kaynaklar oluşturmak için sizin adınıza bir tane oluşturuyor olmalıdır. Hizmet sorumluları genellikle sona erme tarihi ile oluşturulur. Kümeler sonunda, hizmet sorumlusunun yenilenmesi gereken bir duruma ulaşacaktır, aksi takdirde küme çalışmaz. Hizmet sorumlularını yönetmek karmaşıklık ekler. Yönetilen kimlikler temelde hizmet sorumluları etrafında bir sarmalayıcıdır ve yönetimini daha kolay hale getirir. [Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) hakkında daha fazla bilgi edinin makalesi.

AKS, bir sistem tarafından atanan yönetilen kimliğe ve diğer Kullanıcı tarafından atanan kimliğe iki yönetilen kimlik oluşturur. Bir sistem tarafından atanmış yönetilen kimlik, Azure kaynaklarını Kullanıcı adına oluşturmak için Kubernetes bulut sağlayıcısı tarafından kullanılır. Bu sistem tarafından atanan yönetilen kimliğin yaşam döngüsü, kümeye bağlıdır ve küme silindiğinde silinir. AKS Ayrıca, AKS 'nin ACRs 'ye erişmesi, kubelet 'in Azure 'dan meta verileri almasına, vb. erişim yetkisi veren bir kullanıcı tarafından atanan yönetilen kimlik oluşturur.

Bu önizleme döneminde bir hizmet sorumlusu hala gereklidir. İzleme, sanal düğüm, Azure ilkesi ve http uygulama yönlendirme gibi eklentilerin yetkilendirmesi için kullanılacaktır. SPN 'deki addons bağımlılığının kaldırılması devam eden bir iş vardır ve AKS 'deki SPN gereksinimi tamamen kaldırılacak.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis kabul etme sürecindedir. Önizlemeler, "olduğu gibi" ve "kullanılabilir olarak" verilmiştir ve hizmet düzeyi sözleşmelerinden ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteğinin en iyi çaba temelinde kısmen ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri](support-policies.md)
> * [Azure desteği SSS](faq.md)

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdakilere sahip olmanız gerekir:

* Ayrıca Azure CLı sürüm 2.0.70 veya üzeri ve aks-Preview 0.4.14 uzantısı gerekir

## <a name="install-latest-aks-cli-preview-extension"></a>En son AKS CLı önizleme uzantısını yükler

**Aks-Preview 0.4.14** uzantısına veya daha yenisine ihtiyacınız vardır.

```azurecli
az extension update --name aks-preview 
az extension list
```

> [!CAUTION]
> Bir abonelik üzerinde bir özelliği kaydettiğinizde, o özelliği şu anda kaydedemezsiniz. Bazı Önizleme özelliklerini etkinleştirdikten sonra, daha sonra abonelikte oluşturulan tüm AKS kümeleri için varsayılanlar kullanılabilir. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name MSIPreview --namespace Microsoft.ContainerService
```

Durumun *kaydı*gösterilmesi birkaç dakika sürebilir. [Az Feature List] [az-Feature-List] komutunu kullanarak kayıt durumunu denetleyebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MSIPreview')].{Name:name,State:properties.state}"
```

Durum kaydedildiğinde, [az Provider Register] [az-Provider-Register] komutunu kullanarak *Microsoft. ContainerService* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="create-an-aks-cluster-with-managed-identity"></a>Yönetilen kimliğe sahip bir AKS kümesi oluşturma

Artık aşağıdaki CLı komutunu kullanarak, yönetilen kimliklerle bir AKS kümesi oluşturabilirsiniz
```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma
```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-managed-identity
```

## <a name="get-credentials-to-access-the-cluster"></a>Kümeye erişmek için kimlik bilgilerini alın
```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```
Küme birkaç dakika içinde oluşturulduktan sonra, uygulama iş yüklerinizi dağıtabilir ve hizmet sorumlusu tabanlı AKS kümeleriyle yaptığınız gibi etkileşime geçebilirsiniz. 

> [!IMPORTANT]
> * Yönetilen kimlikleri olan AKS kümeleri yalnızca kümenin oluşturulması sırasında etkinleştirilebilir
> * Mevcut AKS kümeleri, yönetilen kimlikleri etkinleştirmek için güncelleştirilemez/yükseltilemez