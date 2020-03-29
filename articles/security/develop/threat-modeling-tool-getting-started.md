---
title: Başlarken - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
description: Bu, Tehdit Modelleme Aracı'nı iş başında vurgulayarak daha derin bir genel bakıştır.
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.subservice: security-develop
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: jegeib
ms.openlocfilehash: 1454826095bcced9b20935405c0befd5a1ed1ddd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68728298"
---
# <a name="getting-started-with-the-threat-modeling-tool"></a>Tehdit Modelleme Aracı ile başlarken

Microsoft Threat Modeling Tool 2018, Eylül 2018'de GA adıyla ücretsiz **[tıklayarak indirin.](https://aka.ms/threatmodelingtool)** Teslim mekanizmasındaki değişiklik, aracı her açtıklarında müşterilere en son geliştirmeleri ve hata düzeltmelerini itmemize olanak sağlayarak bakımı ve kullanımını kolaylaştırır.
Bu makalede, Microsoft SDL tehdit modelleme yaklaşımı ile başlarken alma sürecinde alır ve güvenlik sürecinin bir omurgası olarak büyük tehdit modelleri geliştirmek için aracı nasıl kullanacağınızı gösterir.

Bu makalede, SDL tehdit modelleme yaklaşımının varolan bilgisi ne dayanmaktadır. Hızlı bir inceleme için, **[Tehdit Modelleme Web Uygulamaları](https://msdn.microsoft.com/library/ms978516.aspx)** ve 2006 yılında yayınlanan STRIDE Yaklaşımı MSDN **[makalesini kullanarak Güvenlik Kusurlarını Ortaya Çıkar'ın](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)** arşivlenmiş bir sürümüne bakın.

Hızlı bir şekilde özetlemek gerekirse, yaklaşım bir diyagram oluşturmayı, tehditleri tanımlamayı, azaltmayı ve her bir azaltmayı doğrulamayı içerir. Bu işlemi vurgulayan bir diyagram aşağıda verilmiştir:

![SDL Süreci](./media/threat-modeling-tool-getting-started/sdlapproach.png)

## <a name="starting-the-threat-modeling-process"></a>Tehdit modelleme işleminin başlatılması

Tehdit Modelleme Aracı'nı başlattığınızda, resimde görüldüğü gibi birkaç şey fark edeceksiniz:

![Boş Başlangıç Sayfası](./media/threat-modeling-tool-feature-overview/tmtstart.png)

### <a name="threat-model-section"></a>Tehdit modeli bölümü

| Bileşen                                   | Ayrıntılar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Geri Bildirim, Öneri ve Sorunlar Düğmesi** | SDL her şey için **[MSDN Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)** alır. Diğer kullanıcıların ne yaptığını, geçici çözüm ve önerilerle birlikte okuma fırsatı verir. Aradığınızı hala bulamıyorsanız, destek ekibimizin tmtextsupport@microsoft.com size yardımcı olması için e-posta gönder                                                                                                                            |
| **Model Oluşturma**                          | Diyagramınızı çizmeniz için boş bir tuval açar. Modeliniz için hangi şablonu kullanmak istediğinizi seçtiğinizden emin olun                                                                                                                                                                                                                                                                                                                                                                       |
| **Yeni Modeller için Şablon**                 | Bir model oluşturmadan önce hangi şablonu kullanacağınızı seçmeniz gerekir. Ana şablonumuz, Azure'a özgü şablonlar, tehditler ve azaltıcı etkenler içeren Azure Tehdit Modeli Şablonu'dur. Genel modeller için açılır menüden SDL TM Bilgi Tabanı'nı seçin. Kendi şablonunuzu oluşturmak veya tüm kullanıcılar için yeni bir şablon göndermek mi istiyorsunuz? Daha fazla bilgi edinmek için **[Şablon Deposu](https://github.com/Microsoft/threat-modeling-templates)** GitHub Sayfamıza göz atın                              |
| **Model Aç**                            | <p>Daha önce kaydedilmiş tehdit modellerini açar. En son açılan dosyalaraçmanız gerekiyorsa, son açılan modeller özelliği harikadır. Seçimin üzerine çıktığınızda, modelleri açmanın 2 yolunu bulacaksınız:</p><p><ul><li>Bu Bilgisayardan Aç – yerel depolama alanını kullanarak dosyaaçmanın klasik yolu</li><li>OneDrive'dan açık – ekipler, üretkenliği ve işbirliğini artırmaya yardımcı olmak için tüm tehdit modellerini tek bir konumda kaydetmek ve paylaşmak için OneDrive'daki klasörleri kullanabilir</li></ul></p> |
| **Başlangıç Kılavuzu**                   | Microsoft **[Tehdit Modelleme Aracı](threat-modeling-tool.md)** ana sayfasını açar                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Şablon bölümü

| Bileşen               | Ayrıntılar                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Yeni Şablon Oluştur** | Üzerine inşa etmen için boş bir şablon açar. Sıfırdan şablon oluşturma hakkında kapsamlı bir bilgiye sahip değilseniz, mevcut şablonlardan oluşturmanızı öneririz |
| **Şablonu Aç**       | Değişiklik yapabilmeniz için varolan şablonları açar                                                                                                              |

Tehdit Modelleme Aracı ekibi, araç işlevselliğini ve deneyimini geliştirmek için sürekli olarak çalışmaktadır. Yıl boyunca birkaç küçük değişiklik meydana gelebilir, ancak tüm büyük değişiklikler kılavuzda yeniden yazılmasını gerektirir. En son duyuruları aldığınızdan emin olmak için sık sık bakın.

## <a name="building-a-model"></a>Model oluşturma

Bu bölümde, aşağıdaki:

- Cristina (geliştirici)
- Ricardo (bir program yöneticisi) ve
- Ashish (bir test)

İlk tehdit modellerini geliştirme sürecinden geçiyorlar.

> Ricardo: Merhaba Cristina, tehdit modeli diyagramı üzerinde çalıştım ve detayları doğru yaptığımızdan emin olmak istedim. Bakmama yardım edebilir misin?
> Kesinlikle. Şimdi bir göz atalım.
> Ricardo aracı açar ve ekranını Cristina ile paylaşır.

![Temel Tehdit Modeli](./media/threat-modeling-tool-feature-overview/basictmt.png)

> Cristina: Tamam, basit görünüyor, ama bana yol layabilir misin?
> Ricardo: Tabii! İşte arıza:
> - Bizim insan kullanıcı bir dış varlık olarak çizilir-bir kare
> - Web sunucumuza komutgönderiyorlar.
> - Web sunucusu bir veritabanına danışmanlık hizmeti vermektedir (iki paralel çizgi)

Ne Ricardo sadece Cristina gösterdi bir DFD, **[Veri Akış Diyagramı](https://en.wikipedia.org/wiki/Data_flow_diagram)** için kısa . Tehdit Modelleme Aracı, kullanıcıların farklı varlıkların denetimde nerede olduğunu göstermek için kırmızı noktalı çizgilerle gösterilen güven sınırlarını belirtmelerine olanak tanır. Örneğin, BT yöneticileri kimlik doğrulama amacıyla Etkin Dizin sistemi gerektirir, bu nedenle Etkin Dizin kendi denetiminin dışındadır.

> Cristina: Bana doğru geliyor. Peki ya tehditler?
> Sana göstereyim.

## <a name="analyzing-threats"></a>Tehditleri analiz etme

Simge menü seçiminden (büyüteçli dosya) analiz görünümüne tıkladıktan sonra, varsayılan şablona dayalı olarak bulunan Tehdit Modelleme Aracı'nın oluşturulan tehditler listesine alınır ve bu da **[STRIDE (Sızdırma, Kurcalama, Bilgi Verme, Reddetme, Hizmet Reddi ve Ayrıcalık Yükselmesi)](https://en.wikipedia.org/wiki/STRIDE_(security))** adı verilen SDL yaklaşımını kullanır. Fikir, yazılım bu 6 kategori kullanılarak bulunabilir tehditler, öngörülebilir bir dizi altında gelir.

Bu yaklaşım, bir alarm sistemi eklemeden veya hırsızı kovalamadan önce her kapı ve pencerenin bir kilitleme mekanizmasına sahip olmasını sağlayarak evinizi güvence altına almak gibidir.

![Temel Tehditler](./media/threat-modeling-tool-getting-started/basicthreats.png)

Ricardo listedeki ilk öğeyi seçerek başlar. Şöyle olur:

İlk olarak, iki şablon arasındaki etkileşim geliştirilmiştir

![Etkileşim](./media/threat-modeling-tool-getting-started/interaction.png)

İkinci olarak, Tehdit Özellikleri penceresinde tehditle ilgili ek bilgiler görünür

![Etkileşim Bilgileri](./media/threat-modeling-tool-getting-started/interactioninfo.png)

Oluşturulan tehdit onu potansiyel tasarım kusurları anlamanıza yardımcı olur. STRIDE kategorisi ona potansiyel saldırı vektörleri hakkında bir fikir verirken, ek açıklama ona neyin yanlış olduğunu ve bunu azaltmanın potansiyel yollarını söyler. Düzenleme alanları, yaslama ayrıntılarına not yazmak veya kuruluşunun hata çubuğuna bağlı olarak öncelik derecelendirmelerini değiştirmek için kullanabilir.

Azure şablonları, kullanıcıların yalnızca neyin yanlış olduğunu anlamalarına değil, Azure'a özgü belgelere açıklamalar, örnekler ve köprüler ekleyerek bu şablonu nasıl düzelteceklerini anlamalarına yardımcı olacak ek ayrıntılara sahiptir.

Açıklama, kullanıcıların sahtesini önlemek için bir kimlik doğrulama mekanizması eklemenin önemini fark etmesini sağladı ve üzerinde çalışılacak ilk tehdidi ortaya çıkardı. Cristina ile tartışma içine birkaç dakika, onlar erişim kontrolü ve rolleri uygulanmasının önemini anladım. Ricardo bunların uygulandığından emin olmak için bazı hızlı notlar doldurdu.

Ricardo, Bilgi Açıklama kapsamında tehditlere girerken, erişim kontrol planının denetim ve rapor üretimi için salt okunur hesaplar gerektirdiğini fark etti. Bunun yeni bir tehdit olup olmaması gerektiğini merak etti, ancak hafifletmeler aynıydı, bu yüzden de tehdide de dikkat çekti.
Ayrıca bilgi ifşası hakkında biraz daha düşündü ve yedek kasetlerin şifrelemeye ihtiyacı olacağını fark etti.

Mevcut azaltıcı etkenler veya güvenlik garantileri nedeniyle tasarımiçin geçerli olmayan tehditler Durum açılır tarihinden itibaren "Geçerli Değil" olarak değiştirilebilir. Üç seçenek daha vardır: Başlatılmama - varsayılan seçim, İhtiyaçlar Araştırması – öğeleri takip etmek için kullanılır ve azaltılır – tam olarak üzerinde çalıştıktan sonra.

## <a name="reports--sharing"></a>Paylaşım & raporlar

Ricardo Cristina ile listeyi gözden geçirip önemli notlar, azaltıcı/gerekçeler, öncelik ve durum değişiklikleri ekledikten sonra, uygun güvenlik çalışmalarının uygulanmasını sağlamak için iş arkadaşlarıyla birlikte gitmesi için güzel bir rapor yazdıran Raporlar -> Tam Rapor Oluştur -> Kaydet Raporu'nu seçer.

![Etkileşim Bilgileri](./media/threat-modeling-tool-feature-overview/report.png)

Ricardo bunun yerine dosyayı paylaşmak isterse, bunu kuruluşunun OneDrive hesabında niçin kaydedebilirsiniz. Bunu yaptıktan sonra, belge bağlantısını kopyalayabilir ve iş arkadaşlarıyla paylaşabilir. 

## <a name="threat-modeling-meetings"></a>Tehdit modelleme toplantıları

Ricardo tehdit modelini OneDrive'ı kullanarak meslektaşına gönderdiğinde, test eden Ashish yetersiz kaldı. Ricardo ve Cristina kolayca tehlikeye olabilir oldukça birkaç önemli köşe durumlarda, cevapsız gibi görünüyordu. Şüpheciliği tehdit modellerinin tamamlayıcısıdır.

Bu senaryoda, Ashish tehdit modelini devraldıktan sonra, iki tehdit modelleme toplantısı çağrısında bulundu: bir toplantı süreci senkronize etmek ve diyagramları gözden geçirmek ve sonra tehdit incelemesi ve imzalama için ikinci bir toplantı.

İlk toplantıda, Ashish SDL tehdit modelleme sürecinde herkes yürüyüş 10 dakika geçirdi. Daha sonra tehdit modeli diyagramını çıkardı ve ayrıntılı olarak açıklamaya başladı. Beş dakika içinde, önemli bir eksik bileşen tespit edildi.

Birkaç dakika sonra, Ashish ve Ricardo Web sunucusunun nasıl kurulduğu hakkında uzun bir tartışmaya girdiler. Bu bir toplantı nın devam etmesi için ideal bir yol değildi, ama herkes sonunda bu tutarsızlığı erken keşfetmenin onlara gelecekte zaman kazandıracağını kabul etti.

İkinci toplantıda, ekip tehditleri gözden geçirip, bunları ele almanın bazı yollarını tartıştı ve tehdit modelini imzaladı. Belgeyi kaynak denetimine dönüştürdüler ve geliştirmeye devam ettiler.

## <a name="thinking-about-assets"></a>Varlıklar hakkında düşünme

Tehdit modellenmiş bazı okuyucular biz hiç varlıklar hakkında konuşmadık fark edin. Birçok yazılım mühendisinin yazılımlarını varlık kavramını ve bir saldırganın hangi varlıklarla ilgilenebileceğini anladıklarından daha iyi anladığını keşfettik.

Eğer bir ev modeli tehdit gidiyoruz, aileniz, yeri doldurulamaz fotoğraflar veya değerli sanat düşünerek başlayabilirsiniz. Belki de kimin içeri girebileceğini ve şu anki güvenlik sistemini düşünerek başlayabilirsin. Ya da havuz veya ön veranda gibi fiziksel özellikleri göz önünde bulundurarak başlayabilirsiniz. Bunlar, varlıklar, saldırganlar veya yazılım tasarımı hakkında düşünmeye benzerdir. Bu üç yaklaşımdan herhangi biri işe yarıyor.

Burada sunduğumuz tehdit modelleme yaklaşımı, Microsoft'un geçmişte yaptıklarından çok daha basittir. Yazılım tasarım yaklaşımının birçok ekip için iyi çalıştığını bulduk. Umarız ki buna sizinki de dahildir.

## <a name="next-steps"></a>Sonraki Adımlar

Sorularınızı, yorumlarınızı ve tmtextsupport@microsoft.comendişelerinizi . Başlamak için Tehdit Modelleme Aracı'nı **[indirin.](https://aka.ms/threatmodelingtool)**
