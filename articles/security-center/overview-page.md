---
title: Azure Güvenlik Merkezi 'nin ana panosu veya ' genel bakış ' sayfası
description: Güvenlik Merkezi 'ne Genel Bakış sayfasının özellikleri hakkında bilgi edinin
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: cc52610eacc3916b7a8978cba17a1db3f3d50686
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91447677"
---
# <a name="azure-security-centers-overview-page"></a>Azure Güvenlik Merkezi 'ne genel bakış sayfası

Azure Güvenlik Merkezi 'ni açtığınızda, görüntülenecek ilk sayfa genel bakış sayfasıdır. 

:::image type="content" source="media/overview-page/overview.png" alt-text="Güvenlik Merkezi’ne genel bakış":::

Güvenlik Merkezi 'ne genel bakış sayfası ile iş yüklerinizin güvenliğini bulup değerlendirin ve riskleri belirleyip azaltabilirsiniz.

Genel bakış, hibrit bulut iş yüklerinizin güvenlik duruşuna birleştirilmiş bir görünüm sağlar. Ayrıca, güvenlik uyarılarını, kapsam bilgilerini ve daha fazlasını gösterir.


## <a name="features-of-the-overview-page"></a>Genel Bakış sayfasının özellikleri

:::image type="content" source="media/overview-page/top-bar-of-overview.png" alt-text="Güvenlik Merkezi’ne genel bakış":::

**Üstteki menü çubuğu** şunları sunar:
- **Abonelikler** -bu düğmeyi seçerek Aboneliklerin listesini görüntüleyebilir ve filtreleyebilirsiniz. Güvenlik Merkezi, seçilen aboneliklerin güvenlik duruşunu yansıtacak şekilde ekranı ayarlar.
- **Yenilikler-yeni** özellikler, hata düzeltmeleri ve kullanım dışı işlevlerle güncel tutulabilmeniz için [sürüm notlarını](release-notes.md) açar.
- Aşağıdaki ana kutucuklarda bilgilerin bağlamını göstermek için bağlı bulut hesapları için **üst düzey numaralar** . Ayrıca etkin öneri ve uyarı sayısını.
    [AWS hesaplarınızı](quickstart-onboard-aws.md) ve [GCP projelerinizi](quickstart-onboard-gcp.md)bağlama hakkında daha fazla bilgi edinin.


Sayfanın merkezinde, her biri özel bir panoya her biri daha ayrıntılı bilgi için bağlanan **dört merkezi kutucuk**vardır:
- **Güvenli puan** -Güvenlik Merkezi, güvenlik sorunları için kaynaklarınızı, aboneliklerinizi ve kuruluşunuzu sürekli olarak değerlendirir. Daha sonra her türlü bulguları tek bir puanın içinde toplar, böylece bir bakışta geçerli güvenlik durumunuza göre daha yüksek puan, belirtilen risk düzeyini azaltır. [Daha fazla bilgi edinin](secure-score-security-controls.md).
- **Uyumluluk** -Güvenlik Merkezi, Azure ortamınızın sürekli değerlendirmelerine bağlı olarak uyumluluk gönderinize yönelik öngörüler sağlar. Güvenlik Merkezi, karma bulut ortamınızdaki risk faktörlerini en iyi güvenlik uygulamalarına göre analiz eder. Bu değerlendirmeler, desteklenen bir standartlar kümesinden uyumluluk denetimleriyle eşlenir. [Daha fazla bilgi edinin](security-center-compliance-dashboard.md).
- **Azure Defender** -bu bulut iş yükü koruma platformudur (cwpp), Azure ve hibrit iş yüklerinizin gelişmiş, akıllı ve koruma Için Güvenlik Merkezi kapsamında tümleşiktir. Kutucuk, bağlı kaynaklarınızın kapsamını (Şu anda seçili abonelikler için) ve en son uyarıları (önem derecesine göre renk kodlu) gösterir. [Daha fazla bilgi edinin](azure-defender.md).
- **Envanter** -kutucuk, izlenmeyen VM 'lerin sayısını ve Güvenlik Merkezi ben tarafından izlenen kaynaklarınızın basit bir barbir örneğini gösterir. [Daha fazla bilgi edinin](asset-inventory.md).


**Öngörüler** bölmesi, ortamınız için aşağıdakiler de dahil olmak üzere özelleştirilmiş öğeler sunar:
- En çok saldırıya uğrayan kaynaklar
- Güvenli puanınızı artırmak için en yüksek olasılığından oluşan güvenlik denetimleriniz
- Etkilenen en fazla kaynakla etkin öneriler
- Azure Güvenlik Merkezi uzmanlarının son blog gönderileri

## <a name="next-steps"></a>Sonraki adımlar

Bu sayfa, güvenlik merkezi 'ne genel bakış sayfasını sunmuştur. İlgili bilgiler için bkz.:

- [Varlık envanteri ve yönetim araçlarıyla kaynaklarınızı bulun ve yönetin](asset-inventory.md)
- [Azure Güvenlik Merkezi 'nde güvenli puan](secure-score-security-controls.md)