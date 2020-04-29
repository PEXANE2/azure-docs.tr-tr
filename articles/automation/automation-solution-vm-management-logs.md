---
title: VM 'Leri başlatma/durdurma çözümünü günlüğe sorgulama
description: Bu makalede, Azure Izleyici 'deki VM 'Leri Başlat/Durdur çözümü tarafından oluşturulan günlük verilerinin nasıl sorgulanyapılacağı açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 472f3762ca18f71ba95053576daf025d8477fee9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81604737"
---
# <a name="how-to-query-logs-from-startstop-vms-solution"></a>VM 'Leri başlatma/durdurma çözümünü günlüğe sorgulama

Azure Otomasyonu, iki kayıt türünü bağlı Log Analytics çalışma alanına iletir: iş günlükleri ve iş akışları. Bu veriler Azure Izleyici 'de [sorgu](../azure-monitor/log-query/log-query-overview.md) için kullanılabilir.

## <a name="job-logs"></a>İş günlükleri

|Özellik | Açıklama|
|----------|----------|
|Çağıran |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
|Kategori | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır.|
|CorrelationId | Runbook işinin bağıntı KIMLIĞI olan GUID.|
|JobId | Runbook işinin KIMLIĞI olan GUID.|
|operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer Iş olur.|
|resourceId | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
|ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
|ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|resultType | Runbook işinin durumudur. Olası değerler şunlardır:<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>- Başarılı oldu|
|resultDescription | Runbook iş sonucu durumunu açıklar. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı|
|RunbookName | Runbook’un adını belirtir.|
|SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için, değer OpsManager ' dır|
|StreamType | Olay türünü belirtir. Olası değerler şunlardır:<br>- Ayrıntılı<br>- Çıktı<br>- Hata<br>- Uyarı|
|SubscriptionId | İşin abonelik kimliğini belirtir.
|Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

## <a name="job-streams"></a>İş akışları

|Özellik | Açıklama|
|----------|----------|
|Çağıran |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
|Kategori | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır.|
|JobId | Runbook işinin KIMLIĞI olan GUID.|
|operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer Iş olur.|
|ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
|resourceId | Azure 'da kaynak KIMLIĞINI belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
|ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|resultType | Runbook işinin, olayın oluşturulduğu andaki sonucu. Olası bir değer:<br>- Devam Ediyor|
|resultDescription | Runbook’un çıktı akışını içerir.|
|RunbookName | Runbook’un adı.|
|SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için, değer OpsManager ' dır.|
|StreamType | İş akışı türü. Olası değerler şunlardır:<br>-İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı|
|Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

**Joblogs** veya **jobstreams**kategorisi kayıtlarını döndüren bir günlük araması gerçekleştirdiğinizde, arama tarafından döndürülen güncelleştirmeleri özetleyen bir kutucuk kümesi görüntüleyen **joblogs** veya **jobstreams** görünümünü seçebilirsiniz.

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tabloda, bu çözüm tarafından toplanan iş kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır.

|Sorgu | Açıklama|
|----------|----------|
|Başarıyla tamamlanan runbook ScheduledStartStop_Parent işleri bulun | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Runbook ScheduledStartStop_Parent başarıyla tamamlanmamış olan işleri bulun | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Başarıyla tamamlanan runbook SequencedStartStop_Parent işleri bulun | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Runbook SequencedStartStop_Parent başarıyla tamamlanmamış olan işleri bulun | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="next-steps"></a>Sonraki adımlar

**VM'leri çalışma saatleri dışında Başlat/Durdur** çözümü, önceden tanımlanmış bir uyarı kümesi içermez. DevOps veya işletimsel işlemlerinizi ve yordamlarını desteklemek üzere iş başarısız uyarıları oluşturmayı öğrenmek için Azure Izleyici ile [günlük uyarılarını oluştur](../azure-monitor/platform/alerts-log.md) ' a bakın.