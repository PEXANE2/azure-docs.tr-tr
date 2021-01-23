---
title: Azure Service Fabric 'de desteklenen küme sürümleri
description: Service Fabric ekibi blogundan en yeni sürümlere bağlantı dahil olmak üzere Azure Service Fabric 'deki küme sürümleri hakkında bilgi edinin.
ms.topic: troubleshooting
ms.date: 06/15/2020
ms.openlocfilehash: 82b90d6b0acf5d83f9509abd9ee4b34cb74d1b48
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98736793"
---
# <a name="supported-service-fabric-versions"></a>Desteklenen Service Fabric sürümleri

Kümenizin her zaman desteklenen bir Azure Service Fabric sürümünü çalıştırdığından emin olun. Yeni bir Service Fabric sürümünün yayınlanmasıyla ilgili en az 60 gün sonra, önceki sürüm için destek sona erer. [Service Fabric ekip bloguna](https://azure.microsoft.com/updates/?product=service-fabric)yeni yayınlar hakkında duyurular bulacaksınız.

Service Fabric çalışma zamanının belirli bir sürümü için SDK/NuGet paketlerinin belirtilen veya eski sürümlerini kullanabilirsiniz. Paketlerin daha yeni sürümleri desteklenmez ve bu ortamlar tarafından desteklenmeyen özellik veya protokol değişikliklerine sahip olabileceği için eski kümeleri hedefleme sorunları olabilir.

Kümenizin desteklenen bir Service Fabric sürümünü çalıştırmasını sağlama hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure Service Fabric kümesini yükseltme](service-fabric-cluster-upgrade.md)
- [Tek başına Windows Server kümenizde çalışan Service Fabric sürümünü yükseltme](service-fabric-cluster-upgrade-windows-server.md)


## <a name="unsupported-versions"></a>Desteklenmeyen sürümler

### <a name="upgrade-alert-for-versions-between-57-and-below-6363"></a>5,7 ve altındaki sürümler için yükseltme uyarısı 6.3.63. *
Güvenliği ve kullanılabilirliği artırmak için Azure altyapısı, Service Fabric müşterileri etkileyebilecek bir değişiklik yapar. **Tüm Service Fabric kümeler 5,7 ' den 6,3 ' e kadar desteklenmeyen sürümlerde. etkilendi**. Değişikliğin ele alındığı Service Fabric çalışma zamanına yönelik bir güncelleştirme gerekir. Bu, tüm bölgelerde desteklenen tüm Service Fabric sürümleri için zaten kullanılabilir.

Bir destek planınız varsa ve teknik yardıma ihtiyacınız varsa, Azure Service Fabric için bir destek isteği açıp destek bileti ' nde bu bağlamdan bahsedin **,** Azure destek kanalları üzerinden bizimle iletişime geçin ve bunu yapmanız önerilir.

#### <a name="impact-if-not-upgraded-to-supported-versions"></a>Desteklenen sürümlere yükseltilmeyen etki

**5,7 ' den 6.3.63 ' e kadar desteklenmeyen sürümlerde çalışan Azure Service Fabric kümeleri. \*** , 19 Ocak 2021 tarafından desteklenen sürümlerden birine yükseltmediyse, bu duruma gelmeyecektir.

#### <a name="required-action"></a>Gerekli eylem
Bu değişiklik ile ilgili kapalı kalma süresini veya işlevsellik kaybını engellemek için aşağıda listelenen Service Fabric desteklenen sürümlere yükseltin. Ortamınızdaki sorunları engellemek için lütfen kümelerinizin en az bu sürümde çalıştığından emin olun.

  ###### <a name="supported-service-fabric-runtime-versions"></a>Desteklenen Service Fabric çalışma zamanı sürümleri
   Service Fabric aşağıda listelenen desteklenen sürümler üzerinde değilseniz, lütfen küme kesintilerine engel olmak için gerekli değişiklikleri içeren bu sürümlerden birine yükseltin. **Note:** Tüm 7,2 sürüm sürümleri, gerekli değişiklikleri içerir.
  
  | İşletim Sistemi | Kümedeki geçerli Service Fabric çalışma zamanı | CU/Patch yayını  | 
  | --- | --- |--- | 
  | Windows | 7,0. * | 7.0.478.9590 |
  | Windows | 7,1. * | 7.1.503.9590 |
  | Windows | 7,2. * | 7,2. * |
  | Ubuntu 16 | 7,0. * | 7.0.472.1  |
  | Linux Ubuntu 16,04 | 7,1. * | 7.1.455.1  |
  | Linux Ubuntu 18,04 | 7,1. * | 7.1.455.1804 |
  | Linux Ubuntu 16,04 | 7,2. * | 7,2. * |
  | Linux Ubuntu 18,04 | 7,2. * | 7,2. * |
 
### <a name="upgrade-alert-for-versions-greater-than-63"></a>6,3 'den büyük sürümler için yükseltme uyarısı 
Güvenliği ve kullanılabilirliği artırmak için Azure altyapısı, Service Fabric müşterileri etkileyebilecek bir değişiklik yapar. **[Kapsayıcılar Için açık ağ özelliğini](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode)kullanan tüm Service Fabric kümeleri, desteklenmeyen sürümlerde 6,3 'den büyük ve 7,0 ' den büyük ve 7,0 ve sonraki sürümlerde Desteklenen sürümlerin etkilenmesidir**. Değişikliğin ele alındığı Service Fabric çalışma zamanına yönelik bir güncelleştirme gerekir. Bu, tüm bölgelerde desteklenen tüm Service Fabric sürümleri için zaten kullanılabilir.

 #### <a name="impact-if-not-upgraded-to-supported-versions"></a>Desteklenen sürümlere yükseltilmeyen etki
  Kapsayıcılar için **[açık ağ özelliği özelliğini](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode) kullanan** Azure Service Fabric kümeleri **, 6,3 Ocak 2021** tarafından desteklenen sürümlerden birine yükseltilmeyen işlevler veya hizmet kesintilerinin kaybolması ile karşılaşacaktır.
 
  - **6,3 ' den büyük bir Service Fabric çalıştıran kümeler Için açık ağ ÖZELLIĞI kullanmayın**, küme çalışır durumda kalır.

 - **6,3 ' den büyük Service Fabric bir sürümünü çalıştıran kümeler ve [kapsayıcılar Için açık ağ özelliğini](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode) kullanmak için** , küme kullanılamaz hale gelebilir ve iş yükleriniz için hizmet kesintilerine neden olabilecek çalışmayı durduracaktır.
 
 -   **[7,0 ile 7.0.466 arasında Windows sürümlerini çalıştıran kümeler (her iki sürüm dahil)](https://docs.microsoft.com/azure/service-fabric/service-fabric-versions#supported-version-names) ve Windows Işletim sistemi Windows kapsayıcıları özelliği etkinleştirilmiştir. Not: Linux sürümleri 7.0.457, 7.0.464 ve 7.0.465 etkilenmez**.
    - **Etki**: küme, iş yükleriniz için hizmet kesintilerine neden olabilecek çalışmayı durduracaktır.
       
#### <a name="required-action"></a>Gerekli eylem
Bu değişiklik ile ilgili kapalı kalma süresini veya işlevsellik kaybını engellemek için aşağıda listelenen Service Fabric desteklenen sürümlere yükseltin. Ortamınızdaki sorunları engellemek için lütfen kümelerinizin en az bu sürümde çalıştığından emin olun. 
 
 ###### <a name="supported-service-fabric-runtime-versions"></a>Desteklenen Service Fabric çalışma zamanı sürümleri
 Service Fabric aşağıda listelenen desteklenen sürümler üzerinde değilseniz, lütfen işlev kaybını engellemek için gerekli değişiklikleri içeren bu sürümlerden birine yükseltin.  **Note:** Tüm 7,2 sürüm sürümleri, gerekli değişiklikleri içerir.
 
  | İşletim Sistemi | Kümedeki geçerli Service Fabric çalışma zamanı | CU/Patch yayını  | 
  | --- | --- |--- | 
  | Windows | 7,0. * | 7.0.478.9590 |
  | Windows | 7,1. * | 7.1.503.9590 |
  | Windows | 7,2. * | 7,2. * |
  | Linux Ubuntu 16,04 | 7,0. * | 7.0.472.1  |
  | Linux Ubuntu 16,04 | 7,1. * | 7.1.455.1  |
  | Linux Ubuntu 18,04 | 7,1. * | 7.1.455.1804 |
  | Linux Ubuntu 16,04 | 7,2. * | 7,2. * |
  | Linux Ubuntu 18,04 | 7,2. * | 7,2. * |

## <a name="supported-versions"></a>Desteklenen sürümler
Aşağıdaki tabloda Service Fabric sürümleri ve destek bitiş tarihleri listelenmektedir.

| Kümede Service Fabric çalışma zamanı | Doğrudan küme sürümünden yükseltebilir |Uyumlu SDK veya NuGet paketi sürümü | Destek sonu |
| --- | --- |--- | --- |
| 5.3.121 önceki tüm küme sürümleri | 5.1.158.* |Sürüm 2,3 ' den küçük veya buna eşit |20 Ocak 2017 |
| 5,3. * | 5.1.158.* |Sürüm 2,3 ' den küçük veya buna eşit |24 Şubat 2017 |
| 5,4. * | 5.1.158.* |Sürüm 2,4 ' den küçük veya buna eşit |10 Mayıs 2017       |
| 5,5. * | 5.4.164.* |Sürüm 2,5 ' den küçük veya buna eşit |10 Ağustos 2017    |
| 5,6. * | 5.4.164.* |Sürüm 2,6 ' den küçük veya buna eşit |13 Ekim 2017   |
| 5,7. * | 5.4.164.* |Sürüm 2,7 ' den küçük veya buna eşit |15 Aralık 2017  |
| 6,0. * | 5.6.205.* |Sürüm 2,8 ' den küçük veya buna eşit |30 Mart 2018     |
| 6,1. * | 5.7.221.* |Sürüm 3,0 ' den küçük veya buna eşit |15 Temmuz 2018      |
| 6,2. * | 6.0.232.* |Sürüm 3,1 ' den küçük veya buna eşit |26 Ekim 2018   |
| 6,3. * | 6.1.480.* |Sürüm 3,2 ' den küçük veya buna eşit |31 Mart 2019  |
| 6,4. * | 6.2.301.* |Sürüm 3,3 ' den küçük veya buna eşit |15 Eylül 2019 |
| 6,5. * | 6.4.617.* |Sürüm 3,4 ' den küçük veya buna eşit |1 Ağustos 2020 |
| 7.0.466.* | 6.4.664.* |Sürüm 4,0 ' den küçük veya buna eşit|31 Ocak 2021  |
| 7.0.466.* | 6,5. * |Sürüm 4,0 ' den küçük veya buna eşit|31 Ocak 2021 |
| 7.0.470.* | 7.0.466.* |Sürüm 4,0 ' den küçük veya buna eşit |31 Ocak 2021  |
| 7.0.472.* | 7.0.466.* |Sürüm 4,0 ' den küçük veya buna eşit |31 Ocak 2021  |
| 7.0.478.* | 7.0.466.* |Sürüm 4,0 ' den küçük veya buna eşit |31 Ocak 2021  |
| 7.1.409.* | 7.0.466.* |Sürüm 4,1 ' den küçük veya buna eşit |31 Mart 2021 |
| 7.1.417.* | 7.0.466.* |Sürüm 4,1 ' den küçük veya buna eşit |31 Mart 2021 |
| 7.1.428.* | 7.0.466.* |Sürüm 4,1 ' den küçük veya buna eşit |31 Mart 2021 |
| 7.1.456.* | 7.0.466.* |Sürüm 4,1 ' den küçük veya buna eşit |31 Mart 2021 |
| 7.1.458.* | 7.0.466.* |Sürüm 4,1 ' den küçük veya buna eşit |31 Mart 2021 |
| 7.1.459.* | 7.0.466.* |Sürüm 4,1 ' den küçük veya buna eşit |31 Mart 2021 |
| 7.1.503.* | 7.0.466.* |Sürüm 4,1 ' den küçük veya buna eşit |31 Mart 2021 |
| 7.2.413.* | 7.0.470.* |Sürüm 4,2 ' den küçük veya buna eşit |Geçerli sürüm, bu nedenle bitiş tarihi yok |
| 7.2.432.* | 7.0.470.* |Sürüm 4,2 ' den küçük veya buna eşit |Geçerli sürüm, bu nedenle bitiş tarihi yok |
| 7.2.433.* | 7.0.470.* |Sürüm 4,2 ' den küçük veya buna eşit |Geçerli sürüm, bu nedenle bitiş tarihi yok |
| 7.2.445.* | 7.0.470.* |Sürüm 4,2 ' den küçük veya buna eşit |Geçerli sürüm, bu nedenle bitiş tarihi yok |

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda desteklenen Service Fabric sürümleri için desteklenen işletim sistemleri listelenmektedir.

| İşletim sistemi | Desteklenen en erken Service Fabric sürümü |
| --- | --- |
| Windows Server 2012 R2 | Tüm sürümler |
| Windows Server 2016 | Tüm sürümler |
| Windows Server 1709 | 6.0 |
| Windows Server 1803 | 6.4 |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16,04 | 6.0 |
| Linux Ubuntu 18,04 | 7.1 |

## <a name="supported-version-names"></a>Desteklenen sürüm adları

Aşağıdaki tabloda Service Fabric sürüm adları ve bunlara karşılık gelen sürüm numaraları listelenmektedir.

| Sürüm adı | Windows sürüm numarası | Linux sürüm numarası |
| --- | --- | --- |
| 5,3 RTO | 5.3.121.9494 | NA |
| 5,3 CU1 | 5.3.204.9494 | NA |
| 5,3 CU2 UYGULAMAZSANıZ | 5.3.301.9590 | NA |
| 5,3 CU3 | 5.3.311.9590 | NA |
| 5,4 CU2 UYGULAMAZSANıZ | 5.4.164.9494 | NA |
| 5,5 CU1 | 5.5.216.0    | NA |
| 5,5 CU2 UYGULAMAZSANıZ | 5.5.219.0    | NA |
| 5,5 CU3 | 5.5.227.0    | NA |
| 5,5 CU4 | 5.5.232.0 | NA |
| 5,6 RTO | 5.6.204.9494 | NA |
| 5,6 CU2 UYGULAMAZSANıZ | 5.6.210.9494 | NA |
| 5,6 CU3 | 5.6.220.9494 | NA |
| 5,7 RTO | 5.7.198.9494 | NA |
| 5,7 CU4 | 5.7.221.9494 | NA |
| 6,0 RTO | 6.0.211.9494 | 6.0.120.1 |
| 6,0 CU1 | 6.0.219.9494 | 6.0.127.1 |
| 6,0 CU2 UYGULAMAZSANıZ | 6.0.232.9494 | 6.0.133.1 |
| 6,1 CU1 | 6.1.456.9494 | 6.1.183.1 |
| 6,1 CU2 UYGULAMAZSANıZ | 6.1.467.9494 | 6.1.185.1 |
| 6,1 CU3 | 6.1.472.9494 | NA |
| 6,1 CU4 | 6.1.480.9494 | 6.1.187.1 |
| 6,2 RTO | 6.2.269.9494 | 6.2.184.1 | 
| 6,2 CU1 | 6.2.274.9494 | 6.2.191.1 |
| 6,2 CU2 UYGULAMAZSANıZ | 6.2.283.9494 | 6.2.194.1 |
| 6,2 CU3 | 6.2.301.9494 | 6.2.199.1 |
| 6,3 RTO | 6.3.162.9494 | 6.3.119.1 |
| 6,3 CU1 | 6.3.176.9494 | 6.3.124.1 |
| 6,3 CU1 | 6.3.187.9494 | 6.3.129.1 |
| 6,4 RTO | 6.4.617.9590 | 6.4.625.1 |
| 6,4 CU2 UYGULAMAZSANıZ | 6.4.622.9590 | NA |
| 6,4 CU3 | 6.4.637.9590 | 6.4.634.1 |
| 6,4 CU4 | 6.4.644.9590 | 6.4.639.1 |
| 6,4 CU5 | 6.4.654.9590 | 6.4.649.1 |
| 6,4 CU6 | 6.4.658.9590 | NA |
| 6,4 CU7 | 6.4.664.9590 | 6.4.661.1 |
| 6,4 CU8 | 6.4.670.9590 | NA |
| 6,5 RTO | 6.5.639.9590 | 6.5.435.1 |
| 6,5 CU1 | 6.5.641.9590 | 6.5.454.1 |
| 6,5 CU2 UYGULAMAZSANıZ | 6.5.658.9590 | 6.5.460.1 |
| 6,5 CU3 | 6.5.664.9590 | 6.5.466.1 |
| 6,5 CU5 | 6.5.676.9590 | 6.5.467.1 |
| 7,0 RTO | 7.0.457.9590 | 7.0.457.1 |
| 7,0 CU2 UYGULAMAZSANıZ | 7.0.464.9590 | 7.0.464.1 |
| 7,0 CU3 | 7.0.466.9590 | 7.0.465.1 |
| 7,0 CU4 | 7.0.470.9590 | 7.0.469.1 |
| 7,0 CU6 | 7.0.472.9590 | 7.0.471.1 |
| 7,0 CU9 | 7.0.478.9590 | 7.0.472.1 |
| 7,1 RTO | 7.1.409.9590 | 7.1.410.1 |
| 7,1 CU1 | 7.1.417.9590 | 7.1.418.1 |
| 7,1 CU2 UYGULAMAZSANıZ | 7.1.428.9590 | 7.1.428.1 |
| 7,1 CU3 | 7.1.456.9590 | 7.1.452.1 |
| 7,1 CU5 | 7.1.458.9590 | 7.1.454.1 |
| 7,1 CU6 | 7.1.459.9590 | 7.1.455.1 |
| 7,1 CU8 | 7.1.503.9590 | 7.1.508.1 |
| 7,2 RTO | 7.2.413.9590 | NA |
| 7,2 CU2 UYGULAMAZSANıZ | 7.2.432.9590 | 7.2.431.1 |
| 7,2 CU3 | 7.2.433.9590 | NA |
| 7,2 CU4 | 7.2.445.9590 | 7.2.447.1 |

