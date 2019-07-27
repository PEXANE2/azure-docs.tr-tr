---
title: 'Azure portal: SQL veritabanı yönetilen örneği oluşturma | Microsoft Docs'
description: Erişim için bir SQL veritabanı yönetilen örneği, ağ ortamı ve istemci VM 'si oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 05/07/2019
ms.openlocfilehash: 646a602d7e6c47454f039a5f4f1981a60b56fa56
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567525"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Hızlı Başlangıç: Azure SQL veritabanı yönetilen örneği oluşturma

Bu hızlı başlangıçta Azure portal Azure SQL veritabanı [yönetilen örneği](sql-database-managed-instance.md) oluşturma işlemi adım adım açıklanmaktadır.

> [!IMPORTANT]
> Sınırlamalar için bkz. [Desteklenen bölgeler](sql-database-managed-instance-resource-limits.md#supported-regions) ve [desteklenen Abonelik türleri](sql-database-managed-instance-resource-limits.md#supported-subscription-types).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-managed-instance"></a>Yönetilen örnek oluşturma

Aşağıdaki adımlarda, yönetilen bir örnek oluşturma adımları gösterilmektedir.

1. Azure portal, sol üst köşedeki **kaynak oluştur** ' u seçin.
2. **Yönetilen örneği**bulun ve ardından **Azure SQL yönetilen örneği**' ni seçin.
3. **Oluştur**’u seçin.

   ![Yönetilen örnek oluşturma](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Aşağıdaki tablodaki bilgileri kullanarak, istenen bilgilerle **SQL yönetilen örnek** formunu doldurun.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Abonelik** | Aboneliğiniz. | Yeni kaynaklar oluşturmak için size izin veren bir abonelik. |
   |**Yönetilen örnek adı**|Geçerli bir ad.|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Yönetilen örnek yöneticisi oturum açma**|Geçerli bir Kullanıcı adı.|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Bu ayrılmış sunucu düzeyi bir rol olduğundan "serveradmin" kullanmayın.|
   |**Parola**|Herhangi bir geçerli parola.|Parola en az 16 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
   |**Saat dilimi**|Yönetilen örneğiniz tarafından gözlenecek saat dilimi.|Daha fazla bilgi için bkz. [saat dilimleri](sql-database-managed-instance-timezone.md).|
   |**Mediğinden**|Yönetilen örneğiniz için kullanmak istediğiniz harmanlama.|SQL Server veritabanlarını geçirirseniz, kullanarak `SELECT SERVERPROPERTY(N'Collation')` kaynak harmanlamasını denetleyin ve bu değeri kullanın. Harmanlamalar hakkında daha fazla bilgi için bkz. [sunucu harmanlamasını ayarlama veya değiştirme](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation).|
   |**Location**|Yönetilen örneği oluşturmak istediğiniz konum.|Bölgeler hakkında bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/).|
   |**Sanal ağ**|**Yeni sanal ağ** ya da geçerli bir sanal ağ ve alt ağ oluştur ' u seçin.| Bir ağ veya alt ağ kullanılamaz durumdaysa, yeni yönetilen örnek için bir hedef olarak seçmeden önce [ağ gereksinimlerini karşılamak üzere değiştirilmesi](sql-database-managed-instance-configure-vnet-subnet.md) gerekir. Yönetilen bir örnek için ağ ortamını yapılandırma gereksinimleri hakkında bilgi için bkz. [yönetilen bir örnek için sanal ağ yapılandırma](sql-database-managed-instance-connectivity-architecture.md). |
   |**Ortak uç noktayı etkinleştir**   |Genel uç noktayı etkinleştirmek için bu seçeneği işaretleyin   |Yönetilen örneğin genel veri uç noktası üzerinden erişilebilir olması için, **genel uç noktanın** denetlenmesi gerekir.| 
   |**Erişime izin ver**   |Seçeneklerden birini seçin: <ul> <li>**Azure hizmetleri**</li> <li>**Internet**</li> <li>**Erişim yok**</li></ul>   |Portal deneyimi, güvenlik grubunu genel uç nokta ile yapılandırmaya izin vermez. </br> </br> Senaryonuza bağlı olarak, aşağıdaki seçeneklerden birini seçin: </br> <ul> <li>Azure hizmetleri-Power BI veya başka bir çok kiracılı hizmetten bağlanılırken önerilir. </li> <li> Internet-yönetilen bir örneği hızlı bir şekilde çalıştırmak istediğinizde test amaçları için kullanın. Üretim ortamlarında kullanılması önerilmez. </li> <li> Erişim yok-bu seçenek bir reddetme güvenlik kuralı oluşturur. Yönetilen örneği ortak uç nokta aracılığıyla erişilebilir hale getirmek için bu kuralı değiştirmeniz gerekir. </li> </ul> </br> Genel uç nokta güvenliği hakkında daha fazla bilgi için bkz. [genel uç nokta Ile Azure SQL veritabanı yönetilen örneğini güvenli şekilde kullanma](sql-database-managed-instance-public-endpoint-securely.md).|
   |**Bağlantı türü**|Proxy ve yeniden yönlendirme bağlantı türü arasında seçim yapın.|Bağlantı türleri hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı bağlantı ilkesi](sql-database-connectivity-architecture.md#connection-policy).|
   |**Kaynak grubu**|Yeni veya mevcut bir kaynak grubu.|Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|

   ![Yönetilen örnek formu](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Yönetilen örneği bir örnek yük devretme grubu olarak kullanmak için, kullanıma almayı seçin ve DnsAzurePartner yönetilen örneğini belirtin. Bu özellik önizleme aşamasındadır ve aşağıdaki ekran görüntüsünde gösterilmez.
6. İşlem ve depolama kaynaklarının boyutunu belirlemek ve fiyatlandırma katmanı seçeneklerini gözden geçirmek için **fiyatlandırma katmanını** seçin. 32 GB bellek ve 16 sanal çekirdekli Genel Amaçlı fiyatlandırma katmanı varsayılan değerdir.
7. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın.
8. İşiniz bittiğinde seçiminizi kaydetmek için **Uygula** ' yı seçin. 
9. Yönetilen örneği dağıtmak için **Oluştur** ' u seçin.
10. Dağıtım durumunu görüntülemek için **Bildirimler** simgesini seçin.

    ![Yönetilen örnek dağıtımı ilerleme durumu](./media/sql-database-managed-instance-get-started/deployment-progress.png)

11. Dağıtım ilerlemesini daha fazla izlemek için yönetilen örnek penceresini açmak üzere **dağıtım sürüyor '** ı seçin. 

> [!IMPORTANT]
> Bir alt ağdaki ilk örnek için, dağıtım süresi genellikle sonraki örneklerden daha uzun olur. Dağıtım işlemini iptal etmeyin, çünkü beklediğinizden daha uzun sürer.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Kaynakları gözden geçirin ve tam sunucu adınızı alın

Dağıtım başarılı olduktan sonra, oluşturulan kaynakları gözden geçirin ve daha sonra hızlı başlangıçlarda kullanılmak üzere tam sunucu adını alın.

1. Yönetilen örneğiniz için kaynak grubunu açın. [Yönetilen örnek oluşturma](#create-a-managed-instance) hızlı başlangıç bölümünde sizin için oluşturulan kaynaklarını görüntüleyin.

   ![Yönetilen örnek kaynakları](./media/sql-database-managed-instance-get-started/resources.png)

2. Sizin için oluşturulan kullanıcı tanımlı yol (UDR) tablosunu gözden geçirmek için yol tablosunu seçin.

   ![Rota tablosu](./media/sql-database-managed-instance-get-started/route-table.png)

3. Yönlendirme tablosunda, yönetilen örnek sanal ağının içinden ve içinden trafiği yönlendirmek için girişleri gözden geçirin. Yönlendirme tablonuzu el ile oluşturur veya yapılandırırsanız, bu girdileri yol tablosunda oluşturmanız gerekir.

   ![Yönetilen örnek alt ağını yerel olarak giriş](./media/sql-database-managed-instance-get-started/udr.png)

4. Kaynak grubuna geri dönün ve güvenlik kurallarını gözden geçirmek için ağ güvenlik grubunu seçin.

   ![Ağ güvenlik grubu](./media/sql-database-managed-instance-get-started/network-security-group.png)

5. Gelen ve giden güvenlik kurallarını gözden geçirin. Yönetilen örneğiniz için ortak uç noktalar yapılandırdıysanız daha fazla bilgi için [genel uç noktayı yapılandırma](sql-database-managed-instance-public-endpoint-configure.md#allow-public-endpoint-traffic-on-the-network-security-group) makalesine bakın.

   ![Güvenlik kuralları](./media/sql-database-managed-instance-get-started/security-rules.png)

6. Kaynak grubuna geri dönün ve yönetilen örneğinizi seçin.

   ![Yönetilen örnek](./media/sql-database-managed-instance-get-started/managed-instance.png)

7. **Genel bakış** sekmesinde **ana bilgisayar** özelliğini bulun. Bir sonraki hızlı başlangıçta kullanılmak üzere yönetilen örnek için tam konak adresini kopyalayın.

   ![Konak adı](./media/sql-database-managed-instance-get-started/host-name.png)

   Ad, **your_machine_name. a1b2c3d4e5f6. Database. Windows. net**ile benzerdir.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen bir örneğe nasıl bağlanacağınızı öğrenmek için:
  - Uygulamalar için bağlantı seçeneklerine genel bakış için bkz. [uygulamalarınızı yönetilen bir örneğe bağlama](sql-database-managed-instance-connect-app.md).
  - Azure sanal makinesinden yönetilen bir örneğe bağlanmayı gösteren bir hızlı başlangıç için bkz. [Azure sanal makine bağlantısını yapılandırma](sql-database-managed-instance-configure-vm.md).
  - Noktadan siteye bağlantı kullanarak bir şirket içi istemci bilgisayarından yönetilen bir örneğe bağlanmayı gösteren hızlı başlangıç için bkz. [Noktadan siteye bağlantı yapılandırma](sql-database-managed-instance-configure-p2s.md).
- Mevcut bir SQL Server veritabanını Şirket içinden yönetilen bir örneğe geri yüklemek için: 
    - Bir veritabanı yedekleme dosyasından geri yüklemek için [geçiş Için Azure veritabanı geçiş hizmeti 'ni (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) kullanın. 
    - Bir veritabanı yedekleme dosyasından geri yüklemek için [T-SQL restore komutunu](sql-database-managed-instance-get-started-restore.md) kullanın.
- Yerleşik sorun giderme zekası ile yönetilen örnek veritabanı performansının gelişmiş izlenmesi için, bkz. [Azure SQL Analytics kullanarak Azure SQL veritabanını izleme](../azure-monitor/insights/azure-sql.md).
