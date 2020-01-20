---
title: Azure Red Hat OpenShift 'e giriş
description: Kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için Microsoft Azure Red Hat OpenShift 'in özelliklerini ve avantajlarını öğrenin.
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: overview
ms.date: 05/08/2019
ms.custom: mvc
ms.openlocfilehash: 4be388eec3851154fd0f6af37d03a468fc52197b
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76276066"
---
# <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Microsoft *Azure Red Hat OpenShift* hizmeti, tam olarak yönetilen [OpenShift](https://www.openshift.com/) kümelerini dağıtmanıza olanak sağlar.

Azure Red Hat OpenShift, [Kubernetes](https://kubernetes.io/)'i genişletiyor. Kubernetes ile üretimde çalışan kapsayıcılar, bir görüntü kayıt defteri, depolama yönetimi, ağ çözümleri ve günlük ve izleme araçları gibi ek araçlar ve kaynaklar gerektirir; bunların hepsi sürümü oluşturma ve test edilmiş olmalıdır. Kapsayıcı tabanlı uygulamalar oluşturmak için, ara yazılım, çerçeveler, veritabanları ve CI/CD araçlarıyla daha fazla tümleştirme çalışması gerekir. Azure Red Hat OpenShift, tüm bunu tek bir platformda birleştirir ve bu sayede uygulama ekiplerinin yürütülmesi gerekenleri verirken BT ekiplerine işlem kolaylığı sağlar.

Azure Red Hat OpenShift, tümleşik bir destek deneyimi sağlamak üzere Red Hat ve Microsoft tarafından ortaklaşa tasarlanmış, çalıştırılır ve desteklenir. Çalışması gereken sanal makine yok ve düzeltme eki uygulama gerekli değil. Ana, altyapı ve uygulama düğümleri, Red Hat ve Microsoft tarafından sizin adınıza düzeltme eki uygulanmış, güncelleştirilir ve izlenir. Azure Red Hat OpenShift kümeleriniz Azure aboneliğinize dağıtılır ve Azure faturanızda yer alır.

Kendi kayıt defterinizi, ağ iletişimini, depolamayı ve CI/CD çözümlerinizi seçebilir veya otomatik kaynak kodu yönetimi, kapsayıcı ve uygulama yapıları, dağıtımlar, ölçeklendirme, sistem durumu yönetimi ve daha fazlası için yerleşik çözümleri kullanabilirsiniz. Azure Red Hat OpenShift, Azure Active Directory aracılığıyla tümleşik bir oturum açma deneyimi sağlar.

Başlamak için, [Azure Red Hat OpenShift kümesi oluşturma](tutorial-create-cluster.md) öğreticisini doldurun.

## <a name="access-security-and-monitoring"></a>Erişim, güvenlik ve izleme

Gelişmiş güvenlik ve yönetim için Azure Red Hat OpenShift, Azure Active Directory (Azure AD) ile tümleştirmenize ve Kubernetes rol tabanlı erişim denetimi 'ni (RBAC) kullanmanıza olanak tanır. Ayrıca, kümelerinizin ve kaynaklarınızın sistem durumunu da izleyebilirsiniz.

## <a name="cluster-and-node"></a>Küme ve düğüm

Azure Red Hat OpenShift düğümleri Azure sanal makinelerinde çalışır. Depoları düğümlere ve pod’lara bağlayabilir, küme bileşenlerini yükseltebilir ve GPU’lar kullanabilirsiniz.

## <a name="virtual-networks-and-ingress"></a>Sanal ağlar ve giriş

[Bir Azure Red Hat OpenShift kümesini eşleme yoluyla mevcut bir sanal ağa bağlayabilirsiniz](https://docs.microsoft.com/azure/openshift/tutorial-create-cluster#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) . Bu yapılandırmada, Pod eşlenmiş bir sanal ağdaki diğer hizmetlere bağlanabilir.

Ayrıntılar için bkz. [bir kümenin sanal ağını mevcut bir sanal ağa bağlama](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) .

## <a name="kubernetes-certification"></a>Kubernetes sertifikası

Azure Red Hat OpenShift hizmeti, Kubernetes uyumlu olarak CNCF sertifikalı.

## <a name="next-steps"></a>Sonraki adımlar

Azure Red Hat OpenShift için önkoşulları öğrenin:

> [!div class="nextstepaction"]
> [Geliştirme ortamınızı kurma](howto-setup-environment.md)
