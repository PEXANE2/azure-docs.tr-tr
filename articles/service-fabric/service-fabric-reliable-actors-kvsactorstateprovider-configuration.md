---
title: KVSActorStateProvider ayarlarını değiştir
description: KVSActorStateProvider türünün Azure Service Fabric durum bilgisi olan aktörlerini yapılandırma hakkında bilgi edinin.
author: sumukhs
ms.topic: conceptual
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: cdb115bd57cf3d5af4388f4efa03c2522feef9ca
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75609783"
---
# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Reliable Actors--KVSActorStateProvider 'ı yapılandırma
Belirtilen aktör için yapılandırma klasörü altındaki Microsoft Visual Studio paketi kökünde oluşturulan settings.xml dosyasını değiştirerek, KVSActorStateProvider 'ın varsayılan yapılandırmasını değiştirebilirsiniz.

Azure Service Fabric Runtime, settings.xml dosyasında önceden tanımlanmış bölüm adlarını arar ve temel alınan çalışma zamanı bileşenlerini oluştururken yapılandırma değerlerini kullanır.

> [!NOTE]
> Visual Studio çözümünde oluşturulan settings.xml dosyasında aşağıdaki yapılandırmaların bölüm **adlarını silmeyin veya** değiştirmeyin.
> 
> 

## <a name="replicator-security-configuration"></a>Çoğaltıcı güvenlik yapılandırması
Çoğaltıcı güvenlik yapılandırması, çoğaltma sırasında kullanılan iletişim kanalının güvenliğini sağlamak için kullanılır. Bu, hizmetlerin, yüksek oranda kullanılabilir hale getirilen verilerin da güvenli olmasını sağlamak için her birinin çoğaltma trafiğini göremeyeceği anlamına gelir.
Varsayılan olarak, boş bir güvenlik yapılandırması bölümü, çoğaltma güvenliğini engeller.

> [!IMPORTANT]
> Linux düğümlerinde, sertifikaların ped biçimli olması gerekir. Linux için sertifikaları bulma ve yapılandırma hakkında daha fazla bilgi edinmek için bkz. [Linux 'ta sertifikaları yapılandırma](./service-fabric-configure-certificates-linux.md). 
> 

### <a name="section-name"></a>Bölüm adı
&lt;ActorName &gt; servicerepereptorsecurityconfig

## <a name="replicator-configuration"></a>Çoğaltıcı yapılandırması
Çoğaltıcı yapılandırması, aktör durum sağlayıcısı durumunu yüksek düzeyde güvenilir hale getirmekten sorumlu olan çoğaltıcısını yapılandırır.
Varsayılan yapılandırma, Visual Studio şablonu tarafından oluşturulur ve yeterli olacaktır. Bu bölüm, çoğaltıcının ayarlanmasından kullanılabilecek ek yapılandırmaların ele geçirilmesini sağlar.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName &gt; servicerepereptorconfig

### <a name="configuration-names"></a>Yapılandırma adları
| Name | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| Batchval Gementınterval |Saniye |0,015 |Birincili çoğaltıcının, birincil öğesine bir onay göndermeden önce bir işlem aldıktan sonra beklediği zaman aralığı. Bu Aralık dahilinde işlenen işlemler için gönderilecek diğer tüm onaylar bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |YOK |Varsayılan değer yok--gerekli parametre |Birincil/ikincil çoğaltıcıların, çoğaltma kümesindeki diğer çoğaltıcılar ile iletişim kurmak için kullanacağı IP adresi ve bağlantı noktası. Bu, hizmet bildiriminde bir TCP kaynak uç noktasına başvurmalıdır. Hizmet bildiriminde uç nokta kaynakları tanımlama hakkında daha fazla bilgi edinmek için [hizmet bildirimi kaynaklarına](service-fabric-service-manifest-resources.md) bakın. |
| RetryInterval |Saniye |5 |Bir işlem için onay almazsa, Replicator 'ın bir iletiyi yeniden ilettiği zaman aralığı. |
| MaxReplicationMessageSize |Bayt |50 MB |Tek bir iletide iletilebilecek en büyük çoğaltma verileri boyutu. |
| MaxPrimaryReplicationQueueSize |İşlem sayısı |1024 |Birincil kuyruktaki en fazla işlem sayısı. Birincil çoğaltıcı tüm ikincil replica'lerden onay aldıktan sonra bir işlem serbest bırakılır. Bu değer 64 ' den büyük ve 2 ' nin üssü olmalıdır. |
| MaxSecondaryReplicationQueueSize |İşlem sayısı |2048 |İkincil kuyruktaki en fazla işlem sayısı. Bir işlem, durumu Kalıcılık aracılığıyla yüksek oranda kullanılabilir olduktan sonra serbest bırakılır. Bu değer 64 ' den büyük ve 2 ' nin üssü olmalıdır. |

## <a name="store-configuration"></a>Mağaza yapılandırması
Depo yapılandırması, çoğaltılan durumu kalıcı hale getirmek için kullanılan yerel depoyu yapılandırmak için kullanılır.
Varsayılan yapılandırma, Visual Studio şablonu tarafından oluşturulur ve yeterli olacaktır. Bu bölümde, yerel depoyu ayarlamak için kullanılabilen ek konfigürasyonlar ele alınır.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName &gt; servicelocalstoreconfig

### <a name="configuration-names"></a>Yapılandırma adları
| Name | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| Maxasynccommitdelayınmilliseconds |Mayacak |200 |Dayanıklı yerel depo Yürütmelerinde en fazla toplu işlem aralığını ayarlar. |
| MaxVerPages |Sayfa sayısı |16384 |Yerel depo veritabanında en fazla sürüm sayfası sayısı. Bekleyen işlem sayısı üst sınırını belirler. |

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
Batchbildirimlerin Gementınterval parametresi, çoğaltma gecikmesini denetler. ' 0 ' değeri, aktarım hızına (daha az sayıda bildirim içeren daha fazla onay iletisi gönderilmesi ve işlenmek üzere) karşı olası en düşük gecikme süresine neden olur.
Batchval Gementınterval değeri arttıkça, daha yüksek işlem gecikmesi maliyetinde genel çoğaltma üretilen işi artar. Bu, doğrudan işlem yürütmelerinin gecikmesini dönüştürür.

