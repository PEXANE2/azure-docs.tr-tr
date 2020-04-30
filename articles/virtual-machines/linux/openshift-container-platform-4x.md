---
title: Azure 'da OpenShift kapsayıcı platformu 4. x dağıtma
description: Azure 'da OpenShift kapsayıcı platformu 4. x dağıtın.
author: haroldwongms
manager: mdotson
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.workload: infrastructure
ms.date: 10/14/2019
ms.author: haroldw
ms.openlocfilehash: 14af110b5cf50f167d0c4961e26454bc33c6ed7d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81759484"
---
# <a name="deploy-openshift-container-platform-4x-in-azure"></a>Azure 'da OpenShift kapsayıcı platformu 4. x dağıtma

OpenShift kapsayıcı platformu (OCP) 4,2 dağıtımı artık Azure 'da yükleyici tarafından sağlanan altyapı (ıPı) modeli aracılığıyla desteklenmektedir.  Openshıft 4 ' ü denemek için giriş sayfası [TRY.OpenShift.com](https://try.openshift.com/)' dir. Azure 'da OCP 4,2 'yi yüklemek için [Red Hat OpenShift Küme Yöneticisi](https://cloud.redhat.com/openshift/install/azure/installer-provisioned) sayfasını ziyaret edin.  Bu siteye erişmek için Red Hat kimlik bilgileri gereklidir.


## <a name="notes"></a>Notlar 

 - Azure 'da OCP 4. x yüklemek ve çalıştırmak için bir Azure Active Directory (AAD) hizmet sorumlusu (SP) gerekir
     - SP 'ye Azure Active Directory Graph için **Application. ReadWrite. OwnedBy** API izni verilmelidir
     - AAD Kiracı Yöneticisi, bu API izninin etkili olabilmesi için yönetici onayı vermelidir
     - Aboneliğe **katkıda bulunan** ve **Kullanıcı erişimi yönetici** rollerinin verilmesi gerekir
 - OCP 4. x için yükleme modeli 3. x 'den farklı ve Azure 'da OCP 4. x dağıtımı için kullanılabilecek Azure Resource Manager şablonu yok
 - Yükleme işlemi sırasında sorunlarla karşılaşılırsa, ilgili şirkete başvurun (Microsoft veya Red hat)

| Sorun Açıklaması | İletişim noktası |
|-------------------|---------------|
| Azure 'a özgü sorunlar (AAD, SP, Azure aboneliği vb.)                              | Microsoft |
| OpenShift 'e özgü sorunlar (yükleme hataları/hataları, Red Hat aboneliği vb.) |  Red Hat  |




## <a name="next-steps"></a>Sonraki adımlar

- [OpenShift kapsayıcı platformu ile çalışmaya başlama](https://docs.openshift.com)
