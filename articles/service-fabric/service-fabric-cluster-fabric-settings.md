---
title: Azure Hizmet Kumaş küme ayarlarını değiştirme
description: Bu makalede, özelleştirebileceğiniz kumaş ayarları ve kumaş yükseltme ilkeleri açıklanmaktadır.
ms.topic: reference
ms.date: 08/30/2019
ms.openlocfilehash: a4e64a4db70d419a3ef6441545d53abd298c85bb
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346805"
---
# <a name="customize-service-fabric-cluster-settings"></a>Service Fabric kümesi ayarlarını özelleştirme
Bu makalede, Özelleştirebileceğiniz Hizmet Kumaşı kümenizin çeşitli kumaş ayarları açıklanmaktadır. Azure'da barındırılan kümeler için, ayarları [Azure portalı](https://portal.azure.com) üzerinden veya Azure Kaynak Yöneticisi şablonu kullanarak özelleştirebilirsiniz. Daha fazla bilgi için [bkz.](service-fabric-cluster-config-upgrade-azure.md) Bağımsız kümeler için *ClusterConfig.json* dosyasını güncelleştirerek ve kümenizde yapılandırma yükseltmesi gerçekleştirerek ayarları özelleştirebilirsiniz. Daha fazla bilgi için bkz: [Bağımsız bir kümenin yapılandırmasını yükseltin.](service-fabric-cluster-config-upgrade-windows-server.md)

Üç farklı yükseltme politikası vardır:

- **Dinamik** – Dinamik yapılandırmadaki değişiklikler, Servis Kumaşı işlemlerinin veya servis ana bilgisayar süreçlerinizin herhangi bir işlemin yeniden başlatılmasına neden olmaz. 
- **Statik** – Statik yapılandırmadaki değişiklikler, değişikliği tüketmek için Hizmet Dokusu düğümünün yeniden başlatılmasına neden olur. Düğümlerle ilgili hizmetler yeniden başlatılır.
- **İzin Verilmiyor** – Bu ayarlar değiştirilemez. Bu ayarları değiştirmek için kümenin yok edilmesi ve yeni bir küme oluşturulması gerekir. 

Aşağıda, bölüme göre düzenlenmiş, özelleştirebileceğiniz Kumaş ayarlarının bir listesi verilmiştir.

## <a name="applicationgatewayhttp"></a>UygulamaGateway/ Http

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Uygulama Sertifikası Doğrulama Politikası|dize, varsayılan "Yok"|Statik| Bu, sunucu sertifikasını doğrulamaz; isteği başarılı. Ters proxy'nin güvenebileceği uzak sertifikaların parmak izlerinin virgülle ayrılmış listesi için config ServiceCertificateThumbprints'e bakın. Ters proxy güvenebilirsiniz uzak certs konu adı ve veren parmak izi için config ServiceCommonNameAndIssuer bakın. Daha fazla bilgi için ters [proxy güvenli bağlantı](service-fabric-reverseproxy-configure-secure-communication.md#secure-connection-establishment-between-the-reverse-proxy-and-services)bakın. |
|BodyChunkSize |Uint, varsayılan 16384 |Dinamik| Vücudu okumak için kullanılan baytlar içinde yığın için boyutunu verir. |
|CrlCheckingFlag|uint, varsayılan 0x4000000 |Dinamik| Uygulama/hizmet sertifikası zinciri doğrulaması için bayraklar; örneğin 0x10000000CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x4000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT000 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY 0 devre dışı kalarak CRL denetimi desteklenen değerlerin tam listesini CertGetCertificateChain dwFlags tarafından belgelenir:https://msdn.microsoft.com/library/windows/desktop/aa376078(v=vs.85).aspx  |
|VarsayılanHttpRequestTimeout |Saniyeler içinde zaman. varsayılan 120 |Dinamik|Zaman açıklığı saniye cinsinden belirtin.  Http uygulaması ağ geçidinde işlenen http istekleri için varsayılan istek zaman arasını verir. |
|ForwardClientCertificate|bool, varsayılan FALSE olduğunu|Dinamik|Yanlış olarak ayarlandığında, ters proxy istemci sertifikası için istekte bulunmaz. Doğru ayarlandığında, ters proxy SSL el sıkışma sırasında istemci sertifikası için istek tevekkül eder ve base64 kodlanmış PEM biçimi dizesini X-Client-Certificate.The servis in adlandırılmış bir üstbilgide hizmete iletir. sertifika verilerini inceledikten sonra. Bu doğruysa ve istemci bir sertifika sunmuyorsa, ters proxy boş bir üstbilgi iletir ve servisin servisin servis ini işlemesine izin verir. Ters proxy saydam bir katman olarak hareket edecektir. Daha fazla bilgi için [bkz.](service-fabric-reverseproxy-configure-secure-communication.md#setting-up-client-certificate-authentication-through-the-reverse-proxy) |
|GatewayAuthCredentialType |dize, varsayılan "Yok" |Statik| http app ağ geçidi bitiş noktası Geçerli değerleri kullanmak için güvenlik kimlik bilgileri türünü gösterir None/X509 vardır. |
|GatewayX509CertificateFindType |dize, varsayılan "FindByThumbprint" |Dinamik| GatewayX509CertificateStoreName Desteklenen değer tarafından belirtilen mağazada sertifika aramanasıl yapılacağını gösterir: FindByThumbprint; Findbysubjectname. |
|GatewayX509CertificateFindValue | dize, varsayılan "" |Dinamik| Http uygulaması ağ geçidi sertifikasını bulmak için kullanılan arama filtresi değeri. Bu sertifika https bitiş noktasında yapılandırılır ve hizmetler tarafından gerekirse uygulamanın kimliğini doğrulamak için de kullanılabilir. FindValue ilk aranır; ve eğer bu yoksa; FindValueSecondary yukarı bakılır. |
|GatewayX509CertificateFindValueİkincil | dize, varsayılan "" |Dinamik|Http uygulaması ağ geçidi sertifikasını bulmak için kullanılan arama filtresi değeri. Bu sertifika https bitiş noktasında yapılandırılır ve hizmetler tarafından gerekirse uygulamanın kimliğini doğrulamak için de kullanılabilir. FindValue ilk aranır; ve eğer bu yoksa; FindValueSecondary yukarı bakılır.|
|GatewayX509CertificateStoreName |dize, varsayılan "Benim" |Dinamik| Http app ağ geçidi için sertifika içeren X.509 sertifika mağazasının adı. |
|HttpRequestConnectTimeout|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(5)|Dinamik|Zaman açıklığı saniye cinsinden belirtin.  Http uygulaması ağ geçidinden gönderilen http istekleri için bağlantı zaman arasını verir.  |
|YoksayCrlOfflineHatası|bool, varsayılan DOĞRU|Dinamik|Uygulama/hizmet sertifikası doğrulaması için CRL çevrimdışı hatasını yoksayMa. |
|IsEnabled |Bool, varsayılan yanlış |Statik| HttpApplicationGateway'i etkinleştiri/devre dışı kılabilir. HttpApplicationGateway varsayılan olarak devre dışı bırakılır ve bu config bunu etkinleştirmek için ayarlanması gerekir. |
|NumberofParallelOperations | Uint, varsayılan 5000 |Statik|Http sunucu kuyruğuna nakletmek için okuma sayısı. Bu, HttpGateway tarafından karşılanabilecek eşzamanlı istek sayısını denetler. |
|ServiceResponseHeaders kaldırın|dize, varsayılan "Tarih; Sunucu"|Statik|Hizmet yanıtıkaldırılacak yanıt başlıklarıyarı kolon / virgül ayrılmış listesi; istemciye iletmeden önce. Bu, boş dize olarak ayarlanmışsa; hizmet tarafından döndürülen tüm üstbilgi olarak geçirin. Yani Tarih ve Sunucu üzerine yazmayın |
|ResolveServiceBackoffInterval |Saniye cinsinden zaman, varsayılan 5 |Dinamik|Zaman açıklığı saniye cinsinden belirtin.  Başarısız bir çözüm hizmeti işlemini yeniden denemeden önce varsayılan geri dönüş aralığını verir. |
|SecureOnlyMode|bool, varsayılan FALSE olduğunu|Dinamik| SecureOnlyMode: true: Reverse Proxy yalnızca güvenli uç noktaları yayımlayan hizmetlere iletilir. false: Reverse Proxy isteklerini güvenli/güvenli olmayan uç noktalara iletebilir. Daha fazla bilgi için ters [proxy uç nokta seçim mantığına](service-fabric-reverseproxy-configure-secure-communication.md#endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints)bakın.  |
|ServiceCertificateThumbprints|dize, varsayılan ""|Dinamik|Ters proxy güvenebilirsiniz uzak sertifikalar parmak izlerinin virgülle ayrılmış listesi. Daha fazla bilgi için ters [proxy güvenli bağlantı](service-fabric-reverseproxy-configure-secure-communication.md#secure-connection-establishment-between-the-reverse-proxy-and-services)bakın. |

## <a name="applicationgatewayhttpservicecommonnameandissuer"></a>UygulamaAğ Ağ Geçidi/Http/ServiceCommonNameAndIssuer

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|X509NameMap, varsayılan Yok|Dinamik| Ters proxy güvenebilirsiniz uzak sertifikaların konu adı ve veren parmak izi. Daha fazla bilgi için ters [proxy güvenli bağlantı](service-fabric-reverseproxy-configure-secure-communication.md#secure-connection-establishment-between-the-reverse-proxy-and-services)bakın. |

## <a name="backuprestoreservice"></a>YedeklemeGeri Hizmet

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|MinReplicaSetSize|int, varsayılan 0|Statik|BackupRestoreService için MinReplicaSetSize |
|YerleşimKısıtlamalar|dize, varsayılan ""|Statik|  BackupGeri hizmeti için Yerleşim Kısıtlamaları |
|GizliŞifrelemeCertThumbprint|dize, varsayılan ""|Dinamik|Gizli şifreleme X509 sertifikasının parmak izi |
|SecretEncryptionCertX509StoreName|dize, varsayılan "Benim"|   Dinamik|    Bu, Yedekleme Geri Yükleme hizmeti tarafından kullanılan şifre çözme deposu kimlik bilgilerini şifrelemek için kullanılan X.509 sertifika deposunun adı şifreleme ve şifre çözme için kullanılacak sertifikayı gösterir |
|TargetReplicaSetSize|int, varsayılan 0|Statik| BackupRestoreService için TargetReplicaSetSize |

## <a name="clustermanager"></a>ClusterManager

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|İzin VerÖzelYükseltmeSıralama Politikaları | Bool, varsayılan yanlış |Dinamik|Özel yükseltme sıralama ilkelerine izin verilip verilmedi. Bu, bu özelliği etkinleştiren 2 aşamalı yükseltme gerçekleştirmek için kullanılır. Service Fabric 6.5, küme veya uygulama yükseltmeleri sırasında yükseltme etki alanları için sıralama ilkesi belirtmek için destek ekler. Desteklenen ilkeler Sayısal, Lexicographical, ReverseNumeric ve ReverseLexicographical vardır. Varsayılan sayısaldır. Bu özelliği kullanabilmek için Cluster Manager/ AllowCustomUpgradeSortPolicies kümesi nin SF 6.5 kodu yükseltmeyi tamamladıktan sonra ikinci bir config yükseltme adımı olarak True olarak ayarlanması gerekir. Bunun iki aşamada yapılması önemlidir, aksi takdirde kod yükseltmesi ilk yükseltme sırasında yükseltme sırası hakkında karışabilir.|
|Varsayılan Hizmetleri Yükseltmeyi Etkinleştir | Bool, varsayılan yanlış |Dinamik|Uygulama yükseltme sırasında varsayılan hizmetleri yükseltmeyi etkinleştirin. Varsayılan hizmet açıklamaları yükseltmeden sonra üzerine yazılır. |
|KumaşYükseltmeHealthCheckInterval |Saniye cinsinden zaman, varsayılan 60 |Dinamik|İzlenen Kumaş yükseltmesi sırasında sağlık durumu kontrolü sıklığı |
|KumaşYükseltmeDurumPollInterval |Saniye cinsinden zaman, varsayılan 60 |Dinamik|Kumaş yükseltme durumu için yoklama sıklığı. Bu değer, herhangi bir GetFabricUpgradeProgress çağrısı için güncelleştirme oranını belirler |
|ImageBuilderTimeoutBuffer |Saniye cinsinden zaman, varsayılan 3 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Görüntü Oluşturucu'nun belirli zaman ayırma hatalarının istemciye dönmesine izin verme süresi. Bu arabellek çok küçükse; sonra istemci sunucudan önce zaman ları dışarı ve genel bir zaman alakart hatası alır. |
|AltyapıTaskHealthCheckRetryTimeout | Saniye cinsinden zaman, varsayılan 60 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Bir altyapı görevini işleme alırken başarısız sağlık denetimlerini yeniden denemek için harcanacak süre. Geçirilen bir sağlık kontrolünü gözlemlemek bu zamanlayıcıyı sıfırlar. |
|AltyapıTaskHealthCheckStableDuration | Saniye cinsinden zaman, varsayılan değer 0'dır|Dinamik| Zaman açıklığı saniye cinsinden belirtin. Bir altyapı görevinin işlenmesinden önce art arda geçen sağlık kontrollerini gözlemleme süresi başarıyla tamamlar. Başarısız bir sağlık kontrolü gözlemleyerek bu zamanlayıcı sıfırlar. |
|AltyapıTaskHealthCheckWaitDuration |Saniye cinsinden zaman, varsayılan değer 0'dır|Dinamik| Zaman açıklığı saniye cinsinden belirtin. Bir altyapı görevinin işlenmesinden sonra sağlık kontrollerine başlamadan önce bekleme süresi. |
|AltyapıGörev İşlemeAralığı | Saniye cinsinden zaman, varsayılan süre 10'dur |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Altyapı görev işleme durumu makinesi tarafından kullanılan işleme aralığı. |
|MaxCommunicationTimeout |Saniye cinsinden zaman, varsayılan 600 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. ClusterManager ve diğer sistem hizmetleri arasındaki iç iletişim için maksimum zaman aşımı (örn. Adlandırma Hizmeti; Failover Manager ve benzeri). Bu zaman dışında genel MaxOperationTimeout daha küçük olmalıdır (her istemci işlemi için sistem bileşenleri arasında birden çok iletişim olabilir gibi). |
|MaxDataMigrationTimeout |Saniye cinsinden zaman, varsayılan 600 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Kumaş yükseltmesi gerçekleştikten sonra veri geçişi kurtarma işlemleri için maksimum zaman ası. |
|MaxOperationRetryDelay |Saniye cinsinden zaman, varsayılan 5|Dinamik| Zaman açıklığı saniye cinsinden belirtin. Hatalarla karşılaşıldığında dahili yeniden denemeler için maksimum gecikme. |
|MaxOperationTimeout |Saniye cinsinden zaman, varsayılan MaxValue olduğunu |Dinamik| Zaman açıklığı saniye cinsinden belirtin. ClusterManager'da dahili işleme işlemleri için maksimum genel zaman ayırma. |
|MaxTimeoutRetryBuffer | Saniye cinsinden zaman, varsayılan 600 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Zaman açılışları `<Original Time out> + <MaxTimeoutRetryBuffer>`nedeniyle dahili olarak yeniden denerken en yürüyüşzaman zaman süresini MinOperationTimeout'un artışlarına ek zaman ayarı eklenir. |
|MinOperationTimeout | Saniye cinsinden zaman, varsayılan 60 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. ClusterManager'da dahili işleme işlemleri için minimum genel zaman ayırma. |
|MinReplicaSetSize |Int, varsayılan 3 |İzin Verilmiyor|ClusterManager için MinReplicaSetSize. |
|YerleşimKısıtlamalar | dize, varsayılan "" |İzin Verilmiyor|ClusterManager için Yerleşim Kısıtlamaları. |
|ÇoğunlukLossWaitDuration |Saniye cinsinden zaman, varsayılan MaxValue olduğunu |İzin Verilmiyor| Zaman açıklığı saniye cinsinden belirtin. ClusterManager için QuorumLossWaitDuration. |
|ÇoğaltmaRestartWaitDuration |Saniye cinsinden zaman, varsayılan (60,0 \* 30)|İzin Verilmiyor|Zaman açıklığı saniye cinsinden belirtin. ClusterManager için ÇoğaltmaRestartWaitDuration. |
|ReplicaSetCheckTimeoutRollbackOverride |Saniye cinsinden zaman, varsayılan süre 1200'dür |Dinamik| Zaman açıklığı saniye cinsinden belirtin. ReplicaSetCheckTimeout DWORD'ün maksimum değerine ayarlanmışsa; sonra geri alma amacıyla bu config değeri ile geçersiz kılınmıştır. Roll-forward için kullanılan değer hiçbir zaman geçersiz kılındı. |
|SkipRollbackUpdateDefaultService | Bool, varsayılan yanlış |Dinamik|CM, uygulama yükseltme geri alma sırasında güncelleştirilmiş varsayılan hizmetleri geri alma atlatır. |
|StandbyReplicaKeepDuration | Saniye cinsinden zaman, varsayılan dır (3600,0 \* 2)|İzin Verilmiyor|Zaman açıklığı saniye cinsinden belirtin. ClusterManager için StandByReplicaKeepDuration. |
|TargetReplicaSetSize |Int, varsayılan 7 |İzin Verilmiyor|ClusterManager için TargetReplicaSetSize. |
|YükseltmeHealthCheckInterval |Saniye cinsinden zaman, varsayılan 60 |Dinamik|İzlenen uygulama yükseltmeleri sırasında sistem durumu denetimlerinin sıklığı |
|UpgradeStatusPollInterval |Saniye cinsinden zaman, varsayılan 60 |Dinamik|Uygulama yükseltme durumu için yoklama sıklığı. Bu değer, herhangi bir GetApplicationUpgradeProgress çağrısı için güncelleştirme oranını belirler |
|Tam Müşteri İsteği | Bool, varsayılan yanlış |Dinamik| CM tarafından kabul edildiğinde tam istemci isteği. |

## <a name="common"></a>Common

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|PerfMonitorInterval |Saniye cinsinden zaman, varsayılan değer 1 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Performans izleme aralığı. 0 veya negatif değere ayar izleme devre dışı kılabilir. |

## <a name="defragmentationemptynodedistributionpolicy"></a>ParçalanmaEmptyNodeDistributionPolicy
| **Parametre** | **İzin Verilen Değerler** |**Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|KeyIntegerValueMap, varsayılan Yok|Dinamik|Düğümleri boşaltırken ilke parçalanmasını belirtir. Belirli bir metrik 0 için SF'nin d'ler ve FD'ler arasında düğümleri eşit olarak parçalamaya çalışması gerektiğini gösterir; 1 yalnızca düğümlerin parçalanması gerektiğini gösterir |

## <a name="defragmentationmetrics"></a>ParçalanmaMetrikleri
| **Parametre** | **İzin Verilen Değerler** |**Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|KeyBoolValueMap, varsayılan Yok|Dinamik|Parçalama için kullanılması gereken ölçüler kümesini belirler, yük dengelemeiçin değil. |

## <a name="defragmentationmetricspercentornumberofemptynodestriggeringthreshold"></a>ParçalanmaMetricsPercentOrNumberOfEmptyNodesTetiklemeEşik
| **Parametre** | **İzin Verilen Değerler** |**Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|KeyDoubleValueMap, varsayılan Yok|Dinamik|Aralığında ki yüzde [0,0 - 1,0] veya boş düğüm sayısını >= 1,0 olarak belirterek kümenin parçalanmasını dikkate almak için gereken serbest düğüm sayısını belirler |

## <a name="diagnostics"></a>Tanılama

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|AdminOnlyHttpAudit |Bool, varsayılan doğrudur | Dinamik | Kümenin durumunu etkilemeyen HTTP isteklerini denetlemeyi hariç tinleyerek hariç tinleyerek. Şu anda; yalnızca "GET" türünden istekler hariç tutulur; ama bu değişebilir. |
|AppDiagnosticStoreAccessRequiresImpersonation |Bool, varsayılan doğrudur | Dinamik |Uygulama adına tanı depolarıerişirken kimliğe bürünme gerekip gerekmediği. |
|AppEtwTraceDeletionAgeInDays |Int, varsayılan 3 | Dinamik |Uygulama ETW izlerini içeren eski ETL dosyalarını sildiğimiz gün sayısı. |
|ApplicationLogsFormatVersion |Int, varsayılan 0 | Dinamik |Uygulama günlükleri biçimi için sürüm. Desteklenen değerler 0 ve 1'dir. Sürüm 1, ETW olay kaydından sürüm 0'dan daha fazla alan içerir. |
|DenetimHttpRequests |Bool, varsayılan yanlış | Dinamik | HTTP denetimini açın veya kapatın. Denetimin amacı kümeye karşı gerçekleştirilen faaliyetleri görmektir; isteği kimin başlattığı da dahil olmak üzere. Bunun en iyi deneme günlüğe kaydetme olduğunu unutmayın; ve iz kaybı oluşabilir. "Kullanıcı" kimlik doğrulaması ile HTTP istekleri kaydedilmez. |
|YakalamaHttpTelemetry|Bool, varsayılan doğrudur | Dinamik | HTTP telemetrisini açın veya kapatın. Telemetrinin amacı, Service Fabric'in gelecekteki çalışmaları planlamaya ve sorunlu alanları belirlemeye yardımcı olacak telemetri verilerini yakalayabilmektir. Telemetri herhangi bir kişisel veri veya istek organı kaydetmez. Telemetri aksi yapılandırıldıkça tüm HTTP isteklerini yakalar. |
|ClusterId |Dize | Dinamik |Kümenin benzersiz kimliği. Küme oluşturulduğunda bu oluşturulur. |
|Tüketici Örnekleri |Dize | Dinamik |DCA tüketici örnekleri listesi. |
|DiskfullSafetySpaceInMB |Int, varsayılan 1024 | Dinamik |DCA tarafından kullanılmasından korumak için MB'de kalan disk alanı. |
|EnableCircularTraceSession |Bool, varsayılan yanlış | Statik |Bayrak, dairesel izleme oturumlarının kullanılıp kullanılmayacağını gösterir. |
|EtkinleştirmePlatformEventsFileSink |Bool, varsayılan yanlış | Statik |Diske yazılan platform olaylarını etkinleştirme/devre dışı etme |
|EnableTelemetry |Bool, varsayılan doğrudur | Dinamik |Bu telemetriyi etkinleştirecek veya devre dışı bölecek. |
|HatalarSadeceHttpTelemetry | Bool, varsayılan yanlış | Dinamik | HTTP telemetri yakalama etkinse; yalnızca başarısız istekleri yakalayın. Bu, telemetri için oluşturulan olayların sayısını azaltmaya yardımcı olmaktır. |
|HttpTelemetryYakalama Yüzdesi | int, varsayılan 50 | Dinamik | HTTP telemetri yakalama etkinse; isteklerin yalnızca rasgele bir yüzdesini yakalayın. Bu, telemetri için oluşturulan olayların sayısını azaltmaya yardımcı olmaktır. |
|MaxDiskQuotaInMB |Int, varsayılan 65536 | Dinamik |Windows Fabric günlük dosyaları için MB disk kotası. |
|Üretici Örnekleri |Dize | Dinamik |DCA üretici örneklerinin listesi. |

## <a name="dnsservice"></a>DnsService
| **Parametre** | **İzin Verilen Değerler** |**Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|EnablePartitionedQuery|bool, varsayılan FALSE olduğunu|Statik|Bölümlenmiş hizmetler için DNS sorguları için destek etkinleştirmek için bayrak. Özellik varsayılan olarak kapatılır. Daha fazla bilgi için [Service Fabric DNS Service'e bakın.](service-fabric-dnsservice.md)|
|InstanceCount|int, varsayılan -1|Statik|Varsayılan değer -1'dir, bu da DnsService'in her düğümde çalıştığını anlamına gelir. DnsService iyi bilinen bağlantı noktası 53 kullandığından OneBox'ın bunu 1 olarak ayarlaması gerekir, bu nedenle aynı makinede birden fazla örneği olamaz.|
|IsEnabled|bool, varsayılan FALSE olduğunu|Statik|DnsService'i etkinleştiri/devre dışı kılabilir. DnsService varsayılan olarak devre dışı bırakılır ve bu config bunu etkinleştirmek için ayarlanması gerekir. |
|BölümÖnek|dize, varsayılan "--"|Statik|Bölümlenmiş hizmetler için DNS sorgularında bölüm öneki dize değerini denetler. Değer : <ul><li>Bir DNS sorgusunun parçası olacağı için RFC uyumlu olmalıdır.</li><li>Nokta DNS sonek davranışına engel olduğu için nokta '.'içermemelidir.</li><li>5 karakterden uzun olmamalıdır.</li><li>Boş bir dize olamaz.</li><li>PartitionPrefix ayarı geçersiz kılınmışsa, PartitionSuffix geçersiz kılınmalıdır ve tam tersi.</li></ul>Daha fazla bilgi için [Service Fabric DNS Service.](service-fabric-dnsservice.md).|
|BölümSuffix|dize, varsayılan ""|Statik|Bölümlenmiş hizmetler için DNS sorgularında bölüm sonek dize değerini denetler. Değer : <ul><li>Bir DNS sorgusunun parçası olacağı için RFC uyumlu olmalıdır.</li><li>Nokta DNS sonek davranışına engel olduğu için nokta '.'içermemelidir.</li><li>5 karakterden uzun olmamalıdır.</li><li>PartitionPrefix ayarı geçersiz kılınmışsa, PartitionSuffix geçersiz kılınmalıdır ve tam tersi.</li></ul>Daha fazla bilgi için [Service Fabric DNS Service.](service-fabric-dnsservice.md). |

## <a name="eventstoreservice"></a>EventStoreService

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|MinReplicaSetSize|int, varsayılan 0|Statik|EventStore hizmeti için MinReplicaSetSize |
|YerleşimKısıtlamalar|dize, varsayılan ""|Statik|  EventStore hizmeti için Yerleşim Kısıtlamaları |
|TargetReplicaSetSize|int, varsayılan 0|Statik| EventStore hizmeti için TargetReplicaSetSize |

## <a name="fabricclient"></a>KumaşMüşteri

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|BağlantıInitiallaştırmaTimeout |Saniye cinsinden zaman, varsayılan 2 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. İstemci ağ geçidine bir bağlantı açmaya çalıştığında her zaman için bağlantı zaman aralığı.|
|HealthOperationTimeout |Saniye cinsinden zaman, varsayılan süre 120'dir |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Sağlık Yöneticisi'ne gönderilen rapor iletisinin zaman ası. |
|SağlıkReportRetrySendInterval |Saniye cinsinden zaman, varsayılan değer 30, en az 1 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Raporlama bileşeninin birikmiş sağlık raporlarını Sağlık Yöneticisi'ne yeniden gönderme aralığı. |
|HealthReportSendInterval |Saniye cinsinden zaman, varsayılan 30 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Raporlama bileşeninin birikmiş sağlık raporlarını Sağlık Yöneticisi'ne gönderdiği aralık. |
|KeepAliveIntervalinSeconds |Int, varsayılan 20 |Statik|FabricClient transport'un ağ geçidine canlı mesajlar gönderdiği aralık. 0 için; keepAlive devre dışı bırakılır. Pozitif bir değer olmalı. |
|MaxFileSenderThreads |Uint, varsayılan 10 |Statik|Paralel olarak aktarılan dosyaların maksimum sayısı. |
|DüğümAdresleri |dize, varsayılan "" |Statik|Adlandırma Hizmeti ile iletişim kurmak için kullanılabilecek farklı düğümlerde adresler (bağlantı dizeleri) topluluğu. Başlangıçta İstemci, adreslerden birini rasgele seçerek bağlanır. Bir iletişim veya zaman acısı hatası nedeniyle birden fazla bağlantı dizesi sağlanmışsa ve bağlantı başarısız olursa; İstemci bir sonraki adresi sırayla kullanmak için geçer. Yeniden deneme semantikhakkında ayrıntılar için Adlandırma Hizmeti Adresi yeniden deneme bölümüne bakın. |
|PartitionLocationCacheLimit |Int, varsayılan 100000 |Statik|Hizmet çözünürlüğü için önbelleğe alınan bölüm sayısı (limitsiz olarak 0 olarak ayarlanır). |
|RetryBackoffInterval |Saniye cinsinden zaman, varsayılan 3 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. İşlemi yeniden denemeden önce geri dönüş aralığı. |
|ServiceChangePollInterval |Saniye cinsinden zaman, varsayılan süre 120'dir |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Hizmet değişiklikleri için art arda yapılan anketler arasındaki aralık istemciden kayıtlı hizmet değişikliği bildirimleri geri aramaları için ağ geçidine kadar. |

## <a name="fabrichost"></a>FabricHost

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|ActivationMaxFailureCount |Int, varsayılan 10 |Dinamik|Bu, sistemin vazgeçmeden önce başarısız etkinleştirmeyi yeniden deneyeceği maksimum sayımdır. |
|AktivasyonMaxRetryInterval |Saniye cinsinden zaman, varsayılan 300 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Etkinleştirme için maksimum yeniden deneme aralığı. Her sürekli başarısızlıkta yeniden deneme aralığı Min (ActivationMaxRetryInterval) olarak hesaplanır; Sürekli Başarısızlık Sayısı * ActivationRetryBackoffInterval). |
|AktivasyonRetryBackoffInterval |Saniye cinsinden zaman, varsayılan 5 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Her etkinleştirme hatasında geri tepme aralığı;Her sürekli etkinleştirme hatasında sistem etkinleştirmeyi MaxActivationFailureCount'a kadar yeniden dener. Her denemede yeniden deneme aralığı sürekli etkinleştirme hatası ve etkinleştirme geri dönüş aralığının bir ürünüdür. |
|EtkinleştirmeRestartManagement |Bool, varsayılan yanlış |Dinamik|Bu, sunucunun yeniden başlatılmasını etkinleştirmek içindir. |
|EnableServiceFabricOtomatik Güncellemeler |Bool, varsayılan yanlış |Dinamik|Bu, Windows Update ile kumaş otomatik güncelleştirmeyi etkinleştirmek içindir. |
|EnableServiceFabricBaseUpgrade |Bool, varsayılan yanlış |Dinamik|Bu, sunucu için temel güncelleştirmeyi etkinleştirmek içindir. |
|FailureReportingExpeditedReportingIntervalEnabled | Bool, varsayılan doğrudur | Statik | FabricHost Hata Raporlama modundayken DCA'da daha hızlı yükleme hızları sağlar. |
|FailureReportingTimeout | TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(60) | Statik |Zaman açıklığı saniye cinsinden belirtin. Durumda FabricHost erken aşama başlangıç hatası karşılaşır durumda DCA başarısızlık raporlama için zaman out. | 
|RunDCAOnStartupFailure | Bool, varsayılan doğrudur | Statik |FabricHost'ta başlangıç sorunlarıyla karşılaştığında günlükleri yüklemek için DCA'yı başlatıp başlatmayacağını belirler. | 
|Başlangıç Zamanı |Saniye cinsinden zaman, varsayılan 300 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Fabricactivationmanager başlangıç için zaman dışarı. |
|StopTimeout |Saniye cinsinden zaman, varsayılan 300 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Barındırılan hizmet etkinleştirme için zaman adabı; devre dışı bırakma ve yükseltme. |

## <a name="fabricnode"></a>FabricNode

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|ClientAuthX509FindType |dize, varsayılan "FindByThumbprint" |Dinamik|ClientAuthX509StoreName Tarafından belirlenen mağazada sertifika aramanın nasıl yapılacağını gösterir: FindByThumbprint; Findbysubjectname. |
|ClientAuthX509FindValue |dize, varsayılan "" | Dinamik|Varsayılan yönetici rolü FabricClient için sertifikayı bulmak için kullanılan arama filtresi değeri. |
|ClientAuthX509FindValueSecondary |dize, varsayılan "" |Dinamik|Varsayılan yönetici rolü FabricClient için sertifikayı bulmak için kullanılan arama filtresi değeri. |
|ClientAuthX509StoreName |dize, varsayılan "Benim" |Dinamik|Varsayılan yönetici rolü FabricClient için sertifika içeren X.509 sertifika deposunun adı. |
|ClusterX509FindType |dize, varsayılan "FindByThumbprint" |Dinamik|ClusterX509StoreName Desteklenen değerlerle belirtilen mağazada küme sertifikası aramanın nasıl yapılacağını gösterir: "FindByThumbprint"; "Findbysubjectname" ile "findbysubjectname"; birden fazla eşleşme olduğunda; en uzak süre sonuna sahip olan kullanılır. |
|ClusterX509FindValue |dize, varsayılan "" |Dinamik|Küme sertifikasını bulmak için kullanılan arama filtresi değeri. |
|ClusterX509FindValueİkincil |dize, varsayılan "" |Dinamik|Küme sertifikasını bulmak için kullanılan arama filtresi değeri. |
|ClusterX509StoreName |dize, varsayılan "Benim" |Dinamik|Küme içi iletişimi güvence altına almak için küme sertifikası içeren X.509 sertifika deposunun adı. |
|EndApplicationPortRange |Int, varsayılan 0 |Statik|Alt sistemi barındırarak yönetilen uygulama bağlantı noktalarının sonu (dahil) yoktur. Hosting'de EndpointFilteringEnabled doğruysa gereklidir. |
|ServerAuthX509FindType |dize, varsayılan "FindByThumbprint" |Dinamik|ServerAuthX509StoreName Tarafından belirlenen mağazada sunucu sertifikasının nasıl aranacağı nızı gösterir: FindByThumbprint; Findbysubjectname. |
|ServerAuthX509FindValue |dize, varsayılan "" |Dinamik|Sunucu sertifikasını bulmak için kullanılan arama filtresi değeri. |
|ServerAuthX509FindValueSecondary |dize, varsayılan "" |Dinamik|Sunucu sertifikasını bulmak için kullanılan arama filtresi değeri. |
|ServerAuthX509MağazaAdı |dize, varsayılan "Benim" |Dinamik|Giriş hizmeti için sunucu sertifikası içeren X.509 sertifika deposunun adı. |
|StartApplicationPortRange |Int, varsayılan 0 |Statik|Alt sistem barındırılarak yönetilen uygulama bağlantı noktalarının başlatılması. Hosting'de EndpointFilteringEnabled doğruysa gereklidir. |
|StateTraceInterval |Saniye cinsinden zaman, varsayılan 300 |Statik|Zaman açıklığı saniye cinsinden belirtin. FM/FMM'deki her düğüm ve yukarı düğümdüğümde düğüm durumunu izleme aralığı. |
|UserRoleClientX509FindType |dize, varsayılan "FindByThumbprint" |Dinamik|UserRoleClientX509StoreName Tarafından belirlenen mağazada sertifika aramanın nasıl yapılacağını gösterir: FindByThumbprint; Findbysubjectname. |
|UserRoleClientX509FindValue |dize, varsayılan "" |Dinamik|Varsayılan kullanıcı rolü FabricClient için sertifikayı bulmak için kullanılan arama filtresi değeri. |
|UserRoleClientX509FindValueSecondary |dize, varsayılan "" |Dinamik|Varsayılan kullanıcı rolü FabricClient için sertifikayı bulmak için kullanılan arama filtresi değeri. |
|UserRoleClientX509StoreName |dize, varsayılan "Benim" |Dinamik|Varsayılan kullanıcı rolü FabricClient için sertifika içeren X.509 sertifika deposunun adı. |

## <a name="failovermanager"></a>FailoverManager

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|AllowNodeStateRemovedForSeedNode|Bool, varsayılan FALSE olduğunu |Dinamik|Tohum düğümü için düğüm durumunun kaldırılmasına izin verilip verilmediğini belirtmek için bayrak |
|BuildReplicaTimeLimit|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(3600)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Durumlu bir yineleme oluşturmak için zaman sınırı; daha sonra bir uyarı sağlık raporu başlatılacaktır |
|ClusterPause Eşik|int, varsayılan 1|Dinamik|Sistemdeki düğüm sayısı bu değerin altına iniyorsa, yerleştirme; yük dengelemesi; ve failover durdurulur. |
|CreateInstanceTimeLimit|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(300)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Durumsuz bir örnek oluşturmak için zaman sınırı; daha sonra bir uyarı sağlık raporu başlatılacaktır |
|Beklenen ClusterSize Boyut|int, varsayılan 1|Dinamik|Küme ilk başlatıldığında; FM, diğer hizmetleri yerleştirmeye başlamadan önce bu sayıda düğümün kendilerini bildirmesini bekleyecektir; adlandırma gibi sistem hizmetleri de dahil olmak üzere. Bu değeri artırmak, kümenin başlatılması için gereken süreyi artırır; ancak erken düğümlerin aşırı yüklenmesini ve aynı zamanda daha fazla düğüm çevrimiçi olarak gerekli olacak ek hamleleri önler. Bu değer genellikle ilk küme boyutunun küçük bir kısmını ayarlanmalıdır. |
|ExpectedNodeDeactivationDuration|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(60,0 \* 30)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Bu, bir düğümün devre dışı bırakımı tamamlamak için beklenen süredir. |
|ExpectedNodeFabricUpgradeDuration|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(60,0 \* 30)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Bu, Windows Fabric yükseltmesi sırasında bir düğümün yükseltilmesi için beklenen süredir. |
|Beklenen ÇoğaltmaYükseltme Süresi|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(60,0 \* 30)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Bu, tüm yinelemelerin uygulama yükseltmesi sırasında bir düğümüzerinde yükseltilmesi için beklenen süredir. |
|IsSingletonReplicaMoveAllowedYükseltme Sırasında|bool, varsayılan DOĞRU|Dinamik|Eğer doğru ayarlanırsa; 1 hedef çoğaltma kümesi boyutuna sahip yinelemelerin yükseltme sırasında taşınmasına izin verilir. |
|MinReplicaSetSize|int, varsayılan 3|İzin Verilmiyor|Bu, FM için minimum çoğaltma kümesi boyutudur. Etkin FM kopyalarının sayısı bu değerin altına düşerse; FM, en az min kopya sayısı kurtarılana kadar kümedeki değişiklikleri reddeder |
|YerleşimKısıtlamalar|dize, varsayılan ""|İzin Verilmiyor|Başarısız yönetici yinelemeleri için herhangi bir yerleşim kısıtlaması |
|YerleştirmeZamanıLimit|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(600)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Hedef çoğaltma sayısına ulaşmak için zaman sınırı; daha sonra bir uyarı sağlık raporu başlatılacaktır |
|ÇoğunlukLossWaitDuration |Saniye cinsinden zaman, varsayılan MaxValue olduğunu |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Bu, bir bölümün çoğunluk kaybı durumunda olmasını izin verdiğimiz maksimum süredir. Bu süreden sonra bölünme hala çoğunluk kaybı nda ise; alt kopyaları kayıp olarak dikkate alınarak çoğunluk kaybından bölümlenir. Bunun veri kaybına neden olabileceğini unutmayın. |
|ReconfigurationTimeLimit|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(300)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Yeniden yapılandırma için zaman sınırı; daha sonra bir uyarı sağlık raporu başlatılacaktır |
|ÇoğaltmaRestartWaitDuration|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(60,0 \* 30)|İzin Verilmiyor|Zaman açıklığı saniye cinsinden belirtin. Bu FMService için ReplicaRestartWaitDuration olduğunu |
| SeedNodeQuorumAdditionalBufferNodes | int, varsayılan 0 | Dinamik | Yukarı olması gereken tohum düğümlerinin tamponu (tohum düğümlerinin çoğunluğu ile birlikte) FM en fazla (totalNumSeedNodes - (seedNodeQuorum + SeedNodeQuorum + SeedNodeQuorumAdditionalBufferNodes)) tohum düğümlerinin aşağı inmesine izin vermelidir. |
|StandbyReplicaKeepDuration|Zaman aralığı, varsayılan \* yaygındır::TimeSpan::FromSeconds(3600.0 \* 24 7)|İzin Verilmiyor|Zaman açıklığı saniye cinsinden belirtin. Bu FMService için StandByReplicaKeepDuration olduğunu |
|TargetReplicaSetSize|int, varsayılan 7|İzin Verilmiyor|Bu, Windows Fabric'in koruyacağı FM yinelemelerinin hedef sayısıdır. Daha yüksek bir sayı, FM verilerinin daha yüksek güvenilirliğine neden olabilir; küçük bir performans tradeoff ile. |
|UsermaxStandbyReplicaSayısı |Int, varsayılan 1 |Dinamik|Sistemin kullanıcı hizmetleri için tuttuğu varsayılan maksimum StandBy yineleme sayısı. |
|UserReplicaRestartWaitDuration |Saniye cinsinden zaman, varsayılan 60,0 \* 30 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Kalıcı bir yineleme indiğinde; Windows Fabric, yeni değiştirme yinelemeleri oluşturmadan önce yinelemenin geri gelmesi için bu süreyi bekler (bu da durum bir kopyasını gerektirir). |
|UserStandbyReplicaKeepDuration |Saniye cinsinden zaman, varsayılan 3600,0 \* 24 \* 7 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Kalıcı bir yineleme aşağı bir durumdan geri geldiğinde; zaten değiştirilmiş olabilir. Bu zamanlayıcı, FM'in atmadan önce bekleme yinelemesini ne kadar tutacağını belirler. |

## <a name="faultanalysisservice"></a>Arıza Analizi Hizmeti

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|TamamlanmışEylemKeepDurationinSeconds | Int, varsayılan 604800 |Statik| Bu, terminal durumunda ki eylemleri yaklaşık olarak ne kadar süreyle tutmaktır. Bu da StoredActionCleanupIntervalInSeconds bağlıdır; temizleme işi sadece bu aralıkta yapıldığından. 604800 7 gündür. |
|DataLossCheckPollIntervalIntervalInSeconds|int, varsayılan 5|Statik|Bu, veri kaybının gerçekleşmesini beklerken sistemin gerçekleştirdiği denetimler arasındaki süredir. Veri kaybı numarasının dahili yineleme başına kaç kez kontrol ediliş sayısı DataLossCheckWaitDurationInSeconds/this'dir. |
|DataLossCheckWaitDurationInSeconds|int, varsayılan 25|Statik|Toplam süre; saniye ler içinde; sistem veri kaybı nın gerçekleşmesini bekleyecek. Bu, StartPartitionDataLossAsync() api çağrıldığında dahili olarak kullanılır. |
|MinReplicaSetSize |Int, varsayılan 0 |Statik|HataAnalysisService için MinReplicaSetSize. |
|YerleşimKısıtlamalar | dize, varsayılan ""|Statik| FaultAnalysisService için Yerleşim Kısıtlamaları. |
|ÇoğunlukLossWaitDuration | Saniye cinsinden zaman, varsayılan MaxValue olduğunu |Statik|Zaman açıklığı saniye cinsinden belirtin. FaultAnalysisService için QuorumLossWaitDuration. |
|ReplicaDropWaitDurationInSeconds|int, varsayılan 600|Statik|Bu parametre, veri kaybı apis çağrıldığında kullanılır. Bu sistem, çoğaltma kaldırmak içten çağrıldısonra bir yineleme nin düşmesi için ne kadar bekleyeceğini denetler. |
|ÇoğaltmaRestartWaitDuration |Saniye cinsinden süre, varsayılan süre 60 dakikadır|Statik|Zaman açıklığı saniye cinsinden belirtin. ReplicaRestartWaitDuration FaultAnalysisService için. |
|StandbyReplicaKeepDuration| Saniye cinsinden zaman, varsayılan (60*24*7) dakikadır |Statik|Zaman açıklığı saniye cinsinden belirtin. StandByReplicaKeepDuration FaultAnalysisService için. |
|DepolanmışActionCleanupIntervalInSeconds | Int, varsayılan 3600 |Statik|Mağaza bu sıklıkta temizlenir. Yalnızca terminal durumundaki eylemler; ve en az TamamlanmışEylemKeepDurationInSeconds önce tamamlanmış kaldırılacak. |
|DepolananChaosEventCleanupIntervalInSeconds | Int, varsayılan 3600 |Statik|Mağazanın temizlik için ne sıklıkta denetleneceği dir; olay sayısı 30000'den fazla ise; temizlik başlayacak. |
|TargetReplicaSetSize |Int, varsayılan 0 |Statik|NOT_PLATFORM_UNIX_START FaultAnalysisService için TargetReplicaSetSize. |

## <a name="federation"></a>Federasyon

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Kiralama Süresi |Saniye cinsinden zaman, varsayılan 30 |Dinamik|Bir kira nın düğüm le komşuları arasında sürdüğü süre. |
|KiralamaDurationAcrossFaultDomain |Saniye cinsinden zaman, varsayılan 30 |Dinamik|Bir kiranın hata etki alanları arasında bir düğüm le komşuları arasında sürdüğü süre. |

## <a name="filestoreservice"></a>FileStoreService

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|AcceptChunkUpload|Bool, varsayılan DOĞRU|Dinamik|Dosya deposu hizmetinin kopya uygulama paketi sırasında yığın tabanlı dosya yüklemesini kabul edip etmediğini belirlemek için config. |
|Anonim Erişim Etkin | Bool, varsayılan doğrudur |Statik|FileStoreService paylaşımlarına anonim erişimi etkinleştirin/devre dışı kaltın. |
|CommonName1Ntlmx509CommonName|dize, varsayılan ""|Statik| NTLM kimlik doğrulaması kullanırken CommonName1NtlmPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının ortak adı |
|CommonName1Ntlmx509MağazaKonum|dize, varsayılan "LocalMachine"|Statik|NTLM kimlik doğrulaması kullanırken CommonName1NtlmPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının mağaza konumu |
|CommonName1Ntlmx509StoreName|dize, varsayılan "MY"| Statik|NTLM kimlik doğrulaması kullanırken CommonName1NtlmPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının mağaza adı |
|CommonName2Ntlmx509CommonName|dize, varsayılan ""|Statik|NTLM kimlik doğrulaması kullanırken CommonName2NtlmPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının ortak adı |
|CommonName2Ntlmx509MağazaKonum|dize, varsayılan "LocalMachine"| Statik|NTLM kimlik doğrulaması kullanırken CommonName2NtlmPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının mağaza konumu |
|CommonName2Ntlmx509MağazaAdı|dize, varsayılan "MY"|Statik| NTLM kimlik doğrulaması kullanırken CommonName2NtlmPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının mağaza adı |
|CommonNameNtlmPasswordSecret|SecureString, varsayılan ortak::SecureString("")| Statik|NTLM kimlik doğrulaması kullanırken aynı parolayı oluşturmada tohum olarak kullanılan parola sırrı |
|DiskSpaceHealthReportingIntervalWhenClosetooutdiskSpace |TimeSpan, varsayılan yaygındır::TimeSpan::FromMinutes(5)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Disk uzaydışına yakın olduğunda sistem durumu olayını bildirmek için disk alanının denetlemi arasındaki zaman aralığı. |
|DiskSpaceHealthReportingIntervalWhenEnoughDiskSpace |TimeSpan, varsayılan yaygındır::TimeSpan::FromMinutes(15)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Diskte yeterli alan olduğunda sistem durumu olayını bildirmek için disk alanının denetlemi arasındaki zaman aralığı. |
|EnableImageStoreSağlıkRaporlama |bool, varsayılan DOĞRU |Statik|Dosya deposu hizmetinin sistem durumunu bildirmesi gerekip gerekmediğini belirlemek için config. |
|ÜcretsizDiskSpaceNotificationSizeinkb|int64, varsayılan 25\*1024 |Dinamik|Aşağıda sistem durumu uyarısı nın gerçekleşebileceği boş disk alanının boyutu. Bu config ve FreeDiskSpaceNotificationThresholdeksi minimum değeri sağlık uyarısı gönderme belirlemek için kullanılır. |
|FreeDiskSpaceBildirimThresholdPercentage|çift, varsayılan 0,02 |Dinamik|Sistem durumu uyarısının gerçekleşebileceği boş disk alanı yüzdesi. Bu config ve FreeDiskSpaceNotificationINMB config minimum değeri sağlık uyarısı gönderme belirlemek için kullanılır. |
|OluşturmaV1CommonNameAccount| bool, varsayılan DOĞRU|Statik|Kullanıcı adı V1 oluşturma algoritmasına sahip bir hesap oluşturup oluşturmayacağını belirtir. Service Fabric sürüm 6.1 ile başlayarak; v2 nesilli bir hesap her zaman oluşturulur. V1 hesabı, V2 üretimini desteklemeyen sürümlerden /için yükseltmeler için gereklidir (6.1'den önce).|
|MaxCopyOperationThreads | Uint, varsayılan 0 |Dinamik| İkincil birincil kopyalayabilirsiniz paralel dosyaların maksimum sayısı. '0' == çekirdek sayısı. |
|MaxFileOperationThreads | Uint, varsayılan 100 |Statik| Birincil dosya işlemleri (Kopyalama/Taşıma) gerçekleştirmek için izin verilen en fazla paralel iş parçacığı sayısı. '0' == çekirdek sayısı. |
|MaxRequestProcessingThreads | Uint, varsayılan 200 |Statik|Birincil istekleri işlemek için izin verilen en fazla paralel iş parçacığı sayısı. '0' == çekirdek sayısı. |
|MaxSecondaryFilecopyFailureThreshold | Uint, varsayılan 25|Dinamik|Vazgeçmeden önce ikincil dosya kopyalama yeniden denemelerinin maksimum sayısı. |
|MaxStore İşlemleri | Uint, varsayılan 4096 olduğunu |Statik|Birincil izin verilen en fazla paralel depo hareket işlemleri sayısı. '0' == çekirdek sayısı. |
|AdlandırmaOperationTimeout |Saniye cinsinden zaman, varsayılan 60 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Adlandırma işlemini gerçekleştirmek için zaman ası. |
|Birincil AccountNTLMPasswordSecret | SecureString, varsayılan boş |Statik| NTLM kimlik doğrulaması kullanırken aynı parolayı kullanmak için tohum olarak kullanılan parola sırrı. |
|PrimaryAccountNTLMX509StoreLocation | dize, varsayılan "LocalMachine"|Statik| NTLM kimlik doğrulaması kullanılırken PrimaryAccountNTLMPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının mağaza konumu. |
|PrimaryAccountNTLMX509StoreName | dize, varsayılan "MY"|Statik| NTLM kimlik doğrulaması kullanılırken PrimaryAccountNTLMPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının mağaza adı. |
|PrimaryAccountNTLMX509Thumbprint | dize, varsayılan ""|Statik|NTLM kimlik doğrulaması kullanılırken PrimaryAccountNTLMPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının parmak izi. |
|Birincil Hesap Türü | dize, varsayılan "" |Statik|ACL Için anapara birincil Hesap Türü FileStoreService hisse. |
|Birincil HesapKullanıcı Adı | dize, varsayılan "" |Statik|ACL için ana para öncesi Kullanıcı adı FileStoreService hisse. |
|Birincil HesapKullanıcı Şifresi | SecureString, varsayılan boş |Statik|Anaparanın ACL'ye ait birincil hesap parolası FileStoreService paylaşımları. |
|QueryOperationTimeout | Saniye cinsinden zaman, varsayılan 60 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Sorgu işlemini gerçekleştirmek için zaman arası. |
|İkincil AccountNTLMPasswordSecret | SecureString, varsayılan boş |Statik| NTLM kimlik doğrulaması kullanırken aynı parolayı kullanmak için tohum olarak kullanılan parola sırrı. |
|IkincilAccountNTLMX509StoreLocation | dize, varsayılan "LocalMachine" |Statik|NTLM kimlik doğrulaması kullanılırken SecondaryAccountNTLMPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının mağaza konumu. |
|İkincilAccountNTLMX509StoreName | dize, varsayılan "MY" |Statik|NTLM kimlik doğrulaması kullanılırken SecondaryAccountNTLMPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının mağaza adı. |
|İkincilAccountNTLMX509Thumbprint | dize, varsayılan ""| Statik|NTLM kimlik doğrulaması kullanılırken SecondaryAccountNTLMPasswordSecret'ta HMAC oluşturmak için kullanılan X509 sertifikasının parmak izi. |
|IkincilHesap Türü | dize, varsayılan ""|Statik| ACL Için anapara ikincil Hesap Türü FileStoreService hisse. |
|IkincilHesapKullanıcı Adı | dize, varsayılan ""| Statik|ACL için ana para müdürü ikincil hesap Kullanıcı adı FileStoreService hisse. |
|Ikincil HesapKullanıcı Şifresi | SecureString, varsayılan boş |Statik|Anaparanın ACL'ye ikincil hesap şifresi FileStoreService paylaşımları. |
|İkincilFileCopyRetryDelayMilliseconds|uint, varsayılan 500|Dinamik|Dosya kopyalama yeniden deneme gecikmesi (milisaniye cinsinden).|
|UseChunkContentInTransportMessageMessage|bool, varsayılan DOĞRU|Dinamik|V6.4'te tanıtılan yükleme protokolünün yeni sürümünü kullanmak için bayrak. Bu protokol sürümü, önceki sürümlerde kullanılan SMB protokolünden daha iyi performans sağlayan dosyaları resim deposuna yüklemek için hizmet dokusu taşımayı kullanır. |

## <a name="healthmanager"></a>HealthManager

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|EnableApplicationTypeHealthEvaluation |Bool, varsayılan yanlış |Statik|Küme durumu değerlendirme ilkesi: uygulama türü başına sistem durumu değerlendirmesini etkinleştirin. |
|MaxÖnerilenNumberOfEntityHealthReports|Int, varsayılan 100 |Dinamik|İzleme örgütünün sağlık raporlama mantığı yla ilgili endişeleri dile getirmeden önce bir varlığın sahip olabileceği maksimum sağlık raporu sayısı. Her sağlık kuruluşunun nispeten az sayıda sağlık raporuna sahip olması gerekir. Rapor sayısı bu sayının üzerinde yse; izleme örgütünün uygulanmasıyla ilgili sorunlar olabilir. Çok fazla raporu olan bir varlık, varlık değerlendirildiğinde uyarı sağlık raporu aracılığıyla işaretlenir. |

## <a name="healthmanagerclusterhealthpolicy"></a>HealthManager/ClusterHealthPolicy

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|WarningasError'ı Düşünün |Bool, varsayılan yanlış |Statik|Küme durumu değerlendirme ilkesi: uyarılar hata olarak kabul edilir. |
|MaxPercentSağlıksızApplications | Int, varsayılan 0 |Statik|Küme durumu değerlendirme ilkesi: Kümenin sağlıklı olması için izin verilen sağlıksız uygulamaların maksimum yüzdesi. |
|MaxPercentUnhealthyDüğümler | Int, varsayılan 0 |Statik|Küme durumu değerlendirme ilkesi: Kümenin sağlıklı olması için izin verilen sağlıksız düğümlerin maksimum yüzdesi. |

## <a name="healthmanagerclusterupgradehealthpolicy"></a>HealthManager/ClusterUpgradeHealthPolicy

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|MaxPercentDeltaUnhealthyNodes|int, varsayılan 10|Statik|Küme yükseltme sistem durumu değerlendirme ilkesi: kümenin sağlıklı olması için izin verilen delta sağlıksız düğümlerin maksimum yüzdesi |
|MaxPercentUpgradeDomainDeltaUnhealthyNodes|int, varsayılan 15|Statik|Küme yükseltme sistem durumu değerlendirme ilkesi: Bir yükseltme etki alanında sağlıksız düğümlerin delta maksimum yüzdesi kümenin sağlıklı olması için izin |

## <a name="hosting"></a>Barındırma

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|ActivationMaxFailureCount |Tam sayı, varsayılan 10 |Dinamik|Vazgeçmeden önce sistem yeniden denemelerinin kaç kez etkinleştirilmeden önce başarısız olduğu |
|AktivasyonMaxRetryInterval |Saniye cinsinden zaman, varsayılan 300 |Dinamik|Her sürekli etkinleştirme hatasında, sistem aktivasyonu ActivationMaxFailureCount'a kadar yeniden dener. ActivationMaxRetryInterval her etkinleştirme hatasından sonra yeniden denemeden önce bekleme süresini belirtir |
|AktivasyonRetryBackoffInterval |Saniyecinsinden zaman, varsayılan değer 5'tir |Dinamik|Her etkinleştirme hatasında geri dönüş aralığı; Her sürekli etkinleştirme hatasında, sistem etkinleştirmeyi MaxActivationFailureCount'a kadar yeniden dener. Her denemede yeniden deneme aralığı sürekli etkinleştirme hatası ve etkinleştirme geri dönüş aralığının bir ürünüdür. |
|EtkinleştirmeTimeout| TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(180)|Dinamik| Zaman açıklığı saniye cinsinden belirtin. Uygulama etkinleştirme için zaman adabı; devre dışı bırakma ve yükseltme. |
|UygulamaHostCloseTimeout| TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(120)|Dinamik| Zaman açıklığı saniye cinsinden belirtin. Kumaş çıkışı kendi kendine etkinleştirilen işlemlerde algılandığında; FabricRuntime, kullanıcının ana bilgisayar (applicationhost) işlemindeki tüm kopyaları kapatır. Bu, yakın işlemin zaman amıdır. |
| CnsNetworkPluginCnmUrlPort | wstring, varsayılan L"48080" | Statik | Azure cnm api url bağlantı noktası |
| CnsNetworkPluginCnsUrlPort | wstring, varsayılan L"10090" | Statik | Azure cns url bağlantı noktası |
|Konteyner Hizmet Bağımsız Değişkenleri|dize, varsayılan "-H localhost:2375 -H npipe://"|Statik|Service Fabric (SF) docker daemon yönetir (Win10 gibi windows istemci makineleri hariç). Bu yapılandırma, kullanıcının başlatırken docker daemon'a geçirilmesi gereken özel bağımsız değişkenleri belirtmesine olanak tanır. Özel bağımsız değişkenler belirtildiğinde, Service Fabric '--pidfile' bağımsız değişkeni dışında Docker altyapısına başka bir bağımsız değişken geçmez. Bu nedenle kullanıcılar müşteri bağımsız değişkenlerinin bir parçası olarak '--pidfile' bağımsız değişkenini belirtmemelidir. Ayrıca, özel bağımsız değişkenler, Service Fabric'in onunla iletişim kurabilmesi için Docker Daemon'un Windows'daki varsayılan ad borusunu (veya Linux'taki Unix etki alanı soketi) dinlemesini sağlamalıdır.|
|KonteynerServiceLogFileMaxSizeInKb|int, varsayılan 32768 olduğunu|Statik|Docker kapsayıcıları tarafından oluşturulan günlük dosyasının maksimum dosya boyutu.  Yalnızca Windows.|
|KonteynerImageDownloadTimeout|int, saniye sayısı, varsayılan 1200 (20 dakika)|Dinamik|Görüntü nün indirilmeden önceki saniye sayısı.|
|ContainerImagesToSkip|dize, dikey çizgi karakteri ile ayrılmış görüntü adları, varsayılan ""|Statik|Silinmemesi gereken bir veya daha fazla kapsayıcı görüntüsünün adı.  PruneContainerImages parametresi ile kullanılır.|
|KonteynerServiceLogFileNamePrefix|dize, varsayılan "sfcontainerlogs"|Statik|Docker kapsayıcıları tarafından oluşturulan günlük dosyaları için dosya adı öneki.  Yalnızca Windows.|
|ContainerServiceLogFileRetentionCount|int, varsayılan 10|Statik|Günlük dosyaları üzerine yazılmadan önce docker kapsayıcıları tarafından oluşturulan günlük dosyalarının sayısı.  Yalnızca Windows.|
|CreateFabricRuntimeTimeout|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(120)|Dinamik| Zaman açıklığı saniye cinsinden belirtin. Eşitleme FabricCreateRuntime arama için zaman zaman kaybı değeri |
|DefaultContainerRepositoryHesap Adı|dize, varsayılan ""|Statik|ApplicationManifest.xml'de belirtilen kimlik bilgileri yerine kullanılan varsayılan kimlik bilgileri |
|VarsayılanKonteynerRepositoryPassword|dize, varsayılan ""|Statik|ApplicationManifest.xml'de belirtilen kimlik bilgileri yerine kullanılan varsayılan parola kimlik bilgileri|
|VarsayılanKonteynerRepositoryPasswordType|dize, varsayılan ""|Statik|Dize boş olmadığında, değer "Şifreli" veya "SecretsStoreRef" olabilir.|
|DefaultDnsSearchSuffixBoş|bool, varsayılan FALSE olduğunu|Statik|Varsayılan olarak, hizmet adı kapsayıcı hizmetleri için SF DNS adına eklenir. Bu özellik, çözüm yolunda varsayılan olarak SF DNS adına hiçbir şeyin eklenmeyecek şekilde bu davranışı durdurur.|
|DeploymentMaxFailureCount|int, varsayılan 20| Dinamik|Uygulama dağıtımı, bu uygulamanın düğümüzerinde dağıtımını başarısız lığa uğratmadan önce DeploymentMaxFailureCount süreleri için yeniden denenecektir.| 
|DeploymentMaxRetryInterval| TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(3600)|Dinamik| Zaman açıklığı saniye cinsinden belirtin. Dağıtım için maksimum yeniden deneme aralığı. Her sürekli arızada yeniden deneme aralığı Min (DeploymentMaxRetryInterval) olarak hesaplanır; Sürekli Arıza Sayısı * DeploymentRetryBackoffInterval) |
|DeploymentRetryBackoffInterval| TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(10)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Dağıtım hatası için geri bekleme aralığı. Her sürekli dağıtım hatasında sistem, dağıtımı MaxDeploymentFailureCount'a kadar yeniden dener. Yeniden deneme aralığı, sürekli dağıtım hatasının ve dağıtım geri tepme aralığının bir ürünüdür. |
|Devre Dışı Konteynerler|bool, varsayılan FALSE olduğunu|Statik|Konteynerleri devre dışı bırakmak için Config - devre dışı konfig olan Devre Dışı BırakContainerServiceStartOnContainerActivatorOpen yerine kullanılır |
|Devre dışı KılabilirDockerRequestRetry|bool, varsayılan FALSE olduğunu |Dinamik| Varsayılan olarak SF, DD (docker dameon) ile ona gönderilen her bir http isteği için 'DockerRequestTimeout' zaman aşımı ile iletişim kurar. DD bu süre içinde yanıt vermezse; Üst düzey işlemin hala kalan zamanı varsa SF isteği yeniden gönderir.  Hyperv konteyner ile; DD bazen kapsayıcı getirmek veya devre dışı bırakmak için çok daha fazla zaman alır. Bu gibi durumlarda DD isteği SF perspektifinden zaman ları dışarı ve SF işlemi yeniden dener. Bazen bu DD üzerinde daha fazla baskı ekler gibi görünüyor. Bu config bu yeniden deneme devre dışı bırakıp DD'nin yanıt vermesiiçin beklemenizi sağlar. |
|DnsServerListTwoIps | Bool, varsayılan FALSE olduğunu | Statik | Bu bayraklar, aralıklı sorunları çözmeye yardımcı olmak için yerel dns sunucusuna iki kez ekler. |
| DoNotInjectLocalDnsServer | bool, varsayılan FALSE olduğunu | Statik | Kapsayıcılar için yerel IP'nin DNS sunucusu olarak enjekte edilebilmektedir. |
|EnableActivateNoWindow| bool, varsayılan FALSE olduğunu|Dinamik| Etkinleştirilen işlem arka planda herhangi bir konsol olmadan oluşturulur. |
|EnableContainerServiceDebugMode|bool, varsayılan DOĞRU|Statik|Docker konteynerleri için günlüğü etkinleştirin/devre dışı.  Yalnızca Windows.|
|EnableDockerHealthCheckIntegration|bool, varsayılan DOĞRU|Statik|Docker HEALTHCHECK etkinliklerinin Service Fabric sistem sağlık raporu ile entegrasyonunu sağlar |
|EtkinleştirmeProcessHata|bool, varsayılan FALSE olduğunu|Dinamik| Hata ayıklama altında uygulama ana bilgisayarlarının başlatılmasını sağlar |
|Uç NoktaSağlayıcıEtkin| bool, varsayılan FALSE olduğunu|Statik| Endpoint kaynaklarının Kumaş tarafından yönetilmesini sağlar. FabricNode'da başlangıç ve bitiş uygulama bağlantı noktası aralığının belirtimini gerektirir. |
|FabricContainerAppsEtkin| bool, varsayılan FALSE olduğunu|Statik| |
|FirewallPolicyEtkin|bool, varsayılan FALSE olduğunu|Statik| ServiceManifest'te belirtilen açık bağlantı noktalarıyla Uç Nokta kaynakları için güvenlik duvarı bağlantı noktalarının açılmasını sağlar |
|GetCodePackageActivationContextTimeout|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(120)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. CodePackageActivationContext çağrılarıiçin zaman ödeme değeri. Bu, geçici hizmetler için geçerli değildir. |
|GovernOnlyMainMemoryForProcesses|bool, varsayılan FALSE olduğunu|Statik|Kaynak Yönetişimi'nin varsayılan davranışı, MemoryInMB'de belirtilen sınırı, işlemin kullandığı toplam bellek miktarına (RAM + takas) koymaktır. Sınır aşılırsa; işlem OutOfMemory özel durum alırsınız. Bu parametre doğru olarak ayarlanmışsa; limit yalnızca bir işlemin kullanacağı RAM bellek miktarına uygulanır. Bu sınır aşılırsa; ve bu ayar doğruysa; sonra işletim sistemi ana belleği diske göre değiştirir. |
|IPSağlayıcı Etkin|bool, varsayılan FALSE olduğunu|Statik|IP adreslerinin yönetimini sağlar. |
|IsDefaultContainerRepositoryPasswordEncrypted|bool, varsayılan FALSE olduğunu|Statik|DefaultContainerRepositoryPassword şifrelenmiş olsun veya olmasın.|
|LinuxExternalExecutablePath|dize, varsayılan "/usr/bin/" |Statik|Düğümdeki dış çalıştırılabilir komutların birincil dizini.|
|NTLMAuthentication Etkin|bool, varsayılan FALSE olduğunu|Statik| Makineler arası işlemlerin güvenli bir şekilde iletişim kurabilmesini sağlamak için diğer kullanıcılar olarak çalışan kod paketleri tarafından NTLM kullanımı için destek sağlar. |
|NTLMAuthenticationPasswordSecret|SecureString, varsayılan ortak::SecureString("")|Statik|NTLM kullanıcıları için parola oluşturmak için kullanılan şifreli bir var mı. NTLMAuthenticationEnabled doğruysa ayarlanmalıdır. Konuşlayıcı tarafından doğrulandı. |
|NTLMSecurityUsersByX509CommonnamesRefreshInterval|TimeSpan, varsayılan yaygındır::TimeSpan::FromMinutes(3)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Ortama özel ayarlar Hosting'in FileStoreService NTLM yapılandırması için kullanılacak yeni sertifikaları taradığı periyodik aralık. |
|NTLMSecurityUsersByX509CommonNamesRefreshTimeout|TimeSpan, varsayılan yaygındır::TimeSpan::FromMinutes(4)|Dinamik| Zaman açıklığı saniye cinsinden belirtin. Sertifika ortak adlarını kullanarak NTLM kullanıcılarını yapılandırmak için zaman arası. FileStoreService hisseleri için NTLM kullanıcıları gereklidir. |
|PruneContainerImages|bool, varsayılan FALSE olduğunu|Dinamik| Kullanılmayan uygulama kapsayıcı görüntülerini düğümlerden kaldırın. Bir ApplicationType Service Fabric kümesinden kaydedilmemişse, bu uygulama tarafından kullanılan kapsayıcı görüntüleri Service Fabric tarafından indirildiği düğümlerde kaldırılır. Budama her saat çalışır, bu nedenle görüntülerin kümeden kaldırılması için bir saat (artı görüntüyü budamak için zaman) kadar sürebilir.<br>Service Fabric hiçbir zaman bir uygulamayla ilgili olmayan görüntüleri indirmez veya kaldırmaz.  El ile veya başka bir şekilde indirilen ilgisiz görüntülerin açıkça kaldırılması gerekir.<br>Silinmemesi gereken görüntüler ContainerImagesToSkip parametresinde belirtilebilir.| 
|RegisterCodePackageHostTimeout|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(120)|Dinamik| Zaman açıklığı saniye cinsinden belirtin. FabricRegisterCodePackageHost eşitleme çağrısıiçin zaman ödeme değeri. Bu FWP gibi sadece çok kodlu paket uygulama ana bilgisayarlar için geçerlidir |
|İstekZaman Süresi|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(30)|Dinamik| Zaman açıklığı saniye cinsinden belirtin. Bu, kullanıcının uygulama ana bilgisayarı ile fabrika kaydı gibi çeşitli barındırma işlemleri için Kumaş işlemi arasındaki iletişim zaman arasını temsil eder; runtime kaydı. |
|RunasPolicyEtkin| bool, varsayılan FALSE olduğunu|Statik| Kod paketlerini, kumaş işleminin yürüttüğü kullanıcı dışında yerel kullanıcı olarak çalıştırmayı sağlar. Bu ilkeyi etkinleştirmek için Fabric SİsteM olarak veya SeAssignPrimaryTokenPrivilege olan kullanıcı olarak çalışıyor olmalıdır. |
|ServiceFactoryRegistrationTimeout| TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(120)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Eşitleme Kaydı(Stateless/Stateful)ServiceFactory çağrısı için zaman zaman kaybı değeri |
|ServiceTypeDisableFailureThreshold |Tam sayı, varsayılan 1 |Dinamik|Bu, FailoverManager'a (FM) bu düğümdeki servis türünü devre dışı bırakıp yerleşim için farklı bir düğüm denemesi için bildirimde bulunduğu hata sayısı için eşiktir. |
|ServiceTypeDisableGraceInterval|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(30)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Hizmet türünün devre dışı edilebildiği zaman aralığı |
|ServiceTypeRegistrationTimeout |Saniye cinsinden zaman, varsayılan değer 300 |Dinamik|ServiceType'ın kumaşa kaydedilmesi için izin verilen maksimum süre |
|UseContainerServiceArguments|bool, varsayılan DOĞRU|Statik|Bu config docker daemon geçen bağımsız değişkenleri (config ContainerServiceArguments belirtilen) atlamak için barındırma söyler.|

## <a name="httpgateway"></a>Http://www.Gateway

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|ActiveListeners |Uint, varsayılan 50 |Statik| Http sunucu kuyruğuna nakletmek için okuma sayısı. Bu, HttpGateway tarafından karşılanabilecek eşzamanlı istek sayısını denetler. |
|httpağ geçidisağlık raporuSendInterval |Saniye cinsinden zaman, varsayılan 30 |Statik|Zaman açıklığı saniye cinsinden belirtin. Http Ağ Geçidi'nin birikmiş sağlık raporlarını Sağlık Yöneticisi'ne gönderdiği aralık. |
|HttpStrictTransportSecurityHeader|string,varsayılan ""|Dinamik| HttpGateway tarafından gönderilen her yanıta eklenecek HTTP Sıkı Aktarım Güvenliği üstbilgi değerini belirtin. Dize boş ayarlandığında; bu üstbilgi ağ geçidi yanıtına dahil edilmez.|
|IsEnabled|Bool, varsayılan yanlış |Statik| HttpGateway'i etkinleştiri/devre dışı kılabilir. HttpGateway varsayılan olarak devre dışı bırakılır. |
|MaxEntityBodySize |Uint, varsayılan 4194304 olduğunu |Dinamik|Bir http isteğinden beklenebilecek gövdenin maksimum boyutunu verir. Varsayılan değer 4MB'dur. Bu değere > bir boyut gövdesi varsa, httpgateway isteği başarısız olur. Minimum okuma parçası boyutu 4096 bayt. Yani bu >= 4096 olmalıdır. |

## <a name="imagestoreservice"></a>ImageStoreService

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Etkin |Bool, varsayılan yanlış |Statik|ImageStoreService için Etkin bayrak. Varsayılan: false |
|MinReplicaSetSize | Int, varsayılan 3 |Statik|ImageStoreService için MinReplicaSetSize. |
|YerleşimKısıtlamalar | dize, varsayılan "" |Statik| ImageStoreService için Yerleşim Kısıtlamaları. |
|ÇoğunlukLossWaitDuration | Saniye cinsinden zaman, varsayılan MaxValue olduğunu |Statik| Zaman açıklığı saniye cinsinden belirtin. ImageStoreService için QuorumLossWaitDuration. |
|ÇoğaltmaRestartWaitDuration | Saniye cinsinden zaman, varsayılan 60,0 \* 30 |Statik|Zaman açıklığı saniye cinsinden belirtin. ImageStoreService için ReplicaRestartWaitDuration. |
|StandbyReplicaKeepDuration | Saniye cinsinden zaman, varsayılan 3600,0 \* 2 |Statik| Zaman açıklığı saniye cinsinden belirtin. ImageStoreService için StandByReplicaKeepDuration. |
|TargetReplicaSetSize | Int, varsayılan 7 |Statik|ImageStoreService için TargetReplicaSetSize. |

## <a name="ktllogger"></a>KtlLogger

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Otomatik Bellek Yapılandırması |Int, varsayılan 1 |Dinamik|Bellek ayarlarının otomatik ve dinamik olarak yapılandırılması gerektiğini belirten bayrak. Sıfır ise bellek yapılandırma ayarları doğrudan kullanılır ve sistem koşullarına bağlı olarak değişmez. Eğer bir sonra bellek ayarları otomatik olarak yapılandırılır ve sistem koşullarına göre değişebilir. |
|MaksimumDestagingWriteOlağanüstüInKB | Int, varsayılan 0 |Dinamik|Paylaşılan günlüğün özel günlükten önce ilerlemesine izin verecek KB sayısı. Sınır yok belirtmek için 0'ı kullanın.
|SharedLogId |dize, varsayılan "" |Statik|Paylaşılan günlük kapsayıcısı için benzersiz kılavuz. Kumaş veri kökü altında varsayılan yolu kullanıyorsanız "" kullanın. |
|SharedLogPath |dize, varsayılan "" |Statik|Paylaşılan günlük kapsayıcısını yerleştirmek için konuma giden yol ve dosya adı. Kumaş veri kökü altında varsayılan yolu kullanmak için "" kullanın. |
|PaylaşılanLogSizeInMB |Int, varsayılan 8192 olduğunu |Statik|Paylaşılan günlük kapsayıcısına tahsis etmek için MB sayısı. |
|PaylaşılanLogThrotleLimitInPercentUsed|int, varsayılan 0 | Statik | Azaltma neden olacak paylaşılan günlük kullanım yüzdesi. Değer 0 ile 100 arasında olmalıdır. 0 değeri varsayılan yüzde değerini kullanmayı ima eder. 100 değeri hiçbir daralma anlamına gelir. 1 ile 99 arasındaki bir değer, azaltmanın gerçekleşeceği günlük kullanım yüzdesini belirtir; örneğin paylaşılan günlük 10GB ve değeri 90 ise, 9GB kullanımda olduğunda azaltma gerçekleşir. Varsayılan değerin kullanılması önerilir.|
|YazmaBufferMemoryPoolMaximumInKB | Int, varsayılan 0 |Dinamik|Yazma arabellek bellek havuzunun büyümesine izin verecek KB sayısı. Sınır yok belirtmek için 0'ı kullanın. |
|YazmaBufferMemoryPoolMinimumInKB |Int, varsayılan 8388608 olduğunu |Dinamik|Yazma arabelleği bellek havuzuiçin başlangıçta ayrılacak KB sayısı. Hiçbir limit Varsayılan göstermek için 0 kullanın Aşağıdaki SharedLogSizeInMB ile tutarlı olmalıdır. |

## <a name="managedidentitytokenservice"></a>YönetilenIdentityTokenService
| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|IsEnabled|bool, varsayılan FALSE olduğunu|Statik|Kümedeki Yönetilen Kimlik Belirteç Hizmeti'nin varlığını ve durumunu denetleyen bayrak;bu, Service Fabric uygulamalarının yönetilen kimlik işlevselliğini kullanmak için bir ön koşuldur.|

## <a name="management"></a>Yönetim

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|OtomatikUnprovisionInterval|TimeSpan, varsayılan yaygındır::TimeSpan::FromMinutes(5)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Otomatik uygulama türü temizleme sırasında kayıt dışı uygulama türü için izin verilen temizleme aralığı.|
|AzureStorageMaxConnections | Int, varsayılan 5000 |Dinamik|Azure depolamasına en fazla eşzamanlı bağlantı sayısı. |
|AzureStorageMaxWorkerThreads | Int, varsayılan 25 |Dinamik|Paralel olarak en fazla alt iş parçacığı sayısı. |
|AzureStorageOperationTimeout | Saniye cinsinden zaman, varsayılan 6000 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Xstore işleminin tamamlanması için zaman doldu. |
|TemizlemeApplicationPackageOnProvisionBaşarı|bool, varsayılan FALSE olduğunu |Dinamik|Başarılı bir şekilde uygulama paketinin otomatik olarak temizlenmesini sağlar veya devre dışı kılabilir. |
|TemizlemeKullanılmayanUygulama Türleri|Bool, varsayılan FALSE olduğunu |Dinamik|Bu yapılandırma etkinse, kullanılmayan uygulama türü sürümlerinin en son üç kullanılmayan sürümü atlayarak otomatik olarak kaydedilmesine olanak tanır ve böylece görüntü deposutarafından işgal edilen disk alanını kırpılır. Otomatik temizleme, söz konusu uygulama türü için başarılı bir sağlamanın sonunda tetiklenir ve tüm uygulama türleri için günde bir kez düzenli olarak çalışır. Atlamak için kullanılmayan sürümlerin sayısı parametresi kullanılarak yapılandırılabilir "MaxUnusedAppTypeVersionsToKeep". |
|Devre Dışı ChecksumValidation | Bool, varsayılan yanlış |Statik| Bu yapılandırma, uygulama sağlama sırasında checksum doğrulaması etkinleştirmemize veya devre dışı bırakmamıza olanak tanır. |
|Devre Dışı AtılabilirServerSideCopy | Bool, varsayılan yanlış |Statik|Bu yapılandırma, uygulama sağlama sırasında ImageStore'daki uygulama paketinin sunucu tarafındaki kopyasını etkinleştirir veya devre dışı kılabilir. |
|ImageCachingEtkin | Bool, varsayılan doğrudur |Statik|Bu yapılandırma önbelleğe alma etkinleştirmemizi veya devre dışı açmamızı sağlar. |
|ImageStoreConnectionString |Securestring |Statik|ImageStore için Root'a bağlantı dizesi. |
|ImageStoreMinimumTransferBPS | Int, varsayılan 1024 |Dinamik|Küme ve ImageStore arasındaki minimum aktarım hızı. Bu değer, harici ImageStore'a erişirken zaman aşımını belirlemek için kullanılır. Küme ile ImageStore arasındaki gecikme süresi yüksekse, kümenin harici ImageStore'dan indirebilmesi için daha fazla zaman tanımak için bu değeri değiştirin. |
|MaxUnusedAppTypeVersionsToKeep | Int, varsayılan 3 |Dinamik|Bu yapılandırma, temizleme için atlanacak kullanılmayan uygulama türü sürümlerinin sayısını tanımlar. Bu parametre yalnızca Temizleme KullanılmayanUygulama Türleri parametresi etkinse uygulanabilir. |


## <a name="metricactivitythresholds"></a>MetricActivityEşikler
| **Parametre** | **İzin Verilen Değerler** |**Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|KeyIntegerValueMap, varsayılan Yok|Dinamik|Kümedeki ölçümler için Metrik Etkinlik Eşikleri kümesini belirler. MaxNodeLoad MetricActivityThresholds daha büyükse dengeleme çalışacaktır. Defrag ölçümleri için, Servis Kumaşının düğümü boş olarak değerlendireceği yük miktarını tanımlar |

## <a name="metricbalancingthresholds"></a>Metrik Dengeleme Eşikleri
| **Parametre** | **İzin Verilen Değerler** |**Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|KeyDoubleValueMap, varsayılan Yok|Dinamik|Kümedeki ölçümler için Metrik Dengeleme Eşikleri kümesini belirler. MaxNodeLoad/minNodeLoad MetricBalancingThresholds'tan büyükse dengeleme çalışır. En az bir FD veya UD'deki maxNodeLoad/minNodeLoad MetricBalancingThresholds'tan daha küçükse parçalanma çalışır. |

## <a name="metricloadstickinessforswap"></a>MetricLoadStickinessForSwap
| **Parametre** | **İzin Verilen Değerler** |**Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|KeyDoubleValueMap, varsayılan Yok|Dinamik|Değiştirilende yinelemeye yapışan yükün bölümünü belirler 0 (yük çoğaltma ile yapışmaz) ve 1 (yinelemeli yük çubukları - varsayılan) arasında değer alır |

## <a name="namingservice"></a>Adlandırma Hizmeti

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Ağ GeçidiHizmetDescriptionCacheLimit |Int, varsayılan 0 |Statik|Adlandırma Ağ Geçidi'ndeki LRU hizmet açıklama önbelleğinde tutulan maksimum giriş sayısı (sınır için 0 olarak ayarlanır). |
|MaxClientConnections |Int, varsayılan 1000 |Dinamik|Ağ geçidi başına izin verilen maksimum istemci bağlantısı sayısı. |
|MaxFileOperationTimeout |Saniye cinsinden zaman, varsayılan 30 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Dosya deposu hizmeti işlemi için izin verilen maksimum zaman anına. Daha büyük bir zaman anına işaret eden istekler reddedilir. |
|MaxIndexedBoş Bölümler |Int, varsayılan 1000 |Dinamik|Yeniden bağlanma istemcilerini eşitlemek için bildirim önbelleğinde dizinlenmiş olarak kalacak en fazla boş bölüm sayısı. Bu sayının üzerindeki boş bölümler, artan arama sürümü sırasına göre dizinden kaldırılır. Yeniden bağlanma istemcileri yine de eşitleyebilir ve kaçırılan boş bölüm güncelleştirmelerini alabilir; ancak eşitleme protokolü daha pahalı hale gelir. |
|MaxMessageSize |Int, varsayılan\*4 1024\*1024 |Statik|Adlandırma kullanırken istemci düğümü iletişimi için maksimum ileti boyutu. DOS saldırı azaltma; varsayılan değer 4MB'dur. |
|MaxNamingServiceHealthReports | Int, varsayılan 10 |Dinamik|Adlandırma depolama hizmetinin aynı anda sağlıksız bildirdiği maksimum yavaş işlem sayısı. Eğer 0; tüm yavaş operasyonlar gönderilir. |
|MaxOperationTimeout |Saniye cinsinden zaman, varsayılan 600 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. İstemci işlemleri için izin verilen maksimum zaman anına. Daha büyük bir zaman anına işaret eden istekler reddedilir. |
|MaxOutstandingBildirimlerPerClient |Int, varsayılan 1000 |Dinamik|İstemci kaydından önce en fazla bekleyen bildirim sayısı ağ geçidi tarafından zorla kapatılır. |
|MinReplicaSetSize | Int, varsayılan 3 |İzin Verilmiyor| Güncelleştirmeyi tamamlamak için yazmak için gereken en az Adlandırma Hizmeti yineleme sayısı. Sistemde bu etkinden daha az yineleme varsa, Güvenilirlik Sistemi yinelemeler geri yüklenene kadar Adlandırma Hizmet Deposu'ndaki güncelleştirmeleri reddeder. Bu değer hiçbir zaman TargetReplicaSetSize'dan fazla olmamalıdır. |
|PartitionCount |Int, varsayılan 3 |İzin Verilmiyor|Oluşturulacak Adlandırma Hizmeti deposunun bölüm sayısı. Her bölüm kendi dizin karşılık gelen tek bir bölüm anahtarına sahip; bu yüzden bölüm tuşları [0; PartitionCount] var. Adlandırma Hizmeti bölümlerinin sayısını artırmak, herhangi bir destek yineleme kümesi tarafından tutulan ortalama veri miktarını azaltarak Adlandırma Hizmetinin gerçekleştirebileceği ölçeği artırır; kaynakların artan kullanımı pahasına (PartitionCount*ReplicaSetSize hizmet yinelemeleri korunmalıdır).|
|YerleşimKısıtlamalar | dize, varsayılan "" |İzin Verilmiyor| Adlandırma Hizmeti için yerleşim kısıtlaması. |
|ÇoğunlukLossWaitDuration | Saniye cinsinden zaman, varsayılan MaxValue olduğunu |İzin Verilmiyor| Zaman açıklığı saniye cinsinden belirtin. Bir Adlandırma Hizmeti çoğunluk kaybına girdiğinde; bu zamanlayıcı başlar. Süresi dolduğunda FM aşağı kopyaları kayıp olarak kabul edecektir; ve çoğunluk kurtarmak için çalışır. Bu veri kaybına neden olabilir değil. |
|Onarım Aralığı | Saniye cinsinden zaman, varsayılan 5 |Statik| Zaman açıklığı saniye cinsinden belirtin. Yetki sahibi ve ad sahibi arasındaki adlandırma tutarsızlık onarımına başlayacağı aralık. |
|ÇoğaltmaRestartWaitDuration | Saniye cinsinden zaman, varsayılan (60,0 * 30)|İzin Verilmiyor| Zaman açıklığı saniye cinsinden belirtin. Bir Adlandırma Hizmeti yinelemesi küçüldüğünde; bu zamanlayıcı başlar. Süresi dolduğunda FM aşağı olan kopyaları değiştirmeye başlayacaktır (henüz onları kayıp olarak kabul etmez). |
|ServiceDescriptionCacheLimit | Int, varsayılan 0 |Statik| Ad verme Deposu Hizmeti'ndeki LRU hizmet açıklaması önbelleğinde tutulan maksimum giriş sayısı (limitsiz olarak 0 olarak ayarlanır). |
|ServiceNotificationTimeout |Saniye cinsinden zaman, varsayılan 30 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. İstemciye servis bildirimleri teslim ederken kullanılan zaman aşımı. |
|StandbyReplicaKeepDuration | Saniye cinsinden zaman, varsayılan 3600.0 * 2 |İzin Verilmiyor| Zaman açıklığı saniye cinsinden belirtin. Bir Adlandırma Hizmeti yinelemesi aşağı durumundan geri geldiğinde; zaten değiştirilmiş olabilir. Bu zamanlayıcı, FM'in atmadan önce bekleme yinelemesini ne kadar tutacağını belirler. |
|TargetReplicaSetSize |Int, varsayılan 7 |İzin Verilmiyor|Adlandırma Hizmeti deposunun her bölümü için yineleme kümelerinin sayısı. Yineleme kümelerinin sayısını artırmak, Adlandırma Hizmeti Deposu'ndaki bilgilerin güvenilirlik düzeyini artırır; düğüm hataları sonucunda bilgilerin kaybolacağı değişikliğini azaltmak; Windows Fabric'teki artan yük ve adlandırma verilerine yönelik güncelleştirmeleri gerçekleştirmek için gereken süre.|

## <a name="nodebufferpercentage"></a>DüğümArabellekYüzdesi
| **Parametre** | **İzin Verilen Değerler** |**Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|KeyDoubleValueMap, varsayılan Yok|Dinamik|Metrik ad başına düğüm kapasite yüzdesi; failover durumda için bir düğüm üzerinde bazı boş yer tutmak için bir arabellek olarak kullanılır. |

## <a name="nodecapacities"></a>Düğüm Kapasiteleri

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup |DüğümKapasiteCollectionMap |Statik|Farklı ölçümler için düğüm kapasiteleri koleksiyonu. |

## <a name="nodedomainids"></a>DüğümDomainId'ler

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup |NodeFaultDomainIdCollection |Statik|Düğümün ait olduğu hata etki alanlarını açıklar. Hata etki alanı, veri merkezindeki düğümün konumunu açıklayan bir URI aracılığıyla tanımlanır.  Hata Etki Alanı URI'leri fd:/fd/ biçimindedir ve ardından URI yol segmentindedir.|
|YükseltmeDomainId |dize, varsayılan "" |Statik|Bir düğümün ait olduğu yükseltme etki alanını açıklar. |

## <a name="nodeproperties"></a>Düğüm Özellikleri

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup |NodePropertyCollectionMap |Statik|Düğüm özellikleri için dize anahtar değeri çiftleri topluluğu. |

## <a name="paas"></a>Paas

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|ClusterId |dize, varsayılan "" |İzin Verilmiyor|Yapılandırma koruması için kumaş tarafından kullanılan X509 sertifika deposu. |

## <a name="performancecounterlocalstore"></a>PerformanceCounterLocalStore

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Sayaçlar |Dize | Dinamik |Toplanacak performans sayaçlarının virgülden ayrılmış listesi. |
|IsEnabled |Bool, varsayılan doğrudur | Dinamik |Bayrak, yerel düğümüzerinde performans sayacı koleksiyonunun etkin olup olmadığını gösterir. |
|MaxCounterBinaryFileSizeinMB |Int, varsayılan 1 | Dinamik |Her performans sayacı ikili dosyası için maksimum boyut (MB olarak). |
|NewCounterBinaryFileCreationIntervalinMinutes |Int, varsayılan 10 | Dinamik |Yeni bir performans sayacı ikili dosyası oluşturulduktan sonra maksimum aralık (saniye cinsinden). |
|ÖrneklemeIntervalinSeconds |Int, varsayılan 60 | Dinamik |Performans sayaçları için örnekleme aralığı toplanıyor. |

## <a name="placementandloadbalancing"></a>YerleştirmeAndLoadBalancing

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|AffinityKısıtlamaÖnceliği | Int, varsayılan 0 | Dinamik|Yakınlık kısıtlamasının önceliğini belirler: 0: Sert; 1: Yumuşak; negatif: Yoksay. |
|UygulamaKapasitesiKısıtlama Önceliği | Int, varsayılan 0 | Dinamik|Kapasite kısıtlamasının önceliğini belirler: 0: Sert; 1: Yumuşak; negatif: Yoksay. |
|Otomatik Algılama Kaynakları|bool, varsayılan DOĞRU|Statik|Bu config düğüm (CPU ve Bellek) bu config doğru ayarlandığında kullanılabilir kaynakların otomatik algılama tetikleyecek - biz gerçek kapasiteleri okumak ve kullanıcı kötü düğüm kapasiteleri belirtilen ya da hiç onları tanımlamak değilse bunları düzeltmek Eğer bu config yanlış ayarlanır - biz kullanıcı kötü düğüm kapasiteleri belirtilen bir uyarı izleyecek; ama biz onları düzeltmek olmaz; kullanıcının düğümden > olarak belirtilen kapasitelere sahip olmak istemesi veya kapasitelerin tanımlanmamış olması anlamına gelir; bu sınırsız kapasite varsayar |
|BalancingDelayAfterNewNode | Saniye cinsinden zaman, varsayılan süre 120'dir |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Yeni bir düğüm ekledikten sonra bu süre içinde faaliyetleri dengelemeye başlamayın. |
|BalancingDelayAfterNodeDown | Saniye cinsinden zaman, varsayılan süre 120'dir |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Düğüm aşağı olaydan sonra bu süre içinde faaliyetleri dengelemeye başlamayın. |
|KapasiteKısıtlama Önceliği | Int, varsayılan 0 | Dinamik|Kapasite kısıtlamasının önceliğini belirler: 0: Sert; 1: Yumuşak; negatif: Yoksay. |
|ArdışıkDroppedMovementsHealthReportLimit | Int, varsayılan 20 | Dinamik|Tanılama yapılmadan ve sağlık uyarıları yayınlanmadan önce ResourceBalancer tarafından verilen Hareketlerin bırakıldığı ardışık kez tanımlanır. Negatif: Bu koşul altında Uyarılar Yayılır. |
|ConstraintFixPartialDelayAfterNewNode | Saniye cinsinden zaman, varsayılan süre 120'dir |Dinamik| Zaman açıklığı saniye cinsinden belirtin. DDo yeni bir düğüm ekledikten sonra bu süre içinde FaultDomain ve UpgradeDomain kısıtlama ihlalleri düzeltmek değil. |
|ConstraintFixPartialDelayAfterNodeDown | Saniye cinsinden zaman, varsayılan süre 120'dir |Dinamik| Zaman açıklığı saniye cinsinden belirtin. Bir düğüm aşağı olay dan sonra bu süre içinde FaultDomain ve UpgradeDomain kısıtlama ihlalleri düzeltmeyin. |
|KısıtlamaIhlaliSağlık RaporuLimit | Int, varsayılan 50 |Dinamik| Tanılama nın yapılması ve sistem durumu raporlarının yayılmadan önce çoğaltmayı ihlal eden kısıtlamanın kaç kez düzeltilmesi gerektiğini tanımlar. |
|Ayrıntılı KısıtlamaIhlaliSağlık RaporuLimit | Int, varsayılan 200 |Dinamik| Tanılama nın yapılıp ayrıntılı sağlık raporları yayınlandırılmadan önce yinelemeyi ihlal eden süreyle süreyle süreyle süreyle çıkırlığını tan |
|AyrıntılıTeşhisBilgi ListesiLimit | Int, varsayılan 15 |Dinamik| Tanılamada kesilmeden önce dahil etmek üzere kısıtlama başına tanılama girişlerinin (ayrıntılı bilgilerle) sayısını tanımlar.|
|DetailedNodeListLimit | Int, varsayılan 15 |Dinamik| Yersiz Yineleme raporlarında kesilmeden önce eklemek üzere kısıtlama başına düğüm sayısını tanımlar. |
|DetailedPartitionListLimit | Int, varsayılan 15 |Dinamik| Tanılamada kesilmeden önce dahil edilebilen bir kısıtlama için tanılama girişi başına bölüm sayısını tanımlar. |
|AyrıntılıVerboseHealthReportLimit | Int, varsayılan 200 | Dinamik|Ayrıntılı sistem durumu raporları yayınlandırılmadan önce yersiz bir yinelemenin sıkı olarak yersiz olması gereken sayısı tanımlar. |
|EnforceUserServiceMetricKapasiteleri|bool, varsayılan FALSE olduğunu | Statik |Kumaş hizmetlerinin korunmasını sağlar. Tüm kullanıcı hizmetleri tek bir iş nesnesi/cgroup altındadır ve belirtilen kaynak miktarıyla sınırlıdır. Bu statik olması gerekir (FabricHost yeniden gerektirir) oluşturma / kullanıcı iş nesnesi kaldırma ve Kumaş Ana Bilgisayar açık sırasında yapılan sınırları ayarı. |
|FaultDomainConstraintPriority | Int, varsayılan 0 |Dinamik| Hata etki alanı kısıtlamasının önceliğini belirler: 0: Sert; 1: Yumuşak; negatif: Yoksay. |
|GlobalMovementThrotleCountingInterval | Saniye cinsinden zaman, varsayılan 600 |Statik| Zaman açıklığı saniye cinsinden belirtin. Etki alanı çoğaltma hareketlerini izlemek için geçmiş aralığın uzunluğunu belirtin (GlobalMovementThrottleThreshold ile birlikte kullanılır). Genel azaltmayı tamamen yok saymak için 0 olarak ayarlanabilir. |
|GlobalMovementThrotleThreshold | Uint, varsayılan 1000 |Dinamik| GlobalMovementThrotleCountingInterval tarafından gösterilen geçmiş aralıkta Dengeleme Aşamasında izin verilen maksimum hareket sayısı. |
|GlobalMovementThrotleThresholdForBalancing | Uint, varsayılan 0 | Dinamik|GlobalMovementThrottleCountingInterval tarafından gösterilen geçmiş aralıkta Dengeleme Fazında izin verilen maksimum hareket sayısı. 0 sınır yok gösterir. |
|GlobalMovementThrotleThresholdForPlacement | Uint, varsayılan 0 |Dinamik| GlobalMovementThrottleCountingInterval.0 tarafından gösterilen geçmiş aralıkta Yerleştirme Aşamasında izin verilen maksimum hareket sayısı sınır olmadığını gösterir.|
|GlobalMovementThrotleThresholdPercentage|çift, varsayılan 0|Dinamik|GlobalMovementThrotleCountingInterval tarafından gösterilen geçmiş aralıkta Dengeleme ve Yerleştirme aşamalarında izin verilen maksimum toplam hareket sayısı (kümedeki toplam çoğaltma sayısının yüzdesi olarak ifade edilir). 0 sınır yok gösterir. Hem bu hem de GlobalMovementThrottleThreshold belirtilirse; sonra daha konservatif sınır kullanılır.|
|GlobalMovementThrotleThresholdPercentageForBalancing|çift, varsayılan 0|Dinamik|GlobalMovementThrottleCountingInterval tarafından gösterilen geçmiş aralıkta Dengeleme Fazı'nda izin verilen maksimum hareket sayısı (PLB'deki toplam çoğaltma sayısının yüzdesi olarak ifade edilir). 0 sınır yok gösterir. Hem bu hem de GlobalMovementThrottleThresholdForBalancing belirtilirse; sonra daha konservatif sınır kullanılır.|
|InBuildThrottlingAssociatedMetric | dize, varsayılan "" |Statik| Bu azaltma için ilişkili metrik ad. |
|InBuildThrottlingEtkin | Bool, varsayılan yanlış |Dinamik| Yapı içi azaltmanın etkin olup olmadığını belirleyin. |
|InBuildThrottlingGlobalMaxValue | Int, varsayılan 0 |Dinamik|Genel olarak izin verilen en yüksek yapı içi yineleme sayısı. |
|KesmeDengelemeForAllFailoverUnitUpdates | Bool, varsayılan yanlış | Dinamik|Herhangi bir arıza birimi güncelleştirmesinin hızlı veya yavaş dengeleme çalışmasını kesintiye uğrayıp kesmemesi gerektiğini belirler. FailoverUnit: oluşturulur/silinirse, belirtilen "false" dengeleme çalışması kesintiye uğrar; eksik kopyaları vardır; birincil yineleme konumunu veya yineleme sayısını değiştirdi. Dengeleme çalışması diğer durumlarda kesintiye uğramaz - FailoverUnit: ekstra kopyaları varsa; çoğaltma bayrağını değiştirdi; yalnızca bölüm sürümü veya başka bir durumda değiştirildi. |
|MinConstraintCheckInterval | Saniye cinsinden zaman, varsayılan değer 1 |Dinamik| Zaman açıklığı saniye cinsinden belirtin. Art arda iki kısıtlama denetim turundan önce geçmesi gereken minimum süreyi tanımlar. |
|MinLoadBalancingInterval | Saniye cinsinden zaman, varsayılan 5 |Dinamik| Zaman açıklığı saniye cinsinden belirtin. Art arda iki dengeleme turundan önce geçmesi gereken minimum süreyi tanımlar. |
|MinPlacementInterval | Saniye cinsinden zaman, varsayılan değer 1 |Dinamik| Zaman açıklığı saniye cinsinden belirtin. Art arda iki yerleşim turundan önce geçmesi gereken minimum süreyi tanımlar. |
|MoveExistingReplicaForplacement | Bool, varsayılan doğrudur |Dinamik|Yerleşim sırasında varolan yinelemenin hareket ettirilip taşınmayacağına karar veren ayar. |
|HareketPerPartitionThrotleCountingInterval | Saniye cinsinden zaman, varsayılan 600 |Statik| Zaman açıklığı saniye cinsinden belirtin. Her bölüm için çoğaltma hareketlerini izlemek için geçmiş aralığın uzunluğunu belirtin (MovementPerPartitionThrottleThreshold ile birlikte kullanılır). |
|HareketPerPartitionThrotleThreshold | Uint, varsayılan 50 |Dinamik| Bu bölümün kopyaları için dengeleme ile ilgili hareketlerin sayısı, geçen aralıkta MovementPerFailoverUnitThrottleThreshold'a ulaşmış veya aşmışsa, bir bölüm için dengeleme ile ilgili hareket oluşmaz. MovementPerPartitionThrotleCountingInterval. |
|MoveParentToFixAffinityViolation | Bool, varsayılan yanlış |Dinamik| Üst yinelemelerin yakınlık kısıtlamalarını düzeltmek için taşınıp taşınamayacağına karar veren ayar.|
|KısmenPlaceServices | Bool, varsayılan doğrudur |Dinamik| Kümedeki tüm hizmet yinelemelerinin kendileri için sınırlı uygun düğümler verildiğinde "tümü veya hiç" yerleştirilip yerleştirilmeyeceğini belirler.|
|YerChildWithoutParent | Bool, varsayılan doğrudur | Dinamik|Üst yineleme yoksa alt hizmet yinelemesi yerleştirilip yerleştirilmeyeceğini belirleyen ayar. |
|YerleştirmeKısıtlama Önceliği | Int, varsayılan 0 | Dinamik|Yerleşim kısıtlamasının önceliğini belirler: 0: Sert; 1: Yumuşak; negatif: Yoksay. |
|YerleşimConstraintValidationÖnbellek | Int, varsayılan 10000 |Dinamik| Yerleşim Kısıtlaması İfadelerinin hızlı doğrulanması ve önbelleğe alınması için kullanılan tablonun boyutunu sınırlar. |
|YerleşimAramaZaman Out | Saniye cinsinden zaman, varsayılan değer 0,5 |Dinamik| Zaman açıklığı saniye cinsinden belirtin. Hizmet verirken; bir sonuç dönmeden önce en fazla bu kadar uzun süre arama yapın. |
|PLBRefreshGap | Saniye cinsinden zaman, varsayılan değer 1 |Dinamik| Zaman açıklığı saniye cinsinden belirtin. PLB durumu yeniden yenilemeden önce geçmesi gereken minimum süreyi tanımlar. |
|Tercihedilen KonumKısıtlamaÖnceliği | Int, varsayılan 2| Dinamik|Tercih edilen konum kısıtlamasının önceliğini belirler: 0: Sert; 1: Yumuşak; 2: Optimizasyon; negatif: Yoksay |
|PreferredPrimaryDomainsConstraintPriority| Int, varsayılan 1 | Dinamik| Tercih edilen birincil etki alanı kısıtlamasının önceliğini belirler: 0: Sabit; 1: Yumuşak; negatif: Yoksay |
|YükseltilmişUD'ları Tercih|bool, varsayılan FALSE olduğunu|Dinamik|Zaten yükseltilmiş UD'lere taşımayı tercih eden mantığı açar ve kapatır. SF 7.0 ile başlayarak, bu parametrenin varsayılan değeri TRUE'dan FALSE'a değiştirilir.|
|ÖnleyiciGeçiciOvercommit | Bool, varsayılan yanlış | Dinamik|PLB'nin başlatılan hamlelerle serbest bırakılacak kaynaklara hemen güvenmesi gerektiğini belirler. Varsayılan olarak; PLB dışarı taşımak başlatabilir ve geçici overcommit oluşturabilirsiniz aynı düğüm üzerinde hareket. Bu parametrenin doğru şekilde ayarlanması, bu tür aşırı taahhütleri önler ve isteğe bağlı defrag (aka placementWithMove) devre dışı bırakılır. |
|ScaleoutCountConstraintPriority | Int, varsayılan 0 |Dinamik| Ölçekleme sayısı kısıtlamasının önceliğini belirler: 0: Sert; 1: Yumuşak; negatif: Yoksay. |
|Alt kümelemeEtkin|Bool, varsayılan FALSE olduğunu | Dinamik |Dengeleme için standart sapma hesaplanırken alt kümelenmeyi kabul edin |
|SubclusteringReportingPolicy| Int, varsayılan 1 |Dinamik|Alt kümeleme sağlık raporlarının nasıl ve gönderilip gönderilmediğini tanımlar: 0: Bildirmeyin; 1: Uyarı; 2: Tamam |
|SwapPrimaryThrottlingAssociatedMetric | dize, varsayılan ""|Statik| Bu azaltma için ilişkili metrik ad. |
|SwapPrimaryThrottlingEtkin | Bool, varsayılan yanlış|Dinamik| Takas birincil azaltma nın etkin olup olmadığını belirleyin. |
|SwapPrimaryThrottlingGlobalMaxValue | Int, varsayılan 0 |Dinamik| Genel olarak izin verilen en büyük takas birincil yineleme sayısı. |
|TraceCRMReasons |Bool, varsayılan doğrudur |Dinamik|CRM'nin operasyonel olaylar kanalına hareket verme nedenlerini izleyip izlememe yi belirtir. |
|YükseltmeDomainConstraintPriority | Int, varsayılan 1| Dinamik|Yükseltme etki alanı kısıtlamasının önceliğini belirler: 0: Sert; 1: Yumuşak; negatif: Yoksay. |
|MoveCostReports'u kullan | Bool, varsayılan yanlış | Dinamik|LB'ye puanlama işlevinin maliyet öğesini yoksaymasını bildirir; daha dengeli bir yerleşim için potansiyel olarak çok sayıda hamle elde edin. |
|KullanımAyrıİkincil Yük | Bool, varsayılan doğrudur | Dinamik|Farklı ikincil yük kullanıp kullanılmayacağına karar veren ayar. |
|KullanımAyrıİkincilTaşıma Maliyeti|Bool, varsayılan FALSE olduğunu | Dinamik|Kullanım AyırırİkincilMoveCost kapalı ise her düğüm de ikincil için farklı taşıma maliyeti kullanmanız gerektiğini PLB belirler Ayarı: - Bir düğüm üzerinde ikincil için bildirilen taşıma maliyeti her ikincil (diğer tüm düğümler üzerinde) için overwritting taşıma maliyeti neden olur UseSeparateSecondaryMoveCost açık: - Bir düğüm üzerinde ikincil için bildirilen taşıma maliyeti yalnızca ikincil (diğer düğümlerde ikincil etkisi olmaz) üzerinde etkili olacaktır - Çoğaltma kilitlenme gerçekleşirse - yeni yineleme hizmette belirtilen varsayılan taşıma maliyeti ile oluşturulur düzeyi - PLB varolan çoğaltma taşırsa - taşıma maliyeti ile gider |
|DoğrulamaYerleşimKısıtlaması | Bool, varsayılan doğrudur |Dinamik| Bir hizmetin Hizmet Açıklaması güncelleştirildiğinde, bir hizmetiçin PlacementConstraint ifadesinin doğrulanıp doğrulanmayacağını belirtir. |
|DoğrulamaBirincil Yerleştirme KısıtlamalarıPromote| Bool, varsayılan DOĞRU |Dinamik|Bir hizmet için PlacementConstraint ifadesinin başarısız olduğunda birincil tercih için değerlendirilip değerlendirilmeyeceğini belirtir. |
|VerboseHealthReportLimit | Int, varsayılan 20 | Dinamik|Bir sistem durumu uyarısı bildirilmeden önce bir yinelemenin yersiz olarak kaç kez gitmesi gerektiğini tanımlar (ayrıntılı sistem durumu raporlaması etkinse). |
|NodeLoadsOperationalTracingEtkin | Bool, varsayılan doğrudur |Dinamik|Olay deposunda Düğüm Yükü operasyonel yapısal izleme sağlayan config. |
|NodeLoadsOperationalTracingInterval | TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(20) | Dinamik|Zaman açıklığı saniye cinsinden belirtin. Her hizmet etki alanı için olay deposuna düğüm yüklerini izleme aralığı. |

## <a name="reconfigurationagent"></a>ReconfigurationAgent

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|UygulamaYükseltmeMaxReplicaCloseDuration | Saniye cinsinden zaman, varsayılan 900 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Sistemin, Uygulama Yükseltmesi sırasında birbirine yakın olan yinelemeleri olan hizmet ana bilgisayarlarını sonlandırmadan önce bekleyeceği süre.|
|KumaşYükseltmeMaxReplicaCloseDuration | Saniye cinsinden zaman, varsayılan 900 |Dinamik| Zaman açıklığı saniye cinsinden belirtin. Sistemin, kumaş yükseltmesi sırasında birbirine yakın olan yinelemelere sahip hizmet ana bilgisayarlarını sonlandırmadan önce bekleyeceği süre. |
|ZarifReplicaShutdownMaxDuration|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(120)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Sistemin, yakın sıkışmış yinelemeleri olan hizmet ana bilgisayarlarını sonlandırmadan önce bekleyeceği süre. Bu değer 0 olarak ayarlanırsa, yinelemelerin kapanması için talimat verilmeyecektir.|
|DüğümDeactivationMaxReplicaCloseDuration | Saniye cinsinden zaman, varsayılan 900 |Dinamik|Zaman açıklığı saniye cinsinden belirtin. Düğüm devre dışı bırakma sırasında yakın sıkışmış yinelemeleri olan hizmet ana bilgisayarlarını sonlandırmadan önce sistemin bekleyeceği süre. |
|PeriodicApiSlowTraceInterval | Saniye cinsinden zaman, varsayılan 5 dakika |Dinamik| Zaman açıklığı saniye cinsinden belirtin. PeriodicApiSlowTraceInterval, yavaş API çağrılarının API monitörü tarafından izlendiği aralığı tanımlar. |
|ReplicaChangeRoleFailureRestartThreshold|int, varsayılan 10|Dinamik| Tamsayı. Otomatik azaltma eyleminin (yineleme yeniden başlatma) uygulanacağı birincil promosyon sırasında KISI hatalarının sayısını belirtin. |
|ReplicaChangeRoleFailureWarningReportThreshold|int, varsayılan 2147483647 olduğunu|Dinamik| Tamsayı. Birincil promosyon sırasında api hatalarının sayısını belirtin ve ardından uyarı sağlık raporu yükseltilir.|
|ServiceApiHealthDuration | Saniye cinsinden zaman, varsayılan süre 30 dakikadır |Dinamik| Zaman açıklığı saniye cinsinden belirtin. ServiceApiHealthDuration, bir hizmet API'nin sağlıksız olarak bildirmeden önce çalışmasını ne kadar bekleyeceğimizi tanımlar. |
|ServiceReconfigurationApiHealthDuration | Saniye cinsinden zaman, varsayılan 30 |Dinamik| Zaman açıklığı saniye cinsinden belirtin. ServiceReconfigurationApiHealthDuration sağlıksız bildirmeden önce bir hizmet API çalışması için ne kadar beklemeniz tanımlar. Bu, kullanılabilirliği etkileyen API çağrıları için geçerlidir.|

## <a name="replication"></a>Çoğaltma
| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|BatchAcknowledgementInterval|TimeSpan, varsayılan yaygındır::TimeSpan::FromMilliseconds(15)|Statik|Zaman açıklığı saniye cinsinden belirtin. Bir bildirim göndermeden önce çoğaltıcının bir işlem aldıktan sonra beklediği süreyi belirler. Bu süre içinde alınan diğer işlemler, onaylarını ağ trafiğini azaltan tek bir ileti > geri gönderir, ancak çoğaltıcının iş kısmını potansiyel olarak azaltır.|
|MaxcopyQueueSize|uint, varsayılan 1024|Statik|Bu, çoğaltma işlemlerini sürdüren sıranın başlangıç boyutunu tanımlayan en büyük değerdir. 2 gücünde olması gerektiğini unutmayın. Çalışma zamanı sırasında kuyruk bu boyuta büyürse, birincil ve ikincil çoğalıcılar arasında daraltılır.|
|MaxPrimaryReplicationQueueMemorySize|uint, varsayılan 0|Statik|Bu, baytlar birincil çoğaltma sırasının maksimum değeridir.|
|MaxPrimaryReplicationQueueSize|uint, varsayılan 1024|Statik|Bu, birincil çoğaltma kuyruğunda bulunabilecek en fazla işlem sayısıdır. 2 gücünde olması gerektiğini unutmayın.|
|MaxReplicationMessageSize|uint, varsayılan 52428800 olduğunu|Statik|Çoğaltma işlemlerinin en yüksek ileti boyutu. Varsayılan değer 50MB'dır.|
|MaxSecondaryReplicationQueueMemorySize|uint, varsayılan 0|Statik|Bu, baytlar ikincil çoğaltma sırasının maksimum değeridir.|
|MaxSecondaryReplicationQueueSize|uint, varsayılan 2048 olduğunu|Statik|Bu, ikincil çoğaltma kuyruğunda bulunabilecek en fazla işlem sayısıdır. 2 gücünde olması gerektiğini unutmayın.|
|QueueHealthMonitoringInterval|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(30)|Statik|Zaman açıklığı saniye cinsinden belirtin. Bu değer, çoğaltma işlemi kuyruklarında herhangi bir uyarı/hata durumu olaylarını izlemek için Çoğaltıcı tarafından kullanılan süreyi belirler. '0' değeri sistem durumu izlemeyi devre dışı |
|QueueHealthWarningAtUsagePercent|uint, varsayılan 80|Statik|Bu değer, yüksek sıra kullanımı hakkında uyarı rapor sonra çoğaltma sırası kullanımını (yüzde olarak) belirler. Bunu QueueHealthMonitoringInterval'in bir lütuf aralığından sonra yapıyoruz. Sıra kullanımı yetkisiz kullanım aralığında bu yüzdenin altına düşerse|
|ÇoğaltıcıAdres|dize, varsayılan "localhost:0"|Statik|İşlemleri göndermek/almak için diğer yinelemelerle bağlantı kurmak için Windows Fabric Replicator tarafından kullanılan dize -'IP:Port' şeklindeki bitiş noktası.|
|ÇoğalıcıListenAddress|dize, varsayılan "localhost:0"|Statik|Windows Fabric Replicator tarafından diğer yinelemelerden işlem almak için kullanılan dize -'IP:Port' şeklindeki bitiş noktası.|
|ÇoğalıcıYayın Adresi|dize, varsayılan "localhost:0"|Statik|Diğer yinelemelere işlem göndermek için Windows Fabric Replicator tarafından kullanılan bir dize -'IP:Port' şeklindeki bitiş noktası.|
|RetryInterval|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(5)|Statik|Zaman açıklığı saniye cinsinden belirtin. Bir işlem kaybolduğunda veya reddedildiğinde bu zamanlayıcı çoğaltıcının işlemi ne sıklıkta yeniden göndermeyi deneyeceğini belirler.|

## <a name="resourcemonitorservice"></a>Kaynak MonitorService
| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|IsEnabled|bool, varsayılan FALSE olduğunu |Statik|Hizmet kümede etkin olup olmadığını denetler. |

## <a name="runas"></a>Runas

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|RunasAccountName |dize, varsayılan "" |Dinamik|RunAs hesap adını gösterir. Bu yalnızca "DomainUser" veya "ManagedServiceAccount" hesap türü için gereklidir. Geçerli değerler "etki alanı\kullanıcı" veya "user@domain". |
|RunasAccountType|dize, varsayılan "" |Dinamik|RunAs hesap türünü gösterir. Bu herhangi bir RunAs bölümü Geçerli değerleri için gerekli olan "DomainUser/NetworkService/ManagedServiceAccount/LocalSystem".|
|RunasPassword|dize, varsayılan "" |Dinamik|RunAs hesap parolasını gösterir. Bu yalnızca "DomainUser" hesap türü için gereklidir. |

## <a name="runas_dca"></a>RunAs_DCA

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|RunasAccountName |dize, varsayılan "" |Dinamik|RunAs hesap adını gösterir. Bu yalnızca "DomainUser" veya "ManagedServiceAccount" hesap türü için gereklidir. Geçerli değerler "etki alanı\kullanıcı" veya "user@domain". |
|RunasAccountType|dize, varsayılan "" |Dinamik|RunAs hesap türünü gösterir. Bu herhangi bir RunAs bölümü Geçerli değerleri için gerekli olan "LocalUser/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem". |
|RunasPassword|dize, varsayılan "" |Dinamik|RunAs hesap parolasını gösterir. Bu yalnızca "DomainUser" hesap türü için gereklidir. |

## <a name="runas_fabric"></a>RunAs_Fabric

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|RunasAccountName |dize, varsayılan "" |Dinamik|RunAs hesap adını gösterir. Bu yalnızca "DomainUser" veya "ManagedServiceAccount" hesap türü için gereklidir. Geçerli değerler "etki alanı\kullanıcı" veya "user@domain". |
|RunasAccountType|dize, varsayılan "" |Dinamik|RunAs hesap türünü gösterir. Bu herhangi bir RunAs bölümü Geçerli değerleri için gerekli olan "LocalUser/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem". |
|RunasPassword|dize, varsayılan "" |Dinamik|RunAs hesap parolasını gösterir. Bu yalnızca "DomainUser" hesap türü için gereklidir. |

## <a name="runas_httpgateway"></a>RunAs_HttpGateway

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|RunasAccountName |dize, varsayılan "" |Dinamik|RunAs hesap adını gösterir. Bu yalnızca "DomainUser" veya "ManagedServiceAccount" hesap türü için gereklidir. Geçerli değerler "etki alanı\kullanıcı" veya "user@domain". |
|RunasAccountType|dize, varsayılan "" |Dinamik|RunAs hesap türünü gösterir. Bu herhangi bir RunAs bölümü Geçerli değerleri için gerekli olan "LocalUser/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem". |
|RunasPassword|dize, varsayılan "" |Dinamik|RunAs hesap parolasını gösterir. Bu yalnızca "DomainUser" hesap türü için gereklidir. |

## <a name="security"></a>Güvenlik
| **Parametre** | **İzin Verilen Değerler** |**Yükseltme İlkesi**| **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|AADCertEndpointFormat|dize, varsayılan ""|Statik|AAD Cert Endpoint Formatı, varsayılan Azure Ticari, Azure Kamu gibi\/varsayılan{0}olmayan ortam için belirtilen "https: /login.microsoftonline.us/ /federationmetadata/2007-06/federationmetadata.xml" |
|AADClientApplication|dize, varsayılan ""|Statik|Kumaş İstemcileri temsil eden Yerel İstemci başvuru adı veya kimliği |
|AADClusterApplication|dize, varsayılan ""|Statik|Kümeyi temsil eden Web API uygulama adı veya kimliği |
|AADLoginEndpoint|dize, varsayılan ""|Statik|AAD Giriş Bitiş Noktası, varsayılan Azure Ticari, Azure Kamu gibi varsayılan\/olmayan ortam için belirtilen "https: /login.microsoftonline.us" |
|AADTenantid|dize, varsayılan ""|Statik|Kiracı Kimliği (GUID) |
|KabulExpiredPinnedClusterCertificate|bool, varsayılan FALSE olduğunu|Dinamik|Parmak izi A tarafından bildirilen süresi dolmuş küme sertifikalarını kabul edip etmeyeceğini belirten bayrak Yalnızca küme sertifikaları için geçerlidir; böylece kümeyi canlı tutmak için. |
|AdminClientCertThumbprints|dize, varsayılan ""|Dinamik|Yönetici rolünde istemciler tarafından kullanılan sertifikaların parmak izleri. Virgülle ayrılmış bir isim listesidir. |
|AADTokenEndpointFormat|dize, varsayılan ""|Statik|AAD Token Endpoint, varsayılan Azure Ticari, Azure Kamu gibi varsayılan\/olmayan{0}ortam için belirtilen "https: /login.microsoftonline.us/ " |
|AdminClientClaims|dize, varsayılan ""|Dinamik|Yönetici istemcilerden beklenen tüm olası talepler; ClientClaims ile aynı biçimde; bu liste dahili olarak ClientClaims eklenir; bu nedenle clientclaims için aynı girişleri eklemek için gerek yoktur. |
|AdminClientIdentities|dize, varsayılan ""|Dinamik|Yönetici rolündeki kumaş istemcilerinin Windows kimlikleri; ayrıcalıklı kumaş işlemleri ne yetki için kullanılır. Virgülle ayrılmış bir listedir; her giriş bir alan adı hesabı adı veya grup adıdır. Kolaylık sağlamak için; fabric.exe çalıştıran hesap otomatik olarak yönetici rolü atanır; bu yüzden grup ServiceFabricAdministrators olduğunu. |
|ApprunasAccountGroupX509Folder|dize, varsayılan /home/sfuser/sfusercerts |Statik|AppRunAsAccountGroup X509 sertifikalarının ve özel anahtarlarının bulunduğu klasör |
|SertifikaExpirySafetyMargin|TimeSpan, varsayılan yaygındır::TimeSpan::FromMinutes(43200)|Statik|Zaman açıklığı saniye cinsinden belirtin. Sertifika nın sona ermesi için güvenlik marjı; sertifika sağlık raporu durumu, son kullanma tarihi bundan daha yakın olduğunda Tamam'dan Uyarı'ya değişir. Varsayılan değer 30 gündür. |
|SertifikaSağlıkRaporlamaAralığı|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(3600 * 8)|Statik|Zaman açıklığı saniye cinsinden belirtin. Sertifika durumu raporlaması için aralığı belirtin; varsayılan olarak 8 saat; 0 devre dışı bırakırsa sertifika sağlık raporlama |
|ClientCertThumbprints|dize, varsayılan ""|Dinamik|Kümeyle konuşmak için istemciler tarafından kullanılan sertifikaların parmak izleri; cluster bu yetkilendirme gelen bağlantıyı kullanır. Virgülle ayrılmış bir isim listesidir. |
|ClientClaimAuthEtkin|bool, varsayılan FALSE olduğunu|Statik|İstemciler üzerinde talep tabanlı kimlik doğrulamasının etkin olup olmadığını gösterir; bu gerçek örtülü olarak IstemciRolEnabled ayarlar. |
|Müşteri Talepleri|dize, varsayılan ""|Dinamik|Ağ geçidine bağlanmak için istemcilerden beklenen tüm olası talepler. Bu bir 'VEYA' listesi: \| \| ClaimsEntry ClaimsEntry ClaimsEntry \| \| Entry Entry ... her ClaimEntry bir "VE" listesidir: ClaimType=ClaimValue && ClaimType=ClaimValue && ClaimType=ClaimValue ... |
|Müşteri Kimlikleri|dize, varsayılan ""|Dinamik|FabricClient'ın Windows kimlikleri; adlandırma ağ geçidi, gelen bağlantıları yetkilendirmek için bunu kullanır. Virgülle ayrılmış bir listedir; her giriş bir alan adı hesabı adı veya grup adıdır. Kolaylık sağlamak için; fabric.exe çalıştıran hesaba otomatik olarak izin verilir; böylece grup ServiceFabricAllowedUsers ve ServiceFabricAdministrators vardır. |
|İstemciRolü Etkin|bool, varsayılan FALSE olduğunu|Statik|İstemci rolünün etkin olup olmadığını gösterir; doğru ayarlandığında; istemcilere kimliklerine göre roller atanır. V2 için; etkinleştirmek, YöneticiClientCommonNames/AdminClientIdentits'ta olmayan istemcinin yalnızca salt okunur işlemleri yürütebileceği anlamına gelir. |
|ClusterCertThumbprints|dize, varsayılan ""|Dinamik|Kümeye katılmasına izin verilen sertifikaların parmak izleri; virgülle ayrılmış bir ad listesi. |
|ClusterCredentialType|dize, varsayılan "Yok"|İzin Verilmiyor|Kümeyi güvenli hale getirmek için kullanılacak güvenlik kimlik bilgilerini gösterir. Geçerli değerler "None/X509/Windows" |
|KümeKimlikleri|dize, varsayılan ""|Dinamik|Küme düğümlerinin Windows kimlikleri; küme üyelik yetkilendirmesi için kullanılır. Virgülle ayrılmış bir listedir; her giriş bir alan adı hesabı adı veya grup adıdır |
|KümeSpn|dize, varsayılan ""|İzin Verilmiyor|Kümenin hizmet ana adı; kumaş tek bir etki alanı kullanıcısı olarak çalıştığında (gMSA/domain kullanıcı hesabı). Bu fabric.exe kira dinleyici ve dinleyici spn: federasyon dinleyici; dahili çoğaltma dinleyicileri; runtime hizmet dinleyicisi ve adlandırma ağ geçidi dinleyicisi. Kumaş makine hesabı olarak çalıştığında bu boş bırakılmalıdır; bu durumda dinleyici taşıma adresinden yan bilgi işlem dinleyici SPN bağlama. |
|CrlCheckingFlag|uint, varsayılan 0x4000000|Dinamik|Varsayılan sertifika zinciri doğrulama bayrağı; bileşene özgü bayrak tarafından geçersiz kılınabilir; örneğin Federasyon/X509CertChainFlags 0x100000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20 CERT_CHAIN_REVOCATION_CHECK_CHAIN000000 0x20000000 0x40000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT000 0x800000000 0x8000000000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY 0x8000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000 0x8000000000000 0x800000000000000000000000000000https://msdn.microsoft.com/library/windows/desktop/aa376078(v=vs.85).aspx |
|CrlDisablePeriod|TimeSpan, varsayılan yaygındır::TimeSpan::FromMinutes(15)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Çevrimdışı hatayla karşılaştıktan sonra belirli bir sertifika için CRL denetiminin ne kadar süre devre dışı bırakıldığı; CRL çevrimdışı hata varsa yoksayılabilir. |
|CrlOfflineHealthReportTtl|TimeSpan, varsayılan yaygındır::TimeSpan::FromMinutes(1440)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. |
|Devre Dışı AtılabilirFirewallRuleForDomainProfile| bool, varsayılan DOĞRU |Statik| Etki alanı profili için güvenlik duvarı kuralının etkinleştirilmemesi gerektiğini gösterir |
|Devre Dışı AtılabilirFirewallRuleForPrivateProfile| bool, varsayılan DOĞRU |Statik| Özel profil için güvenlik duvarı kuralının etkinleştirilmemesi durumunda gösterir | 
|Devre Dışı AtılabilirFirewallRuleForPublicProfile| bool, varsayılan DOĞRU | Statik|Genel profil için güvenlik duvarı kuralının etkinleştirilip etkinleştirilmediğini gösterir |
| EnforceLinuxMinTlsVersion | bool, varsayılan FALSE olduğunu | Statik | Eğer doğru ayarlanırsa; yalnızca TLS sürüm 1.2+ desteklenir.  Yanlış sayılsa; önceki TLS sürümlerini destekleyin. Yalnızca Linux için geçerlidir |
| EnforcePrevalidationOnSecurityChanges | bool, varsayılan FALSE olduğunu| Dinamik | Güvenlik ayarlarındaki değişiklikleri algılayıp küme yükseltme davranışını denetleyen bayrak. Küme yükseltmesi 'true' olarak ayarlanırsa, sunu kurallarından herhangi biriyle eşleşen sertifikalardan en az birinin karşılık gelen doğrulama kuralını geçebilmesini sağlamaya çalışır. Ön doğrulama, yeni ayarlar herhangi bir düğüme uygulanmadan önce yürütülür, ancak yükseltmeyi başlatma sırasında Cluster Manager hizmetinin birincil yinelemesini barındıran düğümde yalnızca çalışır. Varsayılan değer şu anda 'false' olarak ayarlanır; sürüm 7.1 ile başlayarak, ayar yeni Azure Hizmet Kumaş kümeleri için 'true' olarak ayarlanır.|
|FabricHostSpn| dize, varsayılan "" |Statik| FabricHost hizmet ana adı; kumaş tek bir etki alanı kullanıcısı (gMSA/etki alanı kullanıcı hesabı) olarak çalıştığında ve FabricHost makine hesabı altında çalıştığında. Bu FabricHost için IPC dinleyici SPN olduğunu; FabricHost makine hesabı altında çalıştığından varsayılan olarak boş bırakılmalıdır |
|YoksayCrlOfflineHatası|bool, varsayılan FALSE olduğunu|Dinamik|Sunucu tarafı gelen istemci sertifikalarını doğruladığında CRL çevrimdışı hatasını yoksayma |
|YoksaysVrCrlOfflineHatası|bool, varsayılan DOĞRU|Dinamik|İstemci tarafı gelen sunucu sertifikalarını doğruladığında CRL çevrimdışı hatasını yoksaymak ister; varsayılan olarak gerçek. İptal edilen sunucu sertifikalarına sahip saldırılar dns'den ödün vermeyi gerektirir; iptal edilen istemci sertifikalarından daha zordur. |
|ServerAuthCredentialType|dize, varsayılan "Yok"|Statik|FabricClient ve Cluster arasındaki iletişimi güvence altına almak için kullanılacak güvenlik kimlik bilgilerini gösterir. Geçerli değerler "None/X509/Windows" |
|ServerCertThumbprints|dize, varsayılan ""|Dinamik|İstemcilerle konuşmak için küme tarafından kullanılan sunucu sertifikalarının parmak izleri; istemciler kümenin kimliğini doğrulamak için bunu kullanır. Virgülle ayrılmış bir isim listesidir. |
|AyarlarX509StoreName| dize, varsayılan "MY"| Dinamik|Konfigürasyon koruması için kumaş tarafından kullanılan X509 sertifika deposu |
|UseClusterCertForIpcServerTlsSecurity|bool, varsayılan FALSE olduğunu|Statik|IPC Server TLS taşıma birimini güvence altına almak için küme sertifikası kullanıp kullanmama |
|X509Klasör|dize, varsayılan /var/lib/waagent|Statik|X509 sertifikalarının ve özel anahtarların bulunduğu klasör |
|TLS1_2_CipherList| string| Statik|Boş olmayan bir dize olarak ayarlanırsa; TLS1.2 ve altında desteklenen şifreleme listesini geçersiz kılar. Desteklenen şifreleme listesini ve TLS1.2 için güçlü şifreleme listesi nin liste formatını almak için 'openssl-şifreler' belgelerine bakın: "ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-G8-G83: ECDHE-RSA-AES-128-GCM-SHA256:ECDHE-ECDSA-AES256-CBC-SHA384:ECDHE-ECDSA-AES128-CBC-SHA256:ECDHE-RSA-AES256-CBC-SHA384:ECDHE-RSA-AES128-CBC-SHA256" Yalnızca Linux için geçerlidir. |

## <a name="securityadminclientx509names"></a>Güvenlik/AdminClientX509İsimler

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|X509NameMap, varsayılan Yok|Dinamik|Bu liste "Ad" ve "Değer" çiftinin listesidir. Her "Ad" yönetici istemci işlemleri için yetkilendirilen X509 sertifikalarının özne ortak adı veya DnsName'sidir. Belirli bir "Ad" için", "Değer" veren intihal için sertifika parmak izlerinin virgüllü ayrı bir listesidir, boş değilse, yönetici istemci sertifikalarının doğrudan vereni listede olmalıdır. |

## <a name="securityclientaccess"></a>Güvenlik/İstemci Erişimi

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|ActivateNode |dize, varsayılan "Yönetici" |Dinamik| Etkinleştirme düğümü için güvenlik yapılandırması. |
|CancelTestCommand |dize, varsayılan "Yönetici" |Dinamik| Belirli bir TestKomutu iptal eder - uçuştaysa. |
|CodePackageControl |dize, varsayılan "Yönetici" |Dinamik| Kod paketlerini yeniden başlatmak için güvenlik yapılandırması. |
|Create Application |dize, varsayılan "Yönetici" | Dinamik|Uygulama oluşturma için güvenlik yapılandırması. |
|CreateComposeDağıtım|dize, varsayılan "Yönetici"| Dinamik|Dosyaları oluşturma tarafından açıklanan bir oluşturma dağıtımı oluşturur |
|Ağ Geçidi Kaynağı Oluşturma|dize, varsayılan "Yönetici"| Dinamik|Ağ geçidi kaynağı oluşturma |
|CreateName |dize, varsayılan "Yönetici" |Dinamik|URI oluşturma adlandırma için güvenlik yapılandırması. |
|CreateNetwork|dize, varsayılan "Yönetici" |Dinamik|Kapsayıcı ağı oluşturur |
|CreateService |dize, varsayılan "Yönetici" |Dinamik| Hizmet oluşturma için güvenlik yapılandırması. |
|CreateServiceFromTemplate |dize, varsayılan "Yönetici" |Dinamik|Şablondan hizmet oluşturma için güvenlik yapılandırması. |
|CreateVolume|dize, varsayılan "Yönetici"|Dinamik|Bir birim oluşturur |
|Devre Dışı BırakmaNode |dize, varsayılan "Yönetici" |Dinamik| Düğümü devre dışı kakma için güvenlik yapılandırması. |
|DeactivateNodesBatch |dize, varsayılan "Yönetici" |Dinamik| Birden çok düğümü devre dışı kakma için güvenlik yapılandırması. |
|Sil |dize, varsayılan "Yönetici" |Dinamik| Görüntü deposu istemci silme işlemi için güvenlik yapılandırmaları. |
|Silme Uygulaması |dize, varsayılan "Yönetici" |Dinamik| Uygulama silme için güvenlik yapılandırması. |
|DeleteComposeDağıtım|dize, varsayılan "Yönetici"| Dinamik|Oluşturma dağıtımını siler |
|Ağ Geçidi Kaynağını Silme|dize, varsayılan "Yönetici"| Dinamik|Ağ geçidi kaynağını siler |
|Silme Adı |dize, varsayılan "Yönetici" |Dinamik|URI silme adlandırma için güvenlik yapılandırması. |
|DeleteNetwork|dize, varsayılan "Yönetici" |Dinamik|Kapsayıcı ağını siler |
|Silme Hizmeti |dize, varsayılan "Yönetici" |Dinamik|Hizmet silme için güvenlik yapılandırması. |
|Silme Hacmi|dize, varsayılan "Yönetici"|Dinamik|Bir birimi siler.| 
|Sayısal Özellikler |dize, varsayılan\|\|"Yönetici Kullanıcı" | Dinamik|Özellik numaralandırmaadlandırma için güvenlik yapılandırması. |
|Sayısal Adlar |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| URI numaralandırmaadlandırma için güvenlik yapılandırması. |
|Dosya İçeriği |dize, varsayılan "Yönetici" |Dinamik| Görüntü deposu istemci dosya aktarımı için güvenlik yapılandırması (küme nin dışında). |
|Filedownload |dize, varsayılan "Yönetici" |Dinamik| Görüntü deposu istemci dosya indirme başlatma (küme için dış). |
|FinishInfrastructureTask |dize, varsayılan "Yönetici" |Dinamik| Altyapı görevlerini bitirmek için güvenlik yapılandırması. |
|GetChaosReport | dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Belirli bir zaman aralığında Kaos durumunu getirir. |
|GetClusterYapılandırma | dize, varsayılan\|\|"Yönetici Kullanıcı" | Dinamik|GetClusterConfiguration'ı bir bölüm üzerinde ikna eder. |
|GetClusterConfigurationUpgradeStatus | dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Bir bölüm üzerinde GetClusterConfigurationUpgradeStatus neden olur. |
|GetFabricUpgradeStatus |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Yoklama küme yükseltme durumu için güvenlik yapılandırması. |
|Klasör Boyutunu GetFolderSize |dize, varsayılan "Yönetici" |Dinamik|FileStoreService'in klasör boyutu için güvenlik yapılandırması |
|GetNodeDeActivationStatus |dize, varsayılan "Yönetici" |Dinamik| Devre dışı bırakma durumunu denetlemek için güvenlik yapılandırması. |
|GetNodeTransitionProgress | dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Düğüm geçiş komutu üzerinde ilerleme elde etmek için güvenlik yapılandırması. |
|GetPartitionDataLossProgress | dize, varsayılan\|\|"Yönetici Kullanıcı" | Dinamik|Bir çağrıldaki veri kaybı api çağrısı için ilerlemeyi getirir. |
|GetPartitionQuorumLossProgress | dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Invoke quorum kaybı api çağrısı için ilerleme yi tir. |
|GetPartitionRestartİlerleme | dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Yeniden başlatma bölümü api çağrısı için ilerlemeyi getirir. |
|GetSecrets|dize, varsayılan "Yönetici"|Dinamik|Gizli değerleri alın |
|GetServiceDescription |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Uzun anket hizmeti bildirimleri ve okuma hizmeti açıklamaları için güvenlik yapılandırması. |
|GetStagingLocation |dize, varsayılan "Yönetici" |Dinamik| Görüntü deposu istemcisi evreleme konum alma için güvenlik yapılandırması. |
|GetStoreLocation |dize, varsayılan "Yönetici" |Dinamik| Görüntü deposu istemcisi konum alma için güvenlik yapılandırması. |
|GetUpgradeOrchestrationServiceState|dize, varsayılan "Yönetici"| Dinamik|Bir bölüm üzerinde GetUpgradeOrchestrationServiceState induces |
|Yükseltmeleri AlmaBekleme Onayı |dize, varsayılan "Yönetici" |Dinamik| Bir bölüm üzerinde GetUpgradesPendingApproval neden olur. |
|Yükseltme Durumu Alma |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Yoklama uygulaması yükseltme durumu için güvenlik yapılandırması. |
|ınternallist |dize, varsayılan "Yönetici" | Dinamik|Görüntü deposu istemci dosya listesi işlemi (dahili) için güvenlik yapılandırması. |
|InvokeContainerApi|string,varsayılan "Yönetici"|Dinamik|Kapsayıcı API'yi çağırma |
|InvokeInfrastructureCommand |dize, varsayılan "Yönetici" |Dinamik| Altyapı görev yönetimi komutları için güvenlik yapılandırması. |
|InvokeInfrastructureQuery |dize, varsayılan\|\|"Yönetici Kullanıcı" | Dinamik|Altyapı görevlerini sorgulamak için güvenlik yapılandırması. |
|Liste |dize, varsayılan\|\|"Yönetici Kullanıcı" | Dinamik|Görüntü deposu istemci dosya listesi işlemi için güvenlik yapılandırması. |
|MoveNextFabricUpgradeDomain |dize, varsayılan "Yönetici" |Dinamik| Açık bir Yükseltme Etki Alanı ile küme yükseltmelerini sürdürmek için güvenlik yapılandırması. |
|MoveNextUpgradeDomain |dize, varsayılan "Yönetici" |Dinamik| Uygulama yükseltmelerini açık bir Yükseltme Etki Alanı yla devam ettirmek için güvenlik yapılandırması. |
|MoveReplicaControl |dize, varsayılan "Yönetici" | Dinamik|Çoğaltmayı taşıyın. |
|NameExists |dize, varsayılan\|\|"Yönetici Kullanıcı" | Dinamik|URI varoluş denetimlerini adlandırmak için güvenlik yapılandırması. |
|DüğümKontrol |dize, varsayılan "Yönetici" |Dinamik| Başlangıç için güvenlik yapılandırması; durdurma; ve düğümleri yeniden başlatın. |
|DüğümKaldırıldı |dize, varsayılan "Yönetici" |Dinamik| Düğüm durumunu bildirmek için güvenlik yapılandırması kaldırıldı. |
|Ping |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| İstemci pingleri için güvenlik yapılandırması. |
|PredeployPackageToNode |dize, varsayılan "Yönetici" |Dinamik| Dağıtım öncesi api. |
|Önek Çözümlü Hizmet |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Şikayet tabanlı hizmet öneki çözümü için güvenlik yapılandırması. |
|PropertyReadBatch |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Adlandırma özelliği için güvenlik yapılandırması okuma işlemleri. |
|PropertyWriteBatch |dize, varsayılan "Yönetici" |Dinamik|Özellik yazma işlemlerini adlandırmak için güvenlik yapılandırmaları. |
|Provizyon Uygulama Türü |dize, varsayılan "Yönetici" |Dinamik| Uygulama türü sağlama için güvenlik yapılandırması. |
|Geçici Kumaş |dize, varsayılan "Yönetici" |Dinamik| MSI ve/veya Cluster Manifest sağlama için güvenlik yapılandırması. |
|Sorgu |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Sorgular için güvenlik yapılandırması. |
|RecoverPartition |dize, varsayılan "Yönetici" | Dinamik|Bir bölümü kurtarmak için güvenlik yapılandırması. |
|RecoverPartitions |dize, varsayılan "Yönetici" | Dinamik|Bölümleri kurtarmak için güvenlik yapılandırması. |
|RecoverServicePartitions |dize, varsayılan "Yönetici" |Dinamik| Hizmet bölümlerini kurtarmak için güvenlik yapılandırması. |
|RecoverSystemPartitions |dize, varsayılan "Yönetici" |Dinamik| Sistem hizmet bölümlerini kurtarmak için güvenlik yapılandırması. |
|RemoveNodeDeactivations |dize, varsayılan "Yönetici" |Dinamik| Birden çok düğümde devre dışı bırakma yı tersine çevirmek için güvenlik yapılandırması. |
|ReportFabricUpgradeHealth |dize, varsayılan "Yönetici" |Dinamik| Geçerli yükseltme ilerlemesiyle küme yükseltmelerini sürdürmek için güvenlik yapılandırması. |
|ReportFault |dize, varsayılan "Yönetici" |Dinamik| Hata bildirmek için güvenlik yapılandırması. |
|ReportHealth |dize, varsayılan "Yönetici" |Dinamik| Sistem durumunu bildirmek için güvenlik yapılandırması. |
|ReportUpgradeHealth |dize, varsayılan "Yönetici" |Dinamik| Geçerli yükseltme ilerleme ile uygulama yükseltmeleri devam etmek için güvenlik yapılandırması. |
|ResetPartitionLoad |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Bir failoverUnit için sıfırlama yükü için güvenlik yapılandırması. |
|ÇözümLeAd Sahibi |dize, varsayılan\|\|"Yönetici Kullanıcı" | Dinamik|Uri sahibinin adlandırmaçözümiçin güvenlik yapılandırması. |
|Çözümleme |dize, varsayılan\|\|"Yönetici Kullanıcı" | Dinamik|Sistem hizmetlerini çözmek için güvenlik yapılandırması. |
|Çözüm Hizmeti |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Şikayet tabanlı hizmet çözümü için güvenlik yapılandırması. |
|ÇözümSistemiServisi|dize, varsayılan\|\|"Yönetici Kullanıcı"|Dinamik| Sistem hizmetlerini çözmek için güvenlik yapılandırması |
|RollbackApplicationYükseltme |dize, varsayılan "Yönetici" |Dinamik| Uygulama yükseltmelerini geri almak için güvenlik yapılandırması. |
|RollbackFabricYükseltme |dize, varsayılan "Yönetici" |Dinamik| Küme yükseltmelerini geri almak için güvenlik yapılandırması. |
|Hizmet Bildirimleri |dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Olay tabanlı hizmet bildirimleri için güvenlik yapılandırması. |
|SetUpgradeOrchestrationServiceState|dize, varsayılan "Yönetici"| Dinamik|Bir bölüm üzerinde SetUpgradeOrchestrationServiceState induces |
|Başlangıç Onaylı Yükseltmeler |dize, varsayılan "Yönetici" |Dinamik| Bir bölüm üzerinde StartApprovedUpgrades neden olur. |
|Başlangıç Kaos |dize, varsayılan "Yönetici" |Dinamik| Kaos Başlar - zaten başlamadıysa. |
|StartClusterConfigurationUpgrade |dize, varsayılan "Yönetici" |Dinamik| StartClusterConfigurationUpgrade'i bir bölüme doğru iter. |
|Başlangıç AltyapıGörevi |dize, varsayılan "Yönetici" | Dinamik|Altyapı görevlerini başlatmak için güvenlik yapılandırması. |
|StartNodeGeçiş |dize, varsayılan "Yönetici" |Dinamik| Düğüm geçişini başlatmak için güvenlik yapılandırması. |
|StartPartitionDataLoss |dize, varsayılan "Yönetici" |Dinamik| Bir bölümüzerinde veri kaybına neden olur. |
|StartPartitionQuorumKaybı |dize, varsayılan "Yönetici" |Dinamik| Bir bölüm üzerinde çoğunluk kaybına neden olur. |
|StartPartitionRestart |dize, varsayılan "Yönetici" |Dinamik| Aynı anda bir bölümün bazı veya tüm yinelemelerini yeniden başlatır. |
|Kaosu Durdurun |dize, varsayılan "Yönetici" |Dinamik| Kaos durur - eğer başlatıldı. |
|ToggleVerboseServicePlacementHealthReporting | dize, varsayılan\|\|"Yönetici Kullanıcı" |Dinamik| Arama Verbose ServicePlacement HealthReporting için güvenlik yapılandırması. |
|UnprovisionApplicationType |dize, varsayılan "Yönetici" |Dinamik| Uygulama türü sağlama sağlama için güvenlik yapılandırması. |
|UnprovisionFabric |dize, varsayılan "Yönetici" |Dinamik| MSI ve/veya Cluster Manifest unprovisioning için güvenlik yapılandırması. |
|GüvenilmezTransportControl |dize, varsayılan "Yönetici" |Dinamik| Davranış eklemek ve kaldırmak için güvenilmez aktarım. |
|Güncelleme Hizmeti |dize, varsayılan "Yönetici" |Dinamik|Hizmet güncelleştirmeleri için güvenlik yapılandırması. |
|Yükseltme Uygulaması |dize, varsayılan "Yönetici" |Dinamik| Uygulama yükseltmelerini başlatmak veya bölmek için güvenlik yapılandırması. |
|YükseltmeComposeDağıtım|dize, varsayılan "Yönetici"| Dinamik|Oluşturma dağıtımını yükseltir |
|YükseltmeKumaş |dize, varsayılan "Yönetici" |Dinamik| Küme yükseltmelerini başlatmak için güvenlik yapılandırması. |
|Karşıya Yükle |dize, varsayılan "Yönetici" | Dinamik|Görüntü deposu istemci yükleme işlemi için güvenlik yapılandırması. |

## <a name="securityclientcertificateissuerstores"></a>Güvenlik/Müşteri SertifikasıVerenMağazalar

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|VerenStoreKeyValueMap, varsayılan Yok |Dinamik|Müşteri sertifikaları için X509 ihraççı sertifika mağazaları; Ad = clientIssuerCN; Değer = virgül ayrılmış mağazalar listesi |

## <a name="securityclientx509names"></a>Güvenlik/IstemciX509Names

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|X509NameMap, varsayılan Yok|Dinamik|Bu liste "Ad" ve "Değer" çiftinin listesidir. Her "Ad" müşteri işlemleri için yetkilendirilen X509 sertifikalarının özne ortak adı veya DnsName'sidir. Belirli bir "Ad" için", "Değer" veren intihal için sertifika parmak izlerinin virgüllü ayrı bir listesidir, boş değilse, istemci sertifikalarının doğrudan vereni listede olmalıdır.|

## <a name="securityclustercertificateissuerstores"></a>Güvenlik/ClusterCertificateIssuerStores

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|VerenStoreKeyValueMap, varsayılan Yok |Dinamik|Küme sertifikaları için X509 ihraççı sertifika depoları; Ad = clusterIssuerCN; Değer = virgül ayrılmış mağazalar listesi |

## <a name="securityclusterx509names"></a>Güvenlik/ClusterX509Names

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|X509NameMap, varsayılan Yok|Dinamik|Bu liste "Ad" ve "Değer" çiftinin listesidir. Her "Ad" küme işlemleri için yetkilendirilen X509 sertifikalarının özne ortak adı veya DnsName'sidir. Belirli bir "Ad","Değer" için, boş değilse, boş değilse, küme sertifikalarının doğrudan vereni listede olmalıdır.|

## <a name="securityservercertificateissuerstores"></a>Güvenlik/ServerCertificateIssuerStores

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|VerenStoreKeyValueMap, varsayılan Yok |Dinamik|Sunucu sertifikaları için X509 veren sertifika depoları; Ad = serverIssuerCN; Değer = virgül ayrılmış mağazalar listesi |

## <a name="securityserverx509names"></a>Güvenlik/ServerX509İsimler

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup|X509NameMap, varsayılan Yok|Dinamik|Bu liste "Ad" ve "Değer" çiftinin listesidir. Her "Ad" sunucu işlemleri için yetkilendirilen X509 sertifikalarının özne ortak adı veya DnsName'sidir. Belirli bir "Ad" için", "Değer", boş değilse, sunucu sertifikalarının doğrudan veren listesinde olması gerekir, veren sabitleyici için sertifika parmak izlerinin virgülayrı bir listesidir.|

## <a name="setup"></a>Kurulum

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|KonteynerAğ Adı|dize, varsayılan ""| Statik |Kapsayıcı ağ kurarken kullanılacak ağ adı.|
|KonteynerAğKurulumu|bool, varsayılan FALSE (Linux) ve varsayılan TRUE (Windows) olduğunu| Statik |Bir kapsayıcı ağı kurmak için olsun.|
|KumaşDataRoot |Dize | İzin Verilmiyor |Service Fabric veri kökü dizini. Azure için varsayılan d:\svcfab |
|FabricLogRoot |Dize | İzin Verilmiyor |Hizmet kumaş günlük kök dizini. Burası SF günlüklerinin ve izlerinin yerleştirildiği yerdir. |
|DüğümlerToBeRemoved|dize, varsayılan ""| Dinamik |Yapılandırma yükseltmesinin bir parçası olarak kaldırılması gereken düğümler. (Sadece Bağımsız Dağıtımlar için)|
|ServiceRunasHesap Adı |Dize | İzin Verilmiyor |Kumaş ana bilgisayar hizmetini çalıştırılan hesap adı. |
|SkipContainerNetworkResetOnReboot|bool, varsayılan FALSE olduğunu|Notallowed|Yeniden başlatılırken kapsayıcı ağını sıfırlamayı atlayıp atlamayacağı.|
|SkipFirewallYapılandırma |Bool, varsayılan yanlış | İzin Verilmiyor |Güvenlik duvarı ayarlarının sistem tarafından ayarlanıp ayarlanmaması gerekmediğini belirtir. Bu yalnızca windows güvenlik duvarı kullanıyorsanız geçerlidir. Üçüncü taraf güvenlik duvarları kullanıyorsanız, sistem ve uygulamaların kullanması için bağlantı noktalarını açmanız gerekir |

## <a name="tokenvalidationservice"></a>TokenValidationService

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Sağlayıcılar |dize, varsayılan "DSTS" |Statik|Virgülle ayrılmış belirteç doğrulama sağlayıcılarının listesi etkinleştirilir (geçerli sağlayıcılar şunlardır: DSTS; AAD). Şu anda herhangi bir zamanda yalnızca tek bir sağlayıcı etkinleştirilebilir. |

## <a name="traceetw"></a>İzleme/Etw

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Düzey |Int, varsayılan 4 | Dinamik |İzleme etw düzeyi 1, 2, 3, 4 değerlerini alabilir. Desteklenebilmek için izleme seviyesini 4'te tutmanız gerekir |

## <a name="transactionalreplicator"></a>İşlemselÇoğaltma

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|BatchAcknowledgementInterval | Saniye cinsinden zaman, varsayılan 0,015 | Statik | Zaman açıklığı saniye cinsinden belirtin. Bir bildirim göndermeden önce çoğaltıcının bir işlem aldıktan sonra beklediği süreyi belirler. Bu süre içinde alınan diğer işlemler, onaylarını ağ trafiğini azaltan tek bir ileti > geri gönderir, ancak çoğaltıcının iş kısmını potansiyel olarak azaltır. |
|MaxcopyQueueSize |Uint, varsayılan 16384 | Statik |Bu, çoğaltma işlemlerini sürdüren sıranın başlangıç boyutunu tanımlayan en büyük değerdir. 2 gücünde olması gerektiğini unutmayın. Çalışma zamanı sırasında kuyruk bu boyuta büyürse, birincil ve ikincil çoğalıcılar arasında daraltılır. |
|MaxPrimaryReplicationQueueMemorySize |Uint, varsayılan 0 | Statik |Bu, baytlar birincil çoğaltma sırasının maksimum değeridir. |
|MaxPrimaryReplicationQueueSize |Uint, varsayılan 8192 | Statik |Bu, birincil çoğaltma kuyruğunda bulunabilecek en fazla işlem sayısıdır. 2 gücünde olması gerektiğini unutmayın. |
|MaxReplicationMessageSize |Uint, varsayılan 52428800 | Statik | Çoğaltma işlemlerinin en yüksek ileti boyutu. Varsayılan değer 50MB'dır. |
|MaxSecondaryReplicationQueueMemorySize |Uint, varsayılan 0 | Statik |Bu, baytlar ikincil çoğaltma sırasının maksimum değeridir. |
|MaxSecondaryReplicationQueueSize |Uint, varsayılan 16384 | Statik |Bu, ikincil çoğaltma kuyruğunda bulunabilecek en fazla işlem sayısıdır. 2 gücünde olması gerektiğini unutmayın. |
|ÇoğaltıcıAdres |dize, varsayılan "localhost:0" | Statik | İşlemleri göndermek/almak için diğer yinelemelerle bağlantı kurmak için Windows Fabric Replicator tarafından kullanılan dize -'IP:Port' şeklindeki bitiş noktası. |

## <a name="transport"></a>Aktarım
| **Parametre** | **İzin Verilen Değerler** |**Yükseltme ilkesi** |**Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|BağlantıOpenTimeout|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(60)|Statik|Zaman açıklığı saniye cinsinden belirtin. Hem gelen hem de kabul eden tarafta bağlantı kurulumu için zaman dilimi (güvenli modda güvenlik anlaşması dahil) |
|FrameHeaderErrorCheckEnabled|bool, varsayılan DOĞRU|Statik|Güvenli olmayan modda çerçeve üstbilgisinde hata denetimi için varsayılan ayar; bileşen ayarı bunu geçersiz kılar. |
|İletiHatası Denetimi Etkin|bool, varsayılan FALSE olduğunu|Statik|İleti üstbilgisinde ve gövdesigüvenli olmayan modda hata denetimi için varsayılan ayar; bileşen ayarı bunu geçersiz kılar. |
|Çözüm Seçeneği|dize, varsayılan "belirtilmemiş"|Statik|FQDN'nin nasıl çözüleceğini belirler.  Geçerli değerler "belirtilmemiş/ipv4/ipv6"dır. |
|SendTimeout|TimeSpan, varsayılan yaygındır::TimeSpan::FromSeconds(300)|Dinamik|Zaman açıklığı saniye cinsinden belirtin. Sıkışmış bağlantıyı algılamak için zaman arayı gönderin. TCP hata raporları bazı ortamda güvenilir değildir. Bunun kullanılabilir ağ bant genişliğine ve giden verilerin boyutuna\*(MaxMessageSize\/\*SendQueueSizeLimit) göre ayarlanması gerekebilir. |

## <a name="upgradeorchestrationservice"></a>UpgradeOrchestrationService

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Otomatik YükseltmeEtkin | Bool, varsayılan doğrudur |Statik| Bir hedef durumu dosyasına dayalı otomatik yoklama ve yükseltme eylemi. |
|Otomatik YükseltmeInstallEnabled|Bool, varsayılan FALSE olduğunu|Statik|Bir hedef durumu dosyasına dayalı kod yükseltme eyleminin otomatik yoklanması, sağlanması ve yüklenmesi.|
|GoalStateExpirationReminderindays|int, varsayılan 30|Statik|Hedef durumu anımsatıcısının gösterilmesi gereken kalan gün sayısını ayarlar.|
|MinReplicaSetSize |Int, varsayılan 0 |Statik |UpgradeOrchestrationService için MinReplicaSetSize.|
|YerleşimKısıtlamalar | dize, varsayılan "" |Statik| UpgradeOrchestrationService için Yerleşim Kısıtlamalar. |
|ÇoğunlukLossWaitDuration | Saniye cinsinden zaman, varsayılan MaxValue olduğunu |Statik| Zaman açıklığı saniye cinsinden belirtin. YükseltmeIçin QuorumLossWaitDuration. |
|ÇoğaltmaRestartWaitDuration | Saniye cinsinden süre, varsayılan süre 60 dakikadır|Statik| Zaman açıklığı saniye cinsinden belirtin. ReplicaRestartWaitDuration UpgradeOrchestrationService için. |
|StandbyReplicaKeepDuration | Saniye cinsinden zaman, varsayılan 60*24*7 dakika |Statik| Zaman açıklığı saniye cinsinden belirtin. UpgradeOrchestrationService için StandByReplicaKeepDuration. |
|TargetReplicaSetSize |Int, varsayılan 0 |Statik |UpgradeOrchestrationService için TargetReplicaSetSize. |
|Yükseltme Onayı Gerekli | Bool, varsayılan yanlış | Statik|Kod yükseltme sağlamak için ayar, devam etmeden üste yürütü |

## <a name="upgradeservice"></a>Yükseltme Hizmeti

| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|BaseUrl | dize, varsayılan "" |Statik|UpgradeService için BaseUrl. |
|ClusterId | dize, varsayılan "" |Statik|UpgradeService için ClusterId. |
|Koordinatör Tipi | dize, varsayılan "WUTest"|İzin Verilmiyor|UpgradeService için KoordinatörTürü. |
|MinReplicaSetSize | Int, varsayılan 2 |İzin Verilmiyor| UpgradeService için MinReplicaSetSize. |
|OnlyBaseUpgrade | Bool, varsayılan yanlış |Dinamik|Yükseltme Hizmeti için Yalnızca BaseUpgrade. |
|YerleşimKısıtlamalar |dize, varsayılan "" |İzin Verilmiyor|Yükseltme hizmeti için Yerleşim Kısıtlamaları. |
|PollIntervalInSeconds|Zaman aralığı, varsayılan yaygındır::TimeSpan::FromSeconds(60) |Dinamik|Zaman açıklığı saniye cinsinden belirtin. ARM yönetimi işlemleri için UpgradeService anketi arasındaki aralık. |
|TargetReplicaSetSize | Int, varsayılan 3 |İzin Verilmiyor| UpgradeService için TargetReplicaSetSize. |
|TestCabFolder | dize, varsayılan "" |Statik| Yükseltme Hizmeti için TestCabFolder. |
|X509findtype | dize, varsayılan ""|Dinamik| UpgradeService için X509FindType. |
|X509FindValue | dize, varsayılan "" |Dinamik| UpgradeService için X509FindValue. |
|X509SecondaryFindValue | dize, varsayılan "" |Dinamik| UpgradeService için X509SecondaryFindValue. |
|X509Mağaza Konumu | dize, varsayılan "" |Dinamik| UpgradeService için X509StoreLocation. |
|X509StoreName | dize, varsayılan "Benim"|Dinamik|UpgradeService için X509StoreName. |

## <a name="userservicemetriccapacities"></a>UserServiceMetricKapasiteleri
| **Parametre** | **İzin Verilen Değerler** | **Yükseltme İlkesi** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
|Propertygroup| UserServiceMetricCapacitiesMap, varsayılan yok | Statik | Kullanıcı hizmetleri kaynak yönetişimi bir koleksiyon Otomatik Algılama mantığını etkilediği gibi statik olması gerekir |

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [Azure kümesinin yapılandırmasını yükseltin](service-fabric-cluster-config-upgrade-azure.md) ve [bağımsız bir kümenin yapılandırmasını yükseltin.](service-fabric-cluster-config-upgrade-windows-server.md)
