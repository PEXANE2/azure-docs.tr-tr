---
title: GüvenilirDictionaryActorStateProvider ayarlarını değiştirin
description: Azure Hizmet Kumaşı'nı güvenilir dictionaryActorStateProvider türünde yapılandırma hakkında bilgi edinin.
author: sumukhs
ms.topic: conceptual
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: fbd6f7cd3ade753c659464522408aa715cce48f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609749"
---
# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Güvenilir Aktörlerin Yapılandırılması--ReliableDictionaryActorStateProvider
Belirtilen aktör için Config klasörü altında Visual Studio paket kökünde oluşturulan settings.xml dosyasını değiştirerek ReliableDictionaryActorStateProvider varsayılan yapılandırmasını değiştirebilirsiniz.

Azure Hizmet Kumaşı çalışma süresi, settings.xml dosyasında önceden tanımlanmış bölüm adlarını arar ve temel çalışma zamanı bileşenlerini oluştururken yapılandırma değerlerini tüketir.

> [!NOTE]
> Visual **not** Studio çözümünde oluşturulan settings.xml dosyasında aşağıdaki yapılandırmaların bölüm adlarını silmeyin veya değiştirmeyin.
> 
> 

ReliableDictionaryActorStateProvider yapılandırmasını etkileyen küresel ayarlar da vardır.

## <a name="global-configuration"></a>Küresel Yapılandırma
KtlLogger bölümünün altındaki küme için küme manifestosunda genel yapılandırma belirtilir. Paylaşılan günlük konumu ve boyutunun ve logger tarafından kullanılan genel bellek sınırlarının yapılandırmasına olanak tanır. Küme bildirimindeki değişikliklerin ReliableDictionaryActorStateProvider ve güvenilir durum hizmetleri kullanan tüm hizmetleri etkilediğini unutmayın.

Küme bildirimi, kümedeki tüm düğümler ve hizmetler için geçerli olan ayarları ve yapılandırmaları tutan tek bir XML dosyasıdır. Dosya genellikle ClusterManifest.xml olarak adlandırılır. Get-ServiceFabricClusterManifest powershell komutunu kullanarak kümeniz için küme bildirimini görebilirsiniz.

### <a name="configuration-names"></a>Yapılandırma adları
| Adı | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| YazmaBufferMemoryPoolMinimumInKB |Kilobayt |8388608 |Logger yazma arabellek bellek havuzu için çekirdek modunda ayırmak için kb en az sayısı. Bu bellek havuzu, diske yazmadan önce durum bilgilerini önbelleğe almak için kullanılır. |
| YazmaBufferMemoryPoolMaximumInKB |Kilobayt |Sınırsız |Logger yazma arabellek bellek havuzu büyüyebilir maksimum boyutu. |
| SharedLogId |GUID |"" |Hizmete özel yapılandırmalarında SharedLogId'i belirtmeyin kümedeki tüm düğümlerde tüm güvenilir hizmetler tarafından kullanılan varsayılan paylaşılan günlük dosyasını tanımlamak için kullanılacak benzersiz bir GUID belirtir. SharedLogId belirtilmişse, SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam nitelikli yol adı |"" |Cluster'daki tüm güvenilir hizmetler tarafından kullanılan ve hizmete özel yapılandırmalarında SharedLogPath'i belirtmeyen paylaşılan günlük dosyasının tam nitelikli yolu belirtir. Ancak, SharedLogPath belirtilmişse, SharedLogId de belirtilmelidir. |
| PaylaşılanLogSizeInMB |Megabayt |8192 |Paylaşılan günlük için statik olarak ayırmak için disk alanımb sayısını belirtir. Değeri 2048 veya daha büyük olmalıdır. |

### <a name="sample-cluster-manifest-section"></a>Örnek küme manifestosu bölümü
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
Kaydedici, güvenilir hizmet çoğaltma ile ilişkili özel günlük yazılmadan önce durum verilerini önbelleğe almak için bir düğüm üzerindeki tüm güvenilir hizmetler için kullanılabilir olmayan sayfalı çekirdek bellekten ayrılan genel bir bellek havuzuvardır. Havuz boyutu WriteBufferMemoryPoolMinimumInKB ve WriteBufferMemoryPoolMaximumInKB ayarları tarafından denetlenir. WriteBufferMemoryPoolMinimumInKB, hem bu bellek havuzunun başlangıç boyutunu hem de bellek havuzunun küçülebileceği en düşük boyutu belirtir. WriteBufferMemoryPoolMaximumInKB bellek havuzu büyüyebilir hangi en yüksek boyutudur. Açılan her güvenilir hizmet çoğaltma writeBufferMemoryPoolMaximumInKB kadar belirlenen bir sistem tarafından bellek havuzunun boyutunu artırabilir. Bellek havuzundan bellek için kullanılabilir den daha fazla talep varsa, bellek için istekler bellek kullanılabilir olana kadar geciktirilir. Bu nedenle, yazma arabelleği bellek havuzu belirli bir yapılandırma için çok küçükse, performans zarar görebilir.

SharedLogId ve SharedLogPath ayarları, kümedeki tüm düğümler için varsayılan paylaşılan günlük için GUID ve konumu tanımlamak için her zaman birlikte kullanılır. Varsayılan paylaşılan günlük, belirli bir hizmet için settings.xml'deki ayarları belirtmeyan tüm güvenilir hizmetler için kullanılır. En iyi performans için, paylaşılan günlük dosyaları, çekişmeyi azaltmak için yalnızca paylaşılan günlük dosyası için kullanılan disklere yerleştirilmelidir.

SharedLogSizeInMB, tüm düğümlerde varsayılan paylaşılan günlük için yer değiştirme için disk alanı miktarını belirtir.  SharedLogSizeINMB belirtilmesi için SharedLogId ve SharedLogPath belirtilmesi gerekmez.

## <a name="replicator-security-configuration"></a>Çoğalıcı güvenlik yapılandırması
Çoğaltma güvenlik yapılandırmaları çoğaltma sırasında kullanılan iletişim kanalını güvence altına almak için kullanılır. Bu, hizmetlerin birbirlerinin çoğaltma trafiğini göremeyeceği ve yüksek oranda kullanılabilir hale getirilen verilerin de güvenli olmasını sağladığı anlamına gelir.
Varsayılan olarak, boş bir güvenlik yapılandırma bölümü çoğaltma güvenliğini engeller.

> [!IMPORTANT]
> Linux düğümlerinde sertifikalar PEM biçimlendirilmiş olmalıdır. Linux için sertifikaları bulma ve yapılandırma hakkında daha fazla bilgi edinmek için [bkz.](./service-fabric-configure-certificates-linux.md) 
> 

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Çoğaltıcı yapılandırması
Çoğalıcı yapılandırmaları, Aktörün Devlet Sağlayıcı durumunu yerel olarak çoğaltarak ve kalıcı hale getirerek son derece güvenilir hale getirmekten sorumlu çoğaltıcıyı yapılandırmak için kullanılır.
Varsayılan yapılandırma Visual Studio şablonu tarafından oluşturulur ve yeterli olmalıdır. Bu bölümde çoğaltıcıyı ayarlamak için kullanılabilen ek yapılandırmalar hakkında konuşuyor.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Yapılandırma adları
| Adı | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Saniye |0.015 |Birincil bir bildirim göndermeden önce ikincil çoğaltıcı için bekleme süresi. Bu aralıkta işlenen işlemler için gönderilecek diğer bildirimler tek bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |Yok |Varsayılan değil-- gerekli parametre |Birincil/ikincil çoğaltıcının çoğaltma kümesindeki diğer çoğalıcılarla iletişim kurmak için kullanacağı IP adresi ve bağlantı noktası. Bu, hizmet bildiriminde bir TCP kaynak bitiş noktasına başvurulmalıdır. Hizmet [bildiriminde](service-fabric-service-manifest-resources.md) uç nokta kaynaklarını tanımlama hakkında daha fazla bilgi için Hizmet bildirimi kaynaklarına bakın. |
| MaxReplicationMessageSize |Bayt |50 MB |Tek bir iletide aktarılabilen çoğaltma verilerinin maksimum boyutu. |
| MaxPrimaryReplicationQueueSize |Operasyon sayısı |8192 |Birincil sırada maksimum işlem sayısı. Birincil çoğalıcı tüm ikincil çoğalıcılardan onay aldıktan sonra bir işlem serbest bırakılır. Bu değer 64'ten büyük ve 2'den büyük olmalıdır. |
| MaxSecondaryReplicationQueueSize |Operasyon sayısı |16384 |İkincil sırada maksimum işlem sayısı. Bir işlem, durumunu son derece kullanılabilir hale getirdikten sonra, ısrarla serbest bırakılır. Bu değer 64'ten büyük ve 2'den büyük olmalıdır. |
| CheckpointThresholdInMB |MB |200 |Durum denetim noktası sonra günlük dosya alanı miktarı. |
| MaxRecordSizeinkb |KB |1024 |Çoğalıcının günlüğe yazabileceği en büyük kayıt boyutu. Bu değer 4'ün bir katı ve 16'dan büyük olmalıdır. |
| OptimizeLogForLowerDiskKullanımı |Boole |true |Doğru olduğunda, günlük, yinelemenin özel günlük dosyasın bir NTFS seyrek dosyası kullanılarak oluşturulabilecek şekilde yapılandırılır. Bu, dosyaiçin gerçek disk alanı kullanımını düşürür. Yanlış olduğunda, dosya en iyi yazma performansını sağlayan sabit ayırmalarla oluşturulur. |
| SharedLogId |Guıd |"" |Bu yinelemeyle birlikte kullanılan paylaşılan günlük dosyasını tanımlamak için kullanılacak benzersiz bir kılavuz belirtir. Genellikle, hizmetler bu ayarı kullanmamalıdır. Ancak, SharedLogId belirtilirse, SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam nitelikli yol adı |"" |Bu yineleme için paylaşılan günlük dosyasının oluşturulacağı tam nitelikli yolu belirtir. Genellikle, hizmetler bu ayarı kullanmamalıdır. Ancak, SharedLogPath belirtilmişse, SharedLogId de belirtilmelidir. |

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
BatchAcknowledgementInterval parametresi çoğaltma gecikmesini denetler. '0' değeri, iş maliyeti yle mümkün olan en düşük gecikme gecikmesi ile sonuçlanır (her biri daha az onay içeren daha fazla onay iletisi gönderilmesi ve işlenmesi gerektiğinden).
BatchAcknowledgementInterval için daha büyük değer, daha yüksek işlem gecikmesi pahasına, genel çoğaltma iş maliyeti daha yüksek. Bu doğrudan işlem taahhütlerinin gecikmesine çevirir.

CheckpointThresholdInMB parametresi, çoğaltıcının durum bilgilerini yinelemenin özel günlük dosyasında depolamak için kullanabileceği disk alanı miktarını denetler. Bunu varsayılan değerden daha yüksek bir değere çıkarmak, kümeye yeni bir yineleme eklendiğinde daha hızlı yeniden yapılandırma sürelerine neden olabilir. Bunun nedeni, günlükteki işlemlerin daha fazla geçmişinin kullanılabilirliği nedeniyle gerçekleşen kısmi durum aktarımıdır. Bu, bir kilitlenmeden sonra bir yinelemenin kurtarma süresini artırabilir.

OptimizeForLowerDiskKullanımı doğru olarak ayarlarsanız, günlük dosya alanı, etkin yinelemelerin günlük dosyalarında daha fazla durum bilgisi depolayabilmesi için aşırı olarak sağlanırken, etkin olmayan yinelemeler daha az disk alanı kullanır. Bu, bir düğüm üzerinde daha fazla yineleme barındırmayı mümkün kılar. OptimizeForLowerDiskUse'yi false olarak ayarlarsanız, durum bilgileri günlük dosyalarına daha hızlı yazılır.

MaxRecordSizeInKB ayarı, çoğalıcı tarafından günlük dosyasına yazılabilir bir kaydın maksimum boyutunu tanımlar. Çoğu durumda, varsayılan 1024-KB kayıt boyutu en iyisidir. Ancak, hizmet daha büyük veri öğelerinin durum bilgilerinin bir parçası olmasıiçin neden oluyorsa, bu değerin artırılması gerekebilir. Daha küçük kayıtlar yalnızca daha küçük kayıt için gereken alanı kullandığından, MaxRecordSizeInKB'yi 1024'ten küçük yapmanın çok az yararı vardır. Bu değerin yalnızca nadir durumlarda değiştirilmesi gerektiğini ugörüyoruz.

SharedLogId ve SharedLogPath ayarları, bir hizmetin düğüm için varsayılan paylaşılan günlükten ayrı bir paylaşılan günlük kullanmasını sağlamak için her zaman birlikte kullanılır. En iyi verimlilik için, mümkün olduğunca çok hizmet aynı paylaşılan günlüğü belirtmelidir. Paylaşılan günlük dosyaları, kafa hareketi çekişmesini azaltmak için yalnızca paylaşılan günlük dosyası için kullanılan disklere yerleştirilmelidir. Bu değerlerin yalnızca nadir durumlarda değiştirilmesi gerektiğini ugörüyoruz.

