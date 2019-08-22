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
ms.date: 08/20/2019
ms.author: spelluru
ms.openlocfilehash: a4cb2abec429a790f493f95d3d16b2ff7b3eb445
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69645022"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs-instructor"></a>Sınıf laboratuvarlarında sanal makineler için parola ayarlama veya sıfırlama (eğitmen)
Laboratuvar sahibi (öğretmen), laboratuvar (Laboratuvar oluşturma Sihirbazı) oluşturma sırasında veya laboratuvar oluşturduktan sonra (panoda) VM 'Lerin parolasını ayarlayabilir/sıfırlayabilir. 

## <a name="set-password-at-the-time-of-lab-creation"></a>Laboratuvar oluşturma sırasında parola ayarla
Laboratuvar sahibi (öğretmen) Laboratuvar oluşturma sihirbazının **kimlik bilgilerini ayarla** sayfasında laboratuvardaki VM 'ler için bir parola ayarlayabilir.

![Kimlik bilgilerini ayarla](../media/tutorial-setup-classroom-lab/set-credentials.png)

Bu sayfadaki **tüm sanal makineler için aynı parolayı kullan** seçeneğini etkinleştirip devre dışı bırakarak bir öğretmen, laboratuvardaki tüm VM 'ler için aynı parolayı kullanmayı seçebilir veya öğrencilerin VM 'leri için parola değiştirmesine izin verebilir. Varsayılan olarak, bu ayar Ubuntu hariç tüm Windows ve Linux işletim sistemi görüntüleri için etkinleştirilmiştir. Bu ayar devre dışı bırakıldığında, sanal makineye ilk kez bağlanmaya çalıştıklarında öğrencilerden bir parola ayarlaması istenir. 

Laboratuvar sahibi, Laboratuvar oluşturma sihirbazının **şablonu Yapılandır** sayfasında bu parolayı sıfırlayabilir (gerekirse). 

![Tamamlanmış şablon yapılandırma sayfası](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)

Laboratuvar sahibi, panoda laboratuvar oluşturulduktan sonra parolayı da sıfırlayabilir. 

## <a name="reset-password-on-the-dashboard"></a>Panodaki parolayı sıfırlama

1. Laboratuvar kutucuğunda Taşma menüsünü (dikey üç nokta) seçin ve **Parolayı Sıfırla**' yı seçin. 

    ![Giriş sayfasındaki parola sıfırlama menüsü](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)
1. **Parolayı ayarla** iletişim kutusunda bir parola girin ve **parolayı ayarla**' yı seçin.
    
    ![Parolayı ayarla iletişim kutusu](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="next-steps"></a>Sonraki adımlar
Sizin (Laboratuvar sahibi olarak) yapılandırabileceğiniz diğer öğrenci kullanım seçenekleri hakkında bilgi edinmek için aşağıdaki makaleye bakın: [Öğrenci kullanımını yapılandırın](how-to-configure-student-usage.md).

Öğrencilerin VM 'lerinin parolalarını nasıl sıfırlayacaklarını öğrenmek için bkz. [sınıf laboratuvarlarında sanal makineler için parola ayarlama veya sıfırlama (öğrenciler)](how-to-set-virtual-machine-passwords-student.md).