---
title: SQL Insights ile uyarı oluşturma (Önizleme)
description: Azure Izleyici 'de SQL Insights ile uyarı oluşturma
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2021
ms.openlocfilehash: 5fe853ee0f7a113bfb8b0511744d9087f67927c4
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610160"
---
# <a name="create-alerts-with-sql-insights-preview"></a>SQL Insights ile uyarı oluşturma (Önizleme)
SQL Insights, genel SQL sorunları için [Azure izleyici 'de uyarı kuralları](../alert/../alerts/alerts-overview.md) oluşturmak için kullanabileceğiniz bir dizi uyarı kuralı şablonu içerir. SQL Insights 'daki uyarı kuralları, Azure Izleyici günlüklerindeki *ınsightsölçümlerini* tablosunda depolanan performans verilerine dayalı günlük uyarı kurallarıdır.  

> [!NOTE]
> Daha fazla SQL Insights uyarı kuralı şablonu için istekleriniz varsa, lütfen bu sayfanın en altındaki bağlantıyı veya Azure portal SQL Insights geri bildirim bağlantısını kullanarak geri bildirim gönderin.

## <a name="enable-alert-rules"></a>Uyarı kurallarını etkinleştir 
Azure portal Azure Izleyici 'deki uyarıları etkinleştirmek için aşağıdaki adımları kullanın.Oluşturulan uyarı kuralları, seçilen izleme profili altında izlenen tüm SQL kaynaklarının kapsamına alınır.  Bir uyarı kuralı tetiklendiğinde, belirli SQL örneğinde veya veritabanında tetiklenir.

> [!NOTE]
> Ayrıca, *ınsightsölçümlerini* tablosundaki veri kümelerinde sorgular çalıştırıp sonra bu sorguları bir uyarı kuralı olarak kaydederek özel [günlük uyarı kuralları](../alerts/alerts-log.md) da oluşturabilirsiniz. 

Azure portal Azure Izleyici menüsünün **Öngörüler** bölümünde **SQL (Önizleme)** seçeneğini belirleyin. **Uyarılar** 'a tıklayın

:::image type="content" source="media/sql-insights-alerts/alerts-button.png" alt-text="Uyarılar düğmesi":::

Sayfanın sağ tarafında **Uyarılar** bölmesi açılır. Varsayılan olarak, seçili izleme profilindeki SQL kaynakları için tetiklenen uyarıları, zaten oluşturduğunuz uyarı kurallarına göre görüntüler. Bir uyarı kuralı oluşturmak için kullanabileceğiniz kullanılabilir şablonların listesini görüntüleyen **Uyarı şablonları**' nı seçin.

:::image type="content" source="media/sql-insights-alerts/alert-templates.png" alt-text="Uyarı şablonları":::

**Uyarı kuralı oluştur** sayfasında, kural için varsayılan ayarları gözden geçirin ve bunları gerektiği gibi düzenleyin. Ayrıca, uyarı kuralı tetiklendiğinde bildirim ve eylem oluşturmak için bir [eylem grubu](../alerts/action-groups.md) seçebilirsiniz. Tüm özelliklerini doğruladıktan sonra uyarı kuralını oluşturmak için **Uyarı kuralını etkinleştir** ' e tıklayın.


:::image type="content" source="media/sql-insights-alerts/alert-rule.png" alt-text="Uyarı kuralları sayfası":::

Uyarı kuralını hemen dağıtmak için, **Uyarı kuralını dağıt**' a tıklayın. Kural şablonunu gerçekten dağıtılmadan önce görüntülemek istiyorsanız, **şablonu görüntüle** ' ye tıklayın.

:::image type="content" source="media/sql-insights-alerts/alert-rule-deploy.png" alt-text="Uyarı kuralını dağıt":::

Şablonları görüntülemeyi seçerseniz, uyarı kuralını oluşturmak için şablon sayfasında **Dağıt** ' ı seçin.

:::image type="content" source="media/sql-insights-alerts/view-template-deploy.png" alt-text="Görünüm şablonundan dağıt":::


## <a name="next-steps"></a>Sonraki adımlar

[Azure izleyici 'de uyarılar](../alerts/alerts-overview.md)hakkında daha fazla bilgi edinin.

