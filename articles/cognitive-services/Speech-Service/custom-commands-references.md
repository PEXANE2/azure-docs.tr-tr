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
ms.openlocfilehash: 83725a3839d36fc753bb43803e67acaca7571a6e
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85851834"
---
# <a name="custom-commands-concepts-and-definitions"></a>Özel komutlar kavramları ve tanımları

Bu makale, özel komutlar uygulamalarına yönelik kavramlar ve tanımlar için bir başvuru görevi görür.

## <a name="commands-configuration"></a>Komutlar yapılandırması
Komutlar, özel komutlar uygulamasının temel yapı taşlarıdır. Bir komut, bir kullanıcı tarafından tanımlanan belirli bir görevi tamamlaması için gereken bir yapılandırma kümesidir.

### <a name="example-sentences"></a>Örnek cümleler
Örnek söyleme, kullanıcının belirli bir komutu tetikleyebilmesi için söyledikleri küme örnekleridir. Ayrıntılı bir liste yerine yalnızca bir örnek belirtmeniz gerekir. 

### <a name="parameters"></a>Parametreler
Parametreler, bir görevi tamamlamaya yönelik komutlar için gereken bilgiler. Karmaşık senaryolarda parametreler, özel eylemleri tetikleyen koşulları tanımlamak için de kullanılabilir.

### <a name="completion-rules"></a>Tamamlama kuralları
Tamamlama kuralları, komut karşılanmaya hazırsa, örneğin kuralların tüm koşulları karşılandıktan sonra yürütülecek bir kural dizisidir.

### <a name="interaction-rules"></a>Etkileşim kuralları
Etkileşim kuralları, daha belirgin veya karmaşık durumları işlemek için ek kurallardır. Ek doğrulamalar ekleyebilir veya onaylar veya tek adımlı bir düzeltme gibi gelişmiş özellikleri yapılandırabilirsiniz. Kendi özel etkileşim kurallarınızı da oluşturabilirsiniz.

## <a name="parameters-configuration"></a>Parametre yapılandırması

Parametreler, bir görevi tamamlaması için komutlar tarafından gereken bilgileri sağlar. Karmaşık senaryolarda parametreler, özel eylemleri tetikleyen koşulları tanımlamak için de kullanılabilir.

### <a name="name"></a>Name
Bir parametre Name özelliği tarafından tanımlanır. Parametreye her zaman açıklayıcı bir ad vermeniz gerekir. Bir parametre farklı bölümler arasında, örneğin koşullar, konuşma yanıtları veya başka eylemler oluştururken belirtilebilir.
 
### <a name="isglobal"></a>IsGlobal
Bu onay kutusu, bu parametrenin kapsamının uygulamadaki tüm komutlarda paylaşılıp paylaşılmadığını gösterir. Bir parametre geneldir ise, değeri herhangi bir komut kapsamından etkinleştirilebilir. Bir değer atandıktan sonra, komutlardan herhangi biri olabilir. 

### <a name="required"></a>Gerekli
Bu onay kutusu, komut yerine getirme veya tamamlama için bu parametre için bir değer gerekip gerekmediğini gösterir. Bir parametre gerekli olarak işaretlenmişse kullanıcıdan bir değer sağlamasını istemek için yanıtları yapılandırmanız gerekir.

### <a name="type"></a>Tür
Özel komutlar aşağıdaki parametre türlerini destekler:

* DateTime
* Coğrafya
* Sayı
* Dize

Tüm bu parametre türleri, Azure portal yapılandırabileceğiniz varsayılan değer yapılandırmalarını destekler.

### <a name="configuration"></a>Yapılandırma
Yapılandırma yalnızca tür dizesi için tanımlanan bir parametre özelliğidir. Aşağıdaki değerler desteklenir:

* **Yok**.
* **Tam girişi kabul et**: etkinleştirildiğinde, bir parametre herhangi bir giriş açıklamasını kabul eder. Bu seçenek, kullanıcının tam utterlik bir parametreye ihtiyacı olduğunda faydalıdır. Posta adresleri bir örnektir.
* **Dış katalogdan önceden tanımlanmış giriş değerlerini kabul et**: Bu değer, çok çeşitli değerleri varsayan bir parametre yapılandırmak için kullanılır. Bir satış kataloğu örneğidir. Bu durumda, Katalog bir dış Web uç noktasında barındırılır ve bağımsız olarak yapılandırılabilir.
* **İç katalogdan önceden tanımlanmış giriş değerlerini kabul et**: Bu değer, birkaç değeri varsayacak bir parametreyi yapılandırmak için kullanılır. Bu durumda, konuşma Studio 'da değerlerin yapılandırılması gerekir.


### <a name="validation"></a>Doğrulama
Doğrulamalar, bir parametre değerindeki kısıtlamaları yapılandırmanıza olanak sağlayan belirli parametre türleri için geçerli olan yapılardır. Şu anda özel komutlar aşağıdaki parametre türlerinde doğrulamaları destekler:

* DateTime
* Sayı

## <a name="rules-configuration"></a>Kural yapılandırması
Özel komutlarda bir kural, karşılandığında, bir dizi *eylemi*yürütülene yönelik bir dizi *koşula* göre tanımlanır. Kurallar, bir sonraki sırayla *yürütme sonrası durumunu* ve *beklentilerini* yapılandırmanıza da imkan tanır.

### <a name="types"></a>Türler
Özel komutlar aşağıdaki kural kategorilerini destekler:

* **Tamamlama kuralları**: Bu kuralların, komut ile tamamlama sırasında yürütülmesi gerekir. Bu bölümde, koşulların doğru olduğu tüm kurallar yürütülür. 
* **Etkileşim kuralları**: Bu kurallar ek özel doğrulamaları, onayları ve tek adımlı bir düzeltmeyi yapılandırmak ya da başka bir özel iletişim kutusu mantığını gerçekleştirmek için kullanılabilir. Etkileşim kuralları her bir işleme sırayla değerlendirilir ve tamamlanma kurallarını tetiklemek için kullanılabilir.

Bir kuralın parçası olarak yapılandırılan farklı eylemler, yazma portalında göründükleri sırada yürütülür.

### <a name="conditions"></a>Koşullar
Koşullar, bir kuralın yürütülmesi için karşılanması gereken gereksinimlerdir. Kural koşulları aşağıdaki türlerde olabilir:

* **Parametre değeri eşittir**: yapılandırılan parametrenin değeri belirli bir değere eşit.
* **Parametre değeri yok**: yapılandırılan parametrelerin herhangi bir değer olmaması gerekir.
* **Gerekli parametreler**: yapılandırılmış parametrenin bir değeri vardır.
* **Tüm gerekli parametreler**: gerekli olarak işaretlenen tüm parametrelerin bir değeri vardır.
* **Güncelleştirilmiş parametreler**: bir veya daha fazla parametre değeri, geçerli girişi (utterance veya Activity) işlemenin sonucu olarak güncelleştirildi.
* **Onay başarılı**oldu: giriş ya da etkinlik başarılı bir onaydı (Evet).
* **Onay reddedildi**: giriş ya da etkinlik başarılı bir onay (Hayır) değildi.
* **Önceki komutun güncelleştirilmesi gerekiyor**: Bu koşul, bir güncelleştirmeyle birlikte bir zaman onayını yakalamak istediğinizde örneklerde kullanılır. Arka planda bu koşul, iletişim kutusu altyapısının, amaç önceki sırayla aynı olduğu ve Kullanıcı bir güncelleştirmeyle yanıt verdiği negatif bir onay algıladığında yapılandırılır.

### <a name="actions"></a>Eylemler
* **Konuşma yanıtı gönder**: istemciye geri konuşma yanıtı gönderin.
* **Parametre değerini Güncelleştir**: bir komut parametresinin değerini belirtilen bir değere güncelleştirin.
* **Parametre değerini temizle**: komut parametresi değerini temizleyin.
* **Web uç noktası çağır**: bir Web uç noktasına çağrı yapın.
* **Etkinliği Istemciye gönder**: istemciye özel etkinlik gönderme.

### <a name="expectations"></a>Beklentileri
Beklentiler, sonraki Kullanıcı girişinin işlenmesine yönelik ipuçları yapılandırmak için kullanılır. Aşağıdaki türler desteklenir:

* **Kullanıcıdan onay bekleniyor**: Bu beklenerek, uygulamanın bir sonraki Kullanıcı girişi için bir onay (Evet/Hayır) beklediği belirtilir.
* **Kullanıcıdan parametre girdisi bekleniyor**: Bu beklenerek uygulamanın Kullanıcı girişinden beklediği bir veya daha fazla komut parametresi belirtilir.

### <a name="post-execution-state"></a>Yürütme sonrası durumu
Yürütme sonrası durumu, geçerli giriş (utterance veya Activity) işlendikten sonra gelen iletişim durumudur. Şu türlerden biri:

* **Komut tamamlandı**: komutu doldurun ve komutun başka hiçbir kuralı işlenmeyecek.
* **Tamamlanma kurallarını Yürüt**: geçerli tamamlanma kurallarını yürütün.
* **Kullanıcının girişini bekle**: sonraki Kullanıcı girişini bekle.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da örneklere bakın](https://aka.ms/speech/cc-samples)
