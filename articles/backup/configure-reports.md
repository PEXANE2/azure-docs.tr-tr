---
title: Azure Backup raporlarını yapılandırma
description: Log Analytics ve Azure çalışma kitaplarını kullanarak Azure Yedekleme için raporları yapılandırma ve görüntüleme
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: bcc87deb19190149329369ca58f54b45b62b41fe
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617815"
---
# <a name="configure-azure-backup-reports"></a>Azure Backup raporlarını yapılandırma

Yedekleme yöneticileri için ortak bir gereksinim, uzun bir süreye yayılan verilere dayalı yedeklemeler hakkında öngörüler elde etmektir. Böyle bir çözüm için kullanım örnekleri şunlardır:

 - Tüketilen bulut depolamanın tahsisi ve tahmini.
 - Yedeklemelerin ve geri yüklemelerin denetlemi.
 - Farklı parçalılık düzeylerinde temel eğilimleri belirleme.

Azure Yedekleme bugün Azure Monitor [günlüklerini](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) ve [Azure çalışma kitaplarını](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks)kullanan bir raporlama çözümü sağlar. Bu kaynaklar, tüm yedekleme mülkünuzde yedeklemeleriniz hakkında zengin bilgiler edinmenize yardımcı olur. Bu makalede, Azure Yedekleme raporlarının nasıl yapılandırılabildiğini ve görüntülenebildiğini açıklanmaktadır.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

* Yedekleme raporları Azure VM'ler için desteklenir, Azure VM'lerde SQL, Azure VM'lerde SAP HANA/ASE, Microsoft Azure Kurtarma Hizmetleri (MARS) aracısı, Microsoft Azure Yedekleme Sunucusu (MABS) ve Sistem Merkezi Veri Koruma Yöneticisi (DPM).
* DPM iş yükleri için Yedekleme raporları DPM Sürüm 5.1.363.0 ve üzeri ve Aracı Sürüm 2.0.9127.0 ve üzeri için desteklenir.
* MABS iş yükleri için Yedekleme raporları MABS Sürüm 13.0.415.0 ve üzeri ve Aracı Sürüm 2.0.9170.0 ve üzeri için desteklenir.
* Yedekleme raporları, verileri kullanıcının erişebildiği bir Log Analytics çalışma alanına gönderildiği sürece tüm yedekleme öğeleri, kasalar, abonelikler ve bölgelerde görüntülenebilir. Bir dizi kasanın raporlarını görüntülemek için, yalnızca kasaların verilerini gönderdiği Log Analytics çalışma alanına okuyucu erişimine sahip olmanız gerekir. Tek tek kasalara girmeniz gerekmez.
* Müşterilerinizin aboneliklerine yetkili erişimi olan bir [Azure Deniz Feneri](https://docs.microsoft.com/azure/lighthouse/) kullanıcısıysanız, bu raporları tüm kiracılarınızda raporları görüntülemek için Azure Deniz Feneri ile kullanabilirsiniz.
* Günlük yedekleme işleri için veriler şu anda raporlarda görüntülenmez.

## <a name="get-started"></a>başlarken

Raporları kullanmaya başlamak için aşağıdaki adımları izleyin.

#### <a name="1-create-a-log-analytics-workspace-or-use-an-existing-one"></a>1. Bir Log Analytics çalışma alanı oluşturun veya mevcut bir çalışma alanı kullanın

Yedekleme raporlama verilerinizi depolamak için bir veya daha fazla Log Analytics çalışma alanı ayarlayın. Bu Log Analytics çalışma alanının oluşturulabileceği konum ve abonelik, kasalarınızın bulunduğu konum ve abonelikten bağımsızdır. 

Bir Günlük Analitiği çalışma alanı ayarlamak için azure [portalında Bir Günlük Analizi çalışma alanı oluşturma bölümüne](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)bakın.

Varsayılan olarak, Günlük Analizi çalışma alanındaki veriler 30 gün boyunca saklanır. Verileri daha uzun bir zaman ufku için görmek için, Log Analytics çalışma alanının bekletme süresini değiştirin. Bekletme süresini değiştirmek için Azure [Monitor günlükleriyle kullanımı ve maliyetleri yönet'e](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage)bakın.

#### <a name="2-configure-diagnostics-settings-for-your-vaults"></a>2. Kasalarınız için tanılama ayarlarını yapılandırın

Kurtarma Hizmetleri kasaları gibi Azure Kaynak Yöneticisi kaynakları, zamanlanmış işlemler ve kullanıcı tarafından tetiklenen işlemler hakkındaki bilgileri tanılama verileri olarak kaydeder. 

Kurtarma Hizmetleri kasanızın izleme bölümünde, **Tanılama ayarlarını** seçin ve Kurtarma Hizmetleri kasasının tanılama verileri için hedefi belirtin. Tanılama olaylarını kullanma hakkında daha fazla bilgi edinmek [için](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events)bkz.

![Tanılama ayarları bölmesi](./media/backup-azure-configure-backup-reports/resource-specific-blade.png)

Azure Yedekleme ayrıca, belirli bir kapsamdaki tüm kasalar için tanılama ayarlarının yapılandırmasını otomatikleştiren yerleşik bir Azure ilkesi de sağlar. Bu ilkenin nasıl kullanılacağını öğrenmek için [bkz.](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics)

> [!NOTE]
> Tanılamayı yapılandırdıktan sonra, ilk veri itme işleminin tamamlanması 24 saat kadar sürebilir. Veriler Log Analytics çalışma alanına akmaya başladıktan sonra, geçerli kısmi güne ait veriler raporlarda gösterilmediği için raporlardaki verileri hemen göremeyebilirsiniz. Daha fazla bilgi için bkz. [Yedekleme raporlarında kullanılan Sözleşmeler.](https://docs.microsoft.com/azure/backup/configure-reports#conventions-used-in-backup-reports) Log Analytics'e veri gönderecek şekilde kasalarınızı yapılandırdıktan iki gün sonra raporları görüntülemeye başlamanızı öneririz.

#### <a name="3-view-reports-in-the-azure-portal"></a>3. Azure portalındaki raporları görüntüleme

Tonlarınızı Log Analytics'e veri gönderecek şekilde yapılandırıldıktan sonra, herhangi bir kasanın bölmesine giderek ve **Yedek Raporlar'ı**seçerek Yedekleme raporlarınızı görüntüleyin.

![Kasa panosu](./media/backup-azure-configure-backup-reports/vault-dashboard.png)

Yedekleme raporu çalışma kitabını açmak için bu bağlantıyı seçin.

> [!NOTE]
> * Şu anda, raporun ilk yüklemesi 1 dakika kadar sürebilir.
> * Kurtarma Hizmetleri kasası yalnızca Yedekleme raporları için bir giriş noktasıdır. Yedekleme raporu çalışma kitabı kasa bölmesinden açıldıktan sonra, tüm kasalarınızda toplanan verileri görmek için uygun Log Analytics çalışma alanları kümesini seçin.

Rapor çeşitli sekmeler içerir:

* **Özet**: Yedekleme gayrimenkulünüzün üst düzey genel görünümünü almak için bu sekmeyi kullanın. Toplam yedekleme öğesi sayısı, tüketilen toplam bulut depolama alanı, korumalı örnek sayısı ve iş yükü türü başına iş başarısı oranına hızlı bir bakış atabilirsiniz. Belirli bir yedekleme yapı türü hakkında daha ayrıntılı bilgi için ilgili sekmelere gidin.

   ![Summary (Özet) sekmesi](./media/backup-azure-configure-backup-reports/summary.png)

* **Yedekleme Öğeleri**: Yedekleme öğesi düzeyinde tüketilen bulut depolama yla ilgili bilgileri ve eğilimleri görmek için bu sekmeyi kullanın. Örneğin, Bir Azure VM yedeklemesinde SQL kullanıyorsanız, yedeklenen her SQL veritabanı için tüketilen bulut depolamasını görebilirsiniz. Ayrıca, belirli bir koruma durumuna sahip yedekleme öğeleriiçin verileri görmeyi de seçebilirsiniz. Örneğin, sekme nin üst kısmındaki **Koruma Durduruldu** döşemesini seçmek, yalnızca Koruma Durduruldu durumundaki Yedekleme öğeleriiçin verileri göstermek için altındaki tüm widget'ları filtreler.

   ![Yedek Öğeler sekmesi](./media/backup-azure-configure-backup-reports/backup-items.png)

* **Kullanım**: Yedeklemeleriniz için anahtar faturalandırma parametrelerini görüntülemek için bu sekmeyi kullanın. Bu sekmede gösterilen bilgiler bir fatura lama varlığı (korumalı kapsayıcı) düzeyindedir. Örneğin, bir DPM sunucusunun Azure'a yedekleniyor olması durumunda, DPM sunucusu için tüketilen korumalı örnekler in ve bulut depolama eğilimini görüntüleyebilirsiniz. Benzer şekilde, Azure Yedekleme'de SQL veya Azure Yedekleme'de SAP HANA kullanıyorsanız, bu sekme, bu veritabanlarının bulunduğu sanal makine düzeyinde kullanımla ilgili bilgiler sağlar.

   ![Kullanım sekmesi](./media/backup-azure-configure-backup-reports/usage.png)

* **İşler**: Günlük başarısız iş sayısı ve iş başarısızlığının en önemli nedenleri gibi işlerde uzun süredir devam eden eğilimleri görüntülemek için bu sekmeyi kullanın. Bu bilgileri hem toplu düzeyde hem de Yedekleme öğesi düzeyinde görüntüleyebilirsiniz. Seçili zaman aralığında bu Yedekleme öğesi üzerinde tetiklenen her iş hakkında ayrıntılı bilgileri görüntülemek için ızgarada belirli bir Yedekleme öğesi seçin.

   ![İşler sekmesi](./media/backup-azure-configure-backup-reports/jobs.png)

* **İlkeler**: İlişkili öğelerin sayısı ve belirli bir ilke kapsamında yedeklenen öğeler tarafından tüketilen toplam bulut depolama alanı gibi tüm etkin ilkelerinizdeki bilgileri görüntülemek için bu sekmeyi kullanın. İlişkili Yedekleme öğelerinin her birinde bilgileri görüntülemek için belirli bir ilke seçin.

   ![İlkeler sekmesi](./media/backup-azure-configure-backup-reports/policies.png)

## <a name="export-to-excel"></a>Excel'e aktar

Tablo veya grafik gibi herhangi bir widget'ın sağ üst kısmındaki aşağı ok düğmesini seçin ve bu widget'ın içeriğini geçerli filtreler uygulanmış olarak Excel sayfası olarak dışa aktarın. Bir tablonun daha fazla satırını Excel'e aktarmak için, her ızgaranın üst kısmındaki **Sayfa Başına Satırlar** açılır ok kullanarak sayfada görüntülenen satır sayısını artırabilirsiniz.

## <a name="pin-to-dashboard"></a>Panoya sabitle

Widget'ı Azure portal panonuza sabitlemek için her widget'ın üst kısmındaki pin düğmesini seçin. Bu özellik, ihtiyacınız olan en önemli bilgileri görüntülemek için özelleştirilmiş panolar oluşturmanıza yardımcı olur.

## <a name="cross-tenant-reports"></a>Kiracı çapraz raporları

[Azure Deniz Feneri'ni](https://docs.microsoft.com/azure/lighthouse/) birden çok kiracı ortamlarında aboneliklere yetkiyle erişen bir şekilde kullanıyorsanız, varsayılan abonelik filtresini kullanabilirsiniz. Verileri görmek istediğiniz tüm abonelikleri seçmek için Azure portalının sağ üst köşesindeki filtre düğmesini seçin. Bunu yapmak, çok kiracılı raporları görüntülemek için kiracılarınızdaki Günlük Analizi çalışma alanlarını seçmenize olanak tanır.

## <a name="conventions-used-in-backup-reports"></a>Yedekleme raporlarında kullanılan sözleşmeler

* Filtreler her sekmede soldan sağa ve yukarıdan aşağıya doğru çalışır. Diğer bir durumda, herhangi bir filtre yalnızca filtrenin sağına veya bu filtrenin altına konumlandırılmış tüm widget'lar için geçerlidir. 
* Renkli bir döşeme seçilmesi, o döşemenin değeriyle ilgili kayıtlar için döşemenin altındaki widget'ları filtreler. Örneğin, **Yedekleme Öğeleri** sekmesinde **Koruma Durduruldu** döşemesini seçmek, Koruma Durduruldu durumundaki yedek öğelerin verilerini göstermek için aşağıdaki ızgaraları ve grafikleri filtreler.
* Renkli olmayan döşemeler tıklatılamaz.
* Geçerli kısmi güne ait veriler raporlarda gösterilmez. Bu nedenle, Zaman **Aralığının** seçili değeri **Son 7 gün**olduğunda, rapor tamamlanan son yedi günün kayıtlarını gösterir. Geçerli gün dahil değil.
* Rapor, seçili zaman aralığında *tetiklenen* işlerin ayrıntılarını (günlük işleri dışında) gösterir. 
* **Bulut Depolama** ve **Korumalı Örnekler** için gösterilen değerler, seçili zaman aralığının *sonundadır.*
* Raporlarda görüntülenen Yedekleme öğeleri, seçili zaman aralığının *sonunda* bulunan öğelerdir. Seçili zaman aralığının ortasında silinen yedekleme öğeleri görüntülenmez. Aynı kural Yedekleme ilkeleri için de geçerlidir.

## <a name="query-load-times"></a>Yükleme sürelerini sorgula

Yedekleme raporundaki widget'lar, kullanıcının Log Analytics çalışma alanlarında çalışan Kusto sorguları tarafından desteklenmektedir. Bu sorgular genellikle daha zengin öngörüler sağlamak için birden çok birleştirme ile büyük miktarda veri işleme içerir. Sonuç olarak, kullanıcı raporları büyük bir yedekleme mülkünde görüntülediğinde widget'lar anında yüklenmeyebilir. Bu tablo, Yedekleme öğesi sayısına ve raporun görüntülendiği zaman aralığına bağlı olarak, farklı widget'ların yüklemek için alabilecekleri süreye ilişkin kabaca bir tahmin sağlar.

| **# Veri kaynakları**                         | **Zaman ufku** | **Yaklaşık yükleme süreleri**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ~5 K                       | 1 ay          | Fayans: 5-10 saniye <br> Izgaralar: 5-10 saniye <br> Grafikler: 5-10 saniye <br> Rapor düzeyi filtreler: 5-10 saniye|
| ~5 K                       | 3 ay          | Fayans: 5-10 saniye <br> Izgaralar: 5-10 saniye <br> Grafikler: 5-10 saniye <br> Rapor düzeyi filtreler: 5-10 saniye|
| ~10 K                       | 3 ay          | Fayans: 15-20 saniye <br> Izgaralar: 15-20 saniye <br> Grafikler: 1-2 dakika <br> Rapor düzeyi filtreler: 25-30 saniye|
| ~15 K                       | 1 ay          | Fayans: 15-20 saniye <br> Izgaralar: 15-20 saniye <br> Grafikler: 50-60 saniye <br> Rapor düzeyi filtreler: 20-25 saniye|
| ~15 K                       | 3 ay          | Fayans: 20-30 saniye <br> Izgaralar: 20-30 saniye <br> Grafikler: 2-3 dakika <br> Rapor düzeyi filtreler: 50-60 saniye |

## <a name="what-happened-to-the-power-bi-reports"></a>Power BI raporlarına ne oldu? 
* Bir Azure depolama hesabından veri kaynağı olan raporlama için daha önceki Power BI şablonu uygulaması, amortisman yolundadır. Raporları görüntülemek için Log Analytics'e kasa tanılama verileri göndermeye başlamanızı öneririz.

* Buna ek olarak, tanılama verilerini bir depolama hesabına veya Log Analytics çalışma alanına göndermenin V1 şeması da bir amortisman yolundadır. V1 şemasını temel alan özel sorgular veya otomasyonlar yazdıysanız, şu anda desteklenen V2 şemasını kullanmak için bu sorguları güncelleştirmeniz önerilir.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Yedekleme ile izleme ve raporlama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-azure-monitor-alert-faq)
