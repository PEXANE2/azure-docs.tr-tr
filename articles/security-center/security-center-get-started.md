---
title: Azure Güvenlik Merkezi hızlı başlangıç-Azure aboneliğinizi Güvenlik Merkezi standardına ekleme | Microsoft Docs
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
ms.openlocfilehash: 689949b2b6fea1dcd06741fd3fc19c9371a96784
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71676387"
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Hızlı başlangıç: Azure aboneliğinizi Güvenlik Merkezi standardına ekleme
Azure Güvenlik Merkezi, hibrit bulut iş yükleriniz genelinde Birleşik güvenlik yönetimi ve tehdit koruması sağlar. Ücretsiz katman yalnızca Azure kaynaklarınız için sınırlı güvenlik sağladığından, Standart katman bu özellikleri şirket içi ve diğer bulutlara genişletir. Güvenlik Merkezi standardı, güvenlik açıklarını bulmanıza ve düzeltmenize, kötü amaçlı etkinlikleri engellemek, analiz ve zekası kullanarak tehditleri tespit etmek ve saldırı altında hızla yanıt vermek için erişim ve uygulama denetimleri uygulamanıza yardımcı olur. Güvenlik Merkezi Standart 'ı ücretsiz olarak deneyebilirsiniz. Daha fazla bilgi edinmek için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/)bakın.

Bu makalede, güvenlik açıklarını ve tehditleri izlemek için, ek güvenlik için Standart katmana yükseltirsiniz ve sanal makinelerinize Microsoft Monitoring Agent yükleyebilirsiniz.

## <a name="prerequisites"></a>Prerequisites
Güvenlik Merkezi 'ni kullanmaya başlamak için Microsoft Azure bir aboneliğiniz olması gerekir. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/)için kaydolabilirsiniz.

Bir aboneliği Standart katmana yükseltmek için abonelik sahibi, abonelik katılımcısı veya güvenlik yöneticisi rolüne sahip olmanız gerekir.

## <a name="enable-your-azure-subscription"></a>Azure aboneliğinizi etkinleştirme

1. [Azure Portal](https://azure.microsoft.com/features/azure-portal/)oturum açın.
2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**' ni seçin. **Güvenlik Merkezi-genel bakış** açılır.

   ![Güvenlik Merkezi 'ne genel bakış][2]

**Güvenlik Merkezi – genel bakış** , karma bulut iş yüklerinizin güvenlik duruşuna birleştirilmiş bir görünüm sunarak iş yüklerinizin güvenliğini keşfetmenize ve değerlendirmenize ve riski belirleyip azaltmanıza olanak sağlar. Güvenlik Merkezi, siz veya başka bir abonelik kullanıcısı tarafından daha önce eklendi Azure aboneliklerinizden herhangi birini, ücretsiz katmana otomatik olarak sağlar.

**Abonelikler menü öğesine** tıklayarak Aboneliklerin listesini görüntüleyebilir ve filtreleyebilirsiniz. Güvenlik Merkezi artık güvenlik açıklarını belirlemek için bu aboneliklerin güvenliğini değerlendirmeye başlar. Değerlendirme türlerini özelleştirmek için güvenlik ilkesini değiştirebilirsiniz. Güvenlik ilkesi, iş yüklerinizin istenen yapılandırmasını tanımlar ve şirket veya mevzuat güvenlik gereksinimleriyle uyumluluğu sağlamaya yardımcı olur.

Güvenlik Merkezi 'nin ilk kez başlatılması sırasında geçen dakikalar içinde şu şekilde karşılaşabilirsiniz:

- Azure aboneliklerinizin güvenliğini artırmanın yolları için **öneriler** . **Öneriler** kutucuğuna tıkladığınızda öncelikli bir liste başlatılır.
- Güvenlik Merkezi tarafından, her birinin güvenlik duruşağıyla birlikte değerlendirilemeyen bir **işlem & uygulamalar**, **ağ**, **veri güvenliği**ve **kimlik &** .

Güvenlik Merkezi 'Nden tam olarak yararlanabilmek için, Standart katmana yükseltmek ve Microsoft Monitoring Agent yüklemek üzere aşağıdaki adımları gerçekleştirmeniz gerekir.

## <a name="upgrade-to-the-standard-tier"></a>Standart katmana yükseltme
Güvenlik Merkezi hızlı başlangıç ve öğreticilerin amacı için Standart katmana yükseltmeniz gerekir. Güvenlik Merkezi standardının ücretsiz bir denemesi vardır. Daha fazla bilgi edinmek için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/)bakın. 

1. Güvenlik Merkezi ana **menüsünde Başlarken ' i seçin.**
 
   ![Kullanmaya başlayın][4]

2. **Yükseltme**altında Güvenlik Merkezi, ekleme için uygun olan abonelikleri ve çalışma alanlarını listeler. 
   - Tüm aboneliklerin ve çalışma alanlarının bir listesini, deneme uygunluğu durumuyla birlikte görmek için Genişletilebilir **uygulama** ' ya tıklayabilirsiniz.
   -    Deneme yanılma uygun olmayan abonelikleri ve çalışma alanlarını yükseltebilirsiniz.
   -    Deneme sürümünüzü başlatmak için uygun çalışma alanlarını ve abonelikleri seçebilirsiniz.
3. Deneme sürümünüzü seçili abonelikler üzerinde başlatmak için **denemeyi Başlat** ' a tıklayın.


  ![Güvenlik uyarıları][9]

## <a name="automate-data-collection"></a>Veri toplamayı otomatikleştirin
Güvenlik Merkezi, güvenlik açıklarını ve tehditleri izlemek için Azure sanal makinelerinizden ve Azure olmayan bilgisayarlardan veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırma ve olay günlüklerini okuyan ve analiz için verileri çalışma alanınıza kopyalayan Microsoft Monitoring Agent kullanılarak toplanır. Varsayılan olarak, güvenlik merkezi sizin için yeni bir çalışma alanı oluşturur.

Otomatik sağlama etkinleştirildiğinde, Güvenlik Merkezi, desteklenen tüm Azure VM 'Lere ve oluşturulan tüm yeni makinelere Microsoft Monitoring Agent yüklenmektedir. Otomatik sağlama kesinlikle önerilir.

Microsoft Monitoring Agent otomatik sağlamayı etkinleştirmek için:

1. Güvenlik Merkezi ana menüsünde, **fiyatlandırma & ayarları**' nı seçin.
2. Aboneliğin satırı üzerinde, ayarları değiştirmek istediğiniz aboneliğe tıklayın.
3. **Veri toplama** sekmesinde, **otomatik sağlamayı** **Açık**olarak ayarlayın.
4. **Kaydet**' i seçin.
---
  ![Otomatik sağlamayı etkinleştir][6]

Azure VM 'lerinize yönelik bu yeni Öngörüler sayesinde Güvenlik Merkezi, sistem güncelleştirme durumu, işletim sistemi güvenlik yapılandırması, uç nokta koruması ve ek güvenlik uyarıları oluşturma ile ilgili ek öneriler sağlayabilir.

  ![Öneri][8]

## <a name="clean-up-resources"></a>Kaynakları Temizleme
Bu koleksiyondaki diğer hızlı başlangıçler ve öğreticiler bu hızlı başlangıç üzerine inşa ediyor. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, standart katmanı çalıştırmaya devam edin ve otomatik sağlamayı etkin tutun. Devam etmeyi planlamıyorsanız veya ücretsiz katmana dönmek istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **fiyatlandırma & ayarları**' nı seçin.
2. Ücretsiz katmana değiştirmek istediğiniz aboneliğe tıklayın.
3. **Fiyatlandırma katmanını** seçin ve standart katmandan ücretsiz katmana abonelik değiştirmek için **ücretsiz** ' ı seçin.
5. **Kaydet**' i seçin.

Otomatik sağlamayı devre dışı bırakmak istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **fiyatlandırma & ayarları**' nı seçin.
2. Otomatik sağlamayı devre dışı bırakmak istediğiniz abonelikte temizleyin.
3. **Veri toplama** sekmesinde, **otomatik sağlamayı** **kapalı**olarak ayarlayın.
4. **Kaydet**' i seçin.

>[!NOTE]
> Otomatik sağlamayı devre dışı bırakmak, aracının sağlandığı Azure VM 'lerinden Microsoft Monitoring Agent kaldırmaz. Otomatik sağlama devre dışı bırakıldığında kaynaklarınız için güvenlik izleme sınırı vardır.
>

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta Standart katmana yükseltmiş ve hibrit bulut iş yükleriniz genelinde birleştirilmiş güvenlik yönetimi ve tehdit koruması için Microsoft Monitoring Agent sağlamış olursunuz. Güvenlik Merkezi 'ni kullanma hakkında daha fazla bilgi edinmek için, şirket içinde ve diğer bulutlarda bulunan Windows bilgisayarlarını ekleme hızlı başlangıçlarına devam edin.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Windows bilgisayarlarını Azure Güvenlik Merkezi 'ne ekleme](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/get-started.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
[9]: ./media/security-center-get-started/select-subscription.png
