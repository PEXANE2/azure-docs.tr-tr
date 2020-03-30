---
title: Azure Laboratuvar Hizmetlerinin bağlantısını kesmede VM'lerin kapatılmasını etkinleştirin | Microsoft Dokümanlar
description: Uzak bir masaüstü bağlantısı kesildiğinde VM'lerin otomatik olarak kapatılmasını nasıl etkinleştirip devre dışı kalacaksınız öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77117140"
---
# <a name="enable-automatic-shutdown-of-vms-on-disconnect"></a>Bağlantıyı kesmede VM'lerin otomatik olarak kapatılmasını etkinleştirme
Bu makalede, uzak bir masaüstü bağlantısı kesildikten sonra **Windows 10** laboratuvar VM'lerinin (şablon veya öğrenci) otomatik olarak kapatılmasını nasıl etkinleştirebileceğinizi veya devre dışı kullanabilirsiniz. Ayrıca, otomatik olarak kapanmadan önce VM'lerin kullanıcının yeniden bağlanmasını ne kadar beklemesi gerektiğini de belirtebilirsiniz.

Bir laboratuvar hesabı yöneticisi, laboratuvar oluşturduğunuz laboratuvar hesabı için bu ayarı yapılandırabilir. Daha fazla bilgi için, [bir laboratuar hesabı için bağlantıyı kesmede VM'lerin otomatik olarak kapatılmasını yapılandırır' bölümüne](how-to-configure-lab-accounts.md#automatic-shutdown-of-vms-on-disconnect)bakın. Bir laboratuvar sahibi olarak, bir laboratuvar oluştururken veya laboratuvar oluşturulduktan sonra ayarı geçersiz kılabilirsiniz. 

## <a name="configure-when-creating-a-lab"></a>Laboratuvar oluştururken yapılandırma
Laboratuvar oluşturma sihirbazının 3. 

![Laboratuvar oluşturma sırasında yapılandırın](../media/how-to-enable-shutdown-disconnect/configure-lab-creation.png)

## <a name="configure-after-the-lab-is-created"></a>Laboratuvar oluşturulduktan sonra yapılandır
Bu ayarı Aşağıdaki resimde gösterildiği gibi **Ayarlar** sayfasında yapılandırabilirsiniz: 

![Laboratuvar oluşturulduktan sonra yapılandır](../media/how-to-enable-shutdown-disconnect/configure-lab-automatic-shutdown.png)

> [!WARNING]
> Bir RDP oturumunu VM'ye kesmeden önce Windows işletim sistemini (OS) VM'de kapattığınızda, otomatik kapatma özelliği düzgün çalışmaz.  

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Sınıf laboratuvarları için pano](use-dashboard.md)