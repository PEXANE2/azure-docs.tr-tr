---
title: Resim küçük resimlerini yeniden boyutlandırma ve kırpma - Bing Web Arama API
titleSuffix: Azure Cognitive Services
description: Bing Arama API'lerinden bazı yanıtlar, Bing tarafından sunulan ve yeniden boyutlandırıp ekebileceğiniz ve sorgu parametreleri içerebileceğiniz küçük resim resimlerine URL'ler içerir.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 05A08B01-89FF-4781-AFE7-08DA92F25047
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: aahi
ms.openlocfilehash: 630b86f55a537d109c851cb585cfccc34d229f83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74110631"
---
# <a name="resize-and-crop-thumbnail-images"></a>Küçük resim görüntülerini yeniden boyutlandırma ve kırpma

Bing Arama API'lerinden bazı yanıtlar, Bing tarafından sunulan ve yeniden boyutlandırıp ekebileceğiniz ve sorgu parametreleri içerebileceğiniz küçük resim resimlerine URL'ler içerir. Örnek:

`https://<host>/th?id=AMMS_92772df988...&w=110&h=73&rs=1&qlt=80&cdv=1&pid=16.1`

Bu küçük resimlerin bir alt kümesini görüntülerseniz, kalan görüntüleri görüntülemek için bir seçenek sağlayın.

> [!NOTE]
> Küçük resim görüntülerini kırpma ve yeniden boyutlandırmanın, Bing Arama API [kullanımı ve görüntüleme gereksinimleri](use-display-requirements.md)tarafından gerekli olduğu gibi üçüncü taraf haklarına saygı gösteren bir arama senaryosu sağlayacağından emin olun.

## <a name="resize-a-thumbnail"></a>Küçük resmi yeniden boyutlandırma 

Bing, küçük resmi yeniden boyutlandırmak için küçük `w` resmin `h` URL'sinde yalnızca bir (genişlik) veya (yükseklik) sorgu parametrelerini belirtmenizi önerir. Yalnızca yükseklik veya genişlik belirtilmesi Bing'in görüntünün orijinal yönünü korumasını sağlar. Piksellerde genişlik ve yüksekliği belirtin. 

Örneğin, orijinal küçük resim 480x620 ise:

`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=480&h=620`

Boyutunu küçültmek, parametreyi `w` yeni bir değere ayarlamak `336`(örneğin) `h` ve parametreyi kaldırmak istiyorsunuz:

`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=336`

Yalnızca küçük resmin yüksekliğini veya genişliğini belirtirseniz, görüntünün orijinal en boy oranı korunur. Her iki parametreyi de belirtirseniz ve en boy oranı korunmazsa, Bing görüntünün kenarlığı için beyaz dolgu ekler.

Örneğin, 480x359 görüntüsünü kırpma yapmadan 200x200 olarak yeniden boyutlandırırsanız, tam genişlik görüntüyü içerir, ancak yükseklik görüntünün üst ve alt kısmında 25 piksel beyaz dolgu içerir. Görüntü 359x480 ise, sol ve sağ kenarlıklar beyaz dolgu içerir. Görüntüyü kırpmak, beyaz dolgu eklenmez.  

Aşağıdaki resim, küçük resim resminin orijinal boyutunu (480x300) gösterir.  
  
![Orijinal manzara görüntüsü](./media/resize-crop/bing-resize-crop-landscape.png)  
  
Aşağıdaki resimde görüntü 200x200'e yeniden boyutlandırılmayı göstermektedir. En boy oranı korunur ve üst ve alt kenarlıklar beyaz ile dolguludur (buradaki siyah kenarlık dolguyu göstermek için dahildir).  
  
![Yeniden boyutlandırılmış manzara görüntüsü](./media/resize-crop/bing-resize-crop-landscape-resized.png)  

Görüntünün özgün genişliği nden ve yüksekliğinden daha büyük boyutlar belirtirseniz, Bing sol ve üst kenarlıklara beyaz dolgu ekler.  

## <a name="request-different-thumbnail-sizes"></a>Farklı küçük resim boyutları isteyin

Farklı bir küçük resim resmi boyutu istemek için, küçük resmin URL'sindeki tüm sorgu parametrelerini `id` kaldırın, ancak `pid` parametreler ve parametreler hariç. Ardından piksellerde `&w` istenen görüntü `&h` boyutuna sahip (genişlik) veya (yükseklik) sorgu parametresini ekleyin, ancak her ikisini birden eklemeyin. Bing görüntünün orijinal en boy oranını koruyacaktır. 

Yukarıdaki URL tarafından belirtilen görüntünün genişliğini 165 piksele çıkarmak için aşağıdaki URL'yi kullanırsınız:

`https://<host>/th?id=AMMS_92772df988...&w=165&pid=16.1`

Görüntünün orijinal boyutundan daha büyük bir görüntü isterseniz, Bing gerektiğinde görüntünün etrafına beyaz dolgu ekler. Örneğin, görüntünün orijinal boyutu 474x316 ise ve `&w` 500 olarak ayarlarsanız, Bing 500x333 görüntü döndürecektir. Bu görüntü, üst ve alt kenarları boyunca 8,5 piksel beyaz dolgu ve sol ve sağ kenarlarında 13 piksel dolgu olacaktır.

İstenen boyut resmin orijinal boyutundan büyükse Bing'in beyaz dolgu `&p` eklemesini önlemek için sorgu parametresini 0 olarak ayarlayın. Örneğin, yukarıdaki URL'ye `&p=0` parametre eklerseniz, Bing 500x333 resim yerine 474x316 resim döndürecektir:

`https://<host>/th?id=AMMS_92772df988...&w=500&p=0&pid=16.1`

Hem de `&w` `&h` sorgu parametrelerini belirtirseniz, Bing görüntünün en boy oranını korur ve gerektiğinde beyaz dolgu ekler. Örneğin, görüntünün orijinal boyutu 474x316 ise ve genişlik ve yükseklik parametrelerini 200x200 olarak ayarlarsanız (),`&w=200&h=200`Bing üst te ve altta 33 piksel beyaz dolgu içeren bir görüntü döndürür. Sorgu parametresini `&p` eklerseniz, Bing 200x134 görüntü döndürür.

## <a name="crop-a-thumbnail"></a>Küçük resim kırpma 

Görüntüyü kırpmak için `c` (kırpma) sorgu parametresini ekleyin. Aşağıdaki değerleri kullanabilirsiniz:
  
- `4`&mdash; Kör Oranı  
- `7`&mdash; Akıllı Oran  

### <a name="smart-ratio-cropping"></a>Akıllı Oran kırpma

Akıllı Oran kırpma isteğinde bulunuyorsanız `c` (parametreyi `7`ayarlayarak), Bing görüntünün en boy oranını korurken ilgi alanının merkezinden bir görüntü kırpacaktır. İlgi çekici bölge, Bing'in belirlediği görüntünün en çok içe aktarma parçaları içerdiği alandır. Aşağıdaki ilgi bir örnek bölge gösterir.  
  
![İlgi alanı](./media/resize-crop/bing-resize-crop-regionofinterest.png)

Bir görüntüyü yeniden boyutlandırıp Akıllı Oran kırpma isteğinde bulunarak Bing, en boy oranını korurken görüntüyü istenen boyuta indirir. Bing daha sonra görüntüyü yeniden boyutlara göre ekinler. Örneğin, yeniden boyutlandırılmış genişlik yüksekliğe daha az veya eşitse, Bing görüntüyü ilgi çeken bölgenin merkezinin solunda ve sağına kırpacaktır. Aksi takdirde, Bing ilgi bölgesinin merkezinin üst ve alt kırpma olacaktır.  
  
 
Aşağıda, Smart Ratio kırpma kullanılarak görüntü200x200'e düşürüldüğü gösterilmektedir. Bing görüntüyü sol üst köşeden ölçtüründen, görüntünün alt kısmı kırpılır. 
  
![200x200'e kırpılmış yatay görüntü](./media/resize-crop/bing-resize-crop-landscape200x200c7.png) 
  
Aşağıda, Smart Ratio kırpma kullanarak görüntünün 200x100'e düşürüldüğü gösterilmektedir. Bing görüntüyü sol üst köşeden ölçtüründen, görüntünün alt kısmı kırpılır. 
   
![200x100'e kırpılmış yatay görüntü](./media/resize-crop/bing-resize-crop-landscape200x100c7.png)
  
Aşağıda, Smart Ratio kırpma kullanarak görüntü100x200'e düşürüldüğü gösterilmektedir. Bing görüntüyü merkezden ölçtüründen, görüntünün sol ve sağ bölümleri kırpılır.
  
![100x200'e kırpılmış yatay görüntü](./media/resize-crop/bing-resize-crop-landscape100x200c7.png) 

Bing görüntünün ilgi çekici bölgesini belirleyemiyorsa, hizmet Kör Oran kırpma kullanır.  

### <a name="blind-ratio-cropping"></a>Kör Oran kırpma

Görme Oranı kırpma isteğiederseniz (parametreyi `c` `4`ayarlayarak), Bing görüntüyü kırpmak için aşağıdaki kuralları kullanır.  
  
- Eğer `(Original Image Width / Original Image Height) < (Requested Image Width / Requested Image Height)`, görüntü sol üst köşesinden ölçülür ve alt kısmında kırpılır.  
- Eğer `(Original Image Width / Original Image Height) > (Requested Image Width / Requested Image Height)`, görüntü merkezinden ölçülür ve solve sağa kırpılır.  

Aşağıdaki 225x300 bir portre görüntü gösterir.  
  
![Orijinal ayçiçeği görüntüsü](./media/resize-crop/bing-resize-crop-sunflower.png)
  
Aşağıdaki görüntü 200x200 Blind Ratio kırpma kullanarak azaltılmış gösterir. Görüntü sol üst köşeden ölçülür ve görüntünün alt kısmı kırpılır.  
  
![Ayçiçeği görüntü 200x200 kırpılmış](./media/resize-crop/bing-resize-crop-sunflower200x200c4.png)
  
Aşağıdaki görüntü 200x100 Blind Ratio kırpma kullanarak azaltılmış gösterir. Görüntü sol üst köşeden ölçülür ve görüntünün alt kısmı kırpılır.  
  
![Ayçiçeği görüntü 200x100 kırpılmış](./media/resize-crop/bing-resize-crop-sunflower200x100c4.png)
  
Aşağıdaki görüntü 100x200 Blind Ratio kırpma kullanarak azaltılmış gösterir. Görüntü, görüntünün sol ve sağ kısımlarının kırpılmasıyla sonuçlanan merkezden ölçülür.  
  
![Ayçiçeği görüntüsü 100x200 kırpılmış](./media/resize-crop/bing-resize-crop-sunflower100x200c4.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Bing Arama API'leri nedir?](bing-api-comparison.md)
* [Bing Arama API’si kullanım ve görüntüleme gereksinimleri](use-display-requirements.md)
