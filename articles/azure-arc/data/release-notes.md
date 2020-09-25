---
title: Azure Arc etkin veri Hizmetleri-sürüm notları
description: En son sürüm notları
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 09/22/2020
ms.topic: conceptual
ms.openlocfilehash: d22976254cc804ca53060fb284abde8e80a684e6
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91319742"
---
# <a name="release-notes---azure-arc-enabled-data-services-preview"></a>Sürüm notları-Azure Arc etkin veri Hizmetleri (Önizleme)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="september-2020"></a>Eylül 2020

Azure Arc etkin veri Hizmetleri genel önizleme için kullanıma sunuldu. Yay etkin veri Hizmetleri, veri hizmetlerini her yerde yönetmenizi sağlar.

- SQL Yönetilen Örnek
- PostgreSQL hiper ölçek

Yönergeler için bkz. [Azure Arc etkin veri Hizmetleri nedir?](overview.md)

### <a name="known-issues"></a>Bilinen sorunlar

Bu sürüm için aşağıdaki sorunlar geçerlidir:

* **PostgreSQL hiper ölçek sunucu grubu siliniyor**: sunucu grubunuzun veya örneğinizin yapılandırmasını değiştirdiyseniz, bir PostgreSQL hiper ölçek sunucu grubunu silmeden önce düzenleme işleminin tamamlanmasını bekleyin.

* ** `azdata notebook run` başarısız olabilir**: Bu sorunu yuvarlayıp `azdata notebook run` bir Python sanal ortamında çalıştırın. Bu sorun ayrıca, Azure Data Studio Dağıtım Sihirbazı 'nı kullanarak bir SQL yönetilen örneği veya PostgreSQL hiper ölçek sunucu grubu oluşturma girişimi için de bildirim sağlar. Bu durumda, Not defterini açabilir ve Not defterinin en üstündeki **Tümünü Çalıştır** düğmesine tıklayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> **Yalnızca bir şeyi denemek mi istiyorsunuz?**  
> Azure Kubernetes Service (AKS), AWS elastik Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) veya bir Azure VM 'de [Azure Arc Jumpstart](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services) ile hızlıca çalışmaya başlayın.

[İstemci araçları 'nı yükler](install-client-tools.md)

[Azure Arc veri denetleyicisi oluşturma](create-data-controller.md) (önce istemci araçlarının yüklenmesini gerektirir)

Azure [Arc üzerinde Azure SQL yönetilen örneği oluşturma](create-sql-managed-instance.md) (önce Azure Arc veri denetleyicisinin oluşturulmasını gerektirir)

[Azure Arc 'Da PostgreSQL Için Azure veritabanı hiper ölçek sunucu grubu oluşturma](create-postgresql-hyperscale-server-group.md) (önce Azure Arc veri denetleyicisi oluşturulmasını gerektirir)

## <a name="known-limitations-and-issues"></a>Bilinen sınırlamalar ve sorunlar

- SQL yönetilen örnek adları en fazla 13 karakter uzunluğunda olabilir
- Azure Arc veri denetleyicisi veya veritabanı örnekleri için yerinde yükseltme yok.
- Yay etkin veri Hizmetleri kapsayıcı görüntüleri imzalanmadı.  Kubernetes düğümlerinizi, imzasız kapsayıcı görüntülerinin çekilme izni verecek şekilde yapılandırmanız gerekebilir.  Örneğin, kapsayıcı çalışma zamanı olarak Docker kullanıyorsanız, DOCKER_CONTENT_TRUST = 0 ortam değişkenini ayarlayabilir ve yeniden başlatabilirsiniz.  Diğer kapsayıcı çalışma zamanları, [OpenShift](https://docs.openshift.com/container-platform/4.5/openshift_images/image-configuration.html#images-configuration-file_image-configuration)içindeki gibi benzer seçeneklere sahiptir.
- Azure portal Azure Arc etkin SQL yönetilen örnekleri veya PostgreSQL hiper ölçek sunucu grupları oluşturulamıyor.
- Şimdilik, NFS kullanıyorsanız, Azure Arc veri denetleyicisini oluşturmadan önce, dağıtım profili dosyanızda allowRunAsRoot değerini true olarak ayarlamanız gerekir.
- Yalnızca SQL ve PostgreSQL oturum açma kimlik doğrulaması.  Azure Active Directory veya Active Directory desteği yok.
- Openshıft üzerinde bir veri denetleyicisi oluşturmak için gevşek güvenlik kısıtlamaları gerekir.  Ayrıntılar için belgelere bakın.
- Postgres hiper ölçek çalışan düğümlerinin sayısını _azaltma_ desteklenmiyor.
- Azure Arc veri denetleyicisi ve veritabanı örnekleri ile Azure Stack hub 'ında Azure Kubernetes hizmet altyapısı 'nı (AKS motoru) kullanıyorsanız, daha yeni bir Kubernetes sürümüne yükseltme desteklenmez. Kubernetes kümesini yükseltmeden önce Azure Arc veri denetleyicisi 'ni ve tüm veritabanı örneklerini kaldırın.
- Önizleme, Postgres sürüm 11 altyapısı için yedekleme/geri yükleme özelliğini desteklemez. Yalnızca Postgres sürüm 12 için yedekleme/geri yüklemeyi destekler.
