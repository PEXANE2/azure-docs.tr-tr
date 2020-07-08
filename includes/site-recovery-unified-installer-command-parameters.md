---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 9d9790c9b3dbe3b130be999dd76092ae64f7b52c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188077"
---
|Parametre Adı| Tür | Açıklama| Olası Değerler|
|-|-|-|-|
| /ServerMode|Zorunlu|Hem yapılandırma hem de işlem sunucusunun mu yoksa yalnızca işlem sunucusunun mu yükleneceğini belirtir|CS<br>PS|
|/InstallLocation|Zorunlu|Bileşenlerin yüklendiği klasör| Bilgisayardaki herhangi bir klasör|
|/MySQLCredsFilePath|Zorunlu|MySQL sunucusu kimlik bilgilerinin depolandığı dosya yolu|Dosya aşağıda belirtilen biçimde olmalıdır|
|/VaultCredsFilePath|Zorunlu|Kasa kimlik bilgileri dosyasının yolu|Geçerli dosya yolu|
|/EnvType|Zorunlu|Korumak istediğiniz ortam türü |VMware<br>NonVMware|
|/PSIP|Zorunlu|Çoğaltma veri aktarımı için kullanılacak NIC’nin IP adresi| Herhangi bir geçerli IP adresi|
|/CSIP|Zorunlu|Yapılandırma sunucusunun dinleme yaptığı NIC’nin IP adresi| Herhangi bir geçerli IP adresi|
|/PassphraseFilePath|Zorunlu|Parola dosyası konumunun tam yolu|Geçerli dosya yolu|
|/BypassProxy|İsteğe Bağlı|Yapılandırma sunucusunun Azure'a bir ara sunucu olmadan bağlandığını belirtir|Yapmak için bu değeri Venu’den alın|
|/ProxySettingsFilePath|İsteğe Bağlı|Ara sunucu ayarları (Varsayılan ara sunucu kimlik doğrulaması gerektirir ya da özel bir ara sunucu kullanılır)|Dosya aşağıda belirtilen biçimde olmalıdır|
|DataTransferSecurePort|İsteğe Bağlı|Çoğaltma verileri için kullanılacak PSIP’deki bağlantı noktası numarası| Geçerli Bağlantı Noktası Numarası (varsayılan değer: 9433)|
|/SkipSpaceCheck|İsteğe Bağlı|Önbellek diski için alan denetimini atlama| |
|/AcceptThirdpartyEULA|Zorunlu|Bayrak, üçüncü taraf EULA'nın kabul edildiğini gösterir| |
|/ShowThirdpartyEULA|İsteğe Bağlı|Üçüncü taraf EULA belgesini görüntüler. Giriş olarak sağlanırsa, diğer tüm parametreler yoksayılır| |
