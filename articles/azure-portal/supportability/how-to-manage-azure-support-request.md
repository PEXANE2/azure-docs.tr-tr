---
title: Azure destek isteğini yönetme
description: Destek isteklerini görüntüleme, ileti gönderme, istek önem derecesini değiştirme, Azure desteği ile tanılama bilgilerini paylaşma, kapalı bir destek isteğini yeniden açma ve dosyaları karşıya yükleme işlemlerinin nasıl yapılacağını açıklar.
tags: billing
ms.assetid: 86697fdf-3499-4cab-ab3f-10d40d3c1f70
ms.topic: how-to
ms.date: 12/14/2020
ms.openlocfilehash: 4d0c03e0035f6b71a23891ac1691f5421c1bdb76
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102502527"
---
# <a name="manage-an-azure-support-request"></a>Azure destek isteğini yönetme

[Azure destek talebi](how-to-create-azure-support-request.md)oluşturduktan sonra, bu makalede ele alınan [Azure Portal](https://portal.azure.com)yönetebilirsiniz. Ayrıca, [Azure destek bileti REST API](/rest/api/support)kullanarak veya [Azure CLI](/cli/azure/azure-cli-support-request)kullanarak, istekleri programlı bir şekilde oluşturabilir ve yönetebilirsiniz.

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

    :::image type="content" source="media/how-to-manage-azure-support-request/file-upload.png" alt-text="Dosyayı karşıya yükle":::

### <a name="file-upload-guidelines"></a>Karşıya dosya yükleme yönergeleri

Karşıya dosya yükleme seçeneğini kullandığınızda aşağıdaki yönergeleri izleyin:

* Gizliliğinizi korumak için, karşıya yüklemenizin kişisel bilgilerini eklemeyin.
* Dosya adı 110 karakterden uzun olmamalıdır.
* Birden fazla dosyayı karşıya yükleyemezsiniz.
* Dosyalar 4 MB 'tan büyük olamaz.
* Tüm dosyalar *. docx* veya *. xlsx* gibi bir dosya adı uzantısına sahip olmalıdır. Aşağıdaki tabloda karşıya yükleme için izin verilen dosya adı uzantıları gösterilmektedir.

| 0-9, A-C    | D-G   | H-N         | O-Q   | R-T      | U-W        | X-Z     |
|-------------|-------|-------------|-------|----------|------------|---------|
| .7z         | .dat  | . har        | . ODX  | . rar     | .uccapilog | .xlam   |
| . a          | . db   | . HWL        | . oft  | . rdl     | . uccplog   | . XLR    |
| . abc        | . DMP  | . ics        | . Old  | . rdlc    | . udcx      | .xls    |
| . adm        | .do_  | dosyasında        | . One  | .re_     | .vb_       | .xlsb   |
| .aspx       | .doc  | .java       | .osd  | . Kaldır  | .vbs_      | .xlsm   |
| . ATF        | .docm | .jpg        | . DıŞı  | . Ren     | . vcf       | .xlsx   |
| . b          | .docx | . LDF        | . P1   | . yeniden adlandır  | . VSD       | .xlt    |
| .ba_        | .dotm | . antetli | . pcap | . rft     | . wdb       | .xltx   |
| . bak        | .dotx | .lo_        | . pdb  | . rpt     | . WKS       | .xml    |
| . blg        | . dtsx | günlük        | .pdf  | . rte     | .wma       | . xmla   |
| .CA_        | . EDS  | . lpk        | . Piz  | .rtf     | .wmv       | .xps    |
| . KABINI        | . EMF  | . manifest   | . pmls | . Run     | . wmz       | . xsd    |
| . Cap        | . eml  | . Master     | .png  | . saz     | . WPS       | . xsn    |
| . CATX       | . emz  | . mdmp       | .potx | .sql     | . WPT       | . xxx    |
| . SATıRıNDA        | . err  | . mof        | .ppt  | . sqlplan | . wsdl      | .z_     |
| . sıkıştırılmış | . etl  | .mp3        | .pptm | . STP     | . wsp       | .z01    |
| . Kurulumunun     | . evt  | .mpg        | .pptx | . svclog  | . WTL       | .z02    |
| . CPK        | . evtx | .ms_        | . prn  | . tdb     | -          | . Zi     |
| . cpp        | . DEĞERINE   | . msg        | . psf  | . tdf     | -          | .zi_    |
| .cs         | .ex_  | . mso        | . pst  | . metin    | -          | .zip    |
| . CSV        | .ex0  | . msu        | . pub  | .thmx    | -          | .zip_   |
| . CVR        | . FRD  | . nfo        | -     | .tif     | -          | . Zipp   |
| -           | .gif  | -           | -     | . trc     | -          | . zip 'lenmiş |
| -           | . Guid | -           | -     | . TTD     | -          | . zipkopyala  |
| -           | . gz   | -           | -     | .tx_     | -          | . ZIPX   |
| -           | -     | -           | -     | .txt     | -          | . zit    |
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
