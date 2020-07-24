---
title: "Kavramlar: FHıR için Azure API 'sinin IoT Bağlayıcısı (Önizleme) özelliğinde veri akışı"
description: IoT bağlayıcısının veri akışını anlayın. IoT Bağlayıcısı, verileri normalleştirir, gruplar, dönüştürür ve ıhır için Azure API 'sine yeniden sürdürür.
services: healthcare-apis
author: ms-puneet-nagpal
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: conceptual
ms.date: 05/13/2020
ms.author: punagpal
ms.openlocfilehash: c2d150fcd35bc51478a1d7f4a0407abce1446c06
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100279"
---
# <a name="iot-connector-preview-data-flow"></a>IoT Bağlayıcısı (Önizleme) veri akışı

Bu makalede IoT Bağlayıcısı 'nda veri akışına genel bakış sunulmaktadır. IoT Bağlayıcısı 'nda cihaz verilerini FHıR tabanlı [izleme kaynaklarına dönüştüren](https://www.hl7.org/fhir/observation.html) farklı veri işleme aşamaları hakkında bilgi edineceksiniz.

![IoT Bağlayıcısı veri akışı](media/concepts-iot-data-flow/iot-connector-data-flow.png)

Yukarıdaki diyagramda IoT Bağlayıcısı içindeki farklı veri akışı aşamaları gösterilmektedir. 

## <a name="ingest"></a>Alma
Alma, cihaz verilerinin IoT bağlayıcısına alındığı ilk aşamadır. Cihaz verileri için alma uç noktası bir [Azure Olay Hub 'ında](https://docs.microsoft.com/azure/event-hubs/)barındırılır. Azure Olay Hub 'ı platformu, saniyede milyonlarca iletiyi alıp işleyebilme olanağı sunan yüksek ölçekli ve üretilen işi destekler. Ayrıca IoT bağlayıcısının iletileri zaman uyumsuz olarak kullanmasını sağlar ve cihaz verilerinin işlenmesi sırasında cihazların beklemesi gereksinimini ortadan kaldırır.

> [!NOTE]
> JSON, cihaz verileri için şu anda desteklenen tek biçimdir.

## <a name="normalize"></a>Normalleştir
Normalleştir, cihaz verilerinin yukarıdaki Azure Olay Hub 'ından alındığı ve cihaz eşleme şablonları kullanılarak işlendiği bir sonraki aşamadır. Bu eşleme süreci, cihaz verilerinin Normalleştirilmemiş bir şemaya dönüştürülmesiyle sonuçlanır. 

Normalleştirme işlemi, sonraki aşamalarda yalnızca veri işlemeyi basitleştirir, ancak aynı zamanda bir giriş iletisini birden çok normalleştirilmiş ileti halinde proje yeteneği sağlar. Örneğin, bir cihaz, gövde sıcaklığı, darbe oranı, kan basıncı ve tek bir iletide çoklu yük ücreti için birden çok önemli işaret gönderebilir. Bu giriş iletisi dört ayrı FHıR kaynağı oluşturur. Her kaynak, giriş iletisi dört farklı normalleştirilmiş iletiye yansıtılmsal şekilde farklı öneme sahip işaretini temsil eder.

## <a name="group"></a>Grup
Grup, önceki aşamada bulunan normalleştirilmiş iletilerin üç farklı parametre kullanılarak gruplandırıldığı bir sonraki aşamadır: cihaz kimliği, ölçüm türü ve zaman aralığı.

Cihaz kimliği ve ölçüm türü gruplama, [Sampleddata](https://www.hl7.org/fhir/datatypes.html#SampledData) ölçüm türünün kullanımını etkinleştirir. Bu tür, FHıR 'deki bir cihazdan bir zaman tabanlı ölçüm serisini göstermek için kısa bir yol sağlar. Ve zaman aralığı, IoT Bağlayıcısı tarafından üretilen izleme kaynaklarının FHıR için Azure API 'sine yazıldığı gecikmeyi denetler.

> [!NOTE]
> Zaman aralığı değeri 15 dakikaya ayarlanır ve önizleme için yapılandırılamaz.

## <a name="transform"></a>Dönüşüm
Dönüştürme aşamasında, gruplanmış normalleştirilmiş iletiler FHıR eşleme şablonları aracılığıyla işlenir. Şablon türüyle eşleşen iletiler, eşleme yoluyla belirtilen FHıR tabanlı izleme kaynaklarına dönüştürülür.

Bu noktada, ilişkili [hasta](https://www.hl7.org/fhir/patient.html) kaynağıyla birlikte [cihaz](https://www.hl7.org/fhir/device.html) kaynağı, iletide bulunan cihaz tanımlayıcısı kullanılarak fhır sunucusundan de alınır. Bu kaynaklar, oluşturulmakta olan gözlem kaynağına bir başvuru olarak eklenir.

> [!NOTE]
> Tüm kimlik arama işlemleri, FHıR sunucusundaki yükü azaltmak üzere çözümlendikten sonra önbelleğe alınır. Birden çok hastaya sahip cihazları yeniden kullanmak istiyorsanız, hasta 'e özgü bir sanal cihaz kaynağı oluşturmanız ve ileti yükünde sanal cihaz tanımlayıcısı göndermeniz önerilir. Sanal cihaz, asıl cihaz kaynağına bir üst öğe olarak bağlanabilir.

FHıR sunucusunda belirli bir cihaz tanımlayıcısı için bir cihaz kaynağı yoksa, sonuç oluşturma sırasında ayarlanan değere bağlıdır `Resolution Type` . Olarak ayarlandığında `Lookup` , söz konusu ileti yok sayılır ve işlem hattı diğer gelen iletileri işlemeye devam eder. Olarak ayarlanırsa `Create` , IoT Bağlayıcısı fhır sunucusunda bir çıplak cihaz ve hasta kaynakları oluşturur.  

## <a name="persist"></a>Persist
İzleme FHıR kaynağı dönüştürme aşamasında oluşturulduktan sonra, kaynak FHıR için Azure API 'sine kaydedilir. FHıR kaynağı yeni ise, sunucuda oluşturulur. FHıR kaynağı zaten mevcutsa, güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz ve FHıR eşleme şablonlarının nasıl oluşturulacağını öğrenmek için sonraki adıma tıklayın.

>[!div class="nextstepaction"]
>[IoT Bağlayıcısı eşleme şablonları](iot-mapping-templates.md)


FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.
