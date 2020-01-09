---
title: Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme | Microsoft Docs
description: Kullanıcılarınızın erişim etkinliğini ve kimlikle ilgili sorunları izleme amacıyla Azure Güvenlik Merkezi'ndeki tanımlama ve erişim özelliklerini nasıl kullanacağınızı öğrenin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2019
ms.author: memildin
ms.openlocfilehash: 6b262baddd10c9d0dff4b196b733972b97d99872
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552993"
---
# <a name="monitor-identity-and-access-preview"></a>Kimlik ve erişimi izleme (Önizleme)
Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, kaynaklarınızı korumak ve korumak için gerekli denetimleri yapılandırma sürecinde size kılavuzluk eden öneriler oluşturur.

Bu makalede, Azure Güvenlik Merkezi 'nin kaynak güvenliği bölümünün **kimlik ve erişim** sayfası açıklanmaktadır.

Bu sayfada görebileceğiniz önerilerin tam listesi için bkz. [kimlik ve erişim önerileri](recommendations-reference.md#recs-identity).

> [!NOTE]
> İzleme kimliği ve erişimi önizleme aşamasındadır ve yalnızca güvenlik merkezi 'nin standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
>

Kimlik, kuruluşunuz için denetim düzlemi olmalıdır ve kimlikleri korumak en iyi öncelikleriniz olmalıdır. Güvenlik çevresi, bir ağ çevre öğesinden bir kimlik çevre 'e geliştirilmiştir. Güvenlik, ağınızı erteleme ve verilerinizi erteleme ve uygulamalarınızın ve kullanıcılarınızın güvenliğini yönetme hakkında daha az hale gelir. Günümüzde buluta taşınan veri ve uygulama miktarı arttıkça kimlik yeni savunma hattı haline geliyor.

Kimlik etkinliklerini izleyerek bir olay gerçekleşmeden önce öngörülü eylemlerde veya bir saldırı girişimini durdurmak için reaktif eylemlerde bulunabilirsiniz. Kimlik & erişim panosu, şunları gibi öneriler sağlar:

- Aboneliğinizde ayrıcalıklı hesaplar için MFA'yı etkinleştirin
- Yazma izinleri olan dış hesapları aboneliğinizden kaldırın
- Ayrıcalıklı dış hesapları aboneliğinizden kaldırın

> [!NOTE]
> Aboneliğinizin 600 'den fazla hesabı varsa, Güvenlik Merkezi, aboneliğinize karşı kimlik önerilerini çalıştıramadı. Çalıştırılmayan öneriler aşağıda "kullanılamayan değerlendirmeler" altında listelenmiştir.
Güvenlik Merkezi, bulut çözümü sağlayıcısı (CSP) ortağının yönetim aracılarından dolayı kimlik önerilerini çalıştıramıyor.
>

## <a name="monitor-identity-and-access"></a>Kimlik ve erişimi izleme

Güvenlik Merkezi kenar çubuğundan ( **kaynaklar**altında) veya genel bakış sayfasından **kimlik & erişim** ' i seçerek tanımlı kimlik ve erişim sorunları listesini açın. 

**Kimlik & erişimi**altında iki sekme vardır:

- **Genel bakış**: Güvenlik Merkezi tarafından tanımlanan öneriler.
- **Abonelikler**: aboneliklerinizin ve geçerli güvenlik durumunun listesi.

[![kimlik & erişimi](./media/security-center-identity-access/identity-dashboard.png)](./media/security-center-identity-access/identity-dashboard.png#lightbox)

### <a name="overview-section"></a>Genel Bakış bölümü
**Genel bakış**' ın altında, önerilerin bir listesi bulunur. İlk sütunda öneriler listelenmiştir. İkinci sütunda, bu öneriden etkilenen toplam abonelik sayısı gösterilmektedir. Üçüncü sütun, sorunun önem derecesini gösterir.

1. Bir öneri seçin. Öneriler penceresi açılır ve şunları görüntüler:

   - Önerinin açıklaması
   - Sağlıksız ve sağlıklı aboneliklerin listesi
   - Başarısız bir değerlendirme nedeniyle taranmamış kaynakların listesi veya kaynak ücretsiz katmanda çalışan bir aboneliğin altında ve değerlendirilmedi

    [![öneriler penceresi](./media/security-center-identity-access/select-subscription.png)](./media/security-center-identity-access/select-subscription.png#lightbox)

1. Ek ayrıntı için listeden bir abonelik seçin.

### <a name="subscriptions-section"></a>Abonelikler bölümü
**Abonelikler**' in altında, aboneliklerin listesi bulunur. İlk sütunda abonelikler listelenir. İkinci sütun, her abonelik için toplam öneri sayısını gösterir. Üçüncü sütun sorunların önem derecelerine gösterir.

[![abonelikler sekmesi](./media/security-center-identity-access/subscriptions.png)](./media/security-center-identity-access/subscriptions.png#lightbox)

1. Bir abonelik seçin. Özet görünümü ile üç sekme açar:

   - **Öneriler**: Değerlendirme başarısız oldu Güvenlik Merkezi tarafından gerçekleştirilen göre.
   - **Değerlendirmeler geçirilen**: Güvenlik Merkezi tarafından geçirilen gerçekleştirdiği değerlendirmeler listesi.
   - **Kullanılamayan değerlendirmeler**: bir hata nedeniyle veya abonelikte 600 'den fazla hesap bulunduğundan başarısız olan değerlendirmelerin listesi.

   **Öneriler** altında, seçilen abonelik ve her bir önerinin önem derecesine ilişkin önerilerin bir listesidir.

   [![abonelik seçme önerileri](./media/security-center-identity-access/recommendations.png)](./media/security-center-identity-access/recommendations.png#lightbox)

1. Önerinin açıklaması, sağlıksız ve sağlıklı aboneliklerin listesi ve taranmamış kaynakların bir listesi için bir öneri seçin.

   [![önerinin açıklaması](./media/security-center-identity-access/designate.png)](./media/security-center-identity-access/designate.png#lightbox)

   Altında **değerlendirmeleri geçirilen** geçilen iç değerlendirmeler listesidir.  Bu değerlendirmeler önemini her zaman büyük/küçük harf yeşildir.

   [![değerlendirmelere geçti](./media/security-center-identity-access/passed-assessments.png)](./media/security-center-identity-access/passed-assessments.png#lightbox)

1. Değerlendirmenin açıklaması ve sağlıklı aboneliklerin listesi için listeden bir geçmiş değerlendirmesi seçin. Sağlıksız abonelikler için başarısız olan tüm abonelikleri listeleyen bir sekme bulunur.

   [![değerlendirmelere geçti](./media/security-center-identity-access/remove.png)](./media/security-center-identity-access/remove.png#lightbox)

> [!NOTE]
> MFA 'yı gerektiren ancak dışlamaları olan bir koşullu erişim ilkesi oluşturduysanız, bazı kullanıcıların MFA olmadan Azure 'da oturum açmasını sağladığından Güvenlik Merkezi MFA önerisi değerlendirmesi ilkeyi uyumlu değil olarak değerlendirir.

## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türlerine uygulanan öneriler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
- [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
- [Azure Güvenlik Merkezi 'nde Azure SQL hizmetinizi ve verilerinizi koruma](security-center-sql-service-recommendations.md)