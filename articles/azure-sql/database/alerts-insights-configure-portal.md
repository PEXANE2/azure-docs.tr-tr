---
title: Kurulum uyarıları ve bildirimleri (Azure portal)
description: Belirttiğiniz koşullar karşılandığında bildirimleri veya Otomasyonu tetikleyebilen uyarılar oluşturmak için Azure portal kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: jrasnik, carlrab
ms.date: 05/04/2020
ms.openlocfilehash: 0e9b8f7bc4129c07145130f198b738839b0c7b55
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84047863"
---
# <a name="create-alerts-for-azure-sql-database-and-azure-synapse-using-azure-portal"></a>Azure portal kullanarak Azure SQL veritabanı ve Azure SYNAPSE için uyarılar oluşturma
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]


## <a name="overview"></a>Genel Bakış

Bu makalede, Azure SQL veritabanı ve Azure Synapse (eskiden Azure SQL veri ambarı) içindeki veritabanları için Azure portal kullanarak nasıl uyarı ayarlanacağı gösterilmektedir. Uyarılar bir e-posta gönderebilir veya bazı bir ölçüm (örneğin, veritabanı boyutu veya CPU kullanımı) eşiğe ulaştığında bir Web kancası çağırabilir. Bu makale, uyarı dönemlerini ayarlamaya yönelik en iyi yöntemleri de sağlar.

> [!NOTE]
> Azure SQL yönetilen örneğe özgü yönergeler için bkz. [Azure SQL yönetilen örneği için uyarı oluşturma](../managed-instance/alerts-create.md).

Azure hizmetleriniz, veya üzerindeki olaylar için izleme ölçümlerini temel alarak bir uyarı alabilirsiniz.

* **Ölçüm değerleri** -belirtilen bir ölçümün değeri her iki yönde atadığınız bir eşiği aştığında, uyarı tetiklenir. Yani, hem koşul ilk karşılandığında hem de daha sonra bu koşul karşılanamadığında bu durum tetiklenir.
* **Etkinlik günlüğü olayları** -bir uyarı *her* olay üzerinde veya yalnızca belirli bir sayıda olay meydana geldiğinde tetiklenebilir.

Bir uyarıyı, tetiklendiğinde aşağıdakileri yapmak için yapılandırabilirsiniz:

* Hizmet yöneticisine ve ortak yöneticilere e-posta bildirimleri gönderme
* Belirttiğiniz ek e-postalara e-posta gönderin.
* Web kancası çağırma

Kullanarak uyarı kuralları hakkında bilgi alabilir ve bunları alabilirsiniz

* [Azure portal](../../azure-monitor/platform/alerts-classic-portal.md)
* [PowerShell](../../azure-monitor/platform/alerts-classic-portal.md)
* [komut satırı arabirimi (CLı)](../../azure-monitor/platform/alerts-classic-portal.md)
* [Azure İzleyici REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Azure portal bir ölçümde uyarı kuralı oluşturma

1. [Portalda](https://portal.azure.com/), izlemekte olduğunuz kaynağı bulun ve seçin.
2. Izleme bölümünde **Uyarılar** ' ı seçin. Metin ve simge farklı kaynaklar için biraz farklılık gösterebilir.  

   ![İzleme](./media/alerts-insights-configure-portal/Alerts.png)
  
3. **Kural oluştur** sayfasını açmak için **Yeni uyarı kuralı** düğmesini seçin.
  ![Kural Oluştur](./media/alerts-insights-configure-portal/create-rule.png)

4. **Koşul** bölümünde **Ekle**' ye tıklayın.
  ![Koşulu tanımla](./media/alerts-insights-configure-portal/create-rule.png)
5. **Sinyal mantığını Yapılandır** sayfasında bir sinyal seçin.
  ![Sinyal seçin ](./media/alerts-insights-configure-portal/select-signal.png) .
6. **CPU yüzdesi**gibi bir sinyal seçtikten sonra **sinyal mantığını Yapılandır** sayfası görünür.
  ![Sinyal mantığını yapılandırma](./media/alerts-insights-configure-portal/configure-signal-logic.png)
7. Bu sayfada, bu eşik türü, işleç, toplama türü, eşik değeri, toplama ayrıntı düzeyi ve değerlendirme sıklığını yapılandırın. Sonra da **Bitti**’ye tıklayın.
8. **Oluşturma kuralında**, var olan bir **Eylem grubunu** seçin veya yeni bir grup oluşturun. Bir eylem grubu, bir uyarı koşulu oluştuğunda gerçekleştirilecek eylemi tanımlamanızı sağlar.
  ![Eylem grubunu tanımla](./media/alerts-insights-configure-portal/action-group.png)

9. Kural için bir ad tanımlayın, isteğe bağlı bir açıklama sağlayın, kural için bir önem düzeyi seçin, kural oluşturulduktan sonra kuralın etkinleştirilip etkinleştirilmeyeceğini seçin ve ardından ölçüm kuralı uyarısı oluşturmak için **kural uyarısı oluştur** ' a tıklayın.

10 dakika içinde, uyarı etkin ve daha önce açıklandığı gibi tetikler.

## <a name="next-steps"></a>Sonraki adımlar

* [Uyarılarda Web kancalarını yapılandırma](../../azure-monitor/platform/alerts-webhooks.md)hakkında daha fazla bilgi edinin.
