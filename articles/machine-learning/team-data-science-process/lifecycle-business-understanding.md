---
title: Ekip Veri Bilimi Sürecinde İş Anlayışı
description: Ekip Veri Bilimi Süreci'ndeki veri bilimi projelerinizin iş anlama aşaması için hedefler, görevler ve teslim edilebilir ler.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a7aaed519f8f97a9be77a263568aeed5257c16d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76710340"
---
# <a name="the-business-understanding-stage-of-the-team-data-science-process-lifecycle"></a>Ekip Veri Bilimi Süreci yaşam döngüsünün iş anlama aşaması

Bu makalede, Ekip Veri Bilimi Süreci'nin (TDSP) iş anlama aşamasıyla ilişkili hedefler, görevler ve teslim edilebilirler sıralanmıştır. Bu işlem, veri bilimi projelerinizi yapılandırmak için kullanabileceğiniz önerilen bir yaşam döngüsü sağlar. Yaşam döngüsü, projelerin genellikle yineleyici olarak yürüttüğü başlıca aşamaları özetler:

   1. **Kurumsal yaklaşım**
   2. **Veri edinme ve anlama**
   3. **Modelleme**
   4. **Dağıtım**
   5. **Müşteri kabulü**

TDSP yaşam döngüsünün görsel bir gösterimi aşağıdavelvettir: 

![TDSP yaşam döngüsü](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefler
* Model hedefleri olarak hizmet verecek ve ilgili ölçümleri kullanılan anahtar değişkenleri belirtin projenin başarısını belirleyin.
* İşletmenin erişebilen veya elde etmesi gereken ilgili veri kaynaklarını tanımlayın.

## <a name="how-to-do-it"></a>Nasıl yapılacağını
Bu aşamada ele verilen iki ana görev vardır: 

   * **Hedefleri tanımlayın**: İş sorunlarını anlamak ve tanımlamak için müşterinizle ve diğer paydaşlarla birlikte çalışın. Veri bilimi tekniklerinin hedeflenebildiği iş hedeflerini tanımlayan soruları formüle edin.
   * **Veri kaynaklarını belirleyin**: Projenin hedeflerini tanımlayan soruları yanıtlamanıza yardımcı olacak ilgili verileri bulun.

### <a name="define-objectives"></a>Hedefleri tanımlama
1. Bu adımın temel amacı, analizin tahmin etmesi gereken temel iş değişkenlerini belirlemektir. Bu değişkenleri *model hedefleri*olarak adlandırıyoruz ve projenin başarısını belirlemek için onlarla ilişkili ölçümleri kullanıyoruz. Bu hedeflere iki örnek satış tahminleri veya bir siparişin sahte olma olasılığıdır.

2. İlgili, belirli ve açık "keskin" sorular sorarak ve rafine ederek proje hedeflerini tanımlayın. Veri bilimi, bu tür soruları yanıtlamak için adlar ve sayılar kullanan bir süreçtir. Genellikle beş tür soruyu yanıtlamak için veri bilimini veya makine öğrenimini kullanırsınız:
 
   * Ne kadar ya da kaç tane? (gerileme)
   * Hangi kategoride? (sınıflandırma)
   * Hangi grup? (kümeleme)
   * Bu garip mi? (anomali tespiti)
   * Hangi seçenek seçilmelidir? (tavsiye)

   Bu sorulardan hangisini sorduğunuza ve yanıtlamanın iş hedeflerinize nasıl ulaşabildiğini belirleyin.

3. Üyelerinin rollerini ve sorumluluklarını belirterek proje ekibini tanımlayın. Daha fazla bilgi keşfettikçe üzerinde titrediğiniz üst düzey bir kilometre taşı planı geliştirin. 

4. Başarı ölçümlerini tanımlayın. Örneğin, bir müşteri karmaşası tahmini elde etmek isteyebilirsiniz. Bu üç aylık projenin sonuna kadar "x" oranına ihtiyacınız vardır. Bu verilerle, çalkalanmayı azaltmak için müşteri promosyonları sunabilirsiniz. Ölçümler **SMART**olmalıdır: 

   * **S**pecific 
   * **M**ölçülebilir
   * **Bir**chievable 
   * **R**elevant 
   * **T**ime bağlı 

### <a name="identify-data-sources"></a>Veri kaynaklarını tanımlama
Keskin sorularınıza bilinen yanıt örneklerini içeren veri kaynaklarını tanımlayın. Aşağıdaki verileri arayın:

* Soruyla alakalı veriler. Hedefin ölçüleri ve hedefle ilgili özellikleriniz var mı?
* Model hedefinizin doğru bir ölçüsü olan veriler ve ilgi çekici özellikler.

Örneğin, varolan sistemlerin sorunu gidermek ve proje hedeflerine ulaşmak için ek veri türlerini toplaması ve günlüğe kaydetmesi gerektiğini görebilirsiniz. Bu durumda, harici veri kaynaklarını aramak veya yeni veri toplamak için sistemlerinizi güncelleştirmek isteyebilirsiniz.

## <a name="artifacts"></a>Yapıtlar
İşte bu aşamada teslim edilir:

   * [Kiralama belgesi](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): TDSP proje yapısı tanımında standart bir şablon sağlanır. Tüzük belgesi yaşayan bir belgedir. Yeni keşifler yaptığınızda ve iş gereksinimleri değiştikçe şablonu proje boyunca güncellersiniz. Önemli olan, bulma işlemi boyunca ilerledikçe daha fazla ayrıntı ekleyerek bu belgeyi yeniden birelemektir. Müşteriyi ve diğer paydaşları değişikliklerin yapımında görev almak ta ve değişikliklerin nedenlerini onlara açıkça iletin.  
   * [Veri kaynakları](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Data_Report/Data%20Defintion.md#raw-data-sources): TDSP proje **Veri raporu** klasöründe bulunan **Veri tanımları** raporunun **Ham veri kaynakları** bölümü veri kaynaklarını içerir. Bu bölümde ham verilerin orijinal ve hedef konumları belirtilir. Daha sonraki aşamalarda, verileri analitik ortamınıza taşımak için komut dosyaları gibi ek ayrıntılar doldurursunuz.  
   * [Veri sözlükleri](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Data_Dictionaries): Bu belge, istemci tarafından sağlanan verilerin açıklamalarını sağlar. Bu açıklamalar şema (veri türleri ve varsa doğrulama kuralları hakkındaki bilgiler) ve varsa varlık ilişkisi diyagramları hakkında bilgi içerir.

## <a name="next-steps"></a>Sonraki adımlar

TDSP'nin yaşam döngüsündeki her adıma bağlantılar aşağıda vereb

   1. [Kurumsal yaklaşım](lifecycle-business-understanding.md)
   2. [Veri edinme ve anlama](lifecycle-data.md)
   3. [Modelleme](lifecycle-modeling.md)
   4. [Dağıtım](lifecycle-deployment.md)
   5. [Müşteri kabulü](lifecycle-acceptance.md)

Belirli senaryolar için işlemdeki tüm adımları gösteren tam izlenme ler sağlarız. [Örnek walkthroughs](walkthroughs.md) makalesi, bağlantılar ve küçük resim açıklamaları içeren senaryoların bir listesini sağlar. İzler, bulut, şirket içi araçlar ve hizmetlerin akıllı bir uygulama oluşturmak için iş akışı veya ardışık ardışık yollarla nasıl birleştirilebildiğini gösterir. 
