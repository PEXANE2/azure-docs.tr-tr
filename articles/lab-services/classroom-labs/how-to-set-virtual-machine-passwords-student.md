---
title: Azure Laboratuvar Hizmetleri'nde sınıf laboratuvarı VM'leri için parolaları sıfırlama | Microsoft Dokümanlar
description: Azure Laboratuvar Hizmetleri'nin sınıf laboratuarlarında sanal makinelerin (VM'ler) parolalarını nasıl sıfırlayabileceğinizi öğrenin.
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
ms.date: 10/31/2019
ms.author: spelluru
ms.openlocfilehash: 7c757ef8508f9364a46116e6ddf19207f23a4b6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73583689"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs-students"></a>Sınıf laboratuarlarında (öğrenciler) sanal makineler için parola ayarlama veya sıfırlama
Bu makalede, öğrencilerin VM'leri için parolayı nasıl ayarlayabilecekleri/sıfırlayabildiği gösterilmektedir. 

## <a name="enable-resetting-of-passwords"></a>Parolaların sıfırlanmasını etkinleştirme
Laboratuarı oluştururken, laboratuvar sahibi tüm sanal makineler için **aynı parolayı kullanabilir**veya devre dışı edebilir. Bu seçenek etkinleştirildiyse, öğrenciler parolayı sıfırlayamaz. Laboratuardaki tüm VM'ler eğitmen tarafından ayarlanan şifrenin aynısı olacak. 

Bu seçenek devre dışı bırakılırsa, kullanıcıların VM'ye ilk kez bağlanmaya çalışırken parola ayarlamaları gerekir. Öğrenciler ayrıca, bu makalenin son bölümünde gösterildiği gibi parolayı daha sonra herhangi bir zamanda sıfırlayabilirler. 

## <a name="reset-password-for-the-first-time"></a>Parolayı ilk kez sıfırla
Tüm **sanal makineler için aynı parolayı kullan** seçeneği devre dışı bırakılmışsa, kullanıcılar (öğrenciler) **Sanal Makineler sayfamdaki** laboratuvar kutucuğundaki **Bağlan** düğmesini seçtiğinde, kullanıcı VM parolasını ayarlamak için aşağıdaki iletişim kutusunu görür: 

![Öğrenci için parolayı sıfırlama](../media/how-to-set-virtual-machine-passwords/student-set-password.png)

## <a name="reset-password-later"></a>Parolayı daha sonra sıfırla
Öğrenci ayrıca laboratuvar döşemesindeki taşma menüsünü **(dikey üç nokta)** tıklayarak ve **parolayı sıfırla'yı**seçerek parolayı ayarlayabilir. 

![Parolayı daha sonra sıfırla](../media/how-to-set-virtual-machine-passwords/student-set-password-2.png)


## <a name="next-steps"></a>Sonraki adımlar
Laboratuvar sahibinin yapılandırabileceği diğer öğrenci kullanım seçenekleri hakkında bilgi edinmek için aşağıdaki makaleye bakın: [Öğrenci kullanımını yapılandırın.](how-to-configure-student-usage.md)
