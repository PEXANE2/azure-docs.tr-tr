---
title: Microsoft Azure Otomasyonu için bir servis talebi açarken toplanacak veriler | Microsoft Docs
description: Bu makalede, Azure Otomasyonu için Microsoft Azure desteğiyle bir servis talebi açmadan önce toplanacak bilgiler açıklanmaktadır.
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
ms.openlocfilehash: d2f97d0b889186324aef9613847e3eddbfe1eb02
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83684852"
---
# <a name="data-to-collect-when-opening-a-case-for-microsoft-azure-automation"></a>Microsoft Azure Otomasyonu için bir servis talebi açarken toplanacak veriler

Bu makalede, Azure Otomasyonu için Microsoft Azure desteğiyle bir servis talebi açmadan önce toplamanız gereken bazı bilgiler açıklanmaktadır. Bu bilgiler, durumu açmak için gerekli değildir. Ancak, Microsoft 'un sorununuzu daha hızlı çözümlemesine yardımcı olabilir. Ayrıca, büyük/küçük harf 'i açtıktan sonra bu veriler destek mühendisi tarafından istenebilir.

## <a name="basic-data"></a>Temel veriler

Bilgi Bankası makalesi [4034605-Azure Otomasyonu ile komut dosyalı tanılamayı yakalama](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)bölümünde açıklanan temel verileri toplayın.

## <a name="data-for-update-management-issues-on-linux"></a>Linux üzerinde Güncelleştirme Yönetimi sorunları için veriler

1. KB [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)' de listelenen öğelere ek olarak, aşağıdaki günlük toplama aracını çalıştırın:

   [OMS Linux Aracısı günlük toplayıcısı](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md)
 
2. **/Var/seçenek/Microsoft/omsagent/Run/automationworker/** Folder içeriğini sıkıştırın ve ardından sıkıştırılmış dosyayı Azure desteği 'ne gönderin.
 
3. Linux raporlarının Log Analytics aracısına ait KIMLIğIN, güncelleştirmeler için izlenmekte olan çalışma alanının KIMLIĞIYLE aynı olduğunu doğrulayın.

## <a name="data-for-update-management-issues-on-windows"></a>Windows üzerinde Güncelleştirme Yönetimi sorunları için veriler

1. [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)' de listelenen öğeler için veri toplayın.

2. Aşağıdaki olay günlüklerini EVTX biçiminde dışarı aktarın:

   * Sistem
   * Uygulama
   * Güvenlik
   * Operations Manager
   * Microsoft-SMA/operasyonel

3. Aracının rapor aldığı çalışma alanının KIMLIĞININ, Windows güncelleştirmeleri tarafından izlenmekte olan çalışma alanı KIMLIĞIYLE aynı olduğunu doğrulayın.

## <a name="data-for-job-issues"></a>İş sorunları için veriler

1. [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics)' de listelenen öğeler için veri toplayın.

2. Bir sorunu olan iş için iş KIMLIĞINI toplayın:

   1. Azure portal **Otomasyon hesapları**' na gidin.
   2. Sorun gidermekte olduğunuz Otomasyon hesabını seçin ve adı unutmayın.
   3. **İşleri**seçin.
   4. Sorun gidermekte olduğunuz işi seçin.
   5. Iş Özeti bölmesinde **Iş kimliği**' nde GUID değeri ' ni arayın.

   ![Iş Özeti bölmesi içindeki iş KIMLIĞI](media/collect-data-microsoft-azure-automation-case/job-summary-job-id.png)

3. Çalıştırdığınız betiğin bir örneğini toplayın.

4. Günlük dosyalarını toplayın:

   1. Azure portal **Otomasyon hesapları**' na gidin.
   2. Sorun gidermekte olduğunuz Otomasyon hesabını seçin.
   3. **İşleri**seçin.
   4. Sorun gidermekte olduğunuz işi seçin.
   5. **Tüm Günlükler**' i seçin.
   6. Sonuç bölmesinde, verileri toplayın.

   ![Tüm günlüklerde listelenen veriler](media/collect-data-microsoft-azure-automation-case/all-logs-data.png)

## <a name="data-for-module-issues"></a>Modül sorunlarıyla ilgili veriler

[Temel veri öğelerine](#basic-data)ek olarak, aşağıdaki bilgileri toplayın:

* Sorunun yeniden üretibilmesini sağlamak için takip ettiğiniz adımlar.
* Herhangi bir hata iletisinin ekran görüntüleri.
* Geçerli modüllerin ve bunların sürüm numaralarının ekran görüntüleri.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma ihtiyacınız varsa:

* Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyası oluşturma. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
