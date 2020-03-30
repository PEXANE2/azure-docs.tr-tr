---
title: Azure Veri Gölü Depolama Gen1 için Azure Akış Analizini Doğrula
description: Bu makalede, Azure Akış Analizi işinizi Azure Veri Gölü Depolama Gen1 çıktısına doğrulamak için yönetilen kimliklerin nasıl kullanılacağı açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/08/2019
ms.custom: seodec18
ms.openlocfilehash: 01741ea56b9e6f55c1393e88fc7991d410c33119
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254384"
---
# <a name="authenticate-stream-analytics-to-azure-data-lake-storage-gen1-using-managed-identities"></a>Yönetilen kimlikleri kullanarak Akış Analizini Azure Veri Gölü Depolama Gen1'e doğrulayın

Azure Akış Analizi, Azure Veri Gölü Depolama (ADLS) Gen1 çıkışıyla yönetilen kimlik doğrulamasını destekler. Kimlik, Azure Etkin Dizini'nde kayıtlı ve belirli bir Akış Analizi işini temsil eden yönetilen bir uygulamadır ve hedeflenen bir kaynağa kimlik doğrulamak için kullanılabilir. Yönetilen kimlikler, parola değişiklikleri veya her 90 günde bir gerçekleşen kullanıcı belirteç leri nedeniyle yeniden kimlik doğrulama gereksinimi gibi kullanıcı tabanlı kimlik doğrulama yöntemlerinin sınırlamalarını ortadan kaldırır. Ayrıca, yönetilen kimlikler, Azure Veri Gölü Depolama Gen1'e çıkan Stream Analytics iş dağıtımlarının otomasyonuna yardımcı olur.

Bu makalede, Azure portalı, Azure Kaynak Yöneticisi şablon dağıtımı ve Visual Studio için Azure Akış Analizi araçları aracılığıyla bir Azure Veri Gölü Depolama Gen1'e çıkan bir Azure Akış Analizi işi için yönetilen kimliği etkinleştirmenin üç yolu gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-portal"></a>Azure portalında

1. Yeni bir Akış Analizi işi oluşturarak veya Azure portalında varolan bir iş açarak başlayın. Ekranın sol tarafında bulunan menü çubuğundan, **Yapıl'ın**altında bulunan **Yönetilen Kimlik'i** seçin.

   ![Akış Analizi yönetilen kimliğini yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Sağda görünen pencereden **Sistem atanmış Yönetilen Kimliği Kullan'ı** seçin. Azure Active Directory'deki Akış Analizi işinin kimliği için bir hizmet yöneticisine **Kaydet'i** tıklatın. Yeni oluşturulan kimliğin yaşam döngüsü Azure tarafından yönetilir. Akış Analizi işi silindiğinde, ilişkili kimlik (diğer bir şey, hizmet sorumlusu) Azure tarafından otomatik olarak silinir.

   Yapılandırma kaydedildiğinde, hizmet sorumlusunun Nesne Kimliği (OID) aşağıda gösterildiği gibi Asıl Kimlik olarak listelenir:

   ![Stream Analytics hizmeti ana kimliği](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
   Hizmet sorumlusu, Stream Analytics işiyle aynı ada sahiptir. Örneğin, işinizin adı **MyASAJob**ise, oluşturulan hizmet sorumlusunun adı da **MyASAJob'tır.**

3. ADLS Gen1 çıktı lavabosundaki çıkış özellikleri penceresinde Kimlik Doğrulama modu açılır penceresini tıklatın ve **Yönetilen Kimlik **'yi seçin.

4. Diğer özellikleri doldurun. ADLS çıktısı oluşturma hakkında daha fazla bilgi edinmek için akış [analitiğiyle Veri gölü Deposu çıktısı oluşturma](../data-lake-store/data-lake-store-stream-analytics.md)bölümüne bakın. İşiniz bittiğinde **Kaydet**’e tıklayın.

   ![Azure Veri Gölü Depolamayı Yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-configure-adls.png)
 
5. ADLS Gen1'inizin Genel Bakış sayfasına gidin ve **Veri gezginine**tıklayın.

   ![Veri Gölü Depolamasına Genel Bakış'ı Yapılandır](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-overview.png)

6. Veri gezgini bölmesinde **Access'i** seçin ve Access bölmesinde **Ekle'yi** tıklatın.

   ![Veri Gölü Depolama Erişimini Yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-access.png)

7. Kullanıcı yı veya **grup** bölmesindeki metin kutusuna servis sorumlusunun adını yazın. Hizmet sorumlusunun adının aynı zamanda ilgili Akış Analizi işinin adı olduğunu unutmayın. Asıl adı yazmaya başladığınızda, metin kutusunun altında görünür. İstenilen hizmet ana adını seçin ve **Seç'i**tıklatın.

   ![Hizmet ana adı seçin](./media/stream-analytics-managed-identities-adls/stream-analytics-service-principal-name.png)
 
8. **İzinler** bölmesinde, **Yazma** ve **Yürütme** izinlerini denetleyin ve **bu Klasör'e ve tüm alt çocuklara**atayın. Sonra **Tamam'ı**tıklatın.

   ![Yazma ve kullanma izinlerini seçme](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. Hizmet sorumlusu, aşağıda gösterildiği gibi **Erişim** bölmesindeki **Atanmış İzinler** altında listelenir. Artık geri dönüp Stream Analytics işinize başlayabilirsiniz.

   ![Portaldaki Akış Analizi erişim listesi](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Veri Gölü Depolama Gen1 dosya sistemi izinleri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen1'de Erişim Denetimi'ne](../data-lake-store/data-lake-store-access-control.md)bakın.

## <a name="stream-analytics-tools-for-visual-studio"></a>Visual Studio için Stream Analytics araçları

1. JobConfig.json'da, **Kullanım Sistemi tarafından atanan Kimliği** **True**olarak ayarlayın.

   ![Stream Analytics iş config yönetilen kimlikler](./media/stream-analytics-managed-identities-adls/adls-mi-jobconfig-vs.png)

2. ADLS Gen1 çıktı lavabosundaki çıkış özellikleri penceresinde Kimlik Doğrulama modu açılır penceresini tıklatın ve **Yönetilen Kimlik **'yi seçin.

   ![ADLS çıktı yönetilen kimlikler](./media/stream-analytics-managed-identities-adls/adls-mi-output-vs.png)

3. Özelliklerin geri kalanını doldurun ve **Kaydet'i**tıklatın.

4. Sorgu düzenleyicisinde **Azure'a Gönder'i** tıklatın.

   İşi gönderdiğiniz zaman, araçlar iki şey yapar:

   * Azure Active Directory'deki Akış Analizi işinin kimliği için otomatik olarak bir hizmet ilkesi oluşturur. Yeni oluşturulan kimliğin yaşam döngüsü Azure tarafından yönetilir. Akış Analizi işi silindiğinde, ilişkili kimlik (diğer bir şey, hizmet sorumlusu) Azure tarafından otomatik olarak silinir.

   * İşyerinde kullanılan ADLS Gen1 öneki yolunun yazma **ve** **çalıştır** izinlerini otomatik olarak ayarlayın ve bu klasöre ve tüm çocuklara atayın.

5. Akış Analizi CI'yi kullanarak aşağıdaki özellikile Kaynak Yöneticisi şablonlarını [oluşturabilirsiniz. CD Nuget paket](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/) sürümü 1.5.0 veya üzeri bir yapı makinesi (Visual Studio dışında). Hizmet yöneticisini almak ve PowerShell üzerinden hizmet ilkesine erişim izni vermek için bir sonraki bölümdeki Kaynak Yöneticisi şablonu dağıtım adımlarını izleyin.

## <a name="resource-manager-template-deployment"></a>Kaynak Yöneticisi şablon dağıtımı

1. Kaynak Yöneticisi şablonunuzun kaynak bölümüne aşağıdaki özelliği ekleyerek yönetilen bir kimliğe sahip bir *Microsoft.StreamAnalytics/streamingjobs* kaynağı oluşturabilirsiniz:

    ```json
    "Identity": {
      "Type": "SystemAssigned",
    },
    ```

   Bu özellik, Azure Kaynak Yöneticisi'ne Azure Akış Analizi işinizin kimliğini oluşturmasını ve yönetmesini söyler.

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

   Gerekli ADLS kaynağına erişim izni vermek için iş yanıtından Asıl Kimlik'e dikkat edin.

   **Kiracı Kimliği,** hizmet yöneticisinin oluşturulduğu Azure Etkin Dizin kiracısının kimliğidir. Hizmet ilkesi, abonelik tarafından güvenilen Azure kiracısında oluşturulur.

   **Tür,** yönetilen kimlik türlerinde açıklandığı gibi yönetilen kimlik türünü gösterir. Yalnızca Atanan Sistem türü desteklenir.

2. PowerShell'i kullanarak servis ilkesine erişim sağlayın. PowerShell üzerinden hizmet ilkesine erişim sağlamak için aşağıdaki komutu uygulayın:

   ```powershell
   Set-AzDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   **PrincipalId,** hizmet sorumlusunun Nesne Kimliği'dir ve hizmet ilkesi oluşturulduktan sonra portal ekranında listelenir. İşi Kaynak Yöneticisi şablonu dağıtımı kullanarak oluşturduysanız, Nesne Kimliği iş yanıtının Kimlik özelliğinde listelenir.

   **Örnek**

   ```powershell
   PS > Set-AzDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Yukarıdaki PowerShell komutu hakkında daha fazla bilgi edinmek için [Set-AzDataLakeStoreItemAclEntry](/powershell/module/az.datalakestore/set-azdatalakestoreitemaclentry) belgelerine bakın.

## <a name="limitations"></a>Sınırlamalar
Bu özellik aşağıdakileri desteklemez:

1. **Çok kiracılı erişim**: Belirli bir Akış Analizi işi için oluşturulan Hizmet ilkesi, işin oluşturulduğu Azure Etkin Dizin kiracısında bulunur ve farklı bir Azure Etkin Dizin kiracısında bulunan bir kaynağa karşı kullanılamaz. Bu nedenle, MSI'ı yalnızca Azure Akış Analizi işinizle aynı Azure Active Directory kiracısında bulunan ADLS Gen 1 kaynaklarında kullanabilirsiniz. 

2. **[Kullanıcı Atanan Kimlik](../active-directory/managed-identities-azure-resources/overview.md)**: desteklenmez. Bu, kullanıcının Stream Analytics işi tarafından kullanılmak üzere kendi hizmet ilkesini giremeyecekleri anlamına gelir. Hizmet sorumlusu Azure Akış Analizi tarafından oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

* [Akış analitiği yle Veri gölü Deposu çıktısı oluşturma](../data-lake-store/data-lake-store-stream-analytics.md)
* [Visual Studio ile Test Stream Analytics sorgularını yerel olarak sorgulayın](stream-analytics-vs-tools-local-run.md)
* [Visual Studio için Azure Akış Analizi araçlarını kullanarak canlı verileri yerel olarak test edin](stream-analytics-live-data-local-testing.md) 
