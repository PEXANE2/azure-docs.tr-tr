---
title: Azure Laboratuvar Hizmetleri'ndeki bir laboratuvar için pazar yeri görüntülerini belirtin
description: Bu makalede, laboratuvar oluşturucusu Azure Lab Hizmetleri'ndeki bir laboratuvar hesabında laboratuvar oluşturmak için kullanabileceği Market görüntülerini nasıl belirtebileceğiniz gösterilmektedir.
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
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: ad56041f853d030e3a286610fe4872bffecaee12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444673"
---
# <a name="specify-marketplace-images-available-to-lab-creators"></a>Laboratuvar oluşturucuları tarafından kullanılabilen Market görüntülerini belirtin
Laboratuvar sahibi olarak laboratuvar oluşturucuların laboratuvar hesabında laboratuvar oluşturmak için kullanacağı Market görüntülerini belirtebilirsiniz. 

## <a name="select-images-available-for-labs"></a>Laboratuvarlar için kullanılabilir görüntüleri seçin
Sol taraftaki menüden **Market görüntüleri**'ni seçin. Listede varsayılan olarak tüm görüntüler (hem etkin hem devre dışı) yer alır. Listenin en üstündeki açılır listeden **yalnızca**/**Devre dışı Bırakılmış** seçeneğini seçerek yalnızca etkin/devre dışı bırakılmış görüntüleri görmek için listeye filtre uygulayabilirsiniz. 
    
![Market görüntüleri sayfası](../media/tutorial-setup-lab-account/marketplace-images-page.png)

Listede görüntülenen Market görüntüleri yalnızca aşağıdaki koşulları karşılayan görüntülerdir:
    
- Tek bir VM oluşturur.
- VM'leri sağlamak için Azure Resource Manager'ı kullanır
- Ek lisans planı satın alınmasını gerektirmez

## <a name="disable-images-for-a-lab"></a>Laboratuvar için görüntüleri devre dışı 
Bir laboratuvar için tek bir görüntüyü devre dışı kayırmak için **... (elips)** son sütunda ve **görüntüyü devre dışı tonuyla**seçin. 

![Tek bir görüntüyü devre dışı bırakma](../media/tutorial-setup-lab-account/disable-one-image.png) 

Alternatif olarak, resim adından önce onay kutusunu seçin ve araç çubuğundaki **seçili görüntüleri devre dışı** kaldırın'ı seçin. 

Aynı anda birden çok görüntüyü devre dışı kılmış için, resim adlarının önünde onay kutularını seçin ve araç çubuğundaki **seçili görüntüleri devre dışı kaldırın'ı** seçin. 

![Birden fazla görüntüyü devre dışı bırakma](../media/tutorial-setup-lab-account/disable-multiple-images.png) 


## <a name="enable-images-for-a-lab"></a>Laboratuvar için görüntüleri etkinleştirme
Devre dışı bırakılmış bir görüntüyü etkinleştirmek için **, ... (elips)** son sütunda ve **görüntüyü etkinleştir'i**seçin. Alternatif olarak, resim adından önce onay kutusunu seçin ve araç çubuğunda **seçili görüntüleri etkinleştir'i** seçin. 

Aynı anda birden çok görüntüyü devre dışı kılmış olmak için, resim adlarının önünde onay kutularını seçin ve araç çubuğunda **seçili görüntüleri etkinleştir'i** seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar sahibi olarak, laboratuvarlar oluşturun ve yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak, bir laboratuvarın kullanımını yapılandırın ve kontrol edin](how-to-configure-student-usage.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)
