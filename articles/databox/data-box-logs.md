---
title: Azure Veri Kutusu, Azure Veri Kutusu Ağır etkinlikleri izleme ve kaydetme| Microsoft Dokümanlar
description: Azure Veri Kutusu ve Azure Veri Kutusu Ağır siparişinizin çeşitli aşamalarında etkinlikleri nasıl izleyip günlüğe kaydedin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 08/08/2019
ms.author: alkohli
ms.openlocfilehash: 74d38af4a64a184b26bd6ba1105db0d2530d8ba6
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81676402"
---
# <a name="tracking-and-event-logging-for-your-azure-data-box-and-azure-data-box-heavy"></a>Azure Veri Kutunuz ve Azure Veri Kutusu Ağır'ınız için izleme ve olay günlüğü

Veri Kutusu veya Veri Kutusu Ağır siparişi aşağıdaki adımlardan geçer: sipariş verme, ayarlama, veri kopyalama, iade, Azure'a yükleme ve doğrulama ve veri silme. Sırayla her adıma karşılık gelir, siparişe erişimi denetlemek, olayları denetlemek, siparişi izlemek ve oluşturulan çeşitli günlükleri yorumlamak için birden çok eylem de yapabilirsiniz.

Aşağıdaki tablo, Veri Kutusu veya Veri Kutusu Ağır sipariş adımlarının bir özetini ve her adımda siparişi izlemek ve denetlemek için kullanılabilen araçları gösterir.

| Veri Kutusu sipariş aşaması       | Takip ve denetim aracı                                                                        |
|----------------------------|------------------------------------------------------------------------------------------------|
| Sipariş oluşturma               | [RBAC üzerinden siparişte erişim denetimi ayarlama](#set-up-access-control-on-the-order)                                                    |
| Sipariş işlendi            | [Siparişi izleme](#track-the-order) <ul><li> Azure portal </li><li> Nakliye taşıyıcı web sitesi </li><li>E-posta bildirimleri</ul> |
| Cihazı ayarlama              | [Etkinlik günlüklerinde](#query-activity-logs-during-setup) oturum açan aygıt kimlik bilgileri                                              |
| Aygıta veri kopyalama        | Veri kopyalama için [ *error.xml* dosyalarını görüntüleme](#view-error-log-during-data-copy)                                                             |
| Göndermeye hazırlama            | [Cihazdaki BOM dosyalarını](#inspect-bom-during-prepare-to-ship) veya bildirim dosyalarını inceleyin                                      |
| Azure'a veri yükleme       | Azure veri merkezinde veri yükleme sırasında hatalar için [kopyalama günlüklerini gözden geçirme](#review-copy-log-during-upload-to-azure)                         |
| Cihazdan veri silme   | Denetim günlükleri ve sipariş geçmişi de dahil olmak üzere [gözetim günlükleri zincirini görüntüleme](#get-chain-of-custody-logs-after-data-erasure)                |

Bu makalede, Veri Kutusu veya Veri Kutusu Ağır siparişizlemek ve denetlemek için çeşitli mekanizmalar veya araçlar ayrıntılı olarak açıklanır. Bu makaledeki bilgiler her ikisi için de geçerlidir, Veri Kutusu ve Veri Kutusu Ağır. Sonraki bölümlerde, Veri Kutusu'na yapılan tüm başvurular Data Box Heavy için de geçerlidir.

## <a name="set-up-access-control-on-the-order"></a>Siparişte erişim denetimini ayarlama

Sipariş ilk oluşturulduğunda siparişinize kimlerin erişebileceğini kontrol edebilirsiniz. Veri Kutusu siparişine erişimi denetlemek için çeşitli kapsamlarda Rol tabanlı Erişim Denetimi (RBAC) rolleri ayarlayın. RBAC rolü erişim türünü belirler – okuma-yazma, salt okunur, bir işlem alt kümesine okuma-yazma.

Azure Veri Kutusu hizmeti için tanımlanabilecek iki rol şunlardır:

- **Veri Kutusu Okuyucu** - kapsamı tarafından tanımlanan bir sipariş(ler) için salt okunur erişim var. Yalnızca siparişin ayrıntılarını görüntüleyebilirler. Depolama hesaplarıyla ilgili diğer ayrıntılara erişemez veya adres ve benzeri sipariş ayrıntılarını düzenemezler.
- **Veri Kutusu Katılımcısı** - yalnızca bir *depolama hesabına yazma erişimi varsa,* yalnızca belirli bir depolama hesabına veri aktarmak için bir sipariş oluşturabilir. Bir depolama hesabına erişimleri yoksa, verileri hesaba kopyalamak için veri kutusu sırası bile oluşturamaz. Bu rol, Depolama hesabıyla ilgili izinleri tanımlamaz veya depolama hesaplarına erişim izni vermez.  

Bir siparişe erişimi kısıtlamak için şunları yapabilirsiniz:

- Sipariş düzeyinde bir rol atayın. Kullanıcı, yalnızca belirli Veri Kutusu sırası ile etkileşimde bulunan roller tarafından tanımlanan bu izinlere sahiptir ve başka bir şey yoktur.
- Kaynak grubu düzeyinde bir rol atayın, kullanıcı bir kaynak grubu içindeki tüm Veri Kutusu siparişlerine erişebilir.

Önerilen RBAC kullanımı hakkında daha fazla bilgi için [Azure RBAC için en iyi uygulamalara](../role-based-access-control/best-practices.md)bakın.

## <a name="track-the-order"></a>Siparişi izleme

Siparişinizi Azure portalı ndan ve gönderi operatörü web sitesinden takip edebilirsiniz. Veri Kutusu siparişini herhangi bir zamanda izlemek için aşağıdaki mekanizmalar devreye sayılmaktadır:

- Aygıt Azure veri merkezinde veya binanızdayken siparişi izlemek için, Azure portalında **Genel Bakış > Veri Kutusu siparişinize** gidin.

    ![Sipariş durumunu görüntüleme ve izleme yok](media/data-box-logs/overview-view-status-1.png)

- Cihaz geçiş sırasında siparişi izlemek için, örneğin ABD'deki UPS web sitesi gibi bölgesel taşıyıcı web sitesine gidin. Siparişinizle ilişkili izleme numarasını sağlayın.
- Veri Kutusu, sipariş oluşturulduğunda sağlanan e-postalara bağlı olarak sipariş durumu değiştiğinde e-posta bildirimleri de gönderir. Tüm Veri Kutusu sipariş durumlarının listesi için [bkz.](data-box-portal-admin.md#view-order-status) Siparişle ilişkili bildirim ayarlarını değiştirmek için [bildirim ayrıntılarını düzenle'ye](data-box-portal-admin.md#edit-notification-details)bakın.

## <a name="query-activity-logs-during-setup"></a>Kurulum sırasında etkinlik günlüklerini sorgula

- Veri Kutunuz binanıza kilitli bir durumda gelir. Siparişiniz için Azure portalında bulunan aygıt kimlik bilgilerini kullanabilirsiniz.  

    Bir Veri Kutusu ayarlandığında, aygıt kimlik bilgilerine kimin eriştedildiğini bilmeniz gerekebilir. **Aygıt kimlik bilgilerine** kimin eriştüğüni bulmak için Etkinlik günlüklerini sorgulayabilirsiniz.  **Aygıt ayrıntılarına > Kimlik Bilgileri** blade'ine erişmesini içeren `ListCredentials` herhangi bir eylem eylem olarak etkinlik günlüklerine kaydedilir.

    ![Etkinlik günlüklerini sorgulama](media/data-box-logs/query-activity-log-1.png)

- Veri Kutusu'ndaki her işaret gerçek zamanlı olarak kaydedilir. Ancak, bu bilgiler yalnızca sipariş başarıyla tamamlandıktan sonra [Denetim günlüklerinde](#audit-logs) kullanılabilir.

## <a name="view-error-log-during-data-copy"></a>Veri kopyalama sırasında hata günlüğünü görüntüleme

Veri Kutusu veya Veri Kutusu Ağır'a kopyalanması sırasında, kopyalanan verilerle ilgili herhangi bir sorun varsa bir hata dosyası oluşturulur.

### <a name="errorxml-file"></a>Hata.xml dosyası

Kopyalama işlerinin hiçbir hata olmadan tamamlandığından emin olun. Kopyalama işlemi sırasında hatalar varsa, günlükleri **Bağlan ve kopyala** sayfasından indirin.

- Veri Kutunuzdaki yönetilen bir disk klasörüne hizalanmış 512 bayt olmayan bir dosyayı kopyaladıysanız, dosya evreleme depolama hesabınıza sayfa blob olarak yüklenmez. Günlüklerde bir hata görürsünüz. Dosyayı kaldırın ve 512 bayt hizalanmış bir dosyayı kopyalayın.
- Bir VHDX veya dinamik bir VHD veya farklı bir VHD (bu dosyalar desteklenmez) kopyaladıysanız, günlüklerde bir hata görürsünüz.

Burada yönetilen disklere kopyalanırken farklı hatalar için *hata.xml* bir örnektir.

```xml
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\differencing-vhd-022019.vhd</file>
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\dynamic-vhd-022019.vhd</file>
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\insidefixedvhdx-022019.vhdx</file>
<file error="ERROR_BLOB_OR_FILE_TYPE_UNSUPPORTED">\StandardHDD\testvhds\insidediffvhd-022019.vhd</file>
```

Burada *hata.xml* farklı hatalar için bir örnek sayfa blobs kopyalanırken.

```xml
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File100Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File786Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File513Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File10Bytes</file>
<file error="ERROR_BLOB_OR_FILE_SIZE_ALIGNMENT">\PageBlob512NotAligned\File500Bytes</file>
```


Burada blobs engellemek için kopyalama yaparken farklı hatalar için *hata.xml* bir örnektir.

```xml
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\ab</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\invalid dns name</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\morethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortesting</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\testdirectory-~!@#$%^&amp;()_+{}</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\test__doubleunderscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\-startingwith-hyphen</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\Starting with Capital</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\_startingwith_underscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\55555555--4444--3333--2222--111111111111</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\1</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\11111111-_2222-_3333-_4444-_555555555555</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\test--doublehyphen</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5Ni3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMS3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMy3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_CONTROL">\InvalidUnicodeFiles\Ã.txt</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwNS3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5OS3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMi3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwNC3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5OC3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTMwMC3vv70=</file>
<file error="ERROR_BLOB_OR_FILE_NAME_CHARACTER_ILLEGAL" name_encoding="Base64">XEludmFsaWRVbmljb2RlRmlsZXNcU3BjQ2hhci01NTI5Ny3vv70=</file>
```

Azure Dosyaları'na kopyalanırken farklı hatalar için *hata.xml* örneği aşağıda verilmiştir.

```xml
<file error="ERROR_BLOB_OR_FILE_SIZE_LIMIT">\AzFileMorethan1TB\AzFile1.2TB</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\testdirectory-~!@#$%^&amp;()_+{}</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\55555555--4444--3333--2222--111111111111</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\-startingwith-hyphen</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\11111111-_2222-_3333-_4444-_555555555555</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_IMPROPER_DASH">\test--doublehyphen</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\ab</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\invalid dns name</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\test__doubleunderscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\morethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortestingmorethan63charactersfortesting</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\_startingwith_underscore</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_LENGTH">\1</file>
<file error="ERROR_CONTAINER_OR_SHARE_NAME_ALPHA_NUMERIC_DASH">\Starting with Capital</file>
```

Yukarıdaki durumların her birinde, bir sonraki adıma geçmeden önce hataları çözümleyin. SMB veya NFS protokolleri aracılığıyla Veri Kutusu'na kopyalanırken alınan hatalar hakkında daha fazla bilgi [için, Sorun Giderme Veri Kutusu ve Veri Kutusu Ağır sorunları'na](data-box-troubleshoot.md)gidin. REST üzerinden Veri Kutusu'na kopyalanırken alınan hatalar hakkında bilgi [için, Sorun Giderme Veri Kutusu Blob depolama sorunları](data-box-troubleshoot-rest.md)'na gidin.

## <a name="inspect-bom-during-prepare-to-ship"></a>Gemiye hazırlanma sırasında BOM'u inceleyin

Gemiye hazırlanma sırasında, Malzeme Faturası (BOM) veya manifesto dosyası olarak bilinen dosyaların bir listesi oluşturulur.

- Veri Kutusuna kopyalanan gerçek adları ve dosya sayısını doğrulamak için bu dosyayı kullanın.
- Dosyaların gerçek boyutlarını doğrulamak için bu dosyayı kullanın.
- *CRC64'ün* sıfır olmayan bir dizeyle karşılık geldiğini doğrulayın. <!--A null value for crc64 indicates that there was a reparse point error)-->

Gönderiye hazırlanma sırasında alınan hatalar hakkında daha fazla bilgi [için, Sorun Giderme Veri Kutusu ve Veri Kutusu Ağır sorunları](data-box-troubleshoot.md)na gidin.

### <a name="bom-or-manifest-file"></a>BOM veya manifesto dosyası

BOM veya manifest dosyası, Veri Kutusu aygıtına kopyalanan tüm dosyaların listesini içerir. BOM dosyasında dosya adları ve karşılık gelen boyutların yanı sıra çekler vardır. Blok lekeleri, sayfa lekeleri, Azure Dosyaları, REST API'leri üzerinden kopyalamak ve Kopya için Veri Kutusu'ndaki yönetilen diskler için ayrı bir BOM dosyası oluşturulur. Gemiye hazırlanma sırasında cihazın yerel web UI'ından BOM dosyalarını indirebilirsiniz.

Bu dosyalar ayrıca Veri Kutusu aygıtında da bulunur ve Azure veri merkezindeki ilişkili depolama hesabına yüklenir.

### <a name="bom-file-format"></a>BOM dosya biçimi

BOM veya manifest dosyası aşağıdaki genel biçimi vardır:

`<file size = "file-size-in-bytes" crc64="cyclic-redundancy-check-string">\folder-path-on-data-box\name-of-file-copied.md</file>`

Burada, veriler Veri Kutusu'ndaki blok blob paylaşımına kopyalandığında oluşturulan bir bildirim örneği verebilirsiniz.

```
<file size="10923" crc64="0x51c78833c90e4e3f">\databox\media\data-box-deploy-copy-data\connect-shares-file-explorer1.png</file>
<file size="15308" crc64="0x091a8b2c7a3bcf0a">\databox\media\data-box-deploy-copy-data\get-share-credentials2.png</file>
<file size="53486" crc64="0x053da912fb45675f">\databox\media\data-box-deploy-copy-data\nfs-client-access.png</file>
<file size="6093" crc64="0xadb61d0d7c6d4deb">\databox\data-box-cable-options.md</file>
<file size="6499" crc64="0x080add29add367d9">\databox\data-box-deploy-copy-data-via-nfs.md</file>
<file size="11089" crc64="0xc3ce6b13a4fe3001">\databox\data-box-deploy-copy-data-via-rest.md</file>
<file size="7749" crc64="0xd2e346a4588e307a">\databox\data-box-deploy-ordered.md</file>
<file size="14275" crc64="0x260296e5d1b1608a">\databox\data-box-deploy-copy-data.md</file>
<file size="4077" crc64="0x2bb0a170225bceec">\databox\data-box-deploy-picked-up.md</file>
<file size="15447" crc64="0xcec0ca8527720b3c">\databox\data-box-portal-admin.md</file>
<file size="9126" crc64="0x820856b5a54321ad">\databox\data-box-overview.md</file>
<file size="10963" crc64="0x5e9a14f9f4784fd8">\databox\data-box-safety.md</file>
<file size="5941" crc64="0x8631d62fbc038760">\databox\data-box-security.md</file>
<file size="12536" crc64="0x8c8ff93e73d665ec">\databox\data-box-system-requirements-rest.md</file>
<file size="3220" crc64="0x7257a263c434839a">\databox\data-box-system-requirements.md</file>
```

BOM veya manifesto dosyaları da Azure depolama hesabına kopyalanır. Azure'a yüklenen dosyaların Veri Kutusu'na kopyalanan verilerle eşleştiğinden doğrulamak için BOM veya bildirim dosyalarını kullanabilirsiniz.

## <a name="review-copy-log-during-upload-to-azure"></a>Azure'a yükleme sırasında kopyalama günlüğünü gözden geçirme

Azure'a veri yüklemesırasında bir kopyalama günlüğü oluşturulur.

### <a name="copy-log"></a>Günlüğü kopyalama

İşlenen her sipariş için, Veri Kutusu hizmeti ilişkili depolama hesabında kopya günlüğü oluşturur. Kopyalama günlüğünde yüklenen toplam dosya sayısı ve Veri Kutusu'ndan Azure depolama hesabınıza veri kopyalanması sırasında hata yapan dosya sayısı bulunur.

Azure'a yükleme sırasında Döngüsel Artıklık Denetimi (CRC) hesaplaması yapılır. Veri kopyalamave veri yüklemesinden sonra crcs karşılaştırılır. CRC uyuşmazlığı, ilgili dosyaların yüklenemediğini gösterir.

Varsayılan olarak, günlükleri adlı `copylog`bir kapsayıcıya yazılır. Günlükler aşağıdaki adlandırma kuralıyla depolanır:

`storage-account-name/databoxcopylog/ordername_device-serial-number_CopyLog_guid.xml`.

Kopyalama günlüğü yolu da portal için **Genel Bakış** bıçak görüntülenir.

![Tamamlandığında Genel Bakış bıçağında oturum açma yolu](media/data-box-logs/copy-log-path-1.png)

### <a name="upload-completed-successfully"></a>Yükleme başarıyla tamamlandı 

Aşağıdaki örnek, başarıyla tamamlanan bir Veri Kutusu yüklemesi için bir kopyalama günlüğünün genel biçimini açıklar:

```
<?xml version="1.0"?>
-<CopyLog Summary="Summary">
<Status>Succeeded</Status>
<TotalFiles>45</TotalFiles>
<FilesErrored>0</FilesErrored>
</CopyLog>
```

### <a name="upload-completed-with-errors"></a>Hatalarla tamamlanan yükleme 

Azure'a yükleme de hatalarla tamamlanabilir.

![Hatalarla tamamlandığında Genel Bakış bıçağında oturum açma yolu](media/data-box-logs/copy-log-path-2.png)

Yüklemenin hatalarla tamamlandığı bir kopyalama günlüğü örneği aşağıda verilmiştir:

```xml
<ErroredEntity Path="iso\samsungssd.iso">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>409</ErrorCode>
  <ErrorMessage>The blob type is invalid for this operation.</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity Path="iso\iSCSI_Software_Target_33.iso">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>409</ErrorCode>
  <ErrorMessage>The blob type is invalid for this operation.</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><CopyLog Summary="Summary">
  <Status>Failed</Status>
  <TotalFiles_Blobs>72</TotalFiles_Blobs>
  <FilesErrored>2</FilesErrored>
</CopyLog>
```
### <a name="upload-completed-with-warnings"></a>Uyarılarla tamamlanan yükleme

Azure'a yükleme, verilerinizin Azure adlandırma kurallarına uymayan kapsayıcı/blob/dosya adları varsa ve adları azure'a yüklemek için değiştirildiyse uyarılarla tamamlar.

![Uyarılarla tamamlandığında genel bakış bıçağını kopyalama yolu](media/data-box-logs/copy-log-path-3.png)

Aşağıda, Azure adlandırma kurallarına uymayan kapsayıcıların Azure'a veri yüklemesi sırasında yeniden adlandırılmasının yapıldığı bir kopya günlüğü örneği verilmiştir.

Kapsayıcılar için yeni benzersiz adlar biçimindedir `DataBox-GUID` ve kapsayıcının verileri yeni yeniden adlandırılmış kapsayıcıya konur. Kopyalama günlüğü, kapsayıcının eski ve yeni kapsayıcı adını belirtir.

```xml
<ErroredEntity Path="New Folder">
   <Category>ContainerRenamed</Category>
   <ErrorCode>1</ErrorCode>
   <ErrorMessage>The original container/share/blob has been renamed to: DataBox-3fcd02de-bee6-471e-ac62-33d60317c576 :from: New Folder :because either the name has invalid character(s) or length is not supported</ErrorMessage>
  <Type>Container</Type>
</ErroredEntity>
```

Burada, Azure adlandırma kurallarına uymayan blob'ların veya dosyaların Azure'a veri yüklemesi sırasında yeniden adlandırılmasının yer aldığı bir kopyalama günlüğü örneği verilmiştir. Yeni blob veya dosya adları, kap için göreli yolun SHA256 özetine dönüştürülür ve hedef türüne göre yola yüklenir. Hedef blok lekeleri, sayfa lekeleri veya Azure Dosyaları olabilir.

Eski `copylog` ve yeni blob veya dosya adını ve Azure'daki yolu belirtir.

```xml
<ErroredEntity Path="TesDir028b4ba9-2426-4e50-9ed1-8e89bf30d285\Ã">
  <Category>BlobRenamed</Category>
  <ErrorCode>1</ErrorCode>
  <ErrorMessage>The original container/share/blob has been renamed to: PageBlob/DataBox-0xcdc5c61692e5d63af53a3cb5473e5200915e17b294683968a286c0228054f10e :from: Ã :because either name has invalid character(s) or length is not supported</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity Path="TesDir9856b9ab-6acb-4bc3-8717-9a898bdb1f8c\Ã">
  <Category>BlobRenamed</Category>
  <ErrorCode>1</ErrorCode>
  <ErrorMessage>The original container/share/blob has been renamed to: AzureFile/DataBox-0xcdc5c61692e5d63af53a3cb5473e5200915e17b294683968a286c0228054f10e :from: Ã :because either name has invalid character(s) or length is not supported</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity Path="TesDirf92f6ca4-3828-4338-840b-398b967d810b\Ã">
  <Category>BlobRenamed</Category>
  <ErrorCode>1</ErrorCode>
  <ErrorMessage>The original container/share/blob has been renamed to: BlockBlob/DataBox-0xcdc5c61692e5d63af53a3cb5473e5200915e17b294683968a286c0228054f10e :from: Ã :because either name has invalid character(s) or length is not supported</ErrorMessage>
  <Type>File</Type>
</ErroredEntity>
```

## <a name="get-chain-of-custody-logs-after-data-erasure"></a>Veri silindikten sonra gözaltı günlükleri zincirini alın

Veriler NIST SP 800-88 Revizyon 1 yönergeleri uyarınca Veri Kutusu disklerinden silindikten sonra, velayet günlüğü zinciri kullanılabilir. Bu günlükler denetim günlüklerini ve sipariş geçmişini içerir. BOM veya manifesto dosyaları da denetim günlükleri ile kopyalanır.

### <a name="audit-logs"></a>Denetim günlükleri

Denetim günlükleri, Azure veri merkezinin dışında yken Veri Kutusu veya Veri Kutusu Ağır'daki paylaşımları nasıl açAbilmek ve bu paylaşımlara erişeceklerine ilişkin bilgiler içerir. Bu günlükler şu adreste bulunur:`storage-account/azuredatabox-chainofcustodylogs`

Aşağıda, veri kutusundan denetim günlüğünün bir örneği verebilirsiniz:

```
9/10/2018 8:23:01 PM : The operating system started at system time ‎2018‎-‎09‎-‎10T20:23:01.497758400Z.
9/10/2018 8:23:42 PM : An account was successfully logged on.
Subject:
    Security ID:        S-1-5-18
    Account Name:       WIN-DATABOXADMIN
    Account Domain: Workgroup
    Logon ID:       0x3E7
Logon Information:
    Logon Type:     3
    Restricted Admin Mode:  -
    Virtual Account:        No
    Elevated Token:     No
Impersonation Level:        Impersonation
New Logon:
    Security ID:        S-1-5-7
    Account Name:       ANONYMOUS LOGON
    Account Domain: NT AUTHORITY
    Logon ID:       0x775D5
    Linked Logon ID:    0x0
    Network Account Name:   -
    Network Account Domain: -
    Logon GUID:     {00000000-0000-0000-0000-000000000000}
Process Information:
    Process ID:     0x4
    Process Name:       
Network Information:
    Workstation Name:   -
    Source Network Address: -
    Source Port:        -
Detailed Authentication Information:
    Logon Process:      NfsSvr
    Authentication Package:MICROSOFT_AUTHENTICATION_PACKAGE_V1_0
    Transited Services: -
    Package Name (NTLM only):   -
    Key Length:     0
This event is generated when a logon session is created. It is generated on the computer that was accessed. 
The subject fields indicate the account on the local system which requested the logon. This is most commonly a service such as the Server service, or a local process such as Winlogon.exe or Services.exe. 
The logon type field indicates the kind of logon that occurred. The most common types are 2 (interactive) and 3 (network).
The New Logon fields indicate the account for whom the new logon was created, i.e. the account that was logged on.
The network fields indicate where a remote logon request originated. Workstation name is not always available and may be left blank in some cases.
The impersonation level field indicates the extent to which a process in the logon session can impersonate.
The authentication information fields provide detailed information about this specific logon request.
    - Logon GUID is a unique identifier that can be used to correlate this event with a KDC event.
    - Transited services indicate which intermediate services have participated in this logon request.
    - Package name indicates which sub-protocol was used among the NTLM protocols.
    - Key length indicates the length of the generated session key. This will be 0 if no session key was requested.
9/10/2018 8:25:58 PM : An account was successfully logged on.
```


## <a name="download-order-history"></a>Sipariş geçmişi indirme

Sipariş geçmişi Azure portalında kullanılabilir. Sipariş tamamlandıysa ve aygıt temizleme (disklerden veri silme) tamamlandıysa, ardından aygıt siparişinize gidin ve **Sipariş ayrıntılarına**gidin. **Sipariş geçmişi indirme** seçeneği bulunur. Daha fazla bilgi için [bkz.](data-box-portal-admin.md#download-order-history)

Sipariş geçmişinde gezinirseniz, şunları görürsünüz:

- Cihazınız için taşıyıcı izleme bilgileri.
- *SecureErase* etkinliği olan olaylar. Bu olaylar, diskteki verilerin silinmesi ile karşılık gelir.
- Veri Kutusu günlük bağlantıları. *Denetim günlükleri,* kopyalama *günlükleri*ve *BOM* dosyaları için yollar sunulur.

Azure portalından sipariş geçmişi günlüğünün bir örneği aşağıda veda edebilirsiniz:

```
-------------------------------
Microsoft Data Box Order Report
-------------------------------
Name                                               : gus-poland                              
StartTime(UTC)                              : 9/19/2018 8:49:23 AM +00:00                       
DeviceType                                     : DataBox                                           
-------------------
Data Box Activities
-------------------
Time(UTC)                 | Activity                       | Status          | Description

9/19/2018 8:49:26 AM      | OrderCreated                   | Completed       |
10/2/2018 7:32:53 AM      | DevicePrepared                 | Completed       |
10/3/2018 1:36:43 PM      | ShippingToCustomer             | InProgress      | Shipment picked up. Local Time : 10/3/2018 1:36:43 PM at AMSTERDAM-NLD                                                                                
10/4/2018 8:23:30 PM      | ShippingToCustomer             | InProgress      | Processed at AMSTERDAM-NLD. Local Time : 10/4/2018 8:23:30 PM at AMSTERDAM-NLD                                                                        
10/4/2018 11:43:34 PM     | ShippingToCustomer             | InProgress      | Departed Facility in AMSTERDAM-NLD. Local Time : 10/4/2018 11:43:34 PM at AMSTERDAM-NLD
10/5/2018 8:13:49 AM      | ShippingToCustomer             | InProgress      | Arrived at Delivery Facility in BRIGHTON-GBR. Local Time : 10/5/2018 8:13:49 AM at LAMBETH-GBR                                                         
10/5/2018 9:13:24 AM      | ShippingToCustomer             | InProgress      | With delivery courier. Local Time : 10/5/2018 9:13:24 AM at BRIGHTON-GBR                                                                               
10/5/2018 12:03:04 PM     | ShippingToCustomer             | Completed       | Delivered - Signed for by. Local Time : 10/5/2018 12:03:04 PM at BRIGHTON-GBR                                                                          
1/25/2019 3:19:25 PM      | ShippingToDataCenter           | InProgress      | Shipment picked up. Local Time : 1/25/2019 3:19:25 PM at BRIGHTON-GBR                                                                                       
1/25/2019 8:03:55 PM      | ShippingToDataCenter           | InProgress      | Processed at BRIGHTON-GBR. Local Time : 1/25/2019 8:03:55 PM at LAMBETH-GBR                                                                            
1/25/2019 8:04:58 PM      | ShippingToDataCenter           | InProgress      | Departed Facility in BRIGHTON-GBR. Local Time : 1/25/2019 8:04:58 PM at BRIGHTON-GBR                                                                    
1/25/2019 9:06:09 PM      | ShippingToDataCenter           | InProgress      | Arrived at Sort Facility LONDON-HEATHROW-GBR. Local Time : 1/25/2019 9:06:09 PM at LONDON-HEATHROW-GBR                                                
1/25/2019 9:48:54 PM      | ShippingToDataCenter           | InProgress      | Processed at LONDON-HEATHROW-GBR. Local Time : 1/25/2019 9:48:54 PM at LONDON-HEATHROW-GBR                                                            
1/25/2019 10:30:20 PM     | ShippingToDataCenter           | InProgress      | Departed Facility in LONDON-HEATHROW-GBR. Local Time : 1/25/2019 10:30:20 PM at LONDON-HEATHROW-GBR
1/28/2019 7:11:35 AM      | ShippingToDataCenter           | InProgress      | Arrived at Delivery Facility in AMSTERDAM-NLD. Local Time : 1/28/2019 7:11:35 AM at AMSTERDAM-NLD                                                     
1/28/2019 9:07:57 AM      | ShippingToDataCenter           | InProgress      | With delivery courier. Local Time : 1/28/2019 9:07:57 AM at AMSTERDAM-NLD                                                                             
1/28/2019 1:35:56 PM      | ShippingToDataCenter           | InProgress      | Scheduled for delivery. Local Time : 1/28/2019 1:35:56 PM at AMSTERDAM-NLD                                                                            
1/28/2019 2:57:48 PM      | ShippingToDataCenter           | Completed       | Delivered - Signed for by. Local Time : 1/28/2019 2:57:48 PM at AMSTERDAM-NLD
1/29/2019 2:18:43 PM      | PhysicalVerification           | Completed       |
1/29/2019 3:49:50 PM      | DeviceBoot                     | Completed       | Appliance booted up successfully.
1/29/2019 3:49:51 PM      | AnomalyDetection               | Completed       | No anomaly detected.
2/12/2019 10:37:03 PM     | DataCopy                       | Resumed         |
2/13/2019 12:05:15 AM     | DataCopy                       | Resumed         |
2/15/2019 7:07:34 PM      | DataCopy                       | Completed       | Copy Completed.
2/15/2019 7:47:32 PM      | SecureErase                    | Started         |
2/15/2019 8:01:10 PM      | SecureErase                    | Completed       | Azure Data Box:<Device-serial-no> has been sanitized according to NIST 800-88 Rev 1.
------------------
Data Box Log Links
------------------
Account Name         : gusacct
Copy Logs Path       : databoxcopylog/gus-poland_<Device-serial-no>_CopyLog_<GUID>.xml
Audit Logs Path      : azuredatabox-chainofcustodylogs\<GUID>\<Device-serial-no>
BOM Files Path       : azuredatabox-chainofcustodylogs\<GUID>\<Device-serial-no>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Veri Kutunuzdaki ve Veri Kutusu Ağır'ınızdaki sorunları nasıl gidereceklerini](data-box-troubleshoot.md)öğrenin.
