---
title: Azure DevTest Labs'ın uygulanmasını düzenleme
description: Bu makalede, kuruluşunuzdaki Azure DevTest Labs'ın uygulanmasını düzenlemek için kılavuz verilmektedir.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: fc6cbbd0aa9cb0750e497c7cc7edbd42f21bda55
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82025008"
---
# <a name="orchestrate-the-implementation-of-azure-devtest-labs"></a>Azure DevTest Labs'ın uygulanmasını düzenleme
Bu makale, Azure DevTest Labs'ın hızlı dağıtımı ve uygulanması için önerilen bir yaklaşım sağlar. Aşağıdaki resim, çeşitli endüstri gereksinimlerini ve senaryolarını desteklemek için esnekliği gözlemlerken genel süreci açıklayıcı bir kılavuz olarak vurgular.

![Azure DevTest Labs'ı uygulama adımları](./media/devtest-lab-guidance-orchestrate-implementation/implementation-steps.png)

## <a name="assumptions"></a>Varsayımlar
Bu makalede, bir DevTest Labs pilot uygulamadan önce yerinde aşağıdaki öğeleri varsayıyor:

- **Azure aboneliği**: Pilot ekip kaynakları bir Azure aboneliğine dağıtmaya erişebilir. İş yükleri yalnızca geliştirme ve sınama ysa, Windows sanal makinelerde ek kullanılabilir görüntüler ve daha düşük ücretler için Enterprise DevTest teklifini seçmeniz önerilir.
- **Şirket Içi Erişim**: Gerekirse şirket içi erişim zaten yapılandırılmıştır. Şirket içi erişim, Siteden siteye VPN bağlantısı veya Ekspres Rota üzerinden gerçekleştirilebilir. Ekspres Rota üzerinden bağlantı genellikle kurmak için haftalar sürebilir, bu projeye başlamadan önce yerinde Express Route olması tavsiye edilir.
- **Pilot Takımlar**: DevTest Labs kullanan ilk geliştirme proje ekibi(ler), ilgili geliştirme veya test faaliyetleriyle birlikte tanımlanmış ve bu takımlar için gereksinimler/hedefler/hedefler belirlemek.

## <a name="milestone-1-establish-initial-network-topology-and-design"></a>Milestone 1: Başlangıç ağ topolojisi ve tasarımını kurun
Azure DevTest Labs çözümünü dağıtırken ilk odak noktası, sanal makineler için planlanan bağlantıyı oluşturmaktır. Aşağıdaki adımlar gerekli yordamları ana hatlarını belirler:

1. Azure'daki DevTest Labs aboneliğine atanan **ilk IP adresi aralıklarını** tanımlayın. Bu adım, gelecekteki genişleme için yeterince büyük bir blok sağlayabilmeniz için VM sayısında beklenen kullanımı tahmin gerektirir.
2. DevTest Labs'a **istenen erişim yöntemlerini belirleyin** (örneğin, harici / dahili erişim). Bu adımda önemli bir nokta, sanal makinelerin genel IP adreslerine (diğer bir deyişle doğrudan internetten erişilebilen) sahip olup olmadığını belirlemektir.
3. Azure bulut ortamının geri kalanı ve şirket içi **bağlantı yöntemlerini belirleyin** ve kurun. Express Route ile zorunlu yönlendirme etkinleştirilmişse, sanal makinelerin şirket güvenlik duvarında geçiş yapmak için uygun proxy yapılandırmalarına ihtiyacı olabilir.
4. **VM'ler birleştirilmişse,** bulut tabanlı bir etki alanına (örneğin AAD Directory Services) mi yoksa şirket içi etki alanına mı katıldıklarını belirleyin. Şirket içinde, sanal makinelerin birleştiği etkin dizini içinde hangi kuruluş biriminin (OU) birleştirdiğini belirleyin. Buna ek olarak, kullanıcıların katılma (veya etki alanında makine kayıtları oluşturma yeteneğine sahip bir hizmet hesabı oluşturma) erişimi olduğunu onaylayın

## <a name="milestone-2-deploy-the-pilot-lab"></a>Milestone 2: Pilot laboratuarı dağıtın
Ağ topolojisi yerleştirildikten sonra, ilk/pilot laboratuvar aşağıdaki adımları atarak oluşturulabilir:

1. İlk DevTest Labs ortamı oluşturun.
2. Laboratuvarda kullanılmak üzere izin verilebilen VM görüntülerini ve boyutlarını belirleyin. DevTest Labs ile kullanılmak üzere Azure'a özel resimlerin yüklenip yüklenmeyeceğine karar verin.
3. Laboratuvar için ilk Role Base Access Controls (RBAC) oluşturarak laboratuvara erişimi güvenli hale getirmek (laboratuvar sahipleri ve laboratuvar kullanıcıları). DevTest Labs ile kimlik için Azure Active Directory ile senkronize etkin dizin hesapları kullanmanızı öneririz.
4. DevTest Laboratuvarlarını zamanlamalar, maliyet yönetimi, talep edilebilir VM'ler, özel görüntüler veya formüller gibi ilkeleri kullanacak şekilde yapılandırın.
5. Azure Repos/Git gibi bir çevrimiçi depo kurun.
6. Ortak veya özel depoların kullanımına veya her ikisinin birleşimine karar verin. Dağıtımlar ve uzun vadeli sürdürülebilirlik için JSON Şablonları düzenleyin.
7. Gerekirse, özel eserler oluşturun. Bu adım isteğe bağlıdır. 

## <a name="milestone-3-documentation-support-learn-and-improve"></a>Kilometre Taşı 3: Dokümantasyon, destek, öğrenme ve geliştirme
İlk pilot takımlar başlamak için derinlemesine desteğe ihtiyaç duyabilir. Azure DevTest Labs'ın sürekli kullanıma sunulması için doğru belgelerin ve desteğin yerinde olduğundan emin olmak için deneyimlerini kullanın.

1. Pilot ekipleri yeni DevTest Labs kaynaklarıyla tanıştırın (demolar, dokümantasyon)
2. Pilot ekiplerin deneyimlerine göre, gerektiğinde dokümantasyon planı ve teslim
3. Yeni ekipler için süreci resmileştirin (laboratuvar oluşturma ve yapılandırma, erişim sağlama vb.)
4. İlk alıma bağlı olarak, IP adresi alanının orijinal tahmininin hala makul ve doğru olduğunu doğrulayın
5. Uygun uyumluluk ve güvenlik incelemelerinin tamamlandığından emin olun

## <a name="next-steps"></a>Sonraki adımlar
Bu serinin bir sonraki makalesine bakın: [Azure DevTest Labs altyapısının yönetimi](devtest-lab-guidance-governance-resources.md)
