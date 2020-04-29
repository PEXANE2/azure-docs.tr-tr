---
title: Kurulum uyarıları ve bildirimleri (Azure portal)
description: Belirttiğiniz koşullar karşılandığında bildirimleri veya Otomasyonu tetikleyebilen SQL veritabanı uyarılarını oluşturmak için Azure portal kullanın.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79209558"
---
# <a name="create-alerts-for-azure-sql-database-and-azure-synapse-analytics-databases-using-azure-portal"></a>Azure portal kullanarak Azure SQL veritabanı ve Azure SYNAPSE Analytics veritabanları için uyarı oluşturma

## <a name="overview"></a>Genel Bakış

Bu makalede, Azure SQL veritabanı ve Azure SYNAPSE Analytics 'te (eskiden Azure SQL veri ambarı) Azure portal kullanarak tek, havuza alınmış ve veri ambarı veritabanları için uyarıların nasıl ayarlanacağı gösterilmektedir. Uyarılar bir e-posta gönderebilir veya bazı bir ölçüm (örneğin, veritabanı boyutu veya CPU kullanımı) eşiğe ulaştığında bir Web kancası çağırabilir. Bu makale, uyarı dönemlerini ayarlamaya yönelik en iyi yöntemleri de sağlar.

> [!IMPORTANT]
> Bu özellik, yönetilen örnekte henüz kullanılamıyor. Alternatif olarak, [dinamik yönetim görünümlerine](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)göre bazı ölçümler için e-posta uyarıları göndermek üzere SQL Agent 'ı kullanabilirsiniz.

Azure hizmetleriniz, veya üzerindeki olaylar için izleme ölçümlerini temel alarak bir uyarı alabilirsiniz.

* **Ölçüm değerleri** -belirtilen bir ölçümün değeri her iki yönde atadığınız bir eşiği aştığında, uyarı tetiklenir. Yani, hem koşul ilk karşılandığında hem de daha sonra bu koşul karşılanamadığında bu durum tetiklenir.
* **Etkinlik günlüğü olayları** -bir uyarı *her* olay üzerinde veya yalnızca belirli bir sayıda olay meydana geldiğinde tetiklenebilir.

Bir uyarıyı, tetiklendiğinde aşağıdakileri yapmak için yapılandırabilirsiniz:

* Hizmet yöneticisine ve ortak yöneticilere e-posta bildirimleri gönderme
* Belirttiğiniz ek e-postalara e-posta gönderin.
* Web kancası çağırma

Kullanarak uyarı kuralları hakkında bilgi alabilir ve bunları alabilirsiniz

* [Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../azure-monitor/platform/alerts-classic-portal.md)
* [komut satırı arabirimi (CLı)](../azure-monitor/platform/alerts-classic-portal.md)
* [Azure İzleyici REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Azure portal bir ölçümde uyarı kuralı oluşturma

1. [Portalda](https://portal.azure.com/), izlemekte olduğunuz kaynağı bulun ve seçin.
2. Izleme bölümünde **Uyarılar** ' ı seçin. Metin ve simge farklı kaynaklar için biraz farklılık gösterebilir.  

   ![İzleme](media/sql-database-insights-alerts-portal/Alerts.png)
  
3. **Kural oluştur** sayfasını açmak için **Yeni uyarı kuralı** düğmesini seçin.
  ![Kural Oluştur](media/sql-database-insights-alerts-portal/create-rule.png)

4. **Koşul** bölümünde **Ekle**' ye tıklayın.
  ![Koşulu tanımla](media/sql-database-insights-alerts-portal/create-rule.png)
5. **Sinyal mantığını Yapılandır** sayfasında bir sinyal seçin.
  ![Sinyal](media/sql-database-insights-alerts-portal/select-signal.png)seçin.
6. **CPU yüzdesi**gibi bir sinyal seçtikten sonra **sinyal mantığını Yapılandır** sayfası görünür.
  ![Sinyal mantığını yapılandırma](media/sql-database-insights-alerts-portal/configure-signal-logic.png)
7. Bu sayfada, bu eşik türü, işleç, toplama türü, eşik değeri, toplama ayrıntı düzeyi ve değerlendirme sıklığını yapılandırın. Sonra da **Bitti**’ye tıklayın.
8. **Oluşturma kuralında**, var olan bir **Eylem grubunu** seçin veya yeni bir grup oluşturun. Bir eylem grubu, bir uyarı koşulu oluştuğunda gerçekleştirilecek eylemi tanımlamanızı sağlar.
  ![Eylem grubunu tanımla](media/sql-database-insights-alerts-portal/action-group.png)

9. Kural için bir ad tanımlayın, isteğe bağlı bir açıklama sağlayın, kural için bir önem düzeyi seçin, kural oluşturulduktan sonra kuralın etkinleştirilip etkinleştirilmeyeceğini seçin ve ardından ölçüm kuralı uyarısı oluşturmak için **kural uyarısı oluştur** ' a tıklayın.

10 dakika içinde, uyarı etkin ve daha önce açıklandığı gibi tetikler.

## <a name="next-steps"></a>Sonraki adımlar

* [Uyarılarda Web kancalarını yapılandırma](../azure-monitor/platform/alerts-webhooks.md)hakkında daha fazla bilgi edinin.
