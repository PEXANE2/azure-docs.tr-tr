---
title: Azure Backup raporlarını yapılandırma
description: Günlük Analitiği ve Azure Çalışma Kitaplarını kullanarak Azure Yedekleme için raporları yapılandırma ve görüntüleme
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: 651d1383f0f292895ed95c91bafd5206d4f04c2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78161210"
---
# <a name="configure-azure-backup-reports"></a>Azure Backup raporlarını yapılandırma

Yedekleme yöneticileri için ortak bir gereksinim, uzun bir süreye yayılan verilere dayalı olarak yedeklemeler hakkında öngörüler elde etmektir. Böyle bir çözüm için birden çok kullanım durumu olabilir - tüketilen bulut depolamanın tahsisi ve tahmini, yedeklemelerin ve geri yüklemelerin denetlenmeleri ve farklı parçalılık düzeylerindeki önemli eğilimlerin tanımlanması.

Bugün Azure Yedekleme, Azure Monitör [Günlükleri](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) ve [Azure Çalışma Kitaplarından](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks)yararlanan ve tüm yedekleme mülkünüzde yedeklemeleriniz hakkında zengin öngörüler elde etmeye yardımcı olan bir raporlama çözümü sunar. Bu makalede, Yedekleme Raporlarınasıl yapılandırılatır ve görüntüleneme açıklanmaktadır.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

* Yedekleme Raporları Azure VM'ler için desteklenir, Azure VM'lerde SQL, Azure VM'lerde SAP HANA/ASE, Azure Yedekleme Aracısı (MARS), Azure Yedekleme Sunucusu (MABS) ve Sistem Merkezi DPM için desteklenir.
* DPM iş yükleri için Yedekleme Raporları DPM Sürüm 5.1.363.0 ve üzeri ve Aracı Sürüm 2.0.9127.0 ve üzeri için desteklenir.
* MABS iş yükleri için Yedekleme Raporları MABS Sürüm 13.0.415.0 ve üzeri ve Aracı Sürüm 2.0.9170.0 ve üzeri için desteklenir.
* Yedekleme Raporları, verileri kullanıcının erişebildiği bir Günlük Analizi (LA) Çalışma Alanına gönderildiği sürece tüm yedekleme öğeleri, kasalar, abonelikler ve bölgelerde görüntülenebilir. 
* Müşterilerinizin aboneliklerine yetkili erişimi olan bir [Azure Deniz Feneri](https://docs.microsoft.com/azure/lighthouse/) kullanıcısıysanız, bu raporları tüm kiracılarınızda raporları görüntülemek için Azure Deniz Feneri ile kullanabilirsiniz.
* Günlük yedekleme işlerine ait veriler şu anda raporlarda görüntülenmez.

## <a name="getting-started"></a>Başlarken

Raporları kullanmaya başlamak için aşağıdaki üç adımı izleyin:

1. **Günlük Analizi (LA) çalışma alanı oluşturun (veya varolan bir çalışma alanı kullanın):**

Yedekleme raporlama verilerinizi depolamak için bir veya daha fazla LA Çalışma alanı ayarlamanız gerekir. Bu LA çalışma alanının oluşturulabileceği konum ve abonelik, kasalarınızın bulunduğu konumdan ve abonelikten bağımsızdır. 

Aşağıdaki makaleye bakın: La Çalışma Alanı ayarlamak için [Azure portalında bir Günlük Analizi Çalışma Alanı oluşturun.](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)

Varsayılan olarak, BIR LA Çalışma alanındaki veriler 30 gün boyunca saklanır. Daha uzun bir zaman ufku için verileri görmek için LA Çalışma Alanı'nın bekletme süresini değiştirin. Bekletme süresini değiştirmek için aşağıdaki makaleye bakın: [Azure Monitör Günlükleri ile kullanımı ve maliyetleri yönetin.](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage)

2. **Kasalarınız için tanılama ayarlarını yapılandırın:**

Kurtarma Hizmetleri kasaları gibi Azure Kaynak Yöneticisi kaynakları, zamanlanmış işlemler ve kullanıcı tarafından tetiklenen işlemler hakkındaki bilgileri tanılama verileri olarak kaydeder. 

Kurtarma Hizmetleri kasanızın izleme bölümünde, **Tanılama ayarlarını** seçin ve Kurtarma Hizmetleri kasasının tanılama verileri için hedefi belirtin. [Tanılama olaylarını kullanma hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events)

![Teşhis Ayarları Blade](./media/backup-azure-configure-backup-reports/resource-specific-blade.png)

Azure Yedekleme, belirli bir kapsamdaki tüm kasalar için tanı ayarlarıyapılandırmasını otomatikleştiren yerleşik bir Azure İlkesi de sağlar. Bu ilkeyi nasıl kullanacağınızı öğrenmek için aşağıdaki makaleye bakın: [Vault Diagnostics Ayarlarını ölçekte yapılandırın](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics)

> [!NOTE]
> Tanılamayı yapılandırdıktan sonra, ilk veri itme işleminin tamamlanması 24 saat kadar sürebilir. Veriler LA Çalışma Alanına akmaya başladıktan sonra, geçerli kısmi güne ait veriler raporlarda gösterilmediğinden, raporlardaki verileri hemen göremeyebilirsiniz (daha fazla ayrıntı [burada).](https://docs.microsoft.com/azure/backup/configure-reports#conventions-used-in-backup-reports) Bu nedenle, loglarınızı günlük analytics'e veri göndermek üzere yapılandırmanızdan 2 gün sonra raporları görüntülemeye başlamanız önerilir.

3. **Azure portalındaki raporları görüntüleyin:**

Kasalarınızı LA'ye veri gönderecek şekilde yapılandırıldıktan sonra, herhangi bir kasanın bıçağına göz atarak ve **Yedekleme Raporları** menü öğesini tıklatarak yedek raporlarınızı görüntüleyin. 

![Kasa Panosu](./media/backup-azure-configure-backup-reports/vault-dashboard.png)

Bu bağlantıyı tıklattığınızda Yedekleme Raporu Çalışma Kitabı açılır.

> [!NOTE]
> * Şu anda, raporun ilk yüklemesi 1 dakika kadar sürebilir.
> * Kurtarma Hizmetleri kasası yalnızca Yedekleme Raporları için bir giriş noktasıdır. Yedekleme Raporları Çalışma Kitabı bir kasanın bıçağından açıldığında, tüm kasalarınızda toplanan verileri görebilirsiniz (uygun LA Çalışma Alanları kümesini seçerek).

Aşağıda, raporun içerdiği çeşitli sekmelerin açıklaması verilmiştir:

* **Özet** - Özet sekmesi yedekleme gayrimenkulünüzün üst düzey bir genel görünümünü sağlar. Özet sekmesi altında, toplam yedekleme öğesi sayısı, tüketilen toplam bulut depolama alanı, korumalı örnek sayısı ve iş yükü türü başına iş başarısı oranı hakkında hızlı bir bakış elde edebilirsiniz. Belirli bir yedek yapı türü yle ilgili daha ayrıntılı bilgi için ilgili sekmelere gidin.

![Summary (Özet) sekmesi](./media/backup-azure-configure-backup-reports/summary.png)

* **Yedekleme Öğeleri** - Yedekleme Öğeleri sekmesi, Yedekleme Öğesi düzeyinde tüketilen bulut depolama yla ilgili bilgileri ve eğilimleri görmenizi sağlar. Örneğin, Azure VM yedeklemesinde SQL kullanıyorsanız, yedeklenen her SQL veritabanı için tüketilen bulut depolamasını görebilirsiniz. Ayrıca, belirli bir koruma durumuna sahip yedekleme öğeleriiçin verileri görmeyi de seçebilirsiniz. Örneğin, sekmenin üst kısmındaki **Koruma Durduruldu** döşemesini tıklattığınızda, yalnızca Koruma Durdurulan durumdaki Yedekleme Öğeleri için verileri göstermek için aşağıdaki tüm widget'ları filtreler.

![Yedek Öğeler sekmesi](./media/backup-azure-configure-backup-reports/backup-items.png)

* **Kullanım** - Kullanım sekmesi, yedeklemeleriniz için anahtar faturalandırma parametrelerini görüntülemenize yardımcı olur. Bu sekmede gösterilen bilgiler bir fatura lama varlığı (korumalı kapsayıcı) düzeyindedir. Örneğin, bir DPM sunucusunun Azure'a yedekleniyor olması durumunda, DPM sunucusu için tüketilen korumalı örnekler in ve bulut depolama eğilimini görüntüleyebilirsiniz. Benzer şekilde, Azure Yedekleme'de SQL veya Azure Yedekleme'de SAP HANA kullanıyorsanız, bu sekme, bu veritabanlarının bulunduğu sanal makine düzeyinde kullanımla ilgili bilgiler sağlar.

![Kullanım sekmesi](./media/backup-azure-configure-backup-reports/usage.png)

* **İşler** - İşler sekmesi, günlük başarısız işlerin sayısı ve iş başarısızlığının en önemli nedenleri gibi işlerde uzun süre çalışan eğilimleri görüntülemenizi sağlar. Bu bilgileri hem toplam düzeyde hem de yedek madde düzeyinde görüntüleyebilirsiniz. Kılavuzdaki belirli bir yedekleme öğesini tıklatmak, seçili zaman aralığında bu yedekleme öğesiüzerinde tetiklenen her iş hakkında ayrıntılı bilgileri görüntülemenize olanak tanır.

![İşler sekmesi](./media/backup-azure-configure-backup-reports/jobs.png)

* **İlkeler -** İlkeler sekmesi, ilişkili öğelerin sayısı ve belirli bir ilke altında yedeklenen öğeler tarafından tüketilen toplam bulut depolama alanı gibi tüm etkin ilkeleriniz hakkındaki bilgileri görüntülemenize olanak tanır. Belirli bir ilkeyi tıklatmak, ilişkili yedekleme öğelerinin her biri hakkındaki bilgileri görüntülemenize olanak tanır.

![İlkeler sekmesi](./media/backup-azure-configure-backup-reports/policies.png)

## <a name="exporting-to-excel"></a>Excel'e Dışa Aktarma

Herhangi bir widget'ın (tablo/grafik) sağ üst kısmındaki aşağı ok düğmesine tıkladığınızda, geçerli filtreler uygulandığı gibi, bu widget'ın içeriğini Excel sayfası olarak dışa dışa dışa dışa ekler. Bir tablonun daha fazla satırını Excel'e aktarmak için, her ızgaranın üst kısmındaki **Sayfa Başına Satır** lar açılır düşüşünü kullanarak sayfada görüntülenen satır sayısını artırabilirsiniz.

## <a name="pinning-to-dashboard"></a>Panoya Sabitleme

Widget'ı Azure portal panonuza sabitlemek için her widget'ın üst kısmındaki Pin Simgesi'ni tıklatın. Bu, ihtiyacınız olan en önemli bilgileri görüntülemek için özelleştirilmiş panolar oluşturmanıza yardımcı olur.

## <a name="cross-tenant-reports"></a>Kiracılar Arası Raporlar

Birden çok kiracı ortamlarında aboneliklere yetkin erişimi olan bir [Azure Deniz Feneri](https://docs.microsoft.com/azure/lighthouse/) kullanıcısıysanız, verileri görmek istediğiniz tüm abonelikleri seçmek için varsayılan abonelik filtresini (Azure portalının sağ üst ağındaki filtre simgesine tıklayarak) kullanabilirsiniz. Bunu yapmak, çok kiracılı raporları görüntülemek için kiracılarınız arasında LA Çalışma Alanlarını seçmenize izin verecektir.

## <a name="conventions-used-in-backup-reports"></a>Yedekleme Raporlarında Kullanılan Sözleşmeler

* Filtreler her sekmede soldan sağa ve yukarıdan aşağıya doğru çalışır, diğer bir süre önce herhangi bir filtre yalnızca filtrenin sağına veya bu filtrenin altına konumlandırılmış tüm widget'lar için geçerlidir. 
* Renkli bir döşemeye tıkladığınızda, döşemenin değerine ilişkin kayıtlar için döşemenin altındaki widget'lar filtreler. Örneğin, Yedekleme Öğeleri sekmesinde *Koruma Durduruldu* döşemesini tıklattığınızda, 'Koruma Durduruldu' durumunda yedek öğelerin verilerini göstermek için aşağıdaki ızgaraları ve grafikleri filtreler.
* Renkli olmayan döşemeler tıklatılamaz.
* Geçerli kısmi güne ait veriler raporlarda gösterilmez. Bu nedenle, Zaman **Aralığı'nın** seçili değeri, ***Son 7 gün***olduğunda, rapor son 7 tamamlanan güne ait kayıtları gösterir (geçerli günü içermez).
* Rapor, seçili zaman aralığında **tetiklenen** İşlerin (günlük işleri dışında) ayrıntılarını gösterir. 
* Bulut Depolama ve Korumalı Örnekler için gösterilen değerler, seçili zaman aralığının **sonundadır.**
* Raporlarda görüntülenen Yedekleme Öğeleri, seçili zaman aralığının **sonunda** bulunan öğelerdir. Seçili zaman aralığının ortasında silinen Yedekleme Öğeleri görüntülenmez. Aynı kural Yedekleme İlkeleri için de geçerlidir.

## <a name="query-load-times"></a>Yükleme sürelerini sorgula

Yedekleme Raporu'ndaki widget'lar, kullanıcının LA Çalışma Alanlarında çalışan Kusto sorguları tarafından desteklenmektedir. Bu sorgular genellikle daha zengin öngörüler sağlamak için birden çok birleştirme yle büyük miktarda verinin işlenmesini içerdiğinden, widget'lar kullanıcı raporları büyük bir yedekleme mülkünde görüntülediğinde anında yüklenmeyebilir. Aşağıdaki tablo, yedekleme öğelerinin sayısına ve raporun görüntülendiği zaman aralığına bağlı olarak, farklı widget'ların yüklemek için alabilecekleri süreye ilişkin kabaca bir tahmin sağlar:

| **# Veri Kaynakları**                         | **Zaman Ufku** | **Yükleme Süreleri (yaklaşık)**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ~5 K                       | 1 ay          | Fayans: 5-10 saniye <br> Izgaralar: 5-10 saniye <br> Grafikler: 5-10 saniye <br> Rapor düzeyi filtreler: 5-10 saniye|
| ~5 K                       | 3 ay          | Fayans: 5-10 saniye <br> Izgaralar: 5-10 saniye <br> Grafikler: 5-10 saniye <br> Rapor düzeyi filtreler: 5-10 saniye|
| ~10 K                       | 3 ay          | Fayans: 15-20 saniye <br> Izgaralar: 15-20 saniye <br> Grafikler: 1-2 dakika <br> Rapor düzeyi filtreler: 25-30 saniye|
| ~15 K                       | 1 ay          | Fayans: 15-20 saniye <br> Izgaralar: 15-20 saniye <br> Grafikler: 50-60 saniye <br> Rapor düzeyi filtreler: 20-25 saniye|
| ~15 K                       | 3 ay          | Fayans: 20-30 saniye <br> Izgaralar: 20-30 saniye <br> Grafikler: 2-3 dakika <br> Rapor düzeyi filtreler: 50-60 saniye |

## <a name="what-happened-to-the-power-bi-reports"></a>Power BI Raporlarına ne oldu?
* Raporlama için daha önceki Power BI şablon uygulamamız (Azure Depolama Hesabından veri kaynaklı) amortisman yolundadır. Raporları görüntülemek için yukarıda açıklandığı gibi Log Analytics'e kasa tanılama verilerini göndermeye başlamanız önerilir.

* Buna ek olarak, bir depolama hesabına veya LA Çalışma alanına tanılama verileri gönderme V1 şeması da bir amortisman yolu üzerindedir. Bu, V1 şemasını temel alan özel sorgular veya otomasyonlar yazdıysanız, şu anda desteklenen V2 şemasını kullanmak için bu sorguları güncelleştirmeniz gerektiği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Yedekleme ile izleme ve raporlama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-azure-monitor-alert-faq)