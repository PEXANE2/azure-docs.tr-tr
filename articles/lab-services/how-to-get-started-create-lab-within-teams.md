---
title: Çalışmaya başlayın ve ekiplerden Azure Lab Services Laboratuvarı oluşturun
description: Nasıl başlacağınızı ve ekiplerden Azure Lab Services Laboratuvarı oluşturmayı öğrenin.
ms.topic: article
ms.date: 10/08/2020
ms.openlocfilehash: 0604e2934ff6b011acfa9dd4a4b25fa58193e69b
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92044454"
---
# <a name="get-started-and-create-a-lab-services-lab-from-teams"></a>Çalışmaya başlayın ve ekiplerden Laboratuvar Hizmetleri Laboratuvarı oluşturun

Bu makalede, **Azure Lab Services** uygulamasının bir takıma nasıl ekleneceği ve ardından MS ekipleri ortamında nasıl laboratuvar oluşturulacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, takımınız için sanal makinelerle bir laboratuvar ayarlarsınız. Laboratuvar hesabında laboratuvar ayarlamak için laboratuvar hesabındaki şu rollerden birinin üyesi olmanız gerekir: Owner, Lab Creator veya katkıda bulunan. Laboratuvar hesabı oluşturmak için kullandığınız hesap, sahip rolüne otomatik olarak eklenir. Bu nedenle, bir laboratuvar oluşturmak için laboratuvar hesabı oluşturmak için kullandığınız kullanıcı hesabını kullanabilirsiniz.

Ekipler içinde Azure Lab Services kullanırken tipik iş akışı burada verilmiştir

1. Kullanıcı Azure portal [bir laboratuvar hesabı oluşturur](tutorial-setup-lab-account.md#create-a-lab-account) .
1. [Laboratuvar hesabı Oluşturucusu, diğer kullanıcıları](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role) **Laboratuvar Oluşturucu** rolüne ekler. Örneğin, laboratuvar hesabı oluşturan/yönetici laboratuvar **Oluşturucu** rolüne eğitimciler ekler; böylece, sınıfları için laboratuvarları oluşturabilirler.
1. Daha sonra, eğitimciler Create Labs, şablon VM 'yi önceden yapılandırır ve ekipteki herkese VM 'yi sağlamak üzere Laboratuvarı yayımlar.
1. Laboratuvar yayımlandıktan sonra, Azure Lab Services için ilk oturum açmada ekip üyeliği listesinde herkese bir sanal makine atanır (SSO) veya [Labs Web sitesine](https://labs.azure.com)erişerek **Azure Lab Services** uygulama içeren sekmeye tıklayın. Böylece kullanıcılar VM 'yi kullanarak iş ve ev ödevini yapabilir.

## <a name="add-azure-lab-services-app-as-a-tab-to-a-team"></a>Takıma bir sekme olarak Azure Lab Services uygulaması ekleme

Ekip sahibi olarak, doğrudan takımlar kanallarınızda **Azure Lab Services** uygulama ekleyebilir ve ardından bu uygulama ekipteki herkes tarafından kullanılabilir. Aşağıdaki üç adımı izleyin:

1. Uygulamayı eklemek istediğiniz takımlar kanalına gidin ve **+** sekme eklemeyi seçin. 
1. Sekme seçeneklerinde **Azure Lab Services** arayın ve bu uygulamayı ekleyin. 

    > [!NOTE]
    > Yalnızca takım **sahipleri** takım için laboratuvarları oluşturabilir.
1. Bu takımda sınıf laboratuvarları oluşturmak için kullanmak istediğiniz bir laboratuvar hizmetleri hesabı seçin. 

    Azure Lab Services, [Azure Lab Services Web sitesinde](https://labs.azure.com) çoklu oturum açma kullanır ve erişiminiz olan tüm Laboratuvar hesaplarını çeker. 

    Ekiplerle aynı kiracıda bulunan ve **sahibi**, **katılımcısı**veya **oluşturan** erişimi olan hesaplar görüntülenir. 

   ![ALS 'e hoş geldiniz](./media/integrate-with-teams/welcome.png) 
1. **Kaydet** ' e basın ve sekme kanala eklenir.

    Artık, kanalınızdan **Azure Lab Services** sekmesini seçebilir ve aşağıdaki adımda anlatıldığı gibi laboratuvarları yönetmeye başlayabilirsiniz.

Laboratuvar hesabı seçildikten sonra, takım sahipleri takım için Labs oluşturabilir. Laboratuvar oluşturma işleminin tamamı ve laboratuar düzeyindeki tüm görevler takımlar içinde gerçekleştirilebilir. Kullanıcılar aynı takım ve takım sahibi içinde birden çok Laboratuvar oluşturma seçeneğine sahip olacak ve laboratuvar hesabı düzeyinde uygun erişimle, yalnızca belirli bir takımla ilişkili laboratuvarları görür.

## <a name="deleting-classroom-labs"></a>Sınıf laboratuvarlarını silme

Takımlar içinde oluşturulan bir laboratuvar, laboratuvar [Hizmetleri Web sitesinde](https://labs.azure.com) , [Azure Lab Services yerleşik sınıf laboratuvarları yönetme](how-to-manage-classroom-labs.md)bölümünde açıklandığı gibi doğrudan laboratuvar silinerek silinebilir. 

Laboratuvar silme, takım silindiğinde de tetiklenir. Laboratuvarın oluşturulduğu takım silinirse, otomatik kullanıcı listesi eşitleme tetiklendikten sonra laboratuvar 24 saat sonra otomatik olarak silinir. 

Sekmeyi silme veya uygulamayı kaldırma, laboratuvarın silinmesine neden olmaz. Sekme silinirse, takım üyelik listesindeki kullanıcılar, laboratuvar silme, Web sitesinde laboratuvar silinerek veya takım silinirken açıkça tetiklenmediği takdirde [Laboratuvar Hizmetleri Web sitesindeki](https://labs.azure.com) VM 'lere erişebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Takımlar içinde bir laboratuvar oluşturulduğunda, laboratuvar Kullanıcı Listesi otomatik olarak doldurulur ve takım üyeliğiyle eşitlenir. Ekip üzerinde sahipler, Üyeler ve konuklar dahil herkes otomatik olarak laboratuvar Kullanıcı listesine eklenecektir. Azure Laboratuvar Hizmetleri, takım üyeliğiyle bir eşitleme koruyacak ve 24 saatte bir otomatik eşitleme tetiklenecektir. Ayrıntılar için bkz.

[Ekiplerden Laboratuvar Hizmetleri Kullanıcı listelerini yönetme](how-to-manage-user-lists-within-teams.md)

### <a name="see-also"></a>Ayrıca bkz.

Ayrıca aşağıdaki makalelere bakın:

- [Takımlara genel bakış içinde Azure Lab Services kullanma](lab-services-within-teams-overview.md)
- [Takımlar içindeki laboratuvar kullanıcı listelerini yönetme](how-to-manage-user-lists-within-teams.md)
- [Laboratuvarın takımlar içindeki VM havuzunu yönetme](how-to-manage-vm-pool-within-teams.md)
- [Takımlar içinde laboratuvar zamanlamaları oluşturma ve yönetme](how-to-create-schedules-within-teams.md)
- [Takımlar içindeki bir VM 'ye erişme – öğrenci görünümü](how-to-access-vm-for-students-within-teams.md)

