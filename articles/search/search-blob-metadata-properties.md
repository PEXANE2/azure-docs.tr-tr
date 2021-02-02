---
title: İçerik meta verileri özellikleri
titleSuffix: Azure Cognitive Search
description: Blobların meta veri özellikleri, bir arama dizinindeki alanlara içerik verebilir veya çalışma zamanında dizin oluşturma davranışını bildiren bilgiler sağlayabilir. Bu makalede, Azure Bilişsel Arama 'de desteklenen meta veri özellikleri listelenmektedir.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/03/2021
ms.openlocfilehash: 79443785dbd8619e22358631c9c36a3da4ef2e84
ms.sourcegitcommit: 445ecb22233b75a829d0fcf1c9501ada2a4bdfa3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99477311"
---
# <a name="content-metadata-properties-used-in-blob-indexing-in-azure-cognitive-search"></a>Azure Bilişsel Arama blob dizinlemesi için kullanılan içerik meta verileri özellikleri

Blob 'lar çeşitli içerikler içerebilir ve bu içerik türlerinin çoğunun blob dizinlemesi içinde yararlı olabilecek meta veri özellikleri vardır. Gibi standart blob özellikleri için arama alanları oluşturabileceğiniz gibi **`metadata_storage_name`** , bir belge biçimine özgü meta veri özellikleri için alanlar oluşturabilirsiniz.

## <a name="supported-document-formats"></a>Desteklenen belge biçimleri

Bilişsel Arama, aşağıdaki belge biçimleri için blob dizinlemeyi destekler:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="properties-by-document-format"></a>Belge biçimine göre Özellikler

Aşağıdaki tabloda her belge biçimi için yapılan işlem özetlenmektedir ve bir blob Indexer tarafından ayıklanan meta veri özellikleri açıklanmaktadır.

| Belge biçimi/içerik türü | Ayıklanan meta veriler | İşleme ayrıntıları |
| --- | --- | --- |
| HTML (metin/html) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |HTML biçimlendirmesini şerit ve metin Ayıkla |
| PDF (uygulama/PDF) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla (görüntüler hariç) |
| DOCX (uygulama/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| DOC (Application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| DOCM (Application/vnd.ms-word.document. macroenabled. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| WORD XML (application/vnd. MS-word2006ml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |XML işaretlemesini şerit ve metin Ayıkla |
| WORD 2003 XML (application/vnd. MS-WordML) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date` |XML işaretlemesini şerit ve metin Ayıkla |
| XLSX (application/vnd. openxmlformats-officedocument. elektronik sayfaml. Sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| XLS (application/vnd. MS-Excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| XLSM (application/vnd. MS-Excel. Sheet. makroenabled. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| PPTX (application/vnd. openxmlformats-officedocument. presentationml. Presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| PPT (application/vnd. MS-PowerPoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| PPTM (application/vnd. MS-PowerPoint. Presentation. macroenabled. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| MSG (application/vnd. MS-Outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_from_email`<br/>`metadata_message_to`<br/>`metadata_message_to_email`<br/>`metadata_message_cc`<br/>`metadata_message_cc_email`<br/>`metadata_message_bcc`<br/>`metadata_message_bcc_email`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Eklerden ayıklanan metin de dahil olmak üzere metni ayıkla. `metadata_message_to_email``metadata_message_cc_email`ve `metadata_message_bcc_email` dize koleksiyonlarıdır, alanların geri kalanı dizelerdir.|
| ODT (application/vnd. oassıs. OpenDocument. Text) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| ODS (application/vnd. oassıs. OpenDocument. Spreadsheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| ODP (application/vnd. oassıs. OpenDocument. Presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`title` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| ZIP (uygulama/zip) |`metadata_content_type` |Arşivdeki tüm belgelerden metin Ayıkla |
| GZ (uygulama/gzip) |`metadata_content_type` |Arşivdeki tüm belgelerden metin Ayıkla |
| EPUB (Application/EPUB + zip) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_title`<br/>`metadata_description`<br/>`metadata_language`<br/>`metadata_keywords`<br/>`metadata_identifier`<br/>`metadata_publisher` |Arşivdeki tüm belgelerden metin Ayıkla |
| XML (Application/XML) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> |XML işaretlemesini şerit ve metin Ayıkla |
| JSON (uygulama/JSON) |`metadata_content_type`<br/>`metadata_content_encoding` |Metin ayıklama<br/>NOTE: bir JSON blobundan birden çok belge alanı ayıklamanız gerekiyorsa, Ayrıntılar için bkz. [JSON Bloblarını dizine ekleme](search-howto-index-json-blobs.md) |
| EML (ileti/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Ekleri dahil metin Ayıkla |
| RTF (uygulama/RTF) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_page_count`<br/>`metadata_word_count`<br/> | Metin ayıklama|
| Düz metin (metin/düz) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> | Metin ayıklama|

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Bilişsel Arama'daki Dizin Oluşturucular](search-indexer-overview.md)
* [AI kullanarak blob 'ları anlama](search-blob-ai-integration.md)
* [Blob dizinlemeye genel bakış](search-blob-storage-integration.md)