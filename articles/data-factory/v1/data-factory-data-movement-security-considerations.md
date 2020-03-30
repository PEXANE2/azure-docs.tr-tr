---
title: Azure Veri Fabrikası'nda veri hareketi için güvenlik konuları
description: Azure Veri Fabrikası'nda veri hareketini güvence altına alma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 1f19d258531e5368238cba72c986aede3f4a64ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80130830"
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Veri Fabrikası - Veri hareketi için güvenlik hususları

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası için veri hareketi güvenlik hususlarına](../data-movement-security-considerations.md)bakın.

## <a name="introduction"></a>Giriş
Bu makalede, Azure Veri Fabrikası'ndaki veri hareketi hizmetlerinin verilerinizi güvence altına almak için kullandığı temel güvenlik altyapısı açıklanmaktadır. Azure Veri Fabrikası yönetim kaynakları Azure güvenlik altyapısı nda oluşturulur ve Azure tarafından sunulan tüm olası güvenlik önlemlerini kullanır.

Bir Data Factory çözümünde bir veya daha fazla [işlem hattı](data-factory-create-pipelines.md) oluşturursunuz. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. Bu boru hatları, veri fabrikasının oluşturulduğu bölgede bulunmaktadır. 

Veri Fabrikası sadece Batı **ABD,** **Doğu ABD**ve **Kuzey Avrupa** bölgelerinde mevcut olsa da, veri hareketi hizmeti çeşitli bölgelerde [küresel olarak](data-factory-data-movement-activities.md#global)kullanılabilir. Veri Hareketi hizmeti, veri hareketi hizmeti henüz o bölgeye dağıtılmadığı takdirde, hizmete alternatif bir bölgeyi kullanması için açıkça talimat vermedikçe, verilerin coğrafi bir bölgeden/ bölgeden ayrılmamasını sağlar. 

Azure Veri Fabrikası'nın kendisi, sertifikalar kullanılarak şifrelenen bulut veri depoları için bağlantılı hizmet kimlik bilgileri dışında hiçbir veri depolamaz. Veri hareketini [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) arasında, verilerin işlenmesini de başka bölgelerde veya şirket içi bir ortamda [işlem hizmetleri](data-factory-compute-linked-services.md) kullanarak düzenlemek için veri temelinde iş akışları oluşturmanızı sağlar. Hem programlama, hem de kullanıcı arabirimi mekanizmalarını kullanarak [iş akışlarını izlemenizi ve yönetmenizi](data-factory-monitor-manage-pipelines.md) de sağlar.

Azure Veri Fabrikası'nı kullanarak veri hareketi şu şekilde **onaylanmıştır:**
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA)  
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018) 
-   [CSA YILDIZ](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)
     
Azure uyumluluğu ve Azure'un kendi altyapısını nasıl güvence altına aldığıyla ilgileniyorsanız, [Microsoft Güven Merkezi'ni](https://microsoft.com/en-us/trustcenter/default.aspx)ziyaret edin. 

Bu makalede, aşağıdaki iki veri hareketi senaryosundaki güvenlik hususlarını gözden geçiriyoruz: 

- **Bulut senaryosu**- Bu senaryoda, hem kaynağınıza hem de hedefinize internet üzerinden genel olarak erişilebilir. Bunlar arasında Azure Depolama, Azure SQL Veri Ambarı, Azure SQL Veritabanı, Azure Veri Gölü Deposu, Amazon S3, Amazon Redshift, Salesforce gibi SaaS hizmetleri ve FTP ve OData gibi web protokolleri gibi yönetilen bulut depolama hizmetleri yer almaktadır. Desteklenen veri kaynaklarının tam listesini [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats)bulabilirsiniz.
- **Karma senaryo**- Bu senaryoda, kaynağınız veya hedefiniz bir güvenlik duvarının arkasında veya şirket içi bir şirket ağının içinde veya veri deposu özel bir ağda/ sanal ağda (çoğu zaman kaynak) ve herkese açık değildir. Sanal makinelerde barındırılan veritabanı sunucuları da bu senaryonun kapsamına girer.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="cloud-scenarios"></a>Bulut senaryoları
### <a name="securing-data-store-credentials"></a>Veri deposu kimlik bilgilerini güvence altına alma
Azure Veri Fabrikası, **Microsoft tarafından yönetilen sertifikaları**kullanarak veri deposu kimlik bilgilerinizi **şifreleyerek** korur. Bu sertifikalar her **iki yılda** bir döndürülür (sertifika yenileme ve kimlik bilgilerinin geçişini içerir). Bu şifrelenmiş kimlik bilgileri, **Azure Veri Fabrikası yönetim hizmetleri tarafından yönetilen**bir Azure Depolama'da güvenli bir şekilde depolanır. Azure Depolama güvenliği hakkında daha fazla bilgi için [Azure Depolama Güvenliği'ne genel bakış](../../security/fundamentals/storage-overview.md)bilgisine bakın.

### <a name="data-encryption-in-transit"></a>Aktarımda veri şifreleme
Bulut veri deposu HTTPS veya TLS'yi destekliyorsa, Veri Fabrikası'ndaki veri hareketi hizmetleri ile bulut veri deposu arasındaki tüm veri aktarımları güvenli kanal HTTPS veya TLS üzerinden yapılır.

> [!NOTE]
> Azure SQL **Veritabanı** ve **Azure SQL Veri Ambarı'na** yapılan tüm bağlantılar, veri veritabanına ve veritabanından aktarım sırasında her zaman şifreleme (SSL/TLS) gerektirir. Bir JSON düzenleyicisi kullanarak bir ardışık yol üzerinde bir yardımcı kaynak yazarken, **şifreleme** özelliğini ekleyin ve **bağlantı dizesinde** **doğru** şekilde ayarlayın. [Kopya Sihirbazı'nı](data-factory-azure-copy-wizard.md)kullandığınızda sihirbaz bu özelliği varsayılan olarak ayarlar. **Azure Depolama**için bağlantı dizesinde **HTTPS'yi** kullanabilirsiniz.

### <a name="data-encryption-at-rest"></a>Bekleme sırasında veri şifrelemesi
Bazı veri depoları, veri şifrelemesini istirahatte destekler. Bu veri depoları için veri şifreleme mekanizmasını etkinleştirmenizi öneririz. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı
Azure SQL Veri Ambarı'ndaki Saydam Veri Şifreleme (TDE), gerçek zamanlı şifreleme gerçekleştirerek ve verilerinizin şifresini tam olarak çözerek kötü amaçlı etkinlik tehdidine karşı koruma sağlar. Bu davranış istemci için saydamdır. Daha fazla bilgi için SQL [Veri Ambarı'nda veritabanını güvenli](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-manage-security.md)olarak ele abakın.

#### <a name="azure-sql-database"></a>Azure SQL Veritabanı
Azure SQL Veritabanı ayrıca, uygulamada değişiklik gerektirmeden gerçek zamanlı şifreleme ve verilerin şifresini çözme gerçekleştirerek kötü amaçlı etkinlik tehdidine karşı koruma sağlayan saydam veri şifrelemesini (TDE) de destekler. Bu davranış istemci için saydamdır. Daha fazla bilgi için Azure [SQL Veritabanı ile Saydam Veri Şifreleme'ye](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)bakın. 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Veri Gölü deposu, hesapta depolanan veriler için şifreleme de sağlar. Etkinleştirildiğinde, Veri Gölü deposu kalıcı olmadan önce verileri otomatik olarak şifreler ve almadan önce şifresini çözer, bu da verileri erişen istemciye saydam hale getirir. Daha fazla bilgi için [Azure Veri Gölü Deposu'nda Güvenlik'e](../../data-lake-store/data-lake-store-security-overview.md)bakın. 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Depolama ve Azure Tablo Depolama
Azure Blob Depolama ve Azure Tablo depolama, depolamaya devam etmeden önce verilerinizi otomatik olarak şifreleyen ve almadan önce şifrelerini çözen Depolama Hizmeti Şifrelemesini (SSE) destekler. Daha fazla bilgi [için, Veriler için Azure Depolama Hizmeti Şifrelemesi'ne](../../storage/common/storage-service-encryption.md)bakın.

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3, Rest'te hem istemci hem de sunucu şifreleme verilerini destekler. Daha fazla bilgi için bkz. Şifreleme [Kullanarak Verileri Koruma.](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html) Şu anda, Veri Fabrikası sanal bir özel bulut (VPC) içinde Amazon S3 desteklemiyor.

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift, veri için küme şifrelemesini destekler. Daha fazla bilgi için [Amazon Redshift Veritabanı Şifreleme'ye](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html)bakın. Şu anda, Veri Fabrikası bir VPC içinde Amazon Redshift desteklemiyor. 

#### <a name="salesforce"></a>Salesforce
Salesforce, tüm dosyaların, eklerin, özel alanların şifrelemesine olanak tanıyan Shield Platform Şifrelemesini destekler. Daha fazla bilgi için bkz: [Web Sunucusu OAuth Kimlik Doğrulama Akışını Anlama.](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm)  

## <a name="hybrid-scenarios-using-data-management-gateway"></a>Karma Senaryolar (Veri Yönetimi Ağ Geçidi'ni kullanarak)
Karma senaryolar, Veri Yönetimi Ağ Geçidi'nin şirket içi bir ağa veya sanal ağ (Azure) veya sanal özel bulut (Amazon) içinde yüklenmesini gerektirir. Ağ geçidi yerel veri depolarına erişebilmeli. Ağ geçidi hakkında daha fazla bilgi için [Veri Yönetimi Ağ Geçidi'ne](data-factory-data-management-gateway.md)bakın. 

![Veri Yönetimi Ağ Geçidi kanalları](media/data-factory-data-movement-security-considerations/data-management-gateway-channels.png)

**Komut kanalı,** Veri Fabrikası ve Veri Yönetimi Ağ Geçidi'ndeki veri hareketi hizmetleri arasındaki iletişimi sağlar. İletişim, etkinlikle ilgili bilgileri içerir. Veri kanalı, şirket içi veri depoları ve bulut veri depoları arasında veri aktarmak için kullanılır.    

### <a name="on-premises-data-store-credentials"></a>Şirket içi veri depolama kimlik bilgileri
Şirket içi veri depolarınızın kimlik bilgileri yerel olarak depolanır (bulutta değil). Onlar üç farklı şekilde ayarlanabilir. 

- Azure Portalı/Kopyalama Sihirbazı'ndan HTTPS üzerinden **düz metin** (daha az güvenli) kullanma. Kimlik bilgileri, şirket içi ağ geçidine düz metin olarak geçirilir.
- **Kopya Sihirbazı'ndan JavaScript Şifreleme kitaplığını**kullanma.
- **Tıklama tabanlı kimlik bilgileri yöneticisi uygulamasını kullanma.** Tıkla-bir uygulama ağ geçidine erişimi olan şirket içi makinede yürütülür ve veri deposu için kimlik bilgileri ayarlar. Bu seçenek ve bir sonraki en güvenli seçeneklerdir. Kimlik bilgisi yöneticisi uygulaması, varsayılan olarak, güvenli iletişim için ağ geçidi ne olan makinede 8050 bağlantı noktasını kullanır.  
- Kimlik bilgilerini şifrelemek için [New-AzDataFactoryEncryptValue](/powershell/module/az.datafactory/New-azDataFactoryEncryptValue) PowerShell cmdlet kullanın. Cmdlet, kimlik bilgilerini şifrelemek için ağ geçidinin kullanacağı sertifikayı kullanır. Bu cmdlet tarafından döndürülen şifrelenmiş kimlik bilgilerini kullanabilir ve [Yeni-AzDataFactoryLinkedService](/powershell/module/az.datafactory/new-azdatafactorylinkedservice) cmdlet ile kullandığınız JSON dosyasındaki **connectionString'in** **EncryptedCredential** öğesine veya portaldaki Veri Fabrikası Düzenleyicisi'ndeki JSON parçacığına ekleyebilirsiniz. Bu seçenek ve tıklama-bir kez uygulama en güvenli seçeneklerdir. 

#### <a name="javascript-cryptography-library-based-encryption"></a>JavaScript şifreleme kitaplığı tabanlı şifreleme
[Kopya](data-factory-copy-wizard.md)Sihirbazı'ndan [JavaScript Şifreleme kitaplığını](https://www.microsoft.com/download/details.aspx?id=52439) kullanarak veri deposu kimlik bilgilerini şifreleyebilirsiniz. Bu seçeneği seçtiğinizde, Kopya Sihirbazı ağ geçidinin ortak anahtarını alır ve veri deposu kimlik bilgilerini şifrelemek için kullanır. Kimlik bilgileri ağ geçidi makinesi tarafından deşifre edilir ve Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx)tarafından korunur.

**Desteklenen tarayıcılar:** IE8, IE9, IE10, IE11, Microsoft Edge ve en son Firefox, Chrome, Opera, Safari tarayıcıları. 

#### <a name="click-once-credentials-manager-app"></a>Bir kez kimlik bilgileri yöneticisi uygulamasına tıklayın
Ardışık hatlar yazarken Azure portalı/Kopyalama Sihirbazı'ndan tıklama tabanlı kimlik yöneticisi uygulamasını başlatabilirsiniz. Bu uygulama, kimlik bilgilerinin tel üzerinde düz metin olarak aktarılmamasını sağlar. Varsayılan olarak, güvenli iletişim için ağ geçidi olan makinede **8050** bağlantı noktasını kullanır. Gerekirse, bu bağlantı noktası değiştirilebilir.  
  
![Ağ geçidi için HTTPS bağlantı noktası](media/data-factory-data-movement-security-considerations/https-port-for-gateway.png)

Şu anda, Veri Yönetimi Ağ Geçidi tek bir **sertifika**kullanır. Bu sertifika ağ geçidi yüklemesi sırasında oluşturulur (Kasım 2016'dan sonra oluşturulan Veri Yönetimi Ağ Geçidi ve sürüm 2.4.xxxx.x veya sonraki sürümiçin geçerlidir). Bu sertifikayı kendi SSL/TLS sertifikanızla değiştirebilirsiniz. Bu sertifika, veri deposu kimlik bilgilerini ayarlamak için ağ geçidi makinesine güvenli bir şekilde bağlanmak için tıklama bir kez kimlik yöneticisi uygulaması tarafından kullanılır. Ağ geçidi olan makinede Windows [DPAPI'yi](https://msdn.microsoft.com/library/ms995355.aspx) kullanarak veri deposu kimlik bilgilerini şirket içinde güvenli bir şekilde saklar. 

> [!NOTE]
> Kasım 2016'dan önce veya sürüm 2.3.xxxx.x sürümünden önce yüklenen eski ağ geçitleri, şifrelenmiş ve bulutta depolanan kimlik bilgilerini kullanmaya devam eder. Ağ geçidini en son sürüme yükseltseniz bile, kimlik bilgileri şirket içi makineye geçirilemez    
  
| Ağ geçidi sürümü (oluşturma sırasında) | Depolanan Kimlik Bilgileri | Kimlik bilgisi şifreleme/ güvenlik | 
| --------------------------------- | ------------------ | --------- |  
| < = 2.3.xxxx.x | Bulut üzerinde | Sertifika kullanılarak şifrelenmiş (Kimlik Bilgisi yöneticisi uygulaması tarafından kullanılandan farklı) | 
| > = 2.4.xxxx.x | Şirket içinde | DPAPI ile güvenli | 
  

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Tüm veri aktarımları, Azure hizmetleriyle iletişim sırasında ortadaki adam saldırılarını önlemek için **TCP üzerinden** güvenli kanal **HTTPS** ve TLS üzerinden yapılır.
 
Şirket içi ağınızla Azure arasındaki iletişim kanalını daha da güvenli hale getirmek için [IPSec VPN](../../vpn-gateway/vpn-gateway-about-vpn-devices.md) veya [Express Route'u](../../expressroute/expressroute-introduction.md) da kullanabilirsiniz.

Sanal ağ, ağınızın buluttaki mantıksal bir temsilidir. IPSec VPN (siteden siteye) veya Express Route (Özel Eşleme) ayarlayarak bir şirket içi ağı Azure sanal ağınıza (VNet) bağlayabilirsiniz     

Aşağıdaki tablo, karma veri hareketi için kaynak ve hedef konumlarının farklı birleşimlerini temel alan ağ ve ağ geçidi yapılandırma önerilerini özetler.

| Kaynak | Hedef | Ağ yapılandırması | Ağ geçidi kurulumu |
| ------ | ----------- | --------------------- | ------------- | 
| Şirket içi | Sanal ağlarda dağıtılan sanal makineler ve bulut hizmetleri | IPSec VPN (noktadan siteye veya siteden siteye) | Ağ geçidi, VNet'te şirket içinde veya bir Azure sanal makinesine (VM) yüklenebilir | 
| Şirket içi | Sanal ağlarda dağıtılan sanal makineler ve bulut hizmetleri | ExpressRoute (Özel Peering) | Ağ geçidi, Şirket içinde veya VNet'te azure VM'de yüklenebilir | 
| Şirket içi | Genel bitiş noktası olan Azure tabanlı hizmetler | ExpressRoute (Genel Bakış) | Ağ geçidi şirket içinde kurulmalıdır | 

Aşağıdaki resimler, Express rotası ve IPSec VPN (Virtual Network ile) kullanarak verileri şirket içi veritabanı ile Azure hizmetleri arasında taşımak için Veri Yönetimi Ağ Geçidi'nin kullanımını gösterir:

**Ekspres Rota:**
 
![Ağ geçidi ile Express Rotasını Kullanma](media/data-factory-data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN:**

![Ağ geçidi ile IPSec VPN](media/data-factory-data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a>Güvenlik duvarı yapılandırmaları ve ağ geçidinin IP adresini beyaz listeye alma

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Şirket içi/özel ağ için güvenlik duvarı gereksinimleri  
Bir kuruluşta, kuruluşun merkezi yönlendiricisinde kurumsal bir **güvenlik duvarı** çalışır. Ayrıca, **Windows güvenlik duvarı** ağ geçidinin yüklü olduğu yerel makinede bir daemon olarak çalışır. 

Aşağıdaki **tablo, şirket güvenlik duvarı**için **giden bağlantı noktası** ve etki alanı gereksinimleri sağlar.

| Etki alanı adları | Giden bağlantı noktaları | Açıklama |
| ------------ | -------------- | ----------- | 
| `*.servicebus.windows.net` | 443, 80 | Veri Fabrikası'ndaki veri hareket hizmetlerine bağlanmak için ağ geçidi tarafından gerekli |
| `*.core.windows.net` | 443 | [Sahnelenen kopyalama](data-factory-copy-activity-performance.md#staged-copy) özelliğini kullandığınızda Azure Depolama Hesabı'na bağlanmak için ağ geçidi tarafından kullanılır. | 
| `*.frontend.clouddatahub.net` | 443 | Azure Veri Fabrikası hizmetine bağlanmak için ağ geçidi tarafından gereklidir. | 
| `*.database.windows.net` | 1433   | (İstEĞE BAĞLI) hedefiniz Azure SQL Veritabanı/ Azure SQL Veri Ambarı olduğunda gereklidir. 1433 bağlantı noktasını açmadan verileri Azure SQL Veritabanı/Azure SQL Veri Ambarı'na kopyalamak için aşamalı kopyalama özelliğini kullanın. | 
| `*.azuredatalakestore.net` | 443 | (İstEĞE BAĞLI) hedefiniz Azure Veri Gölü deposu olduğunda gereklidir | 

> [!NOTE] 
> İlgili veri kaynaklarının gerektirdiği şekilde, bağlantı noktalarını/ beyaz liste alan adlarını şirket güvenlik duvarı düzeyinde yönetmeniz gerekebilir. Bu tabloda yalnızca Azure SQL Veritabanı, Azure SQL Veri Ambarı, Azure Veri Gölü Deposu örnek olarak kullanılır.   

Aşağıdaki **tablo, windows güvenlik duvarı**için **gelen bağlantı noktası** gereksinimlerini sağlar.

| Gelen bağlantı noktaları | Açıklama | 
| ------------- | ----------- | 
| 8050 (TCP) | Ağ geçidinde şirket içi veri depoları için kimlik bilgilerini güvenli bir şekilde ayarlamak için kimlik bilgisi yöneticisi uygulaması tarafından gereklidir. | 

![Ağ geçidi bağlantı noktası gereksinimleri](media/data-factory-data-movement-security-considerations/gateway-port-requirements.png)

#### <a name="ip-configurations-whitelisting-in-data-store"></a>Veri deposunda IP yapılandırmaları/ beyaz liste
Buluttaki bazı veri depoları da bunlara erişen makinenin IP adresinin beyaz listesini gerektirir. Ağ geçidi makinesinin IP adresinin güvenlik duvarında uygun şekilde beyaz listeye alındığından/ yapılandırıldığından emin olun.

Aşağıdaki bulut veri depoları ağ geçidi makinesinin IP adresinin beyaz listesini gerektirir. Bu veri depolarından bazıları, varsayılan olarak, IP adresinin beyaz listesini gerektirmeyebilir. 

- [Azure SQL Veritabanı](../../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Veri Ambarı](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../../cosmos-db/firewall-support.md)
- [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Soru:** Ağ Geçidi farklı veri fabrikalarında paylaşılabilir mi?
**Cevap:** Bu özelliği henüz destekliyoruz. Üzerinde çalışmaya devam ediyoruz.

**Soru:** Ağ geçidinin çalışması için bağlantı noktası gereksinimleri nelerdir?
**Cevap:** Ağ Geçidi, internet için HTTP tabanlı bağlantılar sağlıyor. Bu bağlantıyı yapmak için **443 ve 80 giden bağlantı noktaları** ağ geçidi için açılmalıdır. Gelen **Bağlantı Noktası 8050'yi** yalnızca Kimlik Bilgisi Yöneticisi uygulaması için makine düzeyinde (kurumsal güvenlik duvarı düzeyinde değil) açın. Azure SQL Veritabanı veya Azure SQL Veri Ambarı kaynak/ hedef olarak kullanılıyorsa, **1433** bağlantı noktasını da açmanız gerekir. Daha fazla bilgi için [Güvenlik Duvarı yapılandırmaları ve IP adreslerini beyaz listeye alma](#firewall-configurations-and-whitelisting-ip-address-of gateway) bölümüne bakın. 

**Soru:** Ağ Geçidi için sertifika gereksinimleri nelerdir?
**Cevap:** Geçerli ağ geçidi, veri deposu kimlik bilgilerini güvenli bir şekilde ayarlamak için kimlik bilgisi yöneticisi uygulaması tarafından kullanılan bir sertifika gerektirir. Bu sertifika, ağ geçidi kurulumu tarafından oluşturulan ve yapılandırılan kendi imzalı bir sertifikadır. Bunun yerine kendi TLS/SSL sertifikanızı kullanabilirsiniz. Daha fazla bilgi [için, bir kez kimlik bilgisi yöneticisi uygulama](#click-once-credentials-manager-app) bölümüne bakın. 

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliğinin performansı hakkında daha fazla bilgi için [bkz.](data-factory-copy-activity-performance.md)

 
