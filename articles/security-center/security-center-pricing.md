---
title: Azure Güvenlik Merkezi katmanlarının fiyatlandırması
description: Azure Güvenlik Merkezi, ücretsiz ve standart olmak üzere iki katmanda sunulur. Bu sayfada, ücretsiz 'ten standart sürümüne yükseltme gösterilmektedir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/28/2020
ms.author: memildin
ms.openlocfilehash: f4379a5149fff020eb532f0e17cc1c37fbdcd694
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82232436"
---
# <a name="upgrade-to-standard-tier-for-enhanced-security"></a>Gelişmiş güvenlik için Standart katmana yükseltme

Azure Güvenlik Merkezi, Azure’da, şirket içinde ve diğer bulutlarda çalışan iş yükleri için birleşik güvenlik yönetimi ve gelişmiş tehdit koruması sağlar. Karma bulut iş yükleri, tehditlere maruz kalma olasılığını azaltan etkin savunmaları ve hızlı gelişen siber saldırılarına karşı hızlanmanıza yardımcı olacak akıllı algılama üzerinde görünürlük ve denetim sağlar.

## <a name="pricing-tiers"></a>Fiyatlandırma katmanları
Güvenlik Merkezi iki katmanda sunulur:

- İlk kez Azure portal Azure Güvenlik Merkezi panosunu ziyaret ettiğinizde veya API aracılığıyla programlı olarak etkinleştirilmişse, **ücretsiz** katman tüm Azure aboneliklerinizde etkin olur. Ücretsiz katman, Azure kaynaklarınızı korumanıza yardımcı olmak için güvenlik ilkesi, sürekli güvenlik değerlendirmesi ve eyleme dönüştürülebilir güvenlik önerileri sağlar.

- **Standart** katman, hibrit bulut iş yüklerinizde birleştirilmiş güvenlik yönetimi ve tehdit koruması sağlayarak, ücretsiz katmanın yeteneklerini özel ve diğer genel bulutlarda çalışan iş yüklerine genişletir. Standart katman Ayrıca, saldırıları ve sıfır günlük güvenlik açıklarını belirlemek için yerleşik davranış analizi ve makine öğrenimi, ağ saldırılarına ve kötü amaçlı yazılımların etkilenme olasılığını azaltmak için erişim ve uygulama denetimlerini kullanan tehdit koruması özellikleri de ekler. Ayrıca Standart katman, sanal makineleriniz için güvenlik açığı taraması ekler. Standart katmanı ücretsiz olarak deneyebilirsiniz. Güvenlik Merkezi Standart; VM 'Ler, sanal makine ölçek kümeleri, App Service, SQL sunucuları ve depolama hesapları gibi Azure kaynaklarını destekler. Azure Güvenlik Merkezi Standart sürümüne sahipseniz, kaynak türüne göre destek ' i devre dışı bırakabilirsiniz. 

VM 'Ler için ücretsiz katman güvenlik değerlendirmelerinin çoğunun yanı sıra standart katman güvenlik uyarılarının çoğu, Log Analytics aracısının yüklenmesini gerektirir. Aracıyı Azure sanal makinelerinizde otomatik olarak dağıtmak için, güvenlik merkezi 'nde otomatik sağlamayı etkinleştirebilirsiniz.

## <a name="try-standard-tier-free-for-30-days"></a>Standart katmanı 30 gün boyunca ücretsiz deneyin
Standart katman ilk 30 gün boyunca ücretsizdir. 30 günün sonunda hizmeti kullanmaya devam etmeyi seçmeniz gerekir, kullanım için otomatik olarak ücretlendirilecektir.

Tüm Azure aboneliğini, abonelik içindeki tüm kaynaklar tarafından devralınan Standart katmana yükseltebilirsiniz.

Standart katmanı almak için:

1. **Güvenlik Merkezi** ana menüsünde **fiyatlandırma & ayarları** ' nı seçin.
2. Standart sürümüne yükseltmek istediğiniz aboneliği seçin.
3. **Fiyatlandırma katmanı**'nı seçin.
4. Yükseltmek için **Standart** ' ı seçin.
5. **Kaydet**’e tıklayın.

[![Güvenlik Merkezi fiyatlandırması](media/security-center-pricing/pricing-tier-page.png)](media/security-center-pricing/pricing-tier-page.png#lightbox)

> [!NOTE]
> Gelişmiş tehdit koruması özellikleri dahil tüm güvenlik merkezi özelliklerini etkinleştirmek için, uygun iş yüklerini içeren aboneliğe standart fiyatlandırma katmanını uygulamanız gerekir. Bir çalışma alanı için fiyatlandırmayı yapılandırmak, Azure kaynakları için tam zamanında VM erişimi, uyarlamalı uygulama denetimleri ve ağ algılamalarını etkinleştirmez. 
>
> Depolama hesapları ve PaaS SQL/MySQL/PG sunucuları için Güvenlik Merkezi 'nin tehdit koruması Şu anda tek tek depolama/SQL iş yüklerini korumak için kaynak düzeyinde etkinleştirilebilen tek iş yükleridir.

## <a name="why-upgrade-to-standard"></a>Neden standart olarak yükseltilir?
Güvenlik Merkezi, karma bulut iş yükleriniz için aşağıdakiler dahil gelişmiş güvenlik ve tehdit koruması sağlar:

- **Karma güvenlik** : tüm şirket içi ve bulut iş yüklerinizde güvenliğin birleştirilmiş bir görünümünü alın. Güvenlik standartları ile uyumluluğu sağlamak için güvenlik ilkelerini uygulayın ve hibrit bulut iş yüklerinizin güvenliğini sürekli değerlendirin. Güvenlik duvarları ve diğer iş ortağı çözümleri dahil olmak üzere birden çok kaynaktan güvenlik verilerini toplayın, arayın ve çözümleyin.
- **Güvenlik uyarıları** -gelişmiş analizler ve Microsoft Intelligent Security Graph gelişen Cyber saldırıları üzerinden bir kenar almak için kullanın. , Saldırıları ve sıfır günlük güvenlik açıklarını belirlemek için yerleşik davranış analizi ve makine öğrenimi özelliğinden yararlanın. Gelen saldırılar ve ihlal sonrası etkinlik için ağları, makineleri ve bulut hizmetlerini izleyin. Etkileşimli araçlar ve bağlamsal tehdit zekasıyla araştırmayı kolaylaştırın.
- **Sanal makineler Için güvenlik açığı taraması** -bir tarayıcıyı, güvenlik açığı yönetimi için sektörün en gelişmiş çözümünü sağlayan tüm sanal makinelerinize kolayca dağıtın. Bulguları doğrudan güvenlik merkezi içinde görüntüleyin, araştırın ve düzeltin. 
- **Erişim ve uygulama denetimleri** -belirli iş yüklerinize yönelik makine öğrenimi destekli beyaz listeleme önerilerini uygulayarak kötü amaçlı yazılımları ve diğer istenmeyen uygulamaları engelleyin. Azure VM 'lerinde yönetim bağlantı noktalarına tam zamanında, denetimli erişimle ağ saldırısı yüzeyini küçültün. Bu, deneme yanılma ve diğer ağ saldırılarına maruz kalma olasılığını önemli ölçüde azaltır.
- **Kapsayıcı güvenlik özellikleri** -Kapsayıcılı ortamlarınızda güvenlik açığı yönetimi ve gerçek zamanlı tehdit korumasından yararlanın. Kapsayıcı kayıt defterleri kaynağı etkinleştirilirken, tüm özellikler etkinleştirilene kadar bu işlem 12 saat kadar sürebilir.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi için fiyatlandırmaya sunulmuştur. Standart katmanın gelişmiş güvenlik ve Gelişmiş tehdit koruması hakkında daha fazla bilgi edinmek için bkz.:

- [Azure Güvenlik Merkezi’nde tehdit koruması](threat-protection.md)
- [Tam zamanında VM erişimi denetimi](security-center-just-in-time.md)
- [Kapsayıcı güvenliğine genel bakış](container-security.md)
- [Seçtiğiniz para birimi ve bölgenize göre fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/security-center/)