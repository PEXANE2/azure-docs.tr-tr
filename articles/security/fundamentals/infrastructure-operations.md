---
title: Azure üretim ağının yönetimi - Microsoft Azure
description: Bu makalede, Microsoft'un Azure veri merkezlerini güvence altına almak için Azure üretim ağını nasıl yönettiği ve çalıştığı açıklanmaktadır.
services: security
documentationcenter: n
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2019
ms.author: terrylan
ms.openlocfilehash: d41fe409b4a44a4c2af3670d76dd3a83a300feae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68727125"
---
# <a name="management-and-operation-of-the-azure-production-network"></a>Azure üretim ağının yönetimi ve çalışması    
Bu makalede, Microsoft'un Azure veri merkezlerini güvence altına almak için Azure üretim ağını nasıl yönettiği ve çalıştığı açıklanmaktadır.

## <a name="monitor-log-and-report"></a>İzleme, günlüğe kaydetme ve rapor

Azure üretim ağının yönetimi ve çalışması, Azure ve Azure SQL Veritabanı operasyon ekipleri arasında eşgüdümlü bir çabadır. Ekipler, ortamda çeşitli sistem ve uygulama performans izleme araçları kullanır. Ayrıca ağ aygıtlarını, sunucuları, hizmetleri ve uygulama süreçlerini izlemek için uygun araçları kullanırlar.

İşlem ekipleri, Azure ortamında çalışan hizmetlerin güvenli bir şekilde yürütülmesini sağlamak için, aşağıdaki eylemler de dahil olmak üzere birden çok izleme, günlüğe kaydetme ve raporlama düzeyi uygular:

- Öncelikle, Microsoft İzleme Aracısı (MMA) kumaş denetleyicisi (FC) ve kök işletim sistemi (OS) dahil olmak üzere birçok yerden izleme ve tanılama günlüğü bilgilerini toplar ve dosyaları günlüğe yazar. Aracı, sonunda bilgilerin sindirilmiş bir alt kümesini önceden yapılandırılmış bir Azure depolama hesabına iter. Buna ek olarak, bağımsız izleme ve tanılama hizmeti çeşitli izleme ve tanı lama günlüğü verilerini okur ve bilgileri özetler. İzleme ve tanılama hizmeti bilgileri tümleşik bir günlüğe yazar. Azure, Azure izleme sisteminin bir uzantısı olan özel olarak oluşturulmuş Azure güvenlik izleme sini kullanır. Platformdaki çeşitli noktalardan güvenlikle ilgili olayları gözlemleyen, analiz eden ve raporlayan bileşenlere sahiptir.

- Azure SQL Veritabanı Windows Fabric platformu, Azure SQL Veritabanı için yönetim, dağıtım, geliştirme ve operasyonel gözetim hizmetleri sağlar. Platform dağıtılmış, çok aşamalı dağıtım hizmetleri, sistem durumu izleme, otomatik onarım ve servis sürümü uyumluluğu sunar. Aşağıdaki hizmetleri sağlar:

   - Yüksek doğrulukgeliştirme ortamına sahip hizmet modelleme yetenekleri (veri merkezi kümeleri pahalı ve azdır).
   - Servis bootstrap ve bakım için tek tıklamayla dağıtım ve yükseltme iş akışları.
   - Kendi kendine iyileşmeyi sağlamak için otomatik onarım iş akışları ile sağlık raporlaması.
   - Dağıtılmış bir sistemin düğümleri arasında gerçek zamanlı izleme, uyarı ve hata ayıklama tesisleri.
   - Dağıtılmış kök neden analizi ve hizmet içgörüsü için operasyonel verilerin ve ölçümlerin merkezi olarak toplanması.
   - Dağıtım, değişim yönetimi ve izleme için operasyonel araç.
   - Azure SQL Veritabanı Windows Fabric platformu ve izleme komut dosyaları sürekli çalışır ve gerçek zamanlı olarak izler.

Herhangi bir anormallik oluşursa, Azure olay triajekibi tarafından izlenen olay yanıt işlemi etkinleştirilir. İlgili Azure destek personelinin olaya yanıt vermesi bildirilir. Sorun izleme ve çözümleme, merkezi bir biletleme sisteminde belgelenir ve yönetilir. Sistem çalışma süresi ölçümleri, gizlilik anlaşması (NDA) altında ve istek üzerine kullanılabilir.

## <a name="corporate-network-and-multi-factor-access-to-production"></a>Kurumsal ağ ve üretime çok faktörlü erişim
Kurumsal ağ kullanıcı tabanı, Azure destek personelini içerir. Şirket ağı dahili kurumsal işlevleri destekler ve Azure müşteri desteği için kullanılan dahili uygulamalara erişimi içerir. Şirket ağı, Azure üretim ağından hem mantıksal hem de fiziksel olarak ayrılmıştır. Azure personeli, Azure iş istasyonları ve dizüstü bilgisayarları kullanarak şirket ağına erişir. Tüm kullanıcıların kurumsal ağ kaynaklarına erişmek için kullanıcı adı ve parola da dahil olmak üzere bir Azure Etkin Dizin (Azure AD) hesabına sahip olması gerekir. Kurumsal ağ erişimi, tüm Microsoft personeline, yüklenicilerine ve satıcılarına verilen ve Microsoft Bilgi Teknolojisi tarafından yönetilen Azure AD hesaplarını kullanır. Benzersiz kullanıcı tanımlayıcıları, personeli Microsoft'taki istihdam durumlarına göre ayırır.

Dahili Azure uygulamalarına erişim, Active Directory Federation Services (AD FS) ile kimlik doğrulama yoluyla denetlenir. AD FS, microsoft bilgi teknolojisi tarafından barındırılan ve güvenli bir belirteç ve kullanıcı talepleri uygulayarak kurumsal ağ kullanıcılarının kimlik doğrulamasını sağlayan bir hizmettir. AD FS, dahili Azure uygulamalarının kullanıcıları Microsoft kurumsal etkin dizin etki alanına karşı doğrulamasına olanak tanır. Üretim ağına kurumsal ağ ortamından erişmek için, kullanıcıların çok faktörlü kimlik doğrulaması kullanarak kimlik doğrulaması yapması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft'un Azure altyapısını korumak için ne yaptığı hakkında daha fazla bilgi edinmek için bkz:

- [Azure tesisleri, tesisler ve fiziksel güvenlik](physical-security.md)
- [Azure altyapısı kullanılabilirliği](infrastructure-availability.md)
- [Azure bilgi sistemi bileşenleri ve sınırları](infrastructure-components.md)
- [Azure ağ mimarisi](infrastructure-network.md)
- [Azure üretim ağı](production-network.md)
- [Azure SQL Veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
- [Azure müşteri veri koruması](protection-customer-data.md)
