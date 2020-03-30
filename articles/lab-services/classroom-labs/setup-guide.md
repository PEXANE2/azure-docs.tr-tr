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
ms.openlocfilehash: d58e11551c2857c269e8985e81f84138f6d389ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80370819"
---
# <a name="classroom-lab-setup-guide"></a>Sınıf laboratuvar kurulum kılavuzu

Öğrencilerinize bir laboratuvar yayınlama işlemi, laboratuarınızda oluşturulacak sanal makine (VM) sayısına bağlı olarak birkaç saat kadar sürebilir.  Düzgün çalıştığından emin olmak ve öğrencilerin VM'lerini yayınlamak için yeterli zaman tanımak için bir laboratuvar kurmak için kendinize en az bir gün vermelisiniz.

## <a name="understand-your-classs-lab-requirements"></a>Sınıfınızın laboratuvar gereksinimlerini anlama

Yeni bir laboratuvar kurmadan önce aşağıdaki soruları göz önünde bulundurmanız gerekir:

**Sınıfın hangi yazılım gereksinimleri vardır?**

Sınıfınızın öğrenme hedeflerine bağlı olarak, hangi işletim sistemi, uygulamalar, araçlar, vb. laboratuarın VM'lerine yüklenmesi gerektiğine karar vermelisiniz.   Laboratuvar VM'lerini ayarlamak için üç seçeneğiniz vardır:

- **Azure Marketi görüntüsü kullanın** – Market, laboratuvar oluştururken kullanabileceğiniz yüzlerce görüntü sağlar.  Bazı sınıflar için, bir pazar görüntüsü zaten sınıf için gereken her şeyi içerebilir.

- **Yeni bir özel görüntü oluşturun** - Bir başlangıç noktası olarak bir pazar resmi kullanarak ve ek yazılım yükleyerek, yapılandırma değişiklikleri, vb yaparak özelleştirerek kendi özel görüntü oluşturabilirsiniz.

- **Varolan özel bir resim kullanın** - Daha önce oluşturduğunuz veya okulunuzdaki diğer yöneticiler veya öğretim üyeleri tarafından oluşturulan varolan özel görüntüleri yeniden kullanabilirsiniz; bu, yöneticilerinizin özel görüntüleri kaydetmek için bir depo olan Paylaşılan Resim Galerisi'ni yapılandırmasını gerektirir.

> [!NOTE]
> Yöneticileriniz, bunları kullanabilmeniz için Market ve özel görüntüleri etkinleştirmekten sorumludur; İhtiyacınız olan görüntülerin etkin olduğundan emin olmak için BT departmanınızla koordine olmanız gerekir.  Oluşturduğunuz özel görüntüler, sahip olduğunuz laboratuvarlarda kullanılmak üzere otomatik olarak etkinleştirilir.

**Sınıfın hangi donanım gereksinimleri var?**

Aralarından seçim yapabileceğiniz çeşitli işlem boyutları vardır:

- Öğrencilere birden fazla iç içe vm barındırabilen bir VM'ye erişim verebilmeniz için iç içe geçirilmiş sanallaştırma boyutları; örneğin, bu işlem boyutu genellikle Ağ kursları için kullanılır.

- GPU, öğrencilerinizin Yapay Zeka ve Makine Öğrenimi gibi bilgisayar yoğun uygulama türlerini kullanabilmeleri için boyutlara sahiptir.

Kullanılabilir işlem boyutlarının tam listesini görmek için [VM boyutlandırma](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#vm-sizing) kılavuzuna bakın.

> [!NOTE]
> Laboratuvarınızın oluşturulacağı bölgeye bağlı olarak, bölgeye göre değiştiğinden daha az işlem boyutu görebilirsiniz.  Genel tavsiyemiz, ihtiyaçlarınıza en yakın en küçük işlem boyutunu seçmektir.  Azure Laboratuvar Hizmetleri ile gerekirse daha sonra farklı bir işlem kapasitesine sahip yeni bir laboratuvar kurabilirsiniz.

**Sınıfın dış Azure veya ağ kaynaklarına ne gibi bağımlılıkları var?**

Laboratuvar VM'lerinizin veritabanı, dosya paylaşımı, lisans lama sunucusu vb. gibi harici kaynakları kullanması gerekiyorsa, laboratuvarınızın bu kaynaklara erişebilmesini sağlamak için yöneticilerinizle koordine etmeniz gerekir.

Sanal ağ tarafından güvenli *hale alınmayan* Azure kaynaklarına erişmek için, yöneticileriniz tarafından ek yapılandırma olmadan bu kaynaklara genel internet üzerinden erişebilirsiniz.

> [!NOTE]
> Kaynağı doğrudan VM'ye sağlayarak laboratuvarınızın dış kaynaklara olan bağımlılıklarını azaltıp azaltamayacağınızı düşünmelisiniz.  Örneğin, harici bir veritabanından veri okuma gereksinimini ortadan kaldırmak için veritabanını doğrudan VM'ye yükleyebilirsiniz.  

**Maliyetler nasıl kontrol edilecek?**

Laboratuvar Hizmetleri, yalnızca bir laboratuvar VM'sinin çalıştırdığı süre için ödeme yaptığınız anlamına gelen bir ödeme-as-you-go fiyatlandırma modeli kullanır.  Maliyetleri denetlemek için, genellikle birbiriyle birlikte kullanılan üç seçeneğiniz vardır:

- **Zamanlama** - Zamanlama, laboratuvarlarınızın VM'lerinin başlatılıp kapatıldığında otomatik olarak kontrol etmenizi sağlar.
- **Kontenjan** - Kontenjan, öğrencilerin planlanan saatler dışında VM'ye erişebilmek için erişeedecekleri saat sayısını kontrol eder.  Öğrenci kullanırken kontenjana ulaşılırsa, VM otomatik olarak kapatılır ve kontenjan artırılmadığı sürece öğrenci VM'yi yeniden başlatamaz.
- **Otomatik kapatma** - Otomatik kapatma ayarı etkinleştirildiğinde, bir öğrenciRDP oturumu kesildikten sonra Windows VM'lerin belirli bir süre sonra otomatik olarak kapanmasına neden olur.  Bu ayar varsayılan olarak devre dışıdır.  

    > [!NOTE]
    > Bu ayar şu anda yalnızca Windows için vardır.

**Öğrenciler işlerini nasıl kurtaracaklar?**

Öğrencilerin her biri, laboratuvarın ömrü boyunca kendilerine verilen kendi VM'lerine atanır.  Onlar seçebilirsiniz:

- Doğrudan VM'ye kaydedin.
- OneDrive, GitHub, vb. gibi harici bir depoya kaydedin.

OneDrive'ı kullanmak için, bunu öğrenciler için laboratuvar VM'lerinde otomatik olarak yapılandırmayı seçebilirsiniz.  Bu konuda ek bilgi aşağıda verilmiştir.

> [!NOTE]
> Öğrencilerinizin, sınıf sona erdikten sonra da dahil olmak üzere laboratuvar dışında kaydedilmiş işlerine erişmeye devam etmeleri için, öğrencilerin çalışmalarını harici bir depoya kaydetmelerini öneririz.

**Öğrenciler VM'lerine nasıl bağlanacak?**

RDP'den Windows VM'lere, öğrencilerin [Microsoft Uzak Masaüstü istemcisini](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)kullanmalarını öneririz.  Uzak Masaüstü istemcisi Mac'leri, Chromebook'ları ve Windows'u destekler.

Linux VM'leri için öğrenciler SSH veya RDP kullanabilirler.   RDP kullanarak bağlanmak için, gerekli RDP ve GUI paketlerini yüklemekve yapılandırmak sizin sorumluluğunuzdadır.  Bu konuda daha fazla bilgi aşağıda verilmiştir.

## <a name="set-up-your-lab"></a>Laboratuvarınızı kurma

Sınıfınızın laboratuarı nın gereksinimlerini anladığınızda, bunu ayarlamaya hazırsınız.  Laboratuvarınızı nasıl kurup kurup kurup kurup kurup kurup göreceğinizi görmek için bu bölümdeki bağlantıları izleyin.

1. **Laboratuvar oluşturma**

   Talimatlar için [bir sınıf laboratuvarı oluşturma](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#create-a-classroom-lab) yla ilgili öğreticiye bakın.

    > [!NOTE]
    > Sınıfınız iç içe sanallaştırma gerektiriyorsa, [iç içe geçmeyi etkinleştirmeyi](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-nested-virtualization-template-vm)gösteren nasıl yapılacağını kılavuzundaki adımlara bakın.

1. **Görüntüleri özelleştirin ve laboratuvar VM'lerini yayımlayın**

    Görüntüleri özelleştirmek ve laboratuarınızda VM yayınlamak için, şablon VM olarak adlandırılan özel bir VM'ye bağlanırsınız; aşağıdaki kılavuzlara bakın:
    - [Bir şablon oluşturma ve yönetme VM](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#publish-the-template-vm)
    - [Paylaşılan Resim Galerisi'ni kullanma](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-shared-image-gallery)

    > [!NOTE]
    > Windows kullanıyorsanız, [Windows şablonu VM hazırlamak](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-prepare-windows-template)için nasıl yapılır kılavuzundaki talimatlara da başvurmalısınız.  Bu yönergeler, öğrencilerinizin kullanması için OneDrive ve Office'i ayarlama adımlarını içerir.

1. **VM havuzuve kapasitesini yönetin**

   Sınıfınızın gerektirdiği şekilde VM kapasitesini kolayca ölçeklendirebilir veya küçültebilirsiniz.  VM kapasitesini artırmanın yeni VM'ler kurmayı gerektirdiğinden birkaç saat sürebileceğini unutmayın.  [VM havuzu kurmak ve yönetmek](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-set-virtual-machine-passwords)için nasıl yapIlir kılavuzundaki adımlara bakın.

1. **Laboratuvar kullanıcılarını ekleme ve yönetme**

   Laboratuvarınıza kullanıcı eklemek için aşağıdaki öğreticilerdeki adımlara bakın:
   - [Kullanıcıları laboratuvara ekleme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#add-users-to-the-lab)
   - [Kullanıcılara davetiye gönderme](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#send-invitation-emails-to-users)

    Ayrıca, öğrencilerin kullanabileceği hesap türleri hakkında bilgi için aşağıdaki makaleye bakın:
    - [Öğrenci hesapları](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#student-accounts)
  
1. **Maliyet denetimlerini ayarlama**

    Laboratuvarınızın maliyetlerini denetlemek için zamanlamaları, kotaları ve otomatik kapatmayı ayarlayın; aşağıdaki öğreticiler talimatlarına bakın:

   - [Zamanlama ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)
        > [!NOTE]
        > Yüklediğiniz işletim sistemi türüne bağlı olarak, bir VM'nin başlatılması birkaç dakika sürebilir.  Bir laboratuvar VM'inin zamanlanan saatlerinizde kullanıma hazır olduğundan emin olmak için, VM'lerin çalışmaya ve kullanıma hazır olduğundan emin olmak için VM'leri 30 dakika önceden başlatmanızı öneririz.

   - [Kullanıcılar için kota lar ayarlayın](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users) ve belirli bir kullanıcı için ek kota [ayarlayın](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-additional-quota-for-a-specific-user)
  
   - [Bağlantı kesildiğinde otomatik kapatmayı etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

        > [!NOTE]
        > Zamanlamalar, kotalar ve otomatik kapatma şablonU VM için geçerli *değildir.*  Sonuç olarak, kullanılmadığı zaman vm şablonu kapattığınızdan emin olmalısınız, aksi takdirde maliyete maruz kalır.  Ayrıca, varsayılan olarak, bir laboratuvar oluşturduğunuzda, VM şablonu otomatik olarak başlatılır, bu nedenle laboratuarı ayarlamayı hemen bitirdiğinizden ve şablon VM'yi kapattığınızdan emin olmak istersiniz.

1. **Panoyu kullanma**

    Talimatlar için [laboratuvarın panosunu kullanma kılavuzuna](https://docs.microsoft.com/azure/lab-services/classroom-labs/use-dashboard) bakın.

    > [!NOTE]
    > Panoda gösterilen tahmini maliyetler, öğrencilerin laboratuvar kullanımı için bekleyebileceğiniz maksimum maliyettir.  Örneğin, öğrencileriniz *not* tarafından kullanılmayan kota saatleri için ücretlendirilmez.  Tahmini maliyetler, VM şablonu veya Paylaşılan Resim Galerisi'ni kullanmak için herhangi bir ücret *yansıtmaz.*

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere bakın:

- [Sınıf laboratuvarının kullanımını izleme](tutorial-track-usage.md)
  
- [Bir sınıf laboratuvarına erişim](tutorial-connect-virtual-machine-classroom-lab.md)
