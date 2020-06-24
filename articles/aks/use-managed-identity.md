---
title: Azure Kubernetes hizmetinde Yönetilen kimlikler kullanma
description: Azure Kubernetes hizmeti 'nde (AKS) yönetilen kimlikleri nasıl kullanacağınızı öğrenin
services: container-service
author: mlearned
ms.topic: article
ms.date: 06/04/2020
ms.author: mlearned
ms.openlocfilehash: d512cb94e9a6cef131433880703f8f5150da5a3c
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85249700"
---
# <a name="use-managed-identities-in-azure-kubernetes-service"></a>Azure Kubernetes hizmetinde Yönetilen kimlikler kullanma

Şu anda Azure Kubernetes hizmeti (AKS) kümesi (özellikle Kubernetes bulut sağlayıcısı), Azure 'da yük dengeleyiciler ve yönetilen diskler gibi ek kaynaklar oluşturmak için bir kimlik gerektirir. bu kimlik, *yönetilen bir kimlik* ya da *hizmet sorumlusu*olabilir. [Hizmet sorumlusu](kubernetes-service-principal.md)kullanıyorsanız, bir veya birden çok aks sizin adınıza bir tane oluşturuyor olmalıdır. Yönetilen kimlik kullanıyorsanız, bu işlem sizin için otomatik olarak oluşturulur. Hizmet sorumlularını kullanan kümeler sonunda, kümenin çalışmasını sağlamak için hizmet sorumlusunun yenilenmesi gereken bir duruma ulaşır. Hizmet sorumlularını yönetmek karmaşıklık ekler ve bunun yerine yönetilen kimliklerin kullanılması daha kolay olur. Aynı izin gereksinimleri hem hizmet sorumluları hem de yönetilen kimlikler için geçerlidir.

*Yönetilen kimlikler* temelde hizmet sorumluları etrafında bir sarmalayıcıdır ve yönetimini daha kolay hale getirir. Mı için kimlik bilgisi dönüşü Azure Active Directory varsayılan olarak her 46 günde bir gerçekleşir. AKS, sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik türlerini kullanır. Bu kimlikler Şu anda sabit. Daha fazla bilgi edinmek için [Azure kaynakları için Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)hakkında bilgi edinin.

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki kaynağın yüklü olması gerekir:

- Azure CLı, sürüm 2.2.0 veya üzeri

## <a name="limitations"></a>Sınırlamalar

* Kendi yönetilen kimliklerinizi getir şu anda desteklenmiyor.
* Yönetilen kimlikleri olan AKS kümeleri yalnızca kümenin oluşturulması sırasında etkinleştirilebilir.
* Mevcut AKS kümeleri, yönetilen kimlikleri etkinleştirmek için güncelleştirilemez veya yükseltilemez.
* Küme **yükseltme** işlemleri sırasında, yönetilen kimlik geçici olarak kullanılamaz.

## <a name="summary-of-managed-identities"></a>Yönetilen kimliklerin Özeti

AKS, yerleşik hizmetler ve eklentiler için birkaç yönetilen kimlik kullanır.

| Kimlik                       | Name    | Kullanım örneği | Varsayılan izinler | Kendi kimliğinizi getir
|----------------------------|-----------|----------|
| Denetim düzlemi | görünür değil | AKS tarafından ağ kaynaklarını yönetmek için kullanılır, örneğin, giriş, genel IP vb. için bir yük dengeleyici oluşturun.| Düğüm kaynak grubu için katkıda bulunan rolü | Şu anda desteklenmiyor
| Kubelet | AKS küme adı-agentpool | Azure Container Registry (ACR) ile kimlik doğrulaması | Düğüm kaynak grubu için okuyucu rolü | Şu anda desteklenmiyor
| Eklenti | AzureNPM | Kimlik gerekli değil | NA | No
| Eklenti | Azurecnı ağ izleme | Kimlik gerekli değil | NA | No
| Eklenti | azurepolicy (Gatekeeper) | Kimlik gerekli değil | NA | No
| Eklenti | azurepolicy | Kimlik gerekli değil | NA | No
| Eklenti | Calıco | Kimlik gerekli değil | NA | No
| Eklenti | Pano | Kimlik gerekli değil | NA | No
| Eklenti | HTTPApplicationRouting | Gerekli ağ kaynaklarını yönetir | Düğüm kaynak grubu için okuyucu rolü, DNS bölgesi için katkıda bulunan rolü | No
| Eklenti | Giriş uygulama ağ geçidi | Gerekli ağ kaynaklarını yönetir| Düğüm kaynak grubu için katkıda bulunan rolü | No
| Eklenti | omsagent | AKS ölçümlerini Azure Izleyicisine göndermek için kullanılır | İzleme ölçümleri Yayımcı rolü | No
| Eklenti | Sanal düğüm (ACIConnector) | Azure Container Instances (ACI) için gerekli ağ kaynaklarını yönetir | Düğüm kaynak grubu için katkıda bulunan rolü | No


## <a name="create-an-aks-cluster-with-managed-identities"></a>Yönetilen kimliklerle bir AKS kümesi oluşturma

Artık aşağıdaki CLı komutlarını kullanarak, yönetilen kimliklerle bir AKS kümesi oluşturabilirsiniz.

İlk olarak, bir Azure Kaynak grubu oluşturun:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Ardından, bir AKS kümesi oluşturun:

```azurecli-interactive
az aks create -g myResourceGroup -n MyManagedCluster --enable-managed-identity
```

Yönetilen kimlikler kullanılarak başarılı bir küme oluşturulması, bu hizmet sorumlusu profil bilgilerini içerir:

```json
"servicePrincipalProfile": {
    "clientId": "msi"
  }
```

> [!NOTE]
> Kendi VNet 'i, statik IP adresinizi veya kaynakların çalışan düğümü kaynak grubunun dışında olduğu bağlı Azure diskini oluşturmak ve kullanmak için, rol ataması gerçekleştirmek üzere küme sistemi tarafından atanan yönetilen kimliğin PrincipalId 'sini kullanın. Rol atama hakkında daha fazla bilgi için bkz. [diğer Azure kaynaklarına erişim yetkisi verme](kubernetes-service-principal.md#delegate-access-to-other-azure-resources).
>
> Azure bulut sağlayıcısı tarafından kullanılan küme tarafından yönetilen kimliğin izin verdiği süre, 60 dakika sürebilir.

Son olarak, kümeye erişmek için kimlik bilgilerini alın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```

Küme birkaç dakika içinde oluşturulacak. Böylece, uygulama iş yüklerinizi yeni kümeye dağıtabilir ve hizmet sorumlusu tabanlı AKS kümelerinde yaptığınız gibi etkileşimde bulunabilirsiniz.