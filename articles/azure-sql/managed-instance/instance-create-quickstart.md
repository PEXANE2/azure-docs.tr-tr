---
title: 'Hızlı başlangıç: yönetilen SQL yönetilen örneği (portal) örneği oluşturma'
description: Bu hızlı başlangıçta Azure portal kullanarak erişim için yönetilen bir örnek, ağ ortamı ve istemci VM 'si oluşturun.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: danimir
ms.author: danil
ms.reviewer: sstein, carlrab
ms.date: 09/26/2019
ms.openlocfilehash: b2d43e970012209acb6ed7fbbaafbb1719617280
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533878"
---
# <a name="quickstart-create-a-managed-instance-of-sql-managed-instance"></a>Hızlı başlangıç: yönetilen SQL yönetilen örneği oluşturma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu hızlı başlangıç, Azure portal [Azure SQL yönetilen örneği](sql-managed-instance-paas-overview.md) 'nin yönetilen bir örneğini oluşturmayı öğretir.

> [!IMPORTANT]
> Sınırlamalar için bkz. [Desteklenen bölgeler](resource-limits.md#supported-regions) ve [desteklenen Abonelik türleri](resource-limits.md#supported-subscription-types).

## <a name="create-a-managed-instance"></a>Yönetilen örnek oluşturma

Yönetilen bir örnek oluşturmak için aşağıdaki adımları izleyin: 

### <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Azure portal sol menüsünde **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve arama kutusuna **Azure SQL** ' i girin.
1. **+ Ekle** ' yı seçerek **SQL dağıtım seçeneğini seçin** sayfasını açın. **SQL yönetilen örnekler** kutucuğunda **Ayrıntıları göster** ' i seçerek Azure SQL yönetilen örneği hakkındaki ek bilgileri görüntüleyebilirsiniz.
1. **Oluştur**’u seçin.

   ![Yönetilen örnek oluşturma](./media/instance-create-quickstart/create-managed-instance.png)

4. Gerekli ve isteğe bağlı bilgileri eklemek için **Azure SQL yönetilen örnek** sağlama formu 'ndaki sekmeleri kullanın. Aşağıdaki bölümlerde bu sekmeler açıklanır.

### <a name="basics-tab"></a>Temel bilgiler sekmesi

- **Temel** bilgiler sekmesinde gerekli olan zorunlu bilgileri doldurun. Bu, yönetilen bir örnek sağlamak için gereken en düşük bilgi kümesidir.

   ![Yönetilen örnek oluşturmak için "temel bilgiler" sekmesi](./media/instance-create-quickstart/mi-create-tab-basics.png)

   Bu sekmede gereken bilgiler için bir başvuru olarak aşağıdaki tabloyu kullanın.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Abonelik** | Aboneliğiniz. | Yeni kaynaklar oluşturmak için size izin veren bir abonelik. |
   | **Kaynak grubu** | Yeni veya mevcut bir kaynak grubu.|Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming).|
   | **Yönetilen örnek adı** | Geçerli bir ad.|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming).|
   | **Bölge** |Yönetilen örneği oluşturmak istediğiniz bölge.|Bölgeler hakkında bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/).|
   | **Yönetilen örnek yöneticisi oturum açma** | Geçerli bir Kullanıcı adı. | Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). Bu ayrılmış sunucu düzeyi bir rol olduğundan "serveradmin" kullanmayın.|
   | **Parola** | Herhangi bir geçerli parola.| Parola en az 16 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|

- İşlem ve depolama kaynaklarını belirlemek ve fiyatlandırma katmanlarını gözden geçirmek için **yönetilen örneği Yapılandır** ' ı seçin. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın. İşiniz bittiğinde seçiminizi kaydetmek için **Uygula** ' yı seçin. 

   ![Yönetilen örnek formu](./media/instance-create-quickstart/mi-create-tab-configure-performance.png)

- SQL yönetilen örneği oluşturmadan önce Seçimlerinizi gözden geçirmek için, **gözden geçir + oluştur**seçeneğini belirleyebilirsiniz. Ya da Ileri ' yi seçerek ağ seçeneklerini yapılandırın **: ağ**.

### <a name="networking-tab"></a>Ağ sekmesi

- **Ağ** sekmesinde isteğe bağlı bilgileri doldurun. Bu bilgileri atlarsanız, Portal varsayılan ayarları uygular.

   ![Yönetilen örnek oluşturmak için "ağ" sekmesi](./media/instance-create-quickstart/mi-create-tab-networking.png)

   Bu sekmede gereken bilgiler için bir başvuru olarak aşağıdaki tabloyu kullanın.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Sanal ağ** | **Yeni sanal ağ** ya da geçerli bir sanal ağ ve alt ağ oluştur ' u seçin.| Bir ağ veya alt ağ kullanılamaz durumdaysa, yeni yönetilen örnek için bir hedef olarak seçmeden önce [ağ gereksinimlerini karşılamak üzere değiştirilmesi](vnet-existing-add-subnet.md) gerekir. SQL yönetilen örneği için ağ ortamını yapılandırma gereksinimleri hakkında bilgi için bkz. [SQL yönetilen örneği için sanal ağ yapılandırma](connectivity-architecture-overview.md). |
   | **Bağlantı türü** | Proxy ve yeniden yönlendirme bağlantı türü arasında seçim yapın.|Bağlantı türleri hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örnek bağlantı türü](../database/connectivity-architecture.md#connection-policy).|
   | **Genel uç nokta**  | **Etkinleştir**’i seçin. | Yönetilen bir örneğin genel veri uç noktası üzerinden erişilebilir olması için bu seçeneği etkinleştirmeniz gerekir. | 
   | **Erişime Izin ver** ( **genel uç nokta** etkinse) | Seçeneklerden birini belirleyin.   |Portal deneyimi, bir güvenlik grubunu genel bir uç nokta ile yapılandırmaya izin vermez. </br> </br> Senaryonuza bağlı olarak, aşağıdaki seçeneklerden birini seçin: </br> <ul> <li>**Azure hizmetleri**: Power BI veya başka bir çok kiracılı hizmetten bağlanırken bu seçeneği öneririz. </li> <li> **Internet**: yönetilen bir örneği hızlı bir şekilde çalıştırmak istediğinizde test amaçları için kullanın. Bunu üretim ortamları için önermiyoruz. </li> <li> **Erişim yok**: Bu seçenek bir **reddetme** güvenlik kuralı oluşturur. Yönetilen bir örneği ortak bir uç nokta aracılığıyla erişilebilir hale getirmek için bu kuralı değiştirin. </li> </ul> </br> Genel uç nokta güvenliği hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örneğini genel bir uç noktayla güvenli bir şekilde kullanma](public-endpoint-overview.md).|

- Yönetilen bir örnek oluşturmadan önce Seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** ' u seçin. Ya da Ileri ' yi seçerek daha fazla özel ayar yapılandırın **: ek ayarlar**.

### <a name="additional-settings"></a>Ek ayarlar

- **Ek ayarlar** sekmesinde isteğe bağlı bilgileri doldurun. Bu bilgileri atlarsanız, Portal varsayılan ayarları uygular.

   ![Yönetilen örnek oluşturmak için "ek ayarlar" sekmesi](./media/instance-create-quickstart/mi-create-tab-additional-settings.png)

   Bu sekmede gereken bilgiler için bir başvuru olarak aşağıdaki tabloyu kullanın.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Harmanlama** | Yönetilen örneğiniz için kullanmak istediğiniz harmanlamayı seçin. SQL Server veritabanlarını geçirirseniz, kullanarak kaynak harmanlamasını denetleyin `SELECT SERVERPROPERTY(N'Collation')` ve bu değeri kullanın.| Harmanlamalar hakkında daha fazla bilgi için bkz. [sunucu harmanlamasını ayarlama veya değiştirme](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation).|   
   | **Saat dilimi** | Yönetilen örneğin gözleneceği saat dilimini seçin.|Daha fazla bilgi için bkz. [saat dilimleri](timezones-overview.md).|
   | **Yük devretme ikincili olarak kullan** | **Evet**’i seçin. | Yönetilen örneği yük devretme grubu ikincil olarak kullanmak için bu seçeneği etkinleştirin.|
   | **BIRINCIL SQL yönetilen örneği** ( **Yük devretme Ikincili olarak kullan** **Evet**olarak ayarlanırsa) | Oluşturmakta olduğunuz yönetilen örnekle aynı DNS bölgesine katılacak mevcut bir birincil yönetilen örnek seçin. | Bu adım, yük devretme grubunun oluşturma sonrası yapılandırmasını etkinleştirecektir. Daha fazla bilgi için bkz. [öğretici: bir yük devretme grubuna yönetilen örnek ekleme](failover-group-add-instance-tutorial.md).|

## <a name="review--create"></a>Gözden geçirme ve oluşturma

1. Yönetilen bir örnek oluşturmadan önce Seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** sekmesini seçin.

   ![Yönetilen örnek İnceleme ve oluşturma sekmesi](./media/instance-create-quickstart/mi-create-tab-review-create.png)

1. Yönetilen örneği sağlamaya başlamak için **Oluştur** ' u seçin.

> [!IMPORTANT]
> Yönetilen bir örneği dağıtmak uzun süren bir işlemdir. Alt ağdaki ilk Örneğin dağıtımı, genellikle mevcut yönetilen örneklerle bir alt ağa dağıtmaya kıyasla çok daha uzun sürer. Ortalama sağlama süreleri için bkz. [SQL yönetilen örnek yönetimi işlemleri](sql-managed-instance-paas-overview.md#management-operations).

## <a name="monitor-deployment-progress"></a>Dağıtım ilerlemesini izleme

1. Dağıtım durumunu görüntülemek için **Bildirimler** simgesini seçin.

   ![SQL yönetilen örnek dağıtımının dağıtım ilerlemesi](./media/instance-create-quickstart/mi-create-deployment-in-progress.png)

1. SQL yönetilen örnek penceresini açmak ve dağıtım ilerlemesini izlemek için bildirimde **devam etmekte olan dağıtım '** ı seçin. 

> [!TIP]
> Web tarayıcınızı kapattıysanız veya dağıtım ilerleme ekranından uzaklaşmanız durumunda dağıtım ilerleme durumunu bulmak için aşağıdaki adımları izleyin:
> 1. Azure portal, SQL yönetilen örneği dağıtmakta olduğunuz kaynak grubunu ( **temel bilgiler** sekmesinde) açın.
> 2. **Dağıtımlar**' ı seçin.
> 3. Sürmekte olan SQL yönetilen örnek dağıtım işlemini seçin.

> [!IMPORTANT]
> - SQL yönetilen örneğinin oluşturulması, belirli koşullara bağlı olarak, bir seferde birkaç saat sürebilecek uzun süreli bir işlemdir. Tipik oluşturma süreleri için bkz. [yönetim işlemleri süresi](management-operations-overview.md#management-operations-duration) .
> - Aynı alt ağdaki diğer yönetilen örneklerde uzun süre çalışan geri yükleme veya ölçeklendirme işlemleri olduğunda, SQL yönetilen örnek oluşturma 'nın başlatılması, diğer etkileyen işlemler varsa geciktirilebilir. Daha fazla bilgi için bkz. [yönetim işlemleri çapraz etkisi](management-operations-overview.md#management-operations-cross-impact).
> - Yönetilen örnek oluşturma durumunu alabilmeniz için, kaynak grubu üzerinde **Okuma izinlerine** sahip olmanız gerekir. Yönetilen örnek oluşturma işlemi sırasında bu izne sahip değilseniz veya dosyayı iptal ederseniz, bu, SQL yönetilen örneğinin kaynak grubu dağıtımları listesinde görünmemesine neden olabilir.
>

## <a name="view-resources-created"></a>Oluşturulan kaynakları görüntüle

Yönetilen bir örneğin başarıyla dağıtılması sırasında, oluşturulan kaynakları görüntülemek için:

1. Yönetilen örneğiniz için kaynak grubunu açın. 

   ![SQL yönetilen örnek kaynakları](./media/instance-create-quickstart/resources.png)

## <a name="view-and-fine-tune-network-settings"></a>Ağ ayarlarını görüntüleme ve üzerinde ayarlama

İsteğe bağlı olarak ağ ayarlarını ince ayar yapmak için aşağıdakileri inceleyin:

1. Sizin için oluşturulan kullanıcı tanımlı yolu (UDR) gözden geçirmek için yol tablosunu seçin.

   ![Yol tablosu](./media/instance-create-quickstart/route-table.png)

2. Yönlendirme tablosunda, SQL yönetilen örnek sanal ağının içinden ve içinden trafiği yönlendirmek için girişleri gözden geçirin. Yönlendirme tablonuzu el ile oluşturur veya yapılandırırsanız, bu girdileri SQL yönetilen örnek yolu tablosunda oluşturduğunuzdan emin olun.

   ![Yerel olarak bir SQL yönetilen örnek alt ağı girişi](./media/instance-create-quickstart/udr.png)

3. Kaynak grubuna geri dönün ve ağ güvenlik grubunu seçin.

   ![Ağ güvenlik grubu](./media/instance-create-quickstart/network-security-group.png)

4. Gelen ve giden güvenlik kurallarını gözden geçirin. 

   ![Güvenlik kuralları](./media/instance-create-quickstart/security-rules.png)

> [!IMPORTANT]
> SQL yönetilen örneği için genel bir uç nokta yapılandırdıysanız, genel İnternet 'ten SQL yönetilen örneği bağlantılarına izin vermek için bağlantı noktalarını açmanız gerekir. Daha fazla bilgi için bkz. [SQL yönetilen örneği için genel uç nokta yapılandırma](public-endpoint-configure.md#allow-public-endpoint-traffic-on-the-network-security-group) .
>

## <a name="retrieve-connection-details-to-sql-managed-instance"></a>SQL yönetilen örneği 'ne bağlantı ayrıntılarını al

SQL yönetilen örneğine bağlanmak için, ana bilgisayar adını ve tam etki alanı adını (FQDN) almak için aşağıdaki adımları izleyin:

1. Kaynak grubuna dönün ve yönetilen örneğinizi seçin.

   ![Kaynak grubunda yönetilen örnek](./media/instance-create-quickstart/managed-instance.png)

2. **Genel bakış** sekmesinde **ana bilgisayar** özelliğini bulun. Yönetilen örnek için konak adını sonraki hızlı başlangıçta kullanılmak üzere kopyalayın.

   ![Konak adı](./media/instance-create-quickstart/host-name.png)

   Kopyalanmış değer SQL yönetilen örneğine bağlanmak için kullanılabilecek bir tam etki alanı adı (FQDN) temsil eder. Şu adrese benzer bir örnek: *your_host_name. a1b2c3d4e5f6. Database. Windows. net*.

## <a name="next-steps"></a>Sonraki adımlar

SQL yönetilen örneği 'ne nasıl bağlanacağınızı öğrenmek için:
- Uygulamalar için bağlantı seçeneklerine genel bakış için bkz. [UYGULAMALARıNıZı SQL yönetilen örneğine bağlama](connect-application-instance.md).
- Azure sanal makinesinden SQL yönetilen örneği 'ne bağlanmayı gösteren bir hızlı başlangıç için bkz. [Azure sanal makine bağlantısını yapılandırma](connect-vm-instance-configure.md).
- Noktadan siteye bağlantı kullanarak bir şirket içi istemci bilgisayarından SQL yönetilen örneği 'ne bağlanmayı gösteren bir hızlı başlangıç için bkz. [Noktadan siteye bağlantı yapılandırma](point-to-site-p2s-configure.md).

Mevcut bir SQL Server veritabanını Şirket içinden SQL yönetilen örneği 'ne geri yüklemek için: 
- [Geçiş Için Azure veritabanı geçiş hizmeti](../../dms/tutorial-sql-server-to-managed-instance.md) 'ni kullanarak bir veritabanı yedekleme dosyasından geri yükleme yapın. 
- Bir veritabanı yedekleme dosyasından geri yüklemek için [T-SQL restore komutunu](restore-sample-database-quickstart.md) kullanın.

Yerleşik sorun giderme zekası ile SQL yönetilen örnek veritabanı performansının gelişmiş izlenmesi için, bkz. [Azure SQL Analytics kullanarak Azure SQL yönetilen örneğini izleme](../../azure-monitor/insights/azure-sql.md).
