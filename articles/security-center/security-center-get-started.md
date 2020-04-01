---
title: Standart katmana yükseltme - Azure Güvenlik Merkezi
description: Bu hızlı başlangıçta, ek güvenlik kapsamı için Güvenlik Merkezinizin Standart fiyatlandırma katmanına nasıl yükseltme yapacağınız gösterilmektedir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: memildin
ms.openlocfilehash: 3f0d624605f617a8e5ab914c49c4c94a40ebdcc6
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80435794"
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Hızlı Başlangıç: Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme
Azure Güvenlik Merkezi, hibrit bulut iş yüklerinizde birleşik güvenlik yönetimi ve tehdit koruması sağlar. Ücretsiz katman yalnızca Azure kaynaklarınız için sınırlı güvenlik sunarken Standart katman bu özellikleri şirket içine ve diğer bulutlara genişletir. Güvenlik Merkezi Standart katmanı; güvenlik açıklarını bulup gidermenize, zararlı etkinlikleri engellemek için erişim ve uygulama denetimleri uygulamanıza, analizden ve bilgilerden yararlanarak tehditleri algılamanıza ve saldırı altındayken hızlıca yanıt vermenize yardımcı olur. Güvenlik Merkezi Standard'ı ücretsiz olarak deneyebilirsiniz. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Bu makalede, daha fazla güvenlik için Standart katmana yükseltin ve güvenlik açıklarını ve tehditlerini izlemek için Log Analytics aracısını sanal makinelerinize yüklersiniz.

## <a name="prerequisites"></a>Ön koşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Bir aboneliği Standart katmana yükseltebilmeniz için size Abonelik Sahibi, Abonelik Katkıda Bulunanı veya Güvenlik Yöneticisi rolünün atanması gerekir.

## <a name="enable-your-azure-subscription"></a>Azure aboneliğinizi etkinleştirme

1. [Azure portalında](https://azure.microsoft.com/features/azure-portal/)oturum açın.
2. Microsoft **Azure** menüsünde **Güvenlik Merkezi'ni**seçin. **Güvenlik Merkezi - Genel Bakış** açılır.

   ![Güvenlik Merkezi’ne genel bakış][2]

**Güvenlik Merkezi - Genel Bakış**, hibrit bulut iş yüklerinizin güvenlik durumuna ilişkin birleştirilmiş bir görünüm sağlayarak, iş yüklerinizin güvenliğini tespit edip değerlendirmenize ve riskleri tanımlayıp en aza indirmenize olanak tanır. Güvenlik Merkezi, sizin tarafınızdan veya başka bir abonelik kullanıcısı tarafından daha önce Ücretsiz katmana eklenmemiş olan Azure aboneliklerinizi otomatik olarak etkinleştirir.

**Abonelikler** menü öğesine tıklayarak aboneliklerin listesini görüntüleyebilir ve filtreleyebilirsiniz. Güvenlik Merkezi artık güvenlik açıklarını belirlemek için bu aboneliklerin güvenliğini değerlendirmeye başlayacaktır. Değerlendirme türlerini özelleştirmek için güvenlik ilkesinde değişiklik yapabilirsiniz. Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur.

Güvenlik Merkezi’ni başlattıktan sonraki ilk birkaç dakika içinde şunları görebilirsiniz:

- Azure aboneliklerinizin güvenliğini artırmaya yönelik yöntemlere ilişkin **öneriler**. **Öneriler** kutucuğuna tıkladığınızda bir öncelik listesi açılır.
- Güvenlik Merkezi tarafından her birinin güvenlik durumuyla birlikte değerlendirilen **İşlem ve uygulamalar**, **Ağ İletişimi**, **Veri güvenliği**, **Kimlik ve erişim** kaynaklarının bir envanteri.

Güvenlik Merkezi'nden tam olarak yararlanmak için Standart katmana yükseltmek ve Log Analytics aracısını yüklemek için aşağıdaki adımları tamamlamanız gerekir.

## <a name="upgrade-to-the-standard-tier"></a>Standart katmana yükseltme
Güvenlik Merkezi hızlı başlangıçlarının ve öğreticilerinin amacı doğrultusunda Standart katmana yükseltme yapmanız gerekir. Güvenlik Merkezi Standardı'nın ücretsiz deneme sürümü var. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). 

1. Güvenlik Merkezi ana menüsü altında, **Başlarken**’i seçin.
 
   ![başlarken][4]

2. **Yükselt** altında, Güvenlik Merkezi, ekleme işlemi için uygun abonelikleri ve çalışma alanlarını listeler. 
   - Tüm abonelikleri ve çalışma alanlarını deneme sürümü uygunluk durumlarıyla birlikte listelemek için, genişletilebilir **Deneme sürümünüzü uygulayın**’a tıklayabilirsiniz.
   -    Deneme sürümü için uygun olmayan abonelikler ve çalışma alanlarını yükseltebilirsiniz.
   -    Deneme sürümünüzü başlatmak için uygun çalışma alanlarını ve abonelikleri seçebilirsiniz.
3. Seçili aboneliklerde deneme sürümünüzü başlatmak için **Deneme sürümünü başlat**’a tıklayın.


  ![Güvenlik uyarıları][9]

## <a name="automate-data-collection"></a>Veri toplama işlemini otomatikleştirme
Güvenlik Merkezi, güvenlik açıklarını ve tehditleri izlemek için Azure VM’lerinizden ve Azure olmayan bilgisayarlarınızdan veri toplar. Veriler, makineden çeşitli güvenlikle ilgili yapılandırmaları ve olay günlüklerini okuyan ve verileri analiz için çalışma alanınıza kopyalayan Log Analytics aracısı kullanılarak toplanır. Varsayılan olarak Güvenlik Merkezi sizin için yeni bir çalışma alanı oluşturur.

Otomatik sağlama etkinleştirildiğinde, Güvenlik Merkezi desteklenen tüm Azure VM'lerine ve oluşturulan yeni tüm yeni lere Log Analytics aracısını yükler. Otomatik sağlama önemle önerilir.

Log Analytics aracısının otomatik olarak sağlanmasını etkinleştirmek için:

1. Güvenlik Merkezi ana menüsü **altında, Fiyatlandırma & ayarlarını**seçin.
2. Abonelik satırında, ayarları değiştirmek istediğiniz aboneliği tıklatın.
3. **Veri Toplama** sekmesinde, **Otomatik sağlama**’yı **Açık** olarak ayarlayın.
4. **Kaydet'i**seçin.
---
  ![Otomatik sağlamayı etkinleştirme][6]

Azure VM’lerinize ilişkin bu yeni öngörüyle Güvenlik Merkezi, sistem güncelleştirme durumu, işletim sistemi güvenlik yapılandırmaları ve uç nokta koruması ile ilgili ek Öneriler sunmanın yanı sıra ek Güvenlik uyarıları oluşturabilir.

  ![Öneriler][8]

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız Standart katmanını çalıştırmaya devam edin ve otomatik sağlamayı etkinleştirilmiş halde tutun. Devam etmeyi planlamıyorsanız veya Ücretsiz katmanına dönmek istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönün ve **Fiyatlandırma & ayarlarını**seçin.
2. Ücretsiz katmana değiştirmek istediğiniz aboneliği tıklatın.
3. Aboneliği Standart katmanından Ücretsiz katmanına geçirmek için **Fiyatlandırma katmanı**’nı ve **Ücretsiz**’i seçin.
5. **Kaydet'i**seçin.

Otomatik sağlamayı devre dışı bırakmak istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönün ve **Fiyatlandırma & ayarlarını**seçin.
2. Otomatik sağlamayı devre dışı kaltın istediğiniz aboneliği temizleyin.
3. **Veri Toplama** sekmesinde, **Otomatik sağlama**’yı **Kapalı** olarak ayarlayın.
4. **Kaydet'i**seçin.

>[!NOTE]
> Otomatik sağlamanın devre dışı bırakılması, log analytics aracısını aracının sağlandığı Azure VM'lerinden kaldırmaz. Otomatik sağlamanın devre dışı bırakılması, kaynaklarınızın güvenliğinin izlenmesini kısıtlar.
>

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Standart katmana yükselttiniz ve karma bulut iş yüklerinizde birleşik güvenlik yönetimi ve tehdit koruması için Log Analytics aracısını sayıltınız. Güvenlik Merkezi’ni kullanma hakkında daha fazla bilgi edinmek için şirket içi ortamda ve diğer bulutlarda bulunan Windows bilgisayarları eklemeye ilişkin hızlı başlangıca geçin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Windows bilgisayarları Azure Güvenlik Merkezi’ne ekleme](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/get-started.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
[9]: ./media/security-center-get-started/select-subscription.png
