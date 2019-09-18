---
title: Ücretsiz Azure hesabı için Ölçüm eşleme hizmeti
description: Ücretsiz hesaba dahil edilen hizmetlere ilişkin ölçüm eşleme hizmetini anlayın.
author: amberbhargava
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 8022c065d73aafc53d3dcb77e79c3e6320e0ce39
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "67490402"
---
# <a name="understand-free-service-to-meter-mapping"></a>Ücretsiz ölçüm eşleme hizmetini anlama

Her Azure hizmeti, ölçümlere karşı kullanım bilgilerini gösterir ve Azure faturalama sistemi, hizmetlerle ilgili olarak kullanıcıları ücretlendirmek için bunu kullanır. Ücretsiz hizmet kullanımını daha iyi anlamak için, hizmetlere ilişkin ölçüm eşleme hizmetine bakalım. Nasıl ücretsiz hizmetler oluşturulacağı hakkında bilgi edinmek için bkz. [Ücretsiz Azure hesabıyla ücretsiz hizmetler oluşturma](billing-create-free-services-included-free-account.md).

## <a name="service-to-meter-mapping-for-eligible-services"></a>Uygun hizmetler için ölçüm eşleme hizmeti

|    Hizmet   | Azure portalındaki Ölçüm Adı | Kullanım dosyasındaki/API’deki Ölçüm Adı | Ölçüm Kimliği |
| ------------ | -------------------------- | -------------------------| -------- |
| B1S Linux Sanal Makinesi | İşlem Saatleri - Standard_B1 Sanal Makinesi | İşlem Saatleri - Ücretsiz | 8260cba2-4437-47d1-a31e-2561cd370f50
| B1S Windows Sanal Makinesi | İşlem Saatleri - Standard_B1 Sanal Makinesi (Windows) | İşlem Saatleri - Ücretsiz | ff3e6fa5-ee46-478e-8d0e-b629f4f8a8ac
| B1S Sanal Makinesi - Genel IP Adresleri  | IP Adresi Saatleri - Genel IP Adresleri | IP Adresi Saatleri - Ücretsiz | ae56b367-2708-4454-a3d9-2be7b2364ea1
| CosmosDB | Depolama (GB) - Cosmos DB | Depolama (GB) - Ücretsiz | 59c78b09-08e2-466a-9f3b-57a94c9e2f31
| CosmosDB | 100 İstek Birimi (Saat) - Cosmos DB | 100 İstek Birimi (Saat) - Ücretsiz | 5d638a6f-e221-41cf-ae3f-0f81d368cef6
| Dosya Depolama | Standart GÇ - Dosyalar (GB) - Yerel Olarak Yedekli | Standart GÇ - Dosyalar (GB) - Ücretsiz | a7f2aa67-b9a2-4593-a413-6ec86d6c8e5b
| Dosya Depolama | Standart GÇ - Dosya Okuma İşlemi Birimleri (10.000’lik bloklar) | Standart GÇ - Dosya Okuma İşlemi Birimleri (10.000’lik bloklar) - Ücretsiz | 6207404d-3389-4d20-9087-cc078ddc3fd9
| Dosya Depolama | Standart GÇ - Dosya Yazma İşlemi Birimleri (10.000’lik bloklar) | Standart GÇ - Dosya Yazma İşlem Birimleri (10.000’lik bloklar) - Ücretsiz | 223d8004-d29a-46cf-b4f4-d2d34b12548b
| Dosya Depolama | Standart GÇ - Dosya Protokol İşlemi Birimleri (10.000’lik bloklar) | Standart GÇ - Dosya Protokol İşlemi Birimleri (10.000’lik bloklar) - Ücretsiz | a347d8cc-51d1-4a0e-b9eb-76f67566c3f5
| Dosya Depolama | Standart GÇ - Dosya Listeleme İşlemi Birimleri (10.000’lik bloklar) | Standart GÇ - Dosya Listeleme İşlemi Birimleri (10.000’lik bloklar) - Ücretsiz | e8ae79ad-c2ab-4d82-b226-dd3c33dfd40c
| Sık Erişimli Blok Blobu Depolama | Standart GÇ - Sık Erişimli Blok Blobu Okuma İşlemleri (10.000’lik bloklar) | Standart GÇ - Sık Erişimli Blok Blobu Okuma İşlemleri (10.000’lik bloklar) - Ücretsiz |fd7cfa1e-026e-4be1-871b-1c2386e8902e
| Sık Erişimli Blok Blobu Depolama | Standart GÇ - Sık Erişimli Blok Blobu (GB) - Yerel Olarak Yedekli | Standart GÇ - Sık Erişimli Blok Blobu (GB) - Ücretsiz | 67a3a3fd-826f-42c1-8843-bffa14f0da13
| Sık Erişimli Blok Blobu Depolama | Standart GÇ - Sık Erişimli Blok Blobu Yazma İşlemleri (10.000’lik bloklar) | Standart GÇ - Sık Erişimli Blok Blobu Yazma İşlemleri (10.000’lik bloklar) - Ücretsiz | b34bbb76-edce-4c2d-a288-81a2db1fea53
| Sık Erişimli Blok Blobu Depolama  | Standart GÇ - Sık Erişimli Blok Blobu Yazma/Listeleme İşlemleri (10.000’lik bloklar) | Standart GÇ - Sık Erişimli Blok Blobu Yazma/Listeleme İşlemleri (10.000’lik bloklar) - Ücretsiz | 7e68cf36-1198-4d3b-baa7-86a74c5b3079
| Yönetilen Disk <sup>1</sup>  | Standart Yönetilen Disk/Anlık Görüntüler (GB) - Yerel Olarak Yedekli | Standart Yönetilen Disk/Anlık Görüntüler (GB) - Ücretsiz | ad94c237-52a5-4804-ae65-38c5bf85ef42
| Yönetilen Disk <sup>1</sup>  | Standart Yönetilen Disk İşlemleri (10.000’lik bloklar) | Standart Yönetilen Disk İşlemleri (10.000’lik bloklar) - Ücretsiz | 82cc6ea4-0abd-43ac-acc0-ec34edf0f14c
| Yönetilen Disk <sup>1</sup>  | Premium Depolama - Sayfa Blobu/P6 (Birimler) - Yerel Olarak Yedekli | Premium Depolama - Sayfa Blobu/P6 (Birimler) - Ücretsiz | 2b98c168-27ca-4cc1-b509-e887dec87657
| SQL Veritabanı | Standart S0 Veritabanı Günleri - SQL Veritabanı | Standart S0 Veritabanı Günleri - Ücretsiz | dd6b69d3-9be0-4a91-abff-2c58bbcafd1d
| Paylaşılan - Bant Genişliği <sup>2</sup> | Giden Veri Aktarımı (GB) | Giden Veri Aktarımı (GB) - Ücretsiz | 0fc067a1-65d2-46da-b24b-7a9cbe2c69bd

<sup>1</sup> Bir Windows sanal makinesi oluşturup yönetilen disk seçerseniz, yönetilen disk ölçümünü sanal makinenin parçası olarak kullanırsınız.

<sup>2</sup> Paylaşılan ölçümler birden çok hizmet aracılığıyla kullanılabilir. Örneğin, hem Sanal makineler hem de Depolama alanı, Giden Veri Aktarımı (GB) ölçümüne karşı kullanım bilgilerini gösterir.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Aboneliğinizi yükseltme](billing-upgrade-azure-subscription.md)
