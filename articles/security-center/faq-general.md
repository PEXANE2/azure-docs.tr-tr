---
title: Azure Güvenlik Merkezi SSS - Genel sorular
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
ms.openlocfilehash: 54263a8be900ed381d8450b460e5cf9e6117ac54
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80436128"
---
# <a name="faq---general-questions-about-azure-security-center"></a>SSS - Azure Güvenlik Merkezi ile ilgili genel sorular

## <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?
Azure Güvenlik Merkezi, kaynaklarınızın güvenliği nde daha fazla görünürlük ve denetim le tehditleri önlemenize, algılamanıza ve yanıt vermeye yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Güvenlik Merkezi, veri toplamak ve depolamak için Log Analytics aracısını kullanır. Ayrıntılı bilgi için [Azure Güvenlik Merkezi'nde Veri koleksiyonuna](security-center-enable-data-collection.md)bakın.


## <a name="how-do-i-get-azure-security-center"></a>Azure Güvenlik Merkezi'ni nasıl edinebilirim?
Azure Güvenlik Merkezi, Microsoft Azure aboneliğiniz ile etkinleştirilir ve [Azure portalından](https://azure.microsoft.com/features/azure-portal/)erişilir. Erişmek için [portalda oturum açın,](https://portal.azure.com) **Gözat'ı**seçin ve **Güvenlik Merkezi'ne**gidin.


## <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Hangi Azure kaynakları Azure Güvenlik Merkezi tarafından izlenir?
Azure Güvenlik Merkezi aşağıdaki Azure kaynaklarını izler:

* Sanal makineler (VM' ler) [(Bulut Hizmetleri](../cloud-services/cloud-services-choose-me.md)dahil)
* Sanal makine ölçek kümeleri
* Azure Sanal Ağları
* Kapsayıcılar
* Azure SQL hizmeti
* Azure Depolama hesabınızın
* Azure Web Uygulamaları [(Uygulama Hizmet Ortamında)](../app-service/environment/intro.md)
* VM'lerde ve Uygulama Hizmeti Ortamında web uygulama güvenlik duvarı gibi Azure aboneliğinizle entegre iş ortağı çözümleri

Ayrıca, Azure olmayan (şirket içi dahil) makineler de Azure Güvenlik Merkezi tarafından izlenebilir. Hem [Windows makineleri](./quick-onboard-windows-computer.md) hem de Linux [makineleri](./quick-onboard-linux-computer.md) desteklenir.


## <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Azure kaynaklarımın geçerli güvenlik durumunu nasıl görebiliyorum?
**Güvenlik Merkezi Genel Bakış** sayfası, çevrenizin Genel Güvenlik duruşunu Bilgi İşlem, Ağ, Depolama & verileri ve Uygulamalar'a göre ayrılmıştır. Her kaynak türü, tanımlanmış güvenlik açıklarını gösteren bir göstergeye sahiptir. Her döşemeyi tıklattığınızda, Güvenlik Merkezi tarafından tanımlanan güvenlik sorunlarının bir listesi ve aboneliğinizdeki kaynakların bir envanteri görüntülenir.



## <a name="what-is-a-security-policy"></a>Güvenlik ilkesi nedir?
Güvenlik ilkesi, belirtilen abonelik içindeki kaynaklar için önerilen denetim kümesini tanımlar. Azure Güvenlik Merkezi'nde, Azure abonelikleriniz için şirketinizin güvenlik gereksinimlerine ve her abonelikteki uygulamaların türüne veya duyarlılığına göre ilkeler tanımlarsınız.

Azure Güvenlik Merkezi'nde etkinleştirilen güvenlik ilkeleri güvenlik önerilerini ve izlemeyi yönlendiriyor. Güvenlik ilkeleri hakkında daha fazla bilgi edinmek için [Azure Güvenlik Merkezi'nde Güvenlik durumu denetimine](security-center-monitoring.md)bakın.


## <a name="who-can-modify-a-security-policy"></a>Güvenlik ilkesini kimler değiştirebilir?
Bir güvenlik ilkesini değiştirmek için güvenlik yöneticisi veya bu aboneliğin sahibi veya katılımcısı olmalısınız.

Güvenlik ilkesini nasıl yapılandırıştırılaceksiniz öğrenmek için Azure [Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama'ya](tutorial-security-policy.md)bakın.


## <a name="what-is-a-security-recommendation"></a>Güvenlik önerisi nedir?
Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Olası güvenlik açıkları tanımlandığında, öneriler oluşturulur. Öneriler, gerekli denetimi yapılandırma sürecinde size yol gösterin. Örnekler şunlardır:

* Kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olacak kötü amaçlı yazılımdan koruma sağlama
* [Sanal](../virtual-network/security-overview.md) makinelere olan trafiği denetlemek için ağ güvenlik grupları ve kuralları
* Web uygulamalarınızı hedef leyen saldırılara karşı savunmaya yardımcı olmak için bir web uygulaması güvenlik duvarı sağlama
* Eksik sistem güncelleştirmelerini dağıtma
* Önerilen taban çizgileriyle eşleşmeyen işletim sistemi yapılandırmalarını ele alma

Yalnızca Güvenlik İlkeleri'nde etkinleştirilen öneriler burada gösterilir.



## <a name="what-triggers-a-security-alert"></a>Güvenlik alarmLarını tetikleyen nedir?
Azure Güvenlik Merkezi, Azure kaynaklarınızdan, ağınızdan ve kötü amaçlı yazılımdan koruma ve güvenlik duvarları gibi iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, analiz eder ve birleşir. Tehditler algılandığında bir güvenlik uyarısı oluşturulur. Örneklere şunların algılanması dahildir:

* Bilinen kötü amaçlı IP adresleri ile iletişim kuran tehlikeye girmiş sanal makineler
* Windows hata raporlaması kullanılarak algılanan gelişmiş kötü amaçlı yazılım
* Sanal makinelere karşı deneme yanılma saldırıları
* Kötü Amaçlı Yazılımdan Koruma veya Web Uygulaması Güvenlik Duvarları gibi entegre iş ortağı güvenlik çözümlerinden gelen güvenlik uyarıları


## <a name="why-did-secure-score-values-change"></a>Güvenli Puan değerleri neden değişti? <a name="secure-score-faq"></a>
Şubat 2019 itibariyle, Güvenlik Merkezi, şiddetine daha iyi uyum sağlamak için birkaç önerinin puanını düzeltti. Bu ayarlamanın bir sonucu olarak, genel Güvenli Puan değerlerinde değişiklikler olabilir.  Güvenli Puan hakkında daha fazla bilgi için [Güvenli Puan hesaplaması'na](security-center-secure-score.md)bakın.


## <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Microsoft Güvenlik Yanıt Merkezi ile Azure Güvenlik Merkezi tarafından algılanan ve uyarılanan tehditler arasındaki fark nedir?
Microsoft Güvenlik Yanıt Merkezi (MSRC), Azure ağının ve altyapısının seçici güvenlik izlemesini gerçekleştirir ve üçüncü taraflardan tehdit istihbaratı ve kötüye kullanım şikayetleri alır. MSRC müşteri verilerine yasadışı veya yetkisiz bir taraf tarafından erişildiğini veya müşterinin Azure'u kullanmasının Kabul Edilebilir Kullanım koşullarına uymadığını fark ettiğinde, bir güvenlik olayı yöneticisi müşteriye bildirir. Bildirim genellikle, bir güvenlik kişisi belirtilmemişse, Azure Güvenlik Merkezi'nde belirtilen güvenlik kişilerine veya Azure abonelik sahibine e-posta göndererek gerçekleşir.

Güvenlik Merkezi, müşterinin Azure ortamını sürekli olarak izleyen ve çok çeşitli kötü amaçlı olabilecek etkinlikleri otomatik olarak algılamak için analitik uygulayan bir Azure hizmetidir. Bu algılamalar, Güvenlik Merkezi panosunda güvenlik uyarıları olarak su yüzüne çıkar.