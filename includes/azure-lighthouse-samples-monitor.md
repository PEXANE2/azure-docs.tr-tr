---
title: include dosyası
description: include dosyası
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 03/30/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: aacf1c287dca01b5993d35b311eed22e88ef7e87
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421153"
---
Bu örnekler Azure İzleyici'nin, Azure tarafından atanan temsilcinin kaynak yönetimi için eklenen aboneliklerde uyarı oluşturmak amacıyla nasıl kullanılacağını gösterir.

| **Şablon** | **Açıklama** |
|---------|---------|
| [monitor-delegation-changes](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/tools/monitor-delegation-changes) | Yönetici kiracısında 1 günden fazla süredir etkin olmayan sorgular ve [eklenen veya kaldırılan temsilcilerdeki raporlar](../articles/lighthouse/how-to/monitor-delegation-changes.md) (veya başarılı olmayan denemeler).|
| [alert-using-actiongroup](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/alert-using-actiongroup) | Bu şablon bir Azure uyarısı oluşturur ve mevcut Eylem Grubuna bağlanır.|
| [multiple-loganalytics-alerts](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/multiple-loganalytics-alerts) | Kusto sorgularına dayalı birden fazla Log Analytics uyarısı oluşturur.|
| [delegation-alert-for-customer](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegation-alert-for-customer) | Kullanıcı, yöneten kiracıya bir aboneliği temsilci olarak atadığında kiracıda uyarı dağıtır.|
