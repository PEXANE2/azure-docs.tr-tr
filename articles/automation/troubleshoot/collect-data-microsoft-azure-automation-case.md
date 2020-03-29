---
title: Microsoft Azure Otomasyonu için bir servis talebi açtığınızda toplanacak veriler| Microsoft Dokümanlar
description: Bu makalede, Microsoft Azure Desteği ile Azure Otomasyonu için bir servis talebi açmadan önce toplamanız gereken bazı temel bilgiler açıklanmaktadır.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/23/2019
ms.author: v-miegge
ms.openlocfilehash: 4839ce7a0188c782656fd3a4c42cbdd116b165e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74849386"
---
# <a name="data-to-collect-when-you-open-a-case-for-microsoft-azure-automation"></a>Microsoft Azure Otomasyonu için bir servis talebi açtığınızda toplanan veriler

Bu makalede, Microsoft Azure Desteği ile Azure Otomasyonu için bir servis talebi açmadan önce toplamanız gereken bazı temel bilgiler açıklanmaktadır. Bu bilgiler servis talebinin açılması için gerekli değildir. Ancak, Microsoft'un sorununuzu daha hızlı çözmenize yardımcı olabilir. Ayrıca, servis talebi açıldıktan sonra destek mühendisi tarafından bu veriler istenebilir.

## <a name="collect-more-information"></a>Daha fazla bilgi toplayın

Microsoft Azure Otomasyon Desteği için bir servis talebi açmadan önce aşağıdaki bilgileri toplamanızı öneririz.

### <a name="basic-data-collection"></a>Temel veri toplama

Aşağıdaki Bilgi Bankası makalesinde açıklanan verileri toplayın:

* [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics) Azure Otomasyon komut dosyası yla yazılmış tanılama nasıl yakalanır?

## <a name="collect-data-depending-on-issue"></a>Soruna bağlı olarak veri toplama
 
### <a name="for-issues-that-affect-update-management-on-linux"></a>Linux'ta Güncelleme Yönetimini etkileyen sorunlar için

1. KB [4034605'te](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)listelenen öğelere ek olarak aşağıdaki günlük toplama aracını çalıştırın:

   [OMS Linux Agent Log Collector](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md)
 
2. Aşağıdaki klasörün içeriğini sıkıştırın ve sıkıştırılmış dosyayı Azure Desteği'ne gönderin:

   ``/var/opt/microsoft/omsagent/run/automationworker/``
 
3. OMS Linux Aracısı'nın bildirdiği çalışma alanı kimliğinin güncelleştirmeler için izlenen çalışma alanıyla aynı olduğunu doğrulayın.

### <a name="for-issues-that-affect-update-management-on-windows"></a>Windows'da Güncelleştirme Yönetimi'ni etkileyen sorunlar için

[4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)listelenen öğelere ek olarak, aşağıdakileri yapın:

1. Aşağıdaki olay günlüklerini EVTX biçimine dışa aktar:

   * Sistem
   * Uygulama
   * Güvenlik
   * Operations Manager
   * Microsoft-SMA/Operasyonel

2. Aracının bildirdiği çalışma alanı kimliğinin, Windows Güncelleştirmeleri tarafından izlenen çalışma alanıyla aynı olduğunu doğrulayın.

### <a name="for-issues-that-affect-a-job"></a>Bir işi etkileyen sorunlar için

[4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)listelenen öğelere ek olarak, aşağıdaki bilgileri toplamak:

1. **JobID** numarasını (sorun yaşayan iş için) toplayın:

   1. Azure portalında **Otomasyon Hesapları** bıçağına gidin.
   2. Sorun giderdiğiniz **Otomasyon Hesabı'nı** seçin.
   3. **İşler'i**seçin.
   4. Sorun giderdiğiniz işi seçin.
   5. **İş Özeti**altında, **İş Kimliği** (GUID) arayın.

   ![İş Özeti Bölmesinde İş Kimliği](media/collect-data-microsoft-azure-automation-case/job-summary-job-id.png)

2. Hesap adını toplayın:

   1. Azure portalında **Otomasyon Hesapları** bıçağına gidin.
   2. Sorun giderdiğiniz **Otomasyon Hesabı'nın** adını alın.

3. Çalıştırdığınız komut dosyasının örneğini toplayın.

4. Günlük dosyalarını toplayın:

   1. Azure portalında **Otomasyon Hesapları** bıçağına gidin.
   2. Sorun giderdiğiniz **Otomasyon Hesabı'nı** seçin.
   3. **İşler'i**seçin.
   4. Sorun giderdiğiniz işi seçin.
   5. **Tüm Günlükleri**seçin.
   6. Ortaya çıkan bıçak üzerinde, verileri toplamak.

   ![Tüm Günlükler altında listelenen veriler](media/collect-data-microsoft-azure-automation-case/all-logs-data.png)

### <a name="for-issues-that-affect-modules"></a>Modülleri etkileyen sorunlar için

"Temel veri toplama" altındaki öğelere ek olarak, aşağıdaki bilgileri toplayın:

* Sorunun yeniden üretilebilmeleri için izlediğiniz adımlar.
* Herhangi bir hata iletilerinin ekran görüntüsü.
* Geçerli modüllerin ve sürüm numaralarının ekran görüntüsü.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarına başvurun.

Alternatif olarak, bir Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
