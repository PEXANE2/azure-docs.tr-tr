---
title: Azure HPC önbelleği bağlama
description: İstemcileri Azure HPC önbellek hizmetine bağlama
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: v-erkell
ms.openlocfilehash: 46f221fd7c340b7f321d317f0e7493448d83177c
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71036940"
---
# <a name="mount-the-azure-hpc-cache"></a>Azure HPC önbelleğini bağlama

Önbellek oluşturulduktan sonra, NFS istemcileri basit bir Mount komutuyla erişebilir.

Önbelleğe Genel Bakış sayfasında listelenen Mount adreslerini ve depolama hedefini oluştururken ayarladığınız sanal ad alanı yolunu kullanın. 

![Azure HPC önbellek örneğinin genel bakış sayfasının ekran görüntüsü, sağ alt taraftaki bağlama adresleri listesi etrafında bir vurgulama kutusuyla](media/hpc-cache-mount-addresses.png)

> [!NOTE] 
> Önbellek bağlama adresleri, önbelleğin alt ağının içindeki ağ arabirimlerine karşılık gelir. Bu NIC 'ler kaynak grubunda bitiş `-cluster-nic-` adlarıyla ve bir sayıyla görüntülenir. Bu arabirimleri değiştirmeyin veya silmeyin, aksi durumda önbellek kullanılamaz hale gelir.

Sanal ad alanı yolları, **depolama hedefleri** sayfasında gösterilir. İle ilişkili toplanmış ad alanı yolu (veya yolları) dahil olmak üzere, ayrıntıları görmek için hedef adına tıklayın. 
![tablonun yol sütunundaki bir girdinin etrafında vurgulama kutusuyla, önbelleğin depolama hedefi panelinin ekran görüntüsü](media/hpc-cache-view-namespace-paths.png)

## <a name="mount-command-syntax"></a>Bağlama komutu sözdizimi

Aşağıdaki gibi bir mount komutu kullanın:

> sudo Mount *cache_mount_address*:/*namespace_path* *local_path* {*Seçenekler*}

Örnek: 

```
root@test-client:/tmp# mkdir hpccache
root@test-client:/tmp# sudo mount 10.0.0.28:/blob-demo-0722 ./hpccache/ -orw,tcp,mountproto=tcp,vers3,hard,intr
root@test-client:/tmp# 
```

Bu komut başarılı olduktan sonra, depolama dışarı aktarmanın içerikleri istemcideki ``hpccache`` dizinde görünür olmalıdır. 

> [!NOTE] 
> İstemcileriniz, önbelleğinizi barındıran sanal ağa ve alt ağa erişebilmelidir. Örneğin, aynı sanal ağ içinde istemci VM 'Leri oluşturun veya dışarıdaki bir erişim için bir uç nokta, ağ geçidi veya sanal ağda başka bir çözüm kullanın. Önbelleğin alt ağı içinde başka hiçbir şeyin barındırılayamadığını unutmayın.

### <a name="mount-command-options"></a>Bağlama komutu seçenekleri

Sağlam bir istemci bağlama için, bu ayarları ve bağımsız değişkenleri bağlama komutunuz geçirin: 

``mount -o hard,nointr,proto=tcp,mountproto=tcp,retry=30 ${CACHE_IP_ADDRESS}:/${NAMESPACE_PATH} ${LOCAL_FILESYSTEM_MOUNT_POINT}``

| Önerilen bağlama komutu ayarları | |
--- | --- 
``hard`` | VFXT kümesine yönelik hafif bağlar, uygulama hatalarıyla ve olası veri kaybı ile ilişkilendirilir. 
``proto=netid`` | Bu seçenek NFS ağ hatalarının uygun işlenmesini destekler.
``mountproto=netid`` | Bu seçenek, bağlama işlemleri için ağ hatalarının uygun işlenmesini destekler.
``retry=n`` | Geçici ``retry=30`` bağlama hatalarından kaçınmak için ayarlayın. (Ön plan takmaları farklı bir değer önerilir.)

## <a name="next-steps"></a>Sonraki adımlar

* Verileri önbelleğin depolama hedeflerine taşımak için [yeni Azure Blob depolama alanını](hpc-cache-ingest.md)okuyun.
