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
ms.openlocfilehash: 68a27a325a0ef02c6eeea9867a21ba0e24ab5321
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77117140"
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

> [!WARNING]
> VM 'ye bir RDP oturumunun bağlantısını kesmeden önce bir VM 'deki Windows işletim sistemini (OS) kapatırsanız, oto kapatma özelliği düzgün çalışmaz.  

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Sınıf laboratuvarları panosu](use-dashboard.md)