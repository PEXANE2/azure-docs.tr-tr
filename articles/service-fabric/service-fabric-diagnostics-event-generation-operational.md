---
title: Azure Hizmet Kumaş Etkinlik Listesi
description: Kümeleri izlemeye yardımcı olmak için Azure Service Fabric tarafından sağlanan olayların kapsamlı listesi.
author: srrengar
ms.topic: reference
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: e69b407bc7d58a83616daa44272ec008ccff9fad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258544"
---
# <a name="list-of-service-fabric-events"></a>Hizmet Kumaşı etkinlikleri listesi 

Service Fabric, kümenizin [Hizmet Kumaşı Olayları](service-fabric-diagnostics-events.md)olarak durumunu size bildirmek için birincil küme olayları kümesini ortaya çıkarır. Bunlar, Service Fabric tarafından düğümlerinizde gerçekleştirilen eylemlere ve küme sahibi/işleç tarafından alınan küme veya yönetim kararlarına dayanır. Bu olaylara, [Azure Monitor günlüklerini kümenizle](service-fabric-diagnostics-oms-setup.md)yapılandırma veya [EventStore'u](service-fabric-diagnostics-eventstore.md)sorgulama gibi çeşitli şekillerde yapılandırarak erişilebilir. Windows makinelerde bu olaylar EventLog'a aktarılır , böylece Olay Görüntüleyicisi'nde Service Fabric Events'i görebilirsiniz. 

İşte bu olayların bazı özellikleri
* Her olay, uygulama, hizmet, düğüm, yineleme gibi kümedeki belirli bir varlığa bağlıdır.
* Her olay ortak alanlar kümesi içerir: EventInstanceId, EventName ve Kategori.
* Her olay, olayı ilişkili olduğu varlığa geri bağlayan alanlar içerir. Örneğin, ApplicationCreated olayı oluşturulan uygulamanın adını tanımlayan alanlara sahip olacaktır.
* Olaylar, daha fazla analiz yapmak için çeşitli araçlarda tüketilen şekilde yapılandırılmıştır. Ayrıca, bir olay için ilgili ayrıntılar uzun bir Dize yerine ayrı özellikler olarak tanımlanır. 
* Olaylar Hizmet Kumaşı'ndaki farklı alt sistemler tarafından yazılır aşağıdaki Kaynak(Görev) ile tanımlanır. [Hizmet Kumaş Mimarisi](service-fabric-architecture.md) ve Servis [Kumaş Teknik Genel Bakış](service-fabric-technical-overview.md)bu alt sistemler hakkında daha fazla bilgi edinebilirsiniz.

Burada varlık tarafından düzenlenen bu Hizmet Kumaş ı etkinliklerinin bir listesi verem.

## <a name="cluster-events"></a>Küme olayları

**Yükseltme olaylarını kümele**

Küme yükseltmeleri hakkında daha fazla bilgi [burada](service-fabric-cluster-upgrade-windows-server.md)bulabilirsiniz.

| Eventıd | Adı | Kategori | Açıklama |Kaynak (Görev) | Düzey | 
| --- | --- | --- | --- | --- | --- | 
| 29627 | Küme Yükseltme Başladı | Yükseltme | Küme yükseltmesi başladı | CM | Bilgilendirici |
| 29628 | KümeYükseltme Tamamlandı | Yükseltme | Küme yükseltmesi tamamlandı | CM | Bilgilendirici | 
| 29629 | ClusterUpgradeRollbackBaşlatıldı | Yükseltme | Bir küme yükseltmesi geri almaya başladı  | CM | Uyarı | 
| 29630 | ClusterUpgradeRollbackTamamlandı | Yükseltme | Bir küme yükseltmesi geri alma tamamlandı | CM | Uyarı | 
| 29631 | ClusterUpgradeDomainTamamlandı | Yükseltme | Bir yükseltme etki alanı küme yükseltmesi sırasında yükseltmeyi tamamladı | CM | Bilgilendirici | 

## <a name="node-events"></a>Düğüm olayları

**Düğüm yaşam döngüsü olayları** 

| Eventıd | Adı | Kategori | Açıklama |Kaynak (Görev) | Düzey |
| --- | --- | ---| --- | --- | --- | 
| 18602 | DüğümDeEtkintamamlandı | StateTransition | Düğümün devre dışı bırakılması tamamlandı | FM | Bilgilendirici | 
| 18603 | Düğüm | StateTransition | Küme bir düğüm algılandı | FM | Bilgilendirici | 
| 18604 | DüğümDown | StateTransition | Küme bir düğüm kapatıldığını algıladı. Düğüm yeniden başlatma sırasında, bir NodeDown olayı ve ardından bir NodeUp olayı görürsünüz |  FM | Hata | 
| 18605 | DüğümAddedToCluster | StateTransition |  Kümeye yeni bir düğüm eklendi ve Hizmet Kumaşı bu düğüme uygulamaları dağıtabilir | FM | Bilgilendirici | 
| 18606 | DüğümRemovedFromCluster | StateTransition |  Kümeden bir düğüm kaldırıldı. Service Fabric artık uygulamaları bu düğüme dağıtmaz | FM | Bilgilendirici | 
| 18607 | DüğümDeetkinbaşlatıldı | StateTransition |  Düğümün devre dışı bırakılması başladı | FM | Bilgilendirici | 
| 25621 | NodeOpenSucceeded | StateTransition |  Düğüm başarıyla başladı | FabricNode | Bilgilendirici | 
| 25622 | DüğümOpenFailed | StateTransition |  Bir düğüm başlatıp halkaya katılamadı | FabricNode | Hata | 
| 25624 | DüğümKapalı | StateTransition |  Düğüm başarıyla kapatıldı | FabricNode | Bilgilendirici | 
| 25626 | Düğümİptal edildi | StateTransition |  Bir düğüm zarafetle kapatıldı | FabricNode | Hata | 

## <a name="application-events"></a>Uygulama etkinlikleri

**Uygulama yaşam döngüsü etkinlikleri**

| Eventıd | Adı | Kategori | Açıklama |Kaynak (Görev) | Düzey | 
| --- | --- | --- | --- | --- | --- | 
| 29620 | Oluşturulan Uygulamalar | Yaşam döngüsü | Yeni bir uygulama oluşturuldu | CM | Bilgilendirici | 
| 29625 | Uygulamalar Silindi | Yaşam döngüsü | Varolan bir uygulama silindi | CM | Bilgilendirici | 
| 23083 | UygulamaSüreçÇıktı | Yaşam döngüsü | Bir uygulama içinde bir işlem çıktı | Barındırma | Bilgilendirici | 

**Uygulama yükseltme etkinlikleri**

Uygulama yükseltmeleri hakkında daha fazla bilgi [burada](service-fabric-application-upgrade.md)bulabilirsiniz.

| Eventıd | Adı | Kategori | Açıklama |Kaynak (Görev) | Düzey | 
| --- | --- | ---| --- | --- | --- | 
| 29621 | Başvurular Başladı | Yükseltme | Uygulama yükseltmesi başladı | CM | Bilgilendirici | 
| 29622 | Başvurular Tamamlandı | Yükseltme | Uygulama yükseltmesi tamamlandı | CM | Bilgilendirici | 
| 29623 | UygulamaYükseltmeRollbackStarted | Yükseltme | Uygulama yükseltmesi geri almaya başladı |CM | Uyarı | 
| 29624 | UygulamaYükseltmeRollbackTamamlandı | Yükseltme | Uygulama yükseltmesi geri alma tamamlandı | CM | Uyarı | 
| 29626 | UygulamalarYükseltmeDomainTamamlandı | Yükseltme | Yükseltme etki alanı, uygulama yükseltmesi sırasında yükseltmeyi tamamladı | CM | Bilgilendirici | 

## <a name="service-events"></a>Servis etkinlikleri

**Servis yaşam döngüsü etkinlikleri**

| Eventıd | Adı | Kategori | Açıklama |Kaynak (Görev) | Düzey | 
| --- | --- | ---| --- | --- | --- |
| 18657 | Hizmet Oluşturuldu | Yaşam döngüsü | Yeni bir hizmet oluşturuldu | FM | Bilgilendirici | 
| 18658 | Hizmet Silindi | Yaşam döngüsü | Varolan bir hizmet silindi | FM | Bilgilendirici | 

## <a name="partition-events"></a>Bölüm olayları

**Bölüm taşıma olayları**

| Eventıd | Adı | Kategori | Açıklama |Kaynak (Görev) | Düzey | 
| --- | --- | ---| --- | --- | --- |
| 18940 | PartitionReconfigured | Yaşam döngüsü | Bölüm yeniden yapılandırması tamamlandı | RA | Bilgilendirici | 

## <a name="replica-events"></a>Çoğaltma olayları

**Yineleme yaşam döngüsü olayları**

| Eventıd | Adı | Kategori | Açıklama |Kaynak (Görev) | Düzey |
| --- | --- | ---| --- | --- | --- |
| 61701 | GüvenilirSözlük Açıldı | Yaşam döngüsü | Güvenilir sözlük açıldı | DistributedDictionary | Bilgilendirici |
| 61702 | GüvenilirSözlükKapalı | Yaşam döngüsü | Güvenilir sözlük kapandı | DistributedDictionary | Bilgilendirici |
| 61703 | ReliableDictionaryCheckpointRecovered | Yaşam döngüsü | Güvenilir sözlük kontrol noktasını kurtardı | DistributedDictionary | Bilgilendirici |
| 61704 | GüvenilirSözlükCheckpointFilesSent | Yaşam döngüsü | Yineleme güvenilir sözlük denetim noktası dosyalarını gönderdi | DistributedDictionary | Bilgilendirici |
| 61705 | GüvenilirSözlükCheckpointFilesReceived | Yaşam döngüsü | Yineleme güvenilir sözlük denetim noktası dosyaları aldı | DistributedDictionary | Bilgilendirici |
| 61963 | ReliableQueueOpened | Yaşam döngüsü | Güvenilir sıra açıldı | DistributedQueue | Bilgilendirici |
| 61964 | ReliableQueueKapalı | Yaşam döngüsü | Güvenilir sıra kapandı | DistributedQueue | Bilgilendirici |
| 61965 | ReliableQueueCheckpointKurtarıldı | Yaşam döngüsü | Güvenilir kuyruk denetim noktasını kurtardı | DistributedQueue | Bilgilendirici |
| 61966 | ReliableQueueCheckpointFilesSent | Yaşam döngüsü | Yineleme güvenilir sıranın denetim noktası dosyalarını gönderdi | DistributedQueue | Bilgilendirici |
| 63647 | ReliableQueueCheckpointFilesReceived | Yaşam döngüsü | Yineleme güvenilir sıranın denetim noktası dosyalarını aldı | DistributedQueue | Bilgilendirici |
| 63648 | ReliableConcurrentQueueOpened | Yaşam döngüsü | Güvenilir eşzamanlı kuyruk açıldı | ReliableConcurrentQueue | Bilgilendirici |
| 63649 | ReliableConcurrentQueueKapalı | Yaşam döngüsü | Güvenilir eşzamanlı kuyruk kapandı | ReliableConcurrentQueue | Bilgilendirici |
| 63650 | ReliableConcurrentQueueCheckpointKurtarıldı | Yaşam döngüsü | Güvenilir eşzamanlı kuyruk denetim noktasını kurtardı | ReliableConcurrentQueue | Bilgilendirici |
| 61687 | TStoreHatası | Hata | Güvenilir toplama beklenmeyen bir hata aldı | TStore | Hata |
| 63831 | Birincil FullCopyBaşlatılan | Yaşam döngüsü | Birincil yineleme tam bir kopyasını başlattı | TReplicator | Bilgilendirici |
| 63832 | Birincil Kısmi Fotokopi Başlatılan | Yaşam döngüsü | Birincil yineleme kısmi bir kopyasını başlattı | TReplicator | Bilgilendirici |
| 16831 | BuildIdleReplicaStarted | Yaşam döngüsü | Birincil yineleme boşta yineleme oluşturmaya başladı | Çoğaltma | Bilgilendirici |
| 16832 | BuildIdleReplicaTamamlandı | Yaşam döngüsü | Birincil yineleme boşta yineleme oluşturma tamamlandı | Çoğaltma | Bilgilendirici |
| 16833 | BuildIdleReplicaFailed | Yaşam döngüsü | Birincil yineleme boşta yineleme oluşturma başarısız oldu | Çoğaltma | Uyarı |
| 16834 | BirincilÇoğaltmaQueueFull | Durum | Birincil çoğaltma çoğaltma sırası dolu | Çoğaltma | Uyarı |
| 16835 | Birincil ÇoğaltmaQueueWarning | Durum | Birincil çoğaltma çoğaltma sırası tam yakın | Çoğaltma | Uyarı |
| 16836 | BirincilÇoğaltmaQueueWarningMitigated | Durum | Birincil çoğaltma çoğaltma sırası tamam | Çoğaltma | Bilgilendirici |
| 16837 | İkincilÇoğaltmaQueueFull | Durum | İkincil çoğaltma çoğaltma sırası dolu | Çoğaltma | Uyarı |
| 16838 | İkincilÇoğaltmaQueueWarning | Durum | İkincil çoğaltma çoğaltma sırası doluya yakındır | Çoğaltma | Uyarı |
| 16839 | İkincilÇoğaltmaQueueWarningMitigated | Durum | İkincil çoğaltma çoğaltma sırası tamam | Çoğaltma | Bilgilendirici |
| 16840 | PrimaryFaultedSlowSecondary | Durum | Birincil yineleme yavaş bir ikincil yineleme hata yaptı | Çoğaltma | Uyarı |
| 16841 | ÇoğaltıcıHatalı | Durum | Yineleme hata yaptı | Çoğaltma | Uyarı |

## <a name="container-events"></a>Konteyner olayları

**Konteyner yaşam döngüsü etkinlikleri** 

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 23074 | Konteyner Etkin | Bir kapsayıcı başladı | Barındırma | Bilgilendirici | 1 |
| 23075 | Konteyner Devre Dışı | Bir kapsayıcı durduruldu | Barındırma | Bilgilendirici | 1 |
| 23082 | Konteyner Çıktı | Bir kapsayıcı çıktı - Beklenmeyen Sonlandırma bayrağını kontrol edin | Barındırma | Bilgilendirici | 1 |

## <a name="health-reports"></a>Sağlık raporları

[Service Fabric Health Model](service-fabric-health-introduction.md) zengin, esnek ve genişletilebilir bir sağlık değerlendirmesi ve raporlaması sağlar. Başlangıç Hizmet Kumaş sürüm 6.2, sağlık verileri platform olayları olarak sağlık geçmiş kayıtları sağlamak için yazılır. Sağlık olaylarının hacmini düşük tutmak için, yalnızca Service Fabric etkinlikleri olarak aşağıdakileri yazarız:

* Tüm `Error` `Warning` veya sağlık raporları
* `Ok`geçişler sırasında sağlık raporları
* Bir `Error` veya `Warning` sağlık olayının süresi dolduğunda. Bu, bir varlığın ne kadar süre sağlıksız olduğunu belirlemek için kullanılabilir

**Küme sağlık raporu olayları**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | --- | --- | --- | --- |
| 54428 | ClusterNewHealthReport | Yeni bir küme sağlık raporu kullanılabilir | Hm | Bilgilendirici | 1 |
| 54437 | ClusterHealthReportSüresi Doldu | Varolan küme sağlık raporunun süresi doldu | Hm | Bilgilendirici | 1 |

**Düğüm sağlık raporu olayları**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 54423 | DüğümNewHealthReport | Yeni bir düğüm sağlık raporu kullanılabilir | Hm | Bilgilendirici | 1 |
| 54432 | DüğümHealthReportExpired | Varolan düğüm sağlık raporunun süresi doldu | Hm | Bilgilendirici | 1 |

**Uygulama sağlık raporu olayları**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 54425 | UygulamaYeniSağlık Raporu | Yeni bir uygulama sağlık raporu oluşturuldu. Bu, dağıtılmamış uygulamalar içindir. | Hm | Bilgilendirici | 1 |
| 54426 | DağıtılanUygulamaNewHealthReport | Yeni bir dağıtılmış uygulama durumu raporu oluşturuldu | Hm | Bilgilendirici | 1 |
| 54427 | DeployedServicePackageNewHealthReport | Yeni bir dağıtılmış hizmet sağlık raporu oluşturuldu | Hm | Bilgilendirici | 1 |
| 54434 | UygulamaSağlık Raporu Süresi Doldu | Varolan bir uygulama sağlık raporunun süresi doldu | Hm | Bilgilendirici | 1 |
| 54435 | DeployedApplicationHealthReportExpired | Varolan bir uygulama sistem durumu raporunun süresi doldu | Hm | Bilgilendirici | 1 |
| 54436 | DeployedServicePackageHealthReportExpired | Varolan bir hizmet sağlık raporu süresi doldu | Hm | Bilgilendirici | 1 |

**Hizmet sağlık raporu etkinlikleri**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 54424 | ServiceNewHealthReport | Yeni bir hizmet sağlık raporu oluşturuldu | Hm | Bilgilendirici | 1 |
| 54433 | ServiceHealthReport Süresi Doldu | Varolan bir hizmet sağlık raporunun süresi doldu | Hm | Bilgilendirici | 1 |

**Bölüm sağlık raporu olayları**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 54422 | PartitionNewHealthReport | Yeni bir bölüm sağlık raporu oluşturuldu | Hm | Bilgilendirici | 1 |
| 54431 | PartitionHealthReportSüresi Doldu | Varolan bir bölüm sağlık raporunun süresi doldu | Hm | Bilgilendirici | 1 |

**Yineleme sistem durumu raporu olayları**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 54429 | StatefulReplicaNewHealthReport | Durum dolu bir yineleme sistem durumu raporu oluşturuldu | Hm | Bilgilendirici | 1 |
| 54430 | StatelessInstanceNewHealthReport | Yeni bir stateless örnek sağlık raporu oluşturuldu | Hm | Bilgilendirici | 1 |
| 54438 | StatefulReplicaHealthReportExpired | Varolan durum dolu yineleme sistem durumu raporunun süresi doldu | Hm | Bilgilendirici | 1 |
| 54439 | StatelessInstanceHealthReportExpired | Varolan durum bildirmez örnek sağlık raporunun süresi doldu | Hm | Bilgilendirici | 1 |

## <a name="chaos-testing-events"></a>Kaos test etkinlikleri 

**Kaos oturumu etkinlikleri**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 50021 | Kaos Başladı | Kaos test oturumu başladı | Test edilebilirlik | Bilgilendirici | 1 |
| 50023 | Kaos Durduruldu | Kaos test oturumu durduruldu | Test edilebilirlik | Bilgilendirici | 1 |

**Kaos düğümü olayları**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 50033 | ChaosNodeRestartPlanlanan | Bir düğüm, Kaos test oturumunun bir parçası olarak yeniden başlatılacak şekilde zamanlandı | Test edilebilirlik | Bilgilendirici | 1 |
| 50087 | ChaosNodeRestart Tamamlandı | Bir düğüm, Kaos test oturumunun bir parçası olarak yeniden başlatmayı tamamladı | Test edilebilirlik | Bilgilendirici | 1 |

**Kaos uygulama olayları**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 50053 | ChaosCodePackageRestartScheduled | Kaos test oturumu sırasında bir kod paketi yeniden başlatma sı zamanlandı | Test edilebilirlik | Bilgilendirici | 1 |
| 50101 | ChaosCodePackageRestart Tamamlandı | Kaos test oturumu sırasında bir kod paketi yeniden başlatma tamamlandı | Test edilebilirlik | Bilgilendirici | 1 |

**Kaos bölüm olayları**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 50069 | ChaosPartitionPrimaryMoveScheduled | Birincil bölüm, Kaos test oturumunun bir parçası olarak hareket etmek üzere zamanlandı | Test edilebilirlik | Bilgilendirici | 1 |
| 50077 | ChaosPartitionSecondaryMoveScheduled | İkincil bir bölüm, Kaos test oturumunun bir parçası olarak hareket etmeyi planladı | Test edilebilirlik | Bilgilendirici | 1 |
| 65003 | BölümlemeBirincilHareket Analizi | Birincil bölüm hareketinin daha derin analizi kullanılabilir | Test edilebilirlik | Bilgilendirici | 1 |

**Kaos çoğaltma olayları**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 50047 | ChaosReplicaRestartScheduled | Bir yineleme yeniden başlatma kaos test oturumunun bir parçası olarak zamanlandı | Test edilebilirlik | Bilgilendirici | 1 |
| 50051 | ChaosReplicaRemovalScheduled | Bir yineleme kaldırma Kaos test oturumunun bir parçası olarak zamanlandı | Test edilebilirlik | Bilgilendirici | 1 |
| 50093 | ChaosReplicaRemoval Tamamlandı | Bir yineleme kaldırma Kaos test oturumunun bir parçası olarak tamamlandı | Test edilebilirlik | Bilgilendirici | 1 |

## <a name="other-events"></a>Diğer etkinlikler

**Korelasyon olayları**

| Eventıd | Adı | Açıklama |Kaynak (Görev) | Düzey | Sürüm |
| --- | --- | ---| --- | --- | --- |
| 65011 | KorelasyonOperasyonel | Bir korelasyon algılandı | Test edilebilirlik | Bilgilendirici | 1 |

## <a name="events-prior-to-version-62"></a>Sürüm 6.2'den önceki olaylar

Aşağıda, Service Fabric tarafından sürüm 6.2'den önce sağlanan olayların kapsamlı bir listesi verilmiştir.

| Eventıd | Adı | Kaynak (Görev) | Düzey |
| --- | --- | --- | --- |
| 25620 | Düğüm Açma | FabricNode | Bilgilendirici |
| 25621 | DüğümOpenedSuccess | FabricNode | Bilgilendirici |
| 25622 | DüğümOpenedFailed | FabricNode | Bilgilendirici |
| 25623 | DüğümKapanış | FabricNode | Bilgilendirici |
| 25624 | DüğümKapalı | FabricNode | Bilgilendirici |
| 25625 | DüğümBorting | FabricNode | Bilgilendirici |
| 25626 | Düğümİptal edildi | FabricNode | Bilgilendirici |
| 29627 | ClusterUpgradeStart | CM | Bilgilendirici |
| 29628 | ClusterUpgradeComplete | CM | Bilgilendirici |
| 29629 | ClusterUpgradeRollback | CM | Bilgilendirici |
| 29630 | ClusterUpgradeRollbackKomple | CM | Bilgilendirici |
| 29631 | ClusterUpgradeDomainComplete | CM | Bilgilendirici |
| 23074 | Konteyner Etkin | Barındırma | Bilgilendirici |
| 23075 | Konteyner Devre Dışı | Barındırma | Bilgilendirici |
| 29620 | Oluşturulan Uygulamalar | CM | Bilgilendirici |
| 29621 | UygulamaYükseltmeBaşlat | CM | Bilgilendirici |
| 29622 | UygulamaYükseltmeTamamlandı | CM | Bilgilendirici |
| 29623 | UygulamaUpgradeRollback | CM | Bilgilendirici |
| 29624 | UygulamaYükseltmeRollbackKomple | CM | Bilgilendirici |
| 29625 | Uygulamalar Silindi | CM | Bilgilendirici |
| 29626 | UygulamaYükseltmeDomainComplete | CM | Bilgilendirici |
| 18566 | Hizmet Oluşturuldu | FM | Bilgilendirici |
| 18567 | Hizmet Silindi | FM | Bilgilendirici |

## <a name="next-steps"></a>Sonraki adımlar

* [Service Fabric'te tanılama](service-fabric-diagnostics-overview.md) hakkında genel bilgi alın
* [Service Fabric Eventstore'da](service-fabric-diagnostics-eventstore.md) EventStore hakkında daha fazla bilgi edinin
* Daha fazla günlük toplamak için [Azure Tanılama](service-fabric-diagnostics-event-aggregation-wad.md) yapılandırmanızı değiştirme
* Operasyonel kanal günlüklerinizi görmek için [Uygulama Öngörüleri ayarlama](service-fabric-diagnostics-event-analysis-appinsights.md)
