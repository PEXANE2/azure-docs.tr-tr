---
title: Azure DDoS koruması simülasyonu testi
description: Benzetimler üzerinden test etme hakkında bilgi edinin
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2020
ms.author: yitoh
ms.openlocfilehash: eff738e24b3abce52e80291c55a3ae64c3c8c853
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92905672"
---
# <a name="test-through-simulations"></a>Benzetimler üzerinden test

Düzenli benzetimler sunarak hizmetlerinizin bir saldırıya nasıl yanıt vereceğini gösteren varsayımları test etmek iyi bir uygulamadır. Sınama sırasında, hizmetlerinizin veya uygulamalarınızın beklenen şekilde çalışmaya devam etmesini ve kullanıcı deneyiminin kesintiye uğramaması gerektiğini doğrulayın. Hem teknoloji hem de süreç açısından boşlukları belirleyip DDoS yanıt stratejisinden dahil edin. Üretim ortamına etkisini en aza indirmek için, hazırlama ortamlarında veya yoğun olmayan saatlerde bu tür testleri gerçekleştirmenizi öneririz.

Azure müşterilerinin benzetimler için DDoS koruması etkinleştirilmiş ortak uç noktalara karşı trafik oluşturabileceği bir arabirim oluşturmak için, bir self servis trafik Oluşturucu olan [BreakingPoint bulutu](https://www.ixiacom.com/products/breakingpoint-cloud)ile ortaklıyoruz. Benzetimi şu şekilde kullanabilirsiniz:

- Azure DDoS korumasının Azure kaynaklarınızı DDoS saldırılarına karşı korumanıza nasıl yardımcı olduğunu doğrulayın.
- DDoS saldırısı altındayken olay yanıt sürecinizi iyileştirin.
- DDoS uyumluluğunu belgeleyin.
- Ağ Güvenlik ekiplerinizi eğitme.

## <a name="prerequisites"></a>Önkoşullar

- Bu öğreticideki adımları tamamlayabilmeniz için önce, korunan genel IP adreslerine sahip bir [Azure DDoS standart koruma planı](manage-ddos-protection.md) oluşturmanız gerekir.
- Önce [BreakingPoint bulutu](http://breakingpoint.cloud/)ile bir hesap oluşturmanız gerekir. 

## <a name="configure-a-ddos-test-attack"></a>DDoS test saldırısı yapılandırma

1. Aşağıdaki değerleri girin veya seçin ve ardından **Testi Başlat** ' ı seçin:

    |Ayar        |Değer                                              |
    |---------      |---------                                          |
    |Hedef IP adresi           | Test etmek istediğiniz genel IP adresinden birini girin.                     |
    |Bağlantı Noktası Numarası   | _443_ girin.                       |
    |DDoS profili | **TCP SYN sel** öğesini seçin.|
    |Test boyutu       | **200K PPS, 100 Mbps ve 8 Kaynak IP 'leri seçin.**                                  |
    |Test süresi | **10 dakika** seçin.|

Şu şekilde görünür:

![DDoS saldırı simülasyonu örneği: BreakingPoint bulutu](./media/ddos-attack-simulation/ddos-attack-simulation-example-1.png)

## <a name="monitor-and-validate"></a>İzleme ve doğrulama

1. ' Da oturum açın https://portal.azure.com ve aboneliğinize gidin.
1. Saldırıyı sınadığınız genel IP adresini seçin.
1. **İzleme** seçeneğinin altından **Ölçümler** ’i seçin.
1. **Ölçüm** Için _DDoS saldırısı altında_ öğesini seçin.

Kaynak saldırıya ulaştıktan sonra, aşağıdaki resimde olduğu gibi değerin **0** ' dan **1** ' e değiştiği görmeniz gerekir:

![DDoS saldırı simülasyonu örneği: Portal](./media/ddos-attack-simulation/ddos-attack-simulation-example-2.png)

## <a name="next-steps"></a>Sonraki adımlar

- [DDoS koruması telemetrisini görüntülemeyi ve yapılandırmayı](telemetry-monitoring-alerting.md)öğrenin.
- [DDoS saldırı risk azaltma raporlarının ve akış günlüklerinin nasıl yapılandırılacağını](reports-and-flow-logs.md)öğrenin.
- [DDoS hızlı yanıt verme](ddos-rapid-response.md)hakkında bilgi edinin.