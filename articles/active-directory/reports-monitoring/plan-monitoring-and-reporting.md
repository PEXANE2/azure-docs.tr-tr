---
title: Raporları & izleme dağıtımı - Azure AD
description: Raporlama ve izleme nin nasıl planlanıp yürütüldöğünü açıklar.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: baselden
ms.reviewer: plenzke
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ad84b8910e8d4f8af9845c33c22d128e317dedc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74232114"
---
# <a name="plan-an-azure-active-directory-reporting-and-monitoring-deployment"></a>Azure Etkin Dizin raporlama ve izleme dağıtımı planlama

Azure Etkin Dizin (Azure AD) raporlama ve izleme çözümünüz yasal, güvenlik ve operasyonel gereksinimlerinize ve mevcut ortamınıza ve süreçlerinize bağlıdır. Bu makalede, çeşitli tasarım seçenekleri sunar ve doğru dağıtım stratejisi için size rehberlik eder.

### <a name="benefits-of-azure-ad-reporting-and-monitoring"></a>Azure AD raporlama ve izlemenin avantajları

Azure REKLAM raporlaması, etkinliklerde oturum açma, denetim etkinlikleri ve dizininizdeki değişiklikler de dahil olmak üzere ortamınızdaki Azure REKLAM etkinliğinin kapsamlı bir görünümünü ve günlüklerini sağlar.

Sağlanan verilerle:

* uygulamalarınızın ve hizmetlerinizin nasıl kullanıldığını belirleyin.

* çevrenizin sağlığını etkileyen potansiyel riskleri tespit etmek.

* kullanıcılarınızın işlerini halletmesini engelleyen sorunları giderir.

* Azure REKLAM dizininizdeki değişikliklerin denetim olaylarını görerek öngörüler elde edin.

> [!IMPORTANT]
> Azure AD izleme, Azure AD raporlaması tarafından oluşturulan günlüklerinizi farklı hedef sistemlere yönlendirmenize olanak tanır. Ardından bu günlükleri uzun vadeli kullanım için saklayabilir veya ortamınızla ilgili içgörülere ulaşmak için üçüncü taraf Güvenlik Bilgileri ve Olay Yönetimi (SIEM) araçlarıyla tümleştirebilirsiniz.

Azure AD izleme ile günlükleri şu şekilde yönlendirebilirsiniz:

* arşivleme amaçları için bir Azure depolama hesabı.
* Verileri analiz edebileceğiniz, panolar oluşturabileceğiniz ve belirli olaylarhakkında uyarıda verebileceğiniz Azure Log Analytics çalışma alanı olarak bilinen Azure Monitor günlükleri.
* Splunk, Sumlogic veya QRadar gibi mevcut SIEM araçlarınızla tümleştirebileceğiniz bir Azure etkinlik merkezi.

> [!NOTE]
Kısa bir süre önce Log Analytics yerine Azure Monitor günlükleri terimini kullanmaya başladık. Günlük verileri hala bir Log Analytics çalışma alanında depolanır ve yine de aynı Log Analytics hizmeti tarafından toplanır ve analiz edilir. [Azure Monitor'daki günlüklerin](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection)rolünü daha iyi yansıtacak şekilde terminolojiyi güncelliyoruz. Ayrıntılar için [Azure Monitör terminolojisi değişikliklerine](https://docs.microsoft.com/azure/azure-monitor/azure-monitor-rebrand) bakın.

[Rapor bekletme ilkeleri hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-reports-data-retention)

### <a name="licensing-and-prerequisites-for-azure-ad-reporting-and-monitoring"></a>Azure AD raporlaması ve izleme için lisanslama ve ön koşullar

Azure AD oturum günlüğüne erişmek için Azure AD premium lisansına ihtiyacınız vardır.

[Azure Active Directory fiyatlandırma kılavuzunda](https://azure.microsoft.com/pricing/details/active-directory/)ayrıntılı özellik ve lisans bilgileri için.

Azure AD izleme ve raporlamayı dağıtmak için Azure AD kiracısı için genel yönetici veya güvenlik yöneticisi olan bir kullanıcıya ihtiyacınız olur.

Günlük verilerinizin son hedefine bağlı olarak aşağıdakilerden birine ihtiyacınız vardır:

* ListKeys izinlerine sahip olduğunuz bir Azure depolama hesabı. Blob depolama hesabı değil genel bir depolama hesabı kullanmanızı öneririz. Depolamayla fiyatlandırma bilgileri için bkz. [Azure Depolama fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=storage).

* Üçüncü taraf SIEM çözümleriyle tümleştirmek için bir Azure Etkinlik Hub'ları ad alanı.

* Günlükleri Azure Monitor günlüklerine göndermek için bir Azure Günlük Analizi çalışma alanı.

## <a name="plan-an-azure-reporting-and-monitoring-deployment-project"></a>Bir Azure raporlama ve izleme dağıtım projesi planlama

Bu projede, raporları tüketecek ve izleyecek hedef kitleleri ve Azure AD izleme mimarinizi tanımlarsınız.

### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlarla etkileşimde bulunun

Teknoloji projeleri başarısız olduğunda, genellikle etki, sonuçlar ve sorumluluklar hakkındaki uyumsuz beklentiler nedeniyle bunu yaparlar. Bu tuzaklardan kaçınmak için, [doğru paydaşlarla etkileşimde bulunduğunuzdan emin olun.](https://aka.ms/deploymentplans) Ayrıca, paydaşların ve bunların proje girdive hesap yeteneklerini belgeleyerek projedeki paydaş rollerinin iyi anlaşılmasını sağlamak.

### <a name="plan-communications"></a>İletişimi planlama

İletişim, herhangi bir yeni hizmetin başarısı için çok önemlidir. Kullanıcılarınızla deneyimlerinin nasıl değişeceğini, ne zaman değişeceğini ve sorunlarla karşılaşırsa nasıl destek kazanabileceklerini proaktif bir şekilde iletin.

### <a name="document-your-current-infrastructure-and-policies"></a>Mevcut altyapınızı ve ilkelerinizi belgeleme

Mevcut altyapınız ve politikalarınız raporlama ve izleme tasarımınızı yönlendirecektir. Bildiğinizden emin olun

* Kullandığınız SIEM araçları ne, varsa.

* Mevcut depolama hesapları ve kullanılan izleme de dahil olmak üzere Azure altyapınız.

* Gerekli geçerli uyumluluk çerçeveleri de dahil olmak üzere günlükler için kuruluş bekletme ilkeleriniz. 

## <a name="plan-an-azure-ad-reporting-and-monitoring-deployment"></a>Azure AD raporlaması ve dağıtımını izleme planı

Raporlama ve izleme, iş gereksinimlerinizi karşılamak, kullanım alışkanlıkları hakkında öngörüler elde etmek ve kuruluşlarınuzun güvenlik duruşunu artırmak için kullanılır.

### <a name="business-use-cases"></a>İş kullanım örnekleri

* İş gereksinimlerini karşılamak için çözüm için gerekli
* İş gereksinimlerini karşılamak güzel
* Uygulanamaz

|Alan |Açıklama |
|-|-|
|Bekletme| **30 günden fazla günlük tutma.** Yasal veya iş gereksinimleri nedeniyle, denetim günlüklerini depolamak ve Azure AD günlüklerini 30 günden uzun süre oturum açmak gerekir. |
|Analiz| **Günlüklerin aranabilir olması gerekir.** Depolanan günlüklerin analitik araçlarla aranabilir olması gerekir. |
| Operasyonel İçgörüler| **Çeşitli takımlar için Insights**. Uygulama kullanımı, oturum açma hataları, self servis kullanımı, eğilimler gibi operasyonel öngörüler elde etmek için farklı kullanıcılara erişim sağlama gereksinimi. |
| Güvenlik Öngörüleri| **Çeşitli takımlar için Insights**. Uygulama kullanımı, oturum açma hataları, self servis kullanımı, eğilimler, vb. gibi operasyonel öngörüler elde etmek için farklı kullanıcılara erişim sağlama gereksinimi. |
| SIEM sistemlerine entegrasyon      | **SIEM entegrasyonu**. Mevcut SIEM sistemlerine Azure AD oturum açma ve denetleme günlüklerini entegre etme ve akışla aktarma gereksinimi. |

### <a name="choose-a-monitoring-solution-architecture"></a>İzleme çözümü mimarisini seçin

Azure AD izleme ile Azure AD etkinlik günlüklerinizi işletme gereksinimlerinizi en iyi şekilde karşılayan bir sisteme yönlendirebilirsiniz. Daha sonra, ortamınız hakkında bilgi edinmek ve SIEM araçlarıyla entegre etmek için bunları uzun vadeli raporlama ve analiz ler için saklayabilirsiniz.

#### <a name="decision-flow-chartan-image-showing-what-is-described-in-subsequent-sections"></a>Karar akış şeması![Sonraki bölümlerde açıklananları gösteren bir resim](media/reporting-deployment-plan/deploy-reporting-flow-diagram.png)

#### <a name="archive-logs-in-a-storage-account"></a>Depolama hesabındaki günlükleri arşivleme

Günlükleri bir Azure depolama hesabına yönlendirme yaparak, bunları [bekletme ilkelerimizde](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-reports-data-retention)özetlenen varsayılan bekletme süresinden daha uzun süre saklayabilirsiniz. Günlüklerinizi arşivlemeniz gerekiyorsa, ancak bunları bir SIEM sistemiyle tümleştirmeniz gerekmiyorsa ve devam eden sorgular ve çözümlemelere gerek yoksa bu yöntemi kullanın. İsteğe bağlı aramalar yapmaya devam edebilirsiniz.

[Verileri depolama hesabınıza yönlendirmeyi](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account) öğrenin.

#### <a name="send-logs-to-azure-monitor-logs"></a>Günlükleri Azure Monitor günlüklerine gönderme

[Azure Monitor günlükleri,](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) farklı kaynaklardan gelen izleme verilerini birleştirir. Ayrıca, uygulamalarınızın işleyişi ve kaynakların kullanımı hakkında bilgi veren bir sorgu dili ve analiz motoru da sağlar. Azure AD etkinlik günlüklerini Azure Monitor günlüklerine göndererek, toplanan verileri hızla alabilir, izleyebilir ve uyarabilirsiniz. Verilerinizi doğrudan göndermek istediğiniz ancak sorgu ve çözümleme istediğiniz varolan bir SIEM çözümünüz yoksa bu yöntemi kullanın. Verileriniz Azure Monitor günlüklerine ulaştıktan sonra, isterseniz etkinlik merkezine ve oradan da bir SIEM'e gönderebilirsiniz.

[Azure Monitor günlüklerine nasıl veri göndereceğinizi](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)öğrenin.

Oturum açma ve denetleme etkinlikleriyle ilgili yaygın senaryoları izlemek için Azure AD etkinlik günlükleri için önceden oluşturulmuş görünümleri de yükleyebilirsiniz.

[Azure AD etkinlik günlükleri için günlük analizi görünümlerini nasıl yükleyip kullanacağınızı](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-install-use-log-analytics-views)öğrenin.

#### <a name="stream-logs-to-your-azure-event-hub"></a>Azure etkinlik merkezinize akış günlükleri

Günlükleri bir Azure etkinlik merkezine yönlendirme, üçüncü taraf SIEM araçlarıyla tümleştirmeyi sağlar. Bu tümleştirme, azure REKLAM etkinliği günlük verilerini, çevrenize daha zengin bakış açısı sağlamak için SIEM'iniz tarafından yönetilen diğer verilerle birleştirmenize olanak tanır. 

[Olay hub'ına günlük akışı yapmayı](https://docs.microsoft.com//azure/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub) öğrenin.

## <a name="plan-operations-and-security-for-azure-ad-reporting-and-monitoring"></a>Azure AD raporlaması ve izleme için İşlemleri ve Güvenliği Planlayın

Hissedarların operasyonel öngörüler elde etmek için Azure REKLAM günlüklerine erişmesi gerekir. Olası kullanıcılar arasında güvenlik ekibi üyeleri, dahili veya harici denetçiler ve kimlik ve erişim yönetimi işlemleri ekibi yer almaktadır.

Azure AD rolleri, azure REKLAM Raporlarını rolünüze göre yapılandırma ve görüntüleme özelliğini devretmenizi sağlar. Kuruluşunuzdaki lerin Azure REKLAM raporlarını okumak için kimlerin izin alması gerektiğini ve bunlar için hangi rolün uygun olacağını belirleyin. 

Aşağıdaki roller Azure REKLAM raporlarını okuyabilir:

* Genel Yönetici

* Güvenlik Yöneticisi

* Güvenlik Okuyucu

* Rapor Okuyucu

[Azure AD Yönetim Rolleri](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal)hakkında daha fazla bilgi edinin.

*Her zaman bir hesap uzlaşma riskini azaltmak için en az ayrıcalıklar kavramını uygulayın.* Kuruluşunuzun güvenliğini sağlamak için [Ayrıcalıklı Kimlik Yönetimi'ni](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) uygulamayı düşünün.

##  

## <a name="deploy-azure-ad-reporting-and-monitoring"></a>Azure AD raporlama ve izleme dağıtma

Yukarıdaki tasarım kılavuzunu kullanarak daha önce vermiş olduğunuz kararlara bağlı olarak, bu bölüm sizi farklı dağıtım seçenekleriyle ilgili belgelere yönlendirecektir.

### <a name="consume-and-archive-azure-ad-logs"></a>Azure REKLAM günlüklerini tüketin ve arşivleyin

[Azure portalda etkinlik raporlarını bulma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-find-activity-reports)

[Azure AD günlüklerini bir Azure Depolama hesabına arşivleyin](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account)

### <a name="implement-monitoring-and-analytics"></a>İzleme ve analiz uygulayın

[Günlükleri Azure Monitör'e gönderme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

[Azure Active Directory için günlük analizi görünümlerini yükleme ve kullanma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-install-use-log-analytics-views)

[Azure Monitor günlükleriyle Azure REKLAM etkinlik günlüklerini analiz edin](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics)

* [Azure İzleyici denetim günlükleri şemasını yorumlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema)

* [Azure Monitor'da oturum açma şemasıyorumlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema)

 * [Azure REKLAM günlüğünü bir Azure etkinlik merkezine aktarın](https://docs.microsoft.com/azure/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub)

* [Azure İzleyici kullanarak Azure AD günlüklerini Splunk ile tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/tutorial-integrate-activity-logs-with-splunk)

* [Azure İzleyici kullanarak Azure AD günlüklerini SumoLogic ile tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic)

 

 

## <a name="next-steps"></a>Sonraki adımlar

[Ayrıcalıklı Kimlik Yönetimi'ni](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) uygulamayı düşünün 

[Rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) uygulama yı düşünün

 
