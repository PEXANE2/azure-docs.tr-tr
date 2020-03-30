---
title: Güvenliği artırmak için Azure Güvenlik Merkezi önerilerini kullanın | Microsoft Dokümanlar
description: " Bir güvenlik saldırısını azaltmaya yardımcı olmak için Azure Güvenlik Merkezi'nde güvenlik ilkelerini ve önerilerini nasıl kullanacağınızı öğrenin. "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2019
ms.author: memildin
ms.openlocfilehash: a1034eb47010da2b0e795ee8c79646f06151cac1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77603272"
---
# <a name="use-azure-security-center-recommendations-to-enhance-security"></a>Güvenliği artırmaya yönelik Azure Güvenlik Merkezi önerileri
Bir güvenlik ilkesi ni yapılandırıp Azure Güvenlik Merkezi tarafından sağlanan önerileri uygulayarak önemli bir güvenlik olayı olasılığını azaltabilirsiniz. Bu makalede, güvenlik saldırısını azaltmaya yardımcı olmak için Güvenlik Merkezi'nde güvenlik ilkeleri ve önerileri nasıl kullanacağınızı gösterir. 

Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu çözümlemek için sürekli taramaları otomatik olarak çalıştırAr. Güvenlik Merkezi olası güvenlik açıklarını tanımladığında, gerekli güvenlik denetimlerini yapılandırma sürecinde size yol gösteren öneriler oluşturur. Güvenlik Merkezi önerilerini aşağıdaki istisnalar dışında 24 saat içinde güncelleştirir:

- İşletim sistemi güvenlik yapılandırma önerileri 48 saat içinde güncelleştirilir
- Endpoint Protection sorunları önerileri 8 saat içinde güncelleştirilir

## <a name="scenario"></a>Senaryo
Bu senaryo, Güvenlik Merkezi önerilerini izleyerek ve harekete geçerek bir güvenlik olayı olasılığını azaltmaya yardımcı olmak için Güvenlik Merkezi'ni nasıl kullanacağınızı gösterir. Senaryo, kurgusal şirket Contoso'yu ve Güvenlik Merkezi planlama [ve operasyon kılavuzunda](security-center-planning-and-operations-guide.md#security-roles-and-access-controls)sunulan rolleri kullanır. Bu senaryoda, aşağıdaki kişiliklerin rollerine odaklanıyoruz:

![Senaryo rolleri](./media/security-center-using-recommendations/scenario-roles.png)

Contoso yakın zamanda şirket içi kaynaklarının bir kısmını Azure'a geçirdi. Contoso kaynaklarını korumak ve buluttaki kaynaklarının güvenlik açığını azaltmak istiyor.

## <a name="use-azure-security-center"></a>Azure Güvenlik Merkezi’ni kullanma
Contoso'nun BT güvenliğinden David, güvenlik açıklarını önlemek ve algılamak için Contoso'nun Azure Güvenlik Merkezi abonelikleri için Güvenlik Merkezi'ne binmeyi seçti bile. 

Güvenlik Merkezi, Contoso'nun Azure kaynaklarının güvenlik durumunu otomatik olarak analiz eder ve varsayılan güvenlik ilkeleri uygular. Güvenlik Merkezi olası güvenlik açıklarını tanımladığında, güvenlik ilkesinde ayarlanan denetimleri temel alan **öneriler** oluşturur. 

David, mevcut tüm öneriler ve güvenlik özellikleri paketini almak için tüm aboneliklerinde Azure Güvenlik standart katmanını çalıştırıyor. Jeff ayrıca, Güvenlik Merkezi'nin [Windows](quick-onboard-windows-computer.md) ve [Linux](quick-onboard-linux-computer.md) sunucularında hibrit desteğinden yararlanabilmeleri için henüz buluta geçirilmiş olmayan mevcut tüm şirket içi sunucularında da yer alır.

Jeff bir bulut iş yükü sahibidir. Jeff, Contoso'nun güvenlik politikalarına uygun olarak güvenlik denetimleri uygulamaktan sorumludur. 

Jeff aşağıdaki görevleri gerçekleştirir:

- Güvenlik Merkezi tarafından sağlanan güvenlik önerilerini izleme
- Güvenlik önerilerini değerlendirin ve önerileri uygulayıp uygulamadıklarına veya reddetmeleri gerektiğine karar verin.
- Güvenlik önerileri uygulama

### <a name="remediate-threats-using-recommendations"></a>Önerileri kullanarak tehditleri düzeltin
Günlük izleme faaliyetlerinin bir parçası olarak Jeff Azure'a gelir ve Güvenlik Merkezi'ni açar. 

1. Jeff iş yükünün aboneliklerini seçer.

2. Jeff, aboneliklerin ne kadar güvenli olduğuna dair genel bir resim elde etmek için **Güvenli Puanı** denetler ve skorun 548 olduğunu görür.

3. Jeff önce hangi önerileri ele alacaklarına karar vermeli. Jeff Güvenli Skor'u tıklatıyor ve [Güvenli Puan etkisini](security-center-secure-score.md)ne kadar artırdığını temel alan önerileri işlemeye başlıyor.

4. Jeff bağlı VM'ler ve sunucular çok olduğundan, Jeff **İşlem ve uygulamalar**odaklanmaya karar verir.

5. Jeff Bilgi **İşlem'i ve uygulamaları**tıklattığında, önerilerin bir listesini görür ve Bunları Güvenli Puan etkisine göre işler.

6. Jeff'in çok sayıda Internet'i vardır ve bağlantı noktaları açıkolduğundan, bir saldırganın sunucular üzerinde denetim eline alacağından endişe ederler. Bu yüzden Jeff [**tam zamanında VM erişimi**](security-center-just-in-time.md)kullanmayı seçer.

Jeff, yüksek öncelikli ve orta öncelikli öneriler de devam eder ve uygulama yla ilgili kararlar verir. Her öneri için Jeff, hangi kaynakların etkilendiğini, Güvenli Puan etkisinin ne olduğunu, her önerinin ne anlama geldiğini ve her sorunun nasıl azaltılabildiğini iyileştirme adımlarını anlamak için Güvenlik Merkezi tarafından sağlanan ayrıntılı bilgilere bakar.

## <a name="conclusion"></a>Sonuç
Güvenlik Merkezi'ndeki önerileri izleme, bir saldırı meydana gelmeden önce güvenlik açıklarını ortadan kaldırmanıza yardımcı olur. Önerileri düzeltdiğinizde, Secure Score'unuz ve iş yüklerinizin güvenlik duruşu gelişir. Güvenlik Merkezi, dağıttığınız yeni kaynakları otomatik olarak keşfeder, güvenlik politikanıza göre değerlendirir ve bunları güvence altına almak için yeni öneriler sağlar.


## <a name="next-steps"></a>Sonraki adımlar
Zaman içinde kaynaklarınızı güvende tutabilmeniz için Güvenlik Merkezi'ndeki önerileri düzenli olarak kontrol ettiğiniz bir izleme işleminiz olduğundan emin olun.

Bu senaryo, güvenlik saldırısını azaltmaya yardımcı olmak için Güvenlik Merkezi'ndeki güvenlik ilkelerini ve önerilerini nasıl kullanacağınızı göstermiştir.

[Güvenlik uyarılarını yönetme ve yanıtverme](security-center-managing-and-responding-alerts.md)ile tehditlere nasıl yanıt verilebildiğini öğrenin.
