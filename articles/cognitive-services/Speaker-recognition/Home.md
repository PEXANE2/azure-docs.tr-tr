---
title: Konuşmacı Tanıma API’si nedir?
titleSuffix: Azure Cognitive Services
description: Bilişsel Hizmetlerde Hoparlör Tanıma API'si ile hoparlör doğrulaması ve hoparlör tanımlaması.
services: cognitive-services
author: dwlin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speaker-recognition
ms.topic: overview
ms.date: 10/01/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: f354793e3779f4d9f7be9bae8a21545a15ed1c4c
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73464907"
---
# <a name="speaker-recognition-api---preview"></a>Hoparlör Tanıma API - Önizleme

Hoparlör Tanıma API'leri, hoparlör doğrulaması ve hoparlör tanımlaması için gelişmiş Bilgi alalgoritmaları sağlayan bulut tabanlı APIlerdir. Hoparlör Tanıma iki kategoriye ayrılır: hoparlör doğrulama ve hoparlör tanımlama.

## <a name="speaker-verification"></a>Konuşmacı Doğrulama

Ses, bir birey ile ilişkilendirilebilir benzersiz özelliklere sahiptir.  Uygulamalar, çağrı merkezleri ve web hizmetleri gibi senaryolarda, doğrulama için ek bir faktör olarak ses kullanabilirsiniz.

Hoparlör Doğrulama API'leri, kullanıcıların hem ses hem de konuşma parolalarını kullanarak doğrulamaya yardımcı olmak için akıllı bir araç olarak hizmet verir.

### <a name="enrollment"></a>Kayıt

Konuşmacı doğrulaması için kayıt metin ekidir, bu da konuşmacıların hem kayıt hem de doğrulama aşamalarında kullanmak üzere belirli bir parola seçmesi gerektiği anlamına gelir.

Konuşmacı kayıt aşamasında, konuşmacının sesi belirli bir ifade söyleyerek kaydedilir. Seçilen ifade tanınırken ses özellikleri benzersiz bir ses imzası oluşturmak için ayıklanır. Bu konuşmacı kayıt verileri birlikte konuşmacıyı doğrulamak için kullanılır. Hoparlör kayıt verileri güvenli bir sistemde depolanır. Müşteri ne kadar süreyle saklanmalıdır denetler. Müşteriler API çağrıları aracılığıyla tek tek konuşmacılar için kayıt verilerini oluşturabilir, güncelleyebilir ve kaldırabilir.  Abonelik silindiğinde, abonelikle ilişkili tüm konuşmacı kayıt verileri de silinir.

Müşteriler, hoparlör doğrulaması için kullanıcılardan uygun izinleri aldıklarından emin olmalıdır.

### <a name="verification"></a>Doğrulama

Doğrulama aşamasında, Müşteri doğrulanması için birkişiyle ilişkili kimlikle konuşmacı doğrulama API'sini aramalıdır.  Hizmet, giriş konuşma kaydından ses özelliklerini ve parola yı ayıklar. Daha sonra, müşterinin doğrulamak istediği hoparlör kayıt verilerinin ilgili öğeleriyle özellikleri karşılaştırır ve herhangi bir eşleşmeyi belirler.  Yanıt farklı güven düzeyleri ile "kabul" veya "reddetmek" döndürür.  Müşteri daha sonra, bu kişinin kayıtlı konuşmacı olup olmadığına karar vermenize yardımcı olmak için sonuçların nasıl kullanılacağını belirler.

Eşik güven düzeyi, kullanılan senaryo ve diğer doğrulama etkenlerine göre ayarlanmalıdır. Güven düzeyini denemenizi ve her uygulama için uygun ayarı düşünmenizi öneririz. API'ler, sesin canlı bir kişiden mi yoksa bir taklitten mi yoksa kayıtlı bir konuşmacının kaydından mı ait olduğunu belirlemek için tasarlanmamıştır.

Hizmet, doğrulama aşamasında hizmete gönderilen konuşma kaydını veya ayıklanan ses özelliklerini saklamaz.

Konuşmacı doğrulama hakkında daha fazla ayrıntı için lütfen [Konuşmacı - Doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652) API’sine bakın.

## <a name="speaker-identification"></a>Konuşmacı Belirleme

Uygulamalar, kayıtlı konuşmacılar grubu verilen "kim konuşuyor" tanımlamak için ses kullanabilirsiniz. Hoparlör Tanımlama API'leri, toplantı üretkenliği, kişiselleştirme ve çağrı merkezi transkripsiyongibi senaryolarda kullanılabilir.

### <a name="enrollment"></a>Kayıt

Konuşmacı belirleme için kayıt metinden bağımsızdır; başka bir deyişle, konuşmacının ses kaydında söyleyecekleri üzerinde bir kısıtlama yoktur. Parola gerekmez.

Kayıt aşamasında, hoparlörün sesi kaydedilir ve ses özellikleri benzersiz bir ses imzası oluşturmak için ayıklanır. Çıkarılan konuşma sesi ve özellikleri güvenli bir sistemde saklanır. Müşteri ne kadar süreyle saklanır denetler. Müşteriler API çağrıları aracılığıyla tek tek konuşmacılar için bu hoparlör kayıt verilerini oluşturabilir, güncelleyebilir ve kaldırabilir. Abonelik silindiğinde, abonelikle ilişkili tüm konuşmacı kayıt verileri de silinir.

Müşteriler, hoparlör tanımlaması için kullanıcılardan uygun izinleri aldıklarından emin olmalıdır.

### <a name="identification"></a>Kimlik

Tanımlama aşamasında, hoparlör tanımlama hizmeti giriş konuşma kaydından ses özelliklerini ayıklar. Daha sonra, belirtilen konuşmacı listesinin kayıt verileriyle özellikleri karşılaştırır. Bir eşleşme kayıtlı bir hoparlörle bulunduğunda, yanıt hoparlörün kimliğini güven düzeyiyle döndürür.  Aksi takdirde, hiçbir konuşmacı kayıtlı bir konuşmacıyla eşleşmediğinde yanıt "reddet" döndürür.

Eşik güven düzeyi senaryoya göre ayarlanmalıdır. Güven düzeyini denemenizi ve her uygulama için uygun ayarı düşünmenizi öneririz. API'ler, sesin canlı bir kişiden mi yoksa bir taklitten mi yoksa kayıtlı bir konuşmacının kaydından mı ait olduğunu belirlemek için tasarlanmamıştır.

Hizmet, tanımlama aşaması için hizmete gönderilen konuşma kaydını veya ayıklanan ses özelliklerini saklamaz.

Hoparlör tanımlaması hakkında daha fazla bilgi için lütfen API Hoparlörüne bakın [- Kimlik.](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e)
