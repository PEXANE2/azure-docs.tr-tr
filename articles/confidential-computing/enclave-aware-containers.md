---
title: Azure 'da şifreleme ve duyarlı kapsayıcılar
description: Azure Kubernetes Service (AKS) üzerinde şifreleme için hazır uygulama kapsayıcıları desteği
author: agowdamsft
ms.service: container-service
ms.topic: overview
ms.date: 9/22/2020
ms.author: amgowda
ms.openlocfilehash: b388b8efb7d993baa243975dbf499576246f3185
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91001071"
---
# <a name="enclave-aware-containers"></a>Şifreleme kullanan kapsayıcılar

Bir şifreleme, veri ve kod yürütme için Gizlilik sağlayan korumalı bir bellek bölgesidir. Bu, donanım tarafından güvenliği sağlanmış bir güvenilir yürütme ortamı (t) örneğidir. AKS 'teki gizli bilgi işlem düğümleri, her kapsayıcı uygulaması arasındaki düğümlerde yalıtılmış şifreleme ortamları oluşturmak için [Intel Software Guard uzantıları (SGX)](https://software.intel.com/sgx) kullanır.

Intel SGX sanal makineler gibi, kuşlarda çalışmak üzere geliştirilen kapsayıcı uygulamalarının iki bileşeni vardır:

- Güvenilmeyen bir bileşen (konak olarak adlandırılır) ve
- Güvenilir bir bileşen (kuşatma olarak adlandırılır).

![Şifreleme kullanan kapsayıcı mimarisi](./media/enclave-aware-containers/enclaveawarecontainer.png)

Kuşve duyarlı kapsayıcılar uygulama mimarisi, kod parmak izini düşük bir şekilde tutarken uygulama üzerinde en çok denetimi sağlar. Kuşatma içinde çalışan kodu en aza indirmek, saldırı yüzeyi alanlarının azaltılmasına yardımcı olur.   

## <a name="enablers"></a>Etkinleştiricilerden biridir

### <a name="open-enclave-sdk"></a>Enclave SDK’sını açma
Açık Enclave SDK, donanım tabanlı güvenilen yürütme ortamlarından yararlanarak C, C++ uygulamaları geliştirmeye yönelik, donanımdan bağımsız bir açık kaynak kitaplığıdır. Geçerli uygulama, [ARM TrustZone üzerindeki op-t işletim sistemi](https://optee.readthedocs.io/en/latest/general/about.html)için Önizleme desteğinin yanı sıra Intel SGX için de destek sağlar.

Açık şifreleme tabanlı kapsayıcı uygulamasını [buradan](https://github.com/openenclave/openenclave/tree/master/docs/GettingStartedDocs) kullanmaya başlayın

### <a name="intel-sgx-sdk"></a>Intel SGX SDK
Intel, hem Linux hem de Windows kapsayıcı iş yükleri için SGX uygulamalar oluşturmaya yönelik yazılım geliştirme setini korur. Windows kapsayıcıları Şu anda AKS gizli bilgi işlem düğümleri tarafından desteklenmiyor.

[Buradan](https://software.intel.com/content/www/us/en/develop/topics/software-guard-extensions/sdk.html) Intel SGX tabanlı uygulamalar ile çalışmaya başlayın

### <a name="confidential-consortium-framework-ccf"></a>Gizli konsorsiyum çerçevesi (CCF)
Gizli ve çok taraflı işlem ve verilere odaklanarak güvenli, yüksek oranda kullanılabilir ve performanslı uygulamaların yeni bir kategorisini oluşturmaya yönelik bir açık kaynak çerçevesidir. CCF, kilit tabanlı blok zinciri ve çok taraflı işlem teknolojisinin üretim ve kurumsal benimseme sürecini hızlandırmaya yönelik bir yol sunarak, önemli kurumsal gereksinimleri karşılayan yüksek ölçekli gizli ağları etkinleştirebilir.

Azure gizli bilgi işlem ve CCF 'yi [buradan](https://github.com/Microsoft/CCF) kullanmaya başlayın

### <a name="confidential-inferencing-onnx-runtime"></a>Gizli Çıkarım ONNX Çalışma Zamanı

Açık kaynak şifreleme tabanlı ONNX çalışma zamanı, istemci ve çıkarım hizmeti arasında güvenli bir kanal oluşturur. bunun ne isteğin ne de yanıtın güvenli şifreleme ile ayrılamayacağı doğrulanıyor. 

Bu çözüm, istemci ile sunucu arasında kanıtlama ve doğrulamalar aracılığıyla güven sağlarken, mevcut ML eğitilen modeli getirip confidentially çalıştırmanızı sağlar. 

ML modeli kaldırma ile çalışmaya başlayın ve [burada](https://aka.ms/confidentialinference) onnx çalışma zamanına geçin

## <a name="container-samples-implementations"></a>Kapsayıcı örnekleri uygulamaları

[AKS üzerinde şifreleme kullanan kapsayıcılar için Azure örnekleri](https://github.com/Azure-Samples/enclave-aware-container-samples)

<!-- LINKS - external -->
[Azure Attestation]: https://docs.microsoft.com/en-us/azure/attestation/


<!-- LINKS - internal -->
[DC Virtual Machine]: /confidential-computing/virtual-machine-solutions
[Confidential Containers]: /confidential-computing/containercompute/confidential-containers