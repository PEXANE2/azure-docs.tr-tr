---
title: Azure en iyi uygulamalarında yönetim çözümü | Microsoft Dokümanlar
description: ''
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/27/2017
ms.openlocfilehash: 837fb87d73698961ec1550b122840563d1707f8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663207"
---
# <a name="best-practices-for-creating-management-solutions-in-azure-preview"></a>Azure'da yönetim çözümleri oluşturmak için en iyi uygulamalar (Önizleme)
> [!NOTE]
> Bu, Şu anda önizlemede olan Azure'da yönetim çözümleri oluşturmak için ön belgelerdir. Aşağıda açıklanan herhangi bir şema değişebilir.  

Bu makalede, Azure'da [bir yönetim çözümü dosyası oluşturmak](solutions-solution-file.md) için en iyi uygulamalar sağlanmaktadır.  Ek en iyi uygulamalar tanımlandıkça bu bilgiler güncelleştirilir.

## <a name="data-sources"></a>Veri kaynakları
- Veri kaynakları [Kaynak Yöneticisi şablonuyla yapılandırılabilir,](../../azure-monitor/platform/template-workspace-configuration.md)ancak çözüm dosyasına dahil edilmemelidir.  Bunun nedeni, veri kaynaklarını yapılandırmanın şu anda idempotent olmamasıdır, bu da çözümünüzün kullanıcının çalışma alanında varolan yapılandırmanın üzerine yazabileceği anlamına gelir.<br><br>Örneğin, çözümünüz Uygulama olay günlüğünden Uyarı ve Hata olayları gerektirebilir.  Bunu çözümünüzde bir veri kaynağı olarak belirtirseniz, kullanıcı bu yapılandırılan çalışma alanında varsa Bilgi olaylarını kaldırma riskine dersiniz.  Tüm olayları dahil ettiyseniz, kullanıcının çalışma alanında aşırı Bilgi olayları topluyor olabilirsiniz.

- Çözümünüz standart veri kaynaklarından birinden veri gerektiriyorsa, bunu bir ön koşul olarak tanımlamanız gerekir.  Müşterinin veri kaynağını kendi başına yapılandırması gerektiğini belgelerde belirtin.  
- Gerekli verilerin toplanması için yapılandırılması gereken veri kaynakları konusunda kullanıcıya bilgi vermek için çözümünüzdeki tüm görünümlere veri [akışı doğrulama](../../azure-monitor/platform/view-designer-tiles.md) iletisi ekleyin.  Bu ileti, gerekli veriler bulunamadığında görünümün döşemesinde görüntülenir.


## <a name="runbooks"></a>Runbook'lar
- Çözümünüzdeki her runbook için zamanlamaya göre çalışması gereken bir [Otomasyon çizelgesi](../../automation/automation-schedules.md) ekleyin.
- Log Analytics deposuna veri yazan runbooks tarafından kullanılmak üzere çözümünüze [IngestionAPI modüllerini](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) ekleyin.  Çözüm kaldırılırsa kalacak şekilde bu kaynağa [başvurmak](solutions-solution-file.md#solution-resource) için çözümü yapılandırın.  Bu, modülü paylaşmak için birden çok çözüm sağlar.
- Kullanıcıların daha sonra değiştirmek isteyebileceği çözüme değerler sağlamak için [Otomasyon değişkenlerini](../../automation/automation-schedules.md) kullanın.  Çözüm değişkeni içerecek şekilde yapılandırılsa bile, değeri yine de değiştirilebilir.

## <a name="views"></a>Görünümler
- Tüm çözümler, kullanıcının portalında görüntülenen tek bir görünüm içermelidir.  Görünüm, farklı veri kümelerini göstermek için birden çok [görselleştirme parçası](../../azure-monitor/platform/view-designer-parts.md) içerebilir.
- Gerekli verilerin toplanması için yapılandırılması gereken veri kaynakları konusunda kullanıcıya bilgi vermek için çözümünüzdeki tüm görünümlere veri [akışı doğrulama](../../azure-monitor/platform/view-designer-tiles.md) iletisi ekleyin.
- Çözüm kaldırılırsa kaldırılacak şekilde görünümü [içerecek](solutions-solution-file.md#solution-resource) şekilde çözüm yapılandırır.

## <a name="alerts"></a>Uyarılar
- Kullanıcının çözümü yüklerken bunları tanımlayabilmesi için alıcılar listesini çözüm dosyasında bir parametre olarak tanımlayın.
- Kullanıcının yapılandırmasını değiştirebilmeleri için çözümü [başvuru](solutions-solution-file.md#solution-resource) uyarı kurallarına göre yapılandırın.  Alıcı listesini değiştirme, uyarı eşiğini değiştirme veya uyarı kuralını devre dışı bırakma gibi değişiklikler yapmak isteyebilirler. 


## <a name="next-steps"></a>Sonraki adımlar
* Tasarım ve bir [yönetim çözümü bina](solutions-creating.md)temel süreci ile yürüyün.
* [Çözüm dosyası oluşturmayı](solutions-solution-file.md)öğrenin.
* Yönetim çözümünüze [kaydedilmiş aramaları ve uyarıları ekleyin.](solutions-resources-searches-alerts.md)
* Yönetim çözümünüze [görünümler ekleyin.](solutions-resources-views.md)
* Yönetim çözümünüze [Otomasyon runbook'larını ve diğer kaynakları ekleyin.](solutions-resources-automation.md)

