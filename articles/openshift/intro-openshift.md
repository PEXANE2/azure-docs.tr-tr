---
title: Azure Red Hat OpenShift'e Giriş
description: Kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için Microsoft Azure Red Hat OpenShift'in özelliklerini ve avantajlarını öğrenin.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: overview
ms.date: 05/08/2019
ms.custom: mvc
ms.openlocfilehash: 4be388eec3851154fd0f6af37d03a468fc52197b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76276066"
---
# <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Microsoft *Azure Red Hat OpenShift* hizmeti, tam olarak yönetilen [OpenShift](https://www.openshift.com/) kümelerini dağıtmanızı sağlar.

Azure Red Hat OpenShift [Kubernetes](https://kubernetes.io/)genişletir. Kapsayıcıları Kubernetes ile üretimde çalıştırmak, görüntü kayıt defteri, depolama yönetimi, ağ çözümleri ve günlük ve izleme araçları gibi ek araçlar ve kaynaklar gerektirir ve bunların tümü birlikte sürülür ve test edilmelidir. Kapsayıcı tabanlı uygulamalar oluşturmak, ara yazılımlar, çerçeveler, veritabanları ve CI/CD araçlarıyla daha fazla tümleştirme çalışması gerektirir. Azure Red Hat OpenShift tüm bunları tek bir platformda birleştirerek BT ekiplerine işlem kolaylığı sağlarken uygulama ekiplerine yürütmeleri için gerekenleri sunar.

Azure Red Hat OpenShift, entegre bir destek deneyimi sağlamak için Red Hat ve Microsoft tarafından ortaklaşa tasarlanmış, işletilmektedir ve desteklenir. Çalışacak sanal makine yoktur ve yama gerekmez. Ana, altyapı ve uygulama düğümleri Red Hat ve Microsoft tarafından sizin adınıza yamalı, güncellenir ve izlenir. Azure Kırmızı Şapka OpenShift kümeleriniz Azure aboneliğinize dağıtılır ve Azure faturanıza dahildir.

Kendi kayıt defterinizi, ağ, depolama ve CI/CD çözümlerini seçebilir veya otomatik kaynak kodu yönetimi, kapsayıcı ve uygulama oluşturma, dağıtımlar, ölçekleme, sistem durumu yönetimi ve daha fazlası için yerleşik çözümleri kullanabilirsiniz. Azure Red Hat OpenShift, Azure Active Directory aracılığıyla tümleşik bir oturum açma deneyimi sunar.

Başlamak için Azure [Kırmızı Şapka OpenShift küme](tutorial-create-cluster.md) oluşturma eğitimini tamamlayın.

## <a name="access-security-and-monitoring"></a>Erişim, güvenlik ve izleme

Azure Red Hat OpenShift, gelişmiş güvenlik ve yönetim için Azure Active Directory (Azure AD) ile tümleştirmenize ve Kubernetes rol tabanlı erişim denetimini (RBAC) kullanmanıza olanak tanır. Ayrıca, kümelerinizin ve kaynaklarınızın sistem durumunu da izleyebilirsiniz.

## <a name="cluster-and-node"></a>Küme ve düğüm

Azure Red Hat OpenShift düğümleri Azure sanal makinelerde çalışır. Depoları düğümlere ve pod’lara bağlayabilir, küme bileşenlerini yükseltebilir ve GPU’lar kullanabilirsiniz.

## <a name="virtual-networks-and-ingress"></a>Sanal ağlar ve giriş

Bir [Azure Red Hat OpenShift kümesini,](https://docs.microsoft.com/azure/openshift/tutorial-create-cluster#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) gözleyerek varolan bir sanal ağa bağlayabilirsiniz. Bu yapılandırmada, baklalar eşlenen bir sanal ağdaki diğer hizmetlere bağlanabilir.

Bkz. Bir kümenin sanal ağını ayrıntılar için [varolan bir sanal ağa bağlayın.](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network)

## <a name="kubernetes-certification"></a>Kubernetes sertifikası

Azure Red Hat OpenShift hizmeti, CNCF sertifikalı kubernetes konforde satılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Red Hat OpenShift için ön koşulları öğrenin:

> [!div class="nextstepaction"]
> [Geliştirme ortamınızı kurma](howto-setup-environment.md)
