---
title: Azure Avustralya nedir? | Microsoft Docs
description: Avustralya Kamu politika, yönetmelikler ve yasalların belirli ihtiyaçlarını karşılamak üzere Avustralya bölgeleri içinde Azure 'u yapılandırma kılavuzu.
author: Galey801
ms.service: azure-australia
ms.topic: overview
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: a6bd4a8747ba4aa592914fc970acd756803dd15e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68570883"
---
# <a name="what-is-azure-australia"></a>Azure Avustralya nedir?

2014 ' de Azure, iki bölge ile Avustralya 'da başlatılmıştı; Avustralya Doğu (Sidney) ve Avustralya Güneydoğu (Melbourne). 2018 Nisan 'da, Canbera – Avustralya Orta ve Avustralya Orta 2 içinde bulunan iki yeni Azure bölgesi başlatıldı. Avustralya Orta ve Avustralya Orta 2 bölgeler, özellikle kamu ve kritik Ulusal altyapının ihtiyaçlarını karşılamak üzere tasarlanmıştır ve bu sayede, sistem ve bulut yanına Güvenlik ve esnekliği yalnızca gizli sınıflandırılan ağların bekleniyor. Azure Avustralya, kamu ve kritik Ulusal altyapıların dijital dönüştürülmesine yönelik bir platformdur ve bu gereksinimler için özel olarak tasarlanan Avustralya 'da sunulan tek iş açısından kritik bulut.

Avustralya kamu verileri ve sistemleri için [Microsoft Azure Avustralya 'nın](https://azure.microsoft.com/global-infrastructure/australia/) içinde bağlantı kurma, kullanma ve işletme Için belirli Avustralya kamu gereksinimleri vardır. Bu sayfadaki kaynaklar ayrıca güvenli yapılandırma ve işleme özgü bir odağa sahip tüm müşteriler için geçerli olan genel yönergeler sağlar.

Azure Avustralya Information Security kayıtlı denetçisi (ıRAP) değerlendirmeleri, sertifika verme ve sertifikalı Cloud Services listesine ekleme (CCSL) hakkındaki güncel bilgiler için [Microsoft hizmet güveni portalının](https://aka.ms/au-irap) Avustralya sayfasına bakın. Avustralya sayfasında, kamu ve kritik altyapı sağlayıcılarına özgü diğer Microsoft önerileri de bulacaksınız.

## <a name="principles-for-securing-customer-data-in-azure-australia"></a>Azure Avustralya 'da müşteri verilerini güvenli hale getirme ilkeleri

Azure Avustralya, düzenlenmiş/denetimli veri gereksinimlerinizi karşılayacak bulut çözümleri oluşturmak için kullanabileceğiniz bir dizi özellik ve hizmeti sağlar. Uyumlu bir müşteri çözümü, kullanıma hazır Azure Avustralya özelliklerini katı bir veri güvenliği uygulamasıyla bağlanmış olan etkili bir şekilde hiçbir şey değildir.

Azure Avustralya 'da bir çözüm barındırdığınızda, Microsoft bu gereksinimlerin çoğunu bulut altyapısı düzeyinde işler.

Aşağıdaki diyagramda Azure savunma modelinin derinlemesine bir modeli gösterilmektedir. Örneğin, Microsoft temel bulut altyapısı için güvenlik gereçleri veya müşterilere özgü uygulama gereksinimlerine yönelik Premium DDoS hizmetleri gibi müşteri özellikleri sağlar.

![alternatif metin](media/defenceindepth.png)

Bu makalelerde, hizmet ve uygulamalarınızın güvenliğini sağlamaya yönelik temel ilkeler ana hatlarıyla anlatılmaktadır. bu ilkelerin nasıl uygulanacağı hakkında rehberlik ve en iyi uygulamalar vardır. Diğer bir deyişle, müşterilerin kamu duyarlı ve sınıflandırılan bilgileri işleyen bir çözüm için gerekli olan yükümlülüklerin ve sorumlulukların karşılanması için Azure Avustralya 'nın nasıl akıllı bir şekilde kullanılmasını sağlama.

Azure 'a geçiş yapan Avustralya kamu kurumları için iki belge kategorisi vardır.

## <a name="security-in-azure-australia"></a>Azure Avustralya 'da güvenlik

Kimlik, rol tabanlı erişim denetimi, şifreleme ve hak yönetimi ile veri koruması, etkili izleme ve yapılandırma denetimi, uygulamanız gereken önemli öğelerdir. Bu bölümde, Azure 'un yerleşik yeteneklerini ve bunların ıSM ve ASD 'nin önemli 8 ile ilişkisini açıklayan bir dizi makale vardır.

Bu makalelere *Azure Avustralya 'Da kavramlar-> güvenliği*altındaki menü aracılığıyla erişilebilir.

## <a name="gateways-in-azure-australia"></a>Azure Avustralya 'daki ağ geçitleri

Kamu kurumları için başka bir temel adım, çevre güvenlik özellikleri 'nin kurulması. Bu yetenekler, güvenli Internet ağ geçitleri (SıG) olarak adlandırılır ve Azure kullanılırken bu korumaların yerinde olduğundan emin olmanız sizin sorumluluğunuzdadır. Microsoft bir SıG çalışmaz; Bununla birlikte, tüm müşterileri koruyan Edge Ağ hizmetlerinizi ve Azure ortamınızda dağıtılan belirli hizmetleri birleştirerek eşdeğer bir özelliği çalıştırabilirsiniz.

Bu makalelere *Azure Avustralya 'Daki kavramlar-> ağ geçitleri*altında bulunan menü aracılığıyla erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Anahtar odağınıza Azure 'daki verilerinizin güvenliği varsa, [veri güvenliği](secure-your-data.md) ile başlayın
* Anahtar odağınıza Azure 'da ağ geçidi kullanılıyorsa, [ağ geçidi denetimi, günlüğe kaydetme ve görünürlük](gateway-log-audit-visibility.md)ile başlayın.
