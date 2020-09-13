---
title: Azure Güvenlik Merkezi için sürüm notları
description: Azure Güvenlik Merkezi 'nde nelerin yeni ve değiştirilmiş olduğuna ilişkin bir açıklama.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/12/2020
ms.author: memildin
ms.openlocfilehash: f3aeccd30a9c89c2a43dfb85d4a57274037ec05f
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569263"
---
# <a name="whats-new-in-azure-security-center"></a>Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?

Azure güvenliği etkin bir geliştirme aşamasındadır ve gelişmede geliştirmeler sunar. En son gelişmelerden haberdar olmak için, bu sayfada hakkında bilgi verilmektedir:

- Yeni özellikler
- Hata düzeltmeleri
- Kullanım dışı işlevsellik

Bu sayfa düzenli olarak güncelleştirildiğinden, sık sık ziyaret edin. Altı aydan eski olan öğeleri arıyorsanız, [Azure Güvenlik Merkezi 'ndeki yenilikler Için arşivde](release-notes-archive.md)bulabilirsiniz.


## <a name="september-2020"></a>Eylül 2020

Eylül ayında güncelleştirmeler şunları içerir:

- [Güvenlik açığı değerlendirmesi bulguları artık sürekli dışarı aktarma için kullanılabilir](#vulnerability-assessment-findings-are-now-available-in-continuous-export)
- [Yeni kaynaklar oluştururken öneriler zorlanarak güvenlik yapılandırması yapılandırmalarını engelleyin](#prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources)
- [Ağ güvenlik grubu önerileri geliştirildi](#network-security-group-recommendations-improved)
- [Kullanım dışı önizleme AKS önerisi "Pod güvenlik Ilkeleri Kubernetes hizmetlerinde tanımlanmalıdır"](#deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services)
- [Azure Güvenlik Merkezi 'ndeki e-posta bildirimleri geliştirildi](#email-notifications-from-azure-security-center-improved)
- [Güvenli puan önizleme önerilerini içermez](#secure-score-doesnt-include-preview-recommendations)
- [Öneriler artık önem göstergesi ve yenilik aralığı içeriyor](#recommendations-now-include-a-severity-indicator-and-the-freshness-interval)

### <a name="vulnerability-assessment-findings-are-now-available-in-continuous-export"></a>Güvenlik açığı değerlendirmesi bulguları artık sürekli dışarı aktarma için kullanılabilir

Uyarı ve önerilerinizi Azure Event Hubs, Log Analytics çalışma alanları veya Azure Izleyici ile gerçek zamanlı olarak akışa almak için sürekli dışarı aktarmayı kullanın. Buradan, bu verileri Sıems ile tümleştirebilirsiniz (Azure Sentinel, Power BI, Azure Veri Gezgini ve daha fazlasını yapabilirsiniz.

Güvenlik Merkezi 'nin tümleşik güvenlik açığı değerlendirme araçları, "ana" önerilerinde "sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir" gibi eyleme dönüştürülebilir öneriler olarak kaynaklarınızın bulgularını döndürür. 

Öneriler ' i seçip **güvenlik bulgularını dahil et** seçeneğini etkinleştirdiğinizde, güvenlik bulguları sürekli dışarı aktarma aracılığıyla dışarı aktarmaya hazırdır.

:::image type="content" source="./media/continuous-export/include-security-findings-toggle.png" alt-text="Sürekli dışa aktarma yapılandırmasında güvenlik bulgularını dahil et" :::

İlgili sayfalar:

- [Azure sanal makineleri için Güvenlik Merkezi 'nin tümleşik güvenlik açığı değerlendirme çözümü](deploy-vulnerability-assessment-vm.md)
- [Azure Container Registry görüntüleri için Güvenlik Merkezi 'nin tümleşik güvenlik açığı değerlendirme çözümü](monitor-container-security.md)
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

Önizleme önerisi "Pod güvenlik Ilkeleri, Kubernetes hizmetlerinde tanımlanmalıdır", [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/use-pod-security-policies) belgelerinde açıklandığı şekilde kullanımdan kaldırılmıştır.

Pod güvenlik ilkesi (Önizleme) özelliği kullanımdan kaldırma için ayarlanmış ve 15 Ekim 2020 ' den sonra AKS için Azure Ilkesi 'nin kullanım dışı bırakılmayacak.

Pod güvenlik ilkesi (Önizleme) kullanım dışı olduktan sonra, gelecekteki küme yükseltmelerini gerçekleştirmek ve Azure desteği içinde kalmak için kullanımdan kaldırılan özelliği kullanarak mevcut kümelerin özelliğini devre dışı bırakmanız gerekir.


### <a name="email-notifications-from-azure-security-center-improved"></a>Azure Güvenlik Merkezi 'ndeki e-posta bildirimleri geliştirildi

Güvenlik uyarıları ile ilgili e-postaların aşağıdaki bölgeleri geliştirilmiştir: 

- Tüm önem düzeyleri için uyarılar hakkında e-posta bildirimleri gönderme özelliği eklendi
- Kullanıcılara abonelikte farklı RBAC rolleri bildirme yeteneği eklendi
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



## <a name="august-2020"></a>Ağustos 2020

Ağustos ayında güncelleştirmeler şunlardır:

- [Varlık envanteri-varlıklarınızın güvenlik sonrası yeni görünümü](#asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets)
- [Azure Active Directory güvenlik varsayılanları için destek eklendi (Multi-Factor Authentication için)](#added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication)
- [Hizmet sorumlusu önerisi eklendi](#service-principals-recommendation-added)
- [VM 'lerde güvenlik açığı değerlendirmesi-birleştirilmiş öneriler ve ilkeler](#vulnerability-assessment-on-vms---recommendations-and-policies-consolidated)
- [ASC_default Initiative 'e eklenen yeni AKS güvenlik ilkeleri – yalnızca özel önizleme müşterileri tarafından kullanılmak üzere](#new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only)


### <a name="asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets"></a>Varlık envanteri-varlıklarınızın güvenlik sonrası yeni görünümü

Güvenlik Merkezi 'nin varlık Envanteri (Şu anda önizleme aşamasındadır), güvenlik merkezi 'ne bağladığınız kaynakların güvenlik duruşunu görüntülemek için bir yol sağlar.

Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bu güvenlik açıklarını nasıl düzeltebileceğiniz konusunda öneriler sağlar. Herhangi bir kaynakta bekleyen öneriler olduğunda, bu değişiklikler envanterde görüntülenir.

Güvenlik sonrası verilerinizi araştırmak ve bulgularınızı temel alarak daha fazla eylem gerçekleştirmek için görünümü ve filtrelerini kullanabilirsiniz.

[Varlık envanteri](asset-inventory.md)hakkında daha fazla bilgi edinin.


### <a name="added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication"></a>Azure Active Directory güvenlik varsayılanları için destek eklendi (Multi-Factor Authentication için)

Güvenlik Merkezi, Microsoft 'un ücretsiz kimlik güvenlik korumalarının [güvenlik Varsayılanları](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults)için tam destek ekledi.

Güvenlik Varsayılanları, kuruluşunuzun kimlik ile ilgili ortak saldırılardan savunmak için önceden yapılandırılmış kimlik güvenlik ayarlarını sağlar. Güvenlik Varsayılanları, genel olarak 5.000.000 taneden fazla kiracı koruuyor; 50.000 kiracılar da Güvenlik Merkezi tarafından korunur.

Güvenlik Merkezi artık güvenlik Varsayılanları etkinleştirilmeden bir Azure aboneliğini her belirlediğinde bir güvenlik önerisi sunmaktadır. Bu aşamada, Güvenlik Merkezi, Azure Active Directory (AD) Premium lisansının bir parçası olan koşullu erişimi kullanarak Multi-Factor Authentication 'ı etkinleştirmeyi öneririz. Azure AD Ücretsiz kullanan müşteriler için artık güvenlik varsayılanlarını etkinleştirmenizi öneririz. 

Amacınız, daha fazla müşteriyi MFA ile bulut ortamlarını güvenli hale getirmeye teşvik etmek ve ayrıca, [güvenli puanınızın](https://docs.microsoft.com/azure/security-center/secure-score-security-controls)en yüksek risklerinden birini hafifletmektir.

[Güvenlik Varsayılanları](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults)hakkında daha fazla bilgi edinin.


### <a name="service-principals-recommendation-added"></a>Hizmet sorumlusu önerisi eklendi

Güvenlik Merkezi müşterilerinin aboneliklerini yönetmek için yönetim sertifikaları kullanmalarını sağlamak üzere yeni bir öneri eklenmiştir.

Kullanım önerisi, **yönetim sertifikaları yerine aboneliklerinizi korumak Için hizmet sorumlularını** , aboneliklerinizi daha güvenli bir şekilde yönetmek Için hizmet sorumlularını veya Azure Resource Manager kullanmanızı önerir. 

[Azure Active Directory Içindeki uygulama ve hizmet sorumlusu nesneleri](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object)hakkında daha fazla bilgi edinin.


### <a name="vulnerability-assessment-on-vms---recommendations-and-policies-consolidated"></a>VM 'lerde güvenlik açığı değerlendirmesi-birleştirilmiş öneriler ve ilkeler

Güvenlik Merkezi, bir güvenlik açığı değerlendirme çözümü çalıştırıp çalıştırmadığını belirlemek için sanal makinelerinizi inceler. Bir güvenlik açığı değerlendirme çözümü bulunmazsa, güvenlik merkezi dağıtımı basitleştirmeye yönelik bir öneri sunar.

Güvenlik açıkları bulunduğunda Güvenlik Merkezi, gerektiğinde araştırmanız ve düzeltilmesi için bulguları özetlemeye yönelik bir öneri sunar.

Kullandıkları tarayıcı türünden bağımsız olarak tüm kullanıcılar için tutarlı bir deneyim sağlamak amacıyla, aşağıdaki iki öneriyle dört öneri sunuyoruz:

|Birleşik öneri|Açıklamayı Değiştir|
|----|:----|
|**Sanal makinelerinizde bir güvenlik açığı değerlendirme çözümünün etkinleştirilmesi gerekir**|Aşağıdaki iki öneriyi değiştirir:<br> **•** Sanal makinelerde yerleşik güvenlik açığı değerlendirme çözümünü etkinleştirin (Qualys (artık kullanım dışı) ile desteklenir (Standart katmana dahildir)<br> **•** Güvenlik açığı değerlendirme çözümünün sanal makinelerinizde yüklü olması gerekir (artık kullanım dışı) (Standart ve ücretsiz Katmanlar)|
|**Sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir**|Aşağıdaki iki öneriyi değiştirir:<br>**•** Sanal makinelerinizde bulunan güvenlik açıklarını düzeltin (Qualys tarafından desteklenir) (artık kullanım dışı)<br>**•** Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir (artık kullanım dışı)|
|||

Artık güvenlik merkezi 'nin güvenlik açığı değerlendirmesi uzantısını veya özel olarak lisanslı bir çözümü ("KLG") Qualys veya Rapid7 gibi bir ortaktan dağıtmak için aynı öneriyi kullanacaksınız.

Ayrıca, güvenlik açıkları bulunduğunda ve Güvenlik Merkezi 'ne bildirildiğine göre tek bir öneri, bunları tanımlayan güvenlik açığı değerlendirme çözümüne bakılmaksızın sizi bulgulara bildirir.

#### <a name="updating-dependencies"></a>Bağımlılıklar güncelleştiriliyor

Önceki önerilere veya ilke anahtarlarına/adlara başvuruda bulunan komut dosyalarınız, sorgular veya otomatiklamalar varsa, başvuruları güncelleştirmek için aşağıdaki tabloları kullanın:

##### <a name="before-august-2020"></a>Ağustos 2020 öncesi

|Öneri|Kapsam|
|----|:----|
|**Sanal makinelerde yerleşik güvenlik açığı değerlendirme çözümünü etkinleştirin (Qualys tarafından desteklenir)**<br>Anahtar: 550e890b-e652-4d22-8274-60b3bdb24c63|Yerleşik|
|**Sanal makinelerinizde bulunan güvenlik açıklarını düzeltin (Qualys tarafından desteklenir)**<br>Anahtar: 1195aff-c881-495E-9bc5-1486211ae03f|Yerleşik|
|**Güvenlik açığı değerlendirme çözümünün sanal makinelerinizde yüklü olması gerekir**<br>Anahtar: 01b1ed4c-B733-4FEE-b145-f23236e70cf3|KLG|
|**Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir**<br>Anahtar: 71992a2a-d168-42e0-b10e-6b45fa2ecddb|KLG|
||||


|İlke|Kapsam|
|----|:----|
|**Güvenlik açığı değerlendirmesi sanal makinelerde etkinleştirilmelidir**<br>İlke KIMLIĞI: 501541f7-f7e7-4cd6-868C-4190fdad3ac9|Yerleşik|
|**Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir**<br>İlke KIMLIĞI: 760a85ff-6162-42b3-8d70-698e268f648c|KLG|
||||


##### <a name="from-august-2020"></a>Ağustos 2020 ' den

|Öneri|Kapsam|
|----|:----|
|**Sanal makinelerinizde bir güvenlik açığı değerlendirme çözümünün etkinleştirilmesi gerekir**<br>Anahtar: ffff0522-1e88-47fc-8382-2a80ba848f5d|Yerleşik + KLG|
|**Sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir**<br>Anahtar: 1195aff-c881-495E-9bc5-1486211ae03f|Yerleşik + KLG|
||||

|İlke|Kapsam|
|----|:----|
|[**Güvenlik açığı değerlendirmesi sanal makinelerde etkinleştirilmelidir**](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f501541f7-f7e7-4cd6-868c-4190fdad3ac9)<br>İlke KIMLIĞI: 501541f7-f7e7-4cd6-868C-4190fdad3ac9 |Yerleşik + KLG|
||||


### <a name="new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only"></a>ASC_default Initiative 'e eklenen yeni AKS güvenlik ilkeleri – yalnızca özel önizleme müşterileri tarafından kullanılmak üzere

Kubernetes iş yüklerinin varsayılan olarak güvenli olduğundan emin olmak için, güvenlik merkezi Kubernetes, Kubernetes giriş denetimiyle zorlama seçenekleri de dahil olmak üzere Kubernetes düzey ilkelerini ve sağlamlaştırma

Bu projenin erken aşaması, özel bir önizleme ve ASC_default girişiminin yeni (varsayılan olarak devre dışı) ilkelerinin eklenmesini içerir.

Bu ilkeleri güvenle yoksayabilirsiniz, ortamınızda hiçbir etkisi olmaz. Bunları etkinleştirmek istiyorsanız, önizleme için kaydolun https://aka.ms/SecurityPrP ve aşağıdaki seçeneklerden seçim yapın:

1. **Tek önizleme** – yalnızca bu özel önizlemeye katılacak şekilde. Birleştirmek istediğiniz önizleme olarak "ASC sürekli tarama" yı kesin olarak belirtmeyi unutmayın.
1. **Devam eden program** – bu ve gelecekteki özel önizlemelere eklenmek üzere. Bir profil ve gizlilik sözleşmesini doldurmanız gerekir.


## <a name="july-2020"></a>Temmuz 2020

Temmuz ayında güncelleştirmeler şunları içerir:
- [Sanal makineler için güvenlik açığı değerlendirmesi artık Market olmayan görüntüler için kullanılabilir](#vulnerability-assessment-for-virtual-machines-is-now-available-for-non-marketplace-images)
- [Azure depolama için tehdit koruması Azure dosyaları ve Azure Data Lake Storage 2. içerecek şekilde genişletildi (Önizleme)](#threat-protection-for-azure-storage-expanded-to-include-azure-files-and-azure-data-lake-storage-gen2-preview)
- [Tehdit koruması özelliklerini etkinleştirmeye yönelik sekiz yeni öneri](#eight-new-recommendations-to-enable-threat-protection-features)
- [Kapsayıcı güvenlik geliştirmeleri-daha hızlı kayıt defteri tarama ve yenilenen belgeler](#container-security-improvements---faster-registry-scanning-and-refreshed-documentation)
- [Uyarlamalı uygulama denetimleri, yol kurallarında joker karakterlere yönelik yeni bir öneri ve destek ile güncelleştirildi](#adaptive-application-controls-updated-with-a-new-recommendation-and-support-for-wildcards-in-path-rules)
- [SQL gelişmiş veri güvenliği için altı ilke kullanım dışı](#six-policies-for-sql-advanced-data-security-deprecated)




### <a name="vulnerability-assessment-for-virtual-machines-is-now-available-for-non-marketplace-images"></a>Sanal makineler için güvenlik açığı değerlendirmesi artık Market olmayan görüntüler için kullanılabilir

Güvenlik Merkezi bir güvenlik açığı değerlendirme çözümü dağıttığınızda, dağıtım öncesinde daha önce bir doğrulama denetimi gerçekleştirdi. Denetim, hedef sanal makinenin Market SKU 'sunu doğrulamamıştı. 

Bu güncelleştirmeden denetim kaldırılmıştır ve artık ' özel ' Windows ve Linux makinelerine güvenlik açığı değerlendirme araçları dağıtabilirsiniz. Özel görüntüler, Market varsayılanlarından değiştirdiğiniz alanlardır.

Artık tümleşik güvenlik açığı değerlendirme uzantısını (Qualys tarafından desteklenen) çok sayıda makineye dağıtabseniz de, destek yalnızca [Tümleşik güvenlik açığı tarayıcısını Standart katman VM 'Lerine dağıtma](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-vulnerability-scanner-to-standard-tier-vms) bölümünde listelenen bir işletim sistemi kullanıyorsanız kullanılabilir

[Sanal makineler için tümleşik güvenlik açığı tarayıcısı (yalnızca Standart katman)](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner)hakkında daha fazla bilgi edinin.

Özel olarak lisanslanan güvenlik açığı değerlendirme çözümünüzü [bir iş ortağı güvenlik açığı tarama çözümü dağıtma](deploy-vulnerability-assessment-vm.md)konusunda Qualys veya Rapid7 'tan kullanma hakkında daha fazla bilgi edinin.


### <a name="threat-protection-for-azure-storage-expanded-to-include-azure-files-and-azure-data-lake-storage-gen2-preview"></a>Azure depolama için tehdit koruması Azure dosyaları ve Azure Data Lake Storage 2. içerecek şekilde genişletildi (Önizleme)

Azure depolama için tehdit koruması, Azure depolama hesaplarınızdaki zararlı olabilecek etkinlikleri algılar. Güvenlik Merkezi, depolama hesaplarınıza erişme veya bu hesapları kullanma girişimlerini algıladığında uyarıları görüntüler. 

Verileriniz, blob kapsayıcıları, dosya paylaşımları veya veri Lakes olarak depolanıp saklanmadığı için korunabilir. 

[Azure depolama için tehdit koruması](threat-protection.md#threat-protection-for-azure-storage-)hakkında daha fazla bilgi edinin.




### <a name="eight-new-recommendations-to-enable-threat-protection-features"></a>Tehdit koruması özelliklerini etkinleştirmeye yönelik sekiz yeni öneri

Azure Güvenlik Merkezi 'nin tehdit koruması özelliklerinin aşağıdaki kaynak türleri için etkinleştirilmesi için basit bir yol sağlamak üzere sekiz yeni öneri eklenmiştir: sanal makineler, App Service planlar, Azure SQL veritabanı sunucuları, makinelerdeki SQL Server 'lar, Azure depolama hesapları, Azure Kubernetes hizmet kümeleri, Azure Container Registry kayıt defterleri ve Azure Key Vault kasaları.

Yeni öneriler şunlardır:

- **Azure SQL veritabanı sunucularında gelişmiş veri güvenliği etkinleştirilmelidir**
- **Makinelerdeki SQL sunucularında gelişmiş veri güvenliği etkinleştirilmelidir**
- **Gelişmiş tehdit koruması Azure App Service planlarda etkinleştirilmelidir**
- **Gelişmiş tehdit koruması Azure Container Registry kayıt defterlerinde etkinleştirilmelidir**
- **Gelişmiş tehdit koruması Azure Key Vault kasaları üzerinde etkinleştirilmelidir**
- **Gelişmiş tehdit koruması, Azure Kubernetes hizmet kümelerinde etkinleştirilmelidir**
- **Gelişmiş tehdit koruması, Azure depolama hesaplarında etkinleştirilmelidir**
- **Gelişmiş tehdit koruması sanal makinelerde etkinleştirilmelidir**

Bu yeni öneriler, **Gelişmiş tehdit koruması** güvenlik denetimini etkinleştir ' e aittir.

Öneriler de hızlı düzelme özelliğini içerir. 

> [!IMPORTANT]
> Bu önerilerin herhangi birini yeniden yapmak, ilgili kaynakların korunması için ücretlendirmesiyle sonuçlanır. Geçerli abonelikte ilgili kaynaklarınız varsa, bu ücretler hemen başlayacaktır. Daha sonraki bir tarihte daha sonra ekleyebilirsiniz.
> 
> Örneğin, aboneliğinizde hiç Azure Kubernetes hizmet kümeniz yoksa ve tehdit korumasını etkinleştirirseniz, hiçbir ücret alınmaz. Gelecekte aynı aboneliğe bir küme eklerseniz, bu, otomatik olarak korunur ve ücretler bu anda başlayacaktır.

[Güvenlik önerileri başvuru sayfasında](recommendations-reference.md)bunların her biri hakkında daha fazla bilgi edinin.

[Azure Güvenlik Merkezi 'nde tehdit koruması](https://docs.microsoft.com/azure/security-center/threat-protection)hakkında daha fazla bilgi edinin.




### <a name="container-security-improvements---faster-registry-scanning-and-refreshed-documentation"></a>Kapsayıcı güvenlik geliştirmeleri-daha hızlı kayıt defteri tarama ve yenilenen belgeler

Kapsayıcı güvenlik etki alanındaki sürekli yatırımların bir parçası olarak, güvenlik merkezi 'nin Azure Container Registry ' de depolanan kapsayıcı görüntülerinin dinamik taramalarında önemli bir performans geliştirmesini paylaşabiliriz. Şimdi taramalar genellikle yaklaşık iki dakika içinde tamamlanır. Bazı durumlarda, bu işlem 15 dakikaya kadar sürebilir.

Azure Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri ile ilgili açıklık ve kılavuz kalitesini artırmak için kapsayıcı güvenliği belge sayfalarını de yeniledik. 

Aşağıdaki makalelerde Güvenlik Merkezi 'nin kapsayıcı güvenliği hakkında daha fazla bilgi edinin:

- [Güvenlik Merkezi 'nin kapsayıcı güvenlik özelliklerine genel bakış](https://docs.microsoft.com/azure/security-center/container-security)
- [Azure Container Registry ile tümleştirmenin ayrıntıları](https://docs.microsoft.com/azure/security-center/azure-container-registry-integration)
- [Azure Kubernetes hizmeti ile tümleştirmenin ayrıntıları](https://docs.microsoft.com/azure/security-center/azure-kubernetes-service-integration)
- [Kayıt defterlerine nasıl tarama yapılır ve Docker konaklarınızı sağlamlaştırın](https://docs.microsoft.com/azure/security-center/monitor-container-security)
- [Azure Kubernetes hizmet kümeleri için tehdit koruması özelliklerinden güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-akscluster)
- [Azure Kubernetes hizmet konakları için tehdit koruması özelliklerinden güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-containerhost)
- [Kapsayıcılar için güvenlik önerileri](https://docs.microsoft.com/azure/security-center/recommendations-reference#recs-containers)



### <a name="adaptive-application-controls-updated-with-a-new-recommendation-and-support-for-wildcards-in-path-rules"></a>Uyarlamalı uygulama denetimleri, yol kurallarında joker karakterlere yönelik yeni bir öneri ve destek ile güncelleştirildi

Uyarlamalı uygulama denetimleri özelliği iki önemli güncelleştirme aldı:

* Yeni bir öneri, daha önce izin verilmeyen olası yasal davranışı tanımlar. **Uyarlamalı uygulama denetim ilkenizde yeni öneri, Allowlist kuralları güncellenmelidir**, uyarlamalı uygulama denetimleri ihlal uyarılarındaki hatalı pozitif sonuç sayısını azaltmak için mevcut ilkeye yeni kurallar eklemenizi ister.

* Yol kuralları artık joker karakterleri destekliyor. Bu güncelleştirmeden, joker karakterleri kullanarak izin verilen yol kurallarını yapılandırabilirsiniz. Desteklenen iki senaryo vardır:

    * Bu klasörde ve alt klasörlerde bulunan tüm yürütülebilir dosyalara izin vermek için yolun sonunda bir joker karakter kullanma

    * Bilinen bir yürütülebilir adı, değişen bir klasör adıyla (örn. bilinen bir yürütülebiliri olan kişisel Kullanıcı klasörleri, otomatik olarak oluşturulan klasör adları, vb.) etkinleştirmek için yolun ortasında bir joker karakter kullanma.


[Uyarlamalı uygulama denetimleri hakkında daha fazla bilgi edinin](security-center-adaptive-application.md).



### <a name="six-policies-for-sql-advanced-data-security-deprecated"></a>SQL gelişmiş veri güvenliği için altı ilke kullanım dışı

SQL makinelerinde gelişmiş veri güvenliği ile ilgili altı ilke kullanım dışı bırakılıyor:

- Gelişmiş tehdit koruması türleri SQL yönetilen örneği gelişmiş veri güvenliği ayarları içinde ' All ' olarak ayarlanmalıdır
- Gelişmiş tehdit koruması türleri SQL Server Gelişmiş veri güvenliği ayarları 'nda ' tümü ' olarak ayarlanmalıdır
- SQL yönetilen örneği için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir
- SQL Server için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir
- SQL yönetilen örnek gelişmiş veri güvenliği ayarları 'nda yöneticilere ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir
- SQL Server Gelişmiş veri güvenliği ayarları 'nda Yöneticiler ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir

[Yerleşik ilkeler](security-center-policy-definitions.md)hakkında daha fazla bilgi edinin.





## <a name="june-2020"></a>Haziran 2020

Haziran 'daki güncelleştirmeler şunlardır:
- [Güvenli puan API 'SI (Önizleme)](#secure-score-api-preview)
- [SQL makineler için gelişmiş veri güvenliği (Azure, diğer bulutlar ve şirket içi) (Önizleme)](#advanced-data-security-for-sql-machines-azure-other-clouds-and-on-prem-preview)
- [Log Analytics aracısını Azure Arc makinelerine dağıtmaya yönelik iki yeni öneri (Önizleme)](#two-new-recommendations-to-deploy-the-log-analytics-agent-to-azure-arc-machines-preview)
- [Ölçekte sürekli dışa aktarma ve iş akışı Otomasyonu yapılandırması oluşturmaya yönelik yeni ilkeler](#new-policies-to-create-continuous-export-and-workflow-automation-configurations-at-scale)
- [İnternet 'e yönelik olmayan sanal makineleri korumak için NSG 'lerin kullanılmasına yönelik yeni öneri](#new-recommendation-for-using-nsgs-to-protect-non-internet-facing-virtual-machines)
- [Tehdit koruması ve gelişmiş veri güvenliğini etkinleştirmek için yeni ilkeler](#new-policies-for-enabling-threat-protection-and-advanced-data-security)



### <a name="secure-score-api-preview"></a>Güvenli puan API 'SI (Önizleme)

Artık puanınızı [güvenli Puanlama API 'si](https://docs.microsoft.com/rest/api/securitycenter/securescores/) (Şu anda önizleme aşamasında) aracılığıyla erişebilirsiniz. API yöntemleri, verileri sorgulama ve zaman içinde güvenli Puanlarınızın kendi raporlama mekanizmanızı oluşturma esnekliğini sağlar. Örneğin, belirli bir aboneliğin Puanını almak için **güvenli puanlar** API 'sini kullanabilirsiniz. Ayrıca, güvenlik denetimlerini ve aboneliklerinizin geçerli Puanını listelemek için **güvenli puan denetimleri** API 'sini de kullanabilirsiniz.

Güvenli Puanlama API 'SI ile mümkün olan dış araçların örnekleri için [GitHub topluluğumuzın güvenli Puanlama alanına](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score)bakın.

[Azure Güvenlik Merkezi 'nde güvenli puan ve güvenlik denetimleri](secure-score-security-controls.md)hakkında daha fazla bilgi edinin.



### <a name="advanced-data-security-for-sql-machines-azure-other-clouds-and-on-prem-preview"></a>SQL makineler için gelişmiş veri güvenliği (Azure, diğer bulutlar ve şirket içi) (Önizleme)

Azure Güvenlik Merkezi 'nin SQL makineler için gelişmiş veri güvenliği artık Azure 'da barındırılan SQL sunucularını diğer bulut ortamlarında ve hatta şirket içi makinelerde korur. Bu, karma ortamları tam olarak desteklemek için Azure Native SQL sunucularınız için korumaların kapsamını genişletir.

Gelişmiş veri güvenliği, SQL makinelerinizde bulundukları yerde güvenlik açığı değerlendirmesi ve Gelişmiş tehdit koruması sağlar.

Kurulum iki adımdan oluşur:

1. Azure hesabı bağlantısı sağlamak için Log Analytics aracısını SQL Server ana makinesine dağıtma.

1. Güvenlik Merkezi 'nin fiyatlandırma ve Ayarlar sayfasında isteğe bağlı paket etkinleştiriliyor.

[SQL makinelerinde gelişmiş veri güvenliği](security-center-iaas-advanced-data.md)hakkında daha fazla bilgi edinin.



### <a name="two-new-recommendations-to-deploy-the-log-analytics-agent-to-azure-arc-machines-preview"></a>Log Analytics aracısını Azure Arc makinelerine dağıtmaya yönelik iki yeni öneri (Önizleme)

[Log Analytics aracısını](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent) Azure Arc makinelerinize dağıtmaya yardımcı olmak ve Azure Güvenlik Merkezi tarafından korunduğundan emin olmak için iki yeni öneri eklenmiştir:

- **Log Analytics Aracısı Windows tabanlı Azure Arc makinelerinizde yüklü olmalıdır (Önizleme)**
- **Log Analytics Aracısı, Linux tabanlı Azure yay makinelerinizde yüklü olmalıdır (Önizleme)**

Bu yeni öneriler, var olan (ilgili) öneriyle aynı dört güvenlik denetiminde görünür, **makinelerinizde izleme Aracısı yüklü**olmalıdır: güvenlik yapılandırmalarının düzeltilmesi, uyarlamalı uygulama denetimi uygulamak, sistem güncelleştirmelerini uygulamak ve Endpoint Protection 'ı etkinleştirmek.

Öneriler Ayrıca dağıtım sürecini hızlandırmaya yardımcı olmak için hızlı düzelme özelliğini de içerir. 

[İşlem ve uygulama önerileri](recommendations-reference.md#recs-computeapp) tablosunda bu iki yeni öneri hakkında daha fazla bilgi edinin.

Azure Güvenlik Merkezi ['nin aracıyı Log Analytics aracı nedir?](https://docs.microsoft.com/azure/security-center/faq-data-collection-agents#what-is-the-log-analytics-agent)bölümünde nasıl kullandığı hakkında daha fazla bilgi edinin.

[Azure Arc makinelerinde uzantıları](https://docs.microsoft.com/azure/azure-arc/servers/manage-vm-extensions#enable-extensions-from-the-portal)hakkında daha fazla bilgi edinin.



### <a name="new-policies-to-create-continuous-export-and-workflow-automation-configurations-at-scale"></a>Ölçekte sürekli dışa aktarma ve iş akışı Otomasyonu yapılandırması oluşturmaya yönelik yeni ilkeler

Kuruluşunuzun izleme ve olay yanıtı süreçlerini otomatik hale getirmek, güvenlik olaylarını araştırmak ve azaltmak için gereken süreyi büyük ölçüde iyileştirebilir.

Otomasyon yapılandırmalarınızı kuruluşunuz genelinde dağıtmak için bu yerleşik ' DeployIfdNotExist ' Azure ilkelerini kullanarak [sürekli dışa aktarma](continuous-export.md) ve [iş akışı Otomasyonu](workflow-automation.md) yordamları oluşturun ve yapılandırın:

İlkeler Azure ilkesinde bulunabilir:


|Hedef  |İlke  |İlke KIMLIĞI  |
|---------|---------|---------|
|Olay Hub 'ına sürekli dışarı aktarma|[Azure Güvenlik Merkezi uyarıları ve önerileri için Olay Hub 'ına aktarma dağıtma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fcdfcce10-4578-4ecd-9703-530938e4abcb)|cdfcce10-4578-4ecd-9703-530938e4abcb|
|Log Analytics çalışma alanına sürekli dışarı aktarma|[Azure Güvenlik Merkezi uyarıları ve önerileri için Log Analytics çalışma alanına dışarı aktarma dağıtma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fffb6f416-7bd2-4488-8828-56585fef2be9)|ffb6f416-7bd2-4488-8828-56585fef2be9|
|Güvenlik uyarıları için iş akışı Otomasyonu|[Azure Güvenlik Merkezi uyarıları için Iş akışı Otomasyonu dağıtma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2ff1525828-9a90-4fcf-be48-268cdd02361e)|f1525828-9a90-4fcf-be48-268cdd02361e|
|Güvenlik önerileri için iş akışı Otomasyonu|[Azure Güvenlik Merkezi önerileri için Iş akışı Otomasyonu dağıtma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f73d6ab6c-2475-4850-afd6-43795f3492ef)|73d6ab6c-2475-4850-afd6-43795f3492ef|
||||

[İş akışı Otomasyonu şablonları](https://github.com/Azure/Azure-Security-Center/tree/master/Workflow%20automation)ile çalışmaya başlayın.

[Azure Güvenlik Merkezi uyarılarını ve önerilerini ilke aracılığıyla sürekli olarak dışarı aktarmak](https://techcommunity.microsoft.com/t5/azure-security-center/continuously-export-azure-security-center-alerts-and/ba-p/1440745)için iki dışarı aktarma ilkesini kullanma hakkında daha fazla bilgi edinin.


### <a name="new-recommendation-for-using-nsgs-to-protect-non-internet-facing-virtual-machines"></a>İnternet 'e yönelik olmayan sanal makineleri korumak için NSG 'lerin kullanılmasına yönelik yeni öneri

"Güvenlik en iyi yöntemlerini uygulama" güvenlik denetimi artık aşağıdaki yeni öneriyi içerir:

- **İnternet 'e yönelik olmayan sanal makineler ağ güvenlik gruplarıyla korunmalıdır**

İnternet 'e yönelik sanal makineler, internet 'e yönelik ve internet 'e yönelik olmayan VM 'Ler arasında ayrım yapmadığından, var olan bir önerinin **ağ güvenlik gruplarıyla korunması gerekir**. Her iki için de bir VM bir ağ güvenlik grubuna atanmamışsa yüksek öneme sahip bir öneri oluşturulmuştur. Bu yeni öneri, internet 'e yönelik olmayan makineleri, yanlış pozitif sonuçları azaltmak ve gereksiz yüksek önem derecesine karşı önlemek için ayırır.

[Ağ önerileri](recommendations-reference.md#recs-network) tablosunda daha fazla bilgi edinin.




### <a name="new-policies-for-enabling-threat-protection-and-advanced-data-security"></a>Tehdit koruması ve gelişmiş veri güvenliğini etkinleştirmek için yeni ilkeler

Aşağıdaki yeni ilkeler ASC varsayılan girişimine eklenmiştir ve ilgili kaynak türleri için tehdit koruması veya gelişmiş veri güvenliğini etkinleştirme konusunda yardımcı olmak üzere tasarlanmıştır.

İlkeler Azure ilkesinde bulunabilir:


| İlke                                                                                                                                                                                                                                                                | İlke KIMLIĞI                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| [Azure SQL veritabanı sunucularında gelişmiş veri güvenliği etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f7fe3b40f-802b-4cdd-8bd4-fd799c948cc2)     | 7fe3b40f-802b-4CDD-8bd4-fd799c948cc2 |
| [Makinelerdeki SQL sunucularında gelişmiş veri güvenliği etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f6581d072-105e-4418-827f-bd446d56421b) | 6581d072-105E-4418-827f-bd446d56421b |
| [Gelişmiş tehdit koruması, Azure depolama hesaplarında etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f308fbb08-4ab8-4e67-9b29-592e93fb94fa)           | 308fbb08-4ab8-4e67-9b29-592e93fb94fa |
| [Gelişmiş tehdit koruması Azure Key Vault kasaları üzerinde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f0e6763cc-5078-4e64-889d-ff4d9a839047)           | 0e6763cc-5078-4e64-889d-ff4d9a839047 |
| [Gelişmiş tehdit koruması Azure App Service planlarda etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f2913021d-f2fd-4f3d-b958-22354e2bdbcb)                | 2913021d-f2fd-4f3d-b958-22354e2bdbcb |
| [Gelişmiş tehdit koruması Azure Container Registry kayıt defterlerinde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fc25d9a16-bc35-4e15-a7e5-9db606bf9ed4)   | c25d9a16-bc35-4e15-a7e5-9db606bf9ed4 |
| [Gelişmiş tehdit koruması, Azure Kubernetes hizmet kümelerinde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f523b5cd1-3e23-492f-a539-13118b6d1e3a)   | 523b5cd1-3e23-492F-A539-13118b6d1e3a |
| [Gelişmiş tehdit koruması sanal makinelerde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f4da35fc9-c9e7-4960-aec9-797fe7d9051d)           | 4ba35fc9-c9e7-4960-aec9-797fe7d9051d |
|                                                                                                                                                                                                                                                                       |                                      |

[Azure Güvenlik Merkezi 'Nde tehdit koruması](https://docs.microsoft.com/azure/security-center/threat-protection)hakkında daha fazla bilgi edinin.





## <a name="may-2020"></a>Mayıs 2020

İçindeki güncelleştirmeler şunlar olabilir:
- [Uyarı gizleme kuralları (Önizleme)](#alert-suppression-rules-preview)
- [Sanal makine güvenlik açığı değerlendirmesi genel kullanıma sunuldu](#virtual-machine-vulnerability-assessment-is-now-generally-available)
- [Tam zamanında (JıT) sanal makine (VM) erişiminde yapılan değişiklikler](#changes-to-just-in-time-jit-virtual-machine-vm-access)
- [Özel öneriler ayrı bir güvenlik denetimine taşındı](#custom-recommendations-have-been-moved-to-a-separate-security-control)
- [Denetimlere veya düz liste olarak önerilere bakmak için eklenen değiştirme](#toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list)
- [Genişletilmiş Güvenlik denetimi "en iyi güvenlik uygulamalarını uygulayın"](#expanded-security-control-implement-security-best-practices)
- [Özel meta verileri olan özel ilkeler artık genel kullanıma sunuldu](#custom-policies-with-custom-metadata-are-now-generally-available)
- [Kilitlenme bilgi döküm çözümleme özellikleri, dosya daha az saldırı algılamasına geçiriliyor](#crash-dump-analysis-capabilities-migrating-to-fileless-attack-detection)


### <a name="alert-suppression-rules-preview"></a>Uyarı gizleme kuralları (Önizleme)

Bu yeni Özellik (Şu anda önizlemede), uyarı fatıg azaltmaya yardımcı olur. Zararsız olarak bilinen veya kuruluşunuzdaki normal etkinliklerle ilgili olan uyarıları otomatik olarak gizlemek için kuralları kullanın. Bu, en ilgili tehditlere odaklanmanızı sağlar. 

Etkin gizleme kuralları ile eşleşen uyarılar üretilmeye devam eder, ancak durumları kapatıldı olarak ayarlanır. Durumu Azure portal görebilirsiniz, ancak Güvenlik Merkezi güvenlik uyarılarınıza erişebilirsiniz.

Gizleme kuralları, uyarıların otomatik olarak hangi ölçütlere göre kapatılacağını tanımlar. Genellikle bir gizleme kuralı kullanarak şunları yapabilirsiniz:

- yanlış pozitif sonuçlar olarak belirlediğiniz uyarıları gösterme

- çok sık tetiklenen uyarıları, faydalı olması için gösterme

[Azure Güvenlik Merkezi 'nin tehdit korumasından gelen uyarıları gizleme](alerts-suppression-rules.md)hakkında daha fazla bilgi edinin.


### <a name="virtual-machine-vulnerability-assessment-is-now-generally-available"></a>Sanal makine güvenlik açığı değerlendirmesi genel kullanıma sunuldu

Güvenlik Merkezi 'nin standart katmanı artık, ek ücret ödemeden sanal makinelere yönelik tümleşik bir güvenlik açığı değerlendirmesi içerir. Bu uzantı Qualys tarafından desteklenmektedir ancak bulgularını doğrudan güvenlik merkezi 'ne geri bildirir. Bir Qualys lisansına veya hatta bir Qualys hesabına ihtiyacınız yoktur. her şey güvenlik merkezi 'nin içinde sorunsuz bir şekilde işlenir.

Yeni çözüm Güvenlik Merkezi 'nde güvenlik açıklarını bulmak ve bulguları sunmak için sanal makinelerinizi sürekli tarayabilir. 

Çözümü dağıtmak için yeni güvenlik önerisini kullanın:

"Yerleşik güvenlik açığı değerlendirme çözümünü sanal makinelerde etkinleştir (Qualys tarafından desteklenir)"

[Güvenlik Merkezi 'nin sanal makineler için tümleşik güvenlik açığı değerlendirmesi](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner)hakkında daha fazla bilgi edinin.



### <a name="changes-to-just-in-time-jit-virtual-machine-vm-access"></a>Tam zamanında (JıT) sanal makine (VM) erişiminde yapılan değişiklikler

Güvenlik Merkezi, sanal makinelerinizin yönetim bağlantı noktalarını korumak için isteğe bağlı bir özellik içerir. Bu, deneme yanılma saldırılarına karşı en yaygın bir savunma biçimine karşı savunma sağlar.

Bu güncelleştirme, bu özelliğe aşağıdaki değişiklikleri getirir:

- Bir VM üzerinde JıT 'i etkinleştirmenizi öneren öneri yeniden adlandırıldı. Eskiden, "tam zamanında ağ erişim denetimi sanal makinelere uygulanmalıdır": "sanal makinelerin yönetim bağlantı noktaları tam zamanında ağ erişim denetimiyle korunmalıdır".

- Öneri yalnızca açık yönetim bağlantı noktaları varsa tetiklenir.

[JIT erişim özelliği](security-center-just-in-time.md)hakkında daha fazla bilgi edinin.


### <a name="custom-recommendations-have-been-moved-to-a-separate-security-control"></a>Özel öneriler ayrı bir güvenlik denetimine taşındı

Gelişmiş güvenli puanla sunulan bir güvenlik denetimi "en iyi güvenlik uygulamalarını uygulama" idi. Abonelikleriniz için oluşturulan tüm özel öneriler otomatik olarak bu denetime yerleştirildi. 

Özel önerilerinizi bulmayı kolaylaştırmak için, bunları "özel öneriler" olarak ayrılmış bir güvenlik denetimine taşıdık. Bu denetim, güvenli puanınızı etkilemez.

[Azure Güvenlik Merkezi 'Nde gelişmiş güvenli skor (Önizleme)](secure-score-security-controls.md)bölümünde güvenlik denetimleri hakkında daha fazla bilgi edinin.


### <a name="toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list"></a>Denetimlere veya düz liste olarak önerilere bakmak için eklenen değiştirme

Güvenlik denetimleri, ilgili güvenlik önerilerinin mantıksal gruplarıdır. Bunlar, savunmasız saldırı yüzeylerinizi yansıtır. Denetim, bu önerileri uygulamanıza yardımcı olan yönergelerden oluşan bir güvenlik önerileri kümesidir.

Kuruluşunuzun her bir saldırı yüzeyini ne kadar iyi güvenlik altına almak için her güvenlik denetiminin puanlarını gözden geçirin.

Varsayılan olarak, önerileriniz güvenlik denetimlerinde gösterilmiştir. Bu güncelleştirmeden, bunları bir liste olarak da görüntüleyebilirsiniz. Etkilenen kaynakların sistem durumuna göre sıralanmış basit liste olarak görüntülemek için, yeni ' denetimleri grupla ' seçeneğini kullanın. Geçiş, portaldaki listenin üzerinde bulunur.

Güvenlik denetimleri ve bu geçiş, yeni güvenli puan deneyiminin bir parçasıdır. Portalın içinden görüşlerinizi bize göndermeyi unutmayın.

[Azure Güvenlik Merkezi 'Nde gelişmiş güvenli skor (Önizleme)](secure-score-security-controls.md)bölümünde güvenlik denetimleri hakkında daha fazla bilgi edinin.

!["Denetimlere göre gruplandır" öneriler için değiştirme](\media\secure-score-security-controls\recommendations-group-by-toggle.gif)

### <a name="expanded-security-control-implement-security-best-practices"></a>Genişletilmiş Güvenlik denetimi "en iyi güvenlik uygulamalarını uygulayın" 

Gelişmiş güvenli puanla sunulan bir güvenlik denetimi "en iyi güvenlik uygulamalarını uygulama" dır. Bu denetimde bir öneri olduğunda, güvenli puanı etkilemez. 

Bu güncelleştirmeyle, üç öneri özgün olarak yerleştirildiği denetimlerden ve bu en iyi yöntemler denetimine taşınmıştır. Bu üç önerinin riskini başlangıçta düşündükten daha düşük olduğunu belirlediğimiz için bu adımı aldık.

Ayrıca, bu denetime iki yeni öneri eklenmiştir ve eklendi.

Taşınan üç öneri şunlardır:

- **MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir** (BAŞLANGıÇTA "MFA 'yı etkinleştir" denetiminde)
- **Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir** (başlangıçta "erişim ve izinleri yönetme" denetiminde)
- **Aboneliğiniz için en fazla 3 sahip** belirtilmelidir (başlangıçta "erişim ve izinleri yönetme" denetiminde)

Denetime eklenen iki yeni öneri şunlardır:

- **Konuk yapılandırma uzantısının Windows sanal makinelerinde (Önizleme) yüklü olması gerekir** . [Azure ilkesi Konuk yapılandırması](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration) kullanarak, sanal makinelerin içinde sunucu ve uygulama ayarlarına (yalnızca Windows) yönelik görünürlük sağlar.

- **Makinelerinizde Windows Defender Exploit Guard etkinleştirilmelidir (Önizleme)** -Windows Defender Exploit Guard, Azure Ilke Konuk yapılandırma aracısından yararlanır. Exploit Guard, kuruluşların güvenlik riskini ve üretkenlik gereksinimlerini (yalnızca Windows) dengeleyebilmesini sağlarken, cihazları birçok farklı saldırı vektörü ve çok sayıda kötü amaçlı yazılım saldırılarında yaygın olarak kullanılan blok davranışlarına karşı kilitlemek için tasarlanan dört bileşene sahiptir.

[Exploit Guard Ilkesi oluşturma ve dağıtma](https://docs.microsoft.com/mem/configmgr/protect/deploy-use/create-deploy-exploit-guard-policy)bölümünde Windows Defender Exploit Guard hakkında daha fazla bilgi edinin.

[Gelişmiş güvenli skor (Önizleme)](secure-score-security-controls.md)bölümünde güvenlik denetimleri hakkında daha fazla bilgi edinin.



### <a name="custom-policies-with-custom-metadata-are-now-generally-available"></a>Özel meta verileri olan özel ilkeler artık genel kullanıma sunuldu

Özel ilkeler artık Güvenlik Merkezi öneri deneyiminin, güvenli puanın ve mevzuat uyumluluk standartları panosunun bir parçasıdır. Bu özellik genel kullanıma sunulmuştur ve kuruluşunuzun güvenlik değerlendirmesi kapsamını güvenlik merkezi 'nde genişletmenizi sağlar. 

Azure ilkesinde özel bir girişim oluşturun, buna ilke ekleyin ve Azure Güvenlik Merkezi 'ne ekleyin ve öneriler olarak görselleştirin.

Artık özel öneri meta verilerini düzenleme seçeneğini de ekledik. Meta veri seçenekleri önem derecesi, düzeltme adımları, tehdit bilgileri ve daha fazlasını içerir.  

[Ayrıntılı bilgilerle özel önerilerinizi geliştirme](custom-security-policies.md#enhance-your-custom-recommendations-with-detailed-information)hakkında daha fazla bilgi edinin.



### <a name="crash-dump-analysis-capabilities-migrating-to-fileless-attack-detection"></a>Kilitlenme bilgi döküm çözümleme özellikleri, dosya daha az saldırı algılamasına geçiriliyor 

Windows kilitlenme dökümü Analizi (CDA) algılama yeteneklerini [dosya daha az saldırı algılamasında](https://docs.microsoft.com/azure/security-center/threat-protection#windows-fileless)tümleştireceğiz. Dosya daha az saldırı algılama analizi, Windows makineleri için aşağıdaki güvenlik uyarılarının gelişmiş sürümlerini getirir: kod ekleme keşfedilen, kendini gizleyen Windows modülü algılanan, shellcode keşfedilen ve şüpheli kod segmenti tespit etti.

Bu geçişin avantajlarından bazıları:

- **Proaktif ve zamanında kötü amaçlı yazılım algılama** -bir kilitlenme oluşması ve sonra kötü amaçlı yapıtları bulmak için analiz çalıştırma gibi CDA yaklaşım. Dosya kullanmayan saldırı algılama özelliğinin kullanılması, çalışırken bellek içi tehditlere yönelik proaktif kimlik tanımlamayı sağlar. 

- **Zenginleştirilmiş uyarılar** -dosya daha az saldırı algılamasında bulunan güvenlik uyarıları,, etkin ağ bağlantısı bilgileri gıbı, CDA ' den kullanılamayan zenginler içerir. 

- **Uyarı toplama** -CDA, tek çökme dökümü içinde birden çok saldırı deseni algıladığında birden çok güvenlik uyarısı tetikledi. Dosya Less saldırı algılaması, aynı işlemden alınan tüm saldırı düzenlerini tek bir uyarıya birleştirerek birden çok uyarıyı ilişkilendirme gereksinimini ortadan kaldırır.

- **Log Analytics çalışma alanınızda daha az gereksinim** vardır; potansiyel olarak hassas veriler içeren kilitlenme dökümleri artık Log Analytics çalışma alanınıza yüklenmeyecektir.



## <a name="april-2020"></a>Nisan 2020

Nisan 'daki güncelleştirmeler şunları içerir:
- [Dinamik uyumluluk paketleri artık genel kullanıma sunuldu](#dynamic-compliance-packages-are-now-generally-available)
- [Kimlik önerileri artık Azure Güvenlik Merkezi Ücretsiz katmanına eklenmiştir](#identity-recommendations-now-included-in-azure-security-center-free-tier)


### <a name="dynamic-compliance-packages-are-now-generally-available"></a>Dinamik uyumluluk paketleri artık genel kullanıma sunuldu

Azure Güvenlik Merkezi mevzuat uyumluluk panosu artık ek sektör ve yasal standartları izlemek için **dinamik uyumluluk paketleri** (genel kullanıma sunuldu) içerir.

Dinamik uyumluluk paketleri, Güvenlik Merkezi güvenlik ilkesi sayfasından aboneliğinize veya yönetim grubunuza eklenebilir. Bir standart veya kıyaslama eklendi olduğunuzda, değerlendirme olarak eşlenen tüm ilişkili uyumluluk verileri ile yasal uyumluluk panonuzda standart görüntülenir. Eklendi olan standartlardan herhangi biri için bir özet raporu indirilebilir.

Şimdi, şöyle bir standartlar ekleyebilirsiniz:

- **NIST SP 800-53 R4**
- **SWIFT CSP CSCF-V2020**
- **UK resmi ve UK NHS**
- **Kanada Federal PBMM**
- **Azure CIS 1.1.0 (yeni)** (Azure CIS 1.1.0 'ın daha kapsamlı bir gösterimi)

Ayrıca, Azure **güvenlik kıyaslaması**' nı son zamanlarda, genel uyumluluk çerçevelerine göre güvenlik ve uyum en iyi uygulamaları için Microsoft tarafından yazılan Azure 'a özgü yönergeleri ekledik. Panoda kullanılabilir hale geldiğinde ek standartlar desteklenecektir.  
 
[Yasal uyumluluk panonuzda standartlar kümesini özelleştirme](update-regulatory-compliance-packages.md)hakkında daha fazla bilgi edinin.


### <a name="identity-recommendations-now-included-in-azure-security-center-free-tier"></a>Kimlik önerileri artık Azure Güvenlik Merkezi Ücretsiz katmanına eklenmiştir

Azure Güvenlik Merkezi Ücretsiz katmanında kimlik ve erişime yönelik güvenlik önerileri artık genel kullanıma sunulmuştur. Bu, bulut güvenlik durure yönetimi (CSPM) özelliklerini ücretsiz hale getirme çabalarının bir parçasıdır. Şu anda bu öneriler yalnızca standart fiyatlandırma katmanında kullanılabilir.

Kimlik ve erişim önerilerine örnek olarak şunlar verilebilir:

- "Çok faktörlü kimlik doğrulaması, aboneliğiniz üzerinde sahip izinleri olan hesaplarda etkinleştirilmelidir."
- "Aboneliğiniz için en fazla üç sahip belirtilmelidir."
- "Kullanımdan kaldırılan hesaplar aboneliğinizden kaldırılmalıdır."

Ücretsiz fiyatlandırma katmanında abonelikleriniz varsa, bu değişiklik, kimlik ve erişim güvenliği için hiçbir şekilde değerlendirilmediğinden, bu değişiklikten etkilenmez.

[Kimlik ve erişim önerileri](recommendations-reference.md#recs-identity)hakkında daha fazla bilgi edinin.

[Kimlik ve erişim izleme](security-center-identity-access.md)hakkında daha fazla bilgi edinin.