---
title: Azure Depolama'ya veri aktarmak için Azure İçe Aktarma/Dışa Aktarma'yı kullanma | Microsoft Dokümanlar
description: Azure Depolama'ya veri aktarmak için Azure portalında nasıl içe aktarma ve dışa aktarma işleri oluşturabilirsiniz öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 03/15/2020
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: eee0fc2797fbe0666a6b848fde574c7807f47cc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282452"
---
# <a name="what-is-azure-importexport-service"></a>Azure İçe Aktarma/Verme hizmeti nedir?

Azure İçe Alma/Verme hizmeti, disk sürücülerini bir Azure veri merkezine göndererek azure blob depolamasına ve Azure Dosyalarına büyük miktarda veriyi güvenli bir şekilde almak için kullanılır. Bu hizmet, Azure Blob depolama dan disk sürücülerine veri aktarmak ve şirket içi sitelerinize aktarmak için de kullanılabilir. Bir veya daha fazla disk sürücüsünden gelen veriler Azure Blob depolama alanına veya Azure Dosyaları'na aktarılabilir.

Azure İçe Alma/Dışa Aktarma hizmetiyle kendi disk sürücülerinizi verin ve veri aktarın. Microsoft tarafından sağlanan disk sürücülerini de kullanabilirsiniz.

Microsoft tarafından sağlanan disk sürücülerini kullanarak veri aktarmak istiyorsanız, verileri Azure'a aktarmak için [Azure Veri Kutusu Diski'ni](../../databox/data-box-disk-overview.md) kullanabilirsiniz. Microsoft, sipariş başına 40 TB toplam kapasiteye sahip 5'e kadar şifreli katı hal disk sürücüsü (SSD) bölgesel bir taşıyıcı aracılığıyla veri merkezinize yönlendirir. Disk sürücülerini hızla yapılandırabilir, USB 3.0 bağlantısı üzerinden disk sürücülerine veri kopyalayabilir ve disk sürücülerini Azure'a geri döndürebilirsiniz. Daha fazla bilgi için [Azure Veri Kutusu Diski'ne genel bakış](../../databox/data-box-disk-overview.md)'a gidin.

## <a name="azure-importexport-use-cases"></a>Azure İçe Alma/Dışa Aktarma kullanım örnekleri

Ağ üzerinden veri yüklerken veya indirirken Azure İçe Aktarma/Verme hizmetini kullanmayı düşünün veya ağ üzerinden veri indirme çok yavaşveya ek ağ bant genişliği almak maliyet engelleyicidir. Bu hizmeti aşağıdaki senaryolarda kullanın:

* **Buluta veri geçişi**: Büyük miktarda veriyi hızlı ve etkin bir şekilde Azure'a taşıyın.
* **İçerik dağıtımı**: Müşteri sitelerine hızlı bir şekilde veri gönderin.
* **Yedekleme**: Azure Depolama'da depolamak için şirket içi verilerinizin yedeklerini alın.
* **Veri kurtarma**: Depolamada depolanan büyük miktarda veriyi kurtarın ve şirket içi konumunuza teslim edin.

## <a name="importexport-components"></a>İthalat/Dışa Aktarma bileşenleri

Alma/Dışa Aktarma hizmeti aşağıdaki bileşenleri kullanır:

* **Alma/Dışa Aktarma hizmeti**: Azure portalında bulunan bu hizmet, kullanıcının veri alma (yükleme) ve dışa aktarma (indirme) işlerini oluşturmasına ve izlemesine yardımcı olur.  

* **WAImportExport aracı**: Bu aşağıdakileri yapan bir komut satırı aracıdır:
  * Alma için sevk edilen disk sürücülerinizi hazırlar.
  * Verilerinizin sürücüye kopyalanması na kolaylaştırır.
  * Sürücüdeki verileri AES 128 bit BitLocker ile şifreler. BitLocker anahtarınızı korumak için harici bir anahtar koruyucusu kullanabilirsiniz.
  * Alma oluşturma sırasında kullanılan sürücü günlüğü dosyalarını oluşturur.
  * Dışa aktarma işleri için gereken sürücü sayısını belirlemeye yardımcı olur.

> [!NOTE]
> WAImportExport aracı, sürüm 1 ve 2 olmak üzere iki sürümde mevcuttur. Şunları kullanmanızı öneririz:
>
> * Azure Blob depolama alanına alma/dışa aktarma için sürüm 1.
> * Azure dosyalarına veri almak için sürüm 2.
>
> WAImportExport aracı yalnızca 64 bit Windows işletim sistemiyle uyumludur. Desteklenen belirli işletim sistemi sürümleri için [Azure İçe Aktarma/Verme gereksinimlerine](storage-import-export-requirements.md#supported-operating-systems)gidin.

* **Disk Sürücüleri**: Katı hal sürücülerini (SSD'leri) veya Sabit disk sürücülerini (HDD'leri) Azure veri merkezine taşıyabilirsiniz. Bir alma işi oluştururken, verilerinizi içeren disk sürücüleri sevk emzebilirsiniz. Bir dışa aktarma işi oluştururken, boş sürücüleri Azure veri merkezine yönlendirirsiniz. Belirli disk türleri için [Desteklenen disk türlerine](storage-import-export-requirements.md#supported-hardware)gidin.

## <a name="how-does-importexport-work"></a>İçeri/Dışarı Aktarma nasıl çalışır?

Azure İçe Alma/Dışa Aktarma hizmeti, iş yaratarak Azure Blobs ve Azure Dosyalarına veri aktarımı sağlar. İş oluşturmak için Azure portalLarını veya Azure Kaynak Yöneticisi REST API'yi kullanın. Her iş tek bir depolama hesabıyla ilişkilidir.

İşler içe aktarma veya dışa aktarma işleri olabilir. Bir alma işi verileri Azure Blobs veya Azure dosyalarına içe aktarmanıza olanak sağlarken, dışa aktarma işi verilerin Azure Blobs'tan dışa aktarılmasına izin verir. Bir alma işi için, verilerinizi içeren sürücüler imal emzebilirsiniz. Bir dışa aktarma işi oluşturduğunuzda, boş sürücüleri bir Azure veri merkezine yönlendirirsiniz. Her durumda, iş başına en fazla 10 disk sürücüsü sevk edebilirsiniz.

### <a name="inside-an-import-job"></a>Bir alma işinin içinde

Yüksek düzeyde, bir alma işi aşağıdaki adımları içerir:

1. Aktabiri verilecek verileri, ihtiyacınız olan sürücü sayısını, Azure depolamadaki verileriniz için hedef blob konumunu belirleyin.
2. Disk sürücülerine verileri kopyalamak için WAImportExport aracını kullanın. Disk sürücülerini BitLocker ile şifreleyin.
3. Azure portalında hedef depolama hesabınızda bir alma işi oluşturun. Sürücü günlüğü dosyalarını yükleyin.
4. Sürücüleri size geri göndermeniz için iade adresini ve taşıyıcı hesap numarasını sağlayın.
5. Disk sürücülerini iş oluşturma sırasında sağlanan sevkiyat adresine gönderin.
6. Alma iş ayrıntılarındaki teslim izleme numarasını güncelleştirin ve alma işini gönderin.
7. Sürücüler Azure veri merkezinde alınır ve işlenir.
8. Sürücüler, taşıyıcı hesabınız kullanılarak alma işinde sağlanan iade adresine sevk edilir.

> [!NOTE]
> Yerel (veri merkezi ülke/bölge içinde) gönderiler için lütfen bir yurtiçi taşıyıcı hesabı paylaşın.
>
> Yurt dışı (veri merkezi ülke/bölge dışındaki) gönderiler için lütfen uluslararası bir taşıyıcı hesabı paylaşın.

 ![Şekil 1:İthalat iş akışı](./media/storage-import-export-service/importjob.png)

Veri alma yla ilgili adım adım talimatlar için şu bilgileri verin:

* [Verileri Azure Blobs'a aktarma](storage-import-export-data-to-blobs.md)
* [Verileri Azure Dosyalarına aktarma](storage-import-export-data-to-files.md)

### <a name="inside-an-export-job"></a>Bir ihracat işinin içinde

> [!IMPORTANT]
> Hizmet yalnızca Azure Blobs dışa aktarmayı destekler. Azure dosyalarının dışa aktarımı desteklenmez.

Yüksek düzeyde, bir dışa aktarma işi aşağıdaki adımları içerir:

1. Dışa aktarılacak verileri, ihtiyacınız olan sürücü sayısını, blob depolamada verilerinizin kaynak lekelerini veya kapsayıcı yollarını belirleyin.
2. Azure portalındaki kaynak depolama hesabınızda bir dışa aktarma işi oluşturun.
3. Dışa aktarılacak veriler için kaynak lekeleri veya kapsayıcı yolları belirtin.
4. Sürücüleri size geri göndermeniz için iade adresini ve taşıyıcı hesap numarasını sağlayın.
5. Disk sürücülerini iş oluşturma sırasında sağlanan sevkiyat adresine gönderin.
6. Dışa aktarma iş ayrıntılarındaki teslimat izleme numarasını güncelleştirin ve dışa aktarma işini gönderin.
7. Sürücüler Azure veri merkezinde alınır ve işlenir.
8. Sürücüler BitLocker ile şifrelenir ve tuşlara Azure portalı üzerinden ulaşabilirsiniz.  
9. Sürücüler, taşıyıcı hesabınız kullanılarak alma işinde sağlanan iade adresine sevk edilir.

> [!NOTE]
> Yerel (veri merkezi ülke/bölge içinde) gönderiler için lütfen bir yurtiçi taşıyıcı hesabı paylaşın.
>
> Yurt dışı (veri merkezi ülke/bölge dışındaki) gönderiler için lütfen uluslararası bir taşıyıcı hesabı paylaşın.
  
 ![Şekil 2:Dışa aktarma iş akışı](./media/storage-import-export-service/exportjob.png)

Veri dışa aktarma yla ilgili adım adım talimatlar için [Azure Blobs'tan veri dışa](storage-import-export-data-from-blobs.md)aktar'a gidin.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure İçe Alma/Dışa Aktarma hizmeti, tüm Azure depolama hesaplarına ve tüm Azure depolama hesaplarından veri kopyalamayı destekler. Disk sürücülerini listelenen konumlardan birine yönlendirebilirsiniz. Depolama hesabınız burada belirtilmeyen bir Azure konumundaysa, işi oluşturduğunuzda alternatif bir sevkiyat konumu sağlanır.

### <a name="supported-shipping-locations"></a>Desteklenen sevkiyat yerleri

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

Sürücüdeki veriler AES 128 bit BitLocker Sürücü Şifrelemesi kullanılarak şifrelenir. Bu şifreleme, verilerinizi aktarım sırasında korur.

Alma işleri için sürücüler iki şekilde şifrelenir.  

* Sürücü hazırlama sırasında WAImportExport aracını çalıştırırken *dataset.csv* dosyasını kullanırken seçeneği belirleyin.

* Sürücüde BitLocker şifrelemesini el ile etkinleştirin. Sürücü hazırlama sırasında WAImportExport araç komut satırını çalıştırırken *driveset.csv'deki* şifreleme anahtarını belirtin. BitLocker şifreleme anahtarı harici bir anahtar koruyucusu (Microsoft yönetilen anahtar olarak da bilinir) veya müşteri yönetilen bir anahtar kullanılarak daha da korunabilir. Daha fazla bilgi [için, BitLocker anahtarınızı korumak için müşteri yönetilen anahtarının](storage-import-export-encryption-key-portal.md)nasıl kullanılacağına bakın.

Dışa aktarma işleri için, verileriniz sürücülere kopyalandıktan sonra, hizmet sürücüyü size geri göndermeden önce BitLocker kullanarak şifreler. Şifreleme anahtarı size Azure portalı üzerinden sağlanır. Anahtarı kullanarak WAImporExport aracı kullanılarak sürücünün kilidinin açılması gerekir.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

### <a name="pricing"></a>Fiyatlandırma

**Sürücü taşıma ücreti**

İthalat veya ihracat işinizin bir parçası olarak işlenen her sürücü için bir sürücü taşıma ücreti vardır. [Azure İçe Alma/DışAk Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage-import-export/)ile ilgili ayrıntılara bakın.

**Nakliye maliyetleri**

Sürücüleri Azure'a sevk ettiğinizde, gönderi taşıyıcısına nakliye maliyetini ödentirsiniz. Microsoft sürücüleri size iade ettiğinde, sevkiyat maliyeti iş oluşturma sırasında sağladığınız operatör hesabına ücretlendirilir.

**İşlem maliyetleri**

[İthalat](https://azure.microsoft.com/pricing/details/storage/) ve veri ihracatı sırasında standart depolama işlem ücreti uygulanır. Azure Depolama'dan veri dışa aktarıldığında standart çıkış ücretleri ve depolama işlem ücretleri de uygulanabilir. Çıkış maliyetleri hakkında daha fazla bilgi için [Veri aktarım fiyatlandırması'na bakın.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="next-steps"></a>Sonraki adımlar

İçe Alma/Dışa Aktarma hizmetini aşağıdakiler için nasıl kullanacağınızı öğrenin:

* [Verileri Azure Blobs'a aktarma](storage-import-export-data-to-blobs.md)
* [Azure Blobs'tan veri dışa aktarma](storage-import-export-data-from-blobs.md)
* [Verileri Azure Dosyalarına aktarma](storage-import-export-data-to-files.md)
