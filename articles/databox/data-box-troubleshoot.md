---
title: Azure Veri Kutunuzdaki sorun giderme sorunları, Azure Veri Kutusu Ağır
description: Bu aygıtlara veri kopyalarken Azure Veri Kutusu ve Azure Veri Kutusu Ağır'da görülen sorunları nasıl gidereceğinizi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: 51935516e26f263e44a926bf9b7d7ec24a5eeb9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560074"
---
# <a name="troubleshoot-issues-related-to-azure-data-box-and-azure-data-box-heavy"></a>Azure Veri Kutusu ve Azure Veri Kutusu Ağır ile ilgili sorun giderme sorunları

Bu makalede, Azure Veri Kutusu'nu veya Azure Veri Kutusu Ağır'ı kullanırken görebileceğiniz sorunları nasıl giderileceksiniz hakkında ayrıntılar. Makale, veriler Veri Kutusu'na kopyalandığında veya veri Veri Kutusu'ndan yüklendiğinde görülen olası hataların listesini içerir.

## <a name="error-classes"></a>Hata sınıfları

Veri Kutusu ve Veri Kutusu Ağır'daki hatalar aşağıdaki gibi özetlenmiştir:

| Hata kategorisi*        | Açıklama        | Önerilen eylem    |
|----------------------------------------------|---------|--------------------------------------|
| Kapsayıcı veya paylaşım adları | Kapsayıcı veya paylaşım adları Azure adlandırma kurallarına uymaz.  |Hata listelerini indirin. <br> Kapsayıcıları veya paylaşımları yeniden adlandırın. [Daha fazla bilgi edinin](#container-or-share-name-errors).  |
| Kapsayıcı veya paylaşım boyutu sınırı | Kapsayıcılar veya hisselerdeki toplam veri Azure sınırını aşıyor.   |Hata listelerini indirin. <br> Kapsayıcıdaki veya paylaşımdaki genel verileri azaltın. [Daha fazla bilgi edinin](#container-or-share-size-limit-errors).|
| Nesne veya dosya boyutu sınırı | Kapsayıcılarda veya paylaşımlarda bulunan nesne veya dosyalar Azure sınırını aşıyor.|Hata listelerini indirin. <br> Kapsayıcıdaki dosya boyutunu küçültün veya paylaşın. [Daha fazla bilgi edinin](#object-or-file-size-limit-errors). |    
| Veri veya dosya türü | Veri biçimi veya dosya türü desteklenmez. |Hata listelerini indirin. <br> Sayfa lekeleri veya yönetilen diskler için, verilerin 512 bayt hizalandığından ve önceden oluşturulmuş klasörlere kopyalandığından emin olun. [Daha fazla bilgi edinin](#data-or-file-type-errors). |
| Kritik olmayan blob veya dosya hataları  | Blob veya dosya adları Azure adlandırma kurallarına uymaz veya dosya türü desteklenmez. | Bu blob veya dosyalar kopyalanamayabilir veya adlar değiştirilebilir. [Bu hataları nasıl düzelteceğimiz öğrenin.](#non-critical-blob-or-file-errors) |

\*İlk dört hata kategorisi kritik hatalardır ve gemiye hazırlanmaya devam etmeden önce düzeltilmesi gerekir.


## <a name="container-or-share-name-errors"></a>Kapsayıcı veya paylaşım adı hataları

Bunlar kapsayıcı ve paylaşım adlarıyla ilgili hatalardır.

### <a name="error_container_or_share_name_length"></a>ERROR_CONTAINER_OR_SHARE_NAME_LENGTH     

**Hata açıklaması:** Kapsayıcı veya paylaşım adı 3 ile 63 karakter arasında olmalıdır. 

**Önerilen çözünürlük:** Veri kopyaladığınız Veri Kutusu veya Veri Kutusu Ağır paylaşımının (SMB/NFS) altındaki klasör, depolama hesabınızda bir Azure kapsayıcısı olur. 

- Aygıtın yerel web Kullanıcı Arabirimi'nin **Bağlan ve kopyala** sayfasında, sorun olan klasör adlarını tanımlamak için hata dosyalarını indirin ve gözden geçirin.
- Aşağıdakilerden emin olmak için Veri Kutusu veya Veri Kutusu Ağır paylaşımı altındaki klasör adını değiştirin:

    - Adı 3 ve 63 karakter arasında vardır.
    - İsimlerde yalnızca harfler, sayılar ve tireler olabilir.
    - İsimler tirelerle başlayıp bitiremez.
    - İsimlerde ardışık tireler olamaz.
    - Geçerli adörnekleri: `my-folder-1`,`my-really-extra-long-folder-111`
    - Geçerli olmayan adlara örnekler: `my-folder_1` `my`, `--myfolder` `myfolder--`, , ,`myfolder!`

    Daha fazla bilgi için [kapsayıcı adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#container-names) ve [paylaşım adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#share-names)için Azure adlandırma kurallarına bakın.


### <a name="error_container_or_share_name_alpha_numeric_dash"></a>ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH

**Hata açıklaması:** Kapsayıcı veya paylaşım adı yalnızca harfler, sayılar veya tirelerden oluşmalıdır.

**Önerilen çözünürlük:** Veri kopyaladığınız Veri Kutusu veya Veri Kutusu Ağır paylaşımının (SMB/NFS) altındaki klasör, depolama hesabınızda bir Azure kapsayıcısı olur. 

- Aygıtın yerel web Kullanıcı Arabirimi'nin **Bağlan ve kopyala** sayfasında, sorun olan klasör adlarını tanımlamak için hata dosyalarını indirin ve gözden geçirin.
- Aşağıdakilerden emin olmak için Veri Kutusu veya Veri Kutusu Ağır paylaşımı altındaki klasör adını değiştirin:

    - Adı 3 ve 63 karakter arasında vardır.
    - İsimlerde yalnızca harfler, sayılar ve tireler olabilir.
    - İsimler tirelerle başlayıp bitiremez.
    - İsimlerde ardışık tireler olamaz.
    - Geçerli adörnekleri: `my-folder-1`,`my-really-extra-long-folder-111`
    - Geçerli olmayan adlara örnekler: `my-folder_1` `my`, `--myfolder` `myfolder--`, , ,`myfolder!`

    Daha fazla bilgi için [kapsayıcı adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#container-names) ve [paylaşım adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#share-names)için Azure adlandırma kurallarına bakın.

### <a name="error_container_or_share_name_improper_dash"></a>ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH

**Hata açıklaması:** Kapsayıcı adları ve paylaşım adları tirelerle başlayıp sonakolamaz ve ardışık tireler olamaz.

**Önerilen çözünürlük:** Veri kopyaladığınız Veri Kutusu veya Veri Kutusu Ağır paylaşımının (SMB/NFS) altındaki klasör, depolama hesabınızda bir Azure kapsayıcısı olur. 

- Aygıtın yerel web Kullanıcı Arabirimi'nin **Bağlan ve kopyala** sayfasında, sorun olan klasör adlarını tanımlamak için hata dosyalarını indirin ve gözden geçirin.
- Aşağıdakilerden emin olmak için Veri Kutusu veya Veri Kutusu Ağır paylaşımı altındaki klasör adını değiştirin:

    - Adı 3 ve 63 karakter arasında vardır.
    - İsimlerde yalnızca harfler, sayılar ve tireler olabilir.
    - İsimler tirelerle başlayıp bitiremez.
    - İsimlerde ardışık tireler olamaz.
    - Geçerli adörnekleri: `my-folder-1`,`my-really-extra-long-folder-111`
    - Geçerli olmayan adlara örnekler: `my-folder_1` `my`, `--myfolder` `myfolder--`, , ,`myfolder!`

    Daha fazla bilgi için [kapsayıcı adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#container-names) ve [paylaşım adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#share-names)için Azure adlandırma kurallarına bakın.

## <a name="container-or-share-size-limit-errors"></a>Kapsayıcı veya paylaşım boyutu sınırı hataları

Bunlar, bir kapsayıcıda veya paylaşımda izin verilen verilerin boyutunu aşan verilerle ilgili hatalardır.

### <a name="error_container_or_share_capacity_exceeded"></a>ERROR_CONTAINER_OR_SHARE_CAPACITY_EXCEEDED

**Hata açıklaması:** Azure dosya paylaşımı, bir paylaşımı 5 TB veriyle sınırlar. Bu sınır bazı hisseler için aşıldı.

**Önerilen çözünürlük:** Yerel web Ara Birimi'nin **Bağlan ve kopyala** sayfasında hata dosyalarını indirin ve gözden geçirin.

Hata günlüklerinden bu soruna sahip klasörleri tanımlayın ve bu klasördeki dosyaların 5 TB'nin altında olduğundan emin olun.


## <a name="object-or-file-size-limit-errors"></a>Nesne veya dosya boyutu sınırı hataları

Bunlar, nesnenin veya Azure'da izin verilen dosyanın maksimum boyutunu aşan verilerle ilgili hatalardır. 

### <a name="error_blob_or_file_size_limit"></a>ERROR_BLOB_OR_FILE_SIZE_LIMIT

**Hata açıklaması:** Dosya boyutu yüklemek için maksimum dosya boyutunu aşıyor.

**Önerilen çözünürlük:** Blob veya dosya boyutları yüklemek için izin verilen maksimum sınırı aşıyor.

- Yerel web Ara Birimi'nin **Bağlan ve kopyala** sayfasında hata dosyalarını indirin ve gözden geçirin.
- Blob ve dosya boyutlarının Azure nesne boyutu sınırlarını aşmadığından emin olun.

## <a name="data-or-file-type-errors"></a>Veri veya dosya türü hataları

Bunlar, kapsayıcıda veya paylaşımda bulunan desteklenmeyen dosya türü veya veri türüyle ilgili hatalardır. 

### <a name="error_blob_or_file_size_alignment"></a>ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT

**Hata açıklaması:** Blob veya dosya yanlış hizalanmış.

**Önerilen çözünürlük:** Veri Kutusu veya Veri Kutusu Ağır'daki sayfa blob paylaşımı yalnızca 512 bayt hizalanmış dosyaları destekler (örneğin, VHD/VHDX). Sayfa blob paylaşımına kopyalanan tüm veriler sayfa blobs olarak Azure'a yüklenir.

Sayfa blob paylaşımından VHD/VHDX olmayan verileri kaldırın. Genel veriler için blok blob veya Azure dosyaları için paylaşımları kullanabilirsiniz.

Daha fazla bilgi için [Bkz. Sayfa blobs Genel Bakış.](../storage/blobs/storage-blob-pageblob-overview.md)

### <a name="error_blob_or_file_type_unsupported"></a>ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED

**Hata açıklaması:** Yönetilen bir disk paylaşımında desteklenmeyen bir dosya türü bulunur. Yalnızca sabit VHD'lere izin verilir.

**Önerilen çözünürlük:**

- Yalnızca yönetilen diskler oluşturmak için sabit VHD'leri yüklediğinizden emin olun.
- VHDX dosyaları veya **dinamik** ve **farklılaştırıcı** VHDD'ler desteklenmez.

### <a name="error_directory_disallowed_for_type"></a>ERROR_DIRECTORY_DISALLOWED_FOR_TYPE

**Hata açıklaması:** Yönetilen diskler için önceden varolan klasörlerin hiçbirinde dizin izin verilmez. Bu klasörlerde yalnızca sabit VHD'lere izin verilir.

**Önerilen çözünürlük:** Yönetilen diskler için, her paylaşım da, depolama hesabınızdaki kapsayıcılara karşılık gelen aşağıdaki üç klasör oluşturulur: Premium SSD, Standart HDD ve Standart SSD. Bu klasörler yönetilen diskin performans katmanına karşılık gelir.

- Sayfa blob verilerinizi (VHD' ler) varolan klasörlerden birine kopyaladığınızdan emin olun.
- Bu varolan klasörlerde bir klasöre veya dizine izin verilmez. Önceden varolan klasörlerin içinde oluşturduğunuz klasörleri kaldırın.

Daha fazla bilgi için [bkz.](data-box-deploy-copy-data-from-vhds.md#connect-to-data-box)

### <a name="reparse_point_error"></a>REPARSE_POINT_ERROR

**Hata açıklaması:** Linux'ta sembolik bağlantılara izin verilmez. 

**Önerilen çözünürlük:** Sembolik bağlantılar genellikle bağlantılar, borular ve diğer bu tür dosyalardır. Bağlantıları kaldırın veya bağlantıları çözün ve verileri kopyalayın.


## <a name="non-critical-blob-or-file-errors"></a>Kritik olmayan blob veya dosya hataları

Veri kopyalama sırasında görülen blobs, dosya veya kapsayıcı adları ile ilgili tüm kritik olmayan hatalar aşağıdaki bölümde özetlenmiştir. Bu hatalar varsa, adlar Azure adlandırma kurallarına uyacak şekilde değiştirilir. Veri yüklemesi için karşılık gelen sipariş durumu **uyarılarla tamamlanacaktır.**  

### <a name="error_blob_or_file_name_character_control"></a>ERROR_BLOB_OR_FILE_NAME_CHARACTER_CONTROL

**Hata açıklaması:** Blob veya dosya adları desteklenmeyen denetim karakterleri içerir.

**Önerilen çözünürlük:** Kopyaladığınız lekeler veya dosyalar desteklenmeyen karakterlere sahip adlar içerir.

Yerel web Ara Birimi'nin **Bağlan ve kopyala** sayfasında hata dosyalarını indirin ve gözden geçirin.
Desteklenmeyen karakterleri kaldırmak için dosyaları kaldırın veya yeniden adlandırın.

Daha fazla bilgi [için, blob adları](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) ve [dosya adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names)için Azure adlandırma kurallarına bakın.

### <a name="error_blob_or_file_name_character_illegal"></a>ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL

**Hata açıklaması:** Blob veya dosya adları yasadışı karakterler içerir.

**Önerilen çözünürlük:** Kopyaladığınız lekeler veya dosyalar desteklenmeyen karakterlere sahip adlar içerir.

Yerel web Ara Birimi'nin **Bağlan ve kopyala** sayfasında hata dosyalarını indirin ve gözden geçirin.
Desteklenmeyen karakterleri kaldırmak için dosyaları kaldırın veya yeniden adlandırın.

Daha fazla bilgi [için, blob adları](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) ve [dosya adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names)için Azure adlandırma kurallarına bakın.


### <a name="error_blob_or_file_name_ending"></a>ERROR_BLOB_OR_FILE_NAME_ENDING

**Hata açıklaması:** Blob veya dosya adları kötü karakterlerle sona eriyor.

**Önerilen çözünürlük:** Kopyaladığınız lekeler veya dosyalar desteklenmeyen karakterlere sahip adlar içerir.

Yerel web Ara Birimi'nin **Bağlan ve kopyala** sayfasında hata dosyalarını indirin ve gözden geçirin.
Desteklenmeyen karakterleri kaldırmak için dosyaları kaldırın veya yeniden adlandırın.

Daha fazla bilgi [için, blob adları](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) ve [dosya adları](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names)için Azure adlandırma kurallarına bakın.


### <a name="error_blob_or_file_name_segment_count"></a>ERROR_BLOB_OR_FILE_NAME_SEGMENT_COUNT

**Hata açıklaması:** Blob veya dosya adı çok fazla yol kesimi içerir.

**Önerilen çözünürlük:** Kopyaladığınız bölmeler veya dosyalar en fazla yol kesimi sayısını aşıyor. Yol kesimi ardışık sınırlayıcı karakterler arasındaki dizedir, örneğin, ileri eğik çizgi /.

- Yerel web Ara Birimi'nin **Bağlan ve kopyala** sayfasında hata dosyalarını indirin ve gözden geçirin.
- [Blob adlarının](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) ve [dosya adlarının](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names) Azure adlandırma kurallarına uyduklarından emin olun.

### <a name="error_blob_or_file_name_aggregate_length"></a>ERROR_BLOB_OR_FILE_NAME_AGGREGATE_LENGTH

**Hata açıklaması:** Blob veya dosya adı çok uzun.

**Önerilen çözünürlük:** Blob veya dosya adları maksimum uzunluğu aşıyor.

- Yerel web Ara Birimi'nin **Bağlan ve kopyala** sayfasında hata dosyalarını indirin ve gözden geçirin.
- Blob adı 1.024 karakteri geçmemelidir.
- Adları1024 karakteraşmayan blob veya dosyaları kaldırın veya yeniden adlandırın.

Daha fazla bilgi için, blob adları ve dosya adları için Azure adlandırma kurallarına bakın.

### <a name="error_blob_or_file_name_component_length"></a>ERROR_BLOB_OR_FILE_NAME_COMPONENT_LENGTH

**Hata açıklaması:** Blob veya dosya adındaki segmentlerden biri çok uzun.

**Önerilen çözünürlük:** Blob veya dosya adındaki yol segmentlerinden biri en fazla karakter sayısını aşıyor. Yol kesimi ardışık sınırlayıcı karakterler arasındaki dizedir, örneğin, ileri eğik çizgi /.

- Yerel web Ara Birimi'nin **Bağlan ve kopyala** sayfasında hata dosyalarını indirin ve gözden geçirin.
- [Blob adlarının](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata#blob-names) ve [dosya adlarının](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names) Azure adlandırma kurallarına uyduklarından emin olun.


### <a name="error_container_or_share_name_disallowed_for_type"></a>ERROR_CONTAINER_OR_SHARE_NAME_DISALLOWED_FOR_TYPE

**Hata açıklaması:** Yönetilen disk paylaşımları için uygun olmayan kapsayıcı adları belirtilir.

**Önerilen çözünürlük:** Yönetilen diskler için, her paylaşım içinde, depolama hesabınızdaki kapsayıcılara karşılık gelen aşağıdaki klasörler oluşturulur: Premium SSD, Standart HDD ve Standart SSD. Bu klasörler yönetilen diskin performans katmanına karşılık gelir.

- Sayfa blob verilerinizi (VHD' ler) varolan klasörlerden birine kopyaladığınızdan emin olun. Yalnızca bu varolan kapsayıcılardan gelen veriler Azure'a yüklenir.
- Premium SSD, Standart HDD ve Standart SSD ile aynı düzeyde oluşturulan diğer klasörler geçerli bir performans katmanına karşılık gelmez ve kullanılamaz.
- Performans katmanları nın dışında oluşturulan dosyaları veya klasörleri kaldırın.

Daha fazla bilgi için [bkz.](data-box-deploy-copy-data-from-vhds.md#connect-to-data-box)


## <a name="next-steps"></a>Sonraki adımlar

- [Veri Kutusu Blob depolama sistemi gereksinimleri](data-box-system-requirements-rest.md)hakkında bilgi edinin.
