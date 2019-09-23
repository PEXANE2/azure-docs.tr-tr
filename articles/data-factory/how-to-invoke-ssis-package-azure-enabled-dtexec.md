---
title: Azure özellikli dtexec yardımcı programıyla SQL Server Integration Services (SSIS) paketlerini yürütün | Microsoft Docs
description: Azure özellikli dtexec yardımcı programıyla SQL Server Integration Services (SSIS) paketlerini yürütmeyi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/21/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 80d4bd2f4f9ea4c451f312f05fd42fbc1ba433ab
ms.sourcegitcommit: a19bee057c57cd2c2cd23126ac862bd8f89f50f5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71181333"
---
# <a name="run-sql-server-integration-services-ssis-packages-with-azure-enabled-dtexec-utility"></a>Azure özellikli dtexec yardımcı programıyla SQL Server Integration Services (SSIS) paketlerini çalıştırma
Bu makalede, Azure etkinleştirilmiş **dtexec** (**azuredtexec**) komut istemi yardımcı programı açıklanır.  Azure Data Factory (ADF) ' de Azure-SSIS Integration Runtime (IR) üzerinde SSIS paketlerini çalıştırmak için kullanılır.

Geleneksel **dtexec** yardımcı programı SQL Server gelir, daha fazla bilgi için bkz. [dtexec yardımcı programı](https://docs.microsoft.com/sql/integration-services/packages/dtexec-utility?view=sql-server-2017) belgeleri.  Genellikle, şirket içinde SSIS paketlerini çalıştırmak için etkin Batch, Control-d vb. gibi üçüncü taraf düzenleyiciler/zamanlayıcılar tarafından çağrılır.  Modern **Azuredtexec** yardımcı programı SQL Server Management Studio (SSMS) aracıyla birlikte gelir.  Ayrıca, Azure 'da SSIS paketlerini çalıştırmak için üçüncü taraf düzenleyiciler/zamanlayıcılar tarafından çağrılabilir.  SSIS paketlerinizin buluta geçişini/geçirilmesini &, kaldırma işlemini kolaylaştırır.  Geçişten sonra, günlük işlemlarınızda üçüncü taraf düzenleyiciler/zamanlayıcılar kullanmaya devam etmek istiyorsanız, artık **dtexec**yerine **azuredtexec** 'yi çağırabilir.

**Azuredtexec** , PAKETLERINIZI ADF işlem HATLARıNDA SSIS paket etkinliklerini Yürüt olarak çalıştıracak. daha fazla bilgi için bkz. [SSIS paketlerini ADF etkinlikleri olarak çalıştırma](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) makalesi.  ADF 'niz içinde işlem hatları üreten Azure Active Directory (AAD) uygulaması kullanmak için SSMS aracılığıyla yapılandırılabilir.  Ayrıca, paketlerinizi depoladığınız dosya sistemlerine/dosya paylaşımlarına/Azure dosyalarına erişecek şekilde yapılandırılabilir.  **Azuredtexec** , çağırma seçeneklerine verdiğiniz değerlere bağlı olarak, içinde SSIS paketi yürütme etkinliği ile BENZERSIZ bir ADF işlem hattı oluşturur ve çalıştırır.  **Azuredtexec** 'nin seçenekleri için aynı değerlerle çağrılması, mevcut işlem hattını yeniden çalıştırır.  Options için yeni değerlerle **Azuredtexec** çağırma yeni bir işlem hattı oluşturur.

## <a name="prerequisites"></a>Önkoşullar
**Azuredtexec**'yi kullanmak için, en son SSMS 'yi (sürüm 18,3 veya üzeri) [buradan](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)indirin ve yükleyin.

## <a name="configure-azuredtexec-utility"></a>AzureDTExec yardımcı programını yapılandırma
SSMS 'nin yerel makinenize yüklenmesi de **Azuredtexec**' i yükler.  Ayarlarını yapılandırmak için, SSMS 'yi **yönetici olarak çalıştır** seçeneğiyle başlatın ve basamaklı açılan menü öğesi araçlarını seçin > Azure **'a geçirin-> Azure-Enabled dtexec yapılandırın**.

![Azure etkin dtexec menüsünü yapılandırma](media/how-to-invoke-ssis-package-azure-enabled-dtexec/ssms-azure-enabled-dtexec-menu.png)

Bu eylem, **Azuredtexec. Settings** dosyasına yazmak için yönetim ayrıcalıklarıyla açılması gereken **Azuredtexecconfig** penceresini açılır.  SSMS 'yi yönetici olarak çalıştırdıysanız, ayrıcalıklarınızı yükseltmek için yönetici parolanızı girmeniz için bir kullanıcı hesabı denetimi (UAC) penceresi açılır.

![Azure etkin dtexec ayarlarını yapılandırma](media/how-to-invoke-ssis-package-azure-enabled-dtexec/ssms-azure-enabled-dtexec-settings.png)

**Azuredtexecconfig** penceresinde yapılandırma ayarlarınızı aşağıdaki gibi girebilirsiniz:

- **ApplicationId**: ADF 'niz için işlem hatları oluşturmak üzere doğru izinlerle oluşturduğunuz AAD uygulamasının benzersiz tanımlayıcısını girin. daha fazla bilgi için, bkz. [Azure Portal makale aracılığıyla AAD uygulaması ve hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) .

- **Authenticationkey**: AAD uygulamanız için kimlik doğrulama anahtarını girin.

- **Tenantıd**: Aad uygulamanızın oluşturulduğu AAD kiracısının benzersiz tanımlayıcısını girin.

- **Abonelik kimliği**: ADF 'nizin oluşturulduğu Azure aboneliğinin benzersiz tanımlayıcısını girin.

- **ResourceGroup**: ADF 'nizin oluşturulduğu Azure Kaynak grubunun adını girin.

- **DataFactory**:  ADF 'nizin adını girin, bu, içinde SSIS paketi yürütme etkinliği olan benzersiz işlem hatları, **Azuredtexec**çağrılırken belirtilen seçenek değerlerine göre oluşturulur.

- **Irname**: ADF ' de, **Azuredtexec** çağırma sırasında evrensel adlandırma KURALı (UNC) yolunda belirtilen paketlerin çalışacağı Azure-SSIS IR adını girin.

- **Packageaccessdomain**: **Azuredtexec**ÇAĞRıLıRKEN belirtilen UNC yolundaki paketlerinize erişmek için etki alanı kimlik bilgilerini girin.

- **Packageaccessusername**:  **Azuredtexec**ÇAĞRıLıRKEN belirtilen UNC yolundaki paketlerinize erişmek için Kullanıcı adı kimlik bilgilerini girin.

- **Packageaccesspassword**: **Azuredtexec**ÇAĞRıLıRKEN belirtilen UNC yolundaki paketlerinize erişmek için parola kimlik bilgilerini girin.

- **LogPath**:  Azure-SSIS IR paket yürütmelerinin yazılacağı günlük klasörünün UNC yolunu girin.

- **LogLevel**:  Azure-SSIS IR üzerindeki paket yürütmeleri için önceden tanımlanmış **null**/**temel**/**ayrıntılı**/**performans** seçeneklerinden günlük kaydının seçili kapsamını girin.

- **Logaccessdomain**: **LogPath** belirtildiğinde ve **LogLevel** **null**OLMADıĞıNDA, günlük klasörünüze yazarken, UNC yolundaki günlük klasörünüze erişmek için etki alanı kimlik bilgisini girin.

- **Logaccessusername**: Günlük dosyalarına yazma sırasında günlük klasörünüze erişmek için Kullanıcı adı kimlik bilgisini girin, **logPath** belirtildiğinde gerekir ve **LogLevel** **null**değildir.

- **Logaccesspassword**: Günlük dosyası yazılırken, **logPath** belirtildiğinde ve **LogLevel** **null**olmadığında, günlük klasörünüz için UNC yolundaki günlük klasörünüze erişmek üzere parola kimlik bilgilerini girin.

- **Ardışık düzen ınenamehashstrlen**: **Azuredtexec**çağrılırken sağladığınız seçenek değerlerinden oluşturulan karma dizelerin uzunluğunu girin.  Dizeler, Azure-SSIS IR paketlerinizi çalıştıran ADF işlem hatları için benzersiz adlar oluşturmak üzere kullanılır.  32 karakterlik bir uzunluk yeterlidir.

Paketlerinizi ve günlük dosyalarınızı Şirket içindeki dosya sistemlerinde/dosya paylaşımlarında depolamayı planlıyorsanız, Azure-SSIS IR şirket içi ağınıza bağlı bir sanal ağa katılmanız gerekir. böylece, paketlerinizi getirip günlük dosyalarınızı yazabilir, bkz. [bir VNET 'e ekleme Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) makalesine daha fazla bilgi.

**Azuredtexec. Settings** dosyasına yazılmış gizli değerlerin düz metin olarak gösterilmesini önlemek Için bunları Base64 kodlaması dizelerine kodlayacağız.  **Azuredtexec**'yi çağırdığınızda, tüm Base64 kodlamalı dizelerin özgün değerlerine geri gönderilir.  Bu hesaba erişebilen hesapları sınırlayarak **Azuredtexec. Settings** dosyasını daha da güvenli hale getirebilirsiniz.

## <a name="invoke-azuredtexec-utility"></a>AzureDTExec yardımcı programını çağır
Komut satırı isteminde **Azuredtexec** ' ı çağırabilir ve kullanım örneği senaryosundaki belirli seçenekler için ilgili değerleri sağlayabilirsiniz, örneğin:

`AzureDTExec.exe
  /F \\MyStorageAccount.file.core.windows.net\MyFileShare\MyPackage.dtsx
  /Conf \\MyStorageAccount.file.core.windows.net\MyFileShare\MyConfig.dtsConfig
  /Conn "MyConnectionManager;Data Source=MyDatabaseServer.database.windows.net;User ID=MyAdminUsername;Password=MyAdminPassword;Initial Catalog=MyDatabase"
  /Set \package.variables[MyVariable].Value;MyValue
  /De MyEncryptionPassword`

**Azuredtexec** çağırma, **dtexec**çağırma gibi benzer seçenekler sunar. daha fazla bilgi için bkz. [dtexec yardımcı programı](https://docs.microsoft.com/sql/integration-services/packages/dtexec-utility?view=sql-server-2017) belgeleri.  Şu anda desteklenen seçenekler şunlardır:

- **/F [ile]** : Dosya sistemi/dosya paylaşımında/Azure dosyalarında depolanan bir paketi yükler.  Bu seçeneğin değeri olarak, dosya sistemi/dosya paylaşımında/Azure dosyalarında dtsx uzantısıyla paket dosyanız için UNC yolunu belirtebilirsiniz.  Belirtilen UNC yolu herhangi bir boşluk içeriyorsa, tüm yolun etrafına tırnak işareti koymanız gerekir.

- **/Conf [ıgfile]** : Değerlerin ayıklanacağı bir yapılandırma dosyasını belirtir.  Bu seçeneği kullanarak, paketiniz için tasarım zamanında belirtilenden farklı bir çalışma zamanı yapılandırması belirleyebilirsiniz.  Farklı ayarları bir XML yapılandırma dosyasında depolayıp paket yürütmeden önce yükleyebilirsiniz.  Daha fazla bilgi için bkz. [SSIS paket yapılandırması](https://docs.microsoft.com/sql/integration-services/packages/package-configurations?view=sql-server-2017) makalesi.  Bu seçeneğin değeri olarak, dosya sistemi/dosya paylaşımında/Azure dosyalarında dtsConfig uzantısıyla yapılandırma dosyanız için UNC yolunu belirtebilirsiniz.  Belirtilen UNC yolu herhangi bir boşluk içeriyorsa, tüm yolun etrafına tırnak işareti koymanız gerekir.

- **/Conn [ection]** : Paketinizdeki mevcut bağlantı yöneticileri için bağlantı dizelerini belirtir.  Bu seçeneği kullanarak, paketinizdeki mevcut bağlantı yöneticileri için tasarım zamanında belirtilenlerden farklı çalışma zamanı bağlantı dizeleri ayarlayabilirsiniz.  Bu seçeneğin değeri olarak, aşağıdaki gibi belirtebilirsiniz: `connection_manager_name_or_id;connection_string [[;connection_manager_name_or_id;connection_string]...]`.

- **/Set**: Paketteki bir parametrenin, değişkenin, özelliğin, kapsayıcının, günlük sağlayıcının, Foreach Numaralandırıcının veya bağlantının yapılandırmasını geçersiz kılar.  Bu seçenek birden çok kez belirtilebilir.  Bu seçeneğin değeri olarak, bunu aşağıdaki şekilde belirtebilirsiniz: `property_path;value`örneğin `\package.variables[counter].Value;1` , `counter` değişkenin değerini 1 olarak geçersiz kılar.  Paket Yapılandırma Sihirbazı 'nı, değerini geçersiz kılmak istediğiniz paketteki öğelerin değerini `property_path` bulmak, kopyalamak ve yapıştırmak için kullanabilirsiniz. daha fazla bilgi için [paket Yapılandırma Sihirbazı](https://docs.microsoft.com/sql/integration-services/package-configuration-wizard-ui-reference?view=sql-server-2014) belgelerine bakın.

- **/De [Crypt]** : Paketinizin **EncryptAllWithPassword**/**EncryptSensitiveWithPassword** koruma düzeyiyle yapılandırılan şifre çözme parolasını ayarlar.

## <a name="next-steps"></a>Sonraki adımlar
SSIS paketi yürütme etkinliğindeki benzersiz işlem hatları, **Azuredtexec**çağrıldıktan sonra oluşturulup ÇALıŞTıRıLDıĞıNDA, ADF portalında düzenlenebilir ve yeniden çalıştırılabilir. daha fazla bilgi için bkz. [SSIS paketlerini ADF etkinlikleri olarak çalıştırma](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) makalesi.
