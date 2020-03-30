---
title: Çok kiracılı SaaS öğretici
description: Bağımsız uygulama deseni kullanarak yeni kiracıların sağlanması ve kataloglama
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
ms.date: 09/24/2018
ms.openlocfilehash: 02682a18f14e7ecbf5b42783ab84a1b55a4bb77b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74133137"
---
# <a name="provision-and-catalog-new-tenants-using-the--application-per-tenant-saas-pattern"></a>Kiracı SaaS deseni başına uygulamayı kullanarak yeni kiracıların sağlanması ve katalog

Bu makalede, kiracı SaaS deseni başına bağımsız uygulama kullanarak yeni kiracıların sağlanması ve kataloglama kapsar.
Bu makalede iki ana bölüm vardır:
* Yeni kiracıların sağlanması ve kataloglanması kavramsal tartışması
* Sağlama ve kataloglama yı başaran örnek PowerShell kodunu vurgulayan bir öğretici
    * Öğretici, kiracı deseni başına bağımsız uygulamaya uyarlanmış Wingtip Tickets örnek SaaS uygulamasını kullanır.

## <a name="standalone-application-per-tenant-pattern"></a>Kiracı deseni başına bağımsız uygulama

Kiracı deseni başına bağımsız uygulama, çok kiracılı SaaS uygulamaları için çeşitli desenlerden biridir.  Bu desende, her kiracı için bağımsız bir uygulama sağlanır. Uygulama, uygulama düzeyi bileşenleri ve bir SQL veritabanından oluşur.  Her kiracı uygulaması satıcının aboneliğinde dağıtılabilir.  Alternatif olarak, Azure, bir uygulamanın kiracının aboneliğinde dağıtılabildiği ve kiracı adına satıcı tarafından yönetilebildiği yönetilen yönetilen bir [uygulama programı](https://docs.microsoft.com/azure/managed-applications/overview) sunar.

   ![kiracı başına uygulama deseni](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern.png)

Bir kiracı için bir uygulama dağıtılırken, uygulama ve veritabanı kiracı için oluşturulan yeni bir kaynak grubunda karşılanır.  Ayrı kaynak gruplarının kullanılması, her kiracının uygulama kaynaklarını yalıtır ve bağımsız olarak yönetilmesini sağlar. Her kaynak grubu içinde, her uygulama örneği doğrudan ilgili veritabanına erişmek için yapılandırılır.  Bu bağlantı modeli, uygulama ve veritabanı arasındaki bağlantıları aracılık etmek için katalog kullanan diğer desenlerle tezat oluşturuyor.  Kaynak paylaşımı olmadığından, her kiracı veritabanının en yüksek yükünü işlemek için yeterli kaynaklarla sağlanması gerekir. Bu desen, kiracı yalıtımı ve kaynak maliyetleri üzerinde daha az vurgu güçlü bir vurgu olduğu daha az kiracı ile SaaS uygulamaları için kullanılma eğilimindedir.

## <a name="using-a-tenant-catalog-with-the-application-per-tenant-pattern"></a>Kiracı deseni başına uygulama ile kiracı kataloğu kullanma

Her kiracının uygulaması ve veritabanı tamamen izole edilmiş olsa da, kiracılar arasında çeşitli yönetim ve analiz senaryoları çalışabilir.  Örneğin, uygulamanın yeni bir sürümü için şema değişikliği uygulamak, her kiracı veritabanının şemasında değişiklik gerektirir. Raporlama ve analiz senaryoları, dağıtıldıkları yere bakılmaksızın tüm kiracı veritabanlarına erişim de gerektirebilir.

   ![kiracı başına uygulama deseni](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern-with-catalog.png)

Kiracı kataloğu, bir tanımlayıcının bir sunucu ve veritabanı adına çözülmesini sağlayan bir kiracı tanımlayıcısı ile kiracı veritabanı arasında bir eşleme tutar.  Wingtip SaaS uygulamasında, kiracı tanımlayıcısı, diğer şemalar kullanılabilse de, kiracı adının bir karma olarak hesaplanır.  Bağımsız uygulamaların bağlantıları yönetmek için catalog'a ihtiyacı olmasa da, katalog diğer eylemleri bir kiracı veritabanları kümesine genişletmek için kullanılabilir. Örneğin, Elastik Sorgu, çapraz kiracı raporlaması için sorguların dağıtıldığı veritabanları kümesini belirlemek için kataloğu kullanabilir.

## <a name="elastic-database-client-library"></a>Elastik Veritabanı İstemci Kitaplığı

Wingtip örnek uygulamasında, katalog [Elastik Veritabanı İstemci Kitaplığı](sql-database-elastic-database-client-library.md) 'nın (EDCL) parça yönetimi özellikleri tarafından uygulanır.  Kitaplık, bir uygulamanın veritabanında depolanan bir parça eşlemi oluşturmasına, yönetmesini ve kullanmasını sağlar. Wingtip Biletleri örneğinde, katalog *kiracı katalog* veritabanında saklanır.  Parça, kiracının verilerinin depolandığı parçanın (veritabanı) kiracı anahtarını eşler.  EDCL *işlevleri, kiracı kataloğu* veritabanındaki tablolarda depolanan *genel bir parça eşlemi* ve her parçada depolanan *yerel bir parça eşlemi* yönetir.

SABIT haritadaki girişleri oluşturmak ve yönetmek için EDCL işlevleri uygulamalardan veya PowerShell komut dosyalarından çağrılabilir. Diğer EDCL işlevleri, parça kümesini almak veya verilen kiracı anahtarı için doğru veritabanına bağlanmak için kullanılabilir.

> [!IMPORTANT]
> Katalog veritabanındaki verileri veya kiracı veritabanlarındaki yerel parça eşlemi doğrudan değiştirin. Veri bozulması riski yüksek nedeniyle doğrudan güncelleştirmeler desteklenmez. Bunun yerine, yalnızca EDCL API'leri kullanarak eşleme verilerini düzenleme.

## <a name="tenant-provisioning"></a>Kiracı sağlama

Her kiracı, kaynakların içinde sağlanabilir hale gelmeden önce oluşturulması gereken yeni bir Azure kaynak grubu gerektirir. Kaynak grubu var olduktan sonra, uygulama bileşenlerini ve veritabanını dağıtmak ve veritabanı bağlantısını yapılandırmak için bir Azure Kaynak Yönetimi şablonu kullanılabilir. Veritabanı şemasını başlatmayı sağlamak için şablon bir bacpac dosyası içe aktarabilir.  Alternatif olarak, veritabanı bir 'şablon' veritabanının kopyası olarak oluşturulabilir.  Veritabanı daha sonra ilk mekan verileri yle daha da güncellenir ve katalogda kaydedilir.

## <a name="tutorial"></a>Öğretici

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

* Katalog sağlama
* Katalogda daha önce dağıttığınız örnek kiracı veritabanlarını kaydetme
* Ek bir kiracı sağlama ve katalogda kaydedin

Azure Kaynak Yöneticisi şablonu, uygulamayı dağıtmak ve yapılandırmak, kiracı veritabanını oluşturmak ve sonra bir bacpac dosyasını almak için kullanılır. Alma isteği, eyleme geçirilmeden önce birkaç dakika için sıraya alınabilir.

Bu eğitimin sonunda, katalogda kayıtlı her veritabanı ile bağımsız kiracı uygulamaları kümesi var.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* Üç örnek kiracı uygulaması dağıtıldı. Bu uygulamaları beş dakikadan kısa bir süre içinde dağıtmak için [Bkz.](saas-standaloneapp-get-started-deploy.md)

## <a name="provision-the-catalog"></a>Kataloğu sağlama

Bu görevde, tüm kiracı veritabanlarını kaydetmek için kullanılan kataloğu nasıl sağlarsınız? Yapacaklarınız:

* Azure kaynak yönetimi şablonu kullanarak **katalog veritabanını sağlama.** Veritabanı bir bacpac dosyası içe aktarılarak başolarak başlatılır.
* Daha önce dağıttığınız **örnek kiracı uygulamalarını kaydedin.**  Her kiracı, kiracı adının bir karma sından oluşturulmuş bir anahtar kullanılarak kaydedilir.  Kiracı adı da katalogdaki bir uzantı tablosunda depolanır.

1. PowerShell ISE'de *,...\Öğrenme Modüllerini açın\UserConfig.psm* ve ** \<kullanıcı\> ** değerini üç örnek uygulamayı dağıtırken kullandığınız değere güncelleyin.  **Dosyayı kaydet.**
1. PowerShell ISE, açık *...\Öğrenme Modülleri\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* ve set **$Scenario = 1**. Kiracı kataloğunu dağıtın ve önceden tanımlanmış kiracıları kaydedin.

1. İmlecinizi satırın `& $PSScriptRoot\New-Catalog.ps1`herhangi bir yerine koyarak bir kesme noktası ekleyin ve ardından **F9**tuşuna basın.

    ![izleme için bir kesme noktası ayarlama](media/saas-standaloneapp-provision-and-catalog/breakpoint.png)

1. **F5**tuşuna basarak komut dosyasını çalıştırın.
1.  Komut dosyası yürütme kesme noktasında durduktan sonra, Yeni Katalog.ps1 komut dosyasına adım atmak için **F11** tuşuna basın.
1.  Hata Ayıklama menüsü seçenekleri olan F10 ve F11'i kullanarak komut dosyasının yürütülmesini izleyip çağrılan işlevlerin üzerine veya içine adım atın.
    *   PowerShell komut dosyalarının hata ayıklanması hakkında daha fazla bilgi için [PowerShell komut dosyalarıyla çalışma ve hata ayıklama hakkındaki ipuçlarına](https://docs.microsoft.com/powershell/scripting/components/ise/how-to-debug-scripts-in-windows-powershell-ise)bakın.

Komut dosyası tamamlandıktan sonra, katalog var olacak ve tüm örnek kiracılar kaydedilecek.

Şimdi oluşturduğunuz kaynaklara bakın.

1. Azure [portalını](https://portal.azure.com/) açın ve kaynak gruplarına göz atın.  **Wingtip-sa-catalog-\<kullanıcı\> ** kaynak grubunu açın ve katalog sunucusuna ve veritabanına dikkat edin.
1. Portaldaki veritabanını açın ve sol menüden *Veri gezgini'ni* seçin.  Giriş komutunu tıklatın ve sonra Şifre = **P\@ssword1**girin .


1. *Kiracı kataloğu* veritabanının şemasını keşfedin.
   * `__ShardManagement` Şemadaki nesnelerin tümü Elastik Veritabanı İstemci Kitaplığı tarafından sağlanır.
   * Tablo `Tenants` ve `TenantsExtended` görünüm, ek değer sağlamak için kataloğu nasıl genişletebileceğinizi gösteren örneklere eklenen uzantılardır.
1. Sorguyu çalıştırın. `SELECT * FROM dbo.TenantsExtended`

   ![veri gezgini](media/saas-standaloneapp-provision-and-catalog/data-explorer-tenantsextended.png)

    Veri Gezgini'ni kullanmaya alternatif olarak SQL Server Management Studio'dan veritabanına bağlanabilirsiniz. Bunu yapmak için, sunucu wingtip bağlan-


    Verileri doğrudan katalogda yönetmemeniz gerektiğini unutmayın - her zaman parça yönetimi API'lerini kullanın.

## <a name="provision-a-new-tenant-application"></a>Yeni bir kiracı uygulaması sağlama

Bu görevde, tek bir kiracı uygulamasının nasıl sağverilebildiğini öğrenirsiniz. Yapacaklarınız:

* Kiracı için **yeni bir kaynak grubu oluşturun.**
* Azure kaynak yönetimi şablonu kullanarak **uygulamayı ve veritabanını** yeni kaynak grubuna sağlama.  Bu eylem, bir bacpac dosyası alarak ortak şema ve başvuru verileri ile veritabanı nı başlatmayı içerir.
* **Temel kiracı bilgileriyle veritabanını başlatma.** Bu eylem, etkinlik web sitesinde arka plan olarak kullanılan fotoğrafı belirleyen mekan türünü belirtmeyi içerir.
* **Veritabanını katalog veritabanına kaydedin.**

1. PowerShell ISE, açık *...\Öğrenme Modülleri\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* ve set **$Scenario = 2**. Kiracı kataloğunu dağıtın ve önceden tanımlanmış kiracıları kaydedin

1. İmlecinizi 49 `& $PSScriptRoot\New-TenantApp.ps1`satırına koyarak komut dosyasına bir kesme noktası ekleyin ve ardından **F9**tuşuna basın.
1. **F5**tuşuna basarak komut dosyasını çalıştırın.
1.  Komut dosyası yürütme kesme noktasında durduktan sonra, Yeni Katalog.ps1 komut dosyasına adım atmak için **F11** tuşuna basın.
1.  Hata Ayıklama menüsü seçenekleri olan F10 ve F11'i kullanarak komut dosyasının yürütülmesini izleyip çağrılan işlevlerin üzerine veya içine adım atın.

Kiracı temin edildikten sonra, yeni kiracının etkinlik web sitesi açılır.

   ![kırmızı akçaağaç yarışı](media/saas-standaloneapp-provision-and-catalog/redmapleracing.png)

Daha sonra Azure portalında oluşturulan yeni kaynakları inceleyebilirsiniz.

   ![kırmızı akçaağaç yarış kaynakları](media/saas-standaloneapp-provision-and-catalog/redmapleracing-resources.png)


## <a name="to-stop-billing-delete-resource-groups"></a>Faturalandırmayı durdurmak için kaynak gruplarını silme

Örneği keşfetmeyi bitirdikten sonra, ilişkili faturalandırmayı durdurmak için oluşturduğunuz tüm kaynak gruplarını silin.

## <a name="additional-resources"></a>Ek kaynaklar

- Çok kiracılı SaaS veritabanı uygulamaları hakkında daha fazla bilgi edinmek [için, çok kiracılı SaaS uygulamaları için tasarım desenleri](saas-tenancy-app-design-patterns.md)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> * Wingtip Biletleri SaaS Tek Başına Uygulaması nasıl dağıtılır?
> * Uygulamayı oluşturan sunucular ve veritabanları hakkında.
> * İlgili faturalandırmayı durdurmak için örnek kaynaklar nasıl silinir?

Kataloğun [Wingtip Tickets SaaS uygulamasının](saas-dbpertenant-wingtip-app-overview.md)kiracı başına veritabanı sürümünü kullanarak çeşitli kiracılar arası senaryoları desteklemek için nasıl kullanıldığını keşfedebilirsiniz.
