---
title: Azure İçe Alma/Dışa Aktarma hizmeti için SSS | Microsoft Dokümanlar
description: Azure İçe Aktarma Hizmeti hakkında sık sorulan soruların yanıtlarını okuyun.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 03/12/2020
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: dcf9a278bfc0ede844f8eb59e3a256e40a47c57d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282435"
---
# <a name="azure-importexport-service-frequently-asked-questions"></a>Azure İçe Alma/Dışa Aktarma hizmeti: sık sorulan sorular

Verileri Azure depolama alanına aktarmak için Azure İçe Alma/Dışak hizmetinizi kullandığınızda sahip olabileceğiniz sorular ve yanıtlar aşağıda ve da açıklanmaktadır. Sorular ve yanıtlar aşağıdaki kategorilere ayrılmıştır:

- İthalat/İhracat hizmeti hakkında
- Diskleri alma/dışa aktarma için hazırlama
- İthalat/Dışa Aktarma işleri
- Gönderi diskleri
- Çeşitli

## <a name="about-importexport-service"></a>İthalat/İhracat hizmeti hakkında

### <a name="can-i-copy-azure-file-storage-using-the-azure-importexport-service"></a>Azure İçe Alma/Dışa Aktarma hizmetini kullanarak Azure Dosyası depolama alanını kopyalayabilir miyim?

Evet. Azure İçe Alma/Dışa Aktarma hizmeti, Azure Dosya Depolama'ya aktar'ı destekler. Şu anda Azure Dosyalarının dışa aktarını desteklemez.

### <a name="is-the-azure-importexport-service-available-for-csp-subscriptions"></a>Azure İçe Alma/Dışa Aktarma hizmeti CSP abonelikleri için kullanılabilir mi?

Evet. Azure İçe Alma/Verme hizmeti Bulut Çözüm Sağlayıcıları (CSP) aboneliklerini destekler.

### <a name="can-i-use-the-azure-importexport-service-to-copy-pst-mailboxes-and-sharepoint-data-to-o365"></a>PST posta kutularını ve SharePoint verilerini O365'e kopyalamak için Azure İçe Alma/Dışa Aktarma hizmetini kullanabilir miyim?

Evet. Daha fazla bilgi [için, OFFICE 365'e PST dosyalarını veya SharePoint verilerini içe](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx)aktarın.

### <a name="can-i-use-the-azure-importexport-service-to-copy-my-backups-offline-to-the-azure-backup-service"></a>Yedeklemelerimi çevrimdışı olarak Azure Yedekleme Hizmeti'ne kopyalamak için Azure İçe Alma/Dışa Aktarma hizmetini kullanabilir miyim?

Evet. Daha fazla bilgi için [Azure Yedekleme'de Çevrimdışı Yedekleme iş akışına](../../backup/backup-azure-backup-import-export.md)gidin.

### <a name="can-i-purchase-drives-for-importexport-jobs-from-microsoft"></a>Microsoft'tan alma/dışa aktarma işleri için sürücüler satın alabilir miyim?

Hayır. İthalat ve ihracat işleri için kendi sürücüleri gemi gerekir.

## <a name="preparing-disks-for-importexport"></a>Disklerin içe aktarıla/dışa aktarıniçin hazırlanması

### <a name="can-i-skip-the-drive-preparation-step-for-an-import-job-can-i-prepare-a-drive-without-copying"></a>Bir alma işi için sürücü hazırlama adımını atlayabilir miyim? Kopyalamadan bir sürücü hazırlayabilir miyim?

Hayır. Veri almak için kullanılan tüm sürücü, Azure WAImportExport aracı kullanılarak hazırlanmalıdır. Verileri sürücüye kopyalamak için aracı kullanın.

### <a name="do-i-need-to-perform-any-disk-preparation-when-creating-an-export-job"></a>Bir dışa aktarma işi oluştururken herhangi bir disk hazırlığı gerçekleştirmem gerekiyor mu?

Hayır. Bazı ön kontroller önerilir. Gerekli disk sayısını kontrol etmek için WAImportExport aracının PreviewExport komutunu kullanın. Daha fazla bilgi için, [Dışa Aktarma İşi için Sürücü Kullanımını Önizleme'ye](https://msdn.microsoft.com/library/azure/dn722414.aspx)bakın. Komut, kullanacağınız sürücülerin boyutuna bağlı olarak seçili lekeler için sürücü kullanımını önizlemenize yardımcı olur. Ayrıca, ihracat işi için gönderilen sabit diskten okuyup yazabileceğinizi ve yazabileceğinizi kontrol edin.

## <a name="importexport-jobs"></a>İthalat/Dışa Aktarma işleri

### <a name="can-i-cancel-my-job"></a>İşimi iptal edebilir miyim?

Evet. Bir işi, durumu **Oluşturma** veya **Gönderim**olduğunda iptal edebilirsiniz. Bu aşamaların ötesinde, iş iptal edilemez ve son aşamaya kadar devam eder.

### <a name="how-long-can-i-view-the-status-of-completed-jobs-in-the-azure-portal"></a>Azure portalında tamamlanan işlerin durumunu ne kadar süreyle görüntüleyebilirim?

Tamamlanan işlerin durumunu 90 güne kadar görüntüleyebilirsiniz. Tamamlanan işler 90 gün sonra silinir.

### <a name="if-i-want-to-import-or-export-more-than-10-drives-what-should-i-do"></a>10'dan fazla sürücü almak veya dışa aktarmak istersem, ne yapmalıyım?

Bir alma veya dışa aktarma işi, tek bir işte yalnızca 10 sürücüye başvuruyapabilir. 10'dan fazla sürücü sevk etmek için birden çok iş oluşturmanız gerekir. Aynı işle ilişkili sürücüler aynı pakette birlikte sevk edilmelidir.
Veri kapasitesi birden çok disk alma işini kapsadığında daha fazla bilgi ve kılavuz için Microsoft Destek'e başvurun.

### <a name="the-uploaded-blob-shows-status-as-lease-expired-what-should-i-do"></a>Yüklenen blob "Kira süresi doldu" olarak durumu gösterir. Ne yapmalıyım?

"Süresi Dolan Kiralama" alanını yoksayabilirsiniz. Alma/Dışa Aktarma, başka hiçbir işlemin blob'u paralel olarak güncelleştirilemediğinden emin olmak için yükleme sırasında blob'u kiralar. Süresi Dolan Kiralama, Alma/Dışa Aktarma'nın artık bu kullanıma yüklenmediğini ve blob'un kullanımınız için kullanılabildiğini gösterir.

## <a name="shipping-disks"></a>Gönderi diskleri

### <a name="what-is-the-maximum-number-of-hdd-for-in-one-shipment"></a>Bir gönderide en fazla HDD sayısı nedir?

Bir sevkiyattaki HDD sayısı nın sınırı yoktur. Diskler birden çok işe aitse, aşağıdakileri yapmanızı öneririz:

- ilgili iş adlarıyla diskleri etiketle.
- -1, -2 vb. ile sabitlenmiş bir izleme numarası ile işleri güncelleyin.

### <a name="should-i-include-anything-other-than-the-hdd-in-my-package"></a>Paketime HDD'den başka bir şey eklemeli miyim?

Sadece sabit disklerinizi nakliye paketine gönderin. Güç kaynağı kabloları veya USB kabloları gibi öğeleri eklemeyin.

### <a name="do-i-have-to-ship-my-drives-using-fedex-or-dhl"></a>Sürücülerimi FedEx veya DHL kullanarak göndermem gerekiyor mu?

FedEx, DHL, UPS veya ABD Posta Hizmeti gibi bilinen herhangi bir taşıyıcıyı kullanarak sürücüleri Azure veri merkezine gönderebilirsiniz. Ancak, sürücülerin veri merkezinden size geri sevkiyatı için şunları sağlamanız gerekir:

- ABD ve AB'de bir FedEx hesap numarası veya
- Asya ve Avustralya bölgelerinde bir DHL hesap numarası.

> [!NOTE]
> Hindistan'daki veri merkezleri, sürücüleri döndürmek için antetli kayınızda (teslimat challan) bir beyan mektubu gerektirir. Gerekli giriş kartını ayarlamak için, pick up'ı seçtiğiniz operatörle rezerve etmeli ve ayrıntıları veri merkeziyle paylaşmalısınız.

### <a name="are-there-any-restrictions-with-shipping-my-drive-internationally"></a>Sürücümü uluslararası olarak gönderimde herhangi bir kısıtlama var mı?

Sevk ettiğiniz fiziksel ortamın uluslararası sınırları geçmesi gerekebileceğini lütfen unutmayın. Fiziksel medyanızın ve verilerinizin yürürlükteki yasalara uygun olarak içe aktarılmasını ve/veya dışlanmasını sağlamaksizin sorumlusunuz. Fiziksel ortamı göndermeden önce, medyanızın ve verilerinizin yasal olarak tanımlanan veri merkezine gönderilebebileceğini doğrulamak için danışmanlarınıza danışın. Bu, microsoft'a zamanında ulaştığından emin olmaya yardımcı olur.

### <a name="are-there-any-special-requirements-for-delivering-my-disks-to-a-datacenter"></a>Disklerimi bir veri merkezine teslim etmek için özel gereksinimler var mı?

Gereksinimler belirli Azure veri merkezi kısıtlamalarına bağlıdır.

- Güvenlik nedenleriyle pakete Microsoft veri merkezi Gelen Kimlik numarasının yazılmasını gerektiren birkaç site vardır. Sürücülerinizi veya disklerinizi veri merkezine göndermeden önce, buadbops@microsoft.comnumarayı almak için Azure DataBox İşlemleri () ile iletişime geçin. Bu numara olmadan paket reddedilir.
- Hindistan'daki veri merkezleri, sürücünün Kişisel Bilgilerini gerektirir, örneğin Devlet Kimlik Kartı veya Kanıt No. (örneğin, PAN, AADHAR, DL), adı, temas ve bir kapı giriş kartı almak için araç plaka numarası. Teslimat gecikmelerini önlemek için, taşıyıcınızı bu gereksinimler hakkında bilgilendirin.

### <a name="when-creating-a-job-the-shipping-address-is-a-location-that-is-different-from-my-storage-account-location-what-should-i-do"></a>Bir iş oluştururken, sevkiyat adresi depolama hesabı konumumdan farklı bir konumdur. Ne yapmalıyım?

Bazı depolama hesabı konumları alternatif sevkiyat konumlarına eşlenir. Daha önce kullanılabilen sevkiyat konumları da geçici olarak alternatif konumlara eşlenebilir. Sürücülerinizi göndermeden önce her zaman iş oluşturma sırasında sağlanan sevkiyat adresini kontrol edin.

### <a name="when-shipping-my-drive-the-carrier-asks-for-the-data-center-contact-address-and-phone-number-what-should-i-provide"></a>Sürücümü sevk ederken, operatör veri merkezi iletişim adresini ve telefon numarasını sorar. Ne sağlamalıyım?

Telefon numarası ve DC adresi iş oluşturmanın bir parçası olarak sağlanır.

## <a name="miscellaneous"></a>Çeşitli

### <a name="what-happens-if-i-accidentally-send-an-hdd-that-does-not-conform-to-the-supported-requirements"></a>Yanlışlıkla desteklenen gereksinimlere uymayan bir HDD gönderirsem ne olur?

Azure veri merkezi, desteklenen gereksinimlere uymayan sürücüyü size döndürecektir. Paketteki sürücülerden yalnızca bazıları destek gereksinimlerini karşılıyorsa, bu sürücüler işlenir ve gereksinimleri karşılamayan sürücüler size iade edilir.

### <a name="does-the-service-format-the-drives-before-returning-them"></a>Hizmet, sürücüleri iade etmeden önce biçimlendiriyor mu?

Hayır. Tüm sürücüler BitLocker ile şifrelenir.

### <a name="how-can-i-access-data-that-is-imported-by-this-service"></a>Bu hizmet tarafından alınan verilere nasıl erişebilirim?

Azure depolama hesabınız altındaki verilere erişmek için Azure portalını veya [Depolama Gezgini'ni](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) kullanın.  

### <a name="after-the-import-is-complete-what-does-my-data-look-like-in-the-storage-account-is-my-directory-hierarchy-preserved"></a>Alma işlemi tamamlandıktan sonra, verilerim depolama hesabında nasıl görünür? Dizin hiyerarşim korunuyor mu?

Bir alma işi için bir sabit disk hazırlarken, hedef veri kümesi CSV'deki DstBlobPathOrPrefix alanı tarafından belirtilir. Bu, sabit diskteki verilerin kopyalandığı depolama hesabındaki hedef kapsayıcıdır. Bu hedef kapsayıcıiçinde, sabit diskten klasörler için sanal dizinler oluşturulur ve dosyalar için blobs oluşturulur.

### <a name="if-a-drive-has-files-that-already-exist-in-my-storage-account-does-the-service-overwrite-existing-blobs-or-files"></a>Bir sürücünün depolama hesabımda zaten var olan dosyaları varsa, hizmet varolan blob'ların veya dosyaların üzerine yazar mı?

-sına bağ -lıdır. Sürücüyü hazırlarken, hedef dosyaların üzerinde yazımı mı yoksa veriş:<yeniden adlandırma|over-overwrite|overwrite> adlı veri kümesi CSV dosyasındaki alanı kullanarak üzerine mi yazılması gerektiğini belirtebilirsiniz. Varsayılan olarak, hizmet varolan lekelerin veya dosyaların üzerine yazmak yerine yeni dosyaları yeniden adlandırır.

### <a name="is-the-waimportexport-tool-compatible-with-32-bit-operating-systems"></a>WAImportExport aracı 32 bit işletim sistemleriyle uyumlu mu?

Hayır. WAImportExport aracı yalnızca 64 bit Windows işletim sistemleriyle uyumludur. Desteklenen işletim sisteminin tam listesi için [Desteklenen İşletim Sistemleri'ne](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements)gidin.

### <a name="what-is-the-maximum-block-blob-and-page-blob-size-supported-by-azure-importexport"></a>Azure İçe Aktarma/Dışa Aktarma tarafından desteklenen maksimum Blok Blob ve Sayfa Blob Boyutu nedir?

- Max Blok Blob boyutu yaklaşık 4.768 TB veya 5.000.000 MB'dır.
- Max Page Blob boyutu 8 TB'dir.

### <a name="does-azure-importexport-support-aes-256-encryption"></a>Azure İçe Aktarma/Dışa Aktarma AES-256 şifrelemeyi destekliyor mu?

Hayır. Azure Alma/Dışa Aktarma hizmeti, AES-128 BitLocker şifrelemesi kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İçeri/Dışarı Aktarma nedir?](storage-import-export-service.md)
