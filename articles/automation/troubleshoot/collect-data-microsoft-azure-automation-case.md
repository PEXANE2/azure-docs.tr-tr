---
title: Microsoft Azure Otomasyonu için bir servis talebi açtığınızda toplanacak veriler| Microsoft Dokümanlar
description: Bu makalede, Microsoft Azure Desteği ile Azure Otomasyonu için bir servis talebi açmadan önce toplamanız gereken bazı bilgiler açıklanmaktadır.
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
ms.openlocfilehash: e1d5d791a58f301991819b41757b9021f6e30fc0
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679413"
---
# <a name="data-to-collect-when-you-open-a-case-for-microsoft-azure-automation"></a>Microsoft Azure Otomasyonu için bir servis talebi açtığınızda toplanan veriler

Bu makalede, Microsoft Azure Desteği ile Azure Otomasyonu için bir servis talebi açmadan önce toplamanız gereken bazı bilgiler açıklanmaktadır. Bu bilgiler servis talebinin açılması için gerekli değildir. Ancak, Microsoft'un sorununuzu daha hızlı çözmenize yardımcı olabilir. Ayrıca, servis talebi açıldıktan sonra destek mühendisi tarafından bu veriler istenebilir.

## <a name="basic-data"></a>Temel veriler

Bilgi Bankası madde [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)açıklanan temel verileri toplamak - Nasıl Azure Otomasyon komut dosyası tanılama yakalamak için .

## <a name="data-for-update-management-issues-on-linux"></a>Linux'ta Güncelleme Yönetimi sorunları için veriler

1. KB [4034605'te](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)listelenen öğelere ek olarak aşağıdaki günlük toplama aracını çalıştırın:

   [OMS Linux Agent Log Collector](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md)
 
2. **/var/opt/microsoft/omsagent/run/automationworker/klasörünün** içeriğini sıkıştırın ve sıkıştırılan dosyayı Azure Desteği'ne gönderin.
 
3. Linux için Log Analytics aracısının bildirdiği çalışma alanı kimliğinin, güncelleştirmeler için izlenen çalışma alanı kimliğiyle aynı olduğunu doğrulayın.

## <a name="data-for-update-management-issues-on-windows"></a>Windows'da Güncelleştirme Yönetimi sorunları için veriler

1. [4034605'te](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)listelenen öğeler için veri toplayın.

2. Aşağıdaki olay günlüklerini EVTX biçimine dışa aktar:

   * Sistem
   * Uygulama
   * Güvenlik
   * Operations Manager
   * Microsoft-SMA/Operasyonel

3. Aracının bildirdiği çalışma alanının kimliğinin, Windows Güncelleştirmeleri tarafından izlenen çalışma alanı kimliğiyle aynı olduğunu doğrulayın.

## <a name="data-for-job-issues"></a>İş sorunları için veriler

1. [4034605'te](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)listelenen öğeler için veri toplayın.

2. Sorunu olan iş için iş kimliğini toplayın:

   1. Azure portalında Otomasyon **Hesapları'na**gidin.
   2. Sorun giderdiğiniz Otomasyon hesabını seçin ve adını not edin.
   3. **İşler'i**seçin.
   4. Sorun giderdiğiniz işi seçin.
   5. İş Özeti bölmesinde, **İş Kimliği'ndeki**GUID değerini arayın.

   ![İş Özeti Bölmesinde İş Kimliği](media/collect-data-microsoft-azure-automation-case/job-summary-job-id.png)

3. Çalıştırdığınız komut dosyasının bir örneğini toplayın.

4. Günlük dosyalarını toplayın:

   1. Azure portalında Otomasyon **Hesapları'na**gidin.
   2. Sorun giderdiğiniz Otomasyon hesabını seçin.
   3. **İşler'i**seçin.
   4. Sorun giderdiğiniz işi seçin.
   5. **Tüm Günlükleri**seçin.
   6. Ortaya çıkan bölmede, verileri toplayın.

   ![Tüm Günlükler altında listelenen veriler](media/collect-data-microsoft-azure-automation-case/all-logs-data.png)

## <a name="data-for-module-issues"></a>Modül sorunları için veriler

Temel veri [öğelerine](#basic-data)ek olarak, aşağıdaki bilgileri toplayın:

* Sorunun yeniden üretilebilmeleri için izlediğiniz adımlar.
* Herhangi bir hata iletisinin ekran görüntüleri.
* Geçerli modüllerin ve sürüm numaralarının ekran görüntüleri.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma ihtiyacınız varsa:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport)Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
