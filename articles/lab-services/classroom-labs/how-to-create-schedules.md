---
title: Azure Laboratuvar Hizmetleri'ndeki sınıf laboratuvarları için zamanlama oluşturma | Microsoft Dokümanlar
description: Laboratuvardaki VM'lerin belirli bir zamanda başlayıp kapanması için Azure Lab Hizmetleri'ndeki sınıf laboratuvarları için zamanlamaları nasıl oluşturacağınızhakkında bilgi edinin.
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
ms.date: 10/12/2019
ms.author: spelluru
ms.openlocfilehash: 4887b4359451ca5ce85042b4de42d5376bf4a730
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80667775"
---
# <a name="create-and-manage-schedules-for-classroom-labs-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'ndeki sınıf laboratuvarları için zamanlamaoluşturma ve yönetme 
Zamanlamalar, laboratuvardaki VM'lerin belirli bir zamanda otomatik olarak başlayıp kapanması nı sağlayacak şekilde bir sınıf laboratuarı yapılandırmanıza olanak sağlar. Tek seferlik zamanlama veya yinelenen bir zamanlama tanımlayabilirsiniz. Aşağıdaki yordamlar, bir sınıf laboratuvarı için zamanlamaoluşturmak ve yönetmek için adımlar verir: 

> [!IMPORTANT]
> VM'lerin zamanlanan çalışma [süresi, kullanıcıya ayrılan kotaya](how-to-configure-student-usage.md#set-quotas-for-users)dahil edilmez. Kontenjan, öğrencinin VM'lerde harcadığı zamanlama saatleri dışındaki süreler içindir. 

## <a name="set-a-schedule-for-the-lab"></a>Laboratuvar için bir zamanlama ayarlama
Laboratuardaki VM'lerin belirli zamanlarda otomatik olarak başlatılabilmesi/durdurulması için laboratuvar için zamanlanmış bir olay oluşturun. Daha önce belirttiğiniz kullanıcı kotası, bu zamanlanan saatin dışında her kullanıcıya atanan ek süredir. 

> [!NOTE]
> Başlamadan önce, zamanlamalar laboratuvar sanal makinelerini şu şekilde etkiler: 
>- Şablon sanal makine zamanlamalara dahil edilmez. 
>- Yalnızca atanan sanal makineler başlatılır. Bu, bir makinenin son kullanıcı (öğrenci) tarafından talep edilmezse, makinenin planlanan saatlerde başlatılmayacağı anlamına gelir. 
>- Tüm sanal makineler (kullanıcı tarafından talep edilsin veya talep edilmesin) laboratuar zamanlamasına göre durdurulur. 

1. **Zamanlamalar** sayfasına geçin ve araç çubuğunda **zamanlanmış olay ekle'yi** seçin. 

    ![Zamanlamalar sayfasında zamanlama düğmesi ekleme](../media/how-to-create-schedules/add-schedule-button.png)
2. **Standart'ın** Olay **türü**seçildiğini onaylayın. Yalnızca VM'lerin başlangıç saatini belirtmek için **Başlat'ı** seçin. Yalnızca VM'lerin durmasını belirtmek için **Durdur'u** seçin. 
7. **Yinele** bölümünde geçerli zamanlamayı seçin. 

    ![Zamanlamalar sayfasında zamanlama düğmesi ekleme](../media/how-to-create-schedules/select-current-schedule.png)
5. **Yinele** iletişim kutusunda aşağıdaki adımları yapın:
    1. **Her** **hafta** tekrar alanı için ayarlı olduğunu doğrulayın. 
    3. Başlangıç **tarihini**belirtin.
    4. VM'lerin başlatılmasını istediğiniz **başlangıç saatini** belirtin.
    5. VM'lerin kapatılacağını **niçin durduracağını** belirtin. 
    6. Belirttiğiniz başlangıç ve durdurma saatleri için **saat dilimini** belirtin. 
    2. Zamanlamanın etkili olmasını istediğiniz günleri seçin. Aşağıdaki örnekte, Pazartesi-Perşembe seçilir. 
    8. **Kaydet'i**seçin. 

        ![Yineleme çizelgesini ayarlama](../media/how-to-create-schedules/set-repeat-schedule.png)

3. Şimdi, Notlar için **zamanlanan olay** ekle sayfasında **(isteğe bağlı)** zamanlama için herhangi bir açıklama veya not girin. 
4. **Zamanlanan olay** ekle sayfasında **Kaydet'i**seçin. 

    ![Haftalık program](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="view-schedules-in-calendar"></a>Takvimdeki zamanlamaları görüntüleme
Takvim görünümünde vurgulanan zamanlanan tarihleri ve saatleri aşağıdaki resimde gösterildiği gibi görebilirsiniz:

![Takvim görünümündeki zamanlamalar](../media/how-to-create-schedules/schedules-calendar.png)

Takvimdeki geçerli tarihe geçmek için sağ üst köşedeki **Bugün** düğmesini seçin. Takvimde bir sonraki haftaya geçmek için **sol oku** ve önceki haftaya geçmek için **sağ oku** seçin. 

## <a name="edit-a-schedule"></a>Zamanlamayı düzenleyin
Takvimde vurgulanmış bir zamanlama seçtiğinizde, zamanlamayı **düzenleyecek** veya **silmek** için düğmeleri görürsünüz. 

![Zamanlama sayfasını düzenleyin](../media/how-to-create-schedules/schedule-edit-button.png)

**Zamanlanan etkinlik** sayfasını düzenleyin, zamanlamayı güncelleştirebilir ve **Kaydet'i**seçebilirsiniz. 

## <a name="delete-a-schedule"></a>Zamanlamayı silme

1. Zamanlamayı silmek için, takvimde vurgulanmış bir zamanlama seçin ve çöp kutusu simgesini (sil) düğmesini seçin:

    ![Araç çubuğundaki silme düğmesi](../media/how-to-create-schedules/schedule-delete-button.png)
2. Silme **zamanlanan olay** iletişim kutusunda, silme işlemini onaylamak için **Evet'i** seçin. 



## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Yönetici olarak, laboratuvar hesapları oluşturun ve yönetin](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak, laboratuvarlar oluşturun ve yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak, bir laboratuvarın kullanımını yapılandırın ve kontrol edin](how-to-configure-student-usage.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)
