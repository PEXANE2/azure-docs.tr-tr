---
title: Sorun giderme modülü hataları
titleSuffix: Azure Machine Learning
description: Hata kodlarını kullanarak Azure Machine Learning tasarımcısında sorun giderme modülü özel durumları
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 12/03/2019
ms.openlocfilehash: cda499b81a61a5b78ca86a96372640e368f90357
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80364192"
---
# <a name="exceptions-and-error-codes-for-the-designer-preview"></a>Tasarımcı için özel durumlar ve hata kodları (önizleme)

Bu makalede, makine öğrenimi ardışık alanlarınıza sorun çıkarmanıza yardımcı olmak için Azure Machine Learning tasarımcısındaki (önizleme) hata iletileri ve özel durum kodları açıklanmaktadır.

Tasarımcıda bir hata iletisinin tam metnini almanın iki yolu vardır:  

- Bağlantıyı tıklatın, **Çıkış Günlüğü görüntüle**, sağ bölmede ve alta kaydırın. Ayrıntılı hata iletisi pencerenin son iki satırında görüntülenir.  
  
- Hataya sahip modülü seçin ve kırmızı X'i tıklatın. Yalnızca ilgili hata metni görüntülenir.

## <a name="error-0001"></a>Hata 0001  
 Veri kümesinin bir veya daha fazla belirtilen sütunu bulunamazsa özel durum oluşur.  

 Bir modül için sütun seçimi yapılırsa, ancak seçili sütun(lar) giriş veri kümesinde yoksa bu hatayı alırsınız. Bu hata, bir sütun adını el ile yazdıysanız veya sütun seçici, ardışık lığı çalıştırdığınızda veri kümenizde bulunmayan önerilen bir sütun sağlamışsa oluşabilir.  

**Çözünürlük:** Bu özel durumu atan modülü yeniden ziyaret edin ve sütun adının veya adlarının doğru olduğunu ve başvurulan tüm sütunların var olduğunu doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Belirtilen sütunlardan biri veya daha fazlası bulunamadı.|
|Adı veya dizin "{column_id}" ile sütun bulunamadı.|
|"{arg_name_missing_column}" adlı veya dizinli sütun "{column_id arg_name_missing_column}" içinde yok.|
|"{column_id arg_name_missing_column}" adlı ad veya dizin içeren sütun, "{arg_name_has_column}" sözcüklerinde bulunmaz, "{arg_name_has_column}" içinde bulunur.|
|Adı veya dizini "{column_names}" olan sütunlar bulunamadı.|
|"{column_names}" adı veya dizini olan sütunlar "{arg_name_missing_column}" içinde yok.|
|"column_names{arg_name_missing_column}" adı veya dizini olan sütunlar "{arg_name_missing_column}" içinde bulunmaz, "{arg_name_has_column}" içinde bulunur.|


## <a name="error-0002"></a>Hata 0002  
 Bir veya daha fazla parametre nin ayrıştırılamadığını veya hedef yöntem türü tarafından gerekli olan belirtilen türden dönüştürülememesi durumunda özel durum oluşur.  

 Bu hata, bir parametreyi giriş olarak belirttiğiniz ve değer türü beklenen türden farklı olduğunda Azure Machine Learning'de oluşur ve örtülü dönüştürme gerçekleştirilemez.  

**Çözünürlük:** Modül gereksinimlerini kontrol edin ve hangi değer türünün gerekli olduğunu belirleyin (dize, tümsek, çift, vb.)  

|Özel Durum Mesajları|
|------------------------|
|Parametreyi ayrışdırmak için başarısız oldu.|
|"{arg_name_or_column}" parametresini ayrıştırmak için başarısız oldu.|
|"{arg_name_or_column}" parametresini "{to_type}" olarak dönüştürmede başarısız oldu.|
|"{arg_name_or_column}" parametresini "{from_type}" den "{to_type}" olarak dönüştürmeyi başaramadı.|
|"{arg_name_or_column}" parametre değerini "{arg_value}" den "{from_type}" den "{to_type}" olarak dönüştürmeyi başaramadı.|
|"{fmt}" biçiminde "{fmt}" kullanımı yla "arg_value{from_type}" sütunundaki "{arg_name_or_column}" değerini "{from_type}" ile "{to_type}" olarak dönüştürmeyi başaramadı.|


## <a name="error-0003"></a>Hata 0003  
 Bir veya daha fazla giriş boş veya boşsa özel durum oluşur.  

 Bir modüle giriş veya parametreler boş sayılsa veya boşsa, bu hatayı Azure Machine Learning'de alırsınız.  Bu hata, örneğin, bir parametre için herhangi bir değer yazmadığınızda oluşabilir. Eksik değerlere sahip bir veri kümesi veya boş bir veri kümesi seçtiyseniz de olabilir.  

**Çözünürlük:**

+ Özel durumu oluşturan modülü açın ve tüm girişlerin belirtildiğini doğrulayın. Gerekli tüm girişlerin belirtildiğinden emin olun. 
+ Azure depolama dan yüklenen verilere erişilebilir olduğundan ve hesap adının veya anahtarının değişmediğinden emin olun.  
+ Eksik değerler veya nulls için giriş verilerini denetleyin.
+ Bir veri kaynağında sorgu kullanıyorsanız, verilerin beklediğiniz biçimde döndürüldettiğini doğrulayın. 
+ Veri belirtiminde yazım hataları veya diğer değişiklikleri denetleyin.
  
|Özel Durum Mesajları|
|------------------------|
|Girişlerden biri veya birkaçı boş veya boştur.|
|Giriş "{name}" null veya boş.|


## <a name="error-0004"></a>Hata 0004  
 Parametre belirli değerden küçük veya eşitse özel durum oluşur.  

 İletideki parametre, modülün verileri işlemesi için gereken sınır değerinin altındaysa, bu hatayı Azure Machine Learning'de alırsınız.  

**Çözünürlük:** Özel durum atan modülü yeniden ziyaret edin ve parametreyi belirtilen değerden daha büyük olacak şekilde değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Parametre sınır değerinden büyük olmalıdır.|
|Parametre "{arg_name}" değeri {lower_boundary}'den büyük olmalıdır.|
|Parametre "{arg_name}" değeri "{actual_value}" olan {lower_boundary}'den büyük olmalıdır.|


## <a name="error-0005"></a>Hata 0005  
 Parametre belirli bir değerden küçükse özel durum oluşur.  

 İletideki parametre, modülün verileri işlemesi için gereken sınır değerine eşit sayılsa veya bu hatayı Azure Machine Learning'de alırsınız.  

**Çözünürlük:** Özel durum atan modülü yeniden ziyaret edin ve parametreyi belirtilen değerden büyük veya eşit olacak şekilde değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Parametre, sınır değerinden büyük veya eşit olmalıdır.|
|Parametre "{arg_name}" değeri {lower_boundary}'den büyük veya eşit olmalıdır.|
|Parametre "{arg_name}" değeri "{değer}" daha büyük veya eşit olmalıdır {lower_boundary}.|


## <a name="error-0006"></a>Hata 0006  
 Parametre belirtilen değerden büyük veya eşitse özel durum oluşur.  

 İletideki parametre, modülün verileri işlemesi için gereken sınır değerinden büyük veya eşitse, bu hatayı Azure Machine Learning'de alırsınız.  

**Çözünürlük:** Özel durum atan modülü yeniden ziyaret edin ve parametreyi belirtilen değerden daha az olacak şekilde değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Parametreler uyuşmazlığı. Parametrelerden biri diğerinden daha az olmalıdır.|
|Parametre "{arg_name}" değeri "{upper_boundary_parameter_name}" değerinden daha az olmalıdır.|
|Parametre "{arg_name}" değeri "{değer}" olan {upper_boundary_parameter_name}'den az olmalıdır.|


## <a name="error-0007"></a>Hata 0007  
 Parametre belirli bir değerden büyükse özel durum oluşur.  

 Modülün özelliklerinde izin verilenden daha büyük bir değer belirtirseniz, bu hatayı Azure Machine Learning'de alırsınız. Örneğin, desteklenen tarih aralığının dışında bir veri belirtebilir veya yalnızca üç sütun kullanılabilir olduğunda beş sütunun kullanılacağını belirtebilirsiniz. 

 Bir şekilde eşleşmesi gereken iki veri kümesi belirliyorsanız, bu hatayı da görebilirsiniz. Örneğin, sütunları yeniden adlandırırsanız ve sütunları dizinolarak belirtirseniz, sağladığınız ad sayısı sütun dizilerinin sayısıyla eşleşmelidir. Başka bir örnek, sütunların aynı sayıda satıra sahip olması gereken iki sütun kullanan bir matematik işlemi olabilir. 

**Çözünürlük:**

 + Söz konusu modülü açın ve sayısal özellik ayarlarını gözden geçirin.
 + Parametre değerlerinin o özellik için desteklenen değerler aralığına düştüğünden emin olun.
 + Modül birden çok giriş alıyorsa, girişlerin aynı boyutta olduğundan emin olun.
<!-- + If the module has multiple properties that can be set, ensure that related properties have appropriate values. For example, when using [Group Data into Bins](group-data-into-bins.md), if you use the option to specify custom bin edges, the number of bins must match the number of values you provide as bin boundaries.-->
 + Veri kümesinin veya veri kaynağının değişip değişmediğini denetleyin. Bazen, sütun sayısı, sütun veri türleri veya verilerin boyutu değiştikten sonra, verilerin önceki sürümüyle çalışan bir değer başarısız olur.  

|Özel durum iletileri|
|------------------------|
|Parametreler uyuşmazlığı. Parametrelerden biri diğerinden daha az veya eşit olmalıdır.|
|Parametre "{arg_name}" değeri "{upper_boundary_parameter_name}" değerinden daha az veya eşit olmalıdır.|
|Parametre "{arg_name}" değeri "{actual_value}" daha az veya eşit olmalıdır {upper_boundary}.|
|Parametre "{arg_name}" değeri {actual_value} parametre "{upper_boundary_parameter_name}" değeri {upper_boundary}'den az veya eşit olmalıdır.|


## <a name="error-0008"></a>Hata 0008  
 Parametre aralıkta değilse özel durum oluşur.  

 İletideki parametre, modülün verileri işlemesi için gereken sınırların dışındaysa, bu hatayı Azure Machine Learning'de alırsınız.  

 Örneğin, farklı sayıda sütuna sahip iki veri kümesini birleştirmek için [Satır Ekle'yi](add-rows.md) kullanmaya çalışırsanız bu hata görüntülenir.  

**Çözünürlük:** Özel durum atan modülü yeniden ziyaret edin ve parametreyi belirtilen aralıkta olacak şekilde değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Parametre değeri belirtilen aralıkta değil.|
|Parametre "{arg_name}" değeri aralıkta değildir.|
|Parametre "{arg_name}" değeri [{lower_boundary}, {upper_boundary}] aralığında olmalıdır.|
|Parametre "{arg_name}" değeri aralıkta değildir. {neden}|


## <a name="error-0009"></a>Hata 0009  
 Özel durum, Azure depolama hesabı adı veya kapsayıcı adı yanlış belirtildiğinde oluşur.  

Bu hata, bir Azure depolama hesabının parametrelerini belirttiğiniz zaman Azure Machine Learning tasarımcısında oluşur, ancak ad veya parola çözümlenemez. Parolalarda veya hesap adlarında hatalar birçok nedenden dolayı olabilir:

 + Hesap yanlış türüdür. Bazı yeni hesap türleri Machine Learning tasarımcısı ile kullanım için desteklenmez. Ayrıntılar için [İçe Aktar Verileri'ne](import-data.md) bakın.
 + Yanlış hesap adını girdiniz
 + Hesap artık yok
 + Depolama hesabının parolası yanlış veya değiştirilmiş
 + Kapsayıcı adını belirtmediniz veya kapsayıcı yok
 + Dosya yolunu tam olarak belirtmedin (blob'a giden yol)
   

**Çözünürlük:**

Bu tür sorunlar genellikle hesap adını, parolayı veya kapsayıcı yolunu el ile girmeye çalıştığınızda oluşur. Adları aramanıza ve kontrol edersiniz yardımcı olan [İçe Aktar Veri](import-data.md) modülü için yeni sihirbazı kullanmanızı öneririz.

Ayrıca hesap, kapsayıcı veya blob silinmiş olup olmadığını kontrol edin. Hesap adının ve parolasının doğru girildiğini ve kapsayıcının var olduğunu doğrulamak için başka bir Azure depolama yardımcı programı kullanın. 

Bazı yeni hesap türleri Azure Machine Learning tarafından desteklenmez. Örneğin, yeni "sıcak" veya "soğuk" depolama türleri makine öğrenimi için kullanılamaz. Hem klasik depolama hesapları hem de "Genel amaç" olarak oluşturulan depolama hesapları iyi çalışır.

Blob'a giden tam yol belirtilmişse, yolun **kapsayıcı/blobname**olarak belirtildiğini ve hesapta hem kapsayıcının hem de blob'un bulunduğunu doğrulayın.  

 Yol, önde gelen bir eğik çizgi içermemelidir. Örneğin **/container/blob** yanlıştır ve **kapsayıcı/blob**olarak girilmelidir.  


|Özel Durum Mesajları|
|------------------------|
|Azure depolama hesabı adı veya kapsayıcı adı yanlıştır.|
|Azure depolama hesabı adı "{account_name}" veya kapsayıcı adı "{container_name}" yanlıştır; biçim kapsayıcı /blob bir kapsayıcı adı bekleniyordu.|


## <a name="error-0010"></a>Hata 0010  
 Giriş veri kümelerinde eşleşmesi gereken ancak eşleşmemesi gereken sütun adları varsa özel durum oluşur.  

 İletideki sütun dizininde iki giriş veri kümesinde farklı sütun adları varsa, bu hatayı Azure Machine Learning'de alırsınız.  

**Çözünürlük:** [Meta verileri edit'i](edit-metadata.md) kullanın veya belirtilen sütun dizini için aynı sütun adı olacak şekilde özgün veri kümesini değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Giriş veri kümelerinde karşılık gelen dizin içeren sütunların farklı adları vardır.|
|Sütun adları, giriş veri kümelerinin ({dataset1} ve {dataset2} sırasıyla) sütun {col_index} (sıfır tabanlı) sütunu için aynı değildir.|


## <a name="error-0011"></a>Hata 0011  
 Geçirilen sütun kümesi bağımsız değişkeni veri kümesi sütunlarından herhangi biri için geçerli değilse özel durum oluşur.  

 Belirtilen sütun seçimi, verilen veri kümesindeki sütunların hiçbiriyle eşleşmiyorsa, bu hatayı Azure Machine Learning'de alırsınız.  

 Bir sütun seçmediyseniz ve modülün çalışması için en az bir sütun gerekiyorsa da bu hatayı alabilirsiniz.  

**Çözünürlük:** Modüldeki sütun seçimini, veri kümesindeki sütunlara uygulanacak şekilde değiştirin.  

 Modül, etiket sütunu gibi belirli bir sütun seçmenizi gerektiriyorsa, sağ sütunun seçilip seçilmediğini doğrulayın.  

 Uygun olmayan sütunlar seçilirse, bunları kaldırın ve ardışık hattı yeniden çalıştırın.  

|Özel Durum Mesajları|
|------------------------|
|Belirtilen sütun kümesi veri kümesi sütunlarının hiçbiri için geçerli değildir.|
|Belirtilen sütun kümesi "{column_set}" veri kümesi sütunlarının hiçbiri için geçerli değildir.|


## <a name="error-0012"></a>Hata 0012  
 Bağımsız değişkenler kümesi geçti ile sınıf örneği oluşturulamadı ise özel durum oluşur.  

**Çözünürlük:** Bu hata kullanıcı tarafından işlem dengeçirilemez ve gelecekteki bir sürümde amortismana tabi tutulacaktır.  

|Özel Durum Mesajları|
|------------------------|
|Eğitimsiz model, lütfen önce model eğitin.|
|Eğitimsiz model ({arg_name}), eğitimli modeli kullanın.|


## <a name="error-0013"></a>Hata 0013  
 Modüle geçen öğrenci geçersiz bir türse özel durum oluşur.  

 Bu hata, eğitimli bir model bağlı puanlama modülü ile uyumsuz olduğunda oluşur. <!--For example, connecting the output of [Train Matchbox Recommender](train-matchbox-recommender.md) to [Score Model](score-model.md) (instead of [Score Matchbox Recommender](score-matchbox-recommender.md)) will generate this error when the pipeline is run.  -->

**Çözünürlük:**

Eğitim modülü tarafından üretilen öğrenci türünü belirleyin ve öğrenci için uygun puanlama modüllerini belirleyin. 

Model, özel eğitim modüllerinden herhangi biri kullanılarak eğitildiyse, eğitimli modeli yalnızca ilgili özel puanlama modülüne bağlayın. 


|Model türü|Eğitim modülü| Puanlama modülü|
|----|----|----|
|herhangi bir sınıflandırıcı|[Model Eğitme](train-model.md) |[Modeli Puanlama](score-model.md)|
|herhangi bir regresyon modeli|[Model Eğitme](train-model.md) |[Modeli Puanlama](score-model.md)|

<!--| clustering models| [Train Clustering Model](train-clustering-model.md) or [Sweep Clustering](sweep-clustering.md)| [Assign Data to Clusters](assign-data-to-clusters.md)|
| anomaly detection - One-Class SVM | [Train Anomaly Detection Model](train-anomaly-detection-model.md) |[Score Model](score-model.md)|
| anomaly detection - PCA |[Train Model](train-model.md) |[Score Model](score-model.md) </br> Some additional steps are required to evaluate the model. |
| anomaly detection - time series|  [Time Series Anomaly Detection](time-series-anomaly-detection.md) |Model trains from data and generates scores. The module does not create a trained learner and no additional scoring is required. |
| recommendation model| [Train Matchbox Recommender](train-matchbox-recommender.md) | [Score Matchbox Recommender](score-matchbox-recommender.md) |
| image classification | [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md) | [Score Model](score-model.md) |
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) | [Score Vowpal Wabbit Version 7-4 Model](score-vowpal-wabbit-version-7-4-model.md) |   
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 7-10 Model](train-vowpal-wabbit-version-7-10-model.md) | [Score Vowpal Wabbit Version 7-10 Model](score-vowpal-wabbit-version-7-10-model.md) |
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 8 Model](score-vowpal-wabbit-version-8-model.md) | [Score Vowpal Wabbit Version 8 Model](score-vowpal-wabbit-version-8-model.md) |-->

|Özel Durum Mesajları|
|------------------------|
|Geçersiz türünden bir öğrenci geçirilir.|
|Learner "{arg_name}" geçersiz türü vardır.|
|Learner "{arg_name}" geçersiz türü "{learner_type}" vardır.|


## <a name="error-0014"></a>Hata 0014  
 Özel durum, sütun benzersiz değerlerin sayısı izin verilenden büyükse oluşur.  

 Bu hata, bir sütun çok fazla benzersiz değer içerdiğinde oluşur.  Örneğin, bir sütunun kategorik veri olarak işleneceğini belirtirseniz, ancak sütunda işlemenin tamamlanmasına izin veremeyecek kadar çok benzersiz değer olduğunu belirtirseniz, bu hatayı görebilirsiniz. İki girişteki benzersiz değer sayısı arasında bir uyumsuzluk varsa, bu hatayı da görebilirsiniz.   

**Çözünürlük:**

Hatayı oluşturan modülü açın ve giriş olarak kullanılan sütunları tanımlayın. Bazı modüller için veri kümesi girişine sağ tıklayabilir ve benzersiz değerlerin sayısı ve bunların dağıtımı da dahil olmak üzere tek tek sütunlarla ilgili istatistikleri almak için **Visualize'ı** seçebilirsiniz.

Gruplandırma veya kategorilere ayırma için kullanmayı planladığınız sütunlar için, sütunlarda benzersiz değer sayısını azaltmak için adımlar atın. Sütunun veri türüne bağlı olarak farklı şekillerde azaltabilirsiniz. 
<!--
+ For text data, you might be able to use [Preprocess Text](preprocess-text.md) to collapse similar entries. 
+ For numeric data, you can create a smaller number of bins using [Group Data into Bins](group-data-into-bins.md), remove or truncate values using [Clip Values](clip-values.md), or use machine learning methods such as [Principal Component Analysis](principal-component-analysis.md) or [Learning with Counts](data-transformation-learning-with-counts.md) to reduce the dimensionality of the data.  
-->
> [!TIP]
> Senaryonuzla eşleşen bir çözüm bulamıyor musunuz? Hatayı oluşturan modülün adını ve sütunun veri türünü ve kardinalliğini içeren bu konu hakkında geri bildirim sağlayabilirsiniz. Bu bilgileri, yaygın senaryolar için daha hedefli sorun giderme adımları sağlamak için kullanırız.   

|Özel Durum Mesajları|
|------------------------|
|Sütun benzersiz değerlerinin miktarı izin verilenden daha büyüktür.|
|Sütundaki benzersiz değer sayısı: "{column_name}" izin verilenden daha büyüktür.|
|Sütundaki benzersiz değer sayısı: "{column_name}" {limitation} tuple sayısını aşıyor.|


## <a name="error-0015"></a>Hata 0015  
 Veritabanı bağlantısı başarısız olduysa özel durum oluşur.  

 Yanlış bir SQL hesap adı, parola, veritabanı sunucusu veya veritabanı adı girerseniz veya veritabanı veya sunucuyla ilgili sorunlar nedeniyle veritabanıyla bağlantı kurulamazsa bu hatayı alırsınız.  

**Çözünürlük:** Hesap adının, parolanın, veritabanı sunucusunun ve veritabanının doğru girildiğini ve belirtilen hesabın doğru izin düzeyine sahip olduğunu doğrulayın. Veritabanının şu anda erişilebilir olduğunu doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Veritabanı bağlantısı yapma hatası.|
|Veritabanı bağlantısı yapma hatası: {connection_str}.|


## <a name="error-0016"></a>Hata 0016  
 Modüle geçirilen giriş veri kümelerinin uyumlu sütun türlerine sahip olması ancak olmaması durumunda özel durum oluşur.  

 İki veya daha fazla veri kümesinde geçirilen sütun türleri birbiriyle uyumlu değilse, bu hatayı Azure Machine Learning'de alırsınız.  

**Çözünürlük:** [Meta verileri edit'i](edit-metadata.md) kullanın veya özgün giriş veri kümesini değiştirin<!--, or use [Convert to Dataset](convert-to-dataset.md)--> sütun türlerinin uyumlu olduğundan emin olmak için kullanılır.  

|Özel Durum Mesajları|
|------------------------|
|Giriş veri kümelerinde karşılık gelen dizin içeren sütunların uyumsuz türleri vardır.|
|'{first_col_names}' sütunları tren ve test verileri arasında uyumsuzdur.|
|'{first_col_names}' ve '{second_col_names}' sütunları uyumsuzdur.|
|Sütun öğesi türleri, giriş veri kümelerinin (sırasıyla {first_dataset_names} ve {second_dataset_names} sütunu '{first_col_names}' (sıfır tabanlı) için uyumlu değildir.|


## <a name="error-0017"></a>Hata 0017  
 Seçili bir sütun geçerli modül tarafından desteklenmeyen bir veri türü kullanırsa özel durum oluşur.  

 Örneğin, sütun seçiminiz, matematik işlemi için dize sütunu veya kategorik özellik sütunu olan bir puan sütunu gibi modül tarafından işlenemeyen bir veri türüne sahip bir sütun içeriyorsa, Azure Machine Learning'de bu hatayı alabilirsiniz Gerekli.  

**Çözünürlük:**
 1. Sorun olan sütunu tanımlayın.
 2. Modülün gereksinimlerini gözden geçirin.
 3. Gereksinimlerine uygun hale getirmek için sütunu değiştirin. Sütuna ve denediğiniz dönüşüme bağlı olarak değişiklik yapmak için aşağıdaki modüllerden birkaçını kullanmanız gerekebilir:
    + Sütunların veri türünü değiştirmek veya sütun kullanımını özellikten sayısal, kategorik ten kategorik olarak vb. değiştirmek için [Meta Verileri Edit'i](edit-metadata.md) kullanın.
<!--    + Use [Convert to Dataset](convert-to-dataset.md) to ensure that all included columns use data types that are supported by Azure Machine Learning.  If you cannot convert the columns, consider removing them from the input dataset.
    + Use the [Apply SQL Transformation](apply-sql-transformation.md) or [Execute R Script](execute-r-script.md) modules to cast or convert any columns that cannot be modified using [Edit Metadata](edit-metadata.md). These modules provide more flexibility for working with datetime data types.
    + For numeric data types, you can use the [Apply Math Operation](apply-math-operation.md) module to round or truncate values, or use the [Clip Values](clip-values.md) module to remove out of range values.  -->
 4. Son çare olarak, özgün giriş veri kümesini değiştirmeniz gerekebilir.

> [!TIP]
> Senaryonuzla eşleşen bir çözüm bulamıyor musunuz? Hatayı oluşturan modülün adını ve sütunun veri türünü ve kardinalliğini içeren bu konu hakkında geri bildirim sağlayabilirsiniz. Bu bilgileri, yaygın senaryolar için daha hedefli sorun giderme adımları sağlamak için kullanırız. 

|Özel Durum Mesajları|
|------------------------|
|Geçerli türsütunu işleyemez. Tür modül tarafından desteklenmez.|
|{col_type} türü sütunu işleyemez. Tür modül tarafından desteklenmez.|
|{col_type} türünden "{col_name}" sütunu işlenemez. Tür modül tarafından desteklenmez.|
|{col_type} türünden "{col_name}" sütunu işlenemez. Tür modül tarafından desteklenmez. Parametre adı: {arg_name}.|


## <a name="error-0018"></a>Hata 0018  
 Giriş veri kümesi geçerli değilse özel durum oluşur.  

**Çözünürlük:** Azure Machine Learning'deki bu hata birçok bağlamda görünebilir, bu nedenle tek bir çözüm yoktur. Genel olarak, hata, bir modüle giriş olarak sağlanan verilerin yanlış sayıda sütuna sahip olduğunu veya veri türünün modülün gereksinimleriyle eşleşmediğini gösterir. Örnek:  

-   Modül bir etiket sütunu gerektirir, ancak hiçbir sütun etiket olarak işaretlenmemiştir veya henüz bir etiket sütunu seçmemiş siniz.  
  
-   Modül, verilerin kategorik olmasını gerektirir, ancak verileriniz sayısaldır.  

<!---   The module requires a specific data type. For example, ratings provided to [Train Matchbox Recommender](train-matchbox-recommender.md) can be either numeric or categorical, but cannot be floating point numbers.  -->

-   Veriler yanlış biçimde.  
  
-   İçe aktarılan veriler geçersiz karakterler, kötü değerler veya aralık dışı değerler içerir.  
-   Sütun boş veya çok fazla eksik değer içeriyor.  

 Gereksinimleri ve verilerinizin nasıl olabileceğini belirlemek için, veri kümesini giriş olarak tüketecek olan modülün yardım konusunu gözden geçirin.  

 <!--We also recommend that you use [Summarize Data](summarize-data.md) or [Compute Elementary Statistics](compute-elementary-statistics.md) to profile your data, and use these modules to fix metadata and clean values: [Edit Metadata](edit-metadata.md) and [Clean Missing Data](clean-missing-data.md), [Clip Values](clip-values.md)-->.  

|Özel Durum Mesajları|
|------------------------|
|Dataset geçerli değil.|
|{dataset1} geçersiz veri içerir.|
|{dataset1} ve {dataset2} sütun açısından tutarlı olmalıdır.|
|{dataset1} geçersiz veri içerir, {neden}.|
|{dataset1} {invalid_data_category} içerir. {troubleshoot_hint}|
|{dataset1} geçerli değil, {neden}. {troubleshoot_hint}|


## <a name="error-0019"></a>Hata 0019  
 Özel durum, sütunun sıralanmış değerler içermesi bekleniyorsa oluşur, ancak içermez.  

 Belirtilen sütun değerleri bozuksa, bu hatayı Azure Machine Learning'de alırsınız.  

**Çözünürlük:** Giriş veri kümesini el ile değiştirerek sütun değerlerini sıralayın ve modülü yeniden çalıştırın.  

|Özel Durum Mesajları|
|------------------------|
|Sütundaki değerler sıralanmaz.|
|"{col_index}" sütunundaki değerler sıralanmaz.|
|Veri kümesi "col_index{dataset}" sütunundaki değerler sıralanmaz.|


## <a name="error-0020"></a>Hata 0020  
 Modüle geçirilen bazı veri kümelerinde sütun sayısı çok küçükse özel durum oluşur.  

 Bir modül için yeterli sütun seçilmemişse, bu hatayı Azure Machine Learning'de alırsınız.  

**Çözünürlük:** Modülü yeniden ziyaret edin ve sütun seçicinin seçilen sütun sayısının doğru olduğundan emin olun.  

|Özel Durum Mesajları|
|------------------------|
|Giriş veri kümesindeki sütun sayısı izin verilen minimumdan daha azdır.|
|"{arg_name}" giriş veri kümesindeki sütun sayısı izin verilen minimumdan daha azdır.|
|Giriş veri kümesindeki sütun sayısı , {required_columns_count} sütun(lar) için izin verilen minimumsütundan daha azdır.|
|Giriş veri kümesindeki sütun sayısı "{arg_name}" {required_columns_count} sütun(lar) için izin verilen minimumdan daha azdır.|


## <a name="error-0021"></a>Hata 0021  
 Modüle geçirilen bazı veri kümelerinde satır sayısı çok küçükse özel durum oluşur.  

 Veri kümesinde belirtilen işlemi gerçekleştirmek için yeterli satır olmadığında Azure Machine Learning'de görülen bu hata. Örneğin, giriş veri kümesi boşsa veya bazı minimum satır sayısının geçerli olmasını gerektiren bir işlem gerçekleştirmeye çalışıyorsanız bu hatayı görebilirsiniz. Bu tür işlemler, istatistiksel yöntemlere, belirli türde binning'e ve sayımlarla öğrenmeyi temel alan gruplandırma yı veya sınıflandırmayı içerebilir (ancak bunlarla sınırlı değildir).  

**Çözünürlük:**

 + Hatayı döndüren modülü açın ve giriş veri kümesini ve modül özelliklerini denetleyin. 
 + Giriş veri kümesinin boş olmadığını ve modül yardımında açıklanan gereksinimleri karşılamak için yeterli veri satırı olduğunu doğrulayın.  
 + Verileriniz harici bir kaynaktan yükleniyorsa, veri kaynağının kullanılabilir olduğundan ve veri tanımında alma işleminin daha az satır alabilecek bir hata veya değişiklik olmadığından emin olun.
 + Temizleme, bölme veya birleştirme işlemleri gibi veri türünü veya değerlerin sayısını etkileyebilecek modülün veri yukarı akışında bir işlem gerçekleştiriyorsanız, döndürülen satır sayısını belirlemek için bu işlemlerin çıktılarını denetleyin.  

|Özel Durum Mesajları|
|------------------------|
|Giriş veri kümesindeki satır sayısı izin verilen minimumdan daha azdır.|
|Giriş veri kümesindeki satır sayısı , {required_rows_count} satırı(lar) için izin verilen minimumdan daha azdır.|
|Giriş veri kümesindeki satır sayısı , {required_rows_count} satırı(lar) için izin verilen minimumdan daha azdır. {neden}|
|Giriş veri kümesindeki satır sayısı "{arg_name}" {required_rows_count} satır(lar) için izin verilen minimumdan daha azdır.|
|arg_name Giriş veri kümesindeki satır sayısı {actual_rows_count}, izin verilen {required_rows_count} satır(lar)'ın en azından daha azdır.|
|Giriş veri kümesindeki "arg_name{row_type}" satırı sayısı {actual_rows_count required_rows_count}, izin verilen {required_rows_count} satır(lar)'ın minimumundan daha azdır.|


## <a name="error-0022"></a>Hata 0022  
 Giriş veri kümesinde seçili sütun sayısı beklenen sayıya eşit değilse özel durum oluşur.  

 Azure Machine Learning'deki bu hata, akış aşağı modülü veya işlemi belirli sayıda sütun veya girdi gerektirdiğinde ve çok az sayıda sütun veya giriş sağladığınızda oluşabilir. Örnek:  

-   Tek bir etiket sütunu veya anahtar sütun u belirtin ve yanlışlıkla birden çok sütun seçersiniz.  
  
-   Sütunları yeniden adlandırDın, ancak sütunlardan daha fazla veya daha az ad sağladınız.  
  
-   Kaynak veya hedefteki sütun sayısı değişti veya modül tarafından kullanılan sütun sayısıyla eşleşmiyor.  
  
-   Girişler için virgülden ayrılmış bir değerler listesi sağladınız, ancak değer sayısı eşleşmez veya birden çok giriş desteklenmez.  

**Çözünürlük:** Doğru sütun sayısının seçildiğinden emin olmak için modülü yeniden ziyaret edin ve sütun seçimini denetleyin. Yukarı akış modüllerinin çıktılarını ve akış aşağı işlemlerinin gereksinimlerini doğrulayın.  

 Birden çok sütun (sütun dizinleri, tüm özellikler, tüm sayısal, vb.) seçebilen sütun seçim seçeneklerinden birini kullandıysanız, seçimtarafından döndürülen sütunların tam sayısını doğrulayın.  

 <!--If you are trying to specify a comma-separated list of datasets as inputs to [Unpack Zipped Datasets](unpack-zipped-datasets.md), unpack only one dataset at a time. Multiple inputs are not supported.  -->

 Akış yukarı sütun sayısının veya türünün değişmediğini doğrulayın.  

 Bir modeli eğitmek için bir öneri veri kümesi kullanıyorsanız, tavsiye edenin kullanıcı öğesi çiftleri veya kullanıcı öğesi sıralamalarına karşılık gelen sınırlı sayıda sütun beklediğini unutmayın. Modeli zorlamadan veya öneri veri kümelerini bölmeden önce ek sütunları kaldırın. Daha fazla bilgi için Bkz. [Verileri Böl.](split-data.md)  

|Özel Durum Mesajları|
|------------------------|
|Giriş veri kümesinde seçilen sütun sayısı beklenen sayıya eşit değildir.|
|Giriş veri kümesinde seçilen sütun sayısı {expected_col_count} eşit değildir.|
|Sütun seçim deseni "{selection_pattern_friendly_name}" giriş veri kümesinde seçili sütun sayısını {expected_col_count} eşit değildir.|
|Sütun seçim deseni "{selection_pattern_friendly_name}" giriş veri kümesinde seçilen {expected_col_count} sütun(lar) sağlaması beklenir, ancak {selected_col_count} sütun(lar) gerçekte sağlanır..|


## <a name="error-0023"></a>Hata 0023  

Giriş veri kümesinin hedef sütunu geçerli eğitmen modülü için geçerli değilse özel durum oluşur.  

Azure Machine Learning'deki bu hata, hedef sütun (modül parametrelerinde seçildiği gibi) geçerli veri türünden değilse, tüm eksik değerleri içeriyorsa veya beklendiği gibi kategorik değilse oluşur.  

**Çözünürlük:** Etiket/hedef sütunun içeriğini incelemek için modül girişini yeniden ziyaret edin. Tüm eksik değerlere sahip olmadığından emin olun. Modül hedef sütunun kategorik olmasını bekliyorsa, hedef sütunda birden fazla farklı değer olduğundan emin olun.  

|Özel Durum Mesajları|
|------------------------|
|Giriş veri kümesi desteklenmeyen hedef sütuna sahiptir.|
|Giriş veri kümesinde desteklenmeyen hedef sütun "{column_index}".|
|Giriş veri kümesi , {learner_type} türünden öğrenci için desteklenmeyen hedef sütun "{column_index}" vardır.|


## <a name="error-0024"></a>Hata 0024  
Dataset bir etiket sütunu içermiyorsa özel durum oluşur.  

 Azure Machine Learning'deki bu hata, modül bir etiket sütunu gerektirdiğinde ve veri kümesinde etiket sütunu olmadığında oluşur. Örneğin, puanlanan bir veri kümesinin değerlendirilmesi, doğruluk ölçümlerini hesaplamak için genellikle bir etiket sütununun bulunmasını gerektirir.  

Ayrıca, veri kümesinde bir etiket sütunu bulunması, ancak Azure Machine Learning tarafından doğru algılanmaması da mümkündür.

**Çözünürlük:**

+ Hatayı oluşturan modülü açın ve bir etiket sütunu olup olmadığını belirleyin. Sütun tahmin etmeye çalıştığınız tek bir sonuç (veya bağımlı değişken) içerdiği sürece, sütunun adı veya veri türü önemli değildir. Etikette hangi sütunun olduğundan emin değilseniz, *Sınıf* veya *Hedef*gibi genel bir ad arayın. 
+  Veri kümesi bir etiket sütunu içermiyorsa, etiket sütununaçık veya yanlışlıkla yukarı kaldırılmış olabilir. Ayrıca, veri kümesi bir upstream puanlama modülü çıkışı değildir olabilir.
+ Sütunu etiket sütunu olarak açıkça işaretlemek için [Meta Verileri Edit](edit-metadata.md) modülünü ekleyin ve veri kümesini bağlayın. Yalnızca etiket sütununa ve **Alanlar** açılır listesinden **Etiket'i** seçin. 
+ Etiket olarak yanlış sütun seçilirse, sütundaki meta verileri düzeltmek için **Alanlar'dan** **Etiketi Temizle'yi** seçebilirsiniz. 
  
|Özel Durum Mesajları|
|------------------------|
|Dataset'te etiket sütunu yoktur.|
|"{dataset_name}" içinde etiket sütunu yoktur.|


## <a name="error-0025"></a>Hata 0025  
 Dataset bir puan sütunu içermiyorsa özel durum oluşur.  

 Azure Machine Learning'deki bu hata, değerlendirme modeline giriş geçerli puan sütunları içermiyorsa oluşur. Örneğin, kullanıcı doğru eğitilmiş bir modelle puanlandırIlmeden önce bir veri kümesini değerlendirmeye çalışır veya puan sütunu açıkça yukarı yaslanmış olur. Bu özel durum, iki veri kümesindeki puan sütunları uyumsuzsa da oluşur. Örneğin, doğrusal bir regresörün doğruluğunu ikili bir sınıflandırıcıyla karşılaştırmaya çalışıyor olabilirsiniz.  

**Çözünürlük:** Değerlendirme modeline girişi yeniden gözden geçirin ve bir veya daha fazla puan sütunu içerip içermeden inceleyin. Değilse, veri kümesi puanlandırılmamadı veya puan sütunları bir yukarı modüle düştü.  

|Özel Durum Mesajları|
|------------------------|
|Dataset'te puan sütunu yok.|
|"{dataset_name}" de puan sütunu yoktur.|
|"{dataset_name}" adlı bir "{learner_type}" tarafından üretilen puan sütunu yoktur. Doğru öğrenci türünü kullanarak veri kümesini puan.|


## <a name="error-0026"></a>Hata 0026  
 Aynı ada sahip sütunlara izin verilmiyorsa özel durum oluşur.  

 Azure Machine Learning'deki bu hata, birden çok sütunun aynı ada sahip olması durumunda oluşur. Bu hatayı almanın bir yolu, veri kümesinde üstbilgi satırı yoksa ve sütun adları otomatik olarak atanmışsa: Col0, Col1, vb.  

**Çözünürlük:** Sütunlar aynı ada sahipse, giriş veri kümesi ile modül arasına meta veri seti [edit](edit-metadata.md) modülü ekleyin. **Yeni sütun adları** metin kutusuna yeni adlar yazarak, yeniden adlandırmak için sütunları seçmek için Meta verileri [edit'te](edit-metadata.md) sütun seçiciyi kullanın.  

|Özel Durum Mesajları|
|------------------------|
|Bağımsız değişkenlerde eşit sütun adları belirtilir. Modül tarafından eşit sütun adlarının izin verilmemektedir.|
|"{arg_name_1}" ve "{arg_name_2}" bağımsız değişkenlerinde eşit sütun adlar giremez. Lütfen farklı adlar belirtin.|


## <a name="error-0027"></a>Hata 0027  
 Özel durum, iki nesnenin aynı boyutta olması ancak olmaması durumunda oluşur.  

 Bu, Azure Machine Learning'de sık karşılaşılan bir hatadır ve birçok koşuldan kaynaklanabilir.  

**Çözünürlük:** Belirli bir çözüm yoktur. Ancak, aşağıdaki gibi koşulları denetleyebilirsiniz:  

-   Sütunları yeniden adlandırırsanız, her listenin (giriş sütunları ve yeni adlar listesi) aynı sayıda öğeye sahip olduğundan emin olun.  
  
-   İki veri kümesine katılıyor veya bulabiliyorsanız, aynı şema sahip olduğundan emin olun.  
  
-   Birden çok sütunu olan iki veri kümesini birleştiriyorsanız, anahtar sütunlarının aynı veri türüne sahip olduğundan emin olun ve yinelenenlere izin ver seçeneğini seçin **ve seçimde sütun sırasını koruyun.**  

|Özel Durum Mesajları|
|------------------------|
|Geçirilen nesnelerin boyutu tutarsız.|
|"{friendly_name1}" boyutu "{friendly_name2}" boyutuyla tutarsızdır.|


## <a name="error-0028"></a>Hata 0028  
 Özel durum, sütun kümesi yinelenen sütun adlarını içerdiğinde ve buna izin verilmediğinde oluşur.  

 Azure Machine Learning'deki bu hata, sütun adları çoğaltıldığında oluşur; yani, benzersiz değil.  

**Çözünürlük:** Herhangi bir sütunun adı aynıysa, giriş veri kümesi ile hatayı yükselten modül arasına [Meta Verilerini Edit'in](edit-metadata.md) bir örneğini ekleyin. Yeniden adlandırmak için sütunları seçmek ve **Yeni sütun adları** metin kutusuna yeni sütun adlarını yazmak için Meta Verilerini [Edit'te](edit-metadata.md) Sütun Seçici'yi kullanın. Birden çok sütunu yeniden adlandırırsanız, Yeni **sütun adlarında** yazdığınız değerlerin benzersiz olduğundan emin olun.  

|Özel Durum Mesajları|
|------------------------|
|Sütun kümesi yinelenen sütun adı(lar) içerir.|
|"{duplicated_name}" adı çoğaltılır.|
|"{duplicated_name}" adı "{arg_name}" ile çoğaltılır.|
|"{duplicated_name}" adı çoğaltılır. Ayrıntılar: {ayrıntılar}|


## <a name="error-0029"></a>Hata 0029  
 İstisna, geçersiz URI geçirildiğinde oluşur.  

 Azure Machine Learning'deki bu hata, geçersiz URI'nin geçirilmesi durumunda oluşur.  Aşağıdaki koşullardan herhangi biri doğruysa bu hatayı alırsınız:  

-   Okuma veya yazma için Azure Blob Depolaması için sağlanan Genel veya SAS URI bir hata içerir.  
  
-   SAS için zaman süresi doldu.  
  
-   HTTP kaynağı üzerinden Web URL'si bir dosyayı veya bir loopback URI'yi temsil eder.  
  
-   HTTP üzerinden Web URL'si yanlış biçimlendirilmiş bir URL içerir.  
  
-   URL uzak kaynak tarafından çözülemez.  

**Çözünürlük:** Modülü yeniden ziyaret edin ve URI'nin biçimini doğrulayın. Veri kaynağı HTTP üzerinden bir Web URL'siyse, amaçlanan kaynağın bir dosya veya geri döngü URI (localhost) olmadığını doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Geçersiz Uri geçti.|
|Uri "{invalid_url}" geçersizdir.|


## <a name="error-0030"></a>Hata 0030  
 Bir dosyayı karşıdan yüklemenin mümkün olmadığı durumlarda özel durum oluşur.  

 Azure Machine Learning'deki bu özel durum, bir dosyayı karşıdan yüklemenin mümkün olmadığı durumlarda oluşur. Bir HTTP kaynağından bir okuma denemesi üç (3) yeniden denemeden sonra başarısız olduğunda bu özel durum alırsınız.  

**Çözünürlük:** HTTP kaynağına URI doğru olduğunu ve sitenin şu anda Internet üzerinden erişilebilir olduğunu doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Dosya indirilemiyor.|
|Dosyayı indirirken hata: {file_url}.|


## <a name="error-0031"></a>Hata 0031  
 Sütun kümesindeki sütun sayısı gerekenden azsa özel durum oluşur.  

 Azure Machine Learning'deki bu hata, seçilen sütun sayısı gerekenden azsa oluşur.  Gerekli en az sayıda sütun seçili değilse bu hatayı alırsınız.  

**Çözünürlük:** **Sütun Seçici'yi**kullanarak sütun seçimine ek sütunlar ekleyin.  

|Özel Durum Mesajları|
|------------------------|
|Sütun kümesindeki sütun sayısı gerekenden daha azdır.|
|Giriş bağımsız değişkeni "{arg_name}" için en az {required_columns_count} sütun(lar) belirtilmelidir.|
|Giriş bağımsız değişkeni "{arg_name}" için en az {required_columns_count} sütun(lar) belirtilmelidir. Belirtilen sütunların gerçek sayısı {input_columns_count}'dir.|


## <a name="error-0032"></a>Hata 0032  
 Bağımsız değişken bir sayı değilse özel durum oluşur.  

 Bağımsız değişken çift veya NaN ise bu hatayı Azure Machine Learning'de alırsınız.  

**Çözünürlük:** Geçerli bir değer kullanmak için belirtilen bağımsız değişkeni değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Bağımsız değişken bir sayı değildir.|
|"{arg_name}" bir sayı değildir.|


## <a name="error-0033"></a>Hata 0033  
 Bağımsız değişken Infinity ise özel durum oluşur.  

 Azure Machine Learning'deki bu hata, bağımsız değişken sonsuzsa oluşur. Bağımsız değişken ya da `double.NegativeInfinity` `double.PositiveInfinity`.  

**Çözünürlük:** Belirtilen bağımsız değişkeni geçerli bir değer olarak değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Bağımsız değişken sonlu olmalıdır.|
|"{arg_name}" sonlu değildir.|


## <a name="error-0034"></a>Hata 0034  
 Özel durum, belirli bir kullanıcı öğesi çifti için birden fazla derecelendirme varsa oluşur.  

 Azure Machine Learning'deki bu hata, bir kullanıcı öğesi çiftinin birden fazla derecelendirme değeri varsa, öneride oluşur.  

**Çözünürlük:** Kullanıcı öğesi çiftinin yalnızca bir derecelendirme değerine sahip olduğundan emin olun.  

|Özel Durum Mesajları|
|------------------------|
|Veri kümesindeki değer(ler) için birden fazla derecelendirme vardır.|
|Derecelendirme tahmini veri tablosunda kullanıcı {user} ve madde {item} için birden fazla derecelendirme.|
|{dataset}'te kullanıcı {user} ve madde {item} için birden fazla derecelendirme.|


## <a name="error-0035"></a>Hata 0035  
 Özel durum, belirli bir kullanıcı veya öğe için özellik sağlanmadıysa oluşur.  

 Azure Machine Learning'deki bu hata, puanlama için bir öneri modeli kullanmaya çalıştığınız, ancak bir özellik vektörü bulunamıyor.  

**Çözünürlük:**

Matchbox tavsiye cihazı, öğe özelliklerini veya kullanıcı özelliklerini kullanırken karşılanması gereken belirli gereksinimlere sahiptir.  Bu hata, girdi olarak sağladığınız bir kullanıcı veya öğe için bir özellik vektörü eksik olduğunu gösterir. Her kullanıcı veya öğe için verilerde bir özellik vektörü bulunduğundan emin olun.  

 Örneğin, kullanıcının yaşı, konumu veya geliri gibi özellikleri kullanarak bir öneri modeli eğittiyseniz, ancak şimdi eğitim sırasında görünmeyen yeni kullanıcılar için puanlar oluşturmak istiyorsanız, bazı eşdeğer özellikler (örn. yaş, konum ve gelir değerleri) onlar için uygun tahminler yapmak için yeni kullanıcılar için. 

 Bu kullanıcılar için herhangi bir özelliğiniz yoksa, uygun özellikler oluşturmak için özellik mühendisliğini göz önünde bulundurun.  Örneğin, tek tek kullanıcı yaşınız veya gelir değerleriniz yoksa, bir kullanıcı grubu için kullanılacak yaklaşık değerler oluşturabilirsiniz. 

<!--When you are scoring from a recommendation mode, you can use item or user features only if you previously used item or user features during training. For more information, see [Score Matchbox Recommender](score-matchbox-recommender.md).

For general information about how the Matchbox recommendation algorithm works, and how to prepare a dataset of item features or user features, see [Train Matchbox Recommender](train-matchbox-recommender.md).  -->

 > [!TIP]
 > Karar sizin durumunuz için geçerli değil mi? Bu makale hakkında geri bildirim gönderebilir ve modül ve sütundaki satır sayısı da dahil olmak üzere senaryo hakkında bilgi verebilirsiniz. Bu bilgileri gelecekte daha ayrıntılı sorun giderme adımları sağlamak için kullanacağız.

|Özel Durum Mesajları|
|------------------------|
|Gerekli bir kullanıcı veya öğe için hiçbir özellik sağlanmadı.|
|{required_feature_name} için özellikler gereklidir, ancak sağlanmaz.|


## <a name="error-0036"></a>Hata 0036  
 Özel durum, belirli bir kullanıcı veya öğe için birden çok özellik vektörü sağlanmışsa oluşur.  

 Azure Machine Learning'deki bu hata, bir özellik vektörü birden çok kez tanımlanırsa oluşur.  

**Çözünürlük:** Özellik vektöründen birden fazla kez tanımlanmadığından emin olun.  

|Özel Durum Mesajları|
|------------------------|
|Bir kullanıcı veya öğe için yinelenen özellik tanımı.|


## <a name="error-0037"></a>Hata 0037  
 Birden çok etiket sütunu belirtilirse ve bunlardan birine izin verilirse özel durum oluşur.  

 Azure Machine Learning'deki bu hata, yeni etiket sütunu olarak birden fazla sütun seçilirse oluşur. Denetimli öğrenme algoritmalarının çoğu, hedef veya etiket olarak işaretlenecek tek bir sütun gerektirir.  

**Çözünürlük:** Yeni etiket sütunu olarak tek bir sütun seçtiğinizden emin olun.  

|Özel Durum Mesajları|
|------------------------|
|Birden çok etiket sütunu belirtilir.|
|Birden çok etiket sütunu "{dataset_name}" olarak belirtilir.|


## <a name="error-0039"></a>Hata 0039  
 Bir işlem başarısız olursa özel durum oluşur.  

 Azure Machine Learning'deki bu hata, dahili bir işlem tamamlanamadığında oluşur.  

**Çözünürlük:** Bu hata birçok koşuldan kaynaklanır ve belirli bir çözüm yoktur.  
 Aşağıdaki tablo, bu hata için genel iletiler içerir ve ardından durumun belirli bir açıklaması vardır. 

 Hiçbir ayrıntı yoksa, [geri bildirim gönderin](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) ve hatayı ve ilgili koşulları oluşturan modüller hakkında bilgi sağlayın.

|Özel Durum Mesajları|
|------------------------|
|İşlem başarısız oldu.|
|İşlemi tamamlarken hata: "{failed_operation}".|
|İşlemi tamamlarken hata: "{failed_operation}". Sebep: "{reason}".|


## <a name="error-0042"></a>Hata 0042  
 Özel durum, sütunu başka bir türe dönüştürmenin mümkün olmadığı nda oluşur.  

 Azure Machine Learning'deki bu hata, sütunu belirtilen türe dönüştürmenin mümkün olmadığı durumlarda oluşur.  Bir modül tarih, metin, kayan nokta numarası veya tamsayı gibi belirli bir veri türü gerektiriyorsa, varolan bir sütunu gerekli türe dönüştürmek mümkün değilse, bu hatayı alırsınız.  

Örneğin, bir sütun seçebilir ve bir matematik işleminde kullanılmak üzere sayısal bir veri türüne dönüştürmeyi deneyebilir ve sütun geçersiz veri içeriyorsa bu hatayı alabilirsiniz. 

Kayan nokta sayıları veya çok sayıda benzersiz değeri içeren bir sütunu kategorik bir sütun olarak kullanmaya çalışırsanız, bu hatayı almanız başka bir nedeni. 

**Çözünürlük:**

+ Hatayı oluşturan modülün yardım sayfasını açın ve veri türü gereksinimlerini doğrulayın.
+ Giriş veri kümesindeki sütunların veri türlerini gözden geçirin.
+ Şemasız veri kaynaklarından kaynaklanan verileri inceleyin.
+ Veri kümesini, istenen veri türüne dönüştürmeyi engelleyebilecek eksik değerler veya özel karakterler olup olmadığını denetleyin. 
    + Sayısal veri türleri tutarlı olmalıdır: örneğin, tamsayılar sütunundaki kayan nokta numaralarını denetleyin.
    + Bir sayı sütunundaki metin dizeleri veya NA değerlerini arayın. 
    + Boolean değerleri, gerekli veri türüne bağlı olarak uygun bir gösterime dönüştürülebilir.
    + Unicode olmayan karakterler, sekme karakterleri veya denetim karakterleri için metin sütunlarını inceleme
    + Modelleme hatalarını önlemek için datetime verileri tutarlı olmalıdır, ancak temizleme birçok biçimnedeniyle karmaşık olabilir. Kullanmayı düşünün <!--the [Execute R Script](execute-r-script.md) or -->Temizleme gerçekleştirmek için Python Script modüllerini [çalıştırın.](execute-python-script.md)  
+ Gerekirse, sütunun başarıyla dönüştürülebilmeleri için giriş veri kümesindeki değerleri değiştirin. Değişiklik binning, truncation veya yuvarlama işlemleri, aykırı ortadan kaldırılması veya eksik değerlerin imputation içerebilir. Makine öğreniminde bazı yaygın veri dönüşüm senaryoları için aşağıdaki makalelere bakın:
    + [Eksik Verileri Temizleme](clean-missing-data.md)
    + [Verileri Normalleştirme](normalize-data.md)
<!--+ [Clip Values](clip-values.md) 
    + [Group Data Into Bins](group-data-into-bins.md)
  -->

> [!TIP]
> Karar belirsiz mi, yoksa davanız için geçerli değil mi? Bu makale hakkında geri bildirim gönderebilir ve modül ve sütunun veri türü de dahil olmak üzere senaryo hakkında bilgi verebilirsiniz. Bu bilgileri gelecekte daha ayrıntılı sorun giderme adımları sağlamak için kullanacağız.  

|Özel Durum Mesajları|
|------------------------|
|Dönüştürmeye izin verilmiyor.|
|{type1} türü sütunu {type2} türü sütununa dönüştüremedi.|
|{type1} türünden "{col_name1}" sütununun türü {type2} sütununa dönüştürülemedi.|
|{type1} türünden "{col_name1}" sütununun "{col_name2}" türünden "{col_name2}" sütununa dönüştürülemedi.type2}.|


## <a name="error-0044"></a>Hata 0044  
 Özel durum, varolan değerlerden öğe türü türetmek mümkün olmadığında oluşur.  

 Azure Machine Learning'deki bu hata, bir veri kümesindeki sütun veya sütun türünü çıkaramadığında oluşur. Bu genellikle, farklı öğe türleri ile iki veya daha fazla veri kümeleri concatenating olur. Azure Machine Learning, bir sütunveya sütundaki tüm değerleri bilgi kaybı olmadan temsil edebilecek ortak bir tür belirleyemiyorsa, bu hatayı oluşturur.  

**Çözünürlük:** Birleştirilen her iki veri kümesindeki belirli bir sütundaki tüm değerlerin aynı türde (sayısal, Boolean, kategorik, dize, tarih, vb.) veya aynı türe zorlanabilmesini sağlayın.  

|Özel Durum Mesajları|
|------------------------|
|Sütunun öğe türünü elde edemez.|
|"{column_name}" sütunu için öğe türü türetilemez -- tüm öğeler null başvurulardır.|
|Veri kümesi "{column_name dataset_name}" sütunu için öğe türü elde edilemez -- tüm öğeler null başvurulardır.|


## <a name="error-0045"></a>Hata 0045  
 Özel durum, kaynaktaki karışık öğe türleri nedeniyle bir sütun oluşturmak mümkün olmadığında oluşur.  

 Azure Machine Learning'deki bu hata, birleştirilen iki veri kümesinin eleman türleri farklı olduğunda oluşur.  

**Çözünürlük:** Birleştirilen her iki veri kümesindeki belirli bir sütundaki tüm değerlerin aynı türde olduğundan emin olun (sayısal, Boolean, kategorik, dize, tarih, vb.).  

|Özel Durum Mesajları|
|------------------------|
|Karışık öğe türleri içeren sütun oluşturamaz.|
|Karışık eleman türlerinin id "{column_id}" kimlikli sütunu oluşturulamaz:<br />Veri türü[{row_1}, {column_id}] "{type_1}"dir. <br />Veri türü[{row_2}, {column_id}] "{type_2}" olur.|
|Karışık eleman türlerinin id "{column_id}" kimlikli sütunu oluşturulamaz:<br />{chunk_id_1} öbek "{type_1}" yazımı. <br />{chunk_id_2} öbek boyutunu "{type_2}" olarak yazın: {chunk_size}.|


## <a name="error-0046"></a>Hata 0046  
 Özel durum, belirtilen yolda dizin oluşturmanın mümkün olmadığı nda oluşur.  

 Azure Machine Learning'deki bu hata, belirtilen yolda bir dizin oluşturmanın mümkün olmadığı durumlarda oluşur. Kovan Sorgusu için çıktı dizinine giden yolun herhangi bir bölümü yanlış veya erişilemezse bu hatayı alırsınız.  

**Çözünürlük:** Modülü yeniden ziyaret edin ve dizin yolunun doğru biçimlendirilmiş olduğunu ve geçerli kimlik bilgileriyle erişilebilir olduğunu doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Lütfen geçerli bir çıktı dizini belirtin.|
|Dizin: {path} oluşturulamıyor. Lütfen geçerli bir yol belirtin.|


## <a name="error-0047"></a>Hata 0047  
 Modüle geçirilen bazı veri kümelerinde özellik sütunlarının sayısı çok küçükse özel durum oluşur.  

 Azure Machine Learning'deki bu hata, eğitime giriş veri kümesi algoritma tarafından gerekli olan en az sütun sayısını içermiyorsa oluşur. Genellikle veri kümesi boş veya yalnızca eğitim sütunları içerir.  

**Çözünürlük:** Etiket sütunu dışında bir veya daha fazla ek sütun olduğundan emin olmak için giriş veri kümesini yeniden ziyaret edin.  

|Özel Durum Mesajları|
|------------------------|
|Giriş veri kümesindeki özellik sütunlarının sayısı izin verilen minimumdan daha azdır.|
|Giriş veri kümesindeki özellik sütunlarının sayısı , {required_columns_count} sütun(lar) için izin verilen minimumdan daha azdır.|
|Giriş veri kümesi "{arg_name}" deki özellik sütunlarının sayısı {required_columns_count} sütun(lar) için izin verilen minimumdan daha azdır.|


## <a name="error-0048"></a>Hata 0048  
 Bir dosyayı açmak mümkün olmadığında durumda özel durum oluşur.  

 Azure Machine Learning'deki bu hata, bir dosyayı okumak veya yazmak için açmak mümkün olmadığında oluşur. Bu nedenlerden dolayı bu hatayı alabilirsiniz:  

-   Kapsayıcı veya dosya (blob) yok  
  
-   Dosyanın veya kapsayıcının erişim düzeyi dosyaya erişmenize izin vermez  
  
-   Dosya okumak için çok büyük veya yanlış biçimde  

**Çözünürlük:** Modülü ve okumaya çalıştığınız dosyayı yeniden ziyaret edin.  

 Kapsayıcı ve dosya adlarının doğru olduğunu doğrulayın.  

 Dosyaya erişmek için izniniz olduğunu doğrulamak için Azure klasik portalını veya Azure depolama aracını kullanın.  

  <!--If you are trying to read an image file, make sure that it meets the requirements for image files in terms of size, number of pixels, and so forth. For more information, see [Import Images](import-images.md).  -->

|Özel Durum Mesajları|
|------------------------|
|Dosya açılmıyor.|
|Dosyayı açarken hata: {file_name}.|
|Dosyayı açarken hata: {file_name}. Depolama özel durum iletisi: {özel durum}.|


## <a name="error-0049"></a>Hata 0049  
 Bir dosyayı ayrıştırmak mümkün olmadığında durumda özel durum oluşur.  

 Azure Machine Learning'deki bu hata, bir dosyayı ayrıştırmak mümkün olmadığında oluşur. [İçe Aktarma Veri](import-data.md) modülünde seçilen dosya biçimi dosyanın gerçek biçimiyle eşleşmiyorsa veya dosya tanınmaz bir karakter içeriyorsa bu hatayı alırsınız.  

**Çözünürlük:** Modülü yeniden ziyaret edin ve dosya biçimiyle eşleşmiyorsa dosya biçimi seçimini düzeltin. Mümkünse, dosyanın herhangi bir geçersiz karakter içermediğini doğrulamak için dosyayı inceleyin.  

|Özel Durum Mesajları|
|------------------------|
|Dosyayı ayrışdıramıyor.|
|{file_format} dosyasını ayrışdırırken hata.|
|{file_format} dosyasını ayrışturarak hata: {file_name}.|
|{file_format} dosyasını ayrışdırırken hata. Sebep: {failure_reason}.|
|{file_format} dosyasını ayrışturarak hata: {file_name}. Sebep: {failure_reason}.|


## <a name="error-0052"></a>Hata 0052  
 Azure depolama hesabı anahtarı yanlış belirtilirse özel durum oluşur.  

 Azure Depolama Hesabına erişmek için kullanılan anahtar yanlışsa Azure Machine Learning'deki bu hata oluşur. Örneğin, Azure depolama anahtarı kopyalanıp yapıştırıldığında kesildiyse veya yanlış anahtar kullanıldıysa bu hatayı görebilirsiniz.  

 Azure depolama hesabının anahtarını nasıl alacağınız hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/documentation/articles/storage-create-storage-account-classic-portal/)  

**Çözünürlük:** Modülü yeniden ziyaret edin ve Azure depolama anahtarının hesap için doğru olduğunu doğrulayın; gerekirse anahtarı Azure klasik portalından yeniden kopyalayın.  

|Özel Durum Mesajları|
|------------------------|
|Azure depolama hesabı anahtarı yanlıştır.|


## <a name="error-0053"></a>Hata 0053  
 Kibrit kutusu önerileri için kullanıcı özellikleri veya öğeleri olmadığında özel durum oluşur.  

 Azure Machine Learning'deki bu hata, bir özellik vektörü bulunamadığında oluşur.  

**Çözünürlük:** Giriş veri kümesinde bir özellik vektörü olduğundan emin olun.  

|Özel Durum Mesajları|
|------------------------|
|Kullanıcı özellikleri veya/ve öğeleri gereklidir, ancak sağlanmaz.|


## <a name="error-0056"></a>Hata 0056  
 Bir işlem için seçtiğiniz sütunlar gereksinimleri ihlal ederse özel durum oluşur.  

 Azure Machine Learning'deki bu hata, sütunun belirli bir veri türüne ait olmasını gerektiren bir işlem için sütun ları seçerken oluşur. 

 Sütun doğru veri türüyse, bu hata da olabilir, ancak kullandığınız modül sütunun özellik, etiket veya kategorik sütun olarak da işaretlenolmasını gerektirir.  

  <!--For example, the [Convert to Indicator Values](convert-to-indicator-values.md) module requires that columns be categorical, and will raise this error if you select a feature column or label column.  -->

**Çözünürlük:**

1.  Şu anda seçili olan sütunların veri türünü gözden geçirin. 

2. Seçili sütunların kategorik mi, etiket mi yoksa özellik sütunları mı olduğunu sapta.  
  
3.  Veri türü veya sütun kullanımı için belirli gereksinimler olup olmadığını belirlemek için sütun seçimini yaptığınız modülün yardım konusunu gözden geçirin.  
  
3.  Bu işlem süresince sütun türünü değiştirmek için [Meta Verilerini Edit'i](edit-metadata.md) kullanın. Sütun türünü, akış aşağı işlemleri için gerekirse Meta [verilerini düzenle'nin](edit-metadata.md)başka bir örneğini kullanarak orijinal değerine değiştirdiğinden emin olun.  

|Özel Durum Mesajları|
|------------------------|
|Bir veya daha fazla seçili sütun izin verilen bir kategoride değildi.|
|"{col_name}" adında sütun izin verilen bir kategoride değildir.|


## <a name="error-0057"></a>Hata 0057  
 Özel durum, zaten var olan bir dosya veya blob oluşturmaya çalışırken oluşur.  

 Bu özel durum, Azure Machine Learning'deki bir ardışık noktanın sonuçlarını Azure blob depolamasına kaydetmek için [Dışa Aktarma Verisi](export-data.md) modülünün veya başka bir modülü kullandığınızda, ancak zaten var olan bir dosya veya blob oluşturmaya çalışırsanız oluşur.   

**Çözünürlük:**

 Bu hatayı yalnızca daha önce özellik **Azure blob depolama yazma modunu** **Hata**olarak ayarlarsanız alırsınız. Tasarım gereği, zaten var olan bir blob'a bir veri kümesi yazmaya çalışırsanız, bu modül bir hata kaldırır.

 - Modül özelliklerini açın ve azure **blob depolama yazma modunu** Overwrite olarak **değiştirin.**
 - Alternatif olarak, farklı bir hedef blob veya dosyanın adını yazabilir ve zaten var olmayan bir blob belirttiğinden emin olabilirsiniz.  

|Özel Durum Mesajları|
|------------------------|
|Dosya veya Blob zaten var.|
|Dosya veya Blob "{file_path}" zaten var.|


## <a name="error-0058"></a>Hata 0058  
 Azure Machine Learning'deki bu hata, veri kümesi beklenen etiket sütununa içermiyorsa oluşur.  

 Bu özel durum, sağlanan etiket sütunu öğrenci tarafından beklenen veri veya veri türüyle eşleşmediğinde veya yanlış değerlere sahip olduğunda da oluşabilir. Örneğin, bu özel durum, ikili sınıflandırıcıyı eğitirken gerçek değerli bir etiket sütunu kullanılırken oluşturulur.  

**Çözünürlük:** Çözünürlük, kullanmakta olduğunuz öğrenci veya eğitmene ve veri setinizdeki sütunların veri türlerine bağlıdır. İlk olarak, makine öğrenme algoritması veya eğitim modülü gereksinimlerini doğrulayın.  

 Giriş veri kümesini yeniden ziyaret edin. Etiket olarak kabul görmeyi beklediğiniz sütunun oluşturduğunuz model için doğru veri türüne sahip olduğunu doğrulayın.  

 Eksik değerler için girişleri denetleyin ve gerekirse bunları ortadan kaldırın veya değiştirin.  

 Gerekirse, Meta veri modüllerini [edit'i](edit-metadata.md) ekleyin ve etiket sütununun etiket olarak işaretlendiğini sağlayın.  

|Özel Durum Mesajları|
|------------------------|
|Etiket sütun değerleri ve puanlı etiket sütun değerleri karşılaştırılabilir değildir.|
|Etiket sütunu "{dataset_name}"de beklendiği gibi değildir.|
|Etiket sütunu "{dataset_name}", {reason}'da beklendiği gibi değildir.|
|Etiket sütunu "{column_name}" "{dataset_name}" olarak beklendi.|
|Etiket sütunu "{column_name}" "{dataset_name}", {reason}'da beklenmez.|


## <a name="error-0059"></a>Hata 0059  
 Bir sütun seçicide belirtilen sütun dizini ayrıştırılamazsa özel durum oluşur.  

 Sütun Seçici'yi kullanırken belirtilen sütun dizini ayrıştırılamıyorsa, Azure Machine Learning'deki bu hata oluşur.  Sütun dizini ayrıştırılamayan geçersiz bir biçimde olduğunda bu hatayı alırsınız.  

**Çözünürlük:** Geçerli bir dizin değeri kullanmak için sütun dizini değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Bir veya daha fazla belirtilen sütun dizinleri veya dizin aralıkları ayrıştı olamazdı.|
|Sütun dizini veya aralığı "{column_index_or_range}" ayrıştırılamadı.|


## <a name="error-0060"></a>Hata 0060  
 Özel durum, bir sütun seçicide aralık dışı bir sütun aralığı belirtildiğinde oluşur.  

 Azure Machine Learning'deki bu hata, Sütun Seçici'de aralık dışı bir sütun aralığı belirtildiğinde oluşur. Sütun seçicideki sütun aralığı veri kümesindeki sütunlara karşılık gelmiyorsa bu hatayı alırsınız.  

**Çözünürlük:** Veri kümesindeki sütunlara karşılık gelecek şekilde sütun seçicideki sütun aralığını değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Geçersiz veya belirtilen aralık sütun dizini aralığı dışında.|
|Sütun aralığı "{column_range}" geçersiz veya aralık dışındadır.|


## <a name="error-0061"></a>Hata 0061  
 Özel durum, tablodan farklı sayıda sütuna sahip bir DataTable'a satır eklemeye çalışırken oluşur.  

 Azure Machine Learning'deki bu hata, veri kümesinden farklı sayıda sütuna sahip bir veri kümesine satır eklemeye çalıştığınızda oluşur.  Veri kümesine eklenen satırda giriş veri kümesinden farklı sayıda sütun varsa bu hatayı alırsınız.  Sütun sayısı farklıysa satır veri kümesine eklenemez.  

**Çözünürlük:** Giriş veri kümesini satır eklenerek aynı sayıda sütuna sahip olacak şekilde değiştirin veya eklenen satırı veri kümesiyle aynı sayıda sütuna sahip olacak şekilde değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Tüm tablolarda aynı sayıda sütun olmalıdır.|
|"{chunk_id_1}" sütunları ,"{chunk_id_2}" parçası ile farklıdır: {chunk_size}.|
|"{filename_1}" (count={column_count_1}) dosyasındaki sütun sayısı "{filename_2}" (count={column_count_2}) dosyasıile farklıdır.|


## <a name="error-0062"></a>Hata 0062  
 Özel durum, iki modeli farklı öğrenci türleri ile karşılaştırmaya çalışırken oluşur.  

 Azure Machine Learning'deki bu hata, iki farklı puanlı veri kümesiiçin değerlendirme ölçümleri karşılaştırılamadığunda üretilir. Bu durumda, iki puanlı veri kümesi ni üretmek için kullanılan modellerin etkinliğini karşılaştırmak mümkün değildir.  

**Çözünürlük:** Puanlı sonuçların aynı tür makine öğrenimi modeli (ikili sınıflandırma, regresyon, çok sınıflı sınıflandırma, öneri, kümeleme, anomali algılama, vb.) tarafından üretildiğini doğrulayın Karşılaştırdığınız tüm modeller aynı öğrenci türüne sahip olmalıdır.  

|Özel Durum Mesajları|
|------------------------|
|Tüm modeller aynı öğrenci türüne sahip olmalıdır.|
|Uyumsuz öğrenci türü var: "{actual_learner_type}". Beklenen öğrenci türleri şunlardır: "{expected_learner_type_list}".|


## <a name="error-0064"></a>Hata 0064  
 Azure depolama hesabı adı veya depolama anahtarı yanlış belirtilmişse özel durum oluşur.  

 Azure Makine Öğrenimi'ndeki bu hata, Azure depolama hesabı adı veya depolama anahtarı yanlış belirtilirse oluşur. Depolama hesabı için yanlış bir hesap adı veya parola girerseniz bu hatayı alırsınız. Bu durum, hesap adını veya parolayı el ile girerseniz oluşabilir. Hesap silinmişse de oluşabilir.  

**Çözünürlük:** Hesap adının ve parolasının doğru girildiğini ve hesabın var olduğunu doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Azure depolama hesabı adı veya depolama anahtarı yanlıştır.|
|Azure depolama hesabı adı "{account_name}" veya hesap adının depolama anahtarı yanlıştır.|


## <a name="error-0065"></a>Hata 0065  
 Azure blob adı yanlış belirtilirse özel durum oluşur.  

 Azure Makine Öğrenimi'ndeki bu hata, Azure blob adı yanlış belirtilirse oluşur.  Aşağıdakiler durumunda hata alırsınız:  

-   Blob belirtilen kapsayıcıda bulunamaz.  

 <!---   The fully qualified name of the blob specified for output in one of the [Learning with Counts](data-transformation-learning-with-counts.md) modules is greater than 512 characters.  -->

-   Biçim kodlama lı Excel veya CSV olduğunda, yalnızca kapsayıcı Bir [İçe Aktarma Verisi](import-data.md) isteğinde kaynak olarak belirtilmiştir; bir kapsayıcı içindeki tüm lekelerin içeriğinin içişmesine bu biçimlerle izin verilmez.  
  
-   Bir SAS URI geçerli bir lekenin adını içermez.  

**Çözünürlük:** Özel durum atma modülü yeniden ziyaret edin. Belirtilen blob depolama hesabındaki kapsayıcıda var olduğunu ve izinlerin blob'u görmenizi sağladığını doğrulayın. Kodlama biçimlerine sahip Excel veya CSV'niz varsa, girişin **form kapsayıcı adı/dosya adının** ait olduğunu doğrulayın. Bir SAS URI'nin geçerli bir lekenin adını içerdiğini doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Azure depolama blob adı yanlış.|
|Azure depolama blob adı "{blob_name}" yanlıştır.|
|"{blob_name_prefix}" önekine sahip Azure depolama blob adı yok.|
|"{container_name}" kapsayıcının altında herhangi bir Azure depolama blobları bulunamadı.|
|Joker karakter yolu "{blob_wildcard_path}" olan herhangi bir Azure depolama blobları bulunamadı.|


## <a name="error-0066"></a>Hata 0066  
 Bir kaynak Azure Blob'a yüklenemezse özel durum oluşur.  

 Azure Machine Learning'deki bu hata, bir kaynak Azure Blob'a yüklenemezse oluşur.  <!--You will receive this message if [Train Vowpal Wabbit 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) encounters an error attempting to save either the model or the hash created when training the model.--> Her ikisi de giriş dosyasını içeren hesapla aynı Azure depolama hesabına kaydedilir.  

**Çözünürlük:** Modülü tekrar ziyaret edin. Azure hesap adının, depolama anahtarının ve kapsayıcının doğru olduğunu ve hesabın kapsayıcıya yazma izni olduğunu doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Kaynak Azure depolama alanına yüklenemedi.|
|"{source_path}" dosyası Azure depolama alanına "{dest_path}" olarak yüklenemedi.|


## <a name="error-0067"></a>Hata 0067  
 Özel durum, bir veri kümesinin beklenenden farklı sayıda sütuna sahip olması durumunda oluşur.  

 Azure Machine Learning'deki bu hata, bir veri kümesinde beklenenden farklı sayıda sütun varsa oluşur.  Veri kümesindeki sütun sayısı, modülün yürütme sırasında beklediği sütun sayısından farklı olduğunda bu hatayı alırsınız.  

**Çözünürlük:** Giriş veri kümesini veya parametreleri değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Veri tablosunda beklenmeyen sütun sayısı.|
|Veri kümesinde beklenmeyen sütun sayısı "{dataset_name}".|
|Beklenen "{expected_column_count}" sütun(lar) ancak "{actual_column_count}" sütun(lar) yerine bulundu.|
|"{dataset_name}", "{expected_column_count}" sütun(ları) beklenen ancak "{actual_column_count}" sütun(lar) yerine bulunan giriş veri kümesinde.|


## <a name="error-0068"></a>Hata 0068  
 Belirtilen Hive komut dosyası doğru değilse özel durum oluşur.  

 Azure Machine Learning'deki bu hata, Hive QL komut dosyasında sözdizimi hataları varsa veya Hive yorumlayıcısı sorguyu veya komut dosyasını yürükarırken bir hatayla karşılaşırsa oluşur.  

**Çözünürlük:**

Hive'dan gelen hata iletisi normalde Hata Günlüğü'nde geri bildirilir, böylece belirli bir hataya göre işlem yapabilirsiniz. 

+ Modülü açın ve sorguyu hatalar için inceleyin.  
+ Hadoop kümenizin Hive konsoluna giriş yaparak ve sorguyu çalıştırarak sorgunun Azure Machine Learning dışında doğru çalıştığını doğrulayın.  
+ Yürütülebilir ifadeleri ve yorumları tek bir satırda karıştırmak yerine, Kovan komut dosyanızdaki yorumları ayrı bir satıra yerleştirmeyi deneyin.  

### <a name="resources"></a>Kaynaklar

Makine öğrenimi için Hive sorguları ile ilgili yardım için aşağıdaki makalelere bakın:

+ [Azure Blob Depolama'dan Kovan tabloları oluşturun ve veri yükleyin](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-move-hive-tables)
+ [Hive sorguları içeren tablolardaki verileri keşfedin](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-explore-data-hive-tables)
+ [Hive sorgularını kullanarak bir Hadoop kümesindeki verilerin özelliklerini oluşturma](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-create-features-hive)
+ [SQL Kullanıcıları Için Kovan Hile Sayfası (PDF)](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf)

  
|Özel Durum Mesajları|
|------------------------|
|Kovan komut dosyası yanlış.|


## <a name="error-0069"></a>Hata 0069  
 Belirtilen SQL komut dosyası doğru değilse özel durum oluşur.  

 Azure Machine Learning'deki bu hata, belirtilen SQL komut dosyasında sözdizimi sorunları varsa veya komut dosyasında belirtilen sütunlar veya tablo geçerli değilse oluşur. 

 SQL altyapısı sorguyu veya komut dosyasını çalıştırırken herhangi bir hatayla karşılaşırsa bu hatayı alırsınız. SQL hata iletisi, belirli bir hataya göre işlem yapabilmeniz için normalde Hata Günlüğü'nde geri bildirilir.  

**Çözünürlük:** Modülü yeniden ziyaret edin ve SQL sorgusunu hatalar için inceleyin.  

 Doğrudan veritabanı sunucusuna giriş yaparak ve sorguyu çalıştırarak sorgunun Azure ML dışında doğru çalıştığını doğrulayın.  

 Modül özel durumu tarafından bildirilen bir SQL oluşturulan ileti varsa, bildirilen hataya göre işlem alın. Örneğin, hata iletileri bazen olası hata yla ilgili belirli bir kılavuz içerir:
+ Bir sütun adını yanlış yazmış olabileceğini belirten böyle bir *sütun veya eksik veritabanı yok.* Sütun adının doğru olduğundan eminseniz, sütun tanımlayıcısını içine almak için köşeli ayraçları veya tırnak işaretlerini kullanmayı deneyin.
+ *SQL anahtar \<kelimesinin\>yanındaki SQL mantık hatası*, belirtilen anahtar kelimeden önce sözdizimi hatası olabileceğini belirten

  
|Özel Durum Mesajları|
|------------------------|
|SQL komut dosyası yanlıştır.|
|SQL sorgusu "{sql_query}" doğru değildir.|
|SQL sorgusu "{sql_query}" doğru değildir. Özel durum iletisi: {exception}.|


## <a name="error-0070"></a>Hata 0070  
 Özel durum, var olmayan Azure tablosuna erişmeye çalışırken oluşur.  

 Azure Machine Learning'deki bu hata, var olmayan bir Azure tablosuna erişmeye çalıştığınızda oluşur. Azure Depolama'da okuma veya Yazma'da bulunmayan bir tablo belirtirseniz, bu hatayı alırsınız. İstenilen tablonun adını yanlış yazarsanız veya hedef ad la depolama türü arasında bir uyumsuzluk varsa bu durum olabilir. Örneğin, bir tablodan okumayı amaçladın, ancak bunun yerine bir lekenin adını girdiniz.  

**Çözünürlük:** Tablonun adının doğru olduğunu doğrulamak için modülü yeniden ziyaret edin.  

|Özel Durum Mesajları|
|------------------------|
|Azure tablosu yok.|
|Azure tablosu "{table_name}" yok.|


## <a name="error-0072"></a>Hata 0072  
 Özel durum, bağlantı zaman durumunda oluşur.  

 Azure Machine Learning'deki bu hata, bağlantı zaman ları geçtiğinde oluşur. Yavaş internet bağlantısı gibi veri kaynağı veya hedefle ilgili şu anda bağlantı sorunları varsa veya veri kümesi büyükse ve/veya verilerde okunacak SQL sorgusu karmaşık işleme gerçekleştiriyorsa, bu hatayı alırsınız.  

**Çözünürlük:** Şu anda Azure depolama veya Internet'e yavaş bağlantılarla ilgili sorunlar olup olmadığını belirleyin.  

|Özel Durum Mesajları|
|------------------------|
|Bağlantı zaman oluştu.|


## <a name="error-0073"></a>Hata 0073  
 Bir sütunu başka bir türe dönüştürürken bir hata oluşursa özel durum oluşur.  

 Azure Machine Learning'deki bu hata, sütunu başka bir türe dönüştürmek mümkün olmadığında oluşur.  Bir modül belirli bir tür gerektiriyorsa ve sütunu yeni türe dönüştürmek mümkün değilse, bu hatayı alırsınız.  

**Çözünürlük:** Sütuniç özel duruma göre dönüştürülecek şekilde giriş veri kümesini değiştirin.  

|Özel Durum Mesajları|
|------------------------|
|Sütunu dönüştürmek için başarısız oldu.|
|Sütunu {target_type} olarak dönüştürmek için başarısız oldu.|


## <a name="error-0075"></a>Hata 0075  
Özel durum, bir veri kümesini ölçerken geçersiz bir binning işlevi kullanıldığında oluşur.  

Azure Machine Learning'deki bu hata, desteklenmeyen bir yöntem kullanarak veri çöp çalınması na çalışırken veya parametre birleşimleri geçersiz olduğunda oluşur.  

**Çözünürlük:**

Bu etkinlik için hata işleme, Binning yöntemlerinin daha fazla özelleştirilmesine olanak sağlayan Azure Machine Learning'in önceki bir sürümünde tanıtıldı. Şu anda tüm binning yöntemleri açılır listeden bir seçim dayanmaktadır, bu nedenle teknik olarak artık bu hatayı almak mümkün olmalıdır.

 <!--If you get this error when using the [Group Data into Bins](group-data-into-bins.md) module, consider reporting the issue in the [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MachineLearning), providing the data types, parameter settings, and the exact error message.  -->

|Özel Durum Mesajları|
|------------------------|
|Geçersiz binning işlevi kullanılır.|


## <a name="error-0077"></a>Hata 0077  
 Özel durum bilinmeyen blob dosyası yazma modu geçtiğinde oluşur.  

 Azure Machine Learning'deki bu hata, bir blob dosyası hedefi veya kaynağının belirtimlerinde geçersiz bir bağımsız değişken geçirilirse oluşur.  

**Çözünürlük:** Azure blob depolamasına veri aktaran veya dışa aktaran hemen hemen tüm modüllerde, yazma modunu kontrol eden parametre değerleri açılır liste kullanılarak atanır; bu nedenle, geçersiz bir değer geçmek mümkün değildir ve bu hata görünmemelidir. Bu hata daha sonraki bir sürümde amortismana alınacaktır.  

|Özel Durum Mesajları|
|------------------------|
|Desteklenmeyen blob yazma modu.|
|Desteklenmeyen blob yazma modu: {blob_write_mode}.|


## <a name="error-0078"></a>Hata 0078  
 Özel durum, [İçe Aktar Verileri](import-data.md) için HTTP seçeneği yeniden yönlendirmeyi gösteren bir 3xx durum kodu aldığında oluşur.  

 Azure Machine Learning'deki bu hata, [Alma Verileri](import-data.md) için HTTP seçeneği yeniden yönlendirmeyi gösteren bir 3xx (301, 302, 304, vb.) durum kodu aldığında oluşur. Tarayıcıyı başka bir sayfaya yönlendiren bir HTTP kaynağına bağlanmaya çalışırsanız bu hatayı alırsınız. Güvenlik nedeniyle, Azure Machine Learning için veri kaynağı olarak web sitelerini yönlendirmeye izin verilmez.  

**Çözünürlük:** Web sitesi güvenilir bir web sitesiyse, doğrudan yönlendirilen URL'yi girin.  

|Özel Durum Mesajları|
|------------------------|
|Http yönlendirmeye izin verilmiyor.|


## <a name="error-0079"></a>Hata 0079  
 Azure depolama kapsayıcı adı yanlış belirtilirse özel durum oluşur.  

 Azure Depolama kapsayıcı adı yanlış belirtilirse Azure Machine Learning'deki bu hata oluşur. Azure Blob Depolama'ya yazarken **kapsayıcı seçeneğiyle başlayan blob yolunu** kullanarak hem kapsayıcıyı hem de blob (dosya) adını belirtmediyseniz bu hatayı alırsınız.  

**Çözünürlük:** [Dışa Aktarma Verileri](export-data.md) modüllerini yeniden ziyaret edin ve blob'a giden belirtilen yolun **kapsayıcı/dosya adı**biçiminde hem kapsayıcı hem de dosya adını içerdiğini doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Azure depolama kapsayıcıadı yanlış.|
|Azure depolama kapsayıcı adı "{container_name}" yanlıştır; biçim kapsayıcı /blob bir kapsayıcı adı bekleniyordu.|


## <a name="error-0080"></a>Hata 0080  
 Özel durum, tüm değerlerin eksik olduğu sütun un modül tarafından izin verilmemesi yle oluşur.  

 Azure Machine Learning'deki bu hata, modül tarafından tüketilen sütunlardan biri veya birkaçı eksik değerleri içerdiğinde oluşur. Örneğin, bir modül her sütun için toplu istatistikleri hesaplıyorsa, veri içermeyen bir sütun üzerinde çalışamaz. Bu gibi durumlarda, modül yürütme bu istisna ile durdurulur.  

**Çözünürlük:** Giriş veri kümesini yeniden ziyaret edin ve tüm eksik değerleri içeren sütunları kaldırın.  

|Özel Durum Mesajları|
|------------------------|
|Tüm değerlerin eksik olduğu sütunlara izin verilmez.|
|Sütun {col_index_or_name} tüm değerleri eksik vardır.|


## <a name="error-0081"></a>Hata 0081  
 Azaltılacagi boyut sayisi, en az bir seyrek özellik sütunu içeren giriş veri kümesindeki özellik sütunlarının sayiina eşitse, PCA modülünde istisna oluşur.  

 Azure Machine Learning'deki bu hata, aşağıdaki koşullar yerine getirildiğinde oluşur: (a) giriş veri kümesinin en az bir seyrek sütunu vardır ve (b) istenen son boyut sayısı giriş boyutlarının sayısıyla aynıdır.  

**Çözünürlük:** Girişteki boyut sayısından daha az olması için çıktıdaki boyut sayısını azaltmayı düşünün. Bu PCA uygulamalarında tipiktir.   <!--For more information, see [Principal Component Analysis](principal-component-analysis.md).  -->

|Özel Durum Mesajları|
|------------------------|
|Seyrek özellik sütunları içeren veri kümesi için azaltmak için boyut sayısı özellik sütun sayısından daha az olmalıdır.|


## <a name="error-0082"></a>Hata 0082  
 Özel durum, bir model başarıyla deserialized olamaz oluşur.  

 Azure Machine Learning'deki bu hata, kaydedilmiş bir makine öğrenimi modeli veya dönüşümü, çığır açan bir değişikliğin sonucu olarak Azure Machine Learning çalışma zamanının daha yeni bir sürümü tarafından yüklenemediğinde oluşur.  

**Çözünürlük:** Modeli veya dönüşümü oluşturan eğitim ardışık hattı yeniden çalıştırılmalı ve model veya dönüştürme yeniden kaydedilmelidir.  

|Özel Durum Mesajları|
|------------------------|
|Model, büyük olasılıkla eski bir serileştirme biçimiyle seri hale getirilebileceğinden deserialized olamazdı. Lütfen modeli yeniden eğitin ve yeniden kaydedin.|


## <a name="error-0083"></a>Hata 0083  
 Özel durum, eğitim için kullanılan veri kümesinin somut öğrenci türü için kullanılamıyorsa oluşur.  

 Azure Machine Learning'deki bu hata, veri kümesi eğitilen öğrenciyle uyumsuz olduğunda oluşur. Örneğin, veri kümesi her satırda en az bir eksik değer içerebilir ve sonuç olarak, tüm veri kümesi eğitim sırasında atlanır. Diğer durumlarda, anomali algılama gibi bazı makine öğrenme algoritmaları etiketlerin bulunmasını beklemez ve veri kümesinde etiketler varsa bu özel durumu atabilir.  

**Çözünürlük:** Giriş veri kümesi gereksinimlerini denetlemek için kullanılan öğrencinin belgelerine başvurun. Gerekli tüm sütunların mevcut olduğunu görmek için sütunları inceleyin.  

|Özel Durum Mesajları|
|------------------------|
|Eğitim için kullanılan veri kümesi geçersizdir.|
|{data_name} eğitim için geçersiz veriler içerir.|
|{data_name} eğitim için geçersiz veriler içerir. Öğrenci türü: {learner_type}.|
|{data_name} eğitim için geçersiz veriler içerir. Öğrenci türü: {learner_type}. Sebep: {neden}.|
|Eğitim verileri {data_name}'ye "{action_name}" eylemi uygulanamadı. Sebep: {neden}.|


## <a name="error-0084"></a>Hata 0084  
 R Script'ten üretilen puanlar değerlendirildiğinde özel durum oluşur. Bu şu anda desteklenmiş.  

 Azure Machine Learning'deki bu hata, puanları içeren bir R komut dosyasından çıktı içeren bir modeli değerlendirmek için modüllerden birini kullanmaya çalışırsanız oluşur.  

**Çözünürlük:**

|Özel Durum Mesajları|
|------------------------|
|Özel Model tarafından üretilen puanların değerlendirilmesi şu anda desteklenmemektedir.|


## <a name="error-0085"></a>Hata 0085  
 Özel durum, komut dosyası değerlendirmesi bir hatayla başarısız olduğunda oluşur.  

 Azure Machine Learning'deki bu hata, sözdizimi hataları içeren özel komut dosyası çalıştırdığınızda oluşur.  

**Çözünürlük:** Kodunuzu harici bir düzenleyicide gözden geçirin ve hataları denetleyin.  

|Özel Durum Mesajları|
|------------------------|
|Komut dosyasının değerlendirilmesi sırasında hata.|
|Komut dosyası değerlendirmesi sırasında aşağıdaki hata oluştu, daha fazla bilgi için lütfen çıktı günlüğünü görüntüleyin:<br />{script_language} yorumlayıcısından ---------- Hata ---------- Başlangıç<br />{ileti}<br />{script_language} yorumlayıcısından ---------- Hata Sonu iletisi ----------|


## <a name="error-0090"></a>Hata 0090  
 Özel durum, Hive tablosu oluşturma başarısız olduğunda oluşur.  

 Azure Machine Learning'deki bu hata, Verileri BIR HDInsight kümesine kaydetmek için [Dışa Aktarma Verilerini](export-data.md) veya başka bir seçeneği kullandığınızda oluşur ve belirtilen Hive tablosu oluşturulamaz.  

**Çözünürlük:** Kümeyle ilişkili Azure depolama hesabı adını kontrol edin ve modül özelliklerinde aynı hesabı kullandığınızı doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Kovan tablosu oluşturulamadı. BIR HDInsight kümesi için, kümeyle ilişkili Azure depolama hesabı adının modül parametresinden geçirilenlerle aynı olduğundan emin olun.|
|Kovan tablosu "{table_name}" oluşturulamadı. BIR HDInsight kümesi için, kümeyle ilişkili Azure depolama hesabı adının modül parametresinden geçirilenlerle aynı olduğundan emin olun.|
|Kovan tablosu "{table_name}" oluşturulamadı. BIR HDInsight kümesi için, kümeyle ilişkili Azure depolama hesabı adının "{cluster_name}" olduğundan emin olun.|


## <a name="error-0102"></a>Hata 0102  
 ZIP dosyası ayıklanamadığında atılır.  

 Azure Machine Learning'deki bu hata, .zip uzantılı sıkıştırılmış bir paket içe aktarırken oluşur, ancak paket zip dosyası değildir veya dosya desteklenen zip biçimi kullanmaz.  

**Çözünürlük:** Seçili dosyanın geçerli bir .zip dosyası olduğundan ve desteklenen sıkıştırma algoritmalarından biri kullanılarak sıkıştırılmış olduğundan emin olun.  

 Veri kümelerini sıkıştırılmış biçimde aktarırken bu hatayı alırsanız, içerdiği tüm dosyaların desteklenen dosya biçimlerinden birini kullandığını ve Unicode biçiminde olduğunu doğrulayın.  <!--For more information, see [Unpack Zipped Datasets](unpack-zipped-datasets.md).  -->

 İstenilen dosyaları yeni sıkıştırılmış sıkıştırılmış klasöre okumayı deneyin ve özel modülü yeniden eklemeyi deneyin.  

|Özel Durum Mesajları|
|------------------------|
|Verilen ZIP dosyası doğru biçimde değildir.|


## <a name="error-0105"></a>Hata 0105  
 Modül tanım dosyası desteklenmeyen parametre türü içeriyorsa bu hata görüntülenir  
  
 Azure Machine Learning'deki bu hata, özel bir modül xml tanımı oluşturduğunuzda ve tanımdaki parametre veya bağımsız değişken türü desteklenen bir türle eşleşmediğinde oluşturulur.  
  
**Çözünürlük:** Özel modül xml tanım dosyasındaki herhangi bir **Arg** öğesinin tür özelliğinin desteklenen bir tür olduğundan emin olun.  
  
|Özel Durum Mesajları|  
|------------------------|  
|Desteklenmeyen parametre türü.|  
|Desteklenmeyen parametre{0}türü ' ' belirtilir.|  


## <a name="error-0107"></a>Hata 0107  
 Modül tanım dosyası desteklenmeyen bir çıktı türünü tanımladığında atılan  
  
 Azure Machine Learning'deki bu hata, özel bir modül xml tanımındaki çıkış bağlantı noktası türü desteklenen bir türle eşleşmediğinde oluşur.  
  
**Çözünürlük:** Özel modül xml tanım dosyasındaki bir Çıktı öğesinin tür özelliğinin desteklenen bir tür olduğundan emin olun.  
  
|Özel Durum Mesajları|  
|------------------------|  
|Desteklenmeyen çıktı türü.|  
|Desteklenmeyen çıktı türü '{output_type}' belirtilir.|  


## <a name="error-0125"></a>Hata 0125  
 Birden çok veri kümesi için şema eşleşmediğinde atılan.  

**Çözünürlük:**

|Özel Durum Mesajları|
|------------------------|
|Dataset şema eşleşmiyor.|


## <a name="error-0127"></a>Hata 0127  
 Görüntü piksel boyutu izin verilen sınırı aşıyor  

 Bu hata, sınıflandırma için bir görüntü veri kümesinden görüntüleri okuyorsanız ve görüntüler modelin işleyebilir daha büyük ise oluşur.  

 <!--**Resolution:**
 For more information about the image size and other requirements, see these topics:  

-   [Import Images](import-images.md)  
  
-   [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md)  -->

|Özel Durum Mesajları|
|------------------------|
|Görüntü piksel boyutu izin verilen sınırı aşıyor.|
|'{file_path}' dosyasındaki görüntü piksel boyutu izin verilen sınırı aşıyor: '{size_limit}.'|


## <a name="error-0128"></a>Hata 0128  
 Kategorik sütunlar için koşullu olasılık sayısı sınırı aşıyor.  

**Çözünürlük:**

|Özel Durum Mesajları|
|------------------------|
|Kategorik sütunlar için koşullu olasılık sayısı sınırı aşıyor.|
|Kategorik sütunlar için koşullu olasılık sayısı sınırı aşıyor. '{column_name_or_index_1}' ve '{column_name_or_index_2}' sütunları sorunlu çifttir.|


## <a name="error-0129"></a>Hata 0129  
 Veri kümesindeki sütun sayısı izin verilen sınırı aşıyor.  

**Çözünürlük:**

|Özel Durum Mesajları|
|------------------------|
|Veri kümesindeki sütun sayısı izin verilen sınırı aşıyor.|
|'{dataset_name}' veri kümesindeki sütun sayısı izin verileni aşıyor.|
|'{dataset_name}' veri kümesindeki sütun sayısı izin verilen '{component_name}' sınırını aşıyor.|
|'{dataset_name}' veri kümesindeki sütun sayısı izin verilen '{limit_columns_count}' sınırını aşıyor '{component_name}..|


## <a name="error-0134"></a>Hata 0134
Özel durum, etiket sütunu eksik olduğunda veya etiketlenmiş satır sayısı yetersizolduğunda oluşur.  

Bu hata, modül bir etiket sütunu gerektirdiğinde oluşur, ancak sütun seçimine bir tane eklemediğinizveya etiket sütununda çok fazla değer eksik tir.

Bu hata, önceki bir işlem veri kümesini değiştirdiğinde, alt akış işleminde yetersiz satırlar kullanılabilir hale geldiğinde de oluşabilir. Örneğin, bir veri kümesini değerlere bölmek için **Bölüm ve Örnek** modülünde bir ifade kullandığınızı varsayalım. İfadeniz için eşleşme bulunmazsa, bölümden kaynaklanan veri kümelerinden biri boş olur.

Çözüm: 

 Sütun seçimine bir etiket sütunu eklerseniz ancak bu sütun tanınmıyorsa, etiket sütunu olarak işaretlemek için [Meta Verileri Edit](edit-metadata.md) modüllerini kullanın.

  <!--Use the [Summarize Data](summarize-data.md) module to generate a report that shows how many values are missing in each column. -->
  Ardından, etiket sütununda eksik değerlere sahip satırları kaldırmak için [Eksik Verileri Temizle](clean-missing-data.md) modüllerini kullanabilirsiniz. 

 Geçerli veri ve işlemin gereksinimlerini karşılamak için yeterli satır içerdiğinden emin olmak için giriş veri kümelerinizi denetleyin. Birçok algoritma, bazı minimum sayıda veri satırı gerektiriyorsa bir hata iletisi oluşturur, ancak veriler yalnızca birkaç satır veya yalnızca bir üstbilgi içerir.

|Özel Durum Mesajları|
|------------------------|
|Özel durum, etiket sütunu eksik olduğunda veya etiketlenmiş satır sayısı yetersizolduğunda oluşur.|
|Özel durum, etiket sütunu eksik olduğunda veya {required_rows_count} etiketli satırlardan az olduğunda oluşur.|
|Özel durum, veri kümesi {dataset_name} etiket sütunu eksik olduğunda veya {required_rows_count} etiketli satırlardan az olduğunda oluşur.|


## <a name="error-0138"></a>Hata 0138  
 Bellek tükenmiş, modülün çalışmasını tamamlayamadı. Veri kümesinin alt örneklemesi sorunu hafifletmeye yardımcı olabilir.  

 Bu hata, çalışan modül Azure kapsayıcısında bulunandan daha fazla bellek gerektirdiğinde oluşur. Büyük bir veri kümesiyle çalışıyorsanız ve geçerli işlem belleğe sığmıyorsa bu durum olabilir.  

**Çözünürlük:** Büyük bir veri kümesini okumaya çalışıyorsanız ve işlem tamamlanamıyorsa, veri kümesinin alt örneklemesi yardımcı olabilir.  

  <!--If you use the visualizations on datasets to check the cardinality of columns, only some rows are sampled. To get a full report, use [Summarize Data](summarize-data.md). You can also use the [Apply SQL Transformation](apply-sql-transformation.md) to check for the number of unique values in each column.  

 Sometimes transient loads can lead to this error. Machine support also changes over time. 

 Try using [Principal Component Analysis](principal-component-analysis.md) or one of the provided feature selection methods to reduce your dataset to a smaller set of more feature-rich columns: [Feature Selection](feature-selection-modules.md)  -->

|Özel Durum Mesajları|
|------------------------|
|Bellek tükenmiş, modülün çalışmasını tamamlayamadı.|
|Bellek tükenmiş, modülün çalışmasını tamamlayamadı. Ayrıntılar: {ayrıntılar}|


## <a name="error-0141"></a>Hata 0141  
 Kategorik ve dize sütunlarında seçili sayısal sütunların ve benzersiz değerlerin sayısı çok küçükse özel durum oluşur.  

 Azure Machine Learning'deki bu hata, seçili sütunda işlemi gerçekleştirmek için yeterli benzersiz değer olmadığında oluşur.  

**Çözünürlük:** Bazı işlemler özellik ve kategorik sütunlarda istatistiksel işlemler gerçekleştirir ve yeterli değer yoksa, işlem başarısız olabilir veya geçersiz bir sonuç döndürebilir. Özellik ve etiket sütunlarında kaç değer olduğunu görmek için veri kümenizi denetleyin ve gerçekleştirmeye çalıştığınız işlemin istatistiksel olarak geçerli olup olmadığını belirleyin.  

 Kaynak veri kümesi geçerliyse, bazı akış yukarı veri işleme veya meta veri işleminin verileri değiştirip değiştirmediğini ve bazı değerleri kaldırıp kaldırmadığını da denetleyebilirsiniz.  

 Akış yukarı işlemleri bölme, örnekleme veya yeniden örnekleme içeriyorsa, çıktıların beklenen satır ve değer sayısını içerdiğini doğrulayın.  

|Özel Durum Mesajları|
|------------------------|
|Kategorik ve dize sütunlarında seçilen sayısal sütunların ve benzersiz değerlerin sayısı çok küçüktür.|
|Seçili sayısal sütunların toplam sayısı ve kategorik ve dize sütunlarında (şu anda {actual_num}) bulunan benzersiz değerler en az {lower_boundary}olmalıdır.|


## <a name="error-0154"></a>Hata 0154  
 Özel durum, kullanıcı uyumsuz sütun türüne sahip anahtar sütunlarda veri birleştirmeye çalıştığında oluşur.

|Özel Durum Mesajları|
|------------------------|
|Anahtar sütun öğesi türleri uyumlu değildir.|
|Anahtar sütun öğesi türleri uyumlu değildir. (sol: {keys_left}; sağ: {keys_right})|


## <a name="error-0155"></a>Hata 0155  
 Özel durum, veri kümesinin sütun adları dize olmadığında oluşur.

|Özel Durum Mesajları|
|------------------------|
|Veri çerçevesi sütun adı dize türü olmalıdır. Sütun adları dize değildir.|
|Veri çerçevesi sütun adı dize türü olmalıdır. Sütun adları {column_names} dize değildir.|


## <a name="error-0156"></a>Hata 0156  
 Özel durum, Azure SQL Veritabanı'ndan veri okunamayınca oluşur.

|Özel Durum Mesajları|
|------------------------|
|Azure SQL Veritabanı'ndan veri okunamadı.|
|Azure SQL Veritabanı'ndan veri okunamadı: {detailed_message} DB: {database_server_name}:{database_name} Sorgu: {sql_statement}|


## <a name="error-0157"></a>Hata 0157  
 Datastore bulunamadı.

|Özel Durum Mesajları|
|------------------------|
|Datastore bilgileri geçersizdir.|
|Datastore bilgileri geçersizdir. Çalışma alanında '{datastore_name}' olan AzureML veri deposu '{workspace_name}' alınamadı.|


## <a name="error-0158"></a>Hata 0158
 Dönüşüm dizini geçersiz olduğunda atılır.

|Özel Durum Mesajları|
|------------------------------------------------------------|
|Verilen Dönüşüm Directory geçersizdir.|
|Dönüşüm Dizin "{arg_name}" geçersizdir. Sebep: {neden}. Lütfen Dönüşüm dosyasını oluşturan eğitim denemesini yeniden yayın. Eğitim denemesi silindiyse, lütfen Dönüştür dosyasını yeniden oluşturun ve kaydedin.|


## <a name="error-1000"></a>Hata 1000  
İç kitaplık özel durumu.  

Bu hata, başka türlü işlenmemiş iç altyapı hatalarını yakalamak için sağlanır. Bu nedenle, bu hatanın nedeni hatayı oluşturan modüle bağlı olarak farklı olabilir.  

Daha fazla yardım almak için, hatayla birlikte Azure Machine Learning forumuna, girdi olarak kullanılan veriler de dahil olmak üzere senaryonun açıklamasıyla birlikte ayrıntılı ileti göndermenizi öneririz. Bu geri bildirim, hataları önceliklendirmemize ve daha fazla çalışma için en önemli sorunları belirlememize yardımcı olacaktır.  

|Özel Durum Mesajları|
|------------------------|
|Kitaplık özel durumu.|
|Kitaplık özel durumu: {özel durum}.|
|Bilinmeyen kitaplık özel durumu: {exception}. {customer_support_guidance}.|

