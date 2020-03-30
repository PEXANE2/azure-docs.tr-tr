---
title: Microsoft Cloud App Security ile uygulama görünürlüğü ve denetimi
description: Uygulama risk düzeylerini belirlemenin, ihlalleri ve sızıntıları gerçek zamanlı olarak durdurmanın ve görünürlük ve yönetim için sağlayıcı API'lerinden yararlanmak için uygulama bağlayıcılarını kullanmanın yollarını öğrenin.
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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77069745"
---
# <a name="cloud-app-visibility-and-control"></a>Bulut uygulama görünürlüğü ve denetimi

Bulut uygulama larından ve hizmetlerinden tam olarak yararlanmak için, bt ekibinin kritik verileri korumak için denetimi korurken erişimi desteklemenin doğru dengesini bulması gerekir. Microsoft Cloud App Security, tüm Microsoft ve üçüncü taraf bulut hizmetlerinizde siber tehditleri belirlemek ve bunlarla mücadele etmek için zengin görünürlük, veri yolculuğu üzerinde denetim ve gelişmiş analizsağlar.

## <a name="discover-and-manage-shadow-it-in-your-network"></a>Ağınızda gölge BT’yi keşfetme ve yönetme

BT yöneticilerine çalışanlarının ortalama olarak kaç bulut uygulaması kullandığını düşündükleri sorulduğunda, gerçekte ortalama 1.000'den fazla ayrı uygulama kuruluşunuzdaki çalışanlar tarafından kullanılıyor. Gölge BT, hangi uygulamaların kullanıldığını ve risk seviyenizin ne olduğunu bilmenize ve belirlemenize yardımcı olur. Çalışanların yüzde 80'i, kimsenin incelemediği ve güvenlik ve uyumluluk ilkelerinizle uyumlu olmadığı onaylanmamış uygulamalar kullanır. Ayrıca çalışanlarınız kaynaklarınıza ve uygulamalarınıza şirket ağınızın dışından erişebildiği için, güvenlik duvarlarınızda kurallar ve ilkeler olması artık yeterli değildir.

Hangi uygulamaların kullanıldığını keşfetmek, bu uygulamaların riskini keşfetmek, yeni riskli uygulamaları belirlemek için ilkeleri yapılandırmak ve bu uygulamaları yerel olarak kullanmak için onaylamamak için Microsoft Cloud App Discovery'yi (Azure Active Directory Premium P1 özelliği) kullanın proxy veya güvenlik duvarı cihazı.

- Gölge BT’yi keşfetme ve tanımlama
- Değerlendirme ve analiz etme
- Uygulamalarınızı yönetme
- Gelişmiş Gölge BT bulma raporlaması
- Onaylı uygulamaları kontrol edin
 
### <a name="learn-more"></a>Daha fazla bilgi edinin

- [Ağınızdaki gölge BT' yi keşfedin ve yönetin](https://docs.microsoft.com/cloud-app-security/tutorial-shadow-it)
- [Cloud App Security ile keşfedilen uygulamalar](https://docs.microsoft.com/cloud-app-security/discovered-apps)
 
## <a name="user-session-visibility-and-control"></a>Kullanıcı oturumu görünürlüğü ve denetimi 

Günümüzün işyerinde, olaydan sonra bulut ortamınızda neler olduğunu bilmek genellikle yeterli değildir. Çalışanların verilerinizi ve kuruluşunuzu kasıtlı veya istemeden riske atmadan önce ihlalleri ve sızıntıları gerçek zamanlı olarak durdurmak istiyorsunuz. Microsoft Cloud App Security, Azure Active Directory (Azure AD) ile birlikte bu özellikleri Koşullu Erişim Uygulama Denetimi ile bütünsel ve tümleşik bir deneyimle sunar. 

Oturum denetimi ters proxy mimarisi kullanır ve Azure AD Koşullu Erişim ile benzersiz bir şekilde tümleştirilir. Azure AD Koşullu Erişim, kuruluşunuzun uygulamalarında belirli koşullara bağlı olarak erişim denetimleri uygulamanıza olanak tanır. Koşullar, koşullu Erişim ilkesinin kime (kullanıcı veya kullanıcı grubu) ve hangi (hangi bulut uygulamaları) ve nerede (hangi konumlar ve ağlar) uygulandığını tanımlar. Koşulları belirledikten sonra, kullanıcıları verileri gerçek zamanlı olarak koruyabileceğiniz Bulut Uygulaması Güvenliği'ne yönlendirebilirsiniz.  

Bu denetim le şunları yapabilirsiniz:  
- Dosya indirmelerini kontrol edin
- B2B senaryolarını izleyin  
- Dosyalara erişimi denetleme  
- İndirmede belgeleri koruma  
 
### <a name="learn-more"></a>Daha fazla bilgi edinin

- [Bulut Uygulaması Güvenliğinde Oturum Kontrolü ile uygulamaları koruyun](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
 
## <a name="advanced-app-visibility-and-controls"></a>Gelişmiş uygulama görünürlüğü ve denetimleri 

Uygulama bağlayıcıları, bağlandığınız uygulamalar üzerinde Microsoft Cloud App Security tarafından daha fazla görünürlük ve denetim sağlamak için uygulama sağlayıcılarının API'lerini kullanır. Bulut App Security, bulut sağlayıcısı tarafından sağlanan API'lerden yararlanır. Her hizmetin kendi çerçevesi ve azaltma, API sınırları, dinamik zaman değiştiren API pencereleri ve diğerleri gibi API sınırlamaları vardır. Cloud App Security ürün ekibi, API kullanımını optimize etmek ve en iyi performansı sağlamak için bu hizmetlerle birlikte çalıştı. Bulut App Security motorları, hizmetlerin API'lerine uyguladığı farklı sınırlamaları dikkate alarak, izin verilen maksimum kapasitelerini kullanır. Kiracıdaki tüm dosyaları taramak gibi bazı işlemler, daha uzun bir süreye yayılmasını için çok sayıda API çağrısı gerektirir. Bazı ilkelerin birkaç saat veya gün boyunca çalışmasını bekleyin. 
 
### <a name="learn-more"></a>Daha fazla bilgi edinin  

- [Bulut Uygulaması Güvenliği'ndeki uygulamaları bağlama](https://docs.microsoft.com/cloud-app-security/enable-instant-visibility-protection-and-governance-actions-for-your-apps)

## <a name="next-steps"></a>Sonraki adımlar

- [Ağınızdaki gölge BT' yi keşfedin ve yönetin](https://docs.microsoft.com/cloud-app-security/tutorial-shadow-it)
- [Cloud App Security ile keşfedilen uygulamalar](https://docs.microsoft.com/cloud-app-security/discovered-apps)
- [Bulut Uygulaması Güvenliğinde Oturum Kontrolü ile uygulamaları koruyun](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
- [Bulut Uygulaması Güvenliği'ndeki uygulamaları bağlama](https://docs.microsoft.com/cloud-app-security/enable-instant-visibility-protection-and-governance-actions-for-your-apps)
