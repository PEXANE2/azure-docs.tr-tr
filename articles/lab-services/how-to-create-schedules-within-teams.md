---
title: Takımlar içinde Azure Lab Services zamanlamaları oluşturma
description: Takımlar içinde Laboratuvar Hizmetleri zamanlamaları oluşturmayı öğrenin.
ms.topic: article
ms.date: 10/07/2020
ms.openlocfilehash: 5578a2f63a4d6f58998a54560299a4356ed65997
ms.sourcegitcommit: ba7fafe5b3f84b053ecbeeddfb0d3ff07e509e40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91946852"
---
# <a name="create-and-manage-lab-services-schedules-within-teams"></a>Takımlar içinde Laboratuvar Hizmetleri zamanlamaları oluşturma ve yönetme

Zamanlamalar, laboratuvardaki VM 'Lerin otomatik olarak başlamasını ve belirli bir zamanda kapatılmasını sağlamak için bir derslik Laboratuvarı yapılandırmanıza olanak tanır. Tek seferlik bir zamanlama veya yinelenen bir zamanlama tanımlayabilirsiniz. Aşağıdaki yordamlar, bir derslik Laboratuvarı için zamanlama oluşturma ve yönetme adımları sunar: 

Zamanlamaların laboratuvar sanal makinelerini nasıl etkilediği aşağıda verilmiştir: 

- Şablon sanal makinesi zamanlamalara dahil değildir. 
- Yalnızca atanan sanal makineler başlatılır. Bu, bir makinenin Son Kullanıcı (öğrenci) tarafından istenmediği anlamına gelir, makinenin zamanlanan saatlerde başlamamasını sağlayacaktır. 
- Tüm sanal makineler (bir kullanıcı tarafından talep edilen veya değil) laboratuvar zamanlaması temelinde durdurulur. 

> [!IMPORTANT]
> VM 'lerin zamanlanan çalışma süresi, bir kullanıcıya ayrılan kotaya göre sayılmaz. Kota, bir öğrencinin VM 'lerde harcadığı zamanlama saatleri dışında bir süredir. 

## <a name="createeditdelete-a-schedule-for-the-lab"></a>Laboratuvar için zamanlama oluşturma/düzenleme/silme

Laboratuvardaki VM 'Lerin belirli zamanlarda otomatik olarak başlatılması/durdurulması için laboratuvar için zamanlanmış bir olay oluşturun. Daha önce belirttiğiniz Kullanıcı kotası, bu zamanlanan sürenin dışında her bir kullanıcıya atanan ek süredir. 

1. **Zamanlamalar** sayfasına geçin ve araç çubuğunda **Zamanlanmış olay Ekle** ' yi seçin (pencerenin üst/sol tarafında). 
1. Zamanlama için aşağıdaki parametreleri ayarlayın:
    1. **Standart** **olay türünün**seçili olduğunu onaylayın. Yalnızca VM 'Lerin başlangıç saatini belirtmek için **Başlat** ' ı seçin. Yalnızca VM 'Ler için durma saatini belirtmek üzere **Durdur** ' u seçin. 
    1. **Başlangıç tarihini**belirtin.
    1. VM 'Lerin **başlamasını istediğiniz başlangıç saatini** belirtin.
    1. VM 'Lerin kapatıldığı **bitiş saatini** belirtin. 
    1. Belirttiğiniz başlangıç ve durdurma zamanları için **saat dilimini** belirtin. 
    1. **Yinele** bölümünde **her hafta** veya **hiçbir**şekilde öğesini seçin. 
    1. **Notlar için (isteğe bağlı)**, zamanlama için herhangi bir açıklama veya Not girin. 
1. **Kaydet**’e tıklayın. 

### <a name="view-schedules-in-calendar"></a>Takvim 'de zamanlamaları görüntüleme

Zamanlanan tarihleri ve saatleri takvimde vurgulanmış şekilde görebilirsiniz. Takvimdeki geçerli tarihe geçmek için sağ üst köşedeki **bugün** düğmesini seçin. Önceki haftaya ve **sağ oka** geçiş yapmak için **sol ok** ' i seçerek takvimdeki bir sonraki haftaya geçiş yapın. 

### <a name="edit-a-schedule"></a>Zamanlamayı Düzenle

Takvimde vurgulanan bir zamanlamayı seçtiğinizde, zamanlamayı **düzenleme** veya **silme** düğmelerini görürsünüz. 

**Zamanlanmış olayı Düzenle** sayfasında, zamanlamayı güncelleştirebilir ve **Kaydet**' i seçebilirsiniz. 

### <a name="delete-a-schedule"></a>Zamanlamayı silme

1. Bir zamanlamayı silmek için takvimde vurgulanan bir zamanlamayı seçin ve çöp kutusu simgesini (Sil) seçin:
1. **Zamanlanmış olayı Sil** iletişim kutusunda, silme işlemini onaylamak için **Evet** ' i seçin. 

## <a name="automatic-shutdown-and-disconnect-settings"></a>Otomatik olarak kapalı ve bağlantı kesme ayarları

Sayfanın üst kısmında, **oto kapatma** ayarlarının bağlantısını görürsünüz.

Sanal makineler etkin bir şekilde kullanılmadığınızda ek maliyetleri önceden engellemek için çeşitli oto kapatma maliyeti denetim özelliklerini etkinleştirebilirsiniz. Aşağıdaki üç otomatik kapatılma ve bağlantı kesme özelliğinin birleşimi, kullanıcıların sanal makinelerini yanlışlıkla bırakması için gereken durumların çoğunu yakalar:
 
- İşletim sisteminin boşta kaldığı sanal makinelerle kullanıcıların bağlantısını otomatik olarak keser.
- Kullanıcıların bağlantısı kesildiğinde sanal makineleri otomatik olarak kapat.
- Başlatılan ancak kullanıcılar bağlanmadığında sanal makineleri otomatik olarak kapat.

Daha fazla bilgi için, Ayarlar ' ın yanındaki *Seçenekler simgesine tıklayın* .

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere bakın:

- [Takımlara genel bakış içinde Azure Lab Services kullanma](lab-services-within-teams-overview.md)
- [Çalışmaya başlayın ve ekiplerden Laboratuvar Hizmetleri Laboratuvarı oluşturun](how-to-get-started-create-lab-within-teams.md)
- [Ekiplerden Laboratuvar Hizmetleri Kullanıcı listelerini yönetme](how-to-manage-user-lists-within-teams.md)
- [Ekiplerden laboratuvar hizmetlerinde bir VM havuzunu yönetme](how-to-manage-vm-pool-within-teams.md)
- [Ekiplerden laboratuvar hizmetlerinde bir VM 'ye (öğrenci görünümü) erişme](how-to-access-vm-for-students-within-teams.md)