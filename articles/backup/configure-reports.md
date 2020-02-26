---
title: Azure Backup raporlarını yapılandırma
description: Log Analytics ve Azure çalışma kitaplarını kullanarak Azure Backup raporlarını yapılandırma ve görüntüleme
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: cefe81e53e89b8d7903469e836f3c5d2665febea
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77582713"
---
# <a name="configure-azure-backup-reports"></a>Azure Backup raporlarını yapılandırma

Yedekleme yöneticileri için ortak bir gereksinim, uzun bir süre kapsayan verilere bağlı olarak yedeklemeler hakkında öngörüler elde etmek içindir. Bu tür bir çözüm için birden çok kullanım durumu olabilir: bulut depolama alanını ayırmak ve tahmin etmek, yedeklemelerin denetlenmesi ve geri yüklenmesi ve farklı ayrıntı düzeyi düzeylerinde önemli eğilimleri belirlemek.

Azure Backup, [Azure Izleyici günlüklerinin](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) ve [Azure çalışma kitaplarının](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks)faydalanmasını sağlayan bir raporlama çözümü sağlar. Bu, tüm yedeklemeleriniz genelinde yedeklemeleriniz hakkında zengin Öngörüler almanıza yardımcı olur. Bu makalede, yedekleme raporlarının nasıl yapılandırılacağı ve görüntüleneceği açıklanır.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

* Yedekleme raporları Azure VM 'lerinde, Azure VM 'lerinde SQL 'de SAP HANA/Ao 'da, Azure Backup aracıda (MARS), Azure Backup Sunucusu (MABS) ve System Center DPM 'de desteklenir.
* DPM iş yükleri için yedekleme raporları, DPM sürüm 5.1.363.0 ve üzeri ve aracı sürümü 2.0.9127.0 ve üzeri için desteklenir.
* MABS iş yükleri için yedekleme raporları, MABS sürümü 13.0.415.0 ve üzeri ve aracı sürümü 2.0.9170.0 ve üzeri için desteklenir.
* Yedekleme raporları, verileri kullanıcının erişebileceği bir Log Analytics (LA) çalışma alanına gönderildiği sürece tüm yedekleme öğelerinde, kasaların, aboneliklerde ve bölgelerde görüntülenebilir. 
* Müşterilerinizin aboneliklerine temsilci erişimi olan bir [Azure](https://docs.microsoft.com/azure/lighthouse/) açık Kullanıcı kullanıyorsanız, tüm kiracılarınızdaki raporları görüntülemek için Azure açık thouse ile bu raporları kullanabilirsiniz.
* Günlük yedekleme işleri için veriler şu anda raporlarda görüntülenmiyor.

## <a name="getting-started"></a>Başlarken

Raporları kullanmaya başlamak için aşağıda açıklanan üç adımı izleyin:

1. **Log Analytics (LA) çalışma alanı oluşturun (veya mevcut bir tane kullanın):**

Yedekleme raporlama verilerinizi depolamak için bir veya daha fazla LA çalışma alanı ayarlamanız gerekir. Bu LA çalışma alanının oluşturulabildiği konum ve abonelik, kasalarınızın bulunduğu konumdan ve aboneliğin bağımsızdır. 

Aşağıdaki makaleye bakın: bir LA çalışma alanı ayarlamak için [Azure portal Log Analytics çalışma alanı oluşturun](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) .

Varsayılan olarak, bir LA çalışma alanındaki veriler 30 gün boyunca tutulur. Daha uzun bir süre boyunca verileri görmek için, LA çalışma alanının saklama süresini değiştirin. Saklama süresini değiştirmek için aşağıdaki makaleye bakın: [Azure Izleyici günlükleriyle kullanımı ve maliyetleri yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage).

2. **Kasalarınız için tanılama ayarlarını yapılandırın:**

Kurtarma Hizmetleri kasaları gibi kaynaklar Azure Resource Manager, zamanlanmış işlemler ve Kullanıcı tarafından tetiklenen işlemler hakkındaki bilgileri Tanılama verileri olarak kaydeder. 

Kurtarma Hizmetleri kasaınızın İzleme bölümünde **Tanılama ayarları** ' nı seçin ve kurtarma hizmetleri kasasının tanılama verilerinin hedefini belirtin. [Tanılama olaylarını kullanma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events).

![Tanılama ayarları dikey penceresi](./media/backup-azure-configure-backup-reports/resource-specific-blade.png)

Azure Backup, belirli bir kapsamdaki tüm kasaların tanılama ayarlarının yapılandırılmasını otomatikleştiren yerleşik bir Azure Ilkesi de sağlar. Bu ilkeyi nasıl kullanacağınızı öğrenmek için aşağıdaki makaleye bakın: [kasa tanılama ayarlarını ölçeklendirerek yapılandırma](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics)

3. **Azure portal raporları görüntüle:**

Kasalarınızı LA 'ya veri gönderecek şekilde yapılandırdıktan sonra, herhangi bir kasanın dikey penceresine gidip **yedekleme raporları** menü öğesine tıklayarak yedekleme raporlarınızı görüntüleyin. 

![Kasa panosu](./media/backup-azure-configure-backup-reports/vault-dashboard.png)

Bu bağlantıya tıkladığınızda yedekleme raporu çalışma kitabı açılır.

> [!NOTE]
> Şu anda, raporun ilk yükünün 1 dakikaya kadar zaman alabilir.

Aşağıda, raporun içerdiği çeşitli sekmelerin açıklaması verilmiştir:

* **Özet** -Özet sekmesi, yedeğinize ilişkin üst düzey bir genel bakış sağlar. Özet sekmesinde, toplam yedekleme öğesi sayısı, tüketilen toplam bulut depolaması, korunan örnek sayısı ve iş yükü türü başına iş başarısı oranı hakkında hızlı bir bakış edinebilirsiniz. Belirli bir yedekleme yapıtı türü etrafında daha ayrıntılı bilgi için ilgili sekmelere gidin.

![Özet sekmesi](./media/backup-azure-configure-backup-reports/summary.png)

* **Yedekleme öğeleri** -yedekleme öğeleri sekmesi, bir yedekleme öğesi düzeyinde tüketilen Bulut depolamada bilgi ve eğilimleri görmenizi sağlar. Örneğin, Azure VM yedekleme 'de SQL kullanıyorsanız, yedeklenen her SQL veritabanı için kullanılan bulut depolama alanını görebilirsiniz. Ayrıca, belirli bir koruma durumunun yedekleme öğeleri için verileri görmeyi seçebilirsiniz. Örneğin, sekmenin en üstündeki **koruma durdurulmuş** Kutucuğa tıkladığınızda, yalnızca koruma durdurulmuş durumundaki yedekleme öğeleri için verileri göstermek üzere aşağıdaki tüm pencere öğelerine filtre uygular.

![Yedekleme öğeleri sekmesi](./media/backup-azure-configure-backup-reports/backup-items.png)

* **Kullanım** -kullanım sekmesi, yedeklemeleriniz için önemli faturalandırma parametrelerini görüntülemenize yardımcı olur. Bu sekmede gösterilen bilgiler bir faturalandırma varlığı (korumalı kapsayıcı) düzeyidir. Örneğin, Azure 'a yedeklenmekte olan bir DPM sunucusu söz konusu olduğunda, DPM sunucusu için kullanılan korumalı örneklerin ve bulut depolamanın eğilimini görüntüleyebilirsiniz. Benzer şekilde, Azure Backup 'de SQL kullanıyorsanız veya Azure Backup SAP HANA, bu sekme Bu veritabanlarının bulunduğu sanal makine düzeyinde kullanımla ilgili bilgiler sağlar.

![Kullanım sekmesi](./media/backup-azure-configure-backup-reports/usage.png)

* **İşler-işler** sekmesi, işlerinizde gerçekleşen hatalı iş sayısı ve iş hatasının en önemli nedenleri gibi işlerin uzun süre çalışan eğilimlerini görüntülemenize olanak sağlar. Bu bilgileri hem bir toplama düzeyinde hem de yedekleme öğesi düzeyinde görüntüleyebilirsiniz. Bir kılavuzda belirli bir yedekleme öğesine tıkladığınızda, seçili zaman aralığında o yedekleme öğesinde tetiklenen her bir işle ilgili ayrıntılı bilgileri görüntülemenize izin verir.

![İşler sekmesi](./media/backup-azure-configure-backup-reports/jobs.png)

* **İlkeler** -İlkeler sekmesi, ilişkili öğe sayısı ve belirli bir ilke altında yedeklenen öğeler tarafından tüketilen toplam bulut depolama alanı gibi tüm etkin ilkeleriniz hakkındaki bilgileri görüntülemenize olanak sağlar. Belirli bir ilkeye tıklanması, ilişkili yedekleme öğelerinin her biriyle ilgili bilgileri görüntülemenize olanak sağlar.

![İlkeler sekmesi](./media/backup-azure-configure-backup-reports/policies.png)

## <a name="exporting-to-excel"></a>Excel 'e verme

Herhangi bir pencere öğesinin sağ üst köşesindeki aşağı ok düğmesine tıklamak (tablo/grafik), varolan filtrelerin uygulanmış olduğu gibi bu pencere öğesinin içeriğini Excel sayfası olarak dışa aktarır. Bir tablonun daha fazla satırını Excel 'e aktarmak için, her bir kılavuzun en üstünde bulunan **sayfa başına satır** açılan listesini kullanarak sayfada görüntülenecek satır sayısını artırabilirsiniz.

## <a name="pinning-to-dashboard"></a>Panoya sabitleme

Pencere öğesini Azure portal panonuza sabitlemek için her pencere öğesinin en üstündeki sabitleme simgesine tıklayın. Bu, ihtiyacınız olan en önemli bilgileri görüntüleyecek şekilde uyarlanmış özelleştirilmiş panolar oluşturmanıza yardımcı olur.

## <a name="cross-tenant-reports"></a>Çapraz kiracı raporları

Çoklu kiracı ortamlarında abonelikler için temsilci erişimi olan bir [Azure](https://docs.microsoft.com/azure/lighthouse/) açık Kullanıcı kullanıyorsanız, verileri görmek istediğiniz tüm abonelikleri seçmek için, varsayılan abonelik filtresini (Azure Portal en sağ üst kısmında bulunan filtre simgesine tıklayarak) kullanabilirsiniz. Bunun yapılması, çok kiracılı raporları görüntülemek için kiracılar genelinde LA çalışma alanları seçmenizi sağlar.

## <a name="conventions-used-in-backup-reports"></a>Yedekleme raporlarında kullanılan kurallar

* Filtreler, her sekmede soldan sağa ve yukarıdan aşağıya doğru çalışır, diğer bir deyişle, herhangi bir filtre yalnızca bu filtrenin sağına veya bu filtrenin altına konumlandırılmış olan tüm pencere öğeleri için geçerlidir. 
* Renkli bir kutucuğa tıkladığınızda, o kutucuğun değeriyle ilgili kayıtlar için kutucuğun altındaki pencere öğeleri filtrelendirilir. Örneğin, yedekleme öğeleri sekmesindeki *koruma durdurulmuş* Kutucuğa tıkladığınızda, aşağıdaki kılavuzların ve grafiklerin ' koruma durduruldu ' durumunda yedekleme öğelerinin verilerini göstermesi için filtre uygular.
* Renkli olmayan kutucuklar tıklatılabilir değildir.
* Geçerli kısmi güne ait veriler raporlarda gösterilmez. Bu nedenle, seçili **zaman aralığı** değeri ***son 7 gün***olduğunda, rapor son 7 günlük kayıtları gösterir (geçerli günü içermez).
* Rapor, seçilen zaman aralığında **tetiklenen** işlerin ayrıntılarını (günlük işlerden ayrı olarak) gösterir. 
* Bulut depolama ve korumalı örnekler için gösterilen değerler, seçili zaman aralığının **sonunda** bulunur.
* Raporlarda görüntülenen yedekleme öğeleri, seçili zaman aralığının **sonunda** bulunan öğelerdir. Seçilen zaman aralığının ortasında silinen yedekleme öğeleri görüntülenmez. Aynı kural yedekleme Ilkeleri için de geçerlidir.

## <a name="query-load-times"></a>Sorgu yükleme süreleri

Yedekleme raporundaki pencere öğeleri, kullanıcının LA çalışma alanlarında çalışan kusto sorguları tarafından desteklenir. Bu sorgular genellikle büyük miktarlarda veri işlemeyi, daha zengin Öngörüler sağlamak için birden çok birleştirme işlemini içerir. Kullanıcı büyük bir yedekleme genelinde rapor görüntülediklerinde pencere öğeleri anında yüklenmeyebilir. Aşağıdaki tabloda, farklı pencere öğelerinin yük için alınacağı, yedekleme öğelerinin sayısına ve raporun görüntülendiği zaman aralığına göre kabaca bir tahmin verilmiştir:

| **Veri kaynakları**                         | **Zaman ufuk** | **Yükleme süreleri (yaklaşık)**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ~ 5 K                       | 1 ay          | Kutucuklar: 5-10 saniye <br> Kılavuzlar: 5-10 saniye <br> Grafikler: 5-10 saniye <br> Rapor düzeyi filtreleri: 5-10 saniye|
| ~ 5 K                       | 3 ay          | Kutucuklar: 5-10 saniye <br> Kılavuzlar: 5-10 saniye <br> Grafikler: 5-10 saniye <br> Rapor düzeyi filtreleri: 5-10 saniye|
| ~ 10 K                       | 3 ay          | Kutucuklar: 15-20 saniye <br> Kılavuzlar: 15-20 saniye <br> Grafikler: 1-2 dakika <br> Rapor düzeyi filtreleri: 25-30 saniye|
| ~ 15 K                       | 1 ay          | Kutucuklar: 15-20 saniye <br> Kılavuzlar: 15-20 saniye <br> Grafikler: 50-60 saniye <br> Rapor düzeyi filtreleri: 20-25 saniye|
| ~ 15 K                       | 3 ay          | Kutucuklar: 20-30 saniye <br> Kılavuzlar: 20-30 saniye <br> Grafikler: 2-3 dakika <br> Rapor düzeyi filtreleri: 50-60 saniye |

## <a name="what-happened-to-the-power-bi-reports"></a>Power BI raporlarına ne oldu?
* Raporlama için daha önce Power BI şablon uygulamamız (Azure Storage hesabından alınan verilerin kaynağı) kullanımdan kaldırma yolunda yer alır. Raporları görüntülemek için yukarıda açıklanan Log Analytics kasa tanılama verilerini göndermeye başlamanız önerilir.

* Ayrıca, bir depolama hesabına veya bir LA çalışma alanına Tanılama verileri göndermenin v1 şeması, kullanımdan kaldırma yolunda da olur. Bu, v1 şemasına göre özel sorgular veya bir sorgu yazdıysanız, bu sorguları şu anda desteklenen v2 şemasını kullanmak için güncelleştirmeniz önerilir.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Backup ile izleme ve raporlama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-azure-monitor-alert-faq)