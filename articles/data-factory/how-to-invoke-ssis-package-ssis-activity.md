---
title: SSIS paketi yürütme etkinliği ile SSIS paketini çalıştırma-Azure | Microsoft Docs
description: Bu makalede, SSIS paketi yürütme etkinliğini kullanarak Azure Data Factory işlem hattında SQL Server Integration Services (SSIS) paketinin nasıl çalıştırılacağı açıklanmaktadır.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 09/13/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: d2fe8da1c4d49f5b57f907a5940ec9c445d0d1f7
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70984328"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Azure Data Factory 'de SSIS paketi yürütme etkinliğiyle bir SSIS paketi çalıştırın
Bu makalede, SSIS paketi yürütme etkinliğini kullanarak Azure Data Factory (ADF) işlem hattının SQL Server Integration Services (SSIS) paketinin nasıl çalıştırılacağı açıklanmaktadır. 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Öğreticideki Adım adım yönergeleri izleyerek henüz bir Azure-SSIS Integration Runtime (IR) oluşturun: Sağlama Azure-SSIS IR](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="run-a-package-in-the-azure-portal"></a>Azure portal bir paket çalıştırın
Bu bölümde, SSIS paketinizi çalıştıran, SSIS paketi yürütme etkinliği ile ADF işlem hattı oluşturmak için ADF Kullanıcı arabirimi (UI)/App kullanırsınız.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>SSIS paketi yürütme etkinliği ile işlem hattı oluşturma
Bu adımda, bir işlem hattı oluşturmak için ADF Kullanıcı arabirimi/uygulama kullanırsınız. İşlem hattına bir SSIS paketi yürütme etkinliği ekleyin ve bunu SSIS paketinizi çalıştıracak şekilde yapılandırın. 

1. ADF 'ye genel bakış/giriş sayfasında, Azure portal ADF Kullanıcı arabirimi/uygulama ' yı ayrı bir sekmede başlatmak için **yazar & İzleyici** kutucuğuna tıklayın. 

   ![Data factory giriş sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)

   **Başlarken** sayfasında Işlem **hattı oluştur**' a tıklayın: 

   ![Başlarken sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)

2. **Etkinlikler** araç kutusunda **genel**' i GENIŞLETIN ve ardından bir **SSIS paketi Yürüt** etkinliğini bir işlem hattı tasarımcı yüzeyine bırakın & sürükleyin. 

   ![Bir SSIS paketi yürütme etkinliğini tasarımcı yüzeyine sürükleyin](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. SSIS paketi yürütme etkinliğinin **genel** sekmesinde, etkinlik için bir ad ve açıklama girin. İsteğe bağlı zaman aşımını ve yeniden deneme değerlerini ayarlayın.

   ![Genel sekmesindeki özellikleri ayarla](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. SSIS paketi yürütme etkinliğinin **Ayarlar** sekmesinde, paketinizi çalıştırmak istediğiniz Azure-SSIS IR seçin. Paketiniz veri depolarına erişmek için Windows kimlik doğrulaması kullanıyorsa (örneğin, şirket içi, Azure dosyaları, vb.), **Windows kimlik doğrulaması** onay kutusunu işaretleyin ve paket yürütme kimlik bilgileriniz (**etki alanı** ) için değerleri girin. **Kullanıcı adı parolası)** /. / Alternatif olarak, Azure Key Vault (AKV) içinde depolanan gizli dizileri değerleri olarak kullanabilirsiniz. Bunu yapmak için, ilgili kimlik bilgisinin yanındaki **Azure Anahtar Kasası** onay kutusuna tıklayın, var olan Akv bağlı hizmetinizi seçin/düzenleyin veya yeni bir tane oluşturun ve ardından kimlik bilgisi değerinin gizli adını/sürümünü seçin.  AKV bağlı hizmetinizi oluştururken/düzenlerken, var olan AKV 'nizi seçebilir/düzenleyebilir veya yeni bir tane oluşturabilirsiniz, ancak henüz yapmadıysanız, lütfen AKV 'nize ADF tarafından yönetilen kimlik erişimi verin. Gizli dizilerinizi doğrudan aşağıdaki biçimde de girebilirsiniz: `<AKV linked service name>/<secret name>/<secret version>`. Paketinizin çalışması için 32 bit çalışma zamanı gerekiyorsa, **32 bit çalışma zamanı** onay kutusunu işaretleyin. 

   **Paket konumu**Için **SSISDB**, **dosya sistemi (paket)** veya **dosya sistemi (proje)** seçeneğini belirleyin. Azure-SSIS IR, Azure SQL veritabanı sunucusu/yönetilen örneği tarafından barındırılan SSIS Kataloğu (SSıSDB) ile sağlanmışsa otomatik olarak seçilen paket konumunuz olarak **Sssısdb** seçerseniz, çalıştırılacak paketinizi belirtmeniz gerekir. SSSıSDB 'ye dağıtılır. Azure-SSIS IR çalışıyorsa ve **El Ile yapılan girişler** onay kutusu işaretli DEĞILSE, SSISDB 'deki mevcut klasörlerinize/projelere/paketlerine/ortamlara gözatıp seçebilirsiniz. Yeni eklenen klasörleri/projeleri/paketleri/ortamları SSıSDB 'den getirmek için **Yenile** düğmesine tıklayın, bu nedenle bunlar göz atma ve seçim için kullanılabilir. Paket yürütmelerinin ortamlarını taramak/seçmek için, bu ortamları SSSıSDB altındaki aynı klasörlerden başvuru olarak eklemek üzere projelerinizi önceden yapılandırmanız gerekir. Daha fazla bilgi için bkz. [SSIS ortamları oluşturma/eşleme](https://docs.microsoft.com/sql/integration-services/create-and-map-a-server-environment?view=sql-server-2014).

   **Günlüğe kaydetme düzeyi**için, paket yürütülemenize yönelik önceden tanımlanmış bir günlük kapsamı seçin. Bunun yerine özelleştirilmiş günlük adınızı girmek istiyorsanız **özelleştirilmiş** onay kutusunu işaretleyin. 

   ![Ayarlar sekmesinde özellikleri ayarlama-otomatik](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

   Azure-SSIS IR çalışmıyorsa veya **el ile girdiler** onay kutusu işaretliyse, SSISDB 'den paketinizi ve ortam yollarınızı doğrudan şu biçimlerde girebilirsiniz: `<folder name>/<project name>/<package name>.dtsx` ve. `<folder name>/<environment name>`

   ![Ayarlar sekmesinde özellikleri ayarlama-El Ile](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings2.png)

   Paket konumunuz olarak **dosya sistemi (paket)** seçeneğini belirlerseniz, Azure-SSIS IR SSSıSDB olmadan sağlanmışsa otomatik olarak seçilirse, sizin Için bir evrensel adlandırma KURALı (UNC) yolu sunarak çalıştırılacak paketinizi belirtmeniz gerekir. paket yolundaki paket`.dtsx`dosyası (). Örneğin, paketinizi Azure dosyalarında depolarsanız, paket yolu `\\<storage account name>.file.core.windows.net\<file share name>\<package name>.dtsx`olacaktır. 
   
   Paketinizi ayrı bir dosyada yapılandırırsanız yapılandırma`.dtsConfig` **yolundaki**yapılandırma dosyanıza () bir UNC yolu da sağlamanız gerekir. Örneğin, yapılandırmanızı Azure dosyalarında depolarsanız, yapılandırma yolu `\\<storage account name>.file.core.windows.net\<file share name>\<configuration name>.dtsConfig`olacaktır.

   ![Ayarlar sekmesinde özellikleri ayarlama-El Ile](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings3.png)

   Paket konumunuz olarak **dosya sistemi (proje)** seçeneğini belirlerseniz, proje`.ispac` **yolundaki** () proje dosyanıza () ait bir UNC yolu ve içindeki projenizden bir paket dosyası (`.dtsx`) sağlayarak çalıştırılacak paketinizi belirtmeniz gerekir.  **Paket adı**. Örneğin, projenizi Azure dosyalarında depolarsanız, proje yolu olur `\\<storage account name>.file.core.windows.net\<file share name>\<project name>.ispac`.

   ![Ayarlar sekmesinde özellikleri ayarlama-El Ile](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings4.png)

   Ardından, projenize/paketinize/yapılandırma dosyalarınıza erişmek için kimlik bilgilerini belirtmeniz gerekir. Daha önce paket yürütme kimlik bilgileriniz için değerler girdiyseniz (yukarıya bakın), bunları **paket yürütme kimlik bilgileri Ile aynı şekilde** işaretleyerek yeniden kullanabilirsiniz. Aksi takdirde, paket erişim kimlik bilgileriniz (**etki alanı**/**Kullanıcı adı**/**parolası**) için değerleri girmeniz gerekir. Örneğin, projenizi/paketinizi/yapılandırmayı Azure dosyalarında depolarsanız, **etki alanı** `Azure`olur `<storage account name>`; **Kullanıcı adı** ; olur ve **parola** olur `<storage account key>`. Alternatif olarak, AKV 'de depolanan gizli dizileri değerleri olarak (yukarıya bakın) kullanabilirsiniz. Bu kimlik bilgileri paket ve alt paketlerinize, paketleriniz de dahil olmak üzere tüm yol/aynı proje ve yapılandırmaların yanı sıra paket yürütme görevinin tümüne erişmek için kullanılacaktır. 
   
   Paketinizi SQL Server veri araçları (SSDT) aracılığıyla oluştururken **EncryptAllWithPassword**/**EncryptSensitiveWithPassword** koruma düzeyini kullandıysanız, parolanızın değerini şifrelemeye girmeniz gerekir  **parola**. Alternatif olarak, AKV 'de depolanan bir gizli anahtarı (yukarıdaki bkz. yukarıya bakın) de kullanabilirsiniz. **EncryptSensitiveWithUserKey** koruma düzeyini kullandıysanız, yapılandırma dosyalarında veya **SSIS parametreleri**/**bağlantı yöneticileri**/özelliğindeki hassas değerlerinizi yeniden girmeniz gerekir **Sekmeleri geçersiz kılar** (aşağıya bakın). **EncryptAllWithUserKey** koruma düzeyini kullandıysanız, bu desteklenmez. bu nedenle, SSDT veya `dtutil` komut satırı yardımcı programı aracılığıyla diğer koruma düzeyini kullanmak için paketinizi yeniden yapılandırmanız gerekir. 
   
   **Günlüğe kaydetme düzeyi**için, paket yürütülemenize yönelik önceden tanımlanmış bir günlük kapsamı seçin. Bunun yerine özelleştirilmiş günlük adınızı girmek istiyorsanız **özelleştirilmiş** onay kutusunu işaretleyin. Paket yürütmelerinin paketinizdeki belirtime standart günlük sağlayıcılarını kullanarak günlüğe **kaydedilmesini**istiyorsanız günlük dosyasında UNC yolunu sağlayarak günlük klasörünüzü belirtmeniz gerekir. Örneğin, günlüklerinizi Azure dosyalarında depolu, günlük yolunuz olur `\\<storage account name>.file.core.windows.net\<file share name>\<log folder name>`. Bu yolda, her bir paket çalıştırması için bir alt klasör oluşturulur ve, her beş dakikada bir günlük dosyası oluşturulacak SSIS paketini Çalıştır etkinlik çalıştırma KIMLIĞIYLE adlandırılır. 
   
   Son olarak, günlük klasörünüze erişmek için kimlik bilgilerini de belirtmeniz gerekir. Daha önce paket erişim kimlik bilgileriniz için değerler girdiyseniz (yukarıya bakın), **paket erişimi kimlik bilgileri Ile aynı** öğesini denetleyerek bunları yeniden kullanabilirsiniz. Aksi takdirde, günlüğe kaydetme erişim kimlik bilgileriniz (**etki alanı**/**Kullanıcı adı**/**parolası**) için değerleri girmeniz gerekir. Örneğin, günlüklerinizi Azure dosyalarında depolarsanız, **etki alanı** `Azure`olur `<storage account name>`; **Kullanıcı adı** ve **parola** `<storage account key>`olur. Alternatif olarak, AKV 'de depolanan gizli dizileri değerleri olarak (yukarıya bakın) kullanabilirsiniz. Bu kimlik bilgileri günlüklerinizi depolamak için kullanılacaktır. 
   
   Yukarıda bahsedilen tüm UNC yolları için, tam nitelikli dosya adı 260 karakterden az olmalıdır ve dizin adı 248 karakterden az olmalıdır.

5. SSIS paketi yürütme etkinliğinin **SSIS parametreleri** sekmesinde, Azure-SSIS IR çalışıyorsa, paket konumunuz olarak **sssısdb** seçilidir ve **Ayarlar** sekmesinde **el ile girişler** onay kutusu işaretli değilse, var olan SSIS SSıSDB 'den seçilen projenizdeki/paketteki parametreler, bunlara değer atamanız için görüntülenecektir. Aksi takdirde, bunlara el ile değer atamak için bunları tek tek girebilirsiniz. lütfen mevcut olduklarından ve paket yürütmesinin başarılı olması için doğru girildiğinden emin olun. 
   
   Paketinizin SSDT ve **dosya sistemi (paket)** /**dosya sistemi (proje)** üzerinden oluşturulması sırasında **EncryptSensitiveWithUserKey** koruma düzeyini kullandıysanız, paket konumunuz olarak da yeniden girmeniz gerekir yapılandırma dosyalarında veya bu sekmede bunlara değer atamak için hassas parametreleriniz. 
   
   Parametrelerinizi değer atarken, ifadeleri, işlevleri, ADF sistem değişkenlerini ve ADF işlem hattı parametrelerini/değişkenlerini kullanarak dinamik içerik ekleyebilirsiniz. Alternatif olarak, AKV 'de depolanan gizli dizileri değerleri olarak (yukarıya bakın) kullanabilirsiniz.

   ![SSIS parametreleri sekmesindeki özellikleri ayarlayın](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-ssis-parameters.png)

6. SSIS paketi yürütme etkinliğinin **bağlantı yöneticileri** sekmesinde, Azure-SSIS IR çalışıyorsa, paket konumunuz olarak **sssısdb** seçilidir ve **Ayarlar** sekmesinde **el ile giriş** onay kutusu işaretli değil ise, var olan SSıSDB 'den seçilen projedeki/paketteki bağlantı yöneticileri, değerlerini özelliklerine atamanız için görüntülenecektir. Aksi takdirde, özelliklerine el ile değer atamak için bunları tek tek girebilirsiniz. lütfen mevcut olduklarından ve paket yürütmenin başarılı olması için doğru şekilde girildiğinden emin olun. 
   
   Paketinizin SSDT ve **dosya sistemi (paket)** /**dosya sistemi (proje)** üzerinden oluşturulması sırasında **EncryptSensitiveWithUserKey** koruma düzeyini kullandıysanız, paket konumunuz olarak da yeniden girmeniz gerekir hassas bağlantı Yöneticisi özelliklerinizi, yapılandırma dosyalarında veya bu sekmede bunlara değer atamaya yönelik özelliklerdir. 
   
   Bağlantı Yöneticisi özelliklerine değer atarken, ifadeleri, işlevleri, ADF sistem değişkenlerini ve ADF işlem hattı parametrelerini/değişkenlerini kullanarak dinamik içerik ekleyebilirsiniz. Alternatif olarak, AKV 'de depolanan gizli dizileri değerleri olarak (yukarıya bakın) kullanabilirsiniz.

   ![Bağlantı yöneticileri sekmesindeki özellikleri ayarla](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-connection-managers.png)

7. Özelliği, SSIS paketi yürütme etkinliğinin **geçersiz kılmaları** sekmesinde, seçili paketinizdeki mevcut özelliklerin yollarını, el ile değer atamak için bir tek tek girebilirsiniz. lütfen mevcut ve paketinize doğru şekilde girildiklerinden emin olun yürütme başarılı oldu, örn. Kullanıcı değişkeninizin değerini geçersiz kılmak için, yolunu şu biçimde girin: `\Package.Variables[User::<variable name>].Value`. 
   
   Paketinizin SSDT ve **dosya sistemi (paket)** /**dosya sistemi (proje)** üzerinden oluşturulması sırasında **EncryptSensitiveWithUserKey** koruma düzeyini kullandıysanız, paket konumunuz olarak da yeniden girmeniz gerekir hassas özelliklerinizi yapılandırma dosyalarında veya bu sekmede bunlara değer atamak için. 
   
   Özelliklerine değer atarken, ifadeleri, işlevleri, ADF sistem değişkenlerini ve ADF işlem hattı parametrelerini/değişkenlerini kullanarak dinamik içerik ekleyebilirsiniz.

   ![Özellik geçersiz kılmaları sekmesinde özellikleri ayarla](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-property-overrides.png)

   Yapılandırma dosyalarında ve *SSIS parametreleri* sekmesinde atanan değerler,/bağlantı yöneticileri**özelliği geçersiz** kılınırken **, bağlantı yöneticileri sekmesinde** atanmalarda geçersiz kılınabilir. Ayrıca **özellik geçersiz kılma** sekmesi kullanılarak geçersiz kılınacaktır.

8. İşlem hattı yapılandırmasını doğrulamak için araç çubuğunda **Doğrula** ' ya tıklayın. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

9. **Tümünü Yayımla** düğmesine tıklayarak Işlem hattını ADF 'de yayımlayın. 

### <a name="run-the-pipeline"></a>İşlem hattını çalıştırma
Bu adımda bir işlem hattı çalıştırması tetiklersiniz. 

1. Bir işlem hattı çalıştırması tetiklemek için araç çubuğunda **Tetikle** ' e tıklayın ve **Şimdi Tetikle**' ye tıklayın. 

   ![Şimdi tetikle](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. **İşlem Hattı Çalıştırma** penceresinde **Son**’u seçin. 

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme

1. Soldaki **İzleyici** sekmesine geçin. İşlem hattı çalıştırmasını ve durumunu diğer bilgilerle birlikte (çalıştırma başlangıç saati gibi) görürsünüz. Görünümü yenilemek için **Yenile**’ye tıklayın.

   ![İşlem hattı çalıştırmaları](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına tıklayın. İşlem hattının yalnızca bir etkinliği olduğundan (SSIS paketi yürütme etkinliği) yalnızca bir etkinlik çalıştırması görürsünüz.

   ![Etkinlik çalıştırmaları](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. Paketin yürütüldüğünü doğrulamak için, Azure SQL sunucunuzdaki SSıSDB veritabanında aşağıdaki **sorguyu** çalıştırabilirsiniz. 

   ```sql
   select * from catalog.executions
   ```

   ![Paket yürütmelerini doğrulama](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. Ayrıca, işlem hattı etkinliğinin çıkışından SSıSDB yürütme KIMLIĞINI alabilir ve SQL Server Management Studio (SSMS) ' de daha kapsamlı yürütme günlüklerini ve hata iletilerini denetlemek için KIMLIĞINI kullanabilirsiniz.

   ![Yürütme KIMLIĞINI alın.](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>İşlem hattını bir tetikleyici ile zamanlama

İşlem hattının zamanlamaya göre (saatlik, günlük, vb.) çalışması için zamanlanmış bir tetikleyici de oluşturabilirsiniz. Bir örnek için bkz. [Data Factory-Data Factory Kullanıcı arabirimi oluşturma](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="run-a-package-with-powershell"></a>PowerShell ile bir paket çalıştırma
Bu bölümde, SSIS paketinizi çalıştıran SSIS paketi yürütme etkinliği ile ADF işlem hattı oluşturmak için Azure PowerShell kullanırsınız. 

[Azure PowerShell yüklemek ve yapılandırmak için](/powershell/azure/install-az-ps)adım adım yönergeleri izleyerek en son Azure PowerShell modüllerini yükler.

### <a name="create-an-adf-with-azure-ssis-ir"></a>Azure-SSIS IR ile ADF oluşturma
Azure-SSIS IR zaten sağlanmış olan bir ADF 'yi kullanabilir veya [öğreticideki Adım adım yönergeleri izleyerek Azure-SSIS IR yeni bir ADF oluşturabilirsiniz: SSIS paketlerini PowerShell](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell)aracılığıyla Azure 'a dağıtın.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>SSIS paketi yürütme etkinliği ile işlem hattı oluşturma 
Bu adımda, SSIS paketi yürütme etkinliğine sahip bir işlem hattı oluşturacaksınız. Etkinlik SSSıS paketinizi çalıştırır. 

1. Aşağıdaki örneğe benzer içeriğe sahip **C:\adf\runssispackage** klasöründe **Runssıspackagepipeline. JSON** adlı bir JSON dosyası oluşturun:

   > [!IMPORTANT]
   > Dosyayı kaydetmeden önce nesne adlarını/açıklamalarını/yollarını, özellik/parametre değerlerini, parolaları ve diğer değişken değerlerini değiştirin. 

   ```json
   {
       "name": "RunSSISPackagePipeline",
       "properties": {
           "activities": [{
               "name": "MySSISActivity",
               "description": "My SSIS package/activity description",
               "type": "ExecuteSSISPackage",
               "typeProperties": {
                   "connectVia": {
                       "referenceName": "MyAzureSSISIR",
                       "type": "IntegrationRuntimeReference"
                   },
                   "executionCredential": {
                       "domain": "MyExecutionDomain",
                       "username": "MyExecutionUsername",
                       "password": {
                           "type": "SecureString",
                           "value": "MyExecutionPassword"
                       }
                   },
                   "runtime": "x64",
                   "loggingLevel": "Basic",
                   "packageLocation": {
                       "packagePath": "MyFolder/MyProject/MyPackage.dtsx",
                       "type": "SSISDB"
                   },
                   "environmentPath": "MyFolder/MyEnvironment",
                   "projectParameters": {
                       "project_param_1": {
                           "value": "123"
                       },
                       "project_param_2": {
                           "value": {
                               "value": "@pipeline().parameters.MyProjectParameter",
                               "type": "Expression"
                           }
                       }
                   },
                   "packageParameters": {
                       "package_param_1": {
                           "value": "345"
                       },
                       "package_param_2": {
                           "value": {
                               "type": "AzureKeyVaultSecret",
                               "store": {
                                   "referenceName": "myAKV",
                                   "type": "LinkedServiceReference"
                               },
                               "secretName": "MyPackageParameter"
                           }
                       }
                   },
                   "projectConnectionManagers": {
                       "MyAdonetCM": {
                           "username": {
                               "value": "MyConnectionUsername"
                           },
                           "password": {
                               "value": {
                                   "type": "SecureString",
                                   "value": "MyConnectionPassword"
                               }
                           }
                       }
                   },
                   "packageConnectionManagers": {
                       "MyOledbCM": {
                           "username": {
                               "value": {
                                   "value": "@pipeline().parameters.MyConnectionUsername",
                                   "type": "Expression"
                               }
                           },
                           "password": {
                               "value": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                                   },
                                   "secretName": "MyConnectionPassword",
                                   "secretVersion": "MyConnectionPasswordVersion"
                               }
                           }
                       }
                   },
                   "propertyOverrides": {
                       "\\Package.MaxConcurrentExecutables": {
                           "value": 8,
                           "isSensitive": false
                       }
                   }
               },
               "policy": {
                   "timeout": "0.01:00:00",
                   "retry": 0,
                   "retryIntervalInSeconds": 30
               }
           }]
       }
   }
   ```

Dosya sistemlerinde/dosya paylaşımlarında/Azure dosyalarında depolanan paketleri yürütmek için, paket/günlük konumu özelliklerine ilişkin değerleri aşağıdaki şekilde girebilirsiniz.

   ```json
   {
       {
           {
               {
                   "packageLocation": {
                       "packagePath": "//MyStorageAccount.file.core.windows.net/MyFileShare/MyPackage.dtsx",
                       "type": "File",
                       "typeProperties": {
                           "packagePassword": {
                               "type": "SecureString",
                               "value": "MyEncryptionPassword"
                           },
                           "accessCredential": {
                               "domain": "Azure",
                               "username": "MyStorageAccount",
                               "password": {
                                   "type": "SecureString",
                                   "value": "MyAccountKey"
                               }
                           }
                       }
                   },
                   "logLocation": {
                       "logPath": "//MyStorageAccount.file.core.windows.net/MyFileShare/MyLogFolder",
                       "type": "File",
                       "typeProperties": {
                           "accessCredential": {
                               "domain": "Azure",
                               "username": "MyStorageAccount",
                               "password": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                                   },
                                   "secretName": "MyAccountKey"
                               }
                           }
                       }
                   }
               }
           }
       }
   }
   ```

Dosya sistemlerinde/dosya paylaşımlarında/Azure dosyalarında depolanan projeler içindeki paketleri yürütmek için, paket konumu özelliği için değerleri aşağıdaki gibi girebilirsiniz.

   ```json
   {
       {
           {
               {
                   "packageLocation": {
                       "packagePath": "//MyStorageAccount.file.core.windows.net/MyFileShare/MyProject.ispac:MyPackage.dtsx",
                       "type": "File",
                       "typeProperties": {
                           "packagePassword": {
                               "type": "SecureString",
                               "value": "MyEncryptionPassword"
                           },
                           "accessCredential": {
                               "domain": "Azure",
                               "userName": "MyStorageAccount",
                               "password": {
                                   "type": "SecureString",
                                   "value": "MyAccountKey"
                               }
                           }
                       }
                   }
               }
           }
       }
   }
   ```

2. Azure PowerShell ' de `C:\ADF\RunSSISPackage` klasöre geçin.

3. **Runssispackagepipeline**işlem hattını oluşturmak için **set-AzDataFactoryV2Pipeline** cmdlet 'ini çalıştırın.

   ```powershell
   $DFPipeLine = Set-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                  -ResourceGroupName $ResGrp.ResourceGroupName `
                                                  -Name "RunSSISPackagePipeline"
                                                  -DefinitionFile ".\RunSSISPackagePipeline.json"
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```
   PipelineName      : Adfv2QuickStartPipeline
   ResourceGroupName : <resourceGroupName>
   DataFactoryName   : <dataFactoryName>
   Activities        : {CopyFromBlobToBlob}
   Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
   ```

### <a name="run-the-pipeline"></a>İşlem hattını çalıştırma
İşlem hattını çalıştırmak için **Invoke-AzDataFactoryV2Pipeline** cmdlet 'ini kullanın. Cmdlet, gelecekte izlemek üzere işlem hattı çalıştırma kimliğini döndürür.

```powershell
$RunId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme

İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki PowerShell betiğini çalıştırın. Aşağıdaki betiği kopyalayıp PowerShell penceresine yapıştırın ve ENTER tuşuna basın. 

```powershell
while ($True) {
    $Run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

Ardışık düzeni Azure portal kullanarak da izleyebilirsiniz. Adım adım yönergeler için bkz. işlem hattını [izleme](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="schedule-the-pipeline-with-a-trigger"></a>İşlem hattını bir tetikleyici ile zamanlama
Önceki adımda, işlem hattını talep üzerine çalıştırdınız. İşlem hattını bir zamanlamaya göre (saatlik, günlük, vb.) çalıştırmak için bir zamanlama tetikleyicisi de oluşturabilirsiniz.

1. **C:\adf\runssispackage** klasöründe aşağıdaki Içeriğe sahip **mytrigger. JSON** adlı bir JSON dosyası oluşturun: 

   ```json
   {
       "properties": {
           "name": "MyTrigger",
           "type": "ScheduleTrigger",
           "typeProperties": {
               "recurrence": {
                   "frequency": "Hour",
                   "interval": 1,
                   "startTime": "2017-12-07T00:00:00-08:00",
                   "endTime": "2017-12-08T00:00:00-08:00"
               }
           },
           "pipelines": [{
               "pipelineReference": {
                   "type": "PipelineReference",
                   "referenceName": "RunSSISPackagePipeline"
               },
               "parameters": {}
           }]
       }
   }    
   ```
2. **Azure PowerShell**, **C:\adf\runssispackage** klasörüne geçin.
3. Tetikleyiciyi oluşturan **set-AzDataFactoryV2Trigger** cmdlet 'ini çalıştırın. 

   ```powershell
   Set-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                   -DataFactoryName $DataFactory.DataFactoryName `
                                   -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
   ```
4. Varsayılan olarak, tetikleyici durdurulmuş durumda olur. **Start-AzDataFactoryV2Trigger** cmdlet 'ini çalıştırarak tetikleyiciyi başlatın. 

   ```powershell
   Start-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                     -DataFactoryName $DataFactory.DataFactoryName `
                                     -Name "MyTrigger" 
   ```
5. Tetikleyici, **Get-AzDataFactoryV2Trigger** cmdlet 'ini çalıştırarak başlatıldığını onaylayın. 

   ```powershell
   Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                   -DataFactoryName $DataFactoryName `
                                   -Name "MyTrigger"     
   ```    
6. Sonraki saatten sonra aşağıdaki komutu çalıştırın. Örneğin, geçerli saat 3:25 PM UTC ise, komutu 4 PM UTC 'de çalıştırın. 
    
   ```powershell
   Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                      -DataFactoryName $DataFactoryName `
                                      -TriggerName "MyTrigger" `
                                      -TriggerRunStartedAfter "2017-12-06" `
                                      -TriggerRunStartedBefore "2017-12-09"
   ```

   Paketin yürütüldüğünü doğrulamak için, Azure SQL sunucunuzdaki SSıSDB veritabanında aşağıdaki sorguyu çalıştırabilirsiniz. 

   ```sql
   select * from catalog.executions
   ```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki blog gönderisine bakın:
-   [ADF/ELT iş akışlarınızı, ADF işlem hatlarında SSIS etkinlikleri ile modernleştirin ve genişletme](https://techcommunity.microsoft.com/t5/SQL-Server-Integration-Services/Modernize-and-Extend-Your-ETL-ELT-Workflows-with-SSIS-Activities/ba-p/388370)
