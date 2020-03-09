---
title: Azure Data Lake Storage 1. Azure Stream Analytics kimlik doğrulama
description: Bu makalede, Azure Stream Analytics işinizi Azure Data Lake depolama Gen1 çıkış için kimlik doğrulaması için yönetilen kimlikleri kullanmayı açıklar.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/08/2019
ms.custom: seodec18
ms.openlocfilehash: 01741ea56b9e6f55c1393e88fc7991d410c33119
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78379182"
---
# <a name="authenticate-stream-analytics-to-azure-data-lake-storage-gen1-using-managed-identities"></a>Yönetilen kimlikleri kullanarak Azure Data Lake Storage 1. Stream Analytics kimlik doğrulama

Azure Stream Analytics, Azure Data Lake Storage (ADLS) Gen1 çıktıyla yönetilen kimlik doğrulama destekler. Kimlik Azure Active Directory içinde belirli bir Stream Analytics işi temsil eden kayıtlı bir yönetilen uygulama ve hedeflenen kaynak kimliğini doğrulamak için kullanılabilir. Yönetilen kimlik parola değişikliklerini veya 90 günde gerçekleşen kullanıcı belirteci süre sonu nedeniyle yeniden kimlik doğrulamaya zorlayabilir gerek gibi kullanıcı tabanlı kimlik doğrulama yöntemlerini sınırlamaları ortadan kaldırır. Ayrıca, yönetilen kimlikleri, Azure Data Lake depolama Gen1 output Stream Analytics işi dağıtımlarının Otomasyonu ile yardımcı olur.

Bu makalede, Visual Studio için Azure portal, Azure Resource Manager şablon dağıtımı ve Azure Stream Analytics araçları aracılığıyla bir Azure Data Lake Storage 1. çıkış yapan bir Azure Stream Analytics işi için yönetilen kimliği etkinleştirmek için üç yol gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-portal"></a>Azure portalı

1. Yeni bir Stream Analytics işi oluşturma veya var olan bir işi, Azure portalında açarak başlatın. Ekranın sol tarafındaki menü çubuğundan **Yapılandır**' ın altında bulunan **yönetilen kimlik** ' i seçin.

   ![Stream Analytics yönetilen kimliği yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Sağ tarafta görüntülenen pencereden **sistem tarafından atanan yönetilen kimliği kullan** ' ı seçin. Azure Active Directory Stream Analytics işinin kimliği için hizmet sorumlusuna **Kaydet** ' e tıklayın. Yeni oluşturulan kimlik yaşam döngüsünü Azure tarafından yönetilir. Stream Analytics işi silindiğinde, ilişkili kimlik (diğer bir deyişle, hizmet sorumlusu), Azure tarafından otomatik olarak silinir.

   Yapılandırma kaydedilirken, hizmet sorumlusu nesne kimliği (OID) asıl aşağıda gösterildiği gibi kimlik olarak listelenir:

   ![Stream Analytics Hizmet sorumlusu kimliği](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
   Hizmet sorumlusu, Stream Analytics işiyle aynı ada sahip. Örneğin, işinizin adı **MyASAJob**ise oluşturulan hizmet sorumlusu adı da **MyASAJob**olur.

3. ADLS 1. çıkış havuzunun çıkış özellikleri penceresinde, kimlik doğrulama modu açılan listesine tıklayın ve * * yönetilen kimlik * * öğesini seçin.

4. Kalan özellikleri doldurun. Bir ADLS çıkışı oluşturma hakkında daha fazla bilgi edinmek için bkz. [Stream Analytics Ile Data Lake Store çıkışı oluşturma](../data-lake-store/data-lake-store-stream-analytics.md). İşiniz bittiğinde **Kaydet**' e tıklayın.

   ![Azure Data Lake depolama yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-configure-adls.png)
 
5. ADLS 1. genel bakış sayfasına gidin ve **Veri Gezgini**' ne tıklayın.

   ![Data Lake depolamaya genel bakış yapılandırın](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-overview.png)

6. Veri Gezgini bölmesinde, **erişim** ' i seçin ve erişim bölmesinde **Ekle** ' ye tıklayın.

   ![Data Lake depolama erişimi yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-access.png)

7. **Kullanıcı veya Grup Seç** bölmesindeki metin kutusunda, hizmet sorumlusunun adını yazın. Hizmet sorumlusu adını karşılık gelen Stream Analytics işinin adı olduğunu unutmayın. Asıl adını yazmaya başladığınızda, bu metin kutusunun altında görüntülenir. İstenen hizmet asıl adını seçip **Seç**' e tıklayın.

   ![Bir hizmet asıl adı seçin](./media/stream-analytics-managed-identities-adls/stream-analytics-service-principal-name.png)
 
8. **İzinler** bölmesinde, **yazma** ve **yürütme** izinlerini denetleyin ve **Bu klasöre ve tüm alt öğelere**atayın. Ardından **Tamam**' a tıklayın.

   ![Yazma ve Yürütme izinleri seçin](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. Hizmet sorumlusu aşağıda gösterildiği gibi **erişim** bölmesinde **atanan izinler** altında listelenir. Şimdi, geri dönün ve Stream Analytics işinizi başlatın.

   ![Stream Analytics erişim Portalı'nda listesi](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Data Lake Storage 1. dosya sistemi izinleri hakkında daha fazla bilgi edinmek için bkz. [Azure Data Lake Storage 1. Access Control](../data-lake-store/data-lake-store-access-control.md).

## <a name="stream-analytics-tools-for-visual-studio"></a>Visual Studio için Stream Analytics araçları

1. JobConfig. json dosyasında, **sistem tarafından atanan kimliği kullan** seçeneğini **true**olarak ayarlayın.

   ![Stream Analytics iş yapılandırması tarafından yönetilen kimlikler](./media/stream-analytics-managed-identities-adls/adls-mi-jobconfig-vs.png)

2. ADLS 1. çıkış havuzunun çıkış özellikleri penceresinde, kimlik doğrulama modu açılan listesine tıklayın ve * * yönetilen kimlik * * öğesini seçin.

   ![ADLS çıkış Yönetilen kimlikler](./media/stream-analytics-managed-identities-adls/adls-mi-output-vs.png)

3. Özellikleri geri kalanı doldurun ve **Kaydet**' e tıklayın.

4. Sorgu Düzenleyicisi 'nde **Azure 'A Gönder ' e** tıklayın.

   İşi gönderdiğinizde araçlar iki şey yapar:

   * , Azure Active Directory Stream Analytics işin kimliği için otomatik olarak bir hizmet sorumlusu oluşturur. Yeni oluşturulan kimlik yaşam döngüsünü Azure tarafından yönetilir. Stream Analytics işi silindiğinde, ilişkili kimlik (diğer bir deyişle, hizmet sorumlusu), Azure tarafından otomatik olarak silinir.

   * İşte kullanılan ADLS 1. önek yolu için **yazma** ve **yürütme** izinlerini otomatik olarak ayarlayın ve bu klasöre ve tüm alt öğelere atayın.

5. Stream Analytics CI kullanarak aşağıdaki özelliği içeren Kaynak Yöneticisi şablonları oluşturabilirsiniz [. ](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/)Bir derleme makinesinde (Visual Studio dışında) CD NuGet paketi sürüm 1.5.0 veya üzeri. Hizmet sorumlusunu almak ve PowerShell aracılığıyla hizmet sorumlusuna erişim sağlamak için sonraki bölümde Kaynak Yöneticisi şablonu dağıtım adımlarını izleyin.

## <a name="resource-manager-template-deployment"></a>Resource Manager şablon dağıtımı

1. Kaynak Yöneticisi şablonunuzun kaynak bölümüne aşağıdaki özelliği ekleyerek, yönetilen bir kimlikle bir *Microsoft. StreamAnalytics/streamingjobs* kaynağı oluşturabilirsiniz:

    ```json
    "Identity": {
      "Type": "SystemAssigned",
    },
    ```

   Bu özellik, Azure Resource Manager'ı oluşturma ve Azure Stream Analytics işiniz için Kimlik Yönetimi söyler.

   **Örnek iş**
   
   ```json
   {
     "Name": "AsaJobWithIdentity",
     "Type": "Microsoft.StreamAnalytics/streamingjobs",
     "Location": "West US",
     "Identity": {
       "Type": "SystemAssigned",
     },
     "properties": {
       "sku": {
         "name": "standard"
       },
       "outputs": [
         {
           "name": "string",
           "properties":{
             "datasource": {
               "type": "Microsoft.DataLake/Accounts",
               "properties": {
                 "accountName": "myDataLakeAccountName",
                 "filePathPrefix": "cluster1/logs/{date}/{time}",
                 "dateFormat": "YYYY/MM/DD",
                 "timeFormat": "HH",
                 "authenticationMode": "Msi"
             }
           }
         }
       }
     }
   }
   ```
  
   **Örnek iş yanıtı**

   ```json
   {
    "Name": "mySAJob",
    "Type": "Microsoft.StreamAnalytics/streamingjobs",
    "Location": "West US",
    "Identity": {
      "Type": "SystemAssigned",
        "principalId": "GUID",
        "tenantId": "GUID",
      },
      "properties": {
        "sku": {
          "name": "standard"
        },
     }
   }
   ```

   Asıl gerekli ADLS kaynağına erişim izni vermek için kimlik iş yanıtından not alın.

   **KIRACı kimliği** , hizmet sorumlusunun oluşturulduğu Azure Active Directory kiracının kimliğidir. Hizmet sorumlusu abonelik tarafından güvenilen bir Azure kiracısı oluşturulur.

   **Türü** , yönetilen kimlik türleri bölümünde açıklandığı gibi yönetilen kimliğin türünü gösterir. Yalnızca sistem atanan türü desteklenmiyor.

2. PowerShell kullanarak hizmet sorumlusuna erişim sağlar. Hizmet sorumlusu PowerShell erişim vermek için aşağıdaki komutu yürütün:

   ```powershell
   Set-AzDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   **PrincipalId** , hizmet SORUMLUSUNUN nesne kimliğidir ve hizmet sorumlusu oluşturulduktan sonra Portal ekranında listelenir. Resource Manager şablon dağıtımı kullanarak işi oluşturduysanız, nesne kimliği proje yanıtı kimlik özelliğinde listelenir.

   **Örnek**

   ```powershell
   PS > Set-AzDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Yukarıdaki PowerShell komutu hakkında daha fazla bilgi edinmek için [set-AzDataLakeStoreItemAclEntry](/powershell/module/az.datalakestore/set-azdatalakestoreitemaclentry) belgelerine bakın.

## <a name="limitations"></a>Sınırlamalar
Bu özellik şunları desteklemez:

1. **Çok kiracılı erişim**: belirli bir Stream Analytics işi Için oluşturulan hizmet sorumlusu, işin oluşturulduğu Azure Active Directory kiracısında bulunur ve farklı bir Azure Active Directory kiracısında bulunan bir kaynağa karşı kullanılamaz. Bu nedenle, MSI 'yi yalnızca Azure Stream Analytics işiniz ile aynı Azure Active Directory kiracısındaki ADLS Gen 1 kaynakları üzerinde kullanabilirsiniz. 

2. **[Kullanıcı tarafından atanan kimlik](../active-directory/managed-identities-azure-resources/overview.md)** : desteklenmiyor. Bu, kullanıcının Stream Analytics işleri tarafından kullanılmak üzere kendi hizmet sorumlusunu giremediği anlamına gelir. Hizmet sorumlusu Azure Stream Analytics tarafından oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

* [Stream Analytics ile Data Lake Store çıkışı oluşturma](../data-lake-store/data-lake-store-stream-analytics.md)
* [Visual Studio ile yerel olarak Stream Analytics sorguları test etme](stream-analytics-vs-tools-local-run.md)
* [Visual Studio için Azure Stream Analytics araçlarını kullanarak canlı verileri yerel olarak test etme](stream-analytics-live-data-local-testing.md) 
