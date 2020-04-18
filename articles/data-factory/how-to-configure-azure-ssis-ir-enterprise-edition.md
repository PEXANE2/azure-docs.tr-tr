---
title: Azure-SSIS Tümleştirme Çalışma Süresi için Provizyon Enterprise Edition
description: Bu makalede, Azure-SSIS Tümleştirme Çalışma Süresi için Enterprise Edition'ın özellikleri ve nasıl sağlanır
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: anandsub
ms.openlocfilehash: a0e35a309fd30a36263b498b6a8e74a312004a99
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606167"
---
# <a name="provision-enterprise-edition-for-the-azure-ssis-integration-runtime"></a>Azure-SSIS Tümleştirme Çalışma Süresi için Provizyon Enterprise Edition

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure-SSIS Tümleştirme Çalışma Zamanı'nın Enterprise Sürümü, aşağıdaki gelişmiş ve üstün özellikleri kullanmanıza olanak tanır:
-   Veri Yakalama (CDC) bileşenlerini değiştirme
-   Oracle, Teradata ve SAP BW konektörleri
-   SQL Server Analysis Services (SSAS) ve Azure Analiz Hizmetleri (AAS) konektörleri ve dönüşümleri
-   Bulanık Gruplandırma ve Bulanık Arama dönüşümleri
-   Dönem Çıkarma ve Dönem Arama dönüşümleri

Bu özelliklerden bazıları, Azure-SSIS IR'yi özelleştirmek için ek bileşenler yüklemenizi gerektirir. Ek bileşenlerin nasıl yüklenirhakkında daha fazla bilgi için [Azure-SSIS tümleştirme çalışma zamanı için Özel kuruluma](how-to-configure-azure-ssis-ir-custom-setup.md)bakın.

## <a name="enterprise-features"></a>Kurumsal özellikler

| **Kurumsal Özellikler** | **Açıklama** |
|---|---|
| CDC bileşenleri | CDC Kaynak, Denetim Görevi ve Bölünen Dönüşüm, Azure-SSIS IR Enterprise Edition'da önceden yüklenir. Oracle'a bağlanmak için CDC Tasarımcısı ve Hizmeti'ni başka bir bilgisayara yüklemeniz gerekir. |
| Oracle konektörleri | Oracle Bağlantı Yöneticisi, Kaynak ve Hedef, Azure-SSIS IR Enterprise Edition'da önceden yüklenir. Ayrıca Oracle Çağrı Arabirimi (OCI) sürücüsünü yüklemeniz ve gerekirse Oracle Transport Network Substratını (TNS) Azure-SSIS IR'ye yapılandırmanız gerekir. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Teradata konektörleri | Teradata Bağlantı Yöneticisi, Kaynak ve Hedef'in yanı sıra Teradata Paralel Taşıyıcı (TPT) API ve Teradata ODBC sürücüsünü Azure-SSIS IR Enterprise Edition'a yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md). |
| SAP BW konektörleri | SAP BW Bağlantı Yöneticisi, Kaynak ve Hedef Azure-SSIS IR Enterprise Edition'da önceden yüklenir. SAP BW sürücüsünü Azure-SSIS IR'ye de yüklemeniz gerekir. Bu bağlayıcılar SAP BW 7.0 veya önceki sürümleri destekler. SAP BW veya diğer SAP ürünlerinin sonraki sürümlerine bağlanmak için Azure-SSIS IR'de üçüncü taraf ISV'lerden SAP konektörleri satın alabilir ve yükleyebilirsiniz. Ek bileşenlerin nasıl yüklenirhakkında daha fazla bilgi için [Azure-SSIS tümleştirme çalışma zamanı için Özel kuruluma](how-to-configure-azure-ssis-ir-custom-setup.md)bakın. |
| Analiz Hizmetleri bileşenleri               | Veri Madenciliği Modeli Eğitim Hedefi, Boyut İşleme Hedefi ve Bölüm İşleme Hedefi ve Veri Madenciliği Sorgusu Dönüşümü, Azure-SSIS IR Enterprise Edition'a önceden yüklenir. Tüm bu bileşenler SQL Server Analysis Services'ı (SSAS) destekler, ancak yalnızca Bölüm İşleme Hedefi Azure Analiz Hizmetlerini (AAS) destekler. SSAS'a bağlanmak için, [Windows Kimlik Doğrulama kimlik bilgilerini SSISDB'de yapılandırmanız](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth)da gerekir. Bu bileşenlere ek olarak, Çözümleme Hizmetleri Yürütme DDL Görevi, Çözümleme Hizmetleri İşleme Görevi ve Veri Madenciliği Sorgu Görevi de Azure-SSIS IR Standard/Enterprise Edition'a önceden yüklenir. |
| Bulanık Gruplandırma ve Bulanık Arama dönüşümleri  | Bulanık Gruplandırma ve Bulanık Arama dönüşümleri Azure-SSIS IR Enterprise Edition'da önceden yüklenir. Bu bileşenler, başvuru verilerini depolamak için hem SQL Server'ı hem de Azure SQL Veritabanını destekler. |
| Dönem Çıkarma ve Dönem Arama dönüşümleri | Dönem Ayıklama ve Terim Arama dönüşümleri Azure-SSIS IR Enterprise Edition'da önceden yüklenir. Bu bileşenler, başvuru verilerini depolamak için hem SQL Server'ı hem de Azure SQL Veritabanını destekler. |

## <a name="instructions"></a>Yönergeler

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1.  [Azure PowerShell'i](/powershell/azure/install-az-ps)indirin ve yükleyin.

2.  Azure-SSIS IR'yi PowerShell ile birlikte veya `Set-AzDataFactoryV2IntegrationRuntime` yeniden yapılandırdığınızda, Azure-SSIS IR'yi başlatmadan önce **Sürüm** parametresi değeri olarak **Enterprise** ile çalıştırın. Burada örnek bir komut dosyası:

    ```powershell
    $MyAzureSsisIrEdition = "Enterprise"

    Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName
                                               -Name $MyAzureSsisIrName
                                               -ResourceGroupName $MyResourceGroupName
                                               -Edition $MyAzureSsisIrEdition

    Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName
                                                 -Name $MyAzureSsisIrName
                                                 -ResourceGroupName $MyResourceGroupName
    ```

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure-SSIS tümleştirme çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Azure-SSIS tümleştirme çalışma süresi için ücretli veya lisanslı özel bileşenler nasıl geliştirilir?](how-to-develop-azure-ssis-ir-licensed-components.md)
