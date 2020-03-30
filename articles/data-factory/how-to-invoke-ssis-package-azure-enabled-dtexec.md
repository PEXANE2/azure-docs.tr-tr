---
title: Azure özellikli dtexec yardımcı programı yla SQL Server Entegrasyon Hizmetleri (SSIS) paketlerini yürütün
description: Azure özellikli dtexec yardımcı programıyla SQL Server Integration Services (SSIS) paketlerini nasıl çalıştırabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/21/2019
author: swinarko
ms.author: sawinark
manager: mflasko
ms.reviewer: douglasl
ms.openlocfilehash: a5540eea91937319a6ac947b50698ccaa8b25847
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931694"
---
# <a name="run-sql-server-integration-services-packages-with-the-azure-enabled-dtexec-utility"></a>Azure özellikli dtexec yardımcı programıyla SQL Server Entegrasyon Hizmetleri paketlerini çalıştırın
Bu makalede, Azure özellikli dtexec (AzureDTExec) komut istem yardımcı programı açıklanmaktadır. Azure Veri Fabrikası'ndaki Azure-SSIS Tümleştirme Çalışma Süresi'nde (IR) SQL Server Integration Services (SSIS) paketlerini çalıştırmak için kullanılır.

Geleneksel dtexec yardımcı programı SQL Server ile birlikte gelir. Daha fazla bilgi için [dtexec yardımcı programı'na](https://docs.microsoft.com/sql/integration-services/packages/dtexec-utility?view=sql-server-2017)bakın. SSIS paketlerini şirket içinde çalıştırmak için ActiveBatch ve Control-M gibi üçüncü taraf orkestratörler veya zamanlayıcılar tarafından sık sık çağrılır. 

Modern AzureDTExec yardımcı programı bir SQL Server Management Studio (SSMS) aracıyla birlikte gelir. Ayrıca, Üçüncü taraf orkestratörler veya zamanlayıcılar tarafından Azure'da SSIS paketlerini çalıştırmak için çağrılabilir. SSIS paketlerinizin kaldırılmasını, kaydırMasını veya buluta geçişini kolaylaştırır. Geçişten sonra, günlük işlemlerinizde üçüncü taraf orkestratörleri veya zamanlayıcıları kullanmaya devam etmek istiyorsanız, artık dtexec yerine AzureDTExec'i çağırabilirler.

AzureDTExec, Paketlerinizi Veri Fabrikası ardışık hatlarında SSIS Paketi etkinliklerini yürüterken çalıştırın. Daha fazla bilgi için, [Azure Veri Fabrikası etkinlikleri olarak SSIS paketlerini çalıştır'a](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)bakın. 

AzureDTExec, Veri fabrikanızda ardışık hatlar oluşturan bir Azure Active Directory (Azure AD) uygulamasını kullanacak şekilde SSMS aracılığıyla yapılandırılabilir. Ayrıca, paketlerinizi depoladığınız dosya sistemlerine, dosya paylaşımlarına veya Azure Dosyalarına erişmek için de yapılandırılabilir. AzureDTExec, çağırma seçenekleri için verdiğiniz değerlere bağlı olarak, içinde Execute SSIS Paketi etkinliği bulunan benzersiz bir Veri Fabrikası ardışık hattı oluşturur ve çalıştırZ. AzureDTExec'i seçenekleri için aynı değerlerle çağırmak varolan ardışık alanı yeniden çalıştırıyor.

## <a name="prerequisites"></a>Ön koşullar
AzureDTExec'i kullanmak için, Sürüm 18.3 veya sonraki sürüm olan SSMS'in en son sürümünü indirin ve yükleyin. [Bu web sitesinden](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)indirin.

## <a name="configure-the-azuredtexec-utility"></a>AzureDTExec yardımcı programını yapılandırma
Yerel makinenize SSMS yüklemek AzureDTExec'i de yükler. Ayarlarını yapılandırmak için, Yönetici olarak Çalıştır seçeneğiyle SSMS'i **başlatın.** Ardından, Azure > özellikli**DTExec'i Azure****Yapılandırmasına Geçiren** **Araçlar'ı** > seçin.

![Azure özellikli dtexec menüsünü yapılandırma](media/how-to-invoke-ssis-package-azure-enabled-dtexec/ssms-azure-enabled-dtexec-menu.png)

Bu eylem, *AzureDTExec.settings* dosyasına yazabilmesi için yönetim ayrıcalıklarıyla açılması gereken bir **AzureDTExecConfig** penceresi açar. SSMS'i yönetici olarak çalıştırmadıysanız, Kullanıcı Hesabı Denetimi (UAC) penceresi açılır. Ayrıcalıklarınızı yükseltmek için yönetici parolanızı girin.

![Azure özellikli dtexec ayarlarını yapılandırma](media/how-to-invoke-ssis-package-azure-enabled-dtexec/ssms-azure-enabled-dtexec-settings.png)

**AzureDTExecConfig** penceresinde yapılandırma ayarlarınızı aşağıdaki gibi girin:

- **ApplicationId**: Veri fabrikanızda boru hattı oluşturmak için doğru izinlerle oluşturduğunuz Azure AD uygulamasının benzersiz tanımlayıcısını girin. Daha fazla bilgi için azure [portalı üzerinden bir Azure AD uygulaması ve hizmet sorumlusu oluşturun'a](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)bakın.
- **AuthenticationKey**: Azure AD uygulamanızın kimlik doğrulama anahtarını girin.
- **TenantId**: Azure AD uygulamanızın oluşturulduğu Azure AD kiracısının benzersiz tanımlayıcısını girin.
- **SubscriptionId**: Veri fabrikanızın oluşturulduğu Azure aboneliğinin benzersiz tanımlayıcısını girin.
- **Kaynak Grubu**: Veri fabrikanızın oluşturulduğu Azure kaynak grubunun adını girin.
- **DataFactory**: AzureDTExec'i çağırırken sağlanan seçeneklerin değerlerine göre, içinde SSIS Paketi etkinliğini yürüten benzersiz ardışık lıkların oluşturulduğu veri fabrikanızın adını girin.
- **IRName**: AzureDTExec'i çağırdığınızda Evrensel Adlandırma Sözleşmesi (UNC) yolunda belirtilen paketlerin çalışacağı veri fabrikanıza Azure-SSIS IR adını girin.
- **PackageAccessDomain**: AzureDTExec'i çağırdığınızda belirtilen UNC yollarında paketlerinize erişmek için etki alanı kimlik bilgilerini girin.
- **PackageAccessUserName**: AzureDTExec'i çağırdığınızda belirtilen UNC yollarında paketlerinize erişmek için kullanıcı adı kimlik bilgilerini girin.
- **PackageAccessPassword**: AzureDTExec'i çağırdığınızda belirtilen UNC yollarında paketlerinize erişmek için parola kimlik bilgilerini girin.
- **LogPath**: Azure-SSIS IR'deki paket yürütmelerinizdeki günlük dosyalarının yazıldığı günlük klasörünün UNC yolunu girin.
- **LogLevel**: Azure-SSIS IR'deki paket yürütmeleriniz için önceden tanımlanmış **null**, Basic , **Verbose**veya **Performans** seçeneklerinden günlüğe kaydetmenin seçili kapsamını girin. **Basic**
- **LogAccessDomain**: **LogPath** belirtildiğinde ve **LogLevel** **null**olmadığında gerekli olan günlük dosyalarını yazarken unc yoluna giriş klasörüne erişmek için etki alanı kimlik bilgisini girin.
- **LogAccessUserName**: **LogPath** belirtildiğinde ve **LogLevel** **null**olmadığında gerekli olan günlük dosyalarını yazarken unc yoluna giriş klasörüne erişmek için kullanıcı adı kimlik bilgilerini girin.
- **LogAccessPassword**: **LogPath** belirtildiğinde ve **LogLevel** **null**olmadığında gerekli olan günlük dosyalarını yazarken unc yoluna giriş klasörüne erişmek için parola kimlik belgesini girin.
- **PipelineNameHashStrLen**: AzureDTExec'i çağırırken sağladığınız seçeneklerin değerlerinden oluşturulacak karma dizelerin uzunluğunu girin. Dizeleri, paketlerinizi Azure-SSIS IR'de çalıştıran Veri Fabrikası ardışık adları için benzersiz adlar oluşturmak için kullanılır. Genellikle 32 karakter uzunluğunda yeterlidir.

Paketlerinizi ve dosyalarınızı dosya sistemlerinde veya şirket içinde günlüğe kaydetmek için, paketlerinizi alıp günlük dosyalarınızı yazabilmesi için Azure-SSIS IR'nize şirket içi ağınıza bağlı sanal bir ağa katılın. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)

*AzureDTExec.settings* dosyasına yazılan hassas değerleri düz metin olarak göstermemek için bunları Base64 kodlama dizelerine kodlarız. AzureDTExec'i çağırdığınızda, Base64 kodlanmış tüm dizeleri özgün değerlerine geri dönüştürülür. *AzureDTExec.settings* dosyasını erişebilen hesapları sınırlayarak daha da güvenli hale getirebilirsiniz.

## <a name="invoke-the-azuredtexec-utility"></a>AzureDTExec yardımcı programını çağırma
Komut satırı komut isteminde AzureDTExec'i çağırabilir ve kullanım örneği senaryonuzdaki belirli seçenekler için ilgili değerleri sağlayabilirsiniz.

Yardımcı program ' `{SSMS Folder}\Common7\IDE\CommonExtensions\Microsoft\SSIS\150\Binn`da yüklenir. Her yerden çağrılması için yolunu 'PATH' ortam değişkenine ekleyebilirsiniz.

```dos
> cd "C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\Microsoft\SSIS\150\Binn"
> AzureDTExec.exe  ^
  /F \\MyStorageAccount.file.core.windows.net\MyFileShare\MyPackage.dtsx  ^
  /Conf \\MyStorageAccount.file.core.windows.net\MyFileShare\MyConfig.dtsConfig  ^
  /Conn "MyConnectionManager;Data Source=MyDatabaseServer.database.windows.net;User ID=MyAdminUsername;Password=MyAdminPassword;Initial Catalog=MyDatabase"  ^
  /Set \package.variables[MyVariable].Value;MyValue  ^
  /De MyEncryptionPassword
```

AzureDTExec'i çağırmak, dtexec'i çağırmak gibi benzer seçenekler sunar. Daha fazla bilgi için [dtexec Utility'ye](https://docs.microsoft.com/sql/integration-services/packages/dtexec-utility?view=sql-server-2017)bakın. Şu anda desteklenen seçenekler şunlardır:

- **/F[ile]**: Dosya sisteminde, dosya paylaşımında veya Azure Dosyalarında depolanan bir paket yükler. Bu seçeneğin değeri olarak, .dtsx uzantılı dosya sisteminde, dosya paylaşımında veya Azure Dosyalarınızda paket dosyanızın UNC yolunu belirtebilirsiniz. Belirtilen UNC yolu herhangi bir boşluk içeriyorsa, tüm yolun etrafına tırnak işaretleri koyun.
- **/Conf[igFile]**: Değerleri ayıklamak için bir yapılandırma dosyası belirtir. Bu seçeneği kullanarak, paketiniz için tasarım zamanında belirtilenden farklı bir çalışma zamanı yapılandırması ayarlayabilirsiniz. Farklı ayarları bir XML yapılandırma dosyasında depolayabilir ve paket yürütmenizden önce yükleyebilirsiniz. Daha fazla bilgi için Bkz. [SSIS paket yapılandırmaları.](https://docs.microsoft.com/sql/integration-services/packages/package-configurations?view=sql-server-2017) Bu seçeneğin değerini belirtmek için, dtsConfig uzantılı dosya sisteminde, dosya paylaşımında veya Azure Dosyalarınızda yapılandırma dosyanızın UNC yolunu kullanın. Belirtilen UNC yolu herhangi bir boşluk içeriyorsa, tüm yolun etrafına tırnak işaretleri koyun.
- **/Conn[ection]**: Paketinizdeki mevcut bağlantı yöneticileri için bağlantı dizelerini belirtir. Bu seçeneği kullanarak, paketinizdeki varolan bağlantı yöneticileri için tasarım zamanında belirtilenlerden farklı çalışma zamanı bağlantı dizeleri ayarlayabilirsiniz. Bu seçeneğin değerini aşağıdaki `connection_manager_name_or_id;connection_string [[;connection_manager_name_or_id;connection_string]...]`gibi belirtin: .
- **/Set**: Paketinizdeki parametre, değişken, özellik, konteyner, günlük sağlayıcısı, Foreach noumerator veya bağlantı yapılandırmasını geçersiz kılar. Bu seçenek birden çok kez belirtilebilir. Bu seçeneğin değerini aşağıdaki `property_path;value`gibi belirtin: . Örneğin, `\package.variables[counter].Value;1` değişkenin `counter` değerini 1 olarak geçersiz kılar. **Paketinizdeki** değeri geçersiz kılmak istediğiniz öğelerin `property_path` değerini bulmak, kopyalamak ve yapıştırmak için Paket Yapılandırmasihirbazı'nı kullanabilirsiniz. Daha fazla bilgi için [Paket Yapılandırma sihirbazına](https://docs.microsoft.com/sql/integration-services/package-configuration-wizard-ui-reference?view=sql-server-2014)bakın.
- **/De[crypt]**: **EncryptAllWithPassword**/**EncryptSensitiveWithPassword** koruma düzeyi ile yapılandırılan paketinizin şifre çözme parolasını ayarlar.

> [!NOTE]
> AzureDTExec'i seçenekleri için yeni değerlerle **çağırmak, /De[cript]** seçeneği dışında yeni bir ardışık alan oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

AzureDTExec'i çağırdıktan sonra içinde SSIS Paketi etkinliğini yürüten benzersiz ardışık hatlar oluşturulduktan ve çalıştırıldıktan sonra, bunlar Veri Fabrikası portalında izlenebilir. Daha fazla bilgi için, [Veri Fabrikası faaliyetleri olarak SSIS paketlerini çalıştır'a](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)bakın.

> [!WARNING]
> Oluşturulan ardışık hattın yalnızca AzureDTExec tarafından kullanılması beklenmektedir. Özellikleri veya parametreleri gelecekte değişebilir, bu nedenle başka amaçlariçin değiştirmeyin veya yeniden kullanmayın. Değişiklikler AzureDTExec'i bozabilir. Bu durumda, ardışık düzen hattını silin. AzureDTExec, bir sonraki çağrıldığında yeni bir ardışık hat lar oluşturur.
