---
title: Azure Lab Services için hızlandırılmış laboratuvar hesabı kurulum kılavuzu
description: Bu kılavuz, yöneticilerin okuldaki kullanım için hızlı bir şekilde laboratuvar hesabı ayarlamış yardımcı olur.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 4d9a64fe23c3e5b74e77e704154f5e74bf2066d9
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94490568"
---
# <a name="lab-account-setup-guide"></a>Laboratuvar hesabı kurulum kılavuzu
Azure Lab Services ortamınızı ayarlamak için, yöneticilerin öncelikle Azure aboneliğinizde bir **Laboratuvar hesabı** ayarlaması gerekir. Laboratuvar hesabı, laboratuvarlarınız için bir kapsayıcıdır ve yalnızca birkaç dakika sürer.

Bu kılavuz üç bölümden oluşur:
-  İlk bölüm, laboratuvar hesabınızı *ayarlamadan önce* tamamlanması gereken önkoşullara odaklanır.
-  İkinci bölüm, laboratuvar hesabı ayarlarınızı planlamaya yönelik rehberlik sağlar.
-  Üçüncü bölüm, bir laboratuar hesabı ayarlamaya yönelik adım adım yönergeler sağlar.

## <a name="prerequisites-for-setting-up-your-lab-account"></a>Laboratuvar hesabınızı ayarlamaya yönelik önkoşullar
Bu bölümde, bir laboratuvar hesabı ayarlamadan önce gerçekleştirmeniz gereken önkoşullar özetlenmektedir.

### <a name="obtain-an-azure-subscription"></a>Azure aboneliği edinme
Laboratuvar hesabı oluşturmak için okulunuz için ayarlanmış bir Azure aboneliğine erişmeniz gerekir. Okulunuzda bir veya daha fazla abonelik olabilir. Laboratuvar hesapları da dahil olmak üzere tüm Azure kaynaklarınız ve hizmetleriniz için faturalandırma ve güvenliği yönetmek üzere bir abonelik kullanırsınız.  Azure abonelikleri genellikle BT departmanınız tarafından yönetilir.  Daha fazla bilgi için aşağıdaki konuyu okuyun:
 - [Yönetici Kılavuzu-abonelik](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#subscription)

### <a name="estimate-the-number-of-vms-and-vm-sizes-that-you-need"></a>İhtiyaç duyduğunuz VM 'Lerin ve VM boyutlarının sayısını tahmin edin
Okulunuzun ihtiyaç duyduğu sanal makine (VM) sayısını ve [VM boyutlarını](https://docs.microsoft.com/azure/lab-services/administrator-guide#vm-sizing) tahmin etmeniz gerekir.  Labs\images'nizi nasıl yapılandıracağınıza ilişkin yönergeler için aşağıdaki blog gönderisini okuyun.  Bu blog gönderisi Ayrıca ihtiyaç duyacağınız sanal makine sayısına ve VM boyutlarına karar vermenize yardımcı olur:
- [Fiziksel laboratuvardan Azure Lab Services taşıma](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931)

Ayrıca, laboratuvarların nasıl yapılandırılabileceğine ilişkin ek yönergeler açıklayan bu makaleye bakın:
- [Yönetici Kılavuzu-laboratuvar](https://docs.microsoft.com/azure/lab-services/administrator-guide#classroom-lab)

### <a name="understand-subscription-vm-limits-and-regional-vm-capacity"></a>Abonelik VM sınırlarını ve bölgesel VM kapasitesini anlayın
Laboratuvarlarınızın sanal makine sayısı ve VM boyutlarının tahmininden sonra şunları yapmanız gerekir:

- Azure aboneliğinizin kapasite sınırının, laboratuvarlarda kullanmayı planladığınız sanal makine sayısı ve VM boyutu için izin verdiğinden emin olun.

- Kullanılabilir yeterli VM kapasitesi olan bir bölge içinde laboratuvar hesabınızı oluşturun.

Daha fazla bilgi edinmek için aşağıdaki blog gönderisini okuyun: [VM abonelik limitleri ve bölgesel kapasite](https://techcommunity.microsoft.com/t5/azure-lab-services/vm-subscription-limits-and-regional-capacity/ba-p/1845553).

### <a name="decide-how-many-lab-accounts-to-create"></a>Kaç tane laboratuvar hesabı oluşturulacağını belirleyin

Hızlıca kullanmaya başlamak için kendi kaynak grubu içinde tek bir laboratuar hesabı oluşturun.  Daha sonra, gerektiğinde ek laboratuvar hesapları (ve kaynak grupları) oluşturabilirsiniz. Örneğin, en sonunda, maliyetleri açık bir şekilde ayırmak için bir laboratuvar hesabına ve bir departman başına kaynak grubuna sahip olabilirsiniz.  Laboratuvar hesapları, kaynak grupları ve maliyet ayrımı hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri okuyun:
- [Yönetici Kılavuzu-kaynak grubu](https://docs.microsoft.com/azure/lab-services/administrator-guide#resource-group)
- [Yönetici Kılavuzu-laboratuvar hesabı](https://docs.microsoft.com/azure/lab-services/administrator-guide#lab-account) 
- [Azure Lab Services için maliyet yönetimi](https://docs.microsoft.com/azure/lab-services/cost-management-guide)

## <a name="planning-your-lab-accounts-settings"></a>Laboratuvar hesabınızın ayarlarını planlama

Laboratuvar hesabınızın ayarlarını planlamak için aşağıdaki soruları göz önünde bulundurmanız gerekir.

### <a name="who-should-be-owners-and-contributors-of-the-lab-account"></a>Laboratuvar hesabının sahipleri ve katkıda bulunanlar kim olmalıdır?

   Okulunuzun BT yöneticileri genellikle laboratuvar hesabı için sahipler ve katkıda bulunanlar olur. Laboratuvar hesabındaki tüm laboratuvarlara uygulanan ilkeleri yönetmekten sorumludur. Laboratuvar hesabını oluşturan kişi otomatik olarak bir sahip olur. Aboneliğinizle ilişkili Azure Active Directory (AD) kiracısından ek sahipler ve katkıda bulunanlar ekleyebilirsiniz. Laboratuvar hesabı sahibi ve katkıda bulunan rolleri hakkında daha fazla bilgi için şunu okuyun:
   -  [Yönetici Kılavuzu-kimliği Yönet](https://docs.microsoft.com/azure/lab-services/administrator-guide#manage-identity).

   [!INCLUDE [Select a tenant](./includes/multi-tenant-support.md)]

   Laboratuvar kullanıcıları yalnızca Azure Lab Services içindeki kiracılar arasında erişimi olan sanal makinelerin tek bir listesini görür.

### <a name="who-will-be-allowed-to-create-labs"></a>Laboratuvarların oluşturmasına izin verilecek kim?

   BT ve fakülte üyelerinizin laboratuvarları oluşturmasını tercih edebilirsiniz. Bir Kullanıcı laboratuvar oluşturduğunda, otomatik olarak laboratuvarın sahibi olarak atanır.  Laboratuvarlar oluşturmak için (genellikle aboneliğinizle ilişkili Azure AD kiracısından) laboratuvar hesabındaki laboratuvar Oluşturucu rolüne atanmalıdır.  Laboratuvar Oluşturucu rolü hakkında daha fazla bilgi için şunu okuyun:
   -  [Yönetici Kılavuzu-kimlik yönetme](https://docs.microsoft.com/azure/lab-services/administrator-guide#manage-identity)

### <a name="who-will-be-allowed-to-own-and-manage-labs"></a>Laboratuvarlara sahip ve bunları yönetmesine izin verilecek kim?

   Ayrıca, Labs 'in laboratuvarları oluşturma *yeteneği vermeden own\manage Labs 'i* de ve fakülte olarak tercih edebilirsiniz.  Bu durumda, aboneliğinizin Azure AD kiracısındaki kullanıcılara mevcut laboratuvarlara sahip veya katılımcı atanır.  Laboratuvarın sahibi ve katkıda bulunan rolleri hakkında daha fazla bilgi için şunu okuyun:
   - [Yönetici Kılavuzu-kimlik yönetme](https://docs.microsoft.com/azure/lab-services/administrator-guide#manage-identity)

### <a name="do-you-want-to-save-images-that-can-be-shared-across-labs"></a>Laboratuvarlar arasında paylaşılabilen görüntüleri kaydetmek istiyor musunuz?
Paylaşılan görüntü Galerisi, görüntüleri kaydetmek ve paylaşmak için kullanabileceğiniz bir depodur. Aynı görüntüde olması gereken sınıflar için, laboratuvar oluşturucuları görüntüyü oluşturup paylaşılan görüntü galerisine dışarı aktarabilir.  Bir görüntü paylaşılan görüntü galerisine aktarıldığında, yeni Labs oluşturmak için kullanılabilir.

Ayrıca, görüntülerinizi fiziksel ortamınızda oluşturup paylaşılan görüntü galerisine aktarabilirsiniz.  Bu işlemle ilgili daha fazla bilgi için aşağıdaki blog gönderisini okuyun: 
- [Özel görüntüyü paylaşılan görüntü galerisine aktar](https://techcommunity.microsoft.com/t5/azure-lab-services/import-custom-image-to-shared-image-gallery/ba-p/1777353)

Paylaşılan bir görüntü Galerisi kullanmanız gerektiğine karar verirseniz, laboratuvar hesabınıza paylaşılan bir görüntü Galerisi oluşturmanız veya eklemeniz gerekir. Ayrıca, bu kararı daha sonra bir laboratuvar hesabına iliştirilene kadar erteleyebilirsiniz.  Paylaşılan görüntü galerisi hakkında daha fazla bilgi için şunu okuyun:
- [Yönetici Kılavuzu-paylaşılan görüntü Galerisi](https://docs.microsoft.com/azure/lab-services/administrator-guide#shared-image-gallery)
- [Yönetici Kılavuzu-paylaşılan görüntü Galerisi fiyatlandırması](https://docs.microsoft.com/azure/lab-services/administrator-guide#shared-image-gallery-2)

### <a name="which-images-in-azure-marketplace-will-your-labs-use"></a>Azure Marketi 'ndeki hangi görüntüler laboratuvarlarınız için kullanılır?
Azure Marketi, laboratuvar oluşturucularının laboratuvarı oluşturmak için görüntüyü kullanabilmesi için etkinleştirebileceğiniz yüzlerce görüntü sağlar. Bazı resimlerde, laboratuvarın zaten ihtiyacı olan her şey bulunabilir. Diğer durumlarda, bir görüntüyü başlangıç noktası olarak kullanabilir ve ardından Laboratuvar Oluşturucu ek uygulamalar veya Araçlar yükleyerek bunu özelleştirebilir.

Hangi görüntülerden ihtiyacınız olduğunu bilmiyorsanız, daha sonra bunları etkinleştirmek için daha sonra geri dönebilirsiniz. Ayrıca, hangi görüntülerin kullanılabilir olduğunu görmenin en iyi yolu, ilk olarak bir laboratuar hesabı oluşturmaktır. Bu, kullanılabilir görüntülerin ve içeriklerinin listesini gözden geçirebilmeniz için size erişmenizi sağlar.  Market görüntüleri hakkında daha fazla bilgi için şunu okuyun:
- [Laboratuvar oluşturucuları için kullanılabilen Market görüntülerini belirtme](https://docs.microsoft.com/azure/lab-services/specify-marketplace-images)
  
### <a name="do-the-labs-vms-need-to-have-access-to-other-azure-or-on-premises-resources"></a>Laboratuvarın VM 'lerinin diğer Azure veya şirket içi kaynaklara erişimi olması gerekiyor mu?
Laboratuvar hesabı ayarladığınızda, laboratuvar hesabınızı bir sanal ağ (VNet) ile de eşleyebilir.  Hem VNet hem de laboratuvar hesabınızın aynı bölgede bulunması gerektiğini aklınızda bulundurun.  VNet ile eş yapmanız gerekip gerekmediğine karar vermek için aşağıdaki senaryoları göz önünde bulundurun:

- **Lisanslama sunucusuna erişim**
  
   Azure Marketi görüntülerini kullandığınızda, işletim sistemi lisansının maliyeti Laboratuvar Hizmetleri fiyatlandırmasına paketlenmiştir. Ancak, işletim sisteminin kendisi için lisans sağlamanız gerekmez. Ancak, yüklü ek yazılımlar ve uygulamalar için uygun şekilde bir lisans sağlamanız gerekir.  Bir lisanslama sunucusuna erişmek için:
   - Şirket içi lisanslama sunucusuna bağlanmayı seçebilirsiniz.  Şirket içi lisanslama sunucusuna bağlanmak ek kurulum gerektirir.
   - Daha hızlı bir şekilde ayarlanarak, bir Azure VM üzerinde barındırmanıza olanak tanıyan bir lisans sunucusu oluşturmaktır.  Azure VM, laboratuvar hesabınıza sahip olduğunuz bir sanal ağ içinde bulunur.

- **Dosya paylaşma veya veritabanı gibi diğer şirket içi kaynaklara erişim**

   Genellikle siteden siteye sanal ağ geçidi kullanarak şirket içi kaynaklara erişim sağlamak için bir VNet oluşturursunuz. Bu ortam türünün ayarlanması, ek zaman alır.

- **VNet dışında bulunan diğer Azure kaynaklarına erişim**

   Bir sanal ağ içinde güvenli *olmayan* Azure kaynaklarına erişmeniz gerekiyorsa, herhangi bir eşleme yapmadan bu kaynaklara genel İnternet üzerinden erişebilirsiniz.

Sanal ağlar hakkında daha fazla bilgi için şunu okuyun:
- [Mimari temelleri-sanal ağ](https://docs.microsoft.com/azure/lab-services/classroom-labs-fundamentals#virtual-network)
- [Sanal ağa bağlanma](https://docs.microsoft.com/azure/lab-services/how-to-connect-peer-virtual-network)
- [Azure Lab Services paylaşılan kaynakla Laboratuvar oluşturma](https://docs.microsoft.com/azure/lab-services/how-to-create-a-lab-with-shared-resource)

## <a name="set-up-your-lab-account"></a>Laboratuvar hesabınızı ayarlama

Planlamayı tamamladıktan sonra laboratuvar hesabınızı ayarlamaya hazırsınız demektir.  Bu adımlar, [takımlar ile](https://docs.microsoft.com/azure/lab-services/lab-services-within-teams-overview)bir laboratuvar Azure Lab Services ayarlamak için geçerlidir.

1. **Laboratuvar hesabınızı oluşturun.** Yönergeler için [Laboratuvar hesabı oluşturma](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#create-a-lab-account) öğreticisine bakın.
   
    Adlandırma hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

   - [Kaynaklar için adlandırma Kılavuzu](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#naming)

2. **Laboratuvar Oluşturucu rolüne kullanıcı ekleyin.** Yönergeler için bkz. [Laboratuvar Oluşturucu rolüne kullanıcı ekleme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#add-a-user-to-the-lab-creator-role).


3. **Eş sanal ağa bağlanın.** Yönergeler için bkz. [laboratuvarınızın ağını bir eş sanal ağ ile bağlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network).

   [Laboratuvar VM 'leri adres aralığını yapılandırma](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-lab-accounts#specify-an-address-range-for-vms-in-the-lab)yönergeleri için de başvurmanız gerekebilir.

4. **Görüntüleri etkinleştirin ve gözden geçirin.** Yönergeler için bkz. [Laboratuvar oluşturucuları Için Azure Marketi görüntülerini etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images).

   Her bir Azure Marketi görüntüsünün içeriğini gözden geçirmek için görüntü adını seçin. Örneğin, aşağıdaki ekran görüntüsünde Ubuntu Veri Bilimi VM'si görüntüsünün ayrıntıları gösterilmektedir:

   ![Azure Marketi görüntülerini gözden geçirme ekran görüntüsü](./media/setup-guide/review-marketplace-images.png)

   Laboratuvar hesabınıza bir paylaşılan görüntü Galerisi eklenmişse ve özel görüntülerin laboratuvar oluşturucuları tarafından paylaşılmasını etkinleştirmek istiyorsanız, aşağıdaki ekran görüntüsünde gösterildiği gibi benzer adımları uygulayın:

   ![Paylaşılan görüntü galerisinde özel görüntüleri etkinleştirme ekran görüntüsü](./media/setup-guide/enable-sig-custom-images.png)

## <a name="next-steps"></a>Sonraki adımlar

Laboratuvar ortamının kurulması için ortak olan sonraki adımlar:

- [Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)
- [Laboratuvar Kurulum Kılavuzu](setup-guide.md)
