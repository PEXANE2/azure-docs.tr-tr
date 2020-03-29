---
title: 'K-Means Kümeleme: Modül Referans'
titleSuffix: Azure Machine Learning
description: Kümeleme modellerini eğitmek için Azure Machine Learning'deki K-Means Kümeleme modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/19/2020
ms.openlocfilehash: 9606768288cc74afc24491149eb471944f45e2dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921173"
---
# <a name="module-k-means-clustering"></a>Modül: K-Araçlar Kümeleme

Bu makalede, eğitimsiz bir K-araçları kümeleme modeli oluşturmak için Azure Machine Learning tasarımcısında (önizleme) *K-Means Kümeleme* modülünasıl kullanılacağı açıklanmaktadır. 
 
K-anlamına en basit ve en iyi bilinen *denetimsiz* öğrenme algoritmaları biridir. Algoritmayı aşağıdakiler gibi çeşitli makine öğrenimi görevleri için kullanabilirsiniz: 

* [Anormal verileri algılama.](https://msdn.microsoft.com/magazine/jj891054.aspx)
* Metin belgelerini kümeleme.
* Diğer sınıflandırma veya regresyon yöntemlerini kullanmadan önce veri kümelerini çözümleme. 

Kümeleme modeli oluşturmak için:

* Bu modülü boru hattınıza ekleyin.
* Bir veri kümesi bağlayın.
* Beklediğiniz küme sayısı, kümeleri oluştururken kullanılacak mesafe ölçümü ve benzeri parametreleri ayarlayın. 
  
Modül hiperparametrelerini yapılandırıldıktan sonra, eğitimsiz modeli [Tren Kümeleme Modeline](train-clustering-model.md)bağlarsınız. K-anlamına gelir algoritması denetimsiz bir öğrenme yöntemi olduğundan, etiket sütunu isteğe bağlıdır. 

+ Verileriniz bir etiket içeriyorsa, kümelerin seçimine rehberlik etmek ve modeli optimize etmek için etiket değerlerini kullanabilirsiniz. 

+ Verilerinizin etiketi yoksa, algoritma yalnızca verilere dayalı olarak olası kategorileri temsil eden kümeler oluşturur.  

##  <a name="understand-k-means-clustering"></a>K-ortalamakümele'yi anlama
 
Genel olarak, kümeleme, veri kümesindeki olguları benzer özelliklere sahip kümeler halinde gruplandırmak için yinelemeli teknikler kullanır. Bu gruplandırmalar verileri keşfetmek, verilerdeki anormallikleri tanımlamak ve sonunda öngörülerde bulunmak için yararlıdır. Kümeleme modelleri, bir veri kümesinde, tarama veya basit gözlem le mantıksal olarak elde elemeyebilirsiniz ilişkileri belirlemenize de yardımcı olabilir. Bu nedenlerden dolayı, kümeleme genellikle verileri keşfetmek ve beklenmeyen korelasyonları keşfetmek için makine öğrenimi görevlerinin ilk aşamalarında kullanılır.  
  
 Bir kümeleme modelini K-anlamına gelir yöntemini kullanarak yapılandırdığınızda, modelde istediğiniz *centroid* sayısını gösteren bir hedef *k* numarası belirtmeniz gerekir. Merkezoit her kümeyi temsil eden bir noktadır. K-, gelen her veri noktasını kümeiçindeki kare toplamını en aza indirerek kümelerden birine atar. 
 
Eğitim verilerini işlerken, K-anlamına gelir algoritması rastgele seçilen santriomların ilk kümesi ile başlar. Centroids kümeler için başlangıç noktası olarak hizmet ve tekrarlayıcı konumlarını rafine Lloyd algoritması geçerlidir. K-, bu koşullardan birini veya birkaçını karşıladığında küme oluşturmayı ve rafine etmeyi durdurur:  
  
-   Centroids stabilize, tek tek noktalar için küme atamaları artık değiştirmek ve algoritma bir çözüm üzerinde yakınsadı anlamına gelir.  
  
-   Algoritma, belirtilen yineleme sayısını çalıştırmayı tamamladı.  
  
 Eğitim aşamasını tamamladıktan sonra, K anlamına gelir algoritmasını kullanarak bulduğunuz kümelerden birine yeni servis talepleri atamak için [Kümelere Veri Atamasını](assign-data-to-clusters.md) kullanırsınız. Yeni servis talebi ile her kümenin merkeztaşı arasındaki mesafeyi hesaplayarak küme ataması yaparsınız. Her yeni durum en yakın centroid ile kümeye atanır.  

## <a name="configure-the-k-means-clustering-module"></a>K-Means Kümeleme modüllerini yapılandırma
  
1.  **K-Means Kümeleme** modüllerini ardınıza ekleyin.  
  
2.  Modelin nasıl eğitilmek istediğini belirtmek için **eğitmen modu oluştur** seçeneğini belirleyin.  
  
    -   **Tek Parametre**: Kümeleme modelinde tam olarak kullanmak istediğiniz parametreleri biliyorsanız, bağımsız değişken olarak belirli bir değer kümesi sağlayabilirsiniz.  
  
3.  **Centroid sayısı**için, algoritmanın başlamasını istediğiniz küme sayısını yazın.  
  
     Modelin tam olarak bu sayıda küme üretmesi garanti değildir. Algoritma bu veri noktası sayısıyla başlar ve en iyi yapılandırmayı bulmak için yineler.  
  
4.  Özellikler **Başlangıç,** ilk küme yapılandırmasını tanımlamak için kullanılan algoritmayı belirtmek için kullanılır.  
  
    -   **First N**: Veri kümesinden bazı başlangıç veri noktaları seçilir ve başlangıç aracı olarak kullanılır. 
    
         Bu yöntem aynı zamanda *Forgy yöntemi*olarak adlandırılır.  
  
    -   **Rastgele**: Algoritma bir kümeye rastgele bir veri noktası yerleştirir ve daha sonra kümenin rasgele atanan noktalarının merkezci storoid imiş gibi ilk ortalamayı hesaplar. 

         Bu yöntem, *rasgele bölümleme* yöntemi olarak da adlandırılır.  
  
    -   **K-Means++**: Kümeleri başlatma için varsayılan yöntemdir.  
  
         **K-means++** algoritması 2007 yılında David Arthur ve Sergei Vassilvitskii tarafından standart K-araçları algoritması tarafından kötü kümelemayı önlemek için önerilmiştir. **K-means++** ilk küme merkezlerini seçmek için farklı bir yöntem kullanarak standart K-araçlarını geliştirir.  
  
    
5.  **Rasgele sayı tohumu**için isteğe bağlı olarak küme başlatma için tohum olarak kullanılacak bir değer yazın. Bu değer küme seçimi üzerinde önemli bir etkisi olabilir.  
  
6.  **Metrik**için, küme vektörleri arasındaki mesafeyi ölçmek için veya yeni veri noktaları ile rasgele seçilen merkeztaşı arasındaki işlevi seçin. Azure Machine Learning aşağıdaki küme mesafesi ölçümlerini destekler:  
  
    -   **Öklid**: Öklid uzaklığı genellikle K-araçları kümeleme için küme saçılımı nın bir ölçüsü olarak kullanılır. Bu metrik, noktalar ve merkezler arasındaki ortalama mesafeyi en aza indirdiği için tercih edilir.
  
7.  Yinelemeler için, **merkeztaşı**seçimini tamamlamadan önce algoritmanın eğitim verilerini yineleme sayının sayısını yazın.  
  
     Bu parametreyi, doğruluğu eğitim süresine göre dengelemek için ayarlayabilirsiniz.  
  
8.  **Etiket atarken,** etiket sütununun veri kümesinde varsa nasıl işleneceğini belirten bir seçenek seçin.  
  
     K-ortalama kümeleme denetimsiz bir makine öğrenme yöntemi olduğundan, etiketler isteğe bağlıdır. Ancak, veri kümenizde zaten bir etiket sütunu varsa, kümelerin seçimine rehberlik etmek için bu değerleri kullanabilirsiniz veya değerlerin yoksayıldığını belirtebilirsiniz.  
  
    -   **Etiket sütununa yoksay**: Etiket sütunundaki değerler yoksayılır ve modelin oluşturulmasında kullanılmaz.
  
    -   **Eksik değerleri doldurun**: Etiket sütun değerleri kümelerin oluşturulmasına yardımcı olmak için özellik olarak kullanılır. Herhangi bir satırda bir etiket eksikse, değer diğer özellikler kullanılarak belirlenir.  
  
    -   **En yakın merkezden overwrite**: Etiket sütun değerleri, geçerli centroid'e en yakın noktanın etiketi kullanılarak öngörülen etiket değerleri ile değiştirilir.  

8.  Eğitimden önce özellikleri normalleştirmek istiyorsanız **Normalleştir özelliklerini** seçin.
  
     Normalleştirme uygularsanız, eğitimden önce, veri noktaları `[0,1]` MinMaxNormalizer tarafından normale döndürülmez.

10. Modeli eğitin.  
  
    -   Tek **Parametre** **için oluştur eğitmen modunu** ayarlarsanız, etiketli bir veri seti ekleyin ve [Tren Kümeleme Modeli](train-clustering-model.md) modüllerini kullanarak modeli train' i çalıştırın.  
  
## <a name="results"></a>Sonuçlar

Modeli yapılandırmayı ve eğitmeyi bitirdikten sonra, puan oluşturmak için kullanabileceğiniz bir modele sahip olursunuz. Ancak, modeli eğitmenin birden çok yolu ve sonuçları görüntülemenin ve kullanmanın birden çok yolu vardır: 

### <a name="capture-a-snapshot-of-the-model-in-your-workspace"></a>Çalışma alanınızda modelin anlık görüntüsünü yakalama

[Tren Kümeleme Modeli](train-clustering-model.md) modüllerini kullandıysanız:

1. Tren **Kümeleme Modeli** modülünü seçin ve doğru paneli açın.

2. **Çıktılar** sekmesini seçin. Eğitilmiş modelin bir kopyasını kaydetmek için **Kayıt veri kümesi** simgesini seçin.

Kaydedilen model, modeli kaydettiğiniz anda eğitim verilerini temsil eder. Daha sonra ardışık hatlar üzerinde kullanılan eğitim verilerini güncellerseniz, kaydedilen modeli güncelleştirmez. 

### <a name="see-the-clustering-result-dataset"></a>Kümeleme sonucu veri kümesine bakın 

[Tren Kümeleme Modeli](train-clustering-model.md) modüllerini kullandıysanız:

1. **Tren Kümeleme Modeli** modülüne sağ tıklayın.

2. **Visualize'ı**seçin.

### <a name="tips-for-generating-the-best-clustering-model"></a>En iyi kümeleme modelini oluşturmak için ipuçları  

Kümeleme sırasında kullanılan *tohumlama* işleminin modeli önemli ölçüde etkileyebileceği bilinmektedir. Tohumlama potansiyel santriomlar içine puan ilk yerleştirme anlamına gelir.
 
Örneğin, veri kümesi çok sayıda aykırılık içeriyorsa ve kümeleri tohumlamak için bir aykırı seçiliyorsa, başka hiçbir veri noktası bu kümeye tam olarak uymaz ve küme tek ton olabilir. Yani, sadece bir nokta olabilir.  
  
Bu sorunu birkaç şekilde önleyebilirsiniz:  
  
-   Merkeztaşı sayısını değiştirin ve birden çok tohum değerini deneyin.  
  
-   Metrik veya daha fazla yineleyen değişen birden çok model oluşturun.  
  
Genel olarak, kümeleme modelleri ile, belirli bir yapılandırma yerel olarak optimize edilmiş kümeler kümesi neden olabilir. Başka bir deyişle, model tarafından döndürülen kümeler kümesi yalnızca geçerli veri noktalarına uygundur ve diğer veriler için genelleştirilebilir değildir. Farklı bir ilk yapılandırma kullanırsanız, K-means yöntemi farklı, üstün, yapılandırma bulabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
