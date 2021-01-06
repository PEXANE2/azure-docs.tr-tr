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
ms.openlocfilehash: e3a665e3615c9ff3a68cf13eeaef5e8f41632f6a
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97900369"
---
# <a name="test-through-simulations"></a>Simülasyonlar aracılığıyla test etme

Düzenli benzetimler sunarak hizmetlerinizin bir saldırıya nasıl yanıt vereceğini gösteren varsayımları test etmek iyi bir uygulamadır. Sınama sırasında, hizmetlerinizin veya uygulamalarınızın beklenen şekilde çalışmaya devam etmesini ve kullanıcı deneyiminin kesintiye uğramaması gerektiğini doğrulayın. Hem teknoloji hem de süreç açısından boşlukları belirleyip DDoS yanıt stratejisinden dahil edin. Üretim ortamına etkisini en aza indirmek için, hazırlama ortamlarında veya yoğun olmayan saatlerde bu tür testleri gerçekleştirmenizi öneririz.

Azure müşterilerinin benzetimler için DDoS koruması etkinleştirilmiş ortak uç noktalara karşı trafik oluşturabileceği bir arabirim oluşturmak için, bir self servis trafik Oluşturucu olan [BreakingPoint bulutu](https://www.ixiacom.com/products/breakingpoint-cloud)ile ortaklıyoruz. Benzetimi şu şekilde kullanabilirsiniz:

- Azure DDoS korumasının Azure kaynaklarınızı DDoS saldırılarına karşı korumanıza nasıl yardımcı olduğunu doğrulayın.
- DDoS saldırısı altındayken olay yanıt sürecinizi iyileştirin.
- DDoS uyumluluğunu belgeleyin.
- Ağ Güvenlik ekiplerinizi eğitme.

## <a name="prerequisites"></a>Ön koşullar

- Bu öğreticideki adımları tamamlayabilmeniz için önce, korunan genel IP adreslerine sahip bir [Azure DDoS standart koruma planı](manage-ddos-protection.md) oluşturmanız gerekir.
- Önce [BreakingPoint bulutu](http://breakingpoint.cloud/)ile bir hesap oluşturmanız gerekir. 

## <a name="configure-a-ddos-test-attack"></a>DDoS test saldırısı yapılandırma

1. Aşağıdaki değerleri girin veya seçin ve ardından **Testi Başlat**' ı seçin:

    |Ayar        |Değer                                              |
    |---------      |---------                                          |
    |Hedef IP adresi           | Test etmek istediğiniz genel IP adresinden birini girin.                     |
    |Bağlantı Noktası Numarası   | _443_ girin.                       |
    |DDoS profili | Olası değerler şunlardır; **DNS sel**, **NTPv2 sel**, **SSDP sel**, **TCP SYN sel**, **UDP 64B sel**, **UDP 128b sel**, UDP **256B** sel, UDP **512B** sel, UDP **1024b sel**, UDP **1514b sel**, **UDP parçalanma** **UDP memönbellekli**.|
    |Test boyutu       | Olası değerler **100K PPS, 50 Mbps ve 4 kaynak IP**, **200k PPS, 100 Mbps ve 8 Kaynak IP**, **400K PPS, 200Mbps ve 16 kaynak** ıp, **800k PPS, 400 Mbps ve 32 Kaynak IP 'leri** içerir.                                  |
    |Test süresi | Olası değerler şunlardır **10 dakika**, **15 dakika**, **20 dakika**, **25 dakika**, **30 dakika**.|

Şu şekilde görünür:

![DDoS saldırı simülasyonu örneği: BreakingPoint bulutu](./media/ddos-attack-simulation/ddos-attack-simulation-example-1.png)

## <a name="monitor-and-validate"></a>İzleme ve doğrulama

1. ' Da oturum açın https://portal.azure.com ve aboneliğinize gidin.
1. Saldırıyı sınadığınız genel IP adresini seçin.
1. **İzleme** seçeneğinin altından **Ölçümler**’i seçin.
1. **Ölçüm** Için _DDoS saldırısı altında_ öğesini seçin.

Kaynak saldırıya ulaştıktan sonra, aşağıdaki resimde olduğu gibi değerin **0** ' dan **1**' e değiştiği görmeniz gerekir:

![DDoS saldırı simülasyonu örneği: Portal](./media/ddos-attack-simulation/ddos-attack-simulation-example-2.png)

### <a name="breakingpoint-cloud-api-script"></a>BreakingPoint bulutu API betiği

Bu [API betiği](https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20DDoS%20Protection/Breaking%20Point%20SDK) , bir kez çalıştırılarak veya sıradan testleri zamanlamak için cron kullanarak DDoS testini otomatikleştirmek için kullanılabilir. Bu, günlüğün düzgün yapılandırıldığını ve algılama ve yanıt yordamlarının etkin olduğunu doğrulamak için kullanışlıdır. Betikler bir Linux işletim sistemi (Ubuntu 18,04 LTS ile test edilmiştir) ve Python 3 gerektirir. Dahil edilen betiği kullanarak veya [BreakingPoint Cloud](http://breakingpoint.cloud/) Web sitesindeki belgeleri kullanarak ÖNKOŞULLARı ve API istemcisini yükler.

## <a name="next-steps"></a>Sonraki adımlar

- [DDoS koruması telemetrisini görüntülemeyi ve yapılandırmayı](telemetry.md)öğrenin.
- [DDoS tanılama günlüğünü görüntülemeyi ve yapılandırmayı](diagnostic-logging.md)öğrenin.
- [DDoS hızlı yanıt verme](ddos-rapid-response.md)hakkında bilgi edinin.
