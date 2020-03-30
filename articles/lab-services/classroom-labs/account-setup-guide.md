---
title: Azure Lab Hizmetleri için hızlandırılmış laboratuvar hesabı kurulum kılavuzu
description: Bu kılavuz, yöneticilerin okulları içinde kullanılmak üzere hızlı bir şekilde bir laboratuvar hesabı ayarlamalarına yardımcı olur.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/18/2020
ms.author: spelluru
ms.openlocfilehash: 7b9ef32f16369bbb6c5bb69ef500ec8bb90fde20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80370870"
---
# <a name="lab-account-setup-guide"></a>Laboratuvar hesabı kurulum kılavuzu

Yöneticiler tarafından tamamlanması gereken ilk adım, Azure aboneliğinizde bir laboratuvar hesabı oluşturmaktır.  Laboratuvar hesabı sınıf laboratuarlarınız için bir konteynerdir ve kurulumu yalnızca birkaç dakika sürer.

## <a name="understand-your-schools-lab-account-requirements"></a>Okulunuzun laboratuvar hesabı gereksinimlerini anlama

Laboratuvar hesabınızı okulunuzun ihtiyaçlarına göre nasıl yapılandırabileceğinizi anlamak için aşağıdaki soruları göz önünde bulundurmanız gerekir:

**Azure aboneliğine erişimim var mı?**

Bir laboratuvar hesabı oluşturmak için okulunuzun yapılandırılmış bir Azure aboneliğine erişmeniz gerekir; okulunuzun bir veya daha fazla aboneliği olabilir.  Abonelik, laboratuvar hesapları da dahil olmak üzere içinde kullanılan tüm Azure kaynaklarınızın\hizmetlerinin faturalandırmasını ve güvenliğini yönetmek için kullanılır.

**Kaç tane laboratuvar hesabı oluşturulması gerekiyor?**

Hızlı bir şekilde başlamak için, makul bir yaklaşım tek bir laboratuvar hesabı oluşturmak ve daha sonra gerektiğinde ek Laboratuvar Hesapları oluşturmaktır.  Örneğin, sonunda departman başına bir laboratuvar hesabına sahip olmak için gelişebilir.

**Laboratuvar hesabının sahipleri ve katılımcıları kimler olmalıdır?**

Yöneticileriniz genellikle laboratuvar hesabının sahipleri'dir,çünkü laboratuvar hesabında bulunan tüm laboratuvarlar için geçerli olan ilkeleri yönetmekten sorumludurlar.  Laboratuvar hesabını oluşturan kişi otomatik olarak bir sahiptir.  Laboratuvar hesabı düzeyinde Sahibi\Katılımcı rolünü atayarak bir laboratuvar hesabını yönetmeye yardımcı olmak için ek sahipler\katkıda bulunanlar (genellikle aboneliğinizle ilişkili AAD kiracısından) ekleyebilirsiniz.

**Laboratuvarları oluşturmasına izin verilecek mi\?**

Yöneticilerinizin ve\veya öğretim üyelerinin laboratuvarlar oluşturmasını ve yönetmesini seçebilirsiniz; bu kullanıcılar (genellikle aboneliğinizle ilişkili AAD kiracısından) laboratuvar hesabı içindeki Lab Creator rolüne atanır.

**Laboratuvar oluşturucularına laboratuvarlar arasında paylaşılabilen görüntüleri kaydetme olanağı vermek istiyor musunuz?**

Paylaşılan Resim Galerisi, görüntüleri kaydetmek ve paylaşmak için kullanabileceğiniz bir depodur.  Bunun yararı, aynı görüntülere ihtiyaç duyan birkaç sınıfınız varsa, laboratuvar oluşturucuları görüntüyü bir kez oluşturabilir ve laboratuarlar arasında paylaşabilir.  Ancak, başlamak için, paylaşılan resim galerisi olmadan başlamak son derece makul; ve her zaman daha sonra bir eklemeyi seçebilirsiniz.

Bu soruya 'Evet' yanıtını verdiyseniz, laboratuvar hesabınıza paylaşılan resim galerisi oluşturmanız ve eklemeniz gerekir.  Eğer 'Bilmiyorum' diye cevap verdiyseniz, bu kararı daha sonraya erteleyebilirsiniz.

Laboratuvar hesabınıza bağlı paylaşılan bir Resim Galerisi varsa

**Azure Marketi'ndeki hangi görüntüleri sınıf laboratuvarlarınız kullanır?**

Azure Marketi, laboratuvar oluşturucularının laboratuvarlarını oluşturmak için görüntüyü kullanabilmeleri için etkinleştirebileceğiniz yüzlerce görüntü sağlar.  Bazı görüntüler, bir laboratuvarın zaten ihtiyaç duyduğu her şeyi içerebilir.  Diğer durumlarda, bir görüntüyü başlangıç noktası olarak kullanabilirsiniz ve daha sonra laboratuvar oluşturucusu ek uygulamalar, araçlar vb. yükleyerek görüntüyü özelleştirebilir.

Hangi görüntüleri kullanmanız gerektiğini bilmiyorsanız, bunları etkinleştirmek için her zaman daha sonra bu noktaya geri dönebilirsiniz.  Ayrıca, hangi görüntülerin mevcut olduğunu görmenin en iyi yolu, öncelikle bir laboratuvar hesabı oluşturmaktır – bu size erişim sağlar, böylece kullanılabilir resimlerin listesini ve içeriklerini inceleyebilirsiniz.  Daha fazla bilgi aşağıda verilmiştir.
  
**Laboratuvarın sanal makinelerinin (VM'lerin) diğer Azure veya prem kaynaklarına erişimi olması gerekiyor mu?**

Bir laboratuvar hesabı ayarladığınızda, sanal ağ (VNet) ile eşeme seçeneğiniz de vardır.  Bir VNet ile eşeihtiyacınız olup olmadığına karar vermek için aşağıdaki soruları göz önünde bulundurun:

- **Bir lisans sunucusuna erişim sağlamanız gerekiyor mu?**
  
   Azure Marketi görüntülerini kullanmayı planlıyorsanız, işletim sistemi lisansının maliyeti Laboratuvar Hizmetleri fiyatlandırmasına dahil edilir, bu nedenle işletim sistemi için lisans sağlamanız *gerekmez.*  Ancak, yüklenen ek yazılım\uygulamalar için, uygun bir lisans sağlamanız gerekir.

- **Laboratuvar VM'leri dosya paylaşımı, veritabanı vb. gibi diğer prem kaynaklarına erişmeye ihtiyaç duyar mı?**

   Genellikle siteden siteye Sanal Ağ Ağ Ağ Geçidi'ni kullanarak prem kaynaklarına erişim sağlamak için bir VNet oluşturulması gerekir.  Bir VNet yapılandırılmamışsa, bunun için ek zaman yatırılması gerekir.  Bu kurulumu niçin daha fazla bilgi aşağıda verilmiştir.

- **Laboratuvar VM'lerinin Bir VNet içinde bulunan diğer Azure kaynaklarına erişmeye ihtiyacı var mı?**

    Bir VNet içinde güvenli *olmayan* Azure kaynaklarına erişmeniz gerekiyorsa, bu kaynaklara herhangi bir bakış atmadan herkese açık internet üzerinden erişebilirsiniz.

    Bir veya daha fazla soruya 'Evet' yanıtını verdiyseniz, laboratuvar hesabını bir VNet'e göre bakmanız gerekir.  'Bilmiyorum' yanıtını verdiyseniz, laboratuvar hesabını oluşturduktan sonra her zaman bir VNet'e bakmayı seçebileceğiniz için bu kararı daha sonraya erteleyebilirsiniz.

## <a name="set-up-your-lab-account"></a>Laboratuvar hesabınızı ayarlama

Laboratuvar hesabınızın gereksinimlerini anladıktan sonra, hesabınızı ayarlamaya hazırsınız.  Laboratuvar hesabınızı nasıl ayarlayizleyeceğinizi görmek için bu bölümdeki bağlantıları izleyin:

1. **Laboratuvar hesabınızı oluşturun**

   Talimatlar için [bir laboratuvar hesabı oluşturma](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#create-a-lab-account) yla ilgili öğreticiye bakın.

   Bir laboratuvar hesabı oluştururken, ilgili Azure kaynaklarını tanımanızı yararlı bulabilirsiniz; bu kaynakların oluşturulması hakkında daha fazla bilgi ve rehberlik için aşağıdaki listeye bakın:

   - [Abonelik](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#subscription)
   - [Kaynak Grubu](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#resource-group)
   - [Laboratuvar Hesabı](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#lab-account)
   - [Sınıf Laboratuvarı](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#classroom-lab)
   - [Bölge\Konum Seçme](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#regions-or-locations)
   - [Kaynaklar için Yönlendirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#naming)

2. **Kullanıcıları Lab Creator rolüne ekleme**

   Talimatlar için [Lab Creator rolüne kullanıcı ekleme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#add-a-user-to-the-lab-creator-role) yle ilgili öğreticiye bakın.

   Ayrıca, laboratuvar hesaplarını ve laboratuvarlarını yönetecek kullanıcılara atanabilecek farklı roller hakkında daha fazla bilgi [için, kimlik yönetimi kılavuzuna](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#manage-identity)bakın.

3. **Eş bir VNet'e bağlanma**

   Talimatlar için [laboratuvar ağınızı bir eş VNet'e bağlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network) kılavuzuna bakın.

   Ayrıca, [laboratuvar VM adres aralığının yapılandırılmasıyla](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-lab-accounts#specify-an-address-range-for-vms-in-the-lab)ilgili talimatlara da başvurmanız gerekebilir.

4. **Görüntüleri etkinleştirme ve gözden geçirme**

    Talimatlar için [laboratuvar oluşturucuları için Market görüntülerini etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images) kılavuzuna bakın.

    Her Market resminin içeriğini incelemek için resim adını tıklatın.  Örneğin, Ubuntu Data Science VM görüntüsünün ayrıntılarını gösteren aşağıdaki ekran görüntüsüne bakın:

    ![Pazar yeri resimlerini inceleyin](../media/setup-guide/review-marketplace-images.png)

    Laboratuvar hesabınıza bağlı bir Paylaşılan Resim Galerisi niz varsa ve özel görüntülerin laboratuvar oluşturucuları tarafından paylaşılmasını etkinleştirmek istiyorsanız, aşağıdaki ekran görüntüsünde gösterildiği gibi benzer adımları tamamlamanız gerekir:

    ![Paylaşılan Resim Galerisi'nde özel görüntüleri etkinleştirme](../media/setup-guide/enable-sig-custom-images.png)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere bakın:

- [Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)

- [Sınıf laboratuvar kurulum kılavuzu](setup-guide.md)
