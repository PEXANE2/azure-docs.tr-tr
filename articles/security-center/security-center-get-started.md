---
title: Standart katmana yükseltme-Azure Güvenlik Merkezi
description: Bu hızlı başlangıçta, ek güvenlik için Güvenlik Merkezi 'nin standart fiyatlandırma katmanına nasıl yükselteceğiniz gösterilmektedir.
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
ms.openlocfilehash: 550c9ff57b9c558f2f175165c7f06ead45991be9
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88226030"
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Hızlı Başlangıç: Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme
Azure Güvenlik Merkezi, hibrit bulut iş yüklerinizde birleşik güvenlik yönetimi ve tehdit koruması sağlar. Ücretsiz katman yalnızca Azure kaynaklarınız için sınırlı güvenlik sağladığından, Standart katman bu özellikleri şirket içi ve diğer bulutlara genişletir. Güvenlik Merkezi Standart katmanı; güvenlik açıklarını bulup gidermenize, zararlı etkinlikleri engellemek için erişim ve uygulama denetimleri uygulamanıza, analizden ve bilgilerden yararlanarak tehditleri algılamanıza ve saldırı altındayken hızlıca yanıt vermenize yardımcı olur. Güvenlik Merkezi Standart 'ı ücretsiz olarak deneyebilirsiniz. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Bu makalede, güvenlik açıklarını ve tehditleri izlemek için, ek güvenlik için Standart katmana yükseltirsiniz ve sanal makinelerinize Log Analytics aracısını yüklersiniz.

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Bir aboneliği Standart katmana yükseltmek için abonelik sahibi, abonelik katılımcısı veya güvenlik yöneticisi rolüne sahip olmanız gerekir.

## <a name="enable-your-azure-subscription"></a>Azure aboneliğinizi etkinleştirme

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.

1. **Microsoft Azure** menüsünde **Güvenlik Merkezi**' ni seçin. **Güvenlik Merkezi-genel bakış** açılır.

   ![Güvenlik Merkezi’ne genel bakış][2]

**Güvenlik Merkezi - Genel Bakış**, hibrit bulut iş yüklerinizin güvenlik durumuna ilişkin birleştirilmiş bir görünüm sağlayarak, iş yüklerinizin güvenliğini tespit edip değerlendirmenize ve riskleri tanımlayıp en aza indirmenize olanak tanır. Güvenlik Merkezi, sizin tarafınızdan veya başka bir abonelik kullanıcısı tarafından daha önce Ücretsiz katmana eklenmemiş olan Azure aboneliklerinizi otomatik olarak etkinleştirir.

**Abonelikler** menü öğesine tıklayarak aboneliklerin listesini görüntüleyebilir ve filtreleyebilirsiniz. Güvenlik Merkezi artık güvenlik açıklarını belirlemek için bu aboneliklerin güvenliğini değerlendirmeye başlayacaktır. Değerlendirme türlerini özelleştirmek için güvenlik ilkesinde değişiklik yapabilirsiniz. Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur.

Güvenlik Merkezi’ni başlattıktan sonraki ilk birkaç dakika içinde şunları görebilirsiniz:

- Azure aboneliklerinizin güvenliğini artırmaya yönelik yöntemlere ilişkin **öneriler**. **Öneriler** kutucuğuna tıkladığınızda bir öncelik listesi açılır.
- Güvenlik Merkezi tarafından her birinin güvenlik durumuyla birlikte değerlendirilen **İşlem ve uygulamalar**, **Ağ İletişimi**, **Veri güvenliği**, **Kimlik ve erişim** kaynaklarının bir envanteri.

Güvenlik Merkezi 'Nden tam olarak yararlanabilmek için, Standart katmana yükseltmek ve Log Analytics aracıyı yüklemek üzere aşağıdaki adımları gerçekleştirmeniz gerekir.


## <a name="upgrade-to-the-standard-tier"></a>Standart katmana yükseltme

Güvenlik Merkezi hızlı başlangıçlarının ve öğreticilerinin amacı doğrultusunda Standart katmana yükseltme yapmanız gerekir. Güvenlik Merkezi standardının ücretsiz bir denemesi vardır. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). 

1. Güvenlik Merkezi 'nin kenar çubuğundan **Başlarken**' i seçin.
 
   ![Başlarken](./media/security-center-get-started/get-started-upgrade-tab.png)

    **Yükseltme** sekmesi, ekleme için uygun olan abonelikleri ve çalışma alanlarını listeler.

1. Listeden **Standart katmanı etkinleştirmek için çalışma alanlarını seçin sayfasında** , Yükseltilecek çalışma alanlarını seçin.


    > [!TIP]
    > Ücretsiz deneme için uygun olan bir çalışma alanı seçerseniz, bir sonraki adım bir denemeye başlar. Çalışma alanları deneme için uygun değilse yükseltilir ve ücretler başlatılır.

1. Seçili çalışma alanlarını Standart katmana yükseltmek için **Yükselt** ' i seçin.


## <a name="automate-data-collection"></a>Veri toplama işlemini otomatikleştirme
Güvenlik Merkezi, güvenlik açıklarını ve tehditleri izlemek için Azure VM’lerinizden ve Azure olmayan bilgisayarlarınızdan veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırma ve olay günlüklerini okuyan ve analiz için verileri çalışma alanınıza kopyalayan Log Analytics Aracı kullanılarak toplanır. Varsayılan olarak Güvenlik Merkezi sizin için yeni bir çalışma alanı oluşturur.

Otomatik sağlama etkinleştirildiğinde, Güvenlik Merkezi, desteklenen tüm Azure VM 'lerine ve oluşturulan tüm yeni sanal makinelere Log Analytics aracısını yükleme. Otomatik sağlama önemle önerilir.

Log Analytics aracısının otomatik sağlamasını etkinleştirmek için:

1. Güvenlik Merkezi ana menüsünde, **fiyatlandırma & ayarları**' nı seçin.
1. Aboneliğin satırı üzerinde, ayarları değiştirmek istediğiniz aboneliğe tıklayın.
1. **Veri Toplama** sekmesinde, **Otomatik sağlama**’yı **Açık** olarak ayarlayın.
1. **Kaydet**’i seçin.
---
  ![Otomatik sağlamayı etkinleştirme][6]

Azure VM’lerinize ilişkin bu yeni öngörüyle Güvenlik Merkezi, sistem güncelleştirme durumu, işletim sistemi güvenlik yapılandırmaları ve uç nokta koruması ile ilgili ek Öneriler sunmanın yanı sıra ek Güvenlik uyarıları oluşturabilir.

  ![Öneriler][8]

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, standart katmanı çalıştırmaya devam edin ve otomatik sağlamayı etkin tutun. Devam etmeyi planlamıyorsanız veya Ücretsiz katmanına dönmek istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **fiyatlandırma & ayarları**' nı seçin.
2. Ücretsiz katmana değiştirmek istediğiniz aboneliğe tıklayın.
3. **Fiyatlandırma katmanını** seçin ve standart katmandan ücretsiz katmana abonelik değiştirmek için **ücretsiz** ' ı seçin.
5. **Kaydet**’i seçin.

Otomatik sağlamayı devre dışı bırakmak istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **fiyatlandırma & ayarları**' nı seçin.
2. Otomatik sağlamayı devre dışı bırakmak istediğiniz abonelikte temizleyin.
3. **Veri Toplama** sekmesinde, **Otomatik sağlama**’yı **Kapalı** olarak ayarlayın.
4. **Kaydet**’i seçin.

>[!NOTE]
> Otomatik sağlamayı devre dışı bırakmak, aracının sağlandığı Azure VM 'lerinden Log Analytics aracısını kaldırmaz. Otomatik sağlamanın devre dışı bırakılması, kaynaklarınızın güvenliğinin izlenmesini kısıtlar.
>

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Standart katmana yükseltmiş ve hibrit bulut iş yükleriniz genelinde birleştirilmiş güvenlik yönetimi ve tehdit koruması için Log Analytics aracısına hazırlandınız. Güvenlik Merkezi’ni kullanma hakkında daha fazla bilgi edinmek için şirket içi ortamda ve diğer bulutlarda bulunan Windows bilgisayarları eklemeye ilişkin hızlı başlangıca geçin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Windows bilgisayarları Azure Güvenlik Merkezi’ne ekleme](quick-onboard-windows-computer.md)

Bulut harcamalarınızı iyileştirmek ve kaydetmek istiyor musunuz?

> [!div class="nextstepaction"]
> [Maliyet yönetimi ile maliyetleri çözümlemeye başlayın](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/get-started.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
[9]: ./media/security-center-get-started/select-subscription.png
