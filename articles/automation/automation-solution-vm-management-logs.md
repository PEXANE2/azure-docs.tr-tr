---
title: Başlat/Durdur VM çözümünden günlükleri sorgulama
description: Bu makalede, Azure Monitor'dan Başlat/Durdur VM çözümü tarafından oluşturulan günlük verilerinin nasıl sorgulanır.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 00f6a5f88a66f0a19943ff3a2c722ae1a9938e9e
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80550402"
---
# <a name="how-to-query-logs-from-startstop-vms-solution"></a>Başlat/Durdur VM çözümünden günlükleri sorgulama

Azure Otomasyonu, iki tür kaydı bağlantılı Log Analytics çalışma alanına ilşertır: iş günlükleri ve iş akışları. Bu [veriler, Azure](../azure-monitor/log-query/log-query-overview.md) Monitor'da sorgu için kullanılabilir.

## <a name="job-logs"></a>İş günlükleri

|Özellik | Açıklama|
|----------|----------|
|Çağıran |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
|Kategori | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır.|
|CorrelationId | GUID, runbook işinin Korelasyon Kimliği'dir.|
|JobId | GUID bu runbook iş kimliğidir.|
|operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer İş'tir.|
|resourceId | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
|ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
|ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|resultType | Runbook işinin durumudur. Olası değerler şunlardır:<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>- Başarılı oldu|
|resultDescription | Runbook iş sonucu durumunu açıklar. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı|
|RunbookName | Runbook’un adını belirtir.|
|SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için değer OpsManager'dur|
|StreamType | Olay türünü belirtir. Olası değerler şunlardır:<br>- Ayrıntılı<br>- Çıktı<br>- Hata<br>- Uyarı|
|SubscriptionId | İşin abonelik kimliğini belirtir.
|Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

## <a name="job-streams"></a>İş akışları

|Özellik | Açıklama|
|----------|----------|
|Çağıran |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
|Kategori | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır.|
|JobId | GUID bu runbook iş kimliğidir.|
|operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer İş'tir.|
|ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
|resourceId | Azure'daki kaynak kimliğini belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
|ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|resultType | Runbook işinin, olayın oluşturulduğu andaki sonucu. Olası bir değer:<br>- Devam Ediyor|
|resultDescription | Runbook’un çıktı akışını içerir.|
|RunbookName | Runbook’un adı.|
|SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için değer OpsManager'dur.|
|StreamType | İş akışı türü. Olası değerler şunlardır:<br>- İlerleme<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı|
|Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

**JobLogs** veya **JobStreams**kategori kayıtlarını döndüren herhangi bir günlük araması yaptığınızda, arama tarafından döndürülen güncelleştirmeleri özetleyen bir kutucuk kümesini görüntüleyen **JobLogs** veya **JobStreams** görünümünü seçebilirsiniz.

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tabloda, bu çözüm tarafından toplanan iş kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır.

|Sorgu | Açıklama|
|----------|----------|
|Başarılı bir şekilde tamamlanan runbook ScheduledStartStop_Parent için iş bulma | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Başarılı bir şekilde tamamlanmamış runbook ScheduledStartStop_Parent için iş bulma | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Başarılı bir şekilde tamamlanan runbook SequencedStartStop_Parent için iş bulma | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Başarılı bir şekilde tamamlanmamış runbook SequencedStartStop_Parent için iş bulma | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="next-steps"></a>Sonraki adımlar

Mesai saatleri dışında Başlat/Durdur'un çözümü önceden tanımlanmış bir uyarı kümesi içermez. DevOps veya operasyonel süreç ve yordamlarınızı desteklemek için iş başarısız uyarıları nasıl oluşturup oluşturup oluşturmayı öğrenmek için Azure Monitor ile [günlük oluştur uyarılarını](../azure-monitor/platform/alerts-log.md) gözden geçirin.