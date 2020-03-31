---
title: Azure Güvenlik Merkezi nedir?| Microsoft Docs
description: Bu sayfa, Güvenlik Merkezi'nin güvenlik durumunuzu keşfetme ve bulut ve ön hazırlık kaynaklarının kapsamıyla iyileştirilmesi gibi önemli avantajlarını açıklar.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2019
ms.author: memildin
ms.openlocfilehash: 84ed5db54874231d8bde4d892eedbc60c42a3430
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77918766"
---
# <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?

Azure Güvenlik Merkezi, veri merkezlerinizin güvenlik duruşunu güçlendiren ve buluttaki karma iş yüklerinizde (Azure'da olsun veya olmasın) gelişmiş tehdit koruması sağlayan ve aynı zamanda Azure'da olsun veya olmasın, birleşik bir altyapı güvenlik yönetim sistemidir tesislerinde.

Kaynaklarınızı güvende tutmak, bulut sağlayıcınız Azure ile müşteri niz arasındaki ortak bir çabadır. Buluta geçerken iş yüklerinizin güvenli olduğundan emin olsanız ve aynı zamanda IaaS'e (hizmet olarak altyapı) taşındığınızda PaaS (hizmet olarak platform) ve SaaS 'de (hizmet olarak yazılım) olduğundan daha fazla müşteri sorumluluğu vardır. Azure Güvenlik Merkezi, ağınızı sertleştirmek, hizmetlerinizi güvence altına almak ve güvenlik duruşunuzu en iyi şekilde gördüğünüzden emin olmak için gereken araçları sağlar.

Azure Güvenlik Merkezi en acil üç güvenlik sorununu giderir:

-   **Hızla değişen iş yükleri** - Bu, bulutun hem güçlü hem de sorunlu bir yönüdür. Bir yandan, son kullanıcıların daha fazlasını yapma yetkisi vardır. Diğer yandan, insanların kullandığı ve oluşturduğu sürekli değişen hizmetlerin güvenlik standartlarınıza uygun olduğundan ve en iyi güvenlik deneyimlerine uyduğundan nasıl emin olabilirsiniz?

-   **Giderek daha karmaşık olan saldırılar** - İş yüklerinizi çalıştırdığınız her yerde saldırılar daha karmaşık hale gelmektedir. Aslında İnternete yönelik bir iş yükü olan ve en iyi güvenlik deneyimlerini takip etmemeniz halinde sizi daha da savunmasız bırakabilecek genel bulut iş yüklerinizi güvenli hale getirmeniz zorunludur.

-   **Güvenlik becerileri az bulunur** - Ortamlarınızın korunduğundan emin olmak için, güvenlik uyarıları ile uyarı sistemleri sayısının, gerekli arka plan ve deneyime sahip yönetici sayısından çok daha fazla olması gerekir. En son saldırılardan haberdar olmak her zaman yaşanan bir sorundur ve güvenlik dünyası sürekli değişen bir cephe olduğu sürece yerinde kalmayı imkansız hale getirir.

Bu güçlüklere karşı kendinizi korumanıza yardımcı olmak için, Güvenlik Merkezi aşağıdakileri sağlayan araçlar sunar:

-   **Güvenlik duruşunu güçlendirin**: Güvenlik Merkezi çevrenizi değerlendirir ve kaynaklarınızın durumunu ve güvenli olup olmadıklarını anlamanızı sağlar.

-   **Tehditlere karşı koruyun**: Güvenlik Merkezi iş yüklerinizi değerlendirir ve tehdit önleme önerileri ve güvenlik uyarıları sunar.

-   **Daha hızlı güvenli olun**: Güvenlik Merkezi'nde her şey bulut hızında yapılır. Yerel olarak entegre olduğundan, Güvenlik Merkezi'nin dağıtımı kolaydır ve Azure hizmetleriyle otomatik sağlama ve koruma sağlar.

[!INCLUDE [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)]

## <a name="architecture"></a>Mimari

Güvenlik Merkezi azure'un doğal olarak bir parçası olduğundan, Hizmet Kumaşı, SQL veritabanları ve depolama hesapları da dahil olmak üzere Azure'daki PaaS hizmetleri, herhangi bir dağıtım gerektirmeden Güvenlik Merkezi tarafından izlenir ve korunur.

Buna ek olarak, Security Center, Microsoft İzleme Aracısını yükleyerek bulutta veya şirket içinde, hem Windows hem de Linux sunucuları için Azure olmayan sunucuları ve sanal makineleri korur. Azure sanal makineleri Güvenlik Merkezi'nde otomatik olarak sağlanmaktadır.

Aracılardan ve Azure'dan toplanan olaylar, iş yüklerinizin güvenli olduğundan emin olmak için izlemeniz gereken özel öneriler (sertleştirme görevleri) sağlamak için güvenlik analitiği altyapısında ilişkilendirilir. İş yüklerinizde kötü amaçlı saldırıların gerçekleşmediğinden emin olmak için bu tür uyarıları mümkün olan en kısa sürede araştırmanız gerekir.

Güvenlik Merkezi'ni etkinleştirdiğinizde, Güvenlik Merkezi'ne yerleşik güvenlik ilkesi, Güvenlik Merkezi kategorisi altında yerleşik bir girişim olarak Azure İlkesi'ne yansıtılır. Yerleşik girişim otomatik olarak tüm Güvenlik Merkezi kayıtlı aboneliklere (Ücretsiz veya Standart katmanlar) atanır. Yerleşik girişim yalnızca Denetim ilkeleri içerir. Azure İlkesi'ndeki Güvenlik Merkezi ilkeleri hakkında daha fazla bilgi [için](tutorial-security-policy.md)bkz.

## <a name="strengthen-security-posture"></a>Güvenlik duruşunu güçlendirme

Azure Güvenlik Merkezi, güvenlik duruşunuzu güçlendirmenizi sağlar. Bu, güvenlik en iyi uygulamaları olarak önerilen sertleştirme görevlerini belirlemenize ve gerçekleştirmenize ve bunları makinelerinizde, veri hizmetlerinde ve uygulamalarınız arasında uygulamanıza yardımcı olur. Buna güvenlik ilkelerinizi yönetme ve uygulama ve Azure sanal makinelerinizin, Azure olmayan sunucularınızın ve Azure PaaS hizmetlerinizin uyumlu olduğundan emin olmak da dahildir. Güvenlik Merkezi, ağ güvenliği arazinizde odaklanmış görünürlükle iş yüklerinizde kuş bakışı görünüme sahip olmanız için ihtiyacınız olan araçları sağlar. 

### <a name="manage-organization-security-policy-and-compliance"></a>Kuruluş güvenlik ilkesini ve uyumluluğunu yönetme

İş yüklerinizin güvenli olduğundan emin olmak ve bunu yapmak temel bir güvenlik temelidir ve bu, özel güvenlik ilkelerinin yerinde olmasıyla başlar. Güvenlik Merkezi'ndeki tüm ilkeler Azure ilke denetimlerinin üzerine inşa edildiklerinden, birinci sınıf bir **ilke çözümünün**tüm yelpazesini ve esnekliğini elde emzebilirsiniz. Güvenlik Merkezi'nde, ilkelerinizi yönetim gruplarında, abonelikler arasında ve hatta tüm bir kiracı için çalışacak şekilde ayarlayabilirsiniz.

![Güvenlik Merkezi panosu](media/security-center-intro/sc-dashboard.png)

Güvenlik **Merkezi, Gölge BT aboneliklerini belirlemenize**yardımcı olur. Panonuzda **kapsanmayan** etiketli aboneliklere bakarak, yeni oluşturulan aboneliklerin ne zaman olduğunu hemen anlayabilir ve bunların ilkeleriniz tarafından karşılandıklarından ve Azure Güvenlik Merkezi tarafından korunduğundan emin olabilirsiniz.

![Güvenlik Merkezi ilke panosu](media/security-center-intro/sc-policy-dashboard.png)

Güvenlik Merkezi'ndeki gelişmiş izleme yetenekleri, **zaman içinde uyumluluk ve yönetişimi izlemenize ve yönetmenize**de olanak sağlar. **Genel uyumluluk,** aboneliklerinizin iş yükünüzle ilişkili ilkelerle ne kadar uyumlu olduğunu ölçer. 

![Zaman içinde Güvenlik Merkezi ilkesi](media/security-center-intro/sc-policy-time.png)

### <a name="continuous-assessments"></a>Sürekli değerlendirmeler

Güvenlik Merkezi sürekli olarak iş yükleri arasında dağıtılan yeni kaynakları keşfeder ve bunların güvenlik en iyi uygulamalarına göre yapılandırılıp yapılandırılmadığını değerlendirir, değilse işaretlenir ve ne için öncelikli bir öneri listesi alırsınız makinelerinizi korumak için düzeltmeniz gerekir.

Güvenlik Merkezi'nin ağınızın güvenlik durumunu sürekli olarak izlemek için sağladığı en güçlü araçlardan biri **Ağ haritasıdır.** Harita, iş yüklerinizin topolojisini görmenizi sağlar, böylece her düğümün düzgün şekilde yapılandırılıp yapılmadı sını görebilirsiniz. Düğümlerinizin nasıl bağladığını görebilirsiniz, bu da saldırganın ağınızda sürünmesini kolaylaştırabilecek istenmeyen bağlantıları engellemenize yardımcı olur.

![Güvenlik Merkezi ağ haritası](media/security-center-intro/sc-net-map.png)

Güvenlik Merkezi, **Güvenli Puan**ekleyerek güvenlik uyarılarınızı bir adım daha kolay azaltmayı sağlar. Güvenli Puanlar artık, her bir önerinin genel güvenlik duruşunuz için ne kadar önemli olduğunu anlamanıza yardımcı olmak için aldığınız her öneriyle ilişkilendirilir. Bu, **güvenlik çalışmalarınıza öncelik vermenizi**sağlamada çok önemlidir.

![Güvenlik Merkezi Güvenli Puan](media/security-center-intro/sc-secure-score.png)

### <a name="optimize-and-improve-security-by-configuring-recommended-controls"></a>Önerilen denetimleri yapılandırarak güvenliği optimize edin ve geliştirin

Azure Güvenlik Merkezi'nin değerinin kalbi, önerilerinde yatsın. Öneriler, iş yüklerinizde bulunan belirli güvenlik endişelerine göre uyarlanmıştır ve Güvenlik Merkezi, güvenlik açıklarınızı bulmakla kalarak değil, aynı zamanda bu güvenlik açıklarından nasıl kurtulacağınıza ilişkin özel talimatlar sunarak güvenlik yöneticisinin sizin için çalışmasını sağlar.

![Güvenlik Merkezi önerileri](media/security-center-intro/sc-recommendations.png)

Bu şekilde, Güvenlik Merkezi yalnızca güvenlik ilkeleri ayarlamak için değil, aynı zamanda kaynaklarınız arasında güvenli yapılandırma standartları uygulamak için izin sağlar.

Öneriler, her bir kaynağınızdaki saldırı yüzeyini azaltmanıza yardımcı olur. Buna, her kaynak türünün farklı şekilde değerlendirildiği ve kendi standartları olduğu Azure sanal makineleri, Azure olmayan sunucular ve SQL ve Depolama hesapları gibi Azure PaaS hizmetleri ve daha fazlası dahildir.

![Güvenlik Merkezi öneri örneği](media/security-center-intro/sc-recommendation-example.png)

## <a name="protect-against-threats"></a>Tehditlere karşı koruyun

Güvenlik Merkezi'nin tehdit koruması, Hizmet Olarak Altyapı (IaaS) katmanı, Azure olmayan sunucular ve Azure'daki Hizmet Olarak Platformlar (PaaS) tehditlerini algılamanızı ve önlemenizi sağlar.

Güvenlik Merkezi'nin tehdit koruması, bir saldırı kampanyasının tüm hikayesini, nerede başladığını ve ne leri daha iyi anlamanıza yardımcı olmak için, siber öldürme zinciri analizine dayalı olarak çevrenizdeki uyarıları otomatik olarak ilişkilendiren füzyon öldürücü zincir analizini içerir. kaynaklarınız üzerinde bir tür etkisi vardı.



![Güvenlik Merkezi saldırı önerisi](media/security-center-intro/sc-attack-recommendation.png)

### <a name="integration-with-microsoft-defender-advanced-threat-protection"></a>Microsoft Defender Gelişmiş tehdit koruması ile tümleştirme

Güvenlik Merkezi, Microsoft Defender Gelişmiş Tehdit Koruması ile otomatik, yerel tümleştirmeiçerir. Bu, herhangi bir yapılandırma olmadan Windows ve Linux makinelerinizin Security Center'ın önerileri ve değerlendirmeleri ile tam olarak entegre olduğu anlamına gelir.

Buna ek olarak, Güvenlik Merkezi sunucu ortamlarında uygulama denetim ilkelerini otomatikleştirmenize olanak tanır. Güvenlik Merkezi'ndeki uyarlanabilir uygulama denetimleri, Windows sunucularınızda uçuca uygulama beyaz listesini etkinleştirin. Kuralları oluşturmanız ve ihlalleri kontrol etmeniz gerekmez, hepsi sizin için otomatik olarak yapılır.

### <a name="protect-paas"></a>PaaS'ı Koru

Güvenlik Merkezi, Azure PaaS hizmetleri genelindeki tehditleri algılamanıza yardımcı olur. Azure Uygulama Hizmeti, Azure SQL, Azure Depolama Hesabı ve daha fazla veri hizmeti gibi Azure hizmetlerini hedefleyen tehditleri algılayabilirsiniz. Azure etkinlik günlüklerinizde anormallik algılaması gerçekleştirmek için Microsoft Cloud App Security'nin Kullanıcı ve Varlık Davranış Analizi (UEBA) ile yerel tümleştirmeden de yararlanabilirsiniz.

### <a name="block-brute-force-attacks"></a>Kaba kuvvet saldırılarını engelleme

Güvenlik Merkezi, kaba kuvvet saldırılarına maruz kalmanızı sınırlamanıza yardımcı olur. Sanal makine bağlantı noktalarına erişimi azaltarak, tam zamanında VM erişimini kullanarak, gereksiz erişimi engelleyerek ağınızı sertleşebilirsiniz. Yalnızca yetkili kullanıcılar, izin verilen kaynak IP adresi aralıkları veya IP adresleri için ve sınırlı bir süre için seçili bağlantı noktalarında güvenli erişim ilkeleri ayarlayabilirsiniz.

![Güvenlik Merkezi kaba kuvvet](media/security-center-intro/sc-brute-force.png)

### <a name="protect-data-services"></a>Veri hizmetlerini koruma

Güvenlik Merkezi, verilerinizin Azure SQL'de otomatik olarak sınıflandırını gerçekleştirmenize yardımcı olan özellikleri içerir. Ayrıca Azure SQL ve Depolama hizmetleri genelindeki olası güvenlik açıkları için değerlendirmeler ve bunları nasıl azaltabileceğinize ilişkin öneriler de alabilirsiniz.

### <a name="protect-iot-and-hybrid-cloud-workloads"></a>IoT ve karma bulut iş yüklerini koruyun

Azure Güvenlik Merkezi IoT (Nesnelerin İnterneti), birleşik görünürlük ve denetim, uyarlanabilir tehdit önleme ve şirket içinde çalışan iş yüklerinde akıllı tehdit koruması ve yanıt sağlayarak karma iş yükü korumayı kolaylaştırır, Azure'da ve diğer bulutlarda. Daha fazla bilgi [için IoT için Azure Güvenlik Merkezi'ne](https://docs.microsoft.com/azure/asc-for-iot/)bakın.

## <a name="get-secure-faster"></a>Daha hızlı güvende olun

Microsoft Bulut Uygulama Güvenliği ve Windows Defender Gelişmiş Tehdit Koruması gibi diğer Microsoft güvenlik çözümleriyle sorunsuz entegrasyonla birlikte yerel Azure tümleştirmesi (Azure İlkesi ve Azure Monitör günlükleri dahil) güvenlik çözümü kapsamlı olduğu kadar yerleşik ve kullanıma sunulması da basittir.

Ayrıca, tüm çözümü Azure'un ötesine, diğer bulutlarda ve şirket içi veri merkezlerinde çalışan iş yüklerine genişletebilirsiniz.

### <a name="automatically-discover-and-onboard-azure-resources"></a>Azure kaynaklarını otomatik olarak keşfedin ve yerleşik

Güvenlik Merkezi, Azure ve Azure kaynaklarıyla sorunsuz ve yerel entegrasyon sağlar. Bu, tüm Azure kaynaklarınızda Azure İlkesi ve yerleşik Güvenlik Merkezi ilkelerini içeren eksiksiz bir güvenlik hikayesini bir araya getirebileceğiniz ve bunları oluştururken her şeyin yeni keşfedilen kaynaklara otomatik olarak uygulandığından emin olabileceğiniz anlamına gelir Azure'da.

Kapsamlı günlük koleksiyonu - Windows ve Linux günlükleri tüm güvenlik analitik motoru kaldıraçlı ve öneriler ve uyarılar oluşturmak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik Merkezi ile çalışmaya başlamak için bir Microsoft Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- Azure aboneliğiniz ile Security Center'ın ücretsiz fiyatlandırma katmanı etkinleştirilir. Gelişmiş güvenlik yönetimi ve tehdit koruması özelliklerinden yararlanmak için standart fiyatlandırma katmanına yükseltmeniz gerekir. Standart katman 30 gün boyunca ücretsiz olarak denenebilir. Daha fazla bilgi için [Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/)bakın.
- Güvenlik Merkezi standardını şimdi etkinleştirmeye hazırsanız, [Quickstart: Güvenlik Merkezi Standardı'na](security-center-get-started.md) azure aboneliğinizde adımlar alabilirsiniz.