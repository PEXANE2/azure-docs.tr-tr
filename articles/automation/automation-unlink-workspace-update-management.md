---
title: Güncelleştirme Yönetimi için çalışma alanının Otomasyon hesabından bağlantısını kaldır
description: Bu makalede, bir Log Analytics çalışma alanının Otomasyon hesabından Güncelleştirme Yönetimi için nasıl bağlantısını nasıl bir şekilde kaldırmak açıklanmaktadır
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 4261b3c3fa7aab830f5f57e86ee25f8ba5894849
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83749070"
---
# <a name="unlink-workspace-from-automation-account-for-update-management"></a>Güncelleştirme Yönetimi için çalışma alanının Otomasyon hesabından bağlantısını kaldır

[Güncelleştirme yönetimi](automation-update-management.md) Işlemleri sırasında otomasyon hesabınızı bir Log Analytics çalışma alanıyla tümleştirmemeye karar verebilirsiniz. Bu makalede, çalışma alanının hesabınızdaki bağlantısını kaldırmanız açıklanmaktadır.

1. Adresinden Azure 'da oturum açın https://portal.azure.com .

2. Sanal makinelerinize Güncelleştirme Yönetimi kaldırın. Bkz. [güncelleştirme yönetimi VM 'Leri kaldırma](automation-remove-vms-from-update-management.md).

3. Güncelleştirme Yönetimi Azure SQL izlemenin önceki sürümlerini içeriyorsa, özelliğin kurulumu, kaldırmanız gereken Otomasyon varlıklarını oluşturmuş olabilir. Güncelleştirme Yönetimi için, artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz:

   * Zamanlamayı Güncelleştir-her birinin, oluşturduğunuz güncelleştirme dağıtımıyla eşleşen bir adı vardır.
   * Güncelleştirme Yönetimi için oluşturulan karma çalışan grupları-her biri, machine1. contoso. com_9ceb8108-26c9-4051-B6B3-227600d715c8 ' e benzer şekilde adlandırılır.

4. Otomasyon hesabınızı açın ve sol taraftaki **Ilgili kaynaklar** altında **bağlantılı çalışma alanı** ' nı seçin.

5. Çalışma alanının bağlantısını Kaldır sayfasında, **çalışma alanının bağlantısını kaldır** ' a tıklayın ve istemlere yanıtlayın.

   ![Çalışma alanının bağlantısını Kaldır sayfası](media/automation-unlink-workspace-update-management/automation-unlink-workspace-blade.png).

6. Azure Otomasyonu Log Analytics çalışma alanının bağlantısını kaldırmayı denediğinde, ilerleme durumunu menüdeki **Bildirimler** bölümünden izleyebilirsiniz.

Alternatif olarak, Log Analytics çalışma alanınızın, çalışma alanının içinden Otomasyon hesabınızla bağlantısını kaldırabilirsiniz:

1. Çalışma alanınızda **Ilgili kaynaklar**altında **Otomasyon hesabı** ' nı seçin. 
2. Otomasyon hesabı sayfasında **Hesap bağlantısını kaldır**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md)
* [Güncelleştirme Yönetimi VM 'Leri kaldırma](automation-remove-vms-from-update-management.md)
* [Güncelleştirme Yönetimi sorunlarını giderme](troubleshoot/update-management.md)
