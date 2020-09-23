---
title: Azure portal kullanarak Azure SQL Edge dağıtma
description: Azure portal kullanarak Azure SQL Edge 'i dağıtmayı öğrenin
keywords: SQL Edge dağıtma
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/22/2020
ms.openlocfilehash: ffd967797a4e586387a0385169672220727f78a7
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90886748"
---
# <a name="deploy-azure-sql-edge"></a>Azure SQL Edge 'i dağıtma 

Azure SQL Edge, IoT ve Azure IoT Edge dağıtımları için iyileştirilmiş bir ilişkisel veritabanı altyapısıdır. IoT uygulamaları ve çözümleri için yüksek performanslı veri depolama ve işleme katmanı oluşturma olanakları sağlar. Bu hızlı başlangıçta, Azure portal kullanarak Azure IoT Edge aracılığıyla Azure SQL Edge modülü oluşturmaya nasıl başlacağınız gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* [Azure Portal](https://portal.azure.com/) oturum açın.
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

4. **Cihazdaki modülleri ayarla:** sayfasında, **IoT Edge modüller**altında Azure SQL Edge modülüne tıklayın. Varsayılan modül adı *Azuresqledge*olarak ayarlanır. 

5. **Güncelleştirme IoT Edge modülü** dikey penceresinin *modül ayarları* bölümünde *IoT Edge modül adı*için Istenen değerleri belirtin, ilke ve *istenen durum*' u *yeniden başlatın* . 

   > [!IMPORTANT]    
   > Modülün **görüntü URI** ayarlarını değiştirmeyin veya güncelleştirmeyin.

6. **Güncelleştirme IoT Edge modülü** dikey penceresindeki *ortam değişkenleri* bölümünde, ortam değişkenleri için istenen değerleri belirtin. Azure SQL Edge ortam değişkenlerinin tüm listesi için [ortam değişkenlerini kullanarak yapılandırma](configure.md#configure-by-using-environment-variables)konusuna bakın. Aşağıdaki varsayılan ortam değişkenleri modül için tanımlanmıştır. 

   |**Parametre**  |**Açıklama**|
   |---------|---------|
   | Ad | Modülün adı. |
   | MSSQL_SA_PASSWORD  | SQL Edge yönetici hesabı için güçlü bir parola belirtmek üzere varsayılan değeri değiştirin. |
   | MSSQL_LCID   | İstenen dil KIMLIĞINI SQL Edge için kullanılacak şekilde ayarlamak için varsayılan değeri değiştirin. Örneğin, 1036, Fransızca ' dır. |
   | MSSQL_COLLATION | Varsayılan değeri SQL Edge için Varsayılan harmanlamayı ayarlanacak şekilde değiştirin. Bu ayar, dil KIMLIĞI 'nin (LCıD) varsayılan eşlemesini harmanlama olarak geçersiz kılar. |

   > [!IMPORTANT]    
   > Modülün **ACCEPT_EULA** ortam değişkenini değiştirmeyin veya güncelleştirmeyin.

7. **Güncelleştirme IoT Edge modülü** dikey penceresinin *kapsayıcı oluşturma seçenekleri* bölümünde, aşağıdaki seçenekleri gereksinime göre güncelleştirin. 
   - **Ana bilgisayar bağlantı noktası:** Belirtilen ana bilgisayar bağlantı noktasını kapsayıcıda bağlantı noktası 1433 (varsayılan SQL bağlantı noktası) ile eşleyin.
   - **Bağlar** ve **bağlar:** birden fazla SQL Edge modülü dağıtmanız gerekiyorsa, kalıcı birim için yeni bir kaynak & hedef çifti oluşturmak üzere bağlama seçeneğini güncelleştirdiğinizden emin olun. Bağlama ve birim hakkında daha fazla bilgi için Docker belgelerindeki [birimleri kullanma](https://docs.docker.com/storage/volumes/) konusuna bakın. 

   ```json
   {
    "HostConfig": {
        "CapAdd": [
            "SYS_PTRACE"
        ],
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
        "ClientTransportType=AMQP_TCP_Only",
        "PlanId=asde-developer-on-iot-edge"
    ]
   }
   ```
   > [!IMPORTANT]    
   > `PlanId`Oluşturma yapılandırması ayarında tanımlanan çalıştırılmaları değişkenini değiştirmeyin. Bu değer değiştirilirse Azure SQL Edge kapsayıcısı başlayamaz. 
   
8. **Güncelleştirme IoT Edge modülü** bölmesinde **Güncelleştir**' e tıklayın.
9. **Cihazdaki modülleri ayarla** sayfasında ileri ' ye tıklayın. dağıtımınız için rotalar tanımlamanız gerekiyorsa **yollar >** . Aksi takdirde, **gözden geçir + oluştur**seçeneğine tıklayın. Yolları yapılandırma hakkında daha fazla bilgi için bkz. [IoT Edge modülleri dağıtma ve yolları oluşturma](../iot-edge/module-composition.md).
11. **Güncelleştirme IoT Edge modülü** bölmesinde **Oluştur**' a tıklayın.

## <a name="connect-to-azure-sql-edge"></a>Azure SQL Edge 'e bağlanma

Aşağıdaki adımlar, Azure SQL Edge 'e bağlanmak için kapsayıcının içindeki **sqlcmd**Azure SQL Edge komut satırı aracını kullanır.

> [!NOTE]      
> SQL komut satırı araçları (sqlcmd), Azure SQL Edge kapsayıcıları 'nın ARM64 sürümünde kullanılamaz.

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

Aşağıdaki bölümler, yeni bir veritabanı oluşturmak, veri eklemek ve bir sorgu çalıştırmak için **sqlcmd** ve Transact-SQL ' i kullanma konusunda size rehberlik sağlar.

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

- [SQL Edge 'de ONNX ile Machine Learning ve yapay zeka](onnx-overview.md)
- [IoT Edge kullanarak SQL Edge ile uçtan uca IoT çözümü oluşturma](tutorial-deploy-azure-resources.md)
- [Azure SQL Edge 'de veri akışı](stream-data.md)
- [Dağıtım hatalarını giderme](troubleshoot.md)
