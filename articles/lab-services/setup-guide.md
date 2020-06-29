---
title: Azure Lab Services için hızlandırılmış derslik Laboratuvarı Kurulum Kılavuzu
description: Bu kılavuz, laboratuar oluşturucularının okulda kullanım için hızlı bir şekilde laboratuvar hesabı oluşturmasına yardımcı olur.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: ad1a679c556316c2d23a713ffa5ac5dfe86cce0e
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85445585"
---
# <a name="classroom-lab-setup-guide"></a>Sınıf Laboratuvarı Kurulum Kılavuzu

Öğrencilerinize laboratuvar yayımlama süreci, laboratuvarınızda oluşturulacak sanal makine (VM) sayısına bağlı olarak birkaç saate kadar sürebilir. Doğru çalıştığından ve öğrencilerin sanal makinelerini yayımlaması için yeterli zamana izin verildiğinden emin olmak için bir laboratuvarın en az bir güne ayarlamaya izin verin.

## <a name="understand-the-lab-requirements-of-your-class"></a>Sınıfınızın laboratuvar gereksinimlerini anlayın

Yeni bir laboratuvar ayarlamadan önce aşağıdaki soruları göz önünde bulundurmanız gerekir.

### <a name="what-software-requirements-does-the-class-have"></a>Sınıfta hangi yazılım gereksinimleri vardır?

Sınıfınızın öğrenme hedeflerine göre, laboratuvarın VM 'lerine hangi işletim sistemi, uygulama ve araçların yüklenmesi gerektiğine karar verin. Laboratuvar VM 'Leri ayarlamak için üç seçeneğiniz vardır:

- **Azure Marketi görüntüsünü kullanma**: Azure Marketi, bir laboratuvar oluştururken kullanabileceğiniz yüzlerce görüntü sağlar. Bazı sınıflarda, bu görüntülerden biri sınıfınız için ihtiyaç duyduğunuz her şeyi zaten içeriyor olabilir.

- **Yeni bir özel görüntü oluşturma**: bir Azure Marketi görüntüsünü başlangıç noktası olarak kullanarak kendi özel görüntünüzü oluşturabilir ve ek yazılım yükleyerek ve yapılandırma değişiklikleri yaparak özelleştirebilirsiniz.

- **Mevcut bir özel görüntü kullan**: daha önce oluşturduğunuz veya okulunuzdaki diğer yöneticiler veya fakülte oluşturulmuş özel görüntüleri yeniden kullanabilirsiniz. Bu, yöneticilerin özel görüntüleri kaydetmek için bir depo olan paylaşılan bir görüntü Galerisi 'ni yapılandırmasını gerektirir.

> [!NOTE]
> Yöneticileriniz, Azure Marketi görüntülerini ve özel görüntüleri, bunları kullanabilmeniz için etkinleştirmekten sorumludur. İhtiyacınız olan görüntülerin etkinleştirildiğinden emin olmak için BT departmanınızla koordine edin. Oluşturduğunuz özel görüntüler, sahip olduğunuz laboratuvarlarda kullanılmak üzere otomatik olarak etkinleştirilir.

### <a name="what-hardware-requirements-does-the-class-have"></a>Sınıfta hangi donanım gereksinimleri vardır?

Aralarından seçim yapabileceğiniz çeşitli bilgi işlem boyutları vardır:

- Çoklu, iç içe geçmiş VM 'Leri barındırabilen bir VM 'ye erişim izni verebileceğiniz iç içe sanallaştırma boyutları. Örneğin, bu işlem boyutunu ağ kursları için kullanabilirsiniz.

- GPU boyutları, öğrencilerinizin bilgisayar açısından yoğun uygulamalar türlerini kullanabilmesi için. Örneğin, bu seçim yapay zeka ve makine öğrenimi için uygun olabilir.

Kullanılabilir işlem boyutlarının tüm listesini görmek için [VM boyutlandırmalarındaki](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#vm-sizing) kılavuza bakın.

> [!NOTE]
> Laboratuvarınızın bölgesine bağlı olarak, bu, bölgeye göre farklılık gösterdiği için daha az işlem boyutu görebilirsiniz. Genellikle, gereksinimlerinize en yakın en küçük işlem boyutunu seçmeniz gerekir. Azure Lab Services ile, daha sonra gerekirse, farklı bir işlem kapasitesine sahip yeni bir laboratuvar ayarlayabilirsiniz.

### <a name="what-dependencies-does-the-class-have-on-external-azure-or-network-resources"></a>Sınıfın dış Azure veya ağ kaynaklarında hangi bağımlılıkları vardır?

Laboratuvar sanal makinelerinizin bir veritabanı, dosya paylaşma veya lisanslama sunucusu gibi dış kaynakları kullanması gerekirse, laboratuvarınızın bu kaynaklara erişebildiğinden emin olmak için yöneticilerinizle koordine edin.

Bir sanal ağ tarafından güvenliği *bulunmayan* Azure kaynaklarına erişim için yöneticileriniz tarafından ek yapılandırma yapmanız gerekmez. Bu kaynaklara genel İnternet üzerinden erişebilirsiniz.

> [!NOTE]
> Kaynağı doğrudan VM üzerinde sağlayarak laboratuvarınızın dış kaynaklara bağımlılıklarını azaltacağınızı düşünmeniz gerekir. Örneğin, bir dış veritabanından veri okuma gereksinimini ortadan kaldırmak için veritabanını doğrudan VM 'ye yükleyebilirsiniz.  

### <a name="how-will-costs-be-controlled"></a>Maliyetler nasıl kontrol edilir?

Laboratuvar Hizmetleri bir Kullandıkça Öde fiyatlandırma modeli kullanır. Bu, yalnızca bir laboratuvar VM 'sinin çalıştığı zaman için ödeme yaptığınız anlamına gelir. Maliyetleri denetlemek için, genellikle birbiriyle birlikte kullanılan üç seçeneğiniz vardır:

- **Zamanlama**: bir zamanlama, laboratuvarlarınızın VM 'lerinin ne zaman başlatıldığını ve kapatıldığını otomatik olarak denetlemenize olanak tanır.
- **Kota**: kota, öğrencilerin zamanlanan saatler dışında bir VM 'ye erişiminin olacağı saat sayısını denetler. Bir öğrenci bu dosyayı kullanırken kotaya ulaşıldığında, sanal makine otomatik olarak kapatılır. Kota arttırılmadığı takdirde öğrenci sanal makineyi yeniden başlatabilir.
- **Otomatik kapatma**: etkinleştirildiğinde, otomatik kapatma ayarı Windows VM 'lerinin belirli bir süre sonra otomatik olarak kapatılmasını sağlar ve bir öğrenci Uzak Masaüstü Protokolü (RDP) oturumuyla bağlantısını kesmiş olur. Bu ayar varsayılan olarak devre dışıdır.  

    > [!NOTE]
    > Bu ayar şu anda yalnızca Windows için var.

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

## <a name="set-up-your-lab"></a>Laboratuvarınızı kurma

Sınıfınızın laboratuvarınızın gereksinimlerini anladıktan sonra, ayarlamaya hazırsınız demektir. Laboratuvarınızı ayarlamayı öğrenmek için bu bölümdeki bağlantıları izleyin.

1. **Laboratuvar oluşturun.** Yönergeler için [bir derslik Laboratuvarı oluşturma](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#create-a-classroom-lab) öğreticisine bakın.

    > [!NOTE]
    > Sınıfınız iç içe sanallaştırma gerektiriyorsa, [iç içe sanallaştırmayı etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-nested-virtualization-template-vm)içindeki adımlara bakın.

1. **Görüntüleri özelleştirin ve laboratuvar VM 'Leri yayımlayın.** Şablon VM adlı özel bir VM 'ye bağlanın. Aşağıdaki kılavuzlarda bulunan adımlara bakın:
    - [Şablon VM 'si oluşturma ve yönetme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#publish-the-template-vm)
    - [Paylaşılan görüntü galerisi kullanma](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-shared-image-gallery)

    > [!NOTE]
    > Windows kullanıyorsanız, [bir Windows şablon sanal makinesi hazırlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-prepare-windows-template)bölümündeki yönergeleri de görmeniz gerekir. Bu yönergeler, öğrencilerinizin kullanması için OneDrive ve Office ayarlama adımlarını içerir.

1. **VM havuzunu ve kapasitesini yönetin.** Sınıfınızın gerektirdiği şekilde, sanal makine kapasitesini kolayca ölçeklendirebilir veya azaltabilirsiniz. VM kapasitesini artırmanın birkaç saat sürebileceğini aklınızda bulundurun, çünkü bu yeni VM 'Leri ayarlamayı içerir. [BIR VM havuzunu ayarlama ve yönetme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-set-virtual-machine-passwords)adımları bölümüne bakın.

1. **Laboratuvar kullanıcıları ekleyin ve yönetin.** Laboratuvarınızı Kullanıcı eklemek için aşağıdaki öğreticilerde yer alarak bulunan adımlara bakın:
   - [Laboratuvara Kullanıcı ekleme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#add-users-to-the-lab)
   - [Kullanıcılara davet gönder](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#send-invitation-emails-to-users)

    Öğrencilerin kullanabileceği hesap türleri hakkında bilgi için bkz. [öğrenci hesapları](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#student-accounts).
  
1. **Maliyet denetimlerini ayarlayın.** Laboratuvarınızın maliyetlerini denetlemek için zamanlamalar, Kotalar ve otomatik kapatmalar ayarlayın. Aşağıdaki öğreticilere bakın:

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


1. **Panoyu kullanın.** Yönergeler için bkz. [Laboratuvar panosunu kullanma](https://docs.microsoft.com/azure/lab-services/classroom-labs/use-dashboard).

    > [!NOTE]
    > Panoda gösterilen tahmini maliyet, laboratuvarın öğrencilerinin kullanımı için tahmin edebileceğiniz maksimum maliyettir. Örneğin, öğrencileriniz tarafından *not* kullanılmayan kota saatleri için ücretlendirilmeyecektir. Tahmini maliyetler, şablon VM 'si, paylaşılan görüntü Galerisi veya laboratuvar Oluşturucu bir Kullanıcı makinesini başlattığında herhangi bir ücret *yansıtmazlar* .

## <a name="next-steps"></a>Sonraki adımlar

- [Sınıf laboratuvarının kullanımını izleme](tutorial-track-usage.md)
  
- [Bir sınıf laboratuvarına erişim](tutorial-connect-virtual-machine-classroom-lab.md)