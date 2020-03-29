---
title: Konuşma fonetik setleri - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti fonetik alfabe haritalarının Uluslararası Fonetik Alfabesi 'ne zaman yapılacağını ve hangi kümenin ne zaman kullanılacağını öğrenin.
services: cognitive-services
author: zhaoyunED
manager: junwg
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: jiajzhan
ms.openlocfilehash: 770e97ad126f66efb43bf8cf7eb12f7510858192
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78675337"
---
# <a name="speech-service-phonetic-sets"></a>Konuşma hizmeti fonetik setleri

Konuşma hizmeti, yedi dilden oluşan fonetik alfabeleri ("kısaca telefon kümeleri" olarak tanımlar; `en-US`, `fr-FR` `de-DE`, `es-ES` `ja-JP`, `zh-CN`, `zh-TW`, ve . Konuşma hizmeti telefon genellikle Uluslararası <a href="https://en.wikipedia.org/wiki/International_Phonetic_Alphabet" target="_blank">Fonetik Alfabe (IPA) <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için harita ayarlar. Konuşma hizmeti telefon setleri, Metinden konuşmaya hizmet sunumunun bir parçası olarak [Konuşma Sentezi Biçimlendirme Dili (SSML)](speech-synthesis-markup.md)ile birlikte kullanılır. Bu makalede, bu telefon setlerinin nasıl eşlendiği ve hangi telefon kümesinin ne zaman kullanılacağı öğrenilir.

# <a name="en-us"></a>[tr-ABD](#tab/en-US)

### <a name="english-suprasegmentals"></a>İngilizce suprasegmentals

| Örnek 1 (Ünsüz için başlangıç, sesli harf için sözcük baş harfi) | Örnek 2 (Ünsüz için intervocalic, sesli harf için kelime medial çekirdek) | Örnek 3 (Ünsüz için Coda, sesli harf için son sözcük) | Yorumlar |
|--|--|--|--|
| burger /b er **1** r - g balta r/ | falafel /f balta - l aa **1** - f balta l/ | gitar /g ih - t aa **1** r/ | Konuşma hizmeti telefon seti stresli hece sesli sonra stres koymak |
| inopportune /ih **2** - n aa - p balta r - t uw 1 n/ | benzerlik /d ih - s ih **2**- m balta - l eh 1 - r balta - t iy/ | işgücü /w er 1 r k - f ao **2** r s/ | Konuşma hizmeti telefon seti alt stresli hece nin sesli sonra stres koymak |

### <a name="english-vowels"></a>İngilizce sesli harfler

| `sapi` | `ipa` | Örnek 1     | Örnek 2 | Örnek 3                   |
|--------|-------|---------------|-----------|-----------------------------|
| Iy     | `i`   | **ea**t       | f**ee**l  | vall**ey**                  |
| ıh     | `ɪ`   | **i**f        | f**i**ll  |                             |
| ey     | `eɪ`  | **bir**te       | g**bir**te  | d**ay**                     |
| eh     | `ɛ`   | **e**çok     | p**e**t   | m**eh** (nadir kelime nihayet) |
| Ae     | `æ`   | **bir**ctive    | c**a**t   | n**ah** (nadir kelime nihayet) |
| aa     | `ɑ`   | **o**bstinate | p**o**ppy | r**ah** (nadir kelime nihayet) |
| Ao     | `ɔ`   | **o**aralığı    | c**au**se | Ut**ah**                    |
| ah     | `ʊ`   | b**oo**k      |           |                             |
| ow     | `oʊ`  | **o**ld       | cl**o**ne | g**o**                      |
| Uw     | `u`   | **U**ber      | b**oo**st | t**oo**                     |
| ah     | `ʌ`   | **u**ncle     | c**u**t   |                             |
| ay     | `aɪ`  | **i**ce       | b**i**te  | fl**y**                     |
| aw     | `aʊ`  | **ou**t       | s**ou**th | c**ow**                     |
| oy     | `ɔɪ`  | **oi**l       | j**oi**n  | t**oy**                     |
| y uw   | `ju`  | **Yu**ma      | h**u**adam | f**ew**                     |
| Ax     | `ə`   | **bir**gitmek       | wom**a**n | **bir**                    |

### <a name="english-r-colored-vowels"></a>İngilizce R renkli sesli harfler

| `sapi` | `ipa` | Örnek 1    | Örnek 2      | Örnek 3  |
|--------|-------|--------------|----------------|------------|
| ih r   | `ɪɹ`  | **kulak**s     | t**ir**amisu   | n**kulak**   |
| eh r   | `ɛɹ`  | **hava**uçağı | app**ar**ently | sc**ar**e  |
| uh r   | `ʊɹ`  |              |                | c**sizin**e   |
| ay r   | `aɪɹ` | **Ire**arazi  | f**ir**eplace  | ch**oir**  |
| aw r   | `aʊɹ` | **saat**s    | p**ower**ful   | bizim**our**   |
| ao r   | `ɔɹ`  | **veya**ange   | m**veya**al      | s**kürek**   |
| aa r   | `ɑɹ`  | **ar**tist   | st**ar**t      | c**ar**    |
| er r   | `ɝ`   | **kulak**th    | b**ir**d       | f**sizin**    |
| balta r   | `ɚ`   |              | tüm**er**gy    | supp**er** |

### <a name="english-semivowels"></a>İngilizce Semivowels

| `sapi` | `ipa` | Örnek 1           | Örnek 2  | Örnek 3 |
|--------|-------|---------------------|------------|-----------|
| w      | `w`   | **w**ith, s**ue**de | al**w**ays |           |
| y      | `j`   | **y**ard, f**e**w   | **üzerinde i**  |           |

### <a name="english-aspirated-oral-stops"></a>İngilizce aspire oral durur

| `sapi` | `ipa` | Örnek 1 | Örnek 2   | Örnek 3  |
|--------|-------|-----------|-------------|------------|
| p      | `p`   | **p**ut   | ha**pp**en  | fla**p**   |
| b      | `b`   | **b**ig   | num**b**er  | cra**b**   |
| t      | `t`   | **t**alk  | capi**t**al | sough**t** |
| d      | `d`   | **d**ig   | koştu**d**om  | ro**d**    |
| k      | `k`   | **c**ut   | sla**ck**er | Ira**q**   |
| g      | `g`   | **g o**    | bir**g**o     | dra**g**   |

### <a name="english-nasal-stops"></a>İngilizce Burun durur

| `sapi` | `ipa` | Örnek 1        | Örnek 2  | Örnek 3   |
|--------|-------|------------------|------------|-------------|
| m      | `m`   | **m**at, şut   | ca**m**dönemi | roo**m**    |
| n      | `n`   | **n**o, s**n**ow | te**n**t   | civciv**n** |
| Ng     | `ŋ`   |                  | li**n**k   | s**ing**    |

### <a name="english-fricatives"></a>İngilizce fricatives

| `sapi` | `ipa` | Örnek 1   | Örnek 2        | Örnek 3  |
|--------|-------|-------------|------------------|------------|
| f      | `f`   | **f**ork    | le**f**t         | hal**f**   |
| v      | `v`   | **v**alue   | e**v**ent        | lo**v**e   |
| inci     | `θ`   | **inci yılında**    | empa**th**y      | mon**th**  |
| Dh     | `ð`   | **th**tr    | mo**th**er       | smoo**th** |
| s      | `s`   | **s**o     | ri**s**k         | aslında**s**  |
| z      | `z`   | **z**ap     | bu**s**y         | çocuk**s**   |
| ş     | `ʃ`   | **ş** e    | abbrevia**ti**üzerinde | ru**ş**   |
| Zh     | `ʒ`   | **J**acques | plea**s**ure     | gara**g**e |
| h      | `h`   | **h**elp    | en**h**ance      | a-**h**a!  |

### <a name="english-affricates"></a>İngilizce affricates

| `sapi` | `ipa` | Örnek 1 | Örnek 2    | Örnek 3  |
|--------|-------|-----------|--------------|------------|
| ch     | `tʃ`  | **içinde ch**  | fu**t**üre   | atta**ch** |
| Jh     | `dʒ`  | **j**oy   | ori**g**inal | oran**g e** |

### <a name="english-approximants"></a>İngilizce yaklaşımlar

| `sapi` | `ipa` | Örnek 1          | Örnek 2  | Örnek 3 |
|--------|-------|--------------------|------------|-----------|
| l      | `l`   | **l**id, g**l**reklam  | pa**l**as | chi**ll** |
| r      | `ɹ`   | **r**ed, b**r**ing | bo**rr**ow | ta**r**   |

# <a name="fr-fr"></a>[fr-FR](#tab/fr-FR)

### <a name="french-suprasegmentals"></a>Fransız suprasegmentals

Konuşma servis telefon seti stresli hece nin sesli sonra stres koyar, ancak; `fr-FR` Konuşma hizmeti telefon seti IPA alt stres '' desteklemez. IPA alt stresi gerekiyorsa, IPA'yı doğrudan kullanmalısınız.

### <a name="french-vowels"></a>Fransızca sesli harfler

| `sapi` | `ipa` | Örnek 1     | Örnek 2       | Örnek 3 |
|--------|-------|---------------|-----------------|-----------|
| a      | `a`   | **bir**rbre     | p**a**tte       | ir**a**   |
| aa     | `ɑ`   |               | p**â**te        | p**a**s   |
| aa ~   | `ɑ̃`  | **tr**fant    | enf**en**t      | t**em**ps |
| Ax     | `ə`   |               | p**e**tite      | l**e**    |
| eh     | `ɛ`   | **e**lle      | p**e**rdu       | ét**ai**t |
| Ab     | `ø`   | **œu**fs      | cr**eu**ser     | qu**ab**  |
| ey     | `e`   | ému           | crétin          | ôté       |
| eh ~   | `ɛ̃`  | **im**portant | p**ein**ture    | **içinde** paspas |
| Iy     | `i`   | **i**dée      | evcil hayvan**i**te      | **i**   |
| Oe     | `œ`   | **œu**f       | p**eu**r        |           |
| Oh     | `ɔ`   | **o**bstacle  | c**o**rps       |           |
| oh ~   | `ɔ̃`  | **üzerinde**ze      | r**üzerinde**deur     | b**üzerinde**   |
| ow     | `o`   | **au**diteur  | b**eau**darbe    | p**ô**    |
| oe ~   | `œ̃ ` | **Bm**        | l**un**di       | br**un**  |
| Uw     | `u`   | **ou**trajedi   | intr**ou**vable | **Ou**    |
| Uslu     | `y`   | **u**ne       | p**u**nir       | él**u**   |

### <a name="french-consonants"></a>Fransız ünsüzleri

| `sapi` | `ipa` | Örnek 1   | Örnek 2     | Örnek 3                        |
|--------|-------|-------------|---------------|----------------------------------|
| b      | `b`   | **b**ête    | ha**b**ille   | ro**b**e                         |
| d      | `d`   | **d**ire    | ron**d**eur   | chau**d**e                       |
| f      | `f`   | **f**emme   | su**ff**ixe   | bo**f**                          |
| g      | `g`   | **g**auche  | é**g**bira     | ba**gu**e                        |
| Ng     | `ŋ`   |             |               | [<sup>1</sup>](#fr-1)park**etme** |
| Hy     | `ɥ`   | h**u**ile   | n**u**ire     |                                  |
| k      | `k`   | **c**arte   | é**c**aille   | **c** olmak                          |
| l      | `l`   | **l**ong    | é**l**ire     | ba**l**                          |
| m      | `m`   | **m**adame  | ai**m**er     | po**mm**e                        |
| n      | `n`   | **n**ous    | te**n**ir     | bo**nn**e                        |
| Nj     | `ɲ`   |             |               | pei**gn**e                       |
| p      | `p`   | **p**atte   | re**p**olarak     | ca**p**                          |
| r      | `ʁ`   | **r**at     | cha**r**iot   | senti**r**                       |
| s      | `s`   | **s**ourir  | bir**ss**ez     | pa**ss**e                        |
| ş     | `ʃ`   | **ch**anter | ma**ch**ine   | po**ch**e                        |
| t      | `t`   | **t**ête    | ô**t**er      | ne**t**                          |
| v      | `v`   | **v**ent    | **içinde v**girin  | rê**v**e                         |
| w      | `w`   | **ou**i     | f**ou**ine    |                                  |
| y      | `j`   | **y**od     | p**i**étiner  | Marse**ille**                    |
| z      | `z`   | **z **éro   | rai**s**onner | ro**s**e                         |
| Zh     | `ʒ`   | **j**ardin  | adam**g**er    | piè**g**e                        |
|        | `n‿`  |             |               | u**n** arbre                     |
|        | `t‿`  |             |               | quan**d**                        |
|        | `z‿`  |             |               | di**x**                          |

<a id="fr-1"></a>
**1** *Sadece bazı yabancı kelimeler için.*

> [!TIP]
> `fr-FR` Konuşma hizmeti telefon seti aşağıdaki Fransızca liasions `n‿`desteklemez, , `t‿`ve `z‿`. Bunlar gerekiyorsa, doğrudan IPA kullanmayı düşünmelisiniz.

# <a name="de-de"></a>[de-DE](#tab/de-DE)

### <a name="german-suprasegmentals"></a>Alman suprasegmentals

| Örnek 1 (Ünsüz için başlangıç, sesli harf için sözcük baş harfi) | Örnek 2 (Ünsüz için intervocalic, sesli harf için kelime medial çekirdek) | Örnek 3 (Ünsüz için Coda, sesli harf için son sözcük) | Yorumlar |
|--|--|--|--|
| anders /a **1** n - d balta r s/ | Çarpmazeichen /m uh l - t iy - p l iy - k a - ts y ow **1** n s - ts ay - c n/ | Biologie /b iy - ow - l ow - g iy **1**/ | Konuşma hizmeti telefon seti stresli hece sesli sonra stres koymak |
| Allgemeinwissen /a **2** l - g balta - m ay 1 n - v ih - s n/ | Abfallentsorgungsfirma /a 1 p - f a l - ^ eh n t - z oh **2** balta r - g uh ng s - f ih ax r - m a/ | Computertomographie /k oh m - p y uw 1 - t ax r - t ow - m ow - g r a - f iy **2**/ | Konuşma hizmeti telefon seti alt stresli hece nin sesli sonra stres koymak |

### <a name="german-vowels"></a>Almanca sesli harfler

| `sapi` | `ipa`     | Örnek 1                             | Örnek 2     | Örnek 3                          |
|--------|-----------|---------------------------------------|---------------|------------------------------------|
| A:     | `aː`      | **Bir**ber                              | Maßst**a**b   | Şema**bir**                         |
| a      | `a`       | **Bir**bfall                            | B**a**ch      | Agath**bir**                         |
| Oh     | `ɔ`       | **Ey**sten                             | Pf**o**sten   |                                    |
| eh:    | `ɛː`      | **Ä**hnlichkeit                       | B**ä**r       | [<sup>1.1.2</sup>](#de-v-1) Fasci**ae** |
| eh     | `ɛ`       | **ä**ndern                            | Proz**e**nt   | Amigdal**ae**                      |
| Ax     | `ə`       | [<sup>2</sup>](#de-v-2)'v**e**rstauen | Aach**e**n    | Frag**e**                          |
| Iy     | `iː`      | **Koştum.**                              | abb**yani**gt   | Relativitätstheor**yani**            |
| ıh     | `ɪ`       | **Ben**nnung                            | s**i**ngen    | Ahşap**y**                          |
| Ab     | `øː`      | **Ö**sen                              | abl**ö**sten  | Malm**ö**                          |
| ow     | `o`, `oː` | **o**hne                              | Balk**o**n    | Trept**ow**                        |
| Oe     | `œ`       | **Ö**ffnung                           | bef**ö**rdern |                                    |
| ey     | `e`, `eː` | **E**berhard                          | abf**e**gt    | b                                  |
| Uw     | `uː`      | **U**yapmak                               | H**u**t       | Akk**u**                           |
| ah     | `ʊ`       | **U**nterschiedes                     | b**u**nt      |                                    |
| Ue     | `yː`      | **Ü**bermut                           | pfl**ü**gt    | Erkekler**ü**                           |
| Uslu     | `ʏ`       | **ü**ppig                             | S**y**kök    |                                    |

<a id="de-v-1"></a>
**1** *Sadece yabancı kökenli kelimeler, gibi: Fasci**ae**.*<br>
<a id="de-v-2"></a>
**2** *Kelime-intially sadece yabancı kökenli kelimelerde **bir**ppointment gibi. Hece-başlangıçta: 'v**e**rstauen.*

### <a name="german-diphthong"></a>Alman diphthong

| `sapi` | `ipa`       | Örnek 1    | Örnek 2          | Örnek 3 |
|--------|-------------|--------------|--------------------|-----------|
| ay     | `ai`        | **ei**nsam   | Unabhängigk**ei**t | Abt**ei** |
| aw     | `au`        | **au**ßen    | abb**au**st        | St**au**  |
| oy     | `ɔy`, `ɔʏ̯` | **Ab**phorie | tr**äu**mt         | sch**eu** |

### <a name="german-semivowels"></a>Alman yarı sesli

| `sapi` | `ipa` | Örnek 1 | Örnek 2    | Örnek 3  |
|--------|-------|-----------|--------------|------------|
| balta r   | `ɐ`   |           | abänd**er**n | kilit**er** |

### <a name="german-consonants"></a>Alman ünsüzleri

| `sapi` | `ipa` | Örnek 1 | Örnek 2 | Örnek 3 |
|--|--|--|--|--|
| b | `b` | **B**ank |  | [<sup>1.1.2</sup>](#de-c-1) Pu**b** |  |
| c | `ç` | **Ch**emie | mögli**ch**st | [<sup>2</sup>](#de-c-2)i**ch** |
| d | `d` | **d**anken | [<sup>3.2.2</sup>](#de-c-3) Len**d**l | [<sup>4.2.2</sup>](#de-c-4) Clau**d**e |  |
| Jh | `ʤ` | **J**eff | gemana**g**t | [<sup>5.000</sup>](#de-c-5) Chan**g**e |
| f | `f` | **F**ahrtdauer | angri**ff**slustig | abbruchrei**f** |  |
| g | `g` | **g**ut |  | [<sup>6.000</sup>](#de-c-6) Gre**g** |  |
| h | `h` | **H**ausanbau |  |  |  |
| y | `j` | **J**od | Reakt**i**üzerinde | hu**i** |  |
| k | `k` | **K**oma | Aspe**k**t | Flec**k** |  |
| l | `l` | **l**au | ähne**l**n | zuvie**l** |  |
| m | `m` | **M**ut | A**m**t | Leh**m** |  |
| n | `n` | **n**un | u**n**d | Huh**n** |  |
| Ng | `ŋ` | [<sup>7</sup>](#de-c-7)**Ng**uyen | Schwa**nk** | R**ing** |  |
| p | `p` | **P**artner | ebru**p**t | Ti**p** |  |
| Pf | `pf` | **Pf**erd | baraj**pf**t | Pf**için** |  |
| r | `ʀ`, `r`, `ʁ` | **R**eise | knu**rr**t | Haa**r** |  |
| s | `s` | [<sup>8</sup>](#de-c-8)**S**taccato | bi**s**t | mie**s** |  |
| ş | `ʃ` | **Sch**ule | mi**sch**t | lappi**sch** |  |
| t | `t` | **T**raum | S**t**raße | Mu**t** |  |
| Ts | `ts` | **Z**ug | Ar**z**t | Wit**z** |  |
| ch | `tʃ` | **Tsch**echien | aufgepu**tsch**t | bundesdeu**tsch** |  |
| v | `v` | **w**inken | Q**u**alle | [<sup>9.000</sup>](#de-c-9) Gr**oo**ve |  |
| x | [<sup>10</sup>](#de-c-10)`x`,[<sup>11</sup>](#de-c-11)`ç` | [<sup>12.000.0</sup>](#de-c-12) Ba**ch**erach | Ma**ch**t mögli**ch**st | Schma**ch** 'i**ch** |
| z | `z` | **s**uper |  |  |  |
| Zh | `ʒ` | **G**enre | B**re**ezinski | Edvi**g**e |

<a id="de-c-1"></a>
**1** *Sadece yabancı kökenli kelimeler, gibi: Pu**b**.*<br>
<a id="de-c-2"></a>
**2** *Yumuşak "ch" sonra "e" ve "i"*<br>
<a id="de-c-3"></a>
**3** *Sadece yabancı kökenli kelimeler, gibi: Len**d**l.*<br>
<a id="de-c-4"></a>
**4** *Sadece yabancı kökenli kelimeler gibi: Clau**d**e.*<br>
<a id="de-c-5"></a>
**5** *Sadece yabancı kökenli kelimeler gibi: Chan**g**e.*<br>
<a id="de-c-6"></a>
**6** *Kelime-ölümcül sadece Gre**g**gibi yabancı kökenli kelimelerle .*<br>
<a id="de-c-7"></a>
**7** *Sadece yabancı kökenli kelimeler gibi: **Ng**uyen.*<br>
<a id="de-c-8"></a>
**8** *Sadece yabancı kökenli kelimeler gibi: **S**taccato.*<br>
<a id="de-c-9"></a>
**9** *Sadece yabancı kökenli kelimeler, gibi: Gr**oo**ve.*<br>
<a id="de-c-10"></a>
**10** *IPA `x` sert bir "ch" tüm ön olmayan sesli sonra (a, aa, oh, ow, uh, uw ve diphthong aw).*<br>
<a id="de-c-11"></a>
**11** *IPA `ç` yumuşak bir 'ch' ön sesli sonra (ih, iy, eh, ae, uy, ue, oe, eu da ddhthongs ay, oy) ve ünsüzler*<br>
<a id="de-c-12"></a>
**12** *Kelime-başlangıçta sadece yabancı kökenli kelimeler, gibi: **J**uan. Hece-başlangıçta da gibi kelimelerle: Ba**ch**erach.*<br>

### <a name="german-oral-consonants"></a>Alman sözlü ünsüzleri

| `sapi` | `ipa` | Örnek 1                                  |
|--------|-------|--------------------------------------------|
| ^      | `ʔ`   | beachtlich /b balta - ^ a 1 x t - l ih c/ |

> [!NOTE]
> İki farklı sesli harf\] in arasına bir gs telefonu eklememiz gerekiyor, ancak iki sesli harf gerçek bir diphthong. Bu oral ünsüz bir glottal durdurmak, daha <a href="http://en.wikipedia.org/wiki/Glottal_stop" target="_blank">fazla bilgi <span class="docon docon-navigate-external x-hidden-focus"> </a> </a>için, glottal durdurmak bakın .

# <a name="es-es"></a>[es-ES](#tab/es-ES)

### <a name="spanish-vowels"></a>İspanyolca sesli harfler

| `sapi` | `ipa` | Örnek 1    | Örnek 2     | Örnek 3    |
|--------|-------|--------------|---------------|--------------|
| a      | `a`   | **bir**lto     | c**bir**ntar    | cas**a**     |
| ı      | `i`   | **i**bérica  | av**i**spa    | vergi**i**     |
| e      | `e`   | **e**lefante | **e**nto at    | elefant**e** |
| o      | `o`   | **o**caso    | enc**o**ntrar | ocasenc**o** |
| u      | `u`   | **u**sted    | p**u**nta     | Juanl**u**   |

### <a name="spanish-consonants"></a>İspanyol ünsüzleri

| `sapi` | `ipa`      | Örnek 1  | Örnek 2      | Örnek 3      |
|--------|------------|------------|----------------|----------------|
| b      | `b`        | **b**aobab |                | **b**        |
|        | `β`        |            | bao**b**ab     | baoba**b**     |
| ch     | `tʃ`       | **ch**eque | co**ch**e      | Marraque**ch** |
| d      | `d`        | **d**edo   |                | portlan**d**   |
|        | `ð`        |            | de**d**o       | verda**d**     |
| f      | `f`        | **f**ácil  | ele**f**ante   | pu**f**        |
| g      | `g`        | **g**anga  |                | dópin**g**     |
|        | `ɣ`        |            | bir**g**ua       | tuare**g**     |
| J      | `j`        | **i**odo   | cal**i**ente   | yeniden**y**        |
| Jj     | `j.j` `jj` |            | vi**ll**a      |                |
| k      | `k`        | **c**oche  | bo**c**a       | titáni**c**    |
| l      | `l`        | **l**ápiz  | a**l**a        | corde**l**     |
| Ll     | `ʎ`        | **ll**ave  | desarro**ll**o |                |
| m      | `m`        | **m**sırası | a**m**ar       | álbu**m**      |
| n      | `n`        | **n**ada   | ce**n**a       | rató**n**      |
| Nj     | `ɲ`        | **ñ**aña   | ara**ñ**azo    |                |
| p      | `p`        | **p**oca   | **için p**o       | sto**p**       |
| r      | `ɾ`        |            | ca**r a**       | abri**r**      |
| Rr     | `r`        | **r**adio  | co**rr**e      | pu**rr**       |
| s      | `s`        | **s**aco   | va**s**o       | pelo**s**      |
| t      | `t`        | **t**oldo  | bir**t**ar       | disque**t**    |
| inci     | `θ`        | **z**ebra  | a**z**ul       | lápi**z**      |
| w      | `w`        | h**u**eso  | ag**u**a       | gua**u**       |
| x      | `x`        | **j**ota   | bir**j**o        | relo**j**      |

> [!TIP]
> Konuşma hizmeti telefon seti aşağıdaki İspanyolca `β`IPA'yı `ð`ve `ɣ` `es-ES` Bunlar gerekiyorsa, doğrudan IPA kullanmayı düşünmelisiniz.

# <a name="zh-cn"></a>[zh-CN](#tab/zh-CN)

Konuşma servis telefonu `zh-CN` için ayarlanmış yerel telefon <a href="https://en.wikipedia.org/wiki/Pinyin" target="_blank">Pinyin <span class="docon docon-navigate-external x-hidden-focus"></span> </a> seti ne dayanır.

### <a name="tone"></a>Ton

| Pinyin sesi | `sapi` | Karakter örneği |
|-------------|--------|-------------------|
| mā          | ma 1  | 2010-                 |
| má          | ma 2  | 2010/                 |
| ms          | ma 3  | Bu konuda                 |
| mà          | ma 4  | 2010/                 |
| ma          | ma 5  | 2010/                 |

#### <a name="example"></a>Örnek

| Karakter | Konuşma hizmeti                |
|-----------|-------------------------------|
| 2010'da, 2006'da      | zu 3 - zhi 1 - guan 1 - xi 5 |
| 2007'de, 2        | lei 3 -jin 4                 |
| 2010-2011       | xi 1 - zhai 2 - xiang 4      |

# <a name="zh-tw"></a>[zh-TW](#tab/zh-TW)

Konuşma servis telefonu `zh-TW` için ayarlanmış yerli telefon <a href="https://en.wikipedia.org/wiki/Bopomofo" target="_blank">Bopomofo <span class="docon docon-navigate-external x-hidden-focus"></span> </a> seti dayanmaktadır.

### <a name="tone"></a>Ton

| Konuşma hizmeti tonu | Bopomofo sesi | Örnek (sözcük) | Konuşma hizmeti telefonları | Bopomofo | Pinyin (Japonca) |
|---------------------|---------------|----------------|-----------------------|----------|-------------|
| 2010/                   | empty         | 2010/              | 2007'de, 2006                   | 2007'de, 2       | zhēn        |
| 2010/                   | 2010/             | 2010/              | 2007'de, 2006                   | 2007'de, 2006      | chá         |
| Bu da                   | Bu da             | BİlGİsaA              | 2010-2011                   | 2010-2011      | d          |
| 2010/                   | 2010/             | 2010/              | 2007'de, 2006                   | 2007'de, 2006      | wàng        |
| ˙                   | ˙             | Bu da             | Bir çok şey var.               | Bir çok şey var.  | yonca zi    |

#### <a name="example"></a>Örnek

| Karakter | `sapi`   |
|-----------|----------|
| 2010/         | 2007'de, 2006      |
| 2010-201        | Bir çok şey yok.   |
| 2007'de, 2        | Bir çok şey yok. |

# <a name="ja-jp"></a>[ja-JP](#tab/ja-JP)

Konuşma servis telefonu `ja-JP` için ayarlanmış yerel telefon <a href="https://en.wikipedia.org/wiki/Kana" target="_blank">Kana <span class="docon docon-navigate-external x-hidden-focus"></span> </a> seti dayanmaktadır.

### <a name="stress"></a>Stres

| `sapi` | `ipa`          |
|--------|----------------|
| `ˈ`    | `ˈ`mainstress |
| `+`    | `ˌ`alt stres  |

#### <a name="example"></a>Örnek

| Karakter | `sapi`  | `ipa`       |
|-----------|---------|-------------|
| 2010 yılında, 20        | 2007'de , 2006'da    | goˈwňseji   |
| 2010-2011       | Bu yüzden mi??? | 2007'de |
| 2010'da , 2006       | 中中中中中 +  | sajitecikaˈ |

***