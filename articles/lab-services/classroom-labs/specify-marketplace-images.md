---
title: Azure Lab Services bir laboratuvarın Market görüntülerini belirtin
description: Bu makalede, laboratuvar Oluşturucu 'nun Azure Lab Services laboratuvar hesabında laboratuvarları oluşturmak için kullanabileceği Market görüntülerinin nasıl ayarlanacağı gösterilmektedir.
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
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444673"
---
# <a name="specify-marketplace-images-available-to-lab-creators"></a>Laboratuvar oluşturucuları için kullanılabilen Market görüntülerini belirtme
Laboratuvar sahibi olarak laboratuvar oluşturucuların laboratuvar hesabında laboratuvar oluşturmak için kullanacağı Market görüntülerini belirtebilirsiniz. 

## <a name="select-images-available-for-labs"></a>Labs için kullanılabilen görüntüleri seçme
Sol taraftaki menüden **Market görüntüleri**'ni seçin. Listede varsayılan olarak tüm görüntüler (hem etkin hem devre dışı) yer alır. Yalnızca etkin/devre dışı görüntüleri görmek için en üstteki açılan listeden **Yalnızca etkin**/**Yalnızca devre dışı** seçeneğini belirleyerek listeyi filtreleyebilirsiniz. 
    
![Market görüntüleri sayfası](../media/tutorial-setup-lab-account/marketplace-images-page.png)

Listede görüntülenen Market görüntüleri yalnızca aşağıdaki koşulları karşılayan görüntülerdir:
    
- Tek bir VM oluşturur.
- VM'leri sağlamak için Azure Resource Manager'ı kullanır
- Ek lisans planı satın alınmasını gerektirmez

## <a name="disable-images-for-a-lab"></a>Laboratuvar için görüntüleri devre dışı bırakma 
Bir laboratuvarın tek bir görüntüsünü devre dışı bırakmak için... seçeneğini belirleyin **. (üç nokta)** Son sütunda ve **görüntüyü devre dışı bırak**' ı seçin. 

![Tek bir görüntüyü devre dışı bırakma](../media/tutorial-setup-lab-account/disable-one-image.png) 

Alternatif olarak, görüntü adından önce onay kutusunu işaretleyip araç çubuğunda **Seçili görüntüleri devre dışı bırak** ' ı seçebilirsiniz. 

Aynı anda birden çok görüntüyü devre dışı bırakmak için, görüntü adlarından önce onay kutularını seçin ve araç çubuğunda **Seçili görüntüleri devre dışı bırak** ' ı seçin. 

![Birden fazla görüntüyü devre dışı bırakma](../media/tutorial-setup-lab-account/disable-multiple-images.png) 


## <a name="enable-images-for-a-lab"></a>Laboratuvar için görüntüleri etkinleştirme
Devre dışı bırakılmış bir görüntüyü etkinleştirmek için... seçeneğini belirleyin **. (üç nokta)** Son sütunda ve **görüntüyü etkinleştir**' i seçin. Alternatif olarak, görüntü adından önce onay kutusunu işaretleyip araç çubuğunda **Seçili görüntüleri etkinleştir** ' i seçin. 

Aynı anda birden çok görüntüyü devre dışı bırakmak için, görüntü adlarından önce onay kutularını seçin ve araç çubuğunda **Seçili görüntüleri etkinleştir** ' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar sahibi olarak, Labs oluşturma ve yönetme](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak, şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak, bir laboratuvarın kullanımını yapılandırma ve denetleme](how-to-configure-student-usage.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)
