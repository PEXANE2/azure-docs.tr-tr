---
title: Azure blok zinciri hizmeti muhasebe sürümleri, düzeltme eki uygulama & yükseltme
description: Azure blok zinciri hizmetindeki desteklenen bağış cihazları sürümlerine genel bakış. Sistem düzeltme eki uygulama ve yükseltmeleri için ilkeler ekleme.
ms.date: 06/30/2020
ms.topic: conceptual
ms.reviewer: ravastra
ms.openlocfilehash: ea7c54e229178320329204a3199ab2b4c44058fe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85807749"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Desteklenen Azure blok zinciri hizmeti muhasebe sürümleri

Azure blok zinciri hizmeti, Azure blok zinciri hizmeti 'nde bir konsorsiyum olarak tanımlanan, bilinen katılımcılar grubu içindeki özel işlemlerin işlenmesi için tasarlanan Ethereum tabanlı [çekirdek](https://www.goquorum.com/developers) muhasebeyi kullanır.

Şu anda Azure blok zinciri hizmeti, [çekirdek sürümü 2.6.0](https://github.com/jpmorganchase/quorum/releases/tag/v2.6.0) ve [Tessera işlem yöneticisini](https://github.com/jpmorganchase/tessera)destekler.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme

Çekirdekte sürüm oluşturma, ana, ikincil ve düzeltme eki yayınlar aracılığıyla yapılır. Örneğin, çekirdek sürümü 2.0.1 ise, yayın türü şu şekilde kategorilere ayrılır:

|Ana | İkincil  | Patch  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Azure blok zinciri hizmeti, çekirdeğin düzeltme eki sürümlerini, çekirdekte kullanılabilir hale getirilmekte olan 30 gün içinde var olan çalışan üyelere otomatik olarak güncelleştirir.

## <a name="availability-of-new-ledger-versions"></a>Yeni muhasebe sürümlerinin kullanılabilirliği

Azure blok zinciri hizmeti, çekirdek üreticisinin 60 gün içinde kullanılabilir olması için çekirdek muhasebeyi en son büyük ve küçük sürümlerini sağlar. Yeni bir üye ve konsorsiyumun sağlanmasından sonra aralarından seçim yapmak için en fazla dört alt sürüm verilmiştir. Sürümünden büyük veya küçük bir sürüme yükseltme şu anda desteklenmiyor. Örneğin, sürüm 2. x çalıştırıyorsanız, sürüm 3. x ' e yükseltme şu anda desteklenmemektedir. Benzer şekilde, 2,2 sürümünü çalıştırıyorsanız, sürüm 2,3 ' ye yükseltme şu anda desteklenmemektedir.

## <a name="how-to-check-quorum-ledger-version"></a>Çekirdek defter sürümünü denetleme

Azure blok zinciri hizmeti üyesinde, geth kullanarak düğümünüz ekleyerek veya tanılama günlüklerini görüntüleyerek çekirdek sürümünü kontrol edebilirsiniz.

### <a name="using-geth"></a>Geth kullanma

Geth kullanarak Azure blok zinciri hizmeti düğümüne ekleyin. Örneğin, `geth attach https://myblockchainmember.blockchain.azure.com:3200/<Access key>`.

Düğümünüz bağlandıktan sonra, geth aşağıdaki çıktıya benzer bir çekirdek sürümü raporlar:

``` text
instance: Geth/v1.9.7-stable-9339be03(quorum-v2.6.0)/linux-amd64/go1.13.12
```

Geth kullanma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure blok zinciri hizmeti işlem düğümüne eklemek Için geth kullanma](connect-geth.md).

### <a name="using-diagnostic-logs"></a>Tanılama günlüklerini kullanma

Tanılama günlüklerini etkinleştirirseniz, çekirdek sürümü işlem düğümleri için raporlanır. Örneğin, aşağıdaki düğüm bilgilendirme günlüğü iletisi çekirdek sürümünü içerir.

``` text 
{"NodeName":"transaction-node","Message":"INFO [06-22|05:31:45.156] Starting peer-to-peer node instance=Geth/v1.9.7-stable-9339be03(quorum-v2.6.0)/linux-amd64/go1.13.12\n"}
{"NodeName":"transaction-node","Message":"[*] Starting Quorum node with QUORUM_VERSION=2.6.0, TESSERA_VERSION=0.10.5 and PRIVATE_CONFIG=/working-dir/c/tm.ipc\n"}
111
```

Tanılama günlükleri hakkında daha fazla bilgi için bkz. [Azure izleyici aracılığıyla Azure blok zinciri hizmetini izleme](monitor-azure-blockchain-service.md#diagnostic-settings).

## <a name="how-to-check-genesis-file-content"></a>Genessıs dosya içeriğini denetleme

Blockzincirinizin düğümündeki genessıs dosya içeriğini denetlemek için aşağıdaki Ethereum JavaScript API 'sini kullanabilirsiniz:

``` bash
admin.nodeInfo.protocols
```
Bir geth konsolunu veya bir Web3 kitaplığını kullanarak API 'YI çağırabilirsiniz. Geth kullanma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure blok zinciri hizmeti işlem düğümüne eklemek Için geth kullanma](connect-geth.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure blok zinciri hizmeti sınırları](limits.md)
