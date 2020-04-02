---
title: Azure şifrelemeye genel bakış | Microsoft Dokümanlar
description: Azure'da çeşitli şifreleme seçenekleri hakkında bilgi edinin
services: security
documentationcenter: na
author: Barclayn
manager: barbkess
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: mbaldwin
ms.openlocfilehash: 7ac657e6627aff4eb588506118e5eeeaef70efba
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548604"
---
# <a name="azure-encryption-overview"></a>Azure şifrelemeye genel bakış

Bu makalede, Microsoft Azure'da şifrelemenin nasıl kullanıldığına genel bir bakış sağlanmaktadır. Azure Key Vault ile istirahatte şifreleme, uçuşta şifreleme ve anahtar yönetimi gibi önemli şifreleme alanlarını kapsar. Her bölümde daha ayrıntılı bilgi bağlantıları içerir.

## <a name="encryption-of-data-at-rest"></a>Verilerin istirahatte şifrelemi

Geri kalan veriler, fiziksel ortamda, herhangi bir dijital biçimde kalıcı depolama da bulunan bilgileri içerir. Ortam, manyetik veya optik ortam, arşivlenmiş veriler ve veri yedeklemeleri ile ilgili dosyaları içerebilir. Microsoft Azure, dosya, disk, blob ve tablo depolama gibi farklı gereksinimleri karşılamak için çeşitli veri depolama çözümleri sunar. Microsoft ayrıca Azure [SQL Veritabanı,](../../sql-database/sql-database-technical-overview.md) [Azure Cosmos DB](../../data-factory/introduction.md)ve Azure Veri Gölü'nü korumak için şifreleme sağlar.

Veri şifreleme, hizmet olarak yazılım (SaaS), hizmet olarak platform (PaaS) ve hizmet olarak altyapı (IaaS) bulut modelleri olarak yazılım genelindeki hizmetler için kullanılabilir. Bu makalede, Azure şifreleme seçeneklerini kullanmanıza yardımcı olacak kaynaklar özetler ve sağlar.

Beklemedeki verilerin Azure'da nasıl şifrelendiğine daha ayrıntılı bir tartışma için Azure [Veri Şifreleme-at-Rest](encryption-atrest.md)bölümüne bakın.

## <a name="azure-encryption-models"></a>Azure şifreleme modelleri

Azure, hizmet tarafından yönetilen anahtarları kullanan sunucu tarafı şifrelemesi, Key Vault'ta müşteri tarafından yönetilen anahtarlar veya müşteri tarafından denetlenebilen donanımda müşteri tarafından yönetilen anahtarlar gibi çeşitli şifreleme modellerini destekler. İstemci tarafı şifreleme ile anahtarları şirket içinde veya başka bir güvenli konumda yönetebilir ve depolayabilirsiniz.

### <a name="client-side-encryption"></a>İstemci Tarafında Şifreleme

İstemci tarafı şifreleme, Azure dışında gerçekleştirilir. Bu içerir:

- Müşterinin veri merkezinde çalışan bir uygulama veya bir hizmet uygulaması tarafından şifrelenen veriler.
- Azure tarafından alındığı zaman zaten şifrelenmiş veriler.

İstemci tarafı şifrelemeile bulut hizmeti sağlayıcılarının şifreleme anahtarlarına erişimi yoktur ve bu verilerin şifresini çözemez. Anahtarların kontrolünü tamamen sen sağla.

### <a name="server-side-encryption"></a>Sunucu tarafı şifrelemesi

Sunucu tarafındaki üç şifreleme modeli, gereksinimlerinize göre seçebileceğiniz farklı temel yönetim özellikleri sunar:

- **Servis tarafından yönetilen tuşlar**: Düşük ek yükü ile kontrol ve kolaylık kombinasyonunu sağlar.

- **Müşteri tarafından yönetilen anahtarlar**: Kendi Anahtarlarınızı Getir (BYOK) desteği de dahil olmak üzere anahtarlar üzerinde kontrol sağlar veya yenilerini oluşturmanıza olanak tanır.

- **Müşteri kontrolündeki donanımda servis tarafından yönetilen anahtarlar**: Microsoft denetimi dışında, özel deponuzdaki anahtarları yönetmenize olanak tanır. Bu özellik Host Your Own Key (HYOK) olarak adlandırılır. Ancak yapılandırma karmaşıktır ve çoğu Azure hizmeti bu modeli desteklemez.

### <a name="azure-disk-encryption"></a>Azure disk şifreleme

[Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) teknolojisini ve Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) teknolojisini kullanan [Azure disk şifrelemesini](/azure/security/fundamentals/azure-disk-encryption-vms-vmss)kullanarak Hem işletim sistemi disklerini hem de veri disklerini tam birim şifrelemeyle koruyarak Windows ve Linux sanal makinelerini koruyabilirsiniz.

[Azure Key Vault aboneliğinizde](../../key-vault/key-vault-overview.md)şifreleme anahtarları ve sırları korunur. Azure Yedekleme hizmetini kullanarak, Anahtar Şifreleme Anahtarı (KEK) yapılandırmasını kullanan şifreli sanal makineleri (VM) yedekleyebilir ve geri yükleyebilirsiniz.

### <a name="azure-storage-service-encryption"></a>Azure Depolama Hizmeti Şifrelemesi

Azure Blob depolama ve Azure dosya paylaşımlarında kalan veriler hem sunucu hem de istemci tarafındaki senaryolarda şifrelenebilir.

[Azure Depolama Hizmeti Şifrelemesi (SSE),](../../storage/common/storage-service-encryption.md) verileri depolandırilmeden önce otomatik olarak şifreleyebilir ve verileri aldığınızda otomatik olarak şifreyi çözebilir. İşlem kullanıcılar için tamamen saydamdır. Depolama Hizmeti Şifreleme, mevcut en güçlü blok şifrelerinden biri olan 256 bit [Gelişmiş Şifreleme Standardı (AES) şifrelemesini](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanır. AES şifreleme, şifre çözme ve anahtar yönetimini saydam bir şekilde işler.

### <a name="client-side-encryption-of-azure-blobs"></a>Azure lekelerinistemci tarafı şifrelemesi

Azure lekelerinin istemci tarafı şifrelemesini çeşitli şekillerde gerçekleştirebilirsiniz.

Azure depolama alanınıza yüklemeden önce istemci uygulamalarınızdaki verileri şifrelemek için .NET NuGet paketi için Azure Depolama İstemci Kitaplığını kullanabilirsiniz.

.NET NuGet paketi için Azure Depolama İstemci Kitaplığı hakkında daha fazla bilgi edinmek ve indirmek için [Windows Azure Depolama 8.3.0'a](https://www.nuget.org/packages/WindowsAzure.Storage)bakın.

Key Vault ile istemci tarafı şifreleme kullandığınızda, verileriniz Azure Depolama istemcisi SDK tarafından oluşturulan tek seferlik simetrik İçerik Şifreleme Anahtarı (CEK) kullanılarak şifrelenir. CEK, simetrik anahtar veya asimetrik anahtar çifti olabilecek bir Anahtar Şifreleme Anahtarı (KEK) kullanılarak şifrelenir. Yerel olarak yönetebilir veya Key Vault'ta saklayabilirsiniz. Şifrelenmiş veriler daha sonra Azure Depolama'ya yüklenir.

Key Vault ile istemci tarafı şifreleme hakkında daha fazla bilgi edinmek ve nasıl yapılan larla ilgili talimatlara başlamak için [Bkz.](../../storage/blobs/storage-encrypt-decrypt-blobs-key-vault.md)

Son olarak, verileri Azure Depolama'ya yüklemeden önce istemci tarafı şifrelemesi gerçekleştirmek ve istemciye indirdiğinizde verilerin şifresini çözmek için Java için Azure Depolama İstemci Kitaplığını da kullanabilirsiniz. Bu kitaplık, depolama hesabı anahtar yönetimi için [Key Vault](https://azure.microsoft.com/services/key-vault/) ile tümleştirmeyi de destekler.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Azure SQL Veritabanı ile veri şifrelemesi

[Azure SQL Veritabanı,](../../sql-database/sql-database-technical-overview.md) Azure'da ilişkisel veriler, JSON, uzamsal ve XML gibi yapıları destekleyen genel amaçlı bir ilişkisel veritabanı hizmetidir. SQL Veritabanı, Şeffaf Veri Şifreleme (TDE) özelliği ve her zaman şifrelenmiş özelliği aracılığıyla istemci tarafı şifreleme sayılsa da sunucu tarafı şifrelemeyi destekler.

#### <a name="transparent-data-encryption"></a>Saydam Veri Şifrelemesi

[TDE,](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) sql [server,](https://www.microsoft.com/sql-server/sql-server-2016) [Azure SQL Veritabanı](../../sql-database/sql-database-technical-overview.md)ve [Azure SQL Veri Ambarı](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) veri dosyalarını gerçek zamanlı olarak şifrelemek için, kurtarma sırasında kullanılabilirlik için veritabanı önyükleme kaydında depolanan veritabanı şifreleme anahtarı (DEK) kullanarak kullanılır.

TDE, AES ve Üçlü Veri Şifreleme Standardı (3DES) şifreleme algoritmalarını kullanarak verileri ve günlük dosyalarını korur. Veritabanı dosyasının şifrelemesi sayfa düzeyinde gerçekleştirilir. Şifrelenmiş bir veritabanındaki sayfalar diske yazılmadan önce şifrelenir ve belleğe okunduklarında şifresi çözülür. TDE artık yeni oluşturulan Azure SQL veritabanlarında varsayılan olarak etkinleştirildi.

#### <a name="always-encrypted-feature"></a>Her Zaman Şifrelenmiş özellik

Azure SQL'deki [Her Zaman Şifrelenmiş](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) özelliği yle, verileri Azure SQL Veritabanı'nda depolamadan önce istemci uygulamalarında şifreleyebilirsiniz. Ayrıca, şirket içi veritabanı yönetiminin üçüncü taraflara delegasyonuna olanak sağlayabilir ve verileri sahip olanlar ve görüntüleyebilenler ile verileri yöneten ancak erişimi olmayan kişiler arasındaki ayrımı koruyabilirsiniz.

#### <a name="cell-level-or-column-level-encryption"></a>Hücre düzeyinde veya sütun düzeyinde şifreleme

Azure SQL Veritabanı ile, Transact-SQL kullanarak bir veri sütununa simetrik şifreleme uygulayabilirsiniz. Belirli sütunları ve hatta belirli veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemek için kullanabileceğiniz için bu yaklaşıma [hücre düzeyinde şifreleme veya sütun düzeyinde şifreleme (CLE)](/sql/relational-databases/security/encryption/encrypt-a-column-of-data)adı verilir. Bunu yapmak, sayfalardaki verileri şifreleyen TDE'den daha ayrıntılı şifreleme yeteneği sağlar.

CLE, simetrik veya asimetrik tuşları, sertifikanın ortak anahtarı nı veya 3DES'i kullanan bir parolayı kullanarak verileri şifrelemek için kullanabileceğiniz yerleşik işlevlere sahiptir.

### <a name="cosmos-db-database-encryption"></a>Cosmos DB veritabanı şifrelemesi

[Azure Cosmos DB,](../../cosmos-db/database-encryption-at-rest.md) Microsoft'un genel olarak dağıtılan çok modelli veritabanıdır. Geçici olmayan depolamada (katı hal sürücüleri) Cosmos DB'de depolanan kullanıcı verileri varsayılan olarak şifrelenir. Açmak veya kapatmak için denetim yok. Şifreleme, güvenli anahtar depolama sistemleri, şifreli ağlar ve şifreleme API'leri de dahil olmak üzere bir dizi güvenlik teknolojisi kullanılarak uygulanır. Şifreleme anahtarları Microsoft tarafından yönetilir ve Microsoft dahili yönergelerine göre döndürülür.

### <a name="at-rest-encryption-in-data-lake"></a>Veri Gölü'nde istirahat tesibi şifreleme

[Azure Veri Gölü,](../../data-lake-store/data-lake-store-encryption.md) gereksinimlerin veya şemaların resmi tanımından önce tek bir yerde toplanan her tür verinin kuruluş çapında depolanmasıdır. Data Lake Store, hesabınızın oluşturulması sırasında ayarlanan "varsayılan olarak açık" saydam veri şifrelemesini destekler. Varsayılan olarak, Azure Veri Gölü Deposu anahtarları sizin için yönetir, ancak bunları kendiniz yönetme seçeneğiniz vardır.

Verilerin şifresini çözmek ve çözmek için üç tür anahtar kullanılır: Ana Şifreleme Anahtarı (MEK), Veri Şifreleme Anahtarı (DEK) ve Blok Şifreleme Anahtarı (BEK). MEK, kalıcı ortamüzerinde depolanan DEK'i şifrelemek için kullanılır ve BEK DEK ve veri bloğundan türetilir. Kendi anahtarlarınızı yönetiyorsanız, MEK'i döndürebilirsiniz.

## <a name="encryption-of-data-in-transit"></a>Aktarım sırasında ki verilerin şifrelemesi

Azure, bir konumdan diğerine taşınırken verileri gizli tutmak için birçok mekanizma sunar.

### <a name="tlsssl-encryption-in-azure"></a>Azure'da TLS/SSL şifrelemesi

Microsoft, bulut hizmetleri ve müşteriler arasında seyahat ederken verileri korumak için [Aktarım Katmanı Güvenliği](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) protokolünü kullanır. Microsoft veri merkezleri, Azure hizmetlerine bağlanan istemci sistemleriyle TLS bağlantısı üzerinde anlaşma sağlar. TLS güçlü kimlik doğrulama, ileti gizliliği ve bütünlüğü (ileti tahrifatı, durdurma ve sahteciliğin algılanmasını etkinleştirme), birlikte çalışabilirlik, algoritma esnekliği ve dağıtım ve kullanım kolaylığı sağlar.

[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS), müşterilerin istemci sistemleri ve Microsoft bulut hizmetleri arasındaki bağlantıları benzersiz anahtarlarla korur. Bağlantılar da RSA tabanlı 2.048 bit şifreleme anahtar uzunlukları kullanın. Bu kombinasyon, birinin aktarım sırasında ki verileri yakalamasını ve bunlara erişmesini zorlaştırır.

### <a name="azure-storage-transactions"></a>Azure Depolama işlemleri

Azure portalı üzerinden Azure Depolama ile etkileşimkurduğunuzda, tüm işlemler HTTPS üzerinden gerçekleşir. Azure Depolama ile etkileşim kurmak için DEPOLAMA REST API'sini HTTPS üzerinden de kullanabilirsiniz. Depolama hesabı için gerekli olan güvenli aktarımı etkinleştirerek depolama hesaplarındaki nesnelere erişmek için REST API'lerini aradiğinizde HTTPS kullanımını zorlayabilirsiniz.

Azure Depolama nesnelerine erişimi devretmek için kullanılabilen Paylaşılan Erişim İmzaları[(SAS),](../../storage/common/storage-dotnet-shared-access-signature-part-1.md)Paylaşılan Erişim İmzalarını kullandığınızda yalnızca HTTPS protokolünün kullanılabileceğini belirtme seçeneğini içerir. Bu yaklaşım, SAS belirteçleri ile bağlantı gönderen herkesin uygun protokolü kullanmasını sağlar.

Azure Files paylaşımlarına erişmek için kullanılan [SMB 3.0,](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption)şifrelemeyi destekler ve Windows Server 2012 R2, Windows 8, Windows 8.1 ve Windows 10'da kullanılabilir. Bölgeler arası erişime ve hatta masaüstüne erişime olanak tanır.

İstemci tarafı şifreleme, verileri Azure Depolama örneğinize gönderilmeden önce şifreler, böylece ağ da şifrelenir.

### <a name="smb-encryption-over-azure-virtual-networks"></a>Azure sanal ağları üzerinden Kobİ şifrelemesi 

Windows Server 2012 veya sonraki ülkelerde çalışan VM'lerde [SMB 3.0'ı](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) kullanarak, Azure Sanal Ağlar üzerinden aktarım sırasındaki verileri şifreleyerek veri aktarımlarını güvenli hale getirebilirsiniz. Verileri şifreleyerek, kurcalama ve dinleme saldırılarına karşı korumaya yardımcı olursunuz. Yöneticiler, tüm sunucu veya yalnızca belirli paylaşımlar için Kobİ şifrelemesini etkinleştirebilir.

Varsayılan olarak, bir paylaşım veya sunucu için SMB şifrelemesi açık olduktan sonra, yalnızca SMB 3.0 istemcilerinin şifrelenmiş paylaşımlara erişmesine izin verilir.

## <a name="in-transit-encryption-in-vms"></a>VM'lerde aktarım şifrelemesi

Windows'u çalıştıran VM'lere, buradan ve bunlar arasında aktarım yapan veriler, bağlantının yapısına bağlı olarak çeşitli şekillerde şifrelenir.

### <a name="rdp-sessions"></a>RDP oturumları

Bir Windows istemci bilgisayarından Uzak Masaüstü [Protokolü (RDP)](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) kullanarak veya BIR RDP istemcisi yüklü bir Mac'ten VM'ye bağlanabilir ve oturum açabilirsiniz. RDP oturumlarında ağ üzerinden aktarılan veriler TLS ile korunabilir.

Azure'da bir Linux VM'ye bağlanmak için Uzak Masaüstü'nü de kullanabilirsiniz.

### <a name="secure-access-to-linux-vms-with-ssh"></a>SSH ile Linux VM'lerine güvenli erişim

Uzaktan yönetim için, Azure'da çalışan Linux VM'lere bağlanmak için [Secure Shell](../../virtual-machines/linux/ssh-from-windows.md) 'i (SSH) kullanabilirsiniz. SSH, güvenli olmayan bağlantılar üzerinden güvenli oturum açmalara olanak tanıyan şifreli bir bağlantı protokolüdür. Azure'da barındırılan Linux VM'leri için varsayılan bağlantı protokolüdür. Kimlik doğrulaması için SSH tuşlarını kullanarak, oturum açma gereksinimini ortadan kaldırırsınız. SSH kimlik doğrulaması için ortak/özel anahtar çifti (asimetrik şifreleme) kullanır.

## <a name="azure-vpn-encryption"></a>Azure VPN şifrelemesi

Ağ üzerinden gönderilen verilerin gizliliğini korumak için güvenli bir tünel oluşturan sanal özel bir ağ üzerinden Azure'a bağlanabilirsiniz.

### <a name="azure-vpn-gateways"></a>Azure VPN ağ geçitleri

Bir Azure [VPN ağ geçidini](../../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) kullanarak sanal ağınız ile şirket içi konumunuz arasında genel bağlantı üzerinden şifreli trafik gönderebilir veya sanal ağlar arasında trafik gönderebilirsiniz.

Siteden siteye VPN'ler aktarım şifrelemesi için [IPsec](https://en.wikipedia.org/wiki/IPsec) kullanır. Azure VPN ağ geçitleri varsayılan öneriler kümesi kullanır. Azure VPN ağ geçitlerini Azure varsayılan ilke kümeleri yerine belirli şifreleme algoritmaları ve önemli güçlü yönlerle özel bir IPsec/IKE ilkesi kullanacak şekilde yapılandırabilirsiniz.

### <a name="point-to-site-vpns"></a>Noktadan siteye VPN’ler

Site tabanlı VPN'ler, tek tek istemci bilgisayarların Bir Azure sanal ağına erişmesine izin verir. VPN tünelini oluşturmak için [Güvenli Soket TünelLeme Protokolü (SSTP)](https://technet.microsoft.com/library/2007.06.cableguy.aspx) kullanılır. Güvenlik duvarlarını geçiş yapabilir (tünel HTTPS bağlantısı olarak görünür). Nokta-to-site bağlantısı için kendi iç ortak anahtar altyapınızı (PKI) kök sertifika yetkilisini (CA) kullanabilirsiniz.

Sertifika kimlik doğrulaması veya PowerShell içeren Azure portalını kullanarak sanal ağa noktadan siteye VPN bağlantısı yapılandırabilirsiniz.

Azure sanal ağlarına nokta-to-site VPN bağlantıları hakkında daha fazla bilgi edinmek için bkz:

[Sertifika kimlik doğrulaması kullanarak sanal ağa noktadan siteye bağlantı yapılandırma: Azure portalı](../../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) 

[Sertifika kimlik doğrulamasını kullanarak sanal ağa noktadan siteye bağlantı yapılandırma: PowerShell](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpns"></a>Siteden siteye VPN'ler 

Şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) VPN tüneli üzerinden Bir Azure sanal ağına bağlamak için siteden siteye VPN ağ geçidi bağlantısı kullanabilirsiniz. Bu tür bir bağlantı, kendisine atanmış harici bir genel IP adresine sahip şirket içi bir VPN aygıtı gerektirir.

Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak siteden siteye VPN bağlantısını sanal ağa yapılandırabilirsiniz.

Daha fazla bilgi için bkz.

[Azure portalında siteden siteye bağlantı oluşturma](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[PowerShell'de siteden siteye bağlantı oluşturma](../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[CLI kullanarak siteden siteye VPN bağlantısı olan sanal ağ oluşturma](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-data-lake"></a>Veri Gölü'nde aktarım şifrelemesi

Data Lake Store'da aktarımdaki (diğer adıyla hareket halindeki) veriler de her zaman şifrelenir. Verileri kalıcı ortamda depolamadan önce şifrelemenin yanı sıra, veriler https kullanılarak her zaman aktarım sırasında da güvenli hale verilir. HTTPS, Data Lake Store REST arabirimleri için desteklenen tek protokoldür.

Veri Gölü'nde aktarım sırasında ki verilerin şifrelemesi hakkında daha fazla bilgi edinmek için [Data Lake Store'daki verilerin şifrelemesine](../../data-lake-store/data-lake-store-encryption.md)bakın.

## <a name="key-management-with-key-vault"></a>Key Vault ile anahtar yönetimi

Anahtarların uygun şekilde korunması ve yönetimi olmadan şifreleme yararsız hale getirilir. Key Vault, bulut hizmetleri tarafından kullanılan şifreleme anahtarlarına erişimi yönetmek ve denetlemek için Microsoft tarafından önerilen çözümdür. Anahtarlara erişim izinleri, Azure Etkin Dizin hesapları aracılığıyla hizmetlere veya kullanıcılara atanabilir.

Key Vault, kuruluşların donanım güvenlik modüllerini (HSM' ler) ve anahtar yönetim yazılımlarını yapılandırma, yama ve koruma gereğini giderir. Key Vault'u kullandığınızda, kontrolü sağlarsınız. Microsoft anahtarlarınızı hiçbir zaman görmez ve uygulamaların bunlara doğrudan erişimi yoktur. HSM'lerde anahtarları da içe aktarabilir veya oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure güvenliğine genel bakış](get-started-overview.md)
- [Azure ağ güvenliğine genel bakış](network-overview.md)
- [Azure veritabanı güvenliğine genel bakış](database-security-overview.md)
- [Azure sanal makineler güvenlik genel bakış](virtual-machines-overview.md)
- [Bekleme sırasında veri şifrelemesi](encryption-atrest.md)
- [Veri güvenliği ve şifreleme için en iyi uygulamalar](data-encryption-best-practices.md)
