---
title: Sorun giderme Azure Data Factory | Microsoft Docs
description: Azure Data Factory 'de dış denetim etkinliklerinin nasıl giderileceğini öğrenin.
services: data-factory
author: abnarain
manager: craigg
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 6/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: c76242c176ba4f4c9ffc0d6934f6b645743d77f4
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234578"
---
# <a name="troubleshoot-azure-data-factory"></a>Azure Data Factory sorunlarını giderme

Bu makalede Azure Data Factory içindeki dış denetim etkinlikleri için genel sorun giderme yöntemleri incelenmektedir.

## <a name="azure-databricks"></a>Azure Databricks

| Hata kodu | Hata iletisi                                          | Açıklama                             | Öneri                             |
| -------------- | ----------------------------------------------------- | --------------------------------------------------------------| :----------------------------------------------------------- |
| 3200           | Hata 403.                                                    | Databricks erişim belirtecinin süresi doldu.                         | Varsayılan olarak, Databricks erişim belirteci 90 gün için geçerlidir.  Yeni bir belirteç oluşturun ve bağlı hizmeti güncelleştirin. |
| 3201           | Gerekli alan eksik: Settings. Task. notebook_task. notebook_path | Hatalı yazma: Not defteri yolu doğru belirtilmedi. | Databricks etkinliğinde Not defteri yolunu belirtin. |
| 3201           | Küme... yok.                                 | Yazma hatası: Databricks kümesi yok veya silinmiş. | Databricks kümesinin var olduğunu doğrulayın. |
| 3201           | Geçersiz Python dosya URI 'SI.... Desteklenen URI düzenleri için lütfen Databricks Kullanıcı kılavuzunu ziyaret edin. | Hatalı yazma.                                                | Çalışma alanı adresleme şemaları `dbfs:/folder/subfolder/foo.py` için mutlak yollar ya da databricks dosya sisteminde depolanan dosyalar için belirtin. |
| 3201           | {0}LinkedService 'in, gerekli özellikler olarak etki alanı ve accessToken olması gerekir. | Hatalı yazma.                                                | [Bağlı hizmet tanımını](compute-linked-services.md#azure-databricks-linked-service)doğrulayın. |
| 3201           | {0}LinkedService, oluşturma için mevcut küme KIMLIĞINI ya da yeni küme bilgilerini belirtmelidir. | Hatalı yazma.                                                | [Bağlı hizmet tanımını](compute-linked-services.md#azure-databricks-linked-service)doğrulayın. |
| 3201           | Düğüm türü Standard_D16S_v3 desteklenmiyor. Desteklenen düğüm türleri:   Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3, Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2, Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3, Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2, Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2, Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3, Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s, Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2, Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2, Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3, Standard_ NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2, Standard_L64s_v2, Standard_L80s_v2. | Hatalı yazma.                                                | Hata iletisine bakın.                                          |
| 3201           | Geçersiz notebook_path:... Şu anda yalnızca mutlak yollar destekleniyor. Yolların '/' ile başlaması gerekir. | Hatalı yazma.                                                | Hata iletisine bakın.                                          |
| 3202           | Son 3600 saniye içinde oluşturulmuş 1000 iş zaten var, hız sınırı aşılıyor:   3600 saniyede 1000 iş oluşturma işlemi. | Bir saat içinde çok fazla Databricks çalışıyor.                         | Bu Databricks çalışma alanını kullanan tüm işlem hatlarını iş oluşturma oranı için denetleyin.  İşlem hatları çok fazla Databricks çalıştırıyorsa, bazı işlem hatlarını yeni bir çalışma alanına geçirin. |
| 3202           | İstek nesnesi ayrıştırılamadı: JSON eşleme alanı base_parameters için ' Key ' ve ' Value ' değerinin ayarlanması bekleniyordu, ' Key: "..." ' alındı yerine. | Yazma hatası: Parametresi için değer belirtilmedi.         | JSON işlem hattını inceleyin ve baseParameters not defterindeki tüm parametrelerin boş olmayan bir değer belirttiğinden emin olun. |
| 3202           | Kullanıcı: `SimpleUserContext{userId=..., name=user@company.com, orgId=...}` kümeye erişim yetkisi yok. | Erişim belirtecini oluşturan kullanıcının bağlantılı hizmette belirtilen Databricks kümesine erişmesine izin verilmiyor. | Kullanıcının çalışma alanında gerekli izinlere sahip olduğundan emin olun.   |
| 3203           | Küme sonlandırılmış durumda ve işleri almak için kullanılamaz. Lütfen kümeyi düzeltip daha sonra yeniden deneyin. | Küme sonlandırıldı.    Etkileşimli kümeler için bu bir yarış durumu olabilir. | Bu hatayı önlemenin en iyi yolu iş kümelerini kullanmaktır.             |
| 3204           | İş yürütülemedi.  | Hata iletileri beklenmeyen küme durumu veya belirli bir etkinlik gibi çeşitli sorunları gösterir. Çoğu zaman hiç bir hata iletisi görüntülenmez.                                                          | Yok                                                          |



## <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics

Aşağıdaki tablo U-SQL için geçerlidir.

| Hata kodu         | Hata iletisi                                                | Açıklama                                          | Öneri                            |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2709                 | Erişim belirteci yanlış kiracıya ait.                    | Yanlış Azure Active Directory (Azure AD) kiracısı.                                         | Azure Data Lake Analytics erişmek için kullanılan hizmet sorumlusu başka bir Azure AD kiracısına aittir. Data Lake Analytics hesabıyla aynı kiracıda yeni bir hizmet sorumlusu oluşturun. |
| 2711, 2705, 2704 | Inı. ACL doğrulaması başarısız oldu. Kaynak yok ya da Kullanıcı istenen işlemi gerçekleştirme yetkisine sahip değil.<br/><br/>Kullanıcı Data Lake Store erişemiyor.  <br/><br/>Kullanıcının Data Lake Analytics kullanma yetkisi yok. | Hizmet sorumlusu veya sertifikasının depolama alanındaki dosyaya erişimi yok. | Data Lake Analytics işlerin Data Lake Analytics hesabına ve kök klasörden varsayılan Data Lake Storage örneğine erişimi olduğundan emin olun. |
| 2711                 | ' Azure Data Lake Store ' dosyası veya klasörü bulunamıyor.       | U-SQL dosyasının yolu yanlış veya bağlı hizmet kimlik bilgilerinin erişimi yok. | Bağlı hizmette belirtilen yolu ve kimlik bilgilerini doğrulayın. |
| 2707                 | AzureDataLakeAnalytics hesabı çözümlenemiyor. Lütfen ' AccountName ' ve ' Datalakeanaltici ' öğesini kontrol edin. | Bağlı hizmette Data Lake Analytics hesabı yanlış.                  | Doğru hesabın sağlandığını doğrulayın.             |
| 2703                 | Hata Kimliği: E_CQO_SYSTEM_INTERNAL_ERROR (veya "hata kimliği:" ile başlayan herhangi bir hata). | Hata Data Lake Analytics.                                    | Örnek gibi bir hata, işin Data Lake Analytics gönderildiği ve betiği başarısız olduğu anlamına gelir. Data Lake Analytics araştırın. Portalda Data Lake Analytics hesabına gidin ve Data Factory etkinlik çalıştırma KIMLIĞINI (işlem hattı çalıştırma KIMLIĞI değil) kullanarak işi arayın. İş, hata hakkında daha fazla bilgi sağlar ve sorun gidermenize yardımcı olur. Çözüm açık değilse, Data Lake Analytics destek ekibine başvurun ve hesap adınızı ve iş KIMLIĞINI içeren iş URL 'sini sağlayın. |
| 2709                 | Şu anda işinizi kabul edemedik. Hesabınız için sıraya alınan en fazla iş sayısı 200 ' dir. | Bu hata, Data Lake Analytics azaltma nedeniyle oluşur.                                           | Data Factory Tetikleyicileri ve etkinliklerdeki eşzamanlılık ayarlarını değiştirerek Data Lake Analytics gönderilen işlerin sayısını azaltın. Veya Data Lake Analytics limitlerini artırın. |
| 2709                 | Bu iş, 24 Avustralya gerektirdiğinden reddedildi. Bu hesabın yönetici tanımlı ilkesi, bir işin 5 ' ten fazla au kullanmasını engelliyor. | Bu hata, Data Lake Analytics azaltma nedeniyle oluşur.                                           | Data Factory Tetikleyicileri ve etkinliklerdeki eşzamanlılık ayarlarını değiştirerek Data Lake Analytics gönderilen işlerin sayısını azaltın. Veya Data Lake Analytics limitlerini artırın. |



## <a name="azure-functions"></a>Azure functions (Azure işlevleri)

| Hata kodu | Hata iletisi                           | Açıklama                                                  | Öneri                           |
| ------------ | --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 3600         | Yanıt içeriği geçerli bir JObject değil. | Çağrılan Azure işlevi yanıtta bir JSON yükü döndürmedi. Data Factory içindeki Azure işlevi etkinliği yalnızca JSON yanıt içeriğini destekler. | Azure işlevini, geçerli bir JSON yükü döndürecek şekilde güncelleştirin. Örneğin, C# bir işlev kimlik `(ActionResult)new<OkObjectResult("{` \"\":\"123'i`}");`döndürebilir.\" |
| 3600         | Geçersiz HttpMethod: '... '.               | Etkinlik yükünde belirtilen HTTP yöntemi Azure işlevi etkinliği tarafından desteklenmiyor. | PUT, POST, GET, DELETE, OPTIONS, HEAD veya TRACE gibi desteklenen bir HTTP yöntemi kullanın. |



## <a name="custom"></a>Özel

Aşağıdaki tablo Azure Batch için geçerlidir.

| Hata kodu | Hata iletisi                                                | Açıklama                                                  | Öneri                          |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2500         | Beklenmeyen bir özel durum okuması ve yürütme başarısız oldu.             | Komut başlatılamıyor veya program bir hata kodu döndürdü. | Yürütülebilir dosyanın var olduğundan emin olun. Program başlatılmışsa, *stdout. txt* ve *stderr. txt* ' nin depolama hesabına yüklendiğinden emin olun. Hata ayıklama için kodunuzda bir kopan günlüklerini yayma iyi bir uygulamadır. |
| 2501         | Kullanıcı Batch hesabına erişilemiyor; Lütfen Batch hesabı ayarlarını kontrol edin. | Hatalı toplu erişim anahtarı veya havuz adı.            | Bağlı hizmette havuz adını ve Batch erişim anahtarını doğrulayın. |
| 2502         | Kullanıcı depolama hesabına erişilemiyor; Lütfen depolama hesabı ayarlarını kontrol edin. | Hatalı depolama hesabı adı veya erişim anahtarı.       | Bağlı hizmette depolama hesabı adını ve erişim anahtarını doğrulayın. |
| 2504         | İşlem geçersiz bir durum kodu döndürdü ' BadRequest '.     | Özel etkinliğin folderPath öğesinde çok fazla dosya yok. ResourceFiles 'ın toplam boyutu 32.768 karakterden fazla olamaz. | Gereksiz dosyaları kaldırın. Ya da bunları ayıklayın ve ayıklamak için bir unzip komutu ekleyin. Örneğin,`powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;  $folder\yourProgram.exe` |
| 2505         | Hesap anahtarı kimlik bilgileri kullanılmadığı takdirde paylaşılan erişim Imzası oluşturulamıyor. | Özel etkinlikler yalnızca bir erişim anahtarı kullanan depolama hesaplarını destekler. | Hata açıklamasına bakın.                                            |
| 2507         | Klasör yolu yok veya boş:....            | Belirtilen yoldaki depolama hesabında dosya yok.       | Klasör yolu çalıştırmak istediğiniz yürütülebilir dosyaları içermelidir. |
| 2508         | Kaynak klasörde yinelenen dosyalar var.               | Aynı ada sahip birden çok dosya folderPath 'in farklı alt klasörlerinde. | Özel Etkinlikler, folderPath altındaki klasör yapısını düzleştirebilir.  Klasör yapısını korumanız gerekiyorsa, dosyaları zip halinde ve bir unzip komutu kullanarak Azure Batch ayıklayın. Örneğin,`powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;   $folder\yourProgram.exe` |
| 2509         | Batch URL 'si... geçersiz; URI biçiminde olmalıdır.         | Batch URL 'Leri şuna benzer olmalıdır`https://mybatchaccount.eastus.batch.azure.com` | Hata açıklamasına bakın.                                            |
| 2510         | İstek gönderilirken bir hata oluştu.               | Batch URL 'SI geçersiz.                                         | Batch URL 'sini doğrulayın.                                            |

## <a name="hdinsight"></a>HDInsight

Aşağıdaki tablo Spark, Hive, MapReduce, Pig ve Hadoop akışı için geçerlidir.

| Hata kodu | Hata iletisi                                                | Açıklama                                                  | Öneri                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2300, 2310 | Hadoop işi gönderilemedi. Hata: Uzak ad çözümlenemedi. <br/><br/>Küme bulunamadı. | Belirtilen küme URI 'SI geçersiz.                              | Kümenin silinmediğinden ve belirtilen URI 'nin doğru olduğundan emin olun. URI 'yi bir tarayıcıda açtığınızda, ambarı Kullanıcı arabirimini görmeniz gerekir. Küme bir sanal ağda ise, URI özel URI olmalıdır. Açmak için, aynı sanal ağın parçası olan bir VM kullanın. Daha fazla bilgi için bkz. [Apache Hadoop hizmetlerine doğrudan bağlanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services). |
| 2300         | Hadoop işi gönderilemedi. İş:..., küme:.../. Hata: Bir görev iptal edildi. | İş gönderimi zaman aşımına uğradı.                         | Sorun genel HDInsight bağlantısı ya da ağ bağlantısı olabilir. Önce HDInsight ambarı Kullanıcı arabiriminin herhangi bir tarayıcıdan kullanılabilir olduğunu doğrulayın. Kimlik bilgilerinizin hala geçerli olduğunu doğrulayın. Şirket içinde barındırılan tümleşik çalışma zamanı (IR) kullanıyorsanız bunu, şirket içinde barındırılan IR 'nin yüklü olduğu VM veya makineden gerçekleştirdiğinizden emin olun. Sonra Data Factory işi yeniden göndermeyi deneyin. Hala başarısız olduysa, destek için Data Factory ekibine başvurun. |
| 2300         | Erişilmesini   Ambarı Kullanıcı adı veya parolası yanlış  <br/><br/>Erişilmesini   Kullanıcı Yöneticisi, ambarı 'nda kilitlidir.   <br/><br/>403-Yasak: Erişim reddedildi. | HDInsight kimlik bilgileri yanlış veya zaman aşımına uğradı. | Kimlik bilgilerini düzeltin ve bağlı hizmeti yeniden dağıtın. Önce, herhangi bir tarayıcıda küme URI 'sini açarak ve oturum açmaya çalışarak, kimlik bilgilerinin HDInsight üzerinde çalıştığından emin olun. Kimlik bilgileri çalışmazsa, bunları Azure portal sıfırlayabilirsiniz. |
| 2300, 2310 | 502 - Web sunucusu bir ağ geçidi veya proxy sunucu olarak çalışırken geçersiz yanıt aldı.       <br/>Hatalı ağ geçidi. | Bu hata HDInsight 'tan.                               | Bu hata HDInsight kümesinden. Daha fazla bilgi için bkz. [ambarı Kullanıcı arabirimi 502 hatası](https://hdinsight.github.io/ambari/ambari-ui-502-error.html), [502 Spark thrift sunucusuna bağlanma](https://hdinsight.github.io/spark/spark-thriftserver-errors.html) [502, Spark Thrift sunucusuna bağlanma](https://hdinsight.github.io/spark/spark-thriftserver-errors.html)ve [Application Gateway hatalı ağ geçidi hatalarıyla ilgili sorunları giderme](https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502). |
| 2300         | Hadoop işi gönderilemedi. İş:..., küme:... Hata: {\"Error\":\"çok fazla sayıda gönderme işi isteğiyle temptaton hizmeti meşgul olduğundan, iş gönderme isteğine bakım yapılamıyor. İşlemi yeniden denemeden önce lütfen bir süre bekleyin. Eşzamanlı istekleri yapılandırmak için lütfen config temptaton. parallelliı. job. ini dosyasına bakın.  <br/><br/>Hadoop işi gönderilemedi. İşinden 161dad5d4-6fa8-4ef4-A240-6b6428c5ae2f, küme: `https://abc-analytics-prod-hdi-hd-trax-prod01.azurehdinsight.net/`.   Hata: {\"Error\":\"Java. IO. IOException: org. Apache. Hadoop. yarn. Exceptions. yarnözel durum: Application_1561147195099_3730, YARN: org. Apache. Hadoop. Security. AccessControlException hizmetine gönderilemedi: Queue root. joblauncher zaten 500 uygulama içeriyor, uygulamanın gönderimi kabul edilemiyor: application_1561147195099_3730 \ | HDInsight 'a aynı anda çok fazla iş gönderiliyor. | HDInsight 'a gönderilen eşzamanlı iş sayısını sınırlamayı göz önünde bulundurun. İşlerin aynı etkinlik tarafından gönderiliyorsa Data Factory etkinlik eşzamanlılık bölümüne bakın. Yinelenen işlem hattı çalıştırmaları zaman içinde yayma için Tetikleyicileri değiştirin. Hatanın önereceği şekilde ayarlamak `templeton.parallellism.job.submit` için HDInsight belgelerine bakın. |
| 2303, 2347 | Hadoop işi ' 5 ' çıkış koduyla başarısız oldu. Daha fazlawasbs://adfjobs@adftrialrun.blob.core.windows.net/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderrayrıntı için bkz. ' '.  <br/><br/>Hive yürütmesi, ' UserErrorHiveOdbcCommandExecutionFailure ' hata koduyla başarısız oldu.   Daha fazlawasbs://adfjobs@eclsupplychainblobd.blob.core.windows.net/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.outayrıntı için bkz. ' '. | İş HDInsight 'a gönderildi ve HDInsight üzerinde başarısız oldu. | İş HDInsight 'a başarıyla gönderildi. Kümede başarısız oldu. İş ve günlükleri HDInsight ambarı Kullanıcı arabiriminde açın ya da hata iletisi önerirken dosyayı depolamadan açın. Dosya hata ayrıntılarını gösterir. |
| 2328         | İstek işlenirken iç sunucu hatası oluştu. Lütfen isteği yeniden deneyin veya desteğe başvurun. | Bu hata HDInsight 'ta istek üzerine oluşur.                              | HDInsight sağlama başarısız olduğunda HDInsight hizmetinden bu hata gelir. HDInsight ekibine başvurun ve isteğe bağlı küme adını sağlayın. |
| 2310         | Java. lang. NullPointerException                               | Bu hata, iş bir Spark kümesine gönderildiğinde oluşur.      | Bu özel durum HDInsight 'tan gelir. Asıl sorunu gizler. Destek için HDInsight ekibine başvurun. Bunları küme adı ve etkinlik çalışma zamanı aralığı ile birlikte sağlayın. |
|              | Diğer tüm hatalar                                             |                                                              | HDInsight ve HDInsight [kullanarak sorun giderme](../hdinsight/hdinsight-troubleshoot-guide.md) [hakkında SSS](https://hdinsight.github.io/)bölümüne bakın. |



## <a name="web-activity"></a>Web Etkinliği

| Hata kodu | Hata iletisi                                                | Açıklama                                                  | Öneri                          |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2108         | Geçersiz HttpMethod: '... '.                                    | Web etkinliği, etkinlik yükünde belirtilen HTTP metodunu desteklemiyor. | Desteklenen HTTP yöntemleri konur, POST, al ve SIL. |
| 2108         | Geçersiz sunucu hatası 500.                                     | Uç noktada iç hata.                               | URL 'deki işlevselliği denetlemek için Fiddler veya Postman kullanın. |
| 2108         | Yetkisiz 401.                                             | İstekte geçersiz kimlik doğrulaması yok.                      | Belirtecin geçerliliği bitmiş olabilir. Geçerli bir kimlik doğrulama yöntemi sağlayın. URL 'deki işlevselliği denetlemek için Fiddler veya Postman kullanın. |
| 2108         | Yasak 403.                                                | Gerekli izinler eksik.                                 | Erişilen kaynaktaki Kullanıcı izinlerini denetleyin. URL 'deki işlevselliği denetlemek için Fiddler veya Postman kullanın.  |
| 2108         | Hatalı Istek 400.                                              | Geçersiz HTTP isteği.                                         | İsteğin URL 'sini, fiilini ve gövdesini denetleyin. İsteği doğrulamak için Fiddler veya Postman kullanın.  |
| 2108         | Bulunamadı 404.                                                | Kaynak bulunamadı.                                       | İsteği doğrulamak için Fiddler veya Postman kullanın.  |
| 2108         | Hizmet kullanılamıyor.                                          | Hizmet kullanılamıyor.                                       | İsteği doğrulamak için Fiddler veya Postman kullanın.  |
| 2108         | Desteklenmeyen medya türü.                                       | İçerik türü Web etkinliği gövdesi ile eşleşmiyor.           | Yük biçimiyle eşleşen içerik türünü belirtin. İsteği doğrulamak için Fiddler veya Postman kullanın. |
| 2108         | Aradığınız kaynak kaldırılmış, adı değiştirilmiş ya da geçici olarak kullanılamıyor. | Kaynak kullanılamıyor.                                | Uç noktayı denetlemek için Fiddler veya Postman kullanın. |
| 2108         | Geçersiz bir Yöntem (HTTP fiili) kullanıldığından aradığınız sayfa görüntülenemiyor. | İstekte yanlış bir Web etkinliği yöntemi belirtildi.   | Uç noktayı denetlemek için Fiddler veya Postman kullanın. |
| 2108         | invalid_payload                                              | Web etkinliği gövdesi yanlış.                       | Uç noktayı denetlemek için Fiddler veya Postman kullanın. |

Fiddler 'i kullanarak izlenen Web uygulamasının HTTP oturumunu oluşturma:

1. [Fiddler](https://www.telerik.com/download/fiddler)'i indirin, yükleyin ve açın.

1. Web uygulamanız https kullanıyorsa, **Araçlar** > **Fiddler seçenekler** > **https**' ye gidin. **Yakalama HTTPS bağlantıları** ve **https trafiğinin şifresini çöz**' ü seçin. 
   
   ![Fiddler seçenekleri](media/data-factory-troubleshoot-guide/fiddler-options.png)

1. Uygulamanız SSL sertifikaları kullanıyorsa, Fiddler sertifikasını cihazınıza ekleyin. **Araçlar** > Fiddler > seçenekleri**https**eylemlerikök > **sertifikayı masaüstüne dışarı aktar ' a gidin.**  > 

1. **Dosya** > **yakalama trafiğine**giderek yakalamayı kapatın. Ya da **F12**tuşuna basın.

1. Tüm önbelleğe alınmış öğelerin kaldırılması ve yeniden indirilmesi gereken şekilde tarayıcınızın önbelleğini temizleyin.

1. İstek oluştur: 

   a. **Besteci** sekmesini seçin.

   b. HTTP yöntemini ve URL 'YI ayarlayın.

   c. Gerekiyorsa üstbilgiler ve istek gövdesi ekleyin.

   d. **Yürüt**’ü seçin.

9. Trafiği yeniden yakalamayı açın ve sayfada sorunlu işlemi doldurun.

10. **Dosya** > **Tüm oturumlarını**Kaydet > 'e gidin.

Daha fazla bilgi için bkz. [Fiddler ile çalışmaya](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)başlama.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN forumu](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)



