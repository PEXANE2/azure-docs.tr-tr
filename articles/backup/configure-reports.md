---
title: Azure Backup raporlarını yapılandırma
description: Log Analytics ve Azure çalışma kitaplarını kullanarak Azure Backup raporlarını yapılandırma ve görüntüleme
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: 20dcf7f3f9bbc5626c4a05ef064203b3ae5020cd
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2020
ms.locfileid: "84484969"
---
# <a name="configure-azure-backup-reports"></a>Azure Backup raporlarını yapılandırma

Yedekleme yöneticileri için ortak bir gereksinim, uzun bir süre yayılan verilere göre yedeklemeler hakkında öngörüler elde etmek içindir. Böyle bir çözüm için kullanım örnekleri şunları içerir:

- Tüketilen bulut depolamasını ayırma ve tahmin etme.
- Yedeklemeler ve geri yüklemeler denetimi.
- Farklı ayrıntı düzeyi düzeylerinde önemli eğilimleri tanımlama.

Bugün Azure Backup [Azure izleyici günlüklerini](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) ve [Azure çalışma kitaplarını](https://docs.microsoft.com/azure/azure-monitor/platform/workbooks-overview)kullanan bir raporlama çözümü sağlar. Bu kaynaklar, tüm yedeklemeleriniz genelinde yedeklemeleriniz hakkında zengin Öngörüler elde etmenize yardımcı olur. Bu makalede Azure Backup raporlarının nasıl yapılandırılacağı ve görüntüleneceği açıklanır.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

- Yedekleme raporları Azure VM 'leri, Azure sanal makinelerinde SQL, Azure VM 'lerinde SAP HANA, Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı, Microsoft Azure Backup sunucu (MABS) ve System Center Data Protection Manager (DPM) için desteklenir. Azure dosya paylaşma yedeklemesi için veriler, 1. ve sonrasında oluşturulan tüm kayıtlar için görüntülenir, 2020.
- DPM iş yükleri için yedekleme raporları, DPM sürüm 5.1.363.0 ve üzeri ve aracı sürümü 2.0.9127.0 ve üzeri için desteklenir.
- MABS iş yükleri için yedekleme raporları, MABS sürümü 13.0.415.0 ve üzeri ve aracı sürümü 2.0.9170.0 ve üzeri için desteklenir.
- Yedekleme raporları, verileri kullanıcının erişimi olan bir Log Analytics çalışma alanına gönderildiği sürece tüm yedekleme öğeleri, kasa, abonelik ve bölgelerde görüntülenebilir. Bir kasa kümesinin raporlarını görüntülemek için, yalnızca kasaların verilerini gönderdiği Log Analytics çalışma alanına okuyucu erişiminizin olması gerekir. Bireysel kasaların erişimine sahip olmanız gerekmez.
- Müşterilerinizin aboneliklerine temsilci erişimi olan bir [Azure](https://docs.microsoft.com/azure/lighthouse/) açık Kullanıcı kullanıyorsanız, tüm kiracılarınızdaki raporları görüntülemek için Azure açık thouse ile bu raporları kullanabilirsiniz.
- Şu anda, veriler en fazla 100 Log Analytics çalışma alanı (kiracılar arasında) üzerinde yedekleme raporlarında görüntülenebilir.
- Şu anda raporlarda görüntülenmeyen günlük yedekleme işleri verileri.

## <a name="get-started"></a>başlarken

Raporları kullanmaya başlamak için bu adımları izleyin.

### <a name="1-create-a-log-analytics-workspace-or-use-an-existing-one"></a>1. bir Log Analytics çalışma alanı oluşturun veya mevcut olanı kullanın

Yedekleme raporlama verilerinizi depolamak için bir veya daha fazla Log Analytics çalışma alanı ayarlayın. Bu Log Analytics çalışma alanının oluşturulabildiği konum ve abonelik, kasalarınızın bulunduğu konumdan ve aboneliğin bağımsızdır.

Log Analytics çalışma alanı ayarlamak için, bkz. [Azure portal Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

Varsayılan olarak, bir Log Analytics çalışma alanındaki veriler 30 gün boyunca tutulur. Daha uzun bir süre için verileri görmek üzere Log Analytics çalışma alanının saklama süresini değiştirin. Saklama süresini değiştirmek için bkz. [Azure izleyici günlükleri ile kullanımı ve maliyetleri yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage).

### <a name="2-configure-diagnostics-settings-for-your-vaults"></a>2. kasalarınız için tanılama ayarlarını yapılandırın

Kurtarma Hizmetleri kasaları gibi kaynaklar Azure Resource Manager, zamanlanmış işlemler ve Kullanıcı tarafından tetiklenen işlemler hakkındaki bilgileri Tanılama verileri olarak kaydeder.

Kurtarma Hizmetleri kasaınızın İzleme bölümünde **Tanılama ayarları** ' nı seçin ve kurtarma hizmetleri kasasının tanılama verilerinin hedefini belirtin. Tanılama olaylarını kullanma hakkında daha fazla bilgi için bkz. [Kurtarma Hizmetleri kasaları için tanılama ayarlarını kullanma](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events).

![Tanılama ayarları bölmesi](./media/backup-azure-configure-backup-reports/resource-specific-blade.png)

Azure Backup Ayrıca, belirli bir kapsamdaki tüm kasaların tanılama ayarlarının yapılandırılmasını otomatikleştiren yerleşik bir Azure Ilke tanımı sağlar. Bu ilkeyi nasıl kullanacağınızı öğrenmek için bkz. [kasa tanılama ayarlarını ölçeklendirerek yapılandırma](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics).

> [!NOTE]
> Tanılamayı yapılandırdıktan sonra, ilk veri gönderimi işleminin tamamlanması 24 saate kadar sürebilir. Veriler Log Analytics çalışma alanına akışa başladıktan sonra raporlardaki verileri hemen göremeyebilirsiniz, çünkü geçerli kısmi güne ait veriler raporlarda gösterilmez. Daha fazla bilgi için bkz. [yedekleme raporlarında kullanılan kurallar](https://docs.microsoft.com/azure/backup/configure-reports#conventions-used-in-backup-reports). Vakaları Log Analytics verileri gönderecek şekilde yapılandırdıktan sonra raporları iki gün sonra görüntülemeyi başlatmanız önerilir.

#### <a name="3-view-reports-in-the-azure-portal"></a>3. Azure portal raporları görüntüleme

Vaetlerinizi Log Analytics veri gönderecek şekilde yapılandırdıktan sonra, tüm kasaların bölmesine gidip **yedekleme raporları**' nı seçerek yedekleme raporlarınızı görüntüleyin.

![Kasa panosu](./media/backup-azure-configure-backup-reports/vault-dashboard.png)

Yedekleme raporu çalışma kitabını açmak için bu bağlantıyı seçin.

> [!NOTE]
>
> - Şu anda, raporun ilk yükü 1 dakikaya kadar sürebilir.
> - Kurtarma Hizmetleri Kasası yalnızca yedekleme raporları için bir giriş noktasıdır. Yedekleme raporu çalışma kitabı bir kasadaki bölmeden açıldıktan sonra tüm kasalarınızda toplanan verileri görmek için uygun Log Analytics çalışma alanları kümesini seçin.

Rapor çeşitli sekmeler içerir:

- **Özet**: yedeklemeinize ilişkin üst düzey bir genel bakış almak için bu sekmeyi kullanın. Toplam yedekleme öğesi sayısı, tüketilen toplam bulut depolaması, korunan örnek sayısı ve iş yükü türü başına iş başarısı oranı hakkında hızlı bir bakış edinebilirsiniz. Belirli bir yedekleme yapıtı türü hakkında daha ayrıntılı bilgi için ilgili sekmelere gidin.

   ![Summary (Özet) sekmesi](./media/backup-azure-configure-backup-reports/summary.png)

- **Yedekleme öğeleri**: bir yedekleme öğesi düzeyinde tüketilen Bulut depolamada bilgi ve eğilimleri görmek için bu sekmeyi kullanın. Örneğin, bir Azure VM yedeğine SQL kullanıyorsanız, yedeklenen her SQL veritabanı için kullanılan bulut depolama alanını görebilirsiniz. Ayrıca, belirli bir koruma durumunun yedekleme öğeleri için verileri görmeyi seçebilirsiniz. Örneğin, sekmenin en üstünde **koruma durdurulmuş** kutucuğun seçilmesi, yalnızca koruma durdurulmuş durumundaki yedekleme öğeleri için verileri göstermek üzere altındaki tüm pencere öğelerini filtrelemektir.

   ![Yedekleme öğeleri sekmesi](./media/backup-azure-configure-backup-reports/backup-items.png)

- **Kullanım**: yedeklemelerinize ilişkin anahtar faturalandırma parametrelerini görüntülemek için bu sekmeyi kullanın. Bu sekmede gösterilen bilgiler bir faturalandırma varlığı (korumalı kapsayıcı) düzeyidir. Örneğin, Azure 'a yedeklenmekte olan bir DPM sunucusu söz konusu olduğunda, DPM sunucusu için kullanılan korumalı örneklerin ve bulut depolamanın eğilimini görüntüleyebilirsiniz. Benzer şekilde, Azure Backup ' de SQL kullanıyorsanız veya Azure Backup SAP HANA, bu sekme, bu veritabanlarının bulunduğu sanal makine düzeyinde kullanımla ilgili bilgiler sağlar.

   ![Kullanım sekmesi](./media/backup-azure-configure-backup-reports/usage.png)

> [!NOTE]
> DPM iş yükleri için, kullanıcılar, kurtarma hizmetleri Kasası Genel Bakış sekmesinde gösterilen toplu kullanım değeriyle karşılaştırıldığında, raporlarda gösterilen kullanım değerleri arasında bir hafif fark (DPM sunucusu başına 20 MB) görebilirler. Bu fark, yedekleme için kaydedilen her DPM sunucusunun, raporlama için yapıt olarak ortaya çıkmış olan ilişkili bir ' Metadata ' veri kaynağına sahip olması açısından hesaba göre belirlenir.

- **İşler**: her gün başarısız iş sayısı ve iş hatasının en üst nedenleri gibi işlere yönelik uzun süreli eğilimleri görüntülemek için bu sekmeyi kullanın. Bu bilgileri hem bir toplama düzeyinde hem de yedekleme öğesi düzeyinde görüntüleyebilirsiniz. Seçili zaman aralığında o yedekleme öğesinde tetiklenen her bir işle ilgili ayrıntılı bilgileri görüntülemek için kılavuzda belirli bir yedekleme öğesini seçin.

   ![İşler sekmesi](./media/backup-azure-configure-backup-reports/jobs.png)

- **İlkeler**: ilişkili öğe sayısı ve belirli bir ilke altında yedeklenen öğeler tarafından tüketilen toplam bulut depolama alanı gibi tüm etkin ilkeleriniz hakkındaki bilgileri görüntülemek için bu sekmeyi kullanın. İlişkili yedekleme öğelerinin her biri hakkındaki bilgileri görüntülemek için belirli bir ilkeyi seçin.

   ![İlkeler sekmesi](./media/backup-azure-configure-backup-reports/policies.png)

## <a name="export-to-excel"></a>Excel'e aktar

Bir tablo veya grafik gibi herhangi bir pencere öğesinin sağ üst köşesindeki aşağı ok düğmesini seçin ve bu pencere öğesinin içeriğini bir Excel sayfası olarak, var olan filtrelerin uygulanmış olduğu gibi dışarı aktarın. Bir tablonun daha fazla satırını Excel 'e aktarmak için, her bir kılavuzun en üstünde bulunan **sayfa** aşağı açılan okunu kullanarak sayfada görüntülenecek satır sayısını artırabilirsiniz.

## <a name="pin-to-dashboard"></a>Panoya sabitle

Pencere öğesini Azure portal panonuza sabitlemek için her pencere öğesinin en üstündeki sabitle düğmesini seçin. Bu özellik, ihtiyacınız olan en önemli bilgileri görüntüleyecek şekilde uyarlanmış özelleştirilmiş panolar oluşturmanıza yardımcı olur.

## <a name="cross-tenant-reports"></a>Çapraz kiracı raporları

Çoklu kiracı ortamlarında abonelikler için temsilci erişimi olan [Azure açık Thouse](https://docs.microsoft.com/azure/lighthouse/) kullanırsanız, varsayılan abonelik filtresini kullanabilirsiniz. Verilerini görmek istediğiniz tüm abonelikleri seçmek için Azure portal sağ üst köşesinde bulunan filtre düğmesini seçin. Bunun yapılması, Kiracılarınızın genelinde Log Analytics çalışma alanlarını seçmenizi sağlar ve çok kiracılı raporları görüntüler.

## <a name="conventions-used-in-backup-reports"></a>Yedekleme raporlarında kullanılan kurallar

- Filtreler, her sekmede soldan sağa ve yukarıdan aşağıya doğru çalışır. Diğer bir deyişle, tüm filtreler yalnızca bu filtrenin sağına veya bu filtrenin altına konumlandırılmış olan tüm pencere öğeleri için geçerlidir.
- Renkli bir kutucuk seçilmesi, kutucuğun altındaki pencere öğelerini, bu döşemenin değerine ait olan kayıtlar için filtreler. Örneğin, **yedekleme öğeleri** sekmesindeki **koruma durduruldu** kutucuğunun seçilmesi, aşağıdaki kılavuzların ve grafiklerin koruma durdurulmuş durumda yedekleme öğelerinin verilerini göstermesi için filtre uygular.
- Renksiz olmayan kutucuklar tıklatılabilir değildir.
- Geçerli kısmi güne ait veriler raporlarda gösterilmez. Bu nedenle, **saat aralığının** seçili değeri **son 7 gün**olduğunda, rapor son yedi gün için kayıtları gösterir. Geçerli gün dahil değildir.
- Rapor, seçilen zaman aralığında *tetiklenen* işlerin ayrıntılarını (günlük işlerden ayrı olarak) gösterir.
- **Bulut depolama** ve **korumalı örnekler** için gösterilen değerler seçili zaman aralığının *sonunda* .
- Raporlarda görüntülenen yedekleme öğeleri, seçili zaman aralığının *sonunda* bulunan öğelerdir. Seçilen zaman aralığının ortasında silinen yedekleme öğeleri gösterilmez. Aynı kural yedekleme ilkeleri için de geçerlidir.

## <a name="query-load-times"></a>Sorgu yükleme süreleri

Yedekleme raporundaki pencere öğeleri, kullanıcının Log Analytics çalışma alanlarında çalışan kusto sorguları tarafından desteklenir. Bu sorgular genellikle daha zengin içgörüler sağlamak için birden fazla birleşimle büyük miktarlarda veri işlemeyi içerir. Sonuç olarak, Kullanıcı büyük bir yedekleme genelinde rapor görüntülediğinde pencere öğeleri anında yüklenmeyebilir. Bu tablo, yedekleme öğelerinin sayısına ve raporun görüntülendiği zaman aralığına bağlı olarak, farklı pencere öğelerinin yüklenmeye sürebileceği kabaca bir tahmin sağlar.

| **Veri kaynağı sayısı**                         | **Zaman ufuk** | **Yaklaşık yükleme süreleri**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ~ 5 K                       | 1 ay          | Kutucuklar: 5-10 saniye <br> Kılavuzlar: 5-10 saniye <br> Grafikler: 5-10 saniye <br> Rapor düzeyi filtreleri: 5-10 saniye|
| ~ 5 K                       | 3 ay          | Kutucuklar: 5-10 saniye <br> Kılavuzlar: 5-10 saniye <br> Grafikler: 5-10 saniye <br> Rapor düzeyi filtreleri: 5-10 saniye|
| ~ 10 K                       | 3 ay          | Kutucuklar: 15-20 saniye <br> Kılavuzlar: 15-20 saniye <br> Grafikler: 1-2 dakika <br> Rapor düzeyi filtreleri: 25-30 saniye|
| ~ 15 K                       | 1 ay          | Kutucuklar: 15-20 saniye <br> Kılavuzlar: 15-20 saniye <br> Grafikler: 50-60 saniye <br> Rapor düzeyi filtreleri: 20-25 saniye|
| ~ 15 K                       | 3 ay          | Kutucuklar: 20-30 saniye <br> Kılavuzlar: 20-30 saniye <br> Grafikler: 2-3 dakika <br> Rapor düzeyi filtreleri: 50-60 saniye |

## <a name="what-happened-to-the-power-bi-reports"></a>Power BI raporlarına ne oldu? 

- Azure Storage hesabından veri kaynağı olan raporlama için önceki Power BI şablon uygulaması kullanımdan kaldırma yolunda yer alır. Raporları görüntülemek için Log Analytics kasa Tanılama verileri göndermeye başlayabilmeniz önerilir.

- Ayrıca, bir depolama hesabına veya bir LA çalışma alanına Tanılama verileri göndermenin [v1 şeması](https://docs.microsoft.com/azure/backup/backup-azure-diagnostics-mode-data-model#v1-schema-vs-v2-schema) , kullanımdan kaldırma yolunda da olur. Bu, v1 şemasına göre özel sorgular veya bir sorgu yazdıysanız, bu sorguları şu anda desteklenen v2 şemasını kullanmak için güncelleştirmeniz önerilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Backup ile izleme ve raporlama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-azure-monitor-alert-faq)
