---
title: Azure Laboratuvar Hizmetleri'nde VM'lerin parolalarını ayarlama | Microsoft Dokümanlar
description: Azure Lab Hizmetleri'nin sınıf laboratuarlarında sanal makinelerin (VM'ler) parolalarını nasıl ayarlayıp sıfırlayabileceğinizi öğrenin.
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
ms.date: 02/20/2020
ms.author: spelluru
ms.openlocfilehash: 0ff464936025a20cb6925adc7ef6eb44c2fe1f0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78933829"
---
# <a name="set-up-and-manage-virtual-machine-pool"></a>Sanal makine havuzunu ayarlama ve yönetme 
Bu makalede, aşağıdaki görevleri nasıl yapacağınızı gösterir:

- Laboratuvardaki sanal makine (VM) sayısını artırma
- Tüm VM'leri veya seçili VM'leri başlatma 
- VM'leri Sıfırla

## <a name="update-the-lab-capacity"></a>Laboratuvar kapasitesini güncelleştirin
Laboratuvar kapasitesini (laboratuvardaki sanal makine sayısı) artırmak veya azaltmak için aşağıdaki adımları yapın:

1. Sanal **makine havuzu** sayfasında, **Lab &lt;kapasitesini seçin:&gt; sayı makineleri.**
2. Laboratuvara istediğiniz yeni **VM sayısını** girin. Bu sayı, laboratuvarda kayıtlı kullanıcı sayısından büyük veya eşit olmalıdır. 
3. Ardından **Kaydet**’i seçin. 

    ![Tüm düğmeyi başlat](../media/how-to-set-virtual-machine-passwords/number-of-vms-in-lab.png)
4. Kapasiteyi artırdıysanız, VM veya VM'lerin oluşturulduğunu görebilirsiniz. Listede yeni VM'yi görmüyorsanız, sayfayı yenileyin. 

    ![VM oluşturuluyor](../media/how-to-set-virtual-machine-passwords/vm-being-created.png)

## <a name="start-vms"></a>VM'leri Başlat

### <a name="start-ot-stop-all-vms"></a>Tüm VM'leri durdurun
1. **Sanal makine havuzu** sayfasına geçin. 
2. Araç çubuğundan **Tümünü Başlat'ı** seçin. 

    ![Tüm düğmeyi başlat](../media/how-to-set-virtual-machine-passwords/start-all-vms-button.png)
3. Tüm VM'ler başlatıldıktan sonra, araç çubuğundaki **Tümünü Durdur** düğmesini seçerek tüm VM'leri durdurabilirsiniz. 

    ![Tüm düğmeyi durdur](../media/how-to-set-virtual-machine-passwords/stop-all-vms-button.png)

### <a name="start-selected-vms"></a>Seçili VM'leri başlat
Seçili VM'leri başlatmanın iki yolu vardır (bir veya daha fazla). İlk yol, listedeki VM veya VM'leri seçmek ve ardından araç çubuğunda **Başlat'ı** seçmektir. 

İkinci yol, listede bir veya daha fazla VM seçmek ve **Durum** sütunundaki düğmeyi geçiş yapmaktır. 

![Seçili VM'leri başlat](../media/how-to-set-virtual-machine-passwords/start-selected-vms.png)

Benzer şekilde, **Durum** sütunundaki düğmeyi değiştirerek veya araç çubuğunda **Durdur'u** seçerek bir veya daha fazla VM'yi durdurabilirsiniz. 

> [!NOTE]
> Bir eğitimci öğrenci nin VM'sini açarsa, öğrencinin kontenjanı etkilenmez. Kullanıcı kotası, kullanıcının zamanlanan ders saati dışında kullanabileceği laboratuvar saatsayısını belirtir. Kotalar hakkında daha fazla bilgi için, [kullanıcılar için kota ları ayarla'ya](how-to-configure-student-usage.md?#set-quotas-for-users)bakın.

## <a name="reset-vms"></a>VM'leri Sıfırla
Bir veya daha fazla VM'yi sıfırlamak için bunları listede seçin ve araç çubuğunda **Sıfırla'yı** seçin. 

![Seçili VM'leri sıfırlama](../media/how-to-set-virtual-machine-passwords/reset-vm-button.png)

Sanal **makine(ler)** iletişim kutusunu sıfırla **kutusunda, Sıfırla'yı**seçin. 

![VM iletişim kutusunu sıfırla](../media/how-to-set-virtual-machine-passwords/reset-vms-dialog.png)



## <a name="set-password-for-vms"></a>VM'ler için parola ayarlama
Bir laboratuvar sahibi (öğretmen), laboratuvar (laboratuvar oluşturma sihirbazı) oluşturma sırasında veya **Şablon** sayfasında ki laboratuvarı oluşturduktan sonra VM'lerin parolasını ayarlayabilir/sıfırlayabilir. 

### <a name="set-password-at-the-time-of-lab-creation"></a>Parolayı laboratuvar oluşturma sırasında ayarlama
Bir laboratuvar sahibi (öğretmen), laboratuvar oluşturma sihirbazının **Sanal makine kimlik bilgileri** sayfasında laboratuvardaki VM'ler için parola ayarlayabilir.

![Yeni laboratuvar penceresi](../media/tutorial-setup-classroom-lab/virtual-machine-credentials.png)

Bu sayfadaki tüm sanal **makineler için aynı parolayı kullan** seçeneğini etkinleştirerek/devre dışı bırakarak, öğretmen laboratuardaki tüm SANAL'lar için aynı parolayı kullanmayı seçebilir veya öğrencilerin SANAL'ları için parola ayarlamalarına izin verebilir. Varsayılan olarak, bu ayar Ubuntu hariç tüm Windows ve Linux işletim sistemi görüntüleri için etkinleştirilir. Bu ayar devre dışı bırakıldığında, öğrencilerden VM'ye ilk kez bağlanmaya çalıştıklarında bir parola ayarlamaları istenir. 

### <a name="reset-password-later"></a>Parolayı daha sonra sıfırla

1. Laboratuvarın **Şablon** sayfasında araç çubuğundaki **parolayı sıfırla'yı** seçin. 
1. **Parolayı Sıfırla** iletişim kutusunda, parola girin ve **parolayı sıfırla'yı**seçin.
    
    ![Parola iletişim kutusunu ayarlama](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="connect-to-student-vms"></a>Öğrenci VM'lerine bağlanma
Aşağıdaki koşullar yerine getirildiğinde laboratuvar oluşturucusu (eğitmen/profesör) öğrenci VM'sine bağlanabilir: 

- Laboratuarı oluştururken **tüm sanal makineler için aynı parolayı kullan** seçeneği seçildi
- VM çalışıyor 

 Öğrenci VM'sine bağlanmak için, listedeki VM fareyi tuşlayın ve bilgisayar düğmesini seçin.  

![Öğrenci VM düğmesine bağlanın](../media/how-to-set-virtual-machine-passwords/connect-student-vm.png)

> [!NOTE]
> Profesör VM'yi başlatTığında ve ona bağlandığında, öğrenci kontenjanı etkilenmez. 

## <a name="export-list-of-virtual-machines-to-a-csv-file"></a>Sanal makinelerin listesini CSV dosyasına dışa aktarma

1. **Sanal makine havuzu** sekmesine geçin.
2. Araç çubuğunda **...** (elips) seçeneğini belirleyin ve ardından **CSV'yi dışa**aktar'ı seçin. 

    ![Sanal makinelerin dışa aktarma listesi](../media/how-to-export-users-virtual-machines-csv/virtual-machines-export-csv.png)

## <a name="next-steps"></a>Sonraki adımlar
(Laboratuvar sahibi olarak) yapılandırabileceğiniz diğer öğrenci kullanım seçenekleri hakkında bilgi edinmek için aşağıdaki makaleye bakın: [Öğrenci kullanımını yapılandırın.](how-to-configure-student-usage.md)

Öğrencilerin Sanal M'leri için parolaları nasıl sıfırlayabildiğini öğrenmek [için sınıf laboratuarlarında (öğrenciler) sanal makineler için parola yı ayarla veya sıfırla'](how-to-set-virtual-machine-passwords-student.md)ya bakın.