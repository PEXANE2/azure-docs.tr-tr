---
title: Azure portal kullanarak Azure SQL Edge (Önizleme) dağıtma
description: Azure SQL Edge 'i (Önizleme) Azure portal kullanarak dağıtmayı öğrenin
keywords: SQL Edge dağıtma
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 7af4264860f8d9950515cd5302f03822e7cbac39
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88816873"
---
# <a name="deploy-azure-sql-edge-preview"></a>Azure SQL Edge 'i dağıtma (Önizleme) 

Azure SQL Edge (Önizleme), IoT ve Azure IoT Edge dağıtımları için iyileştirilmiş bir ilişkisel veritabanı altyapısıdır. IoT uygulamaları ve çözümleri için yüksek performanslı veri depolama ve işleme katmanı oluşturma olanakları sağlar. Bu hızlı başlangıçta, Azure portal kullanarak Azure IoT Edge aracılığıyla Azure SQL Edge modülü oluşturmaya nasıl başlacağınız gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* [Azure portalında](https://portal.azure.com/) oturum açın.
* [Azure IoT Hub](../iot-hub/iot-hub-create-through-portal.md)oluşturun.
* Azure portal bir [IoT Edge cihazı](../iot-edge/how-to-register-device-portal.md)kaydedin.
* [Azure portal IoT Edge modülünü dağıtmak](../iot-edge/how-to-deploy-modules-portal.md)üzere IoT Edge cihazı hazırlayın.

> [!NOTE]
> Bir Azure Linux sanal makinesini IoT Edge bir cihaz olarak dağıtmak için bu [hızlı başlangıç kılavuzuna](../iot-edge/quickstart-linux.md)bakın.

## <a name="deploy-sql-edge-module-from-azure-marketplace"></a>Azure Marketi 'nden SQL Edge modülünü dağıtma

Azure Marketi, [IoT Edge modüller](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)dahil olmak üzere Azure 'da çalışmak üzere sertifikalı ve iyileştirilmiş çok sayıda kurumsal uygulama ve çözüm arasında dolaşabileceğiniz çevrimiçi bir uygulamalar ve hizmetler marketi. Azure SQL Edge, Market aracılığıyla bir uç cihaza dağıtılabilir.

1. Azure Market 'te Azure SQL Edge modülünü bulun.<br><br>

   ![Market 'te SQL Edge](media/deploy-portal/find-offer-marketplace.png)

2. Gereksinimlerinize en iyi eşleşen yazılım planını seçin ve **Oluştur**' a tıklayın. <br><br>

   ![Doğru yazılım planını seçin](media/deploy-portal/pick-correct-plan.png)

3. IoT Edge modülü için hedef cihazlar sayfasında, aşağıdaki ayrıntıları belirtin ve ardından **Oluştur** ' a tıklayın.

   |**Alan**  |**Açıklama**  |
   |---------|---------|
   |Abonelik  |  IoT Hub oluşturulduğu Azure aboneliği |
   |IoT Hub   |  IoT Edge cihazının kaydedildiği IoT Hub adı ve sonra "bir cihaza dağıt" seçeneğini belirleyin|
   |IoT Edge cihaz adı  |  SQL Edge 'in dağıtılacağı IoT Edge cihazının adı |

4. **Modülleri ayarla** sayfasında, dağıtım modülleri bölümüne gıdın ve SQL Edge modülüne göre **Yapılandır** ' a tıklayın. 

5. **IoT Edge özel modüller** bölmesinde, ortam değişkenleri için istenen değerleri belirtin ve/veya modül için oluşturma seçeneklerini ve istenen özellikleri özelleştirin. Desteklenen ortam değişkenlerinin tüm listesi için [SQL Server kapsayıcı ortam değişkenlerine](/sql/linux/sql-server-linux-configure-environment-variables/)başvurabilirsiniz.

   |**Parametre**  |**Açıklama**|
   |---------|---------|
   | Name | Modülün adı. |
   |SA_PASSWORD  | SQL Edge yönetici hesabı için güçlü bir parola belirtin. |
   |MSSQL_LCID   | SQL Server için kullanılacak dil KIMLIĞINI ayarlar. Örneğin, 1036, Fransızca ' dır. |
   |MSSQL_COLLATION | SQL Server için Varsayılan harmanlamayı ayarlar. Bu ayar, dil KIMLIĞI 'nin (LCıD) varsayılan eşlemesini harmanlama olarak geçersiz kılar. |

   > [!NOTE]
   > Lütfen **görüntü URI** 'sini veya modüldeki **ACCEPT_EULA** ayarlarını değiştirmeyin veya güncelleştirmeyin.

6. **IoT Edge özel modüller** bölmesinde, **konak bağlantı noktası**için istediğiniz kapsayıcı oluşturma seçenekleri değerini güncelleştirin. Birden fazla SQL DB Edge modülü dağıtmanız gerekiyorsa lütfen, kalıcı birim için yeni bir kaynak & hedef çifti oluşturmak üzere bağlama seçeneğini güncelleştirdiğinizden emin olun. Bağlama ve birim hakkında daha fazla bilgi için Docker belgelerindeki [birimleri kullanma](https://docs.docker.com/storage/volumes/) konusuna bakın. 

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

7. **IoT Edge özel modüller** BÖLMESINDE, SQL paketinin konumunu ve Stream Analytics iş bilgisini dahil etmek için *set Module ikizi 'ın istenen özelliklerini* güncelleştirin. Bu iki alan isteğe bağlıdır ve SQL Edge modülünü bir veritabanı ve akış işiyle dağıtmak istiyorsanız kullanılmalıdır.

   ```json
       {
         "properties.desired":
         {
           "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
           "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
         }
       }
   ```

8. **IoT Edge özel modüller** bölmesinde, *Yeniden Başlat ilkesini* her zaman ve *istenen durum* ' a ayarlayın.
9. **IoT Edge özel modüller** bölmesinde **Kaydet**' e tıklayın.
10. **Modülleri ayarla** sayfasında **İleri**' ye tıklayın.
11. **Modülleri ayarla** sayfasının **route (Isteğe bağlı) yolunu belirtin** sayfasında modüle modül veya modülle yönelik yolları IoT Edge hub iletişimi için belirtin [ve IoT Edge ' de yolları oluşturma '](../iot-edge/module-composition.md)ya bakın.
12. **İleri**’ye tıklayın.
13. **Gönder**' e tıklayın.

## <a name="connect-to-azure-sql-edge"></a>Azure SQL Edge 'e bağlanma

Aşağıdaki adımlar, Azure SQL Edge 'e bağlanmak için kapsayıcının içindeki **sqlcmd**Azure SQL Edge komut satırı aracını kullanır.

> [!NOTE]
> sqlcmd Aracı, SQL Edge kapsayıcıları 'nın ARM64 sürümünde kullanılamaz.

1. `docker exec -it`Çalışan kapsayıcının içinde etkileşimli bir bash kabuğu başlatmak için komutunu kullanın. Aşağıdaki örnekte, `azuresqledge` `Name` IoT Edge modülünüzün parametresi tarafından belirtilen addır.

   ```bash
   sudo docker exec -it azuresqledge "bash"
   ```

2. Kapsayıcının içindeyken sqlcmd ile yerel olarak bağlanın. Sqlcmd, varsayılan olarak yolda değildir, bu nedenle tam yolu belirtmeniz gerekir.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "<YourNewStrong@Passw0rd>"
   ```

   > [!TIP]
   > Komut satırında parolayı, girmeniz istenecek şekilde atlayabilirsiniz.

3. Başarılı olursa bir **sqlcmd** komut istemine almalısınız: `1>` .

## <a name="create-and-query-data"></a>Veri oluşturma ve sorgulama

Aşağıdaki bölümler, yeni bir veritabanı oluşturmak, veri eklemek ve basit bir sorgu çalıştırmak için **sqlcmd** ve Transact-SQL ' i kullanma konusunda size rehberlik sağlar.

### <a name="create-a-new-database"></a>Yeni veritabanı oluşturma

Aşağıdaki adımlar adlı yeni bir veritabanı oluşturur `TestDB` .

1. **Sqlcmd** komut isteminde, bir test veritabanı oluşturmak Için aşağıdaki Transact-SQL komutunu yapıştırın:

   ```sql
   CREATE DATABASE TestDB
   Go
   ```

2. Bir sonraki satırda, sunucunuzdaki tüm veritabanlarının adını döndürmek için bir sorgu yazın:

   ```sql
   SELECT Name from sys.Databases
   Go
   ```

### <a name="insert-data"></a>Veri ekleme

Sonra yeni bir tablo oluşturun `Inventory` ve iki yeni satır ekleyin.

1. **Sqlcmd** komut isteminde bağlamı yeni `TestDB` veritabanına geçir:

   ```sql
   USE TestDB
   ```

2. Adlı yeni tablo oluştur `Inventory` :

   ```sql
   CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT)
   ```

3. Yeni tabloya veri ekle:

   ```sql
   INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
   ```

4. `GO`Önceki komutları yürütmek için yazın:

   ```sql
   GO
   ```

### <a name="select-data"></a>Verileri seçme

Şimdi tablodaki verileri döndürmek için bir sorgu çalıştırın `Inventory` .

1. **Sqlcmd** komut isteminde, `Inventory` miktardan 152 ' den büyük olan tablodaki satırları döndüren bir sorgu girin:

   ```sql
   SELECT * FROM Inventory WHERE quantity > 152;
   ```

2. Komutu yürütün:

   ```sql
   GO
   ```

### <a name="exit-the-sqlcmd-command-prompt"></a>Sqlcmd komut isteminden çıkın

1. **Sqlcmd** oturumunuzu sonlandırmak için şunu yazın `QUIT` :

   ```sql
   QUIT
   ```

2. Kapsayıcıda etkileşimli komut isteminden çıkmak için yazın `exit` . Etkileşimli bash kabuğu 'ndan çıktıktan sonra Kapsayıcınız çalışmaya devam eder.

## <a name="connect-from-outside-the-container"></a>Kapsayıcının dışından Bağlan

SQL bağlantılarını destekleyen herhangi bir harici Linux, Windows veya macOS aracından Azure SQL Edge Örneğinizde SQL sorgularını bağlanabilir ve çalıştırabilirsiniz. Dışından bir SQL Edge kapsayıcısına bağlanma hakkında daha fazla bilgi için bkz. [Azure SQL Edge 'e bağlanma ve sorgulama](https://docs.microsoft.com/azure/azure-sql-edge/connect).

Bu hızlı başlangıçta, bir IoT Edge cihazında SQL Edge modülü dağıttınız. 

## <a name="next-steps"></a>Sonraki Adımlar

- [SQL Edge 'de ONNX ile Machine Learning ve yapay zeka](onnx-overview.md).
- [IoT Edge kullanarak SQL Edge ile uçtan uca IoT çözümü oluşturma](tutorial-deploy-azure-resources.md).
- [Azure SQL Edge 'de veri akışı](stream-data.md)
