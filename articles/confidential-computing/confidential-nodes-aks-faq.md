---
title: Azure Kubernetes Service (AKS) üzerinde gizli düğümler desteği hakkında sık sorulan sorular
description: Azure Kubernetes hizmeti (AKS) & Azure gizli bilgi Işlem (ACC) düğümleri desteğiyle ilgili bazı yaygın soruların yanıtlarını bulun.
author: agowdamsft
ms.service: container-service
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: amgowda
ms.openlocfilehash: b4b6c04a1e35031387a006b6350ebf6cbe502fee
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90998242"
---
# <a name="frequently-asked-questions-about-confidential-computing-nodes-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) üzerinde gizli bilgi Işlem düğümleri hakkında sık sorulan sorular

Bu makalede, Azure Kubernetes Service (AKS) üzerinde Intel SGX tabanlı gizli bilgi işlem düğümleri hakkında sık sorulan sorular ele alınmaktadır. Başka sorularınız varsa, e-posta gönderin acconaks@microsoft.com .

## <a name="what-service-level-agreement-sla-and-azure-support-is-provided-during-the-preview"></a>Önizleme sırasında ne Hizmet Düzeyi Sözleşmesi (SLA) ve Azure desteği sunulmaktadır? 

[Burada](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)tanımlanan ürün ÖNIZLEMESI sırasında SLA sağlanmaz. Ancak ürün desteği, Azure desteği aracılığıyla sağlanır.

## <a name="what-is-attestation-and-how-can-we-do-attestation-of-apps-running-in-enclaves"></a>Kanıtlama nedir ve şifreli olarak çalışan uygulamalar için kanıtlama yapabilir mi? 

Kanıtlama, belirli donanım platformunda yazılım parçasının düzgün bir şekilde örneğinin oluşturulmasını gösteren ve doğrulama işlemidir. Ayrıca, bu özelliğin güvenli bir platformda çalıştığı ve kurcalanmadığından emin olmak için kanıtsızmasını sağlar. Kuşatma uygulamaları için kanıtlama 'nın nasıl yapıldığı hakkında [daha fazla bilgi edinin](attestation.md) .

## <a name="can-i-bring-my-existing-containerized-applications-and-run-it-on-aks-with-azure-confidential-computing"></a>Mevcut Kapsayıcılı uygulamalarınızı getirebilir ve Azure gizli bilgi Işlem ile AKS 'de çalıştırabilir miyim? 

Evet, platform etkinleştiricileri hakkında daha fazla ayrıntı için [gizli kapsayıcılar sayfasını](confidential-containers.md) gözden geçirin.

## <a name="what-intel-sgx-driver-version-is-installed-in-the-aks-image"></a>AKS görüntüsüne Intel SGX sürücü sürümü yüklendi mi? 

Şu anda, Azure gizli bilgi işlem DCSv2 VM 'Leri Intel SGX DCAP 1,33 ile yüklenir. 

## <a name="can-i-open-an-azure-support-ticket-if-i-run-into-issues"></a>Sorun yaşıyorsanız bir Azure destek bileti açabilir miyim? 

Evet. Önizleme sırasında Azure desteği sağlanır. Ürün, önizleme aşamasında olduğundan SLA ekli değil.

## <a name="can-i-inject-post-install-scriptscustomize-drivers-to-the-nodes-provisioned-by-aks"></a>Yüklemeyi başlatma betikleri ekleyebilir/sürücüleri AKS tarafından sağlanan düğümlere özelleştirebilir miyim? 

Hayır. [Aks motoru tabanlı gizli bilgi işlem düğümleri](https://github.com/Azure/aks-engine/blob/master/docs/topics/sgx.md) , özel yüklemelere izin veren gizli bilgi işlem düğümlerini destekler.

## <a name="should-i-be-using-a-docker-base-image-to-get-started-on-enclave-applications"></a>Şifreleme uygulamalarını kullanmaya başlamak için bir Docker temel görüntüsü mi kullanmalıyım? 

Çeşitli etkinleştiriciler (ISV 'Ler ve OSS projeleri), gizli kapsayıcıları etkinleştirmek için yollar sağlar. Diğer ayrıntılar ve uygulamalara yönelik ayrı başvurular için [gizli kapsayıcılar sayfasını](confidential-containers.md) gözden geçirin.

## <a name="can-i-run-acc-nodes-with-other-standard-aks-skus-build-a-heterogenous-node-pool-cluster"></a>Diğer standart AKS SKU 'Ları ile hesap düğümleri çalıştırabilir miyim (bir heterojen düğüm havuzu kümesi derleyebilir)? 

Evet, hesap düğümleri dahil olmak üzere aynı AKS kümesi içinde farklı düğüm havuzlarını çalıştırabilirsiniz. Belirli bir düğüm havuzundaki şifreleme uygulamalarınızı hedeflemek için, düğüm seçicileri eklemeyi veya EPC sınırlarını uygulamayı düşünün. Gizli düğümlerdeki hızlı başlangıç hakkında daha fazla ayrıntı için [buradaki](confidential-nodes-aks-get-started.md)ayrıntıları inceleyin.

## <a name="can-i-run-windows-nodes-and-windows-containers-with-acc"></a>Windows düğümlerini ve Windows kapsayıcılarını ACC ile çalıştırabilir miyim? 

Şu anda değil. Windows düğümleri veya kapsayıcı gereksinimleriniz varsa bizimle iletişime geçin. 

## <a name="what-if-my-container-size-is-more-than-available-epc-memory"></a>Kapsayıcı boyutumun kullanılabilir EPC belleğinden daha fazla olması durumunda ne olur? 

EPC belleği, uygulamanızın kuşatma içinde yürütülmek üzere programlanan bölümü için geçerlidir. Kapsayıcının toplam boyutu, en fazla kullanılabilir EPC belleği ile karşılaştırmak için doğru bir yoldur. Aslında DCSv2 makineler,, uygulamanın güvenilmeyen bölümünün kullanabildiği en fazla 32 GB VM belleğine izin verir. Ancak, Kapsayıcınız kullanılabilir EPC belleğinden daha fazlasını kullanıyorsa, bu durumda programın kuşsında çalışan bölümünün performansı etkilenebilir.

Çalışan düğümlerinde EPC belleğini daha iyi yönetmek için, Kubernetes aracılığıyla EPC bellek tabanlı sınır yönetimini göz önünde bulundurun. Başvuru olarak aşağıdaki örneği izleyin

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: sgx-test
  labels:
    app: sgx-test
spec:
  template:
    metadata:
      labels:
        app: sgx-test
    spec:
      containers:
      - name: sgxtest
        image: oeciteam/sgx-test:1.0
        resources:
          limits:
            kubernetes.azure.com/sgx_epc_mem_in_MiB: 10 # This limit will automatically place the job into confidential computing node. Alternatively you can target deployment to nodepools
      restartPolicy: Never
  backoffLimit: 0
```

## <a name="what-happens-if-my-enclave-consumes-more-than-maximum-available-epc-memory"></a>Şifremin kullanılabilir maksimum EPC belleği tükettiği durumlarda ne olur? 

Toplam kullanılabilir EPC belleği, aynı VM 'lerdeki veya çalışan düğümlerdeki şifreleme uygulamaları arasında paylaşılır. Uygulamanız kullanılabilir olandan daha fazla EPC belleği kullanıyorsa, uygulama performansı etkilenebilir. Bu nedenle, Yukarıdaki örneklerde gösterildiği gibi, çalışan düğümleri başına kullanılabilir EPC belleğini daha iyi yönetmek için, dağıtım YAML dosyanızda uygulama başına toleranation ayarını yapmanızı öneririz. Alternatif olarak, çalışan düğümü havuzu VM boyutlarını yukarı veya daha fazla düğüm eklemek için her zaman seçim yapabilirsiniz. 

## <a name="why-cant-i-do-forks--and-exec-to-run-multiple-processes-in-my-enclave-application"></a>Neden çatallar () yapamıyorum, yoksa kuşatma uygulamamda birden çok işlemi çalıştırmak için exec mi? 

Şu anda Azure gizli bilgi işlem DCsv2 SKU 'ları, bir kuşın içinde yürütülen program için tek bir adres alanını destekler. Tek işlem, yüksek güvenlik etrafında tasarlanan geçerli bir kısıtlamadır. Ancak, gizli kapsayıcı etkinleştiricileri bu kısıtlamayı aşmak için alternatif uygulamalara sahip olabilir.

## <a name="do-you-automatically-install-any-additional-daemonsets-to-expose-the-sgx-drivers"></a>SGX sürücüleri kullanıma sunmak için ek daemonsets otomatik olarak yüklenmesini ister misiniz? 

Evet. Daemonset adı SGX-Device-Plugin ve SGX-quote-Helper ' dır. [Buradaki](confidential-nodes-aks-overview.md)ilgili amaçlar hakkında daha fazla bilgi edinin.  

## <a name="what-is-the-vm-sku-i-should-be-choosing-for-confidential-computing-nodes"></a>Gizli bilgi işlem düğümleri için seçim yapmam gereken VM SKU 'SU nedir? 

DCSv2 SKU 'Ları. [DCSv2 SKU 'ları](../virtual-machines/dcv2-series.md) [desteklenen bölgelerde](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines&regions=all)kullanılabilir.

## <a name="can-i-still-schedule-and-run-non-enclave-containers-on-confidential-computing-nodes"></a>Hala gizli bilgi işlem düğümlerinde şifreleme olmayan kapsayıcılar zamanlayabilir ve çalıştırabilir miyim? 

Evet. VM 'Lerde ayrıca standart kapsayıcı iş yüklerini çalıştırabilirler. Dağıtım modellerine karar vermeden önce uygulamalarınızın güvenlik ve tehdit modelini göz önünde bulundurun.

## <a name="can-i-provision-aks-with-dcsv2-node-pools-through-azure-portal"></a>Azure portal aracılığıyla AKS 'i DCSv2 node havuzlarıyla sağlayabilir miyim? 

Evet. Azure CLı, [burada](confidential-nodes-aks-get-started.md)belgelenen bir alternatif olarak da kullanılabilir.

## <a name="what-ubuntu-version-and-vm-generation-is-supported"></a>Hangi Ubuntu sürümü ve VM üretimi destekleniyor? 

Gen 2 üzerinde 18,04. 

## <a name="can-we-change-the-current-intel-sgx-dcap-diver-version-on-aks"></a>Aks 'deki geçerli Intel SGX Dcap sürücüsünü sürümünü değiştirebilir miyim? 

Hayır. Herhangi bir özel yükleme gerçekleştirmek için, [aks-Engine gizli bilgi Işlem çalışan düğümleri](https://github.com/Azure/aks-engine/blob/master/docs/topics/sgx.md) dağıtımlarını seçmenizi öneririz. 

## <a name="what-version-of-kubernetes-do-you-support-and-recommend"></a>Hangi Kubernetes sürümü destekliyoruz ve öneriyorsunuz? 

Kubernetes sürüm 1,16 ve üstünü destekliyoruz ve öneriyoruz 

## <a name="what-are-the-known-current-limitation-or-technical-limitations-of-the-product-in-preview"></a>Önizlemedeki ürünün bilinen geçerli sınırlaması veya teknik sınırlamaları nelerdir? 

- Yalnızca Ubuntu 18,04 Gen 2 VM düğümlerini destekler 
- Windows düğümü desteği veya Windows kapsayıcıları desteği yok
- EPC bellek tabanlı yatay Pod otomatik ölçeklendirme desteklenmez. CPU ve normal bellek tabanlı ölçekleme desteklenir.
- Gizli uygulamalar için AKS 'teki dev alanları şu anda desteklenmiyor

## <a name="next-steps"></a>Sonraki Adımlar
Gizli kapsayıcılar hakkında daha fazla ayrıntı için [gizli kapsayıcılar sayfasını](confidential-containers.md) gözden geçirin.