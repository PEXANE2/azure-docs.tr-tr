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
ms.openlocfilehash: 8fcc46487e7f7c2d075639f10a30cae9950ff31b
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879590"
---
# <a name="lab-account-setup-guide"></a>Laboratuvar hesabı kurulum kılavuzu

İlk adım olarak, yöneticilerin Azure aboneliğiniz içinde bir laboratuvar hesabı ayarlamaları gerekir. Laboratuvar hesabı sınıf laboratuarlarınız için bir konteynerdir ve kurulumu yalnızca birkaç dakika sürer.

## <a name="understand-your-schools-lab-account-requirements"></a>Okulunuzun laboratuvar hesabı gereksinimlerini anlama

Laboratuvar hesabınızı okulunuzun ihtiyaçlarına göre nasıl yapılandırabileceğinizi anlamak için bu soruları göz önünde bulundurmalısınız.

### <a name="do-i-have-access-to-an-azure-subscription"></a>Azure aboneliğine erişimim var mı?

Bir laboratuvar hesabı oluşturmak için okulunuzun yapılandırılmış bir Azure aboneliğine erişmeniz gerekir. Okulunuzun bir veya daha fazla aboneliği olabilir. Bir aboneliği, laboratuvar hesapları da dahil olmak üzere tüm Azure kaynaklarınız ve hizmetleriniz için faturalandırma ve güvenliği yönetmek için kullanırsınız.

### <a name="how-many-lab-accounts-need-to-be-created"></a>Kaç tane laboratuvar hesabı oluşturulması gerekiyor?

Hızlı bir şekilde başlamak için tek bir laboratuvar hesabı oluşturun ve ardından gerektiğinde ek laboratuvar hesapları oluşturun. Örneğin, sonunda departman başına bir laboratuvar hesabınız olabilir.

### <a name="who-should-be-owners-and-contributors-of-the-lab-account"></a>Laboratuvar hesabının sahipleri ve katılımcıları kimler olmalıdır?

Yöneticileriniz genellikle bir laboratuvar hesabının sahipleri ve katkıda bulunanlarıdır. Laboratuvar hesabında yer alan tüm laboratuvarlar için geçerli olan politikaların yönetiminden sorumludurlar. Laboratuvar hesabını oluşturan kişi otomatik olarak bir sahiptir. Genellikle aboneliğinizle ilişkili Azure Etkin Dizin (Azure AD) kiracısından ek sahipler ve katkıda bulunanlar ekleyebilirsiniz. Bu, laboratuvar hesabı düzeyinde sahibi veya katılımcı rolünü atayarak bir laboratuvar hesabının yönetilmesine yardımcı olmak için yararlı olabilir.

### <a name="who-will-be-allowed-to-create-and-manage-labs"></a>Laboratuvarları kimler oluşturup yönetebilecek?

Yöneticilerinizin ve öğretim üyelerinizin laboratuvarları oluşturmasını ve yönetmesini seçebilirsiniz. Bu kullanıcılar (genellikle aboneliğinizle ilişkili Azure AD kiracısından) laboratuvar hesabındaki Laboratuvar Oluşturucusu rolüne atanır.

### <a name="do-you-want-to-give-lab-creators-the-ability-to-save-images-that-can-be-shared-across-labs"></a>Laboratuvar oluşturucularına laboratuvarlar arasında paylaşılabilen görüntüleri kaydetme olanağı vermek istiyor musunuz?

Paylaşılan resim galerisi, görüntüleri kaydetmek ve paylaşmak için kullanabileceğiniz bir depodur. Aynı görüntülere ihtiyaç duyan birkaç sınıfınız varsa, laboratuvar oluşturucuları görüntüyü bir kez oluşturabilir ve laboratuarlar arasında paylaşabilir. Ancak, başlamak için paylaşılan bir resim galerisine ihtiyacınız yoktur, çünkü her zaman daha sonra ekleyebilirsiniz.

Bu soruya "evet" yanıtını verdiyseniz, laboratuvar hesabınıza paylaşılan bir resim galerisi oluşturmanız veya eklemeniz gerekir. "Bilmiyorum" yanıtını verdiysen bu kararı sonraya kadar erteleyebilirsin.

### <a name="which-images-in-azure-marketplace-will-your-classroom-labs-use"></a>Azure Marketi'ndeki hangi görüntüleri sınıf laboratuvarlarınız kullanacak?

Azure Marketi, laboratuvar oluşturucularının laboratuvarlarını oluşturmak için görüntüyü kullanabilmeleri için etkinleştirebileceğiniz yüzlerce görüntü sağlar. Bazı görüntüler, bir laboratuvarın zaten ihtiyaç duyduğu her şeyi içerebilir. Diğer durumlarda, bir görüntüyü başlangıç noktası olarak kullanabilirsiniz ve ardından laboratuvar oluşturucusu ek uygulamalar veya araçlar yükleyerek görüntüyü özelleştirebilir.

Hangi görüntüleri kullanmanız gerektiğini bilmiyorsanız, bunları etkinleştirmek için her zaman daha sonra bu noktaya geri dönebilirsiniz. Ayrıca, hangi görüntülerin kullanılabildiği görmek için en iyi yolu ilk önce bir laboratuvar hesabı oluşturmaktır. Bu, kullanılabilir resimlerin listesini ve içeriklerini gözden geçirebilmeniz için size erişim sağlar.
  
### <a name="do-the-labs-virtual-machines-need-to-have-access-to-other-azure-or-on-premises-resources"></a>Laboratuvarın sanal makinelerinin diğer Azure veya şirket içi kaynaklara erişimi olması gerekiyor mu?

Bir laboratuvar hesabı ayarladığınızda, sanal ağla eşeme seçeneğiniz de var. Buna ihtiyacınız olup olmadığına karar vermek için aşağıdaki soruları göz önünde bulundurun:

- **Bir lisans sunucusuna erişim sağlamanız gerekiyor mu?**
  
   Azure Marketi görüntülerini kullanmayı planlıyorsanız, işletim sistemi lisansının maliyeti laboratuvar hizmetlerinin fiyatlandırmasına dahil edilir. Bu nedenle, işletim sisteminin kendisi için lisans sağlamanız gerekmez. Ancak, yüklenen ek yazılım lar ve uygulamalar için uygun şekilde bir lisans sağlamanız gerekir.

- **Laboratuvar VM'lerinin dosya paylaşımı veya veritabanı gibi diğer şirket içi kaynaklara erişmeye ihtiyacı var mı?**

   Genellikle siteden siteye sanal ağ ağ ağ geçidi kullanarak şirket içi kaynaklara erişim sağlamak için sanal bir ağ oluşturursunuz. Yapılandırılmış bir sanal ağınız yoksa, bunun için ek zaman yatırmanız gerekir.

- **Laboratuvar Sanal M'lerinin sanal ağ içinde bulunan diğer Azure kaynaklarına erişmeye ihtiyacı var mı?**

   Sanal ağ içinde güvenli *olmayan* Azure kaynaklarına erişmeniz gerekiyorsa, bu kaynaklara herhangi bir göz atamadan herkese açık internet üzerinden erişebilirsiniz.

Bir veya daha fazla soruya "evet" yanıtını verdiyseniz, laboratuvar hesabını sanal bir ağa eşlemeniz gerekir. "Bilmiyorum" diye cevap verdiysen bu kararı sonraya kadar erteleyebilirsin. Laboratuvar hesabını oluşturduktan sonra sanal ağa eş vermeyi her zaman seçebilirsiniz.

## <a name="set-up-your-lab-account"></a>Laboratuvar hesabınızı ayarlama

Laboratuvar hesabınızın gereksinimlerini anladıktan sonra, hesabınızı ayarlamaya hazırsınız.

1. **Laboratuvar hesabınızı oluşturun.** Talimatlar için [bir laboratuvar hesabı oluşturma](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#create-a-lab-account) yla ilgili öğreticiye bakın.

   Bir laboratuvar hesabı oluştururken, ilgili Azure kaynaklarını tanımanızı yararlı bulabilirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

   - [Abonelik](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#subscription)
   - [Kaynak grubu](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#resource-group)
   - [Laboratuvar hesabı](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#lab-account)
   - [Sınıf laboratuvarı](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#classroom-lab)
   - [Bölge ve konum seçme](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#regionslocations)
   - [Kaynaklar için adlandırma kılavuzu](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#naming)

2. **Kullanıcıları laboratuvar oluşturucu rolüne ekleyin.** Yönergeler için, [laboratuvar oluşturucu rolüne kullanıcı ekleme ye](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#add-a-user-to-the-lab-creator-role)bakın.

   Ayrıca, laboratuvar hesaplarını ve laboratuvarlarını yönetecek kullanıcılara atanabilecek farklı roller hakkında daha fazla bilgi [için, kimlik yönetimi kılavuzuna](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#manage-identity)bakın.

3. **Eş sanal ağa bağlanın.** Talimatlar için, [laboratuvarınızın ağını bir eş sanal ağına bağlamaya](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network)bakın.

   Ayrıca, [laboratuvar VM adres aralığının yapılandırılmasıyla](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-lab-accounts#specify-an-address-range-for-vms-in-the-lab)ilgili talimatlara da başvurmanız gerekebilir.

4. **Görüntüleri etkinleştirin ve gözden geçirin.** Talimatlar için, [laboratuvar oluşturucuları için Azure Marketi görüntülerini etkinleştirme'ye](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images)bakın.

   Her Azure Marketi görüntüsünün içeriğini gözden geçirmek için resim adını seçin. Örneğin, aşağıdaki ekran görüntüsü Ubuntu Data Science VM görüntüsünün ayrıntılarını gösterir:

   ![Azure Marketi görüntülerini gözden geçirin ekran görüntüsü](../media/setup-guide/review-marketplace-images.png)

   Laboratuvar hesabınıza bağlı paylaşılan bir resim gaseniz ve özel görüntülerin laboratuvar oluşturucuları tarafından paylaşılmasını etkinleştirmek istiyorsanız, aşağıdaki ekran görüntüsünde gösterilenlere benzer adımları tamamlayın:

   ![Paylaşılan resim galerisinde özel görüntüleri etkinleştirme ekran görüntüsü](../media/setup-guide/enable-sig-custom-images.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)

- [Sınıf laboratuvar kurulum kılavuzu](setup-guide.md)
