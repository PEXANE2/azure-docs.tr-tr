---
title: Azure Güvenlik Hizmetleri ve Teknolojileri | Microsoft Dokümanlar
description: Makale, Azure Güvenlik hizmetlerinin ve teknolojilerinin seçilmiş bir listesini sağlar.
services: security
documentationcenter: na
author: terrylanfear
manager: barbkess
editor: TomSh
ms.assetid: a5a7f60a-97e2-49b4-a8c5-7c010ff27ef8
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/29/2019
ms.author: terrylan
ms.openlocfilehash: 83548063a796401792bd19e7ec27be36a0efa4e4
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549200"
---
# <a name="security-services-and-technologies-available-on-azure"></a>Azure'da kullanılabilen güvenlik hizmetleri ve teknolojileri

Mevcut ve gelecekteki Azure müşterileri ile yaptığımız görüşmelerde sık sık "Azure'un sunduğu güvenlikle ilgili tüm hizmetlerin ve teknolojilerin bir listesi var mı?" sorusu sık sık sorulur.

Bulut hizmeti sağlayıcısı seçeneklerini değerlendirdiğinizde, bu bilgilere sahip olmak yararlı olur. Bu yüzden size başlamak için bu listeyi sağladık.

Zaman içinde, bu liste de Azure gibi değişecek ve büyüyecektir. Güvenlikle ilgili hizmetlerimiz ve teknolojilerimizden haberdar olmak için bu sayfayı düzenli olarak kontrol ettiğinizden emin olun.

## <a name="general-azure-security"></a>Genel Azure güvenliği
|Hizmet|Açıklama|
|--------|--------|
|[Azure&nbsp;&nbsp;Güvenlik Merkezi](/azure/security-center/security-center-intro)| Karma bulut iş yüklerinde güvenlik yönetimi ve gelişmiş tehdit koruması sağlayan bir bulut iş yükü koruma çözümü.|
|[Azure Key Vault](/azure/key-vault/key-vault-overview)| Uygulamalarınızı çalışır hale getirmek için gereken parolalar, bağlantı dizeleri ve diğer bilgiler için güvenli bir sır deposu. |
|[Azure İzleyici günlükleri](/azure/log-analytics/log-analytics-overview)|Telemetri ve diğer verileri toplayan ve uygulamalarınız ve kaynaklarınız için operasyonel öngörüler sunmak için bir sorgu dili ve analiz motoru sağlayan bir izleme hizmeti. Tek başına veya Güvenlik Merkezi gibi diğer hizmetlerle kullanılabilir. |
|[Azure Geliştirme/Test Laboratuvarları](/azure/lab-services/devtest-lab-overview)|Geliştiricilerin ve sınayıcıların Azure'da hızlı bir şekilde ortam lar oluşturmasına yardımcı olurken, atıkları en aza indirir ve maliyeti kontrol eder.  |

<!---|[Azure&nbsp;Disk&nbsp;Encryption](/azure/azure-security-disk-encryption-overview)| THIS WILL GO TO THE NEW OVERVIEW TOPIC MEGHAN STEWART IS WRITING|--->

## <a name="storage-security"></a>Depolama alanı güvenliği
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;&nbsp;Depolama&nbsp;Hizmeti Şifrelemesi](/azure/storage/common/storage-service-encryption)|Azure depolama alanında verilerinizi otomatik olarak şifreleyen bir güvenlik özelliği.   |
|[StorSimple Şifreli Karma Depolama](/azure/storsimple/storsimple-ova-overview)| Şirket içi aygıtlar ve Azure bulut depolama arasında depolama görevlerini yöneten tümleşik bir depolama çözümü.|
|[Azure İstemci Tarafı Şifrelemesi](/azure/storage/common/storage-client-side-encryption)| Azure Depolama'ya yüklemeden önce istemci uygulamaları içindeki verileri şifreleyen istemci tarafı şifreleme çözümü; ayrıca indirirken verilerin şifresini çözer. |
| [Azure Depolama Paylaşılan Erişim İmzaları](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)|Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar.  |
|[Azure Depolama Hesap Anahtarları](/azure/storage/common/storage-create-storage-account)| Azure depolama için, depolama hesabına erişildiğinde kimlik doğrulama için kullanılan bir erişim denetimi yöntemi. |
|[SMB 3.0 Şifreleme ile Azure Dosyası paylaşımları](/azure/storage/files/storage-files-introduction)|Sunucu İleti Bloğu (SMB) dosya paylaşım protokolü için otomatik ağ şifrelemesi sağlayan bir ağ güvenliği teknolojisi. |
|[Azure Depolama Analitiği](/rest/api/storageservices/Storage-Analytics)| Depolama hesabınızdaki veriler için günlük ve metrik oluşturma teknolojisi. |

<!------>

## <a name="database-security"></a>Veritabanı güvenliği
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;&nbsp;SQL Güvenlik Duvarı](/azure/sql-database/sql-database-firewall-configure)|Ağ tabanlı saldırılara karşı veritabanına karşı koruyan bir ağ erişim denetimi özelliği. |
|[Azure&nbsp;&nbsp;SQL&nbsp;Hücre Düzeyi Şifreleme](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)| Parçalı düzeyde şifreleme sağlayan bir veritabanı güvenlik teknolojisi.  |
| [Azure&nbsp;&nbsp;SQL Bağlantı Şifrelemesi](/azure/sql-database/sql-database-control-access)|SQL Veritabanı güvenliği sağlamak için erişimi IP adresine göre bağlantıyı sınırlayan güvenlik duvarı kuralları, kullanıcıların kimliğini kanıtlamasını gerektiren kimlik doğrulama sistemleri ve kullanıcıları belirli eylemler ve verilerle sınırlayan yetkilendirme sistemleriyle denetler. |
| [Azure SQL Her Zaman Şifreleme](/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)|Azure SQL Veritabanı veya SQL Server veritabanlarında depolanan kredi kartı numaraları veya ulusal kimlik numaraları (örneğin, ABD sosyal güvenlik numaraları) gibi hassas verileri korur.  |
| [Azure&nbsp;&nbsp;SQL Şeffaf Veri Şifreleme](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current)| Tüm veritabanının depolanmasını şifreleyen bir veritabanı güvenlik özelliği. |
| [Azure SQL Veritabanı Denetimi](/azure/sql-database/sql-database-auditing)|Veritabanı olaylarını izleyen ve bunları Azure depolama hesabınızdaki bir denetim günlüğüne yazan bir veritabanı denetleme özelliği.  |


## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;&nbsp;Role&nbsp;Tabanlı Erişim Denetimi](/azure/active-directory/role-based-access-control-configure)|Kullanıcıların yalnızca kuruluş içindeki rollerine bağlı olarak erişmeleri gereken kaynaklara erişmelerine olanak sağlamak üzere tasarlanmış bir erişim denetimi özelliği.  |
| [Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis)|Azure'da çok kiracılı, bulut tabanlı dizin ve birden çok kimlik yönetimi hizmetlerini destekleyen bulut tabanlı kimlik doğrulama deposu.  |
| [Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview)|Azure tabanlı uygulamaları kullanırken müşterilerin profillerini nasıl kaydolmaları, oturum açmaları ve yönetmeleri üzerinde denetim sağlayan bir kimlik yönetimi hizmeti.   |
| [Azure Active Directory Domain Services](/azure/active-directory-domain-services/overview)| Active Directory Domain Services'ın bulut tabanlı ve yönetilen bir sürümü. |
| [Azure Çok Faktörlü Kimlik Doğrulama](/azure/active-directory/authentication/multi-factor-authentication)| Güvenli bilgilere erişime izin vermeden önce birkaç farklı kimlik doğrulama ve doğrulama biçimi kullanan bir güvenlik hükmü. |

## <a name="backup-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;Yedekleme](/azure/backup/backup-introduction-to-azure-backup)| Azure bulutundaki verileri yedeklemek ve geri yüklemek için kullanılan Azure tabanlı bir hizmet. |
| [Azure&nbsp;&nbsp;Site Kurtarma](/azure/site-recovery/site-recovery-overview)|Fiziksel ve sanal makinelerde (VM) çalışan iş yüklerini birincil siteden ikincil bir konuma kopyalayan çevrimiçi bir hizmet, bir hatadan sonra hizmetlerin kurtarılmasını etkinleştirir. |

## <a name="networking"></a>Ağ
|Hizmet|Açıklama|
|------|--------|
| [Ağ&nbsp;&nbsp;Güvenlik Grupları](/azure/virtual-network/virtual-networks-nsg)| Kararverme veya reddetme için 5-tuple kullanan ağ tabanlı bir erişim denetimi özelliği.  |
| [Azure VPN Ağ Geçidi](/azure/vpn-gateway/vpn-gateway-about-vpngateways)| Azure Sanal Ağlar'a tesisler arası erişime izin vermek için VPN bitiş noktası olarak kullanılan bir ağ aygıtı.  |
| [Azure Application Gateway](/azure/application-gateway/application-gateway-introduction)|URL'ye göre yönlendirebilen ve SSL boşaltma gerçekleştirebilen gelişmiş bir web uygulama yük dengeleyicisi. |
|[Web uygulaması güvenlik duvarı](/azure/frontdoor/waf-overview) (WAF)|Web uygulamalarınızın yaygın açıklardan ve güvenlik açıklarına karşı merkezi koruma sağlayan Uygulama Ağ Geçidi özelliği|
| [Azure Yük Dengeleyicisi](/azure/load-balancer/load-balancer-overview)|Bir TCP/UDP uygulama ağı yük dengeleyicisi. |
| [Azure ExpressRoute](/azure/expressroute/expressroute-introduction)| Şirket içi ağlar la Azure Sanal Ağlar arasında özel bir WAN bağlantısı. |
| [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview)| Küresel bir DNS yük dengeleyicisi.|
| [Azure Uygulama Proxy](/azure/active-directory/active-directory-application-proxy-get-started)| Şirket içinde barındırılan web uygulamaları için uzaktan erişimi sağlamak için kullanılan kimlik doğrulama ön uç. |
|[Azure Güvenlik Duvarı](/azure/firewall/overview)|Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı ağ güvenliği hizmeti.|
|[Azure DDoS koruması](/azure/virtual-network/ddos-protection-overview)|Uygulama tasarımı ile birlikte en iyi uygulamaları, DDoS saldırılarına karşı savunma sağlar.|
|[Sanal Ağ hizmeti bitiş noktaları](/azure/virtual-network/virtual-network-service-endpoints-overview)|Sanal ağ özel adres alanınızı ve VNet'inizin kimliğini doğrudan bağlantı yla Azure hizmetlerine genişletir.|