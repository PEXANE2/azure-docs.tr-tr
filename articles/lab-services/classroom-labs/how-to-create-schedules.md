---
title: Azure Lab Services sınıf laboratuvarları için zamanlama oluşturma | Microsoft Docs
description: Laboratuvardaki VM 'Lerin belirli bir zamanda başlamasını ve kapatılmasını sağlamak üzere Azure Lab Services sınıfta derslik Labs için zamanlamalar oluşturmayı öğrenin.
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
ms.date: 01/30/2019
ms.author: spelluru
ms.openlocfilehash: f607ba68563aa92797f45cf77db0575ae6802fee
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385591"
---
# <a name="create-and-manage-schedules-for-classroom-labs-in-azure-lab-services"></a>Azure Lab Services sınıf laboratuvarları için zamanlamalar oluşturma ve yönetme 
Zamanlamalar, laboratuvardaki VM 'Lerin otomatik olarak başlamasını ve belirli bir zamanda kapatılmasını sağlamak için bir derslik Laboratuvarı yapılandırmanıza olanak tanır. Tek seferlik bir zamanlama veya yinelenen bir zamanlama tanımlayabilirsiniz. Aşağıdaki yordamlar, bir derslik Laboratuvarı için zamanlama oluşturma ve yönetme adımları sunar: 

> [!IMPORTANT]
> VM 'lerin zamanlanan çalışma süresi, [bir kullanıcıya ayrılan kotaya](how-to-configure-student-usage.md#set-quotas-for-users)göre sayılmaz. Kota, bir öğrencinin VM 'lerde harcadığı zamanlama saatleri dışında bir süredir. 

## <a name="add-a-schedule-once"></a>Zamanlama Ekle (bir kez)

1. **Zamanlamalar** sayfasına geçin ve araç çubuğunda **zamanlama Ekle** ' yi seçin. 

    ![Zamanlamalar sayfasında zamanlama Ekle düğmesi](../media/how-to-create-schedules/add-schedule-button.png)
2. **Zamanlama Ekle** sayfasında, en üstte **bir kez** seçeneğinin seçili olduğunu onaylayın. Aksi takdirde **bir kez**seçin. 
3. **Zamanlama tarihi (gerekli)** için tarihi girin veya takvim simgesini seçerek bir tarih seçin. 
4. **Başlangıç saati**Için, VM 'lerin başlamasını istediğiniz saati seçin. Durdurma zamanı ayarlanmamışsa başlangıç zamanı gereklidir. Yalnızca durdurma zamanını belirtmek istiyorsanız **Başlangıç olayını kaldır** ' ı seçin. **Başlangıç saati** devre dışıysa, etkinleştirmek için açılan listenin yanındaki **Başlangıç olayı Ekle** ' yi seçin. 
5. **Durdurma zamanı**Için, VM 'lerin kapatılmasını istediğiniz saati seçin. Başlangıç saati ayarlanmamışsa durdurma zamanı gereklidir. Yalnızca başlangıç saatini belirtmek istiyorsanız **durdurma olayını kaldır** ' ı seçin. **durdurma zamanı** devre dışıysa, etkinleştirmek için açılan listenin yanındaki **Durdur olayını Ekle** ' yi seçin.
6. **Saat dilimi (gerekli)** için belirttiğiniz başlangıç ve durdurma zamanları için saat dilimini seçin. 
7. **Notlar**için, zamanlama için herhangi bir açıklama veya Not girin. 
8. **Kaydet**’i seçin. 

    ![Onetime zamanlaması](../media/how-to-create-schedules/add-schedule-page.png)

## <a name="add-a-recurring-schedule-weekly"></a>Yinelenen zamanlama ekleme (haftalık)

1. **Zamanlamalar** sayfasına geçin ve araç çubuğunda **zamanlama Ekle** ' yi seçin. 

    ![Zamanlamalar sayfasında zamanlama Ekle düğmesi](../media/how-to-create-schedules/add-schedule-button.png)
2. **Zamanlama Ekle** sayfasında, en üstte **haftalık** olarak geçiş yapın. 
3. **Zamanlama günleri (gerekli)** için, zamanlamanın etkili olmasını istediğiniz günleri seçin. Aşağıdaki örnekte Pazartesi-Cuma seçilidir. 
4. **Kimden** alanı için **Zamanlama başlangıç tarihini** girin veya **Takvim** düğmesini seçerek bir tarih seçin. Bu alan zorunludur. 
5. **Zamanlama bitiş tarihi**Için, VM 'lerin kapatılacak bitiş tarihini girin veya seçin. 
6. **Başlangıç zamanı**Için, VM 'lerin başlamasını istediğiniz saati seçin. Durdurma zamanı ayarlanmamışsa başlangıç zamanı gereklidir. Yalnızca durdurma zamanını belirtmek istiyorsanız **Başlangıç olayını kaldır** ' ı seçin. **Başlangıç saati** devre dışıysa, etkinleştirmek için açılan listenin yanındaki **Başlangıç olayı Ekle** ' yi seçin. 
7. **Durdurma zamanı**Için, sanal makinelerin kapatılmasını istediğiniz saati seçin. Başlangıç saati ayarlanmamışsa durdurma zamanı gereklidir. Yalnızca başlangıç saatini belirtmek istiyorsanız **durdurma olayını kaldır** ' ı seçin. **durdurma zamanı** devre dışıysa, etkinleştirmek için açılan listenin yanındaki **Durdur olayını Ekle** ' yi seçin.
8. **Saat dilimi (gerekli)** için belirttiğiniz başlangıç ve durdurma zamanları için saat dilimini seçin.  
9. **Notlar**için, zamanlama için herhangi bir açıklama veya Not girin. 
10. **Kaydet**’i seçin. 

    ![Haftalık zamanlama](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="view-schedules-in-calendar"></a>Takvim 'de zamanlamaları görüntüleme
Aşağıdaki görüntüde gösterildiği gibi, Takvim görünümünde vurgulanmış olan zamanlanmış tarihleri ve zamanları görebilirsiniz:

![Takvim görünümündeki zamanlamalar](../media/how-to-create-schedules/schedules-in-calendar.png)

Takvimdeki geçerli tarihe geçmek için sağ üst köşedeki **bugün** düğmesini seçin. Önceki haftaya ve **sağ oka** geçiş yapmak için **sol ok** ' i seçerek takvimdeki bir sonraki haftaya geçiş yapın. 

## <a name="edit-a-schedule"></a>Zamanlamayı Düzenle
Takvimdeki vurgulanmış bir zamanlamaya çift tıkladığınızda ya da araç çubuğunda **kurşun kalem** düğmesini seçtiğinizde, **Zamanlamayı Düzenle** sayfasını görürsünüz. Bu sayfadaki ayarların güncelleştirilmesi, **zamanlama Ekle** sayfasında, [Yinelenen zamanlama ekleme](#add-a-recurring-schedule-weekly) bölümünde açıklandığı gibi ayarları güncelleştirme ile aynıdır. 

![Zamanlamayı Düzenle sayfası](../media/how-to-create-schedules/edit-schedule-page.png)

## <a name="delete-a-schedule"></a>Zamanlamayı silme

1. Bir zamanlamayı silmek için, aşağıdaki görüntüde gösterildiği gibi araç çubuğundaki çöp kutusu (Sil) düğmesini seçin:

    ![Araç çubuğundaki Sil düğmesi](../media/how-to-create-schedules/delete-schedule-button.png)

    Takvimdeki zamanlanmış tarihler ve saatler için Sil düğmesini kullanabilir ve **Sil**' i seçebilirsiniz. 
2. **Zamanlamaları silme** sayfasında **Evet**' i seçin.

    ![Zamanlamaları silme onayı](../media/how-to-create-schedules/delete-schedules-confirmation.png)




## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Yönetici olarak, laboratuvar hesapları oluşturun ve yönetin](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak, Labs oluşturma ve yönetme](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak, bir laboratuvarın kullanımını yapılandırma ve denetleme](how-to-configure-student-usage.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)
