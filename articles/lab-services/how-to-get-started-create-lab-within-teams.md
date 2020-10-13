---
title: Çalışmaya başlayın ve ekiplerden Azure Lab Services Laboratuvarı oluşturun
description: Nasıl başlacağınızı ve ekiplerden Azure Lab Services Laboratuvarı oluşturmayı öğrenin.
ms.topic: article
ms.date: 10/08/2020
ms.openlocfilehash: b585196fe61a09697cfa203aaa33f08afae2b427
ms.sourcegitcommit: ba7fafe5b3f84b053ecbeeddfb0d3ff07e509e40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91946848"
---
# <a name="get-started-and-create-a-lab-services-lab-from-teams"></a>Çalışmaya başlayın ve ekiplerden Laboratuvar Hizmetleri Laboratuvarı oluşturun

Bu makalede, takımlara bir Azure Lab Services uygulamasının nasıl ekleneceği gösterilmektedir. Ardından, ekiplerden Laboratuvar oluşturma.

## <a name="add-a-lab-services-app-to-teams"></a>Takımlara Laboratuvar Hizmetleri uygulaması ekleme

Laboratuvar hizmetlerini doğrudan takımlar kanallarınıza ekleyebilirsiniz ve ardından, uygulama ekipteki herkes tarafından kullanılabilir. Bu üç adımı izleyin:

1. Uygulamayı eklemek istediğiniz takımlar kanalına gidin ve **+** "..." seçeneğine tıklayarak sekme eklemeyi seçin. sağ taraftaki pencerenin en üstünden. 
1. Sekme seçeneklerinde **Azure Lab Services** arayın ve bu uygulamayı ekleyin. 

    > [!NOTE]
    > Yalnızca takım **sahipleri** takım için laboratuvarları oluşturabilir.
1. Bu takımda sınıf laboratuvarları oluşturmak için kullanmak istediğiniz bir laboratuvar hizmetleri hesabı seçin. 

    Azure Lab Services, [Azure Lab Services Web sitesinde](https://labs.azure.com) çoklu oturum açma kullanır ve erişiminiz olan tüm Laboratuvar hesaplarını çeker. 

    Ekiplerle aynı kiracıda bulunan ve **sahibi**, **katılımcısı**veya **oluşturan** erişimi olan hesaplar görüntülenir. 

   ![ALS 'e hoş geldiniz](./media/integrate-with-teams/welcome.png) 
1. **Kaydet** ' e basın ve uygulama takımlara eklenir ve sekme kanala eklenir. 

    Artık, kanalınızdan **Azure Lab Services** sekmesini seçebilir ve aşağıdaki adımda anlatıldığı gibi laboratuvarları yönetmeye başlayabilirsiniz.

    Bir takımın bir üyesi sekmeyi eklediğinde, Kanaldaki herkes için görüntülenir. Uygulamaya erişimi olan tüm kullanıcılar, Microsoft ekipleri için kullandıkları kimlik bilgileriyle çoklu oturum açma erişimi sağlar. Uygulamaya erişimi olmayan tüm kullanıcılar ekiplerde sekmeyi görebilir, ancak kullanıcılara şirket içi uygulamaya ve uygulamanın yayımlanmış sürümüne Azure portal izin verene kadar engellenmiştir.

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

Laboratuvar hesabı seçildikten sonra, takım sahipleri takım için Labs oluşturabilir. Laboratuvar oluşturma işleminin tamamı ve laboratuar düzeyindeki tüm görevler takımlar içinde gerçekleştirilebilir. Kullanıcılar aynı takım ve takım sahibi içinde birden çok Laboratuvar oluşturma seçeneğine sahip olacak ve laboratuvar hesabı düzeyinde uygun erişimle, yalnızca belirli bir takımla ilişkili laboratuvarları görür.

## <a name="giving-access-to-users-of-the-lab-account"></a>Laboratuvar hesabı kullanıcılarına erişim verme

Laboratuvar hesabı düzeyindeki kullanıcılara erişim sağlama, [Azure](https://ms.portal.azure.com/) portalında gerçekleşecektir.

1. Azure portal Azure Lab Services hesabınıza gidin. 
1. **Laboratuvar hesabı** sayfasında **ERIŞIM denetimi (IAM)** seçeneğini belirleyin, araç çubuğunda **+ Ekle** ' yi seçin ve ardından araç çubuğunda **+ rol ataması Ekle** ' yi seçin. 

    ![Access Control > rol ataması Ekle düğmesi](./media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. **Rol ataması Ekle** sayfasında, **rol**için **Laboratuvar Oluşturucu** ' yı seçin, laboratuvar oluşturucuları rolüne eklemek istediğiniz kullanıcıyı seçin ve **Kaydet**' i seçin. 

    ![Laboratuvar Oluşturucu Ekle](./media/tutorial-setup-lab-account/add-lab-creator.png)

### <a name="creating-classroom-labs"></a>Sınıf laboratuvarları oluşturma

Sınıf laboratuvarları oluşturma işlemi, ekiplerden veya [Laboratuvar Hizmetleri Web sitesinden](https://labs.azure.com)Labs oluştururken de aynıdır. 

Bu makaleyi ayarlama hakkında ayrıntılar için bkz. ana hat laboratuvarları oluşturma işlemi: [Azure Lab Services sınıf laboratuvarlarını yönetin](how-to-manage-classroom-labs.md).

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
- [Ekiplerden laboratuvar hizmetlerinde bir VM havuzunu yönetme](how-to-manage-vm-pool-within-teams.md)
- [Ekiplerden Laboratuvar Hizmetleri zamanlamaları oluşturma](how-to-create-schedules-within-teams.md)
- [Ekiplerden laboratuvar hizmetlerinde bir VM 'ye (öğrenci görünümü) erişme](how-to-access-vm-for-students-within-teams.md)

