---
title: ReliableDictionaryActorStateProvider ayarlarını değiştir
description: ReliableDictionaryActorStateProvider türünde Azure Service Fabric durum bilgisi olan aktörleri yapılandırma hakkında bilgi edinin.
author: sumukhs
ms.topic: conceptual
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: fbd6f7cd3ade753c659464522408aa715cce48f9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75609749"
---
# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Reliable Actors yapılandırma--ReliableDictionaryActorStateProvider
Belirtilen aktör için yapılandırma klasörü altındaki Visual Studio paket kökünde oluşturulan Settings. xml dosyasını değiştirerek, ReliableDictionaryActorStateProvider 'ın varsayılan yapılandırmasını değiştirebilirsiniz.

Azure Service Fabric Runtime, Settings. xml dosyasında önceden tanımlanmış bölüm adlarını arar ve temel alınan çalışma zamanı bileşenlerini oluştururken yapılandırma değerlerini kullanır.

> [!NOTE]
> Visual Studio çözümünde oluşturulan Settings. xml dosyasında aşağıdaki yapılandırmaların bölüm **adlarını silmeyin veya** değiştirmeyin.
> 
> 

ReliableDictionaryActorStateProvider yapılandırmasını etkileyen genel ayarlar da vardır.

## <a name="global-configuration"></a>Genel yapılandırma
Genel yapılandırma, Ktlgünlükçü bölümünün altındaki kümenin küme bildiriminde belirtilir. Paylaşılan günlük konumunun ve boyutunun ve günlük kaydı tarafından kullanılan genel bellek sınırlarının yapılandırılmasına izin verir. Küme bildirimindeki değişikliklerin, ReliableDictionaryActorStateProvider ve güvenilir durum bilgisi olan hizmetler kullanan tüm hizmetleri etkilediğini unutmayın.

Küme bildirimi, kümedeki tüm düğümlere ve hizmetlere uygulanan ayarları ve konfigürasyonları tutan tek bir XML dosyasıdır. Dosya genellikle ClusterManifest. xml olarak adlandırılır. Get-ServiceFabricClusterManifest PowerShell komutunu kullanarak kümenizin küme bildirimini görebilirsiniz.

### <a name="configuration-names"></a>Yapılandırma adları
| Adı | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| Writebuffermemorypoolminimumınkb |KB |8388608 |Günlükçü yazma arabelleği bellek havuzu için çekirdek modunda ayrılacak en az KB sayısı. Bu bellek havuzu, diske yazmadan önce durum bilgilerini önbelleğe almak için kullanılır. |
| WriteBufferMemoryPoolMaximumInKB |KB |Sınırsız |Günlükçü yazma arabelleği bellek havuzunun büyüyebileceği maksimum boyut. |
| Sharedlogıd |GUID |"" |Kümedeki tüm güvenilir hizmetler tarafından, hizmet özel yapılandırmasında Sharedlogıd belirtmeyen tüm tüm düğümlerde kullanılan varsayılan paylaşılan günlük dosyasını tanımlamak için kullanılacak benzersiz bir GUID belirtir. Sharedlogıd belirtilmişse, SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam nitelikli yol adı |"" |Tüm güvenilir hizmetler tarafından, hizmet özel yapılandırmasında SharedLogPath belirtmeyen tüm düğümlerde, paylaşılan günlük dosyasının kullandığı tam yolu belirtir. Ancak SharedLogPath belirtilmişse Sharedlogıd de belirtilmelidir. |
| Sharedlogsizeınmb |TI |8192 |Paylaşılan günlük için statik olarak ayrılacak MB disk alanı sayısını belirtir. Değer 2048 veya daha büyük olmalıdır. |

### <a name="sample-cluster-manifest-section"></a>Örnek küme bildirim bölümü
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Açıklamalar
Günlükçü, güvenilir hizmet çoğaltmasıyla ilişkili ayrılmış günlüğe yazılmadan önce, durum verilerinin önbelleğe alınması için bir düğümdeki tüm güvenilir hizmetler tarafından kullanılabilen, disk belleğine alınmayan çekirdek bellekten ayrılan küresel bir bellek havuzuna sahiptir. Havuz boyutu Writebuffermemorypoolminimumınkb ve WriteBufferMemoryPoolMaximumInKB Settings tarafından denetlenir. Writebuffermemorypoolminimumınkb, bu bellek havuzunun başlangıç boyutunu ve bellek havuzunun küçülebileceği en düşük boyutu belirtir. WriteBufferMemoryPoolMaximumInKB, bellek havuzunun büyüyebileceği en yüksek boyutudur. Açık olan her güvenilir hizmet çoğaltması, bellek havuzunun boyutunu WriteBufferMemoryPoolMaximumInKB olan bir sistem tarafından belirlenen miktarda artırabilir. Bellek havuzundan kullanılabilir olandan daha fazla bellek talebi varsa bellek kullanılabilir olana kadar bellek istekleri gecikecek. Bu nedenle, yazma arabelleği bellek havuzu belirli bir yapılandırma için çok küçükse performans düşebilir.

Sharedlogıd ve SharedLogPath ayarları her zaman, kümedeki tüm düğümler için varsayılan paylaşılan günlük için GUID ve konum tanımlamak üzere birlikte kullanılır. Varsayılan paylaşılan günlük, belirli bir hizmet için Settings. xml dosyasında ayarları belirtmeyen tüm güvenilir hizmetler için kullanılır. En iyi performansı elde etmek için, paylaşılan günlük dosyaları yalnızca paylaşılan günlük dosyası için kullanılan disklere yerleştirilmelidir ve bu da çekişmeyi azaltır.

Sharedlogsizeınmb, tüm düğümlerde varsayılan paylaşılan günlük için önceden ayrılacak disk alanı miktarını belirtir.  Sharedlogsizeınmb 'nin belirtime sırası için Sharedlogıd ve SharedLogPath 'in belirtilmesi gerekmez.

## <a name="replicator-security-configuration"></a>Çoğaltıcı güvenlik yapılandırması
Çoğaltıcı güvenlik yapılandırması, çoğaltma sırasında kullanılan iletişim kanalının güvenliğini sağlamak için kullanılır. Bu, hizmetlerin, yüksek oranda kullanılabilir hale getirilen verilerin da güvenli olmasını sağlamak için her birinin çoğaltma trafiğini göremeyeceği anlamına gelir.
Varsayılan olarak, boş bir güvenlik yapılandırması bölümü, çoğaltma güvenliğini engeller.

> [!IMPORTANT]
> Linux düğümlerinde, sertifikaların ped biçimli olması gerekir. Linux için sertifikaları bulma ve yapılandırma hakkında daha fazla bilgi edinmek için bkz. [Linux 'ta sertifikaları yapılandırma](./service-fabric-configure-certificates-linux.md). 
> 

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;servicerepereptorsecurityconfig

## <a name="replicator-configuration"></a>Çoğaltıcı yapılandırması
Çoğaltıcı yapılandırması, durumu yerel olarak çoğaltırken ve kalıcı hale getirerek aktör durum sağlayıcısı durumunu yüksek düzeyde güvenilir hale getirmekten sorumlu olan çoğaltıcıyı yapılandırmak için kullanılır.
Varsayılan yapılandırma, Visual Studio şablonu tarafından oluşturulur ve yeterli olacaktır. Bu bölüm, çoğaltıcının ayarlanmasından kullanılabilecek ek yapılandırmaların ele geçirilmesini sağlar.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;servicerepereptorconfig

### <a name="configuration-names"></a>Yapılandırma adları
| Adı | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| Batchval Gementınterval |Saniye |0,015 |Birincili çoğaltıcının, birincil öğesine bir onay göndermeden önce bir işlem aldıktan sonra beklediği zaman aralığı. Bu Aralık dahilinde işlenen işlemler için gönderilecek diğer tüm onaylar bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |Yok |Varsayılan değer yok--gerekli parametre |Birincil/ikincil çoğaltıcıların, çoğaltma kümesindeki diğer çoğaltıcılar ile iletişim kurmak için kullanacağı IP adresi ve bağlantı noktası. Bu, hizmet bildiriminde bir TCP kaynak uç noktasına başvurmalıdır. Hizmet bildiriminde uç nokta kaynakları tanımlama hakkında daha fazla bilgi edinmek için [hizmet bildirimi kaynaklarına](service-fabric-service-manifest-resources.md) bakın. |
| MaxReplicationMessageSize |Bayt |50 MB |Tek bir iletide iletilebilecek en büyük çoğaltma verileri boyutu. |
| MaxPrimaryReplicationQueueSize |İşlem sayısı |8192 |Birincil kuyruktaki en fazla işlem sayısı. Birincil çoğaltıcı tüm ikincil replica'lerden onay aldıktan sonra bir işlem serbest bırakılır. Bu değer 64 ' den büyük ve 2 ' nin üssü olmalıdır. |
| MaxSecondaryReplicationQueueSize |İşlem sayısı |16384 |İkincil kuyruktaki en fazla işlem sayısı. Bir işlem, durumu Kalıcılık aracılığıyla yüksek oranda kullanılabilir olduktan sonra serbest bırakılır. Bu değer 64 ' den büyük ve 2 ' nin üssü olmalıdır. |
| CheckpointThresholdInMB |MB |200 |Durum denetim noktası olan günlük dosyası alanı miktarı. |
| Maxrecordsizeınkb |KB |1024 |Çoğaltıcının günlüğe yazabileceği en büyük kayıt boyutu. Bu değer 4 ' ün katı ve 16 ' dan büyük olmalıdır. |
| OptimizeLogForLowerDiskUsage |Boole |true |Doğru olduğunda, günlük, çoğaltmanın ayrılmış günlük dosyası bir NTFS seyrek dosya kullanılarak oluşturulacak şekilde yapılandırılır. Bu, dosya için gerçek disk alanı kullanımını düşürür. Yanlış olduğunda, dosya, en iyi yazma performansını sağlayan sabit ayırmalar ile oluşturulur. |
| Sharedlogıd |guid |"" |Bu çoğaltmayla kullanılan paylaşılan günlük dosyasını tanımlamak için kullanılacak benzersiz bir GUID belirtir. Genellikle, hizmetler bu ayarı kullanmamalıdır. Ancak, Sharedlogıd belirtilmişse, SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam nitelikli yol adı |"" |Bu çoğaltmanın paylaşılan günlük dosyasının oluşturulacağı tam yolu belirtir. Genellikle, hizmetler bu ayarı kullanmamalıdır. Ancak SharedLogPath belirtilmişse Sharedlogıd de belirtilmelidir. |

## <a name="sample-configuration-file"></a>Örnek yapılandırma dosyası
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
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

Checkpointthresholınvomb parametresi, Replicator 'ın, durum bilgilerini çoğaltmanın ayrılmış günlük dosyasında depolamak için kullanabileceği disk alanı miktarını denetler. Bunu varsayılandan daha yüksek bir değere yükseltmek, kümesine yeni bir çoğaltma eklendiğinde daha hızlı yeniden yapılandırma süreleriyle sonuçlanabilir. Bu, günlükteki daha fazla işlem geçmişinin kullanılabilirliği nedeniyle gerçekleşen kısmi durum aktarımından kaynaklanır. Bu, kilitlenme sonrasında bir çoğaltmanın kurtarma süresini artırabilir.

Optimizeforaldiskusage değerini doğru olarak ayarlarsanız, etkin çoğaltmaların günlük dosyalarında daha fazla durum bilgisi depolayabilmesi için günlük dosyası alanı kullanıma sunulacaktır, ancak etkin olmayan çoğaltmalar daha az disk alanı kullanacaktır. Bu, bir düğümde daha fazla çoğaltma barındırmanıza olanak tanır. Optimizeforaldiskusage değerini false olarak ayarlarsanız, durum bilgileri günlük dosyalarına daha hızlı yazılır.

Maxrecordsizeınkb ayarı, çoğaltıcının günlük dosyasına yazılabileceği en büyük bir kayıt boyutunu tanımlar. Çoğu durumda, varsayılan 1024 KB kayıt boyutu idealdir. Ancak, hizmet daha büyük veri öğelerinin durum bilgilerinin bir parçası olmasına neden oluyorsa, bu değerin artırılması gerekebilir. Daha küçük kayıtlar yalnızca daha küçük bir kayıt için gereken alanı kullanırken, 1024 'den daha küçük olan Maxrecordsizeınkb 'yi yaparken çok daha fazla avantaj vardır. Bu değerin yalnızca nadir durumlarda değiştirilmesi gerektiğini umuz.

Bir hizmetin, düğümün varsayılan paylaşılan günlüğünden ayrı bir paylaşılan günlük kullanmasını sağlamak için Sharedlogıd ve SharedLogPath ayarları her zaman birlikte kullanılır. En iyi verimlilik için, olabildiğince fazla hizmetin aynı paylaşılan günlüğü belirtmesi gerekir. Paylaşılan günlük dosyaları, baş hareket çekişmesini azaltmak için yalnızca paylaşılan günlük dosyası için kullanılan disklere yerleştirilmelidir. Bu değerlerin yalnızca nadir durumlarda değiştirilmesi gerektiğini umuz.

