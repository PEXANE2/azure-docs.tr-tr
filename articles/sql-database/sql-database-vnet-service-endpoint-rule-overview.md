---
title: VNet uç noktaları ve tek ve havuzlu veritabanları için kurallar
description: Bir alt ağı Sanal Ağ hizmeti bitiş noktası olarak işaretleyin. Ardından ACL için sanal ağ kuralı olarak bitiş noktası Azure SQL Veritabanı. Sql Veritabanı sonra alt net üzerinde tüm sanal makineler ve diğer düğümler iletişim kabul eder.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto, genemi
ms.date: 11/14/2019
ms.openlocfilehash: 7032f9e8f57ea9400bf6a92f89b13fa1866f8fc1
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414389"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-database-servers"></a>Veritabanı sunucuları için sanal ağ hizmeti uç noktalarını ve kurallarını kullanma

*Sanal ağ kuralları,* Azure [SQL Veritabanı'ndaki](sql-database-technical-overview.md) tek veritabanlarınız ve elastik havuzunuz için veya SQL [Veri Ambarı'ndaki](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) veritabanlarınızın sanal ağlardaki belirli alt ağlardan gönderilen iletişimleri kabul edip etmediğini kontrol eden bir güvenlik duvarı güvenlik özelliğidir. Bu makalede, sanal ağ kuralı özelliğinin bazen Azure SQL Veritabanınıza ve SQL Veri Ambarınızla güvenli bir şekilde iletişim edeyimize izin vermek için neden en iyi seçenek olduğu açıklanmaktadır.

> [!IMPORTANT]
> Bu makale, Azure SQL sunucusu ve Azure SQL sunucusunda oluşturulan SQL Veritabanı ve SQL Veri Ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır. Bu makale, Azure SQL Veritabanı'nda yönetilen bir **örnek** dağıtımı için geçerli *değildir,* çünkü bununla ilişkili bir hizmet bitiş noktası yoktur.

Sanal ağ kuralı oluşturmak için öncelikle kuralın başvurulması için bir [sanal ağ hizmeti bitiş noktası][vm-virtual-network-service-endpoints-overview-649d] olması gerekir.

## <a name="how-to-create-a-virtual-network-rule"></a>Sanal ağ kuralı nasıl oluşturulur?

Yalnızca sanal ağ kuralı oluşturursanız, [bu makalenin ilerleyen saatlerinde](#anchor-how-to-by-using-firewall-portal-59j)adımlara ve açıklamaya geçebilirsiniz.

<!--<a name="anch-details-about-vnet-rules-38q"/> -->

## <a name="details-about-virtual-network-rules"></a>Sanal ağ kuralları hakkında ayrıntılar

Bu bölümde sanal ağ kuralları hakkında çeşitli ayrıntılar açıklanmaktadır.

### <a name="only-one-geographic-region"></a>Sadece bir coğrafi bölge

Her Sanal Ağ hizmeti bitiş noktası yalnızca bir Azure bölgesi için geçerlidir. Bitiş noktası, diğer bölgelerin alt ağdan iletişimi kabul etmesini sağlamaz.

Herhangi bir sanal ağ kuralı, temel bitiş noktasının geçerli olduğu bölgeyle sınırlıdır.

### <a name="server-level-not-database-level"></a>Sunucu düzeyinde değil, veritabanı düzeyi

Her sanal ağ kuralı, yalnızca sunucudaki belirli bir veritabanı için değil, tüm Azure SQL Veritabanı sunucunuz için geçerlidir. Başka bir deyişle, sanal ağ kuralı veritabanı düzeyinde değil, sunucu düzeyinde geçerlidir.

- Buna karşılık, IP kuralları her iki düzeyde de uygulanabilir.

### <a name="security-administration-roles"></a>Güvenlik yönetimi rolleri

Sanal Ağ hizmeti uç noktalarının yönetiminde güvenlik rollerinin ayrılması vardır. Eylem aşağıdaki rollerin her birinden gereklidir:

- **Ağ Yöneticisi:** &nbsp; Bitiş noktasını açın.
- **Veritabanı Yöneticisi:** &nbsp; Verilen alt ağı SQL Veritabanı sunucusuna eklemek için erişim denetim listesini (ACL) güncelleştirin.

*RBAC alternatif:*

Network Admin ve Database Admin rolleri sanal ağ kurallarını yönetmek için gerekenden daha fazla yetenek vardır. Yalnızca yeteneklerinin bir alt kümesi gereklidir.

Yalnızca gerekli yetenek alt kümesine sahip tek bir özel rol oluşturmak için Azure'da [rol tabanlı erişim denetimi (RBAC)][rbac-what-is-813s] kullanma seçeneğiniz vardır. Özel rol, Ağ Yöneticisi'ni veya Veritabanı Yöneticisini dahil etmek yerine kullanılabilir. Bir kullanıcıyı özel bir role eklerseniz, kullanıcıyı diğer iki ana yönetici rolüne eklerseniz, güvenlik maruziyetinizin yüzey alanı daha düşüktür.

> [!NOTE]
> Bazı durumlarda Azure SQL Veritabanı ve VNet alt ağı farklı aboneliklerdedir. Bu gibi durumlarda aşağıdaki yapılandırmaları sağlamanız gerekir:
>
> - Her iki abonelik de aynı Azure Etkin Dizin kiracısında olmalıdır.
> - Kullanıcı, hizmet uç noktalarını etkinleştirme ve verilen Sunucuya bir VNet alt ağı ekleme gibi işlemleri başlatmak için gerekli izinlere sahiptir.
> - Her iki aboneliğin de Microsoft.Sql sağlayıcısının kayıtlı olması gerekir.

## <a name="limitations"></a>Sınırlamalar

Azure SQL Veritabanı için sanal ağ kuralları özelliği aşağıdaki sınırlamaları vardır:

- SQL Veritabanınızın güvenlik duvarında, her sanal ağ kuralı bir alt ağa başvurur. Tüm bu başvurulan alt ağlar, SQL Veritabanı'nı barındıran aynı coğrafi bölgede barındırılmalıdır.

- Her Azure SQL Veritabanı sunucusunda herhangi bir sanal ağ için en fazla 128 ACL girişi olabilir.

- Sanal ağ kuralları yalnızca Azure Kaynak Yöneticisi sanal ağları için geçerlidir; ve [klasik dağıtım modeli][arm-deployment-model-568f] ağları için değil.

- Sanal ağ hizmeti bitiş noktalarını Azure SQL Veritabanı'na açmak, MySQL ve PostgreSQL Azure hizmetlerinin uç noktalarını da sağlar. Ancak, uç noktaları AİLE ile, uç noktalardan MySQL veya PostgreSQL örneklerinize bağlanma girişimleri başarısız olabilir.
  - Bunun altında yatan neden, MySQL ve PostgreSQL büyük olasılıkla yapılandırılmış bir sanal ağ kuralı olmamasıdır. MySQL ve PostgreSQL için Azure Veritabanı için bir sanal ağ kuralı yapılandırmanız gerekir ve bağlantı başarılı olur.

- Güvenlik duvarında, IP adresi aralıkları aşağıdaki ağ öğeleri için geçerlidir, ancak sanal ağ kuralları aşağıdakileri yapmaz:
  - [Siteden Siteye (S2S) sanal özel ağ (VPN)][vpn-gateway-indexmd-608y]
  - [ExpressRoute][expressroute-indexmd-744v] ile şirket içi

### <a name="considerations-when-using-service-endpoints"></a>Hizmet Bitiş Noktalarını kullanırken dikkat edilmesi gerekenler

Azure SQL Veritabanı için hizmet bitiş noktalarını kullanırken aşağıdaki hususları gözden geçirin:

- **Azure SQL Veritabanı Genel IP'lerine giden ler gereklidir**: Bağlantıya izin vermek için Ağ Güvenlik Gruplarının (NSG'ler) Azure SQL Veritabanı IP'lerine açılması gerekir. Bunu Azure SQL Veritabanı için NSG [Hizmet Etiketleri'ni](../virtual-network/security-overview.md#service-tags) kullanarak yapabilirsiniz.

### <a name="expressroute"></a>ExpressRoute

ExpressRoute'u [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) binanızdan kullanıyorsanız, herkese açık bir şekilde bakmak veya Microsoft'a bakmak için kullanılan NAT IP adreslerini tanımlamanız gerekir. Ortak eşleme için, her bir ExpressRoute varsayılan olarak bağlantı hattında trafik Microsoft Azure omurga ağına girdiğinde Azure hizmet trafiğine uygulanan iki NAT IP adresi kullanılır. Microsoft eşlemesi için, kullanılan NAT IP adresleri müşteri tarafından sağlanır veya hizmet sağlayıcısı tarafından sağlanır. Hizmet kaynaklarınıza erişime izin vermek için, bu genel IP adreslerine kaynak IP güvenlik duvarı ayarında izin vermeniz gerekir. Ortak eşleme ExpressRoute bağlantı hattı IP adreslerinizi bulmak için Azure portalında [ExpressRoute ile bir destek bileti açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). [ExpressRoute genel ve Microsoft eşlemesi için NAT](../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering) hakkında daha fazla bilgi edinin.
  
Devrenizden Azure SQL Veritabanı'na iletişime izin vermek için, NAT'nizin genel IP adresleri için IP ağ kuralları oluşturmanız gerekir.

<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->

## <a name="impact-of-using-vnet-service-endpoints-with-azure-storage"></a>Azure depolama alanıyla VNet Hizmet Bitiş Noktalarını kullanmanın etkisi

Azure Depolama, Azure Depolama hesabınızla bağlantınızı sınırlamanızı sağlayan aynı özelliği uygulamıştır. Bu özelliği Azure SQL Server tarafından kullanılan bir Azure Depolama hesabıyla kullanmayı seçerseniz, sorunlarla karşılaşabilirsiniz. Ardından, Azure SQL Veritabanı ve Azure SQL Veri Ambarı özelliklerinin bundan etkilenen bir listesi ve tartışması yer almaktadır.

### <a name="azure-sql-data-warehouse-polybase"></a>Azure SQL Veri Ambarı PolyBase

PolyBase genellikle Azure Depolama hesaplarından Azure SQL Veri Ambarı'na veri yüklemek için kullanılır. Limitlerden veri yüklediğiniz Azure Depolama hesabı yalnızca bir VNet alt ağına erişirse, PolyBase'den Hesaba bağlantı bozulur. VNet'e güvenli olan Azure Depolama'ya bağlanan Azure SQL Veri Ambarı ile hem PolyBase alma hem de dışa aktarma senaryolarını etkinleştirmek için aşağıdaki adımları izleyin:

#### <a name="prerequisites"></a>Ön koşullar

- Bu kılavuzu kullanarak Azure [PowerShell'i yükleyin.](https://docs.microsoft.com/powershell/azure/install-az-ps)
- Genel amaçlı bir v1 veya blob depolama hesabınız varsa, öncelikle bu [kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade)kullanarak genel amaçlı v2'ye yükseltmeniz gerekir.
- Azure Depolama hesabı **Güvenlik Duvarları ve Sanal ağlar** ayarları menüsü altında açık olan bu depolama **hesabına erişmek için güvenilir Microsoft hizmetlerine izin** vermelisiniz. Daha fazla bilgi için bu [kılavuza](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) bakın.

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. AzureRM modülü en az Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecektir.  Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumlulukları hakkında daha fazla bilgi için [yeni Azure PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

#### <a name="steps"></a>Adımlar

1. PowerShell'de Azure **SQL Sunucunuzu** Azure SQL Veri Ambarı örneğini Azure Active Directory (AAD) ile kaydedin:

   ```powershell
   Connect-AzAccount
   Select-AzSubscription -SubscriptionId <subscriptionId>
   Set-AzSqlServer -ResourceGroupName your-database-server-resourceGroup -ServerName your-SQL-servername -AssignIdentity
   ```

1. Bu [kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)kullanarak genel amaçlı bir **v2 Depolama Hesabı** oluşturun.

   > [!NOTE]
   > - Genel amaçlı bir v1 veya blob depolama hesabınız varsa, öncelikle bu [kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade)kullanarak **v2'ye yükseltmeniz** gerekir.
   > - Azure Veri Gölü Depolama Gen2 ile ilgili bilinen sorunlar için lütfen bu [kılavuza](https://docs.microsoft.com/azure/storage/data-lake-storage/known-issues)bakın.

1. Depolama hesabınız **altında, Access Control (IAM)** düğmesine gidin ve **rol ataması ekle'yi**tıklatın. Depolama **Blob Veri Katılımcısı** RBAC rolünü, adım 1'de olduğu gibi Azure Active Directory 'e (AAD) kaydettiğiniz Azure SQL Veri Ambarınızı barındıran Azure SQL Sunucunuza atayın.

   > [!NOTE]
   > Bu adımı yalnızca Sahip ayrıcalığına sahip üyeler gerçekleştirebilir. Azure kaynakları için çeşitli yerleşik roller için bu [kılavuza](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles)bakın.
  
1. **Azure Depolama hesabına polybase bağlantısı:**

   1. Daha önce bir veritabanı **[ana anahtarı](https://docs.microsoft.com/sql/t-sql/statements/create-master-key-transact-sql)** oluşturmadıysanız:

       ```sql
       CREATE MASTER KEY [ENCRYPTION BY PASSWORD = 'somepassword'];
       ```

   1. **IDENTITY = 'Yönetilen Hizmet Kimliği'** ile veritabanı kapsamı adı oluşturma :

       ```sql
       CREATE DATABASE SCOPED CREDENTIAL msi_cred WITH IDENTITY = 'Managed Service Identity';
       ```

       > [!NOTE]
       > - Bu mekanizma kapakların altında [Yönetilen Kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) kullandığından Azure Depolama erişim anahtarıyla Secret belirtmenize gerek yoktur.
       > - VNet'e güvenli Azure Depolama hesabıyla çalışmak için PolyBase bağlantısı için KİmLİk adı **'Yönetilen Hizmet Kimliği'** olmalıdır.

   1. PolyBase'i `abfss://` kullanarak genel amaçlı v2 depolama hesabınıza bağlanmak için şema ile harici veri kaynağı oluşturun:

       ```SQL
       CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss WITH (TYPE = hadoop, LOCATION = 'abfss://myfile@mystorageaccount.dfs.core.windows.net', CREDENTIAL = msi_cred);
       ```

       > [!NOTE]
       > - Genel amaçlı v1 veya blob depolama hesabıyla ilişkili harici tablolarınız varsa, önce bu dış tabloları bırakmalı ve ardından ilgili dış veri kaynağını bırakmalısınız. Ardından, yukarıdaki gibi `abfss://` genel amaçlı v2 depolama hesabına bağlanan şema ile harici veri kaynağı oluşturun ve bu yeni dış veri kaynağını kullanarak tüm dış tabloları yeniden oluşturun. Komut [Dosyalarını Oluştur ve Yayımla Sihirbazı'nı](https://docs.microsoft.com/sql/ssms/scripting/generate-and-publish-scripts-wizard) kullanarak tüm dış tablolar için komut dosyaları oluşturabilirsiniz.
       > - Şema hakkında `abfss://` daha fazla bilgi için bu [kılavuza](https://docs.microsoft.com/azure/storage/data-lake-storage/introduction-abfs-uri)bakın.
       > - CREATE EXTERNAL DATA SOURCE hakkında daha fazla bilgi için bu [kılavuza](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql)bakın.

   1. [Dış tabloları](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql)kullanarak normal olarak sorgula.

### <a name="azure-sql-database-blob-auditing"></a>Azure SQL Veritabanı Blob Denetimi

Blob denetimi denetim günlüklerini kendi depolama hesabınıza iter. Bu depolama hesabı VNet Hizmeti uç noktaları özelliğini kullanıyorsa, Azure SQL Veritabanı'ndan depolama hesabına bağlantı bozulur.

## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>VNet Hizmet Bitiş Noktalarını açmadan sunucunuza VNet Güvenlik Duvarı kuralı ekleme

Uzun zaman önce, bu özellik geliştirilmeden önce, Güvenlik Duvarı'nda canlı bir VNet kuralını uygulayamadan önce VNet hizmet bitiş noktalarını Açmanız gerekiyordu. Bitiş noktaları, belirli bir VNet alt ağıyla azure SQL Veritabanı ile ilişkilidir. Ancak şimdi Ocak 2018 **itibariyle, IgnoreMissingVNetServiceEndpoint** bayrağını ayarlayarak bu gereksinimi aşabilirsiniz.

Yalnızca Bir Güvenlik Duvarı kuralı ayarlamak sunucunun güvenliğini sağlamaya yardımcı olmaz. Güvenliğin etkili olması için VNet hizmet uç noktalarını açmanız gerekir. Hizmet bitiş noktalarını Açık'ı açtığınızda, VNet alt ağınız Kapalı'dan Açık'a geçişi tamamlayana kadar kapalı kalma süresiyle karşılar. Bu özellikle büyük VNets bağlamında geçerlidir. Geçiş sırasında kapalı kalma süresini azaltmak veya ortadan kaldırmak için **Yok SaymaVNetServiceEndpoint** bayrağını kullanabilirsiniz.

**PowerShell'i kullanarak IgnoreMissingVNetServiceEndpoint** bayrağını ayarlayabilirsiniz. Ayrıntılar [için, Azure SQL Veritabanı için sanal ağ hizmet bitiş noktası ve kuralı oluşturmak için PowerShell'e][sql-db-vnet-service-endpoint-rule-powershell-md-52d]bakın.

## <a name="errors-40914-and-40615"></a>Hatalar 40914 ve 40615

Bağlantı hatası 40914, Azure portalındaki Güvenlik Duvarı bölmesinde belirtildiği gibi *sanal ağ kurallarıyla*ilgilidir. Hata 40615 benzer, ancak Güvenlik Duvarı'ndaki *IP adresi kurallarıyla* ilgilidir.

### <a name="error-40914"></a>Hata 40914

*İleti metni:* Giriş tarafından istenen sunucu '*[sunucu adı]*' açılamaz. İstemcinin sunucuya erişmesine izin verilmez.

*Hata açıklaması:* İstemci, sanal ağ sunucusu uç noktalarıolan bir alt ağdadır. Ancak Azure SQL Veritabanı sunucusunda, alt ağa SQL Veritabanı ile iletişim kurma hakkı veren bir sanal ağ kuralı yok.

*Hata çözümü:* Azure portalının Güvenlik Duvarı bölmelerinde, alt ağ için [sanal ağ kuralı eklemek](#anchor-how-to-by-using-firewall-portal-59j) için sanal ağ kuralları denetimini kullanın.

### <a name="error-40615"></a>Hata 40615

*İleti metni:* Giriş tarafından{0}istenen sunucu ' ' 'yu açılamaz. IP adresi '{1}' olan istemcinin sunucuya erişmesine izin verilmez.

*Hata açıklaması:* İstemci, Azure SQL Veritabanı sunucusuna bağlanma yetkisi olmayan bir IP adresinden bağlanmaya çalışıyor. Sunucu güvenlik duvarında, istemcinin belirtilen IP adresinden SQL Veritabanı ile iletişim kurmasına izin veren bir IP adresi kuralı yok.

*Hata çözümü:* Ip kuralı olarak istemcinin IP adresini girin. Bu işlemi Azure portalındaki Güvenlik Duvarı bölmesini kullanarak yapabilirsiniz.

<a name="anchor-how-to-by-using-firewall-portal-59j" />

## <a name="portal-can-create-a-virtual-network-rule"></a>Portal sanal ağ kuralı oluşturabilir

Bu bölümde, Azure SQL Veritabanınızda sanal *ağ kuralı* oluşturmak için [Azure portalını][http-azure-portal-link-ref-477t] nasıl kullanabileceğiniz gösterilmektedir. Kural, SQL Veritabanınıza *Sanal Ağ hizmeti bitiş noktası*olarak etiketlenmiş belirli bir alt ağdan gelen iletişimi kabul etmesini söyler.

> [!NOTE]
> Azure SQL Veritabanı sunucunuzun VNet güvenlik duvarı kurallarına bir hizmet bitiş noktası eklemek istiyorsanız, öncelikle alt ağ için hizmet bitiş noktalarının açık olduğundan emin olun.
>
> Alt ağ için hizmet bitiş noktaları açık değilse, portal bunları etkinleştirmenizi ister. Kuralı eklediğiniz aynı bıçaktaki **Etkinleştir** düğmesini tıklatın.

## <a name="powershell-alternative"></a>PowerShell alternatifi

Bir komut dosyası da PowerShell cmdlet **New-AzSqlServerVirtualNetworkRule** veya [az ağ vnet oluşturmak](/cli/azure/network/vnet#az-network-vnet-create)kullanarak sanal ağ kuralları oluşturabilirsiniz. İlgileniyorsanız, [Azure SQL Veritabanı için bir Sanal Ağ hizmeti bitiş noktası ve kuralı oluşturmak için PowerShell'e][sql-db-vnet-service-endpoint-rule-powershell-md-52d]bakın.

## <a name="rest-api-alternative"></a>REST API alternatifi

Dahili olarak, SQL VNet eylemleri için PowerShell cmdlets REST API'leri arayın. REST API'lerini doğrudan arayabilirsiniz.

- [Sanal Ağ Kuralları: İşlemler][rest-api-virtual-network-rules-operations-862r]

## <a name="prerequisites"></a>Ön koşullar

Azure SQL Veritabanı ile ilgili belirli Sanal Ağ hizmeti bitiş noktası *türü adı* ile etiketlenmiş bir alt ağınız olması gerekir.

- İlgili uç nokta türü adı **Microsoft.Sql'dir.**
- Alt ağınız tür adı yla etiketlenmemişse, [bkz.][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]

<a name="a-portal-steps-for-vnet-rule-200" />

## <a name="azure-portal-steps"></a>Azure portal adımları

1. [Azure Portal][http-azure-portal-link-ref-477t] oturum açın.

2. ARAMA ve **SQL sunucuları**seçin, sonra sunucu seçin. **Güvenlik**altında, **Güvenlik Duvarları ve sanal ağları**seçin.

3. Azure **hizmetlerine Erişime İzin Ver** denetimini OFF olarak ayarlayın.

    > [!IMPORTANT]
    > Denetim kümesini AÇIK olarak bırakırsanız, Azure SQL Veritabanı sunucunuz Azure sınırı içindeki herhangi bir alt ağdan yani Azure veri merkezleri için tanımlanan aralıklar içinde kabul edilen IP adreslerinden birinden kaynaklanan iletişimi kabul eder. Denetim kümesini A)'ya bırakmak, güvenlik açısından aşırı erişim olabilir. Microsoft Azure Sanal Ağ hizmeti bitiş noktası özelliği, SQL Veritabanı'nın sanal ağ kuralı özelliğiyle eşgüdüm içinde, birlikte güvenlik yüzey alanınızı azaltabilir.

4. **Sanal ağlar** bölümünde **+ Varolan** denetimi ekle'yi tıklatın.

    ![Varolan ekle'yi tıklatın (SQL kuralı olarak alt ağ bitiş noktası).][image-portal-firewall-vnet-add-existing-10-png]

5. Yeni **Oluştur/Güncelleştir** bölmesinde, denetimleri Azure kaynaklarınızın adlarıyla doldurun.

    > [!TIP]
    > Alt ağınız için doğru **Adres önekini** eklemeniz gerekir. Değeri portalda bulabilirsiniz.
    > **Tüm kaynakları** &gt; gezin **Tüm türleri** &gt; **Sanal ağlar**. Filtre sanal ağlarınızı görüntüler. Sanal ağıtıklatın ve sonra **Subnets**tıklatın. **ADDRESS RANGE** sütununda ihtiyacınız olan Adres öneki vardır.

    ![Yeni kural için alanları doldurun.][image-portal-firewall-create-update-vnet-rule-20-png]

6. Bölmenin altındaki **Tamam** düğmesini tıklatın.

7. Güvenlik duvarı bölmesindeki ortaya çıkan sanal ağ kuralına bakın.

    ![Güvenlik duvarı bölmesindeki yeni kurala bakın.][image-portal-firewall-vnet-result-rule-30-png]

> [!NOTE]
> Aşağıdaki durumlar veya durumlar kurallara uygulanır:
>
> - **Hazır:** Başlattığınız işlemin Başarılı olduğunu gösterir.
> - **Başarısız oldu:** Başlattığınız işlemin Başarısız olduğunu gösterir.
> - **Silindi:** Yalnızca Sil işlemi için geçerlidir ve kuralın silindiğini ve artık geçerli olmadığını gösterir.
> - **Devam Ediyor:** İşlemin devam ettiğini gösterir. İşlem bu durumdayken eski kural geçerlidir.

<a name="anchor-how-to-links-60h" />

## <a name="related-articles"></a>İlgili makaleler:

- [Azure sanal ağ hizmeti bitiş noktaları][vm-virtual-network-service-endpoints-overview-649d]
- [Azure SQL Veritabanı sunucu düzeyi ve veritabanı düzeyinde güvenlik duvarı kuralları][sql-db-firewall-rules-config-715d]

Azure SQL Veritabanı için sanal ağ kuralı özelliği Eylül 2017 sonlarında kullanıma sunuldu.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal ağ hizmeti bitiş noktası ve ardından Azure SQL Veritabanı için sanal ağ kuralı oluşturmak için PowerShell'i kullanın.][sql-db-vnet-service-endpoint-rule-powershell-md-52d]
- [Sanal Ağ Kuralları:][rest-api-virtual-network-rules-operations-862r] REST API'leri ile işlemler

<!-- Link references, to images. -->
[image-portal-firewall-vnet-add-existing-10-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png
[image-portal-firewall-create-update-vnet-rule-20-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png
[image-portal-firewall-vnet-result-rule-30-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png

<!-- Link references, to text, Within this same GitHub repo. -->
[arm-deployment-model-568f]: ../azure-resource-manager/management/deployment-models.md
[expressroute-indexmd-744v]: ../expressroute/index.yml
[rbac-what-is-813s]:../role-based-access-control/overview.md
[sql-db-firewall-rules-config-715d]: sql-database-firewall-configure.md
[sql-db-vnet-service-endpoint-rule-powershell-md-52d]: sql-database-vnet-service-endpoint-rule-powershell.md
[sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]: sql-database-vnet-service-endpoint-rule-powershell.md#a-verify-subnet-is-endpoint-ps-100
[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[vm-virtual-network-service-endpoints-overview-649d]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview
[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml

<!-- Link references, to text, Outside this GitHub repo (HTTP). -->
[http-azure-portal-link-ref-477t]: https://portal.azure.com/
[rest-api-virtual-network-rules-operations-862r]: https://docs.microsoft.com/rest/api/sql/virtualnetworkrules

<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON
- Azure CLI
- ARM templates
-->
