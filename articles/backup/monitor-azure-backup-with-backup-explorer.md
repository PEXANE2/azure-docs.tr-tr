---
title: Yedekleme Gezgini ile yedeklemelerinizi izleme
description: Yedekleme Gezgini 'nin kasaların, aboneliklerin, bölgelerin ve kiracıların tamamında gerçek zamanlı olarak izlenmesini gerçekleştirmek için nasıl kullanılacağını açıklayan bir makale
ms.reviewer: dcurwin
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: 331d8aeeb828dedb6700a94fafa074c179bef7ab
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76992189"
---
# <a name="monitor-your-backups-with-backup-explorer"></a>Yedekleme Gezgini ile yedeklemelerinizi izleme

Kuruluşlar buluta daha fazla veya daha fazla makine yedeklemesinde, yedeklemeleri verimli bir şekilde izlemek için büyük bir emlak genelinde yedeklemeler hakkındaki işletimsel bilgileri görüntülemek için merkezi bir konum olması önemli hale gelir.

Yedekleme Gezgini çalışma kitabı, yedekleme müşterilerinin Azure 'daki tüm yedekleme genelinde yedekleme ile ilgili işlem izleme etkinliklerini gerçekleştirmek için tek bir cam bölmesine sahip olmasını sağlayan yerleşik bir Azure Izleyici çalışma kitabıdır (kapsayıcı kiracılar, konumlar, Abonelikler, kaynak grupları ve Kasaların hepsi merkezi bir yerden yapılır. Yaygın olarak, aşağıdaki özellikleri sağlar:

* **Ölçek perspektifinde:** yedekleme öğelerinin, işlerin, uyarıların, ilkelerin ve kaynakların tamamı boyunca yedekleme için yapılandırılmayan toplu bir görünümü. 
* **Ayrıntıya gitme** : her bir varlığın (işler, uyarılar, ilkeler ve yedekleme öğeleri) tek bir yerden ayrıntılı bilgilerini almayı seçebilirsiniz.
* Eyleme dönüştürülebilir **arabirimler** – bir sorunu tanımladıktan sonra, Yedekleme öğesine veya Azure kaynağına sorunsuz bir şekilde gezinerek eylemler gerçekleştirmeyi seçebilirsiniz.

Yukarıdaki yetenekler Azure Kaynak Graph ve Azure Izleyici çalışma kitapları ile yerel tümleştirme tarafından kullanıma hazır olarak sağlanır.

> [!NOTE]
> * Yedekleme Gezgini Şu anda yalnızca Azure VM verileri için kullanılabilir.
> * Yedekleme Gezgini, son 7 gün içinde yedeklemeleriniz hakkındaki bilgileri (en fazla) görüntülemek için bir işlemsel pano olarak tasarlanmıştır.
> * Şu anda, yedekleme Gezgini şablonunun özelleştirilmesi desteklenmez. Ayrıca, şu anda Azure Kaynak Grafiği verilerinde özel bir tahmin yazmak önermiyoruz.

## <a name="getting-started"></a>Başlangıç

Kurtarma Hizmetleri kasalarınıza giderek ve **genel bakış** sayfasındaki **yedekleme Gezgini** bağlantısına tıklayarak yedekleme Gezgini 'ne erişebilirsiniz.

![Kasa hızlı bağlantısı](media/backup-azure-monitor-with-backup-explorer/vault-quick-link.png)

Bağlantıya tıkladığınızda, erişiminiz olan tüm kasaların ve aboneliklerde toplanmış bir görünüm sağlayan yedekleme Gezgini açılır. Azure açık bir hesabı kullanıyorsanız, erişiminiz olan tüm kiracılar genelinde verileri görüntüleyebilirsiniz (çapraz kiracı görünümlerinde aşağıdaki bölümde daha fazla bilgi bulabilirsiniz).

![Gezgin giriş sayfası](media/backup-azure-monitor-with-backup-explorer/explorer-landing-page.png)

## <a name="backup-explorer-use-cases"></a>Yedekleme Gezgini kullanım örnekleri

Yedekleme Gezgini, her sekmenin belirli bir tür yedekleme yapıtı (örneğin, yedekleme öğeleri, işler, ilkeler vb.) hakkında ayrıntılı bilgi sağlayan birden çok sekmeden oluşur. Bu bölümde, sekmelerin her birine ilişkin kısa bir genel bakış sunulmaktadır. Videolar, sekmelerin her biri için örnek kullanım durumları sağlar ve Kullanıcı için kullanılabilen çeşitli denetimlerin bir açıklamasıyla birlikte.

### <a name="summary"></a>Özet

Özet sekmesi, yedeğinizin genel koşulundaki hızlı bir bakış sağlamanıza olanak sağlar. Korunan öğe sayısı, korumanın etkinleştirilmediği öğe sayısı, son 24 saat içinde kaç iş başarılı olduğunu vb. gibi bilgileri görüntüleyebilirsiniz. 

Diğer sekmelerin her biri farklı bir varlığı temsil eder; örneğin, yedekleme öğeleri, yedekleme Işleri, yedekleme uyarıları ve yedekleme Ilkeleri. Ayrıca, yedeklemenin yapılandırılmadığı makineler hakkındaki bilgileri de görüntüleyebilirsiniz. Bu sekmelerin herhangi birine tıkladığınızda, bu varlığa özgü bilgiler gösterilir.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQYd]

### <a name="backup-items"></a>Yedekleme öğeleri

Abonelik, kasa ve diğer parametrelere göre filtrelenen yedekleme öğelerinizin her birini görüntüleyebilirsiniz. Bir yedekleme öğesinin adına tıklamak, bu yedekleme öğesi için Azure dikey penceresini açmanızı sağlar. Örneğin, tablosundan, ' X ' öğesi için son yedeklemenin başarısız olduğunu gözlemleyebilirsiniz. ' X ' öğesine tıkladığınızda, bu öğe için yedekleme dikey penceresi açılır ve burada isteğe bağlı yedekleme işlemini tetikleyebilirsiniz.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQYc]

### <a name="jobs"></a>İş

Işler sekmesine giderek, son yedi gün içinde tetiklenen tüm işlerin ayrıntılarını görüntüleyebilirsiniz. Burada Iş Işlemine, Iş durumuna ve hata koduna göre filtreleyebilirsiniz (başarısız olan işler söz konusu olduğunda).

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nOrh]

### <a name="alerts"></a>Uyarılar

Son yedi gün içinde, uyarılar sekmesine tıklayarak vakalarınızda tetiklenen tüm uyarıların ayrıntılarını görüntüleyebilirsiniz. Burada, kayıtları uyarının türüne (örneğin, yedekleme hatası, geri yükleme hatası), uyarının geçerli durumunu (örneğin, etkin veya çözümlenmiş) ve uyarı önem derecesini (örneğin, kritik, uyarı, bilgi) göre filtreleyebilirsiniz. Ayrıca, VM bağlantısına tıklayarak Azure VM 'ye gidebilir ve gerekli işlemleri gerçekleştirebilirsiniz.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nTxe]

### <a name="policies"></a>İlkeler

Ilkeler sekmesine tıklanması, yedeklemeinizde oluşturulmuş tüm yedekleme ilkelerine ilişkin önemli bilgileri görüntülemenize olanak sağlar. Her ilkeyle ilişkili kaç öğe olduğunu, her ilke tarafından belirtilen bekletme aralığı ve yedekleme sıklığı ile birlikte görebilirsiniz.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nLKV]

### <a name="backup-not-enabled"></a>Yedekleme etkin değil

Kuruluşun yedekleme yöneticisinin henüz yedeklemenin etkin olmadığı hızlı bir şekilde belirlenmesi önemlidir, böylece koruma gerektiren tüm makineler için yedekleme etkinleştirilebilir. **Etkin** sekmesine tıklanması, bu görevde size yardımcı olur.

Bu sekmede, korunmayan öğelerin listesini içeren bir tablo görürsünüz. Kuruluşunuz üretim makinelerine ve test makinelerine farklı Etiketler atama ya da her biri ayrı bir yedekleme ilkesi gerektiren farklı işlevlere hizmet eden makineler için bir uygulama kullanıyorsa, etiketlere göre filtreleme, bir öğesine özgü bilgileri görüntülemenize yardımcı olur. belirli makine sınıfları. Herhangi bir öğenin adına tıkladığınızda bu öğe için **yedekleme yapılandırma** dikey penceresine yönlendirilirsiniz. Bu öğe, uygun bir yedekleme ilkesiyle yedeklemeyi tercih edebilirsiniz.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQXZ]

## <a name="exporting-to-excel"></a>Excel 'e verme

Herhangi bir pencere öğesinin sağ üst köşesindeki aşağı ok düğmesine tıklamak (tablo/grafik), varolan filtrelerin uygulanmış olduğu gibi bu pencere öğesinin içeriğini Excel sayfası olarak dışa aktarır. Bir tablonun daha fazla satırını Excel 'e aktarmak için, her sekmenin en üstünde bulunan **sayfa başına satır** açılan listesini kullanarak sayfada görüntülenecek satır sayısını artırabilirsiniz.

## <a name="pinning-to-dashboard"></a>Panoya sabitleme

Bu pencere öğesini Azure portal panonuza sabitlemek için her pencere öğesinin en üstündeki sabitleme simgesine tıklayabilirsiniz. Bu, ihtiyacınız olan en önemli bilgileri görüntüleyecek şekilde uyarlanmış özelleştirilmiş panolar oluşturmanıza yardımcı olur.

## <a name="cross-tenant-views"></a>Çapraz kiracı görünümleri

Çoklu kiracı ortamlarında abonelikler için temsilci erişimi olan bir Azure ışıklı Kullanıcı kullanıyorsanız, istediğiniz tüm abonelikleri seçmek için varsayılan abonelik filtresini (Azure portal en sağ üst köşesindeki filtre simgesine tıklayarak) kullanabilirsiniz. için verileri görüntüleyin. Bunun yapılması, yedekleme Gezgini 'nin bu aboneliklerdeki tüm kasalardaki bilgileri toplamasını sağlar. [Burada](https://docs.microsoft.com/azure/lighthouse/overview)Azure ışıklı kullanım hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki Adımlar

[Yedekleme verileriniz hakkında öngörü almak için Azure Izleyici 'yi nasıl kullanacağınızı öğrenin](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)