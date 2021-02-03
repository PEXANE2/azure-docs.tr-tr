---
title: Azure Güvenlik Merkezi için sürüm notları
description: Azure Güvenlik Merkezi 'nde nelerin yeni ve değiştirilmiş olduğuna ilişkin bir açıklama
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2021
ms.author: memildin
ms.openlocfilehash: cdc29f89307a986b2d71604ca495eac45458632b
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99526628"
---
# <a name="whats-new-in-azure-security-center"></a>Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?

Güvenlik Merkezi, etkin geliştirme aşamasındadır ve gelişmede geliştirmeler sunar. En son gelişmelerden haberdar olmak için bu sayfada yeni özellikler, hata düzeltmeleri ve kullanım dışı işlevler hakkında bilgi verilmektedir.

Bu sayfa sıklıkla güncelleştirildi, bu nedenle sık olarak yeniden ziyaret edin. 

Güvenlik Merkezi 'ne yakında çıkacak *planlı* değişiklikler hakkında bilgi edinmek için bkz. [Azure Güvenlik Merkezi 'nde yapılan önemli değişiklikler](upcoming-changes.md). 

> [!TIP]
> Altı aydan eski olan öğeleri arıyorsanız, [Azure Güvenlik Merkezi 'ndeki yenilikler Için arşivde](release-notes-archive.md)bulabilirsiniz.


## <a name="february-2021"></a>Şubat 2021

Şubat 'taki güncelleştirmeler şunları içerir:

- [Kubernetes iş yükü koruma önerileri genel kullanıma sunuldu (GA)](#kubernetes-workload-protection-recommendations-released-for-general-availability-ga)
- [Öneri ayrıntıları sayfasından ilkeye doğrudan bağlantı](#direct-link-to-policy-from-recommendation-details-page)
- [SQL veri sınıflandırması önerisi artık güvenli puanınızı etkilemiyor](#sql-data-classification-recommendation-no-longer-affect-your-secure-score)

### <a name="kubernetes-workload-protection-recommendations-released-for-general-availability-ga"></a>Kubernetes iş yükü koruma önerileri genel kullanıma sunuldu (GA)

Kubernetes iş yükü korumaları için öneri kümesinin genel kullanılabilirliğini (GA) duyurmaktan mutluluk duyuyoruz.

Kubernetes iş yüklerinin varsayılan olarak güvenli olduğundan emin olmak için, güvenlik merkezi Kubernetes giriş denetimi ile zorlama seçenekleri dahil olmak üzere Kubernetes düzeyi sağlamlaştırma önerilerini ekledi.

Kubernetes için Azure Ilke eklentisi, Azure Kubernetes Service (AKS) kümenize yüklendiğinde, Kubernetes API sunucusuna gönderilen her istek, kümede kalıcı hale gelmeden önce 13 güvenlik önerisi olarak görüntülenen, önceden tanımlanmış en iyi uygulamalar kümesine göre izlenir. Daha sonra, en iyi uygulamaları zorlamak ve gelecekteki iş yükleri için bunları zorunlu kılmak üzere yapılandırabilirsiniz.

Örneğin, ayrıcalıklı kapsayıcıların oluşturulmaması ve ileride yapılacak istekleri engellenemeyeceksiniz.

[Kubernetes giriş denetimini kullanan Iş yükü koruma en iyi uygulamaları](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control)hakkında daha fazla bilgi edinin.

> [!NOTE]
> Öneriler önizlemede olduğundan, AKS küme kaynağını sağlıksız bir şekilde işlemedik ve güvenli puanınızın hesaplamalarına dahil etmiyordu. Bu GA duyurusu ile bunlar puan hesaplamasına dahil edilir. Zaten düzeltilmediyse, bu durum güvenli puanınızda küçük bir etkiye neden olur. [Azure Güvenlik Merkezi 'ndeki önerileri](security-center-remediate-recommendations.md)düzeltme bölümünde açıklandığı gibi, mümkün olan yerlerde bunları düzeltin.


### <a name="direct-link-to-policy-from-recommendation-details-page"></a>Öneri ayrıntıları sayfasından ilkeye doğrudan bağlantı

Bir önerinin ayrıntılarını gözden geçirirken genellikle temel alınan ilkeyi görmeniz yararlı olur. Bir ilke tarafından desteklenen her öneri için, öneri ayrıntıları sayfasından yeni bir bağlantı vardır:

:::image type="content" source="media/release-notes/view-policy-definition.png" alt-text="Öneriyi destekleyen belirli bir ilke için Azure Ilke sayfasına bağlantı":::

İlke tanımını görüntülemek ve değerlendirme mantığını gözden geçirmek için bu bağlantıyı kullanın. 

[Güvenlik önerileri başvuru kılavuzumuzdaki](recommendations-reference.md)önerilerin listesini gözden geçiriyorsunuz, Ayrıca bu bağlantıları ilke tanımı sayfalarına da görürsünüz:

:::image type="content" source="media/release-notes/view-policy-definition-from-documentation.png" alt-text="Belirli bir ilke için Azure Ilke sayfasına doğrudan Azure Güvenlik Merkezi öneriler başvuru sayfasından erişme":::


### <a name="sql-data-classification-recommendation-no-longer-affect-your-secure-score"></a>SQL veri sınıflandırması önerisi artık güvenli puanınızı etkilemiyor

**SQL veritabanlarınızdaki önerinin hassas verileri** , artık güvenli puanınızı etkilememelidir. Bu, denetim artık 0 ' ın güvenli bir puan değerine sahip olması için **veri sınıflandırması** güvenlik denetimindeki tek öneriden oluşur.



## <a name="january-2021"></a>Ocak 2021

Ocak ayında güncelleştirmeler şunları içerir:

- [Azure Güvenlik kıyaslaması artık Azure Güvenlik Merkezi için varsayılan ilke girişimidir](#azure-security-benchmark-is-now-the-default-policy-initiative-for-azure-security-center)
- [Şirket içi ve çoklu bulut makinelerinde güvenlik açığı değerlendirmesi, genel kullanıma sunuldu (GA)](#vulnerability-assessment-for-on-premise-and-multi-cloud-machines-is-released-for-general-availability-ga)
- [Yönetim grupları için güvenli puan şimdi önizlemede kullanılabilir](#secure-score-for-management-groups-is-now-available-in-preview)
- [Güvenli puan API 'SI genel kullanıma sunuldu (GA)](#secure-score-api-is-released-for-general-availability-ga)
- [App Service için Azure Defender 'a Dangling DNS korumaları eklendi](#dangling-dns-protections-added-to-azure-defender-for-app-service)
- [Çoklu bulut bağlayıcıları genel kullanıma sunuldu (GA)](#multi-cloud-connectors-are-released-for-general-availability-ga)
- [Abonelikler ve yönetim grupları için tüm önerileri güvenli puanınızdan muaf tutma](#exempt-entire-recommendations-from-your-secure-score-for-subscriptions-and-management-groups)
- [Kullanıcılar artık genel yöneticlerinden kiracı genelinde görünürlük isteğinde bulunabilir](#users-can-now-request-tenant-wide-visibility-from-their-global-administrator)
- [Azure Güvenlik kıyaslamasının kapsamını artırmak için 35 önizleme önerisi eklendi](#35-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark)
- [Filtrelenmiş öneriler listesinin CSV dışa aktarılması](#csv-export-of-filtered-list-of-recommendations)
- ["Uygulanamaz" kaynakları artık Azure Ilke değerlendirmelerinde "uyumlu" olarak bildirildi](#not-applicable-resources-now-reported-as-compliant-in-azure-policy-assessments)
- [Sürekli dışa aktarma ile güvenli Puanlama ve mevzuat uyumluluk verilerinin haftalık anlık görüntülerini dışarı aktarma (Önizleme)](#export-weekly-snapshots-of-secure-score-and-regulatory-compliance-data-with-continuous-export-preview)


### <a name="azure-security-benchmark-is-now-the-default-policy-initiative-for-azure-security-center"></a>Azure Güvenlik kıyaslaması artık Azure Güvenlik Merkezi için varsayılan ilke girişimidir

Azure Güvenlik kıyaslaması, yaygın uyumluluk çerçevelerine göre güvenlik ve uyum en iyi uygulamaları için Microsoft tarafından yazılan, Azure 'a özgü bir dizi kılavuzlardır. Bu, yaygın olarak kullanılan kıyaslama, [Internet güvenliği (CIS) Için merkezden](https://www.cisecurity.org/benchmark/azure/) ve [ulusal standartlar ve teknolojı Enstitüsü (NIST)](https://www.nist.gov/) ile bulut merkezli güvenliğe odaklanarak yapılar oluşturur.

Son aylarda Güvenlik Merkezi 'nin yerleşik güvenlik önerileri listesi, Bu kıyaslama kapsamımızın kapsamını genişletmenizi önemli ölçüde artmıştır.

Bu sürümden, kıyaslama, güvenlik merkezi 'nin önerilerinden ve varsayılan ilke girişimi olarak tam tümleştirilen bir temelidir. 

Tüm Azure hizmetlerinin belgelerinde güvenlik temeli sayfası vardır. Örneğin, [Bu güvenlik merkezi 'nin temelini](security-baseline.md). Bu temeller Azure Güvenlik kıyaslaması üzerine kurulmuştur.

Güvenlik Merkezi 'nin mevzuat uyumluluk panosunu kullanıyorsanız, bir geçiş dönemi boyunca kıyaslamaya ait iki örnek görürsünüz:

:::image type="content" source="media/release-notes/regulatory-compliance-with-azure-security-benchmark.png" alt-text="Azure Güvenlik Merkezi 'nin Azure Güvenlik kıyaslaması gösteren yasal uyumluluk panosu":::

Mevcut öneriler etkilenmemiştir ve kıyaslama arttıkça, değişiklikler otomatik olarak güvenlik merkezi 'nde yansıtılır. 

Daha fazla bilgi edinmek için aşağıdaki sayfalara bakın:

- [Azure Güvenlik kıyaslaması hakkında daha fazla bilgi](../security/benchmarks/introduction.md)
- [Yasal uyumluluk panonuzda standartlar kümesini özelleştirme](update-regulatory-compliance-packages.md)

### <a name="vulnerability-assessment-for-on-premise-and-multi-cloud-machines-is-released-for-general-availability-ga"></a>Şirket içi ve çoklu bulut makinelerinde güvenlik açığı değerlendirmesi, genel kullanıma sunuldu (GA)

Ekim 'de, Azure Arc etkin sunucularını, sunucuların tümleşik güvenlik açığı değerlendirmesi tarayıcısı (Qualys tarafından desteklenir) [Için Azure Defender](defender-for-servers-introduction.md)ile taramanın bir önizlemesini duyurduk.

Artık genel kullanılabilirlik (GA) için yayımlanmıştır.

Azure dışı makinelerinizde Azure yayı 'yi etkinleştirdiyseniz, Güvenlik Merkezi, tümleşik güvenlik açığı tarayıcısını bunlara el ile ve ölçekli olarak dağıtmayı sağlar.

Bu güncelleştirmeyle, Azure Defender 'ın tüm Azure ve Azure dışı varlıklarınızda güvenlik açığı yönetimi programınızı birleştirmek için **Azure Defender** 'ın gücünü açığa çıkarın.

Ana yetenekler:

- Azure Arc makinelerinde VA (güvenlik açığı değerlendirmesi) tarayıcı sağlama durumunu izleme
- Tümleşik VA aracısını korumasız Windows ve Linux Azure yay makinelerine sağlama (el ile ve ölçekli)
- Dağıtılan aracılardan algılanan güvenlik açıklarını alma ve çözümleme (el ile ve ölçekli)
- Azure sanal makineleri ve Azure yay makineleri için Birleşik deneyim

[Tümleşik güvenlik açığı tarayıcısını karma makinelerinize dağıtma hakkında daha fazla bilgi edinin](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines).

[Azure Arc etkin sunucuları hakkında daha fazla bilgi edinin](../azure-arc/servers/index.yml).


### <a name="secure-score-for-management-groups-is-now-available-in-preview"></a>Yönetim grupları için güvenli puan şimdi önizlemede kullanılabilir

Güvenli puan sayfası artık abonelik düzeyine ek olarak yönetim gruplarınız için toplanmış güvenli puanları gösterir. Böylece artık kuruluşunuzdaki yönetim gruplarının listesini ve her bir yönetim grubunun Puanını görebilirsiniz.

:::image type="content" source="media/secure-score-security-controls/secure-score-management-groups.png" alt-text="Yönetim gruplarınız için güvenli puanları görüntüleme.":::

[Azure Güvenlik Merkezi 'nde güvenli puan ve güvenlik denetimleri](secure-score-security-controls.md)hakkında daha fazla bilgi edinin.

### <a name="secure-score-api-is-released-for-general-availability-ga"></a>Güvenli puan API 'SI genel kullanıma sunuldu (GA)

Artık puanınızı [güvenli Puanlama API 'si](/rest/api/securitycenter/securescores/)aracılığıyla erişebilirsiniz. API yöntemleri, verileri sorgulama ve zaman içinde güvenli Puanlarınızın kendi raporlama mekanizmanızı oluşturma esnekliğini sağlar. Örneğin:

- belirli bir aboneliğin Puanını almak için **güvenli puanlar** API 'sini kullanma
- güvenlik denetimlerini ve aboneliklerinizin geçerli Puanını listelemek için **güvenli puan denetimleri** API 'sini kullanın

[GitHub Community 'nin güvenli Puanlama alanındaki](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score)güvenli Puanlama API 'si ile mümkün kılınan dış araçlar hakkında bilgi edinin.

[Azure Güvenlik Merkezi 'nde güvenli puan ve güvenlik denetimleri](secure-score-security-controls.md)hakkında daha fazla bilgi edinin.


### <a name="dangling-dns-protections-added-to-azure-defender-for-app-service"></a>App Service için Azure Defender 'a Dangling DNS korumaları eklendi

Alt etki alanı kuruluşları, kuruluşlar için ortak ve yüksek öneme sahip bir tehdittir. Önceden sağlanmış bir Web sitesine işaret eden bir DNS kaydınız varsa, alt etki alanı ele alma işlemi gerçekleşebilir. Bu tür DNS kayıtları, "dangze DNS" girişleri olarak da bilinir. CNAME kayıtları özellikle bu tehdide karşı savunmasız. 

Alt etki alanı cılar, tehdit aktörlerini bir kuruluşun etki alanı için tasarlanan trafiği kötü amaçlı etkinlik gerçekleştiren bir siteye yeniden yönlendirmek üzere etkinleştirir.

App Service için Azure Defender, artık bir App Service Web sitesi kullanımdan kalkdığı zaman DNS girdilerini algılar. Bu, DNS girişinin var olmayan bir kaynağa işaret ettiği ve Web sitenizin ele alındığı bir alt etki alanıyla güvenlik açığının olduğu bir çalışmadır. Bu korumalar, etki alanlarınızın Azure DNS veya dış etki alanı kaydedicisi ile yönetilip yönetilmediği ve hem Windows hem de Linux üzerinde App Service App Service için geçerlidir.

Daha fazla bilgi edinin:

- [Uyarı başvuru tablosu App Service](alerts-reference.md#alerts-azureappserv) -BIR tehlike DNS girişi algılandığında tetiklenecek Iki yeni Azure Defender uyarısı içerir
- [DNS girişlerinin tehlikelere geçmeyi önleyin ve alt etki alanı devrini kullanmaktan kaçının](../security/fundamentals/subdomain-takeover.md)
- [App Service için Azure Defender 'a giriş](defender-for-app-service-introduction.md)


### <a name="multi-cloud-connectors-are-released-for-general-availability-ga"></a>Çoklu bulut bağlayıcıları genel kullanıma sunuldu (GA)

Bulut güvenlik hizmetleri genellikle birden çok bulut platformunu kapsayan bulut iş yükleri ile aynı olmalıdır.

Azure Güvenlik Merkezi, Azure, Amazon Web Services (AWS) ve Google Cloud Platform (GCP) iş yüklerini korur.

AWS veya GCP hesaplarınızı bağlamak, AWS güvenlik merkezi ve GCP güvenlik komut merkezi gibi yerel güvenlik araçlarını Azure Güvenlik Merkezi 'ne tümleştirir.

Bu özellik, güvenlik merkezi 'nin tüm büyük bulut ortamlarında görünürlük ve koruma sağladığı anlamına gelir. Bu tümleştirmenin avantajlarından bazıları şunlardır:

- Otomatik aracı sağlama-Güvenlik Merkezi, Log Analytics aracısını AWS örneklerinizi dağıtmak için Azure yay kullanır
- İlke yönetimi
- Güvenlik açığı yönetimi
- Gömülü uç nokta algılama ve yanıtı (EDR)
- Güvenlik yapılandırması hataları algılanamadı
- Tüm bulut sağlayıcılarından gelen güvenlik önerilerini gösteren tek bir görünüm
- Tüm kaynaklarınızı Güvenlik Merkezi 'nin güvenli puan hesaplamalarına dahil edin
- AWS ve GCP kaynaklarınızın yasal uyumluluk değerlendirmeleri

Güvenlik Merkezi 'nin menüsünden **çoklu bulut bağlayıcıları** ' nı seçin ve yeni bağlayıcılar oluşturma seçeneklerini görürsünüz:

:::image type="content" source="./media/quickstart-onboard-aws/add-aws-account.png" alt-text="Güvenlik Merkezi 'nin çoklu bulut bağlayıcıları sayfasına AWS hesabı Ekle düğmesi":::

Daha fazlasını öğrenin:
- [AWS hesaplarınızı Azure Güvenlik Merkezi 'ne bağlama](quickstart-onboard-aws.md)
- [GCP hesaplarınızı Azure Güvenlik Merkezi 'ne bağlama](quickstart-onboard-gcp.md)


### <a name="exempt-entire-recommendations-from-your-secure-score-for-subscriptions-and-management-groups"></a>Abonelikler ve yönetim grupları için tüm önerileri güvenli puanınızdan muaf tutma

Tüm önerilerin dahil olduğu istisna özelliğini genişletiyoruz. Güvenlik Merkezi 'nin abonelikleriniz, yönetim grubunuz veya kaynaklarınız için yaptığı güvenlik önerilerini hassas bir şekilde ayarlamaya yönelik daha fazla seçenek sağlar.

Bazen, sorunu Güvenlik Merkezi 'nin algıladığı üçüncü taraf bir araçla çözeceğinizde, bir kaynak sağlıksız olarak listelenecektir. Ya da bir öneri, ait olmadığı yerde bir kapsamda gösterilecektir. Öneri, belirli bir abonelik için uygun olmayabilir. Ya da kuruluşunuz, belirli kaynakla veya öneriyle ilgili riskleri kabul etmeye karar verdi.

Bu önizleme özelliğiyle artık öneri için bir istisna oluşturabilirsiniz:

- İleride sağlıksız kaynaklarla listelenmediğinden emin olmak için **bir kaynağı muaf tut** ve güvenli puanınızı etkilemez. Kaynak geçerli değil olarak listelenecektir ve bu nedenle seçtiğiniz belirli bir gerekçe ile "muaf tutulan" olarak gösterilir.

- Önerinin güvenli puanınızı etkilememesini sağlamak için **bir aboneliği veya yönetim grubunu muaf tut** ve gelecekte abonelik veya yönetim grubu için gösterilmeyecektir. Bu, mevcut kaynaklarla ve gelecekte oluşturduğunuz herhangi bir kaynak ile ilgilidir. Öneri, seçtiğiniz kapsam için seçtiğiniz belirli bir gerekçe ile işaretlenir.

[Güvenli puanınızdan kaynakları ve önerileri muaf tutmak](exempt-resource.md)için daha fazla bilgi edinin.



### <a name="users-can-now-request-tenant-wide-visibility-from-their-global-administrator"></a>Kullanıcılar artık genel yöneticlerinden kiracı genelinde görünürlük isteğinde bulunabilir

Bir kullanıcının güvenlik merkezi verilerini görme izni yoksa, kuruluşunuzun genel Yöneticisi 'nden izin istemek için bir bağlantı görür. İstek, beğendikleri rolü ve neden gerekli olduğu konusunda gerekçe içerir.

:::image type="content" source="media/security-center-management-groups/request-tenant-permissions.png" alt-text="Kullanıcıya kiracı genelinde izinler isteyebilecekleri bir başlık bildiren başlık.":::

[Sizinki yetersiz olduğunda kiracı genelinde Izin iste](security-center-management-groups.md#request-tenant-wide-permissions-when-yours-are-insufficient) hakkında daha fazla bilgi edinin.


### <a name="35-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark"></a>Azure Güvenlik kıyaslamasının kapsamını artırmak için 35 önizleme önerisi eklendi

Azure Güvenlik kıyaslaması, Azure Güvenlik Merkezi 'ndeki varsayılan ilke girişimidir. 

Bu kıyaslama kapsamını artırmak için, güvenlik merkezi 'ne aşağıdaki 35 önizleme önerileri eklenmiştir.

> [!TIP]
> Önizleme önerileri bir kaynağı sağlıksız bir şekilde işlemez ve güvenli puanınızın hesaplamalarına dahil değildir. Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunmak için bunları mümkün olduğunda düzeltin. [Azure Güvenlik Merkezi 'ndeki önerileri düzeltin](security-center-remediate-recommendations.md)bölümünde bu önerilere yanıt verme hakkında daha fazla bilgi edinin.

| Güvenlik denetimi                     | Yeni öneriler                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bekleyen şifrelemeyi etkinleştir            | -Azure Cosmos DB hesapları, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır<br>-Azure Machine Learning çalışma alanları, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)<br>-MySQL sunucuları için kendi anahtar veri korumanın etkinleştirilmesi gerekir<br>-PostgreSQL sunucuları için kendi anahtar veri korumanızı getir özelliği etkinleştirilmelidir<br>Bilişsel hizmetler hesapları, müşteri tarafından yönetilen bir anahtarla (CMK) veri şifrelemeyi etkinleştirmelidir<br>-Kapsayıcı kayıt defterleri, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)<br>-SQL yönetilen örnekler, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır<br>-SQL sunucuları, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır<br>-Depolama hesapları, şifreleme için müşteri tarafından yönetilen anahtar (CMK) kullanmalıdır                                                                                                                                                              |
| En iyi güvenlik uygulamalarını uygulayın    | -Abonelikler güvenlik sorunları için bir iletişim e-posta adresine sahip olmalıdır<br> -Log Analytics aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir<br> -Yüksek önem derecesine sahip uyarılar için e-posta bildirimi etkinleştirilmelidir<br> -Yüksek önem derecesi uyarıları için abonelik sahibine e-posta bildirimi etkinleştirilmelidir<br> -Anahtar kasaları Temizleme koruması etkinleştirilmelidir<br> -Anahtar kasaları geçici silme etkin olmalıdır |
| Erişimi ve izinleri yönetme        | -Function uygulamalarında ' Istemci sertifikaları (gelen istemci sertifikaları) ' etkin olmalıdır |
| DDoS saldırılarına karşı uygulamaları koruma | -Web uygulaması güvenlik duvarı (WAF) Application Gateway için etkinleştirilmelidir<br> -Azure ön kapı hizmeti hizmeti için Web uygulaması güvenlik duvarı (WAF) etkinleştirilmelidir |
| Yetkisiz ağ erişimini kısıtla | -Güvenlik duvarının Key Vault üzerinde etkinleştirilmesi gerekir<br> -Özel uç nokta Key Vault için yapılandırılmalıdır<br> -Uygulama yapılandırması özel bağlantı kullanmalıdır<br> -Redsıs için Azure önbelleği bir sanal ağ içinde yer almalıdır<br> -Azure Event Grid etki alanları özel bağlantı kullanmalıdır<br> -Azure Event Grid konular özel bağlantı kullanmalıdır<br> -Azure Machine Learning çalışma alanları özel bağlantı kullanmalıdır<br> -Azure SignalR hizmeti özel bağlantı kullanmalıdır<br> -Azure yay bulutu, ağ ekleme kullanmalıdır<br> -Kapsayıcı kayıt defterleri Kısıtlanmamış ağ erişimine izin vermiyor<br> -Kapsayıcı kayıt defterleri özel bağlantı kullanmalıdır<br> -MariaDB sunucuları için genel ağ erişimi devre dışı bırakılmalıdır<br> -MySQL sunucuları için genel ağ erişimi devre dışı bırakılmalıdır<br> -PostgreSQL sunucuları için genel ağ erişimi devre dışı bırakılmalıdır<br> -Depolama hesabı bir özel bağlantı bağlantısı kullanmalıdır<br> -Depolama hesapları, sanal ağ kurallarını kullanarak ağ erişimini kısıtlamalı<br> -VM görüntü Oluşturucu şablonlarının özel bağlantı kullanması gerekir|
|                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

İlgili bağlantılar:

- [Azure Güvenlik kıyaslaması hakkında daha fazla bilgi](../security/benchmarks/introduction.md)
- [MariaDB için Azure veritabanı hakkında daha fazla bilgi edinin](../mariadb/overview.md)
- [MySQL için Azure veritabanı hakkında daha fazla bilgi edinin](../mysql/overview.md)
- [PostgreSQL için Azure veritabanı hakkında daha fazla bilgi edinin](../postgresql/overview.md)




### <a name="csv-export-of-filtered-list-of-recommendations"></a>Filtrelenmiş öneriler listesinin CSV dışa aktarılması 

2020 Kasım 'da öneriler sayfasına filtreler ekledik ([öneriler listesi şimdi filtreler içeriyor](#recommendations-list-now-includes-filters)). Aralık ayında, bu filtreleri genişlettik ([Öneriler sayfasında ortam, önem derecesi ve kullanılabilir yanıtlar için yeni filtreler bulunur](#recommendations-page-has-new-filters-for-environment-severity-and-available-responses)). 

Bu duyuruda CSV dışarı aktarmanın yalnızca filtrelenmiş listede görüntülenen önerileri içermesi **IÇIN CSV 'ye indir** düğmesinin davranışını değiştiriyorsunuz. 

Örneğin, aşağıdaki görüntüde, listenin iki önerilere göre filtrelendiğine bakabilirsiniz. Oluşturulan CSV dosyası, bu iki önerinin etkilediği her kaynak için durum ayrıntılarını içerir.   

:::image type="content" source="media/security-center-managing-and-responding-alerts/export-to-csv-with-filters.png" alt-text="Filtrelenmiş önerileri bir CSV dosyasına dışarı aktarma":::

[Azure Güvenlik Merkezi 'Nde güvenlik önerileri](security-center-recommendations.md)hakkında daha fazla bilgi edinin.


### <a name="not-applicable-resources-now-reported-as-compliant-in-azure-policy-assessments"></a>"Uygulanamaz" kaynakları artık Azure Ilke değerlendirmelerinde "uyumlu" olarak bildirildi

Daha önce, bir öneri için değerlendirilen ve **geçerli olmayan** kaynaklar Azure ilkesinde "uyumlu değil" olarak göründü. Kullanıcı eylemi, durumunu "uyumlu" olarak değiştiremez. Bu değişiklik ile, gelişmiş açıklık için "uyumlu" olarak bildirilir.

Azure Ilkesinde, uyumlu kaynakların sayısının arttıracağı tek etkisi görünür. Azure Güvenlik Merkezi 'nde güvenli puanınızın hiçbir etkisi olmayacaktır.


### <a name="export-weekly-snapshots-of-secure-score-and-regulatory-compliance-data-with-continuous-export-preview"></a>Sürekli dışa aktarma ile güvenli Puanlama ve mevzuat uyumluluk verilerinin haftalık anlık görüntülerini dışarı aktarma (Önizleme)

Güvenli Puanlama ve mevzuat uyumluluk verilerinin haftalık anlık görüntülerini dışarı aktarmak için [sürekli dışa aktarma](continuous-export.md) araçlarına yeni bir önizleme özelliği ekledik.

Sürekli dışarı aktarma tanımladığınızda dışa aktarma sıklığını ayarlayın:

:::image type="content" source="media/release-notes/export-frequency.png" alt-text="Sürekli dışarı aktarmanın sıklığını seçme":::

- **Akış** – bir kaynağın sistem durumu güncelleştirildiğinde değerlendirmeler gerçek zamanlı olarak gönderilir (hiçbir güncelleştirme gerçekleşmezse, hiçbir veri gönderilmez).
- **Anlık görüntüler** : tüm yasal uyumluluk değerlendirmelerinin geçerli durumunun bir anlık görüntüsü her hafta gönderilir (Bu, güvenli puanlar ve mevzuat uyumluluk verilerinin haftalık anlık görüntüleri için bir önizleme özelliğidir).

[Güvenlik Merkezi verilerini sürekli dışa aktarma](continuous-export.md) bölümünde bu özelliğin tüm özellikleri hakkında daha fazla bilgi edinin

## <a name="december-2020"></a>Aralık 2020

Aralık ayında güncelleştirmeler şunlardır:

- [Makinelerde SQL Server 'lar için Azure Defender genel kullanıma sunuldu](#azure-defender-for-sql-servers-on-machines-is-generally-available)
- [Azure SYNAPSE Analytics adanmış SQL havuzu için SQL için Azure Defender desteği genel kullanıma sunuldu](#azure-defender-for-sql-support-for-azure-synapse-analytics-dedicated-sql-pool-is-generally-available)
- [Genel Yöneticiler, artık kendi kiracı düzeyinde izinler verebilir](#global-administrators-can-now-grant-themselves-tenant-level-permissions)
- [İki yeni Azure Defender planı: DNS için Azure Defender ve Kaynak Yöneticisi için Azure Defender (önizlemede)](#two-new-azure-defender-plans-azure-defender-for-dns-and-azure-defender-for-resource-manager-in-preview)
- [Azure portal yeni güvenlik uyarıları sayfası (Önizleme)](#new-security-alerts-page-in-the-azure-portal-preview)
- [Azure SQL veritabanı & SQL yönetilen örneği 'nde yeniden kullanıma alınmış güvenlik merkezi deneyimi](#revitalized-security-center-experience-in-azure-sql-database--sql-managed-instance)
- [Varlık envanter araçları ve filtreleri güncelleştirildi](#asset-inventory-tools-and-filters-updated)
- [SSL sertifikaları isteyen web uygulamaları hakkında öneri artık güvenli puanın bir parçası değil](#recommendation-about-web-apps-requesting-ssl-certificates-no-longer-part-of-secure-score)
- [Öneriler sayfasında ortam, önem derecesi ve kullanılabilir yanıtlar için yeni filtreler bulunur](#recommendations-page-has-new-filters-for-environment-severity-and-available-responses)
- [Sürekli dışa aktarma yeni veri türlerini ve geliştirilmiş deployıfnotexist ilkelerini alır](#continuous-export-gets-new-data-types-and-improved-deployifnotexist-policies)


### <a name="azure-defender-for-sql-servers-on-machines-is-generally-available"></a>Makinelerde SQL Server 'lar için Azure Defender genel kullanıma sunuldu

Azure Güvenlik Merkezi, SQL sunucuları için iki Azure Defender planı sunar:

- Azure **SQL veritabanı sunucuları Için Azure Defender** -Azure Native SQL Server 'larınızı savunma 
- **MAKINELERDE SQL Server 'lar Için Azure Defender** -aynı korumaların karma, çok yüksek ve şirket ıçı ortamlarda SQL Server 'larınıza göre uzatıp

Bu duyuru sayesinde, **SQL Için Azure Defender** artık veritabanlarınızı ve bu verilerin bulundukları yerden korunmasını sağlar.

SQL için Azure Defender, güvenlik açığı değerlendirme özelliklerini içerir. Güvenlik açığı değerlendirme aracı aşağıdaki gelişmiş özellikleri içerir:

- Güvenlik açığı sonuçlarını, gerçek güvenlik sorunlarını temsil eden bir şekilde iyileştirmek için **temel yapılandırma** (yeni!). Temel güvenlik durumlarınızı kurduktan sonra, güvenlik açığı değerlendirmesi aracı yalnızca bu temel durumdan sapmaları raporlar. Taban çizgisiyle eşleşen sonuçlar, sonraki taramaların geçirilmesi olarak değerlendirilir. Bu, sizin ve analistlerinizin önemli olduğunda ilgilenmeniz için odaklanmasına olanak tanır.
- Keşfedilen bulguları ve kaynaklarınızla ilişkilerini *anlamanıza* yardımcı olacak **ayrıntılı kıyaslama bilgileri** .
- Belirlenen riskleri azaltmanıza yardımcı olacak **Düzeltme betikleri** .

[SQL Için Azure Defender](defender-for-sql-introduction.md)hakkında daha fazla bilgi edinin.


### <a name="azure-defender-for-sql-support-for-azure-synapse-analytics-dedicated-sql-pool-is-generally-available"></a>Azure SYNAPSE Analytics adanmış SQL havuzu için SQL için Azure Defender desteği genel kullanıma sunuldu

Azure SYNAPSE Analytics (eski adıyla SQL DW), kurumsal veri depolama ve büyük veri analizlerini birleştiren bir analiz hizmetidir. Adanmış SQL havuzları, Azure SYNAPSE 'ın kurumsal veri ambarı özellikleridir. [Azure SYNAPSE Analytics (eski ADıYLA SQL DW)](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)hakkında daha fazla bilgi edinin.

SQL için Azure Defender, adanmış SQL havuzlarınızı ile korur:

- Tehditleri ve saldırıları algılamak için **Gelişmiş tehdit koruması** 
- Güvenlik yapılandırmalarını belirlemek ve düzeltmek için Güvenlik **açığı değerlendirme özellikleri**

SQL için Azure Defender 'ın Azure SYNAPSE Analytics SQL havuzları desteği, Azure Güvenlik Merkezi 'nde Azure SQL veritabanlarına otomatik olarak eklenir. Azure portal SYNAPSE çalışma alanı sayfanızda yeni bir "SQL için Azure Defender" sekmesi bulacaksınız.

[SQL Için Azure Defender](defender-for-sql-introduction.md)hakkında daha fazla bilgi edinin.


### <a name="global-administrators-can-now-grant-themselves-tenant-level-permissions"></a>Genel Yöneticiler, artık kendi kiracı düzeyinde izinler verebilir

**Genel yönetici** Azure Active Directory rolüne sahip bir kullanıcının kiracı genelinde sorumlulukları olabilir, ancak Azure Güvenlik Merkezi 'nde kuruluş genelinde bilgileri görüntülemek için Azure izinlerinin olmaması gerekir. 

Kendi kiracı düzeyi izinlerini atamak için, [kiracı genelindeki izinleri kendinize verme](security-center-management-groups.md#grant-tenant-wide-permissions-to-yourself)bölümündeki yönergeleri izleyin.


### <a name="two-new-azure-defender-plans-azure-defender-for-dns-and-azure-defender-for-resource-manager-in-preview"></a>İki yeni Azure Defender planı: DNS için Azure Defender ve Kaynak Yöneticisi için Azure Defender (önizlemede)

Azure ortamınız için iki yeni bulut Yerel enine koruma özelliği ekledik.

Bu yeni korumalar tehdit aktörlerini saldırılara karşı dayanıklılığını büyük ölçüde geliştirir ve Azure Defender tarafından korunan Azure kaynakları sayısını önemli ölçüde artırır.

- **Kaynak Yöneticisi Için Azure Defender** -kuruluşunuzda gerçekleştirilen tüm kaynak yönetimi işlemlerini otomatik olarak izler. Daha fazla bilgi için bkz.
    - [Kaynak Yöneticisi için Azure Defender 'a giriş](defender-for-resource-manager-introduction.md)
    - [Resource Manager için Azure Defender uyarılarına yanıt verme](defender-for-resource-manager-usage.md)
    - [Kaynak Yöneticisi için Azure Defender tarafından sunulan uyarıların listesi](alerts-reference.md#alerts-resourcemanager)

- **DNS Için Azure Defender** -Azure kaynaklarınızdaki tüm DNS sorgularını sürekli izler. Daha fazla bilgi için bkz.
    - [DNS için Azure Defender 'a giriş](defender-for-dns-introduction.md)
    - [DNS için Azure Defender uyarılarına yanıt verme](defender-for-dns-usage.md)
    - [DNS için Azure Defender tarafından sunulan uyarıların listesi](alerts-reference.md#alerts-dns)


### <a name="new-security-alerts-page-in-the-azure-portal-preview"></a>Azure portal yeni güvenlik uyarıları sayfası (Önizleme)

Azure Güvenlik Merkezi 'nin güvenlik uyarıları sayfası, şunları sağlayacak şekilde yeniden tasarlandı:

- **Uyarılar Için geliştirilmiş önceliklendirme deneyimi** -uyarıları daha kolay hale getirmeye ve en ilgili tehditlere odaklanmaya yardımcı olma, listede özelleştirilebilir filtreler ve gruplandırma seçenekleri de bulunur
- **Uyarılar listesinde,** MITRE ATT&ACK Tactika gibi daha fazla bilgi
- **Örnek uyarılar oluşturma düğmesi** -Azure Defender özelliklerini değerlendirmek ve uyarı yapılandırmanızı sınamak IÇIN (SIEM tümleştirmesi, e-posta bildirimleri ve iş akışı tahminleri için), tüm Azure Defender planlarından örnek uyarılar oluşturabilirsiniz
- **Azure Sentinel 'in olay deneyimiyle hizalaması** -her iki ürünü de kullanan müşteriler için, aralarında geçiş yapmak artık daha basit bir deneyimdir ve bunlardan birini öğrenmenin kolay bir deneyimi vardır.
- Büyük uyarı listeleri için **daha iyi performans**
- Uyarı listesi aracılığıyla **klavye gezintisi**
- **Azure Kaynak grafının uyarıları** -tüm kaynaklarınızın kusto benzeri API 'Si olan Azure Kaynak Grafında uyarıları sorgulayabilirsiniz. Bu, kendi uyarı panolarınızı oluşturuyorsanız de kullanışlıdır. [Azure Kaynak Grafiği hakkında daha fazla bilgi edinin](../governance/resource-graph/index.yml).

Yeni deneyimle erişmek için, güvenlik uyarıları sayfasının en üstündeki başlığından ' Şimdi deneyin ' bağlantısını kullanın.

:::image type="content" source="media/security-center-managing-and-responding-alerts/preview-alerts-experience-banner.png" alt-text="Yeni önizleme uyarıları deneyimine bağlantı içeren başlık":::

Yeni uyarılar deneyiminden örnek uyarılar oluşturmak için bkz. [örnek Azure Defender uyarıları](security-center-alert-validation.md#generate-sample-azure-defender-alerts)oluşturma.


### <a name="revitalized-security-center-experience-in-azure-sql-database--sql-managed-instance"></a>Azure SQL veritabanı & SQL yönetilen örneği 'nde yeniden kullanıma alınmış güvenlik merkezi deneyimi 

SQL 'deki güvenlik merkezi deneyimi, aşağıdaki güvenlik merkezi ve SQL için Azure Defender özelliklerine erişim sağlar:

- **Güvenlik önerileri** : Güvenlik Merkezi, olası güvenlik yapılandırmalarını belirlemek üzere tüm bağlı Azure kaynaklarının güvenlik durumunu düzenli aralıklarla analiz eder. Daha sonra bu güvenlik açıklarını düzeltme ve kuruluşların güvenlik duruşunu geliştirme hakkında öneriler sağlar.
- **Güvenlik uyarıları** : SQL ekleme, deneme yanılma saldırıları ve ayrıcalık kötüye kullanımı gibi tehditler IÇIN Azure SQL etkinliklerini sürekli olarak izleyen bir algılama hizmeti. Bu hizmet, güvenlik merkezi 'nde ayrıntılı ve eyleme dayalı güvenlik uyarılarını tetikler ve Microsoft 'un Azure-Native SıEM çözümü olan Azure Sentinel ile araştırmalar devam ettirme seçeneklerini sunar.
- **Bulmalar** : Azure SQL yapılandırmasını sürekli olarak izleyen ve güvenlik açıklarını düzeltmeye yardımcı olan bir güvenlik açığı değerlendirme hizmeti. Değerlendirme taramaları, ayrıntılı güvenlik bulguları ile birlikte Azure SQL güvenlik durumlarına genel bir bakış sağlar.     

:::image type="content" source="media/release-notes/azure-security-center-experience-in-sql.png" alt-text="Azure Güvenlik Merkezi 'nin SQL için güvenlik özellikleri Azure SQL içinden kullanılabilir":::


### <a name="asset-inventory-tools-and-filters-updated"></a>Varlık envanter araçları ve filtreleri güncelleştirildi

Azure Güvenlik Merkezi 'ndeki envanter sayfası aşağıdaki değişikliklerle yenilendi:

- Araç çubuğuna **Kılavuzlar ve geri bildirimler** eklendi. Bu, ilgili bilgi ve araçların bağlantılarını içeren bir bölme açar. 
- Kaynaklarınız için kullanılabilen varsayılan filtrelere **Abonelik filtresi** eklendi.
- Geçerli filtre seçeneklerini bir Azure Kaynak Grafiği sorgusu olarak **açmak için sorgu bağlantısını açın** (eski adıyla "kaynak grafik Gezgininde görünüm" olarak adlandırılır).
- Her filtrenin **işleç seçenekleri** . Artık ' = ' dışında daha fazla mantıksal işleç arasından seçim yapabilirsiniz. Örneğin, başlıkları ' Encrypt ' dizesini içeren etkin önerilere sahip tüm kaynakları bulmak isteyebilirsiniz. 

    :::image type="content" source="media/release-notes/inventory-filter-operators.png" alt-text="Varlık envanterinin filtrelerinde operatör seçeneğinin denetimleri":::

Envanter hakkında daha fazla bilgi edinmek için [varlık envanteriyle kaynaklarınızı bulun ve yönetin](asset-inventory.md).


### <a name="recommendation-about-web-apps-requesting-ssl-certificates-no-longer-part-of-secure-score"></a>SSL sertifikaları isteyen web uygulamaları hakkında öneri artık güvenli puanın bir parçası değil

"Web Apps tüm gelen istekler için bir SSL sertifikası istemelidir" önerisi güvenlik denetiminden, en **iyi güvenlik uygulamalarını** (yani, hiçbir noktaya değer olmaksızın) uygulama **erişimi ve izinleri yönetme** (en fazla 4 Pts değeri). 

Bir Web uygulamasının bir sertifikayı talep etmelerini sağlamak, bunu kesinlikle daha güvenli hale getirir. Ancak, herkese açık Web uygulamaları için önemli değildir. Sitenize HTTPS değil, HTTP üzerinden eriştiğinizde hiçbir istemci sertifikası almazsınız. Bu nedenle, uygulamanız için istemci sertifikaları gerekiyorsa, HTTP üzerinden uygulamanıza yönelik isteklere izin vermeniz gerekir. [Azure App Service IÇIN TLS karşılıklı kimlik doğrulamasını yapılandırma](../app-service/app-service-web-configure-tls-mutual-auth.md)bölümünde daha fazla bilgi edinin.

Bu değişiklik ile, öneri artık puanınızı etkilemeyecek önerilen en iyi uygulamadır. 

[Güvenlik denetimlerinde ve bunların önerilerinde](secure-score-security-controls.md#security-controls-and-their-recommendations)her güvenlik denetiminde hangi önerilerin olduğunu öğrenin.


### <a name="recommendations-page-has-new-filters-for-environment-severity-and-available-responses"></a>Öneriler sayfasında ortam, önem derecesi ve kullanılabilir yanıtlar için yeni filtreler bulunur

Azure Güvenlik Merkezi, tüm bağlı kaynakları izler ve güvenlik önerileri oluşturur. Karma bulut durunuzu güçlendirin ve kuruluşunuz, sektör ve ülkeniz ile ilgili ilkeler ve standartlarla uyumluluğu izleyin.

Güvenlik Merkezi, kapsamını ve özelliklerini genişletmeye devam ettiğinden, güvenlik önerilerinin listesi her ay büyüyordur. Örneğin, [Azure Güvenlik kıyaslamasının kapsamını artırmak için bkz. 29 Preview önerileri eklendi](#29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark).

Büyüyen liste ile, en fazla ilgilendiğiniz olanları bulmak için önerileri filtrelemeniz gerekir. Kasım ayında öneriler sayfasına filtreler ekledik (bkz. [öneriler listesi şimdi filtreler içeriyor](#recommendations-list-now-includes-filters)).

Bu ay eklenen filtreler şunlara göre öneriler listesini iyileştirmek için seçenekler sağlar:

- **Ortam** -AWS, GCP veya Azure kaynaklarınız (veya herhangi bir bileşim) için öneriler görüntüleyin
- **Önem derecesi** -Güvenlik Merkezi tarafından ayarlanan önem derecesine göre önerileri görüntüleme
- **Yanıt eylemleri** -Güvenlik Merkezi yanıt seçeneklerinin kullanılabilirliğine göre önerileri görüntüleyin: hızlı düzeltme, reddetme ve zorlama

    > [!TIP]
    > Yanıt eylemleri filtresi, **kullanılabilir hızlı düzeltilmesi (Evet/Hayır)** filtresinin yerini alır. 
    > 
    > Bu yanıt seçeneklerinin her biri hakkında daha fazla bilgi edinin:
    > - [Hızlı düzeltme düzeltme](security-center-remediate-recommendations.md#quick-fix-remediation)
    > - [Zorlama/Reddetme önerileriyle yanlış yapılandırmaları önleme](prevent-misconfigurations.md)

:::image type="content" source="./media/release-notes/added-recommendations-filters.png" alt-text="Güvenlik denetimine göre gruplanmış öneriler" lightbox="./media/release-notes/added-recommendations-filters.png":::

### <a name="continuous-export-gets-new-data-types-and-improved-deployifnotexist-policies"></a>Sürekli dışa aktarma yeni veri türlerini ve geliştirilmiş deployıfnotexist ilkelerini alır

Azure Güvenlik Merkezi 'nin sürekli dışa aktarma araçları, ortamınızdaki diğer izleme araçlarıyla kullanılmak üzere güvenlik merkezi 'nin önerilerini ve uyarılarını dışa aktarmanız sağlar.

Sürekli dışarı aktarma, nelerin dışa aktarılacağını ve nerede gidebileceklerini tamamen özelleştirmenizi sağlar. Tüm ayrıntılar için bkz.  [Güvenlik Merkezi verilerini sürekli dışa aktarma](continuous-export.md).

Bu araçlar geliştirilmiştir ve aşağıdaki yollarla genişletilmiştir:

- **Sürekli dışa aktarma 'nın deployıfnotexist ilkeleri geliştirildi**. İlkeler şimdi:

    - **Yapılandırmanın etkinleştirilip etkinleştirilmediğini denetleyin.** Değilse, ilke uyumlu değil olarak görünür ve uyumlu bir kaynak oluşturur. Azure Ilke şablonları hakkında daha fazla bilgi edinmek için [sürekli dışarı aktarma ayarlama](continuous-export.md#set-up-a-continuous-export)bölümünde "Azure ilke sekmesi ile ölçeklendirin ölçeğinde dağıtım".

    - **Güvenlik bulgularını dışarı aktarma desteği.** Azure Ilke şablonlarını kullanırken, sürekli dışarı aktarmayı bulguları içerecek şekilde yapılandırabilirsiniz. Bu, güvenlik açığı değerlendirme tarayıcılarından bulguları veya ' Ana ' öneri "sistem güncelleştirmeleri" makinelerinizde yüklü olmalıdır "gibi ' Sub ' önerilerine sahip olan önerileri dışarı aktarırken geçerlidir.
    
    - **Güvenli puanı verileri dışarı aktarma desteği.**

- **Mevzuat uyumluluk değerlendirmesi verileri eklendi (önizlemede).** Artık bir Log Analytics çalışma alanına veya Olay Hub 'ına tüm özel girişimler dahil olmak üzere yasal uyumluluk değerlendirmelerine yönelik güncelleştirmeleri sürekli dışa aktarabilirsiniz. Bu özellik Ulusal/bağımsız bulutlarda kullanılamaz.

    :::image type="content" source="media/release-notes/continuous-export-regulatory-compliance-option.png" alt-text="Sürekli dışa aktarma verilerinize mevzuat uyumlu değerlendirme bilgilerini ekleme seçenekleri.":::


## <a name="november-2020"></a>Kasım 2020

Kasım 'daki güncelleştirmeler şunlardır:

- [Azure Güvenlik kıyaslamasının kapsamını artırmak için 29 önizleme önerileri eklenmiştir](#29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark)
- [NıST SP 800 171 R2, güvenlik merkezi 'nin mevzuat uyumluluk panosuna eklendi](#nist-sp-800-171-r2-added-to-security-centers-regulatory-compliance-dashboard)
- [Öneriler listesi şimdi filtreler içeriyor](#recommendations-list-now-includes-filters)
- [Otomatik sağlama deneyimi geliştirildi ve genişletildi](#auto-provisioning-experience-improved-and-expanded)
- [Güvenli puan, sürekli dışarı aktarma (Önizleme) ile kullanılabilir](#secure-score-is-now-available-in-continuous-export-preview)
- ["Makinelerinizde sistem güncelleştirmeleri yüklenmelidir" önerisi artık alt öneriler içeriyor](#system-updates-should-be-installed-on-your-machines-recommendation-now-includes-subrecommendations)
- [Azure portal ilke yönetimi sayfasında artık varsayılan ilke atamalarının durumu gösterilmektedir](#policy-management-page-in-the-azure-portal-now-shows-status-of-default-policy-assignments)

### <a name="29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark"></a>Azure Güvenlik kıyaslamasının kapsamını artırmak için 29 önizleme önerileri eklenmiştir

Azure Güvenlik kıyaslaması, yaygın uyumluluk çerçevelerine dayalı olarak güvenlik ve uyumluluk en iyi uygulamaları için Microsoft tarafından yazılan, Azure 'a özgü bir dizi kılavuzlardır. [Azure Güvenlik Karşılaştırması hakkında daha fazla bilgi edinin](../security/benchmarks/introduction.md).

Bu kıyaslama kapsamını artırmak için Güvenlik Merkezi 'ne aşağıdaki 29 önizleme önerileri eklenmiştir.

Önizleme önerileri bir kaynağı sağlıksız bir şekilde işlemez ve güvenli puanınızın hesaplamalarına dahil değildir. Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunmak için bunları mümkün olduğunda düzeltin. [Azure Güvenlik Merkezi 'ndeki önerileri düzeltin](security-center-remediate-recommendations.md)bölümünde bu önerilere yanıt verme hakkında daha fazla bilgi edinin.

| Güvenlik denetimi                     | Yeni öneriler                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yoldaki verileri şifreleme              | -PostgreSQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir<br>-MySQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir<br>-TLS API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-TLS, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-TLS, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-FTPS API uygulamanızda gerekli olmalıdır<br>-FTPS, işlev uygulamanızda gerekli olmalıdır<br>-FTPS Web uygulamanızda gerekli olmalıdır                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Erişimi ve izinleri yönetme        | -Web uygulamaları tüm gelen istekler için bir SSL sertifikası istemelidir<br>-Yönetilen kimlik API uygulamanızda kullanılmalıdır<br>-Yönetilen kimlik, işlev uygulamanızda kullanılmalıdır<br>-Yönetilen kimliğin Web uygulamanızda kullanılması gerekir                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Yetkisiz ağ erişimini kısıtla | -PostgreSQL sunucuları için özel uç noktanın etkinleştirilmesi gerekir<br>-Özel uç noktanın MariaDB sunucuları için etkinleştirilmesi gerekir<br>-MySQL sunucuları için özel uç noktanın etkinleştirilmesi gerekir                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Denetim ve günlüğe kaydetmeyi etkinleştirme          | -Uygulama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| En iyi güvenlik uygulamalarını uygulayın    | -Azure Backup sanal makineler için etkinleştirilmelidir<br>-Coğrafi olarak yedekli yedekleme, MariaDB için Azure veritabanı 'nda etkinleştirilmelidir<br>-MySQL için Azure veritabanı için coğrafi olarak yedekli yedeklemenin etkinleştirilmesi gerekir<br>-PostgreSQL için Azure veritabanı için coğrafi olarak yedekli yedeklemenin etkinleştirilmesi gerekir<br>-PHP, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-PHP, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Java, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Java, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Java, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Python, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Python, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-Python, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>-SQL Server 'lar için denetim bekletme en az 90 gün olarak ayarlanmalıdır |
|                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

İlgili bağlantılar:

- [Azure Güvenlik kıyaslaması hakkında daha fazla bilgi](../security/benchmarks/introduction.md)
- [Azure API Apps hakkında daha fazla bilgi](../app-service/app-service-web-tutorial-rest-api.md)
- [Azure işlev uygulamaları hakkında daha fazla bilgi edinin](../azure-functions/functions-overview.md)
- [Azure Web Apps hakkında daha fazla bilgi](../app-service/overview.md)
- [MariaDB için Azure veritabanı hakkında daha fazla bilgi edinin](../mariadb/overview.md)
- [MySQL için Azure veritabanı hakkında daha fazla bilgi edinin](../mysql/overview.md)
- [PostgreSQL için Azure veritabanı hakkında daha fazla bilgi edinin](../postgresql/overview.md)


### <a name="nist-sp-800-171-r2-added-to-security-centers-regulatory-compliance-dashboard"></a>NıST SP 800 171 R2, güvenlik merkezi 'nin mevzuat uyumluluk panosuna eklendi

NıST SP 800-171 R2 Standard artık, Azure Güvenlik Merkezi 'nin mevzuat uyumluluk panosu ile birlikte kullanılmak üzere yerleşik bir girişim olarak sunulmaktadır. Denetimlerle ilgili eşlemeler, [NıST SP 800-171 R2 mevzuatı uyumluluğu yerleşik girişiminin ayrıntıları](../governance/policy/samples/nist-sp-800-171-r2.md)bölümünde açıklanmaktadır. 

Standart aboneliğinizi aboneliklerinize uygulamak ve uyumluluk durumunuzu sürekli olarak izlemek için, [mevzuat uyumluluk panonuzdaki standartlar kümesini özelleştirme](update-regulatory-compliance-packages.md)bölümündeki yönergeleri kullanın.

:::image type="content" source="media/release-notes/nist-sp-800-171-r2-standard.png" alt-text="Güvenlik Merkezi 'nin mevzuat uyumluluk panosundaki NıST SP 800 171 R2 Standard":::

Bu uyumluluk standardı hakkında daha fazla bilgi için bkz. [NıST SP 800-171 R2](https://csrc.nist.gov/publications/detail/sp/800-171/rev-2/final).


### <a name="recommendations-list-now-includes-filters"></a>Öneriler listesi şimdi filtreler içeriyor

Artık güvenlik önerileri listesini bir dizi ölçüte göre filtreleyebilirsiniz. Aşağıdaki örnekte, öneriler listesi şu önerileri gösterecek şekilde filtrelenmiştir:

- **genel kullanıma sunulmuştur** (yani Önizleme)
- **depolama hesapları** içindir
- **hızlı düzeltme** düzeltmesini destekleme

:::image type="content" source="media/release-notes/recommendations-filters.png" alt-text="Öneriler listesi için filtreler":::


### <a name="auto-provisioning-experience-improved-and-expanded"></a>Otomatik sağlama deneyimi geliştirildi ve genişletildi

Otomatik sağlama özelliği, güvenlik merkezi 'nin korumalarının avantajlarından faydalanabilmesi için gerekli uzantıları yeni ve mevcut Azure VM 'lerine yükleyerek yönetim yükünü azaltmaya yardımcı olur. 

Azure Güvenlik Merkezi arttıkça, daha fazla uzantı geliştirilmiştir ve Güvenlik Merkezi, kaynak türlerinin daha büyük bir listesini izleyebilir. Otomatik sağlama araçları artık Azure Ilkesinin yeteneklerini kullanarak diğer uzantıları ve kaynak türlerini destekleyecek şekilde genişletilmiştir.

Artık otomatik sağlamayı yapılandırabilirsiniz:

- Log Analytics aracısı
- Yeni Kubernetes için Azure Ilke eklentisi
- Yeni Microsoft bağımlılık Aracısı

[Azure Güvenlik Merkezi 'Nden otomatik sağlama aracıları ve uzantıları](security-center-enable-data-collection.md)hakkında daha fazla bilgi edinin.


### <a name="secure-score-is-now-available-in-continuous-export-preview"></a>Güvenli puan, sürekli dışarı aktarma (Önizleme) ile kullanılabilir

Güvenli puanı sürekli dışa aktarma ile Azure Event Hubs veya bir Log Analytics çalışma alanına gerçek zamanlı olarak yaptığınız değişiklikleri gerçek zamanlı olarak akışla aktarabilirsiniz. Bu özelliği şu şekilde kullanın:

- Dinamik raporlarla zaman içindeki güvenli puanınızı izleyin
- güvenli Puanlama verilerini Azure Sentinel 'e (veya başka bir SıEM) aktarma
- Bu verileri, kuruluşunuzda güvenli puanı izlemek için zaten kullanmakta olabileceğiniz işlemlerle tümleştirin

[Güvenlik Merkezi verilerinin sürekli olarak nasıl dışarı aktarılacağı](continuous-export.md)hakkında daha fazla bilgi edinin.


### <a name="system-updates-should-be-installed-on-your-machines-recommendation-now-includes-subrecommendations"></a>"Makinelerinizde sistem güncelleştirmeleri yüklenmelidir" önerisi artık alt öneriler içeriyor

**Sistem güncelleştirmelerinin, makinelerinizde yüklü olması** önerilir. Yeni sürüm, eksik olan her güncelleştirme için alt öneriler içerir ve aşağıdaki geliştirmeleri getirir:

- Azure portal Azure Güvenlik Merkezi sayfalarında yeniden tasarlanan bir deneyim. **Makinelerinizde sistem güncelleştirmeleri** için öneri ayrıntıları sayfası, aşağıda gösterildiği gibi bulguları listesini içerir. Tek bir bulma seçtiğinizde, Ayrıntılar bölmesi düzeltme bilgilerinin bağlantısını ve etkilenen kaynakların listesini açar.

    :::image type="content" source="./media/upcoming-changes/system-updates-should-be-installed-subassessment.png" alt-text="Güncelleştirilmiş öneri için Portal deneyimindeki alt önerilerden birini açma":::

- Azure Kaynak grafiğinden (ARG) öneri için verileri zenginleştirir. Bağımsız değişken, verimli kaynak araştırması sağlamak için tasarlanan bir Azure hizmetidir. Ortamınızı etkin bir şekilde yönetebilmeniz için, belirli bir abonelik kümesi genelinde ölçeği sorgulamak için bağımsız değişkeni kullanabilirsiniz. 

    Azure Güvenlik Merkezi için bağımsız değişken ve [kusto sorgu dili (KQL)](/azure/data-explorer/kusto/query/) kullanarak çok çeşitli güvenlik sonrası verileri sorgulayabilirsiniz.

    Daha önce, bu öneriyi bağımsız değişken ' de sorguladıysanız, yalnızca bir makinenin bir makinede düzeltilme ihtiyacı vardır. Aşağıdaki gelişmiş sürümü sorgusu, tüm eksik sistem güncelleştirmelerini makineye göre gruplanmış olarak döndürür.

    ```kusto
    securityresources
    | where type =~ "microsoft.security/assessments/subassessments"
    | where extract(@"(?i)providers/Microsoft.Security/assessments/([^/]*)", 1, id) == "4ab6e3c5-74dd-8b35-9ab9-f61b30875b27"
    | where properties.status.code == "Unhealthy"
    ```

### <a name="policy-management-page-in-the-azure-portal-now-shows-status-of-default-policy-assignments"></a>Azure portal ilke yönetimi sayfasında artık varsayılan ilke atamalarının durumu gösterilmektedir

Artık abonelikleriniz için, Azure portal Güvenlik Merkezi 'nin **güvenlik ilkesi** sayfasında, aboneliklerinizde varsayılan güvenlik merkezi ilkesi atanmış olup olmadığını görebilirsiniz.

:::image type="content" source="media/release-notes/policy-assignment-info-per-subscription.png" alt-text="Azure Güvenlik Merkezi 'nin varsayılan ilke atamalarını gösteren ilke yönetimi sayfası":::

## <a name="october-2020"></a>Ekim 2020

Ekim 'deki güncelleştirmeler şunlardır:
- [Şirket içi ve çoklu bulut makinelerinde güvenlik açığı değerlendirmesi (Önizleme)](#vulnerability-assessment-for-on-premise-and-multi-cloud-machines-preview)
- [Azure Güvenlik Duvarı önerisi eklendi (Önizleme)](#azure-firewall-recommendation-added-preview)
- [Yetkili IP aralıklarının, hızlı düzeltmeyle güncelleştirilmiş Kubernetes Hizmetleri önerisi üzerinde tanımlanması gerekir](#authorized-ip-ranges-should-be-defined-on-kubernetes-services-recommendation-updated-with-quick-fix)
- [Mevzuat uyumluluk panosu artık standartları kaldırma seçeneğini içermektedir](#regulatory-compliance-dashboard-now-includes-option-to-remove-standards)
- [Microsoft. Security/Securitydurumlarının tablosu Azure Kaynak grafiğinden kaldırıldı (ARG)](#microsoftsecuritysecuritystatuses-table-removed-from-azure-resource-graph-arg)

### <a name="vulnerability-assessment-for-on-premise-and-multi-cloud-machines-preview"></a>Şirket içi ve çoklu bulut makinelerinde güvenlik açığı değerlendirmesi (Önizleme)

[Sunucular Için Azure Defender 'ın](defender-for-servers-introduction.md)tümleşik güvenlik açığı değerlendirmesi tarayıcısı (Qualys tarafından desteklenir) artık Azure Arc etkin sunucularını tarar.

Azure dışı makinelerinizde Azure yayı 'yi etkinleştirdiyseniz, Güvenlik Merkezi, tümleşik güvenlik açığı tarayıcısını bunlara el ile ve ölçekli olarak dağıtmayı sağlar.

Bu güncelleştirmeyle, Azure Defender 'ın tüm Azure ve Azure dışı varlıklarınızda güvenlik açığı yönetimi programınızı birleştirmek için **Azure Defender** 'ın gücünü açığa çıkarın.

Ana yetenekler:

- Azure Arc makinelerinde VA (güvenlik açığı değerlendirmesi) tarayıcı sağlama durumunu izleme
- Tümleşik VA aracısını korumasız Windows ve Linux Azure yay makinelerine sağlama (el ile ve ölçekli)
- Dağıtılan aracılardan algılanan güvenlik açıklarını alma ve çözümleme (el ile ve ölçekli)
- Azure sanal makineleri ve Azure yay makineleri için Birleşik deneyim

[Tümleşik güvenlik açığı tarayıcısını karma makinelerinize dağıtma hakkında daha fazla bilgi edinin](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines).

[Azure Arc etkin sunucuları hakkında daha fazla bilgi edinin](../azure-arc/servers/index.yml).


### <a name="azure-firewall-recommendation-added-preview"></a>Azure Güvenlik Duvarı önerisi eklendi (Önizleme)

Tüm sanal ağlarınızı Azure Güvenlik Duvarı ile korumak için yeni bir öneri eklenmiştir.

Bu öneri, **sanal ağların Azure Güvenlik Duvarı tarafından korunması gerekir** ve Azure Güvenlik Duvarı 'nı kullanarak sanal ağlarınıza erişimi kısıtlayıp olası tehditleri önlemenizi önerir.

[Azure Güvenlik Duvarı](https://azure.microsoft.com/services/azure-firewall/)hakkında daha fazla bilgi edinin.


### <a name="authorized-ip-ranges-should-be-defined-on-kubernetes-services-recommendation-updated-with-quick-fix"></a>Yetkili IP aralıklarının, hızlı düzeltmeyle güncelleştirilmiş Kubernetes Hizmetleri önerisi üzerinde tanımlanması gerekir

Öneri **yetkılı IP aralıklarının artık, Kubernetes hizmetlerinde tanımlanması gerekir** . Şimdi hızlı bir çözüm seçeneği vardır.

Bu öneri ve diğer tüm güvenlik merkezi önerileri hakkında daha fazla bilgi için bkz. [güvenlik önerileri-bir başvuru kılavuzu](recommendations-reference.md).

:::image type="content" source="./media/release-notes/authorized-ip-ranges-recommendation.png" alt-text="Yetkili IP aralıkları hızlı düzelme seçeneği ile Kubernetes Hizmetleri önerisi üzerinde tanımlanmalıdır":::


### <a name="regulatory-compliance-dashboard-now-includes-option-to-remove-standards"></a>Mevzuat uyumluluk panosu artık standartları kaldırma seçeneğini içermektedir

Güvenlik Merkezi 'nin yasal uyumluluk panosu, uyumlu uyumluluk denetimleri ve gereksinimlerini nasıl karşıladığınızı temel alarak uyumlulukla ilgili öngörüler sağlar.

Pano, varsayılan bir mevzuat standartları kümesi içerir. Sağlanan standartlardan herhangi biri kuruluşunuzla ilgili değilse, bir abonelik için kullanıcı arabiriminden kaldırma işlemi artık basit bir işlemdir. Standartlar yalnızca *abonelik* düzeyinde kaldırılabilir; Yönetim grubu kapsamı değil.

[Panonuzdan bir standart kaldırma](update-regulatory-compliance-packages.md#removing-a-standard-from-your-dashboard)hakkında daha fazla bilgi edinin.


### <a name="microsoftsecuritysecuritystatuses-table-removed-from-azure-resource-graph-arg"></a>Microsoft. Security/Securitydurumlarının tablosu Azure Kaynak grafiğinden kaldırıldı (ARG)

Azure Kaynak Grafiği, ortamınızı etkili bir şekilde yönetebilmeniz için belirli bir abonelik kümesi genelinde ölçeği sorgulama özelliği ile verimli kaynak araştırması sağlamak üzere tasarlanan Azure hizmetidir. 

Azure Güvenlik Merkezi için bağımsız değişken ve [kusto sorgu dili (KQL)](/azure/data-explorer/kusto/query/) kullanarak çok çeşitli güvenlik sonrası verileri sorgulayabilirsiniz. Örneğin:

- Varlık envanteri kullanır (bağımsız değişken)
- [Multi-Factor Authentication (MFA) etkin olmayan hesapların nasıl tanımlanacağına](security-center-identity-access.md#identify-accounts-without-multi-factor-authentication-mfa-enabled) yönelik örnek bir bağımsız değişken sorgusu belgeliyoruz

Bağımsız değişken içinde, sorgularda kullanabileceğiniz veri tabloları vardır.

:::image type="content" source="./media/release-notes/azure-resource-graph-tables.png" alt-text="Azure Kaynak Grafiği Gezgini ve kullanılabilir tablolar":::

> [!TIP]
> ARG belgesi, [Azure Kaynak Grafiği tablosu ve kaynak türü başvurusu](../governance/resource-graph/reference/supported-tables-resources.md)'ndaki tüm kullanılabilir tabloları listeler.

Bu güncelleştirmeden, **Microsoft. Security/Securitydurumlarının** tablosu kaldırılmıştır. Securitydurumlarının API 'SI hala kullanılabilir.

Veri değiştirme, Microsoft. Security/değerlendirmeleri tablosu tarafından kullanılabilir.

Microsoft. Security/Securitydurumlardan ve Microsoft. Security/değerlendirmeleri arasındaki önemli fark, ilki değerlendirmelerin toplanmasının, saniyenin her biri için tek bir kayıt tuttuğunda olduğu sürece.

Örneğin, Microsoft. Security/Securitydurumlar, iki Polimanlar dizisiyle bir sonuç döndürür:

```
{
id: "/subscriptions/449bcidd-3470-4804-ab56-2752595 felab/resourceGroups/mico-rg/providers/Microsoft.Network/virtualNetworks/mico-rg-vnet/providers/Microsoft.Security/securityStatuses/mico-rg-vnet",
name: "mico-rg-vnet",
type: "Microsoft.Security/securityStatuses",
properties:  {
    policyAssessments: [
        {assessmentKey: "e3deicce-f4dd-3b34-e496-8b5381bazd7e", category: "Networking", policyName: "Azure DDOS Protection Standard should be enabled",...},
        {assessmentKey: "sefac66a-1ec5-b063-a824-eb28671dc527", category: "Compute", policyName: "",...}
    ],
    securitystateByCategory: [{category: "Networking", securityState: "None" }, {category: "Compute",...],
    name: "GenericResourceHealthProperties",
    type: "VirtualNetwork",
    securitystate: "High"
}
```
Ancak, Microsoft. Security/değerlendirmeleri, bu tür bir ilke değerlendirmesi için aşağıdaki gibi bir kayıt tutacaktır:

```
{
type: "Microsoft.Security/assessments",
id:  "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourceGroups/mico-rg/providers/Microsoft. Network/virtualNetworks/mico-rg-vnet/providers/Microsoft.Security/assessments/e3delcce-f4dd-3b34-e496-8b5381ba2d70",
name: "e3deicce-f4dd-3b34-e496-8b5381ba2d70",
properties:  {
    resourceDetails: {Source: "Azure", Id: "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourceGroups/mico-rg/providers/Microsoft.Network/virtualNetworks/mico-rg-vnet"...},
    displayName: "Azure DDOS Protection Standard should be enabled",
    status: (code: "NotApplicable", cause: "VnetHasNOAppGateways", description: "There are no Application Gateway resources attached to this Virtual Network"...}
}

{
type: "Microsoft.Security/assessments",
id:  "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourcegroups/mico-rg/providers/microsoft.network/virtualnetworks/mico-rg-vnet/providers/Microsoft.Security/assessments/80fac66a-1ec5-be63-a824-eb28671dc527",
name: "8efac66a-1ec5-be63-a824-eb28671dc527",
properties: {
    resourceDetails: (Source: "Azure", Id: "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourcegroups/mico-rg/providers/microsoft.network/virtualnetworks/mico-rg-vnet"...),
    displayName: "Audit diagnostic setting",
    status:  {code: "Unhealthy"}
}
```

**Artık değerlendirme tablosunu kullanmak için Securitydurumlar kullanarak var olan bir bağımsız değişken sorgusunun dönüştürülmesiyle bir örnek:**

Securitydurumlarının başvurduğu sorgu:

```kusto
SecurityResources 
| where type == 'microsoft.security/securitystatuses' and properties.type == 'virtualMachine'
| where name in ({vmnames}) 
| project name, resourceGroup, policyAssesments = properties.policyAssessments, resourceRegion = location, id, resourceDetails = properties.resourceDetails
```

Değerlendirme tablosu için değiştirme sorgusu:

```kusto
securityresources
| where type == "microsoft.security/assessments" and id contains "virtualMachine"
| extend resourceName = extract(@"(?i)/([^/]*)/providers/Microsoft.Security/assessments", 1, id)
| extend source = tostring(properties.resourceDetails.Source)
| extend resourceId = trim(" ", tolower(tostring(case(source =~ "azure", properties.resourceDetails.Id,
source =~ "aws", properties.additionalData.AzureResourceId,
source =~ "gcp", properties.additionalData.AzureResourceId,
extract("^(.+)/providers/Microsoft.Security/assessments/.+$",1,id)))))
| extend resourceGroup = tolower(tostring(split(resourceId, "/")[4]))
| where resourceName in ({vmnames}) 
| project resourceName, resourceGroup, resourceRegion = location, id, resourceDetails = properties.additionalData
```

Aşağıdaki bağlantılardan daha fazla bilgi edinin:
- [Azure Kaynak Grafı Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)
- [Kusto Sorgu Dili (KQL)](/azure/data-explorer/kusto/query/)


## <a name="september-2020"></a>Eylül 2020

Eylül ayında güncelleştirmeler şunları içerir:
- [Güvenlik Merkezi yeni bir görünüm alır!](#security-center-gets-a-new-look)
- [Azure Defender yayımlandı](#azure-defender-released)
- [Key Vault için Azure Defender genel kullanıma sunuldu](#azure-defender-for-key-vault-is-generally-available)
- [Dosyalar ve ADLS 2. için depolama koruması için Azure Defender, genel kullanıma sunuldu](#azure-defender-for-storage-protection-for-files-and-adls-gen2-is-generally-available)
- [Varlık Envanteri araçları genel kullanıma sunuldu](#asset-inventory-tools-are-now-generally-available)
- [Kapsayıcı kayıt defterlerinin ve sanal makinelerin taranmasını bulma için belirli bir güvenlik açığını devre dışı bırakma](#disable-a-specific-vulnerability-finding-for-scans-of-container-registries-and-virtual-machines)
- [Kaynağı öneriden muaf tutma](#exempt-a-resource-from-a-recommendation)
- [Güvenlik Merkezi 'ndeki AWS ve GCP bağlayıcıları, çok bulut deneyimi getirir](#aws-and-gcp-connectors-in-security-center-bring-a-multi-cloud-experience)
- [Kubernetes iş yükü koruma önerisi paketi](#kubernetes-workload-protection-recommendation-bundle)
- [Güvenlik açığı değerlendirmesi bulguları artık sürekli dışarı aktarma için kullanılabilir](#vulnerability-assessment-findings-are-now-available-in-continuous-export)
- [Yeni kaynaklar oluştururken öneriler zorlanarak güvenlik yapılandırması yapılandırmalarını engelleyin](#prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources)
- [Ağ güvenlik grubu önerileri geliştirildi](#network-security-group-recommendations-improved)
- [Kullanım dışı önizleme AKS önerisi "Pod güvenlik Ilkeleri Kubernetes hizmetlerinde tanımlanmalıdır"](#deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services)
- [Azure Güvenlik Merkezi 'ndeki e-posta bildirimleri geliştirildi](#email-notifications-from-azure-security-center-improved)
- [Güvenli puan önizleme önerilerini içermez](#secure-score-doesnt-include-preview-recommendations)
- [Öneriler artık önem göstergesi ve yenilik aralığı içeriyor](#recommendations-now-include-a-severity-indicator-and-the-freshness-interval)


### <a name="security-center-gets-a-new-look"></a>Güvenlik Merkezi yeni bir görünüm alır!

Güvenlik Merkezi 'nin portal sayfaları için yenilenmiş bir kullanıcı arabirimi yayımladık. Yeni sayfalar, güvenli puan, varlık envanteri ve Azure Defender için yeni bir genel bakış sayfası ve panolar içerir.

Yeniden tasarlanan genel bakış sayfasında artık güvenli puan, varlık envanteri ve Azure Defender panolarına erişim için bir kutucuk vardır. Ayrıca, mevzuat uyumluluk panosuna bağlanan bir kutucuk vardır.

[Genel bakış sayfası](overview-page.md)hakkında daha fazla bilgi edinin.


### <a name="azure-defender-released"></a>Azure Defender yayımlandı

**Azure Defender** , Azure ve hibrit iş yüklerinizin gelişmiş, akıllı ve koruma Için Güvenlik Merkezi kapsamında tümleşik olan bulut iş yükü koruma platformudur (cwpp). Güvenlik Merkezi 'nin standart fiyatlandırma katmanı seçeneğini değiştirir. 

Azure Defender 'ı Azure Güvenlik Merkezi 'nin **fiyatlandırma ve ayarlar** alanından etkinleştirdiğinizde, aşağıdaki Defender planlarının hepsi aynı anda etkinleştirilmiştir ve ortamınızın işlem, veri ve hizmet katmanları için kapsamlı savunma sağlar:

- [Sunucular için Azure Defender](defender-for-servers-introduction.md)
- [App Service için Azure Defender](defender-for-app-service-introduction.md)
- [Depolama için Azure Defender](defender-for-storage-introduction.md)
- [SQL için Azure Defender](defender-for-sql-introduction.md)
- [Key Vault için Azure Defender](defender-for-key-vault-introduction.md)
- [Kubernetes için Azure Defender](defender-for-kubernetes-introduction.md)
- [Kapsayıcı kayıt defterleri için Azure Defender](defender-for-container-registries-introduction.md)

Bu planların her biri, güvenlik merkezi belgelerinde ayrı olarak açıklanmıştır.

Adanmış panosu sayesinde Azure Defender, sanal makineler, SQL veritabanları, kapsayıcılar, Web uygulamaları, ağınız ve daha fazlası için güvenlik uyarıları ve Gelişmiş tehdit koruması sağlar.

[Azure Defender hakkında daha fazla bilgi](azure-defender.md)

### <a name="azure-defender-for-key-vault-is-generally-available"></a>Key Vault için Azure Defender genel kullanıma sunuldu

Azure Key Vault, şifreleme anahtarlarını ve sertifikalar, bağlantı dizeleri ve parolalar gibi gizli dizileri koruyan bir bulut hizmetidir. 

**Key Vault Için Azure Defender** , ek bir güvenlik zekası katmanı sunarak Azure Key Vault için Azure yerel ve Gelişmiş tehdit koruması sağlar. Uzantıya göre Key Vault için Azure Defender, bu nedenle Key Vault hesaplarınıza bağlı kaynakların çoğunu koruyor.

İsteğe bağlı plan artık GA 'dir. Bu özellik, "Azure Key Vault için Gelişmiş tehdit koruması" olarak önizlemededir.

Ayrıca, Azure portal Key Vault sayfalarında artık **Güvenlik Merkezi** önerileri ve uyarıları için ayrılmış bir **güvenlik** sayfası vardır.

[Key Vault Için Azure Defender](defender-for-key-vault-introduction.md)'da daha fazla bilgi edinin.


### <a name="azure-defender-for-storage-protection-for-files-and-adls-gen2-is-generally-available"></a>Dosyalar ve ADLS 2. için depolama koruması için Azure Defender, genel kullanıma sunuldu 

**Depolama Için Azure Defender** , Azure depolama hesaplarınızdaki zararlı olabilecek etkinlikleri algılar. Verileriniz, blob kapsayıcıları, dosya paylaşımları veya veri Lakes olarak depolanıp saklanmadığı için korunabilir.

[Azure dosyaları](../storage/files/storage-files-introduction.md) ve [Azure Data Lake Storage 2.](../storage/blobs/data-lake-storage-introduction.md) için destek genel kullanıma sunulmuştur.

1 Ekim 2020 ' den bu hizmetlerde kaynakları koruma ücretlendirmeye başlayacağız.

[Depolama Için Azure Defender](defender-for-storage-introduction.md)'da daha fazla bilgi edinin.


### <a name="asset-inventory-tools-are-now-generally-available"></a>Varlık Envanteri araçları genel kullanıma sunuldu

Azure Güvenlik Merkezi 'nin varlık Envanteri sayfası, güvenlik merkezi 'ne bağladığınız kaynakların güvenlik duruşunu görüntülemek için tek bir sayfa sağlar.

Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bu güvenlik açıklarını nasıl düzeltebileceğiniz konusunda öneriler sağlar.

Herhangi bir kaynakta bekleyen öneriler olduğunda, bu değişiklikler envanterde görüntülenir.

[Varlık envanteriyle kaynaklarınızı araştırıp yönetme](asset-inventory.md)hakkında daha fazla bilgi edinin.



### <a name="disable-a-specific-vulnerability-finding-for-scans-of-container-registries-and-virtual-machines"></a>Kapsayıcı kayıt defterlerinin ve sanal makinelerin taranmasını bulma için belirli bir güvenlik açığını devre dışı bırakma

Azure Defender, Azure Container Registry ve sanal makinelerinizdeki görüntüleri taramak için güvenlik açığı tarayıcıları içerir.

Bir bulmayı yok saymanız gereken bir kuruluş varsa, bunu düzeltmek yerine isteğe bağlı olarak devre dışı bırakabilirsiniz. Devre dışı bulgular, güvenli puanınızı etkilemez veya istenmeyen gürültü oluşturmaz.

Bir bulma, devre dışı bırakma kurallarında tanımladığınız ölçütlerle eşleştiğinde, bu, bulguları listesinde görünmez.

Bu seçenek, için öneriler ayrıntıları sayfalarından kullanılabilir:

- **Azure Container Registry görüntülerdeki güvenlik açıkları düzeltilmelidir**
- **Sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir**

[Kapsayıcı görüntüleriniz için belirli bulguları devre dışı bırakma konusunda](defender-for-container-registries-usage.md#disable-specific-findings-preview) daha fazla bilgi edinin ve [sanal makineleriniz için belirli bulguları devre dışı bırakın](remediate-vulnerability-findings-vm.md#disable-specific-findings-preview)


### <a name="exempt-a-resource-from-a-recommendation"></a>Kaynağı öneriden muaf tutma

Bazen bir kaynak belirli bir öneriyle ilgili sağlıksız olarak listelenecektir (ve bu nedenle, güvenli puanınızı düşürürken). Güvenlik Merkezi tarafından izlenmeyen bir işlem tarafından düzeltilebilir olabilir. Ya da kuruluşunuz söz konusu kaynak için riski kabul etmeye karar verdi. 

Bu gibi durumlarda, bir istisna kuralı oluşturabilir ve ileride kaynağın sağlıksız kaynaklar arasında listelenmemesini sağlayabilirsiniz. Bu kurallar, aşağıda açıklandığı gibi belgelenmiş gerekçeler içerebilir.

[Bir kaynağı önerilerden ve güvenli puanın dışında bırakma](exempt-resource.md)hakkında daha fazla bilgi edinin.


### <a name="aws-and-gcp-connectors-in-security-center-bring-a-multi-cloud-experience"></a>Güvenlik Merkezi 'ndeki AWS ve GCP bağlayıcıları, çok bulut deneyimi getirir

Bulut güvenlik hizmetleri genellikle birden çok bulut platformunu kapsayan bulut iş yükleri ile aynı olmalıdır.

Azure Güvenlik Merkezi artık Azure, Amazon Web Services (AWS) ve Google Cloud Platform (GCP) iş yüklerini korur.

AWS ve GCP hesaplarınızı Güvenlik Merkezi 'ne ekleme, AWS güvenlik hub, GCP güvenlik komutu ve Azure Güvenlik Merkezi ' ni tümleştirir. 

[AWS hesaplarınızı Azure Güvenlik Merkezi 'Ne bağlama](quickstart-onboard-aws.md) ve [GCP hesaplarınızı Azure Güvenlik Merkezi 'ne bağlama](quickstart-onboard-gcp.md)konusunda daha fazla bilgi edinin.


### <a name="kubernetes-workload-protection-recommendation-bundle"></a>Kubernetes iş yükü koruma önerisi paketi

Kubernetes iş yüklerinin güvenli olduğundan emin olmak için, Güvenlik Merkezi, Kubernetes giriş denetimi ile zorlama seçenekleri dahil olmak üzere Kubernetes düzeyi sağlamlaştırma önerilerini ekliyor.

AKS kümenizdeki Kubernetes için Azure Ilkesi eklentisini yüklediğinizde, Kubernetes API sunucusuna gönderilen her istek, kümede kalıcı hale gelmeden önce önceden tanımlanmış en iyi uygulamalar kümesine göre izlenir. Daha sonra, en iyi uygulamaları zorlamak ve gelecekteki iş yükleri için bunları zorunlu kılmak üzere yapılandırabilirsiniz.

Örneğin, ayrıcalıklı kapsayıcıların oluşturulmaması ve ileride yapılacak istekleri engellenemeyeceksiniz.

[Kubernetes giriş denetimini kullanan Iş yükü koruma en iyi uygulamaları](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control)hakkında daha fazla bilgi edinin.


### <a name="vulnerability-assessment-findings-are-now-available-in-continuous-export"></a>Güvenlik açığı değerlendirmesi bulguları artık sürekli dışarı aktarma için kullanılabilir

Azure Event Hubs, Log Analytics çalışma alanları veya Azure Izleyici ile uyarı ve önerilerinizi gerçek zamanlı olarak akışa almak için sürekli dışarı aktarmayı kullanın. Buradan, bu verileri Sıems ile tümleştirebilirsiniz (Azure Sentinel, Power BI, Azure Veri Gezgini ve daha fazlasını yapabilirsiniz.

Güvenlik Merkezi 'nin tümleşik güvenlik açığı değerlendirme araçları, "ana" önerilerinde "sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir" gibi eyleme dönüştürülebilir öneriler olarak kaynaklarınızın bulgularını döndürür. 

Öneriler ' i seçip **güvenlik bulgularını dahil et** seçeneğini etkinleştirdiğinizde, güvenlik bulguları sürekli dışarı aktarma aracılığıyla dışarı aktarmaya hazırdır.

:::image type="content" source="./media/continuous-export/include-security-findings-toggle.png" alt-text="Sürekli dışa aktarma yapılandırmasında güvenlik bulgularını dahil et" :::

İlgili sayfalar:

- [Azure sanal makineleri için Güvenlik Merkezi 'nin tümleşik güvenlik açığı değerlendirme çözümü](deploy-vulnerability-assessment-vm.md)
- [Azure Container Registry görüntüleri için Güvenlik Merkezi 'nin tümleşik güvenlik açığı değerlendirme çözümü](defender-for-container-registries-usage.md)
- [Sürekli dışarı aktarma](continuous-export.md)

### <a name="prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources"></a>Yeni kaynaklar oluştururken öneriler zorlanarak güvenlik yapılandırması yapılandırmalarını engelleyin

Güvenlik yapılandırması hataları, güvenlik olaylarının önemli bir nedendir. Güvenlik Merkezi artık, belirli önerilere göre yeni kaynakların yanlış yapılandırmalarını *önlemeye* yardımcı olur. 

Bu özellik, iş yüklerinizi güvende tutmaya ve güvenli puanınızı sabitetmenize yardımcı olabilir.

Belirli bir öneriye göre güvenli bir yapılandırma uygulamak, iki modda sunulur:

- Azure Ilkesinin **reddetme** efektini kullanarak, sağlıksız kaynakların oluşturulmasını durdurabilirsiniz

- **Uygula** seçeneğini kullanarak Azure Ilkesinin **Deployifnotexist** efektinin avantajlarından yararlanabilir ve oluşturma sırasında uyumlu olmayan kaynakları otomatik olarak düzeltebilirsiniz
 
Bu, seçilen güvenlik önerileri için kullanılabilir ve kaynak Ayrıntıları sayfasının en üstünde bulunabilir.

[Zorla/reddetme önerilerini kullanarak yanlış yapılandırma önleme konusunda](prevent-misconfigurations.md)daha fazla bilgi edinin.

###  <a name="network-security-group-recommendations-improved"></a>Ağ güvenlik grubu önerileri geliştirildi

Ağ güvenlik gruplarıyla ilgili aşağıdaki güvenlik önerileri, bazı hatalı pozitif sonuç örneklerini azaltmak için geliştirilmiştir.

- Tüm ağ bağlantı noktaları, VM 'niz ile ilişkili NSG 'de sınırlandırılmalıdır
- Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır
- Internet 'e yönelik sanal makineler ağ güvenlik gruplarıyla korunmalıdır
- Alt ağlar bir ağ güvenlik grubuyla ilişkilendirilmelidir


### <a name="deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services"></a>Kullanım dışı önizleme AKS önerisi "Pod güvenlik Ilkeleri Kubernetes hizmetlerinde tanımlanmalıdır"

Önizleme önerisi "Pod güvenlik Ilkeleri, Kubernetes hizmetlerinde tanımlanmalıdır", [Azure Kubernetes hizmeti](../aks/use-pod-security-policies.md) belgelerinde açıklandığı şekilde kullanımdan kaldırılmıştır.

Pod güvenlik ilkesi (Önizleme) özelliği kullanımdan kalkmaya yönelik olarak ayarlanmıştır ve, AKS için Azure Ilkesi için, 15 Ekim 2020 ' den sonra kullanılabilir olmayacaktır.

Pod güvenlik ilkesi (Önizleme) kullanım dışı olduktan sonra, gelecekteki küme yükseltmelerini gerçekleştirmek ve Azure desteği içinde kalmak için kullanımdan kaldırılan özelliği kullanarak mevcut kümelerin özelliğini devre dışı bırakmanız gerekir.


### <a name="email-notifications-from-azure-security-center-improved"></a>Azure Güvenlik Merkezi 'ndeki e-posta bildirimleri geliştirildi

Güvenlik uyarıları ile ilgili e-postaların aşağıdaki bölgeleri geliştirilmiştir: 

- Tüm önem düzeyleri için uyarılar hakkında e-posta bildirimleri gönderme özelliği eklendi
- Abonelikte farklı Azure rolleriyle kullanıcılara bildirim verme özelliği eklendi
- En yüksek öneme sahip uyarılarda (orijinal ihlal etmeme olasılığı yüksek olan), abonelik sahiplerini varsayılan olarak önceden bilgilendiriyoruz
- E-posta bildirimleri yapılandırma sayfasından telefon numarası alanını kaldırdık

[Güvenlik uyarıları için e-posta bildirimlerini ayarlama](security-center-provide-security-contact-details.md)bölümünde daha fazla bilgi edinin.


### <a name="secure-score-doesnt-include-preview-recommendations"></a>Güvenli puan önizleme önerilerini içermez 

Güvenlik Merkezi, güvenlik sorunları için kaynaklarınızı, aboneliklerinizi ve kuruluşunuzu sürekli olarak değerlendirir. Daha sonra her türlü bulguları tek bir puanın içinde toplar, böylece bir bakışta geçerli güvenlik durumunuza göre daha yüksek puan, belirtilen risk düzeyini azaltır.

Yeni tehditler bulunduğundan yeni güvenlik önerileri yeni öneriler aracılığıyla güvenlik merkezi 'nde kullanılabilir hale getirilir. Güvenli puanınızın beklenmedik şekilde değişiklik yaşamamasını önlemek ve yeni önerilerin puanlarını etkilemeden önce keşfedebileceğiniz bir yetkisiz kullanım süresi sağlamak için, **Önizleme** olarak işaretlenen öneriler artık güvenli puanınızın hesaplamalarına dahil değildir. Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunduklarında, bunlar mümkün olduğunda yine de düzeltilmelidir.

Ayrıca, **Önizleme** önerileri bir kaynağı "sağlıksız" olarak işlemez.

Önizleme önerisi örneği:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Önizleme bayrağıyla ilgili öneri":::

[Güvenli skor hakkında daha fazla bilgi edinin](secure-score-security-controls.md).


### <a name="recommendations-now-include-a-severity-indicator-and-the-freshness-interval"></a>Öneriler artık önem göstergesi ve yenilik aralığı içeriyor

Önerilerin ayrıntılar sayfasında artık bir yeniliği aralığı göstergesi (her ne zaman geçerlidir) ve önerinin önem derecesine ilişkin net bir ekran görüntülenir.

:::image type="content" source="./media/release-notes/recommendations-severity-freshness-indicators.png" alt-text="Yeniliği ve önem derecesini gösteren öneri sayfası":::