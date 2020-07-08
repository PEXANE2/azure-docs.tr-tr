---
title: Bağlantı kesildiğinde VM 'lerin kapatılmasını etkinleştir Azure Lab Services | Microsoft Docs
description: Uzak Masaüstü bağlantısı kesildiğinde sanal makinelerin otomatik olarak kapatılmasını etkinleştirmeyi veya devre dışı bırakmayı öğrenin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 0c5c22d5e4a9d66413e37cce095f5497915bd122
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445721"
---
# <a name="enable-automatic-shutdown-of-vms-on-disconnect"></a>Bağlantı kesildiğinde VM 'lerin otomatik kapatılmasını etkinleştir
Bu makalede, Uzak Masaüstü bağlantısı kesildikten sonra **Windows 10** laboratuvar VM 'lerinin (şablon veya öğrenci) otomatik olarak kapatılmasını nasıl etkinleştirebilirim veya devre dışı bırakabilirim. Ayrıca, sanal makinelerin otomatik olarak kapanmadan önce kullanıcının yeniden bağlanmasına ne kadar süre beklemesi gerektiğini de belirtebilirsiniz.

Laboratuvar hesabı Yöneticisi, laboratuvarları oluşturduğunuz laboratuvar hesabı için bu ayarı yapılandırabilir. Daha fazla bilgi için bkz. [Laboratuvar hesabı için bağlantı kesildiğinde sanal makinelerin otomatik olarak kapatılmasını yapılandırma](how-to-configure-lab-accounts.md). Laboratuvar sahibi olarak, laboratuvar oluştururken veya laboratuvar oluşturulduktan sonra ayarı geçersiz kılabilirsiniz. 

## <a name="configure-when-creating-a-lab"></a>Laboratuvar oluştururken yapılandırma
Laboratuvar oluşturma Sihirbazı 'nın adım 3 sayfasında bu özelliği etkinleştirebilir veya devre dışı bırakabilir ve ayrıca, sanal makinenin otomatik olarak kapanmadan önce kullanıcının yeniden bağlantı kurmasını bekleyeceği süreyi de belirtebilirsiniz. 

![Laboratuvar oluşturma sırasında yapılandırma](./media/how-to-enable-shutdown-disconnect/configure-lab-creation.png)

## <a name="configure-after-the-lab-is-created"></a>Laboratuvar oluşturulduktan sonra yapılandırma
Bu ayarı, aşağıdaki görüntüde gösterildiği gibi **Ayarlar** sayfasında yapılandırabilirsiniz: 

![Laboratuvar oluşturulduktan sonra yapılandırma](./media/how-to-enable-shutdown-disconnect/configure-lab-automatic-shutdown.png)

> [!WARNING]
> VM 'ye bir RDP oturumunun bağlantısını kesmeden önce bir VM 'deki Windows işletim sistemini (OS) kapatırsanız, oto kapatma özelliği düzgün çalışmaz.  

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Sınıf laboratuvarları panosu](use-dashboard.md)