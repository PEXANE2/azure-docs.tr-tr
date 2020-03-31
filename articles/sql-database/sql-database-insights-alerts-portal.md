---
title: Kurulum uyarıları ve bildirimleri (Azure portalı)
description: Belirttiğiniz koşullar karşılandığında bildirimleri veya otomasyonu tetikleyebilen SQL Veritabanı uyarıları oluşturmak için Azure portalını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: jrasnik, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 67c47b35e84a93d7d9032ad55b425ae2bb6971fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79209558"
---
# <a name="create-alerts-for-azure-sql-database-and-azure-synapse-analytics-databases-using-azure-portal"></a>Azure portalLarını kullanarak Azure SQL Veritabanı ve Azure Synapse Analytics veritabanları için uyarılar oluşturma

## <a name="overview"></a>Genel Bakış

Bu makalede, Azure portalını kullanarak Azure SQL Veritabanı ve Azure Synapse Analytics'te (eski adıyla Azure SQL Veri Ambarı) tek, havuzlu ve veri ambarı veritabanları için uyarılar nasıl ayarlanabileceğiniz gösterilmektedir. Bazı ölçümler (örneğin veritabanı boyutu veya CPU kullanımı) eşiğe ulaştığında uyarılar size bir e-posta gönderebilir veya bir web kancası arayabilir. Bu makalede, uyarı dönemleri ayarlamak için en iyi uygulamaları da sağlar.

> [!IMPORTANT]
> Bu özellik yönetilen örnekte henüz kullanılamıyor. Alternatif olarak, Dinamik Yönetim Görünümleri'ni temel alan bazı [Dynamic Management Views](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)ölçümler için e-posta uyarıları göndermek için SQL Agent'ı kullanabilirsiniz.

Azure hizmetleriniz için izleme ölçümlerine veya etkinliklere dayalı bir uyarı alabilirsiniz.

* **Metrik değerler** - Belirli bir metnin değeri her iki yönde de atadığınız bir eşiği geçtiğinde uyarı tetikler. Diğer bir tarihte, hem koşul ilk karşılaştığında hem de daha sonra bu koşul yerine getirilmediğinde tetikler.
* **Etkinlik günlüğü olayları** - Bir uyarı *her* olayda veya yalnızca belirli sayıda olay meydana geldiğinde tetikleyebilir.

Bir uyarıyı tetiklediğinde aşağıdakileri yapmak üzere yapılandırabilirsiniz:

* Hizmet yöneticisine ve yardımcı yöneticilere e-posta bildirimleri gönderme
* Belirttiğiniz ek e-postalara e-posta gönderin.
* Web kancası çağırma

Kullanarak uyarı kurallarını yapılandırabilir ve hakkında bilgi alabilirsiniz

* [Azure portalında](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [Powershell](../azure-monitor/platform/alerts-classic-portal.md)
* [komut satırı arabirimi (CLI)](../azure-monitor/platform/alerts-classic-portal.md)
* [Azure İzleyici REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Azure portalı ile bir metrikte uyarı kuralı oluşturma

1. [Portalda,](https://portal.azure.com/)izlemekle ilgilendiğiniz kaynağı bulun ve seçin.
2. İzleme bölümünde **Uyarılar'ı** seçin. Metin ve simge farklı kaynaklar için biraz değişebilir.  

   ![İzleme](media/sql-database-insights-alerts-portal/Alerts.png)
  
3. **Oluştur kuralı** sayfasını açmak için Yeni **uyarı kuralı** düğmesini seçin.
  ![Kural oluşturma](media/sql-database-insights-alerts-portal/create-rule.png)

4. **Koşul** bölümünde **Ekle'yi**tıklatın.
  ![Durumu tanımlama](media/sql-database-insights-alerts-portal/create-rule.png)
5. **Yapılı sinyal mantık** sayfasında bir sinyal seçin.
  ![Sinyali](media/sql-database-insights-alerts-portal/select-signal.png)seçin.
6. **CPU yüzdesi**gibi bir sinyal seçtikten sonra, **Yapılandırma sinyali mantığı** sayfası görüntülenir.
  ![Sinyal mantığını yapılandırma](media/sql-database-insights-alerts-portal/configure-signal-logic.png)
7. Bu sayfada, bu eşik türünü, işleciyi, toplama türünü, eşik değerini, toplama taneciklerini ve değerlendirme sıklığını yapılandırın. Sonra da **Bitti**’ye tıklayın.
8. Oluştur **kuralında,** varolan bir **Eylem grubu** seçin veya yeni bir grup oluşturun. Bir eylem grubu, bir uyarı durumu oluştuğunda yapılacak eylemi tanımlamanızı sağlar.
  ![Eylem grubunu tanımlama](media/sql-database-insights-alerts-portal/action-group.png)

9. Kural için bir ad tanımlayın, isteğe bağlı bir açıklama sağlayın, kural için önem düzeyi seçin, kural oluşturma üzerine kuralı etkinleştirip etkinleştirmeyi seçin ve sonra metrik kural uyarısı oluşturmak için **kural uyarısı oluştur'u** tıklatın.

10 dakika içinde, uyarı etkin dir ve daha önce açıklandığı gibi tetiklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Uyarılardaki web hook'ları yapılandırma](../azure-monitor/platform/alerts-webhooks.md)hakkında daha fazla bilgi edinin.
