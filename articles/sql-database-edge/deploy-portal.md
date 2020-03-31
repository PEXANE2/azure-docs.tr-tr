---
title: Azure portalını kullanarak SQL Veritabanı Kenarı Önizlemesini Dağıtma | Microsoft Dokümanlar
description: Azure portalını kullanarak Azure SQL Veritabanı Kenarını nasıl dağıtılayın öğrenin
keywords: sql veritabanı kenarını dağıtmak
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 9da922de38d820864b3f83de80fe64eb3ac792e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246730"
---
# <a name="deploy-azure-sql-database-edge-preview"></a>Azure SQL Veritabanı Kenarı Önizlemesini Dağıtma

Azure SQL Veritabanı Kenarı Önizleme, IoT ve Azure IoT Edge dağıtımları için optimize edilmiş ilişkisel bir veritabanı altyapısıdır. IoT uygulamaları ve çözümleri için yüksek performanslı bir veri depolama ve işleme katmanı oluşturmak için yetenekler sağlar. Bu hızlı başlangıç, Azure portalını kullanarak Azure IoT Edge üzerinden bir Azure SQL Veritabanı Kenarı modülü oluşturmaya nasıl başlayacağınızı gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/)oluşturun.
* [Azure portalında](https://portal.azure.com/)oturum açın.
* SQL Database Edge dağıtımı için aboneliği etkinleştirmek için [burada](https://azure.microsoft.com/services/sql-database-edge/#contact)bir istek gönderin.
* Azure [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md)oluşturun.
* Azure [portalından bir IoT Edge Aygıtı kaydedin.](../iot-edge/how-to-register-device-portal.md)
* IoT Edge aygıtını [Azure portalından IoT Edge modüllerini dağıtmak](../iot-edge/how-to-deploy-modules-portal.md)için hazırlayın.

> [!NOTE]
> Azure Linux VM'yi IoT Edge aygıtı olarak dağıtmak için bu [hızlı başlangıç kılavuzuna](../iot-edge/quickstart-linux.md)bakın.

## <a name="deploy-sql-database-edge-module-from-azure-marketplace"></a>Azure Marketinden SQL Veritabanı Kenar Modüllerini Dağıtma

Azure Marketi, [IoT Edge modülleri](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)de dahil olmak üzere Azure'da çalışacak şekilde onaylı ve optimize edilmiş çok çeşitli kurumsal uygulamalara ve çözümlere göz atabileceğiniz çevrimiçi bir uygulama ve hizmet pazarıdır. Azure SQL Veritabanı Kenarı, pazar yeri üzerinden bir kenar aygıtına dağıtılabilir.

1. Azure Marketi'ndeki Azure SQL Veritabanı Kenarı modüllerini bulun.<br><br>

   ![MarketPlace'te SQL Veritabanı Kenarı](media/deploy-portal/find-offer-marketplace.png)

2. Gereksinimlerinize en uygun yazılım planını seçin ve **Oluştur'u**tıklatın. <br><br>

   ![Doğru yazılım planını seçin](media/deploy-portal/pick-correct-plan.png)

3. IoT Edge Modülü için Hedef Aygıtlar sayfasında aşağıdaki ayrıntıları belirtin ve ardından **Oluştur'u** tıklatın

   |**Alan**  |**Açıklama**  |
   |---------|---------|
   |Abonelik  |  IoT Hub'ın oluşturulduğu Azure aboneliği |
   |IoT Hub   |  IoT Edge aygıtının kayıtlı olduğu IoT Hub'ının adı ve ardından "Aygıta Dağıt" seçeneğini belirleyin|
   |IoT Edge Cihaz Adı  |  SQL Database Edge'in dağıtılanacağı IoT Edge aygıtının adı |

4. **Modülleri Ayarla** sayfasında, dağıtım modülleri bölümüne gidin ve SQL Veritabanı Kenarı modülüne karşı **Yapılandır'ı** tıklatın. 

5. **IoT Edge Özel Modüller** bölmede, ortam değişkenleri için istenen değerleri belirtin ve/veya modül için oluşturma seçeneklerini ve istenen özellikleri özelleştirin. Desteklenen ortam değişkenlerinin tam listesi için [SQL Server Konteyner Çevre Değişkenleri'ni başvurun.](/sql/linux/sql-server-linux-configure-environment-variables/)

   |**Parametre**  |**Açıklama**|
   |---------|---------|
   | Adı | Modülün adı. |
   |SA_PASSWORD  | SQL Database Edge yönetici hesabı için güçlü bir parola belirtin. |
   |MSSQL_LCID   | SQL Server için kullanılacak dil kimliğini ayarlar. Örneğin, 1036 Fransızcadır. |
   |MSSQL_COLLATION | SQL Server için varsayılan harmanlama ayarlar. Bu ayar, dil kimliği (LCID) ile harmanlama için varsayılan eşleme geçersiz kılar. |

   > [!NOTE]
   > Lütfen **Image URI'yi** veya modüldeki **ACCEPT_EULA** ayarlarını değiştirmeyin veya güncellemeyin.

6. **IoT Edge Özel Modüller** bölmesinde, **kapsayıcıyı güncelleştirin Ana Bağlantı Noktası**için istenen değerde seçenekler oluşturun. Birden fazla SQL DB Edge modülü dağıtmanız gerekiyorsa, kalıcı birim için yeni bir kaynak & hedef çifti oluşturmak için bağlar seçeneğini güncelleştirin. Bağlar ve hacim hakkında daha fazla bilgi için docker belgelerindeki [birimleri kullan'a](https://docs.docker.com/storage/volumes/) bakın. 

   ```json
       {
         "HostConfig": {
           "Binds": [
             "sqlvolume:/sqlvolume"
           ],
           "PortBindings": {
             "1433/tcp": [
               {
                 "HostPort": "1433"
               }
             ]
           },
           "Mounts": [
             {
               "Type": "volume",
               "Source": "sqlvolume",
               "Target": "/var/opt/mssql"
             }
           ]
         },
         "Env": [
           "MSSQL_AGENT_ENABLED=TRUE",
           "MSSQL_PID=Developer"
         ]
       }
   ```

7. **IoT Edge Özel Modüller** bölmesinde, SQL paketinin konumunu ve akış analizi iş bilgilerini içerecek şekilde *Set modülü ikizinin istenen özelliklerini* güncelleyin. Bu iki alan isteğe bağlıdır ve SQL Veritabanı Kenarı modüllerini bir veritabanı ve akış işiyle dağıtmak istiyorsanız kullanılmalıdır.

   ```json
       {
         "properties.desired":
         {
           "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
           "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
         }
       }
   ```

8. **IoT Edge Özel Modüller** bölmesinde, *Yeniden Başlatma İlkesi'ni* her zaman ve *istenilen durum* için çalışmaya ayarlayın.
9. **IoT Edge Özel Modüller** bölmesine **kaydet'i**tıklatın.
10. **Modülleri Ayarla** sayfasında **İleri'yi**tıklatın.
11. **Modülleri Ayarla** sayfasının **Rotasını Belirt (isteğe bağlı)** üzerinde, Modülden Modüle veya Modül'e IoT Edge Hub iletişimine giden yolları belirtin, [modülleri dağıt'a bakın ve IoT Edge'de rotalar kurun.](../iot-edge/module-composition.md)
12. **İleri**'ye tıklayın.
13. **Gönder'i**tıklatın.

Bu hızlı başlatmada, bir IoT Edge aygıtına bir SQL Veritabanı Kenar Modülü dağıttınız.

## <a name="next-steps"></a>Sonraki Adımlar

- [SQL Database Edge'de ONNX ile Makine Öğrenimi ve Yapay Zeka](onnx-overview.md).
- IoT Edge kullanarak SQL Database Edge ile IoT Çözümle uçtan uca bir çözüm oluşturma.
