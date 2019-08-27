---
title: 'Hızlı başlangıç: Microsoft Azure Data Box Disk | Microsoft Docs'
description: Azure Data Box Disk'inizi Azure portalda hızlıca dağıtmak için bu hızlı başlangıçtan faydalanın
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: quickstart
ms.date: 08/26/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to quickly deploy Data Box Disk so as to import data into Azure.
ms.openlocfilehash: 9664fd35462ab6084b203fe4ccd3c10435d64991
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70035632"
---
::: zone target="docs"

# <a name="quickstart-deploy-azure-data-box-disk-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portal kullanarak Azure Data Box Disk dağıtma

::: zone-end

::: zone target="chromeless"

# <a name="get-started-with-azure-data-box-disk-using-azure-portal"></a>Azure portal kullanarak Azure Data Box Disk kullanmaya başlama

::: zone-end

::: zone target="docs"

Bu hızlı başlangıçta Azure portalı kullanarak Azure Data Box Disk'i dağıtma adımları anlatılmaktadır. Bu adımlar sipariş oluşturma, diskleri alma, paketini açma, bağlama, disklere veri kopyalama ve Azure'a yükleme işlemlerini kapsamaktadır.

Ayrıntılı adım adım dağıtım ve izleme yönergeleri için [Öğreticiye gidin: Order Azure Data Box Disk](data-box-disk-deploy-ordered.md). 

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

::: zone-end

::: zone target="chromeless"

Bu hızlı başlangıçta Azure portalı kullanarak Azure Data Box Disk'i dağıtma adımları anlatılmaktadır. Adımlar, Azure 'a yüklenecek şekilde Önkoşulları Gözden geçirme, disklerin kilidini açma, disklere bağlanma ve verileri kopyalama işlemlerini içerir.

::: zone-end

::: zone target="docs"

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

- Aboneliğinizde Azure Data Box hizmetinin etkinleştirildiğinden emin olun. Bu hizmeti aboneliğinizde etkinleştirmek için bkz. [Hizmete kaydolma](https://aka.ms/azuredataboxfromdiskdocs).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://aka.ms/azuredataboxfromdiskdocs](https://aka.ms/azuredataboxfromdiskdocs) adresinden Azure portalında oturum açın.

::: zone-end

::: zone target="chromeless"

## <a name="prerequisites"></a>Önkoşullar

- [Öğreticiyi kullanarak Data Box disk siparişinizi yerleştirdiniz: Order Azure Data Box Disk](data-box-disk-deploy-ordered.md).
- Disklerinizi aldınız ve portaldaki iş durumu **Teslim Edildi** olarak güncelleştirildi.
- Aldığınız disk sayısını ve disk başına bir bağlantı kablosu olduğunu denetlediyseniz.
- Verileri kopyalayabilmeniz için kullanabileceğiniz bir istemci bilgisayarınız var. İstemci bilgisayarınızda:

    - Desteklenen bir [işletim sistemi](data-box-disk-system-requirements.md#supported-operating-systems-for-clients)çalıştırın.
    - Bir Windows istemcisi ise, [diğer gerekli yazılımların](data-box-disk-system-requirements.md#other-required-software-for-windows-clients) yüklü olmasını gerektirir.

::: zone-end

::: zone target="docs"

## <a name="order"></a>Sipariş verme

Bu adım yaklaşık 5 dakika sürer.

1. Azure portalda yeni bir Azure Data Box kaynağı oluşturun. 
2. Bu hizmetin etkinleştirildiği bir aboneliği seçin ve aktarım türünü **İçeri aktarma** olarak belirleyin. Verilerin bulunduğu **Kaynak ülkeyi** ve veri aktarımı için **Azure hedef bölgesini** seçin.
3. **Data Box Disk**'i seçin. Maksimum çözüm kapasitesi 35 TB olarak belirlenmiştir ve daha büyük veriler için birden fazla disk siparişi oluşturabilirsiniz.  
4. Sipariş ayrıntılarını ve sevkiyat bilgilerini girin. Hizmet bölgenizde kullanılabilir durumdaysa bildirim e-posta adreslerini girin, özeti gözden geçirin ve siparişi oluşturun.

Sipariş oluşturulduktan sonra diskler gönderilmek üzere hazırlanır.

## <a name="unpack"></a>Paketi açma

Bu adım yaklaşık 5 dakika sürer.

Data Box Disk, UPS Express Box içinde gönderilir. Kutuyu açın ve içinde şunların bulunduğundan emin olun:

- Baloncuklu ambalaja sarılı 1 ile 5 USB disk.
- Disk başına bir bağlantı kablosu.
- İade için sevkiyat etiketi.

## <a name="connect-and-unlock"></a>Bağlama ve kilidini açma

Bu adım yaklaşık 5 dakika sürer.

1. Diski desteklenen bir işletim sisteminin çalıştırıldığı Windows/Linux bilgisayarına bağlamak için, verilen kabloyu kullanın. Desteklenen işletim sistemi sürümleri hakkında daha fazla bilgi için bkz. [Azure Data Box Disk sistem gereksinimleri](data-box-disk-system-requirements.md). 
2. Disk kilidini açmak için:

    1. Azure portalında **Genel > Cihaz Ayrıntıları**'na gidin ve destek anahtarını alın.
    2. İşletim sistemine özgü Data Box Disk kilit açma aracını disklere veri kopyalamak için kullanılacak bilgisayara indirin ve ayıklayın. 
    3. Data Box Disk kilit açma aracını çalıştırın ve destek anahtarını sağlayın. Yeniden takılan tüm diskler için kilit açma aracını tekrar çalıştırın ve destek anahtarını sağlayın. **Disk kilidini açmak için BitLocker iletişim kutusunu veya BitLocker anahtarını kullanmayın.** Disklerin kilidini açma hakkında daha fazla bilgi için [Windows istemcisinde disklerin kilidini açma](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client) veya [Linux Istemcisindeki disklerin kilidini açma](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client)sayfasına gidin.
    4. Diske araç tarafından atanan sürücü harfi görüntülenir. Disk sürücü harfini not edin. Bu harf sonraki adımlarda kullanılacaktır.

## <a name="copy-data-and-validate"></a>Verileri kopyalama ve doğrulama

Bu işlemi tamamlamak için gereken süre verilerinizin boyutuna göre değişir.

1. Sürücü, *Pageblob*, *blockblob*, *AzureFile*, *manageddisk*ve *databoxdiskımport* klasörlerini içerir. Blok blobu olarak içeri aktarılması gereken verileri sürükleyip *BlockBlob* klasörüne bırakın. Benzer şekilde, VHD/VHDX gibi verileri *Pageblob* klasörüne sürükleyip bırakın ve *AzureFile*'e uygun verileri sürükleyin. Yönetilen diskler olarak karşıya yüklemek istediğiniz VHD 'leri *Manageddisk*altındaki bir klasöre kopyalayın.

    *BlockBlob* ve *PageBlob* klasörlerinin altındaki her klasör için Azure depolama hesabında bir kapsayıcı oluşturulur. *AzureFile*altında bir alt klasör için dosya paylaşma oluşturulur.

    *BlockBlob* ve *PageBlob* klasörlerinin altındaki tüm dosyalar Azure Depolama hesabındaki varsayılan `$root` kapsayıcısına kopyalanır. Dosyaları *AzureFile*içindeki bir klasöre kopyalayın. Doğrudan *AzureFile* klasörüne kopyalanan tüm dosyalar başarısız olur ve blok Blobları olarak karşıya yüklenir.

    > [!NOTE]
    > - Tüm kapsayıcılar, Bloblar ve dosyalar [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions)uymalıdır. Bu kurallara uyulmaması halinde veriler Azure'a yüklenemez.
    > - Dosyaların blok Blobları için ~ 4,75 TiB, sayfa Blobları için yaklaşık 8 TiB ve Azure dosyaları için ~ 1 TiB 'yi aşmadığından emin olun.

2. **(Isteğe bağlı ancak önerilir)** Kopyalama işlemi tamamlandıktan sonra, `DataBoxDiskValidation.cmd` *veri boxdiskımport* klasöründe sağlanmış olan en az ' ı çalıştırmanızı ve dosyaları doğrulamak için 1 seçeneğini seçmenizi önemle öneririz. Ayrıca, bu sürenin izin verdiğimiz, doğrulama için sağlama toplamı oluşturmak için 2 seçeneğini kullanmanızı öneririz (veri boyutuna bağlı olarak zaman alabilir). Bu adımlar, verileri Azure 'a yüklerken oluşan hataların olasılığını en aza indirir.
3. Sürücüyü güvenle kaldırın.

## <a name="ship-to-azure"></a>Azure'a gönderme

Bu adımın tamamlanması yaklaşık 5-7 dakika sürer.

1. Tüm diskleri orijinal paketine yerleştirin. Paketle gönderilen sevkiyat etiketini kullanın. Etiket hasar gördüyse veya kaybolduysa portaldan indirin. **Genel bakış**'a gidin ve komut çubuğundan **Sevkiyat etiketini indirin**'e tıklayın.
2. Kapattığınız paketi kargoya verin.  

Data Box Disk hizmeti bir e-posta bildirimi gönderir ve Azure portalında sipariş durumunu güncelleştirir.

## <a name="verify-your-data"></a>Verilerinizi doğrulama

Bu işlemi tamamlamak için gereken süre verilerinizin boyutuna göre değişir.

1. Data Box Disk, Azure veri merkezi ağına bağlandığında veriler otomatik olarak Azure'a yüklenir.
2. Veri kopyalama işlemi tamamlandığında Azure Data Box hizmeti Azure portaldan bildirim gönderir.
    
    1. Hata günlüklerini kontrol ederek hata olup olmadığını kontrol edin ve gerekli eylemleri gerçekleştirin.
    2. Kaynaktan silmeden önce verilerinizin depolama hesaplarında olduğundan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu adımın tamamlanması 2-3 dakika sürer.

Temizlemek için Data Box siparişini iptal edebilir ve ardından silebilirsiniz.

- Data Box siparişini işleme alınmadan önce Azure portaldan iptal edebilirsiniz. Siparişler işleme alındıktan sonra iptal edilemez. Sipariş, tamamlanma aşamasına gelene kadar ilerler.

    Siparişi iptal etmek için **Genel bakış**'a gidin ve komut çubuğundan **İptal**'e tıklayın.  

- Siparişin durumu **Tamamlandı** veya **İptal edildi** olduğunda Azure portaldan silebilirsiniz.

    Siparişi silmek için **Genel bakış**'a gidin ve komut çubuğundan **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure'a veri aktarımı konusunda yardım almak için Azure Data Box Disk'i dağıttınız. Azure Data Box Disk yönetimi hakkında daha fazla bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Azure portalı kullanarak Data Box Disk'i yönetme](data-box-portal-ui-admin.md)

::: zone-end
