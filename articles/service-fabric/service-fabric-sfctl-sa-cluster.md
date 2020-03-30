---
title: Azure Servis Kumaş CLI- sfctl sa-cluster
description: Azure Service Fabric komut satırı arabirimi sfctl hakkında bilgi edinin. Bağımsız kümeleri yönetmek için komutların listesini içerir.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 233148c04fb1a9c6cf1d6c7042c12c54eebd0205
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76904931"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Tek başına Service Fabric kümelerini yönetin.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| config | Service Fabric bağımsız küme yapılandırmasını alın. |
| config yükseltme | Service Fabric bağımsız kümesinin yapılandırmasını yükseltmeye başlayın. |
| yükseltme durumu | Hizmet Kumaşı bağımsız kümesinin küme yapılandırma yükseltme durumunu alın. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-küme config
Service Fabric bağımsız küme yapılandırmasını alın.

Küme yapılandırması küme, güvenlik yapılandırmaları, hata ve yükseltme etki alanı topolojileri, vb farklı düğüm türleri içeren küme özellikleri içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --configuration-api-version [Gerekli] | Bağımsız küme json yapılandırmasının API sürümü. |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa-küme config-upgrade
Service Fabric bağımsız kümesinin yapılandırmasını yükseltmeye başlayın.

Sağlanan yapılandırma yükseltme parametrelerini doğrulayın ve parametreler geçerliyse küme yapılandırmasını yükseltmeye başlayın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --küme-config [Gerekli] | Küme yapılandırması. |
| --uygulama-sağlık politikaları | JSON, hata yükseltmeden önce uygulama türü adı ve maksimum yüzde sağlıksız çiftleri sözlüğü kodlanır. |
| --delta-sağlıksız düğümler | Yükseltme sırasında izin verilen maksimum delta sistem durumu bozulması yüzdesi. İzin verilen değerler sıfırdan 100'e kadar olan tümseci değerleridir. |
| --sağlık-kontrol-yeniden deneme | Uygulama veya küme sağlıklı değilse, sistem durumu denetimleri gerçekleştirme girişimleri arasındaki süre.  Varsayılan\: PT0H0M0S. |
| --sağlık-kontrol-kararlı | Yükseltme bir sonraki yükseltme etki alanına geçmeden önce uygulamanın veya kümenin sağlıklı kalması gereken süre.  Varsayılan\: PT0H0M0S. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --sağlık-check-bekle | Sistem durumu denetimleri işlemini başlatmadan önce bir yükseltme etki alanını tamamladıktan sonra bekleme süresi.  Varsayılan\: PT0H0M0S. |
| --zaman ayarı -t | Varsayılan\: 60. |
| --sağlıksız uygulamalar | Yükseltme sırasında izin verilen en yüksek sağlıksız uygulamaların yüzdesi. İzin verilen değerler sıfırdan 100'e kadar olan tümseci değerleridir. |
| --sağlıksız düğümler | Yükseltme sırasında izin verilen en yüksek sağlıksız düğüm yüzdesi. İzin verilen değerler sıfırdan 100'e kadar olan tümseci değerleridir. |
| --upgrade-etki alanı-delta-sağlıksız düğümler | Yükseltme sırasında alan adı sistem durumu bozulmasının izin verilen maksimum yüzdesi. İzin verilen değerler sıfırdan 100'e kadar olan tümseci değerleridir. |
| --upgrade-etki alanı-zaman ayarı | FailureAction yürütülmeden önce her yükseltme etki alanının tamamlanması gereken süre.  Varsayılan\: PT0H0M0S. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |
| --yükseltme-zaman ayarı | FailureAction yürütülmeden önce genel yükseltmenin tamamlanması gereken süre.  Varsayılan\: PT0H0M0S. <br><br> İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |

### <a name="examples"></a>Örnekler

Küme yapılandırma güncelleştirmesi başlatma
```
sfctl sa-cluster config-upgrade --cluster-config <YOUR CLUSTER CONFIG> --application-health-
policies "{"fabric:/System":{"ConsiderWarningAsError":true}}"
```

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-küme yükseltme-durum
Hizmet Kumaşı bağımsız kümesinin küme yapılandırma yükseltme durumunu alın.

Hizmet Kumaşı bağımsız kümesinin küme yapılandırma yükseltme durum ayrıntılarını alın.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --zaman ayarı -t | İşlemi saniyeler içinde gerçekleştirmek için sunucu zaman. Bu zaman sonu, istemcinin istenen işlemin tamamlanmasını beklemek istediği süreyi belirtir. Bu parametrenin varsayılan değeri 60 saniyedir.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Küresel Argümanlar

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göstermek için günlüğe kaydetme ayrıntılılığını artırın. |
| --yardım -h | Bu yardım iletisi ve çıkış göster. |
| --çıkış -o | Çıktı biçimi.  İzin\: verilen değerler json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --sorgu | JMESPath sorgu dizesi. Daha\:fazla bilgi ve örnekler için http //jmespath.org/'a bakın. |
| --ayrıntılı | Günlüğe kaydetme ayrıntılılığını artırın. Tam hata ayıklama günlükleri için --hata ayıklama kullanın. |


## <a name="next-steps"></a>Sonraki adımlar
- Servis Kumaş ı CLI'yi [ayarlayın.](service-fabric-cli.md)
- [Örnek komut dosyalarını](/azure/service-fabric/scripts/sfctl-upgrade-application)kullanarak Service Fabric CLI'yi nasıl kullanacağınızı öğrenin.