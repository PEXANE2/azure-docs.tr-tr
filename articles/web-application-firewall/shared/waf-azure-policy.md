---
title: Azure Web uygulaması güvenlik duvarı ve Azure Ilkesi
description: Azure Ilkesiyle birleştirilmiş Azure Web uygulaması güvenlik duvarı (WAF), WAF kaynakları için kurumsal standartları zorunlu kılabilir ve uyumluluğu değerlendirmenize yardımcı olabilir
author: tremansdoerfer
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: conceptual
ms.date: 06/23/2020
ms.author: rimansdo
ms.openlocfilehash: 4c1fd53eb6ebf1f1aebdfba99b736e26bd6cff2b
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307000"
---
# <a name="azure-web-application-firewall-and-azure-policy"></a>Azure Web uygulaması güvenlik duvarı ve Azure Ilkesi

Azure Ilkesiyle birleştirilmiş Azure Web uygulaması güvenlik duvarı (WAF), kurumsal standartları zorunlu kılabilir ve WAF kaynakları için uygun ölçekte uyumluluğu değerlendirmenize yardımcı olabilir. Azure ilkesi, ortamın genel durumunu değerlendirmek için toplanan bir görünüm sağlayan, kaynak başına, ilke başına ayrıntı düzeyi ile ayrıntıya gitme olanağı sunan bir idare aracıdır. Azure ilkesi Ayrıca, mevcut kaynaklar için toplu düzeltme ve yeni kaynaklar için otomatik düzeltme aracılığıyla kaynaklarınızı uyumluluğa getirmeye yardımcı olur.

## <a name="azure-policies-for-web-application-firewall"></a>Web uygulaması güvenlik duvarı için Azure Ilkeleri

WAF kaynaklarını yönetmek için birkaç yerleşik Azure Ilkesi vardır. İlkelerin ve bunların işlevlerini bir dökümü aşağıdaki gibidir:

1. **Azure ön kapı hizmeti veya Application Gateway Için Web uygulaması güvenlik duvarı etkinleştirilmelidir**: kaynak oluşturma üzerinde bir WAF varsa, Azure ön kapı Hizmetleri ve uygulama ağ geçitleri üzerinde değerlendirilir. İlkenin üç etkisi vardır: denetim, reddetme ve devre dışı bırakma. Denetim, bir Azure ön kapısının veya Application Gateway WAF olmadığında ve kullanıcıların Azure ön kapısının ne olduğunu veya Application Gateway Şu anda uyumlu olmadığını görmesini sağlar. Reddetme, bir WAF iliştiriliyorsa Azure ön kapı hizmeti 'nin veya Application Gateway oluşturulmasını engeller. Devre dışı bırakılan bu ilkeyi kapatır.

2. **Web uygulaması güvenlik duvarı Application Gateway ve Azure ön kapı hizmeti için ayarlanmış bir mod**olmalıdır: Web uygulaması güvenlik duvarı, içinde olduğu modda, önleme ya da algılama için değerlendirilir. İlke, Web uygulaması güvenlik duvarları arasında mod tutarlılığı sağlar. İlkenin üç etkisi vardır: denetim, reddetme ve devre dışı bırakma. Bir WAF belirtilen moda uygun olmadığında denetim izler. Deny, doğru modda değilse WAF 'nin oluşturulmasını engeller. Devre dışı bırakılan bu ilkeyi kapatır.


## <a name="launch-an-azure-policy"></a>Azure Ilkesi başlatma


1.  Azure giriş sayfasında, arama çubuğuna Ilke yazın ve Azure Ilkesi simgesine tıklayın.

2.  Azure ilke hizmetinde, **yazma**altında, **atamalar**' ı seçin.

![Azure Web uygulaması güvenlik duvarı](../media/waf-azure-policy/policy-home.png)

3.  Atamalar sayfasında, üst kısımdaki **Ilke ata** simgesini seçin.

![Azure Web uygulaması güvenlik duvarı](../media/waf-azure-policy/assign-policy.png)

4.  Ilke atama sayfası temelleri sekmesinde aşağıdaki alanları güncelleştirin:
    1.  **Kapsam**: Azure ilkesi tarafından hangi Azure aboneliklerinin ve kaynak gruplarının etkilenmelidir ' ı seçin.
    2.  **Dışlamalar**: ilkeden dışlamak için kapsamdaki kaynakları seçin 
    3.  **Ilke tanımı**: dışlamalarla kapsama uygulanacak Azure ilkesini seçin. İlgili Web uygulaması güvenlik duvarı Azure Ilkesini seçmek için arama çubuğuna "Web uygulaması güvenlik duvarı" yazın.

![Azure Web uygulaması güvenlik duvarı](../media/waf-azure-policy/policy-listings.png)


5.  **Parametreler** sekmesini seçin ve ilke parametrelerini güncelleştirin. Parametrenin ne yaptığını netleştirmek için, daha fazla açıklama için parametre adının yanındaki bilgi simgesinin üzerine gelin.

6.  Azure ilkenizi tamamlamak için **gözden geçir + oluştur** ' u seçin. Yeni kaynaklar için etkin olana kadar Azure ilkesi yaklaşık 15 dakika sürer.
