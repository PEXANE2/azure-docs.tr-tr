---
title: Dosya paylaşımı içeriğini SharePoint Online'a taşımak için Azure Veri Kutusu Ağır'ı kullanın
description: Azure Veri Kutusu Ağır'ınızı kullanarak dosya paylaşımı içeriğini Share Point Online'a nasıl geçirilir öğrenmek için bu öğreticiyi kullanın
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/18/2019
ms.author: alkohli
ms.openlocfilehash: f97ea17551d4415f7ed6371853172cfde30fe4b6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77560057"
---
# <a name="use-the-azure-data-box-heavy-to-migrate-your-file-share-content-to-sharepoint-online"></a>Dosya paylaşım içeriğinizi SharePoint Online'a geçirmek için Azure Veri Kutusu Heavy'yi kullanın

Dosya paylaşım içeriğinizi SharePoint Online ve OneDrive'a kolayca geçirmek için Azure Veri Kutusu Ağır'ınızı ve SharePoint Geçiş Aracınızı (SPMT) kullanın. Data Box Heavy'yi kullanarak, verileri aktarmak için Geniş Alan ağı (WAN) bağlantınızdaki bağımlılığı kaldırabilirsiniz.

Microsoft Azure Veri Kutusu, Microsoft Azure portalından bir cihaz sipariş etmenizi sağlayan bir hizmettir. Daha sonra sunucularınızdan aygıta terabaytlara kadar veri kopyalayabilirsiniz. Microsoft'a geri gönderildikten sonra verileriniz Azure'a kopyalanır. Aktarmak istediğiniz verilerin boyutuna bağlı olarak aşağıdakilerden birini seçebilirsiniz:

- Küçük ve orta veri kümeleri için sipariş başına 35-TB kullanılabilir kapasiteye sahip [Veri Kutusu Diski.](https://docs.microsoft.com/azure/databox/data-box-disk-overview)
- Orta ve büyük veri kümeleri için cihaz başına 80 TB kullanılabilir kapasiteye sahip [Veri Kutusu.](https://docs.microsoft.com/azure/databox/data-box-overview)
- Büyük veri kümeleri için cihaz başına 770-TB kullanılabilir kapasiteye sahip [Veri Kutusu Ağır.](https://docs.microsoft.com/azure/databox/data-box-heavy-overview)

Bu makalede, dosya paylaşım içeriğinizi SharePoint Online'a geçirmek için Veri Kutusu Heavy'nin nasıl kullanılacağı özellikle anlatIlir.

## <a name="requirements-and-costs"></a>Gereksinimler ve maliyetler

### <a name="for-data-box-heavy"></a>Data Box Heavy için

- Data Box Heavy yalnızca Kurumsal Sözleşme (EA), Bulut çözüm sağlayıcısı (CSP) veya Azure sponsorluk teklifleri için kullanılabilir. Aboneliğiniz yukarıdaki türlerden herhangi birinde düşmüyorsa, aboneliğinizi yükseltmek için Microsoft Destek'e başvurun veya [Azure abonelik fiyatlandırmasını](https://azure.microsoft.com/pricing/)görün.
- Data Box Heavy'yi kullanmanın bir ücreti vardır. [Data Box Heavy fiyatlandırmasını](https://azure.microsoft.com/pricing/details/databox/heavy/)gözden geçirdiğinizden emin olun.


### <a name="for-sharepoint-online"></a>SharePoint Online için

- [SharePoint Geçiş Aracı (SPMT) için Minimum gereksinimleri gözden geçirin.](https://docs.microsoft.com/sharepointmigration/how-to-use-the-sharepoint-migration-tool)

## <a name="workflow-overview"></a>İş akışına genel bakış

Bu iş akışı, Azure Veri Kutusu Ağır'ın yanı sıra SharePoint Online'da da adımlar gerçekleştirmenizi gerektirir.
Aşağıdaki adımlar Azure Veri Kutusu Heavy'nizle ilgilidir.

1. Azure Data Box Heavy sipariş etme’yi tamamladınız.
2. Cihazınızı alın ve ayarlayın.
3. Şirket içi dosya paylaşımınızdaki verileri cihazınızdaki Azure Dosyaları için klasöre kopyalayın.
4. Kopya tamamlandıktan sonra, talimatları uyarınca cihazı geri gönderin.
5. Verilerin Azure'a tamamen yüklenmesini bekleyin.

Aşağıdaki adımlar SharePoint Online ile ilgilidir.

6. Azure portalında bir VM oluşturun ve Azure dosya paylaşımını üzerine monte edin.
7. Azure VM'de SPMT aracını yükleyin.
8. *Kaynak*olarak Azure dosya paylaşımını kullanarak SPMT aracını çalıştırın.
9. Aracın son adımlarını tamamlayın.
10. Verilerinizi doğrulayın ve doğrulayın.

## <a name="use-data-box-heavy-to-copy-data"></a>Verileri kopyalamak için Veri Kutusu Ağır'ı kullanma

Verileri Veri Kutusu Ağır'ınıza kopyalamak için aşağıdaki adımları izleyin.

1. [Veri Kutunuzu Ağır Sipariş Edin.](data-box-heavy-deploy-ordered.md)
2. Veri Kutunuzu Ağır aldıktan [sonra, Veri Kutusu Ağır'ı ayarlayın.](data-box-heavy-deploy-set-up.md) Aygıtınızdaki düğümleri kablolar ve yapılandırırsınız.
3. [Verileri Azure Veri Kutusu Heavy'ye kopyalayın.](data-box-heavy-deploy-copy-data.md) Kopyalama sırasında aşağıdakileri yaptığınızdan emin olun:

    - Verileri kopyalamak için Yalnızca Veri Kutusu Ağır'daki *StorageAccountName_AzFile* klasörünü kullanın. Bunun nedeni, verilerin blok blobs veya sayfa lekeleri değil, bir Azure dosya paylaşımında sona ermesini istemenizdir.
    - Dosyaları *StorageAccountName_AzFile* klasördeki bir klasöre kopyalayın. *StorageAccountName_AzFile* klasör içindeki bir alt klasör bir dosya paylaşımı oluşturur. Doğrudan *StorageAccountName_AzFile* klasöre kopyalanan dosyalar başarısız olur ve blok blobs olarak yüklenir. Bu, bir sonraki adımda VM'nize monte edeceğiniz dosya paylaşımıdır.
    - Verileri, Veri Kutusu Ağır'ınızın her iki düğümüne de kopyalayın.
3. Çalıştır [Cihazınıza gemi için hazırlanın.](data-box-heavy-deploy-picked-up.md#prepare-to-ship) Başarılı bir şekilde gönderiyapmaya hazırlanırsa, Azure'a başarılı bir dosya yüklemesi sağlar.
4. [Cihazı döndürün.](data-box-heavy-deploy-picked-up.md#ship-data-box-heavy-back)
5. [Azure'a yüklenen verileri doğrulayın.](data-box-heavy-deploy-picked-up.md#verify-data-upload-to-azure)

## <a name="use-spmt-to-migrate-data"></a>Verileri geçirmek için SPMT'yi kullanma

Azure veri ekibinden veri kopyanızın tamamlandığına dair onay aldıktan sonra, verilerinizi SharePoint Online'a geçirin.

En iyi performans ve bağlantı için bir Azure Sanal Makinesi (VM) oluşturmanızı öneririz.

1. Azure portalında oturum açın ve [ardından sanal bir makine oluşturun.](../virtual-machines/windows/quick-create-portal.md)
2. [Azure dosya paylaşımını VM'ye monte edin.](../storage/files/storage-how-to-use-files-windows.md#mount-the-azure-file-share-with-file-explorer)
3. [SharePoint Geçiş aracını indirin](https://spmtreleasescus.blob.core.windows.net/install/default.htm) ve Azure VM'nize yükleyin.
4. SharePoint Geçiş Aracı'nı başlatın. **Oturum Aç'ı** tıklatın ve Office 365 kullanıcı adınızı ve şifrenizi girin.
5. İstendiğinde **verileriniz nerededir?** **File share** Verilerinizin bulunduğu Azure dosya paylaşımına giden yolu girin.
6. Hedef konumunuz da dahil olmak üzere kalan istemleri normal olarak izleyin. Daha fazla bilgi için [SharePoint Geçiş Aracı'nı nasıl kullanacağız konusuna](https://docs.microsoft.com/sharepointmigration/how-to-use-the-sharepoint-migration-tool)gidin.

> [!IMPORTANT]
> - Verilerinizin Azure'da olup olmadığınıza bakılmaksızın, verilerin SharePoint Online'a girme hızı çeşitli etkenlerden etkilenir. Bu faktörleri anlamak, geçişinizin verimliliğini planlamanıza ve en üst düzeye çıkarmanıza yardımcı olur.  Daha fazla bilgi için [SharePoint Online ve OneDrive geçiş Hızı'na](/sharepointmigration/sharepoint-online-and-onedrive-migration-speed)gidin.
> - Verileri SharePoint Online'a taşırken dosyalardaki varolan izinleri kaybetme riski vardır. Ayrıca, *tarafından oluşturulan* ve tarih tarafından değiştirilen bazı meta verileri de *kaybedebilirsiniz.*

## <a name="next-steps"></a>Sonraki adımlar

[Veri Kutunuzu Ağır Sipariş Edin](./data-box-heavy-deploy-ordered.md)