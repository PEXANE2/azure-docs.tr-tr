---
title: BLOB depolama ve Azure Stream Analytics Azure Data Lake Gen2 çıkışı
description: Bu makalede BLOB depolama ve Azure Stream Analytics için çıkış olarak Azure Data Lake Gen 2 açıklanır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 2ab45f4c64e6993f70f08f04ee413211abb0307d
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88875908"
---
# <a name="blob-storage-and-azure-data-lake-gen2-output-from-azure-stream-analytics"></a>BLOB depolama ve Azure Stream Analytics Azure Data Lake Gen2 çıkışı

Data Lake Storage 2. Azure Storage 'ı Azure 'da kurumsal veri oluşturma için temel oluşturur. Başlangıçtan itibaren yüzlerce Gigabit işleme petabaytlarca, çok büyük miktarlarda veriyi kolay bir şekilde yönetmenize olanak tanılarken, Data Lake Storage 2. birden fazla bilgi için başlangıç sürümünden tasarlanan. Data Lake Storage 2. temel bir parçası, blob depolamaya hiyerarşik bir ad alanının eklenmesinin bir parçasıdır.

Azure Blob depolama, büyük miktarlarda yapılandırılmamış verileri bulutta depolamak için uygun maliyetli ve ölçeklenebilir bir çözüm sunar. BLOB depolama ve kullanım kullanımıyla ilgili bir giriş için bkz. [Azure Portal blob 'Ları yükleme, indirme ve listeleme](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="output-configuration"></a>Çıkış yapılandırması

Aşağıdaki tabloda, bir blob veya ADLS 2. çıkışı oluşturmak için özellik adları ve bunların açıklamaları listelenmektedir.

| Özellik adı       | Açıklama                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| Çıktı diğer adı        | Sorgu çıkışını bu blob depolamaya yönlendirmek için sorgularda kullanılan kolay bir ad. |
| Depolama hesabı     | Çıktlarınızı gönderdiğiniz depolama hesabının adı.               |
| Depolama hesabı anahtarı | Depolama hesabıyla ilişkili gizli anahtar.                              |
| Depolama kapsayıcısı   | Azure Blob hizmetinde depolanan BLOB 'lar için mantıksal gruplama. Blob hizmetine bir blob yüklediğinizde, o blob için bir kapsayıcı belirtmeniz gerekir. |
| Yol deseni | İsteğe bağlı. Belirtilen kapsayıcı içinde bloblarınızı yazmak için kullanılan dosya yolu deseninin. <br /><br /> Yol modelinde, Blobların yazıldığı sıklığı belirtmek için tarih ve saat değişkenlerinin bir veya daha fazla örneğini kullanmayı seçebilirsiniz: <br /> {date}, {Time} <br /><br />Blob 'ları bölümlemek için olay verilerinize özel bir {Field} adı belirtmek için özel blob bölümlemesini kullanabilirsiniz. Alan adı alfasayısal olur ve boşluk, kısa çizgi ve alt çizgi içerebilir. Özel alanlardaki kısıtlamalar şunlardır: <ul><li>Alan adları büyük/küçük harfe duyarlı değildir. Örneğin, hizmet "ID" sütununu ve "ID" sütununu ayırt edemiyor.</li><li>İç içe alanlara izin verilmez. Bunun yerine, alanı "düzleştirmek" için iş sorgusunda bir diğer ad kullanın.</li><li>İfadeler alan adı olarak kullanılamaz.</li></ul> <br />Bu özellik, yoldaki özel tarih/saat biçimi belirleyici yapılandırmalarının kullanılmasını mümkün. Özel tarih ve saat biçimleri tek seferde belirtilmelidir ve {DateTime: \<specifier> } anahtar sözcüğü ile alınmıştır. İçin izin verilen girişler \<specifier> yyyy, mm, m, gg, d, hh, H, mm, m, SS veya s şeklindedir. {DateTime: \<specifier> } anahtar sözcüğü, özel tarih/saat yapılandırması oluşturmak için yolunda birden çok kez kullanılabilir. <br /><br />Örnekler: <ul><li>Örnek 1: Cluster1/logs/{Date}/{Time}</li><li>Örnek 2: Cluster1/logs/{Date}</li><li>Örnek 3: Cluster1/{client_id}/{Date}/{Time}</li><li>Örnek 4: Cluster1/{DateTime: ss}/{myField} burada sorgunun olduğu yer: Data. myField ' ı girişte myField olarak BELIRLEYIN;</li><li>Örnek 5: Cluster1/Year = {DateTime: yyyy}/ay = {DateTime: DD}/gün = {DateTime: dd}</ul><br />Oluşturulan klasör yapısının zaman damgası UTC ve yerel saate göre değil.<br /><br />Dosya adlandırma aşağıdaki kuralı kullanır: <br /><br />{Path önek kalıbı}/schemaHashcode_Guid_Number. Extension<br /><br />Örnek çıkış dosyaları:<ul><li>Mbir put/20170901/00/45434_gguid_1.csv</li>  <li>Mbir put/20170901/01/45434_gguid_1.csv</li></ul> <br />Bu özellik hakkında daha fazla bilgi için bkz. [özel blob çıkış bölümlendirme Azure Stream Analytics](stream-analytics-custom-path-patterns-blob-storage-output.md). |
| Tarih biçimi | İsteğe bağlı. Ön ek yolunda Tarih belirteci kullanılıyorsa, dosyalarınızın düzenlendiği tarih biçimini seçebilirsiniz. Örnek: YYYY/AA/GG |
| Saat biçimi | İsteğe bağlı. Ön ek yolunda zaman belirteci kullanılıyorsa, dosyalarınızın düzenlendiği saat biçimini belirtin. Şu anda desteklenen tek değer HH 'dir. |
| Olay serileştirme biçimi | Çıkış verileri için serileştirme biçimi. JSON, CSV, avro ve Parquet destekleniyor. |
|Minimum satırlar |Toplu iş başına en az satır sayısı. Parquet için her Batch yeni bir dosya oluşturur. Geçerli varsayılan değer 2.000 satırdır ve izin verilen en fazla 10.000 satır olur.|
|En uzun süre |Toplu iş başına en fazla bekleme süresi. Bu süreden sonra, en düşük satır gereksinimi karşılanmasa bile toplu iş çıktıya yazılır. Geçerli varsayılan değer 1 dakikadır ve izin verilen en fazla 2 saattir. Blob çıktınızdan yol deseninin sıklığı varsa, bekleme süresi bölüm saat aralığından daha yüksek olamaz.|
| Encoding    | CSV veya JSON biçimi kullanıyorsanız, bir kodlama belirtilmesi gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8 ' i destekler. |
| Sınırlayıcı   | Yalnızca CSV serileştirme için geçerlidir. Stream Analytics CSV verilerini seri hale getirmek için bir dizi ortak sınırlayıcıları destekler. Desteklenen değerler virgül, noktalı virgül, boşluk, sekme ve dikey çubuklardır. |
| Biçimlendir      | Yalnızca JSON serileştirme için geçerlidir. **Satır ayrımı** , çıktının her bir JSON nesnesine yeni bir satırla ayrılmış şekilde biçimlendirildiğini belirtir. **Satır ayrılmış**' i SEÇERSENIZ, JSON tek seferde bir nesne okur. Tüm içerik geçerli bir JSON değil. **Dizi** , çıktının JSON nesneleri dizisi olarak biçimlendirildiğini belirtir. Bu dizi yalnızca iş durdurulduğunda veya Stream Analytics bir sonraki zaman penceresine taşındığında kapatılır. Genel olarak, çıkış dosyası hala üzerine yazılırken özel bir işleme gerektirmediğinden, hat ayrımı olan JSON kullanılması tercih edilir. |

## <a name="blob-output-files"></a>Blob çıkış dosyaları

Blob depolamayı çıkış olarak kullanırken, blob 'da aşağıdaki durumlarda yeni bir dosya oluşturulur:

* Dosya izin verilen maksimum blok sayısını aşarsa (Şu anda 50.000). İzin verilen en fazla blok sayısına ulaşmadan izin verilen en fazla blok sayısına ulaşmanız gerekir. Örneğin, çıkış oranı yüksekse, blok başına daha fazla bayt görebilirsiniz ve dosya boyutu daha büyüktür. Çıkış hızı düşükse, her bloğun daha az verileri vardır ve dosya boyutu daha küçüktür.
* Çıktıda bir şema değişikliği varsa ve çıkış biçimi sabit şema (CSV ve avro) gerektirir.
* Bir iş yeniden başlatılırsa, bir kullanıcı tarafından durduruluyor ve başlatıldığında ya da sistem bakımı veya hata kurtarma için dahili olarak.
* Sorgu tamamen bölümlenmiş ise ve her bir çıkış bölümü için yeni bir dosya oluşturulur.
* Kullanıcı bir dosyayı veya depolama hesabının kapsayıcısını silerse.
* Çıkış zaman yol ön eki düzenine göre bölümlenmiş ise ve sorgu bir sonraki saate geçerse yeni bir blob kullanılır.
* Çıkış özel bir alanla bölümlense ve yoksa, bölüm anahtarı başına yeni bir blob oluşturulur.
* Çıkış, bölüm anahtarı kardinalitesi 8.000 ' i aşarsa ve bölüm anahtarı başına yeni bir blob oluşturulduysa özel bir alanla bölümlenmiştir.

## <a name="partitioning"></a>Bölümleme

Bölüm anahtarı için yol deseninin olay alanlarınızın {Date} ve {Time} belirteçlerini kullanın. YYYY/AA/GG, GG/AA/YYYY veya AA-GG-YYYY gibi bir tarih biçimi seçin. SS, saat biçimi için kullanılır. Blob çıktısı, {FieldName} veya {DateTime:} tek bir özel olay özniteliğiyle bölümlenebilir \<specifier> . Çıktı yazıcılarının sayısı, [tam paralelleştirilebilir sorgular](stream-analytics-scale-jobs.md)için giriş bölümlemesini izler.

## <a name="output-batch-size"></a>Çıkış toplu iş boyutu

En büyük ileti boyutu için bkz. [Azure depolama sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits). En büyük blob blok boyutu 4 MB ve maksimum blob Bock sayısı 50.000 ' dir. |

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı başlangıç: Azure CLı kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-cli.md)
* [Hızlı başlangıç: ARM şablonu kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-resource-manager.md)
* [Hızlı başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Hızlı başlangıç: Visual Studio Code Azure Stream Analytics iş oluşturma](quick-create-vs-code.md)
