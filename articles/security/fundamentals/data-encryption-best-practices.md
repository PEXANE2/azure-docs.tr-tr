---
title: Veri güvenliği ve şifreleme en iyi uygulamalar - Microsoft Azure
description: Bu makalede, Azure özelliklerinde yerleşik olarak veri güvenliği ve şifreleme için en iyi uygulamalar kümesi sağlanmaktadır.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
editor: TomSh
ms.assetid: 17ba67ad-e5cd-4a8f-b435-5218df753ca4
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2020
ms.author: terrylan
ms.openlocfilehash: c5bf62f434b2095f7200b5562c38c252a0195c5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243503"
---
# <a name="azure-data-security-and-encryption-best-practices"></a>Azure veri güvenliği ve şifreleme için en iyi yöntemler
Bu makalede, veri güvenliği ve şifreleme için en iyi uygulamalar açıklanmaktadır.

En iyi uygulamalar fikir birliğine dayanır ve geçerli Azure platformu özellikleri ve özellik kümeleriyle çalışır. Görüşler ve teknolojiler zaman içinde değişir ve bu makale, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

## <a name="protect-data"></a>Veri koruma
Buluttaki verilerin korunmasına yardımcı olmak için, verilerinizin oluşabileceği olası durumları ve bu durum için hangi denetimlerin kullanılabildiğini hesaba katmanız gerekir. Azure veri güvenliği ve şifreleme için en iyi uygulamalar aşağıdaki veri durumlarla ilgilidir:

- Istirahatte: Bu, manyetik veya optik disk olsun, fiziksel ortamda statik olarak var olan tüm bilgi depolama nesnelerini, kapsayıcıları ve türlerini içerir.
- Aktarım sırasında: Veriler bileşenler, konumlar veya programlar arasında aktarılırken, bu veri aktarım sırasındadır. Örnekler, ağ üzerinden, bir servis veri yolu üzerinden (ExpressRoute gibi karma bağlantılar da dahil olmak üzere şirket içi buluta ve tam tersi) veya giriş/çıkış işlemi sırasında aktarılmasıdır.

## <a name="choose-a-key-management-solution"></a>Önemli bir yönetim çözümü seçin

Anahtarlarınızı korumak, verilerinizi bulutta korumak için çok önemlidir.

[Azure Key Vault](/azure/key-vault/key-vault-overview), bulut uygulamalarının ve hizmetlerinin kullandığı şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Anahtar Kasası anahtar yönetimi işlemini kolaylaştırır ve verilerinize erişen ve bunları şifreleyen anahtarları denetiminizde tutmanıza olanak sağlar. Geliştiriciler, geliştirme ve test için dakikalar içinde anahtar oluşturabilir ve sonra bunları üretim anahtarlarına geçirebilir. Güvenlik yöneticileri gerektiğinde anahtarlara izin verebilir (ve iptal edebilir).

Tonoz adı verilen birden çok güvenli kapsayıcı oluşturmak için Key Vault'u kullanabilirsiniz. Bu kasalar HSM'ler tarafından desteklenen. Kasalar, uygulama gizli dizilerinin depolanmasını merkezi hale getirerek güvenlik bilgilerini kazayla kaybetme olasılığını azaltmaya yardımcı olur. Anahtar kasaları ayrıca içlerinde depolanmış her şeye erişimi denetler ve günlüğe kaydeder. Azure Key Vault, Taşıma Katmanı Güvenliği (TLS) sertifikalarını isteme ve yenileme işlemlerini halledebilir. Sertifika yaşam döngüsü yönetimi için sağlam bir çözüm için özellikler sağlar.

Azure Key Vault, uygulama anahtarlarını ve sırlarını desteklemek üzere tasarlanmıştır. Key Vault kullanıcı parolaları için bir mağaza olması amaçlanmamıştır.

Key Vault'u kullanmak için en iyi güvenlik uygulamaları aşağıda veda edilebilmelidir.

**En iyi uygulama**: Belirli bir kapsamda kullanıcılara, gruplara ve uygulamalara erişim izni verilmesi.   
**Ayrıntı**: RBAC'ın önceden tanımlanmış rollerini kullanın. Örneğin, anahtar kasalarını yönetmek için bir kullanıcıya erişim izni vermek için, önceden tanımlanmış [Anahtar Kasa Katılımcısı](/azure/role-based-access-control/built-in-roles) rolünü belirli bir kapsamda bu kullanıcıya atarsınız. Bu durumda kapsam bir abonelik, bir kaynak grubu veya sadece belirli bir anahtar kasası olacaktır. Önceden tanımlanmış roller gereksinimlerinize uymuyorsa, [kendi rollerinizi tanımlayabilirsiniz.](/azure/role-based-access-control/custom-roles)

**En iyi yöntem**: Kullanıcıların neye erişene erişene sahip olduğunu kontrol edin.   
**Detay**: Anahtar kasasına erişim iki ayrı arayüz üzerinden kontrol edilir: yönetim düzlemi ve veri düzlemi. Yönetim düzlemi ve veri düzlemi erişim denetimleri birbirinden bağımsız olarak çalışır.

Kullanıcıların neye erişebileceğini denetlemek için RBAC kullanın. Örneğin, anahtar kasasında anahtarları kullanmak için bir uygulama erişimi vermek istiyorsanız, yalnızca anahtar kasa erişim ilkelerini kullanarak veri düzlemi erişim izinleri vermeniz gerekir ve bu uygulama için yönetim düzlemi erişimi gerekmez. Buna karşılık, bir kullanıcının kasa özelliklerini ve etiketlerini okuyabilmesini, ancak anahtar, parola veya sertifikalara erişmemesini istiyorsanız, RBAC kullanarak bu kullanıcıya okuma erişimi verebilirsiniz ve veri düzlemine erişim gerekmez.

**En iyi uygulama**: Sertifikaları anahtar kasanızda saklayın. Sertifikalarınız çok değerlidir. Yanlış ellerde, uygulamanızın güvenliği veya verilerinizin güvenliği tehlikeye atılabilir.   
**Ayrıntı**: Azure Kaynak Yöneticisi, Azure Anahtar Kasası'nda depolanan sertifikaları, VM'ler dağıtıldığında Azure VM'lere güvenli bir şekilde dağıtabilir. Anahtar kasası için uygun erişim ilkelerini ayarlayarak, sertifikanıza kimlerin erişebileceğini de denetleyebilirsiniz. Diğer bir avantaj ise tüm sertifikalarınızı Azure Key Vault’ta yönetmenizdir. Daha fazla bilgi [için müşteri tarafından yönetilen Key Vault'tan VM'lere Sertifikaları Dağıt'a](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) bakın.

**En iyi yöntem**: Anahtar kasalarının veya anahtar kasa nesnelerinin silinmesini kurtarabildiğinizden emin olun.   
**Detay**: Anahtar kasalarının veya anahtar kasa nesnelerinin silinmesi yanlışlıkla veya kötü niyetli olabilir. Başta bekleyen verileri şifrelemek için kullanılan anahtarlar için olmak üzere, Key Vault’un yazılım silme ve temizleme koruması özelliklerini etkinleştirin. Bu anahtarların silinmesi veri kaybına eşdeğerdir; bu nedenle, gerekirse silinen kasaları ve kasa nesnelerini kurtarabilirsiniz. Anahtar Kasa kurtarma işlemlerini düzenli olarak uygulayın.

> [!NOTE]
> Bir kullanıcı bir anahtar kasası yönetim düzleminde katkıda bulunan izinlerine (RBAC) sahipse, bir anahtar kasası erişim ilkesi belirleyerek kendisine veri düzlemine erişim izni verebilir. Anahtar kasalarınıza, anahtarlarınıza, anahtarlarınıza, sırlarınıza ve sertifikalarınıza yalnızca yetkili kişilerin erişebilmesini ve yönetebilmesini sağlamak için, kimin anahtar kasalarınıza erişebileceğini sıkı bir şekilde kontrol etmenizi öneririz.
>
>

## <a name="manage-with-secure-workstations"></a>Güvenli iş istasyonlarıyla yönetin

> [!NOTE]
> Abonelik yöneticisi veya sahibi güvenli bir erişim iş istasyonu veya ayrıcalıklı bir erişim iş istasyonu kullanmalıdır.
>
>

Saldırıların büyük çoğunluğu son kullanıcıyı hedef lediğinden, bitiş noktası birincil saldırı noktalarından biri haline gelir. Bitiş noktasını tehlikeye atan bir saldırgan, kuruluşun verilerine erişmek için kullanıcının kimlik bilgilerini kullanabilir. Uç nokta saldırılarının çoğu, kullanıcıların yerel iş istasyonlarında yönetici olması durumundan yararlanır.

**En iyi uygulama**: Hassas hesapları, görevleri ve verileri korumak için güvenli bir yönetim iş istasyonu kullanın.   
**Ayrıntı**: İş istasyonlarındaki saldırı yüzeyini azaltmak için ayrıcalıklı bir [erişim iş istasyonu](https://technet.microsoft.com/library/mt634654.aspx) kullanın. Bu güvenli yönetim iş istasyonları, bu saldırılardan bazılarını azaltmanıza ve verilerinizin daha güvenli olduğundan emin olsanız yardımcı olabilir.

**En iyi uygulama**: Uç nokta koruması sağlayın.   
**Ayrıntı**: Veri konumuna (bulut veya şirket içi) bakılmaksızın veri tüketmek için kullanılan tüm aygıtlarda güvenlik ilkelerini uygulayın.

## <a name="protect-data-at-rest"></a>Bekleyen verileri koruma

[Veri şifreleme, veri](https://cloudblogs.microsoft.com/microsoftsecure/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) gizliliği, uyumluluk ve veri egemenliği yolunda atılmış zorunlu bir adımdır.

**En iyi yöntem**: Verilerinizin korunmasına yardımcı olmak için disk şifreleme uygulayın.   
**Ayrıntı**: [Azure Disk Şifreleme'yi](/azure/security/azure-security-disk-encryption-overview)kullanın. BT yöneticilerinin Windows ve Linux IaaS VM disklerini şifrelemesini sağlar. Disk Şifreleme, işletim sistemi ve veri diskleri için birim şifreleme sağlamak için endüstri standardı Windows BitLocker özelliğini ve Linux dm-crypt özelliğini birleştirir.

Azure Depolama ve Azure SQL Veritabanı varsayılan olarak verileri parolayla şifreler ve birçok hizmet bir seçenek olarak şifreleme sunar. Verilerinize erişen ve bunları şifreleyen anahtarların denetimini ele almak için Azure Key Vault kullanabilirsiniz. [Daha fazla bilgi edinmek için Azure kaynak sağlayıcıları şifreleme modeli desteğine](encryption-atrest.md#azure-resource-providers-encryption-model-support)bakın.

**En iyi uygulamalar**: Yetkisiz veri erişimiyle ilgili riskleri azaltmaya yardımcı olmak için şifrelemekullanın.   
**Ayrıntı**: Hassas verileri onlara yazmadan önce sürücülerinizi şifreleyin.

Veri şifrelemesini zorlamayan kuruluşlar, veri gizliliği sorunlarına daha fazla maruz kalır. Örneğin, yetkisiz veya haydut kullanıcılar tehlikeye giren hesaplardaki verileri çalabilir veya Açık Formatta kodlanmış verilere yetkisiz erişim elde edebilir. Şirketler ayrıca, endüstri yönetmeliklerine uymak için veri güvenliğini artırmak için çalışkan olduklarını ve doğru güvenlik denetimlerini kullandıklarını kanıtlamalıdır.

## <a name="protect-data-in-transit"></a>Aktarılan verileri koruma

Aktarımdaki verilerin korunması veri koruma stratejinizin temel parçalarından biri olmalıdır. Veriler birçok konum arasında gidip geldiğinden, farklı konumlar arasındaki veri alışverişinde her zaman SSL/TLS protokollerini kullanmanızı öneririz. Bazı durumlarda VPN kullanarak şirket içi ile bulut altyapılarınız arasındaki iletişim kanalının tamamını yalıtmak isteyebilirsiniz.

Şirket içi altyapınızla Azure arasında taşınan veriler için HTTPS veya VPN gibi uygun korumaları göz önünde bulundurun. Azure sanal ağı ile genel internet üzerinden şirket içi konum arasında şifreli trafik gönderirken [Azure VPN Ağ Geçidi'ni](../../vpn-gateway/index.yml)kullanın.

Aşağıda Azure VPN Ağ Geçidi, SSL/TLS ve HTTPS kullanmaya özgü en iyi uygulamalar vereyim.

**En iyi uygulama**: Şirket içinde bulunan birden çok iş istasyonundan Azure sanal ağına güvenli erişim.   
**Detay**: [Siteden siteye VPN](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)kullanın.

**En iyi uygulama**: Şirket içinde bulunan tek bir iş istasyonundan Azure sanal ağına güvenli erişim.   
**Detay**: [Noktadan siteye VPN](/azure/vpn-gateway/vpn-gateway-point-to-site-create)kullanın.

**En iyi yöntem**: Daha büyük veri kümelerini özel bir yüksek hızlı WAN bağlantısı üzerinden taşıyın.   
**Detay**: [ExpressRoute](/azure/expressroute/expressroute-introduction)kullanın. ExpressRoute kullanmayı seçerseniz ek koruma için SSL/TLS veya başka protokoller kullanarak verileri uygulama düzeyinde de şifreleyebilirsiniz.

**En iyi uygulama**: Azure portalı üzerinden Azure Depolama ile etkileşim kurun.   
**Detay**: Tüm işlemler HTTPS üzerinden gerçekleşir. [Azure Depolama](https://azure.microsoft.com/services/storage/)ile etkileşimde kalmak için HTTPS üzerinden Storage [REST API'sini](https://msdn.microsoft.com/library/azure/dd179355.aspx) de kullanabilirsiniz.

Aktarım sırasında verileri koruyamayan [kuruluşlar, ortadaki adam saldırılarına,](https://technet.microsoft.com/library/gg195821.aspx) [gizlice dinlemeye](https://technet.microsoft.com/library/gg195641.aspx)ve oturum kaçırmaya karşı daha duyarlıdır. Bu saldırılar gizli verilere erişim kazanmanın ilk adımı olabilir.

## <a name="secure-email-documents-and-sensitive-data"></a>Güvenli e-posta, belgeler ve hassas veriler

Şirketiniz dışında paylaştığınız e-postaları, belgeleri ve hassas verileri kontrol etmek ve güvenli hale getirmek istiyorsunuz. [Azure Information Protection](/azure/information-protection/), kuruluşların belgelerini ve e-postalarını sınıflandırmasına, etiketlemesine ve korumasına yardımcı olan bulut tabanlı bir çözümdür. Bu işlem, kuralları ve koşulları tanımlayan yöneticiler tarafından, kullanıcılar tarafından el ile veya kullanıcıların öneriler aldığı bir kombinasyon tarafından otomatik olarak yapılabilir.

Sınıflandırma, verilerin nerede depolandığıveya kiminle paylaşıldıklarına bakılmaksızın her zaman tanımlanabilir. Etiketler üst bilgi, alt bilgi veya filigran gibi görsel işaretler içerir. Dosyalara ve e-posta üst bilgilerine düz metin olarak meta veriler eklenir. Açık metin, veri kaybını önlemek için çözümler gibi diğer hizmetlerin sınıflandırmayı belirleyebilmesini ve uygun eylemi gerçekleştirebilmesini sağlar.

Koruma teknolojisi Azure Hakları Yönetimi 'ni (Azure RMS) kullanır. Bu teknoloji, Office 365 ve Azure Active Directory gibi diğer Microsoft bulut hizmetleri ve uygulamalarıyla tümleşiktir. Bu koruma teknolojisinde şifreleme, kimlik ve yetkilendirme ilkeleri kullanılır. Azure RMS aracılığıyla uygulanan koruma, kuruluşunuzun, ağların, dosya sunucularının ve uygulamalarının içinde veya dışında, konumdan bağımsız olarak belgeler ve e-postalarda kalır.

Bu bilgi koruma çözümü, diğer kişilerle paylaşılsa bile verilerinizin denetimini siz de sağlar. Azure RMS'yi, ister şirket içinde ister bulutta olsun, kendi iş hattı uygulamalarınızla ve yazılım satıcılarının bilgi koruma çözümleriyle de kullanabilirsiniz.

Şunları yapmanızı öneririz:

- Kuruluşunuz için [Azure Bilgi Koruması'nı dağıtın.](/azure/information-protection/deployment-roadmap)
- İş gereksinimlerinizi yansıtan etiketler uygulayın. Örneğin: Bu verileri sınıflandırmak ve korumak için çok gizli veriler içeren tüm belge ve e-postalara "son derece gizli" adlı bir etiket uygulayın. Ardından, belirttiğiniz kısıtlamalarla bu verilere yalnızca yetkili kullanıcılar erişebilir.
- Kuruluşunuzun koruma hizmetini nasıl kullandığını izleyebilmeniz [için Azure RMS için kullanım günlüğe kaydetmeyi](/azure/information-protection/log-analyze-usage) yapılandırın.

[Veri sınıflandırması](https://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) ve dosya koruması konusunda zayıf olan kuruluşlar, veri sızıntısına veya veri kötüye kullanımına karşı daha duyarlı olabilir. Uygun dosya koruması yla, işletmeniz hakkında bilgi edinmek, riskli davranışları algılamak ve düzeltici önlemler almak, belgelere erişimi izlemek ve benzeri konularda veri akışlarını analiz edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure'u kullanarak bulut çözümlerinizi tasarlarken, dağıtırken ve yönetirken kullanabileceğiniz daha fazla güvenlik en iyi uygulamaları için Azure güvenlik en iyi uygulamaları ve [desenleri](best-practices-and-patterns.md) görün.

Azure güvenliği ve ilgili Microsoft hizmetleri hakkında daha genel bilgi sağlamak için aşağıdaki kaynaklar kullanılabilir:
* [Azure Güvenlik Ekibi Blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure Güvenlik'teki en son bilgiler için
* [Microsoft Güvenlik Yanıt Merkezi](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar da dahil olmak üzere Microsoft güvenlik açıklarının rapor edilebildiği veya e-posta yoluylasecure@microsoft.com
