---
title: Azure Lab Services için hızlandırılmış laboratuvar Kurulum Kılavuzu
description: Bu kılavuz, laboratuar oluşturucularının okulda kullanım için hızlı bir şekilde laboratuvar hesabı oluşturmasına yardımcı olur.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: f7423a76fd3ceb238c8c5c1a4ea794ff83b28b4a
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94491673"
---
# <a name="lab-setup-guide"></a>Laboratuvar Kurulum Kılavuzu

Öğrencilerinize laboratuvar yayımlamaya yönelik işlem birkaç saate kadar sürebilir.  Süre miktarı, laboratuvarınızda oluşturulacak sanal makine (VM) sayısına bağlıdır. Bir laboratuvarın düzgün çalıştığından ve öğrencilerin VM 'Leri yayımlaması için yeterli zamana izin vermek için en az bir güne izin verin.

## <a name="understand-the-lab-requirements-of-your-class"></a>Sınıfınızın laboratuvar gereksinimlerini anlayın

Yeni bir laboratuvar ayarlamadan önce aşağıdaki soruları göz önünde bulundurmanız gerekir.

### <a name="what-software-requirements-does-the-class-have"></a>Sınıfta hangi yazılım gereksinimleri vardır?

Sınıfınızın öğrenme hedeflerine göre, laboratuvarın VM 'lerine hangi işletim sistemi, uygulama ve araçların yüklenmesi gerektiğine karar verin. Laboratuvar VM 'Leri ayarlamak için üç seçeneğiniz vardır:

- **Azure Marketi görüntüsünü kullanma** : Azure Marketi, bir laboratuvar oluştururken kullanabileceğiniz yüzlerce görüntü sağlar. Bazı sınıflarda, bu görüntülerden biri sınıfınız için ihtiyaç duyduğunuz her şeyi zaten içeriyor olabilir.

- **Yeni bir özel görüntü oluşturma** : bir Azure Marketi görüntüsünü başlangıç noktası olarak kullanarak kendi özel görüntünüzü oluşturabilir ve ek yazılım yükleyerek ve yapılandırma değişiklikleri yaparak özelleştirebilirsiniz.

- **Mevcut bir özel görüntü kullan** : daha önce oluşturduğunuz veya okulunuzdaki diğer yöneticiler veya fakülte oluşturulmuş özel görüntüleri yeniden kullanabilirsiniz. Özel görüntüleri kullanmak için, yöneticileriniz paylaşılan bir görüntü Galerisi ayarlaması gerekir.  Paylaşılan görüntü Galerisi, özel görüntüleri kaydetmek için kullanılan bir depodur.

> [!NOTE]
> Yöneticileriniz, Azure Marketi görüntülerini ve özel görüntüleri, bunları kullanabilmeniz için etkinleştirmekten sorumludur. İhtiyacınız olan görüntülerin etkinleştirildiğinden emin olmak için BT departmanınızla koordine edin. Oluşturduğunuz özel görüntüler, sahip olduğunuz laboratuvarlarda kullanılmak üzere otomatik olarak etkinleştirilir.

### <a name="what-hardware-requirements-does-the-class-have"></a>Sınıfta hangi donanım gereksinimleri vardır?

Aralarından seçim yapabileceğiniz farklı işlem boyutları vardır:

- Çoklu, iç içe geçmiş VM 'Leri barındırabilmesi için öğrencilerle erişim verebileceğiniz iç içe sanallaştırma boyutları. Örneğin, bu işlem boyutunu ağ veya ahlak olabilecek sınıflar için kullanabilirsiniz.

- GPU boyutları, öğrencilerinizin bilgisayar açısından yoğun uygulamalar türlerini kullanabilmesi için. Örneğin, bu seçenek genellikle yapay zeka ve makine öğrenimi ile kullanılır.

Uygun VM boyutunu seçme kılavuzu için aşağıdaki makaleleri okuyun:
- [VM boyutlandırma](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#vm-sizing)
- [Fiziksel laboratuvardan Azure Lab Services taşıma](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931)

> [!NOTE]
> Laboratuvarınızın bölgesine bağlı olarak, bu, bölgeye göre farklılık gösterdiği için daha az işlem boyutu görebilirsiniz. Genellikle, gereksinimlerinize en yakın en küçük işlem boyutunu seçmeniz gerekir. Azure Lab Services ile, daha sonra gerekirse, farklı bir işlem kapasitesine sahip yeni bir laboratuvar ayarlayabilirsiniz.

### <a name="what-dependencies-does-the-class-have-on-external-azure-or-network-resources"></a>Sınıfın dış Azure veya ağ kaynaklarında hangi bağımlılıkları vardır?
Laboratuvar sanal makinelerinizin, bir veritabanına, dosya paylaşımıyla veya lisanslama sunucusuna erişim gibi dış kaynaklara erişmesi gerekebilir.  Laboratuvar sanal makinelerinizin dış kaynakları kullanmasına izin vermek için BT yöneticilerinizle koordine edin.

> [!NOTE]
> Kaynağı doğrudan VM üzerinde sağlayarak laboratuvarınızın dış kaynaklara bağımlılıklarını azaltacağınızı düşünmeniz gerekir. Örneğin, bir dış veritabanından veri okuma gereksinimini ortadan kaldırmak için veritabanını doğrudan VM 'ye yükleyebilirsiniz.  

### <a name="how-will-costs-be-controlled"></a>Maliyetler nasıl kontrol edilir?
Laboratuvar Hizmetleri bir Kullandıkça Öde fiyatlandırma modeli kullanır. Bu, yalnızca bir laboratuvar VM 'sinin çalıştığı zaman için ödeme yaptığınız anlamına gelir. Maliyetleri denetlemek için genellikle birlikte kullanılan üç seçeneğiniz vardır:

- **Zamanlama** : bir zamanlama, laboratuvarlarınızın VM 'lerinin ne zaman başlatıldığını ve kapatıldığını otomatik olarak denetlemenize olanak tanır.
- **Kota** : kota, öğrencilerin zamanlanan saatler dışında bir VM 'ye erişiminin olacağı saat sayısını denetler.  Bir öğrenci VM 'sini kullanırken ve kotasına ulaşıldığında, VM otomatik olarak kapanır.  Kota arttırılmadığı takdirde öğrenci sanal makineyi yeniden başlatabilir.
- **Otomatik kapatma** : etkinleştirildiğinde, otomatik kapatma ayarı, bir öğrencinin Uzak Masaüstü Protokolü (RDP) oturumundan bağlantısı kesildikten sonra Windows VM 'lerinin otomatik olarak kapatılmasını sağlar. Bu ayar varsayılan olarak devre dışıdır.

Daha fazla bilgi için aşağıdaki makaleleri okuyun:
- [Maliyetleri tahmin etme](https://docs.microsoft.com/azure/lab-services/cost-management-guide#estimate-the-lab-costs)
- [Maliyetleri yönetme](https://docs.microsoft.com/azure/lab-services/cost-management-guide#manage-costs)

### <a name="how-will-students-save-their-work"></a>Öğrenciler işlerini nasıl kaydeder?
Öğrencilerin her biri kendi VM 'sine atanır ve bu, laboratuvarın ömrü boyunca kendilerine atanır. Şunları seçebilecekleri:

- Doğrudan VM 'ye kaydedin.
- OneDrive veya GitHub gibi bir dış konuma kaydedin.

OneDrive 'ı laboratuvar VM 'lerinde öğrenciler için otomatik olarak yapılandırmak mümkündür.

> [!NOTE]
> Öğrencilerinizin, kendi kayıtlı işlerine laboratuvar dışında devam ettiğinden emin olmak için ve sınıfı bittikten sonra, öğrencilerin çalışmalarını bir dış depoya kaydetmelerini öneririz.

### <a name="how-will-students-connect-to-their-vm"></a>Öğrenciler, VM 'lerine nasıl bağlanır?
Windows VM 'lerine RDP için, öğrencilerin [Microsoft Uzak masaüstü istemcisini](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)kullanmasını öneririz. Uzak Masaüstü istemcisi, Mac, Kmebook ve Windows 'u destekler.

Linux VM 'Ler için, öğrenciler SSH veya RDP kullanabilir. Öğrencilerin RDP kullanarak bağlanmasını sağlamak için gerekli RDP ve GUI paketlerini yükleyip yapılandırmanız gerekir.

### <a name="will-students-also-be-using-microsoft-teams"></a>Öğrenciler Microsoft ekipleri de kullanıyor mu?
Azure Lab Services, Microsoft ekipleri ile tümleşir, böylece fakülte laboratuvarları takımlar içinde oluşturabilir ve yönetebilir.  Benzer şekilde, öğrenciler takımlar dahilinde laboratuvara erişebilir.

Daha fazla bilgi için aşağıdaki makaleyi inceleyin:
- [Microsoft ekipleri dahilinde Azure Lab Services](https://docs.microsoft.com/azure/lab-services/lab-services-within-teams-overview)

## <a name="set-up-your-lab"></a>Laboratuvarınızı kurma

Sınıfınızın laboratuvarınızın gereksinimlerini anladıktan sonra, ayarlamaya hazırsınız demektir. Laboratuvarınızı ayarlamayı öğrenmek için bu bölümdeki bağlantıları izleyin.  Ekipler içinde laboratuvarları kullanıyorsanız, farklı adımların sağlandığını unutmayın.

1. **Laboratuvar oluşturun.** Laboratuvar oluşturma hakkında öğreticilere bakın:
    - Yönergeler için [bir derslik Laboratuvarı oluşturun](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#create-a-classroom-lab) .
    - [Teams'den laboratuvar oluşturma](https://docs.microsoft.com/azure/lab-services/how-to-get-started-create-lab-within-teams)

    > [!NOTE]
    > Sınıfınız iç içe sanallaştırma gerektiriyorsa, [iç içe sanallaştırmayı etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-nested-virtualization-template-vm)içindeki adımlara bakın.

1. **Görüntüleri özelleştirin ve laboratuvar VM 'Leri yayımlayın.** Şablon VM adlı özel bir VM 'ye bağlanın. Aşağıdaki kılavuzlarda bulunan adımlara bakın:
    - [Şablon VM 'si oluşturma ve yönetme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#publish-the-template-vm)
    - [Paylaşılan görüntü galerisi kullanma](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-shared-image-gallery)

    > [!NOTE]
    > Windows kullanıyorsanız, [bir Windows şablon sanal makinesi hazırlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-prepare-windows-template)bölümündeki yönergeleri de görmeniz gerekir. Bu yönergeler, öğrencilerinizin kullanması için OneDrive ve Office ayarlama adımlarını içerir.

1. **VM havuzunu ve kapasitesini yönetin.** Sınıfınızın gerektirdiği şekilde, sanal makine kapasitesini kolayca ölçeklendirebilir veya azaltabilirsiniz. Yeni VM 'Lerin ayarlanmakta olması nedeniyle VM kapasitesini artırmanın birkaç saat sürebileceğini aklınızda bulundurun. Aşağıdaki makalelerde bulunan adımlara bakın:
    - [Bir VM havuzunu ayarlama ve yönetme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-set-virtual-machine-passwords)
    - [Ekiplerden laboratuvar hizmetlerinde bir VM havuzunu yönetme](https://docs.microsoft.com/azure/lab-services/how-to-manage-vm-pool-within-teams)

1. **Laboratuvar kullanıcıları ekleyin ve yönetin.** Laboratuvarınızı Kullanıcı eklemek için aşağıdaki öğreticilerde yer alarak bulunan adımlara bakın:
   - [Laboratuvara Kullanıcı ekleme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#add-users-to-the-lab)
   - [Kullanıcılara davet gönder](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#send-invitation-emails-to-users)
   - [Ekiplerden Laboratuvar Hizmetleri Kullanıcı listelerini yönetme](https://docs.microsoft.com/azure/lab-services/how-to-manage-user-lists-within-teams)

    Öğrencilerin kullanabileceği hesap türleri hakkında bilgi için bkz. [öğrenci hesapları](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#student-accounts).
  
1. **Maliyet denetimlerini ayarlayın.** Laboratuvarınızın maliyetlerini denetlemek için zamanlamalar, Kotalar ve oto kapatma ayarlayın. Aşağıdaki öğreticilere bakın:

   - [Zamanlama ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)

        > [!NOTE]
        > Yüklediğiniz işletim sisteminin türüne bağlı olarak, bir VM 'nin başlatılması birkaç dakika sürebilir. Bir laboratuvar VM 'sinin zamanlanan saatleriniz sırasında kullanıma hazırlandığından emin olmak için, VM 'Leri 30 dakika önce başlatmanız önerilir.

   - [Kullanıcılar için kotalar ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users) ve [belirli bir kullanıcı için ek kota ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-additional-quotas-for-specific-users)
  
   - [Bağlantı kesildiğinde otomatik kapatmayı etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

        > [!NOTE]
        > Zamanlamalar ve kotalar, şablon VM 'ye uygulanmaz, ancak otomatik kapatılma ayarları uygulanır. 
        > 
        > Laboratuvar oluşturduğunuzda, şablon VM oluşturulur ancak başlatılmaz. Bunu başlatabilir, ona bağlanabilir ve laboratuvar için önkoşul olan herhangi bir yazılımı yükleyebilir ve sonra yayımlayabilirsiniz. Şablon VM 'yi yayımladığınızda, bunu yapmadıysanız, sizin için otomatik olarak kapatılır. 
        > 
        > Şablon VM 'Leri çalışırken **Maliyet** doğurur, bu nedenle çalışıyor olması GEREKMIYORSA şablon VM 'nin kapatıldığından emin olun.

    - [Takımlar içinde Laboratuvar Hizmetleri zamanlamaları oluşturma ve yönetme](https://docs.microsoft.com/azure/lab-services/how-to-create-schedules-within-teams) 

1. **Panoyu kullanın.** Yönergeler için bkz. [Laboratuvar panosunu kullanma](https://docs.microsoft.com/azure/lab-services/classroom-labs/use-dashboard).

    > [!NOTE]
    > Panoda gösterilen tahmini maliyet, laboratuvarın öğrencilerinin kullanımı için tahmin edebileceğiniz maksimum maliyettir. Örneğin, öğrencileriniz tarafından *not* kullanılmayan kota saatleri için ücretlendirilmeyecektir. Tahmini maliyetler, şablon VM 'si, paylaşılan görüntü Galerisi veya laboratuvar Oluşturucu bir Kullanıcı makinesini başlattığında herhangi bir ücret *yansıtmazlar* .

## <a name="next-steps"></a>Sonraki adımlar

- [Sınıf laboratuvarının kullanımını izleme](tutorial-track-usage.md)
  
- [Bir sınıf laboratuvarına erişim](tutorial-connect-virtual-machine-classroom-lab.md)
