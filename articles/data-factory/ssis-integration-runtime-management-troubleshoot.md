---
title: Azure Data Factory 'de SSIS Integration Runtime yönetimi sorunlarını giderme | Microsoft Docs
description: Bu makale, SSIS Integration Runtime (SSIS IR) yönetim sorunları için sorun giderme kılavuzu sağlar
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/08/2019
author: chinadragon0515
ms.author: dashe
ms.reviewer: sawinark
manager: craigg
ms.openlocfilehash: 17fe8d9ed02156b8fe9aafd7adca946531895883
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253025"
---
# <a name="troubleshoot-ssis-integration-runtime-management-in-azure-data-factory"></a>Azure Data Factory 'de SSIS Integration Runtime yönetimi sorunlarını giderme

Bu belge, SSIS Integration Runtime (SSIS IR) yönetim sorunlarına yönelik sorun giderme kılavuzu sağlar.

## <a name="overview"></a>Genel Bakış

SSIS IR sağlama veya sağlama konusunda herhangi bir sorun varsa ADF portalında bir hata mesajı olacaktır veya PowerShell cmdlet 'inden döndürülür. Hata, ayrıntılı bir hata iletisiyle hata kodu olarak her zaman biçimindedir.

Bu, hata kodu ınternalservererror ise hizmette bazı geçici sorunlar olduğu anlamına gelir. İşlemi daha sonra yeniden deneyebilirsiniz. Yeniden deneme konusunda yardım yoksa Azure Data Factory destek ekibine başvurun.

Hatalara neden olabilecek üç ana dış bağımlılık vardır: Hata kodu ınternalservererror değilse Azure SQL veritabanı sunucusu veya yönetilen örnek, özel kurulum betiği ve sanal ağ yapılandırması.

## <a name="azure-sql-database-server-or-managed-instance-issues"></a>Azure SQL veritabanı sunucusu veya yönetilen örnek sorunları

SSIS-SSSıS Katalog veritabanı ile SSIS IR sağlanması halinde bir Azure SQL veritabanı sunucusu veya yönetilen örnek gereklidir. Azure SQL veritabanı sunucusuna veya yönetilen örneğe SSIS IR tarafından erişilebilmelidir. Azure SQL veritabanı sunucusunun veya yönetilen örneğin hesabının SSIS Katalog veritabanı (SSSıSDB) oluşturma izni olmalıdır. Herhangi bir hata varsa, ADF portalında ayrıntılı SQL özel durum iletisi içeren bir hata kodu gösterilir. Hata kodlarının sorunlarını gidermek için aşağıdaki adımları izleyin.

### <a name="azuresqlconnectionfailure"></a>Azuressqlconnectionfailure

Bu sorunu, yeni bir SSIS IR sağlarken veya IR çalışırken görebilirsiniz.

IR sağlama sırasında hata görüyorsanız ve hata iletisinde ayrıntı SqlException iletisi alırsanız, bu durum aşağıdaki nedenlerden kaynaklanabilir.

* Ağ bağlantısı sorunu. SQL Server veya yönetilen örnek ana bilgisayar adına erişilebilir olup olmadığını denetleyin ve sunucuya erişmek için SSIS IR güvenlik duvarı veya NSG bloğu yoktur.
* Oturum açılamadı ve SQL kimlik doğrulaması kullanılıyor. Girilen hesap SQL Server oturum açamıyor anlamına gelir. Doğru Kullanıcı hesabının sağlandığından emin olun.
* Oturum açma başarısız oldu ve AAD kimlik doğrulaması (yönetilen kimlik) kullanılıyor. Fabrikanızın yönetilen kimliğini bir AAD grubuna ekleyin ve yönetilen kimliğin Katalog veritabanı sunucunuza erişim izinleri olduğundan emin olun.
* Bağlantı zaman aşımı, her zaman güvenlikle ilgili yapılandırma nedeniyle oluşur. Yeni bir sanal makine oluşturmanız önerilir, IR VNet 'te ise VM 'nin aynı IR VNet 'e katılmasını, ardından SSMS 'yi yüklemesini ve Azure SQL veritabanı sunucusunu ya da yönetilen örnek durumunu denetlemesini öneririz.

Diğer sorunlar için, ayrıntılı SQL özel durum hata iletisine bakın ve hata iletisinde gösterilen sorunu giderin. Hala sorun yaşıyorsanız, Azure SQL veritabanı sunucusu veya yönetilen örnek desteği ekibine başvurun.

IR çalışıyor sırasında hata görürseniz, SSIS IR çalışan düğümünün Azure SQL veritabanı sunucusuna veya yönetilen örneğe artık erişemediğini belirten bazı ağ güvenlik grupları veya güvenlik duvarı değişiklikleri olabilir. Azure SQL veritabanı sunucusuna veya yönetilen örneğe erişmek için SSIS IR çalışan düğümünün engellemesini kaldırın.

### <a name="catalogcapacitylimiterror"></a>CatalogCapacityLimitError

Hata iletisi "' SSSıSDB ' veritabanı boyut kotasına ulaştı. Verileri bölümleyin veya silin, dizinleri bırakın veya olası çözümler için belgelere başvurun. " Olası çözümler şunlardır:
* SSSıSDB 'nizin boyut kotasını artırın.
* SSSıSDB 'nin yapılandırmasını değiştirerek şu şekilde boyutu azaltın:
   * Saklama süresini ve proje sürümü sayısını azaltma.
   * Günlük tutma dönemi azaltıyor.
   * Günlüğün varsayılan düzeyini değiştirme ve bu şekilde devam eden.

### <a name="catalogdbbelongstoanotherir"></a>CatalogDbBelongsToAnotherIR

Bu hata, Azure SQL veritabanı sunucusunda veya yönetilen örnekte zaten başka bir IR tarafından oluşturulan ve kullanılan bir SSSıSDB 'nin bulunduğu anlamına gelir. Farklı bir Azure SQL veritabanı sunucusu veya yönetilen örnek sağlamanız ya da var olan SSSıSDB 'yi silmeniz ve yeni IR 'yi yeniden başlatmanız gerekir.

### <a name="catalogdbcreationfailure"></a>CatalogDbCreationFailure

Bu hatanın nedeni aşağıdakilerden biri olabilir.

* SSIS IR için yapılandırılmış kullanıcı hesabının veritabanını oluşturma izni yok. Kullanıcıya veritabanını oluşturma izni verebilirsiniz.
* Yürütme zaman aşımı, DB işlem zaman aşımı vb. gibi veritabanı zaman aşımı oluşturun. Sorunu çözülemediğini görmek için daha sonra yeniden deneyebilirsiniz. Yeniden deneme işe yaramazsa Azure SQL veritabanı sunucusu veya yönetilen örnek desteği ekibine başvurun.

Diğer sorunlar için SQL özel durum hata iletisini kontrol edin ve hata iletisinde belirtilen sorunu giderin. Hala sorun yaşıyorsanız, Azure SQL veritabanı sunucusu veya yönetilen örnek desteği ekibine başvurun.

### <a name="invalidcatalogdb"></a>Invalidcatalogdb

Hata iletisi, "geçersiz nesne adı ' Catalog. catalog_properties '." gibidir. Bu, zaten SSSıSDB adlı bir veritabanınız var, ancak SSIS IR tarafından oluşturulmamış ya da veritabanı, son SSIS IR sağlama 'daki hatalardan kaynaklanan geçersiz bir durumda. Var olan veritabanını SSSıSDB adıyla bırakabilir veya IR için yeni bir Azure SQL veritabanı sunucusu veya yönetilen örnek yapılandırabilirsiniz.

## <a name="custom-setup"></a>Özel Kurulum

Özel Kurulum, SSIS IR 'nizin sağlanması veya yeniden yapılandırılması sırasında kendi kurulum adımlarınızı eklemek için bir arabirim sağlar. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirme çalışma zamanı için kurulumu özelleştirme](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Kapsayıcıdaki tüm dosyalar SSIS IR çalışan düğümüne indirileceği için kapsayıcının yalnızca gerekli özel kurulum dosyalarını içerdiğinden emin olun. SSIS IR 'de betiği çalıştırmadan önce betik yürütme sorunlarını gidermek için özel kurulum betiğini yerel bir makinede sınamanız önerilir.

Özel kurulum betiği kapsayıcısı, her ne kadar çalışan IR sırasında, özel kurulum betiğini indirmek ve yeniden yüklemek için kapsayıcıya yeniden erişmesi gereken bir şekilde düzenli olarak güncelleştirilir. Bu denetim, kapsayıcının erişilebilir olup olmadığını ve Main. cmd dosyasının var olup olmadığını içerir.

Özel kurulumda herhangi bir hata, CustomSetupScriptFailure kodunda bir hata görürsünüz, bir alt hata kodu olan hata iletisini kontrol edin.  Alt hata kodlarının sorunlarını gidermek için aşağıdaki adımları izleyin.  

### <a name="customsetupscriptblobcontainerinaccessible"></a>Customsetupscriptblobcontainererişilemeyen

Bu, SSIS IR 'nin özel kurulum için Azure Blob kapsayıcınıza erişemeyeceği anlamına gelir. Kapsayıcının SAS URI 'sinin erişilebilir olduğunu ve süre dolmadığından emin olun.

IR çalışır durumdaysa önce IR 'yi durdurun, IR 'yi yeni özel kurulum kapsayıcısı SAS URI 'SI ile yeniden yapılandırın ve ardından IR 'yi yeniden başlatın.

### <a name="customsetupscriptnotfound"></a>CustomSetupScriptNotFound

Bu, SSIS IR 'nin blob kapsayıcınızda özel kurulum betiği (Main. cmd) bulamadığı anlamına gelir. Özel Kurulum yüklemesinin giriş noktası olan kapsayıcıda Main. cmd ' nin bulunduğundan emin olun.

### <a name="customsetupscriptexecutionfailure"></a>CustomSetupScriptExecutionFailure

Özel kurulum komut dosyasının (Main. cmd) yürütülmesi başarısız oldu, önce betiği yerel makinenizde deneyebilir veya blob kabınızda özel kurulum yürütme günlüklerini kontrol edebilirsiniz.

### <a name="customsetupscripttimeout"></a>CustomSetupScriptTimeout

Özel kurulum betiği zaman aşımı yürütme anlamına gelir. Blob kabınızda yalnızca gerekli özel kurulum dosyalarını içerdiğinden emin olun. Blob kabınızda özel kurulum yürütme günlüklerini de denetleyebilirsiniz. Özel kurulum için maksimum süre, zaman aşımına uğramadan önce 45 dakika veya en fazla süre, kapsayıcıınızdan tüm dosyaları indirme ve bunları SSIS IR 'ye yükleme süresini içerir. Daha uzun bir süre gerekliyse, bir destek bileti yükseltin.

### <a name="customsetupscriptloguploadfailure"></a>CustomSetupScriptLogUploadFailure

Özel Kurulum yürütme günlüklerinin blob kapsayıcısına yüklenmesi başarısız oldu, çünkü SSIS IR 'nin blob kapsayıcınıza veya bazı depolama ya da ağ sorunlarına yazma izni yoktur. Özel Kurulum başarılı olursa, bu hata SSIS işlevini etkilemez, ancak Günlükler eksiktir. Özel Kurulum başka bir hatayla başarısız olduysa ve günlüğü karşıya yükleyemedik, bu hatayı ilk olarak raporlarız ve bu sorun çözümlendikten sonra, daha fazla belirtilen sorunu bildirecağız. Bu sorun yeniden denendikten sonra çözülemezse, Azure Data Factory destek ekibine başvurun.

## <a name="virtual-network-configuration"></a>Sanal ağ yapılandırması

SSIS IR 'yi bir sanal ağa (VNet) eklerken, kullanıcı aboneliği altında VNet 'i kullanır. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

VNet ile ilgili sorun olduğunda, aşağıdaki gibi bir hata görürsünüz

### <a name="invalidvnetconfiguration"></a>Invalidvnetconfiguration

Bunun nedeni, değişken nedenlerinden kaynaklanabilir. Alt hata kodlarının sorunlarını gidermek için aşağıdaki adımları izleyin.

### <a name="forbidden"></a>Yasak

Hata iletisi, "SubnetID geçerli hesap için etkinleştirilmemiş. Microsoft. Batch kaynak sağlayıcısı, VNet 'in aynı aboneliğine kayıtlı değil. "

Azure Batch sanal ağınıza erişemeyeceği anlamına gelir. Microsoft. Batch kaynak sağlayıcısını aynı VNet aboneliğine kaydedin.

### <a name="invalidpropertyvalue"></a>InvalidPropertyValue

Hata iletisi "belirtilen VNet yok ya da Batch hizmetinin buna erişimi yok" veya "belirtilen alt ağ xxx yok" gibidir.

Bu, VNet 'in bulunmadığı veya Azure Batch hizmetin bu ağa erişemediği veya belirtilen alt ağın bulunmadığı anlamına gelir. VNet ve alt ağın mevcut olduğundan ve Azure Batch erişebildiğinden emin olun.

### <a name="misconfigureddnsserverornsgsettings"></a>Hatalı Configureddnsserverornsgsettings

İleti "VNET 'teki Integration Runtime sağlanamadı. DNS sunucusu veya NSG ayarları yapılandırılırsa, DNS sunucusunun erişilebilir olduğundan ve NSG 'nin doğru yapılandırıldığından emin olun "

Büyük olasılıkla DNS sunucusu veya NSG ayarlarının özelleştirilmiş bir yapılandırması olabilir. Bu, SSIS IR tarafından gerekli olan Azure sunucusu adının çözümlenemeyeceğini veya erişilememesine neden olur. Daha fazla bilgi için bkz. [SSIS IR VNET yapılandırma](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) belgesi. Hala sorun yaşıyorsanız Azure Data Factory destek ekibine başvurun.

### <a name="vnetresourcegrouplockedduringupgrade"></a>VNetResourceGroupLockedDuringUpgrade

SSIS IR, düzenli aralıklarla otomatik olarak güncelleştirilir ve yükseltme sırasında yeni bir Azure Batch havuzu oluşturulur ve eski Azure Batch havuzu silinir, eski havuzun VNet ile ilgili kaynağı silinir ve yeni VNet ile ilgili kaynak şu şekilde oluşturulur. aboneliğiniz. Bu hata, abonelikte veya kaynak grubu düzeyinde silme kilidi nedeniyle eski havuz için VNet ile ilgili kaynağın silinmesinin başarısız olduğu anlamına gelir. Silme kilidini kaldırma yardımı.

### <a name="vnetresourcegrouplockedduringstart"></a>VNetResourceGroupLockedDuringStart

SSIS IR sağlama nedeni nedenlerden dolayı başarısız olabilir ve bir hata oluşursa oluşturulan tüm kaynaklar silinir. Ancak, abonelikte veya kaynak grubu düzeyinde kaynak silme kilidi olduğundan VNet kaynakları silinemedi. Silme kilidini kaldırın ve IR 'yi yeniden başlatın.

### <a name="vnetresourcegrouplockedduringstop"></a>VNetResourceGroupLockedDuringStop

SSIS IR 'yi durdururken, VNet ile ilgili tüm kaynaklar silinir, ancak abonelikte veya kaynak grubu düzeyinde kaynak silme kilidi olduğundan silme işlemi başarısız olur. Silme kilidini kaldırmak ve durdurmayı yeniden denemek için yardım.

### <a name="nodeunavailable"></a>NodeUnavailable

Bu hata, çalışan IR sırasında oluşur, bu durum IR 'nin öncesinde sistem durumu olduğu ve sağlıksız olacağı anlamına gelir, her zaman DNS sunucusu veya NSG yapılandırması değiştiği ve SSIS IR 'nin bağımlı hizmete bağlanamadığı, DNS sunucusunu veya NSG yapılandırma sorunlarını gidermeye yardımcı olma Daha fazla bilgi için bkz. [SSIS IR VNET yapılandırması](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Hala sorun yaşıyorsanız Azure Data Factory destek ekibine başvurun.
