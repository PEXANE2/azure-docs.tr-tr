---
title: Azure Arc etkin veri Hizmetleri-bilinen sorunlar
description: Bilinen en son sorunlar
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 03/02/2021
ms.topic: conceptual
ms.openlocfilehash: d4667e8fa3a5624dddc3cb0dd792fc73ea812332
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101693230"
---
# <a name="known-issues---azure-arc-enabled-data-services-preview"></a>Bilinen sorunlar-Azure Arc etkin veri Hizmetleri (Önizleme)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="february-2021"></a>Şubat 2021


- Bağlı küme modu devre dışı
- Azure Arc etkin PostgreSQL hiper ölçek, belirttiğiniz zaman göreli noktaya geri yükleme yaparken yanlış bir hata iletisi döndürüyor. Örneğin, geri yükleme için yedeklemelerinizin sahip olduğu sürümden daha eski bir nokta belirttiyseniz, geri yükleme şu şekilde bir hata iletisiyle başarısız olur: `ERROR: (404). Reason: Not found. HTTP response body: {"code":404, "internalStatus":"NOT_FOUND", "reason":"Failed to restore backup for server...}` . Bu durumda, yedeklemeleriniz olan tarih aralığı içinde bir zaman noktası olduğunu belirterek komutu yeniden başlatın. Bu aralığı öğrenmek için, yedeklemelerinizi listeleyin ve alındığı tarihlere bakın.
- Tam geri yükleme yapılırken bir yedekleme kimliği gereklidir. Varsayılan olarak, bir yedekleme kimliği belirtmezseniz, en son yedekleme kullanılacaktır. Bu sürümde çalışmaz.

## <a name="introduced-prior-to-february-2021"></a>Şubat 2021 ' den önce sunulmuştur

### <a name="data-controller"></a>Veri denetleyicisi

- Azure Kubernetes Service (AKS) üzerinde Kubernetes sürüm 1.19. x desteklenmez.
- Kubernetes 1,19 üzerinde `containerd` desteklenmez.
- Azure 'daki veri denetleyicisi kaynağı şu anda bir Azure kaynağıdır. Silme gibi tüm güncelleştirmeler Kubernetes kümesine geri yayılmaz.
- Örnek adları 13 karakterden fazla olamaz
- Azure Arc veri denetleyicisi veya veritabanı örnekleri için yerinde yükseltme yok.
- Arc özellikli veri hizmetleri kapsayıcı görüntüleri imzalanmadı.  Kubernetes düğümlerinizi, imzasız kapsayıcı görüntülerine çekilmesine izin verecek şekilde yapılandırmanız gerekebilir.  Örneğin, kapsayıcı çalışma zamanı olarak Docker kullanıyorsanız, DOCKER_CONTENT_TRUST = 0 ortam değişkenini ayarlayabilir ve yeniden başlatabilirsiniz.  Diğer kapsayıcı çalışma zamanları [OpenShift](https://docs.openshift.com/container-platform/4.5/openshift_images/image-configuration.html#images-configuration-file_image-configuration)'te olduğu gibi benzer seçeneklere sahiptir.
- Azure portal Azure Arc etkin SQL yönetilen örnekleri veya PostgreSQL hiper ölçek sunucu grupları oluşturulamıyor.
- Şimdilik, NFS kullanıyorsanız, `allowRunAsRoot` `true` Azure Arc veri denetleyicisini oluşturmadan önce dağıtım profili dosyanızda öğesini olarak ayarlamanız gerekir.
- Yalnızca SQL ve PostgreSQL oturum açma kimlik doğrulaması.  Azure Active Directory veya Active Directory desteği yok.
- Openshıft üzerinde bir veri denetleyicisi oluşturmak için gevşek güvenlik kısıtlamaları gerekir.  Ayrıntılar için belgelere bakın.
- Azure Arc veri denetleyicisi ve veritabanı örnekleri ile Azure Stack Hub üzerinde Azure Kubernetes hizmeti (AKS) altyapısı kullanıyorsanız, daha yeni bir Kubernetes sürümüne yükseltme desteklenmez. Kubernetes kümesini yükseltmeden önce Azure Arc veri denetleyicisi 'ni ve tüm veritabanı örneklerini kaldırın.
- [Birden çok kullanılabilirlik bölgesini](../../aks/availability-zones.md) kapsayan aks kümeleri, Azure Arc etkin veri Hizmetleri için şu anda desteklenmiyor. Bu sorundan kaçınmak için, Azure portal ' de AKS kümesi oluşturduğunuzda, bölgelerin kullanılabildiği bir bölgeyi seçerseniz seçim denetiminden tüm bölgeleri temizleyin. Aşağıdaki resme bakın:

   :::image type="content" source="media/release-notes/aks-zone-selector.png" alt-text="Hiçbirini belirtmek için her bir bölgenin onay kutularını temizleyin.":::

### <a name="postgresql"></a>PostgreSQL

- Azure Arc etkin PostgreSQL hiper ölçek, belirttiğiniz zaman göreli noktaya geri yükleme yaparken yanlış bir hata iletisi döndürüyor. Örneğin, geri yükleme için yedeklemelerinizin sahip olduğu sürümden daha eski bir nokta belirlediyseniz geri yükleme şu şekilde bir hata iletisiyle başarısız olur: `ERROR: (404). Reason: Not found. HTTP response body: {"code":404, "internalStatus":"NOT_FOUND", "reason":"Failed to restore backup for server...}`
Bu durumda, yedeklemeleriniz olan tarih aralığı içinde bir zaman noktası olduğunu belirterek komutu yeniden başlatın. Bu aralığı, yedeklemelerinizi listeleyerek ve bunların alındığı tarihlere bakarak belirlersiniz.
- Zaman içindeki bir noktaya geri yükleme yalnızca sunucu grupları arasında desteklenir. Zaman içinde bir noktaya geri yükleme işleminin hedef sunucusu, yedeklemeyi aldığınız sunucu olamaz. Farklı bir sunucu grubu olmalıdır. Ancak, tam geri yükleme aynı sunucu grubu için desteklenir.
- Tam geri yükleme yapılırken bir yedekleme kimliği gereklidir. Varsayılan olarak, bir yedekleme kimliği belirtmezseniz, en son yedekleme kullanılacaktır. Bu sürümde çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar

> **Yalnızca bir şeyi denemek mi istiyorsunuz?**  
> AKS, AWS elastik Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) veya bir Azure VM 'de [Azure Arc Jumpstart](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/) ile hızlı bir şekilde çalışmaya başlayın.

- [İstemci araçları 'nı yükler](install-client-tools.md)
- [Azure Arc veri denetleyicisi oluşturma](create-data-controller.md) (önce istemci araçlarının yüklenmesini gerektirir)
- Azure [Arc üzerinde Azure SQL yönetilen örneği oluşturma](create-sql-managed-instance.md) (önce Azure Arc veri denetleyicisinin oluşturulmasını gerektirir)
- [Azure Arc 'Da PostgreSQL Için Azure veritabanı hiper ölçek sunucu grubu oluşturma](create-postgresql-hyperscale-server-group.md) (önce Azure Arc veri denetleyicisi oluşturulmasını gerektirir)
- [Azure hizmetleri için kaynak sağlayıcıları](../../azure-resource-manager/management/azure-services-resource-providers.md)
- [Sürüm notları-Azure Arc etkin veri Hizmetleri (Önizleme)](release-notes.md)
