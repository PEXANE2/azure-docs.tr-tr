---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 04/20/2020
ms.author: cephalin
ms.openlocfilehash: c3fa57dd162fbbfbf0d46f73bffc78f279ef2968
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "83649144"
---
* **Kullanıcı düzeyi kimlik bilgileri**: tüm Azure hesabı için bir kimlik bilgileri kümesi. Azure hesabının erişim iznine sahip olduğu herhangi bir abonelikte her türlü uygulama için App Service dağıtmak üzere kullanılabilir. Bu, Portal GUI 'de (uygulamanın [kaynak sayfasının](../articles/azure-resource-manager/management/manage-resources-portal.md#manage-resources) **genel bakış** ve **özellikleri** gibi) ortaya çıkacak olan varsayılan bir kümesidir. Bir kullanıcıya Role-Based Access Control (RBAC) veya coadmin (izinleri aracılığıyla uygulama erişimi verildiğinde, bu kullanıcı, erişim iptal edilene kadar kendi Kullanıcı düzeyi kimlik bilgilerini kullanabilir. Bu kimlik bilgilerini diğer Azure kullanıcılarıyla paylaşmayın.

* **Uygulama düzeyi kimlik bilgileri**: her bir uygulama için bir kimlik bilgileri kümesi. Yalnızca bu uygulamaya dağıtmak için kullanılabilir. Her uygulama için kimlik bilgileri, uygulama oluşturma sırasında otomatik olarak oluşturulur. Bunlar el ile yapılandırılamaz, ancak her zaman sıfırlanabilir. Bir kullanıcıya (RBAC) aracılığıyla uygulama düzeyi kimlik bilgilerine erişim izni verilmesi için, bu kullanıcının uygulamada katkıda bulunması veya daha yüksek olması gerekir (Web sitesi katılımcısı yerleşik rolü dahil). Okuyucuların bu kimlik bilgilerine yayımlamasına ve bu kimlik bilgilerine erişemediğine izin verilmez.