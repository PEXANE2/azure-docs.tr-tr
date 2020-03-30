---
title: Azure Monitör'de günlük verilerini çözümlemek için görünümler oluşturma | Microsoft Dokümanlar
description: Azure Monitörü'nde Görünüm Tasarımcısı'nı kullanarak, Azure portalında görüntülenen özel görünümler oluşturabilir ve Günlük Analizi çalışma alanındaki verilerle ilgili çeşitli görselleştirmeler içerebilir. Bu makalede, Görünüm Tasarımcısı'na genel bir bakış içerir ve özel görünümler oluşturma ve düzenleme yordamları sunar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/10/2019
ms.openlocfilehash: 9a7521f61dc59bd954629a05638c159ab0e70556
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658498"
---
# <a name="create-custom-views-by-using-view-designer-in-azure-monitor"></a>Azure Monitör'de Görünüm Tasarımcısı'nı kullanarak özel görünümler oluşturun
Azure Monitörü'nde Görünüm Tasarımcısı'nı kullanarak, Azure portalında Günlük Analizi çalışma alanınızdaki verileri görselleştirmenize yardımcı olabilecek çeşitli özel görünümler oluşturabilirsiniz. Bu makalede, Görünüm Tasarımcısı ve özel görünümler oluşturma ve düzenleme yordamları genel bir bakış sunar.

> [!IMPORTANT]
> Azure Monitor'daki görünümler aşamalı olarak devre dışı ediliyor ve ek işlevler sağlayan [çalışma kitaplarıyla](workbooks-overview.md) değiştiriliyor. Mevcut görünümlerinizi çalışma kitaplarına dönüştürme yle ilgili ayrıntılar için [Azure Monitor görünüm tasarımcısını çalışma kitapları geçiş kılavuzuna](view-designer-conversion-overview.md) bakın.

Görünüm Tasarımcısı hakkında daha fazla bilgi için bkz:

* [Kutucuk başvurusu](view-designer-tiles.md): Özel görünümlerinizde kullanılabilir kutucukların her biri için ayarlar için bir başvuru kılavuzu sağlar.
* [Visualization parçası başvurusu](view-designer-parts.md): Özel görünümlerinizde bulunan görselleştirme parçalarının ayarları için bir başvuru kılavuzu sağlar.


## <a name="concepts"></a>Kavramlar
Görünümler, Azure portalındaki Azure Monitöre **Genel Bakış** sayfasında görüntülenir. **Öngörüler** bölümünde **daha fazla** seçeneğini tıklayarak **Azure Monitor** menüsünden bu sayfayı açın. Her özel görünümdeki döşemeler alfabetik olarak görüntülenir ve izleme çözümleri için kutucuklar aynı çalışma alanına yüklenir.

![Genel bakış sayfası](media/view-designer/overview-page.png)

Görünüm Tasarımcısı ile oluşturduğunuz görünümler aşağıdaki tabloda açıklanan öğeleri içerir:

| Bölüm | Açıklama |
|:--- |:--- |
| Kutucuklar | Azure Monitöre **Genel Bakış** sayfanızda görüntülenir. Her döşeme, temsil eder özel görünümün görsel bir özetini görüntüler. Her döşeme türü, kayıtlarınızın farklı bir görselleştirmesini sağlar. Özel bir görünüm görüntülemek için bir döşeme seçersiniz. |
| Özel görünüm | Bir kiremit seçtiğinizde görüntülenir. Her görünüm bir veya daha fazla görselleştirme parçası içerir. |
| Görselleştirme parçaları | Bir veya daha fazla [günlük sorgusunu](../log-query/log-query-overview.md)temel alan Log Analytics çalışma alanında verilerin görselleştirilmesini sunun. Çoğu parça, üst düzey görselleştirme sağlayan bir üstbilgi ve en üst sonuçları görüntüleyen bir liste içerir. Her parça türü, Log Analytics çalışma alanındaki kayıtların farklı bir görselleştirmesini sağlar. Ayrıntılı kayıtlar sağlayan bir günlük sorgusu gerçekleştirmek için parçadaki öğeleri seçersiniz. |

## <a name="required-permissions"></a>Gerekli izinler
Görünümoluşturmak veya değiştirmek için Log Analytics çalışma alanında en az [katılımcı düzeyinde izinlere](manage-access.md#manage-access-using-azure-permissions) ihtiyacınız vardır. Bu izne sahip değilseniz, Tasarımcıyı Görüntüle seçeneği menüde görüntülenmez.


## <a name="work-with-an-existing-view"></a>Varolan bir görünümle çalışma
Görünüm Tasarımcısı ile oluşturulan görünümler aşağıdaki seçenekleri görüntüler:

![Genel bakış menüsü](media/view-designer/overview-menu.png)

Seçenekler aşağıdaki tabloda açıklanmıştır:

| Seçenek | Açıklama |
|:--|:--|
| Yenile   | Görünümü en son verilerle yeniler. | 
| Günlükler      | Günlük sorgularıyla verileri çözümlemek için [Log Analytics'i](../log-query/portals.md) açar. |
| Düzenle       | İçeriğini ve yapılandırmasını düzenlemek için Görünümü Tasarımcı'da açar.  |
| Kopyalama      | Yeni bir görünüm oluşturur ve Görünüm Tasarımcısı'nda açar. Yeni görünümün adı özgün adla aynıdır, ancak *kopya* ona eklenen ile. |
| Tarih aralığı | Görünüme dahil olan veriler için tarih ve saat aralığı filtresini ayarlayın. Bu tarih aralığı, görünümdeki sorgularda ayarlanan tarih aralıklarından önce uygulanır.  |
| +          | Görünüm için tanımlanan özel bir filtre tanımlayın. |


## <a name="create-a-new-view"></a>Yeni bir görünüm oluşturma
Log Analytics çalışma alanınızın menüsünde **View Designer'ı** seçerek Görünüm Tasarımcısı'nda yeni bir görünüm oluşturabilirsiniz.

![Tasarımcı döşemesi görüntüle](media/view-designer/view-designer-tile.png)


## <a name="work-with-view-designer"></a>Görünüm Tasarımcısı ile Çalışma
Yeni görünümler oluşturmak veya varolanları yeniden oluşturmak için Görünüm Tasarımcısı'nı kullanırsınız. 

Görünüm Tasarımcısı'nın üç bölmesi vardır: 
* **Tasarım**: Oluşturduğunuz veya düzenlediğiniz özel görünümü içerir. 
* **Denetimler**: **Tasarım** bölmesine eklediğiniz döşemeleri ve parçaları içerir. 
* **Özellikler**: Döşemelerin veya seçili parçaların özelliklerini görüntüler.

![Görünüm Tasarımcısı](media/view-designer/view-designer-screenshot.png)

### <a name="configure-the-view-tile"></a>Görünüm döşemesini yapılandırma
Özel bir görünümde yalnızca tek bir döşeme olabilir. Geçerli döşemeyi görüntülemek veya alternatif bir döşeme seçmek için **Denetim** bölmesinde **Döşeme** sekmesini seçin. **Özellikler** bölmesi geçerli döşemenin özelliklerini görüntüler. 

Döşeme özelliklerini [Döşeme başvurusundaki](view-designer-tiles.md) bilgilere göre yapılandırabilir ve değişiklikleri kaydetmek için **Uygula'yı** tıklatabilirsiniz.

### <a name="configure-the-visualization-parts"></a>Görselleştirme parçalarını yapılandırma
Görünüm, herhangi bir sayıda görselleştirme parçası içerebilir. Görünüme parça eklemek için **Görünüm** sekmesini seçin ve ardından bir görselleştirme bölümü seçin. **Özellikler** bölmesi seçili parçanın özelliklerini görüntüler. 

Görünüm özelliklerini [Görselleştirme bölümü başvurusundaki](view-designer-parts.md) bilgilere göre yapılandırabilir ve değişiklikleri kaydetmek için **Uygula'yı** tıklatabilirsiniz.

Görünümlerin yalnızca bir sıra görselleştirme parçası vardır. Varolan parçaları yeni bir konuma sürükleyerek yeniden düzenleyebilirsiniz.

Parçanın sağ üst kısmındaki **X'i** seçerek görüntübir bölümü görünümden kaldırabilirsiniz.


### <a name="menu-options"></a>Menü seçenekleri
Görünümlerle edit modunda çalışma seçenekleri aşağıdaki tabloda açıklanmıştır.

![Menüyü edit](media/view-designer/edit-menu.png)

| Seçenek | Açıklama |
|:--|:--|
| Kaydet        | Değişikliklerinizi kaydeder ve görünümü kapatır. |
| İptal      | Değişikliklerinizi atar ve görünümü kapatır. |
| Görünümü Sil | Görünümü siler. |
| Dışarı Aktarma      | Görünümü, başka bir çalışma alanına aktarabileceğiniz bir [Azure Kaynak Yöneticisi şablonuna](../../azure-resource-manager/templates/template-syntax.md) dışa aktarın. Dosyanın adı görünümün adıdır ve *omsview* uzantısı vardır. |
| İçeri Aktarma      | Başka bir çalışma alanından dışa aktardığınız *omsview* dosyasını içeri aktarın. Bu eylem, varolan görünümün yapılandırmasını üzerine yazar. |
| Kopyalama       | Yeni bir görünüm oluşturur ve Görünüm Tasarımcısı'nda açar. Yeni görünümün adı özgün adla aynıdır, ancak *kopya* ona eklenen ile. |

## <a name="next-steps"></a>Sonraki adımlar
* Özel görünümünüze [Kutucuklar](view-designer-tiles.md) ekleyin.
* Özel görünümünüze [Görselleştirme parçaları](view-designer-parts.md) ekleyin.
