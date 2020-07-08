---
title: Değişiklik İzleme ve envanter için çalışma alanının Otomasyon hesabından bağlantısını kaldırma
description: Bu makalede, Değişiklik İzleme ve envanter için Log Analytics bir çalışma alanının Otomasyon hesabından nasıl bağlantısının yapılacağı açıklanmaktadır
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 2be702ec6e820fe71dd8d2da7aa4cf831b52402e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83828262"
---
# <a name="unlink-workspace-from-automation-account"></a>Çalışma alanının Otomasyon hesabından bağlantısını kaldırma

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

* Değişiklik İzleme ve envanterle çalışmak için bkz. [Manage değişiklik izleme and Inventory](change-tracking-file-contents.md).
* Genel özellik sorunlarını gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md).
