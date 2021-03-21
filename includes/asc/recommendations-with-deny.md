---
author: memildin
ms.service: security-center
ms.topic: include
ms.date: 03/14/2021
ms.author: memildin
ms.custom: generated
ms.openlocfilehash: c142f6a00275b4b21b6bbc64e51a5c4d7d957258
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103467993"
---
- Güvenlik Duvarı ve sanal ağ yapılandırmalarına sahip depolama hesaplarına erişim kısıtlı depolama hesapları ile ağ erişimini kısıtlamaları gerekir
- Otomasyon hesabı değişkenleri şifreli Otomasyon hesabı değişkenleri şifrelenmelidir
- Redo için Azure önbelleği bir sanal ağ içinde yer almalıdır
- Azure Cosmos DB hesaplar, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır
- Azure Machine Learning çalışma alanları, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)
- Azure yay bulutu, ağ ekleme kullanmalıdır
- Bilişsel hizmetler hesapları, müşteri tarafından yönetilen bir anahtarla (CMK) veri şifrelemeyi etkinleştirmelidir
- Kapsayıcı CPU ve bellek sınırları zorunlu kılınmalıdır
- Kapsayıcı görüntüleri yalnızca güvenilen kayıt defterlerinden dağıtılmalıdır
- Kapsayıcı kayıt defterleri, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)
- Ayrıcalık yükseltme ile kapsayıcının önlenebilir olması gerekir
- Gizli ana bilgisayar ad alanlarını paylaşan kapsayıcıların önlenebilir olması gerekir
- Kapsayıcılar yalnızca izin verilen bağlantı noktalarında dinleme yapılmalıdır
- Kapsayıcılar için sabit (salt okunurdur) kök dosya sistemi zorunlu kılınmalıdır
- Key Vault anahtarların bir sona erme tarihi olmalıdır
- Key Vault gizli dizileri için bir sona erme tarihi olmalıdır
- Anahtar kasaları Temizleme koruması etkin olmalıdır
- Anahtar kasaları geçici silme etkin olmalıdır
- Kapsayıcılar için en az ayrıcalıklı Linux özellikleri zorlanmalıdır
- Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir ve yalnızca redo için Azure önbelleğinize güvenli bağlantılar etkinleştirilmelidir
- Kapsayıcılar AppArmor profilinin geçersiz kılınması veya devre dışı bırakılması kısıtlanıyor
- Ayrıcalıklı kapsayıcılar kaçınılmalıdır
- Kök kullanıcı olarak çalışan kapsayıcılar önlenebilir olmalıdır
- Depolama hesaplarına Güvenli aktarım özelliğinin etkinleştirilmesi gerekir depolama hesaplarına Güvenli aktarım etkinleştirilmelidir
- Service Fabric kümelerinde ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmalıdır Service Fabric kümeler Kümeprotectionlevel özelliğinin EncryptAndSign olarak ayarlanmış olması gerekir
- Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır
- Hizmetlerin yalnızca izin verilen bağlantı noktalarını dinlemesi gerekir
- Depolama hesabı genel erişimine izin verilmemelidir
- Depolama hesaplarının yeni Azure Resource Manager kaynak depolama hesaplarına geçirilmesi gerekir Azure Resource Manager
- Depolama hesapları, sanal ağ kurallarını kullanarak ağ erişimini kısıtlamalı
- Konak ağ ve bağlantı noktalarının kullanımı sınırlandırılmalıdır
- , Güvenliği aşılmış kapsayıcılardan düğüm erişimini kısıtlamak için pod HostPath birimi takiciler, bilinen bir listeyle sınırlandırılmalıdır
- Azure Key Vault depolanan sertifikaların geçerlilik süresi 12 ayı aşmamalıdır
- Sanal makinelerin yeni Azure Resource Manager kaynaklara geçirilmesi gerekir sanal makineler yeni Azure Resource Manager kaynaklarına geçirilmelidir
- Web uygulaması güvenlik duvarı (WAF) Application Gateway için etkinleştirilmelidir
- Azure ön kapı hizmeti hizmeti için Web uygulaması güvenlik duvarı (WAF) etkinleştirilmelidir

