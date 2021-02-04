---
title: Depolama için Azure Defender-avantajlar ve Özellikler
description: Depolama için Azure Defender 'ın avantajları ve özellikleri hakkında bilgi edinin.
author: memildin
ms.author: memildin
ms.date: 02/04/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: eb1635cec2b0bcf7f2c13101b2aeab25a869dc66
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99558548"
---
# <a name="introduction-to-azure-defender-for-storage"></a>Depolama için Azure Defender'a giriş

**Depolama Için Azure Defender** , depolama hesaplarınıza yönelik olağan dışı ve potansiyel olarak zararlı girişimleri algılayan bir Azure Native Security Intelligence katmanıdır. Bağlamsal güvenlik uyarıları ve önerileri sağlamak için güvenlik AI ve [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684) 'ın gelişmiş özelliklerinden yararlanır.

Etkinlikte anomali oluştuğunda güvenlik uyarıları tetiklenir. Bu uyarılar Azure Güvenlik Merkezi ile tümleşiktir ve ayrıca, şüpheli etkinliklerin ayrıntıları ve tehditleri İnceleme ve düzeltme önerileri ile abonelik yöneticilerine e-posta yoluyla da gönderilir.

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel kullanılabilirlik (GA)|
|Fiyat|**Depolama Için Azure Defender** , [fiyatlandırma sayfasında](security-center-pricing.md) gösterildiği gibi faturalandırılır|
|Korumalı Depolama türleri:|[Blob Depolama](https://azure.microsoft.com/services/storage/blobs/)<br>[Azure Dosyaları](../storage/files/storage-files-introduction.md)<br>[Azure Data Lake Storage 2. Nesil](../storage/blobs/data-lake-storage-introduction.md)|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) US Gov<br>![No](./media/icons/no-icon.png) Çin gov, diğer gov|
|||


## <a name="what-are-the-benefits-of-azure-defender-for-storage"></a>Depolama için Azure Defender 'ın yararları nelerdir?

Depolama için Azure Defender şunları sağlar:

- **Azure-Native Security** -1-etkinleştirme ' ye tıklayarak depolama için Defender, Azure blob, Azure dosyaları ve veri Lakes 'de depolanan verileri korur. Azure-Native hizmeti olarak, depolama için Defender, Azure tarafından yönetilen tüm veri varlıkları genelinde merkezi güvenlik sağlar ve Azure Sentinel gibi diğer Azure Güvenlik hizmetleriyle tümleşiktir.
- Microsoft Threat Intelligence tarafından desteklenen **zengin algılama paketi** -depolama için Defender 'daki algılamalar, anonim erişim, güvenliği aşılmış kimlik bilgileri, sosyal mühendislik, ayrıcalık kötüye kullanımı ve kötü amaçlı içerik gibi en üst depolama tehditleri kapsar.
- **Ölçek** Güvenlik Merkezi 'nin Otomasyon araçlarındaki yanıt, belirlenen tehditleri önlemeye ve bunlara yanıt vermeye daha kolay hale getirir. [Güvenlik Merkezi tetikleyicilerine yönelik yanıtları otomatikleştirmede](workflow-automation.md)daha fazla bilgi edinin.

:::image type="content" source="media/defender-for-storage-introduction/defender-for-storage-high-level-overview.png" alt-text="Depolama için Azure Defender özelliklerine üst düzey genel bakış":::


## <a name="what-kind-of-alerts-does-azure-defender-for-storage-provide"></a>Azure Defender, depolama için ne tür uyarılar sağlar?

Güvenlik uyarıları şunları yaparken tetiklenir:

- Bir Tor çıkış düğümünden veya Microsoft Threat Intelligence tarafından şüpheli olarak kabul edilen bir IP 'den başarılı erişim gibi **şüpheli erişim desenleri**
- **Şüpheli etkinlikler** -anormal veri ayıklama veya olağan dışı erişim izinleri değişikliği
- Kötü amaçlı yazılım dosyaları (karma saygınlığı analizine göre) veya kimlik avı içeriğinin barındırılması gibi **kötü amaçlı Içeriğin karşıya yüklenmesi**

Uyarılar, bunları tetikleyen olayın ayrıntılarını ve tehditleri İnceleme ve düzeltme önerilerini içerir. Uyarılar, Azure Sentinel 'e veya diğer üçüncü taraf SıEM 'ye veya başka bir dış araca aktarılabilir.

> [!TIP]
> Azure Defender 'ı abonelik düzeyinde [depolama için yapılandırmak](../storage/common/azure-defender-storage-configure.md?tabs=azure-security-center) en iyi uygulamadır, ancak aynı zamanda [tek bir depolama hesabında da yapılandırabilirsiniz](../storage/common/azure-defender-storage-configure.md?tabs=azure-portal).


## <a name="what-is-hash-reputation-analysis-for-malware"></a>Kötü amaçlı yazılım için karma saygınlığı analizi nedir?

Karşıya yüklenen bir dosyanın şüpheli olup olmadığını öğrenmek için depolama için Azure Defender, [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684)tarafından desteklenen karma saygınlığı analizini kullanır. Tehdit koruması araçları karşıya yüklenen dosyaları taramaz, böylece depolama günlüklerini inceler ve yeni yüklenen dosyaların karmalarını, bilinen virüsler, Truva atları, casus yazılım ve fidye ile karşılaştırın. 

Bir dosyanın kötü amaçlı yazılım içermesi şüpheli olduğu durumlarda, güvenlik merkezi bir uyarı görüntüler ve isteğe bağlı olarak, şüpheli dosyayı silmek üzere depolama sahibini onay için e-posta ile gönderebilir. Karma saygınlığı analizinin kötü amaçlı yazılım içerdiğini gösterdiği dosyaların bu otomatik olarak kaldırılmasını ayarlamak için, ["depolama hesabına yüklenen potansiyel kötü amaçlı yazılımlar" içeren uyarılarda tetiklemek üzere bir iş akışı Otomasyonu](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-respond-to-potential-malware-uploaded-to-azure-storage/ba-p/1452005)dağıtın.

> [!NOTE]
> Güvenlik Merkezi 'nin tehdit koruması yeteneklerini etkinleştirmek için, ilgili iş yüklerini içeren abonelikte Azure Defender 'ı etkinleştirmeniz gerekir.
>
> **Depolama Için Azure Defender 'ı** abonelik düzeyinde veya kaynak düzeyinde etkinleştirebilirsiniz.

## <a name="trigger-a-test-alert-for-azure-defender-for-storage"></a>Depolama için Azure Defender için bir test uyarısı tetikleyin

Ortamınızdaki depolama için Azure Defender 'daki güvenlik uyarılarını test etmek için, aşağıdaki adımlarla "bir Tor çıkış düğümünden depolama hesabına erişim" uyarısını oluşturun:

1. Depolama için Azure Defender 'ı etkinleştirmek üzere bir depolama hesabı açın.
1. Kenar çubuğundan "kapsayıcılar" ı seçin ve var olan bir kapsayıcıyı açın veya yeni bir kapsayıcı oluşturun.

    :::image type="content" source="media/defender-for-storage-introduction/opening-storage-container.png" alt-text="Azure depolama hesabından bir blob kapsayıcısı açma" lightbox="media/defender-for-storage-introduction/opening-storage-container.png":::

1. Bu kapsayıcıya bir dosya yükleyin.

    > [!CAUTION]
    > Gizli veriler içeren bir dosyayı karşıya yüklemeyin.

1. "SAS oluştur" seçeneğini belirlemek için karşıya yüklenen dosyadaki bağlam menüsünü kullanın.

    :::image type="content" source="media/defender-for-storage-introduction/generate-sas.png" alt-text="Blob kapsayıcısındaki bir dosya için SAS oluştur seçeneği":::

1. Varsayılan seçenekleri bırakın ve **SAS belirteci ve URL Oluştur '** u seçin.

1. Oluşturulan SAS URL 'sini kopyalayın.

1. Yerel makinenizde, Tor tarayıcısını açın.

    > [!TIP]
    > Tor proje sitesinden Tor indirebilirsiniz [https://www.torproject.org/download/](https://www.torproject.org/download/) .

1. Tor tarayıcısında SAS URL 'sine gidin.

1. Adım 3 ' te karşıya yüklediğiniz dosyayı indirin.

    İki saat içinde güvenlik merkezi 'nden aşağıdaki güvenlik uyarısını alırsınız:

    :::image type="content" source="media/defender-for-storage-introduction/tor-access-alert-storage.png" alt-text="Tor çıkış düğümünden erişimle ilgili güvenlik uyarısı":::

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, depolama için Azure Defender hakkında bilgi edindiniz.

İlgili malzemeler için aşağıdaki makalelere bakın: 

- Bir uyarının Güvenlik Merkezi tarafından oluşturulup oluşturulmadığı veya Güvenlik Merkezi tarafından farklı bir güvenlik ürününden alınıp alınmayacağı, dışarı aktarabilirsiniz. Uyarılarınızı Azure Sentinel 'e, herhangi bir üçüncü taraf SıEM 'e veya herhangi bir harici araca aktarmak için [uyarıları BIR SıEM 'ye aktarma](continuous-export.md)konusundaki yönergeleri izleyin.
- [Depolama için gelişmiş Defender 'ı etkinleştirme](../storage/common/azure-defender-storage-configure.md)
- [Depolama uyarıları için Azure Defender listesi](alerts-reference.md#alerts-azurestorage)
- [Microsoft 'un tehdit bilgileri özellikleri](https://go.microsoft.com/fwlink/?linkid=2128684)