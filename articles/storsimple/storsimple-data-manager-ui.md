---
title: Microsoft Azure StorSimple Veri Yöneticisi Kullanıcı UI
description: StorSimple Data Manager hizmeti Kullanıcı UI'nin nasıl kullanılacağını açıklar
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: 85be49ad88ac62d90235c3da6b89b0da6a11487c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78933761"
---
# <a name="manage-the-storsimple-data-manager-service-in-azure-portal"></a>Azure portalında StorSimple Data Manager hizmetini yönetme

Bu makalede, StorSimple 8000 serisi cihazlarda bulunan verileri dönüştürmek için StorSimple Data Manager UI'yi nasıl kullanabileceğiniz açıklanmaktadır. Dönüştürülmüş veriler daha sonra Azure Medya Hizmetleri, Azure HDInsight, Azure Machine Learning ve Azure Bilişsel Arama gibi diğer Azure hizmetleri tarafından tüketilebilir.


## <a name="use-storsimple-data-transformation"></a>StorSimple Veri Dönüşümü'ni kullanma

StorSimple Veri Yöneticisi, veri dönüşümünde anlık olarak bulunan kaynaktır. Veri Dönüşümü hizmeti, verileri StorSimple biçiminden yerel biçime blobs veya Azure Dosyaları'nda dönüştürmenize olanak tanır. StorSimple yerel biçim verilerini dönüştürmek için, StorSimple 8000 serisi aygıtınız hakkındaki ayrıntıları ve dönüştürmek istediğiniz ilgi çekici verileri belirtmeniz gerekir.

### <a name="create-a-storsimple-data-manager-service"></a>StorSimple Data Manager hizmeti oluşturma

StorSimple Data Manager hizmeti oluşturmak için aşağıdaki adımları gerçekleştirin.

1. [Azure portalda](https://portal.azure.com/) oturum açmak için Microsoft hesabınızın kimlik bilgilerini kullanın.

2. **+ Kaynak oluştur'u** tıklatın ve StorSimple Data Manager'ı arayın.

    ![StorSimple Data Manager hizmeti oluşturma 1](./media/storsimple-data-manager-ui/create-service-1.png)

3. StorSimple Data Manager'ı tıklatın ve sonra **Oluştur'u**tıklatın.
    
    ![StorSimple Data Manager hizmeti oluşturma 2](./media/storsimple-data-manager-ui/create-service-3.png)

3. Yeni hizmet için aşağıdakileri belirtin:

   1. StorSimple Veri Yöneticiniz için benzersiz bir **Hizmet adı** sağlayın. Hizmetinizi tanımlayabilmek için kullanılan kolay bir addır. Ad harf, rakam ve tirelerden oluşan 3-24 karakter arası uzunlukta olabilir. Ad bir harf veya sayıyla başlamalı ve bitmelidir.

   2. Açılan listeden bir **Abonelik** seçin. Abonelik fatura hesabınıza bağlıdır. Yalnızca bir aboneliğiniz varsa, bu alan otomatik olarak doldurulur (ve seçilemez).

   3. Varolan bir kaynak grubu seçin veya yeni bir grup oluşturun. Daha fazla bilgi edinmek için bkz. [Azure kaynak grupları](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

   4. Depolama hesaplarınızı ve StorSimple Veri Yöneticisi hizmetinizi barındıran hizmetinizin **Konumunu** belirtin. StorSimple Device Manager hizmetiniz, Veri Yöneticisi hizmetiniz ve ilişkili depolama hesabınızın tümü desteklenen bölgelerde olmalıdır.
    
   5. Panonuzda bu hizmete bağlantı almak için **panoya Sabitle'yi**seçin.
    
   6. **Oluştur'u**tıklatın.

      ![StorSimple Data Manager hizmeti oluşturma 3](./media/storsimple-data-manager-ui/create-service-4.png)

Hizmetin oluşturulması birkaç dakika sürer. Hizmet başarıyla oluşturulduktan ve yeni hizmet görüntülendikten sonra bir bildirim görürsünüz.

### <a name="create-a-data-transformation-job-definition"></a>Veri dönüştürme iş tanımı oluşturma

StorSimple Data Manager hizmeti nde, bir veri dönüştürme iş tanımı oluşturmanız gerekir. İş tanımı, storsimple verilerinin ayrıntılarını yerel biçimde bir depolama hesabına taşımakla ilgilendiğiniz ayrıntıları belirtir. Bir iş tanımı oluşturduğunuzda, bu işi farklı çalışma zamanı ayarlarıyla yeniden çalıştırabilirsiniz.

İş tanımı oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Oluşturduğunuz hizmete gidin. Yönetim **> İş tanımlarına**gidin.

2. + **İş tanımını**tıklatın.

    ![+İş Tanımı'nı tıklatın](./media/storsimple-data-manager-ui/create-job-definition-1.png)

3. İş tanımınız için bir ad sağlayın. Ad 3 ile 63 karakter arasında olabilir. Ad büyük harf ve küçük harfler, sayılar ve tireler içerebilir.

4. İşinizin çalıştığı yeri belirtin. Bu konum, hizmetin dağıtıldığı konumdan farklı olabilir.

5. Kaynak veri deposunu belirtmek için **Kaynak'ı** tıklatın.

    ![Kaynak veri repo'su yapılandırma](./media/storsimple-data-manager-ui/create-job-definition-2.png)

6. Bu yeni bir Veri Yöneticisi hizmeti olduğundan, hiçbir veri deposu yapılandırılmamıştır. **Veri kaynağını yapılandırıldığında,** StorSimple 8000 serisi cihazınızın ayrıntılarını ve ilgi çekici verileri belirtin.

   StorSimple Device Manager'ınızı veri deposu olarak eklemek için veri deposu açılır düşüşünde **yeni ekle'yi** tıklatın ve ardından **Veri Deposu Ekle'yi**tıklatın.

    ![Yeni veri reposu ekleme](./media/storsimple-data-manager-ui/create-job-definition-3.png)
  
   1. Veri deposu türü olarak **StorSimple 8000 serisi Yöneticisi'ni** seçin.
    
   2. Kaynak veri deponuz için uygun bir ad girin.
    
   3. Açılan listeden StorSimple Device Manager hizmetinizle ilişkili bir abonelik seçin.
    
   4. **Kaynak**için StorSimple Aygıt Yöneticisi'nin adını sağlayın.

   5. StorSimple Device Manager hizmeti için **Hizmet veri şifreleme** anahtarını girin. 

      ![Kaynak veri repo 1 yapılandırma](./media/storsimple-data-manager-ui/create-job-definition-4.png)

      Bittiğinde **Tamam'ı** tıklatın. Bu, veri deponuzu kaydeder. Bu StorSimple Aygıt Yöneticisi'ni bu parametreleri tekrar girmeden diğer iş tanımlarında yeniden kullanın. Yeni oluşturulan kaynak veri deposunun açılır açılır da gösterilmesi için **Tamam'ı** tıklattıktan sonra birkaç saniye sürer.

7. **Veri deposu**için açılan listeden oluşturduğunuz veri deposunu seçin. 

   1. İlgi çekici verileri içeren StorSimple 8000 serisi aygıtın adını girin.

   2. İlgi nizi çeken verilerinizi içeren StorSimple aygıtında bulunan birimin adını belirtin.

   3. **Filtre** alt bölümünde, _\MyRootDirectory\Data_ biçiminde ilgi nizi çeken verilerinizi içeren kök dizinini girin. _\C:\Data_ gibi sürücü harfleri desteklenmez. Ayrıca buraya herhangi bir dosya filtresi ekleyebilirsiniz.

   4. Veri dönüştürme hizmeti yalnızca Azure'a itilen verilerin en son anlık görüntüsünde çalışır.

   5. **Tamam**'a tıklayın.

      ![Kaynak veri repo 2 yapılandırma](./media/storsimple-data-manager-ui/create-job-definition-8.png)

8. Ardından, hedef veri deposunun yapılandırılması gerekir. Dosyaları bu hesaptaki blob'lara koymak için depolama hesapları seçin. Açılan açılır durumda, **yeni ekle'yi** seçin ve **ardından ayarları yapılandırın' ı**seçin.

9. Eklemek istediğiniz hedef deposu türünü ve depoyla ilişkili diğer parametreleri seçin.

    Bir Depolama hesabı türü hedefi seçerseniz, bir dost adı, abonelik (hizmet veya diğerininkiyle aynı seçimi seçin) ve bir depolama hesabı belirtebilirsiniz.
        ![Hedef veri repo 1 yapılandırma](./media/storsimple-data-manager-ui/create-job-definition-10.png)

    İş çalıştığında bir depolama sırası oluşturulur. Bu kuyruk, hazır olduğunda dönüştürülen bloblar hakkında iletilerle doldurulur. Bu kuyruğun adı, iş tanımının adıyla aynıdır.
    
10. Veri deposunu ekledikten sonra birkaç dakika bekleyin.
    
    1. **Hedef hesap adındaki**açılır listeden hedef olarak oluşturduğunuz depoyu seçin.

    2. Depolama türünü blobs veya dosya olarak seçin. Dönüştürülen verilerin bulunduğu depolama kapsayıcısının adını belirtin. **Tamam**'a tıklayın.

        ![Hedef veri repo depolama hesabını yapılandırma](./media/storsimple-data-manager-ui/create-job-definition-16.png)

11. Ayrıca, işi çalıştırmadan önce iş süresi tahmini sunma seçeneğini de işaretleyebilirsiniz. İş tanımını oluşturmak için **Tamam'ı** tıklatın. İş tanımınız tamamlandı. Bu iş tanımını, farklı çalışma zamanı ayarlarıyla UI üzerinden birden çok kez kullanabilirsiniz.

    ![Tam iş tanımı](./media/storsimple-data-manager-ui/create-job-definition-13.png)

    Yeni oluşturulan iş tanımı, bu hizmetin iş tanımları listesine eklenir.

### <a name="run-the-job-definition"></a>İş tanımını çalıştırma

Verileri StorSimple'dan iş tanımında belirttiğiniz depolama hesabına taşımanız gerektiğinde, verileri çalıştırmanız gerekir. Çalışma zamanında, bazı parametreler farklı şekilde belirtilebilir. Adımlar şu şekildedir:

1. StorSimple Data Manager hizmetinizi seçin ve **Yönetim > İş tanımlarına**gidin. Çalıştırmak istediğiniz iş tanımını seçin ve tıklatın.
     
     ![İş çalıştırmayı başlat 1](./media/storsimple-data-manager-ui/start-job-run1.png)

2. **Şimdi Çalıştır'ı**tıklatın.
     
     ![İş çalıştırmayı başlat 2](./media/storsimple-data-manager-ui/start-job-run2.png)

3. Bu iş çalışması için değiştirmek isteyebileceğin ayarları değiştirmek için **Çalıştır ayarlarını** tıklatın. **Tamam'ı** tıklatın ve sonra işinizi başlatmak için **Çalıştır'ı** tıklatın.

    ![İş çalıştırmayı başlat 3](./media/storsimple-data-manager-ui/start-job-run3.png)

4. Bu işi izlemek için StorSimple Veri Yöneticinizdeki **İşler'e** gidin. **İşler** bıtır'daki izlemenin yanı sıra, bir dosya StorSimple'dan depolama hesabına her taşındığında iletinin eklendiği depolama kuyruğunu da dinleyebilirsiniz.

    ![İş çalıştırmayı başlat 4](./media/storsimple-data-manager-ui/start-job-run4.png)

### <a name="view-logs-after-job-completion"></a>İş tamamlandıktan sonra günlükleri görüntüleme

Bir iş tamamlandıktan sonra, işin durumunu görüntüleyebilirsiniz. İş durumu **Başarılı**Olabilir , **Kısmen Başarılı** ve **Başarısız oldu.** Başarıyla kopyalanan dosyaların ve kopyalanamayan dosyaların listesini görüntüleyebilirsiniz. Bu listeler, hedef depolama hesabınızda **"storsimple-data-manager-joblogs"** adı verilen bir kapsayıcıda kullanılabilir. Bu kapsayıcının içinde, iş tanımınızla aynı ada sahip bir klasör alabilirsiniz. Bunun içinde, listelerinizi içerecek her iş çalışması için bir klasör oluşturulur. Bu klasörün adı, iş ayrıntıları sayfasından alabileceğiniz işin GUID'i olacaktır. Alternatif olarak, çoğu durumda işler sayfasının içindeki kopya günlükleri için bir bağlantı görürsünüz.
Bu klasörde göreceğiniz 2 csv dosyaları kümesi vardır. **Kopyalanan dosya listesi** yle başlayan tüm dosyalar... başarıyla kopyalanan dosyaların listesini içerir. **Failedfilelist** ile başlayan tüm dosyalar... bir hata iletisiyle birlikte kopyalanamayan dosyalar içerir.


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Data Manager işlerini başlatmak için .NET SDK'yı kullanın.](storsimple-data-manager-dotnet-jobs.md)
