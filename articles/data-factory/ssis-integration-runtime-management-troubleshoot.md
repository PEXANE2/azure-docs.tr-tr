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
ms.openlocfilehash: 8abffdf443e26c03c38c12a3947a47a94157c9da
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68609630"
---
# <a name="troubleshoot-ssis-integration-runtime-management-in-azure-data-factory"></a>Azure Data Factory 'de SSIS Integration Runtime yönetimi sorunlarını giderme

Bu makalede, SSIS IR olarak da bilinen Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) içindeki yönetim sorunlarına yönelik sorun giderme kılavuzu sunulmaktadır.

## <a name="overview"></a>Genel Bakış

SSIS IR sağlama veya sağlamayı kaldırma sırasında herhangi bir sorunla karşılaşırsanız, Microsoft Azure Data Factory portalında bir hata iletisi veya bir PowerShell cmdlet 'inden döndürülen bir hata görürsünüz. Hata her zaman, ayrıntılı bir hata iletisiyle birlikte hata kodu biçiminde görüntülenir.

Hata kodu ınternalservererror ise, hizmette geçici sorunlar olur ve işlemi daha sonra yeniden denemeniz gerekir. Yeniden deneme işlemi yardım vermezse, Azure Data Factory destek ekibine başvurun.

Aksi takdirde, üç önemli dış bağımlılıklar hatalara neden olabilir: bir Azure SQL veritabanı sunucusu veya yönetilen örnek, özel bir kurulum betiği ve sanal ağ yapılandırması.

## <a name="azure-sql-database-server-or-managed-instance-issues"></a>Azure SQL veritabanı sunucusu veya yönetilen örnek sorunları

SSIS IR 'yi bir SSIS Katalog veritabanıyla sağlıyorsanız, bir Azure SQL veritabanı sunucusu veya yönetilen örnek gereklidir. SSIS IR, Azure SQL veritabanı sunucusuna veya yönetilen örneğe erişebilmelidir. Ayrıca, Azure SQL veritabanı sunucusunun veya yönetilen örneğin hesabının bir SSIS Katalog veritabanı (SSSıSDB) oluşturma izni olmalıdır. Bir hata oluşursa, Data Factory portalında ayrıntılı bir SQL özel durum iletisi içeren bir hata kodu gösterilir. Hata kodlarının sorunlarını gidermek için aşağıdaki listede bulunan bilgileri kullanın.

### <a name="azuresqlconnectionfailure"></a>Azuressqlconnectionfailure

Yeni bir SSIS IR sağlanırken veya IR çalışırken bu sorunu görebilirsiniz. IR sağlama sırasında bu hatayla karşılaşırsanız, aşağıdaki sorunlardan birini belirten hata iletisinde ayrıntılı bir SqlException iletisi alabilirsiniz:

* Bir ağ bağlantısı sorunu. SQL Server veya yönetilen örnek ana bilgisayar adının erişilebilir olup olmadığını denetleyin. Ayrıca, bir güvenlik duvarı veya ağ güvenlik grubu (NSG) için SSIS IR erişiminin engellemediğini doğrulayın.
* SQL kimlik doğrulaması sırasında oturum açma başarısız oldu. Girilen hesap SQL Server veritabanında oturum açamıyor. Doğru Kullanıcı hesabını sağladığınızdan emin olun.
* Microsoft Azure Active Directory (Azure AD) kimlik doğrulaması (yönetilen kimlik) sırasında oturum açma başarısız oldu. Fabrikanızın yönetilen kimliğini bir AAD grubuna ekleyin ve yönetilen kimliğin Katalog veritabanı sunucunuza erişim izinlerine sahip olduğundan emin olun.
* Bağlantı zaman aşımı. Bu hata her zaman güvenlikle ilgili bir yapılandırma nedeniyle oluşur. Şunları yapmanızı öneririz:
  1. Yeni bir VM oluşturun.
  1. IR bir sanal ağda ise, VM 'yi IR 'nin aynı Microsoft Azure Sanal Ağ birleştirin.
  1. SSMS 'yi yükleyip Azure SQL veritabanı sunucusunu veya yönetilen örnek durumunu denetleyin.

Diğer sorunlar için ayrıntılı SQL özel durum hata iletisinde gösterilen sorunu giderin. Hala sorun yaşıyorsanız, Azure SQL veritabanı sunucusu veya yönetilen örnek desteği ekibine başvurun.

IR çalışırken hata görürseniz, ağ güvenlik grubu veya güvenlik duvarı değişiklikleri büyük olasılıkla SSIS IR çalışan düğümünün Azure SQL veritabanı sunucusuna veya yönetilen örneğe erişmesini engelleyebilir. Azure SQL veritabanı sunucusuna veya yönetilen örneğe erişebilmeleri için SSIS IR çalışan düğümünün engellemesini kaldırın.

### <a name="catalogcapacitylimiterror"></a>CatalogCapacityLimitError

İşte bu tür bir hata iletisi şöyle görünebilir: "' SSSıSDB ' veritabanı boyut kotasına ulaştı. Verileri bölümleyin veya silin, dizinleri bırakın veya olası çözümler için belgelere başvurun. " 

Olası çözümler şunlardır:
* SSSıSDB 'nizin kota boyutunu artırın.
* Boyutunu azaltmak için SSSıSDB yapılandırmasını değiştirin:
   * Saklama süresini ve proje sürümü sayısını azaltma.
   * Günlüğün Bekletme dönemi azaltıyor.
   * Günlüğün varsayılan düzeyini değiştirme.

### <a name="catalogdbbelongstoanotherir"></a>CatalogDbBelongsToAnotherIR

Bu hata, Azure SQL veritabanı sunucusunun veya yönetilen örneğin zaten bir SSSıSDB 'ye sahip olduğu ve başka bir IR tarafından kullanıldığı anlamına gelir. Farklı bir Azure SQL veritabanı sunucusu veya yönetilen örnek sağlamanız ya da var olan SSSıSDB 'yi silmeniz ve yeni IR 'yi yeniden başlatmanız gerekir.

### <a name="catalogdbcreationfailure"></a>CatalogDbCreationFailure

Bu hata, aşağıdakilerden biri nedeniyle oluşabilir:

* SSIS IR için yapılandırılmış kullanıcı hesabının veritabanını oluşturma izni yok. Kullanıcıya veritabanını oluşturma izni verebilirsiniz.
* Bir yürütme zaman aşımı veya bir DB işlem zaman aşımı gibi veritabanı oluşturma sırasında zaman aşımı oluşur. İşlemi daha sonra yeniden denemeniz gerekir. Yeniden deneme işe yaramazsa, Azure SQL veritabanı sunucusu veya yönetilen örnek desteği ekibine başvurun.

Diğer sorunlar için, SQL özel durum hata iletisini kontrol edin ve hata ayrıntılarında bahsedilen sorunu giderin. Hala sorun yaşıyorsanız, Azure SQL veritabanı sunucusu veya yönetilen örnek desteği ekibine başvurun.

### <a name="invalidcatalogdb"></a>Invalidcatalogdb

Bu tür bir hata iletisi şöyle görünür: "Geçersiz nesne adı ' Catalog. catalog_properties '." Bu durumda, zaten SSSıSDB adlı bir veritabanınız var, ancak SSIS IR tarafından oluşturulmamış veya veritabanı, son SSIS IR sağlama hatalarından kaynaklanan geçersiz bir durumda. Var olan veritabanını SSSıSDB adıyla bırakabilir veya IR için yeni bir Azure SQL veritabanı sunucusu veya yönetilen örneği yapılandırabilirsiniz.

## <a name="custom-setup-issues"></a>Özel Kurulum sorunları

Özel Kurulum, SSIS IR 'nizin sağlanması veya yeniden yapılandırılması sırasında kendi kurulum adımlarınızı eklemek için bir arabirim sağlar. Daha fazla bilgi için bkz. [Azure-ssıs Integration Runtime için kurulumu özelleştirme](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Kapsayıcının yalnızca gerekli özel kurulum dosyalarını içerdiğinden emin olun; kapsayıcıdaki tüm dosyalar SSIS IR çalışan düğümüne indirilir. Betiği SSIS IR 'de çalıştırmadan önce betik yürütme sorunlarını gidermek için özel kurulum betiğini yerel bir makinede test etmenizi öneririz.

SSIS IR düzenli olarak güncelleştirildiğinden, özel kurulum betiği kapsayıcısı IR çalışırken kontrol edilecek. Bu güncelleştirme, özel kurulum betiğini indirmek ve yeniden yüklemek için kapsayıcıya erişim gerektirir. İşlem ayrıca kapsayıcının erişilebilir olup olmadığını ve Main. cmd dosyasının mevcut olup olmadığını denetler.

Özel kurulumu içeren herhangi bir hata için, Customsetupscriptblobcontainererişilemeyen veya CustomSetupScriptNotFound gibi Sub Code ile bir CustomSetupScriptFailure hata kodu görürsünüz.

### <a name="customsetupscriptblobcontainerinaccessible"></a>Customsetupscriptblobcontainererişilemeyen

Bu hata, SSIS IR 'nin özel kurulum için Azure Blob kapsayıcınıza erişemeyeceği anlamına gelir. Kapsayıcının SAS URI 'sinin erişilebilir olduğundan ve süresi dolmadığından emin olun.

Çalışıyorsa IR 'yi durdurun, yeni özel kurulum kapsayıcısı SAS URI 'SI ile IR 'yi yeniden yapılandırın ve ardından IR 'yi yeniden başlatın.

### <a name="customsetupscriptnotfound"></a>CustomSetupScriptNotFound

Bu hata, SSIS IR 'nin blob kapsayıcıda özel bir kurulum betiği (Main. cmd) bulamadığı anlamına gelir. Özel Kurulum yüklemesinin giriş noktası olan kapsayıcıda Main. cmd ' nin bulunduğundan emin olun.

### <a name="customsetupscriptexecutionfailure"></a>CustomSetupScriptExecutionFailure

Bu hata, özel kurulum betiğini (Main. cmd) yürütmenin başarısız olduğu anlamına gelir. Betiği önce yerel makinenizde deneyin veya blob kabınızda özel kurulum yürütme günlüklerine bakın.

### <a name="customsetupscripttimeout"></a>CustomSetupScriptTimeout

Bu hata bir yürütme özel kurulum betiği zaman aşımını gösterir. Blob kabınızda yalnızca gerekli özel kurulum dosyalarını içerdiğinden emin olun. Blob kabınızda özel kurulum yürütme günlüklerini de denetlemeniz gerekir. Özel kurulum için maksimum süre, zaman aşımına uğramadan 45 dakikadır ve en fazla süre, kapsayıcıınızdan tüm dosyaları indirme ve SSIS IR 'ye yükleme süresini içerir. Daha uzun bir süreye ihtiyacınız varsa, bir destek bileti yükseltin.

### <a name="customsetupscriptloguploadfailure"></a>CustomSetupScriptLogUploadFailure

Bu hata, Özel Kurulum yürütme günlüklerini blob kapsayıcınıza yükleme denemesinin başarısız olduğu anlamına gelir. Bu sorun, SSIS IR 'nin blob kapsayıcınızda veya depolama ya da ağ sorunları nedeniyle yazma izinleri olmadığından meydana gelir. Özel Kurulum başarılı olursa, bu hata SSIS işlevini etkilemez, ancak Günlükler eksik olur. Özel Kurulum başka bir hatayla başarısız olursa ve günlük karşıya yüklenememişse, bu hatayı öncelikle günlüğün analiz için karşıya yüklenebilmesi için rapor göndereceğiz. Ayrıca, bu sorun çözümlendikten sonra, daha belirli sorunları bildirecağız. Bu sorun bir yeniden denemeden sonra çözümlenemezse, Azure Data Factory destek ekibine başvurun.

## <a name="virtual-network-configuration"></a>Sanal ağ yapılandırması

SSIS IR 'yi Azure sanal ağına katdığınızda, SSIS IR, kullanıcı aboneliği altındaki sanal ağı kullanır. Daha fazla bilgi için bkz. bir [Azure-ssıs Integration Runtime sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Ağla ilgili sanal bir sorun olduğunda aşağıdaki hatalardan birini görürsünüz.

### <a name="invalidvnetconfiguration"></a>Invalidvnetconfiguration

Bu hata çeşitli nedenlerden kaynaklanabilir. Sorunu gidermek için [yasaklanmış](#forbidden), [InvalidPropertyValue](#invalidpropertyvalue)ve [Misconfigureddnsserverornsgsettings](#misconfigureddnsserverornsgsettings) bölümlerine bakın.

### <a name="forbidden"></a>Yasak

Bu tür bir hata şuna benzeyebilir: "SubnetID geçerli hesap için etkinleştirilmemiş. Microsoft. Batch kaynak sağlayıcısı, VNet 'in aynı aboneliğine kayıtlı değil. "

Bu ayrıntılar Azure Batch sanal ağınıza erişemiyorum anlamına gelir. Microsoft. Batch kaynak sağlayıcısını, sanal ağ ile aynı abonelikte kaydettirin.

### <a name="invalidpropertyvalue"></a>InvalidPropertyValue

Bu tür bir hata aşağıdakilerden birine benzemeyebilir: 

- "Belirtilen sanal ağ yok ya da Batch hizmetinin erişimi yok."
- "Belirtilen alt ağ xxx yok."

Bu hatalar, sanal ağın mevcut olmadığı, Azure Batch hizmetinin buna erişemediğini veya belirtilen alt ağın mevcut olmadığı anlamına gelir. Sanal ağın ve alt ağın mevcut olduğundan ve Azure Batch bunlara erişebildiğinden emin olun.

### <a name="misconfigureddnsserverornsgsettings"></a>Hatalı Configureddnsserverornsgsettings

Bu tür bir hata iletisi şöyle görünebilir: "VNet 'te Integration Runtime sağlanamadı. DNS sunucusu veya NSG ayarları yapılandırılırsa, DNS sunucusunun erişilebilir olduğundan ve NSG 'nin doğru yapılandırıldığından emin olun. "

Bu durumda, büyük olasılıkla DNS sunucusu veya NSG ayarlarının özelleştirilmiş bir yapılandırması vardır. Bu, SSIS IR tarafından istenen Azure sunucu adının çözümlenme veya erişilmesine karşı yapılmasını engeller. Daha fazla bilgi için bkz. [SSIS IR sanal ağ yapılandırması](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Hala sorun yaşıyorsanız Azure Data Factory destek ekibine başvurun.

### <a name="vnetresourcegrouplockedduringupgrade"></a>VNetResourceGroupLockedDuringUpgrade

SSIS IR, düzenli aralıklarla otomatik olarak güncelleştirilir. Yükseltme sırasında yeni bir Azure Batch havuzu oluşturulur ve eski Azure Batch havuzu silinir. Ayrıca, eski havuz için sanal ağla ilgili kaynaklar silinir ve aboneliğiniz altında sanal ağla ilgili yeni kaynaklar oluşturulur. Bu hata, abonelikte veya kaynak grubu düzeyinde silme kilidi nedeniyle eski havuz için sanal ağla ilgili kaynakların silinmesinin başarısız olduğu anlamına gelir. Müşteri, silme kilidini denetletiğinden ve ayarlamadığı için bu durumda silme kilidini kaldırmalıdır.

### <a name="vnetresourcegrouplockedduringstart"></a>VNetResourceGroupLockedDuringStart

SSIS IR sağlama başarısız olursa, oluşturulan tüm kaynaklar silinir. Ancak, abonelikte veya kaynak grubu düzeyinde bir kaynak silme kilidi varsa, sanal ağ kaynakları beklendiği gibi silinmez. Bu hatayı onarmak için, silme kilidini kaldırın ve IR 'yi yeniden başlatın.

### <a name="vnetresourcegrouplockedduringstop"></a>VNetResourceGroupLockedDuringStop

SSIS IR 'yi durdurduğunuzda, sanal ağla ilgili tüm kaynaklar silinir. Ancak, abonelikte veya kaynak grubu düzeyinde bir kaynak silme kilidi varsa silme işlemi başarısız olabilir. Burada, müşteri, silme kilidini denetler ve ayarlar. Bu nedenle, silme kilidini kaldırması ve sonra SSIS IR 'yi yeniden durdurması gerekir.

### <a name="nodeunavailable"></a>NodeUnavailable

IR çalışırken bu hata oluşur ve IR 'nin sağlıksız hale geldiği anlamına gelir. Bu hata her zaman, SSIS IR 'nin gerekli bir hizmete bağlanmasını engelleyen DNS sunucusu veya NSG yapılandırmasındaki bir değişiklikten kaynaklanır. DNS sunucusu ve NSG yapılandırması müşteri tarafından denetlentiğinden, müşterinin kendi sonunda engelleyici sorunları çözmesi gerekir. Daha fazla bilgi için bkz. [SSIS IR sanal ağ yapılandırması](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Hala sorun yaşıyorsanız Azure Data Factory destek ekibine başvurun.
