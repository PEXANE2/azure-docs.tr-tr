---
title: Azure destek isteğini yönetme
description: Destek isteklerini görüntüleme, ileti gönderme, istek önem derecesini değiştirme, Azure desteği ile tanılama bilgilerini paylaşma, kapalı bir destek isteğini yeniden açma ve dosyaları karşıya yükleme işlemlerinin nasıl yapılacağını açıklar.
tags: billing
ms.assetid: 86697fdf-3499-4cab-ab3f-10d40d3c1f70
ms.topic: how-to
ms.date: 12/14/2020
ms.openlocfilehash: 8110f87401da1352309fb55615093d49981c754d
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97504823"
---
# <a name="manage-an-azure-support-request"></a>Azure destek isteğini yönetme

[Azure destek talebi](how-to-create-azure-support-request.md)oluşturduktan sonra, bu makalede ele alınan [Azure Portal](https://portal.azure.com)yönetebilirsiniz. Ayrıca, [Azure destek bileti REST API](/rest/api/support)kullanarak istekleri programlı bir şekilde oluşturabilir ve yönetebilirsiniz.

## <a name="view-support-requests"></a>Destek isteklerini görüntüleme

Destek isteklerinin ayrıntılarını ve durumunu, **Yardım +**  >   **tüm destek isteklerini** destekle ' ya giderek görüntüleyin.

:::image type="content" source="media/how-to-manage-azure-support-request/all-requests-lower.png" alt-text="Tüm destek istekleri":::

Bu sayfada, destek isteklerini arayabilir, filtreleyip sıralayabilirsiniz. Önem derecesi ve istekle ilişkili tüm iletiler dahil olmak üzere ayrıntıları görüntülemek için bir destek isteği seçin.

## <a name="send-a-message"></a>İleti gönderme

1. **Tüm destek istekleri** sayfasında, destek isteği ' ni seçin.

1. **Destek isteği** sayfasında **Yeni ileti**' yı seçin.

1. İletinizi girin ve **Gönder**' i seçin.

## <a name="change-the-severity-level"></a>Önem düzeyini değiştirme

> [!NOTE]
> En yüksek önem düzeyi, [destek planınıza](https://azure.microsoft.com/support/plans)bağlıdır.
>

1. **Tüm destek istekleri** sayfasında, destek isteği ' ni seçin.

1. **Destek isteği** sayfasında **Değiştir**' i seçin.

    :::image type="content" source="media/how-to-manage-azure-support-request/change-severity.png" alt-text="Destek isteği önem derecesini Değiştir":::

1. Azure portal, isteğinizin zaten bir destek mühendisine atanmış olmasına bağlı olarak iki ekranın birini gösterir:

    - İsteğiniz atanmamışsa aşağıdakine benzer bir ekran görürsünüz. Yeni bir önem düzeyi seçin ve ardından **Değiştir**' i seçin.

        :::image type="content" source="media/how-to-manage-azure-support-request/unassigned-can-change-severity.png" alt-text="Yeni bir önem düzeyi seçin":::

    - İsteğiniz atanmışsa, aşağıdakine benzer bir ekran görürsünüz. **Tamam**' ı seçin, sonra önem düzeyinde bir değişiklik istemek için [Yeni bir ileti](#send-a-message) oluşturun.

        :::image type="content" source="media/how-to-manage-azure-support-request/assigned-cant-change-severity.png" alt-text="Yeni bir önem derecesi düzeyi seçemezsiniz":::

## <a name="share-diagnostic-information-with-azure-support"></a>Tanılama bilgilerini Azure desteği ile paylaşma

Bir destek isteği oluşturduğunuzda, varsayılan olarak **Tanılama bilgilerini paylaşma** seçeneği seçilidir. Bu, Azure desteğinin Azure kaynaklarınızdan [Tanılama bilgileri](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) toplamasına izin verir:

* İstek oluşturulduktan sonra bu seçeneği temizleyemezsiniz.

* İstek oluştururken seçeneğini temizlerseniz, istek oluşturulduktan sonra bu seçeneği belirleyebilirsiniz.

    1. **Tüm destek istekleri** sayfasında, destek isteği ' ni seçin.
    
    1. **Destek isteği** sayfasında **izin ver**' i seçin ve ardından **Evet** ve **Tamam**' ı seçin.
    
        :::image type="content" source="media/how-to-manage-azure-support-request/grant-permission-manage.png" alt-text="Tanılama bilgileri için izinleri verme":::

## <a name="upload-files"></a>Dosyaları karşıya yükleme

Dosya yükleme seçeneğini kullanarak, tanılama dosyalarını veya bir destek isteğiyle ilgili düşündüğünüz diğer dosyaları karşıya yükleyebilirsiniz.

1. **Tüm destek istekleri** sayfasında, destek isteği ' ni seçin.

1. **Destek isteği** sayfasında dosyanızı bulun ve **karşıya yükle**' yi seçin. Birden çok dosya varsa, işlemi tekrarlayın.

    :::image type="content" source="media/how-to-manage-azure-support-request/file-upload.png" alt-text="Dosya yükleme":::

### <a name="file-upload-guidelines"></a>Karşıya dosya yükleme yönergeleri

Karşıya dosya yükleme seçeneğini kullandığınızda aşağıdaki yönergeleri izleyin:

* Gizliliğinizi korumak için, karşıya yüklemenizin kişisel bilgilerini eklemeyin.
* Dosya adı 110 karakterden uzun olmamalıdır.
* Birden fazla dosyayı karşıya yükleyemezsiniz.
* Dosyalar 4 MB 'tan büyük olamaz.
* Tüm dosyalar *. docx* veya *. xlsx* gibi bir dosya adı uzantısına sahip olmalıdır. Aşağıdaki tabloda karşıya yükleme için izin verilen dosya adı uzantıları gösterilmektedir.

| 0-9, A-C     | D-G   | H-M         | N-P   | R-T      | U-W        | X-Z     |
|-------------|-------|-------------|-------|----------|------------|---------|
| .7z         | .dat  | . har        | . ODX  | . rar     | . tdb       | .xlam   |
| . a          | . db   | . HWL        | . oft  | . rdl     | . tdf       | . XLR    |
| . abc        | . DMP  | . ics        | . Old  | . rdlc    | . metin      | .xls    |
| . adm        | .do_  | dosyasında        | . One  | .re_     | .thmx      | .xlsb   |
| .aspx       | .doc  | .java       | .osd  | . reg     | .tif       | .xlsm   |
| . ATF        | .docm | .jpg        | . DıŞı  | . Kaldır  | . trc       | .xlsx   |
| . b          | .docx | . LDF        | . P1   | . Ren     | . TTD       | .xlt    |
| .ba_        | .dotm | . antetli | . pcap | . yeniden adlandır  | .tx_       | .xltx   |
| . bak        | .dotx | . lnk        | . pdb  | . rft     | .txt       | .xml    |
| .bat        | . dtsx | .lo_        | .pdf  | . rpt     | .uccapilog | . xmla   |
| . blg        | . EDS  | günlük        | . Piz  | . rte     | . uccplog   | .xps    |
| .CA_        | . EMF  | . lpk        | . pmls | .rtf     | . udcx      | . xsd    |
| . KABINI        | . eml  | . manifest   | .png  | . Run     | .vb_       | . xsn    |
| . Cap        | . emz  | . Master     | .potx | . saz     | .vbs_      | . xxx    |
| . CATX       | . err  | . mdmp       | .ppt  | .sql     | . vcf       | .z_     |
| . SATıRıNDA        | . etl  | . mof        | .pptm | . sqlplan | . VSD       | .z01    |
| . sıkıştırılmış | . evt  | .mp3        | .pptx | . STP     | . wdb       | .z02    |
| . Kurulumunun     | . evtx | .mpg        | . prn  | . svclog  | . WKS       | . Zi     |
| . CPK        | . DEĞERINE   | .ms_        | . psf  | -        | .wma       | .zi_    |
| . cpp        | .ex_  | . msg        | . pst  | -        | .wmv       | .zip    |
| .cs         | .ex0  | .msi        | . pub  | -        | . wmz       | .zip_   |
| . CSV        | . FRD  | . mso        | -     | -        | . WPS       | . Zipp   |
| . CVR        | .gif  | . msu        | -     | -        | . WPT       | . zip 'lenmiş |
| -           | . Guid | . nfo        | -     | -        | . wsdl      | . zipkopyala  |
| -           | . gz   | -           | -     | -        | . wsp       | . ZIPX   |
| -           | -     | -           | -     | -        | . WTL       | . zit    |
| -           | -     | -           | -     | -        | -          | . ZX    |
| -           | -     | -           | -     | -        | -          | . zzz    |

## <a name="close-a-support-request"></a>Bir destek isteğini kapat

Bir destek isteğini kapatmanız gerekirse, isteğin kapatılmasını isteyen [bir ileti gönderin](#send-a-message) .

## <a name="reopen-a-closed-request"></a>Kapatılan bir isteği yeniden açın

Kapalı bir destek isteğini yeniden açmanız gerekiyorsa, isteği otomatik olarak yeniden açan [Yeni bir ileti](#send-a-message)oluşturun.

## <a name="cancel-a-support-plan"></a>Bir destek planını iptal etme

Bir destek planını iptal etmeniz gerekiyorsa bkz. [bir destek planını Iptal etme](../../cost-management-billing/manage/cancel-azure-subscription.md#cancel-a-support-plan).

## <a name="next-steps"></a>Sonraki adımlar

[Azure destek isteği oluşturma](how-to-create-azure-support-request.md)

[Azure destek bileti REST API](/rest/api/support)
