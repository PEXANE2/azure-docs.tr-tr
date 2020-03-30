---
title: KVSActorStateProvider ayarlarını değiştir
description: Azure Service Fabric'i kvsactorStateProvider türünün durum lu aktörlerini yapılandırma hakkında bilgi edinin.
author: sumukhs
ms.topic: conceptual
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: cdb115bd57cf3d5af4388f4efa03c2522feef9ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609783"
---
# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Güvenilir Aktörlerin Yapılandırılması--KVSActorStateProvider
Belirtilen aktör için Config klasörü altında Microsoft Visual Studio paket kökünde oluşturulan settings.xml dosyasını değiştirerek KVSActorStateProvider varsayılan yapılandırmasını değiştirebilirsiniz.

Azure Hizmet Kumaşı çalışma süresi, settings.xml dosyasında önceden tanımlanmış bölüm adlarını arar ve temel çalışma zamanı bileşenlerini oluştururken yapılandırma değerlerini tüketir.

> [!NOTE]
> Visual **not** Studio çözümünde oluşturulan settings.xml dosyasında aşağıdaki yapılandırmaların bölüm adlarını silmeyin veya değiştirmeyin.
> 
> 

## <a name="replicator-security-configuration"></a>Çoğalıcı güvenlik yapılandırması
Çoğaltma güvenlik yapılandırmaları çoğaltma sırasında kullanılan iletişim kanalını güvence altına almak için kullanılır. Bu, hizmetlerin birbirlerinin çoğaltma trafiğini göremeyeceği ve yüksek oranda kullanılabilir hale getirilen verilerin de güvenli olmasını sağladığı anlamına gelir.
Varsayılan olarak, boş bir güvenlik yapılandırma bölümü çoğaltma güvenliğini engeller.

> [!IMPORTANT]
> Linux düğümlerinde sertifikalar PEM biçimlendirilmiş olmalıdır. Linux için sertifikaları bulma ve yapılandırma hakkında daha fazla bilgi edinmek için [bkz.](./service-fabric-configure-certificates-linux.md) 
> 

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Çoğaltıcı yapılandırması
Çoğalıcı yapılandırmaları, Aktör Durum Sağlayıcısı durumunu son derece güvenilir hale getirmekten sorumlu çoğaltıcıyı yapılandırır.
Varsayılan yapılandırma Visual Studio şablonu tarafından oluşturulur ve yeterli olmalıdır. Bu bölümde çoğaltıcıyı ayarlamak için kullanılabilen ek yapılandırmalar hakkında konuşuyor.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Yapılandırma adları
| Adı | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Saniye |0.015 |Birincil bir bildirim göndermeden önce ikincil çoğaltıcı için bekleme süresi. Bu aralıkta işlenen işlemler için gönderilecek diğer bildirimler tek bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |Yok |Varsayılan değil-- gerekli parametre |Birincil/ikincil çoğaltıcının çoğaltma kümesindeki diğer çoğalıcılarla iletişim kurmak için kullanacağı IP adresi ve bağlantı noktası. Bu, hizmet bildiriminde bir TCP kaynak bitiş noktasına başvurulmalıdır. Hizmet [bildiriminde](service-fabric-service-manifest-resources.md) uç nokta kaynaklarını tanımlama hakkında daha fazla bilgi için Hizmet bildirimi kaynaklarına bakın. |
| RetryInterval |Saniye |5 |Çoğaltıcı, bir işlem için onay almazsa, iletiyi yeniden ilettiği zaman dilimi. |
| MaxReplicationMessageSize |Bayt |50 MB |Tek bir iletide aktarılabilen çoğaltma verilerinin maksimum boyutu. |
| MaxPrimaryReplicationQueueSize |Operasyon sayısı |1024 |Birincil sırada maksimum işlem sayısı. Birincil çoğalıcı tüm ikincil çoğalıcılardan onay aldıktan sonra bir işlem serbest bırakılır. Bu değer 64'ten büyük ve 2'den büyük olmalıdır. |
| MaxSecondaryReplicationQueueSize |Operasyon sayısı |2048 |İkincil sırada maksimum işlem sayısı. Bir işlem, durumunu son derece kullanılabilir hale getirdikten sonra, ısrarla serbest bırakılır. Bu değer 64'ten büyük ve 2'den büyük olmalıdır. |

## <a name="store-configuration"></a>Mağaza yapılandırması
Mağaza yapılandırmaları, çoğaltılan durumu sürdürmek için kullanılan yerel depoyu yapılandırmak için kullanılır.
Varsayılan yapılandırma Visual Studio şablonu tarafından oluşturulur ve yeterli olmalıdır. Bu bölümde, yerel mağazayı ayarlamak için kullanılabilen ek yapılandırmalardan bahsedilir.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Yapılandırma adları
| Adı | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| MaxAsyncCommitDelayInMilliseconds |Milisaniye |200 |Dayanıklı yerel mağaza taahhütleri için maksimum toplu işlem aralığını ayarlar. |
| MaxverPages |Sayfa sayısı |16384 |Yerel mağaza veritabanındaki en fazla sürüm sayfası sayısı. En fazla bekleyen hareket sayısını belirler. |

## <a name="sample-configuration-file"></a>Örnek yapılandırma dosyası
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```
## <a name="remarks"></a>Açıklamalar
BatchAcknowledgementInterval parametresi çoğaltma gecikmesini denetler. '0' değeri, iş maliyeti yle mümkün olan en düşük gecikme gecikmesi ile sonuçlanır (her biri daha az onay içeren daha fazla onay iletisi gönderilmesi ve işlenmesi gerektiğinden).
BatchAcknowledgementInterval için daha büyük değer, daha yüksek işlem gecikmesi pahasına, genel çoğaltma iş maliyeti daha yüksek. Bu doğrudan işlem taahhütlerinin gecikmesine çevirir.

