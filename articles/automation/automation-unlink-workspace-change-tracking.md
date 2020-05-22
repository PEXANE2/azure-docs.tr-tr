---
title: Değişiklik İzleme ve envanter için çalışma alanının Otomasyon hesabından bağlantısını kaldırma
description: Bu makalede, Değişiklik İzleme ve envanter için Log Analytics bir çalışma alanının Otomasyon hesabından nasıl bağlantısının yapılacağı açıklanmaktadır
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: df2de44c2c8831fa4319b80484a119052434f8fb
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83749112"
---
# <a name="unlink-workspace-from-automation-account-for-change-tracking-and-inventory"></a>Değişiklik İzleme ve envanter için çalışma alanının Otomasyon hesabından bağlantısını kaldırma

[Değişiklik izleme ve envanter](change-tracking.md) Işlemlerini etkinleştirirken Otomasyon hesabınızı bir Log Analytics çalışma alanıyla tümleştirmemeye karar verebilirsiniz. Bu makalede, çalışma alanının hesabınızdaki bağlantısını kaldırmanız açıklanmaktadır.

1. Adresinden Azure 'da oturum açın https://portal.azure.com .

2. Sanal makinelerinize Güncelleştirme Yönetimi kaldırın. Bkz. [değişiklik izleme ve envanterden VM 'Leri kaldırma](automation-remove-vms-from-change-tracking.md).

3. Değişiklik İzleme ve Inventory Azure SQL izlemenin önceki sürümlerini içeriyorsa, özelliğin kurulumu, kaldırmanız gereken Otomasyon varlıklarını oluşturmuş olabilir. Bu varlıkları bulun ve kaldırın.

4. Otomasyon hesabınızı açın ve sol taraftaki **Ilgili kaynaklar** altında **bağlantılı çalışma alanı** ' nı seçin.

5. Çalışma alanının bağlantısını Kaldır sayfasında, **çalışma alanının bağlantısını kaldır** ' a tıklayın ve istemlere yanıtlayın.

   ![Çalışma alanının bağlantısını Kaldır sayfası](media/automation-unlink-workspace-change-tracking/automation-unlink-workspace-blade.png).

6. Azure Otomasyonu Log Analytics çalışma alanının bağlantısını kaldırmayı denediğinde, ilerleme durumunu menüdeki **Bildirimler** bölümünden izleyebilirsiniz.

Alternatif olarak, Log Analytics çalışma alanınızın, çalışma alanının içinden Otomasyon hesabınızla bağlantısını kaldırabilirsiniz:

1. Çalışma alanınızda **Ilgili kaynaklar**altında **Otomasyon hesabı** ' nı seçin. 
2. Otomasyon hesabı sayfasında **Hesap bağlantısını kaldır**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Değişiklik İzleme ve stoku yönetme](change-tracking-file-contents.md)
* [Değişiklik İzleme ve envanterden VM 'Leri kaldırma](automation-remove-vms-from-change-tracking.md)
* [Azure VM 'deki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)
* [Değişiklik İzleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md)
