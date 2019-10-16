---
title: Azure Lab Services VM 'Leri için parola ayarlama | Microsoft Docs
description: Azure Lab Services derslik laboratuvarlarında sanal makineler (VM) için parolaları ayarlamayı ve sıfırlamayı öğrenin.
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
ms.openlocfilehash: 40cdd0adf7bf100e1dbca64dbba68db3bc59a4fe
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331538"
---
# <a name="set-up-and-manage-virtual-machine-pool"></a>Sanal makine havuzunu ayarlama ve yönetme 
Bu makalede aşağıdaki görevlerin nasıl yapılacağı gösterilmektedir:

- Laboratuvardaki sanal makinelerin (VM) sayısını artırın
- Tüm VM 'Leri veya seçili VM 'Leri Başlat 
- VM 'Leri sıfırlama

## <a name="update-the-lab-capacity"></a>Laboratuvar kapasitesini güncelleştirme
Laboratuvar kapasitesini artırmak veya azaltmak için (bir laboratuvardaki sanal makinelerin sayısı), aşağıdaki adımları uygulayın:

1. **Sanal makine havuzu** sayfasında, **laboratuvar kapasitesi: &lt;number @ no__t-3 makineler**' i seçin.
2. Laboratuvarda istediğiniz yeni **VM sayısını** girin. Bu sayı, laboratuvarda kayıtlı Kullanıcı sayısından büyük veya buna eşit olmalıdır. 
3. Ardından **Kaydet**’i seçin. 

    ![Tümünü Başlat düğmesi](../media/how-to-set-virtual-machine-passwords/number-of-vms-in-lab.png)
4. Kapasiteyi arttırdıysanız, oluşturulmakta olan sanal makineyi veya VM 'Leri görebilirsiniz. 

    ![Oluşturulmakta olan VM](../media/how-to-set-virtual-machine-passwords/vm-being-created.png)

## <a name="start-vms"></a>VM 'Leri başlatma

### <a name="start-ot-stop-all-vms"></a>Tüm VM 'Leri durdur
1. **Sanal makine havuzu** sayfasına geçin. 
2. Araç çubuğundan **Tümünü Başlat** ' ı seçin. 

    ![Tümünü Başlat düğmesi](../media/how-to-set-virtual-machine-passwords/start-all-vms-button.png)
3. Tüm VM 'Ler başlatıldıktan sonra, araç çubuğundaki **Tümünü Durdur** düğmesini seçerek tüm VM 'leri durdurabilirsiniz. 

    ![Tümünü Durdur düğmesi](../media/how-to-set-virtual-machine-passwords/stop-all-vms-button.png)

### <a name="start-selected-vms"></a>Seçili VM 'Leri Başlat
Seçilen VM 'Leri başlatmak için iki yol vardır (bir veya daha fazla). İlk yöntem, listedeki VM 'yi veya VM 'Leri seçmek ve sonra araç çubuğundan **Başlat** ' ı seçmeniz gerekir. İkinci yöntem, listedeki VM veya VM 'Leri seçmek, satırlardan birindeki **durum** sütununda açılan menü ' yi seçmek ve ardından **Başlat**' ı seçmeniz gerekir. 

![Seçili VM 'Leri Başlat](../media/how-to-set-virtual-machine-passwords/start-selected-vms.png)

Benzer şekilde, **durum** sütunundaki açılan listeyi kullanarak bir veya daha fazla VM 'yi durdurabilir veya araç çubuğundan **durursunuz** . 

## <a name="reset-vms"></a>VM 'Leri sıfırlama
Bir veya daha fazla VM 'yi sıfırlamak için listeden bunları seçin ve ardından araç çubuğunda **Sıfırla** ' yı seçin. 

![Seçili VM 'Leri Sıfırla](../media/how-to-set-virtual-machine-passwords/reset-vm-button.png)

**Sanal makineleri Sıfırla** Iletişim kutusunda **Sıfırla**' yı seçin. 

![VM 'yi Sıfırla iletişim kutusu](../media/how-to-set-virtual-machine-passwords/reset-vms-dialog.png)



## <a name="set-password-for-vms"></a>VM 'Ler için parola ayarlama
Laboratuvar sahibi (öğretmen), Laboratuvar oluşturma (Laboratuvar oluşturma Sihirbazı) sırasında veya **şablon** sayfasında laboratuvar oluşturduktan sonra VM 'lerin parolasını ayarlayabilir/sıfırlayabilir. 

### <a name="set-password-at-the-time-of-lab-creation"></a>Laboratuvar oluşturma sırasında parola ayarla
Laboratuvar sahibi (öğretmen) Laboratuvar oluşturma sihirbazının **sanal makine kimlik bilgileri** sayfasında laboratuvardaki VM 'ler için bir parola ayarlayabilir.

![Yeni laboratuvar penceresi](../media/tutorial-setup-classroom-lab/virtual-machine-credentials.png)

Bu sayfadaki **tüm sanal makineler için aynı parolayı kullan** seçeneğini etkinleştirip devre dışı bırakarak bir öğretmen, laboratuvardaki tüm VM 'ler için aynı parolayı kullanmayı seçebilir veya öğrencilerin VM 'leri için parola değiştirmesine izin verebilir. Varsayılan olarak, bu ayar Ubuntu hariç tüm Windows ve Linux işletim sistemi görüntüleri için etkinleştirilmiştir. Bu ayar devre dışı bırakıldığında, sanal makineye ilk kez bağlanmaya çalıştıklarında öğrencilerden bir parola ayarlaması istenir. 

### <a name="reset-password-later"></a>Parolayı daha sonra Sıfırla

1. Laboratuvarın **şablon** sayfasında, araç çubuğunda **Parolayı Sıfırla** ' yı seçin. 

    ![Giriş sayfasındaki parola sıfırlama menüsü](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)
1. **Parolayı Sıfırla** iletişim kutusunda bir parola girin ve **Parolayı Sıfırla**' yı seçin.
    
    ![Parolayı ayarla iletişim kutusu](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="next-steps"></a>Sonraki adımlar
Sizin (Laboratuvar sahibi olarak) yapılandırabileceğiniz diğer öğrenci kullanım seçenekleri hakkında bilgi edinmek için şu makaleye bakın: [öğrenci kullanımını yapılandırma](how-to-configure-student-usage.md).

Öğrencilerin VM 'lerinin parolalarını nasıl sıfırlayacaklarını öğrenmek için bkz. [sınıf laboratuvarlarında sanal makineler için parola ayarlama veya sıfırlama (öğrenciler)](how-to-set-virtual-machine-passwords-student.md).