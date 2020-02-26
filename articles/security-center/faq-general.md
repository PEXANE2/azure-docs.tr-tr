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
ms.openlocfilehash: b13e5e0000cdd8e33f459400cc4c24d107ccaba3
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77604799"
---
# <a name="faq---general-questions-about-azure-security-center"></a>SSS-Azure Güvenlik Merkezi ile ilgili genel sorular

## <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?
Azure Güvenlik Merkezi, kaynak güvenliğine yönelik artırılmış görünürlük ve denetim ile tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Güvenlik Merkezi, veri toplamak ve depolamak için Microsoft Monitoring Agent kullanır. Ayrıntılı Ayrıntılar için bkz. [Azure Güvenlik Merkezi 'Nde veri toplama](security-center-enable-data-collection.md).


## <a name="how-do-i-get-azure-security-center"></a>Azure Güvenlik Merkezi nasıl alabilirim?
Azure Güvenlik Merkezi, Microsoft Azure aboneliğiniz ile etkinleştirilir ve [Azure Portal](https://azure.microsoft.com/features/azure-portal/)erişilir. Erişmek için [portalda oturum açın](https://portal.azure.com), **Araştır**' ı seçin ve **Güvenlik Merkezi**' ne gidin.


## <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Hangi Azure kaynakları, Azure Güvenlik Merkezi tarafından izlenir?
Azure Güvenlik Merkezi, aşağıdaki Azure kaynakları izler:

* Sanal makineler (VM 'Ler) ( [Cloud Services](../cloud-services/cloud-services-choose-me.md)dahil)
* Sanal makine ölçek kümeleri
* Azure Sanal Ağları
* Azure SQL Hizmeti
* Azure Storage hesabınız
* Azure Web Apps ( [App Service ortamı](../app-service/environment/intro.md))
* Web uygulaması güvenlik duvarı vm'lerde ve App Service ortamı gibi Azure aboneliğinizle tümleşik iş ortağı çözümleri

Ayrıca, Azure dışı (Şirket içi) bilgisayarlar da Azure Güvenlik Merkezi tarafından izlenebilir (hem [Windows bilgisayarları](./quick-onboard-windows-computer.md) hem de [Linux bilgisayarları](./quick-onboard-linux-computer.md) desteklenir)


## <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Azure Kaynaklarımın geçerli güvenlik durumunu nasıl görebilirim?
**Güvenlik Merkezi 'Ne genel bakış** sayfası, ortamınızın Işlem, ağ, depolama & verileri ve uygulamalar tarafından ayrılmış genel güvenlik duruşunu gösterir. Olası güvenlik açıkları tanımladıysanız her kaynak türünün bir göstergesi gösteren vardır. Her kutucuğa tıklandığında, aboneliğinizdeki kaynaklar envanterini birlikte Güvenlik Merkezi tarafından tanımlanan güvenlik sorunların bir listesini görüntüler.



## <a name="what-is-a-security-policy"></a>Güvenlik İlkesi nedir?
Güvenlik İlkesi belirtilen Abonelikteki kaynaklar için önerilen denetim kümesini tanımlar. Azure Güvenlik Merkezi'nde şirketinizin güvenlik gereksinimlerine veya uygulamaların türüne ya da her Abonelikteki verilerin duyarlılığına göre Azure Abonelikleriniz için ilkeler tanımlarsınız.

Azure Güvenlik Merkezi sürücü güvenlik önerilerini ve izlemeyi etkinleştirilen güvenlik ilkeleri. Güvenlik ilkeleri hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik sistem durumu izleme](security-center-monitoring.md).


## <a name="who-can-modify-a-security-policy"></a>Güvenlik İlkesi değiştirebilecekleri?
Güvenlik ilkesini değiştirmek için bir güvenlik yöneticisi veya sahibi veya katkıda bulunanı o aboneliğin olması gerekir.

Güvenlik ilkesi yapılandırma hakkında bilgi edinmek için bkz. [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md).


## <a name="what-is-a-security-recommendation"></a>Bir güvenlik önerisi nedir?
Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Olası güvenlik açıkları tanımlandığında, öneri oluşturulur. Öneriler gerekli denetimi yapılandırma işlemi boyunca size rehberlik. Örnekler şunlardır:

* Kötü amaçlı yazılımdan koruma tanımlamak ve kötü amaçlı yazılımı kaldırmak için sağlama
* Sanal makinelere trafiği denetlemek için [ağ güvenlik grupları](../virtual-network/security-overview.md) ve kuralları
* Web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için bir web uygulaması güvenlik duvarı sağlama
* Eksik sistem güncelleştirmelerini dağıtma
* Önerilen taban çizgileriyle eşleşmeyen işletim sistemi yapılandırmalarını ele alma

Güvenlik ilkeleri ile etkinleştirilen önerileri burada gösterilir.



## <a name="what-triggers-a-security-alert"></a>Hangi güvenlik uyarısı tetikler?
Azure Güvenlik Merkezi otomatik olarak toplar, çözümler ve Azure kaynaklarınızı, ağ ve kötü amaçlı yazılımdan koruma ve güvenlik duvarları gibi iş ortağı çözümlerinden günlük verilerini birleştirir. Tehditler algılandığında bir güvenlik uyarısı oluşturulur. Örneklere şunların algılanması dahildir:

* Bilinen kötü amaçlı IP adresleri ile iletişim kuran tehlikeye girmiş sanal makineler
* Windows Hata Raporlama kullanılarak algılanan Gelişmiş kötü amaçlı yazılım
* Sanal makinelere karşı deneme yanılma saldırıları
* Kötü amaçlı yazılımdan koruma veya Web uygulaması güvenlik duvarları gibi tümleşik iş ortağı güvenlik çözümlerinden güvenlik uyarıları


## Neden güvenli puan değerleri değişir? <a name="secure-score-faq"></a>
Şubat 2019 itibariyle Güvenlik Merkezi, önem derecesini daha iyi uydurmak için birkaç önerilerin Puanını ayarladı. Bu ayarlamanın sonucunda, genel olarak güvenli puan değerlerinde değişiklikler olabilir.  Güvenli Puanlama hakkında daha fazla bilgi için bkz. [güvenli Puanlama hesaplama](security-center-secure-score.md).


## <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Microsoft Security Response Center ve Azure Güvenlik Merkezi tarafından uyarı ve algılanan tehditlere arasındaki fark nedir?
Microsoft Güvenlik Yanıt Merkezi (MSRC), select güvenlik Azure ağ ve altyapı izleme gerçekleştirir ve tehdit zekasını ve kötüye şikayetlerinin Üçüncü taraflardan alır. MSRC müşteri verilerini bir yasadışı veya yetkisiz bir tarafın eriştiğini veya müşterinin Azure kullanımına şartlarını kabul edilebilir kullanım için uyumlu değil, uyumlu hale geldiğinde, güvenlik olay manager müşteri bildirir. Bildirim, genellikle güvenlik ilgili kişi belirtilmezse, Azure Güvenlik Merkezi veya Azure aboneliği sahibi belirtilen güvenlik kişilere bir e-posta göndererek gerçekleşir.

Güvenlik Merkezi sürekli olarak müşterinin Azure ortamına izler ve analytics çok çeşitli kötü amaçlı etkinlik otomatik olarak algılamak için geçerli bir Azure hizmetidir. Bu algılamaların amacı, Güvenlik Merkezi panosunda güvenlik uyarıları olarak takip edilir.