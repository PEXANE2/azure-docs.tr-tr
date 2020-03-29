---
title: Standart tier'in gelişmiş güvenliği - Azure Güvenlik Merkezi
description: " Gelişmiş güvenlik için Azure Güvenlik Merkezi Standardı'na nasıl biningerektiğini öğrenin. "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/19/2019
ms.author: memildin
ms.openlocfilehash: be26a9d4c66412518079de303ac0764d979c3e7c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77912061"
---
# <a name="onboarding-to-azure-security-center-standard-for-enhanced-security"></a>Gelişmiş güvenlik için Azure Güvenlik Merkezi Standart sürümüne ekleme
Karma bulut iş yükleriniçin gelişmiş güvenlik yönetimi ve tehdit korumasından yararlanmak için Güvenlik Merkezi Standardına yükseltin. Standart ücretsiz deneyebilirsiniz. Daha fazla bilgi için Güvenlik Merkezi [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/) bakın.

Güvenlik Merkezi standart katmanı şunları içerir:

- **Karma güvenlik** – Tüm şirket içi ve bulut iş yüklerinizde güvenliğin birleşik bir görünümünü elde edin. Güvenlik ilkelerini uygulayın ve güvenlik standartlarına uygunluğu sağlamak için karma bulut iş yüklerinizin güvenliğini sürekli olarak değerlendirin. Güvenlik duvarları ve diğer iş ortağı çözümleri de dahil olmak üzere çeşitli kaynaklardan güvenlik verilerini toplayın, arayın ve analiz edin.
- **Güvenlik uyarıları** - Gelişen siber saldırılara karşı avantaj elde etmek için gelişmiş analizleri ve Microsoft Akıllı Güvenlik Grafiği'ni kullanın. Saldırıları ve sıfır gün açıklarını belirlemek için yerleşik davranış analizive makine öğreniminden yararlanın. Gelen saldırılar ve ihlal sonrası etkinlikler için ağları, makineleri ve bulut hizmetlerini izleyin. Etkileşimli araçlar ve bağlamsal tehdit istihbaratı ile araştırmayı kolaylaştırın.
- **Erişim ve uygulama denetimleri** - Belirli iş yüklerinize uyarlanmış ve makine öğrenimi tarafından desteklenen beyaz liste önerileri uygulayarak kötü amaçlı yazılımları ve diğer istenmeyen uygulamaları engelleyin. Azure VM'lerde yönetim bağlantı noktalarına tam zamanında, kontrollü erişimle ağ saldırı yüzeyini küçülterek kaba kuvvete ve diğer ağ saldırılarına maruz kalma riskini önemli ölçüde azaltın.

## <a name="detecting-unprotected-resources"></a>Korumasız kaynakları algılama
Güvenlik Merkezi, Güvenlik Merkezi Standart sürümü için etkinleştirilmemiş herhangi bir Azure aboneliğini veya çalışma alanını otomatik olarak algılar. Buna, Güvenlik Merkezi Ücretsiz sürümünü kullanan Azure abonelikleri ve etkin bir Güvenlik çözümü olmayan çalışma alanları dahildir.

Tüm Azure aboneliğini, abonelik teki tüm desteklenen kaynaklar tarafından devralınan Standart katmana yükseltebilirsiniz. Standart katmanı bir çalışma alanına uygulamak, çalışma alanına rapor veren tüm kaynaklar için geçerlidir.

> [!NOTE]
> Maliyetlerinizi yönetmek ve belirli bir aracı kümesiyle sınırlayarak bir çözüm için toplanan veri miktarını sınırlamak isteyebilirsiniz. [Çözüm hedefleme,](../operations-management-suite/operations-management-suite-solution-targeting.md) çözüme bir kapsam uygulamanızı ve çalışma alanındaki bilgisayarların bir alt kümesini hedeflemenize olanak tanır. Çözüm hedeflemekullanıyorsanız, Güvenlik Merkezi çalışma alanını çözüme sahip değil olarak listeler.
>
>

## <a name="upgrade-an-azure-subscription-or-workspace"></a>Azure aboneliğini veya çalışma alanını yükseltme
Aboneliği veya çalışma alanını standart olarak yükseltmek için:
1. Güvenlik Merkezi ana menüsü altında, **Başlarken**’i seçin.
  ![Başlarken](./media/security-center-onboarding/get-started.png)
2. **Yükselt** altında, Güvenlik Merkezi, ekleme işlemi için uygun abonelikleri ve çalışma alanlarını listeler. 
   - Tüm abonelikleri ve çalışma alanlarını deneme sürümü uygunluk durumlarıyla birlikte listelemek için, genişletilebilir **Deneme sürümünüzü uygulayın**’a tıklayabilirsiniz.
   -    Deneme sürümü için uygun olmayan abonelikler ve çalışma alanlarını yükseltebilirsiniz.
   -    Deneme sürümünüzü başlatmak için uygun çalışma alanlarını ve abonelikleri seçebilirsiniz.
3.  Seçili aboneliklerde deneme sürümünüzü başlatmak için **Deneme sürümünü başlat**’a tıklayın.
  ![Aboneliği seçin](./media/security-center-onboarding/select-subscription.png)


   > [!NOTE]
   > Güvenlik Merkezi'nin Ücretsiz özellikleri yalnızca Azure VM'lerinize ve VMSS'lerinize uygulanır. Azure olmayan bilgisayarlarınıza Ücretsiz özellikler uygulanmaz. Standart'ı seçerseniz, Standart özellikleri çalışma alanına rapor eden tüm Azure VM'lere, VM ölçek kümelerine ve Azure'a uygun olmayan bilgisayarlara uygulanır. Azure ve Azure olmayan kaynaklarınız için gelişmiş güvenlik sağlamak için Standart'ı uygulamanızı öneririz.
   >

## <a name="onboard-non-azure-computers"></a>Yerleşik Azure olmayan bilgisayarlar
Güvenlik Merkezi, Azure dışı bilgisayarların güvenlik durumunu izleyebilir ancak öncelikle bu kaynakları eklemeniz gerekir. Azure olmayan bilgisayarları **Başlangıç** bıçağından veya **İşlem** bıçağından ekleyebilirsiniz. Her iki yöntemde de yürüyeceğiz.

### <a name="add-new-non-azure-computers-from-getting-started"></a>**Başlangıç** ayından yeni Azure olmayan bilgisayarlar ekleme

1. **Başlarken**geri dön.
2. **Başlangıç** sekmesini seçin.

   ![Azure'a Ait Olmayan](./media/security-center-onboarding/non-azure.png)

3. **Yeni Azure dışı bilgisayarlar ekle** altında, **Yapılandır**’a tıklayın. Log Analytics çalışma alanlarınızın bir listesi gösterilir. Listede, varsa, otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi tarafından sizin için oluşturulan varsayılan çalışma alanı bulunur. Bu çalışma alanını veya kullanmak istediğiniz başka bir çalışma alanını seçin.

   ![Azure dışı bilgisayar ekleme][7]

Varolan çalışma alanlarınız varsa, **bunlar yeni Azure olmayan bilgisayarlar ekle**altında listelenir. Varolan bir çalışma alanına bilgisayar ekleyebilir veya yeni bir çalışma alanı oluşturabilirsiniz. Yeni bir çalışma alanı oluşturmak için, **yeni bir çalışma alanı eklemek**bağlantısını seçin.

### <a name="add-new-non-azure-computers-from-compute"></a>**İşlem'den** yeni Azure olmayan bilgisayarlar ekleme

**Yeni bir çalışma alanı oluşturun ve bilgisayar ekleyin**

1. **Azure'a yeni olmayan bilgisayarlar ekle'nin** **altında, yeni bir çalışma alanı ekleyin'i**seçin.

   ![Yeni bir çalışma alanı ekleme][4]

2. **Güvenlik ve Denetim**altında, yeni bir çalışma alanı oluşturmak için **OMS Çalışma Alanı'nı** seçin.
   > [!NOTE]
   > OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.
3. **OMS Çalışma Alanı**altında, çalışma alanınız için bilgileri girin.
4. **OMS Çalışma Alanı**altında **Tamam'ı**seçin. Tamam'ı seçtikten sonra, aracıyı yapılandırmada kullanmak üzere çalışma alanı kimliğiniz için bir Windows veya Linux aracısı ve anahtarları indirmek için bir bağlantı alırsınız.
5. **Güvenlik ve Denetim**altında, **Tamam'ı**seçin.

**Varolan bir çalışma alanı seçin ve bilgisayar ekleyin**

Yukarıda gösterildiği gibi **Onboarding'den**gelen iş akışını izleyerek bir bilgisayar ekleyebilirsiniz. **Ayrıca, Compute'dan**iş akışını izleyerek bir bilgisayar ekleyebilirsiniz. Bu örnekte, **Compute'u**kullanıyoruz.

1. Güvenlik Merkezi'nin ana menüsüne ve **Genel Bakış** panosuna dönün.

   ![Genel Bakış][5]

2. **Uygulamaları & İşlem'i**seçin.
3. **& uygulamaları hesapla,** **bilgisayar ekle'yi**seçin.

   ![Bilgi İşlem dikey penceresi][6]

4. **Azure'a yeni olmayan bilgisayarlar ekle'nin**altında, bilgisayarınızı bağlamak için bir çalışma alanı seçin ve Bilgisayar **Ekle'yi**tıklatın.

   ![Bilgisayar ekleme][7]

   **Direct Agent** blade, bir Windows veya Linux aracısının yanı sıra aracıyı yapılandırmada kullanılacak çalışma alanı kimliği ve anahtarları indirmek için bir bağlantı sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nin gelişmiş güvenliğinden yararlanmak için Azure ve Azure olmayan kaynaklara nasıl bindirileceğinizi öğrendiniz. Yerleşik kaynaklarınızla daha fazla şey yapmak için bkz.

- [Veri toplamayı etkinleştirme](security-center-enable-data-collection.md)
- [Tehdit zekası raporu](security-center-threat-report.md)
- [Tam zamanında VM erişimi](security-center-just-in-time.md)

<!--Image references-->
[1]: ./media/security-center-onboarding/onboard.png
[2]: ./media/security-center-onboarding/onboard-subscription.png
[3]: ./media/security-center-onboarding/get-started.png
[4]: ./media/security-center-onboarding/create-workspace.png
[5]: ./media/security-center-onboarding/overview.png
[6]: ./media/security-center-onboarding/compute-blade.png
[7]: ./media/security-center-onboarding/add-computer.png
[8]: ./media/security-center-onboarding/onboard-workspace.png
