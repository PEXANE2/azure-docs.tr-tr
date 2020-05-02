---
title: Azure Red Hat OpenShift 'e giriş
description: Kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için Microsoft Azure Red Hat OpenShift 'in özelliklerini ve avantajlarını öğrenin.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: overview
ms.date: 04/24/2020
ms.custom: mvc
ms.openlocfilehash: a3bdc3673474b778aa7c1003e48e215bac6d05bf
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82628529"
---
# <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Microsoft *Azure Red Hat OpenShift* hizmeti, tam olarak yönetilen [OpenShift](https://www.openshift.com/) kümelerini dağıtmanıza olanak sağlar.

Azure Red Hat OpenShift, [Kubernetes](https://kubernetes.io/)'i genişletiyor. Kubernetes ile üretimde çalışan kapsayıcılar ek araçlar ve kaynaklar gerektirir. Bu genellikle görüntü kayıt defterleri, depolama yönetimi, ağ çözümleri ve günlüğe kaydetme ve izleme araçlarının birlikte kullanılması ve bunların birlikte yönetilmesi ve test olması gereken tüm özellikleri içerir. Kapsayıcı tabanlı uygulamalar oluşturmak için, ara yazılım, çerçeveler, veritabanları ve CI/CD araçlarıyla daha fazla tümleştirme çalışması gerekir. Azure Red Hat OpenShift, tüm bunu tek bir platformda birleştirir ve bu sayede uygulama ekiplerinin yürütülmesi gerekenleri verirken BT ekiplerine işlem kolaylığı sağlar.

Azure Red Hat OpenShift, tümleşik bir destek deneyimi sağlamak üzere Red Hat ve Microsoft tarafından ortaklaşa tasarlanmış, çalıştırılır ve desteklenir. Çalışması gereken sanal makine yok ve düzeltme eki uygulama gerekli değil. Ana, altyapı ve uygulama düğümleri, Red Hat ve Microsoft tarafından sizin adınıza düzeltme eki uygulanmış, güncelleştirilir ve izlenir. Azure Red Hat OpenShift kümeleriniz Azure aboneliğinize dağıtılır ve Azure faturanızda yer alır.

Kendi kayıt defterinizi, ağ iletişimini, depolamayı ve CI/CD çözümlerinizi seçebilir veya otomatik kaynak kodu yönetimi, kapsayıcı ve uygulama yapıları, dağıtımlar, ölçeklendirme, sistem durumu yönetimi ve daha fazlası için yerleşik çözümleri kullanabilirsiniz. Azure Red Hat OpenShift, Azure Active Directory aracılığıyla tümleşik bir oturum açma deneyimi sağlar.

Başlamak için, [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md) öğreticisini doldurun.

## <a name="access-security-and-monitoring"></a>Erişim, güvenlik ve izleme

Gelişmiş güvenlik ve yönetim için Azure Red Hat OpenShift, Azure Active Directory (Azure AD) ile tümleştirmenize ve Kubernetes rol tabanlı erişim denetimi 'ni (RBAC) kullanmanıza olanak tanır. Ayrıca, kümelerinizin ve kaynaklarınızın sistem durumunu da izleyebilirsiniz.

## <a name="cluster-and-node"></a>Küme ve düğüm

Azure Red Hat OpenShift düğümleri Azure sanal makinelerinde çalışır. Depolama alanı düğümlerine ve yığınlara bağlanabilir ve küme bileşenlerini yükseltebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Red Hat OpenShift için önkoşulları öğrenin:

> [!div class="nextstepaction"]
> [Geliştirme ortamınızı kurma](tutorial-create-cluster.md)
