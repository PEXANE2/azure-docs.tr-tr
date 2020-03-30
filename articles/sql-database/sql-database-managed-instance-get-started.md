---
title: 'Azure portalı: Yönetilen bir örnek oluşturma'
description: Erişim için SQL Veritabanı yönetilen örnek, ağ ortamı ve istemci VM oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: danimir
ms.author: danil
ms.reviewer: sstein, carlrab
ms.date: 09/26/2019
ms.openlocfilehash: 25128442cd922f6b9130586e245695b6880f661c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80257623"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Quickstart: Azure SQL Veritabanı yönetilen örnek oluşturma

Bu hızlı başlangıç, Azure portalında Azure SQL Veritabanı yönetilen bir [örneği](sql-database-managed-instance.md) oluşturma konusunda size yol açar.

> [!IMPORTANT]
> Sınırlamalar için [bkz.](sql-database-managed-instance-resource-limits.md#supported-regions) [Supported subscription types](sql-database-managed-instance-resource-limits.md#supported-subscription-types)

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

Azure aboneliğiniz yoksa, ücretsiz [bir hesap oluşturun.](https://azure.microsoft.com/free/)

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-a-managed-instance"></a>Yönetilen örnek oluşturma

Aşağıdaki adımlar, yönetilen bir örneğin nasıl oluşturulabileceğinizi gösterir:

1. Azure portalının sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna **Azure SQL** girin.
2. SELECT SQL dağıtım **seçeneği** sayfasını açmak için **+Ekle'yi** seçin. **Yönetilen örnekler** döşemesindeki **ayrıntıları göster'i** seçerek Azure SQL Veritabanı yönetilen örneği hakkında ek bilgileri görüntüleyebilirsiniz.
3. **Oluştur'u**seçin.

   ![Yönetilen örnek oluşturma](./media/sql-database-managed-instance-get-started/create-managed-instance.png)

4. Gerekli ve isteğe bağlı bilgileri eklemek için **Azure SQL Veritabanı Yönetilen Örnek** Oluşturma formundaki sekmeleri kullanın. Aşağıdaki bölümlerde bu sekmeleri açıklayınız.

### <a name="basics"></a>Temel Bilgiler

- **Temel Bilgiler** sekmesinde gerekli olan zorunlu bilgileri doldurun. Bu, yönetilen bir örneği sağlamak için gereken en az bilgi kümesidir.

   ![Yönetilen bir örnek oluşturmak için "Temeller" sekmesi](./media/sql-database-managed-instance-get-started/tabs/mi-create-tab-basics.png)

   Bu sekmede gerekli bilgiler için aşağıdaki tabloyu başvuru olarak kullanın.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Abonelik** | Aboneliğiniz. | Yeni kaynaklar oluşturmanıza izin veren abonelik. |
   | **Kaynak grubu** | Yeni veya varolan bir kaynak grubu.|Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming).|
   | **Yönetilen örnek adı** | Geçerli bir isim.|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming).|
   | **Bölge** |Yönetilen örneği oluşturmak istediğiniz bölge.|Bölgeler hakkında bilgi için [Azure bölgeleri'ne](https://azure.microsoft.com/regions/)bakın.|
   | **Yönetilen örnek yöneticisi oturum açma** | Geçerli kullanıcı adı. | Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). "Serveradmin" kullanmayın, çünkü bu ayrılmış bir sunucu düzeyi rolü.|
   | **Parola** | Geçerli bir parola.| Parola en az 16 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|

- Bilgi işlem ve depolama kaynaklarını boyutlandırmak ve fiyatlandırma katmanlarını gözden geçirmek için **Yönetilen Örneği Yapılandır'ı** seçin. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın. İşinizi bitirdiğinizde, seçiminizi kaydetmek için **Uygula'yı** seçin. 

   ![Yönetilen örnek formu](./media/sql-database-managed-instance-get-started/tabs/mi-create-tab-configure-performance.png)

- Yönetilen bir örnek oluşturmadan önce seçimlerinizi gözden geçirmek için **Gözden Geçir + oluştur**seçeneğini belirleyebilirsiniz. Veya **İleri: Ağ'ı**seçerek ağ seçeneklerini yapılandırın.

### <a name="networking"></a>Ağ Oluşturma

- **Ağ** sekmesinde isteğe bağlı bilgileri doldurun. Bu bilgileri atlarsanız, portal varsayılan ayarları uygular.

   ![Yönetilen bir örnek oluşturmak için "Ağ" sekmesi](./media/sql-database-managed-instance-get-started/tabs/mi-create-tab-networking.png)

   Bu sekmede gerekli bilgiler için aşağıdaki tabloyu başvuru olarak kullanın.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Sanal ağ** | Yeni **sanal ağ veya** geçerli bir sanal ağ ve alt ağ oluşturun'ı seçin.| Bir ağ veya alt ağ kullanılamıyorsa, yeni yönetilen örnek için hedef olarak seçmeden önce [ağ gereksinimlerini karşılamak](sql-database-managed-instance-configure-vnet-subnet.md) için değiştirilmesi gerekir. Yönetilen bir örnek için ağ ortamını yapılandırma gereksinimleri hakkında bilgi için [bkz.](sql-database-managed-instance-connectivity-architecture.md) |
   | **Bağlantı türü** | Proxy ve yönlendirme bağlantı türü arasında seçim yapın.|Bağlantı türleri hakkında daha fazla bilgi için [Azure SQL Veritabanı bağlantı ilkesine](sql-database-connectivity-architecture.md#connection-policy)bakın.|
   | **Genel uç nokta**  | **Etkinleştir**’i seçin. | Yönetilen bir örneğin ortak veri bitiş noktası üzerinden erişilebilir olması için bu seçeneği etkinleştirmeniz gerekir. | 
   | **Erişime izin ver** **(Genel bitiş noktası** etkinse) | Seçeneklerden birini seçin.   |Portal deneyimi, bir güvenlik grubunu ortak bir bitiş noktasıyla yapılandırmaya olanak tanır. </br> </br> Senaryonuza bağlı olarak, aşağıdaki seçeneklerden birini seçin: </br> <ul> <li>**Azure hizmetleri**: Power BI'den veya başka bir çok kiracılı hizmetten bağlanırken bu seçeneği öneririz. </li> <li> **Internet**: Yönetilen bir örneği hızlı bir şekilde döndürmek istediğinizde test amacıyla kullanın. Üretim ortamları için önermiyoruz. </li> <li> **Erişim yok**: Bu seçenek bir **Reddet** güvenlik kuralı oluşturur. Yönetilen bir örneği genel bir bitiş noktası üzerinden erişilebilir hale getirmek için bu kuralı değiştirin. </li> </ul> </br> Ortak uç nokta güvenliği hakkında daha fazla bilgi için [bkz.](sql-database-managed-instance-public-endpoint-securely.md)|

- Yönetilen bir örnek oluşturmadan önce seçimlerinizi gözden geçirmek için **Gözden Geçir + oluştur'u** seçin. Veya **İleri: Ek ayarları**seçerek daha özel ayarları yapılandırın.

### <a name="additional-settings"></a>Ek ayarlar

- **Ek ayarlar** sekmesinde isteğe bağlı bilgileri doldurun. Bu bilgileri atlarsanız, portal varsayılan ayarları uygular.

   ![Yönetilen bir örnek oluşturmak için "ek ayarlar" sekmesi](./media/sql-database-managed-instance-get-started/tabs/mi-create-tab-additional-settings.png)

   Bu sekmede gerekli bilgiler için aşağıdaki tabloyu başvuru olarak kullanın.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Harmanlama** | Yönetilen örneğiniz için kullanmak istediğiniz harmanlama yı seçin. Veritabanlarını SQL Server'dan geçiş temalıysanız, bu `SELECT SERVERPROPERTY(N'Collation')` değeri kullanarak kaynak harmasyonunu denetleyin ve kullanın.| Harmanlamalar hakkında daha fazla bilgi için sunucu [collation'ı ayarla veya değiştir'e](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation)bakın.|   
   | **Saat dilimi** | Yönetilen örneğinizin izleyeceği saat dilimini seçin.|Daha fazla bilgi için [Saat dilimlerine](sql-database-managed-instance-timezone.md)bakın.|
   | **İkincil üzerinde başarısız olarak kullanın** | **Evet'i**seçin. | Yönetilen örneği ikincil bir başarısız grup olarak kullanmak için bu seçeneği etkinleştirin.|
   | **Birincil yönetilen örnek** **(başarısız olarak kullan ikincil** **evet**olarak ayarlanmışsa) | Oluşturduğunuz yönetilen örnekle aynı DNS bölgesinde birleşecek varolan birincil yönetilen örneği seçin. | Bu adım, başarısız grubun oluşturma sonrası yapılandırmasını sağlar. Daha fazla bilgi için [bkz: Öğretici: Başarısız bir gruba SQL Veritabanı yönetilen örnek ekleyin.](sql-database-managed-instance-failover-group-tutorial.md)|

### <a name="review--create"></a>İnceleme + oluşturma

5. Yönetilen örneği oluşturmadan önce seçimlerinizi gözden geçirmek için **Gözden Geçir + oluştur** sekmesini seçin.

   ![Yönetilen bir örneği gözden geçirme ve oluşturma sekmesi](./media/sql-database-managed-instance-get-started/tabs/mi-create-tab-review-create.png)

6. Yönetilen örneği oluşturmaya başlamak için **Oluştur'u** seçin.

> [!IMPORTANT]
> Yönetilen bir örneği dağıtmak uzun süren bir işlemdir. Alt ağdaki ilk örneğin dağıtımı genellikle varolan yönetilen örnekleri olan bir alt ağa dağıtmaktan çok daha uzun sürer. Ortalama sağlama süreleri için [Yönetilen örnek yönetim işlemlerine](sql-database-managed-instance.md#managed-instance-management-operations)bakın.

### <a name="monitor-deployment-progress"></a>Dağıtım ilerlemesini izleme

7. Dağıtım durumunu görüntülemek için **Bildirimler** simgesini seçin.

   ![Yönetilen örnek dağıtımının dağıtım ilerleme süreci](./media/sql-database-managed-instance-get-started/in-progress/mi-create-deployment-in-progress.png)

8. Yönetilen örnek penceresini açmak ve dağıtım ilerlemesini daha fazla izlemek için bildirimde devam eden **Dağıtım'ı** seçin. 

> [!TIP]
> Web tarayıcınızı kapattıysanız veya dağıtım ilerleme ekranından uzaklaştıysanız, dağıtım ilerleme ekranını geri bulmak için aşağıdaki adımları izleyin:
> 1. Azure portalında, yönetilen bir örneği dağıtdığınız kaynak grubunu **(Temeller** sekmesinde) açın.
> 2. **Dağıtımları**seçin.
> 3. Devam eden yönetilen örnek dağıtım işlemini seçin.

> [!IMPORTANT]
> Yönetilen örnek oluşturma durumunu alabilmek için kaynak grubu üzerinden **izinleri okumanız** gerekir. Yönetilen örnek oluşturma sürecindeyken bu izne sahip değilseniz veya iptal ederseniz, yönetilen örneğin kaynak grubu dağıtımları listesinde görünmemesine neden olabilir.
>

## <a name="post-deployment-operations"></a>Dağıtım sonrası işlemler

Oluşturulan kaynakları gözden geçirmek için ağ ayarlarına ince ayar yapın ve ana bilgisayar bağlantı ayrıntılarını (FQDN) almak için bu bölümde açıklanan adımları izleyin.

### <a name="view-resources-created"></a>Oluşturulan kaynakları görüntüleme

Oluşturulan kaynakları görüntülemek için yönetilen örneğin başarılı bir şekilde dağıtılmasından sonra:

1. Yönetilen örneğiniz için kaynak grubunu açın. [Yönetilen örnek](#create-a-managed-instance) hızlı başlat'ta sizin için oluşturulan kaynaklarını görüntüleyin.

   ![Yönetilen örnek kaynakları](./media/sql-database-managed-instance-get-started/resources.png)

### <a name="view-and-fine-tune-network-settings"></a>Ağ ayarlarını görüntüleme ve ince ayar

Ağ ayarlarını isteğe bağlı olarak hassas bir şekilde ayarlamak için aşağıdakileri inceleyin:

1. Sizin için oluşturulan kullanıcı tanımlı rotayı (UDR) incelemek için rota tablosunu seçin.

   ![Yol tablosu](./media/sql-database-managed-instance-get-started/route-table.png)

2. Rota tablosunda, trafiği yönetilen örneğin sanal ağından ve içinde yönlendirmek için girişleri gözden geçirin. Rota tablonuzu el ile oluşturur veya yapılandırırsanız, yönetilen örnek rota tablosunda bu girişleri oluşturduğunuzdan emin olun.

   ![Yönetilen bir örnek alt ağının yerele girişi](./media/sql-database-managed-instance-get-started/udr.png)

3. Kaynak grubuna dönün ve ağ güvenlik grubunu seçin.

   ![Ağ güvenlik grubu](./media/sql-database-managed-instance-get-started/network-security-group.png)

4. Gelen ve giden güvenlik kurallarını gözden geçirin. 

   ![Güvenlik kuralları](./media/sql-database-managed-instance-get-started/security-rules.png)

> [!IMPORTANT]
> Yönetilen örneğiniz için ortak bitiş noktasını yapılandırmışsanız, bağlantıların genel Internet'ten yönetilen örneğine izin veren ağ trafiğine izin vermek için bağlantı noktalarını açmanız gerekir, [bkz.](sql-database-managed-instance-public-endpoint-configure.md#allow-public-endpoint-traffic-on-the-network-security-group)
>

### <a name="retrieve-connection-details-to-managed-instance"></a>Yönetilen örneğe bağlantı ayrıntılarını alma

Yönetilen örneğe bağlanmak için ana bilgisayar adı ve tam nitelikli etki alanı adı (FQDN) almak için aşağıdaki adımları izleyin:

1. Kaynak grubuna dönün ve yönetilen örneğini seçin.

   ![Kaynak grubunda yönetilen örnek](./media/sql-database-managed-instance-get-started/managed-instance.png)

2. Genel **Bakış** sekmesinde, **Ana Bilgisayar** özelliğini bulun. Yönetilen örnek için ana bilgisayar adını sonraki hızlı başlangıçta kopyalayın.

   ![Konak adı](./media/sql-database-managed-instance-get-started/host-name.png)

   Kopyalanan değer, yönetilen örne bağlanmak için kullanılabilecek tam nitelikli bir etki alanı adını (FQDN) temsil eder. Aşağıdaki adres örneğine benzer: *your_host_name.a1b2c3d4e5f6.database.windows.net*.

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen bir örne nasıl bağlanılabildiğini öğrenmek için:
- Uygulamalar için bağlantı seçeneklerine genel bir bakış için [bkz.](sql-database-managed-instance-connect-app.md)
- Azure sanal makineden yönetilen bir örne nasıl bağlanılabildiğini gösteren hızlı bir başlangıç için [bkz.](sql-database-managed-instance-configure-vm.md)
- Şirket içinde bir istemci bilgisayardan, noktadan siteye bağlantı kullanarak yönetilen bir örne nasıl bağlanılabildiğini gösteren hızlı [bir](sql-database-managed-instance-configure-p2s.md)başlangıç için bkz.

Varolan bir SQL Server veritabanını şirket içinde yönetilen bir örne geri yüklemek için: 
- Bir veritabanı yedekleme dosyasından geri yüklemek için [geçiş için Azure Veritabanı Geçiş Hizmeti'ni](../dms/tutorial-sql-server-to-managed-instance.md) kullanın. 
- Veritabanı yedekleme dosyasından geri yüklemek için [T-SQL RESTORE komutunu](sql-database-managed-instance-get-started-restore.md) kullanın.

Yerleşik sorun giderme zekasıyla yönetilen örnek veritabanı performansının gelişmiş izlenmesi için [Azure SQL Analytics'i kullanarak Azure SQL Veritabanını Izleyin](../azure-monitor/insights/azure-sql.md)bölümüne bakın.
