---
title: Azure depolama 'ya/Azure depolama 'dan veri aktarmak için Azure Içeri/dışarı aktarma kullanma | Microsoft Docs
description: Azure depolama 'ya ve Azure Storage 'dan veri aktarmaya yönelik Azure portal içeri ve dışarı aktarma işleri oluşturmayı öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/06/2020
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 120dbe9c4b79755105ba634c68606edfbfdc4ad2
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872443"
---
# <a name="what-is-azure-importexport-service"></a>Azure Içeri/dışarı aktarma hizmeti nedir?

Azure Içeri/dışarı aktarma hizmeti, bir Azure veri merkezine disk sürücüleri göndererek büyük miktarlarda verileri Azure Blob depolama ve Azure dosyalarına güvenli bir şekilde aktarmak için kullanılır. Bu hizmet Ayrıca, Azure Blob depolamadan disk sürücülerine veri aktarmak ve şirket içi sitelerinize göndermek için de kullanılabilir. Bir veya daha fazla disk sürücüsünden alınan veriler, Azure Blob depolama veya Azure dosyaları 'na aktarılabilir.

Azure Içeri/dışarı aktarma hizmeti ile kendi disk sürücülerinizi sağlayın ve verileri aktarın. Microsoft tarafından sağlanan disk sürücüleri de kullanabilirsiniz.

Microsoft tarafından sağlanan disk sürücüleri kullanarak veri aktarmak istiyorsanız, Azure 'a veri aktarmak için [Azure Data Box disk](../../databox/data-box-disk-overview.md) kullanabilirsiniz. Microsoft, bir bölgesel taşıyıcı aracılığıyla veri merkezinize her sipariş için 40 TB toplam kapasiteye sahip 5 adede kadar şifrelenmiş katı hal disk sürücüsü (SSD 'Ler) sağlar. Disk sürücüleri hızlıca yapılandırabilir, bir USB 3,0 bağlantısı üzerinden disk sürücülerine veri kopyalayabilir ve disk sürücüleri tekrar Azure 'a gönderebilirsiniz. Daha fazla bilgi için [Azure Data Box disk genel bakış](../../databox/data-box-disk-overview.md)sayfasına gidin.

## <a name="azure-importexport-use-cases"></a>Azure Içeri/dışarı aktarma kullanım örnekleri

Ağ üzerinden veri yükleme veya indirme sırasında Azure Içeri/dışarı aktarma hizmeti 'ni kullanmayı düşünün veya daha fazla ağ bant genişliği alma maliyeti düşük olur. Bu hizmeti aşağıdaki senaryolarda kullanın:

* **Buluta veri geçişi**: büyük miktarlarda verileri Azure 'a hızlı ve uygun maliyetli bir şekilde taşıyın.
* **İçerik dağıtımı**: müşteri sitelerinize hızlı bir şekilde veri gönderebilirsiniz.
* **Yedekleme**: Azure Storage 'da depolanacak şirket içi verilerinizin yedeklerini alın.
* **Veri kurtarma**: depolama alanında depolanan büyük miktarda veriyi kurtarın ve şirket içi konumunuza teslim edin.

## <a name="importexport-components"></a>Bileşenleri içeri/dışarı aktarma

İçeri/dışarı aktarma hizmeti aşağıdaki bileşenleri kullanır:

* **İçeri/dışarı aktarma hizmeti**: bu hizmet Azure Portal, kullanıcının veri içeri aktarma (karşıya yükleme) ve dışarı aktarma (indirme) işleri oluşturmasına ve izlemesine yardımcı olur.  

* **Waımportexport aracı**: Bu, aşağıdakileri yapan bir komut satırı aracıdır:
  * İçeri aktarma için gönderilen disk sürücülerinizi hazırlar.
  * Verilerinizin sürücüye kopyalanmasını kolaylaştırır.
  * AES 256 bit BitLocker ile sürücüdeki verileri şifreler. BitLocker anahtarınızı korumak için bir dış anahtar koruyucu kullanabilirsiniz.
  * İçeri aktarma oluşturma sırasında kullanılan sürücü günlüğü dosyalarını oluşturur.
  * Dışarı aktarma işleri için gereken sürücü sayısını belirlemenize yardımcı olur.

> [!NOTE]
> Waımportexport aracı iki sürümde, sürüm 1 ve 2 ' de kullanılabilir. Şunları kullanmanızı öneririz:
>
> * Azure Blob depolama alanına içeri/dışarı aktarma için sürüm 1.
> * Azure dosyalarına veri aktarmak için sürüm 2.
>
> WAImportExport aracı yalnızca 64 bit Windows işletim sistemiyle uyumludur. Desteklenen belirli işletim sistemi sürümleri için [Azure içeri/dışarı aktarma gereksinimleri](storage-import-export-requirements.md#supported-operating-systems)' ne gidin.

* **Disk sürücüleri**: Azure veri merkezine katı hal sürücüleri (SSD) veya sabit disk sürücüleri (HDD 'ler) gönderebilirsiniz. İçeri aktarma işi oluştururken, verilerinizi içeren disk sürücüleri sevk edersiniz. Bir dışarı aktarma işi oluştururken, boş sürücüleri Azure veri merkezine sevk edersiniz. Belirli disk türleri için [Desteklenen disk türleri](storage-import-export-requirements.md#supported-hardware)' ne gidin.

## <a name="how-does-importexport-work"></a>İçeri/Dışarı Aktarma nasıl çalışır?

Azure Içeri/dışarı aktarma hizmeti, Azure Bloblarına ve Azure dosyalarına veri aktarımına iş oluşturma olanağı sağlar. İşleri oluşturmak için Azure portal veya Azure Resource Manager REST API kullanın. Her iş, tek bir depolama hesabıyla ilişkilendirilir.

İşler, işleri içeri veya dışarı aktarmaya yönelik olabilir. İçeri aktarma işi, verileri Azure Blob 'larına veya Azure dosyalarına aktarmanıza karşın dışarı aktarma işi verilerin Azure Bloblarından dışarı aktarılmasını sağlar. İçeri aktarma işi için verilerinizi içeren sürücüler sevk edersiniz. Bir dışarı aktarma işi oluşturduğunuzda, boş sürücüleri bir Azure veri merkezine sevk edersiniz. Her durumda, iş başına en fazla 10 disk sürücüsü gönderebilirsiniz.

### <a name="inside-an-import-job"></a>İçeri aktarma işi içinde

Yüksek düzeyde, bir içeri aktarma işi aşağıdaki adımları içerir:

1. İçeri aktarılacak verileri, gereken sürücü sayısını, Azure depolama 'da verileriniz için hedef blob konumunu belirleme.
2. Verileri disk sürücülerine kopyalamak için Waımportexport aracını kullanın. Disk sürücüleri BitLocker ile şifreleyin.
3. Hedef depolama hesabınızda Azure portal bir içeri aktarma işi oluşturun. Sürücü günlüğü dosyalarını karşıya yükleyin.
4. Sürücülerin size geri sevk edilmesi için dönüş adresi ve taşıyıcı hesap numarasını girin.
5. Disk sürücüleri iş oluşturma sırasında verilen sevkiyat adresine gönderin.
6. İçeri aktarma iş ayrıntılarında teslim izleme numarasını güncelleştirin ve içeri aktarma işini iletin.
7. Sürücüler Azure veri merkezinde alınır ve işlenir.
8. Sürücüler, içeri aktarma işinde verilen dönüş adresine taşıyıcı Hesabınız kullanılarak gönderilir.

> [!NOTE]
> Yerel (veri merkezi ülke/bölge içinde) sevkıyatlar için lütfen bir yurtiçi taşıyıcı hesabı paylaşabilirsiniz.
>
> Kuruluşunun (veri merkezi ülke/bölge dışında) sevkiyatları için lütfen bir uluslararası taşıyıcı hesabı paylaşabilirsiniz.

 ![Şekil 1: iş akışını Içeri aktarma](./media/storage-import-export-service/importjob.png)

Veri içeri aktarma hakkında adım adım yönergeler için şuraya gidin:

* [Azure Bloblarına veri aktarma](storage-import-export-data-to-blobs.md)
* [Azure dosyalarına veri aktarma](storage-import-export-data-to-files.md)

### <a name="inside-an-export-job"></a>Dışarı aktarma işi içinde

> [!IMPORTANT]
> Hizmet yalnızca Azure bloblarının dışarı aktarılmasını destekler. Azure dosyalarını dışarı aktarma desteklenmiyor.

Yüksek düzeyde, bir dışarı aktarma işi aşağıdaki adımları içerir:

1. Dışarı aktarılacak verileri, gereken sürücü sayısını, kaynak bloblarını veya blob depolama alanındaki verilerinizin kapsayıcı yollarını belirleme.
2. Kaynak depolama hesabınızda Azure portal bir dışarı aktarma işi oluşturun.
3. Dışarı aktarılacak veriler için kaynak bloblarını veya kapsayıcı yollarını belirtin.
4. Sürücülerin size geri sevk edilmesi için dönüş adresi ve taşıyıcı hesap numarasını girin.
5. Disk sürücüleri iş oluşturma sırasında verilen sevkiyat adresine gönderin.
6. Dışarı aktarma iş ayrıntılarında teslim izleme numarasını güncelleştirin ve dışarı aktarma işini iletin.
7. Sürücüler Azure veri merkezinde alınır ve işlenir.
8. Sürücüler BitLocker ile şifrelenir ve anahtarlar Azure portal aracılığıyla kullanılabilir.  
9. Sürücüler, içeri aktarma işinde verilen dönüş adresine taşıyıcı Hesabınız kullanılarak gönderilir.

> [!NOTE]
> Yerel (veri merkezi ülke/bölge içinde) sevkıyatlar için lütfen bir yurtiçi taşıyıcı hesabı paylaşabilirsiniz.
>
> Kuruluşunun (veri merkezi ülke/bölge dışında) sevkiyatları için lütfen bir uluslararası taşıyıcı hesabı paylaşabilirsiniz.
  
 ![Şekil 2: iş akışını dışarı aktarma](./media/storage-import-export-service/exportjob.png)

Veri dışa aktarma hakkında adım adım yönergeler için [Azure Bloblarından verileri dışarı aktarma](storage-import-export-data-from-blobs.md)bölümüne gidin.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Içeri/dışarı aktarma hizmeti, tüm Azure depolama hesaplarına veri kopyalamayı destekler. Disk sürücülerinizi listelenen konumlardan birine gönderebilirsiniz. Depolama Hesabınız burada belirtilmeyen bir Azure konumunuzda ise, işi oluşturduğunuzda alternatif bir sevkiyat konumu sağlanır.

### <a name="supported-shipping-locations"></a>Desteklenen sevkiyat konumları

|Ülke/Bölge  |Ülke/Bölge  |Ülke/Bölge  |Ülke/Bölge  |
|---------|---------|---------|---------|
|Doğu ABD    | Kuzey Avrupa        | Orta Hindistan        |US Gov Iowa         |
|Batı ABD     |Batı Avrupa         | Güney Hindistan        | Doğu US DoD        |
|Doğu ABD 2    | Doğu Asya        |  Batı Hindistan        | Orta US DoD        |
|Batı ABD 2     | Güneydoğu Asya        | Orta Kanada        | Doğu Çin         |
|Orta ABD     | Doğu Avustralya        | Doğu Kanada        | Kuzey Çin        |
|Orta Kuzey ABD     |  Güneydoğu Avustralya       | Güney Brezilya        | Güney Birleşik Krallık        |
|Orta Güney ABD     | Batı Japonya        |Güney Kore - Orta         | Orta Almanya        |
|Orta Batı ABD     |  Doğu Japonya       | US Gov Virginia        | Kuzeydoğu Almanya        |

## <a name="security-considerations"></a>Güvenlik konuları

Sürücüdeki veriler, AES 256-bit BitLocker Sürücü Şifrelemesi kullanılarak şifrelenir. Bu şifreleme, aktarım sırasında verilerinizi korur.

İçeri aktarma işleri için, sürücüler iki şekilde şifrelenir.  

* Sürücü hazırlığı sırasında Waımportexport aracını çalıştırırken *DataSet. csv* dosyasını kullanırken bu seçeneği belirtin.

* Sürücüde BitLocker şifrelemesini el ile etkinleştirin. Sürücü hazırlama sırasında Waımportexport aracı komut satırını çalıştırırken, *driveset. csv* dosyasında şifreleme anahtarını belirtin. BitLocker şifreleme anahtarı, bir dış anahtar koruyucusu (Microsoft tarafından yönetilen anahtar olarak da bilinir) veya müşteri tarafından yönetilen anahtar kullanılarak daha fazla korunabilir. Daha fazla bilgi için, bkz. [BitLocker anahtarınızı korumak için müşteri tarafından yönetilen anahtar kullanma](storage-import-export-encryption-key-portal.md).

Dışarı aktarma işleri için verileriniz sürücülere kopyalandıktan sonra hizmet, bu sürücüyü size geri teslim etmeden önce BitLocker 'ı kullanarak şifreler. Şifreleme anahtarı Azure portal aracılığıyla size sağlanır. Anahtar kullanılarak Waımpımprexport Aracı kullanılarak sürücünün kilidinin açılması gerekir.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

### <a name="pricing"></a>Fiyatlandırma

**Sürücü işleme ücreti**

İçeri veya dışarı aktarma işinizin bir parçası olarak işlenen her sürücü için bir sürücü işleme ücreti vardır. [Azure içeri/dışarı aktarma fiyatlandırmasıyla](https://azure.microsoft.com/pricing/details/storage-import-export/)ilgili ayrıntılara bakın.

**Teslimat maliyetleri**

Azure 'a sürücü gönderdiğinizde, Sevkiyat masrafını sevkiyat taşıyıcısı üzerinden ödeyin. Microsoft, sürücüleri size döndürdüğünde, sevkiyat maliyeti iş oluşturma sırasında verdiğiniz taşıyıcı hesaba ücretlendirilir.

**İşlem maliyetleri**

[Standart depolama işlem ücreti](https://azure.microsoft.com/pricing/details/storage/) içeri aktarma sırasında, verilerin dışarı aktarılması sırasında da geçerlidir. Standart çıkış ücretleri ayrıca, veriler Azure depolama alanından aktarıldığında depolama işlem ücretleriyle birlikte da geçerlidir. Çıkış maliyetleri hakkında daha fazla bilgi için bkz [. veri aktarımı fiyatlandırması.](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="next-steps"></a>Sonraki adımlar

Içeri/dışarı aktarma hizmetini kullanarak şunları yapmayı öğrenin:

* [Azure Bloblarına veri aktarma](storage-import-export-data-to-blobs.md)
* [Azure Bloblarından verileri dışarı aktarma](storage-import-export-data-from-blobs.md)
* [Azure dosyalarına veri aktarma](storage-import-export-data-to-files.md)
