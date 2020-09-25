---
title: Sanal makineler için Azure oto yönetimi SSS
description: Sanal makineler için Azure oto yönetimi hakkında sık sorulan soruların yanıtları.
author: deanwe
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: troubleshooting
ms.date: 09/04/2020
ms.author: deanwe
ms.openlocfilehash: fa24c0db398c4c799d218ff5e8ec8e3d3e321742
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91311555"
---
# <a name="frequently-asked-questions-for-azure-automanage-for-vms"></a>VM 'Ler için Azure oto yönetimi hakkında sık sorulan sorular

Bu makalede, [sanal makineler Için Azure oto yönetimi](automanage-virtual-machines.md)hakkında en yaygın soruların yanıtları sağlanmaktadır.

Azure sorununuz bu makalede giderilmemişse, [MSDN ve Stack Overflow](https://azure.microsoft.com/support/forums/)Azure forumlarını ziyaret edin. Sorununuzu bu forumlara gönderebilir veya [ @AzureSupport Twitter 'da](https://twitter.com/AzureSupport)ilan edebilirsiniz. Ayrıca, bir Azure destek isteği de gönderebilirsiniz. Destek isteği göndermek için [Azure Desteği sayfasında](https://azure.microsoft.com/support/options/) **Destek Al**' ı seçin.


## <a name="azure-automanage-for-virtual-machines"></a>Sanal makineler için Azure oto yönetimi

**Azure oto yönetimi 'ni etkinleştirmek için gereken tüm Önkoşullar nelerdir?**

Aşağıda, Azure oto yönetimi 'ni etkinleştirme önkoşulları verilmiştir:
- Yalnızca Windows Server VM 'Leri
- VM 'Ler çalışıyor olmalıdır
- VM 'Ler desteklenen bir bölgede olmalıdır
- Kullanıcının doğru izinleri olması gerekir
- Yalnızca ölçek olmayan küme VM 'Leri
- VM 'Ler farklı bir abonelikte bulunan bir Log Analytics çalışma alanına bağlanmalıdır

**Oto yönetimi etkinleştirmek için hangi RBAC izni gereklidir?**

Mevcut bir oto Yönet hesabı olan bir VM 'de, oto Yönet 'i etkinleştirirseniz, VM 'nin bulunduğu kaynak grubuna katkıda bulunan rolü gerekir. 

Etkinleştirme sırasında yeni bir bir bir bir bir bir bir bir bir oto Yönet hesabı kullanıyorsanız, kullanıcıların aboneliğe sahip olması veya katkıda bulunan + Kullanıcı erişimi yönetici rolü olması gerekir.


**Hangi bölgeler destekleniyor?**

Şu bölgelerdeki sanal makineler desteklenir: Batı Avrupa, Doğu ABD, Batı ABD 2, Kanada Orta, Orta Batı ABD.


**Azure otomatik yönetimi hangi özellikleri otomatikleştirin?**

Oto yönetimi, [burada](virtual-machines-best-practices.md)LISTELENEN hizmetlerin VM yaşam döngüsü boyunca kaydeder, yapılandırır ve izler.


**Azure oto yönetimi 'nde konfigürasyonları özelleştirebilir miyim?**

Müşteriler, Yapılandırma tercihleri aracılığıyla Azure Backup bekletme gibi belirli hizmetlere yönelik ayarları özelleştirebilir. Değiştirilebilen ayarların tam listesi için belgelerimize [bakın.](virtual-machines-best-practices.md)


**Azure, hem Linux hem de Windows VM 'leriyle çalışır mı?**

Şu anda, oto yönetimi Windows Server Azure VM 'lerini destekler.


**Yalnızca bir VM kümesi üzerinde seçmeli bir şekilde yedekleme uygulayabilir miyim?**

Seçilen yeni ve mevcut VM 'Lerde tıklama ve işaret basitliği ile, oto yönetimi etkinleştirilebilir. Aynı zamanda, oto yönetimi de devre dışı bırakılabilir.


**Azure, sanal makine ölçek kümesindeki VM 'Leri destekler mi?**

Hayır, Azure oto yönetimi şu anda bir sanal makine ölçek kümesindeki VM 'Leri desteklemiyor.


**Azure 'un maliyeti ne kadar sürer?**

Azure oto yönetimi, genel önizleme aşamasında ek ücret ödemeden kullanılabilir. Azure Backup gibi bağlı Azure kaynakları maliyet doğuracaktır.


**Azure ilkesi aracılığıyla bir oto Yönet uygulayabilir miyim?**

Evet, otomatik olarak tanımlanan kapsamdaki tüm VM 'lere otomatik Yönet uygulayacak yerleşik bir ilkemiz var. Ayrıca, yapılandırma profilini (DevTest veya üretim), sizin de oto Yönet hesabınızla birlikte belirtmeniz gerekir. [Burada](virtual-machines-policy-enable.md)Azure Ilkesi aracılığıyla oto yönetimi etkinleştirme hakkında daha fazla bilgi edinin.


**Bir oto Yönet hesabı nedir?**

Otomatikmanage hesabı, güvenlik bağlamını sağlayan bir MSI (Yönetilen Hizmet Kimliği) veya otomatik işlemlerin gerçekleştirileceği kimliği sağlar.


**Oto yönetimi etkinleştirilirken, seçili VM 'Lerin yanı sıra ek VM 'Leri etkilerler.**

VM 'niz mevcut bir Log Analytics çalışma alanına bağlıysa, bu çözümleri uygulamak için bu çalışma alanını yeniden kullanacağız: Değişiklik İzleme, envanter ve Güncelleştirme Yönetimi. Bu çalışma alanına bağlı tüm VM 'Ler, bu çözümlerin etkinleştirilmesini sağlar. 


**VM 'nin yapılandırma profilini değiştirebilir miyim?**

Şu anda, bu VM için, bu sanal makine için yeniden Yönet 'i devre dışı bırakmanız ve ardından, istenen yapılandırma profili ve tercihleriyle, tekrar yönetmeyi yeniden etkinleştirmeniz gerekir.


**VM 'im, Güncelleştirme Yönetimi gibi bir hizmet için zaten yapılandırılmışsa otomatik yönetilecek yeniden yapılandırılacak mi?**
Hayır, tekrar Yönet bunu yeniden yapılandırmayacak. Bu hizmetle ilişkili kaynakları DRFT için izlemeye başlayacağız.


**Neden sanal makinelerimin, oto Yönet portalında başarısız bir durumu var?**

Durumu *başarısız*olarak GÖRÜRSENIZ, sanal makinenizin bulunduğu kaynak grubu aracılığıyla dağıtımda sorun giderebilirsiniz. **Kaynak grupları**' na gidin, kaynak grubunuzu seçin, **dağıtımlar** ' a tıklayın ve hata ayrıntılarıyla birlikte *başarısız* durumuna bakın.

**Oto yönetimi için nasıl sorun giderme desteği alabilirim?**

[Teknik destek talebi bileti](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)oluşturabilirsiniz. **Hizmet** seçeneği için *izleme ve yönetim* bölümünde, ara ' yı arayın *ve seçin.*


## <a name="next-steps"></a>Sonraki adımlar

Azure portal sanal makineler için oto yönetimini etkinleştirmeyi deneyin.

> [!div class="nextstepaction"]
> [Azure portal sanal makineler için oto yönetimini etkinleştir](quick-create-virtual-machines-portal.md)