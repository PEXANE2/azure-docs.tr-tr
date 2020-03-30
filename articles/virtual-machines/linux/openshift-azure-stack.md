---
title: Azure Yığını'nda OpenShift'i dağıtma
description: Azure Yığını'nda OpenShift'i dağıtın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/14/2019
ms.author: haroldw
ms.openlocfilehash: d6c73b8cd33aa85793a2ce839410065e03b97be7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74035542"
---
# <a name="deploy-openshift-container-platform-or-okd-in-azure-stack"></a>Azure Yığını'nda OpenShift Kapsayıcı Platformu veya OKD dağıtma

OpenShift Azure Yığını'nda dağıtılabilir. Azure ve Azure Yığını arasında bazı önemli farklar vardır, bu nedenle dağıtım biraz farklılık gösterir ve yetenekler de biraz farklılık gösterir.

Şu anda Azure Bulut Sağlayıcısı Azure Yığını'nda çalışmıyor. Bu nedenle, Azure Yığını'nda kalıcı depolama için disk eklesini kullanamazsınız. Bunun yerine, NFS, iSCSI, GlusterFS, vb. gibi diğer depolama seçeneklerini yapılandırabilirsiniz. Alternatif olarak, CNS'yi etkinleştirebilir ve kalıcı depolama için GlusterFS'yi kullanabilirsiniz. CNS etkinse, GlusterFS kullanımı için ek depolama alanı yla birlikte üç ek düğüm dağıtılır.

Azure Yığını'nda OpenShift Kapsayıcı Platformu'na veya OKD'yi dağıtmak için birkaç yöntemden birini kullanabilirsiniz:

- Gerekli Azure altyapı bileşenlerini el ile dağıtabilir ve [ardından OpenShift Kapsayıcı Platformu belgelerini](https://docs.openshift.com/container-platform) veya [OKD belgelerini takip edebilirsiniz.](https://docs.okd.io)
- OpenShift Kapsayıcı Platformu kümesinin dağıtımını kolaylaştıran varolan bir [Kaynak Yöneticisi şablonu](https://github.com/Microsoft/openshift-container-platform/) da kullanabilirsiniz.
- OKD kümesinin dağıtımını kolaylaştıran varolan bir [Kaynak Yöneticisi şablonu](https://github.com/Microsoft/openshift-origin) da kullanabilirsiniz.

Kaynak Yöneticisi şablonu kullanıyorsanız, uygun dalı seçin (azurestack-release-3.x). API sürümleri Azure ve Azure Yığını arasında farklı olduğundan Azure için şablonlar çalışmaz. RHEL görüntü başvurusu şu anda azuredeploy.json dosyasında bir değişken olarak sabit kodlanmış durumda ve resminize uyacak şekilde değiştirilmesi gerekir.

```json
"imageReference": {
    "publisher": "Redhat",
    "offer": "RHEL-OCP",
    "sku": "7-4",
    "version": "latest"
}
```

Tüm seçenekler için Red Hat aboneliği gereklidir. Dağıtım sırasında Red Hat Enterprise Linux örneği Red Hat aboneliğine kaydedilir ve OpenShift Konteyner Platformu'nun yataylıklarını içeren Pool ID'ye eklenir.
Geçerli bir Red Hat Subscription Manager (RHSM) kullanıcı adı, parola ve Havuz Kimliği olduğundan emin olun. Alternatif olarak, Etkinleştirme Anahtarı, Org Kimliği ve Havuz Kimliği kullanabilirsiniz.  Bu bilgileri' de https://access.redhat.comoturum atamama yla doğrulayabilirsiniz.

## <a name="azure-stack-prerequisites"></a>Azure Yığını ön koşulları

Bir OpenShift kümesini dağıtmak için Azure Yığını ortamınıza bir RHEL (OpenShift Kapsayıcı Platformu) veya CentOS görüntüsünün (OKD) eklenmesi gerekir. Bu resimleri eklemek için Azure Stack yöneticinize başvurun. Talimatları burada bulabilirsiniz:

- https://docs.microsoft.com/azure/azure-stack/azure-stack-add-vm-image
- https://docs.microsoft.com/azure/azure-stack/azure-stack-marketplace-azure-items
- https://docs.microsoft.com/azure/azure-stack/azure-stack-redhat-create-upload-vhd

## <a name="deploy-by-using-the-openshift-container-platform-or-okd-resource-manager-template"></a>OpenShift Kapsayıcı Platformu veya OKD Kaynak Yöneticisi şablonu kullanarak dağıtma

Kaynak Yöneticisi şablonu kullanarak dağıtmak için, giriş parametrelerini sağlamak için bir parametre dosyası kullanırsınız. Dağıtımı daha da özelleştirmek için GitHub repo'yu çatalla ve uygun öğeleri değiştirin.

Bazı yaygın özelleştirme seçenekleri şunlardır, ancak bunlarla sınırlı değildir:

- Burç VM boyutu (azuredeploy.json değişkeni)
- Adlandırma kuralları (azuredeploy.json'daki değişkenler)
- OpenShift küme özellikleri, ana bilgisayar dosyası (deployOpenShift.sh) üzerinden değiştirildi
- RHEL görüntü başvurusu (azuredeploy.json değişkeni)

Azure CLI'yi kullanarak dağıtma adımları için [OpenShift Kapsayıcı Platformu](./openshift-container-platform-3x.md) bölümündeki veya [OKD](./openshift-okd.md) bölümündeki uygun bölümü izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-container-platform-3x-post-deployment.md)
- [Azure'da OpenShift dağıtımını sorun giderme](./openshift-container-platform-3x-troubleshooting.md)