---
title: Sorun Giderme SSIS Tümleştirme Runtime yönetimi
description: Bu makale, SSIS Tümleştirme Çalışma Zamanı (SSIS IR) yönetim sorunları için sorun giderme kılavuzu sağlar
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: chinadragon0515
ms.author: dashe
ms.reviewer: sawinark
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 07/08/2019
ms.openlocfilehash: 0324044d93f12f6ac6ec96ff1a31be8ee02ada41
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414708"
---
# <a name="troubleshoot-ssis-integration-runtime-management-in-azure-data-factory"></a>Azure Veri Fabrikası'nda SSIS Tümleştirme Çalışma Zamanı yönetimini sorun giderme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, SSIS IR olarak da bilinen Azure-SQL Server Integration Services (SSIS) Tümleştirme Çalışma Süresi'ndeki (IR) yönetim sorunları için sorun giderme kılavuzu sağlanmaktadır.

## <a name="overview"></a>Genel Bakış

SSIS IR'yi sağlarken veya yok ederken herhangi bir sorunla karşınıza çıkarsa, Microsoft Azure Veri Fabrikası portalında bir hata iletisi veya PowerShell cmdlet'ten döndürülen bir hata görürsünüz. Hata her zaman ayrıntılı bir hata iletisi ile bir hata kodu biçiminde görünür.

Hata kodu InternalServerError ise, hizmetgeçici sorunları vardır ve daha sonra işlemi yeniden denemelisiniz. Yeniden deneme nin yardımı yoksa, Azure Veri Fabrikası destek ekibine başvurun.

Aksi takdirde, üç büyük dış bağımlılık hatalara neden olabilir: Bir Azure SQL Veritabanı sunucusu veya yönetilen örnek, özel bir kurulum komut dosyası ve sanal ağ yapılandırması.

## <a name="azure-sql-database-server-or-managed-instance-issues"></a>Azure SQL Veritabanı sunucusu veya yönetilen örnek sorunları

SSIS IR'yi bir SSIS katalog veritabanıyla sağlıyorsanız Azure SQL Veritabanı sunucusu veya yönetilen örneği gereklidir. SSIS IR'nin Azure SQL Veritabanı sunucusuna veya yönetilen örneğine erişebilmesi gerekir. Ayrıca Azure SQL Veritabanı sunucusu veya yönetilen örneğinin hesabı bir SSIS katalog veritabanı (SSISDB) oluşturma iznine sahip olmalıdır. Hata varsa, ayrıntılı SQL özel durum iletisiyle hata kodu Data Factory portalında gösterilir. Hata kodlarındaki sorunları gidermek için aşağıdaki listede yer alan bilgileri kullanın.

### <a name="azuresqlconnectionfailure"></a>AzureSqlConnectionFailure

Yeni SSIS IR sağlarken veya IR çalıştırılırken bu hatayı görebilirsiniz. IR sağlama sırasında bu hatayla karşılaşırsanız hata iletisinde aşağıdaki sorunlardan birine işaret eden ayrıntılı bir hata alabilirsiniz:

* Ağ bağlantısı sorunu. SQL Server veya yönetilen örnek konak adının erişilebilir olup olmadığını denetleyin. Ayrıca hiçbir güvenlik duvarı veya ağ güvenlik grubunun (NSG) SSIS IR'nin sunucuya erişimini engellemediğini doğrulayın.
* SQL kimlik doğrulaması sırasında oturum açma başarısız oldu. Sağlanan hesap SQL Server veritabanında oturum açamıyor. Doğru kullanıcı hesabını sağladığınızdan emin olun.
* Microsoft Azure Active Directory (Azure AD) kimlik doğrulaması (yönetilen kimlik) sırasında oturum açma başarısız oldu. Fabrikanızın yönetilen kimliğini bir AAD grubuna ekleyin ve yönetilen kimliğin katalog veritabanı sunucunuza erişim izinleri olduğundan emin olun.
* Bağlantı zaman aşımı. Bu hatanın nedeni her zaman güvenlikle ilgili yapılandırmadır. Şunları yapmanızı öneririz:
  1. Yeni bir VM oluşturun.
  1. IR sanal ağdaysa, VM'ye aynı Microsoft Azure Sanal IR Ağı'na katılın.
  1. SSMS'i yükleyin ve Azure SQL Veritabanı sunucusunu veya yönetilen örnek durumunu denetleyin.

Diğer sorunlar için, ayrıntılı SQL Özel Durum hata iletisinde gösterilen sorunu çözün. Sorun yaşamaya devam ediyorsanız Azure SQL Veritabanı sunucusu veya yönetilen örneğinin destek takımına başvurun.

Hatayı IR çalıştırılırken görüyorsanız, ağ güvenlik grubu veya güvenlik duvarı değişiklikleri SSIS IR çalışan düğümünün Azure SQL Veritabanı sunucusuna veya yönetilen örneğine erişmesini engelliyor olabilir. SSIS IR çalışan düğümünün engelini kaldırarak Azure SQL Veritabanı sunucusuna veya yönetilen örneğine erişebilmesini sağlayın.

### <a name="catalogcapacitylimiterror"></a>CatalogCapacityLimitError

Bu tür bir hata iletisi şu şekilde görünebilir: "Veritabanı 'SSISDB' boyut kotasına ulaştı. Verileri bölme veya silme, dizinleri bırakma veya olası çözümler için belgelere başvurun." 

Olası çözümler şunlardır:
* SSISDB'nizin kota boyutunu artırın.
* SSISDB'nin yapılandırmasını değiştirerek boyutu şöyle azaltın:
   * Bekletme süresini ve proje sürümlerinin sayısını azaltma.
   * Günlüğün bekletme süresini azaltmak.
   * Günlüğün varsayılan düzeyini değiştirme.

### <a name="catalogdbbelongstoanotherir"></a>CatalogDbBelongsToAnotherIR

Bu hata Azure SQL Veritabanı sunucusunun veya yönetilen örneğinin zaten bir SSISDB'si olduğu ve bunun başka bir IR tarafından kullanıldığı anlamına gelir. Farklı bir Azure SQL Veritabanı sunucusu veya yönetilen örneği sağlamalı veya mevcut SSISDB'yi silip yeni IR'yi yeniden başlatmalısınız.

### <a name="catalogdbcreationfailure"></a>CatalogDbCreationFailure

Bu hatanın oluşmasının nedeni aşağıdakilerden biri olabilir:

* SSIS IR için yapılandırılan kullanıcı hesabının veritabanı oluşturma izni yoktur. Kullanıcıya veritabanını oluşturma izni verebilirsiniz.
* Veritabanı oluşturma sırasında yürütme zaman aşımı veya DB işlemi zaman aşımı gibi bir zaman aşımı oluşmuştur. İşlemi daha sonra yeniden denemelisiniz. Yeniden deneme işe yaramazsa Azure SQL Veritabanı sunucusu veya Yönetilen Örneğinin destek takımına başvurun.

Diğer sorunlar için SQL Özel Durumu hata iletisini gözden geçirin ve hata ayrıntılarında belirtilen sorunu çözün. Sorun yaşamaya devam ediyorsanız Azure SQL Veritabanı sunucusu veya yönetilen örneğinin destek takımına başvurun.

### <a name="invalidcatalogdb"></a>InvalidCatalogDb

Bu tür bir hata iletisi şuna benzer: "Geçersiz nesne adı 'catalog.catalog_properties'." Bu durumda, ssisdb adında bir veritabanınız zaten var, ancak SSIS IR tarafından oluşturulmadı veya veritabanı son SSIS IR sağlamadaki hatalardan kaynaklanan geçersiz bir durumdadır. SSISDB adlı mevcut veritabanını bırakabilir veya IR için yeni bir Azure SQL Veritabanı sunucusu veya yönetilen örneği yapılandırabilirsiniz.

## <a name="custom-setup-issues"></a>Özel kurulum sorunları

SSIS IR'nizin sağlanması veya yeniden yapılandırılması sırasında özel kurulum kendi kurulum adımlarınızı eklemeniz için bir arabirim sağlar. Daha fazla bilgi için bkz. [Azure-SSIS Integration Runtime için kurulumu özelleştirme](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Kapsayıcınızın yalnızca gerekli kurulum dosyalarını içerdiğinden emin olun; kapsayıcıdaki tüm dosyalar SSIS IR çalışan düğümüne indirilecektir. Özel kurulum betiğini SSIS IR'de çalıştırmadan önce betik yürütme sorunlarını çözmek için betiği yerel makinede test etmenizi öneririz.

Özel kurulum betiği kapsayıcısı IR çalıştırılırken denetlenir çünkü SSIS IR düzenli aralıklarla güncelleştirilir. Bu güncelleştirme, özel kurulum betiğini indirmek ve yeniden yüklemek için kapsayıcıya erişim gerektirir. İşlemde kapsayıcının erişilebilir ve main.cmd dosyasının mevcut olup olmadığı da denetlenir.

Özel kurulum içeren herhangi bir hata için, CustomSetupScriptBlobContainerInaccessible veya CustomSetupScriptNotFound gibi alt kodu ile bir CustomSetupScriptFailure hata kodu görürsünüz.

### <a name="customsetupscriptblobcontainerinaccessible"></a>CustomSetupScriptBlobContainerInaccessible

Bu hata SSIS IR'nin özel kurulum için Azure blob kapsayıcınıza erişemediği anlamına gelir. Kapsayıcının SAS URI'sinin ulaşılabilir olduğundan ve süresinin dolmadığından emin olun.

Çalışıyorsa IR'yi durdurun, IR'yi yeni özel kurulum kapsayıcısı SAS URI değeriyle yeniden yapılandırın ve sonra yeniden başlatın.

### <a name="customsetupscriptnotfound"></a>CustomSetupScriptNotFound

Bu hata SSIS IR'nin blob kapsayıcınızda özel kurulum betiğini (main.cmd) bulamadığı anlamına gelir. Kapsayıcıda, özel kurulum yüklemesinin giriş noktası olan main.cmd dosyasının mevcut olduğundan emin olun.

### <a name="customsetupscriptexecutionfailure"></a>CustomSetupScriptExecutionFailure

Bu hata özel kurulum betiğinin (main.cmd) yürütülemediği anlamına gelir. Betiği önce yerel makinenizde deneyin veya blob kapsayıcınızda özel kurulum yürütme günlüklerini gözden geçirin.

### <a name="customsetupscripttimeout"></a>CustomSetupScriptTimeout

Bu hata özel kurulum betiğinin zaman aşımına uğradığını gösterir. Betiğinizin sessiz yürütülebildiğinden, etkileşimli giriş gerektirmediğinden ve blob kapsayıcınızda yalnızca gerekli özel kurulum dosyalarının bulunduğundan emin olun. Betiği öncelikle yerel makinede test etmeniz önerilir. Blob kapsayıcınızda özel kurulum yürütme günlüklerini gözden geçirmelisiniz. Özel kurulum için zaman aşımına uğramadan verilen maksimum süre 45 dakikadır ve maksimum süre kapsayıcınızdan tüm dosyaları indirme ve bunları SSIS IR'ye yükleme süresini içerir. Daha uzun süreye ihtiyacınız olursa bir destek bileti oluşturun.

### <a name="customsetupscriptloguploadfailure"></a>CustomSetupScriptLogUploadFailure

Bu hata özel kurulum yürütme günlüklerini blob kapsayıcınıza yükleme girişiminin başarısız olduğu anlamına gelir. Bu sorun SSIS IR'nin blob kapsayıcınıza yazma izinleri olmadığı için ya da depolama veya ağ sorunlarından dolayı oluşabilir. Özel kurulum başarılı olursa bu hata hiçbir SSIS işlevini etkilemez ama günlükler eksik olur. Özel kurulum başka bir hatayla başarısız olursa ve günlük yüklenmezse, günlüğün karşıya yüklenip analizde kullanılabilmesi için önce bu hatayı bildiririz. Aynı zamanda, bu sorun çözüldükten sonra diğer belirli sorunları da bildiririz. Yeniden deneme sonrasında bu sorun çözülmezse Azure Data Factory destek takımına başvurun.

## <a name="virtual-network-configuration"></a>Sanal ağ yapılandırması

SSIS IR'yi Azure Sanal Ağıyla birleştirdiğinizde, SSIS IR kullanıcı aboneliği altındaki sanal ağı kullanır. Daha fazla bilgi için bkz. [Azure-SSIS Integration Runtime'ı sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Sanal Ağ ile ilgili bir sorun olduğunda aşağıdaki hatalardan birini görürsünüz.

### <a name="invalidvnetconfiguration"></a>InvalidVnetConfiguration

Bu hata çeşitli nedenlerle oluşabilir. Sorunu gidermek için [Yasak](#forbidden), [InvalidPropertyValue](#invalidpropertyvalue) ve [MisconfiguredDnsServerOrNsgSettings](#misconfigureddnsserverornsgsettings) bölümlerine bakın.

### <a name="forbidden"></a>Yasak

Bu tür bir hata buna benzeyebilir: "SubnetId cari hesap için etkinleştirilemedi. Microsoft.Batch kaynak sağlayıcısı VNet'in aynı aboneliği altında kayıtlı değildir."

Bu ayrıntılar Azure Batch'in sanal ağınıza erişemediği anlamına gelir. Microsoft.Batch kaynak sağlayıcısını Sanal Ağ ile aynı aboneliğe kaydedin.

### <a name="invalidpropertyvalue"></a>InvalidPropertyValue

Bu tür bir hata aşağıdakilerden birine benzer olabilir: 

- "Belirtilen VNet yok veya Toplu İşlem hizmetinin bu hizmete erişimi yok."
- "Belirtilen alt ağ xxx yok."

Bu hatalar sanal ağın var olmadığı, Azure Batch hizmetinin bu sanal ağa erişemediği veya sağlanan alt ağın var olmadığı anlamına gelir. Sanal ağ ile alt ağın var olduğundan ve Azure Batch'in bunlara erişebildiğinden emin olun.

### <a name="misconfigureddnsserverornsgsettings"></a>MisconfiguredDnsServerOrNsgSettings

Bu tür bir hata iletisi şu şekilde görünebilir: "VNet'te Tümleştirme Runtime'ı sağlayamayan. DNS sunucusu veya NSG ayarları yapılandırılırsa, DNS sunucusuna erişilebilir olduğundan ve NSG'nin düzgün şekilde yapılandırıldığından emin olun."

Bu durumda büyük olasılıkla DNS sunucusunda ve NSG ayarlarında SSIS IR için gereken Azure sunucu adının çözümlenmesini veya bu ada erişilmesini engelleyen özelleştirilmiş bir yapılandırma kullanıyorsunuzdur. Daha fazla bilgi için bkz. [SSIS IR Sanal Ağ yapılandırması](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Sorunlarınız devam ediyorsa Azure Data Factory destek takımına başvurun.

### <a name="vnetresourcegrouplockedduringupgrade"></a>VNetResourceGroupLockedDuringUpgrade

SSIS IR düzenli aralıklarla otomatik olarak güncelleştirilecek. Yükseltme sırasında yeni bir Azure Batch havuzu oluşturulur ve eski Azure Batch havuzu silinir. Ayrıca, eski havuzun Sanal Ağ ile ilgili kaynakları da silinir ve aboneliğinizin altında yeni Sanal Ağ ile ilgili kaynaklar oluşturulur. Bu hata, abonelik veya kaynak grubu düzeyindeki bir silme kilidinden dolayı eski havuzun Sanal Ağ ile ilgili kaynaklarını silmenin başarısız olduğu anlamına gelir. Silme kilidini müşteri denetlediği ve ayarladığı için, bu durumda silme kilidini kaldırması gerekir.

### <a name="vnetresourcegrouplockedduringstart"></a>VNetResourceGroupLockedDuringStart

SSIS IR sağlama başarısız olursa, oluşturulan tüm kaynaklar silinir. Bununla birlikte abonelik veya kaynak grubu düzeyinde bir kaynak silme kilidi varsa Sanal Ağ kaynakları beklendiği gibi silinmez. Bu hatayı düzeltmek için silme kilidini kaldıın ve IR'yi yeniden başlatın.

### <a name="vnetresourcegrouplockedduringstop"></a>VNetResourceGroupLockedDuringStop

SSIS IR'yi durdurduğunuzda Sanal Ağ ile ilgili tüm kaynaklar silinir. Ama abonelik veya kaynak grubu düzeyinde bir kaynak silme kilidi olduğunda silme işlemi başarısız olur. Burada da silme kilidini müşteri denetler ve ayarlar. Bu nedenle silme kilidini kaldırması ve sonra SSIS IR'yi yeniden durdurması gerekir.

### <a name="nodeunavailable"></a>NodeUnavailable

Bu hata IR çalışırken oluşur ve IR'nin artık iyi durumda olmadığı anlamına gelir. Bu hata her zaman DNS sunucusunda veya NSG yapılandırmasında yapılan ve SSIS IR'nin gerekli hizmete bağlanmasını engelleyen bir değişiklikten kaynaklanır. DNS sunucusunun ve NSG'nin yapılandırması müşteri tarafından denetlendiğinden, müşterinin kendi tarafında bağlantıyı engelleyen sorunları düzeltmesi gerekir. Daha fazla bilgi için bkz. [SSIS IR Sanal Ağ yapılandırması](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Sorunlarınız devam ediyorsa Azure Data Factory destek takımına başvurun.

## <a name="static-public-ip-addresses-configuration"></a>Statik genel IP adresleri yapılandırması

Azure-SSIS IR'ye Azure Sanal Ağı'na katıldığınızda, IR'nin belirli IP adreslerine erişimi sınırlayan veri kaynaklarına erişebilmesi için IR için kendi statik genel IP adreslerinizi de getirebilirsiniz. Daha fazla bilgi için bkz. [Azure-SSIS Integration Runtime'ı sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Yukarıdaki sanal ağ sorunlarının yanı sıra, statik genel IP adresleriyle ilgili sorunu da karşılaabilirsiniz. Lütfen yardım için aşağıdaki hataları kontrol edin.

### <a name="invalidpublicipspecified"></a><a name="InvalidPublicIPSpecified"></a>GeçersizPublicIPSpecified

Bu hata, Azure-SSIS IR'yi başlattığınızda çeşitli nedenlerle oluşabilir:

| Hata iletisi | Çözüm|
|:--- |:--- |
| Sağlanan statik genel IP adresi zaten kullanılmış, lütfen Azure-SSIS Tümleştirme Çalışma süreniz için kullanılmayan iki adres sağlayın. | Kullanılmayan iki statik genel IP adresi seçmeli veya belirtilen genel IP adresine yapılan geçerli başvuruları kaldırmalı ve ardından Azure-SSIS IR'yi yeniden başlatmalısınız. |
| Sağlanan statik genel IP adresidn adı yoktur, lütfen azure-SSIS Tümleştirme Çalışma süreniz için bunlardan ikisine DNS adı sağlayın. | Aşağıdaki resimde görüldüğü gibi, Azure portalında genel IP adresinin DNS adını kurabilirsiniz. Belirli adımlar şunlardır: (1) Azure portalını açın ve bu genel IP adresinin kaynak sayfasına gidin; (2) **Yapılandırma** bölümünü seçin ve DNS adını ayarlayın, sonra **Kaydet** düğmesini tıklatın; (3) Azure-SSIS IR'nizi yeniden başlatın. |
| Azure-SSIS Tümleştirme Çalışma süreniz için sağlanan VNet ve statik genel IP adresleri aynı konumda olmalıdır. | Azure Ağı'nın gereksinimlerine göre, statik genel IP adresi ve sanal ağ aynı konumda ve abonelikte olmalıdır. Lütfen iki geçerli statik genel IP adresi sağlayın ve Azure-SSIS IR'yi yeniden başlatın. |
| Sağlanan statik genel IP adresi temel bir adrestir, lütfen Azure-SSIS Tümleştirme Çalışma süreniz için iki standart adres sağlayın. | Yardım için [Genel IP Adresi'nin SK'lerine](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#sku) bakın. |

![Azure-SSIS IR](media/ssis-integration-runtime-management-troubleshoot/setup-publicipdns-name.png)

### <a name="publicipresourcegrouplockedduringstart"></a>PublicIPResourceGroupLockedDuringStart

Azure-SSIS IR sağlama başarısız olursa, oluşturulan tüm kaynaklar silinir. Ancak, abonelik veya kaynak grubunda (statik genel IP adresinizi içeren) bir kaynak silme kilidi varsa, ağ kaynakları beklendiği gibi silinmez. Hatayı gidermek için lütfen silme kilidini kaldırın ve IR'yi yeniden başlatın.

### <a name="publicipresourcegrouplockedduringstop"></a>PublicIPResourceGroupLockedDuringStop

Azure-SSIS IR'yi durdurduğunuzda, ortak IP adresinizi içeren kaynak grubunda oluşturulan tüm ağ kaynakları silinir. Ancak, abonelik veya kaynak grubunda (statik genel IP adresinizi içeren) bir kaynak silme kilidi varsa silme işlemi başarısız olabilir. Lütfen silme kilidini kaldırın ve IR'yi yeniden başlatın.

### <a name="publicipresourcegrouplockedduringupgrade"></a>PublicIPResourceGroupLockedYükseltme Sırasında

Azure-SSIS IR düzenli olarak otomatik olarak güncelleştirilir. Yükseltme sırasında yeni IR düğümleri oluşturulur ve eski düğümler silinir. Ayrıca, eski düğümler için oluşturulan ağ kaynakları (örn. yük dengeleyicisi ve ağ güvenlik grubu) silinir ve aboneliğiniz altında yeni ağ kaynakları oluşturulur. Bu hata, abonelik veya kaynak grubundaki (statik genel IP adresinizi içeren) silme kilidi nedeniyle eski düğümler için ağ kaynaklarını silmenin başarısız olduğu anlamına gelir. Eski düğümleri temizleyebilmemiz ve eski düğümlerin statik genel IP adresini serbest bırakabilmemiz için lütfen silme kilidini kaldırın. Aksi takdirde statik genel IP adresi serbest bırakılamaz ve IR'nizi daha da yükseltemeyiz.

### <a name="publicipnotusableduringupgrade"></a>PublicIPNotUsableYükseltme Sırasında

Kendi statik genel IP adreslerinizi getirmek istediğinizde, iki genel IP adresi sağlanmalıdır. Bunlardan biri hemen IR düğümleri oluşturmak için kullanılacak ve başka bir IR yükseltme sırasında kullanılacaktır. Bu hata, yükseltme sırasında diğer genel IP adresi kullanışsız olduğunda oluşabilir. Olası nedenler için lütfen [GeçersizKamuIPSpecified'e](#InvalidPublicIPSpecified) bakın.