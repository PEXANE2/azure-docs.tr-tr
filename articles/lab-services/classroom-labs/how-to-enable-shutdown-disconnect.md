---
title: Bağlantı kesildiğinde VM 'lerin kapatılmasını etkinleştir Azure Lab Services | Microsoft Docs
description: Uzak Masaüstü bağlantısı kesildiğinde sanal makinelerin otomatik olarak kapatılmasını etkinleştirmeyi veya devre dışı bırakmayı öğrenin.
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
ms.date: 12/13/2019
ms.author: spelluru
ms.openlocfilehash: e615170952ea2987639a0bfc269ad5a1692e1e59
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75480807"
---
# <a name="enable-automatic-shutdown-of-vms-on-disconnect"></a>Bağlantı kesildiğinde VM 'lerin otomatik kapatılmasını etkinleştir
Bu makalede, Uzak Masaüstü bağlantısı kesildikten sonra **Windows 10** laboratuvar VM 'lerinin (şablon veya öğrenci) otomatik olarak kapatılmasını nasıl etkinleştirebilirim veya devre dışı bırakabilirim. Ayrıca, sanal makinelerin otomatik olarak kapanmadan önce kullanıcının yeniden bağlanmasına ne kadar süre beklemesi gerektiğini de belirtebilirsiniz.

Laboratuvar hesabı Yöneticisi, laboratuvarları oluşturduğunuz laboratuvar hesabı için bu ayarı yapılandırabilir. Daha fazla bilgi için bkz. [Laboratuvar hesabı için bağlantı kesildiğinde sanal makinelerin otomatik olarak kapatılmasını yapılandırma](how-to-configure-lab-accounts.md#automatic-shutdown-of-vms-on-disconnect). Laboratuvar sahibi olarak, laboratuvar oluştururken veya laboratuvar oluşturulduktan sonra ayarı geçersiz kılabilirsiniz. 

## <a name="configure-when-creating-a-lab"></a>Laboratuvar oluştururken yapılandırma
Laboratuvar oluşturma Sihirbazı 'nın adım 3 sayfasında bu özelliği etkinleştirebilir veya devre dışı bırakabilir ve ayrıca, sanal makinenin otomatik olarak kapanmadan önce kullanıcının yeniden bağlantı kurmasını bekleyeceği süreyi de belirtebilirsiniz. 

![Laboratuvar oluşturma sırasında yapılandırma](../media/how-to-enable-shutdown-disconnect/configure-lab-creation.png)

## <a name="configure-after-the-lab-is-created"></a>Laboratuvar oluşturulduktan sonra yapılandırma
Bu ayarı, aşağıdaki görüntüde gösterildiği gibi **Ayarlar** sayfasında yapılandırabilirsiniz: 

![Laboratuvar oluşturulduktan sonra yapılandırma](../media/how-to-enable-shutdown-disconnect/configure-lab-automatic-shutdown.png)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Sınıf laboratuvarları panosu](use-dashboard.md)