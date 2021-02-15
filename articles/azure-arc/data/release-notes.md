---
title: Azure Arc etkin veri Hizmetleri-sürüm notları
description: En son sürüm notları
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 02/11/2021
ms.topic: conceptual
ms.openlocfilehash: f303ddb4d32da4c4cb6609f3ceec34e5c83529a8
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100391465"
---
# <a name="release-notes---azure-arc-enabled-data-services-preview"></a>Sürüm notları-Azure Arc etkin veri Hizmetleri (Önizleme)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="january-2021"></a>Ocak 2021

### <a name="new-capabilities-and-features"></a>Yeni özellikler ve Özellikler

Azure Data CLı ( `azdata` ) sürüm numarası: 20.3.0. Adresinden indirin [https://aka.ms/azdata](https://aka.ms/azdata) . `azdata` [Azure Data CLI ( `azdata` ) uygulamasını yükleyebilirsiniz](/sql/azdata/install/deploy-install-azdata).


Ek güncelleştirmeler şunlardır:
- Yerelleştirilmiş Portal 17 yeni dil için kullanılabilir
- Kuin-Native. YAML dosyalarında küçük değişiklikler
- Yeni Grafana ve kibana sürümleri
- Azure Data Studio çözümlenen not defterlerinde azdata kullanırken Python ortamları ile ilgili sorunlar
- Pg_audit uzantısı artık PostgreSQL hiper ölçek için kullanılabilir
- Bir PostgreSQL hiper ölçek veritabanının tam geri yüklenmesi sırasında bir yedekleme KIMLIĞI artık gerekli değildir
- Durum (sistem durumu), bir sunucu grubu oluşturan tüm PostgreSQL örnekleri için raporlanır

   Önceki sürümlerde, durum sunucu grubu düzeyinde toplanmış ve PostgreSQL düğüm düzeyinde toplanmıyor.

- PostgreSQL dağıtımları artık Create komutlarında belirtilen birim boyutu parametrelerini karşılayamıyor
- Bir sunucu grubu düzenlenirken altyapı sürümü parametreleri artık kabul edilir
- PostgreSQL hiper ölçek etkinleştirilmiş Azure Arc 'ın adlandırma kuralı değiştirildi

    Şu biçimdedir: `ServergroupName{c, w}-n` . Örneğin, üç düğüm içeren bir sunucu grubu, bir düzenleyici düğümü ve iki çalışan düğümü şöyle temsil edilir:
   - `Postgres01c-0` (düzenleyici düğümü)
   - `Postgres01w-0` (çalışan düğümü)
   - `Postgres01w-1` (çalışan düğümü)

## <a name="december-2020"></a>Aralık 2020

### <a name="new-capabilities--features"></a>Yeni yetenekler & özellikleri

Azure Data CLı ( `azdata` ) sürüm numarası: 20.2.5. Adresinden indirin [https://aka.ms/azdata](https://aka.ms/azdata) .

SQL yönetilen örneği ve PostgreSQL hiper ölçek için uç noktalarını, Azure Data CLı ( `azdata` ) ile ve komutlarını kullanarak görüntüleyin `azdata arc sql endpoint list` `azdata arc postgres endpoint list` .

Azure Data Studio kullanarak SQL yönetilen örnek kaynağını (CPU çekirdeği ve bellek) isteklerini ve sınırlarını düzenleyin.

Azure Arc etkin PostgreSQL Hyperscale artık, her iki sürüm 11 ve 12 ' den PostgreSQL için tam yedekleme geri yüklemeye ek olarak zaman içinde nokta geri yüklemeye olanak sağlar. Zaman içindeki nokta geri yükleme özelliği, geri yüklenecek belirli bir tarih ve saati belirtmenize olanak tanır.

Azure Arc 'ın hiper ölçek etkin PostgreSQL için adlandırma kuralı değişti. Şu biçimdedir: ServergroupName {r, s}-_n_. Örneğin, üç düğüm içeren bir sunucu grubu, bir düzenleyici düğümü ve iki çalışan düğümü şöyle temsil edilir:
- `postgres02r-0` (düzenleyici düğümü)
- `postgres02s-0` (çalışan düğümü)
- `postgres02s-1` (çalışan düğümü)

### <a name="breaking-change"></a>Son değişiklik

#### <a name="new-resource-provider"></a>Yeni kaynak sağlayıcısı

Bu sürümde, adlı güncelleştirilmiş bir [kaynak sağlayıcısı](../../azure-resource-manager/management/azure-services-resource-providers.md) tanıtılmıştır `Microsoft.AzureArcData` . Bu özelliği kullanabilmeniz için önce bu kaynak sağlayıcısını kaydetmeniz gerekir. 

Bu kaynak sağlayıcısını kaydetmek için: 

1. Azure portal **abonelikler** ' i seçin 
2. Aboneliğinizi seçin
3. **Ayarlar** altında **kaynak sağlayıcıları** ' nı seçin. 
4. Kayıt Ara `Microsoft.AzureArcData` ve Seç  

[Azure kaynak sağlayıcıları ve türlerindeki](../../azure-resource-manager/management/resource-providers-and-types.md)ayrıntılı adımları gözden geçirebilirsiniz. Bu değişiklik Ayrıca, Azure portal yüklediğiniz tüm mevcut Azure kaynaklarını da kaldırır. Kaynak sağlayıcısını kullanabilmeniz için, veri denetleyicisini güncelleştirmeniz ve en son CLI 'yi kullanmanız gerekir `azdata` .  

### <a name="platform-release-notes"></a>Platform sürüm notları

#### <a name="direct-connectivity-mode"></a>Doğrudan bağlantı modu

Bu sürüm doğrudan bağlantı modunu tanıtır. Doğrudan bağlantı modu, veri denetleyicisinin kullanım bilgilerini Azure 'a otomatik olarak yüklemesine olanak sağlar. Kullanım yükleme işleminin bir parçası olarak, Arc veri denetleyicisi kaynağı zaten karşıya yükleme yoluyla oluşturulmadıysa portalda otomatik olarak oluşturulur `azdata` .  

Veri denetleyicisini oluştururken doğrudan bağlantı belirtebilirsiniz. Aşağıdaki örnek, `azdata arc dc create` `arc` doğrudan bağlantı modu () kullanılarak adlandırılmış bir veri denetleyicisi oluşturur `connectivity-mode direct` . Örneği çalıştırmadan önce `<subscription id>` ABONELIK Kimliğinizle değiştirin.

```console
azdata arc dc create --profile-name azure-arc-aks-hci --namespace arc --name arc --subscription <subscription id> --resource-group my-resource-group --location eastus --connectivity-mode direct
```

### <a name="known-issues"></a>Bilinen sorunlar

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

#### <a name="postgresql"></a>PostgreSQL

- Azure Arc etkin PostgreSQL hiper ölçek, belirttiğiniz zaman göreli noktaya geri yükleme yaparken yanlış bir hata iletisi döndürüyor. Örneğin, geri yükleme için yedeklemelerinizin içerebileceği sürümden daha eski bir nokta belirlediyseniz geri yükleme şu şekilde bir hata iletisiyle başarısız olur: hata: (404). Neden: bulunamadı. HTTP yanıt gövdesi: {"Code": 404, "ınternalstatus": "NOT_FOUND", "Neden": "sunucu yedeklemesi geri yüklenemedi...}
Bu durumda, yedeklemeleriniz olan tarih aralığı içinde bir zaman noktası olduğunu belirterek komutu yeniden başlatın. Bu aralığı, yedeklemelerinizi listeleyerek ve bunların alındığı tarihlere bakarak belirlersiniz.
- Zaman içindeki bir noktaya geri yükleme yalnızca sunucu grupları arasında desteklenir. Zaman içinde bir noktaya geri yükleme işleminin hedef sunucusu, yedeklemeyi aldığınız sunucu olamaz. Farklı bir sunucu grubu olmalıdır. Ancak, tam geri yükleme aynı sunucu grubu için desteklenir.
- Tam geri yükleme yapılırken bir yedekleme kimliği gereklidir. Varsayılan olarak, bir yedekleme kimliği belirtmezseniz, en son yedekleme kullanılacaktır. Bu sürümde çalışmaz.

## <a name="october-2020"></a>Ekim 2020 

Azure Data CLı ( `azdata` ) sürüm numarası: 20.2.3. Adresinden indirin [https://aka.ms/azdata](https://aka.ms/azdata) .

### <a name="breaking-changes"></a>Yeni değişiklikler

Bu sürümde aşağıdaki son değişiklikler tanıtılmaktadır: 

* PostgreSQL özel kaynak tanımında (CRD), terim `shards` olarak yeniden adlandırılır `workers` . Bu terim ( `workers` ) komut satırı parametre adıyla eşleşir.

* `azdata arc postgres server delete` bir Postgres örneğini silmeden önce onay ister.  `--force`İstemi atlamak için kullanın.

### <a name="additional-changes"></a>Ek değişiklikler

* Çağrılan yeni bir isteğe bağlı parametre eklendi `azdata arc postgres server create` `--volume-claim mounts` . Değer, birim talebi TAK'nin bir virgülle ayrılmış listesidir. Birim talebi bağlama, birim türü ve PVC adı çiftidir. Şu anda desteklenen tek birim türü `backup` .  PostgreSQL içinde, birim türü olduğunda, `backup` PVC öğesine bağlanır `/mnt/db-backups` .  Bu, bir PostgresSQL örneğinin yedeğinin başka bir örneğe geri yüklenebilmesi için PostgresSQL örnekleri arasında yedeklemelerin paylaşılmasını mümkün bir şekilde sunar.

* PostgresSQL özel kaynak tanımları için yeni kısa adlar: 
  * `pg11` 
  * `pg12`
* Telemetri karşıya yüklemesi, Kullanıcı şunları içerir:
   * Azure 'a yüklenen noktaların sayısı veya 
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

## <a name="next-steps"></a>Sonraki adımlar

> **Yalnızca bir şeyi denemek mi istiyorsunuz?**  
> AKS, AWS elastik Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) veya bir Azure VM 'de [Azure Arc Jumpstart](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/) ile hızlı bir şekilde çalışmaya başlayın.

- [İstemci araçları 'nı yükler](install-client-tools.md)
- [Azure Arc veri denetleyicisi oluşturma](create-data-controller.md) (önce istemci araçlarının yüklenmesini gerektirir)
- Azure [Arc üzerinde Azure SQL yönetilen örneği oluşturma](create-sql-managed-instance.md) (önce Azure Arc veri denetleyicisinin oluşturulmasını gerektirir)
- [Azure Arc 'Da PostgreSQL Için Azure veritabanı hiper ölçek sunucu grubu oluşturma](create-postgresql-hyperscale-server-group.md) (önce Azure Arc veri denetleyicisi oluşturulmasını gerektirir)
- [Azure hizmetleri için kaynak sağlayıcıları](../../azure-resource-manager/management/azure-services-resource-providers.md)
