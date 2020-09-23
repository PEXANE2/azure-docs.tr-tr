---
title: Azure Arc özellikli veri hizmetleri nedir?
description: Azure Arc etkin veri hizmetlerini tanıtır
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 09/22/2020
ms.topic: overview
ms.openlocfilehash: 47fef490c5ece577823a14e3fa4c415f0f613ccb
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948141"
---
# <a name="what-are-azure-arc-enabled-data-services-preview"></a>Azure Arc etkin veri Hizmetleri (Önizleme) nedir?

Azure Arc, Azure veri Hizmetleri 'ni şirket içinde, kenarda ve Kubernetes ve tercih ettiğiniz altyapıyı kullanarak genel bulutlarda çalıştırmanızı olanaklı kılar.

Şu anda aşağıdaki Azure Arc etkin veri Hizmetleri önizlemede sunulmaktadır:

- SQL Yönetilen Örnek
- PostgreSQL hiper ölçek

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="always-current"></a>Her zaman geçerli

Azure Arc etkin SQL yönetilen örneği ve Azure Arc etkin PostgreSQL hiper ölçek gibi Azure Arc etkin veri Hizmetleri, bakım yamaları ve Azure 'daki deneyimle ilgili yeni özellikler dahil olmak üzere sık aralıklarla güncelleştirmeler alıyor. Microsoft Container Registry güncelleştirmeleri size sağlanır ve dağıtım, ilkelerinize uygun olarak sizin tarafınızdan ayarlanır. Bu şekilde, şirket içi veritabanları denetim bakımını sürdürirken güncel kalabilirler. Azure Arc etkin veri Hizmetleri bir abonelik hizmeti olduğundan, veritabanlarınız için artık destek dışı durumlara sahip olmayacaktır.

## <a name="elastic-scale"></a>Esnek ölçek

Şirket içi bulut benzeri esneklik sayesinde, altyapınızın kullanılabilir kapasitesine bağlı olarak, Azure 'da yaptığınız gibi, uygulamalarınızı dinamik bir şekilde yukarı veya aşağı ölçeklendirmenize olanak sağlar. Bu özellik, herhangi bir ölçekte, alt saniyelik yanıt süresi ile verilerin gerçek zamanlı olarak alınması ve sorgulanmasını gerektiren senaryolar da dahil olmak üzere geçici ihtiyaçları olan veri bloğu senaryolarını karşılar. Ayrıca, PostgreSQL için Azure veritabanı hiper ölçek 'in benzersiz Hyper ölçek dağıtım seçeneğini kullanarak veritabanı örneklerini genişletebilirsiniz. Bu özellik, veri iş yükleri için benzersiz*genişleme okuma ve* yazma işlemleri kullanarak kapasite iyileştirmesi için ek bir artırma sağlar.

## <a name="self-service-provisioning"></a>Self Servis sağlama

Azure Arc Ayrıca, hızlı dağıtım ve ölçek açısından otomasyon gibi diğer bulut avantajlarına da sahiptir. Kubernetes tabanlı düzenleme sayesinde, GUI veya CLı araçlarından birini kullanarak bir veritabanını Saniyeler içinde dağıtabilirsiniz.

## <a name="unified-management"></a>Birleşik yönetim

Azure portal, Azure Data Studio ve Azure veri CLı gibi tanıdık araçları kullanarak artık Azure Arc ile dağıtılan tüm veri varlıklarınızın Birleşik bir görünümünü elde edebilirsiniz. Yalnızca ortamınızda ve Azure 'da çeşitli ilişkisel veritabanlarını görüntüleyebiliyor ve yönetebileceksiniz, ayrıca temel altyapı kapasitesini ve sistem durumunu çözümlemek için Kubernetes API 'Lerinden Günlükler ve telemetri alabilirsiniz. Yerelleştirilmiş Log Analytics ve performans izlemenin yanı sıra, artık tüm emlak konusunda kapsamlı operasyonel içgörüler için Azure Izleyici 'den yararlanabilirsiniz.

## <a name="disconnected-scenario-support"></a>Bağlantısı kesik senaryo desteği

Self Servis sağlama, otomatik yedeklemeler/geri yükleme ve izleme gibi hizmetlerin çoğu, altyapınızda yerel olarak Azure bağlantısı olmadan veya doğrudan bağlantılı olarak çalışabilir. Azure 'a doğrudan bağlantı kurmak, Azure Izleyici gibi diğer Azure hizmetleriyle ve Azure Arc etkin veri hizmetlerinizi yönetmek için dünyanın herhangi bir yerinden Azure portal ve Azure Resource Manager API 'Leri kullanma olanağı için ek seçenekler açar.

## <a name="next-steps"></a>Sonraki adımlar

> **Yalnızca bir şeyi denemek mi istiyorsunuz?**  
> Azure Kubernetes Service (AKS), AWS elastik Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) veya bir Azure VM 'de [Azure Arc Jumpstart](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services) ile hızlıca çalışmaya başlayın.

[İstemci araçları 'nı yükler](install-client-tools.md)

[Azure Arc veri denetleyicisi oluşturma](create-data-controller.md) (önce istemci araçlarının yüklenmesini gerektirir)

Azure [Arc üzerinde Azure SQL yönetilen örneği oluşturma](create-sql-managed-instance.md) (önce Azure Arc veri denetleyicisinin oluşturulmasını gerektirir)

[Azure Arc 'Da PostgreSQL Için Azure veritabanı hiper ölçek sunucu grubu oluşturma](create-postgresql-hyperscale-server-group.md) (önce Azure Arc veri denetleyicisi oluşturulmasını gerektirir)
