---
title: Düzenli aralıklarla yedekleme yapılandırmasını anlama
description: Uygulama verilerinizin periyodik veri yedeklemesini etkinleştirmek için Service Fabric'in periyodik yedekleme ve geri yükleme özelliğini kullanın.
author: hrushib
ms.topic: article
ms.date: 2/01/2019
ms.author: hrushib
ms.openlocfilehash: 34c6495e094a1160f6ac75b9f098934d5cbce967
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610157"
---
# <a name="understanding-periodic-backup-configuration-in-azure-service-fabric"></a>Azure Hizmet Kumaşı'nda periyodik yedekleme yapılandırmalarını anlama

Güvenilir durum lu hizmetlerin izinin veya Güvenilir Aktörlerin periyodik yedeklemesini yapılandırmak aşağıdaki adımlardan oluşur:

1. **Yedekleme ilkelerinin oluşturulması**: Bu adımda, gereksinimlere bağlı olarak bir veya daha fazla yedekleme ilkeleri oluşturulur.

2. **Yedeklemeyi etkinleştirme**: Bu adımda, **Adım 1'de** oluşturulan yedekleme ilkelerini gerekli varlıklara, _Uygulamayla,_ _Hizmete_veya _Bölüme_ilişkilendiriniz.

## <a name="create-backup-policy"></a>Yedekleme İlkesi Oluştur

Yedekleme ilkesi aşağıdaki yapılandırmalardan oluşur:

* **Veri kaybında otomatik geri yükleme**: Bölümün bir veri kaybı olayı yla karşılaşması durumunda, kullanılabilir en son yedeklemeyi kullanarak otomatik olarak geri yüklemeyi tetikleyip tetiklemeyeceğini belirtir.

* **Maksimum artımlı yedeklemeler**: İki tam yedekleme arasında alınacak maksimum artımlı yedekleme sayısını tanımlar. Maksimum artımlı yedeklemeler üst sınırı belirtir. Belirtilen sayıda artımlı yedekleme aşağıdaki koşullardan birinde tamamlanmadan önce tam bir yedekleme alınabilir

    1. Yineleme birincil hale geldiğinden beri tam bir yedekleme almamıştır.

    2. Son yedeklemeden bu yana bazı günlük kayıtları kesildi.

    3. Çoğaltma MaxAccumulatedBackupLogSizeInMB sınırını geçti.

* **Yedekleme çizelgesi**: Periyodik yedeklemelerin alınabilmek için zaman veya sıklık. Yedeklemeleri belirli aralıklarla veya belirli bir zamanda günlük/haftalık olarak yinelenecek şekilde zamanlayabilirsiniz.

    1. **Frekans tabanlı yedekleme çizelgesi**: İhtiyaç sabit aralıklarla veri yedeklemesi yapılacaksa, bu zamanlama türü kullanılmalıdır. Art arda iki yedekleme arasında istenen zaman aralığı ISO8601 biçimi kullanılarak tanımlanır. Sıklık tabanlı yedekleme zamanlaması dakikaya kadar aralık çözümlemesi destekler.
        ```json
        {
            "ScheduleKind": "FrequencyBased",
            "Interval": "PT10M"
        }
        ```

    2. **Zaman tabanlı yedekleme çizelgesi**: Gereksinim günün veya haftanın belirli saatlerinde veri yedeklemesi almak için bu zamanlama türü kullanılmalıdır. Zamanlama frekans türü günlük veya haftalık olabilir.
        1. ** _Günlük_ Saat tabanlı yedekleme çizelgesi**: Günün belirli saatlerinde veri yedeklemesi için ihtiyaç id'i varsa, bu zamanlama türü kullanılmalıdır. Bunu belirtmek için, `ScheduleFrequencyType` _Günlük_olarak ayarlayın; ve `RunTimes` ISO8601 formatında gün içinde istenilen zaman listesine ayarlanmış, zaman ile birlikte belirtilen tarih yoksayılır. Örneğin, `0001-01-01T18:00:00` tarih bölüm _0001-01-01_göz ardı ederek, her gün _18:00'i_ temsil eder. Aşağıdaki örnek, günlük yedeklemeyi her gün _09:00_ ve _18:00'de_ tetikleyen yapılandırmayı göstermektedir.

            ```json
            {
                "ScheduleKind": "TimeBased",
                "ScheduleFrequencyType": "Daily",
                "RunTimes": [
                  "0001-01-01T09:00:00Z",
                  "0001-01-01T18:00:00Z"
                ]
            }
            ```

        2. ** _Haftalık_ Zaman tabanlı yedekleme çizelgesi**: Günün belirli saatlerinde veri yedeklemesi için ihtiyaç id'i varsa, bu zamanlama türü kullanılmalıdır. Bunu belirtmek için `ScheduleFrequencyType` Haftalık _olarak_ayarlayın; yedeklemenin tetiklemesi gereken ve ISO8601 `RunTimes` formatında gün içinde istenen zamanın listesine ayarlandığı bir haftadaki günlerin listesine ayarlanır, `RunDays` saatle birlikte belirtilen tarih göz ardı edilir. Periyodik yedeklemenin tetiklenecek bir haftanın günlerinin listesi. Aşağıdaki örnek, Pazartesi'den Cuma'ya saat _09:00_ ve _18:00'de_ günlük yedeklemeyi tetikleyen yapılandırmayı göstermektedir.

            ```json
            {
                "ScheduleKind": "TimeBased",
                "ScheduleFrequencyType": "Weekly",
                "RunDays": [
                   "Monday",
                   "Tuesday",
                   "Wednesday",
                   "Thursday",
                   "Friday"
                ],
                "RunTimes": [
                  "0001-01-01T09:00:00Z",
                  "0001-01-01T18:00:00Z"
                ]
            }
            ```

* **Yedekleme depolama**: Yedekleme yüklemek için konumu belirtir. Depolama, Azure blob deposu veya dosya paylaşımı olabilir.
    1. **Azure blob deposu**: Oluşturulan yedeklemeleri Azure'da depolama ihtiyacı olduğunda bu depolama türü seçilmelidir. Hem _bağımsız_ hem de _Azure tabanlı_ kümeler bu depolama türünü kullanabilir. Bu depolama türü için açıklama bağlantı dizesi ve yedekleme yüklenmesi gereken kapsayıcının adı gerektirir. Belirtilen ada sahip kapsayıcı kullanılamıyorsa, yedekleme nin yüklenmesi sırasında oluşturulur.
        ```json
        {
            "StorageKind": "AzureBlobStore",
            "FriendlyName": "Azure_storagesample",
            "ConnectionString": "<Put your Azure blob store connection string here>",
            "ContainerName": "BackupContainer"
        }
        ```

    2. **Dosya paylaşımı**: Bu depolama türü, şirket içinde veri yedekleme depolamaihtiyacı olduğunda _bağımsız_ kümeler için seçilmelidir. Bu depolama türü için açıklama, yedeklemelerin yüklenmesi gereken dosya paylaşım yolunu gerektirir. Dosya paylaşımına erişim aşağıdaki seçeneklerden biri kullanılarak yapılandırılabilir
        1. Dosya paylaşımına erişimin Hizmet Kumaşı kümesine ait tüm bilgisayarlara sağlandığı _Tümleşik Windows Kimlik Doğrulama._ Bu durumda, _dosya paylaşımı_ tabanlı yedekleme depolama yapılandırmak için aşağıdaki alanları ayarlayın.

            ```json
            {
                "StorageKind": "FileShare",
                "FriendlyName": "Sample_FileShare",
                "Path": "\\\\StorageServer\\BackupStore"
            }
            ```

        2. Dosya paylaşımına erişimin belirli kullanıcılara sağlandığı _kullanıcı adı ve parolayı kullanarak dosya paylaşımını koruma._ Dosya paylaşımı depolama belirtimi, kimlik doğrulamanın birincil kullanıcı adı ve birincil parolayla başarısız olması durumunda geri dönüş kimlik bilgilerini sağlamak için ikincil kullanıcı adı ve ikincil parola belirtme olanağı da sağlar. Bu durumda, _dosya paylaşımı_ tabanlı yedekleme depolama yapılandırmak için aşağıdaki alanları ayarlayın.

            ```json
            {
                "StorageKind": "FileShare",
                "FriendlyName": "Sample_FileShare",
                "Path": "\\\\StorageServer\\BackupStore",
                "PrimaryUserName": "backupaccount",
                "PrimaryPassword": "<Password for backupaccount>",
                "SecondaryUserName": "backupaccount2",
                "SecondaryPassword": "<Password for backupaccount2>"
            }
            ```

> [!NOTE]
> Depolama güvenilirliğinin yedekleme verilerinin güvenilirlik gereksinimlerini karşıladığından veya aştığından emin olun.
>

* **Bekletme İlkesi**: Yapılandırılan depolama alanında yedekleme tutmak için ilke belirtir. Yalnızca Temel Bekletme İlkesi desteklenir.
    1. **Temel Bekletme İlkesi**: Bu bekletme ilkesi, artık gerekli olmayan yedekleme dosyalarını kaldırarak en iyi depolama kullanımını sağlamaya olanak tanır. `RetentionDuration`yedeklemelerin depoda tutulması gereken süreyi ayarlamak için belirtilebilir. `MinimumNumberOfBackups`belirtilen yedekleme sayısının her zaman . `RetentionDuration` Aşağıdaki örnekte, yedeklemeleri _10_ gün boyunca tutmak için yapılandırma gösteriş ve yedek sayısının _20'nin_altına inmesine izin vermez.

        ```json
        {
            "RetentionPolicyType": "Basic",
            "RetentionDuration" : "P10D",
            "MinimumNumberOfBackups": 20
        }
        ```

## <a name="enable-periodic-backup"></a>Periyodik yedeklemeyi etkinleştirme
Veri yedekleme gereksinimlerini karşılamak için yedekleme ilkesi ni tanımladıktan sonra, yedekleme ilkesi uygun bir _uygulama_veya _hizmet_veya bir _bölüm_ile ilişkilendirilmelidir.

### <a name="hierarchical-propagation-of-backup-policy"></a>Yedekleme ilkesinin hiyerarşik yayılımı
Hizmet Kumaşında, uygulama, hizmet ve bölümler arasındaki [ilişki, Uygulama modelinde](./service-fabric-application-model.md)açıklandığı gibi hiyerarşiktir. Yedekleme ilkesi bir _uygulama,_ _hizmet_veya hiyerarşideki bir _bölümle_ ilişkilendirilebilir. Yedekleme ilkesi hiyerarşik olarak bir sonraki düzeye yayılır. Bir _uygulamayla_oluşturulan ve ilişkili tek bir yedekleme ilkesi olduğunu varsayarsak, tüm _Güvenilir durumlu hizmetlere_ ait tüm durum lu bölümler ve _uygulamanın_ _Güvenilir Aktörleri_ yedekleme ilkesi kullanılarak yedeklenir. Veya yedekleme ilkesi _Güvenilir bir durum hizmetiyle_ilişkiliyse, tüm bölümleri yedekleme ilkesi kullanılarak yedeklenir.

### <a name="overriding-backup-policy"></a>Yedekleme ilkesini geçersiz kılma
Daha yüksek frekans çizelgesi kullanarak veri yedeklemesi veya farklı bir depolama hesabına yedekleme yapılması gereken belirli hizmetler dışında, aynı yedekleme zamanlamasına sahip veri yedeklemesinin uygulamanın tüm hizmetleri için gerekli olduğu bir senaryo olabilir veya Fileshare. Bu tür senaryoları gidermek için yedekleme geri yükleme hizmeti, hizmet ve bölüm kapsamında yayılan ilkeyi geçersiz kılmak için bir tesis sağlar. Yedekleme ilkesi _hizmet_ veya _bölüm_ilişkili olduğunda, varsa, yayılan yedekleme ilkesi geçersiz kılar.

### <a name="example"></a>Örnek

Bu _örnekte, MyApp_A_ ve _MyApp_B_olmak üzere iki uygulamaiçeren kurulum kullanır. Uygulama _MyApp_A_ iki Güvenilir Stateful hizmetleri içerir, _SvcA1_ & _SvcA3_, ve bir Güvenilir Aktör hizmet, _ActorA2_. _SvcA1_ üç bölüm içerirken _ActorA2_ ve _SvcA3_ her biri iki bölüm içerir.  Uygulama _MyApp_B_ üç Güvenilir Stateful hizmetleri içerir, _SvcB1_, _SvcB2_, ve _SvcB3_. _SvcB1_ ve _SvcB2_ her biri iki bölüm içerirken, _SvcB3_ üç bölüm içerir.

Bu uygulamaların veri yedekleme gereksinimlerinin aşağıdaki gibi olduğunu varsayalım

1. MyApp_A
    1. Uygulamaya ait tüm _Güvenilir Stateful hizmetlerinin_ ve _Güvenilir Aktörlerin_ tüm bölümleri için günlük veri yedeklemesini oluşturun. Yedekleme verilerini konuma _yükleyin BackupStore1_.

    2. Hizmetlerden biri olan _SvcA3,_ her saat başı veri yedeklemegerektirir.

    3. _Bölüm SvcA1_P2_ veri boyutu beklenenden daha fazladır ve yedekleme verileri farklı depolama konumu _BackupStore2_depolanmalıdır.

2. MyApp_B
    1. _SvcB1_ hizmetinin tüm bölümleri için her Pazar saat 08:00'de veri yedeklemesi oluşturun. Yedekleme verilerini konuma _yükleyin BackupStore1_.

    2. Bölüm _SvcB2_P1_için her gün 08:00'de veri yedeklemesi oluşturun. Yedekleme verilerini konuma _yükleyin BackupStore1_.

Bu veri yedekleme gereksinimlerini gidermek için, BP_5 BP_1 yedekleme ilkeleri oluşturulur ve yedekleme aşağıdaki gibi etkinleştirilir.
1. MyApp_A
    1. Frekansın 24 Saat olarak ayarlandığı sıklık tabanlı yedekleme zamanlaması yla _BP_1_yedekleme ilkesi oluşturun. ve yedekleme depolama depolama konumu _BackupStore1_kullanmak için yapılandırılmıştır. [Uygulama Yedekleme API'sini Etkinleştir'i](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enableapplicationbackup) kullanarak Uygulama _MyApp_A_ için bu ilkeyi etkinleştirin. Bu eylem, _Güvenilir Durum hizmetleri_ ve uygulama _MyApp_A_ait _Güvenilir Aktörler_ tüm bölümleri için yedekleme ilkesi _BP_1_ kullanarak veri yedekleme sağlar.

    2. Frekansın 1 Saat olarak ayarlandığı sıklık tabanlı yedekleme zamanlaması yla _BP_2_yedekleme ilkesi oluşturun. ve yedekleme depolama depolama konumu _BackupStore1_kullanmak için yapılandırılmıştır. Hizmet Yedekleme API [etkinleştirin](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enableservicebackup) kullanarak hizmet _SvcA3_ için bu ilkeyi etkinleştirin. Bu eylem, bu bölümler için yedekleme _ilkesi_ _BP_2_ kullanarak veri yedeklemeye yol açan _hizmet SvcA3'ün_ tüm bölümleri için açıkça etkinleştirilmiş yedekleme ilkesi _BP_2_ tarafından BP_1 yayılan ilkeyi geçersiz kılar.

    3. Frekansın 24 Saat olarak ayarlandığı sıklık tabanlı yedekleme zamanlaması yla _BP_3_yedekleme ilkesi oluşturun. ve yedekleme depolama depolama konumu _BackupStore2_kullanmak için yapılandırılmıştır. Bu ilkeyi, Bölüm Yedekleme _API'sini_ Etkinleştir'i kullanarak SvcA1_P2 [bölümleme](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enablepartitionbackup) için etkinleştirin. Bu eylem, bölüm _SvcA1_P2_için açıkça etkinleştirilmiş yedekleme ilkesi _BP_3_ _BP_1_ yayılan ilkeyi geçersiz kılar.

2. MyApp_B
    1. Zamanlama sıklığı nın haftalık olarak ayarlandığı, çalışma günsayısının Pazar gününe ayarlandığı ve çalışma saatlerini 08:00 olarak ayarladığı zaman tabanlı yedekleme zamanlaması yla _BP_4_yedekleme ilkesi oluşturun. Yedekleme depolama depolama konumu _BackupStore1_kullanmak için yapılandırılmıştır. Hizmet Yedekleme API [etkinleştirin](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enableservicebackup) kullanarak hizmet _SvcB1_ için bu ilkeyi etkinleştirin. Bu eylem, _hizmet SvcB1_tüm bölümleri için yedekleme ilkesi _BP_4_ kullanarak veri yedekleme sağlar.

    2. Zamanlama sıklığı nın günlük olarak ayarlandığı ve çalışma saatinin 08:00 olarak ayarlandığı zaman tabanlı yedekleme zamanlaması yla _BP_5_yedekleme ilkesi oluşturun. Yedekleme depolama depolama konumu _BackupStore1_kullanmak için yapılandırılmıştır. Bu ilkeyi, [Bölüm Yedekleme](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-enablepartitionbackup) API'sini Etkinleştir'i kullanarak bölüm _SvcB2_P1_ için etkinleştirin. Bu _eylem,_ SvcB2_P1 için yedekleme ilkesi _BP_5_ kullanarak veri yedekleme sağlar.

Aşağıdaki diyagram, açıkça etkinleştirilmiş yedekleme ilkelerini ve yayılan yedekleme ilkelerini görüntüler.

![Hizmet Kumaş Uygulama Hiyerarşisi][0]

## <a name="disable-backup"></a>Yedeklemeyi devre dışı
Verileri yedeklemeye gerek olmadığında yedekleme ilkeleri devre dışı tutulabilir. Bir _uygulamada_ etkinleştirilen yedekleme ilkesi yalnızca Uygulama Yedekleme API'sını [devre dışı bırakma](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-disableapplicationbackup) kullanılarak aynı _uygulamada_ devre dışı edilebilir, bir _hizmette_ etkinleştirilen Yedekleme ilkesi, Hizmet Yedekleme API'sını devre [dışı bırakma](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-disableservicebackup) kullanarak aynı _hizmette_ devre dışı edilebilir ve bir _bölümde_ etkinleştirilen Yedekleme ilkesi, [Devre Dışı Bırakma Bölümü Yedekleme](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-disablepartitionbackup) API'sını kullanarak aynı _bölümde_ devre dışı edilebilir.

* Bir _uygulama_ için yedekleme ilkesinin devre dışı bırakılması, yedekleme ilkesinin Güvenilir Durum lu hizmet bölümlerine veya Güvenilir Aktör bölümlerine yayılması sonucunda gerçekleşen tüm periyodik veri yedeklemelerini durdurur.

* Bir _hizmet_ için yedekleme ilkesinin devre dışı bırakılması, bu yedekleme ilkesinin _hizmetin_bölümlerine yayılması sonucunda gerçekleşen tüm periyodik veri yedeklemelerini durdurur.

* Bir _bölüm_ için yedekleme ilkesini devre dışı bırakmak, bölümdeki yedekleme ilkesi nedeniyle gerçekleşen tüm periyodik veri yedeklemesini durdurur.

* Bir varlık (uygulama/hizmet/bölüm) için yedeklemedevre `CleanBackup` dışı bırakılırken, yapılandırılmış depolama daki tüm yedeklemeleri silmek için _doğru_ olarak ayarlanabilir.
    ```json
    {
        "CleanBackup": true 
    }
    ```

## <a name="suspend--resume-backup"></a>Yedeklemeyi askıya & devam edin
Bazı durum verilerin periyodik yedekleme geçici askıya talep edebilir. Bu durumda, gereksinime bağlı olarak, askıya yedekleme API bir _Uygulama,_ _Hizmet_veya _Bölüm_kullanılabilir. Periyodik yedekleme süspansiyonu, uygulandığı noktadan itibaren uygulama hiyerarşisinin alt ağacı üzerinden geçişli dir. 

* [Uygulama YedeklemeYI Askıya Al](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-suspendapplicationbackup) API'sını kullanarak bir _Uygulamada_ askıya alma uygulandığında, bu uygulama kapsamındaki tüm hizmetler ve bölümler verilerin periyodik olarak yedeklemi için askıya alınır.

* [Hizmet Yedeklemeyi Askıya Alma](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-suspendservicebackup) API'si kullanılarak bir _Hizmete_ askıya alma uygulandığında, bu hizmetin altındaki tüm bölümler verilerin periyodik olarak yedeklemi için askıya alınır.

* Askıya [Alma Bölümü Yedekleme](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-suspendpartitionbackup) API'si kullanılarak bir _Bölüme_ süspansiyon uygulandığında, bu hizmetin altındaki bölümleri askıya alan bölümler, verilerin periyodik olarak yedeklemi için askıya alınır.

Askıya alma gereksinimi sona erdikten sonra, periyodik veri yedeklemesi ilgili özgeçmiş yedekleme API'si kullanılarak geri yüklenebilir. Periyodik yedekleme, askıya alınan aynı _uygulama,_ _hizmet_veya _bölüm_ olarak sürdürülmelidir.

* Bir _Uygulamada_süspansiyon uygulandıysa, [Özgeçmiş Uygulaması Yedekleme](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-resumeapplicationbackup) API'si kullanılarak devam edilmelidir. 

* Bir _Hizmette_süspansiyon uygulandıysa, Devam Hizmeti [Yedekleme](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-resumeservicebackup) API'si kullanılarak devam edilmelidir.

* Bir _Bölüm'de_süspansiyon uygulandıysa, [Devam Bölümü Yedekleme](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-resumepartitionbackup) API'si kullanılarak devam edilmelidir.

### <a name="difference-between-suspend-and-disable-backups"></a>Askıya Alma ve Yedeklemeleri Devre Dışı Arası arasındaki fark
Yedekleme, belirli bir uygulama, hizmet veya bölüm için artık gerekli olmadığında yedeklemeyi devre dışı kalmalıdır. Bir tüm varolan yedekleme de silinir anlamına gelir doğru olması için temiz yedekleme parametresi ile birlikte yedekleme isteği devre dışı bırakılabilirsiniz. Ancak, askıya alma, yerel diskin dolması veya bilinen ağ sorunu nedeniyle yedekleme yüklemenin başarısız olması gibi yedeklemeleri geçici olarak kapatmak istediği senaryolarda kullanılacaktır. 

Devre dışı bırakma yalnızca daha önce yedekleme için etkinleştirilen bir düzeyde çağrılabilir ancak süspansiyon şu anda yedekleme için etkinleştirilen herhangi bir düzeyde doğrudan veya devralma/ hiyerarşi yoluyla uygulanabilir. Örneğin, bir uygulama düzeyinde yedekleme etkinse, yalnızca uygulama düzeyinde devre dışı bırakma çağrılabilir, ancak askıya alma uygulama, bu uygulama nın altındaki herhangi bir hizmet veya bölüm çağrılabilir. 

## <a name="auto-restore-on-data-loss"></a>Veri kaybında otomatik geri yükleme
Hizmet bölümü beklenmeyen hatalar nedeniyle veri kaybedebilir. Örneğin, bir bölüm için üç yinelemeden ikisinin diski (birincil yineleme dahil) bozuk veya silinir.

Service Fabric, bölümün veri kaybı içinde olduğunu algıladığında, bölüm üzerindeki `OnDataLossAsync` arabirim yöntemini çağırır ve bölümün veri kaybından çıkmak için gerekli eylemi almasını bekler. Bu durumda, bölümdeki etkili yedekleme ilkesi `AutoRestoreOnDataLoss` bayrak `true` olarak ayarlanmışsa, geri yükleme bu bölüm için kullanılabilir en son yedekleme kullanılarak otomatik olarak tetiklenir.

## <a name="get-backup-configuration"></a>Yedekleme yapılandırması alın
Bir _uygulama,_ _hizmet_ve _bölüm_ kapsamında yedekleme yapılandırma bilgilerini almak için ayrı API'ler kullanılabilir hale getirilir. [Uygulama Yedekleme Yapılandırma Bilgileri alın](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getapplicationbackupconfigurationinfo), Hizmet Yedekleme Yapılandırma Bilgileri [alın](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getservicebackupconfigurationinfo)ve Bölüm Yedekleme Yapılandırma [Bilgileri alın](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupconfigurationinfo) bu API'ler sırasıyla vardır. Esas olarak, bu API'ler geçerli yedekleme ilkesini, yedekleme ilkesinin uygulandığı kapsamı ve yedekleme süspansiyon ayrıntılarını döndürer. Aşağıda bu API'lerin döndürülen sonuçları hakkında kısa bir açıklama yer almıştır.

- Uygulama yedekleme yapılandırma bilgileri: uygulamada uygulanan yedekleme ilkesinin ayrıntılarını ve uygulamaya ait hizmetlerde ve bölümlerde aşırı basan tüm ilkeleri sağlar. Ayrıca uygulama ve hizmetleri için süspansiyon bilgileri ve bölümleri içerir.

- Hizmet yedekleme yapılandırma bilgileri: hizmetteki etkili yedekleme ilkesinin ayrıntılarını ve bu iipolitikasın uygulandığı kapsamı ve bölümlerindeki tüm aşırı basmış ilkeleri sağlar. Ayrıca hizmet ve bölümleri için süspansiyon bilgilerini içerir.

- Bölüm yedekleme yapılandırma bilgileri: bölümlemede etkili yedekleme ilkesinin ayrıntılarını ve bu iidiliğin uygulandığı kapsamı sağlar. Ayrıca bölümler için süspansiyon bilgilerini içerir.

## <a name="list-available-backups"></a>Kullanılabilir yedeklemeleri listele

Kullanılabilir yedeklemeler Yedek Liste API'si Al kullanılarak listelenebilir. API çağrısının sonucu, geçerli yedekleme ilkesinde yapılandırılan yedekleme deposunda bulunan tüm yedeklemelerle ilgili yedekleme bilgileri öğelerini içerir. Bu API'nin farklı türevleri, bir uygulamaya, hizmete veya bölüme ait kullanılabilir yedeklemeleri listelemek için sağlanır. Bu API'ler, geçerli tüm bölümlerin _mevcut en son_ yedeklemesini almayı veya başlangıç tarihi ve bitiş _tarihine_ göre yedeklemelerin filtrelesini desteklemeyi destekler. _end date_

Bu API'ler, _MaxResults_ parametresi sıfır olmayan pozitif tamsayı olarak ayarlandığında, API maksimum _MaxResults_ yedekleme bilgi öğelerini döndürdüğünde sonuçların paginasyonuna da destek verir. Durumda, _MaxResults_ değerinden daha fazla yedek bilgi öğesi kullanılabilir, sonra bir devamı belirteci döndürülür. Geçerli devamı belirteç parametresi sonraki sonuç kümesini almak için kullanılabilir. Geçerli devam belirteç değeri API'nin bir sonraki çağrısına geçirildiğinde, API sonraki sonuç kümesini döndürür. Kullanılabilir tüm sonuçlar döndürüldüğünde yanıta devam belirteci dahil edilmez.

Aşağıda desteklenen türevleri hakkında kısa bilgiler yer alan bilgiler yer adatır.

- [Uygulama Yedekleme Listesini Al](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getapplicationbackuplist): Verilen Service Fabric uygulamasına ait her bölüm için kullanılabilen yedeklerin listesini verir.

- [Hizmet Yedekleme Listesi Al](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getservicebackuplist): Verilen Service Fabric hizmetine ait her bölüm için kullanılabilen yedeklerin listesini verir.
 
- [Bölüm Yedekleme Listesini Al](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackuplist): Belirtilen bölüm için kullanılabilen yedeklerin listesini verir.

## <a name="next-steps"></a>Sonraki adımlar
- [Yedekleme geri yükleme REST API başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/backup-policy-association-example.png
