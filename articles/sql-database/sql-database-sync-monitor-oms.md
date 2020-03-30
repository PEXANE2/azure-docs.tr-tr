---
title: Azure Monitor günlükleriyle SQL Veri Eşitlemesini Izleyin
description: Azure Monitor günlüklerini kullanarak Azure SQL Veri Eşitlemesini nasıl izleyeceğinizi öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
ms.date: 12/20/2018
ms.openlocfilehash: 0ed0bd3544fff89c8230267e3d6d8826c5ae3c7c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74114606"
---
# <a name="monitor-sql-data-sync-with-azure-monitor-logs"></a>Azure Monitor günlükleriyle SQL Veri Eşitlemesini Izleyin 

SQL Veri Eşitleme etkinlik günlüğünü denetlemek ve hataları ve uyarıları algılamak için daha önce Azure portalında SQL Veri Eşitlemesini el ile denetlemeniz veya PowerShell veya REST API'sini kullanmanız gerekiyordu. Veri Eşitleme izleme deneyimini geliştiren özel bir çözümü yapılandırmak için bu makaledeki adımları izleyin. Bu çözümü senaryonuza uyacak şekilde özelleştirebilirsiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md).

> [!IMPORTANT]
> Azure SQL Veri Eşitlemesi şu anda Azure SQL Veritabanı Yönetilen Örneği'ni **desteklemez.**

## <a name="monitoring-dashboard-for-all-your-sync-groups"></a>Tüm Eşitleme Gruplarınız için İzleme Panosu 

Sorunları aramak için artık her Eşitleme Grubunun günlüklerine tek tek bakmanız gerekmez. Özel bir Azure Monitör görünümü kullanarak aboneliklerinizden tüm Eşitleme Gruplarınızı tek bir yerde izleyebilirsiniz. Bu görünüm, SQL Veri Eşitleme müşterileri için önemli olan bilgileri yüzeyler.

![Veri Eşitleme izleme panosu](media/sql-database-sync-monitor-oms/sync-monitoring-dashboard.png)

## <a name="automated-email-notifications"></a>Otomatik E-posta bildirimleri

Artık azure portalında veya PowerShell veya REST API'de günlüğü el ile denetlemeniz gerekmez. [Azure Monitor günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)ile, bir hata oluştuğunda bunları görmesi gereken kişilerin e-posta adreslerine doğrudan giden uyarılar oluşturabilirsiniz.

![Veri Eşitleme e-posta bildirimleri](media/sql-database-sync-monitor-oms/sync-email-notifications.png)

## <a name="how-do-you-set-up-these-monitoring-features"></a>Bu izleme özelliklerini nasıl ayarlıyorsun? 

Aşağıdaki şeyleri yaparak SQL Veri Eşitleme için özel bir Azure Monitor günlükleri izleme çözümünü bir saatten kısa bir sürede uygulayın:

Üç bileşeni yapılandırmanız gerekir:

-   SQL Veri Eşitleme günlük verilerini Azure Monitor günlüklerine beslemek için bir PowerShell runbook.

-   E-posta bildirimleri için Azure Monitör uyarısı.

-   İzleme için bir Azure Monitör Görünümü.

### <a name="samples-to-download"></a>İndirilen örnekler

Aşağıdaki iki örneği indirin:

-   [Veri Senkronizasyonu Günlüğü PowerShell Runbook](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Veri Senkronizasyonu Azure Monitör Görünümü](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki şeyleri ayarladığınızdan emin olun:

-   Azure Otomasyon hesabı

-   Log Analytics çalışma alanı

## <a name="powershell-runbook-to-get-sql-data-sync-log"></a>PowerShell Runbook SQL Veri Eşitleme Günlüğü almak için 

SQL Veri Eşitleme günlük verilerini çekmek ve Azure Monitor günlüklerine göndermek için Azure Automation'da barındırılan bir PowerShell runbook kullanın. Örnek bir komut dosyası dahildir. Ön koşul olarak, bir Azure Otomasyon hesabınız olması gerekir. Daha sonra bir runbook oluşturmanız ve çalıştırmak için zamanlamalısınız. 

### <a name="create-a-runbook"></a>Runbook oluşturma

Runbook oluşturma hakkında daha fazla bilgi için [ilk PowerShell runbook'uma](https://docs.microsoft.com/azure/automation/automation-first-runbook-textual-powershell)bakın.

1.  Azure Otomasyon hesabınızaltında, İşlem Otomasyonu altında **Runbook** sekmesini seçin.

2.  RunBook sayfasının sol üst köşesinde **runbook ekle'yi** seçin.

3.  **Varolan bir Runbook'u içe aktar'ı**seçin.

4.  **Runbook dosyası**altında, `DataSyncLogPowerShellRunbook` verilen dosyayı kullanın. **Runbook türünü** ' `PowerShell`olarak ayarlayın. Runbook'a bir ad ver.

5.  **Oluştur'u**seçin. Artık bir runbook'un var.

6.  Azure Otomasyon Hesabınız altında Paylaşılan Kaynaklar altında **Değişkenler** sekmesini seçin.

7.  Değişkenler sayfasında **bir değişken ekle'yi** seçin. Runbook için son yürütme saatini depolamak için bir değişken oluşturun. Birden çok runbook'unuzun varsa, her runbook için bir değişkene ihtiyacınız olur.

8.  Değişken adını olarak `DataSyncLogLastUpdatedTime` ayarlayın ve Türünü DateTime olarak ayarlayın.

9.  Runbook'u seçin ve sayfanın üst kısmındaki edit düğmesini tıklatın.

10. Hesabınız ve SQL Veri Eşitleme yapılandırmanız için gerekli değişiklikleri yapın. (Daha ayrıntılı bilgi için örnek komut dosyasına bakın.)

    1.  Azure bilgileri.

    2.  Eşitleme Grubu bilgileri.

    3.  Azure Monitor bilgileri kaydeder. Bu bilgileri Azure Portalı'nda bulabilirsiniz | Ayarlar | Bağlantılı Kaynaklar. Azure Monitor günlüklerine veri gönderme hakkında daha fazla bilgi için, [HTTP Veri Toplayıcı API 'si (önizleme) ile Azure Monitor günlüklerine veri gönder'e](../azure-monitor/platform/data-collector-api.md)bakın.

11. Çalışma kitabını Test bölmesinde çalıştırın. Başarılı olduğundan emin olun.

    Hatalarınız varsa, en son PowerShell modüllerini yüklediğinizden emin olun. En son PowerShell modüllerini Otomasyon Hesabınızdaki **Modüller Galerisi'ne** yükleyebilirsiniz.

12. **Yayımla'yı** tıklatın

### <a name="schedule-the-runbook"></a>Runbook'u zamanlama

Runbook'u zamanlamak için:

1.  Runbook'un altında, Kaynaklar altında **Zamanlamalar** sekmesini seçin.

2.  Zamanlamalar sayfasında **Zamanlama Ekle'yi** seçin.

3.  **Çalışma kitabınıza Zamanlama Bağla'yı**seçin.

4.  **Yeni bir zamanlama oluştur'u seçin.**

5.  **Yinelemeyi** Yinelemeolarak ayarlayın ve istediğiniz aralığı ayarlayın. Burada, komut dosyasında ve Azure Monitor günlüklerinde aynı aralığı kullanın.

6.  **Oluştur'u**seçin.

### <a name="check-the-automation"></a>Otomasyonu kontrol edin

Otomasyonunuzun beklendiği gibi çalışıp çalışmadığını izlemek için, otomasyon hesabınıza **genel bakış** altında, **İzleme**altında **İş İstatistikleri** görünümünü bulun. Kolay görüntüleme için bu görünümü panonuza sabitle. Runbook gösterisinin başarılı çalıştırmaları "Tamamlandı" ve Başarısız çalıştırmalar "Başarısız" olarak gösterir.

## <a name="create-an-azure-monitor-reader-alert-for-email-notifications"></a>E-posta Bildirimleri için Azure Monitörü Okuyucu Uyarısı Oluşturma

Azure Monitor günlüklerini kullanan bir uyarı oluşturmak için aşağıdaki leri yapın. Ön koşul olarak, Azure Monitor günlüklerinin bir Log Analytics çalışma alanına bağlanması gerekir.

1.  Azure portalında, **Günlük Arama'yı**seçin.

2.  Seçtiğiniz aralıkta eşitleme grubuna göre hataları ve uyarıları seçmek için bir sorgu oluşturun. Örnek:

    `DataSyncLog_CL | where LogLevel_s != "Success" | summarize AggregatedValue = count() by bin(TimeGenerated,60m),SyncGroupName_s`

3.  Sorguyu çalıştırdıktan sonra **Uyarı**yazan çanı seçin.

4.  **Dayalı Oluşturma uyarısı**altında, Metrik **Ölçümü**seçin.

    1.  Toplam Değeri **'den Büyük**olarak ayarlayın.

    2.  **Büyük'ten**sonra, bildirimleri almadan önce geçilme eşiğini girin. Veri Eşitleme'de geçici hatalar bekleniyor. Gürültüyü azaltmak için eşiği 5'e ayarlayın.

5.  **Eylemler**altında, **E-posta bildirimini** "Evet" olarak ayarlayın. İstediğine göre e-posta alıcılarını girin.

6.  **Kaydet**'e tıklayın. Belirtilen alıcılar artık hatalar oluştuğunda e-posta bildirimleri alır.

## <a name="create-an-azure-monitor-view-for-monitoring"></a>İzleme için Azure Monitör Görünümü Oluşturma

Bu adım, belirtilen tüm eşitleme gruplarını görsel olarak izlemek için bir Azure Monitor görünümü oluşturur. Görünüm birkaç bileşen içerir:

-   Tüm eşitleme gruplarının kaç hata, başarı ve uyarı olduğunu gösteren genel bir yapı.

-   Eşitleme grubu başına hata ve uyarı sayısını gösteren tüm eşitleme grupları için bir döşeme. Sorun olmayan gruplar bu döşemede görünmez.

-   Her Eşitleme Grubu için hata, başarı ve uyarı sayısını ve son hata iletilerini gösteren bir döşeme.

Azure Monitör görünümünü yapılandırmak için aşağıdaki leri yapın:

1.  Log Analytics çalışma alanı giriş sayfasında, **görünüm tasarımcısını**açmak için soldaki artıyı seçin.

2.  Görünüm tasarımcısının üst çubuğunda **İçe Aktar'ı** seçin. Ardından "DataSyncLogOMSView" örnek dosyasını seçin.

3.  Örnek görünümü iki eşitleme grubu yönetmek içindir. Senaryonuza uyacak şekilde bu görünümü edin. **Edit'i** tıklatın ve aşağıdaki değişiklikleri yapın:

    1.  Gerektiğinde Galeri'den yeni "Donut & Listesi" nesneleri oluşturun.

    2.  Her döşemede, bilgilerinizle sorguları güncelleştirin.

        1.  Her döşemede, TimeStamp_t aralığını istediğiniz gibi değiştirin.

        2.  Her Eşitleme Grubu için kutucuklarda, Eşitleme Grubu adlarını güncelleştirin.

    3.  Her döşemede, başlığı gerektiği gibi güncelleştirin.

4.  **Kaydet'i** tıklatın ve görünüm hazır.

## <a name="cost-of-this-solution"></a>Bu çözümün maliyeti

Çoğu durumda, bu çözüm ücretsizdir.

**Azure Otomasyonu:** Kullanımınıza bağlı olarak Azure Otomasyon uyrumduğunuz bir maliyet oluşabilir. Ayda ilk 500 dakikalık çalışma süresi ücretsizdir. Çoğu durumda, bu çözümün ayda 500 dakikadan az kullanılması beklenir. Ücretleri önlemek için, runbook'u iki saat veya daha uzun bir aralıkta çalışacak şekilde zamanlayın. Daha fazla bilgi için [Otomasyon fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.

**Azure Monitör günlükleri:** Kullanımınıza bağlı olarak Azure Monitor günlükleriyle ilişkili bir maliyet olabilir. Ücretsiz katman, günde 500 MB yutulan veri içerir. Çoğu durumda, bu çözümün günde 500 MB'dan daha az yutması beklenir. Kullanımı azaltmak için, runbook'ta yer alan yalnızca hata filtresini kullanın. Günde 500 MB'dan fazla kullanıyorsanız, sınırlamaya ulaşıldığında analitik lerin durma riskini önlemek için ücretli katmanı yükseltin. Daha fazla bilgi için [Azure Monitor günlükleri fiyatlandırması](https://azure.microsoft.com/pricing/details/log-analytics/)'na bakın.

## <a name="code-samples"></a>Kod örnekleri

Bu makalede açıklanan kod örneklerini aşağıdaki konumlardan indirin:

-   [Veri Senkronizasyonu Günlüğü PowerShell Runbook](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Veri Senkronizasyonu Azure Monitör Görünümü](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

## <a name="next-steps"></a>Sonraki adımlar
SQL Data Sync hakkında daha fazla bilgi için bkz.:

-   Genel Bakış - [Azure SQL Veri Eşitlemi ile verileri birden çok bulut ve şirket içi veritabanları nda eşitleme](sql-database-sync-data.md)
-   Veri Eşitlemeyi Ayarlama
    - Portalda - [Öğretici: Azure SQL Veritabanı ve SQL Server arasında verileri şirket içinde eşitlemek için SQL Veri Eşitlemeyi'ni ayarlayın](sql-database-get-started-sql-data-sync.md)
    - PowerShell ile
        -  [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Veri Eşitleme Aracısı - [Azure SQL Veri Eşitleme için Veri Eşitleme Aracısı](sql-database-data-sync-agent.md)
-   En iyi uygulamalar - [Azure SQL Veri Eşitleme için en iyi uygulamalar](sql-database-best-practices-data-sync.md)
-   Sorun Giderme - [Azure SQL Veri Eşitleme ile sorun giderme](sql-database-troubleshoot-data-sync.md)
-   Eşitleme şeasını güncelleştirme
    -   Transact-SQL ile - [Azure SQL Veri Eşitleme'deki şema değişikliklerinin çoğaltılması otomatikleştirin](sql-database-update-sync-schema.md)
    -   PowerShell ile - [Varolan bir eşitleme grubunda eşitleme şemasını güncellemek için PowerShell'i kullanın](scripts/sql-database-sync-update-schema.md)

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

-   [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
-   [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
