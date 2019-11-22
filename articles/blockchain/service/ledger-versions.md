---
title: Azure blok zinciri hizmeti muhasebe sürümleri, düzeltme eki uygulama & yükseltme
description: Systems düzeltme eki uygulama ve sistem tarafından yönetilen ve Kullanıcı tarafından yönetilen yükseltmelerle ilgili ilkeler de dahil olmak üzere Azure blok zinciri hizmeti 'ndeki desteklenen bağış cihazları sürümlerine genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 11/20/2019
ms.topic: conceptual
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 03eaf86ece191cce7eef6647a582a66495692db2
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74284918"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Desteklenen Azure blok zinciri hizmeti muhasebe sürümleri

Azure blok zinciri hizmeti, Azure blok zinciri hizmeti 'nde bir konsorsiyum olarak tanımlanan, bilinen katılımcılar grubu içindeki özel işlemlerin işlenmesi için tasarlanan Ethereum tabanlı [çekirdek](https://www.goquorum.com/developers) muhasebeyi kullanır.

Şu anda Azure blok zinciri hizmeti, [çekirdek sürümü 2.2.3](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.3) ve [Tessera işlem yöneticisini](https://github.com/jpmorganchase/tessera)destekler.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme

Çekirdekte sürüm oluşturma, ana, ikincil ve düzeltme eki yayınlar aracılığıyla yapılır. Örneğin, çekirdek sürümü 2.0.1 ise, yayın türü şu şekilde kategorilere ayrılır:

|Büyük | Küçük  | Düzeltmesi  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Azure blok zinciri hizmeti, çekirdeğin düzeltme eki sürümlerini, çekirdekte kullanılabilir hale getirilmekte olan 30 gün içinde var olan çalışan üyelere otomatik olarak güncelleştirir.

## <a name="availability-of-new-ledger-versions"></a>Yeni muhasebe sürümlerinin kullanılabilirliği

Azure blok zinciri hizmeti, çekirdek üreticisinin 60 gün içinde kullanılabilir olması için çekirdek muhasebeyi en son büyük ve küçük sürümlerini sağlar. Yeni bir üye ve konsorsiyumun sağlanmasından sonra aralarından seçim yapmak için en fazla dört alt sürüm verilmiştir. Sürümünden büyük veya küçük bir sürüme yükseltme şu anda desteklenmiyor. Örneğin, sürüm 2. x çalıştırıyorsanız, sürüm 3. x ' e yükseltme şu anda desteklenmemektedir. Benzer şekilde, 2,2 sürümünü çalıştırıyorsanız, sürüm 2,3 ' ye yükseltme şu anda desteklenmemektedir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure blok zinciri hizmeti sınırları](limits.md)
