---
title: include dosyası
description: include dosyası
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 12/11/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 0056e18b6cb3aad2a4504bbe20b3b3421793489e
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97356282"
---
Bu örnekler Azure İzleyici'nin, Azure tarafından atanan temsilcinin kaynak yönetimi için eklenen aboneliklerde uyarı oluşturmak amacıyla nasıl kullanılacağını gösterir.

| **Şablon** | **Açıklama** |
|---------|---------|
| [monitor-delegation-changes](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/monitor-delegation-changes) | Yönetici kiracısında 1 günden fazla süredir etkin olmayan sorgular ve [eklenen veya kaldırılan temsilcilerdeki raporlar](../articles/lighthouse/how-to/monitor-delegation-changes.md) (veya başarılı olmayan denemeler).|
| [alert-using-actiongroup](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/alert-using-actiongroup) | Bu şablon bir Azure uyarısı oluşturur ve mevcut Eylem Grubuna bağlanır.|
| [multiple-loganalytics-alerts](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/multiple-loganalytics-alerts) | Kusto sorgularına dayalı birden fazla Log Analytics uyarısı oluşturur.|
| [delegation-alert-for-customer](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/delegation-alert-for-customer) | Kullanıcı, yöneten kiracıya bir aboneliği temsilci olarak atadığında kiracıda uyarı dağıtır.|
| [workbook-activitylogs-by-domain](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/workbook-activitylogs-by-domain) | Abonelikler genelindeki Azure Etkinlik günlüklerini etki alanı adına göre filtreleme seçeneğiyle birlikte görüntüler. |
