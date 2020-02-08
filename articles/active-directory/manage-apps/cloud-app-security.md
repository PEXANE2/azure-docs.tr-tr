---
title: Microsoft Cloud App Security ile uygulama görünürlüğü ve denetimi
description: Uygulama risk düzeylerini belirlemeyi, ihlal ve sızıntıları gerçek zamanlı olarak durdurmayı ve uygulama bağlayıcılarını kullanarak görünürlük ve idare için sağlayıcı API 'Lerinden yararlanmanızı sağlayan yollar edinin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 02/03/2020
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 206e1a06acddae0973d5dbc7e64212026149f217
ms.sourcegitcommit: a460fdc19d6d7af6d2b5a4527e1b5c4e0c49942f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77069745"
---
# <a name="cloud-app-visibility-and-control"></a>Cloud App görünürlük ve denetimi

Bulut uygulamalarının ve hizmetlerinin tam avantajlarından yararlanmak için, bir BT ekibinin, kritik verileri korumak için denetimi korurken doğru destekleyici erişim dengesini bulması gerekir. Microsoft Cloud App Security, tüm Microsoft ve üçüncü taraf bulut hizmetlerinize ilişkin zengin görünürlük, veri yolculuğu üzerinde denetim ve gelişmiş analizler sağlar.

## <a name="discover-and-manage-shadow-it-in-your-network"></a>Ağınızda gölge BT 'yi bulma ve yönetme

BT yöneticileri, çalışanlarının ne kadar çok sayıda bulut uygulaması kullandığını sorduğu zaman, gerçekte 30 veya 40 olduğunu düşünürken, ortalama, kuruluşunuzda çalışanlar tarafından kullanılan 1.000 ayrı farklı uygulama. Gölge BT, hangi uygulamaların kullanıldığını ve risk düzeyinin ne olduğunu bilmenize ve belirlemenize yardımcı olur. Çalışanların yüzde 80 ' i gözden geçirilmemiş ve güvenlik ve uyumluluk ilkelerinizle uyumlu olmayan, tasdiksiz uygulamalar kullanıyor. Ve çalışanlarınız, kaynak ve uygulamalarınıza şirket ağınızın dışından erişebildiğinden, artık güvenlik duvarlarınızda kuralların ve ilkelerin olması yeterli değildir.

Hangi uygulamaların kullanıldığını bulmak için Microsoft Bulut uygulama bulma (Azure Active Directory Premium P1 özelliği) kullanın, bu uygulamaların riskini keşfetme, yeni riskli uygulamaları belirlemek için ilkeleri yapılandırma ve bu uygulamaların yerel olarak bu uygulamaları kullanarak engellemesini kaldırma proxy veya güvenlik duvarı gereci.

- Gölge BT 'yi bul ve tanımla
- Değerlendirin ve çözümleyin
- Uygulamalarınızı yönetin
- Gelişmiş gölge BT bulma raporlaması
- Tasdikli uygulamaları denetleme
 
### <a name="learn-more"></a>Daha fazlasını öğrenin

- [Ağınızda gölge BT 'yi bulma ve yönetme](https://docs.microsoft.com/cloud-app-security/tutorial-shadow-it)
- [Cloud App Security ile bulunan uygulamalar](https://docs.microsoft.com/cloud-app-security/discovered-apps)
 
## <a name="user-session-visibility-and-control"></a>Kullanıcı oturumu görünürlüğü ve denetimi 

Bugünün çalışma alanında, bulut ortamınızda olaydan sonra neler olduğunu bilmeniz yeterli değil genellikle. Çalışanlar kasıtlı olarak veya farkında olmadan verilerinizi ve kuruluşunuzu riske almadan önce gerçek zamanlı olarak ihlal etmek ve sızıntıları durdurmak istiyorsunuz. Azure Active Directory (Azure AD) ile birlikte Microsoft Cloud App Security, bu özellikleri Koşullu Erişim Uygulama Denetimi bütünsel ve tümleşik bir deneyime sunar. 

Oturum denetimi ters bir ara sunucu mimarisi kullanır ve Azure AD koşullu erişimiyle benzersiz bir şekilde tümleşiktir. Azure AD koşullu erişimi, kuruluşunuzun uygulamalarında belirli koşullara göre erişim denetimleri zorlamanıza olanak sağlar. Koşullar, kimlerin (Kullanıcı veya Kullanıcı grubu) ve ne (bulut uygulamaları) ve koşullu erişim ilkesinin hangi konuma uygulanacağını tanımlar. Koşulları belirledikten sonra, verileri gerçek zamanlı olarak koruyabileceğiniz Cloud App Security kullanıcılara yönlendirebilirsiniz.  

Bu denetimle şunları yapabilirsiniz:  
- Dosya indirmelerini denetle
- B2B senaryolarını izleme  
- Dosyalara erişimi denetleme  
- İndirme sırasında belgeleri koruma  
 
### <a name="learn-more"></a>Daha fazlasını öğrenin

- [Cloud App Security oturum denetimi ile uygulamaları koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
 
## <a name="advanced-app-visibility-and-controls"></a>Gelişmiş uygulama görünürlüğü ve denetimleri 

Uygulama bağlayıcıları, daha fazla görünürlük ve denetim Microsoft Cloud App Security tarafından bağlandığınız uygulamalar üzerinde etkinleştirmek için uygulama sağlayıcılarının API'leri kullanın. Cloud App Security, bulut sağlayıcısı tarafından sunulan API 'lerden yararlanır. Her hizmetin kendi çerçevesi ve azaltma, API sınırları, dinamik süre değiştirme API pencereleri ve diğerleri gibi API sınırlamaları vardır. Cloud App Security ürün ekibi, API 'lerin kullanımını iyileştirmek ve en iyi performansı sağlamak için bu hizmetlerle birlikte çalışmıştır. Farklı sınırlamalar Hizmetleri API 'lerine göre hesaba katılarak, Cloud App Security motorları izin verilen en büyük kapasiteyi kullanır. Kiracıdaki tüm dosyaları tarama gibi bazı işlemler, daha uzun bir süre içinde yayılmaları için çok sayıda API çağrısı gerektirir. Bazı ilkelerin birkaç saat veya gün boyunca çalışmasını bekliyor. 
 
### <a name="learn-more"></a>Daha fazlasını öğrenin  

- [Cloud App Security uygulamaları bağlama](https://docs.microsoft.com/cloud-app-security/enable-instant-visibility-protection-and-governance-actions-for-your-apps)

## <a name="next-steps"></a>Sonraki adımlar

- [Ağınızda gölge BT 'yi bulma ve yönetme](https://docs.microsoft.com/cloud-app-security/tutorial-shadow-it)
- [Cloud App Security ile bulunan uygulamalar](https://docs.microsoft.com/cloud-app-security/discovered-apps)
- [Cloud App Security oturum denetimi ile uygulamaları koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
- [Cloud App Security uygulamaları bağlama](https://docs.microsoft.com/cloud-app-security/enable-instant-visibility-protection-and-governance-actions-for-your-apps)
