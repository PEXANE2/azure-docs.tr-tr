---
title: Azure Stack Edge cihazını dağıtmaya yönelik ön dağıtım denetim listesi | Microsoft Docs
description: Bu makalede, Azure Stack Edge cihazınızı dağıtmadan önce toplanacak bilgiler açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/26/2020
ms.author: alkohli
ms.openlocfilehash: 065a6595ba89e00310ff41cd63b929f6d6d83222
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89087159"
---
# <a name="deployment-checklist-for-your-azure-stack-edge-device"></a>Azure Stack Edge cihazınız için dağıtım denetim listesi  

Bu makalede, Azure Stack Edge cihazınızın gerçek dağıtımının önüne toplanacak bilgiler açıklanmaktadır. 

Azure Stack Edge cihazına sipariş ettikten sonra ve cihazı almadan önce bu bilgilere sahip olduğunuzdan emin olmak için aşağıdaki denetim listesini kullanın. 

## <a name="deployment-checklist"></a>Dağıtım denetim listesi 

| Aşama                             | Parametre                                                                                                                                                                                                                           | Ayrıntılar                                                                                                           |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Cihaz yönetimi               | <li>Microsoft Azure aboneliği</li><li>Azure Active Directory Graph API'si</li><li>Microsoft Azure Depolama hesabı</li>|<li>Azure Stack Edge/Data Box Gateway için etkinleştirildi, katkıda bulunan izinleri</li><li>Yönetici veya Kullanıcı erişimi sağlayın</li><li>Erişim kimlik bilgilerine ihtiyacınız</li> |
| Cihaz yüklemesi               | Paketteki güç kabloları. <br>ABD için, 125 V için derecelendirilen bir SVE 18/3 kablosu ve NEMA 5-15P ile C13 (çıktı girişi) Bağlayıcısı ile 15 AMPS dağıtılır.                                                                                                                                                                                                          | Cihaz ile birlikte sunulur.<br>Daha fazla bilgi için [ülkeye göre desteklenen güç](azure-stack-edge-technical-specifications-power-cords-regional.md) 'lerin listesine bakın                                                                                        |
|                                   | <li>Bağlantı noktası 1 için en az 1 X 1-GbE RJ-45 ağ kablosu </li><li> Bağlantı noktası 3, bağlantı noktası 4, bağlantı noktası 5 veya bağlantı noktası 6 için en az 1 X 25-GbE SFP + bakır kablo</li>| Müşterinin bu kabloları sağlaması gerekir.<br>Cihaz ağ kartları için desteklenen ağ kabloları, anahtarlar ve alıcı vericiler tam listesi için bkz [. Cavium Fastlınq 41000 serisi birlikte çalışabilirlik matrisi.](https://www.marvell.com/documents/xalflardzafh32cfvi0z/)                                                                          | 
| Cihaz bağlantısı ilk kez      | Bağlantı noktası 1 ' in ilk bağlantı için sabit bir IP (192.168.100.10/24) vardır. <li>192.168.100.0/24 ağında bir IP adresi ile bağlantı noktası 1 ' e doğrudan bağlantı için bir dizüstü bilgisayar gerektir.</li><li> Daha fazla yapılandırma için, şu adreste bulunan cihazın yerel kullanıcı arabirimini kullanın: `https://192.168.100.10`</li>|                                                                                                                   |
| Cihaz oturum açma                      | Cihaz Yöneticisi parolası 8 ila 16 karakter arasında olmalıdır. <br>Şu karakterlerden üçünü içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler.                                            | Varsayılan parola ilk oturum açma sırasında süresi dolan *Parola1* .                                                     |
| Ağ ayarları                  | Cihaz 2 x 1 GbE, 4 x 25 GbE ağ bağlantı noktalarıyla gelir. <li>Bağlantı noktası 1 yalnızca yönetim ayarlarını yapılandırmak için kullanılır. Bir veya daha fazla veri bağlantı noktası bağlanabilir ve yapılandırılabilir. </li><li> Bağlantı noktası 2-bağlantı noktası 6 arasındaki en az bir veri ağı arabiriminin Internet 'e bağlanması gerekir (Azure bağlantısı ile).</li><li> IP ayarları DHCP/static IPv4 yapılandırmasını destekler. | Statik IPv4 yapılandırması için IP, DNS sunucusu ve varsayılan ağ geçidi gerekir.                                                                                                                  |
| İşlem ağ ayarları     | <li>Kubernetes düğümleri için 2 statik genel IP ve işlem modüllerine erişmek için Azure Stack Edge hub hizmeti için en az 1 statik IP gerektir.</li><li>Kubernetes kümesi dışından dışarıdan erişilmesi gereken her ek hizmet veya kapsayıcı için bir IP gerekir.</li>                                                                                                                       | Yalnızca statik IPv4 yapılandırması desteklenir.                                                                      |
| Seçim Web proxy ayarları     | <li>Web proxy sunucusu IP/FQDN, bağlantı noktası </li><li>Web Proxy Kullanıcı adı, parola</li>                                                                                                                                                                                                    | Şu anda işlem kurulumu ile desteklenmiyor.                                                                     |
| Güvenlik Duvarı ve bağlantı noktası ayarları        | [Listelenen URL düzenlerini ve bağlantı noktalarını](azure-stack-edge-system-requirements.md#networking-port-requirements) , cihaz IP 'leri için izin verilecek şekilde kullanın.                                                                                                                                                  |                                                                                                                   |
| Seçim MAC adresi            | MAC adresinin beyaz listeye alınması gerekiyorsa, cihazın yerel kullanıcı arabiriminden bağlı bağlantı noktasının adresini alın. |                                                                                                                   |
| Seçim Ağ anahtarı bağlantı noktası    | Cihaz, işlem için Hyper-V VM 'lerini barındırır. Bazı ağ anahtarı bağlantı noktası yapılandırmalarında bu ayarlar varsayılan olarak yoktur.                                                                                                        |                                                                                                                   |
| Önerilen Zaman ayarları       | Saat dilimini, birincil NTP sunucusunu, ikincil NTP sunucusunu yapılandırın.                                                                                                                                                                    | Yerel ağ üzerinde birincil ve ikincil NTP sunucusunu yapılandırın.<br>Yerel sunucu kullanılabilir değilse, genel NTP sunucuları yapılandırılabilir.                                                    |
| Seçim Sunucu ayarlarını Güncelleştir | <li>Yerel ağ üzerinde güncelleştirme sunucusu IP adresi, WSUS sunucusu için yol gerektir. </li> | Varsayılan olarak, genel Windows Update sunucusu kullanılır.|
| Cihaz ayarları                   | <li>DNS kuruluşunda kayıtlı cihaz adı </li><li>DNS etki alanı</li> |                                                                                                                   |
| Seçim Sertifika                      | Cihaz tarafından oluşturulan sertifikaları kullanın <br><br> Kendi sertifikalarınızı getirirken şunlar gerekir: <li>*. Cer* ile der formatında güvenilen kök imza sertifikası </li><li>*PFX* biçimindeki uç nokta sertifikaları</li>|Uç nokta sertifikaları Azure Resource Manager, BLOB depolama, yerel Web Kullanıcı arabirimi için içerir.                                                                                                                   |
| Etkinleştirme                        | Azure Stack Edge/Data Box Gateway kaynağından etkinleştirme anahtarı gerektir.                                                                                                                                                       | Oluşturulduktan sonra anahtarın 3 gün içinde süresi dolar.                                                                        |


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Edge cihazınızı](azure-stack-edge-gpu-deploy-prep.md)dağıtmaya hazırlanın.



