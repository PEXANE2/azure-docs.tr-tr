---
title: Azure Güvenlik Merkezi 'nde çalışma kitapları Galerisi
description: Tümleşik Azure Izleyici çalışma kitapları Galerisi ile Azure Güvenlik Merkezi verileriniz için zengin ve etkileşimli raporlar oluşturmayı öğrenin
author: memildin
ms.author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/04/2021
ms.openlocfilehash: 198702f619e490e8000e4430aab23a7f6bfb6d85
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102107981"
---
# <a name="create-rich-interactive-reports-of-security-center-data"></a>Zengin ve etkileşimli güvenlik merkezi verileri raporları oluşturun

[Azure Izleyici çalışma kitapları](../azure-monitor/visualize/workbooks-overview.md) , veri analizi için esnek bir tuval ve Azure Portal içinde zengin görsel raporların oluşturulmasını sağlar. Azure 'da birden çok veri kaynağına dokunmanıza ve bunları Birleşik etkileşimli deneyimler halinde birleştirmeye olanak tanır.

Çalışma kitapları, Azure verilerinizi görselleştirmeye yönelik zengin bir özellik kümesi sağlar. Her görselleştirme türünün ayrıntılı örnekleri için bkz. [görselleştirmeler örnekleri ve belgeleri](../azure-monitor/visualize/workbooks-text-visualizations.md). 

Azure Güvenlik Merkezi 'nde, kuruluşunuzun güvenlik duruşunu izlemek için yerleşik raporlara erişebilirsiniz. Ayrıca, güvenlik merkezi 'nden veya desteklenen diğer veri kaynaklarından çok çeşitli verileri görüntülemek için özel raporlar oluşturabilirsiniz.

:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-snip.png" alt-text="Zaman içindeki güvenli puan raporu":::

## <a name="availability"></a>Kullanılabilirlik

| Görünüş                          | Ayrıntılar                                                                                                                                      |
|---------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------|
| Yayın durumu:                  | Önizleme<br>[!INCLUDE [Legalese](../../includes/security-center-preview-legal-text.md)]                                                       |
| Fiyat                        | Ücretsiz                                                                                                                                         |
| Gerekli roller ve izinler: | Çalışma kitaplarını kaydetmek için, hedef kaynak grubunda en az bir çalışma kitabı katılımcısı izninizin olması gerekir                                      |
| Larının                         | ![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov) |
|                                 |                                                                                                                                              |

## <a name="workbooks-gallery-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde çalışma kitapları Galerisi

Tümleşik Azure çalışma kitapları işlevselliğiyle Azure Güvenlik Merkezi, kendi özel ve etkileşimli raporlarınızı oluşturmayı basit hale getirir. Güvenlik Merkezi Ayrıca, özelleştirme için şu raporların hazırlanacağı bir çalışma kitabı Galerisi içerir:

- **Zaman Içinde güvenli puan** -aboneliklerinizin puanlarını ve kaynaklarınızın önerilerini izleyin
- **Sistem güncelleştirmeleri** -kaynaklar, işletim sistemi, önem derecesi ve daha fazlası için eksik sistem güncelleştirmelerini görüntüleyin
- **Güvenlik açığı değerlendirmesi bulguları** -Azure kaynaklarınızın güvenlik açığı taramalarının bulgularını görüntüleyin

:::image type="content" source="media/custom-dashboards-azure-workbooks/workbooks-gallery-security-center.png" alt-text="Azure Güvenlik Merkezi 'nde yerleşik çalışma kitapları Galerisi":::

Sağlanan raporlardan birini seçin veya kendi raporlarınızı oluşturun.

> [!TIP]
> Sağlanan raporlardan herhangi birini memnuniyet için özelleştirmek için **Düzenle** düğmesini kullanın. Düzenlemeden sonra **Kaydet** ' i seçtiğinizde değişiklikler yeni bir çalışma kitabına kaydedilir.
> 
> :::image type="content" source="media/custom-dashboards-azure-workbooks/editing-supplied-workbooks.png" alt-text="Sağlanan çalışma kitaplarını belirli gereksinimleriniz için özelleştirmek üzere Düzenle":::

### <a name="use-the-secure-score-over-time-report"></a>' Zamana göre güvenli puan ' raporunu kullanın

Bu rapor, Log Analytics çalışma alanınızdan güvenli puan verilerini kullanır. Bu verilerin sürekli dışa aktarma aracından dışarı aktarılması gerekir [Azure Portal Içindeki Güvenlik Merkezi sayfalarından sürekli dışarı aktarmayı yapılandırma](continuous-export.md?tabs=azure-portal)başlığı altında açıklandığı gibi.

Sürekli dışarı aktarmayı ayarlarken, dışa aktarma sıklığını hem **akış güncelleştirmelerine** hem de **anlık görüntülere** ayarlayın.

:::image type="content" source="media/custom-dashboards-azure-workbooks/export-frequency-both.png" alt-text="Zaman içindeki güvenli puan çalışma kitabında, sürekli dışa aktarma yapılandırmanızda dışarı aktarma sıklığı ayarlarından bu seçeneklerin her ikisini de seçmeniz gerekir":::

> [!NOTE]
> Anlık görüntüler haftalık olarak verildiğinde, bu rapordaki verileri görüntüleyebilmeniz için ilk anlık görüntünün aktarılması için en az bir hafta beklemeniz gerekir.

> [!TIP]
> Kuruluşunuz genelinde sürekli dışarı aktarmayı yapılandırmak için, [ölçeklendirerek sürekli dışarı aktarmayı yapılandırma](continuous-export.md?tabs=azure-policy)bölümünde açıklanan sağlanan Azure Ilkesi ' DeployIfNotExist ' ilkelerini kullanın.

Zaman içinde güvenli puan raporunda, abonelikler için seçili çalışma alanlarına raporlama yapmak için beş grafik bulunur:


|Graf  |Örnek  |
|---------|---------|
|**Geçen hafta ve aya ilişkin eğilimleri puan edin**<br>Abonelikleriniz için puanları geçerli puanı ve genel eğilimlerini izlemek için bu bölümü kullanın.|:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-table-1.png" alt-text="Yerleşik rapordaki güvenli Puanlama için eğilimler":::|
|**Tüm seçili abonelikler için toplu puan**<br>Seçili zaman aralığındaki herhangi bir tarihte toplanan puanı görmek için farenizi eğilim çizgisinin herhangi bir noktasına getirin.|:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-table-2.png" alt-text="Tüm seçili abonelikler için toplu puan":::|
|**En iyi durumda olmayan kaynaklarla öneriler**<br>Bu tablo, seçilen dönemde en fazla kaynağa sağlıksız olarak değiştirilen önerileri önceliklendirmenize yardımcı olur.|:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-table-3.png" alt-text="En iyi durumda olmayan kaynaklarla öneriler":::|
|**Belirli güvenlik denetimleri için puanlar**<br>Güvenlik Merkezi 'nin güvenlik denetimleri, önerilerin mantıksal gruplandırmalarıdır. Bu grafik, tüm denetimlerinizin haftalık puanlarını bir bakışta gösterir.|:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-table-4.png" alt-text="Seçilen süre boyunca güvenlik denetimlerinizin puanları":::|
|**Kaynak değişiklikleri**<br>Seçilen dönemde durumu değişmiş olan (sağlıklı, sağlıksız veya geçerli değil) en çok kaynakla ilgili öneriler burada listelenir. Belirli kaynakları listeleyerek yeni bir tablo açmak için listeden herhangi bir önerisi seçin.|:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-table-5.png" alt-text="Sistem durumu değişmiş olan en fazla kaynakla öneriler":::|

### <a name="use-the-system-updates-report"></a>' Sistem güncelleştirmeleri ' raporunu kullanın

Bu rapor, "sistem güncelleştirmelerinin makinelerinizde yüklü olması gerekir" güvenlik önerisini temel alır.

Rapor, bekleyen güncelleştirmeleri olan makineleri belirlemenize yardımcı olur.

Seçili abonelikler için durumu şunlara göre görüntüleyebilirsiniz:

- Bekleyen güncelleştirmeleri olan kaynakların listesi
- Kaynaklarınızda eksik olan güncelleştirmelerin listesi

:::image type="content" source="media/custom-dashboards-azure-workbooks/system-updates-report.png" alt-text="Güvenlik Merkezi 'nin sistem güncelleştirmeleri raporu, eksik güncelleştirmeler güvenlik önerisi temelinde":::

### <a name="use-the-vulnerability-assessment-findings-report"></a>' Güvenlik açığı değerlendirmesi bulguları ' raporunu kullanın

Güvenlik Merkezi, makineleriniz, kapsayıcı kayıt defterlerinde kapsayıcılar ve SQL Server 'lar için güvenlik açığı tarayıcıları içerir.

Bu tarayıcıları kullanma hakkında daha fazla bilgi edinin:

- [Tümleşik VA tarayıcısıyla makinelerinizi tarayın](deploy-vulnerability-assessment-vm.md)
- [Kayıt defteri görüntülerinizi güvenlik açıkları için tarama](defender-for-container-registries-usage.md)
- [SQL kaynaklarınızı güvenlik açıklarına karşı tarayın](defender-for-sql-on-machines-vulnerability-assessment.md)

Bu tarayıcıların her biri için bulguları ayrı önerilerden raporlanır:

- Sanal makinelerinizdeki güvenlik açıkları düzeltilmelidir
- Azure Container Registry görüntülerdeki güvenlik açıkları düzeltilmelidir (Qualys tarafından desteklenir)
- SQL veritabanlarındaki güvenlik açığı değerlendirmesi bulguları düzeltildi
- Makinelerdeki SQL sunucularınızda güvenlik açığı değerlendirmesi bulguları düzeltildi

Bu rapor, bu bulguları toplar ve bunları önem derecesi, kaynak türü ve kategoriye göre düzenler.

:::image type="content" source="media/custom-dashboards-azure-workbooks/vulnerability-assessment-findings-report.png" alt-text="Güvenlik Merkezi 'nin güvenlik açığı değerlendirme bulguları raporu":::


## <a name="import-workbooks-from-other-workbook-galleries"></a>Diğer çalışma kitabı galerilerinden çalışma kitaplarını içeri aktarma

Diğer Azure hizmetlerinde çalışma kitapları oluşturduysanız ve bunları Azure Güvenlik Merkezi çalışma kitapları galerinize taşımak istiyorsanız:

1. Hedef çalışma kitabını açın.

1. Araç çubuğundan **Düzenle**' yi seçin.

    :::image type="content" source="media/custom-dashboards-azure-workbooks/editing-workbooks.png" alt-text="Azure Izleyici çalışma kitabını Düzenle":::

1. Araç çubuğundan **</>** Gelişmiş Düzenleyici girmek için seçin.

    :::image type="content" source="media/custom-dashboards-azure-workbooks/editing-workbooks-advanced-editor.png" alt-text="Galeri şablonu JSON kodunu almak için Gelişmiş Düzenleyici başlatılıyor":::

1. Çalışma kitabının Galeri şablonunu JSON olarak kopyalayın.

1. Çalışma kitabı galerisini Güvenlik Merkezi 'nde açın ve menü çubuğundan **Yeni**' yi seçin.
1. **</>** Gelişmiş Düzenleyici girmek için öğesini seçin.
1. Tüm Galeri şablonu JSON öğesine yapıştırın.
1. **Uygula**’yı seçin.
1. Araç çubuğundan **farklı kaydet**' i seçin.

    :::image type="content" source="media/custom-dashboards-azure-workbooks/editing-workbooks-save-as.png" alt-text="Çalışma kitabını Güvenlik Merkezi 'ndeki galeriye kaydetme":::

1. Çalışma kitabını kaydetmek için gerekli ayrıntıları girin:
   1. Çalışma kitabı için bir ad
   2. İstenen bölge
   3. Abonelik, kaynak grubu ve uygun şekilde paylaşma.

Kaydedilen çalışma kitabınızı **son değiştirilen çalışma kitapları** kategorisinde bulacaksınız.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, yerleşik raporlar ve kendi özel ve etkileşimli raporlarınızı oluşturma seçeneği bulunan güvenlik merkezi 'nin tümleşik Azure Izleyici çalışma kitapları sayfası açıklanmıştır.

- [Azure Izleyici çalışma kitapları](../azure-monitor/visualize/workbooks-overview.md) hakkında daha fazla bilgi edinin
- Yerleşik raporlar, verilerini Güvenlik Merkezi 'nin önerilerinden çeker. [Güvenlik önerileri-bir başvuru kılavuzu](recommendations-reference.md) ' nda birçok güvenlik önerisi hakkında bilgi edinin