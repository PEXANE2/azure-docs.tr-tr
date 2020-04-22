---
title: Güvenli bir web uygulaması geliştirin | Microsoft Dokümanlar
description: Bu basit örnek uygulama, Azure'da geliştirdiğinizde uygulamanızı ve kuruluşunuzun güvenlik duruşunu iyileştiren en iyi güvenlik uygulamalarını uygular.
keywords: yok
services: security
documentationcenter: na
author: isaiah-msft
manager: barbkess
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security
ms.subservice: security-develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: terrylan
ms.openlocfilehash: 730e478622da8cd90af1c559e4d0c6fd04151cca
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686802"
---
# <a name="develop-a-secure-web-app"></a>Güvenli web uygulaması geliştirme

Bu örnek, Azure'da uygulama geliştirmek için güvenlik kaynaklarına bağlantılar içeren bir web sayfası görüntüleyen basit bir Python uygulamasıdır. Uygulama, Azure'da uygulamalar geliştirdiğinizde uygulamanızın ve kuruluşunuzun güvenlik duruşunu iyileştirmeye yardımcı olabilecek en iyi güvenlik uygulamalarını uygular.

Uygulama bileşenlerinin düzgün şekilde yapılandırıldığından emin olmak için bu makalede açıklanan adımları sırayla izlemeniz gerekir. Veritabanı, Azure Uygulama Hizmeti, Azure Anahtar Kasası örneği ve Azure Uygulama Ağ Geçidi örneği birbirine bağlıdır.

Dağıtım komut dosyaları altyapıyı ayarlar. Dağıtım komut dosyalarını çalıştırdıktan sonra, bileşenleri ve hizmetleri birbirine bağlamak için Azure portalında bazı el ile yapılandırma yapmanız gerekir.

Örnek uygulama, uygulamalarında güvenlik önlemleri uygulamak isteyen Azure'da yeni başlayanlar için hedeflenmiştir.

Bu uygulamayı geliştirirken ve dağıtAbiliyorsanız, şunları öğreneceksiniz:

- Bir Azure Anahtar Kasası örneği oluşturun, ondan sırları depolayın ve alın.
- PostgreSQL için Azure Veritabanı'nı dağıtın, güvenli parolalar ayarlayın ve bu veritabanına erişim yetkisi verin.
- Linux için Azure Web Apps'ta bir Alpine Linux kapsayıcısı çalıştırın ve Azure kaynakları için yönetilen kimlikleri etkinleştirin.
- [OWASP Top 10 Ruleset](https://coreruleset.org/)kullanan bir güvenlik duvarıyla bir Azure Uygulama Ağ Geçidi örneği oluşturun ve yapılandırın.
- Azure hizmetlerini kullanarak aktarım sırasında ve istirahatte veri şifrelemesini etkinleştirin.

Bu uygulamayı geliştirip dağıttıktan sonra, açıklanan yapılandırma ve güvenlik önlemleriyle birlikte aşağıdaki örnek web uygulamasını kurmuş olabilirsiniz.

![Örnek web uygulaması](./media/secure-web-app/demo-app.png)

## <a name="architecture"></a>Mimari

Uygulama, üç katmanlı tipik bir n katmanı uygulamasıdır. İzleme ve gizli yönetim bileşenleritümleşik ön uç, arka uç ve veritabanı katmanı burada gösterilmiştir:

![Uygulama mimarisi](./media/secure-web-app/architecture.png)

Mimari şu bileşenlerden oluşur:

- [Azure Uygulama Ağ Geçidi](../../application-gateway/index.yml). Uygulama mimarimiz için ağ geçidi ve güvenlik duvarı sağlar.
- [Linux'ta Azure Web Uygulamaları](../../app-service/containers/app-service-linux-intro.md). Python uygulamasını Linux ortamında çalıştırmak için kapsayıcı çalışma süresini sağlar.
- [Azure Anahtar Kasası](../../key-vault/index.yml). Uygulamamızın sırlarını saklar ve şifreler ve bunların etrafında erişim politikaları oluşturulmasını yönetir.
- [PostgreSQL için Azure Veritabanı](https://azure.microsoft.com/services/postgresql/). Uygulamamızın verilerini güvenli bir şekilde saklar.
- [Azure Güvenlik Merkezi](../../security-center/index.yml) ve [Azure Uygulama Öngörüleri.](../../azure-monitor/app/app-insights-overview.md) Uygulamamızın işleyişinde izleme ve uyarılar sağlar.

## <a name="threat-model"></a>Tehdit modeli

Tehdit modelleme, işletmenize ve uygulamanıza yönelik olası güvenlik tehditlerini belirleme ve ardından uygun bir azaltma planının uygulanmasını sağlama işlemidir.

Bu örnek, güvenli örnek uygulama için tehdit modellemesi uygulamak için [Microsoft Tehdit Modelleme Aracı'nı](threat-modeling-tool.md) kullanmıştır. Bileşenleri ve veri akışlarını diyagramlayarak, geliştirme sürecinin başlarında sorunları ve tehditleri tanımlayabilirsiniz. Bu daha sonra zaman ve para tasarrufu sağlar.

Bu örnek uygulama için tehdit modeli:

![Tehdit modeli](./media/secure-web-app/threat-model.png)

Tehdit modelleme aracının oluşturduğu bazı örnek tehditler ve olası güvenlik açıkları aşağıdaki ekran görüntüsünde gösterilir. Tehdit modeli, maruz kalan saldırı yüzeyine genel bir bakış sağlar ve geliştiricileri sorunları nasıl azaltacakları konusunda düşünmeye teşvik eder.

![Tehdit modeli çıktısı](./media/secure-web-app/threat-model-output.png)

Örneğin, önceki tehdit modeli çıkışındaki SQL enjeksiyonu, kullanıcı girişlerinin temizlenmesi ve PostgreSQL için Azure Veritabanı'nda depolanan işlevler kullanılarak azaltılır. Bu azaltma, veri okuma ve yazma sırasında sorguların rasgele yürütülmesini engeller.

Geliştiriciler, tehdit modeli çıkışındaki tehditlerin her birini azaltarak sistemin genel güvenliğini artırır.

## <a name="deployment"></a>Dağıtım

Aşağıdaki seçenekler, Azure Uygulama Hizmeti'nde Linux çalıştırmanızı sağlar:

- Azure'da destekleyici teknolojilerle oluşturulmuş önceden oluşturulmuş Microsoft kapsayıcıları listesinden (Python, Ruby, PHP, Java, Node.js, .NET Core) bir kapsayıcı seçin.
- Özel olarak üretilen bir kapsayıcı kullanın. Görüntünün kaynağı olarak kendi konteyner kayıt defterlerinizi seçin ve HTTP'yi destekleyen birçok kullanılabilir teknoloji üzerine inşa edin.

Bu örnekte, webapp'ı Uygulama Hizmeti'ne dağıtacak ve kaynakları oluşturacak dağıtım komut dosyasını çalıştırırsınız.

Uygulama aşağıda gösterilen farklı dağıtım modellerini kullanabilir:

![Dağıtım veri akış diyagramı](./media/secure-web-app/deployment.png)

Azure'da uygulamaları dağıtmanın birçok yolu vardır:

- Azure Resource Manager şablonları
- PowerShell
- Azure CLI
- Azure portal
- Azure DevOps

Bu uygulama kullanılır:

- [Docker](https://docs.docker.com/) oluşturmak ve konteyner görüntüleri oluşturmak için.
- Dağıtım için [Azure CLI.](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Konteyner kayıt defteri olarak [Docker Hub.](https://hub.docker.com/)

## <a name="security-considerations"></a>Güvenlik konuları

### <a name="network"></a>Ağ

Örnek uygulama, ağa giren ve çıkan akıta geçen veriler için uçtan uca TLS/SSL şifrelemesi kullanır. Ağ geçidi, kendi imzalı bir sertifikayla yapılandırılır.
> [!IMPORTANT]
> Bu gösteride kendi imzalı bir sertifika kullanılır. Üretim ortamında, doğrulanmış bir Sertifika Yetkilisi'nden (CA) sertifika almanız gerekir.

Uygulama güvenlik duvarı ayrıca gelen trafiği denetler ve ağ trafiğinde kötü amaçlı trafik algılandığında yöneticileri uyarır.
Uygulama Ağ Geçidi, tehdit modelinde bulunan DDoS ve SQL enjeksiyon tehditleri olasılığını azaltır.

### <a name="identity"></a>Kimlik

Portalda oturum açmak için örnek uygulama, kaynaklara erişim atanmış Azure Etkin Dizin (Azure AD) yöneticileri için Çok Faktörlü Kimlik Doğrulaması'nı kullanır.
Örnek uygulama, Azure Key Vault'tan sırları okuma ve alma izinleri elde etmek için yönetilen kimlikleri kullanır ve uygulamanın sırları okumak için kod kimlik bilgileri ne gerek duymamasını sağlar. Azure AD, yönetilen kimlikler kullanıldığında uygulamanın okuması gereken hizmet ilkelerini otomatik olarak oluşturur ve sırları değiştirir.

Azure kaynakları ve MFA için yönetilen kimlikler, düşmanların ayrıcalık kazanmasını ve sistemdeki ayrıcalıklarını yükseltmesini zorlaştırır. Bu tehdit, tehdit modelinde işaret edildi.
Uygulama, OAuth uygulamasına kayıtlı kullanıcıların uygulamada oturum açmalarına olanak tanıyan OAuth'u kullanır.

### <a name="storage"></a>Depolama

PostgreSQL veritabanındaki veriler, PostgreSQL için Azure Veritabanı tarafından otomatik olarak şifrelenir. Veritabanı, yalnızca dağıtılan App Service web uygulamasının doğru kimlik doğrulama kimlik bilgileriyle veritabanı kaynaklarına erişebilmeleri için App Service IP adreslerini yetkilendiriyor.

### <a name="logging-and-auditing"></a>Günlük kaydı ve denetim

Uygulama, oluşan ölçümleri, günlükleri ve özel durumları izlemek için Uygulama Öngörüleri'ni kullanarak günlüğe kaydetmeyi uygular. Bu günlük, geliştiricileri ve operasyon ekibi üyelerini uygulamanın durumu hakkında bilgilendirmek için yeterli uygulama meta verisi sağlar. Ayrıca, güvenlik olayları durumunda geri dönmek için yeterli veri sağlar.

## <a name="cost-considerations"></a>Maliyetle ilgili konular

Zaten bir Azure hesabınız yoksa, ücretsiz bir hesap oluşturabilirsiniz. Başlamak için [ücretsiz hesap sayfasına](https://azure.microsoft.com/free/) gidin, ücretsiz bir Azure hesabıyla neler yapabileceğinizi görün ve hangi ürünlerin 12 ay boyunca ücretsiz olduğunu öğrenin.

Güvenlik özellikleriyle örnek uygulamadaki kaynakları dağıtmak için bazı premium özellikler için ödeme yapmanız gerekir. Uygulama ölçeklendirildikçe ve Azure tarafından sunulan ücretsiz katmanlar ve denemeler uygulama gereksinimlerini karşılamak için yükseltilmesi gerektiğinden, maliyetleriniz artabilir. Maliyetlerinizi tahmin etmek için Azure [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

### <a name="prerequisites"></a>Ön koşullar

Uygulamayı çalışır hale getirmek için aşağıdaki araçları yüklemeniz gerekir:

- Uygulama kodunu değiştirmek ve görüntülemek için bir kod düzenleyicisi. [Visual Studio Code](https://code.visualstudio.com/) açık kaynak seçeneğidir.
- Geliştirme bilgisayarınızda [Azure CLI.](/cli/azure/install-azure-cli)
- Sisteminizde [git.](https://git-scm.com/) Git, kaynak kodunu yerel olarak klonlamak için kullanılır.
- [jq](https://stedolan.github.io/jq/), kullanıcı dostu bir şekilde JSON sorgulama kiçin bir UNIX aracı.

Örnek uygulamanın kaynaklarını dağıtmak için bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa, örnek uygulamayı test etmek için [ücretsiz bir hesap oluşturabilirsiniz.](https://azure.microsoft.com/free/)

Bu araçları yükledikten sonra uygulamayı Azure'da dağıtmaya hazırsınız.

### <a name="environment-setup"></a>Ortamı ayarlama

Ortamı ve aboneliği ayarlamak için dağıtım komut dosyalarını çalıştırın:

1. Kaynak kod deposunu klonlamak için şu Git komutunu kullanın:

   ```shell
   git clone https://github.com/Azure-Samples/sample-linux-python-app tutorial-project
   ```

2. Dizine geçmek için şu komutu kullanın:

   ```shell
   cd tutorial-project/scripts
   ```

3. Komut dosyaları klasöründe kullandığınız platforma (Windows veya Linux) özgü dosyalar vardır. Azure CLI zaten yüklenmiş olduğundan, bu Azure CLI komutunu çalıştırarak komut isteminde Azure hesabında oturum açın:

   ```azurecli-interactive
   az login
   ```

Tarayıcı açılır, kimlik bilgilerinizle oturum açar. Oturum açtıktan sonra, komut isteminden kaynakları dağıtmaya başlayabilirsiniz.

Dağıtım komut `deploy-powershell.ps1` dosyaları `deploy-bash.sh` ve tüm uygulamayı dağıtan kod içerir.
Çözümü dağıtmak için:

1. PowerShell'deyseniz, bölge `deploy-powershell.ps1` ve `./deploy-powershell.ps1 REGION RESOURCE_GROUP_NAME` kaynak grup adını uygun Azure bölgeleri ve kaynak grubu için bir adla değiştirerek dosyayı çalıştırın
2. Linux'ta ysanız dosyayı `deploy-bash.sh` yazarak `/deploy-bash.sh REGION RESOURCE_GROUP_NAME`çalıştırın, dosyayı yazarak yürütülebilir hale getirmeniz gerekebilir`chmod +x deploy-bash.sh`

Aşağıdaki örneklerde, anahtar bileşenlerin parçacıkları sergilenebilmektedir. Dağıtı dosyalarını çalıştırarak örnekleri tek tek veya bileşenlerin geri kalanıyla birlikte dağıtabilirsiniz.

### <a name="implementation-guidance"></a>Uygulama kılavuzu

Dağıtım komut dosyası, dört aşamaya ayrılabilen bir komut dosyasıdır. Her [aşama, mimari diyagramında](#architecture)yer alan bir Azure kaynağını dağıtır ve yapılandırır.

Dört aşama:

- Azure Anahtar Kasası'nı dağıtın.
- PostgreSQL için Azure Veritabanı'nı dağıtın.
- Azure Web Uygulamalarını Linux'ta dağıtın.
- Uygulama Ağ Geçidi'ni web uygulaması güvenlik duvarıyla dağıtın.

Her aşama, önceden dağıtılan kaynaklardan gelen yapılandırmayı kullanarak bir önceki aşamaya göre inşa edilir.

Uygulama adımlarını tamamlamak [için, Önkoşullar](#prerequisites)altında listelenen araçları yüklediğinizden emin olun.

#### <a name="deploy-azure-key-vault"></a>Azure Anahtar Kasası Dağıtma

Bu bölümde, sırları ve sertifikaları depolamak için kullanılan bir Azure Anahtar Kasası örneği oluşturabilir ve dağıtabilirsiniz.

Dağıtımı tamamladıktan sonra, Azure'da dağıtılan bir Azure Anahtar Kasası örneğine sahip siniz.

Azure CLI'yi kullanarak Azure Anahtar Kasası'nı dağıtmak için:

1. Azure Anahtar Kasası için değişkenleri bildirin.
2. Azure Anahtar Kasası sağlayıcısını kaydedin.
3. Örnek için kaynak grubu oluşturun.
4. Adım 3'te oluşturulan kaynak grubunda Azure Anahtar Kasası örneğini oluşturun.

   ```powershell-interactive

    function Get-Hash() {
        return (New-Guid).Guid.Split('-')[4]
    }

   az provider register -n Microsoft.KeyVault

   # Create the Azure Key Vault instance
   Write-Host "Creating Azure Key Vault instance: $($kvName)"
   az keyvault create --name $kvName `
       --resource-group $ResourceGroup `
       --location $Location `
       --verbose

   # Generate usernames and passwords using system functions and environment variables
   Write-Host "Generating PostgreSQL username and password"
   $pgUsername = "$($env:Username)$(Get-Hash)"
   $pgPassword = (New-Guid).Guid

   # Set the username secret in the Azure Key Vault instance
   Write-Host "Setting PostgreSQL username in KeyVault"
   az keyvault secret set --vault-name $kvName `
       --name PGUSERNAME `
       --value $pgUsername `
       --verbose

   # Set the password secret in the Azure Key Vault instance
   Write-Host "Setting PostgreSQL password in KeyVault"
   az keyvault secret set --vault-name $kvName `
       --name PGPASSWORD `
       --value $pgPassword `
       --verbose

   ```

Kaynaklara erişmek için Key Vault kullanan uygulamalarda Azure kaynakları için yönetilen kimlikleri kullanmak en iyi yöntemdir. Key Vault'un erişim anahtarları kodda veya yapılandırmada depolandığında güvenlik duruşunuz artar.

#### <a name="deploy-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanını Dağıtma

PostgreSQL için Azure Veritabanı aşağıdaki şekilde çalışır, önce veritabanı sunucusunu oluşturun, ardından şemayı ve verileri depolamak için veritabanını oluşturun.

Dağıtımı tamamladıktan sonra, Azure'da çalışan bir PostgreSQL sunucunuz ve veritabanınız olur.

Azure CLI'yi kullanarak PostgreSQL için Azure Veritabanı'nı dağıtmak için:

1. Azure CLI ve Azure abonelik kurulumunuzla bir terminal açın.
2. Veritabanına erişmek için kullanılan güvenli bir kullanıcı adı ve parola birleşimi oluşturun. (Bunlar, bunları kullanan uygulamalar için Azure Key Vault'ta depolanmalıdır.)
3. PostgreSQL sunucu örneğini oluşturun.
4. Adım 3'te oluşturduğunuz sunucu örneğinde bir veritabanı oluşturun.
5. PostgreSQL örneğinde PostgreSQL komut dosyalarını çalıştırın.

Aşağıdaki kod, yukarıdaki anahtar atlama adımından Azure KeyVault'ta depolanan PGUSERNAME ve PGPASSWORD sırlarına dayanır.

   ```powershell-interactive
   $pgUsername = $(az keyvault secret show --name PGUSERNAME --vault-name $kvName --query value) -replace '"',''
   $pgPassword = $(az keyvault secret show --name PGPASSWORD --vault-name $kvName --query value) -replace '"',''

   # Create an Azure Database for PostgreSQL server
   Write-Host "Creating the PostreSQL server: $($dbServer)"
   az  postgres server create -l $Location `
       --resource-group $ResourceGroup `
       --name $dbServer `
       --admin-user $pgUsername `
       --admin-password $pgPassword `
       --sku-name B_Gen5_1 `
       --verbose

   # Create a database in the server instance created above
   Write-Host "Creating the PostgreSQL database: $($dbName)"
   az postgres db create --resource-group $ResourceGroup `
       --server-name $dbServer `
       --name $dbName `
       --verbose

   # Retrieve the database information above
   $db = (az postgres server show --resource-group $ResourceGroup `
           --name $dbServer)

   $db = $db | ConvertFrom-Json

   # Get the Fully Qualified Domain Name (FQDN) of the database to use in
   # the database connection strings to be stored in Azure Key Vault
   Write-Host "Generating the PostgreSQL connection string"
   $PGCONNECTIONSTRING = "postgresql+psycopg2://${pgUsername}@$($db.fullyQualifiedDomainName):${pgPassword}@$($db.fullyQualifiedDomainName):$dbPort/$($dbName)?sslmode=$($dbSSLMode)"
   $PGCONNECTIONSTRING = $PGCONNECTIONSTRING + '"&"' + "sslrootcert=$($dbRootCertPath)"

   # Set the database connection string above into Azure Key Vault
   Write-Host "Setting the PostgreSQL connection string into KeyVault"
   az keyvault secret set --vault-name $kvName `
       --name PGCONNECTIONSTRING `
       --value $PGCONNECTIONSTRING `
       --verbose
   ```

Veritabanını dağıttıktan sonra kimlik bilgilerini ve bağlantı dizesini Azure Key Vault'ta depolamanız gerekir.
Komut dosyaları klasöründe, çalıştırdığınızda depolanan işlevleri oluşturan PL/pgSQL kodunu içeren bir `functions.sql` dosya vardır. Bu dosyanın çalıştırılması, SQL enjeksiyonu sınırlamak için girişleri parametreize eder.

PostgreSQL veritabanına bağlanmak için `psql` kullanılan bir araç ile birlikte verilir. Çalıştırmak `functions.sql`için, yerel makinenizden PostgreSQL örneği için Azure Veritabanı'na bağlanmanız ve buradan çalıştırmanız gerekir. Psql aracının yüklenmesi, her işletim sisteminde PostgreSQL için varsayılan yüklemeye dahildir.
Daha fazla bilgi için [psql Belgeleri'ne](https://www.postgresql.org/docs/9.3/app-psql.html)bakın.

Azure Bulut Kabuğu `psql` aracı da içerir. Bulut Kabuğu Simgesini seçerek Bulut Kabuğu'nu doğrudan Azure portalından kullanabilirsiniz.

PostgreSQL örneğine uzaktan erişimi etkinleştirmek için PostgreSQL'deki IP adresini yetkilendirmeniz gerekir.
**Bağlantı güvenlik** sekmesine giderek, istemci IP **Ekle'yi**seçerek ve yeni ayarları kaydederek bu erişimi etkinleştirin.

![İstemci IP'yi yetkilendirme](./media/secure-web-app/add-client-ip-postgres.png)

Yerel psql aracı yerine Cloud Shell kullanıyorsanız, **Azure hizmetlerine erişime izin ver'i** seçin ve Cloud Shell erişiminiz için değerini **ON** olarak değiştirin.

Ardından, Azure portalındaki PostgreSQL örneğinin **Bağlantı dizeleri** sekmesinden bağlantı dize parametreleri ile aşağıdaki psql komutunu çalıştırarak örneğe bağlanın.
Boş ayraçları veritabanının Bağlantı String bıçağındaki parametrelerle ve parolayla Azure Key Vault'un parolasıyla değiştirin.

```shell
psql "host={} port=5432 dbname=hellodb user={} password=PGPASSWORD sslmode=require"
```

Veritabanına bağlı olduğunuzdan emin olduktan sonra aşağıdaki PL/pgSQL komut dosyasını çalıştırın. Komut dosyası, veritabanına veri eklemek için kullanılan depolanan işlevleri oluşturur.

```shell
CREATE OR REPLACE FUNCTION insert_visitor(country VARCHAR(40), browser VARCHAR(40), operating_system VARCHAR(40)) RETURNS void AS $$
BEGIN
    INSERT INTO visitor(
        country,
        browser,
        operating_system,
        date_visited)
    VALUES (
        country,
        browser,
        operating_system,
        NOW()
    );
END;
$$ LANGUAGE PLPGSQL;

CREATE OR REPLACE FUNCTION insert_azure_document(title VARCHAR(40), url VARCHAR(100), category VARCHAR(40)) RETURNS void AS $$
BEGIN
    INSERT INTO azure_document(
        title,
        url,
        category)
    VALUES (
        title,
        url,
        category
    );
END;
$$ LANGUAGE PLPGSQL;
```

PostgreSQL için TLS ve Sertifika Yetkilisi (CA) doğrulaması nasıl ayarlandığı hakkında daha fazla bilgi için, [PostgreSQL için Azure Veritabanı'nda TLS bağlantısını yapılandırma ya](/azure/postgresql/concepts-ssl-connection-security)da yapılandırma bilgisine bakın.

Bir kök sertifikası kapsayıcıya dahildir. Sertifikayı almak için atılan adımlar şunlardır:

1. [Sertifika](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt)Yetkilisi'nden sertifika dosyasını indirin.
2. [OpenSSL'yi makinenize indirin ve kurun.](/azure/postgresql/concepts-ssl-connection-security)
3. Sertifika dosyanızın kodunu çöz:

   ```shell
   openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
   ```

PostgreSQL için TLS güvenliğinin nasıl yapılandırılabildiğini buradan okuyabilirsiniz [TLS Bağlantı Güvenliğini Yapılandırın.](/azure/postgresql/concepts-ssl-connection-security)

#### <a name="deploy-azure-web-apps-on-linux"></a>Azure Web Uygulamalarını Linux'ta dağıtma

Azure, Python, Ruby, C#ve Java gibi yaygın olarak kullanılan diller için önceden oluşturulmuş kapsayıcılar ve görüntüler sağladığından, Azure Uygulama Hizmeti'nin üstüne Linux hizmetlerini kolayca oluşturabilirsiniz. Azure, hemen hemen tüm programlama dillerinin Azure Uygulama Hizmeti platformunda çalışmasına izin veren özel kapsayıcıları da destekler.

Dağıtılan uygulama en son Ubuntu Linux dağıtımı üzerinde çalışan basit bir Python uygulamasıdır. Kimlik bilgisi yönetimi ve veri depolama için önceki bölümlerde oluşturulan Azure Key Vault ve PostgreSQL örneklerine bağlanır.

Aşağıdaki Docker dosyası uygulamanın kök klasöründe sağlanır:

```dockerfile
# Docker file for the basic web app
# Using the latest Alpine Linux

FROM alpine:latest

# Copy requirements to the container
COPY requirements.txt /tmp/requirements.txt

# Install Python and PostgreSQL dependencies
RUN apk update && \
    apk add --update bash gcc libffi-dev musl-dev  postgresql-dev python3 python3-dev && \
    rm -r /usr/lib/python*/ensurepip && \
    pip3 install --upgrade pip setuptools && \
    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
    if [[ ! -e /usr/bin/python ]]; then ln -sf /usr/bin/python3 /usr/bin/python; fi && \
    pip3 install --no-cache-dir -r /tmp/requirements.txt && \
    rm -rf /.wh /root/.cache /var/cache /tmp/requirements.txt

# Change the working directory inside the container to /hello
WORKDIR /hello

# Copy the application code into the container
COPY . /hello

# Set the FLASK_APP environment variable used by flask migrate
ENV FLASK_APP=app.py

# Copy the init script to the container
COPY init.sh /usr/local/bin/

# Make the init script executable
RUN  chmod u+x /usr/local/bin/init.sh

# Expose the container web service endpoint
EXPOSE 8000

# Run the app with a non root user
RUN addgroup -g 1000 -S appgroup && \
    adduser  -u 1000 -S appuser -G appgroup

# Allow the non root user to access the folder
RUN chown -R appuser:appgroup /hello

# Switch to the non root user
USER appuser

# Set the init script as the file to be run during container startups
ENTRYPOINT ["/usr/local/bin/init.sh"]
```

Yukarıdaki Dockerfile, Azure Kapsayıcı Kayıt Defteri'nde `mcr.microsoft.com/samples/basic-linux-app`barındırılan kapsayıcıyı oluşturmak için kullanılır.

Aşağıdaki kod:

1. Uygulama Hizmeti örneğinin değişkenlerini ve adlarını bildirir.
2. Uygulama Hizmeti planı için kaynak grubu oluşturur.
3. Linux kapsayıcıları örneğinde Bir Azure Web Uygulamaları sağlar.
4. Web uygulaması kapsayıcısı için günlüğe kaydetmeyi sağlar.
5. Kapsayıcının uygulama ayarlarında bazı uygulama yapılandırmaları ayarlar.

   ```powershell-interactive
   Write-Host "Retrieving the Azure Key Vault URL"
   $kvURI = $(az keyvault show --name $kvName --query properties.vaultUri)

   # Create the App Service plan, using --linux for running containers on Web Apps on Linux
   Write-Host "Creating App Service Plan: $($appName)"
   az appservice plan create --name $appServicePlanName `
       --resource-group $ResourceGroup `
       --location $Location `
       --number-of-workers 1 `
       --sku B1 `
       --is-linux `
       --verbose

   # Create the web app
   Write-Host "Creating Azure Web App for Linux: $($appName)"
   az webapp create --name $appName `
       --resource-group $ResourceGroup `
       --plan $appServicePlanName `
       --deployment-container-image-name $containerName `
       --verbose

   # Assign a system-assigned identity
   # This creates a service principal to be used for managed identities for Azure resources allowing access to Key Vault Secrets without using auth keys/tokens
   Write-Host "Assigning Service Principal Identity to webapp: $($appName)"
   az webapp identity assign --name $appName `
       --resource-group $ResourceGroup `
       --verbose

   # Configure logging for the Docker container on App Service
   Write-Host "Configuring logging for the web app: $($appName)"
   az webapp log config --name $appName `
       --resource-group $ResourceGroup `
       --application-logging true `
       --detailed-error-messages true `
       --docker-container-logging filesystem `
       --verbose

   # Set app configuration settings to be set in the container's environment variables
   Write-Host "Setting app settings for our web app: $($appName)"
   az webapp config appsettings set --name $appName `
       --resource-group $ResourceGroup `
       --settings WEBSITE_TIME_ZONE=$timezone KEY_VAULT_URI=$kvURI `
       --verbose

   # Stop the web app to enable us to first allow the created managed identities for Azure resources service principals access to the Key Vault instance
   az webapp stop --name $appName `
       --resource-group $ResourceGroup `
       --verbose

   # Get all the outbound IPs the App Service instance might use
   Write-Host "Adding outbound Azure App Service IP's to the PostgreSQL database firewall."
   $outboundIps = (az webapp show --resource-group $ResourceGroup `
       --name $appName `
       --query outboundIpAddresses `
       --output tsv)

   # Loop over all the outbound IP addresses and authorize them in the PostgreSQL firewall
   $outboundIps = $outboundIps.Split(',')
   for($i=0; $i -lt $outboundIps.length; $i++) {
       Write-Output "Adding IP Rule $($outboundIps[$i]) on PostgreSQL for App Service"

       az postgres server firewall-rule create --name "OUTBOUND_IP_RULE$i" `
           --resource-group $ResourceGroup `
           --server-name $dbServer `
           --start-ip-address $outboundIps[$i] `
           --end-ip-address $outboundIps[$i] `
           --verbose
   }
   ```

Bu komut dosyası, kod veya yapılandırmada sert kodlama sırları olmadan Azure Key Vault ile etkileşim kurmak için MSI ile kullanılabilecek Uygulama Hizmeti örneği için atanmış bir kimlik oluşturur.

Erişim ilkesi sekmesinde atanan kimliği yetkilendirmek için portaldaki Azure Anahtar Kasası örneğine gidin. **Yeni erişim ilkesi ekle'yi**seçin. **Select principal**altında, oluşturulan Uygulama Hizmeti örneğinin adına benzer uygulama adını arayın.
Uygulamaya bağlı bir hizmet ilkesi görünür olmalıdır. Aşağıdaki ekran görüntüsünde gösterildiği gibi, seçin ve erişim ilkesi sayfasını kaydedin.

Uygulamanın yalnızca anahtarları alması gerektiğinden, verilen ayrıcalıkları azaltırken erişime izin vererek sırlar seçeneklerinden izin **al'ı** seçin.

![Anahtar Vault Erişim Politikası](./media/secure-web-app/kv-access-policy.png)

*Key Vault erişim ilkesi oluşturma*

Erişim ilkesini kaydedin ve ilkeleri güncelleştirmek için **Access İlkeleri** sekmesindeki yeni değişikliği kaydedin.

#### <a name="deploy-application-gateway-with-web-application-firewall-enabled"></a>Web uygulaması güvenlik duvarı etkinleştirilmiş Uygulama Ağ Geçidi'ni dağıtma

Web uygulamalarında, hizmetleri doğrudan internet üzerinden dış dünyaya maruz bırakmanız önerilmez.
Yük dengeleme ve güvenlik duvarı kuralları, gelen trafik üzerinde daha fazla güvenlik ve denetim sağlar ve bunu yönetmenize yardımcı olur.

Uygulama Ağ Geçidi örneğini dağıtmak için:

1. Uygulama ağ geçidini barındırmak için kaynak grubu oluşturun.
2. Ağ geçidine bağlanmak için sanal bir ağ sağlama.
3. Sanal ağdaki ağ geçidi için bir alt ağ oluşturun.
4. Genel bir IP adresi sağlama.
5. Uygulama ağ geçidini sağlama.
6. Ağ geçidinde web uygulaması güvenlik duvarını etkinleştirin.

   ```powershell-interactive
   az keyvault certificate create --vault-name $kvName `
       --name $certName `
       --policy `@policy.json `
       --verbose

   az keyvault secret download --file $filePath `
       --encoding base64 `
       --name $certName `
       --vault-name $kvName `
       --verbose

   $pfxFile = Get-PfxData -FilePath $filePath

   $certPassword = Get-Random

   az keyvault secret set --vault-name $kvName `
       --name CERTPASSWORD `
       --value $certPassword `
       --verbose

   $signPassword = ConvertTo-SecureString $certPassword -Force -AsPlainText
   Export-PfxCertificate -PFXData $pfxFile -FilePath $certPath -Password $signPassword
   ```

Önceki komut dosyası:

1. Azure'da kendi imzalı yeni bir sertifika oluşturur.
2. Kendi imzalı sertifikayı base64 kodlanmış bir dosya olarak karşıdan yükler.
3. Kendi imzalı sertifika için bir parola oluşturur.
4. Sertifikayı parolayla imzalanmış bir PFX dosyası olarak dışa aktarır.
5. Sertifikanın parolasını Azure Key Vault'ta saklar.

Bu bölümde uygulama ağ geçidi dağıdır:

```powershell-interactive
# Create a virtual network required by the gateway
Write-Host "Creating the Azure Virtual Network: $($vnetName)"
az network vnet create --name $vnetName `
    --resource-group $ResourceGroup `
    --location $Location `
    --address-prefix $vnetAddressPrefix `
    --verbose

# Add a subnet to the virtual network
Write-Host "Creating the Subnet: $($gwSubnet)"
az network vnet subnet create --name $gwSubnet `
    --resource-group $ResourceGroup `
    --vnet-name $vnetName `
    --address-prefix $gatewayAddressPrefix `
    --verbose

# Create a public IP address that will be used by clients to access the application gateway
Write-Host "Creating the Public IP Address: $($publicIpName)"
az network public-ip create --resource-group $ResourceGroup `
    --name $publicIpName `
    --verbose

# Create the application gateway
Write-Host "Creating the Application Gateway: $($gwName)"
az network application-gateway create `
    --name $gwName `
    --resource-group $ResourceGroup `
    --location $Location `
    --vnet-name $vnetName `
    --subnet $gwSubnet `
    --public-ip-address $publicIpName `
    --http-settings-cookie-based-affinity Disabled `
    --frontend-port 443 `
    --http-settings-protocol Https `
    --http-settings-port 443 `
    --capacity 2 `
    --sku WAF_Medium `
    --cert-file $certPath `
    --cert-password $certPassword `
    --verbose

# Enable the firewall with OWASP Ruleset 3.0 on the application gateway
Write-Host "Creating the Application Gateway WAF Configuration"
az network application-gateway waf-config set `
    --enabled true `
    --gateway-name $gwName `
    --resource-group $ResourceGroup `
    --firewall-mode Detection `
    --rule-set-version 3.0 `
    --verbose

# Retrieve the name of the HTTP settings that will be updated below
$gwHTTPSettings = $(az network application-gateway http-settings list --resource-group $ResourceGroup `
    --gateway-name $gwName)

$gwHTTPSettings = $gwHTTPSettings | ConvertFrom-Json
$gwHTTPSettingsName = $gwHTTPSettings.name

# Retrieve the name of the backend address pool that will be updated below
$gwAddressPool = $(az network application-gateway address-pool list --resource-group $ResourceGroup `
    --gateway-name $gwName)

$gwAddressPool = $gwAddressPool | ConvertFrom-Json
$gwAddressPoolName = $gwAddressPool.name

# Update the backend pool with the App Service host name
Write-Host "Updating the Azure Application Gateway backend pool host name: $($appHostName)"
az network application-gateway address-pool update --name $gwAddressPoolName `
    --resource-group $ResourceGroup `
    --gateway-name $gwName `
    --servers $appHostName `
    --verbose

# Create a probe that will check the backend pool's availability
Write-Host "Updating the Azure Application Gateway Probe: $($gwProbe)"
az network application-gateway probe create --gateway-name $gwName `
    --name $gwProbe `
    --resource-group $ResourceGroup `
    --protocol Https `
    --path $gwProbePath `
    --host-name-from-http-settings true `
    --verbose

# Update the app to user https and to pick the host name from the backend settings
Write-Host "Deploying the updated application gateway"
az network application-gateway http-settings update --gateway-name $gwName `
    --resource-group $ResourceGroup `
    --name $gwHTTPSettingsName `
    --connection-draining-timeout 0 `
    --enable-probe true `
    --host-name-from-backend-pool true `
    --probe $gwProbe `
    --protocol Https `
    --port 443 `
    --verbose
```

Dağıtımı tamamladıktan sonra, web uygulaması güvenlik duvarı etkin leştirilmiş bir uygulama ağ geçidiniz vardır.

Ağ geçidi örneği HTTPS için 443 bağlantı noktasını ortaya çıkarır. Bu yapılandırma, uygulamamıza yalnızca HTTPS üzerinden 443 bağlantı noktasından erişilmesini sağlar.

Kullanılmayan bağlantı noktalarını engellemek ve saldırı yüzeyine maruz kalmanın sınırlandırılması en iyi güvenlik uygulamasıdır.

#### <a name="add-network-security-groups-to-the-app-service-instance"></a>Uygulama Hizmeti örneğine ağ güvenlik grupları ekleme

Uygulama Hizmeti örnekleri sanal ağlarla tümleştirilebilir. Bu tümleştirme, uygulamanın gelen ve giden trafiğini yöneten ağ güvenliği grubu ilkeleriyle yapılandırılmasını sağlar.

1. Bu özelliği etkinleştirmek için, **Ayarlar**altında Azure Uygulaması hizmet örneği bıçak üzerinde **Ağ'ı**seçin. Sağ bölmede, **VNet Tümleştirmesi**altında, **yapılandırmak için buraya tıklayın'ı**seçin.

   ![Yeni sanal ağ entegrasyonu](./media/secure-web-app/app-vnet-menu.png)

    *App Service için yeni sanal ağ entegrasyonu*

1. Bir sonraki sayfada **VNET Ekle 'yi (önizleme)** seçin.

1. Sonraki menüde, dağıtımda oluşturulan ve .' `hello-vnet`ile başlayan sanal ağı seçin. Yeni bir alt ağ oluşturabilir veya varolan bir alt ağ seçebilirsiniz.
   Bu durumda, yeni bir alt ağ oluşturun. Adres **aralığını** **10.0.3.0/24** olarak ayarlayın ve alt **net app-subnet'inadını**belirleyin.

   ![App Service sanal ağ yapılandırması](./media/secure-web-app/app-vnet-config.png)

    *App Service için sanal ağ yapılandırması*

Artık sanal ağ tümleştirmesini etkinleştirdiğinize göre, uygulamamıza ağ güvenlik grupları ekleyebilirsiniz.

1. Arama kutusunu kullanın, **ağ güvenlik gruplarını**arayın. Sonuçlarda **Ağ güvenlik gruplarını** seçin.

    ![Ağ güvenlik gruplarını arama](./media/secure-web-app/nsg-search-menu.png)

    *Ağ güvenlik gruplarını arama*

2. Sonraki menüde **Ekle'yi**seçin. NSG'nin **adını** ve bulunması gereken **Kaynak grubunu** girin. Bu NSG, uygulama ağ geçidinin alt ağına uygulanacaktır.

    ![NSG oluşturma](./media/secure-web-app/nsg-create-new.png)

    *NSG oluşturma*

3. NSG oluşturulduktan sonra seçin. Bıçaklarında, **Ayarlar**altında **Gelen Güvenlik kurallarını**seçin. Bu ayarları, bağlantı noktası 443 üzerinden uygulama ağ geçidine gelen bağlantılara izin verecek şekilde yapılandırın.

   ![NSG'yi yapılandırın](./media/secure-web-app/nsg-gateway-config.png)

   *NSG'yi yapılandırın*

4. Ağ geçidi NSG için giden kurallarda, hizmet etiketini `AppService`hedefleyen bir kural oluşturarak Uygulama Hizmeti örneğine giden bağlantılara izin veren bir kural ekleyin:

   ![NSG için giden kuralları ekleme](./media/secure-web-app/nsg-outbound-allowappserviceout.png)

   *NSG için giden kuralları ekleme*

    Ağ geçidinin giden kuralları sanal ağa göndermesine izin vermek için başka bir giden kural ekleyin.

   ![Başka bir giden kuralı ekleme](./media/secure-web-app/nsg-outbound-vnet.png)

    *Başka bir giden kuralı ekleme*

5. NSG'nin alt ağlarında, **Ilişkilendir'i**seçin , dağıtımda oluşturulan sanal ağı seçin ve **gw-subnet**adlı ağ geçidi alt ünü seçin. NSG alt ağa uygulanır.

6. Önceki adımda olduğu gibi, bu kez App Service örneği için başka bir NSG oluşturun. Ona bir isim ver. Uygulama ağ geçidi NSG için yaptığınız gibi bağlantı noktası 443 için gelen kuralı ekleyin.

   Bir Uygulama Hizmeti Ortamı örneğinde dağıtılan bir Uygulama Hizmeti örneğiniz varsa, ki bu uygulama için geçerli değildir, Uygulama Hizmeti NSG'nizin gelen güvenlik gruplarında 454-455 bağlantı noktalarını açarak Azure Hizmet Durumu sondalarına izin vermek için gelen kurallar ekleyebilirsiniz. Yapılandırma şu şekildedir:

   ![Azure Hizmet Durumu sondaları için kurallar ekleme](./media/secure-web-app/nsg-create-healthprobes.png)

    *Azure Hizmet Durumu sondaları için kurallar ekleme (yalnızca Uygulama Hizmet Ortamı)*

7. Giden güvenlik kurallarında, Uygulama Hizmeti örneğinin PostgreSQL veritabanıyla iletişim kurmasını sağlayan yeni bir giden güvenlik kuralı oluşturun. Şu şekilde yapılandırın:

   ![Giden PostgreSQL bağlantılarına izin verme kuralı](./media/secure-web-app/nsg-outbound-postgresql.png)

   *Giden PostgreSQL bağlantılarına izin vermek için kural ekleme*

Saldırı yüzeyini sınırlamak için, Uygulama Hizmeti ağ ayarlarını yalnızca uygulama ağ geçidinin uygulamaya erişmesine izin verecek şekilde değiştirin.
Bunu, App Service ağ sekmesine giderek, **IP Kısıtlamaları** sekmesini seçerek ve yalnızca uygulama ağ geçidinin IP'sinin hizmete doğrudan erişmesine izin veren bir kural oluşturarak yaparsınız.

Genel bakış sayfasından ağ geçidinin IP adresini alabilirsiniz. IP **Adresi CIDR** sekmesine, ip adresini bu `<GATEWAY_IP_ADDRESS>/32`biçimde girin: .

![Yalnızca ağ geçidine izin ver](./media/secure-web-app/app-allow-gw-only.png)

*Uygulama Hizmetine erişmek için yalnızca ağ geçidi IP'sine izin ver*

#### <a name="implement-azure-active-directory-oauth"></a>Azure Active Directory OAuth'u uygulayın

Örnek web uygulaması sayfasında dağıtılan Azure belgeleri, uygulamamızda korunması gereken kaynaklardır. Farklı kimlik doğrulama akışları kullanarak web, masaüstü ve mobil uygulamalar için kimlik doğrulamasını uygulamak için Azure Active Directory 'i (Azure AD) kullanabilirsiniz.
Uygulama, tek kiracılı Azure AD kullanıcı listemize eklenen kullanıcıların profillerini okumasına olanak tanıyan **Microsoft ile Giriş'i**kullanır.

Azure portalında, uygulamayı gerekli kimlik bilgilerini kullanacak şekilde yapılandırın:

1. **Azure Etkin Dizini'ni**seçin veya arama kutusunu kullanarak bu dizinde arama yapın.

2. **Yeni kayıt**seçin:

   ![Kayıt oluşturma](./media/secure-web-app/ad-auth-create.png)

   *Azure AD uygulama kaydı oluşturma*

3. Bir sonraki sayfaya uygulama adını girin. **Desteklenen hesap türleri**altında, yalnızca bu kuruluş **dizinindeki Hesapları**seçin.
    **Redirect URI**altında, uygulamanın artı bir belirteç bitiş noktası ile çalışacak temel etki alanını girin. Örneğin: *GATEWAY_HASH*.cloudapp.net/token.

   ![Azure AD uygulama kaydını yapılandırma](./media/secure-web-app/ad-auth-type.png)

   *Azure AD uygulama kaydını yapılandırma*

4. Size kayıtlı uygulamayı ve bilgilerini gösteren bir ekran sunulur. Bu bilgileri Azure Anahtar Kasası örneğine eklemeniz gerekir.
   1. Uygulama (istemci) kimliğini kopyalayın ve Key `CLIENTID`Vault'a 'olarak kaydedin.
   2. Önceki adımda girdiğiniz yeniden yönlendirme URI'yi `REDIRECTURI`kopyalayın ve '' olarak kaydedin.
   3. Biçim *adı*.microsoftonline.com olan Azure AD varsayılan dizin adını kopyalayın ve `TENANT`Anahtar Kasası'na 'olarak kaydedin.
   4. Daha önce oluşturduğunuz Azure AD uygulamasının **Sertifikalar & Sırları** sekmesine gidin ve aşağıdaki ekran görüntüsünde gösterildiği gibi **Yeni istemci sırrını**seçin. Bir son kullanma tarihi ayarlayın ve ardından oluşturulan değeri `CLIENTSECRET`kopyalayın ve Key Vault'a 'olarak kaydedin.

      ![Azure AD yetkilendirme sırrı](./media/secure-web-app/ad-auth-secrets.png)

      *Azure AD yetkilendirme sırrı*

   5. Herhangi bir komut satırı/çevrimiçi aracı kullanarak güvenli bir rasgele gizli anahtar oluşturun. Olarak Key Vault `FLASKSECRETKEY`içine kaydedin. Uygulama çerçevesi oturumoluşturmak için bu anahtarı kullanır.
        Gizli bir anahtarı niçin oluşturacağınızı öğrenmek için [Flask Sessions'a](http://flask.pocoo.org/docs/1.0/quickstart/#sessions)bakın.

5. Oturum açma işlemlerini yapılandırıldıktan sonra, kaynakta oturum açmalarına izin vermek için kullanıcıları Azure AD bağlantısına eklemeniz gerekir. Bunları eklemek için Azure AD'deki **Kullanıcılar** sekmesine gidin, **Tüm kullanıcıları**seçin ve ardından Yeni kullanıcı veya **Yeni** **konuk kullanıcıyı**seçin. Sınama için bir konuk kullanıcı ekleyebilir ve kullanıcıyı dizine davet edebilirsiniz. Veya uygulamanın üzerinde çalıştığınıalan doğrulanmışsa yeni bir kullanıcı ekleyebilirsiniz. Bu örnekte, yalnızca Azure AD kiracısında kayıtlı kullanıcılar erişim için kaydedilebilir. Çok kiracılı oturum açma erişimi hakkında bilgi için belgelere bakın.

   ![Kullanıcıları varsayılan etki alanına ekleme](./media/secure-web-app/ad-auth-add-user.png)

   *Kullanıcıları varsayılan Azure Etkin Dizin etki alanına ekleme*

Key Vault'a Azure AD yapılandırmasını ve sırlarını ekledikten sonra, kullanıcılar Azure OAuth kimlik doğrulaması kullanarak uygulamaya kimlik doğrulaması yapabilir.
Uygulama kodunda, bu işlem Azure Etkin Dizin Kimlik Doğrulama Kitaplığı (ADAL) tarafından işlenir.

Sırlar Key Vault'ta ve uygulama nın sırlara ve veritabanına erişebildiği için, uygulama hizmetine\/ağ geçidinin başvuru URL'sinden (https: /GATEWAY_HASH.cloudapp.net) ulaşılabilir.

Azure AD'de oturum açtığınızda, "Kullanıcı oturum açmaya çalıştığınız dizine kayıtlı değil" yazan bir hata alırsanız, kullanıcıyı eklemeniz gerekir. Kullanıcıyı eklemek için Azure AD'nin **Kullanıcılar** sekmesine gidin ve bilgilerini girerek kullanıcıyı el ile ekleyin veya konuk kullanıcı olarak e-posta adresini **Davet Konuk** bıçağında Azure AD'ye girerek kullanıcıyı davet edin.

#### <a name="deploy-application-insights"></a>Application Insights'ı dağıtma
Uygulama dağıtılmış ve çalıştığına göre, günlüğe kaydetme ve veri toplamayı izlemeyle birlikte uygulama içinde oluşan hataları işlemeniz gerekir.
Günlüğe kaydetme ve izleme veri toplama, uygulamada meydana gelen denetim olaylarına bir görünüm sağlar.

Application Insights, kullanıcılar veya sistem tarafından oluşturulabilen günlükleri toplayan bir hizmettir.

Uygulama Öngörüleri örneği oluşturmak için:

1. Azure portalındaki arama kutusunu kullanarak **Uygulama Öngörüleri'ni** arayın.
2. **Application Insights**’ı seçin. Bir örnek oluşturmak için burada gösterilen ayrıntıları sağlayın.

   ![Uygulama Öngörüleri örneği oluşturma](./media/secure-web-app/app-insights-data.png)

Dağıtım tamamlandıktan sonra bir Uygulama Öngörüleri örneğine sahip olursunuz.

Applications Insights örneğini oluşturduktan sonra, uygulamayı buluta günlük göndermenize olanak tanıyan enstrümantasyon anahtarından haberdar etmeniz gerekir. Bunu, Uygulama Öngörüleri anahtarını alarak ve Azure'un Uygulama Öngörüleri için sağladığı uygulama kitaplıklarında kullanarak yaparsınız. En iyi yöntem, anahtarları ve sırları güvende tutmak için Azure Key Vault'ta depolamaktır.

Temel örnek uygulama için, Applications Insights örneğini oluşturduktan sonra, uygulamayı buluta günlük göndermenize olanak tanıyan enstrümantasyon anahtarından haberdar etmeniz gerekir.
Key Vault'ta `APPINSIGHTSKEY` bir sır ayarlayın ve değerini enstrümantasyon anahtarı olarak ayarlayın. Bunu yapmak, uygulamanın Uygulama Öngörüleri'ne günlükler ve ölçümler göndermesine olanak tanır.

#### <a name="implement-multi-factor-authentication-for-azure-active-directory"></a>Azure Etkin Dizini için Çok Faktörlü Kimlik Doğrulama'yı uygulama

Yöneticilerin portaldaki abonelik hesaplarının korunduğundan emin olması gerekir. Abonelik, oluşturduğunuz kaynakları yönettiği için saldırılara karşı savunmasızdır. Aboneliği korumak için, aboneliğin **Azure Etkin Dizin** sekmesinde Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirin.

Azure AD, belirli bir ölçüte uyan bir kullanıcıya veya kullanıcı gruplarına uygulanan ilkelere göre çalışır.
Azure, yöneticilerin portalda oturum açmak için iki faktörlü kimlik doğrulamasına ihtiyaç duyduğunu belirten bir varsayılan ilke oluşturur.
Bu ilkeyi etkinleştirdikten sonra, oturum açmanız ve Azure portalında tekrar oturum açmanız istenebilir.

Yönetici oturum açmaları için MFA'yı etkinleştirmek için:

1. Azure portalındaki **Azure Etkin Dizin** sekmesine gidin
2. Güvenlik kategorisi altında koşullu erişimi seçin. Şu ekranı görürsünüz:

   ![Koşullu Erişim - İlkeler](./media/secure-web-app/ad-mfa-conditional-add.png)

Yeni bir ilke oluşturamıyorsanız:

1. **MFA** sekmesine gidin.
2. Ücretsiz deneme sürümüne abone olmak için Azure AD Premium **Free deneme sürümünü** seçin.

   ![Azure AD Premium ücretsiz deneme sürümü](./media/secure-web-app/ad-trial-premium.png)

Koşullu erişim ekranına geri dönün.

1. Yeni ilke sekmesini seçin.
2. İlke adını girin.
3. MFA'yı etkinleştirmek istediğiniz kullanıcıları veya grupları seçin.
4. **Access denetimleri** **altında, Hibe** sekmesini seçin ve ardından çok **faktörlü kimlik doğrulamasını (ve** isterseniz diğer ayarları) talep et'i seçin.

   ![MFA gerektirme](./media/secure-web-app/ad-mfa-conditional-add.png)

Ekranın üst kısmındaki onay kutusunu seçerek ilkeyi etkinleştirebilir veya **Koşullu Erişim** sekmesinde bunu yapabilirsiniz. İlke etkinleştirildiğinde, kullanıcıların portalda oturum açabilmesi için MFA'ya ihtiyaç duyar.

Tüm Azure yöneticileri için MFA gerektiren bir temel ilke vardır. Hemen portalda etkinleştirebilirsiniz. Bu ilkeyi etkinleştirmek geçerli oturumu geçersiz kakabilir ve sizi yeniden oturum açmaya zorlayabilir.

Temel ilke etkin değilse:

1. **Yöneticiler için MFA'yı**gerektir'i seçin.
2. **Hemen Kullan ilkesini**seçin.

   ![Hemen Kullan ilkesini seçin](./media/secure-web-app/ad-mfa-conditional-enable.png)

#### <a name="use-azure-sentinel-to-monitor-apps-and-resources"></a>Uygulamaları ve kaynakları izlemek için Azure Sentinel'i kullanın

Bir uygulama büyüdükçe, kaynaklardan alınan tüm güvenlik sinyallerini ve ölçümlerini toplamak ve bunları eylem odaklı bir şekilde kullanışlı hale getirmek zorlaşır.

Azure Sentinel, veri toplamak, olası tehdit türlerini algılamak ve güvenlik olaylarında görünürlük sağlamak üzere tasarlanmıştır.
Azure Sentinel, el ile müdahale beklerken, uyarıları ve olay yönetimi işlemlerini başlatmak için önceden yazılmış oyun kitaplarına güvenebilir.

Örnek uygulama, Azure Sentinel'in izleyebileceği çeşitli kaynaklardan oluşur.
Azure Sentinel'i ayarlamak için öncelikle çeşitli kaynaklardan toplanan tüm verileri depolayan bir Log Analytics çalışma alanı oluşturmanız gerekir.

Bu çalışma alanını oluşturmak için:

1. Azure portalındaki arama **kutusunda, Log Analytics'i**arayın. **Günlük Analizi çalışma alanlarını**seçin.

   ![Günlük Analizi çalışma alanlarını ara](./media/secure-web-app/sentinel-log-analytics.png)

    *Günlük Analizi çalışma alanlarını ara*

2. Sonraki sayfada **Ekle'yi** seçin ve ardından çalışma alanı için bir ad, kaynak grubu ve konum sağlayın.
   ![Log Analytics çalışma alanı oluşturma](./media/secure-web-app/sentinel-log-analytics-create.png)

   *Log Analytics çalışma alanı oluşturma*

3. **Azure Sentinel'i**aramak için arama kutusunu kullanın.

   ![Azure Sentinel'i arama](./media/secure-web-app/sentinel-add.png)

    *Azure Sentinel'i arama*

4. **Ekle'yi** seçin ve ardından daha önce oluşturduğunuz Log Analytics çalışma alanını seçin.

   ![Günlük Analizi çalışma alanı ekleme](./media/secure-web-app/sentinel-workspace-add.png)

    *Günlük Analizi çalışma alanı ekleme*

5. Azure **Sentinel - Veri bağlayıcıları** sayfasında, **Yapılandırma** **altında, Veri bağlayıcılarını**seçin. Azure Sentinel'de analiz için Log Analytics depolama örneğine bağlayabileceğiniz bir dizi Azure hizmeti görürsünüz.

   ![Günlük Analytics veri bağlayıcıları](./media/secure-web-app/sentinel-connectors.png)

    *Azure Sentinel'e veri bağlayıcısı ekleme*

   Örneğin, uygulama ağ geçidini bağlamak için aşağıdaki adımları izleyin:

   1. Azure Uygulama Ağ Geçidi örnek bıçaklarını açın.
   2. **İzleme** bölümünde **Tanılama ayarları**'nı seçin.
   3. **Tanıayar ayarı ekle'yi**seçin.

      ![Uygulama Ağ Geçidi tanılama ekleme](./media/secure-web-app/sentinel-gateway-connector.png)

      *Uygulama Ağ Geçidi tanılama ekleme*

   4. **Tanılama ayarları** sayfasında, oluşturduğunuz Günlük Analizi çalışma alanını seçin ve ardından toplayıp Azure Sentinel'e göndermek istediğiniz tüm ölçümleri seçin. **Kaydet**’i seçin.

        ![Azure Sentinel konektör ayarları](./media/secure-web-app/sentinel-connector-settings.png)

        *Azure Sentinel konektör ayarları*

  Kaynaktan alınan ölçümler, sorgulayıp araştırabileceğiniz Azure Sentinel'dedir.

   Azure Key Vault, genel IP adresi, PostgreSQL için Azure Veritabanı ve hesabınızdaki tanılama günlüklerini destekleyen tüm hizmetler için tanı ayarlarına aynı ölçümleri ekleyin.

Ölçümleri ayarladıktan sonra Azure Sentinel'in çözümleme verileri vardır.

## <a name="evaluate-and-verify"></a>Değerlendirme ve doğrulama

Mimariyi geliştirip dağıttıktan sonra, kodun ve dağıtılan hizmetlerin güvenlik standartlarını karşıladığından emin olmanız gerekir. Yazılımı doğrulamak için atabileceğiniz bazı adımlar şunlardır:

- Statik kod analizi
- Güvenlik açığı taraması
- Uygulama bağımlılıklarında güvenlik açıklarını bulma ve düzeltme

Bunlar güvenli geliştirme de en iyi uygulamalar için temel yapı taşlarıdır.

### <a name="static-code-analysis"></a>Statik kod analizi

Örnek uygulama için, statik analiz araçlarıyla doğrulama, kusurlu kontrol ve veri akışı analizi gibi teknikler kullanarak uygulama kodundaki güvenlik açıklarını bulmayı içerir. Python statik analiz araçları, uygulamanızın güvenli olduğuna daha fazla güven verir.

**Lint uygulama**

Python linting kitaplığı PyFlakes, burada gösterildiği gibi, ölü kodu ve kullanılmayan işlevleri uygulamalardan kaldırmanıza yardımcı olur:

![PyFlakes](./media/secure-web-app/pyflakes.png)

Linting, çalışma süresi sırasında kodunuzu daha temiz ve daha az hataya yatkın hale getirebilecek ipuçları ve olası değişiklikler sağlar.

**PyLint**

PyLint bu proje için en fazla değeri sağladı. Sunucuda çalışan kodun güvenli olduğundan emin olmak için kod standardı denetimleri, hata denetimi ve yeniden düzenleme ipuçları gerçekleştirir. Kodunuzu güncelleştirmek için PyLint'i kullanarak hataları ortadan kaldırabilir ve aşağıdaki resimlerde belirtildiği gibi PyLint derecelendirmesini geliştirebilirsiniz.

![PyLint'ten önce](./media/secure-web-app/before-pylint.png)

*PyLint'ten önce*

Linting araçları tarafından bulunan bazı kod hatalarını düzelttindikten sonra, kodun hatalara yatkın olmadığına daha fazla güvenirsiniz. Hataları ngidermek, kod üretim ortamlarına dağıtıldığında oluşabilecek güvenlik risklerini önemli ölçüde azaltır.

![Pylint'ten sonra](./media/secure-web-app/after-pylint.png)

*PyLint'ten sonra*

### <a name="vulnerability-scanning"></a>Güvenlik açığı taraması

[OWASP'nin ZAP](https://www.zaproxy.org/) aracı, güvenlik açıkları için örnek uygulamayı denetlemek için kullanabileceğiniz açık kaynaklı bir web uygulaması güvenlik açığı tarayıcısidir. Aracı örnek uygulamada çalıştırmak bazı olası hataları ve saldırı vektörlerini ortaya çıkarır.

![ZAP aracı](./media/secure-web-app/zap-tool.png)

*ZAP aracı*

### <a name="find-and-fix-vulnerabilities-in-app-dependencies"></a>Uygulama bağımlılıklarında güvenlik açıklarını bulma ve düzeltme

Uygulama bağımlılıklarını bulmak ve düzeltmek için [OWASP'nin Bağımlılık Denetimi'ni](https://owasp.org/www-project-dependency-check/)kullanabilirsiniz.

Güvenlik, bağımlılıkları kontrol eden benzer bir uygulamadır. [GitHub'da](https://github.com/pyupio/safety)bulabilirsiniz. Tanınmış güvenlik açığı veritabanlarında bulunan güvenlik açıkları için güvenlik taramaları.

![Güvenlik](./media/secure-web-app/pysafety.png)

*Güvenlik*

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler, güvenli uygulamalar tasarlamanıza, geliştirmenize ve dağıtmanıza yardımcı olabilir.

- [Tasarım](secure-design.md)
- [Geliştirme](secure-develop.md)
- [Dağıt](secure-deploy.md)
