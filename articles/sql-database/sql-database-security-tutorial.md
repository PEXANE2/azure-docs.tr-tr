---
title: Tek veya havuzlu bir veritabanını güvenli hale
description: Azure SQL Veritabanı'nda tek veya havuza edilmiş bir veritabanını güvence altına almak için size teknikler ve özellikler hakkında eğitim veren bir öğretici.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
ms.date: 09/03/2019
ms.custom: seoapril2019
ms.openlocfilehash: 9e3c5c12157a007bcad59a78b4623ff4d5a0041f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238835"
---
# <a name="tutorial-secure-a-single-or-pooled-database"></a>Öğretici: Tek veya havuza veritabanını güvenli hale

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Sunucu düzeyi ve veritabanı düzeyinde güvenlik duvarı kuralları oluşturma
> - Azure Etkin Dizin (AD) yöneticisini yapılandırma
> - SQL kimlik doğrulaması, Azure AD kimlik doğrulaması ve güvenli bağlantı dizeleri ile kullanıcı erişimini yönetme
> - Gelişmiş veri güvenliği, denetim, veri maskeleme ve şifreleme gibi güvenlik özelliklerini etkinleştirme

Azure SQL Veritabanı, verileri tek veya havuza alınan bir veritabanında, şunları yapabilirsinize izin vererek güvence altına almaktadır:

- Güvenlik duvarı kurallarını kullanarak erişimi sınırlandırın
- Kimlik gerektiren kimlik doğrulama mekanizmalarını kullanma
- Rol tabanlı üyelikler ve izinlerle yetkilendirme kullanma
- Güvenlik özelliklerini etkinleştirme

> [!NOTE]
> Yönetilen bir örnekteki Azure SQL veritabanı, Azure SQL veritabanı yönetilen [örnek](sql-database-managed-instance-index.yml) ve bağlantı [mimarisinde](sql-database-managed-instance-connectivity-architecture.md)açıklandığı gibi ağ güvenliği kuralları ve özel uç noktaları kullanılarak güvenli hale gelir.

Daha fazla bilgi edinmek için [Azure SQL Veritabanı güvenlik genel bakışı](/azure/sql-database/sql-database-security-index) ve [yetenek](sql-database-security-overview.md) makalelerine bakın.

> [!TIP]
> Aşağıdaki Microsoft Learn modülü, Azure SQL [Veritabanınızı](https://docs.microsoft.com/learn/modules/secure-your-azure-sql-database/)nasıl güvenli hale erdirdiğiniz için ücretsiz bilgi edinmenize yardıyor.

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyi tamamlamak için aşağıdaki ön koşullara sahip olduğunuzdan emin olun:

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)
- Azure SQL sunucusu ve veritabanı
  - [Azure portalı](sql-database-single-database-get-started.md), [CLI](sql-database-cli-samples.md)veya [PowerShell](sql-database-powershell-samples.md) ile oluşturun

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Öğreticideki tüm adımlar için [Azure portalında](https://portal.azure.com/) oturum açın

## <a name="create-firewall-rules"></a>Güvenlik duvarı kuralları oluşturma

SQL veritabanları Azure'daki güvenlik duvarları yla korunur. Varsayılan olarak, sunucu ve veritabanına tüm bağlantılar reddedilir. Daha fazla bilgi edinmek için [Azure SQL Veritabanı sunucu düzeyi ve veritabanı düzeyinde güvenlik duvarı kurallarına](sql-database-firewall-configure.md)bakın.

En güvenli yapılandırma için **Azure hizmetlerine erişime izin ver'** **in.** Ardından, Azure VM veya bulut hizmeti gibi bağlanması gereken kaynak için ayrılmış bir [IP (klasik dağıtım)](../virtual-network/virtual-networks-reserved-public-ip.md) oluşturun ve yalnızca bu IP adresinin güvenlik duvarı üzerinden erişmesine izin verin. [Kaynak yöneticisi](/azure/virtual-network/virtual-network-ip-addresses-overview-arm) dağıtım modelini kullanıyorsanız, her kaynak için özel bir genel IP adresi gerekir.

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir şirket ağı içinden bağlanmaya çalışıyorsanız, 1433 bağlantı noktası üzerindeki giden trafiğe ağınızın güvenlik duvarı izin vermeyebilir. Bu nedenle, yöneticiniz 1433 bağlantı noktasını açmadıkça Azure SQL Veritabanı sunucusuna bağlanamazsınız.

### <a name="set-up-sql-database-server-firewall-rules"></a>SQL Database sunucu güvenlik duvarı kurallarını ayarlama

Sunucu düzeyindeKI IP güvenlik duvarı kuralları, aynı SQL Veritabanı sunucusundaki tüm veritabanları için geçerlidir.

Sunucu düzeyinde bir güvenlik duvarı kuralı ayarlamak için:

1. Azure portalında, sol menüden **SQL veritabanlarını** seçin ve **SQL veritabanları** sayfasında veritabanınızı seçin.

    ![sunucu güvenlik duvarı kuralı](./media/sql-database-security-tutorial/server-name.png)

    > [!NOTE]
    > Daha sonra eğitimde kullanılmak üzere tam nitelikli sunucu adınızı *(yourserver.database.windows.net*gibi) kopyalayıp emin olun.

1. Genel **Bakış** sayfasında sunucu **güvenlik duvarını ayarla'yı**seçin. Veritabanı sunucusuiçin **Güvenlik Duvarı ayarları** sayfası açılır.

   1. Geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğuna **istemci IP ekle'yi** seçin. Kural, tek bir IP adresi veya çeşitli IP adresleri için 1433 bağlantı noktasını açabilir. **Kaydet'i**seçin.

      ![sunucu güvenlik duvarı kuralı ayarla](./media/sql-database-security-tutorial/server-firewall-rule2.png)

   1. **Tamam'ı** seçin ve **Güvenlik Duvarı ayarları** sayfasını kapatın.

Artık sunucuda belirtilen IP adresine veya IP adresi aralığına sahip herhangi bir veritabanına bağlanabilirsiniz.

### <a name="setup-database-firewall-rules"></a>Veritabanı güvenlik duvarı kurallarını kurulum

Veritabanı düzeyinde güvenlik duvarı kuralları yalnızca tek tek veritabanları için geçerlidir. Veritabanı, sunucu nun başarısız olduğu bir dönemde bu kuralları korur. Veritabanı düzeyindegüvenlik duvarı kuralları yalnızca Transact-SQL (T-SQL) deyimleri kullanılarak ve yalnızca sunucu düzeyinde bir güvenlik duvarı kuralını yapılandırdıktan sonra yapılandırılabilir.

Veritabanı düzeyinde güvenlik duvarı kuralını kurmak için:

1. Örneğin SQL Server Management [Studio'yu](./sql-database-connect-query-ssms.md)kullanarak veritabanına bağlanın.

1. **Object Explorer'da**veritabanına sağ tıklayın ve **Yeni Sorgu'yu**seçin.

1. Sorgu penceresinde, bu ifadeyi ekleyin ve ip adresini ortak IP adresinize değiştirin:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

1. Araç çubuğunda, **Execute** güvenlik duvarı kuralını oluşturmak için Yürüt''ün'u seçin.

> [!NOTE]
> *Ana* veritabanına bağlı olmanızı gerekirken, [sp_set_firewall_rule](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database?view=azuresqldb-current) komutunu kullanarak SSMS'te sunucu düzeyinde bir güvenlik duvarı kuralı da oluşturabilirsiniz.

## <a name="create-an-azure-ad-admin"></a>Azure AD yöneticisi oluşturma

Uygun Azure Etkin Dizin (AD) yönetilen etki alanını kullandığınızdan emin olun. AD etki alanını seçmek için Azure portalının sağ üst köşesini kullanın. Bu işlem, aynı aboneliğin hem Azure AD hem de Azure SQL veritabanınızı veya veri ambarınızı barındıran SQL Server için kullanıldığını doğrular.

   ![seçim-reklam](./media/sql-database-security-tutorial/8choose-ad.png)

Azure AD yöneticisini ayarlamak için:

1. Azure portalında, **SQL sunucu** sayfasında **Active Directory yöneticisini**seçin. Sonraki **seçimi Set admin**.

    ![active directory seçme](./media/sql-database-security-tutorial/admin-settings.png)  

    > [!IMPORTANT]
    > Bu görevi gerçekleştirmek için "Şirket Yöneticisi" veya "Genel Yönetici" olmanız gerekir.

1. Yönetici **Ekle** sayfasında, AD kullanıcısını veya grubunu arayın ve seçin ve **Seç'i**seçin. Etkin Dizininizin tüm üyeleri ve grupları listelenir ve gri renkte olan girişler Azure AD yöneticileri olarak desteklenmez. [Azure REKLAM özelliklerine ve sınırlamalarını](sql-database-aad-authentication.md#azure-ad-features-and-limitations)görün.

    ![yönetici seçin](./media/sql-database-security-tutorial/admin-select.png)

    > [!IMPORTANT]
    > Rol tabanlı erişim denetimi (RBAC) yalnızca portal için geçerlidir ve SQL Server'a yayılmaz.

1. **Active Directory yönetici** sayfasının üst kısmında **Kaydet'i**seçin.

    Yönetici değiştirme işlemi birkaç dakika sürebilir. Yeni yönetici **Active Directory yönetici** kutusunda görünür.

> [!NOTE]
> Azure AD yöneticisi ayarlarken, yeni yönetici adı (kullanıcı veya grup) *ana* veritabanında SQL Server kimlik doğrulama kullanıcısı olarak bulunamaz. Varsa, kurulum başarısız olur ve bu tür bir yönetici adının zaten var olduğunu belirten değişiklikleri geri alacaktır. SQL Server kimlik doğrulama kullanıcısı Azure AD'nin bir parçası olmadığından, kullanıcıyı Azure AD kimlik doğrulaması kullanarak bağlama çabası başarısız olur.

Azure AD yapılandırma hakkında bilgi için bkz:

- [Şirket içi kimliklerinizi Azure AD ile tümleştirme](../active-directory/hybrid/whatis-hybrid-identity.md)
- [Azure AD'ye kendi etki alanı adınızı ekleyin](../active-directory/active-directory-domains-add-azure-portal.md)
- [Microsoft Azure artık Windows Server AD ile federasyonu destekliyor](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/)
- [Azure AD dizinini yönetme](../active-directory/fundamentals/active-directory-administer.md)
- [PowerShell'i kullanarak Azure AD'yi yönetme](/powershell/azure/overview?view=azureadps-2.0)
- [Hibrit kimlik gerekli bağlantı noktaları ve protokoller](../active-directory/hybrid/reference-connect-ports.md)

## <a name="manage-database-access"></a>Veritabanı erişimini yönetme

Kullanıcıları veritabanına ekleyerek veya güvenli bağlantı dizeleriyle kullanıcı erişimine izin vererek veritabanı erişimini yönetin. Bağlantı dizeleri dış uygulamalar için yararlıdır. Daha fazla bilgi için [girişleri ve kullanıcı hesaplarını ve](sql-database-manage-logins.md) AD kimlik [doğrulamasını](sql-database-aad-authentication.md)yönet'e bakın.

Kullanıcı eklemek için veritabanı kimlik doğrulama türünü seçin:

- **SQL kimlik doğrulaması,** oturum açmalar için bir kullanıcı adı ve parola kullanın ve yalnızca sunucu içinde belirli bir veritabanı bağlamında geçerlidir

- **Azure AD kimlik doğrulaması**, Azure AD tarafından yönetilen kimlikleri kullanma

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

SQL kimlik doğrulaması olan bir kullanıcı eklemek için:

1. Örneğin SQL Server Management [Studio'yu](./sql-database-connect-query-ssms.md)kullanarak veritabanına bağlanın.

1. **Object Explorer'da**veritabanına sağ tıklayın ve **Yeni Sorgu'yu**seçin.

1. Sorgu penceresinde aşağıdaki komutu girin:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

1. Araç çubuğunda, **Execute** kullanıcıyı oluşturmak için Yürüt'''ün

1. Varsayılan olarak, kullanıcı veritabanına bağlanabilir ancak verileri okuma veya yazma izni yoktur. Bu izinleri vermek için, yeni bir sorgu penceresinde aşağıdaki komutları uygulayın:

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

> [!NOTE]
> Yeni kullanıcılar oluşturma gibi yönetici görevlerini yürütmeleri gerekmedikçe, veritabanı düzeyinde yönetici olmayan hesaplar oluşturun.

### <a name="azure-ad-authentication"></a>Azure AD kimlik doğrulaması

Azure Active Directory kimlik doğrulaması, veritabanı kullanıcılarının içerdiği şekilde oluşturulmasını gerektirir. İçerdiği veritabanı kullanıcı eşlemleri, Azure AD dizinindeki bir kimliğe eş tanır ve *ana* veritabanında oturum açmaz. Azure AD kimliği tek bir kullanıcı veya grup için olabilir. Daha fazla bilgi için [Bkz. İçe Bilgi veritabanı kullanıcıları, veritabanınızı taşınabilir hale getirin](https://msdn.microsoft.com/library/ff929188.aspx) ve Azure AD'yi kullanarak kimlik doğrulaması yapma hakkında [Azure AD öğreticisini](./sql-database-aad-authentication-configure.md) gözden geçirin.

> [!NOTE]
> Azure portalı kullanılarak veritabanı kullanıcıları (yöneticiler hariç) oluşturulamaz. Azure RBAC rolleri SQL sunucularına, veritabanlarına veya veri ambarlarına yayılmaz. Bunlar yalnızca Azure kaynaklarını yönetmek için kullanılır ve veritabanı izinleri için geçerli değildir.
>
> Örneğin, *SQL Server Katılımcısı* rolü bir veritabanına veya veri ambarına bağlanmak için erişim sağlamaz. Bu izin, T-SQL deyimleri kullanılarak veritabanı içinde verilmelidir.

> [!IMPORTANT]
> İki nokta üst `:` üste veya `&` ampersand gibi özel karakterler T-SQL `CREATE LOGIN` ve `CREATE USER` deyimlerde kullanıcı adlarında desteklenmez.

Azure AD kimlik doğrulaması olan bir kullanıcı eklemek için:

1. En azından *ALTER ANY USER* izniyle bir Azure REKLAM hesabı kullanarak Azure SQL sunucunuza bağlanın.

1. **Object Explorer'da**veritabanına sağ tıklayın ve **Yeni Sorgu'yu**seçin.

1. Sorgu penceresinde aşağıdaki komutu girin `<Azure_AD_principal_name>` ve Azure AD kullanıcısının ana adını veya Azure AD grubunun görüntü adını değiştirin:

   ```sql
   CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
   ```

> [!NOTE]
> Azure AD kullanıcıları veritabanı meta verilerinde `E (EXTERNAL_USER)` gruplar `X (EXTERNAL_GROUPS)` için tür ve tür içeren olarak işaretlenir. Daha fazla bilgi için [bkz: sys.database_principals.](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql)

### <a name="secure-connection-strings"></a>Güvenli bağlantı dizeleri

İstemci uygulaması ve SQL veritabanı arasında güvenli ve şifreli bir bağlantı sağlamak için bir bağlantı dizesi aşağıdakilere göre yapılandırılmalıdır:

- Şifreli bağlantı isteği
- Sunucu sertifikasına güvenmeyin

Bağlantı, Transport Layer Security (TLS) kullanılarak kurulur ve ortadaki adam saldırısı riskini azaltır. Bağlantı dizeleri veritabanı başına kullanılabilir ve ADO.NET, JDBC, ODBC ve PHP gibi istemci sürücüleri desteklemek için önceden yapılandırılmıştır. TLS ve bağlantı hakkında daha fazla bilgi için bkz. [TLS konuları](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

Güvenli bir bağlantı dizesini kopyalamak için:

1. Azure portalında, sol menüden **SQL veritabanlarını** seçin ve **SQL veritabanları** sayfasında veritabanınızı seçin.

1. Genel **Bakış** sayfasında veritabanı **bağlantı dizelerini göster'i**seçin.

1. Bir sürücü sekmesi seçin ve bağlantı dizesinin tamamını kopyalayın.

    ![ADO.NET bağlantı dizesi](./media/sql-database-security-tutorial/connection.png)

## <a name="enable-security-features"></a>Güvenlik özelliklerini etkinleştirme

Azure SQL Veritabanı, Azure portalı kullanılarak erişilen güvenlik özellikleri sağlar. Bu özellikler, yalnızca veritabanında bulunan veri maskeleme dışında hem veritabanı hem de sunucu için kullanılabilir. Daha fazla bilgi için Gelişmiş [veri güvenliği,](sql-database-advanced-data-security.md) [Denetim,](sql-database-auditing.md) [Dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md)ve Saydam veri [şifreleme'ye](transparent-data-encryption-azure-sql.md)bakın.

### <a name="advanced-data-security"></a>Gelişmiş veri güvenliği

Gelişmiş veri güvenliği özelliği, olası tehditleri oluştukları anda algılar ve anormal etkinliklerhakkında güvenlik uyarıları sağlar. Kullanıcılar bu şüpheli olayları denetim özelliğini kullanarak keşfedebilir ve olayın veritabanındaki verilere erişip erişmemeyebileceğini, ihlal edip etmeyebileceğini belirleyebilir. Kullanıcılara ayrıca bir güvenlik açığı değerlendirmesi ve veri bulma ve sınıflandırma aracını içeren bir güvenlik genel bakışı da sağlanır.

> [!NOTE]
> Örnek bir tehdit, saldırganların uygulama girişlerine kötü amaçlı SQL enjekte ettiği bir işlem olan SQL enjeksiyonudur. Bir uygulama daha sonra bilmeden kötü amaçlı SQL yürütmek ve saldırganların ihlali veya veritabanında veri değiştirmek için erişim sağlar.

Gelişmiş veri güvenliğini etkinleştirmek için:

1. Azure portalında, sol menüden **SQL veritabanlarını** seçin ve **SQL veritabanları** sayfasında veritabanınızı seçin.

1. Genel **Bakış** sayfasında **Sunucu adı** bağlantısını seçin. Veritabanı sunucusu sayfası açılır.

1. SQL **sunucu** sayfasında **Güvenlik** bölümünü bulun ve **Gelişmiş Veri Güvenliği'ni**seçin.

   1. Özelliği etkinleştirmek için **Gelişmiş Veri Güvenliği** altında **A.B.'yi** seçin. Güvenlik açığı değerlendirme sonuçlarını kaydetmek için bir depolama hesabı seçin. Ardından **Kaydet'i**seçin.

      ![Gezinti bölmesi](./media/sql-database-security-tutorial/threat-settings.png)

      Ayrıca, güvenlik uyarıları, depolama ayrıntıları ve tehdit algılama türleri alacak şekilde e-postaları yapılandırabilirsiniz.

1. Veritabanınızın **SQL veritabanları** sayfasına dönün ve **Güvenlik** bölümü altında Gelişmiş **Veri Güvenliği'ni** seçin. Burada veritabanı için çeşitli güvenlik göstergeleri bulabilirsiniz.

    ![Tehdit durumu](./media/sql-database-security-tutorial/threat-status.png)

Anormal etkinlikler algılanırsa, olay hakkında bilgi içeren bir e-posta alırsınız. Bu, etkinliğin doğasını, veritabanını, sunucuyu, olay zamanını, olası nedenlerini ve olası tehdidi araştırmak ve azaltmak için önerilen eylemleri içerir. Böyle bir e-posta alınırsa, Azure portalını başlatmak ve olay saatine ait ilgili denetim kayıtlarını göstermek için **Azure SQL Denetim Günlüğü** bağlantısını seçin.

   ![Tehdit algılama e-postası](./media/sql-database-security-tutorial/threat-email.png)

### <a name="auditing"></a>Denetim

Denetim özelliği veritabanı olaylarını izler ve olayları Azure depolama, Azure Monitor günlükleri veya bir etkinlik hub'ındaki bir denetim günlüğüne yazar. Denetim, mevzuata uygunluğun korunmasına, veritabanı etkinliğini anlamanıza ve olası güvenlik ihlallerini gösterebilecek tutarsızlıklar ve anormallikler hakkında bilgi edinmeye yardımcı olur.

Denetimi etkinleştirmek için:

1. Azure portalında, sol menüden **SQL veritabanlarını** seçin ve **SQL veritabanları** sayfasında veritabanınızı seçin.

1. **Güvenlik** bölümünde **Denetim'i**seçin.

1. **Denetim** ayarları altında, aşağıdaki değerleri ayarlayın:

   1. **Denetimi AYıLt'a** ayarlayın. **ON**

   1. Denetim **günlüğü hedefini** aşağıdakilerden biri olarak seçin:

       - **Depolama**, olay günlüklerinin kaydedildiği ve *.xel* dosyaları olarak indirilebildiği bir Azure depolama hesabı

          > [!TIP]
          > Denetim raporu şablonlarından en iyi şekilde yararlanmak için denetlenen tüm veritabanları için aynı depolama hesabını kullanın.

       - Sorgu veya daha fazla analiz için olayları otomatik olarak depolayan **Log Analytics**

           > [!NOTE]
           > Analytics, özel uyarı kuralları ve Excel veya Power BI dışa aktarma gibi gelişmiş özellikleri desteklemek için **Log Analytics çalışma alanı** gereklidir. Çalışma alanı olmadan, yalnızca sorgu düzenleyicisi kullanılabilir.

       - **Olayların**diğer uygulamalarda kullanılmak üzere yönlendirilmesine olanak tanıyan Olay Hub'ı

   1. **Kaydet'i**seçin.

      ![Denetim ayarları](./media/sql-database-security-tutorial/audit-settings.png)

1. Artık veritabanı olayları verilerini görüntülemek için **denetim günlüklerini** görüntüle'yi seçebilirsiniz.

    ![Denetim kayıtları](./media/sql-database-security-tutorial/audit-records.png)

> [!IMPORTANT]
> PowerShell veya REST API kullanarak denetim olaylarını nasıl daha da özelleştireceğimiz konusunda [SQL veritabanı denetimine](sql-database-auditing.md) bakın.

### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme

Veri maskeleme özelliği, hassas verileri veritabanınızda otomatik olarak gizler.

Veri maskeleme etkinleştirmek için:

1. Azure portalında, sol menüden **SQL veritabanlarını** seçin ve **SQL veritabanları** sayfasında veritabanınızı seçin.

1. **Güvenlik** bölümünde Dinamik **Veri Maskeleme'yi**seçin.

1. **Dinamik veri maskeleme** ayarları altında, maske leme kuralı eklemek için **maske ekle'yi** seçin. Azure, seçilecek kullanılabilir veritabanı şemalarını, tabloları ve sütunlarını otomatik olarak dolduracaktır.

    ![Maske ayarları](./media/sql-database-security-tutorial/mask-settings.png)

1. **Kaydet'i**seçin. Seçili bilgiler artık gizlilik için maskelenir.

    ![Maske örneği](./media/sql-database-security-tutorial/mask-query.png)

### <a name="transparent-data-encryption"></a>Saydam veri şifrelemesi

Şifreleme özelliği verilerinizi otomatik olarak şifreler ve şifrelenmiş veritabanına erişen uygulamalarda değişiklik gerektirmez. Yeni veritabanları için şifreleme varsayılan olarak açıktır. Ayrıca SSMS ve [Her Zaman şifrelenmiş](sql-database-always-encrypted.md) özelliği kullanarak verileri şifreleyebilirsiniz.

Şifrelemeyi etkinleştirmek veya doğrulamak için:

1. Azure portalında, sol menüden **SQL veritabanlarını** seçin ve **SQL veritabanları** sayfasında veritabanınızı seçin.

1. **Güvenlik** **bölümünde, Saydam veri şifreleme'yi**seçin.

1. Gerekirse, **Veri şifrelemesini** **ON**olarak ayarlayın. **Kaydet'i**seçin.

    ![Saydam Veri Şifrelemesi](./media/sql-database-security-tutorial/encryption-settings.png)

> [!NOTE]
> Şifreleme durumunu görüntülemek için [SSMS](./sql-database-connect-query-ssms.md) kullanarak veritabanına `encryption_state` bağlanın ve [sys.dm_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) görünümünün sütununa sorgulayın. Bir durum `3` veritabanının şifrelenmiş olduğunu gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, veritabanınızın güvenliğini birkaç basit adımla geliştirmeyi öğrendiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> - Sunucu düzeyi ve veritabanı düzeyinde güvenlik duvarı kuralları oluşturma
> - Azure Etkin Dizin (AD) yöneticisini yapılandırma
> - SQL kimlik doğrulaması, Azure AD kimlik doğrulaması ve güvenli bağlantı dizeleri ile kullanıcı erişimini yönetme
> - Gelişmiş veri güvenliği, denetim, veri maskeleme ve şifreleme gibi güvenlik özelliklerini etkinleştirme

Coğrafi dağılımı nasıl uygulayacağınızı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[Coğrafi olarak dağıtılmış bir veritabanı uygulama](sql-database-implement-geo-distributed-database.md)
