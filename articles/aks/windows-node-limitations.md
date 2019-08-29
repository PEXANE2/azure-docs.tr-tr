---
title: Azure Kubernetes Service (AKS) içindeki Windows Server düğüm havuzlarının sınırlamaları
description: Azure Kubernetes Service 'te (AKS) Windows Server düğüm havuzlarını ve uygulama iş yüklerini çalıştırırken oluşan bilinen sınırlamalar hakkında bilgi edinin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: c2c9e3d29ced5f75873656e253ecdbab5efe7df8
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114418"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içindeki Windows Server düğüm havuzları ve uygulama iş yükleri için geçerli sınırlamalar

Azure Kubernetes Service (AKS) ' de, düğümlerde Konuk işletim sistemi olarak Windows Server çalıştıran bir düğüm havuzu oluşturabilirsiniz. Bu düğümler, .NET Framework yerleşik olanlar gibi yerel Windows kapsayıcı uygulamaları çalıştırabilir. Linux ve Windows işletim sisteminin kapsayıcı desteği sağladığı önemli farklılıklar söz konusu olduğunda, bazı yaygın Kubernetes ve pod ile ilgili özellikler Şu anda Windows düğüm havuzları için kullanılamaz.

Bu makalede, AKS 'deki Windows Server düğümlerine yönelik bazı sınırlamalar ve işletim sistemi kavramları özetlenmektedir. Windows Server için düğüm havuzları Şu anda önizlemededir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis kabul etme sürecindedir. Önizlemeler, "olduğu gibi" ve "kullanılabilir olarak" verilmiştir ve hizmet düzeyi sözleşmelerinden ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteğinin en iyi çaba temelinde kısmen ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="limitations-for-windows-server-in-kubernetes"></a>Kubernetes 'te Windows Server için sınırlamalar

Windows Server kapsayıcıları, Windows tabanlı bir kapsayıcı ana bilgisayarında çalıştırılmalıdır. AKS 'de Windows Server kapsayıcıları çalıştırmak için, Konuk işletim sistemi olarak [Windows Server çalıştıran bir düğüm havuzu oluşturabilirsiniz][windows-node-cli] . Pencere sunucusu düğüm havuzu desteği, Kubernetes projesinde yukarı akış Windows Server 'ın bir parçası olan bazı sınırlamalar içerir. Bu sınırlamalar AKS 'e özgü değildir. Kubernetes 'te Windows Server için bu yukarı akış desteği hakkında daha fazla bilgi için, bkz. [Kubernetes 'Teki Windows Server kapsayıcıları](https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/#supported-functionality-and-limitations).

Kubernetes içindeki Windows Server kapsayıcıları için aşağıdaki yukarı akış sınırlamaları AKS ile ilgilidir:

- Windows Server kapsayıcıları yalnızca temeldeki Windows Server düğümü IŞLETIM sistemiyle eşleşen Windows Server 2019 ' i kullanabilir.
    - Temel işletim sistemi olarak Windows Server 2016 kullanılarak oluşturulan kapsayıcı görüntüleri desteklenmez.
- Ayrıcalıklı kapsayıcılar kullanılamaz.
- RunAsUser, SELinux, AppArmor veya POSIX özellikleri gibi Linux 'a özgü özellikler Windows Server kapsayıcılarında kullanılamaz.
    - UıUı/GUID gibi Linux 'a özgü olan dosya sistemi sınırlamaları, Kullanıcı başına izinler de Windows Server kapsayıcılarında kullanılamaz.
- Azure diskleri ve Azure dosyaları, Windows Server kapsayıcısında NTFS birimleri olarak erişilen desteklenen birim türleridir.
    - NFS tabanlı depolama/birimler desteklenmez.

## <a name="aks-limitations-for-windows-server-node-pools"></a>Windows Server düğüm havuzları için AKS sınırlamaları

AKS 'deki Windows Server düğüm havuzu desteği için aşağıdaki ek sınırlamalar geçerlidir:

- AKS kümesi, her zaman ilk düğüm havuzu olarak bir Linux düğüm havuzu içerir. AKS kümesinin kendisi silinmediği takdirde bu ilk Linux tabanlı düğüm havuzu silinemez.
- AKS kümelerinin Azure CNı (Gelişmiş) ağ modelini kullanması gerekir.
    - Kubenet (temel) ağı desteklenmez. Kubenet kullanan bir AKS kümesi oluşturamazsınız. Ağ modellerindeki farklılıklar hakkında daha fazla bilgi için bkz. [AKS 'teki uygulamalar Için ağ kavramları][azure-network-models].
    - Azure CNı ağ modeli, IP adresi yönetimi için ek planlama ve dikkat edilecek noktalar gerektirir. Azure CNı 'nin nasıl planlanacağı ve uygulanacağı hakkında daha fazla bilgi için bkz. [AKS 'de Azure CNI ağını yapılandırma][configure-azure-cni].
- En son düzeltme eki düzeltmeleri ve güncelleştirmeleri sürdürmek için AKS 'deki Windows Server düğümleri en son Windows Server 2019 sürümüne *yükseltilmelidir* . Windows güncelleştirmeleri, AKS 'deki temel düğüm görüntüsünde etkin değildir. Windows Update yayın döngüsünün ve kendi doğrulama işleminizin etrafında düzenli bir zamanlamaya göre, AKS kümenizdeki Windows Server düğüm havuzunda bir yükseltme gerçekleştirmeniz gerekir. Bir Windows Server düğüm havuzunu yükseltme hakkında daha fazla bilgi için bkz. [AKS 'de düğüm havuzunu yükseltme][nodepool-upgrade].
    - Bu Windows Server düğüm yükseltmeleri, eski düğüm kaldırılmadan önce yeni bir düğümün dağıtıldığı bir sanal ağ alt ağında ek IP adreslerini geçici olarak kullanır.
    - Yeni bir düğüm dağıtıldığında, eski düğüm kaldırıldıktan sonra da vCPU kotaları abonelikte geçici olarak tüketilebilir.
    - Aks 'deki Linux düğümlerinde olduğu gibi kullanarak `kured` yeniden başlatmaları otomatik olarak güncelleştiremez ve yönetemezsiniz.
- AKS kümesinde en fazla sekiz düğüm havuzu olabilir.
    - Bu sekiz düğüm havuzunda en fazla 400 düğüme sahip olabilirsiniz.
- Windows Server düğüm havuzu adının 6 karakterlik bir sınırı vardır.
- Ağ Ilkesi ve küme otomatik algılama gibi AKS 'deki Önizleme özellikleri Windows Server düğümleri için onaylanmamış.
- Giriş denetleyicileri yalnızca bir NodeSelector kullanılarak Linux düğümlerinde zamanlanmalıdır.
- Azure Dev Spaces Şu anda yalnızca Linux tabanlı düğüm havuzları için kullanılabilir.
- Windows Server düğümleri bir Active Directory etki alanına katılmamışsa grup tarafından yönetilen hizmet hesapları (gMSA) desteği şu anda AKS 'de kullanılamaz.
    - Açık kaynaklı, yukarı akış [aks-motor][aks-engine] projesi şu anda bu özelliği kullanmanız gerekiyorsa gMSA desteği sağlıyor.

## <a name="os-concepts-that-are-different"></a>Farklı işletim sistemi kavramları

Kubernetes, geçmişte Linux odaklı. Yukarı akış [Kubernetes.io][kubernetes] Web sitesinde kullanılan birçok örnek, Linux düğümlerinde kullanılmak üzere tasarlanmıştır. Windows Server kapsayıcıları kullanan dağıtımlar oluşturduğunuzda, işletim sistemi düzeyinde aşağıdaki noktalar geçerlidir:

- **Identity** -Linux, tamsayı türleri olarak temsil edilen UserID (UID) ve GROUPıD (GID) kullanır. Kullanıcı ve grup adları kurallı değildir; yalnızca */etc/Groups* ' ın veya bir */etc/passwd* ' in UID + GID ' ye geri doğru bir diğer addır.
    - Windows Server, Windows güvenlik erişimi Yöneticisi (SAM) veritabanında depolanan daha büyük bir ikili güvenlik tanımlayıcısı (SID) kullanır. Bu veritabanı, konak ve kapsayıcılar ile ya da kapsayıcılar arasında paylaşılmaz.
- **Dosya izinleri** -Windows Server, bir bit MASKESI ve UID + GID yerine SID 'lere dayalı bir erişim denetim listesi kullanır
- **Dosya yolları** -Windows Server 'daki kural/yerine \ ' i kullanmaktır.
    - Birimleri takmaya yönelik Pod özellikleri, Windows Server kapsayıcıları için yolu doğru belirtin. Örneğin, bir Linux kapsayıcısında */mnt/Volume* bağlama noktası yerine, *K:* sürücüsü olarak takılacak bir sürücü harfi ve */k/birimi* gibi bir konum belirtin.

## <a name="next-steps"></a>Sonraki adımlar

AKS 'de Windows Server kapsayıcıları kullanmaya başlamak için [AKS 'de Windows Server çalıştıran bir düğüm havuzu oluşturun][windows-node-cli].

<!-- LINKS - external -->
[upstream-limitations]: https://kubernetes.io/docs/setup/windows/#limitations
[kubernetes]: https://kubernetes.io
[aks-engine]: https://github.com/azure/aks-engine

<!-- LINKS - internal -->
[azure-network-models]: concepts-network.md#azure-virtual-networks
[configure-azure-cni]: configure-azure-cni.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[windows-node-cli]: windows-container-cli.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[azure-outbound-traffic]: ../load-balancer/load-balancer-outbound-connections.md#defaultsnat
