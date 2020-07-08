---
title: Azure destek isteğini yönetme
description: Destek isteklerini görüntüleme, ileti gönderme, istek önem derecesini değiştirme, Azure desteği ile tanılama bilgilerini paylaşma, kapalı bir destek isteğini yeniden açma ve dosyaları karşıya yükleme işlemlerinin nasıl yapılacağını açıklar.
author: mgblythe
tags: billing
ms.assetid: 86697fdf-3499-4cab-ab3f-10d40d3c1f70
ms.service: azure-supportability
ms.topic: how-to
ms.date: 06/30/2020
ms.author: mblythe
ms.openlocfilehash: f3b4806bf46750d74a54f68bd2ab58e402e75091
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852381"
---
# <a name="manage-an-azure-support-request"></a>Azure destek isteğini yönetme

[Azure destek talebi](how-to-create-azure-support-request.md)oluşturduktan sonra, bu makalede ele alınan [Azure Portal](https://portal.azure.com)yönetebilirsiniz. Ayrıca, [Azure destek bileti REST API](/rest/api/support)kullanarak istekleri programlı bir şekilde oluşturabilir ve yönetebilirsiniz.

## <a name="view-support-requests"></a>Destek isteklerini görüntüleme

Destek isteklerinin ayrıntılarını ve durumunu, **Yardım +**  >   **tüm destek isteklerini**destekle ' ya giderek görüntüleyin.

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
* Tüm dosyalar *. docx* veya *. xlsx*gibi bir dosya adı uzantısına sahip olmalıdır. Aşağıdaki tabloda karşıya yükleme için izin verilen dosya adı uzantıları gösterilmektedir.

| 0-9, A-C    | D-G   | H-M         | N-P   | R-T      | U-W        | X-Z     |
|-------------|-------|-------------|-------|----------|------------|---------|
| .7z         | .dat  | . HWL        | . ODX  | . rar     | . tdb       | .xlam   |
| . a          | . db   | . ics        | . oft  | . rdl     | . tdf       | . XLR    |
| . abc        | . DMP  | dosyasında        | . Old  | . rdlc    | . metin      | .xls    |
| . adm        | . do_  | .java       | . One  | . re_     | .thmx      | .xlsb   |
| .aspx       | .doc  | .jpg        | .osd  | . reg     | .tif       | .xlsm   |
| . ATF        | .docm | . LDF        | . DıŞı  | . Kaldır  | . trc       | .xlsx   |
| . b          | .docx | . antetli | . P1   | . Ren     | . TTD       | .xlt    |
| . ba_        | .dotm | . lnk        | . pcap | . yeniden adlandır  | . tx_       | .xltx   |
| . bak        | .dotx | . lo_        | . pdb  | . rft     | .txt       | .xml    |
| .bat        | . dtsx | günlük        | .pdf  | . rpt     | .uccapilog | . xmla   |
| . blg        | . EDS  | . lpk        | . Piz  | . rte     | . uccplog   | .xps    |
| . CA_        | . EMF  | . manifest   | . pmls | .rtf     | . udcx      | . xsd    |
| . KABINI        | . eml  | . Master     | .png  | . Run     | . vb_       | . xsn    |
| . Cap        | . emz  | . mdmp       | .potx | . saz     | . vbs_      | . xxx    |
| . CATX       | . err  | . mof        | .ppt  | .sql     | . vcf       | . z_     |
| . SATıRıNDA        | . etl  | .mp3        | .pptm | . sqlplan | . VSD       | .z01    |
| . sıkıştırılmış | . evt  | .mpg        | .pptx | . STP     | . wdb       | .z02    |
| . Kurulumunun     | . evtx | . ms_        | . prn  | . svclog  | . WKS       | . Zi     |
| . CPK        | . DEĞERINE   | . msg        | . psf  |   -       | .wma       | . zi_    |
| . cpp        | . ex_  | .msi        | . pst  |  -        | .wmv       | .zip    |
| .cs         | .ex0  | . mso        | . pub  | -         | . wmz       | . zip_   |
| . CSV        | . FRD  | . msu        | -      |-          | . WPS       | . Zipp   |
| . CVR        | .gif  | . nfo        | -      |-          | . WPT       | . zip 'lenmiş |
| -            | . Guid | -            | -      | -         | . wsdl      | . zipkopyala  |
| -            | . gz   | -            | -      | -         | . wsp       | . ZIPX   |
| -            | -      | -            | -      | -         | . WTL       | . zit    |
| -            | -      | -            | -      | -         |     -       | . ZX    |
| -            | -      | -            | -      | -         |  -          | . zzz    |

## <a name="reopen-a-closed-request"></a>Kapatılan bir isteği yeniden açın

Kapalı bir destek isteğini yeniden açmanız gerekiyorsa, isteği otomatik olarak yeniden açan [Yeni bir ileti](#send-a-message)oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

[Azure destek isteği oluşturma](how-to-create-azure-support-request.md)

[Azure destek bileti REST API](/rest/api/support)
