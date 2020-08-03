---
title: "Kavramlar: FHıR için Azure API 'sinin FHIR (Önizleme) özelliği için Azure IoT Bağlayıcısı 'nda veri akışı"
description: FHıR (Önizleme) veri akışı için Azure IoT bağlayıcısını anlayın. FHıR için Azure IoT Bağlayıcısı (Önizleme) FHAR için Azure API 'ye Azure API 'ye veri eklemek, onları normalleştirerek, dönüştürmektir ve devam ettirir.
services: healthcare-apis
author: ms-puneet-nagpal
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: conceptual
ms.date: 07/31/2020
ms.author: punagpal
ms.openlocfilehash: 43b7bcba97617d6931fd5c191e62e833a25bf89d
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2020
ms.locfileid: "87513391"
---
# <a name="azure-iot-connector-for-fhir-preview-data-flow"></a>FHıR için Azure IoT Bağlayıcısı (Önizleme) veri akışı

Bu makalede, FHIR * için Azure IoT Bağlayıcısı 'nda veri akışına genel bakış sunulmaktadır. Azure IoT Bağlayıcısı 'nda, cihaz verilerini FHıR tabanlı [gözlemleme](https://www.hl7.org/fhir/observation.html) kaynaklarına dönüştüren farklı veri işleme aşamaları hakkında bilgi edineceksiniz.

![FHıR veri akışı için Azure IoT Bağlayıcısı](media/concepts-iot-data-flow/iot-connector-data-flow.png)

Yukarıdaki diyagramda, FHAR için Azure IoT bağlayıcısını kullanan ortak veri akışları gösterilmektedir. 

Aşağıda, verilerin FHıR için Azure IoT Bağlayıcısı tarafından alındıktan sonra geçen farklı aşamalar verilmiştir.

## <a name="ingest"></a>Alma
Inest, cihaz verilerinin FHıR için Azure IoT Bağlayıcısı 'na alındığı ilk aşamadır. Cihaz verileri için alma uç noktası bir [Azure Olay Hub 'ında](https://docs.microsoft.com/azure/event-hubs/)barındırılır. Azure Olay Hub 'ı platformu, saniyede milyonlarca iletiyi alıp işleyebilme olanağı sunan yüksek ölçekli ve üretilen işi destekler. Ayrıca, FHıR için Azure IoT bağlayıcısının iletileri zaman uyumsuz olarak kullanmasını sağlar ve cihaz verilerinin işlenmesi sırasında cihazların beklemesi gereksinimini ortadan kaldırır.

> [!NOTE]
> JSON, cihaz verileri için şu anda desteklenen tek biçimdir.

## <a name="normalize"></a>Normalleştir
Normalleştir, cihaz verilerinin yukarıdaki Azure Olay Hub 'ından alındığı ve cihaz eşleme şablonları kullanılarak işlendiği bir sonraki aşamadır. Bu eşleme süreci, cihaz verilerinin Normalleştirilmemiş bir şemaya dönüştürülmesiyle sonuçlanır. 

Normalleştirme işlemi, sonraki aşamalarda yalnızca veri işlemeyi basitleştirir, ancak aynı zamanda bir giriş iletisini birden çok normalleştirilmiş ileti halinde proje yeteneği sağlar. Örneğin, bir cihaz, gövde sıcaklığı, darbe oranı, kan basıncı ve tek bir iletide çoklu yük ücreti için birden çok önemli işaret gönderebilir. Bu giriş iletisi dört ayrı FHıR kaynağı oluşturur. Her kaynak, giriş iletisi dört farklı normalleştirilmiş iletiye yansıtılmsal şekilde farklı öneme sahip işaretini temsil eder.

## <a name="group"></a>Grup
Grup, önceki aşamada bulunan normalleştirilmiş iletilerin üç farklı parametre kullanılarak gruplandırıldığı bir sonraki aşamadır: cihaz kimliği, ölçüm türü ve zaman aralığı.

Cihaz kimliği ve ölçüm türü gruplama, [Sampleddata](https://www.hl7.org/fhir/datatypes.html#SampledData) ölçüm türünün kullanımını etkinleştirir. Bu tür, FHıR 'deki bir cihazdan bir zaman tabanlı ölçüm serisini göstermek için kısa bir yol sağlar. Ve zaman dilimi, FHıR için Azure IoT Bağlayıcısı tarafından üretilen izleme kaynaklarının FHıR için Azure API 'sine yazıldığı gecikmeyi denetler.

> [!NOTE]
> Zaman aralığı değeri 15 dakikaya ayarlanır ve önizleme için yapılandırılamaz.

## <a name="transform"></a>Dönüşüm
Dönüştürme aşamasında, gruplanmış normalleştirilmiş iletiler FHıR eşleme şablonları aracılığıyla işlenir. Şablon türüyle eşleşen iletiler, eşleme yoluyla belirtilen FHıR tabanlı izleme kaynaklarına dönüştürülür.

Bu noktada, ilişkili [hasta](https://www.hl7.org/fhir/patient.html) kaynağıyla birlikte [cihaz](https://www.hl7.org/fhir/device.html) kaynağı, iletide bulunan cihaz tanımlayıcısı kullanılarak fhır sunucusundan de alınır. Bu kaynaklar, oluşturulmakta olan gözlem kaynağına bir başvuru olarak eklenir.

> [!NOTE]
> Tüm kimlik arama işlemleri, FHıR sunucusundaki yükü azaltmak üzere çözümlendikten sonra önbelleğe alınır. Birden çok hastaya sahip cihazları yeniden kullanmak istiyorsanız, hasta 'e özgü bir sanal cihaz kaynağı oluşturmanız ve ileti yükünde sanal cihaz tanımlayıcısı göndermeniz önerilir. Sanal cihaz, asıl cihaz kaynağına bir üst öğe olarak bağlanabilir.

FHıR sunucusunda belirli bir cihaz tanımlayıcısı için bir cihaz kaynağı yoksa, sonuç oluşturma sırasında ayarlanan değere bağlıdır `Resolution Type` . Olarak ayarlandığında `Lookup` , söz konusu ileti yok sayılır ve işlem hattı diğer gelen iletileri işlemeye devam eder. Olarak ayarlanırsa `Create` , fhır için Azure IoT Bağlayıcısı, fhır sunucusunda bir çıplak cihaz ve hasta kaynakları oluşturur.  

## <a name="persist"></a>Persist
İzleme FHıR kaynağı dönüştürme aşamasında oluşturulduktan sonra, kaynak FHıR için Azure API 'sine kaydedilir. FHıR kaynağı yeni ise, sunucuda oluşturulur. FHıR kaynağı zaten mevcutsa, güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz ve FHıR eşleme şablonlarının nasıl oluşturulacağını öğrenmek için sonraki adıma tıklayın.

>[!div class="nextstepaction"]
>[FHıR eşleme şablonları için Azure IoT Bağlayıcısı](iot-mapping-templates.md)

* Azure portal, FHıR için Azure IoT Bağlayıcısı, IoT Bağlayıcısı (Önizleme) olarak adlandırılır.

FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.
