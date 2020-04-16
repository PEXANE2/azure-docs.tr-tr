---
title: Execute SSIS Paketi etkinliği yle bir SSIS paketi çalıştırın
description: Bu makalede, Execute SSIS Paketi etkinliğini kullanarak Bir Azure Veri Fabrikası ardışık hattında bir SQL Server Integration Services (SSIS) paketinin nasıl çalıştırılacak olduğu açıklanmaktadır.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.author: sawinark
author: swinarko
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 11/14/2019
ms.openlocfilehash: f400463f6102d46d9da48bbb10466ad4ca04a69b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81413231"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Data Factory'de SSIS Paketini Yürüt etkinliğiyle bir SSIS paketi çalıştırma

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, Execute SSIS Paketi etkinliğini kullanarak Bir Azure Veri Fabrikası ardışık hattında bir SQL Server Integration Services (SSIS) paketinin nasıl çalıştırılacak olduğu açıklanmaktadır. 

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Öğreticideki adım adım yönergeleri izleyerek zaten bir Azure-SSIS tümleştirme çalışma zamanı (IR) [oluşturun: Azure-SSIS IR'yi sağlama](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="run-a-package-in-the-azure-portal"></a>Azure portalında paket çalıştırma
Bu bölümde, SSIS paketinizi çalıştıran bir Execute SSIS Paketi etkinliği içeren bir Veri Fabrikası ardışık birimi oluşturmak için Veri Fabrikası kullanıcı arabirimini (UI) veya uygulamayı kullanırsınız.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Execute SSIS Paketi etkinliği olan bir ardışık hat lar oluşturma
Bu adımda, bir ardışık hat lar oluşturmak için Veri Fabrikası UI veya uygulamasını kullanırsınız. Ardışık yapıya bir Execute SSIS Paketi etkinliği ekler ve SSIS paketinizi çalıştıracak şekilde yapılandırın. 

1. Azure portalındaki Veri Fabrikası'na genel bakışınızda veya ana sayfanızda, Veri Fabrikası Kullanıcı Arabirimi'ni veya uygulamayı ayrı bir sekmede başlatmak için **Yazar & Monitör** döşemesini seçin. 

   ![Veri Fabrikası ana sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)

   **Kullanmaya başlama** sayfasında **İşlem hattı oluştur** seçeneğini belirleyin. 

   ![Başlarken sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)

1. **Etkinlikler** araç kutusunda, **Genel**genişletin. Ardından bir **Execute SSIS Paketi** etkinliğini boru hattı tasarımcısı yüzeyine sürükleyin. 

   ![Execute SSIS Paketi etkinliğini tasarımcı yüzeyine sürükleyin](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

1. Execute SSIS Paketi etkinliği için **Genel** sekmesinde, etkinlik için bir ad ve açıklama sağlayın. İsteğe bağlı **Zaman Ekme** ve **Yeniden Deneme** değerlerini ayarlayın.

   ![Genel sekmesinde özellikleri ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

1. SSIS Paketi etkinliğini yürüt'ün **Ayarlar** sekmesinde, paketinizi çalıştırmak istediğiniz bir Azure-SSIS IR seçin. Paketiniz veri depolarına erişmek için Windows kimlik doğrulaması kullanıyorsa (örneğin, SQL sunucuları veya şirket içi veya Azure Dosyaları dosya paylaşımları), **Windows kimlik doğrulama** onay kutusunu seçin. Paket yürütme kimlik bilgilerinizin değerlerini **Etki Alanı,** **Kullanıcı Adı**ve **Parola** kutularına girin. 

    Alternatif olarak, Azure anahtar kasanızda depolanan sırları değerleri olarak kullanabilirsiniz. Bunu yapmak için, ilgili kimlik bilgisinin yanındaki **AZURE KEY VAULT** onay kutusunu seçin. Varolan anahtar kasabağlantılı hizmetinizi seçin veya edin veya yeni bir hizmet oluşturun. Ardından, kimlik bilgilerinizin gizli adını veya sürümünü seçin.

    Anahtar kasasına bağlı hizmetinizi oluşturduğunuzda veya yaptığınızda, varolan anahtar kasanızı seçebilir veya edinebilir veya yeni bir tane oluşturabilirsiniz. Veri Fabrikası'na, daha önce yapmadıysanız, anahtar kasanıza kimlik erişimi verildiğinden emin olun. Ayrıca sırlarınızı doğrudan aşağıdaki biçimde `<Key vault linked service name>/<secret name>/<secret version>`girebilirsiniz: . Paketinizin çalışması için 32 bit çalışma süresi gerekiyorsa, **32 Bit çalışma zamanı** onay kutusunu seçin.

   **Paket konumu**için **SSISDB,** Dosya Sistemi **(Paket)**, Dosya Sistemi **(Proje)** veya **Gömülü paketi**seçin. Azure-SSIS IR'niz bir Azure SQL Veritabanı sunucusu veya yönetilen örnek tarafından barındırılan SSIS kataloğu (SSISDB) ile sağlanmışsa otomatik olarak seçilen paket konumunuz olarak **SSISDB'yi** seçerseniz, Paketinizi SSISDB'de dağıtılan çalıştırmak üzere belirtin. 

    Azure-SSIS IR'niz çalışıyorsa ve **El Ile girişler** onay kutusu temizlenmişse, SSISDB'den varolan klasörlerinize, projelerinize, paketlerinize veya ortamlarınıza göz atın ve seçin. Yeni eklenen klasörlerinizi, projelerinizi, paketlerinizi veya ortamlarınızı SSISDB'den almak için **Yenile'yi** seçin ve böylece tarama ve seçim için kullanılabilir hale getirin. Paket yürütmeleriniziçin ortamlara göz atmak veya seçmek için, bu ortamları SSISDB altındaki aynı klasörlerden referans olarak eklemek için projelerinizi önceden yapılandırmanız gerekir. Daha fazla bilgi için Bkz. [SSIS ortamları oluştur ve eşle.](https://docs.microsoft.com/sql/integration-services/create-and-map-a-server-environment?view=sql-server-2014)

   **Günlük düzeyi**için, paket yürütmeniz için önceden tanımlanmış bir günlüğe kaydetme kapsamını seçin. Bunun yerine özelleştirilmiş günlük adınızı girmek istiyorsanız **Özelleştirilmiş** onay kutusunu seçin. 

   ![Ayarlar sekmesinde özellikleri ayarlama - Otomatik](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

   Azure-SSIS IR'niz çalışmıyorsa veya **El Ile girişler** onay kutusu seçiliyse, paket ve çevre yollarınızı `<folder name>/<project name>/<package name>.dtsx` `<folder name>/<environment name>`doğrudan Aşağıdaki biçimlerde SSISDB'den girin: ve .

   ![Ayarlar sekmesinde özellikleri ayarlama - El Kitabı](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings2.png)

   Azure-SSIS IR'niz SSISDB olmadan sağlanmışsa otomatik olarak seçilen paket konumunuz olarak **Dosya Sistemi'ni (Paket)** seçerseniz, Paket yol kutusunda (`.dtsx`) **Paket** dosyanıza Evrensel Adlandırma Sözleşmesi (UNC) yolu sağlayarak paketinizi çalıştırın. Örneğin, paketinizi Azure Dosyaları'nda saklarsanız, `\\<storage account name>.file.core.windows.net\<file share name>\<package name>.dtsx`paket yolu . 
   
   Paketinizi ayrı bir dosyada yapılandırırsanız, **Yapılandırma yolu** kutusunda ki yapılandırma`.dtsConfig`dosyanıza ( ) bir UNC yolu sağlamanız gerekir. Örneğin, yapılandırmanızı Azure Dosyaları'nda depolarsanız, `\\<storage account name>.file.core.windows.net\<file share name>\<configuration name>.dtsConfig`yapılandırma yolu .

   ![Ayarlar sekmesinde özellikleri ayarlama - El Kitabı](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings3.png)

   Paket konumunuz olarak **Dosya Sistemi'ni (Project)** seçerseniz, Proje yol kutusunda ( )`.ispac`Proje **yolunuzda** ( ) ve`.dtsx` **Paket adı** kutusundaki projenizden bir paket dosyası ( ) sağlayarak çalıştırmak üzere paketinizi belirtin. Örneğin, projenizi Azure Dosyaları'nda depolarsanız, `\\<storage account name>.file.core.windows.net\<file share name>\<project name>.ispac`proje yolu .

   ![Ayarlar sekmesinde özellikleri ayarlama - El Kitabı](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings4.png)

   Ardından, projenize, paketinize veya yapılandırma dosyalarınıza erişmek için kimlik bilgilerini belirtin. Paket yürütme kimlik bilgilerinizin değerlerini daha önce girdiyseniz (öncekiye bakın), **paket yürütme kimlik bilgileri** onay kutusunu aynı seçerek bunları yeniden kullanabilirsiniz. Aksi takdirde, Paket erişim kimlik bilgilerinizin değerlerini **Etki Alanı,** **Kullanıcı Adı**ve **Parola** kutularına girin. Örneğin, projenizi, paketinizi veya yapılandırmanızı Azure Dosyaları'nda `Azure`depolarsanız, `<storage account name>`etki alanı `<storage account key>`, kullanıcı adı ve parola . 

   Alternatif olarak, anahtar kasanızda depolanan sırları değerleri olarak kullanabilirsiniz (öncekiye bakın). Bu kimlik bilgileri, Paket Görev Yürüt'teki paketinize ve alt paketlerinize, hepsi kendi yollarından veya aynı projeden ve paketlerinizde belirtilenleri içeren yapılandırmalardan erişmek için kullanılır. 

   Paket konumunuz olarak **Gömülü paketi** seçerseniz, paketinizi çalıştırmak için sürükleyin ve bırakın veya dosya klasöründen sağlanan kutuya **yükleyin.** Paketiniz otomatik olarak sıkıştırılır ve etkinlik yüküne gömülür. Katıştİlke katıştın, düzenleme için paketinizi daha sonra **indirebilirsiniz.** Ayrıca, gömülü paketinizi birden çok faaliyette kullanılabilecek bir boru hattı parametresine atayarak **parametrenize parametre** ekleyebilir ve böylece boru hattı yükünüzün boyutunu optimize edebilirsiniz. Katıştırılmış paketinizin tümü şifrelenmemişse ve içinde Yürüt Paket Görevi'nin kullanımını tespit edersek, **Yürüt Paket Görev** onay kutusu otomatik olarak seçilir ve dosya sistemi referansları içeren ilgili alt paketler otomatik olarak eklenecektir. Paket Görev Yürüt'ün kullanımını algılayamazsak, **Paket Görev Yürüt** onay kutusunu el ile seçmeniz ve bunları gömmeniz için dosya sistemi referanslarıyla ilgili alt paketleri tek tek eklemeniz gerekir. Alt paketler SQL Server referansları kullanıyorsa, lütfen SQL Server'a Azure-SSIS IR'niz tarafından erişilebildiğinden emin olun.  Alt paketler için proje başvurularının kullanımı şu anda desteklenmemektedir.
   
   ![Ayarlar sekmesinde özellikleri ayarlama - El Kitabı](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings5.png)
   
   PAKETInizi SQL Server Veri Araçları ile oluşturduğunuzda **EncryptAllWithPassword** veya **EncryptSensitiveWithPassword** koruma düzeyini kullandıysanız, Şifrenizin değerini **Şifreleme parola** kutusuna girin. Alternatif olarak, anahtar kasanızda depolanan bir sırrı değeri olarak kullanabilirsiniz (öncekiye bakın). **EncryptSensitiveWithUserKey** koruma düzeyini kullandıysanız, hassas değerlerinizi yapılandırma dosyalarına veya **SSIS**Parametrelerinde, **Bağlantı Yöneticileri'nde**veya **Özellik Geçersiz Kılar** sekmelerinde yeniden girin (daha sonrabakın). 

   **EncryptAllWithUserKey** koruma düzeyini kullandıysanız, desteklenmez. Paketinizi SQL Server Data Tools veya `dtutil` komut satırı yardımcı programı aracılığıyla başka bir koruma düzeyi kullanacak şekilde yeniden yapılandırmanız gerekir. 
   
   **Günlük düzeyi**için, paket yürütmeniz için önceden tanımlanmış bir günlüğe kaydetme kapsamını seçin. Bunun yerine özelleştirilmiş günlük adınızı girmek istiyorsanız **Özelleştirilmiş** onay kutusunu seçin. Paketinizde belirtilebilen standart günlük sağlayıcılarını kullanmanın ötesinde paket yürütmelerinizi günlüğe kaydetme yapmak istiyorsanız, **UNC** yolunu Günlük yolu kutusunda sağlayarak günlük klasörünüzü belirtin. Örneğin, günlüklerinizi Azure Dosyaları'nda depolarsanız, `\\<storage account name>.file.core.windows.net\<file share name>\<log folder name>`günlük yolunuz . Her paket çalıştırılan ve günlük dosyalarının her beş dakikada bir oluşturulduğu Execute SSIS Package etkinlik çalıştırılan kimlikten sonra bu şekilde bir alt klasör oluşturulur. 
   
   Son olarak, günlük klasörünüze erişmek için kimlik bilgilerini belirtin. Paket erişim kimlik bilgilerinizin değerlerini daha önce girdiyseniz (öncekiye bakın), **paket erişim kimlik bilgileri** onay kutusunu aynı seçerek bunları yeniden kullanabilirsiniz. Aksi takdirde, **Etki Alanı,** **Kullanıcı Adı**ve **Parola** kutularında günlük erişim kimlik bilgilerinizin değerlerini girin. Örneğin, günlüklerinizi Azure Dosyaları'nda depolarsanız, `Azure`etki alanı `<storage account name>`, kullanıcı adı `<storage account key>`ve parola . 

    Alternatif olarak, anahtar kasanızda depolanan sırları değerleri olarak kullanabilirsiniz (öncekiye bakın). Bu kimlik bilgileri günlüklerinizi depolamak için kullanılır. 
   
   Daha önce bahsedilen tüm UNC yolları için, tam nitelikli dosya adı 260 karakterden az olmalıdır. Dizin adı 248 karakterden az olmalıdır.

1. Execute SSIS Paketi etkinliği için **SSIS Parametreleri** sekmesinde, Azure-SSIS IR'niz çalışıyorsa, Paket konumunuz olarak **SSISDB** seçilir ve **Ayarlar** sekmesindeki **Manuel girişler** onay kutusu temizlenir, seçtiğiniz proje veya Paketteki mevcut SSIS parametreleri, onlara değer atamanız için görüntülenir. Aksi takdirde, değerleri el ile atamak için bunları tek tek girebilirsiniz. Bunların var olduğundan ve paket yürütmenizin başarılı olması için doğru şekilde girildiğinden emin olun. 
   
   Paketinizi SQL Server Veri Araçları ve **Dosya Sistemi (Paket)** veya **Dosya Sistemi (Project)** üzerinden oluşturduğunuzda **EncryptSensitiveWithUserKey** koruma düzeyini kullandıysanız, yapılandırma dosyalarında veya bu sekmede değerleri atamak için hassas parametrelerinizi yeniden girmeniz gerekir. 
   
   Parametrelerinize değer atadığınızda, ifadeler, işlevler, Veri Fabrikası sistem değişkenleri ve Veri Fabrikası boru hattı parametreleri veya değişkenlerini kullanarak dinamik içerik ekleyebilirsiniz. Alternatif olarak, anahtar kasanızda depolanan sırları değerleri olarak kullanabilirsiniz (öncekiye bakın).

   ![SSIS Parametreleri sekmesinde özellikleri ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-ssis-parameters.png)

1. SSIS Paketi etkinliğinin **Bağlantı Yöneticileri** sekmesinde, Azure-SSIS IR'niz çalışıyorsa, Paket konumunuz olarak **SSISDB** seçilir ve **Ayarlar** sekmesindeki **Manuel girişler** onay kutusu temizlenir, seçili projenizdeki veya SSISDB'den paketteki mevcut bağlantı yöneticileri, özelliklerine değer atamanız için görüntülenir. Aksi takdirde, değerleri özelliklerine el ile atamak için bunları tek tek girebilirsiniz. Bunların var olduğundan ve paket yürütmenizin başarılı olması için doğru şekilde girildiğinden emin olun. 
   
   Paketinizi SQL Server Veri Araçları ve **Dosya Sistemi (Paket)** veya **Dosya Sistemi (Project)** üzerinden oluşturduğunuzda **EncryptSensitiveWithUserKey** koruma düzeyini kullandıysanız, yapılandırma dosyalarında veya bu sekmede değerleri atamak için hassas bağlantı yöneticisi özelliklerinizi yeniden girmeniz gerekir. 
   
   Bağlantı yöneticisi özelliklerinize değerler atadığınızda, ifadeler, işlevler, Veri Fabrikası sistem değişkenleri ve Veri Fabrikası boru hattı parametreleri veya değişkenleri kullanarak dinamik içerik ekleyebilirsiniz. Alternatif olarak, anahtar kasanızda depolanan sırları değerleri olarak kullanabilirsiniz (öncekiye bakın).

   ![Bağlantı Yöneticileri sekmesinde özellikleri ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-connection-managers.png)

1. SSIS Paketi etkinliği için **Özellik Geçersiz Kılmasekmesi'nde,** değerleri el ile atamak için seçili paketinizdeki varolan özelliklerin yollarını tek tek girin. Bunların var olduğundan ve paket yürütmenizin başarılı olması için doğru şekilde girildiğinden emin olun. Örneğin, kullanıcı değişkeninizin değerini geçersiz kılmak için yolunu aşağıdaki `\Package.Variables[User::<variable name>].Value`biçimde girin: . 
   
   Paketinizi SQL Server Veri Araçları ve **Dosya Sistemi (Paket)** veya **Dosya Sistemi (Project)** üzerinden oluşturduğunuzda **EncryptSensitiveWithUserKey** koruma düzeyini kullandıysanız, yapılandırma dosyalarında veya bu sekmede değerleri atamak için hassas özelliklerinizi yeniden girmeniz gerekir. 
   
   Özelliklerinize değer atadığınızda, ifadeler, işlevler, Veri Fabrikası sistem değişkenleri ve Veri Fabrikası boru hattı parametreleri veya değişkenlerini kullanarak dinamik içerik ekleyebilirsiniz.

   ![Özellik Geçersiz Kılmalar sekmesindeki özellikleri ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-property-overrides.png)

   Yapılandırma dosyalarında ve **SSIS Parametreleri** sekmesinde atanan değerler **Bağlantı Yöneticileri** veya Özellik **Geçersiz Kılma** sekmeleri kullanılarak geçersiz kılınabilir. **Bağlantı Yöneticileri** sekmesinde atanan değerler, **Özellik Geçersiz Kılmalar** sekmesi kullanılarak da geçersiz kılınabilir.

1. Ardışık hatyapılandırmasını doğrulamak için araç çubuğunda **Doğrula'yı** seçin. **Pipeline Doğrulama Raporunu**kapatmak için **>>**.

1. Veri Fabrikası'nda ardışık bir şekilde yayımlamak için **Tümünü Yayımla'yı**seçin. 

### <a name="run-the-pipeline"></a>İşlem hattını çalıştırma
Bu adımda, bir ardışık işlem yürütmeyi tetiklersiniz. 

1. Bir ardışık hatlar çalışmasını tetiklemek için araç çubuğunda **Tetikleyici'yi** seçin ve **şimdi Tetikle'yi**seçin. 

   ![Şimdi tetikle](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. **İşlem Hattı Çalıştırma** penceresinde **Son**’u seçin. 

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme

1. Soldaki **İzleyici** sekmesine geçin. Ardışık ardışık iştin ve durumunu, **Çalıştır Başlangıç** saati gibi diğer bilgilerle birlikte görürsünüz. Görünümü yenilemek için **Yenile**’yi seçin.

   ![İşlem hattı çalıştırmaları](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. **Eylemler** sütununda **Etkinlik Çalıştırma İşlemlerini Görüntüle**’yi seçin. Yalnızca bir etkinlik çalışır, çünkü ardışık alan yalnızca bir etkinlik vardır. Execute SSIS Paketi etkinliğidir.

   ![Etkinlik çalıştırmaları](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. Paketin yürütüldünden sonra doğrulanması için sql sunucunuzdaki SSISDB veritabanına karşı aşağıdaki sorguyu çalıştırın. 

   ```sql
   select * from catalog.executions
   ```

   ![Paket yürütmelerini doğrulama](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. Ayrıca, ssisdb yürütme kimliğini boru hattı etkinliği çalıştır'ın çıktısından alabilir ve SQL Server Management Studio'da daha kapsamlı yürütme günlüklerini ve hata iletilerini denetlemek için kimliği kullanabilirsiniz.

   ![İnfaz kimliğini al.](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>Ardışık hattı bir tetikleyiciyle zamanlama

Ayrıca, ardışık hattın saatlik veya günlük gibi bir zamanlamaya göre çalışıyor olması için ardışık hattınız için zamanlanmış bir tetikleyici de oluşturabilirsiniz. Örneğin, [bkz.](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule)

## <a name="run-a-package-with-powershell"></a>PowerShell ile bir paket çalıştırın
Bu bölümde, SSIS paketinizi çalıştıran bir Execute SSIS Paketi etkinliği içeren bir Veri Fabrikası ardışık hattı oluşturmak için Azure PowerShell'i kullanırsınız. 

[Azure PowerShell'in nasıl yüklenir ve yapılandırılabilen yönergeleri](/powershell/azure/install-az-ps)adım adım izleyerek en son Azure PowerShell modüllerini yükleyin.

### <a name="create-a-data-factory-with-azure-ssis-ir"></a>Azure-SSIS IR ile bir veri fabrikası oluşturma
Azure-SSIS IR'si zaten sağlanmış olan varolan bir veri fabrikasını kullanabilir veya Azure-SSIS IR ile yeni bir veri fabrikası oluşturabilirsiniz. Öğretici'deki adım adım talimatları [izleyin: SSIS paketlerini PowerShell üzerinden Azure'a dağıtın.](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell)

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Execute SSIS Paketi etkinliği olan bir ardışık hat lar oluşturma 
Bu adımda, Bir Execute SSIS Paketi etkinliği içeren bir ardışık hatlar oluşturmak. Etkinlik SSIS paketinizi çalıştırıyor. 

1. *C:\ADF\RunSSISPackage* klasöründe Aşağıdaki örneğe benzer içeriğe sahip *RunSSISPackagePipeline.json* adlı bir JSON dosyası oluşturun.

   > [!IMPORTANT]
   > Dosyayı kaydetmeden önce nesne adlarını, açıklamaları ve yolları, özellik veya parametre değerlerini, parolaları ve diğer değişken değerlerini değiştirin. 
    
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

   Dosya sistemlerinde, dosya paylaşımlarında veya Azure Dosyalarında depolanan paketleri yürütmek için paketinizin değerlerini girin ve konum özelliklerini aşağıdaki gibi kaydedin:

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

   Dosya sistemlerinde, dosya paylaşımlarında veya Azure Dosyalarında depolanan projelerde paketleri yürütmek için paket konum özelliğinizin değerlerini aşağıdaki gibi girin:

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

   Katıştırılmış paketleri yürütmek için paket konum özelliğinizin değerlerini aşağıdaki gibi girin:

   ```json
   {
       {
           {
               {
                   "packageLocation": {
                       "type": "InlinePackage",
                       "typeProperties": {
                           "packagePassword": {
                               "type": "SecureString",
                               "value": "MyEncryptionPassword"
                           },
                           "packageName": "MyPackage.dtsx",
                           "packageContent":"My compressed/uncompressed package content",
                           "packageLastModifiedDate": "YYYY-MM-DDTHH:MM:SSZ UTC-/+HH:MM"
                       }
                   }
               }
           }
       }
   }
   ```

2. Azure PowerShell'de *C:\ADF\RunSSISPackage* klasörüne geçin.

3. **RunSSISPackagePipeline**ardışık hattını oluşturmak için **Set-AzDataFactoryV2Pipeline** cmdlet'i çalıştırın.

   ```powershell
   $DFPipeLine = Set-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                  -ResourceGroupName $ResGrp.ResourceGroupName `
                                                  -Name "RunSSISPackagePipeline"
                                                  -DefinitionFile ".\RunSSISPackagePipeline.json"
   ```

   Örnek çıktı şu şekildedir:

   ```
   PipelineName      : Adfv2QuickStartPipeline
   ResourceGroupName : <resourceGroupName>
   DataFactoryName   : <dataFactoryName>
   Activities        : {CopyFromBlobToBlob}
   Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
   ```

### <a name="run-the-pipeline"></a>İşlem hattını çalıştırma
Boru hattını çalıştırmak için **Invoke-AzDataFactoryV2Pipeline** cmdlet'i kullanın. Cmdlet, gelecekte izlemek üzere işlem hattı çalıştırma kimliğini döndürür.

```powershell
$RunId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme

İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki PowerShell betiğini çalıştırın. PowerShell penceresinde aşağıdaki komut dosyasını kopyalayın veya yapıştırın ve Enter'u seçin. 

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

Azure portalını kullanarak da boru hattını izleyebilirsiniz. Adım adım talimatlar için [bkz.](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

### <a name="schedule-the-pipeline-with-a-trigger"></a>Ardışık hattı bir tetikleyiciyle zamanlama
Önceki adımda, talep üzerine boru hattı koştu. Ayrıca, ardışık hattı saatlik veya günlük gibi bir zamanlamada çalıştırmak için bir zamanlama tetikleyicisi de oluşturabilirsiniz.

1. *C:\ADF\RunSSISPackage* klasöründe Aşağıdaki içeriği içeren *MyTrigger.json* adlı bir JSON dosyası oluşturun: 
        
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
    
1. Azure PowerShell'de *C:\ADF\RunSSISPackage* klasörüne geçin.
1. Tetikleyiciyi oluşturan **Set-AzDataFactoryV2Trigger** cmdlet'i çalıştırın. 

   ```powershell
   Set-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                   -DataFactoryName $DataFactory.DataFactoryName `
                                   -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
   ```
1. Varsayılan olarak, tetikleyici durduruldu durumundadır. **Start-AzDataFactoryV2Trigger** cmdlet'i çalıştırarak tetiği başlatın. 

   ```powershell
   Start-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                     -DataFactoryName $DataFactory.DataFactoryName `
                                     -Name "MyTrigger" 
   ```
1. Tetikleyicinin **Get-AzDataFactoryV2Trigger** cmdlet'i çalıştırarak başlatıldığından onaylayın. 

   ```powershell
   Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                   -DataFactoryName $DataFactoryName `
                                   -Name "MyTrigger"     
   ```    
1. Sonraki saatten sonra aşağıdaki komutu çalıştırın. Örneğin, geçerli saat 15:25 UTC ise, komutu 14:00 UTC'de çalıştırın. 
    
   ```powershell
   Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                      -DataFactoryName $DataFactoryName `
                                      -TriggerName "MyTrigger" `
                                      -TriggerRunStartedAfter "2017-12-06" `
                                      -TriggerRunStartedBefore "2017-12-09"
   ```

   Paketin yürütüldünden sonra doğrulanması için sql sunucunuzdaki SSISDB veritabanına karşı aşağıdaki sorguyu çalıştırın. 

   ```sql
   select * from catalog.executions
   ```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki blog gönderisini görün:
- [Azure Veri Fabrikası boru hatlarındaki SSIS etkinlikleri ile ETL/ELT iş akışlarınızı modernize edin ve genişletin](https://techcommunity.microsoft.com/t5/SQL-Server-Integration-Services/Modernize-and-Extend-Your-ETL-ELT-Workflows-with-SSIS-Activities/ba-p/388370)
