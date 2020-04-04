---
title: Yönetilen Örnek için kurulum uyarıları ve bildirimleri (Azure portalı)
description: Belirttiğiniz koşullar karşılandığında bildirimleri veya otomasyonu tetikleyebilen SQL Yönetilen Örnek uyarıları oluşturmak için Azure portalını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 04/02/2020
ms.openlocfilehash: a332627d149a36ba5d5beb2626023e58a221f0d6
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639204"
---
# <a name="create-alerts-for-azure-sql-managed-instance-using-the-azure-portal"></a>Azure portalını kullanarak Azure SQL Yönetilen Örnek için uyarılar oluşturma

## <a name="overview"></a>Genel Bakış

Bu makalede, Azure portalını kullanarak Azure SQL Yönetilen Örnek Veritabanı'ndaki veritabanları için uyarılar nasıl ayarlayabileceğiniz gösterilmektedir. Bazı ölçümler (örneğin depolama alanı boyutu veya CPU kullanımı) eşiğe ulaştığında uyarılar size bir e-posta gönderebilir veya bir web kancası arayabilir. Bu makalede, uyarı dönemleri ayarlamak için en iyi uygulamaları da sağlar.

Azure hizmetleriniz için izleme ölçümlerine veya etkinliklere dayalı bir uyarı alabilirsiniz.

* **Metrik değerler** - Belirli bir metnin değeri her iki yönde de atadığınız bir eşiği geçtiğinde uyarı tetikler. Diğer bir tarihte, hem koşul ilk karşılaştığında hem de daha sonra bu koşul yerine getirilmediğinde tetikler.
* **Etkinlik günlüğü olayları** - Bir uyarı *her* olayda veya yalnızca belirli sayıda olay meydana geldiğinde tetikleyebilir.

Bir uyarıyı tetiklediğinde aşağıdakileri yapmak üzere yapılandırabilirsiniz:

* Hizmet yöneticisine ve yardımcı yöneticilere e-posta bildirimleri gönderme
* Belirttiğiniz ek e-postalara e-posta gönderin.
* Web kancası çağırma

Kullanarak uyarı kurallarını yapılandırabilir ve hakkında bilgi alabilirsiniz

* [Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../azure-monitor/platform/alerts-classic-portal.md)
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
   ![Sinyali seçin](media/sql-database-insights-alerts-portal/select-signal.png)
6. **CPU yüzdesi**gibi bir sinyal seçtikten sonra, **Yapılandırma sinyali mantığı** sayfası görüntülenir.
   ![Sinyal mantığını yapılandırma](media/sql-database-insights-alerts-portal/configure-signal-logic.png)
7. Bu sayfada, bu eşik türünü, işleciyi, toplama türünü, eşik değerini, toplama taneciklerini ve değerlendirme sıklığını yapılandırın. Sonra da **Bitti**’ye tıklayın.
8. Oluştur **kuralında,** varolan bir **Eylem grubu** seçin veya yeni bir grup oluşturun. Bir eylem grubu, bir uyarı durumu oluştuğunda yapılacak eylemi tanımlamanızı sağlar.
  ![Eylem grubunu tanımlama](media/sql-database-insights-alerts-portal/action-group.png)

9. Kural için bir ad tanımlayın, isteğe bağlı bir açıklama sağlayın, kural için önem düzeyi seçin, kural oluşturma üzerine kuralı etkinleştirip etkinleştirmeyi seçin ve sonra metrik kural uyarısı oluşturmak için **kural uyarısı oluştur'u** tıklatın.

10 dakika içinde, uyarı etkin dir ve daha önce açıklandığı gibi tetiklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Uyarılardaki web hook'ları yapılandırma](../azure-monitor/platform/alerts-webhooks.md)hakkında daha fazla bilgi edinin.
