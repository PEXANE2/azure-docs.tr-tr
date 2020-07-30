---
title: 'Öğretici: Azure SYNAPSE Analytics ile çalışmaya başlama-Power BI çalışma alanı verilerini görselleştirin'
description: Bu öğreticide, Power BI çalışma alanı oluşturmayı, Azure SYNAPSE çalışma alanınızı bağlamayı ve Azure SYNAPSE çalışma alanındaki verileri kullanan bir Power BI veri kümesi oluşturmayı öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 40a2cd5898ede7fc30db9a8475c5ab7cc68095ff
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337645"
---
# <a name="visualize-data-with-power-bi"></a>Power BI ile verileri görselleştirme

Bu öğreticide, Power BI çalışma alanı oluşturmayı, Azure SYNAPSE çalışma alanınızı bağlamayı ve Azure SYNAPSE çalışma alanınızdaki verileri kullanan bir Power BI veri kümesi oluşturmayı öğreneceksiniz. 

## <a name="overview"></a>Genel Bakış

NYC TAXI verilerinden, toplanan veri kümelerini iki tabloda oluşturduk:
- **nyctaxi. passengercountstats**
- **SQLDB1. dbo. PassengerCountStats**

Power BI çalışma alanını Azure SYNAPSE çalışma alanınıza bağlayabilirsiniz. Bu özellik Power BI çalışma alanınıza kolayca veri almanıza olanak sağlar. Power BI raporlarınızı doğrudan Azure SYNAPSE çalışma alanınızda düzenleyebilirsiniz.

### <a name="create-a-power-bi-workspace"></a>Power BI çalışma alanı oluşturma

1. [Powerbi.Microsoft.com](https://powerbi.microsoft.com/)'de oturum açın.
1. **NYCTaxiWorkspace1**adlı yeni bir Power BI çalışma alanı oluşturun.

### <a name="link-your-azure-synapse-workspace-to-your-new-power-bi-workspace"></a>Azure SYNAPSE çalışma alanınızı yeni Power BI çalışma alanınıza bağlama

1. SYNAPSE Studio 'da **Manage**  >  **bağlı hizmetleri**Yönet ' e gidin.
1. Power BI **Yeni**  >  **Bağlan**' ı seçin ve ardından bu alanları ayarlayın:

    |Ayar | Önerilen değer | 
    |---|---|
    |**Ad**|**NYCTaxiWorkspace1**|
    |**Çalışma alanı adı**|**NYCTaxiWorkspace1**|

1. **Oluştur**’u seçin.

### <a name="create-a-power-bi-dataset-that-uses-data-in-your-azure-synapse-workspace"></a>Azure SYNAPSE çalışma alanınızdaki verileri kullanan bir Power BI veri kümesi oluşturma

1. SYNAPSE Studio 'da Power BI **geliştirme**bölümüne gidin  >  **Power BI**.
1. **NYCTaxiWorkspace1**  >  **Power BI veri kümelerine** gidin ve **Yeni Power BI veri kümesi ' ni**seçin.
1. **SQLDB1** veritabanının üzerine gelin ve **. pbıd dosyasını indir**' i seçin.
1. İndirilen **. pbıds** dosyasını açın. Power BI Masaüstü açılır ve Azure SYNAPSE çalışma alanınızda **SQLDB1** 'e otomatik olarak bağlanır.
1. **SQL Server veritabanı**adında bir iletişim kutusu görünürse:
    1. **Microsoft hesabı**seçin.
    1. **Oturum aç** ' ı seçin ve hesabınızda oturum açın.
    1. **Bağlan**'ı seçin.
1. **Gezgin** iletişim kutusu açıldıktan sonra, **Passengercountstats** tablosunu denetleyip **Yükle**' yi seçin.
1. **Bağlantı ayarları** iletişim kutusu göründükten sonra **DirectQuery**  >  **Tamam**' ı seçin.
1. Sol taraftaki **rapor** düğmesini seçin.
1. Raporunuza **çizgi grafik** ekleyin.
    1. **Passengercount** sütununu **görsel öğeler**  >  **eksenine**sürükleyin.
    1. **Sumüçlü uzaklığı** ve **avgüçlü uzaklığı** sütunlarını **görselleştirmeler**  >  **değerlerine**sürükleyin.
1. **Giriş** sekmesinde **Yayımla**' yı seçin.
1. Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin.
1. **Passengeranalysis. pbix**dosya adını seçin ve ardından **Kaydet**' i seçin.
1. **Bir hedef seçin**bölümünde **NYCTaxiWorkspace1**öğesini seçin ve ardından **Seç**' e tıklayın.
1. Yayımlamanın bitmesini bekleyin.

### <a name="configure-authentication-for-your-dataset"></a>Veri kümeniz için kimlik doğrulamasını yapılandırma

1. [Powerbi.Microsoft.com](https://powerbi.microsoft.com/) açın ve **oturum açın**.
1. Sol tarafta, **çalışma alanları**altında **NYCTaxiWorkspace1** çalışma alanını seçin.
1. Bu çalışma alanının içinde, **Passenger Analizi** adlı bir veri kümesini ve **Passenger Analizi**adlı bir raporu bulun.
1. **Passengeranalysis** veri kümesinin üzerine gelin, üç nokta (...) düğmesini seçin ve ardından **Ayarlar**' ı seçin.
1. **Veri kaynağı kimlik bilgileri**' nde, **kimlik doğrulama yöntemini** **OAuth2**olarak ayarlayın ve **oturum aç**' ı seçin.

### <a name="edit-a-report-in-synapse-studio"></a>SYNAPSE Studio 'da rapor düzenleme

1. SYNAPSE Studio 'ya dönün ve **Kapat ve Yenile '** yi seçin.
1. **Geliştirme** merkezine gidin.
1. **Power BI** üzerine gelin ve **Power BI Reports** düğümünü Yenile düğümünü seçin.
1. **Power BI** altında şunları görmeniz gerekir:
    * **NYCTaxiWorkspace1**  >  **Power BI veri kümeleri**altında, **passengeranalysis**adlı yeni bir veri kümesi.
    * **NYCTaxiWorkspace1**  >  **Power BI raporlarında**, **passengeranalysis**adlı yeni bir rapor.
1. **Passengeranalysis** raporunu seçin. Rapor açılır ve doğrudan SYNAPSE Studio içinde düzenleyebilirsiniz.

## <a name="monitor-activities"></a>Etkinlikleri izleme

1. SYNAPSE Studio 'da **izleyici** hub 'ına gidin.
1. Bu konumda, çalışma alanında yer alan tüm etkinliklerin geçmişini görebilir ve bunların şimdi etkin olduğunu görebilirsiniz.
1. Çalışma alanında daha önce ne yaptığını görmek için işlem **hattı çalıştırmalarını**, **Apache Spark uygulamaları**ve **SQL isteklerini** inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İzleyici](get-started-monitor.md)
                                 

