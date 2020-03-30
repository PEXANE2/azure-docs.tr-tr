---
title: Yedekleme Gezgini ile yedeklemelerinizi izleme
description: Bu makalede, tonozlar, abonelikler, bölgeler ve kiracılar arasında yedeklemegerçek zamanlı izleme gerçekleştirmek için Yedekleme Gezgini nasıl kullanılacağı açıklanmaktadır.
ms.reviewer: dcurwin
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: fa30a061dfe0d9f7721bd2405280f8a01bea87fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131801"
---
# <a name="monitor-your-backups-with-backup-explorer"></a>Yedekleme Gezgini ile yedeklemelerinizi izleme

Kuruluşlar buluta giderek daha fazla makine yedeklemedikçe, bu yedeklemeleri verimli bir şekilde izlemek giderek daha önemli hale gelir. Başlamak için en iyi yolu büyük bir emlak genelinde operasyonel bilgileri görüntülemek için merkezi bir konum kullanmaktır.

Backup Explorer, Azure Yedekleme müşterilerine bu tek ve merkezi konumu sağlayan yerleşik bir Azure Monitor çalışma kitabıdır. Yedekleme Gezgini, azure'daki tüm Yedekleme mülkünde, kiracılara, konumlara, aboneliklere, kaynak gruplarını ve kasaları kapsayan operasyonel etkinlikleri izlemenize yardımcı olur. Genel olarak, Yedekleme Gezgini aşağıdaki özellikleri sağlar:

* **Ölçekperspektifi**: Tüm mülkte yedekleme için henüz yapılandırılmamış yedekleme öğelerinin, işlerin, uyarıların, ilkelerin ve kaynakların toplu bir görünümünü alın. 
* **Ayrıntılı çözümleme**: İşlerinizin her biri hakkında ayrıntılı bilgileri, uyarıları, ilkeleri ve yedekleme öğelerini tek bir yerde görüntüleyin.
* **İşleme Edilebilir arabirimler**: Bir sorunu tanımladıktan sonra, ilgili yedekleme öğesine veya Azure kaynağına sorunsuz bir şekilde giderek sorunu çözebilirsiniz.

Bu özellikler, Azure Kaynak Grafiği ve Azure Monitor çalışma kitaplarıyla yerel tümleştirme yle kutudan uzak olarak sağlanır.

> [!NOTE]
> * Backup Explorer şu anda yalnızca Azure sanal makineleri (VM) verileri için kullanılabilir.
> * Yedekleme Gezgini, son 7 gün içinde (maksimum) yedeklemeleriniz hakkındaki bilgileri görüntülemek için bir operasyon panosu olmalıdır.
> * Yedekleme Gezgini şu anda ulusal bulutlarda desteklenmez.
> * Şu anda, Yedekleme Gezgini şablonu özelleştirme desteklenmez. 
> * Azure Kaynak Grafiği verilerine özel otomasyonlar yazmanızı önermiyoruz.

## <a name="get-started"></a>Kullanmaya başlayın

Yedekleme Hizmetleri kasanızdan herhangi birini seçerek ve **Genel Bakış** bölmesinde Yedek Gezgin bağlantısını seçerek **Yedekleme Gezgini'ne** erişebilirsiniz.

![Vault hızlı bağlantı](media/backup-azure-monitor-with-backup-explorer/vault-quick-link.png)

Bağlantıyı seçmek, erişebildiğiniz tüm kasalar ve abonelikler arasında toplu bir görünüm sağlayan Yedek Gezgin'i açar. Bir Azure Deniz Feneri hesabı kullanıyorsanız, erişebildiğiniz tüm kiracılardaki verileri görüntüleyebilirsiniz. Daha fazla bilgi için, bu makalenin sonundaki "Kiracı görüşleri çapraz görünümleri" bölümüne bakın.

![Yedek Explorer açılış sayfası](media/backup-azure-monitor-with-backup-explorer/explorer-landing-page.png)

## <a name="backup-explorer-use-cases"></a>Yedek Explorer kullanım örnekleri

Yedek Gezgin, her biri belirli bir yedekleme yapı (örneğin, bir yedek öğe, iş veya ilke) hakkında ayrıntılı bilgi sağlayan birden çok sekme görüntüler. Bu bölümde sekmelerin her biri için kısa bir genel bakış sağlar. Videolar, her yedek yapı için örnek kullanım örnekleri nin yanı sıra kullanılabilir denetimlerin açıklamalarını sağlar.

### <a name="the-summary-tab"></a>Özet sekmesi

**Özet** sekmesi, yedekleme gayrimenkulünüzün genel durumuna hızlı bir bakış sağlar. Örneğin, korunan öğe sayısını, koruma nın etkinleştirilmemiş madde sayısını veya son 24 saat içinde kaç işin başarılı olduğunu görebilirsiniz.


> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQYd]

### <a name="the-backup-items-tab"></a>Yedekleme Öğeleri sekmesi

Yedekleme öğelerinizin her birini abonelik, kasa ve diğer özelliklere göre filtreleyebilir ve görüntüleyebilirsiniz. Yedek öğenin adını seçerek, bu öğe için Azure bölmesini açabilirsiniz. Örneğin, tablodan, son yedekleme *nin X*öğesi için başarısız olduğunu gözlemleyebilirsiniz. *X'i*seçerek, isteğe bağlı yedekleme işlemini tetikleyebileceğiniz öğenin **Yedekleme** bölmesini açabilirsiniz.


> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQYc]

### <a name="the-jobs-tab"></a>İşler sekmesi

Son 7 gün içinde tetiklenen tüm işlerin ayrıntılarını görüntülemek için **İşler** sekmesini seçin. Burada, *İş İşleyişi,* *İş Durumu*ve Hata *Kodu* (başarısız işler için) tarafından filtreleyebilirsiniz.


> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nOrh]

### <a name="the-alerts-tab"></a>Uyarılar sekmesi

Son 7 gün içinde kasalarınızda oluşturulan tüm uyarıların ayrıntılarını görüntülemek için **Uyarılar** sekmesini seçin. Uyarıları türüne göre filtreleyebilirsiniz (*Yedekleme Hatası* veya *Geri Yükleme Hatası*), geçerli durum (*Etkin* veya *Çözümlü*), ve önem derecesi (*Kritik*, *Uyarı*, veya *Bilgi*). Ayrıca Azure VM'ye gitmek ve gerekli herhangi bir eylemi yapmak için bir bağlantı seçebilirsiniz.


> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nTxe]

### <a name="the-policies-tab"></a>İlkeler sekmesi

Yedekleme mülkünzde oluşturulan tüm yedekleme ilkeleriyle ilgili önemli bilgileri görüntülemek için **İlkeler** sekmesini seçebilirsiniz. İlke tarafından belirtilen bekletme aralığı ve yedekleme sıklığıyla birlikte her ilkeyle ilişkili öğe sayısını görüntüleyebilirsiniz.


> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nLKV]

### <a name="the-backup-not-enabled-tab"></a>Yedekleme Etkin değil sekmesi

Koruma gerektiren tüm makineler için yedekleme etkinleştirilmelidir. Yedekleme Gezgini ile yedek yöneticiler, bir kuruluştaki hangi makinelerin henüz yedekleme tarafından korunmadığını hızla belirleyebilir. Bu bilgileri görüntülemek için **Etkin Olmayan Yedekleme** sekmesini seçin.

**Yedek Etkin değil** bölmesi, korumasız makinelerin listesini içeren bir tablo görüntüler. Kuruluşunuz üretim makinelerine ve test makinelerine veya çeşitli işlevlere hizmet veren makinelere farklı etiketler atayabilir. Her makine sınıfının ayrı bir yedekleme ilkesine ihtiyacı olduğundan, etiketlere göre filtreleme, her bir inanca özgü bilgileri görüntülemenize yardımcı olur. Herhangi bir makinenin adını seçmek, sizi uygun bir yedekleme ilkesi uygulamayı seçebileceğiniz makinenin **ConfigurE Backup** bölmesine yönlendirir.


> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQXZ]

## <a name="export-to-excel"></a>Excel'e aktar

Herhangi bir tablonun veya grafiğin içeriğini Excel elektronik tablosu olarak dışa aktarabilirsiniz. İçeriği, varolan filtrelerinizin uygulandığı gibi dışa aktarılır. Ek tablo satırları dışa aktarmak için, her sekonun üst kısmındaki **Sayfa Başına Satırlar** açılır listesini kullanarak sayfada görüntülenecek satır sayısını artırabilirsiniz.

## <a name="pin-to-the-dashboard"></a>Panoya sabitleme

Azure portal panonuza sabitlemek için her tablonun veya grafiğin üst kısmındaki "pin" simgesini seçebilirsiniz. Bu bilgileri sabitlemek, sizin için en önemli bilgileri görüntülemek için özelleştirilmiş bir pano oluşturmanıza yardımcı olur.

## <a name="cross-tenant-views"></a>Kiracı lar arası görünümler

Birden çok kiracı ortamlarında aboneliklere yetkili erişimi olan bir Azure Deniz Feneri kullanıcısıysanız, varsayılan abonelik filtresini kullanabilirsiniz. Azure portalının sağ üst kısmındaki "filtre" simgesini seçerek verileri görmek istediğiniz abonelikleri görüntülersiniz. Bu özelliği kullandığınızda, Backup Explorer seçili aboneliklerinizdeki tüm kasalar hakkındaki bilgileri toplar. Daha fazla bilgi için Azure [Deniz Feneri nedir?](https://docs.microsoft.com/azure/lighthouse/overview)

## <a name="next-steps"></a>Sonraki adımlar

[Yedekleme verileriniz hakkında bilgi edinmek için Azure Monitörünü nasıl kullanacağınızı öğrenin](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)
