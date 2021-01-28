---
title: Azure Arc etkin PostgreSQL Hyperscale sınırlamaları
description: Azure Arc etkin PostgreSQL Hyperscale sınırlamaları
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 01/21/2021
ms.topic: how-to
ms.openlocfilehash: ecf3c1f0d553c6521178fa5e1f67fc1cf95de73e
ms.sourcegitcommit: 04297f0706b200af15d6d97bc6fc47788785950f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98987076"
---
# <a name="limitations-of-azure-arc-enabled-postgresql-hyperscale"></a>Azure Arc etkin PostgreSQL Hyperscale sınırlamaları

Bu makalede, Azure Arc etkin PostgreSQL hiper ölçeklendirmeye yönelik sınırlamalar açıklanmaktadır. 

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

- Aynı sunucu grubuna belirli bir noktaya geri yükleme (belirli bir tarih ve saate geri yükleme gibi) desteklenmez. Zaman içinde bir noktaya geri yükleme yaparken, ' yi geri yüklemeden önce dağıttığınız farklı bir sunucu grubuna geri yüklemeniz gerekir. Yeni sunucu grubuna geri yükledikten sonra, kaynak sunucu grubunu silebilirsiniz.
- Bir yedeğin tüm içeriğini geri yükleme (zaman içinde belirli bir noktaya geri yükleme yerine), PostgreSQL sürüm 12 için desteklenir. Zaman çizelgeleriyle PostgreSQL altyapısının sınırlamasından dolayı PostgreSQL sürüm 11 ' de desteklenmez. Bir dağıtım içeriğinin tamamını bir PostgreSQL sunucu grubu 11 sürümüne geri yüklemek için, farklı bir sunucu grubuna geri yüklemeniz gerekir.


## <a name="databases"></a>Veritabanları

Bir sunucu grubunda birden fazla veritabanının barındırılması desteklenmez.


## <a name="security"></a>Güvenlik

Kullanıcıları ve rolleri yönetme desteklenmez. Şimdilik, Postgres standart kullanıcısını kullanmaya devam edin.

## <a name="roles-and-responsibilities"></a>Roller ve sorumluluklar

Microsoft ve müşterileri arasındaki roller ve sorumluluklar, Azure PaaS hizmetleri (hizmet olarak platform) ve Azure hibrit (Azure Arc etkin PostgreSQL hiper ölçeği gibi) arasında farklılık gösterir. 

### <a name="frequently-asked-questions"></a>Sık sorulan sorular

Aşağıdaki tabloda, destek rolleri ve sorumlulukları ile ilgili sık sorulan soruların yanıtları özetlenmektedir.

| Soru                      | Hizmet olarak Azure platformu (PaaS) | Azure yay karma Hizmetleri |
|:----------------------------------|:------------------------------------:|:---------------------------:|
| Altyapıyı kim sağlıyor?  | Microsoft                          | Müşteri                  |
| Yazılımı kim sağlıyor? *       | Microsoft                          | Microsoft                 |
| İşlemler kim? | Microsoft                          | Müşteri                  |
| Microsoft SLA 'Lar sağlıyor mu?      | Yes                                | Hayır                        |
| SLA 'ların ücreti nedir? | Microsoft                          | Müşteri                  |

\* Azure hizmetleri

__Microsoft, Azure Arc karma Hizmetleri 'nde SLA 'Ları neden sağlamıyor?__ Microsoft bu altyapıyı sahibi olmadığından ve uygulamayı işlemediği için. Müşteriler.

## <a name="next-steps"></a>Sonraki adımlar

- **Deneyin.** Azure Kubernetes Service (AKS), AWS elastik Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) veya bir Azure VM 'de [Azure Arc Jumpstart](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services) ile hızlıca çalışmaya başlayın. 

- **Kendinizinkini oluşturun.** Kendi Kubernetes kümenizde oluşturmak için aşağıdaki adımları izleyin: 
   1. [İstemci araçları 'nı yükler](install-client-tools.md)
   2. [Azure Arc veri denetleyicisi oluşturma](create-data-controller.md)
   3. [Azure Arc 'da PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu oluşturma](create-postgresql-hyperscale-server-group.md) 

- **Learn**
   - [Azure Arc etkin veri hizmetleri hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/azure-arc/hybrid-data-services)
   - [Azure Arc hakkında bilgi edinin](https://aka.ms/azurearc)
