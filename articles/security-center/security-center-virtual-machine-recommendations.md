---
title: Azure Güvenlik Merkezi 'nde sanal makine önerileri | Microsoft Docs
description: Bu belge, sanal makinelerinizi ve bilgisayarlarınızı ve Web uygulamalarınızı ve App Service ortamlarınızı korumanıza yardımcı olacak Azure Güvenlik Merkezi önerilerini açıklamaktadır.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f5ce7f62-7b12-4bc0-b418-4a2f9ec49ca1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2018
ms.author: rkarlin
ms.openlocfilehash: 798e027ca611905766b1fb8bcdb89cba4aeaf9b2
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69531329"
---
# <a name="understand-azure-security-center-resource-recommendations"></a>Azure Güvenlik Merkezi kaynak önerilerini anlama


## <a name="recommendations"></a>Öneriler
Kullanılabilir Işlem ve uygulama hizmetleri önerilerini ve uygulamayı uyguladığınızda bunlardan ne yaptığını anlamanıza yardımcı olması için aşağıdaki tabloları başvuru olarak kullanın.

### <a name="computers"></a>Bilgisayarlar
| Öneri | Açıklama |
| --- | --- |
| Abonelikler için veri toplamayı etkinleştirin|Her bir aboneliğiniz ve aboneliklerinizdeki tüm sanal makinelerine (VM) için güvenlik ilkesinde veri toplamayı etkinleştirmenizi önerir. |
| Azure Depolama Hesabı için şifrelemeyi etkinleştirin| Bekleyen veriler için Azure Depolama Hizmeti Şifrelemesi etkinleştirmenizi önerir. Depolama Hizmeti Şifrelemesi (SSE), Azure depolama 'ya yazıldığında verileri şifreleyerek ve almadan önce şifrelerini çözerek işe yarar. SSE Şu anda yalnızca Azure Blob hizmeti için kullanılabilir ve blok Blobları, sayfa Blobları ve ekleme Blobları için kullanılabilir. Daha fazla bilgi için bkz. [bekleyen veriler için depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).</br>SSE yalnızca Kaynak Yöneticisi depolama hesaplarında desteklenir. Klasik depolama hesapları Şu anda desteklenmiyor. Klasik ve Kaynak Yöneticisi dağıtım modellerini anlamak için bkz. Azure dağıtım modelleri. |
| Güvenlik yapılandırmalarını düzelt|İşletim sistemi yapılandırmalarınızı önerilen güvenlik yapılandırması kurallarıyla hizalamanızı önerir, Örneğin Parolaların kaydedilmesine izin vermeyin. |
| Sistem güncelleştirmelerini uygulayın |VM’lerinize eksik sistem güvenliği güncelleştirmelerini ve kritik güncelleştirmeleri dağıtmanızı önerir. |
| Anlık ağ erişim denetimi uygulama| Tam zamanında VM erişimi uygulamanızı önerir. Tam zamanında özelliği önizleme aşamasındadır ve Güvenlik Merkezi 'nin standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md). |
| Sistem güncelleştirmelerinden sonra sistemi yeniden başlatma|Sistem güncelleştirmelerini uygulama işlemini tamamlamak için VM’yi yeniden başlatmanızı önerir. |
| Endpoint Protection yükle|VM’lere (yalnızca Windows VM’leri) kötü amaçlı yazılımdan koruma programları sağlamanızı önerir. |
| VM Aracısını etkinleştirme |Hangi VM’lerin VM Aracısı gerektirdiğini görmenizi sağlar. Düzeltme eki tarama, temel tarama ve kötü amaçlı yazılımdan koruma programları sağlamak üzere VM’lere VM Aracısı yüklenmelidir. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. [VM Aracısı ve Uzantılar – 2. Kısım](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) makalesinde VM Aracısının nasıl yüklendiğine ilişkin bilgiler verilmektedir. |
| Disk şifrelemesi uygulayın|Azure Disk Şifrelemesi kullanarak VM’nizi şifrelemenizi önerir (Windows ve Linux VM’leri). Şifreleme hem işletim sistemi hem de VM’nizin üzerindeki veri birimleri için önerilir. |
| İşletim sistemi sürümünü güncelleştirme|Bulut hizmetiniz için işletim sistemi (OS) sürümünü, işletim sistemi aileniz için kullanılabilen en son sürüme güncelleştirmenizi önerir.  Cloud Services hakkında daha fazla bilgi edinmek için bkz. Cloud Services genel bakış. |
| Güvenlik açığı değerlendirmesi yüklü değil|Sanal makinenize bir güvenlik açığı değerlendirme çözümü yüklemenizi önerir. |
| Güvenlik açıklarını düzeltin) |VM’nize yüklü güvenlik açığı değerlendirme çözümü tarafından algılanan sistem ve uygulama güvenlik açıklarını görmenizi sağlar. |

### Uygulama Hizmetleri<a name="app-services"></a>
| Öneri | Açıklama |
| --- | --- |
| App Service yalnızca HTTPS üzerinden erişilebilir olmalıdır | App Service erişimini yalnızca HTTPS üzerinden sınırlandırmanızı önerir. |
| Web uygulaması için Web yuvalarını devre dışı bırakılmalıdır| Web uygulamaları içinde Web Yuvaları kullanımını dikkatle incelemenizi önerir.  Web yuvaları Protokolü farklı güvenlik tehdidi türlerine savunmasızdır. |
| Web uygulamanız için özel etki alanları kullanın | Bir Web uygulamasını, kimlik avı ve diğer DNS ile ilgili saldırıları gibi yaygın saldırılara karşı korumak için özel etki alanları kullanmanızı önerir. |
| Web uygulaması için IP kısıtlamalarını yapılandırma | Uygulamanıza erişmelerine izin verilen IP adreslerinin bir listesini tanımlamanızı önerir.  IP kısıtlamaları kullanımı, bir web uygulaması genel saldırılara karşı korur. |
| Tüm izin verme ('* ') uygulamanıza erişmek için kaynaklar | WEBSITE_LOAD_CERTIFICATES parametresini ' *' olarak ayarlayamamanızı önerir. Parametresinin '* ' olarak ayarlanması, tüm sertifikaların Web uygulamalarına ait kişisel sertifika deponuza yüklenebileceği anlamına gelir.  Site çalışma zamanında tüm sertifikalara erişim gerektiğini olası olmadığından bu en düşük öncelik ilkesini kötüye neden olabilir. |
| CORS, her kaynağın uygulamanıza erişmesine izin vermemelidir | Yalnızca gerekli etki alanlarının Web uygulamanızla etkileşime geçmesini sağlar. Kaynak kaynak paylaşımı (CORS) tüm etki alanları web uygulamanıza erişmek izin vermemelisiniz. |
| Kullanım en son .NET Framework Web uygulaması için desteklenir. | En son güvenlik sınıfları için en son .NET Framework sürümünü kullanmanızı önerir. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz. |
| Web uygulaması için desteklenen en son Java sürümünü kullanın | En son güvenlik sınıfları için en son Java sürümünü kullanmanızı önerir. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz. |
| Web uygulaması için desteklenen en son PHP sürümünü kullanın | En son güvenlik sınıfları için en son PHP sürümünü kullanmanızı önerir. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz. |
| [Web uygulaması güvenlik duvarı ekleme](security-center-add-web-application-firewall.md) |Web uç noktaları için bir Web uygulaması güvenlik duvarı (WAF) dağıtmanızı önerir. Açık gelen Web bağlantı noktaları (80.443) ile ilişkili bir ağ güvenlik grubuna sahip olan genel kullanıma açık IP (örnek düzeyi IP veya yük dengeli IP) için WAF önerisi gösterilmektedir.</br></br>Güvenlik Merkezi, sanal makinelerde ve App Service Ortamı üzerinde Web uygulamalarınızı hedefleme saldırılarına karşı savunmaya yardımcı olmak için bir WAF sağlamanızı önerir. App Service Ortamı (Ao), Azure App Service uygulamaları güvenli bir şekilde çalıştırmak için tam olarak yalıtılmış ve ayrılmış bir ortam sağlayan bir Azure App Service [Premium](https://azure.microsoft.com/pricing/details/app-service/) hizmet planı seçeneğidir. Ao hakkında daha fazla bilgi edinmek için [App Service ortamı belgelerine](../app-service/environment/intro.md)bakın.</br></br>Bu uygulamaları var olan WAF dağıtımlarınıza ekleyerek güvenlik merkezi 'nde birden çok Web uygulamasını koruyabilirsiniz. |
| [Uygulama korumasını sonlandırma](security-center-add-web-application-firewall.md#finalize-application-protection) |Bir WAF yapılandırmasını gerçekleştirmek için trafiğin WAF gerecine tekrar yönlendirilmelidir. Bu öneriyi takip eden, gerekli kurulum değişikliklerini tamamlar. |
| Web uygulaması için desteklenen en son Node.js sürümünü kullanın | En son güvenlik sınıfları için en son Node. js sürümünü kullanmanızı önerir. Eski sınıfları ve türleri kullanma, uygulamanızı saldırılara açık hale getirebilirsiniz. |
| CORS her kaynağın işlev uygulamanıza erişmek izin vermemelidir | Yalnızca gerekli etki alanlarının Web uygulamanızla etkileşime geçmesini sağlar. Kaynak kaynak paylaşımı (CORS) tüm etki alanlarının işlev uygulamanıza erişmek izin vermemelisiniz. |
| İşlev uygulaması için özel etki alanları kullanın | Bir işlev uygulamasını kimlik avı ve diğer DNS ile ilgili saldırılar gibi yaygın saldırılara karşı korumak için özel etki alanları kullanmanızı önerir. |
| İşlev uygulaması için IP kısıtlamalarını yapılandırma | Uygulamanıza erişmelerine izin verilen IP adreslerinin bir listesini tanımlamanızı önerir. IP kısıtlamaları kullanımını bir işlev uygulaması genel saldırılara karşı korur. |
| İşlev uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır | Işlev uygulamalarının erişimini yalnızca HTTPS üzerinden sınırlandırmanızı önerir. |
| İşlev Uygulaması için uzaktan hata ayıklama kapatılmalıdır | Artık kullanmanıza gerek kalmadığında İşlev Uygulaması için hata ayıklamayı kapatmanızı önerir. Uzaktan hata ayıklama, gelen bağlantı noktası üzerinde bir işlev uygulaması açılmasını gerektirir. |
| İşlev uygulaması için Web yuvalarını devre dışı bırakılmalıdır | Işlev uygulamaları dahilinde Web Yuvaları kullanımını dikkatle incelemenizi önerir. Web yuvaları Protokolü farklı güvenlik tehdidi türlerine savunmasızdır. |


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Kimlik ve erişim Azure Güvenlik Merkezi'nde izleme](security-center-identity-access.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
* [Azure Güvenlik Merkezi'nde Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png
