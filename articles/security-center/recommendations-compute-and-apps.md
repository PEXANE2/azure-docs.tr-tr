---
title: Uygulamalar & makineler için Azure Güvenlik Merkezi önerileri
description: Azure Güvenlik Merkezi 'nin, sanal makinelerinizi, bilgisayarlarınızı, Web uygulamalarınızı ve App Service ortamlarınızı korumanıza yardımcı olan güvenlik önerileri.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2019
ms.author: memildin
ms.openlocfilehash: c0cf5951daf004a0c805b688f08d1ccfa4679615
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74782170"
---
# <a name="compute-and-app-recommendations---reference-guide"></a>İşlem ve uygulama önerileri-başvuru kılavuzu

Bu makale, Azure Güvenlik Merkezi 'nde aşağıdaki kaynak türleriyle ilgili olarak görebileceğiniz önerilerin tam listesini sağlar:

* VM 'Ler ve bilgisayarlar
* VM Ölçek Kümeleri
* Bulut Hizmetleri
* Uygulama hizmetleri
* Kapsayıcılar
* İşlem kaynakları

Bunların nasıl bulunacağını ve nasıl çözümlendiğinin bir açıklaması için [buraya](security-center-virtual-machine-protection.md)bakın.

## İşlem ve uygulama önerileri<a name="compute-and-app-recs"></a>
|Kaynak türü|Güvenlik puanı|Öneri|Açıklama|
|----|----|----|----|
|App Service|20|Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Web uygulamalarına erişimi yalnızca HTTPS üzerinden sınırlayın.|
|App Service|20|İşlev Uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Işlev uygulamalarının erişimini yalnızca HTTPS üzerinden sınırlayın.|
|App Service|5|Uygulama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir|Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye atılırsa araştırma amaçlarıyla etkinlik izlerini yeniden oluşturmayı sağlar. |
|App Service|10|Web uygulaması için uzaktan hata ayıklama kapatılmalıdır|Artık kullanmanız gerekmiyorsa Web uygulamaları için hata ayıklamayı kapatın. Uzaktan hata ayıklama, bir İşlev Uygulaması gelen bağlantı noktalarının açılmasını gerektirir.|
|App Service|10|Işlev uygulaması için uzaktan hata ayıklama kapatılmalıdır|Artık kullanmanız gerekmiyorsa İşlev Uygulaması için hata ayıklamayı kapatın. Uzaktan hata ayıklama, bir İşlev Uygulaması gelen bağlantı noktalarının açılmasını gerektirir.|
|App Service|10|Tüm (' * ') kaynaklarının uygulamanıza erişmesine izin verme| WEBSITE_LOAD_CERTIFICATES parametre kümesine "" olarak izin vermeyin. Parametresinin ' ' olarak ayarlanması, tüm sertifikaların Web uygulamalarına ait kişisel sertifika deposuna yüklendiği anlamına gelir. Bu, sitenin çalışma zamanında tüm sertifikalara erişmesi gerektiğinden, en az ayrıcalık ilkesinin kötüye kullanılmasına neden olabilir.|
|App Service|20|CORS, her kaynağın Web uygulamalarınıza erişmesine izin vermemelidir|Yalnızca gerekli etki alanlarının Web uygulamanızla etkileşime girmesine izin verin. Çapraz kaynak kaynak paylaşımı (CORS), tüm etki alanlarının Web uygulamanıza erişmesine izin vermemelidir.|
|App Service|20|CORS, her kaynağın İşlev Uygulaması erişmesine izin vermemelidir| Yalnızca gerekli etki alanlarının işlev uygulamanızla etkileşime girmesine izin verin. Çapraz kaynak kaynak paylaşımı (CORS), tüm etki alanlarının işlev uygulamanıza erişmesine izin vermemelidir.|
|İşlem kaynakları (Batch)|1|Toplu Iş hesaplarında ölçüm uyarısı kuralları yapılandırılmalıdır|Batch hesabında ölçüm uyarı kurallarını yapılandırma ve ölçüm havuzunu etkinleştirme Tamamlanan olayları ve havuz silme başlangıç olaylarını silme|
|İşlem kaynakları (Service Fabric)|10|Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır|Istemci kimlik doğrulamasını yalnızca Service Fabric Azure Active Directory aracılığıyla gerçekleştirin.|
|İşlem kaynakları (Otomasyon hesabı)|5|Otomasyon hesabı değişkenleri şifrelenmelidir|Hassas verileri depolarken Otomasyon hesabı değişken varlıklarının şifrelenmesini etkinleştirin.|
|İşlem kaynakları (arama)|5|Arama Hizmetleri için tanılama günlüklerini etkinleştirme denetimi|Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye atılırsa araştırma amaçlarıyla etkinlik izlerini yeniden oluşturmayı sağlar. |
|İşlem kaynakları (hizmet veri yolu)|5|Service Bus tanılama günlükleri etkinleştirilmelidir|Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye atılırsa araştırma amaçlarıyla etkinlik izlerini yeniden oluşturmayı sağlar. |
|İşlem kaynakları (Stream Analytics)|5|Azure Stream Analytics tanılama günlükleri etkinleştirilmelidir|Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye atılırsa araştırma amaçlarıyla etkinlik izlerini yeniden oluşturmayı sağlar. |
|İşlem kaynakları (Batch)|5|Toplu Iş hesaplarında tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye atılırsa araştırma amaçlarıyla etkinlik izlerini yeniden oluşturmayı sağlar. |
|İşlem kaynakları (Olay Hub 'ı)|5|Olay Hub 'ındaki tanılama günlükleri etkinleştirilmelidir|Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye atılırsa araştırma amaçlarıyla etkinlik izlerini yeniden oluşturmayı sağlar. |
|İşlem kaynakları (Logic Apps)|5|Logic Apps tanılama günlüklerini etkinleştirme|Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye atılırsa araştırma amaçlarıyla etkinlik izlerini yeniden oluşturmayı sağlar. |
|İşlem kaynakları (Service Fabric)|15|ClusterProtectionLevel özelliğini EncryptAndSign olarak ayarlayın Service Fabric|Service Fabric, birincil küme sertifikası kullanarak düğümden düğüme iletişim için üç koruma düzeyi (None, Sign ve EncryptAndSign) sağlar. Tüm düğümden düğüme mesajların şifrelendiğinden ve dijital olarak imzalandığından emin olmak için koruma düzeyini ayarlayın. |
|İşlem kaynakları (hizmet veri yolu)|1|Service Bus ad alanından RootManageSharedAccessKey hariç tüm yetkilendirme kurallarını kaldır |Service Bus istemcileri, bir ad alanındaki tüm kuyruklara ve konulara erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamalıdır. En az ayrıcalık güvenlik modeliyle uyum sağlamak için, yalnızca belirli varlığa erişim sağlamak üzere kuyruklar ve konular için varlık düzeyinde erişim ilkeleri oluşturmanız gerekir.|
|İşlem kaynakları (Olay Hub 'ı)|1|RootManageSharedAccessKey hariç tüm yetkilendirme kuralları, Olay Hub 'ı ad alanından kaldırılmalıdır|Olay Hub 'ı istemcileri, bir ad alanındaki tüm kuyruklara ve konulara erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamalıdır. En az ayrıcalık güvenlik modeliyle uyum sağlamak için, yalnızca belirli varlığa erişim sağlamak üzere kuyruklar ve konular için varlık düzeyinde erişim ilkeleri oluşturmanız gerekir.|
|İşlem kaynakları (Olay Hub 'ı)|5|Olay Hub 'ı varlığındaki yetkilendirme kuralları tanımlanmalıdır|En az ayrıcalıklı erişim sağlamak için Olay Hub 'ı varlığındaki yetkilendirme kurallarını denetleyin.|
|Makine|50|Makinelerinize izleme Aracısı 'nı yükler|Her makinede veri toplamayı, güncelleştirme taramayı, temel taramayı ve Endpoint Protection 'ı etkinleştirmek için Izleme aracısını yükler.|
|Makine|50|Abonelikleriniz için otomatik sağlamayı ve veri toplamayı etkinleştirin |Aboneliklerinize eklenen her makinede veri toplamayı, güncelleştirme taramayı, temel taramayı ve Endpoint Protection 'ı etkinleştirmek için aboneliklerinizdeki makineler için otomatik sağlamayı ve veri toplamayı etkinleştirin.|
|Makine|40|Makinelerinizdeki izleme Aracısı sistem durumu sorunlarını çözün|Tam Güvenlik Merkezi koruması için, sorun giderme kılavuzundaki yönergeleri izleyerek makinelerinizdeki izleme Aracısı sorunlarını çözün| 
|Makine|40|Makinelerinizdeki Endpoint Protection sistem durumu sorunlarını çözün|Tam Güvenlik Merkezi koruması için, sorun giderme kılavuzundaki yönergeleri izleyerek makinelerinizdeki izleme Aracısı sorunlarını çözün.|
|Makine|40|Makinelerinizde eksik tarama verilerinin sorunlarını giderme|Sanal makinelerde ve bilgisayarlarda eksik tarama verilerinde sorun giderin. Makinelerinizde eksik tarama verileri, güncelleştirme taraması, temel tarama ve eksik Endpoint Protection çözüm taraması gibi eksik güvenlik değerlendirmelerine neden olur.|
|Makine|40|Sistem güncelleştirmelerinin makinelerinizde yüklü olması gerekir|Windows ve Linux sanal makinelerinizi ve bilgisayarlarınızı güvenli hale getirmek için eksik sistem güvenliği ve kritik güncelleştirmeleri yükler
|Makine|15|Web uygulaması güvenlik duvarı ekleme| Web uygulamalarınızı güvenli hale getirmek için bir Web uygulaması güvenlik duvarı (WAF) çözümü dağıtın. |
|Makine|40|Bulut hizmeti rolleriniz için işletim sistemi sürümünü güncelleştirme|Bulut hizmeti rolleriniz için işletim sistemi (OS) sürümünü, işletim sistemi aileniz için kullanılabilen en son sürüme güncelleştirin.|
|Makine|35|Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir|Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıklarını, saldırılara karşı korumak için düzeltin.|
|Makine|35|Kapsayıcılarınızdaki güvenlik yapılandırmasındaki güvenlik açıklarını düzeltin|Programları saldırılara karşı korumak için Docker yüklü makinelerde güvenlik yapılandırmasındaki güvenlik açıklarını düzeltin.|
|Makine|25|Uyarlamalı uygulama denetimlerini etkinleştir|Azure 'da bulunan VM 'leriniz üzerinde hangi uygulamaların çalıştırılabileceği denetlemek için uygulama denetimini etkinleştirin. Bu, VM 'lerinizi kötü amaçlı yazılımlara karşı korumanıza yardımcı olur. Güvenlik Merkezi, her bir VM 'de çalışan uygulamaları çözümlemek için makine öğrenimini kullanır ve bu zekası kullanarak izin verme kuralları uygulamanıza yardımcı olur. Bu özellik, uygulama izin verme kurallarını yapılandırma ve sürdürme sürecini basitleştirir.|
|Makine|20|Makinelerinize Endpoint Protection çözümünü yükler|Sanal makinelerinize ve güvenlik açıklarına karşı korumak için bir uç nokta koruma çözümü yüklersiniz.|
|Makine|20|Sistem güncelleştirmelerini uygulamak için makinelerinizi yeniden başlatın|Sistem güncelleştirmelerini uygulamak ve makinenin güvenlik açıklarına karşı güvenliğini sağlamak için makinelerinizi yeniden başlatın.|
|Makine|15|Disk şifrelemesi sanal makinelere uygulanmalıdır|Hem Windows hem de Linux sanal makineleri için Azure disk şifrelemesi 'ni kullanarak sanal makine disklerinizi şifreleyin. Azure disk şifrelemesi (ADE), Windows 'un sektör standardı BitLocker özelliği ve Linux 'un DM-Crypt özelliğini kullanır ve verilerinizi korumanıza ve korumanıza yardımcı olmak için işletim sistemi ve veri diski şifrelemesi sağlar ve kurumsal güvenlik ve uyumluluğunuzu karşılamanıza yardımcı olur Müşteri Azure Anahtar Kasası 'ndaki taahhütler. Uyumluluk ve güvenlik gereksiniminize, kısa ömürlü (yerel olarak bağlanmış geçici) disk şifrelemesi de dahil olmak üzere, şifreleme anahtarlarınızı kullanarak veri uçtan uca şifrelemeyi gerektirdiğinde Azure disk şifrelemesi ' ni kullanın. Alternatif olarak, varsayılan olarak yönetilen diskler varsayılan olarak, şifreleme anahtarlarının Azure 'da Microsoft tarafından yönetilen anahtarlar olduğu Azure Depolama Hizmeti Şifrelemesi kullanılarak şifrelenir. Bu, uyumluluk ve güvenlik gereksinimlerinizi karşılıyorsa, gereksinimlerinizi karşılamak için varsayılan yönetilen disk şifrelemesi üzerinden yararlanabilirsiniz.|
|Makine|30|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yüklemesi|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yüklemesi|
|Makine|15|Web uygulaması güvenlik duvarı ekleme| Web uygulamalarınızı güvenli hale getirmek için bir Web uygulaması güvenlik duvarı (WAF) çözümü dağıtın. |
|Makine|30|Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir|Bir güvenlik açığı değerlendirmesi 3. taraf çözümü dağıtılan sanal makineler, uygulama ve işletim sistemi açıklarına karşı sürekli olarak değerlendirilir. Bu güvenlik açıkları bulunduğunda, önerinin bir parçası olarak daha fazla bilgi için kullanılabilir.|
|Makine|30|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yüklemesi|Sanal makinelerinize bir güvenlik açığı değerlendirme çözümü yüklemesi|
|Makine|1|Sanal makinelerin yeni Azurerd kaynaklarına geçirilmesi gerekir|Sanal makineleriniz için Azure Resource Manager kullanarak: daha güçlü erişim denetimi (RBAC), daha iyi denetim, Kaynak Yöneticisi tabanlı dağıtım ve idare, yönetilen kimliklere erişim, gizli dizi için anahtar kasasına erişim, Daha kolay güvenlik yönetimi için Azure AD tabanlı kimlik doğrulaması ve Etiketler ve kaynak grupları desteği. |
|Makine|30|Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir|Bir güvenlik açığı değerlendirmesi 3. taraf çözümü dağıtılan sanal makineler, uygulama ve işletim sistemi açıklarına karşı sürekli olarak değerlendirilir. Bu güvenlik açıkları bulunduğunda, önerinin bir parçası olarak daha fazla bilgi için kullanılabilir.|
|Sanal makine ölçek kümesi |4|Sanal makine ölçek kümelerindeki tanılama günlükleri etkinleştirilmelidir|Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, araştırma amaçları için etkinlik izlerini yeniden oluşturmayı sağlar. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye girerse yararlıdır.|
|Sanal makine ölçek kümesi|35|Sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir|Saldırılara karşı korumak için sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıklarını düzeltin. |
|Sanal makine ölçek kümesi|5|Sanal makine ölçek kümelerinde Endpoint Protection sistem durumu başarısızlıklarını düzelt|Sanal makine ölçek kümelerinizi tehdit ve güvenlik açıklarına karşı korumak için Endpoint Protection sistem durumu başarısızlıklarını düzeltin. |
|Sanal makine ölçek kümesi|10|Endpoint Protection 'ın sanal makinelere yüklenmesi gerekir|Sanal makine ölçek kümelerinizi tehdit ve güvenlik açıklarına karşı korumak için bir uç nokta koruma çözümü yüklemek. |
|Sanal makine ölçek kümesi|40|Sanal makine ölçek kümelerindeki sistem güncelleştirmeleri yüklenmelidir|Windows ve Linux sanal makine ölçek kümelerinizin güvenliğini sağlamak için eksik sistem güvenliği ve kritik güncelleştirmeleri yükler. |
|


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türlerine uygulanan öneriler hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme](security-center-identity-access.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
* [Azure Güvenlik Merkezi 'nde Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)
