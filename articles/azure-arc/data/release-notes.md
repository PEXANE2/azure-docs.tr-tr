---
title: Azure Arc etkin veri Hizmetleri-sürüm notları
description: En son sürüm notları
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 10/29/2020
ms.topic: conceptual
ms.openlocfilehash: 82dd2f16fa43b52ba4c6dfacd26da5da622523b2
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321707"
---
# <a name="release-notes---azure-arc-enabled-data-services-preview"></a>Sürüm notları-Azure Arc etkin veri Hizmetleri (Önizleme)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="october-2020"></a>Ekim 2020 

Azure Data CLı ( `azdata` ) sürüm numarası: 20.2.3. Adresinden indirin [https://aka.ms/azdata](https://aka.ms/azdata) .

### <a name="breaking-changes"></a>Yeni değişiklikler

Bu sürümde aşağıdaki son değişiklikler tanıtılmaktadır: 

* PostgreSQL özel kaynak tanımında (CRD), terim `shards` olarak yeniden adlandırılır `workers` . Bu terim ( `workers` ) komut satırı parametre adıyla eşleşir.

* `azdata arc postgres server delete` bir Postgres örneğini silmeden önce onay ister.  `--force`İstemi atlamak için kullanın.

### <a name="additional-changes"></a>Ek değişiklikler

* Çağrılan yeni bir isteğe bağlı parametre eklendi `azdata arc postgres server create` `--volume-claim mounts` . Değer, birim talebi TAK'nin bir virgülle ayrılmış listesidir. Birim talebi bağlama, birim türü ve PVC adı çiftidir. Şu anda desteklenen tek birim türü `backup` .  PostgreSQL içinde, birim türü olduğunda, `backup` PVC öğesine bağlanır `/mnt/db-backups` .  Bu, bir PostgresSQL örneğinin yedeğinin başka bir örneğe geri yüklenebilmesi için PostgresSQL örnekleri arasında yedeklemelerin paylaşılmasını mümkün bir şekilde sunar.

* PostgresSQL özel kaynak tanımlarına yönelik yeni bir kısa ad: 

  * `pg11` 

  * `pg12`

* Telemetri karşıya yüklemesi, Kullanıcı şunları içerir:

   * Azure 'a yüklenen noktaların sayısı

     veya 

   * Azure 'a hiçbir veri yüklenmediyse, yeniden denemek için bir istem yazın.

* `azdata arc dc debug copy-logs` Şimdi de klasörden okur `/var/opt/controller/log` ve Linux üzerinde PostgreSQL altyapı günlüklerini toplar.

*   PostgreSQL hiper Ölçeklendirmesiyle yedek oluşturma ve geri yükleme sırasında çalışma göstergesi görüntüleyin.

* `azdata arc postrgres backup list` Şimdi yedek boyut bilgilerini içerir.

* SQL yönetilen örnek yönetici adı özelliği, Azure portal genel bakış dikey penceresinin sağ sütununa eklendi.

* Azure Data Studio, PostgreSQL hiper ölçek için çalışan düğüm sayısı, sanal çekirdek ve bellek ayarlarının yapılandırılmasını destekler. 

* Önizleme, Postgres sürüm 11 ve 12 için yedekleme/geri yüklemeyi destekler.

## <a name="september-2020"></a>Eylül 2020

Azure Arc etkin veri Hizmetleri genel önizleme için kullanıma sunuldu. Yay etkin veri Hizmetleri, veri hizmetlerini her yerde yönetmenizi sağlar.

- SQL Yönetilen Örnek
- PostgreSQL hiper ölçek

Yönergeler için bkz. [Azure Arc etkin veri Hizmetleri nedir?](overview.md)

## <a name="known-limitations-and-issues"></a>Bilinen sınırlamalar ve sorunlar

- Örnek adları en fazla 13 karakter uzunluğunda olabilir
- Azure Arc veri denetleyicisi veya veritabanı örnekleri için yerinde yükseltme yok.
- Arc özellikli veri hizmetleri kapsayıcı görüntüleri imzalanmadı.  Kubernetes düğümlerinizi, imzasız kapsayıcı görüntülerine çekilmesine izin verecek şekilde yapılandırmanız gerekebilir.  Örneğin, kapsayıcı çalışma zamanı olarak Docker kullanıyorsanız, DOCKER_CONTENT_TRUST = 0 ortam değişkenini ayarlayabilir ve yeniden başlatabilirsiniz.  Diğer kapsayıcı çalışma zamanları [OpenShift](https://docs.openshift.com/container-platform/4.5/openshift_images/image-configuration.html#images-configuration-file_image-configuration)'te olduğu gibi benzer seçeneklere sahiptir.
- Azure portal Azure Arc etkin SQL yönetilen örnekleri veya PostgreSQL hiper ölçek sunucu grupları oluşturulamıyor.
- Şimdilik, NFS kullanıyorsanız, `allowRunAsRoot` `true` Azure Arc veri denetleyicisini oluşturmadan önce dağıtım profili dosyanızda öğesini olarak ayarlamanız gerekir.
- Yalnızca SQL ve PostgreSQL oturum açma kimlik doğrulaması.  Azure Active Directory veya Active Directory desteği yok.
- Openshıft üzerinde bir veri denetleyicisi oluşturmak için gevşek güvenlik kısıtlamaları gerekir.  Ayrıntılar için belgelere bakın.
- Azure Arc veri denetleyicisi ve veritabanı örnekleri ile Azure Stack hub 'ında Azure Kubernetes hizmet altyapısı 'nı (AKS motoru) kullanıyorsanız, daha yeni bir Kubernetes sürümüne yükseltme desteklenmez. Kubernetes kümesini yükseltmeden önce Azure Arc veri denetleyicisi 'ni ve tüm veritabanı örneklerini kaldırın.
- Azure Kubernetes hizmeti (AKS), [birden çok kullanılabilirlik bölgesini](../../aks/availability-zones.md) kapsayan kümeler Şu anda Azure Arc etkin veri Hizmetleri için desteklenmiyor. Bu sorundan kaçınmak için, Azure portal ' de AKS kümesi oluşturduğunuzda, bölgelerin kullanılabildiği bir bölgeyi seçerseniz seçim denetiminden tüm bölgeleri temizleyin. Aşağıdaki resme bakın:

   :::image type="content" source="media/release-notes/aks-zone-selector.png" alt-text="Hiçbirini belirtmek için her bir bölgenin onay kutularını temizleyin.":::


### <a name="known-issues-for-azure-arc-enabled-postgresql-hyperscale"></a>Azure Arc etkin PostgreSQL hiper ölçek için bilinen sorunlar   

- Önizleme, PostgreSQL için yedekleme/geri yükleme sürümü 11 altyapısını desteklemez. Yalnızca PostgreSQL için yedekleme/geri yükleme sürüm 12 ' i destekler.
- `azdata arc dc debug copy-logs` nWindows üzerinde PostgreSQL altyapısı günlükleri toplanmaz.
- Yeni silinen bir sunucu grubunun adı ile bir sunucu grubu yeniden oluşturmak başarısız olabilir veya askıda kalabilir. 
   - **Geçici çözüm** Bir sunucu grubunu yeniden oluşturduğunuzda veya daha önce silinen sunucu grubunun yük dengeleyiciyi/dış hizmetini beklerken aynı adı yeniden kullanmayın. Sildiğiniz sunucu grubunun adının olduğunu `postgres01` ve bir ad alanında barındırıldığını varsayarsak, `arc` aynı ada sahip bir sunucu grubunu yeniden oluşturmadan önce, `postgres01-external-svc` kubectl komutunun çıkışında bu işlemi gösterene kadar bekleyin `kubectl get svc -n arc` .
 - Azure Data Studio genel bakış sayfası ve Işlem + depolama yapılandırması sayfasını yüklemek yavaş. 



## <a name="next-steps"></a>Sonraki adımlar
  
> **Yalnızca bir şeyi denemek mi istiyorsunuz?**  
> Azure Kubernetes Service (AKS), AWS elastik Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) veya bir Azure VM 'de [Azure Arc Jumpstart](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services) ile hızlıca çalışmaya başlayın.

[İstemci araçları 'nı yükler](install-client-tools.md)

[Azure Arc veri denetleyicisi oluşturma](create-data-controller.md) (önce istemci araçlarının yüklenmesini gerektirir)

Azure [Arc üzerinde Azure SQL yönetilen örneği oluşturma](create-sql-managed-instance.md) (önce Azure Arc veri denetleyicisinin oluşturulmasını gerektirir)

[Azure Arc 'Da PostgreSQL Için Azure veritabanı hiper ölçek sunucu grubu oluşturma](create-postgresql-hyperscale-server-group.md) (önce Azure Arc veri denetleyicisi oluşturulmasını gerektirir)
