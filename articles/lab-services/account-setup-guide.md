---
title: Azure Lab Services için hızlandırılmış laboratuvar hesabı kurulum kılavuzu
description: Bu kılavuz, yöneticilerin okuldaki kullanım için hızlı bir şekilde laboratuvar hesabı ayarlamış yardımcı olur.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: c186560b27ebcb543a23785dc5fbc556614f64b9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445432"
---
# <a name="lab-account-setup-guide"></a>Laboratuvar hesabı kurulum kılavuzu

İlk adım olarak, Yöneticiler Azure aboneliğinizde bir laboratuvar hesabı ayarlamış olmalıdır. Laboratuvar hesabı, sınıf laboratuvarlarınız için bir kapsayıcıdır ve yalnızca birkaç dakika sürer.

## <a name="understand-your-schools-lab-account-requirements"></a>Okulunuzun laboratuvar hesabı gereksinimlerini anlayın

Laboratuvar hesabınızı okulunuzun ihtiyaçlarına göre nasıl yapılandıracağınızı anlamak için, bu soruları göz önünde bulundurmanız gerekir.

### <a name="do-i-have-access-to-an-azure-subscription"></a>Bir Azure aboneliğine erişimim var mı?

Laboratuvar hesabı oluşturmak için okulunuz için yapılandırılmış bir Azure aboneliğine erişmeniz gerekir. Okulunuzda bir veya daha fazla abonelik olabilir. Laboratuvar hesapları da dahil olmak üzere tüm Azure kaynaklarınız ve hizmetleriniz için faturalandırma ve güvenliği yönetmek üzere bir abonelik kullanırsınız.

### <a name="how-many-lab-accounts-need-to-be-created"></a>Kaç laboratuvar hesabının oluşturulması gerekiyor?

Hızlıca kullanmaya başlamak için tek bir laboratuar hesabı oluşturun ve daha sonra gerektiğinde ek laboratuvar hesapları oluşturun. Örneğin, bölüm başına en sonunda bir laboratuar hesabınız olabilir.

### <a name="who-should-be-owners-and-contributors-of-the-lab-account"></a>Laboratuvar hesabının sahipleri ve katkıda bulunanlar kim olmalıdır?

Yöneticileriniz genellikle laboratuvar hesabı için sahipler ve katkıda bulunanlar. Laboratuvar hesabındaki tüm laboratuvarlara uygulanan ilkeleri yönetmekten sorumludur. Laboratuvar hesabını oluşturan kişi otomatik olarak bir sahip olur. Genellikle aboneliğinizle ilişkili olan Azure Active Directory (Azure AD) kiracısından ek sahipler ve katkıda bulunanlar ekleyebilirsiniz. Bu, laboratuvar hesabı düzeyinde sahip veya katkıda bulunan rolü atayarak bir laboratuvar hesabının yönetilmesine yardımcı olmak için yararlı olabilir.

### <a name="who-will-be-allowed-to-create-and-manage-labs"></a>Laboratuvarları oluşturmalarına ve yönetmesine izin verilecek kim?

Yöneticileriniz ve fakülte üyelerinizin laboratuvarları oluşturmasını ve yönetmesini tercih edebilirsiniz. Bu kullanıcılar (genellikle aboneliğinizle ilişkili Azure AD kiracısından) laboratuvar hesabı içindeki laboratuvar Oluşturucu rolüne atanır.

### <a name="do-you-want-to-give-lab-creators-the-ability-to-save-images-that-can-be-shared-across-labs"></a>Laboratuvar oluşturucuları, laboratuvarlar arasında paylaşılabilen görüntüleri kaydetme yeteneği vermek istiyor musunuz?

Paylaşılan görüntü Galerisi, görüntüleri kaydetmek ve paylaşmak için kullanabileceğiniz bir depodur. Aynı görüntülere ihtiyacı olan birkaç sınıfınız varsa, laboratuvar oluşturucuları görüntüyü bir kez oluşturabilir ve laboratuvarlar arasında paylaşabilir. Ancak, başlamak için, her zaman bir daha sonra ekleyebileceğiniz paylaşılan görüntü galerisine ihtiyacınız yoktur.

Bu soruya "Evet" yanıtını verdiyseniz, laboratuvar hesabınıza paylaşılan bir görüntü Galerisi oluşturmanız veya eklemeniz gerekir. "Bilmiyorum" yanıtını verdiyseniz, bu kararı daha sonra erteleyebilirsiniz.

### <a name="which-images-in-azure-marketplace-will-your-classroom-labs-use"></a>Ders laboratuvarlarınızın Azure Market 'teki hangi görüntüleri kullanması gerekir?

Azure Marketi, laboratuvar oluşturucularının laboratuvarı oluşturmak için görüntüyü kullanabilmesi için etkinleştirebileceğiniz yüzlerce görüntü sağlar. Bazı resimlerde, laboratuvarın zaten ihtiyacı olan her şey bulunabilir. Diğer durumlarda, bir görüntüyü başlangıç noktası olarak kullanabilir ve ardından Laboratuvar Oluşturucu ek uygulamalar veya Araçlar yükleyerek bunu özelleştirebilir.

Kullanmanız gereken görüntüleri bilmiyorsanız, bunları etkinleştirmek için her zaman daha sonra geri dönebilirsiniz. Ayrıca, hangi görüntülerin kullanılabilir olduğunu görmenin en iyi yolu, ilk olarak bir laboratuar hesabı oluşturmaktır. Bu sayede, kullanılabilir görüntülerin ve içeriklerinin listesini gözden geçirebilmeniz için erişim elde edersiniz.
  
### <a name="do-the-labs-virtual-machines-need-to-have-access-to-other-azure-or-on-premises-resources"></a>Laboratuvarın sanal makinelerinin diğer Azure veya şirket içi kaynaklara erişimi olması gerekiyor mu?

Bir laboratuvar hesabı ayarlarken, bir sanal ağ ile eş oluşturma seçeneğiniz de vardır. Bunun gerekli olup olmadığına karar vermek için aşağıdaki soruları göz önünde bulundurun:

- **Bir lisanslama sunucusuna erişim sağlamanız gerekiyor mu?**
  
   Azure Marketi görüntülerini kullanmayı planlıyorsanız, işletim sistemi lisansının maliyeti Laboratuvar Hizmetleri fiyatlandırmasına paketlenmiştir. Bu nedenle, işletim sisteminin kendisi için lisans sağlamanız gerekmez. Ancak, yüklü ek yazılımlar ve uygulamalar için uygun şekilde bir lisans sağlamanız gerekir.

- **Laboratuvar VM 'lerinin dosya paylaşma veya veritabanı gibi diğer şirket içi kaynaklara erişmesi gerekir mi?**

   Genellikle siteden siteye sanal ağ geçidi kullanarak şirket içi kaynaklara erişim sağlamak için bir sanal ağ oluşturursunuz. Yapılandırılmış bir sanal ağınız yoksa, bunun için ek zaman yatırmaya ihtiyacınız vardır.

- **Laboratuvar VM 'lerinin bir sanal ağ içinde bulunan diğer Azure kaynaklarına erişmesi gerekiyor mu?**

   Bir sanal ağ içinde güvenliği *bulunmayan* Azure kaynaklarına erişmeniz gerekiyorsa, herhangi bir eşleme yapmadan bu kaynaklara genel İnternet üzerinden erişebilirsiniz.

Bir veya daha fazla soruya "Evet" yanıtını verdiyseniz, laboratuvar hesabını bir sanal ağ ile eşetmeniz gerekir. "Bilmiyorum" yanıtını verdiyseniz, bu kararı daha sonra erteleyebilirsiniz. Laboratuvar hesabı oluşturduktan sonra her zaman bir sanal ağı tercih edebilirsiniz.

## <a name="set-up-your-lab-account"></a>Laboratuvar hesabınızı ayarlama

Laboratuvar hesabınızın gereksinimlerini anladıktan sonra, ayarlamaya hazırsınız demektir.

1. **Laboratuvar hesabınızı oluşturun.** Yönergeler için [Laboratuvar hesabı oluşturma](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#create-a-lab-account) öğreticisine bakın.

   Laboratuvar hesabı oluştururken, ilgili Azure kaynakları hakkında bilgi edinmek için size yardımcı olabilir. Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

   - [Abonelik](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#subscription)
   - [Kaynak grubu](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#resource-group)
   - [Laboratuvar hesabı](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#lab-account)
   - [Sınıf Laboratuvarı](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#classroom-lab)
   - [Bölge ve konum seçme](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#regionslocations)
   - [Kaynaklar için adlandırma Kılavuzu](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#naming)

2. **Laboratuvar Oluşturucu rolüne kullanıcı ekleyin.** Yönergeler için bkz. [Laboratuvar Oluşturucu rolüne kullanıcı ekleme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#add-a-user-to-the-lab-creator-role).

   Ayrıca, laboratuvar hesaplarını ve laboratuvarları yönetecek kullanıcılara atanabilecek farklı roller hakkında daha fazla bilgi için bkz. [kimlik yönetme Kılavuzu](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#manage-identity).

3. **Eş sanal ağa bağlanın.** Yönergeler için bkz. [laboratuvarınızın ağını bir eş sanal ağ ile bağlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network).

   [Laboratuvar VM 'leri adres aralığını yapılandırma](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-lab-accounts#specify-an-address-range-for-vms-in-the-lab)yönergeleri için de başvurmanız gerekebilir.

4. **Görüntüleri etkinleştirin ve gözden geçirin.** Yönergeler için bkz. [Laboratuvar oluşturucuları Için Azure Marketi görüntülerini etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images).

   Her bir Azure Marketi görüntüsünün içeriğini gözden geçirmek için görüntü adını seçin. Örneğin, aşağıdaki ekran görüntüsünde Ubuntu Veri Bilimi VM'si görüntüsünün ayrıntıları gösterilmektedir:

   ![Azure Marketi görüntülerini gözden geçirme ekran görüntüsü](./media/setup-guide/review-marketplace-images.png)

   Laboratuvar hesabınıza eklenmiş bir paylaşılan görüntü galeriniz varsa ve özel görüntülerin laboratuvar oluşturucuları tarafından paylaşılmasını etkinleştirmek istiyorsanız, aşağıdaki ekran görüntüsünde gösterilenler ile benzer adımları uygulayın:

   ![Paylaşılan görüntü galerisinde özel görüntüleri etkinleştirme ekran görüntüsü](./media/setup-guide/enable-sig-custom-images.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)

- [Sınıf Laboratuvarı Kurulum Kılavuzu](setup-guide.md)
