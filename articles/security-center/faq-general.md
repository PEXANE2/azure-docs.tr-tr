---
title: Azure Güvenlik Merkezi SSS-Genel sorular
description: Tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olan bir ürün olan Azure Güvenlik Merkezi hakkında sık sorulan genel sorular
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: e2cc04be5a79009f92e275f9de46df60a8e23c8b
ms.sourcegitcommit: 354a302d67a499c36c11cca99cce79a257fe44b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82106585"
---
# <a name="faq---general-questions-about-azure-security-center"></a>SSS-Azure Güvenlik Merkezi ile ilgili genel sorular

## <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?
Azure Güvenlik Merkezi, kaynak güvenliğine yönelik artırılmış görünürlük ve denetim ile tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Güvenlik Merkezi, veri toplamak ve depolamak için Log Analytics aracısını kullanır. Ayrıntılı Ayrıntılar için bkz. [Azure Güvenlik Merkezi 'Nde veri toplama](security-center-enable-data-collection.md).


## <a name="how-do-i-get-azure-security-center"></a>Azure Güvenlik Merkezi 'Ni Nasıl yaparım? mi?
Azure Güvenlik Merkezi, Microsoft Azure aboneliğiniz ile etkinleştirilir ve [Azure Portal](https://azure.microsoft.com/features/azure-portal/)erişilir. Erişmek için [portalda oturum açın](https://portal.azure.com), **Araştır**' ı seçin ve **Güvenlik Merkezi**' ne gidin.


## <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Azure Güvenlik Merkezi tarafından hangi Azure kaynakları izleniyor?
Azure Güvenlik Merkezi aşağıdaki Azure kaynaklarını izler:

* Sanal makineler (VM 'Ler) ( [Cloud Services](../cloud-services/cloud-services-choose-me.md)dahil)
* Sanal makine ölçek kümeleri
* Azure Sanal Ağları
* Kapsayıcılar
* Azure SQL hizmeti
* Azure Depolama hesabınızın
* Azure Web Apps ( [App Service ortamı](../app-service/environment/intro.md))
* VM 'lerde ve App Service Ortamı bir Web uygulaması güvenlik duvarı gibi Azure aboneliğiniz ile tümleştirilmiş iş ortağı çözümleri

Ayrıca, Azure dışı (Şirket içi) makineler de Azure Güvenlik Merkezi tarafından izlenebilir. Hem [Windows makineleri](./quick-onboard-windows-computer.md) hem de [Linux makineleri](./quick-onboard-linux-computer.md) desteklenir.


## <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Azure kaynaklarım 'ın geçerli güvenlik durumunu nasıl görebilirim?
**Güvenlik Merkezi 'Ne genel bakış** sayfası, ortamınızın Işlem, ağ, depolama & verileri ve uygulamalar tarafından ayrılmış genel güvenlik duruşunu gösterir. Her kaynak türünün, tanımlanan güvenlik açıklarını gösteren bir göstergesi vardır. Her kutucuğa tıklamak, Güvenlik Merkezi tarafından tanımlanan güvenlik sorunlarının bir listesini, aboneliğinizdeki kaynakların envanterini gösterir.



## <a name="what-is-a-security-policy"></a>Güvenlik ilkesi nedir?
Bir güvenlik ilkesi, belirtilen abonelikteki kaynaklar için önerilen denetim kümesini tanımlar. Azure Güvenlik Merkezi 'nde, şirketinizin güvenlik gereksinimlerine ve uygulama türüne ve her abonelikteki verilerin duyarlılığına göre Azure abonelikleriniz için ilkeler tanımlarsınız.

Azure Güvenlik Merkezi 'nde etkinleştirilen güvenlik ilkeleri, güvenlik önerileri ve izleme. Güvenlik ilkeleri hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik sistem durumu izleme](security-center-monitoring.md).


## <a name="who-can-modify-a-security-policy"></a>Güvenlik ilkesini kim değiştirebilir?
Bir güvenlik ilkesini değiştirmek için, bir güvenlik yöneticisi veya bu aboneliğin sahibi veya katılımcısı olmanız gerekir.

Güvenlik ilkesi yapılandırma hakkında bilgi edinmek için bkz. [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md).


## <a name="what-is-a-security-recommendation"></a>Güvenlik önerisi nedir?
Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Olası güvenlik açıkları tanımlandığında, öneriler oluşturulur. Öneriler, gerekli denetimi yapılandırma sürecinde size yol gösterir. Örnekler şunlardır:

* Kötü amaçlı yazılımdan koruma sağlama kötü amaçlı yazılımları tanımanıza ve kaldırmanıza yardımcı olur
* Sanal makinelere trafiği denetlemek için [ağ güvenlik grupları](../virtual-network/security-overview.md) ve kuralları
* Web uygulamalarınızı hedefleyen saldırılara karşı savunmasına yardımcı olmak için bir Web uygulaması güvenlik duvarı sağlama
* Eksik sistem güncelleştirmelerini dağıtma
* Önerilen taban çizgileriyle eşleşmeyen işletim sistemi yapılandırmalarını ele alma

Yalnızca güvenlik Ilkelerinde etkinleştirilen öneriler burada gösterilmiştir.



## <a name="what-triggers-a-security-alert"></a>Güvenlik uyarısı ne tetikler?
Azure Güvenlik Merkezi, Azure kaynaklarınızdan, ağdan ve kötü amaçlı yazılımdan koruma ve güvenlik duvarları gibi iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve kullanır. Tehditler algılandığında bir güvenlik uyarısı oluşturulur. Örneklere şunların algılanması dahildir:

* Bilinen kötü amaçlı IP adresleri ile iletişim kuran tehlikeye girmiş sanal makineler
* Windows hata bildirimi kullanılarak algılanan gelişmiş kötü amaçlı yazılım
* Sanal makinelere karşı deneme yanılma saldırıları
* Kötü amaçlı yazılımdan koruma veya Web uygulaması güvenlik duvarları gibi tümleşik iş ortağı güvenlik çözümlerinin güvenlik uyarıları


## <a name="why-did-secure-score-values-change"></a>Neden güvenli puan değerleri değişir? <a name="secure-score-faq"></a>
Şubat 2019 itibariyle Güvenlik Merkezi, önem derecesini daha iyi uydurmak için birkaç önerilerin Puanını ayarladı. Bu ayarlamanın sonucunda, genel olarak güvenli puan değerlerinde değişiklikler olabilir.  Güvenli Puanlama hakkında daha fazla bilgi için bkz. [güvenli Puanlama hesaplama](security-center-secure-score.md).


## <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Microsoft Güvenlik Yanıt Merkezi 'nin Azure Güvenlik Merkezi 'ne göre algılanan ve uyarı oluşan tehditler arasındaki fark nedir?
Microsoft Güvenlik Yanıt Merkezi (MSRC), Azure ağ ve altyapısının güvenlik izlemesini seçin ve üçüncü taraflardan tehdit zekasını ve uygunsuz kullanım şikayetini alır. MSRC müşteri verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğine veya müşterinin Azure 'un kullanımı, kabul edilebilir kullanım koşullarına uymadığında, bir güvenlik olay Yöneticisi müşteriyi bilgilendirir. Bildirim genellikle Azure Güvenlik Merkezi 'nde belirtilen güvenlik kişilerine veya bir güvenlik kişisi belirtilmemişse Azure abonelik sahibine bir e-posta göndererek oluşur.

Güvenlik Merkezi, müşterinin Azure ortamını sürekli olarak izleyen ve analiz uygulayan ve çok sayıda kötü amaçlı olabilecek etkinlikleri otomatik olarak algılayan bir Azure hizmetidir. Bu algılamalar Güvenlik Merkezi panosunda güvenlik uyarıları olarak ortaya çıkmış.