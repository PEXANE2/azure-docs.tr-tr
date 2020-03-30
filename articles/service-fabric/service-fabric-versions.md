---
title: Azure Hizmet Kumaşı'nda desteklenen küme sürümleri
description: Service Fabric ekibi blogundaki en yeni sürümlere bağlantı da dahil olmak üzere Azure Hizmet Kumaşı'ndaki küme sürümleri hakkında bilgi edinin.
ms.topic: troubleshooting
ms.date: 03/02/2020
ms.openlocfilehash: 1158266857f0864ed2f442edb100032c70063719
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385053"
---
# <a name="supported-service-fabric-versions"></a>Desteklenen Hizmet Kumaşı sürümleri

Kümenizin her zaman desteklenen bir Azure Hizmet Kumaşı sürümü çalıştığından emin olun. Service Fabric'in yeni bir sürümünün piyasaya sürülüğünü duyurduktan en az 60 gün sonra, önceki sürüm için destek sona erer. [Service Fabric takım blogunda](https://azure.microsoft.com/updates/?product=service-fabric)yeni sürümduyurularını bulabilirsiniz.

Kümenizin desteklenen Bir Hizmet Kumaşı sürümünü çalıştırmayı nasıl sürdüreceklerine ilişkin ayrıntılar için aşağıdaki belgelere bakın:

- [Azure Hizmet Kumaşı kümesini yükseltme](service-fabric-cluster-upgrade.md)
- [Bağımsız Windows Server kümenizde çalışan Hizmet Dokusu sürümünü yükseltme](service-fabric-cluster-upgrade-windows-server.md)

## <a name="supported-versions"></a>Desteklenen sürümler

Aşağıdaki tabloda Hizmet Kumaşı sürümleri ve bunların destek bitiş tarihleri listelenebedilir.

| Kümede Servis Kumaş çalışma süresi | Doğrudan küme sürümünden yükseltilebilir |Uyumlu SDK veya NuGet paket sürümü | Destek sonu |
| --- | --- |--- | --- |
| 5.3.121'den önceki tüm küme sürümleri | 5.1.158.* |Sürüm 2.3'ten daha az veya eşit |20 Ocak 2017 |
| 5.3.* | 5.1.158.* |Sürüm 2.3'ten daha az veya eşit |Şubat 24, 2017 |
| 5.4.* | 5.1.158.* |Sürüm 2.4'ten daha az veya eşit |10 Mayıs 2017       |
| 5.5.* | 5.4.164.* |Sürüm 2.5'ten daha az veya eşit |Ağustos 10,2017    |
| 5.6.* | 5.4.164.* |Sürüm 2.6'dan daha az veya eşit |13 Ekim 2017   |
| 5.7.* | 5.4.164.* |Sürüm 2.7'den daha az veya eşit |15 Aralık 2017  |
| 6.0.* | 5.6.205.* |Sürüm 2.8'den daha az veya eşit |30 Mart 2018     |
| 6.1.* | 5.7.221.* |Sürüm 3.0'dan daha az veya eşit |Temmuz 15, 2018      |
| 6.2.* | 6.0.232.* |Sürüm 3.1'den daha az veya eşit |Ekim 26, 2018   |
| 6.3.* | 6.1.480.* |Sürüm 3.2'den daha az veya eşit |Mart 31, 2019  |
| 6.4.* | 6.2.301.* |Sürüm 3.3'ten daha az veya eşit |Eylül 15, 2019 |
| 6.5.* | 6.4.617.* |Sürüm 3.4'ten daha az veya eşit |1 Ağustos 2020 |
| 7.0.466.* | 6.4.664.* |Sürüm 4.0'dan daha az veya eşit|Geçerli sürüm, bu nedenle bitiş tarihi yok |
| 7.0.466.* | 6.5.* |Sürüm 4.0'dan daha az veya eşit|Geçerli sürüm, bu nedenle bitiş tarihi yok |
| 7.0.470.* | 7.0.466.* |Sürüm 4.0'dan daha az veya eşit |Geçerli sürüm, bu nedenle bitiş tarihi yok |

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda desteklenen Service Fabric sürümleri için desteklenen işletim sistemleri listelenir.

| İşletim sistemi | En erken desteklenen Service Fabric sürümü |
| --- | --- |
| Windows Server 2012 R2 | Tüm sürümler |
| Windows Server 2016 | Tüm sürümler |
| Windows Server 1709 | 6.0 |
| Windows Server 1803 | 6.4 |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6.0 |

## <a name="supported-version-names"></a>Desteklenen sürüm adları

Aşağıdaki tabloda Service Fabric'in sürüm adları ve bunların karşılık gelen sürüm numaraları listelenebedilir.

| Sürüm adı | Windows sürüm numarası | Linux sürüm numarası |
| --- | --- | --- |
| 5.3 RTO | 5.3.121.9494 | NA |
| 5.3 CU1 | 5.3.204.9494 | NA |
| 5.3 CU2 | 5.3.301.9590 | NA |
| 5.3 CU3 | 5.3.311.9590 | NA |
| 5.4 CU2 | 5.4.164.9494 | NA |
| 5.5 CU1 | 5.5.216.0    | NA |
| 5.5 CU2 | 5.5.219.0    | NA |
| 5.5 CU3 | 5.5.227.0    | NA |
| 5.5 CU4 | 5.5.232.0    | NA |
| 5.6 RTO | 5.6.204.9494 | NA |
| 5.6 CU2 | 5.6.210.9494 | NA |
| 5.6 CU3 | 5.6.220.9494 | NA |
| 5.7 RTO | 5.7.198.9494 | NA |
| 5.7 CU4 | 5.7.221.9494 | NA |
| 6.0 RTO | 6.0.211.9494 | 6.0.120.1 |
| 6.0 CU1 | 6.0.219.9494 | 6.0.127.1 |
| 6.0 CU2 | 6.0.232.9494 | 6.0.133.1 |
| 6.1 CU1 | 6.1.456.9494 | 6.1.183.1 |
| 6.1 CU2 | 6.1.467.9494 | 6.1.185.1 |
| 6.1 CU3 | 6.1.472.9494 | NA |
| 6.1 CU4 | 6.1.480.9494 | 6.1.187.1 |
| 6.2 RTO | 6.2.269.9494 | 6.2.184.1 | 
| 6.2 CU1 | 6.2.274.9494 | 6.2.191.1 |
| 6.2 CU2 | 6.2.283.9494 | 6.2.194.1 |
| 6.2 CU3 | 6.2.301.9494 | 6.2.199.1 |
| 6.3 RTO | 6.3.162.9494 | 6.3.119.1 |
| 6.3 CU1 | 6.3.176.9494 | 6.3.124.1 |
| 6.3 CU1 | 6.3.187.9494 | 6.3.129.1 |
| 6.4 RTO | 6.4.617.9590 | 6.4.625.1 |
| 6.4 CU2 | 6.4.622.9590 | NA |
| 6.4 CU3 | 6.4.637.9590 | 6.4.634.1 |
| 6.4 CU4 | 6.4.644.9590 | 6.4.639.1 |
| 6.4 CU5 | 6.4.654.9590 | 6.4.649.1 |
| 6.4 CU6 | 6.4.658.9590 | NA |
| 6.4 CU7 | 6.4.664.9590 | 6.4.661.1 |
| 6.4 CU8 | 6.4.670.9590 | NA |
| 6.5 RTO | 6.5.639.9590 | 6.5.435.1 |
| 6.5 CU1 | 6.5.641.9590 | 6.5.454.1 |
| 6.5 CU2 | 6.5.658.9590 | 6.5.460.1 |
| 6.5 CU3 | 6.5.664.9590 | 6.5.466.1 |
| 6.5 CU5 | 6.5.676.9590 | 6.5.467.1 |
| 7.0 RTO | 7.0.457.9590 | 7.0.457.1 |
| 7.0 CU2 | 7.0.464.9590 | 7.0.464.1 |
| 7.0 CU3 | 7.0.466.9590 | 7.0.465.1 |
| 7.0 CU4 | 7.0.470.9590 | 7.0.469.1 |
