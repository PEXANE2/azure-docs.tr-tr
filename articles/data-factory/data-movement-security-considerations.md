---
title: Güvenlikle ilgili dikkat edilmesi gerekenler
description: Azure Veri Fabrikası'ndaki veri hareketi hizmetlerinin verilerinizin güvenliğini sağlamaya yardımcı olmak için kullandığı temel güvenlik altyapısını açıklar.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/11/2020
ms.openlocfilehash: bb3f22223bd64c06cfa4a5f6ffabe7b128dff1d5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416462"
---
#  <a name="security-considerations-for-data-movement-in-azure-data-factory"></a>Azure Veri Fabrikası'nda veri hareketi için güvenlik konuları
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
>
> * [Sürüm 1](v1/data-factory-data-movement-security-considerations.md)
> * [Geçerli sürüm](data-movement-security-considerations.md)

 [!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, Azure Veri Fabrikası'ndaki veri hareketi hizmetlerinin verilerinizin güvenliğini sağlamaya yardımcı olmak için kullandığı temel güvenlik altyapısı açıklanmaktadır. Veri Fabrikası yönetim kaynakları Azure güvenlik altyapısı üzerine kuruludur ve Azure tarafından sunulan tüm olası güvenlik önlemlerini kullanır.

Bir Data Factory çözümünde bir veya daha fazla [işlem hattı](concepts-pipelines-activities.md) oluşturursunuz. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. Bu boru hatları, veri fabrikasının oluşturulduğu bölgede bulunmaktadır. 

Veri Fabrikası yalnızca birkaç bölgede kullanılabilse de, veri uyumluluğu, verimliliği ve azaltılmış ağ çıkış maliyetlerini sağlamak için veri hareketi hizmeti [küresel olarak kullanılabilir.](concepts-integration-runtime.md#integration-runtime-location) 

Azure Veri Fabrikası, sertifikalar kullanılarak şifrelenen bulut veri depoları için bağlantılı hizmet kimlik bilgileri dışında hiçbir veri depolamaz. Veri Fabrikası ile, [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats)arasında veri hareketini ve diğer bölgelerdeki veya şirket içi ortamda [bilgi işlem hizmetlerini](compute-linked-services.md) kullanarak verilerin işlenmesini düzenlemek için veri tabanlı iş akışları oluşturursunuz. Ayrıca SDK'lar ve Azure Monitör'ü kullanarak iş akışlarını izleyebilir ve yönetebilirsiniz.

Veri Fabrikası için sertifikalı olmuştur:

| **[CSA STAR Belgelendirme](https://www.microsoft.com/trustcenter/compliance/csa-star-certification)** |
| :----------------------------------------------------------- |
| **[ISO 20000-01:2011](https://www.microsoft.com/trustcenter/Compliance/ISO-20000-1)** |
| **[ISO 22301:2012](https://www.microsoft.com/trustcenter/compliance/iso-22301)** |
| **[ISO 27001:2013](https://www.microsoft.com/trustcenter/compliance/iso-iec-27001)** |
| **[ISO 27017:2015](https://www.microsoft.com/trustcenter/compliance/iso-iec-27017)** |
| **[ISO 27018:2014](https://www.microsoft.com/trustcenter/compliance/iso-iec-27018)** |
| **[ISO 9001:2015](https://www.microsoft.com/trustcenter/compliance/iso-9001)** |
| **[SOK 1, 2, 3](https://www.microsoft.com/trustcenter/compliance/soc)** |
| **[HIPAA BAA](https://www.microsoft.com/trustcenter/compliance/hipaa)** |

Azure uyumluluğu ve Azure'un kendi altyapısını nasıl güvence altına aldığıyla ilgileniyorsanız, [Microsoft Güven Merkezi'ni](https://microsoft.com/en-us/trustcenter/default.aspx)ziyaret edin. Tüm Azure Uyumluluğu tekliflerinin en https://aka.ms/AzureComplianceson listesi için kontrol edin - .

Bu makalede, aşağıdaki iki veri hareketi senaryosundaki güvenlik hususlarını gözden geçiriyoruz: 

- **Bulut senaryosu**: Bu senaryoda, hem kaynağınıza hem de gideceğiniz yere Internet üzerinden genel olarak erişilebilir. Bunlar arasında Azure Depolama, Azure SQL Veri Ambarı, Azure SQL Veritabanı, Azure Veri Gölü Deposu, Amazon S3, Amazon Redshift, Salesforce gibi SaaS hizmetleri ve FTP ve OData gibi web protokolleri gibi yönetilen bulut depolama hizmetleri yer almaktadır. [Desteklenen veri depolarında ve biçimlerinde](copy-activity-overview.md#supported-data-stores-and-formats)desteklenen veri kaynaklarının tam listesini bulun.
- **Karma senaryo**: Bu senaryoda, kaynağınız veya hedefiniz bir güvenlik duvarının arkasında veya şirket içi bir şirket ağının içindedir. Veya, veri deposu özel bir ağda veya sanal ağda (çoğu zaman kaynak) ve genel olarak erişilemez. Sanal makinelerde barındırılan veritabanı sunucuları da bu senaryonun kapsamına girer.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="cloud-scenarios"></a>Bulut senaryoları

### <a name="securing-data-store-credentials"></a>Veri deposu kimlik bilgilerini güvence altına alma

- **Şifrelenmiş kimlik bilgilerini Azure Veri Fabrikası yönetilen deposunda saklayın.** Veri Fabrikası, Microsoft tarafından yönetilen sertifikalarla şifreleyerek veri depolama kimlik bilgilerinizin korunmasına yardımcı olur. Bu sertifikalar her iki yılda bir döndürülür (sertifika yenileme ve kimlik bilgilerinin geçişini içerir). Azure Depolama güvenliği hakkında daha fazla bilgi için Azure [Depolama güvenliğine genel bakış](../security/fundamentals/storage-overview.md)bilgisine bakın.
- **Kimlik bilgilerini Azure Anahtar Kasası'nda saklayın.** Veri deposunun kimlik bilgisini Azure Key [Vault'ta](https://azure.microsoft.com/services/key-vault/)da depolayabilirsiniz. Veri Fabrikası, bir etkinliğin yürütülmesi sırasında kimlik bilgisini alır. Daha fazla bilgi için [Azure Anahtar Kasası'nda Mağaza kimlik bilgilerine](store-credentials-in-key-vault.md)bakın.

### <a name="data-encryption-in-transit"></a>Aktarımda veri şifreleme
Bulut veri deposu HTTPS veya TLS'yi destekliyorsa, Veri Fabrikası'ndaki veri hareketi hizmetleri ile bulut veri deposu arasındaki tüm veri aktarımları güvenli kanal HTTPS veya TLS üzerinden yapılır.

> [!NOTE]
> Azure SQL Veritabanı ve Azure SQL Veri Ambarı'na yapılan tüm bağlantılar, veri veritabanına ve veritabanından aktarım sırasında şifreleme (SSL/TLS) gerektirir. JSON kullanarak bir ardışık kaynak yazarken, şifreleme özelliğini ekleyin ve bağlantı dizesinde **doğru** şekilde ayarlayın. Azure Depolama için bağlantı dizesinde **HTTPS'yi** kullanabilirsiniz.

> [!NOTE]
> Oracle'dan veri aktarırken aktarım sırasında şifrelemeyi etkinleştirmek için aşağıdaki seçeneklerden birini izleyin:
> 1. Oracle sunucusunda Oracle Advanced Security 'ye (OAS) gidin ve Triple-DES Şifreleme (3DES) ve Gelişmiş Şifreleme Standardı'nı (AES) destekleyen şifreleme ayarlarını yapılandırın ve ayrıntılar için [buraya](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759) bakın. ADF, Oracle'a bağlantı kurarken OAS'de yapılandırdığınız şifreleme yöntemini kullanmak için otomatik olarak görüşür.
> 2. ADF'de, bağlantı dizesinde (Bağlantılı Hizmet'te) EncryptionMethod=1 ekleyebilirsiniz. Bu şifreleme yöntemi olarak SSL/TLS kullanır. Bunu kullanmak için, şifreleme çakışmasını önlemek için Oracle sunucu tarafındaki OAS'daki SSL olmayan şifreleme ayarlarını devre dışı bilmelidir.

> [!NOTE]
> Kullanılan TLS sürümü 1.2'dir.

### <a name="data-encryption-at-rest"></a>Bekleme sırasında veri şifrelemesi
Bazı veri depoları, veri şifrelemesini istirahatte destekler. Bu veri depoları için veri şifreleme mekanizmasını etkinleştirmenizi öneririz. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı
Azure SQL Veri Ambarı'ndaki Saydam Veri Şifreleme (TDE), gerçek zamanlı şifreleme ve verilerinizin şifresini çözme gerçekleştirerek kötü amaçlı etkinlik tehdidine karşı korumaya yardımcı olur. Bu davranış istemci için saydamdır. Daha fazla bilgi için SQL [Veri Ambarı'nda veritabanını güvenli](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-manage-security.md)olarak ele abakın.

#### <a name="azure-sql-database"></a>Azure SQL Veritabanı
Azure SQL Veritabanı, uygulamada değişiklik gerektirmeden, gerçek zamanlı şifreleme ve verilerin şifresini çözerek kötü amaçlı etkinlik tehdidine karşı korumaya yardımcı olan saydam veri şifrelemesini (TDE) de destekler. Bu davranış istemci için saydamdır. Daha fazla bilgi için SQL [Veritabanı ve Veri Ambarı için Saydam veri şifrelemesi'ne](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)bakın.

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Veri Gölü Deposu, hesapta depolanan veriler için şifreleme de sağlar. Etkinleştirildiğinde, Data Lake Store kalıcı olmadan önce verileri otomatik olarak şifreler ve almadan önce şifresini çözer ve verilere erişen istemciye saydam hale getirir. Daha fazla bilgi için [Azure Veri Gölü Deposu'nda Güvenlik'e](../data-lake-store/data-lake-store-security-overview.md)bakın. 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob depolama ve Azure Tablo depolama
Azure Blob depolama ve Azure Tablo depolama, geri almadan önce depolamaya devam etmeden önce verilerinizi otomatik olarak şifreleyen ve şifrelerini çözen Depolama Hizmeti Şifrelemesini (SSE) destekler. Daha fazla bilgi [için, Veriler için Azure Depolama Hizmeti Şifrelemesi'ne](../storage/common/storage-service-encryption.md)bakın.

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3, hem istemci hem de sunucu şifreleme sayılsa da verileri destekler. Daha fazla bilgi için bkz. Şifreleme [Kullanarak Verileri Koruma.](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html)

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift, veri için küme şifrelemesini destekler. Daha fazla bilgi için [Amazon Redshift Veritabanı Şifreleme'ye](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html)bakın. 

#### <a name="salesforce"></a>Salesforce
Salesforce, tüm dosyaların, eklerin ve özel alanların şifrelemesine olanak tanıyan Shield Platform Şifrelemesini destekler. Daha fazla bilgi için bkz: [Web Sunucusu OAuth Kimlik Doğrulama Akışını Anlama.](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm)  

## <a name="hybrid-scenarios"></a>Karma senaryolar
Karma senaryolar, kendi barındırılan tümleştirme çalışma zamanının şirket içi bir ağa, sanal bir ağa (Azure) veya sanal özel bulut (Amazon) içinde yüklenmesini gerektirir. Kendi kendine barındırılan tümleştirme çalışma zamanı yerel veri depolarına erişebilmeli. Kendi kendine barındırılan tümleştirme çalışma zamanı hakkında daha fazla bilgi için, [kendi kendine barındırılan tümleştirme çalışma süresini nasıl oluşturup yapılandırılabildiğini](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)öğrenin. 

![kendi kendine barındırılan tümleştirme çalışma zamanı kanalları](media/data-movement-security-considerations/data-management-gateway-channels.png)

Komut kanalı, Veri Fabrikası'ndaki veri hareketi hizmetleri ile kendi kendine barındırılan tümleştirme çalışma süresi arasındaki iletişimi sağlar. İletişim, etkinlikle ilgili bilgileri içerir. Veri kanalı, şirket içi veri depoları ve bulut veri depoları arasında veri aktarmak için kullanılır.    

### <a name="on-premises-data-store-credentials"></a>Şirket içi veri depolama kimlik bilgileri
Kimlik bilgileri veri fabrikasında depolanabilir veya Azure Key Vault'tan çalışma sırasında [veri fabrikası tarafından başvurulabilir.](store-credentials-in-key-vault.md) Kimlik bilgilerini veri fabrikasında depolıyorsanız, her zaman kendi barındırılan tümleştirme çalışma zamanında şifrelenmiş olarak depolanır. 
 
- **Kimlik bilgilerini yerel olarak depolayın.** JSON'da bağlantı dizeleri ve kimlik bilgileri satır lı **Set-AzDataFactoryV2LinkedService** cmdlet'i doğrudan kullanıyorsanız, bağlantılı hizmet şifrelenir ve kendi barındırılan tümleştirme çalışma zamanında depolanır.  Bu durumda kimlik bilgileri, son derece güvenli olan azure arka uç hizmeti aracılığıyla, sonunda şifrelendiği ve depolandığı kendi kendine barındırılan tümleştirme makinesine akar. Kendi kendine barındırılan tümleştirme çalışma zamanı, hassas verileri ve kimlik bilgilerini şifrelemek için Windows [DPAPI'yi](https://msdn.microsoft.com/library/ms995355.aspx) kullanır.

- **Kimlik bilgilerini Azure Anahtar Kasası'nda saklayın.** Veri deposunun kimlik bilgisini Azure Key [Vault'ta](https://azure.microsoft.com/services/key-vault/)da depolayabilirsiniz. Veri Fabrikası, bir etkinliğin yürütülmesi sırasında kimlik bilgisini alır. Daha fazla bilgi için [Azure Anahtar Kasası'nda Mağaza kimlik bilgilerine](store-credentials-in-key-vault.md)bakın.

- **Kimlik bilgilerini Azure arka ucu üzerinden kendi barındırılan tümleştirme çalışma süresine akmadan yerel olarak depolayın.** Kimlik bilgilerini, kimlik bilgilerini veri fabrikası arka uçlarından aktamak zorunda kalmadan, kimlik bilgilerini kendi barındırılan tümleştirme çalışma zamanında yerel olarak şifrelemek ve depolamak istiyorsanız, [Azure Veri Fabrikası'ndaki şirket içi veri depoları için kimlik bilgilerini şifreleme](encrypt-credentials-self-hosted-integration-runtime.md)adımlarını izleyin. Tüm bağlayıcılar bu seçeneği destekler. Kendi kendine barındırılan tümleştirme çalışma zamanı, hassas verileri ve kimlik bilgilerini şifrelemek için Windows [DPAPI'yi](https://msdn.microsoft.com/library/ms995355.aspx) kullanır. 

   Bağlantılı hizmet kimlik bilgilerini ve bağlantılı hizmetteki hassas ayrıntıları şifrelemek için **Yeni AzDataFactoryV2LinkedServiceEncryptedCredential** cmdlet'i kullanın. Daha **sonra, Set-AzDataFactoryV2LinkedService** cmdlet'i kullanarak bağlantılı bir hizmet oluşturmak için JSON döndürülen (bağlantı dizesinde **Şifrelenmiş Credential** öğesi ile) kullanabilirsiniz.  


#### <a name="ports-used-when-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Bağlantılı hizmeti, barındırılan tümleştirme çalışma zamanında şifrelerken kullanılan bağlantı noktaları
Varsayılan olarak PowerShell, güvenli iletişim için kendi kendine barındırılan tümleştirme çalışma süresine sahip makinede 8060 portunu kullanır. Gerekirse, bu bağlantı noktası değiştirilebilir.  

![Ağ geçidi için HTTPS bağlantı noktası](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Tüm veri aktarımları, Azure hizmetleriyle iletişim sırasında ortadaki adam saldırılarını önlemek için TCP üzerinden güvenli kanal HTTPS ve TLS üzerinden yapılır.

Şirket içi ağınızla Azure arasındaki iletişim kanalını daha da güvenli hale getirmek için [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) veya [Azure ExpressRoute'u](../expressroute/expressroute-introduction.md) da kullanabilirsiniz.

Azure Sanal Ağ, ağınızın buluttaki mantıksal bir temsilidir. IPSec VPN (siteden siteye) veya ExpressRoute (özel bakış) ayarlayarak şirket içi ağı sanal ağınıza bağlayabilirsiniz.    

Aşağıdaki tablo, karma veri hareketi için farklı kaynak ve hedef konum kombinasyonlarını temel alan ağ ve kendi kendine barındırılan tümleştirme çalışma zamanı yapılandırma önerilerini özetler.

| Kaynak      | Hedef                              | Ağ yapılandırması                    | Tümleştirme çalışma zamanı kurulumu                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Şirket içi | Sanal ağlarda dağıtılan sanal makineler ve bulut hizmetleri | IPSec VPN (noktadan siteye veya siteden siteye) | Kendi kendine barındırılan tümleştirme çalışma süresi, sanal ağdaki bir Azure sanal makinesine yüklenmelidir.  |
| Şirket içi | Sanal ağlarda dağıtılan sanal makineler ve bulut hizmetleri | ExpressRoute (özel akran)           | Kendi kendine barındırılan tümleştirme çalışma süresi, sanal ağdaki bir Azure sanal makinesine yüklenmelidir.  |
| Şirket içi | Genel bitiş noktası olan Azure tabanlı hizmetler | ExpressRoute (Microsoft peering)            | Kendi kendine barındırılan tümleştirme çalışma süresi şirket içinde veya Azure sanal makinesine yüklenebilir. |

Aşağıdaki resimler, ExpressRoute ve IPSec VPN (Azure Sanal Ağı ile) kullanarak verileri şirket içi veritabanı ile Azure hizmetleri arasında taşımak için kendi barındırılan tümleştirme çalışma süresini gösterir:

**ExpressRoute**

![Ağ geçidi ile ExpressRoute'u kullanma](media/data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN**

![Ağ geçidi ile IPSec VPN](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-allow-list-setting-up-for-ip-addresses"></a><a name="firewall-configurations-and-allow-list-setting-up-for-ip-address-of-gateway"></a>Güvenlik duvarı yapılandırmaları ve IP adresleri için liste ayarlamaya izin verme

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Şirket içi/özel ağ için güvenlik duvarı gereksinimleri    
Bir kuruluşta, kuruluşun merkezi yönlendiricisinde kurumsal bir güvenlik duvarı çalışır. Windows Güvenlik Duvarı, kendi kendine barındırılan tümleştirme çalışma zamanının yüklendiği yerel makinede bir daemon olarak çalışır. 

Aşağıdaki tablo, şirket güvenlik duvarları için giden bağlantı noktası ve etki alanı gereksinimlerini sağlar:

[!INCLUDE [domain-and-outbound-port-requirements](../../includes/domain-and-outbound-port-requirements.md)]

> [!NOTE] 
> İlgili veri kaynaklarının gerektirdiği şekilde, şirket güvenlik duvarı düzeyindeki etki alanları için bağlantı noktalarını yönetmeniz veya izin verme listenizi ayarlamanız gerekebilir. Bu tabloda yalnızca Azure SQL Veritabanı, Azure SQL Veri Ambarı ve Azure Veri Gölü Deposu örnek olarak kullanılır.   

Aşağıdaki tablo, Windows Güvenlik Duvarı için gelen bağlantı noktası gereksinimlerini sağlar:

| Gelen bağlantı noktaları | Açıklama                              |
| ------------- | ---------------------------------------- |
| 8060 (TCP)    | Azure Veri Fabrikası'ndaki şirket içi [veri depoları için Şifreleme kimlik bilgilerinde](encrypt-credentials-self-hosted-integration-runtime.md)açıklandığı gibi PowerShell şifreleme cmdlet'i ve kendi barındırılan tümleştirme çalışma zamanında şirket içi veri depoları için kimlik bilgilerini güvenli bir şekilde ayarlamak için kimlik bilgisi yöneticisi uygulaması tarafından gereklidir. |

![Ağ geçidi bağlantı noktası gereksinimleri](media/data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-and-allow-list-setting-up-in-data-stores"></a>IP yapılandırmaları ve veri depolarında liste kurulumuna izin verme
Buluttaki bazı veri depoları, mağazaya erişen makinenin IP adresine izin vermenizi de gerektirir. Kendi kendine barındırılan tümleştirme çalışma zamanı makinesinin IP adresine izin verildiğinden veya güvenlik duvarında uygun şekilde yapılandırıldığından emin olun.

Aşağıdaki bulut veri depoları, kendi barındırılan tümleştirme çalışma zamanı makinesinin IP adresine izin vermenizi gerektirir. Bu veri depolarından bazıları, varsayılan olarak izin listesi gerektirmeyebilir. 

- [Azure SQL Veritabanı](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Veri Ambarı](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../cosmos-db/firewall-support.md)
- [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Kendi kendine barındırılan tümleştirme çalışma süresi farklı veri fabrikalarında paylaşılabilir mi?**

Evet. Diğer ayrıntıları [burada](https://azure.microsoft.com/blog/sharing-a-self-hosted-integration-runtime-infrastructure-with-multiple-data-factories/) bulabilirsiniz.

**Kendi kendine barındırılan tümleştirme çalışma zamanının çalışması için bağlantı noktası gereksinimleri nelerdir?**

Kendi kendine barındırılan tümleştirme çalışma süresi, Internet'e erişmek için HTTP tabanlı bağlantılar sağlar. Bu bağlantıyı kurabilmesi için, giden bağlantı noktaları 443'ün kendi kendine barındırılan tümleştirme çalışma süresi için açılması gerekir. Gelen bağlantı noktası 8060'ı yalnızca kimlik bilgisi yöneticisi uygulaması için makine düzeyinde (kurumsal güvenlik duvarı düzeyinde değil) açın. Azure SQL Veritabanı veya Azure SQL Veri Ambarı kaynak veya hedef olarak kullanılıyorsa, 1433 portu da açmanız gerekir. Daha fazla bilgi için Güvenlik Duvarı yapılandırmalarına bakın ve IP adresleri bölümü [için liste ayarlamalarına izin verin.](#firewall-configurations-and-allow-list-setting-up-for-ip-address-of-gateway) 


## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası Kopyalama Etkinliği performansı hakkında daha fazla bilgi için Bkz. [Kopyalama Etkinliği performansı ve atokskılavuzu.](copy-activity-performance.md)

 
