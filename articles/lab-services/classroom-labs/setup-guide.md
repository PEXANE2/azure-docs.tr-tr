---
title: Azure Lab Hizmetleri için hızlandırılmış sınıf laboratuvarı kurulum kılavuzu
description: Bu kılavuz, laboratuvar oluşturucularının okulları içinde kullanılmak üzere hızlı bir şekilde bir laboratuvar hesabı kurmalarına yardımcı olur.
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
ms.openlocfilehash: e00b6dd5af1cb489aee9e8b4c9f3337eb02e4b14
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878111"
---
# <a name="classroom-lab-setup-guide"></a>Sınıf laboratuvar kurulum kılavuzu

Öğrencilerinize bir laboratuvar yayınlama işlemi, laboratuarınızda oluşturulacak sanal makine (VM) sayısına bağlı olarak birkaç saat kadar sürebilir. Düzgün çalıştığından emin olmak ve öğrencilerin VM'lerini yayınlamak için yeterli zaman tanımak için bir laboratuvar kurmak için en az bir gün bekleyin.

## <a name="understand-the-lab-requirements-of-your-class"></a>Sınıfınızın laboratuvar gereksinimlerini anlama

Yeni bir laboratuvar kurmadan önce aşağıdaki soruları göz önünde bulundurmalısınız.

### <a name="what-software-requirements-does-the-class-have"></a>Sınıfın hangi yazılım gereksinimleri vardır?

Sınıfınızın öğrenme hedeflerine bağlı olarak, laboratuvarın VM'lerine hangi işletim sisteminin, uygulamaların ve araçların yüklenmesi gerektiğine karar verin. Laboratuvar VM'lerini ayarlamak için üç seçeneğiniz vardır:

- **Azure Marketi resmi kullanın**: Azure Marketi, laboratuvar oluştururken kullanabileceğiniz yüzlerce görüntü sağlar. Bazı sınıflar için, bu resimlerden biri zaten sınıf için gereken her şeyi içerebilir.

- **Yeni bir özel resim oluşturma**: Başlangıç noktası olarak Azure Marketi görüntüsünü kullanarak ve ek yazılım yükleyerek ve yapılandırma değişiklikleri yaparak özelleştirerek kendi özel resminizi oluşturabilirsiniz.

- **Varolan özel bir görüntü kullanın**: Daha önce oluşturduğunuz veya okulunuzdaki diğer yöneticiler veya öğretim üyeleri tarafından oluşturulan varolan özel görüntüleri yeniden kullanabilirsiniz. Bu, yöneticilerinizin özel görüntüleri kaydetmek için bir depo olan paylaşılan bir resim galerisini yapılandırmasını gerektirir.

> [!NOTE]
> Yöneticileriniz Azure Marketi görüntülerini ve özel görüntüleri kullanabilmeniz için etkinleştirmekten sorumludur. İhtiyacınız olan görüntülerin etkin olduğundan emin olmak için BT departmanınızla işbirliği yapın. Oluşturduğunuz özel görüntüler, sahip olduğunuz laboratuvarlarda kullanılmak üzere otomatik olarak etkinleştirilir.

### <a name="what-hardware-requirements-does-the-class-have"></a>Sınıfın hangi donanım gereksinimleri var?

Aralarından seçim yapabileceğiniz çeşitli işlem boyutları vardır:

- İç içe geçme boyutları, böylece öğrencilere birden çok iç içe, iç içe vm barındırma yeteneğine sahip bir VM erişim verebilirsiniz. Örneğin, ağ kursları için bu işlem boyutunu kullanabilirsiniz.

- GPU boyutları, böylece öğrencilerinizin bilgisayar yoğun uygulama türlerini kullanabilirsiniz. Örneğin, bu seçim yapay zeka ve makine öğrenimi için uygun olabilir.

Kullanılabilir işlem boyutlarının tam listesini görmek için [VM boyutlandırma](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#vm-sizing) kılavuzuna bakın.

> [!NOTE]
> Laboratuvarınızın bölgesine bağlı olarak, bu bölgeye göre değiştiğinden, kullanılabilir daha az işlem boyutu görebilirsiniz. Genellikle, gereksinimlerinize en yakın en küçük işlem boyutunu seçmeniz gerekir. Azure Laboratuvar Hizmetleri ile gerekirse daha sonra farklı bir işlem kapasitesine sahip yeni bir laboratuvar kurabilirsiniz.

### <a name="what-dependencies-does-the-class-have-on-external-azure-or-network-resources"></a>Sınıfın dış Azure veya ağ kaynaklarına ne gibi bağımlılıkları var?

Laboratuvar VM'lerinizin veritabanı, dosya paylaşımı veya lisans sunucusu gibi dış kaynakları kullanması gerekiyorsa, laboratuvarınızın bu kaynaklara erişebilmesini sağlamak için yöneticilerinizle işbirliği yapmak.

Sanal ağ tarafından güvenli *hale alınmayan* Azure kaynaklarına erişmek için yöneticileriniz tarafından ek yapılandırma aramanız gerekmez. Bu kaynaklara genel internet üzerinden erişebilirsiniz.

> [!NOTE]
> Kaynağı doğrudan VM'ye sağlayarak laboratuvarınızın dış kaynaklara olan bağımlılıklarını azaltıp azaltamayacağınızı düşünmelisiniz. Örneğin, harici bir veritabanından veri okuma gereksinimini ortadan kaldırmak için veritabanını doğrudan VM'ye yükleyebilirsiniz.  

### <a name="how-will-costs-be-controlled"></a>Maliyetler nasıl kontrol edilecek?

Laboratuvar Hizmetleri, yalnızca bir laboratuvar VM'sinin çalıştırDığı süre için ödeme yaptığınız anlamına gelen, istediğiniz kadar öde fiyatlandırma modeli kullanır. Maliyetleri denetlemek için, genellikle birbiriyle birlikte kullanılan üç seçeneğiniz vardır:

- **Zamanlama**: Zamanlama, laboratuvarlarınızın VM'lerinin başlatılıp kapatıldığında otomatik olarak kontrol etmenizi sağlar.
- **Kontenjan**: Kontenjan, öğrencilerin planlanan saatler dışında VM'ye erişebilmek için erişeedecekleri saat sayısını kontrol eder. Öğrenci kullanırken kontenjana ulaşılırsa, VM otomatik olarak kapatılır. Kontenjan artırılmadığı sürece öğrenci VM'yi yeniden başlatamaz.
- **Otomatik kapatma**: Etkinleştirildiğinde, otomatik kapatma ayarı, bir öğrenciuzak masaüstü protokolü (RDP) oturumundan bağlantı kesildikten sonra Windows VM'lerinin belirli bir süre sonra otomatik olarak kapanmasına neden olur. Bu ayar varsayılan olarak devre dışıdır.  

    > [!NOTE]
    > Bu ayar şu anda yalnızca Windows için vardır.

### <a name="how-will-students-save-their-work"></a>Öğrenciler işlerini nasıl kurtaracaklar?

Öğrencilerin her biri, laboratuvarın ömrü boyunca kendilerine atanan kendi VM'lerine atanır. Onlar seçebilirsiniz:

- Doğrudan VM'ye kaydedin.
- OneDrive veya GitHub gibi harici bir konuma kaydedin.

OneDrive'ı laboratuvar VM'lerinde öğrenciler için otomatik olarak yapılandırmak mümkündür.

> [!NOTE]
> Öğrencilerinizin kayıtlı işlerine laboratuvar dışında ve ders sona erdikten sonra erişimlerini sürdürmelerini sağlamak için, öğrencilerin çalışmalarını harici bir depoya kaydetmelerini öneririz.

### <a name="how-will-students-connect-to-their-vm"></a>Öğrenciler VM'lerine nasıl bağlanacak?

RDP'den Windows VM'lere, öğrencilerin [Microsoft Uzak Masaüstü istemcisini](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)kullanmalarını öneririz. Uzak Masaüstü istemcisi Mac'leri, Chromebook'ları ve Windows'u destekler.

Linux VM'leri için öğrenciler SSH veya RDP kullanabilirler. Öğrencilerin RDP kullanarak bağlanabilmesi için, gerekli RDP ve GUI paketlerini yüklemeniz ve yapılandırmanız gerekir.

## <a name="set-up-your-lab"></a>Laboratuvarınızı kurma

Sınıfınızın laboratuarı nın gereksinimlerini anladıktan sonra, bunu ayarlamaya hazırsınız. Laboratuvarınızı nasıl kurup kurup kurup kurup kurup kurup göreceğinizi görmek için bu bölümdeki bağlantıları izleyin.

1. **Bir laboratuvar oluşturun.** Talimatlar için [bir sınıf laboratuvarı oluşturma](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#create-a-classroom-lab) yla ilgili öğreticiye bakın.

    > [!NOTE]
    > Sınıfınız iç içe sanallaştırma gerektiriyorsa, [iç içe geçmeyi etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-nested-virtualization-template-vm)adımlarına bakın.

1. **Görüntüleri özelleştirin ve laboratuvar VM'lerini yayımlayın.** Şablon VM adı verilen özel bir VM'ye bağlanın. Aşağıdaki kılavuzlarda aşağıdaki adımlara bakın:
    - [Bir şablon oluşturma ve yönetme VM](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#publish-the-template-vm)
    - [Paylaşılan görüntü galerisi kullanma](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-shared-image-gallery)

    > [!NOTE]
    > Windows kullanıyorsanız, [Windows şablonu VM hazırlarken](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-prepare-windows-template)yönergeleri de görmeniz gerekir. Bu yönergeler, öğrencilerinizin kullanması için OneDrive ve Office'i ayarlama adımlarını içerir.

1. **VM havuzuve kapasitesini yönetin.** Sınıfınızın gerektirdiği şekilde VM kapasitesini kolayca ölçeklendirebilir veya küçültebilirsiniz. Bu yeni VM'ler ayarlamayı içerdiğinden, VM kapasitesini artırmanın birkaç saat sürebileceğini unutmayın. [VM havuzu kurma ve yönetme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-set-virtual-machine-passwords)adımlarını görün.

1. **Laboratuvar kullanıcılarını ekleyin ve yönetin.** Laboratuvarınıza kullanıcı eklemek için aşağıdaki öğreticilerdeki adımlara bakın:
   - [Kullanıcıları laboratuvara ekleme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#add-users-to-the-lab)
   - [Kullanıcılara davetiye gönderme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#send-invitation-emails-to-users)

    Öğrencilerin kullanabileceği hesap türleri hakkında bilgi için [Öğrenci hesaplarına](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#student-accounts)bakın.
  
1. **Maliyet denetimlerini ayarlayın.** Laboratuvarınızın maliyetlerini denetlemek için zamanlamaları, kotaları ve otomatik kapatmayı ayarlayın. Aşağıdaki öğreticilere bakın:

   - [Zamanlama ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)
        > [!NOTE]
        > Yüklediğiniz işletim sisteminin türüne bağlı olarak, VM'nin başlatılması birkaç dakika sürebilir. Bir laboratuvar VM'inin planlanan saatlerinizde kullanıma hazır olduğundan emin olmak için VM'leri 30 dakika önceden başlatmanızı öneririz.

   - [Kullanıcılar için kota lar ayarlayın](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users) ve belirli bir kullanıcı için ek kota [ayarlayın](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-additional-quotas-for-specific-users)
  
   - [Bağlantı kesildiğinde otomatik kapatmayı etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

        > [!NOTE]
        > Zamanlamalar, kotalar ve otomatik kapatma şablonU VM için geçerli değildir. Sonuç olarak, kullanılmadığında VM şablonlarını kapattığınızdan emin olmalısınız. Aksi takdirde, maliyete maruz devam ediyor. Ayrıca, varsayılan olarak, bir laboratuvar oluşturduğunuzda, vm şablonu otomatik olarak başlatılır. Laboratuarı ayarlamayı hemen bitirdiğinize ve VM şablonu'nu kapattığınızdan emin olun.

1. **Panoyu kullan.** Talimatlar için, [laboratuvarın panosunu kullanarak](https://docs.microsoft.com/azure/lab-services/classroom-labs/use-dashboard)bakın.

    > [!NOTE]
    > Panoda gösterilen tahmini maliyet, öğrencilerin laboratuvar kullanımı için bekleyebileceğiniz maksimum maliyettir. Örneğin, öğrencileriniz *not* tarafından kullanılmayan kota saatleri için ücretlendirilmez. Tahmini maliyetler, VM şablonu veya paylaşılan resim galerisini kullanmak için herhangi bir ücret *yansıtmaz.*

## <a name="next-steps"></a>Sonraki adımlar

- [Sınıf laboratuvarının kullanımını izleme](tutorial-track-usage.md)
  
- [Bir sınıf laboratuvarına erişim](tutorial-connect-virtual-machine-classroom-lab.md)