---
title: Azure Kubernetes Service (AKS) genel önizlemede gizli bilgi işlem düğümleri
description: AKS üzerinde gizli bilgi işlem düğümleri
services: virtual-machines
author: agowdamsft
ms.service: container-service
ms.topic: overview
ms.date: 9/22/2020
ms.author: amgowda
ms.openlocfilehash: a009cd7763b4a4dc0c502d4c47a20d6fdffe61d7
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92125450"
---
# <a name="confidential-computing-nodes-on-azure-kubernetes-service-public-preview"></a>Azure Kubernetes hizmetindeki gizli bilgi işlem düğümleri (Genel Önizleme)

[Azure gizli bilgi işlem](overview.md) , önemli verilerinizi kullanımda iken korumanıza olanak sağlar. Temel alınan altyapılar, donanım tarafından desteklenen bir güvenilir yürütme kapsayıcı ortamları ile diğer uygulamalardan, yöneticilerden ve bulut sağlayıcılarından bu verileri korur.

## <a name="overview"></a>Genel Bakış

Azure Kubernetes hizmeti (AKS), Intel SGX tarafından desteklenen [DCsv2 gizli bilgi işlem düğümlerinin](confidential-computing-enclaves.md) eklenmesini destekler. Bu düğüm çalıştırmaları, kullanıcı düzeyindeki kodun özel belleğe ayrılmasına izin vererek, kritik iş yüklerini donanım tabanlı bir güvenilir yürütme ortamı (t) içinde çalıştırabilir. Bu özel bellek bölgeleri, şifreleme olarak adlandırılır. Kuşışları, daha yüksek bir ayrıcalıkla çalışan işlemlerden kodu ve verileri korumak için tasarlanmıştır. SGX yürütme modeli, Konuk işletim sistemi, konak işletim sistemi ve hiper yönetici ara katmanlarını kaldırır. *Kapsayıcı başına yalıtılmış yürütme modeli tabanlı donanım* , uygulamaların özel bir bellek bloğunu ŞIFRELI tutarken CPU ile doğrudan yürütülmesini sağlar. Gizli bilgi işlem düğümleri, AKS 'teki kapsayıcı uygulamalarının genel güvenlik duruşunu ve derinlemesine savunma kapsayıcısı stratejisi hakkında harika bir ek yardım sağlar. 

![SGX düğümüne genel bakış](./media/confidential-nodes-aks-overview/sgxaksnode.jpg)

## <a name="aks-confidential-nodes-features"></a>AKS gizli düğümleri özellikleri

- SGX güvenilir yürütme ortamı (t) ile donanım tabanlı ve işlem düzeyi kapsayıcı yalıtımı 
- Heterojen düğüm havuzu kümeleri (gizli ve gizli olmayan düğüm havuzlarını karıştırma)
- Şifrelenmiş sayfa önbelleği (EPC) bellek tabanlı Pod zamanlaması
- SGX DCAP sürücüsü önceden yüklendi
- Intel FSGS düzeltme eki önceden yüklenmiş
- CPU tüketimi tabanlı yatay Pod otomatik ölçeklendirmeyi ve küme otomatik ölçeklendirmeyi destekler
- AKS daemonset aracılığıyla proc kanıtlama Yardımcısı
- Linux kapsayıcıları Ubuntu 18,04 Gen 2 VM çalışan düğümleri aracılığıyla desteklenir

## <a name="aks-provided-daemon-sets-addon"></a>AKS tarafından sunulan Daemon kümeleri (eklenti)

#### <a name="sgx-device-plugin"></a>SGX cihaz eklentisi <a id="sgx-plugin"></a>

SGX cihaz eklentisi, EPC belleği için Kubernetes cihaz eklentisi arabirimini uygular. Etkin olarak, bu eklenti, EPC belleğini Kubernetes 'te ek bir kaynak türü yapar. Kullanıcılar, bu kaynak üzerinde diğer kaynaklar gibi sınırlar belirtebilir. Zamanlama işlevinden ayrı olarak, cihaz eklentisi gizli iş yükü kapsayıcılarına SGX cihaz sürücüsü izinleri atamaya yardımcı olur. EPC bellek tabanlı dağıtım () örneğinin örnek bir uygulamasını `kubernetes.azure.com/sgx_epc_mem_in_MiB` [burada bulabilirsiniz](https://github.com/Azure-Samples/confidential-computing/blob/main/containersamples/helloworld/helm/templates/helloworld.yaml)

#### <a name="sgx-quote-helper-service"></a>SGX quote yardımcı hizmeti <a id="sgx-quote"></a>

Uzaktan kanıtlama gerçekleştiren uygulamaların bir TEKLIF oluşturması gerekir. TEKLIF, kimliğin ve uygulamanın durumunun ve içinde çalıştığı ortamın üzerinde şifreleme sağlaması sağlar. TEKLIF üretimi, SGX platformu yazılım bileşenlerinin (PSW/DCAP) bir parçası olan belirli güvenilir yazılım bileşenlerine dayanır. Bu PSW, düğüm başına çalışan bir Daemon kümesi olarak paketlenmiştir. Bu, yararlanılabilir ve uygulamalardan gelen kanıtlama teklıfı istenirken olabilir. AKS tarafından sağlanan hizmetin kullanılması, konaktaki PSW ve diğer yazılım bileşenleri arasındaki uyumluluğun daha iyi korunmasına yardımcı olur. Kullanımı ve özellik ayrıntıları hakkında [daha fazla bilgi edinin](confidential-nodes-out-of-proc-attestation.md) .

## <a name="programming--application-models"></a>& uygulama modellerini programlama

### <a name="confidential-containers"></a>Gizli kapsayıcılar

[Gizli kapsayıcılar](confidential-containers.md) , mevcut programları ve çoğu **ortak programlama dili** çalışma zamanını (Python, Node, Java vb.), kaynak kodu değişikliği veya yeniden derleme olmadan mevcut kitaplık bağımlılıklarıyla birlikte çalıştırır. Bu model, Azure Iş ortakları & açık kaynak projelerle gizliliği sağlamak için en hızlı modeldir. Güvenli şifreleyler içinde çalışmak üzere hazırlama için hazırlanmış olan kapsayıcı görüntüleri gizli kapsayıcılar olarak adlandırılır.

### <a name="enclave-aware-containers"></a>Şifreleme kullanan kapsayıcılar

AKS, gizli düğümlerde çalışmak üzere programlanan uygulamaları destekler ve SDK 'lar ve çerçeveler aracılığıyla sunulan **özel yönerge kümesini** kullanır. Bu uygulama modeli en düşük güvenilir bilgi Işlem tabanı (TCB) ile uygulamalarınıza en çok denetim sağlar. Şifreleme kullanan kapsayıcılar hakkında [daha fazla bilgi edinin](enclave-aware-containers.md) .

## <a name="next-steps"></a>Sonraki Adımlar

[AKS kümesini gizli bilgi işlem düğümleri ile dağıtma](./confidential-nodes-aks-get-started.md)

[Hızlı başlangıç gizli kapsayıcı örnekleri](https://github.com/Azure-Samples/confidential-container-samples)

[DCsv2 SKU listesi](https://docs.microsoft.com/azure/virtual-machines/dcv2-series)

<!-- LINKS - external -->
[Azure Attestation]: https://docs.microsoft.com/en-us/azure/attestation/


<!-- LINKS - internal -->
[DC Virtual Machine]: /confidential-computing/virtual-machine-solutions
