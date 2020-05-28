---
title: 'Hızlı başlangıç: Azure SQL yönetilen örneği oluşturma (portal)'
description: Bu hızlı başlangıçta Azure portal kullanarak erişim için bir Azure SQL yönetilen örneği, ağ ortamı ve istemci VM 'si oluşturun.
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
ms.openlocfilehash: cc3a25992297dd8deb02deb2c561cad4b53e318b
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84113736"
---
# <a name="quickstart-create-an-azure-sql-managed-instance"></a>Hızlı başlangıç: Azure SQL yönetilen örneği oluşturma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu hızlı başlangıçta Azure portal [Azure SQL yönetilen örneği](sql-managed-instance-paas-overview.md) oluşturma öğretilir.

> [!IMPORTANT]
> Sınırlamalar için bkz. [Desteklenen bölgeler](resource-limits.md#supported-regions) ve [desteklenen Abonelik türleri](resource-limits.md#supported-subscription-types).

## <a name="create-sql-managed-instance"></a>SQL Yönetilen Örnek oluşturma

Azure SQL ile yönetilen bir örnek oluşturmak için aşağıdaki adımları izleyin: 

### <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

1. [Azure Portal](https://portal.azure.com/)oturum açın.
1. Azure portal sol menüsünde **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve arama kutusuna **Azure SQL** ' i girin.
1. **+ Ekle** ' yı seçerek **SQL dağıtım seçeneğini seçin** sayfasını açın. **SQL yönetilen örnek** kutucuğunda **Ayrıntıları göster** ' i seçerek Azure SQL yönetilen örneği hakkındaki ek bilgileri görüntüleyebilirsiniz.
1. **Oluştur**’u seçin.

   ![SQL yönetilen örneği oluşturma](./media/instance-create-quickstart/create-managed-instance.png)

4. Gerekli ve isteğe bağlı bilgileri eklemek için **Azure SQL yönetilen örnek** sağlama formu 'ndaki sekmeleri kullanın. Aşağıdaki bölümlerde bu sekmeler açıklanır.

### <a name="basics-tab"></a>Temel bilgiler sekmesi

- **Temel** bilgiler sekmesinde gerekli olan zorunlu bilgileri doldurun. Bu, bir SQL yönetilen örneği sağlamak için gereken en düşük bilgi kümesidir.

   ![SQL yönetilen örneği oluşturmaya yönelik "temel bilgiler" sekmesi](./media/instance-create-quickstart/mi-create-tab-basics.png)

   Bu sekmede gereken bilgiler için bir başvuru olarak aşağıdaki tabloyu kullanın.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Abonelik** | Aboneliğiniz. | Yeni kaynaklar oluşturmak için size izin veren bir abonelik. |
   | **Kaynak grubu** | Yeni veya mevcut bir kaynak grubu.|Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming).|
   | **Yönetilen örnek adı** | Geçerli bir ad.|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming).|
   | **Geli** |SQL yönetilen örneğini oluşturmak istediğiniz bölge.|Bölgeler hakkında bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/).|
   | **Yönetilen örnek yöneticisi oturum açma** | Geçerli bir Kullanıcı adı. | Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). Bu ayrılmış sunucu düzeyi bir rol olduğundan "serveradmin" kullanmayın.|
   | **Parola** | Herhangi bir geçerli parola.| Parola en az 16 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|

- İşlem ve depolama kaynaklarını belirlemek ve fiyatlandırma katmanlarını gözden geçirmek için **yönetilen örneği Yapılandır** ' ı seçin. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın. İşiniz bittiğinde seçiminizi kaydetmek için **Uygula** ' yı seçin. 

   ![Yönetilen örnek formu](./media/instance-create-quickstart/mi-create-tab-configure-performance.png)

- SQL yönetilen örneği oluşturmadan önce Seçimlerinizi gözden geçirmek için, **gözden geçir + oluştur**seçeneğini belirleyebilirsiniz. Ya da Ileri ' yi seçerek ağ seçeneklerini yapılandırın **: ağ**.

### <a name="networking-tab"></a>Ağ sekmesi

- **Ağ** sekmesinde isteğe bağlı bilgileri doldurun. Bu bilgileri atlarsanız, Portal varsayılan ayarları uygular.

   ![SQL yönetilen örneği oluşturmaya yönelik "ağ oluşturma" sekmesi](./media/instance-create-quickstart/mi-create-tab-networking.png)

   Bu sekmede gereken bilgiler için bir başvuru olarak aşağıdaki tabloyu kullanın.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Sanal ağ** | **Yeni sanal ağ** ya da geçerli bir sanal ağ ve alt ağ oluştur ' u seçin.| Bir ağ ya da alt ağ kullanılamaz durumdaysa, yeni SQL yönetilen örneği için hedef olarak seçmeden önce [ağ gereksinimlerini karşılayacak şekilde değiştirilmesi](vnet-existing-add-subnet.md) gerekir. Bir SQL yönetilen örneği için ağ ortamını yapılandırma gereksinimleri hakkında bilgi için bkz. bir [SQL yönetilen örneği için sanal ağ yapılandırma](connectivity-architecture-overview.md). |
   | **Bağlantı türü** | Proxy ve yeniden yönlendirme bağlantı türü arasında seçim yapın.|Bağlantı türleri hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örnek bağlantı türü](../database/connectivity-architecture.md#connection-policy).|
   | **Genel uç nokta**  | **Etkinleştir**’i seçin. | Bir SQL yönetilen örneği için genel veri uç noktası üzerinden erişilebilir olması için, bu seçeneği etkinleştirmeniz gerekir. | 
   | **Erişime Izin ver** ( **genel uç nokta** etkinse) | Seçeneklerden birini belirleyin.   |Portal deneyimi, bir güvenlik grubunu genel bir uç nokta ile yapılandırmaya izin vermez. </br> </br> Senaryonuza bağlı olarak, aşağıdaki seçeneklerden birini seçin: </br> <ul> <li>**Azure hizmetleri**: Power BI veya başka bir çok kiracılı hizmetten bağlanırken bu seçeneği öneririz. </li> <li> **Internet**: bir SQL yönetilen örneğini hızlı bir şekilde çalıştırmak istediğinizde test amaçları için kullanın. Bunu üretim ortamları için önermiyoruz. </li> <li> **Erişim yok**: Bu seçenek bir **reddetme** güvenlik kuralı oluşturur. Bir SQL yönetilen örneğinin genel bir uç nokta aracılığıyla erişilebilir olması için bu kuralı değiştirin. </li> </ul> </br> Genel uç nokta güvenliği hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örneğini genel bir uç noktayla güvenli bir şekilde kullanma](public-endpoint-overview.md).|

- SQL yönetilen örneği oluşturmadan önce Seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** ' u seçin. Ya da Ileri ' yi seçerek daha fazla özel ayar yapılandırın **: ek ayarlar**.

### <a name="additional-settings"></a>Ek ayarlar

- **Ek ayarlar** sekmesinde isteğe bağlı bilgileri doldurun. Bu bilgileri atlarsanız, Portal varsayılan ayarları uygular.

   ![SQL yönetilen örneği oluşturmak için "ek ayarlar" sekmesi](./media/instance-create-quickstart/mi-create-tab-additional-settings.png)

   Bu sekmede gereken bilgiler için bir başvuru olarak aşağıdaki tabloyu kullanın.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Harmanlama** | SQL yönetilen örneğiniz için kullanmak istediğiniz harmanlamayı seçin. SQL Server veritabanlarını geçirirseniz, kullanarak kaynak harmanlamasını denetleyin `SELECT SERVERPROPERTY(N'Collation')` ve bu değeri kullanın.| Harmanlamalar hakkında daha fazla bilgi için bkz. [sunucu harmanlamasını ayarlama veya değiştirme](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation).|   
   | **Saat dilimi** | SQL yönetilen örneğinizin gözleyeceksiniz saat dilimini seçin.|Daha fazla bilgi için bkz. [saat dilimleri](timezones-overview.md).|
   | **Yük devretme ikincili olarak kullan** | **Evet**' i seçin. | SQL yönetilen örneğini yük devretme grubu ikincil olarak kullanmak için bu seçeneği etkinleştirin.|
   | **BIRINCIL SQL yönetilen örneği** ( **Yük devretme Ikincili olarak kullan** **Evet**olarak ayarlanırsa) | Oluşturmakta olduğunuz SQL yönetilen örneğiyle aynı DNS bölgesine katılacak mevcut bir birincil SQL yönetilen örneği seçin. | Bu adım, yük devretme grubunun oluşturma sonrası yapılandırmasını etkinleştirecektir. Daha fazla bilgi için bkz. [öğretici: bir yük devretme grubuna SQL VERITABANı SQL yönetilen örneği ekleme](failover-group-add-instance-tutorial.md).|

## <a name="review--create"></a>Gözden geçir + oluştur

1. SQL yönetilen örneğini oluşturmadan önce Seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** sekmesini seçin.

   ![SQL yönetilen örnek İnceleme ve oluşturma sekmesi](./media/instance-create-quickstart/mi-create-tab-review-create.png)

1. SQL yönetilen örneğini sağlamaya başlamak için **Oluştur** ' u seçin.

> [!IMPORTANT]
> SQL yönetilen örneği dağıtmak uzun süren bir işlemdir. Alt ağdaki ilk Örneğin dağıtımı genellikle var olan SQL yönetilen örneklerine sahip bir alt ağa dağıtmaktan çok daha uzun sürer. Ortalama sağlama süreleri için bkz. [SQL yönetilen örnek yönetimi işlemleri](sql-managed-instance-paas-overview.md#management-operations).

## <a name="monitor-deployment-progress"></a>Dağıtım ilerlemesini izleme

1. Dağıtım durumunu görüntülemek için **Bildirimler** simgesini seçin.

   ![SQL yönetilen örnek dağıtımının dağıtım ilerlemesi](./media/instance-create-quickstart/mi-create-deployment-in-progress.png)

1. SQL yönetilen örnek penceresini açmak ve dağıtım ilerlemesini izlemek için bildirimde **devam etmekte olan dağıtım '** ı seçin. 

> [!TIP]
> Web tarayıcınızı kapattıysanız veya dağıtım ilerleme ekranından uzaklaşmanız durumunda dağıtım ilerleme durumunu bulmak için aşağıdaki adımları izleyin:
> 1. Azure portal, bir SQL yönetilen örneği dağıttığınız kaynak grubunu ( **temel bilgiler** sekmesinde) açın.
> 2. **Dağıtımlar**' ı seçin.
> 3. Sürmekte olan SQL yönetilen örnek dağıtım işlemini seçin.

> [!IMPORTANT]
> SQL yönetilen örnek oluşturma 'nın durumunu alabilmeniz için, kaynak grubu üzerinde **Okuma izinlerine** sahip olmanız gerekir. SQL yönetilen örneği oluşturma işlemi sırasında bu izne sahip değilseniz veya iptal ederseniz, bu, SQL yönetilen örneğinin kaynak grubu dağıtımları listesinde görünmemesine neden olabilir.
>

## <a name="view-resources-created"></a>Oluşturulan kaynakları görüntüle

SQL yönetilen örneğinin başarıyla dağıtılması sırasında, oluşturulan kaynakları görüntülemek için:

1. SQL yönetilen örneğiniz için kaynak grubunu açın. 

   ![SQL yönetilen örnek kaynakları](./media/instance-create-quickstart/resources.png)

## <a name="view-and-fine-tune-network-settings"></a>Ağ ayarlarını görüntüleme ve üzerinde ayarlama

İsteğe bağlı olarak ağ ayarlarını ince ayar yapmak için aşağıdakileri inceleyin:

1. Sizin için oluşturulan kullanıcı tanımlı yolu (UDR) gözden geçirmek için yol tablosunu seçin.

   ![Yol tablosu](./media/instance-create-quickstart/route-table.png)

2. Yönlendirme tablosunda, SQL yönetilen örneğinin sanal ağından gelen ve içindeki trafiği yönlendirmek için girişleri gözden geçirin. Yönlendirme tablonuzu el ile oluşturur veya yapılandırırsanız, bu girdileri SQL yönetilen örnek yolu tablosunda oluşturduğunuzdan emin olun.

   ![Yerel olarak bir SQL yönetilen örnek alt ağı girişi](./media/instance-create-quickstart/udr.png)

3. Kaynak grubuna geri dönün ve ağ güvenlik grubunu seçin.

   ![Ağ güvenlik grubu](./media/instance-create-quickstart/network-security-group.png)

4. Gelen ve giden güvenlik kurallarını gözden geçirin. 

   ![Güvenlik kuralları](./media/instance-create-quickstart/security-rules.png)

> [!IMPORTANT]
> SQL yönetilen örneğiniz için genel bir uç nokta yapılandırdıysanız, genel Internet 'ten SQL yönetilen örneği bağlantılarına izin vermek için bağlantı noktalarını açmanız gerekir. daha fazla bilgi için bkz. [SQL yönetilen örneği için genel uç nokta yapılandırma](public-endpoint-configure.md#allow-public-endpoint-traffic-on-the-network-security-group) .
>

## <a name="retrieve-connection-details-to-sql-managed-instance"></a>SQL yönetilen örneği 'ne bağlantı ayrıntılarını al

SQL yönetilen örneğine bağlanmak için, ana bilgisayar adı ve tam etki alanı adı (FQDN) almak için aşağıdaki adımları izleyin:

1. Kaynak grubuna dönün ve SQL yönetilen örneğinizi seçin.

   ![Kaynak grubundaki SQL yönetilen örneği](./media/instance-create-quickstart/managed-instance.png)

2. **Genel bakış** sekmesinde **ana bilgisayar** özelliğini bulun. SQL yönetilen örneği için ana bilgisayar adını sonraki hızlı başlangıçta kullanılmak üzere kopyalayın.

   ![Konak adı](./media/instance-create-quickstart/host-name.png)

   Kopyalanmış değer SQL yönetilen örneğine bağlanmak için kullanılabilecek bir tam etki alanı adı (FQDN) temsil eder. Şu adrese benzer bir örnek: *your_host_name. a1b2c3d4e5f6. Database. Windows. net*.

## <a name="next-steps"></a>Sonraki adımlar

SQL yönetilen örneğine bağlanma hakkında bilgi edinmek için:
- Uygulamalar için bağlantı seçeneklerine genel bakış için bkz. [uygulamalarınızı BIR SQL yönetilen örneğine bağlama](connect-application-instance.md).
- Azure sanal makinesinden bir SQL yönetilen örneğine bağlanmayı gösteren bir hızlı başlangıç için bkz. [Azure sanal makine bağlantısını yapılandırma](connect-vm-instance-configure.md).
- Noktadan siteye bağlantı kullanarak bir şirket içi istemci bilgisayarından SQL yönetilen örneği 'ne bağlanmayı gösteren bir hızlı başlangıç için bkz. [Noktadan siteye bağlantı yapılandırma](point-to-site-p2s-configure.md).

Mevcut bir SQL Server veritabanını Şirket içinden bir SQL yönetilen örneğine geri yüklemek için: 
- [Geçiş Için Azure veritabanı geçiş hizmeti](../../dms/tutorial-sql-server-to-managed-instance.md) 'ni kullanarak bir veritabanı yedekleme dosyasından geri yükleme yapın. 
- Bir veritabanı yedekleme dosyasından geri yüklemek için [T-SQL restore komutunu](restore-sample-database-quickstart.md) kullanın.

Yerleşik sorun giderme zekası ile SQL yönetilen örnek veritabanı performansının gelişmiş izlenmesi için, bkz. [Azure SQL Analytics kullanarak Azure SQL yönetilen örneğini izleme](../../azure-monitor/insights/azure-sql.md).
