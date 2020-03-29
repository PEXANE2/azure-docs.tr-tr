---
title: Azure Hizmet Kumaşı Güvenilir Hizmetleri Yapılandır
description: Azure Hizmet Kumaşı uygulamasında ve tek bir hizmet için devlet tarafından sunulan Güvenilir Hizmetleri yapılandırma hakkında bilgi edinin.
author: sumukhs
ms.topic: conceptual
ms.date: 10/02/2017
ms.author: sumukhs
ms.openlocfilehash: 9743213394b59af701b25b8be9dd48cf4310b499
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645523"
---
# <a name="configure-stateful-reliable-services"></a>Devlet tarafından güvenilir hizmetleri yapılandırma
Güvenilir hizmetler için iki yapılandırma ayarı kümesi vardır. Kümedeki tüm güvenilir hizmetler için bir küme genel, diğer küme ise belirli bir güvenilir hizmete özgüdür.

## <a name="global-configuration"></a>Küresel Yapılandırma
Genel güvenilir hizmet yapılandırması, KtlLogger bölümünün altındaki küme için küme manifestosunda belirtilir. Paylaşılan günlük konumu ve boyutunun ve logger tarafından kullanılan genel bellek sınırlarının yapılandırmasına olanak tanır. Küme bildirimi, kümedeki tüm düğümler ve hizmetler için geçerli olan ayarları ve yapılandırmaları tutan tek bir XML dosyasıdır. Dosya genellikle ClusterManifest.xml olarak adlandırılır. Get-ServiceFabricClusterManifest powershell komutunu kullanarak kümeniz için küme bildirimini görebilirsiniz.

### <a name="configuration-names"></a>Yapılandırma adları
| Adı | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| YazmaBufferMemoryPoolMinimumInKB |Kilobayt |8388608 |Logger yazma arabellek bellek havuzu için çekirdek modunda ayırmak için kb en az sayısı. Bu bellek havuzu, diske yazmadan önce durum bilgilerini önbelleğe almak için kullanılır. |
| YazmaBufferMemoryPoolMaximumInKB |Kilobayt |Sınırsız |Logger yazma arabellek bellek havuzu büyüyebilir maksimum boyutu. |
| SharedLogId |GUID |"" |Hizmete özel yapılandırmalarında SharedLogId'i belirtmeyin kümedeki tüm düğümlerde tüm güvenilir hizmetler tarafından kullanılan varsayılan paylaşılan günlük dosyasını tanımlamak için kullanılacak benzersiz bir GUID belirtir. SharedLogId belirtilmişse, SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam nitelikli yol adı |"" |Cluster'daki tüm güvenilir hizmetler tarafından kullanılan ve hizmete özel yapılandırmalarında SharedLogPath'i belirtmeyen paylaşılan günlük dosyasının tam nitelikli yolu belirtir. Ancak, SharedLogPath belirtilmişse, SharedLogId de belirtilmelidir. |
| PaylaşılanLogSizeInMB |Megabayt |8192 |Paylaşılan günlük için statik olarak ayırmak için disk alanımb sayısını belirtir. Değeri 2048 veya daha büyük olmalıdır. |

Azure ARM veya şirket içi JSON şablonunda, aşağıdaki örnekte, devlet tarafından sunulan hizmetler için güvenilir koleksiyonları destekleyecek şekilde oluşturulan paylaşılan işlem günlüğünün nasıl değiştirilen gösterilmektedir.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="sample-local-developer-cluster-manifest-section"></a>Örnek yerel geliştirici küme bildirimi bölümü
Bunu yerel geliştirme ortamınızda değiştirmek istiyorsanız, yerel clustermanifest.xml dosyasını değiştirmeniz gerekir.

```xml
   <Section Name="KtlLogger">
     <Parameter Name="SharedLogSizeInMB" Value="4096"/>
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
   </Section>
```

### <a name="remarks"></a>Açıklamalar
Kaydedici, güvenilir hizmet çoğaltma ile ilişkili özel günlük yazılmadan önce durum verilerini önbelleğe almak için bir düğüm üzerindeki tüm güvenilir hizmetler için kullanılabilir olmayan sayfalı çekirdek bellekten ayrılan genel bir bellek havuzuvardır. Havuz boyutu WriteBufferMemoryPoolMinimumInKB ve WriteBufferMemoryPoolMaximumInKB ayarları tarafından denetlenir. WriteBufferMemoryPoolMinimumInKB, hem bu bellek havuzunun başlangıç boyutunu hem de bellek havuzunun küçülebileceği en düşük boyutu belirtir. WriteBufferMemoryPoolMaximumInKB bellek havuzu büyüyebilir hangi en yüksek boyutudur. Açılan her güvenilir hizmet çoğaltma writeBufferMemoryPoolMaximumInKB kadar belirlenen bir sistem tarafından bellek havuzunun boyutunu artırabilir. Bellek havuzundan bellek için kullanılabilir den daha fazla talep varsa, bellek için istekler bellek kullanılabilir olana kadar geciktirilir. Bu nedenle, yazma arabelleği bellek havuzu belirli bir yapılandırma için çok küçükse, performans zarar görebilir.

SharedLogId ve SharedLogPath ayarları, kümedeki tüm düğümler için varsayılan paylaşılan günlük için GUID ve konumu tanımlamak için her zaman birlikte kullanılır. Varsayılan paylaşılan günlük, belirli bir hizmet için settings.xml'deki ayarları belirtmeyan tüm güvenilir hizmetler için kullanılır. En iyi performans için, paylaşılan günlük dosyaları, çekişmeyi azaltmak için yalnızca paylaşılan günlük dosyası için kullanılan disklere yerleştirilmelidir.

SharedLogSizeInMB, tüm düğümlerde varsayılan paylaşılan günlük için yer değiştirme için disk alanı miktarını belirtir.  SharedLogSizeINMB belirtilmesi için SharedLogId ve SharedLogPath belirtilmesi gerekmez.

## <a name="service-specific-configuration"></a>Hizmete Özel Yapılandırma
Yapılandırma paketini (Config) veya hizmet uygulamasını (kodu) kullanarak durumlu Güvenilir Hizmetler'in varsayılan yapılandırmalarını değiştirebilirsiniz.

* **Config** - Config paketi üzerinden yapılandırma, microsoft visual studio paket kökünde oluşturulan Settings.xml dosyasının uygulamadaki her hizmet için Config klasörü altında değiştirilmesiyle gerçekleştirilir.
* **Kod** - Kod üzerinden yapılandırma, uygun seçenekler kümesi ile ReliableStateManager yapılandırma nesnesi kullanarak bir ReliableStateManager oluşturarak gerçekleştirilir.

Varsayılan olarak, Azure Hizmet Dokusu çalışma zamanı Ayarlar.xml dosyasında önceden tanımlanmış bölüm adlarını arar ve temel çalışma zamanı bileşenlerini oluştururken yapılandırma değerlerini tüketir.

> [!NOTE]
> Hizmetinizi kod yoluyla yapılandırmayı planlamadığınız sürece Visual Studio çözümünde oluşturulan Settings.xml dosyasındaki aşağıdaki yapılandırmaların bölüm adlarını **not** silmeyin.
> Config paketini veya bölüm adlarını yeniden adlandırmak, ReliableStateManager'ı yapılandırırken bir kod değişikliği gerektirir.
> 
> 

### <a name="replicator-security-configuration"></a>Çoğalıcı güvenlik yapılandırması
Çoğaltma güvenlik yapılandırmaları çoğaltma sırasında kullanılan iletişim kanalını güvence altına almak için kullanılır. Bu, hizmetlerin birbirlerinin çoğaltma trafiğini göremeyecekleri ve yüksek oranda kullanılabilir hale getirilen verilerin de güvenli olmasını sağlayacağı anlamına gelir. Varsayılan olarak, boş bir güvenlik yapılandırma bölümü çoğaltma güvenliğini engeller.

> [!IMPORTANT]
> Linux düğümlerinde sertifikalar PEM biçimlendirilmiş olmalıdır. Linux için sertifikaları bulma ve yapılandırma hakkında daha fazla bilgi edinmek için [bkz.](./service-fabric-configure-certificates-linux.md) 
> 
> 

### <a name="default-section-name"></a>Varsayılan bölüm adı
ReplicatorSecurityConfig

> [!NOTE]
> Bu bölüm adını değiştirmek için, bu hizmet için ReliableStateManager oluştururken çoğaltıcıSecuritySectionName parametresini ReliableStateManagerConfiguration constructor'a geçersiz kılın.
> 
> 

### <a name="replicator-configuration"></a>Çoğaltıcı yapılandırması
Çoğalıcı yapılandırmaları, durumu yerel olarak çoğaltarak ve kalıcı hale getirerek, duruma uygun Güvenilir Hizmet in durumunu son derece güvenilir hale getirmekten sorumlu çoğaltıcıyı yapılandırır.
Varsayılan yapılandırma Visual Studio şablonu tarafından oluşturulur ve yeterli olmalıdır. Bu bölümde çoğaltıcıyı ayarlamak için kullanılabilen ek yapılandırmalar hakkında konuşuyor.

### <a name="default-section-name"></a>Varsayılan bölüm adı
ÇoğalTıcıConfig

> [!NOTE]
> Bu bölüm adını değiştirmek için, bu hizmet için ReliableStateManager oluştururken çoğaltıcıAyarlarSectionName parametresini ReliableStateManagerConfiguration constructor'a geçersiz kılın.
> 
> 

### <a name="configuration-names"></a>Yapılandırma adları
| Adı | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Saniye |0.015 |Birincil bir bildirim göndermeden önce ikincil çoğaltıcı için bekleme süresi. Bu aralıkta işlenen işlemler için gönderilecek diğer bildirimler tek bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |Yok |Varsayılan değil-- gerekli parametre |Birincil/ikincil çoğaltıcının çoğaltma kümesindeki diğer çoğalıcılarla iletişim kurmak için kullanacağı IP adresi ve bağlantı noktası. Bu, hizmet bildiriminde bir TCP kaynak bitiş noktasına başvurulmalıdır. Hizmet [bildiriminde](service-fabric-service-manifest-resources.md) uç nokta kaynaklarını tanımlama hakkında daha fazla bilgi için Hizmet bildirimi kaynaklarına bakın. |
| MaxPrimaryReplicationQueueSize |Operasyon sayısı |8192 |Birincil sırada maksimum işlem sayısı. Birincil çoğalıcı tüm ikincil çoğalıcılardan onay aldıktan sonra bir işlem serbest bırakılır. Bu değer 64'ten büyük ve 2'den büyük olmalıdır. |
| MaxSecondaryReplicationQueueSize |Operasyon sayısı |16384 |İkincil sırada maksimum işlem sayısı. Bir işlem, durumunu son derece kullanılabilir hale getirdikten sonra, ısrarla serbest bırakılır. Bu değer 64'ten büyük ve 2'den büyük olmalıdır. |
| CheckpointThresholdInMB |MB |50 |Durum denetim noktası sonra günlük dosya alanı miktarı. |
| MaxRecordSizeinkb |KB |1024 |Çoğalıcının günlüğe yazabileceği en büyük kayıt boyutu. Bu değer 4'ün bir katı ve 16'dan büyük olmalıdır. |
| MinLogSizeInMB |MB |0 (sistem belirlendi) |İşlem günlüğünün minimum boyutu. Günlüğün bu ayarın altındaki bir boyuta kadar kırmasına izin verilmeyecektir. 0, çoğaltıcının minimum günlük boyutunu belirleyeceğini gösterir. İlgili günlük kayıtlarının kesilen şansı düşürüldüğünden, bu değerin artırılması kısmi kopyalar ve artımlı yedeklemeler yapma olasılığını artırır. |
| KesilmeThresholdFactor |Faktörü |2 |Günlüğün boyutunu belirler, kesilme tetiklenir. Kesilme eşiği MinLogSizeInMB tarafından TruncationThresholdFactor ile çarpılır tarafından belirlenir. TruncationThresholdFactor 1'den büyük olmalıdır. MinLogSizeInMB * TruncationThresholdFactor MaxStreamSizeINMB daha az olmalıdır. |
| AzaltmaThresholdFactor |Faktörü |4 |Günlüğün boyutunu belirler, yineleme daraltılmış başlar. Azaltma eşiği (MB olarak) Max ((MinLogSizeINMB * ThrottlingThresholdFactor),(CheckpointThresholdInMB * ThrottlingThresholdFactor)) tarafından belirlenir. Azaltma eşiği (MB'de) kesilme eşiğinden (MB'de) daha büyük olmalıdır. Kesilme eşiği (MB'de) MaxStreamSizeINMB'den daha az olmalıdır. |
| MaxAccumulatedBackupLogSizeinMB |MB |800 |Belirli bir yedekleme günlüğü zincirinde yedek günlüklerin maksimum birikmiş boyutu (MB'de). Artımlı yedekleme, ilgili tam yedeklemenin bu boyuttan daha büyük olması nedeniyle birikmiş yedekleme günlüklerinin oluşturulmasına neden olacak bir yedekleme günlüğü oluşturursa, artımlı yedekleme istekleri başarısız olur. Bu gibi durumlarda, kullanıcının tam bir yedekleme alması gerekir. |
| SharedLogId |GUID |"" |Bu yinelemeile birlikte kullanılan paylaşılan günlük dosyasını tanımlamak için kullanılacak benzersiz bir GUID belirtir. Genellikle, hizmetler bu ayarı kullanmamalıdır. Ancak, SharedLogId belirtilirse, SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam nitelikli yol adı |"" |Bu yineleme için paylaşılan günlük dosyasının oluşturulacağı tam nitelikli yolu belirtir. Genellikle, hizmetler bu ayarı kullanmamalıdır. Ancak, SharedLogPath belirtilmişse, SharedLogId de belirtilmelidir. |
| SlowApiMonitoringDuration |Saniye |300 |Yönetilen API çağrıları için izleme aralığını ayarlar. Örnek: kullanıcı yedek geri arama işlevi sağladı. Aradan zaman geçtikten sonra Sağlık Müdürü'ne bir uyarı sağlık raporu gönderilir. |
| LogTruncationIntervalSeconds |Saniye |0 |Her yinelemede günlük kesilmenin başlatılabildiği yapılandırılabilir aralık. Günlük boyutu yerine zamana göre kesilen günlük sağlamak için kullanılır. Bu ayar, silinen girişlerin güvenilir sözlükte tasfiyesini de zorlar. Bu nedenle silinen öğelerin zamanında temizlenmesini sağlamak için kullanılabilir. |
| EnableStableReads |Boole |False |Kararlı okumaları etkinleştirmek, ikincil yinelemeleri, çoğunluk tarafından iptal edilmiş dönen değerlerle sınırlandırMaktadır. |

### <a name="sample-configuration-via-code"></a>Kod üzerinden örnek yapılandırma
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Örnek yapılandırma dosyası
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
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


### <a name="remarks"></a>Açıklamalar
BatchAcknowledgementInterval çoğaltma gecikmesi denetler. '0' değeri, iş maliyeti yle mümkün olan en düşük gecikme gecikmesi ile sonuçlanır (her biri daha az onay içeren daha fazla onay iletisi gönderilmesi ve işlenmesi gerektiğinden).
BatchAcknowledgementInterval için daha büyük değer, daha yüksek işlem gecikmesi pahasına, genel çoğaltma iş maliyeti daha yüksek. Bu doğrudan işlem taahhütlerinin gecikmesine çevirir.

CheckpointThresholdInMB değeri, çoğaltıcının yinelemenin özel günlük dosyasında durum bilgilerini depolamak için kullanabileceği disk alanı miktarını denetler. Bunu varsayılan değerden daha yüksek bir değere çıkarmak, kümeye yeni bir yineleme eklendiğinde daha hızlı yeniden yapılandırma sürelerine neden olabilir. Bunun nedeni, günlükteki işlemlerin daha fazla geçmişinin kullanılabilirliği nedeniyle gerçekleşen kısmi durum aktarımıdır. Bu, bir kilitlenmeden sonra bir yinelemenin kurtarma süresini artırabilir.

MaxRecordSizeInKB ayarı, çoğalıcı tarafından günlük dosyasına yazılabilir bir kaydın maksimum boyutunu tanımlar. Çoğu durumda, varsayılan 1024-KB kayıt boyutu en iyisidir. Ancak, hizmet daha büyük veri öğelerinin durum bilgilerinin bir parçası olmasıiçin neden oluyorsa, bu değerin artırılması gerekebilir. Daha küçük kayıtlar yalnızca daha küçük kayıt için gereken alanı kullandığından, MaxRecordSizeInKB'yi 1024'ten küçük yapmanın çok az yararı vardır. Bu değerin sadece nadir durumlarda değiştirilmesi gerektiğini ugörüyoruz.

SharedLogId ve SharedLogPath ayarları, bir hizmetin düğüm için varsayılan paylaşılan günlükten ayrı bir paylaşılan günlük kullanmasını sağlamak için her zaman birlikte kullanılır. En iyi verimlilik için, mümkün olduğunca çok hizmet aynı paylaşılan günlüğü belirtmelidir. Paylaşılan günlük dosyaları, kafa hareketi çekişmesini azaltmak için yalnızca paylaşılan günlük dosyası için kullanılan disklere yerleştirilmelidir. Bu değerin sadece nadir durumlarda değiştirilmesi gerektiğini ugörüyoruz.

## <a name="next-steps"></a>Sonraki adımlar
* [Visual Studio'da Servis Kumaşı uygulamanızı hata ayıklama](service-fabric-debugging-your-application.md)
* [Güvenilir Hizmetler için geliştirici başvurusu](https://msdn.microsoft.com/library/azure/dn706529.aspx)

