---
title: Özel komutlar kavramları ve tanımları-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlar uygulamaları için kavramlar ve tanımlar hakkında bilgi edineceksiniz.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 5a5e3590e0e184633760f080dfd3402ed75bd48e
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307894"
---
# <a name="custom-commands-concepts-and-definitions"></a>Özel komutlar kavramları ve tanımları

Bu makale, özel komutlar uygulamalarına yönelik kavramlar ve tanımlar için bir başvuru görevi görür.

## <a name="commands-configuration"></a>Komutlar yapılandırması
Komutlar, özel komutlar uygulamasının temel yapı taşlarıdır. Bir komut, bir kullanıcı tarafından tanımlanan belirli bir görevi tamamlaması için gereken yapılandırmaların bir kümesidir.

### <a name="example-sentences"></a>Örnek cümleler
Örnek söyleme, kullanıcının belirli bir komutu tetikleyebilmesi için söyledikleri küme örnekleridir. Ayrıntılı bir liste yerine yalnızca bir örnek belirtmeniz gerektiğini unutmayın. 

### <a name="parameters"></a>Parametreler
Parametreler, bir görevi tamamlamaya yönelik komutlar için gereken bilgiler. Karmaşık senaryolarda parametreler, özel eylemleri tetikleyen koşulları tanımlamak için de kullanılabilir.

### <a name="completion-rules"></a>Tamamlama kuralları
Komut karşılanmaya hazırlandıktan sonra yürütülecek kurallar dizisi, yani kuralların tüm koşulları karşılandıktan sonra.

### <a name="interaction-rules"></a>Etkileşim kuralları
Daha belirgin veya karmaşık durumları işlemek için ek kurallar. Ek doğrulamalar ekleyebilir veya onaylar veya tek adımlı düzeltme gibi gelişmiş özellikleri yapılandırabilirsiniz. Kendi özel etkileşim kurallarınızı da oluşturabilirsiniz.

## <a name="parameters-configuration"></a>Parametre yapılandırması

Parametreler, bir görevi tamamlaması için komutlar tarafından gereken bilgileri sağlar. Karmaşık senaryolarda parametreler, özel eylemleri tetikleyen koşulları tanımlamak için de kullanılabilir.

### <a name="name"></a>Name
Bir parametre Name özelliği tarafından tanımlanır. Parametreye her zaman açıklayıcı bir ad vermeniz gerekir. Bir parametreye, koşullar, konuşma yanıtları veya başka eylemler oluştururken olduğu gibi farklı bölümler arasında başvurulabilir.
 
### <a name="isglobal"></a>IsGlobal
Bu parametrenin kapsamının uygulamadaki tüm komutlarda paylaşılıp paylaşılmadığını belirten onay kutusu. Bir parametre geneldir ise, değeri herhangi bir komut kapsamından ve bir değer atandıktan sonra belirtilebilir. Bu, komutlardan herhangi biri tarafından başvurulabilir. 

### <a name="required"></a>Gerekli
Bu parametre için bir değerin, komut yerine getirme/tamamlama için gerekli olup olmadığını belirten onay kutusu. Bir parametre gerekli olarak işaretlenmişse kullanıcıdan değer sağlamasını istemek için yanıtları yapılandırmanız gerekir.

### <a name="type"></a>Tür
Özel komutlar aşağıdaki parametre türlerini destekler-


* DateTime
* Coğrafya
* Sayı
* Dize

Tüm bu parametre türleri, portaldan yapılandırabileceğiniz varsayılan değer yapılandırmalarını destekler.

### <a name="configuration"></a>Yapılandırma
Yapılandırma yalnızca Type: String için tanımlanan bir parametre özelliğidir. Desteklenen değerler aşağıda verilmiştir
* Yok
* Tam girişi kabul et: Bu seçeneği etkinleştirmek, parametresi herhangi bir girişi kabul eder, bu, kullanıcının tam utterlik içeren bir parametre ihtiyacı olduğunda yararlıdır. Örneğin, posta adresleri.
* Dış katalogdan önceden tanımlanmış giriş değerlerini kabul et: çok çeşitli değerleri varsayabilir parametreyi yapılandırmak için kullanılır. örn. satış kataloğu. Bu durumda, Katalog bir dış Web uç noktasında barındırılır ve bağımsız olarak yapılandırılabilir.
* İç katalogdan önceden tanımlanmış giriş değerlerini kabul et: birkaç değeri varsayabilir parametreyi yapılandırmak için kullanılır. Bu durumda, bu değerlerin konuşma Studio 'da yapılandırılması gerekir.


### <a name="validation"></a>Doğrulama
Doğrulamalar, parametre değerindeki kısıtlamaları yapılandırmanıza olanak sağlayan belirli parametre türleri için geçerli olan yapılardır. Şu anda, özel komutlar aşağıdaki parametre türlerinde doğrulamaları destekler:
* DateTime
* Sayı

## <a name="rules-configuration"></a>Kural yapılandırması
Özel komutlarda bir kural, karşılandığında, bir dizi **eylemi**yürütecek olan bir dizi **koşula**göre tanımlanır. Kural ayrıca bir sonraki sefer için **gönderi yürütme durumunu** ve **beklentilerini** yapılandırmanızı sağlar.

### <a name="types"></a>Türler
Özel komutlar aşağıdaki kural kategorilerini destekler:
* Tamamlama kuralları

    Komut yerine getirme sırasında yürütülecek kuralların listesi. Bu bölümde, koşulların doğru olduğu tüm kurallar yürütülür.
    
* Etkileşim kuralları

    Etkileşim kuralları ek özel doğrulamaları, onayları, tek adımlı düzeltmeleri veya diğer özel iletişim kutusu mantığını sağlamak için kullanılabilir. Bu kurallar her bir işlem sırayla değerlendirilir ve tamamlanma kurallarını tetiklemek için kullanılabilir.

Bir kuralın parçası olarak yapılandırılan farklı eylemler, yazma portalında göründükleri sırada yürütülür.
    
### <a name="conditions"></a>Koşullar
Bir kuralın yürütülmesi için karşılanması gereken koşulların kümesi. Kural koşulu aşağıdaki türlerde olabilir:
* Parametre değeri eşittir: yapılandırılan parametrenin değeri belirli bir değere eşit.
* Parametre değeri yok: yapılandırılmış parametrelerde hiçbir değer bulunmamalıdır.
* Gerekli parametreler: yapılandırılmış parametre bir değere sahip.
* Tüm gerekli parametreler: gerekli olarak işaretlenen tüm parametrelerin bir değeri vardır.
* Güncelleştirilmiş Parametreler: bir veya daha fazla parametre değeri, geçerli girişin (utterance/Activity) işlenmesinin sonucu olarak güncelleştirildi.
* Onay başarılı oldu: giriş utterance/Activity başarılı bir onaydı (Evet).
* Onay reddedildi: giriş utterance/Activity başarılı bir onaydı (Hayır).
* Önceki komutun güncelleştirilmesi gerekiyor: Bu koşul, bir güncelleştirmeyle birlikte bir zaman onayını yakalamak istediğinizde örneklerde kullanılır. Arka planda bu, iletişim kutusu altyapısının, amaç önceki sırayla aynı olduğu ve kullanıcının bir güncelleştirmeyle yanıt verdiği negatif bir onay algıladığında yapılandırılır.

### <a name="actions"></a>Eylemler
* Konuşma yanıtı gönder: istemciye geri konuşma yanıtı gönderin.
* Parametre değerini güncelleştir: bir komut parametresinin değerini belirtilen bir değere güncelleştirin.
* Parametre değerini temizle komut parametresi değerinin işaretini kaldırın.
* Çağrı Web uç noktası: bir Web uç noktasına çağrı yapın.
* Etkinliği istemciye gönder: istemciye özel etkinlik gönderme.

### <a name="expectations"></a>Beklentileri
Beklentiler, sonraki Kullanıcı girişinin işlenmesine yönelik ipuçları yapılandırmak için kullanılır. Aşağıdaki türler desteklenir:
* Kullanıcıdan onay bekleniyor: uygulamanın bir sonraki Kullanıcı girişi için bir onay (Evet/Hayır) beklediği belirtilir.
* Kullanıcıdan parametre girdisi bekleniyor: uygulamanın Kullanıcı girişinden beklediği bir veya daha fazla komut parametresi belirtin.

### <a name="post-execution-state"></a>Yürütme sonrası durumu
Geçerli giriş işlendikten sonra iletişim kutusu durumu (utterance/Activity). Şu türlerden biri:
* Komut tamamlandı: komutu doldurun ve komutun başka hiçbir kuralı işlenmeyecek.
* Tamamlanma kurallarını Yürüt: geçerli tamamlanma kurallarını yürütün.
* Kullanıcının girişini bekle: sonraki Kullanıcı girişini bekle.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da örneklere bakın](https://aka.ms/speech/cc-samples)
