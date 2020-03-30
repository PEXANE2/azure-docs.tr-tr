---
title: Azure HPC Önbelleğini Takma
description: İstemcileri Azure HPC Önbellek hizmetine bağlama
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: d906ed9a1a55e936c6374806a9037085c47e3b01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73582223"
---
# <a name="mount-the-azure-hpc-cache"></a>Azure HPC Önbelleğini Dağın

Önbellek oluşturulduktan sonra, NFS istemcileri basit bir montaj komutuyla erişebilir.

Montaj komutu iki öğeden oluşur:

* Önbelleğin montaj adreslerinden biri (önbelleğe genel bakış sayfasında listelenir)
* Depolama hedefini oluşturduğunuzda ayarladığınız sanal ad alanı yolu

![Sağ alttaki montaj adresleri listesinin etrafında bir vurgu kutusu olan Azure HPC Önbellek örneğinin Genel Bakış sayfasının ekran görüntüsü](media/hpc-cache-mount-addresses.png)

> [!NOTE] 
> Önbellek montaj adresleri, önbelleğin alt ağındaki ağ arabirimlerine karşılık gelir. Bir kaynak grubunda, bu NIC'ler biten `-cluster-nic-` adlar ve bir sayı ile listelenir. Bu arabirimleri değiştirmeyin veya silmeyin, yoksa önbellek kullanılamaz hale gelir.

Sanal ad alanı yolları **Depolama hedefleri** sayfasında gösterilir. Ayrıntılarıyla ilişkili toplu ad alanı yolları da dahil olmak üzere ayrıntılarını görmek için tek bir depolama hedef adını tıklatın.

![önbelleğin Depolama hedef panelinin ekran görüntüsü, tablonun Yol sütunundaki bir girişin etrafında bir vurgu kutusu yla](media/hpc-cache-view-namespace-paths.png)

## <a name="mount-command-syntax"></a>Montaj komutu sözdizimi

Aşağıdaki gibi bir montaj komutu kullanın:

> sudo mount *cache_mount_address*:/*namespace_path* *local_path* {*seçenekler*}

Örnek:

```
root@test-client:/tmp# mkdir hpccache
root@test-client:/tmp# sudo mount 10.0.0.28:/blob-demo-0722 ./hpccache/ -orw,tcp,mountproto=tcp,vers3,hard
root@test-client:/tmp# 
```

Bu komut başarılı olduktan sonra, depolama dışa aktarma ``hpccache`` içeriği istemci üzerinde dizinde görünür olmalıdır.

> [!NOTE] 
> Müşterilerinizin önbelleğinizi barındıran sanal ağa ve alt ağa erişebilmesi gerekir. Örneğin, aynı sanal ağ içinde istemci VM'leri oluşturun veya dışarıdan erişmek için sanal ağda bir bitiş noktası, ağ geçidi veya başka bir çözüm kullanın. Önbelleğin alt ağı içinde başka hiçbir şeyin barındırılamayabileceğini unutmayın.

### <a name="mount-command-options"></a>Montaj komut seçenekleri

Sağlam bir istemci yuvası için, montaj komutunuzda bu ayarları ve bağımsız değişkenleri geçirin: 

``mount -o hard,proto=tcp,mountproto=tcp,retry=30 ${CACHE_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``

| Önerilen montaj komutu ayarları | |
--- | --- 
``hard`` | Azure HPC Önbelleğine yumuşak bağlar uygulama hataları ve olası veri kaybıyla ilişkilidir. 
``proto=netid`` | Bu seçenek, NFS ağ hatalarının uygun şekilde işlenmesini destekler.
``mountproto=netid`` | Bu seçenek, montaj işlemleri için ağ hatalarının uygun şekilde işlenmesini destekler.
``retry=n`` | Geçici ``retry=30`` montaj hatalarını önlemek için ayarlayın. (Ön plandaki bağlarda farklı bir değer önerilir.)

## <a name="next-steps"></a>Sonraki adımlar

* Verileri önbelleğin depolama hedeflerine taşımak [için, yeni Azure Blob depolama alanını doldur'u](hpc-cache-ingest.md)okuyun.
