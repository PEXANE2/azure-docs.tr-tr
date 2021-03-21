---
title: Azure Güvenlik Merkezi 'nin ana panosu veya ' genel bakış ' sayfası
description: Güvenlik Merkezi 'ne Genel Bakış sayfasının özellikleri hakkında bilgi edinin
author: memildin
ms.author: memildin
ms.date: 03/04/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 768d69b626a870910d6734e1a1ddc29871f96afb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102099786"
---
# <a name="azure-security-centers-overview-page"></a>Azure Güvenlik Merkezi 'ne genel bakış sayfası

Azure Güvenlik Merkezi 'ni açtığınızda, görüntülenecek ilk sayfa genel bakış sayfasıdır. 

Bu etkileşimli Pano, hibrit bulut iş yüklerinizin güvenlik duruşuna birleştirilmiş bir görünüm sağlar. Ayrıca, güvenlik uyarılarını, kapsam bilgilerini ve daha fazlasını gösterir.

Daha ayrıntılı bilgi edinmek için sayfadaki herhangi bir öğeyi seçebilirsiniz.

:::image type="content" source="media/overview-page/overview.png" alt-text="Güvenlik Merkezi’ne genel bakış":::

## <a name="features-of-the-overview-page"></a>Genel Bakış sayfasının özellikleri

:::image type="content" source="media/overview-page/top-bar-of-overview.png" alt-text="Güvenlik Merkezi 'nin Genel Bakış sayfasının üst çubuğu":::

**Üstteki menü çubuğu** şunları sunar:
- **Abonelikler** -bu düğmeyi seçerek Aboneliklerin listesini görüntüleyebilir ve filtreleyebilirsiniz. Güvenlik Merkezi, seçilen aboneliklerin güvenlik duruşunu yansıtacak şekilde ekranı ayarlar.
- **Yenilikler-yeni** özellikler, hata düzeltmeleri ve kullanım dışı işlevlerle güncel tutulabilmeniz için [sürüm notlarını](release-notes.md) açar.
- Aşağıdaki ana kutucuklarda bilgilerin bağlamını göstermek için bağlı bulut hesapları için **üst düzey numaralar** . Ayrıca, değerlendirilen kaynakların sayısı, etkin öneriler ve güvenlik uyarıları. [Varlık envanterine](asset-inventory.md)erişmek için değerlendirilen kaynakların numarasını seçin. [AWS hesaplarınızı](quickstart-onboard-aws.md) ve [GCP projelerinizi](quickstart-onboard-gcp.md)bağlama hakkında daha fazla bilgi edinin.


Sayfanın merkezinde, her biri özel bir panoya her biri daha ayrıntılı bilgi için bağlanan **dört merkezi kutucuk** vardır:
- **Güvenli puan** -Güvenlik Merkezi, güvenlik sorunları için kaynaklarınızı, aboneliklerinizi ve kuruluşunuzu sürekli olarak değerlendirir. Daha sonra her türlü bulguları tek bir puanın içinde toplar, böylece bir bakışta geçerli güvenlik durumunuza göre daha yüksek puan, belirtilen risk düzeyini azaltır. [Daha fazla bilgi edinin](secure-score-security-controls.md).
- **Mevzuat uyumluluğu** -Güvenlik Merkezi, Azure ortamınızın sürekli değerlendirmelerine bağlı olarak uyumlulukla ilgili öngörüler sağlar. Güvenlik Merkezi, karma bulut ortamınızdaki risk faktörlerini en iyi güvenlik uygulamalarına göre analiz eder. Bu değerlendirmeler, desteklenen bir standartlar kümesinden uyumluluk denetimleriyle eşlenir. [Daha fazla bilgi edinin](security-center-compliance-dashboard.md).
- **Azure Defender** -bu, Azure ve hibrit iş yüklerinizin gelişmiş ve akıllı koruması Için Güvenlik Merkezi kapsamında tümleşik olan bulut iş yükü koruma platformudur (cwpp). Kutucuk, bağlı kaynaklarınızın kapsamını (Şu anda seçili abonelikler için) ve en son uyarıları (önem derecesine göre renk kodlu) gösterir. [Daha fazla bilgi edinin](azure-defender.md).
- **Güvenlik duvarı Yöneticisi** -bu kutucuk, [Azure Güvenlik Duvarı Yöneticisi](../firewall-manager/overview.md)'ndeki hub 'larınızın ve ağların durumunu gösterir. 


**Öngörüler** bölmesi, ortamınız için aşağıdakiler de dahil olmak üzere özelleştirilmiş öğeler sunar:
- En çok saldırıya uğrayan kaynaklar
- Güvenli puanınızı artırmak için en yüksek olasılığından oluşan güvenlik denetimleriniz
- Etkilenen en fazla kaynakla etkin öneriler
- Azure Güvenlik Merkezi uzmanlarının son blog gönderileri

## <a name="next-steps"></a>Sonraki adımlar

Bu sayfa, güvenlik merkezi 'ne genel bakış sayfasını sunmuştur. İlgili bilgiler için bkz.:

- [Varlık envanteri ve yönetim araçlarıyla kaynaklarınızı bulun ve yönetin](asset-inventory.md)
- [Azure Güvenlik Merkezi'nde güvenlik puanı](secure-score-security-controls.md)