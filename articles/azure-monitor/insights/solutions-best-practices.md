---
title: Azure 'da en iyi deneyimler için yönetim çözümü
description: Bu makalede, bir yönetim çözümü dosyası oluşturmaya yönelik ipuçları hakkında bilgi edinin. Bkz. veri kaynakları, runbook 'lar, görünümler ve uyarılarla çalışma.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/27/2017
ms.openlocfilehash: 7cb300297336edcce4294b800520ad570b12bcde
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80548158"
---
# <a name="best-practices-for-creating-management-solutions-in-azure-preview"></a>Azure 'da yönetim çözümleri oluşturmak için en iyi uygulamalar (Önizleme)
> [!NOTE]
> Bu, şu anda önizleme aşamasında olan Azure 'da yönetim çözümleri oluşturmaya yönelik bir belgedir. Aşağıda açıklanan tüm şemalarla değişiklik yapılır.  

Bu makalede, Azure 'da [bir yönetim çözümü dosyası oluşturmak](solutions-solution-file.md) için en iyi yöntemler sunulmaktadır.  Bu bilgiler, diğer en iyi uygulamalar tanımlandıkları için güncelleştirilir.

## <a name="data-sources"></a>Veri kaynakları
- Veri kaynakları [bir kaynak yöneticisi şablonuyla yapılandırılabilir](../../azure-monitor/platform/template-workspace-configuration.md), ancak bir çözüm dosyasına dahil edilmemelidir.  Bunun nedeni, veri kaynaklarının yapılandırılmasının Şu anda ıdempotent anlamına gelir. Bu, çözümünüzün kullanıcının çalışma alanındaki mevcut yapılandırmanın üzerine yazılmasına neden olabilir.<br><br>Örneğin, çözümünüz uygulama olay günlüğünden uyarı ve hata olayları gerektirebilir.  Bunu çözümünüzde bir veri kaynağı olarak belirtirseniz, Kullanıcı çalışma alanında bu yapılandırıldıysa, bilgi olaylarını kaldırma riskiyle karşılaşırsınız.  Tüm olayları eklediyseniz, kullanıcının çalışma alanında aşırı bilgi olayları toplanmaktadır.

- Çözümünüz standart veri kaynaklarından birindeki verileri gerektiriyorsa, bunu bir önkoşul olarak tanımlamanız gerekir.  Müşterinin veri kaynağını kendi kendine yapılandırması gereken durum.  
- Kullanıcılara, gerekli verilerin toplanması için yapılandırılması gereken veri kaynaklarını bildirmek üzere çözümünüzdeki görünümlere bir [veri akışı doğrulama](../../azure-monitor/platform/view-designer-tiles.md) iletisi ekleyin.  Bu ileti, gerekli veriler bulunamadığında görünümün kutucuğunda görüntülenir.


## <a name="runbooks"></a>Runbook'lar
- Çözümünüzde bir zamanlamaya göre çalışması gereken her runbook için bir [Otomasyon zamanlaması](../../automation/automation-schedules.md) ekleyin.
- Log Analytics deposuna veri yazan runbook 'lar tarafından kullanılmak üzere çözümünüze [ınestionapi modülünü](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) ekleyin.  Çözümü, çözüm kaldırılırsa kalacak şekilde bu kaynağa [başvuracak](solutions-solution-file.md#solution-resource) şekilde yapılandırın.  Bu, modülün paylaşılması için birden çok çözümün kullanılmasına izin verir.
- Kullanıcıların daha sonra değiştirmek isteyebileceğiniz çözüme değerler sağlamak için [Otomasyon değişkenlerini](../../automation/automation-schedules.md) kullanın.  Çözüm değişkeni içerecek şekilde yapılandırılmış olsa bile, bu değer yine de değiştirilebilir.

## <a name="views"></a>Görünümler
- Tüm çözümlerin, kullanıcının portalında görüntülenen tek bir görünümü içermesi gerekir.  Görünümü, farklı veri kümelerini göstermek için birden çok [görselleştirme bölümü](../../azure-monitor/platform/view-designer-parts.md) içerebilir.
- Kullanıcılara, gerekli verilerin toplanması için yapılandırılması gereken veri kaynaklarını bildirmek üzere çözümünüzdeki görünümlere bir [veri akışı doğrulama](../../azure-monitor/platform/view-designer-tiles.md) iletisi ekleyin.
- Çözümü, çözüm kaldırılırsa kaldırılacak şekilde, görünümü [içerecek](solutions-solution-file.md#solution-resource) şekilde yapılandırın.

## <a name="alerts"></a>Uyarılar
- Alıcı listesini çözüm dosyasında bir parametre olarak tanımlayın, böylece kullanıcı çözümü yüklerken bunları tanımlayabilirler.
- Kullanıcının yapılandırmalarını değiştirebilmeleri için uyarıları uyarı kurallarına [başvuracak](solutions-solution-file.md#solution-resource) şekilde yapılandırın.  Alıcı listesini değiştirme, uyarının eşiğini değiştirme veya uyarı kuralını devre dışı bırakma gibi değişiklikler yapmak isteyebilir. 


## <a name="next-steps"></a>Sonraki adımlar
* Temel [bir yönetim çözümü tasarlama ve oluşturma](solutions-creating.md)sürecini adım adım inceleyin.
* [Çözüm dosyası oluşturmayı](solutions-solution-file.md)öğrenin.
* [Kayıtlı aramaları ve uyarıları](solutions-resources-searches-alerts.md) yönetim çözümünüze ekleyin.
* Yönetim çözümünüze [Görünümler ekleyin](solutions-resources-views.md) .
* Yönetim çözümünüze [Otomasyon Runbook 'larını ve diğer kaynakları ekleyin](solutions-resources-automation.md) .

