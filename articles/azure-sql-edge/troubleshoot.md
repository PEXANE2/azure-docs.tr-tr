---
title: Azure SQL Edge dağıtımları sorunlarını giderme
description: Azure SQL Edge dağıtımı sırasında olası hatalar hakkında bilgi edinin
keywords: SQL Edge, sorun giderme, dağıtım hataları
services: sql-edge
ms.service: sql-edge
ms.topic: troubleshooting
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/22/2020
ms.openlocfilehash: 517fed0dd9eb1736344546bde9f79e52ee17182f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91333112"
---
# <a name="troubleshooting-azure-sql-edge-deployments"></a>Azure SQL Edge dağıtımları sorunlarını giderme 

Bu makalede, Azure SQL Edge kapsayıcıları dağıtılırken ve kullanılırken görülen olası hatalar hakkında bilgi sağlanır ve bu sorunları çözmeye yardımcı olmak için sorun giderme teknikleri sağlanmıştır. 

Azure SQL Edge iki dağıtım modelini destekler: 
- Azure IoT Edge aracılığıyla bağlı dağıtım: Azure SQL Edge, Azure Marketi 'nde kullanılabilir ve [Azure IoT Edge](../iot-edge/about-iot-edge.md)bir modül olarak dağıtılabilir. Daha fazla bilgi için bkz. [Azure SQL Edge 'ı dağıtma](deploy-portal.md).<br>

- Bağlantısı kesik dağıtım: Azure SQL Edge kapsayıcı görüntüleri Docker Hub 'ından çekilir ve tek başına bir Docker kapsayıcısı veya bir Kubernetes kümesi olarak dağıtılabilir. Daha fazla bilgi için bkz. [Azure SQL Edge 'ı Docker Ile dağıtma](disconnected-deployment.md) ve [Kubernetes 'Te Azure SQL Edge kapsayıcısı dağıtma](deploy-kubernetes.md).

## <a name="troubleshooting-iot-edge-device-and-deployments"></a>Cihaz ve dağıtımların sorunlarını giderme IoT Edge

Azure IoT Edge aracılığıyla SQL Edge dağıtma sırasında hata alırsanız, `iotedge` hizmetin düzgün yapılandırıldığından ve çalıştığından emin olun. Aşağıdaki belgeler Azure IoT Edge ilgili sorunları giderirken yararlı olabilir:
- [Azure IoT Edge Için sık karşılaşılan sorunlar ve çözümleri](../iot-edge/troubleshoot-common-errors.md).
- [IoT Edge cihazınızda sorun giderme](../iot-edge/troubleshoot.md)

## <a name="docker-command-errors"></a>Docker komut hataları

Herhangi bir komut için hata alırsanız `docker` , Docker hizmetinin çalıştığından emin olun ve yükseltilmiş izinlerle çalıştırmayı deneyin.

Örneğin, Linux 'ta komutları çalıştırırken şu hatayı alabilirsiniz `docker` :

```output
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

Linux 'ta bu hatayı alırsanız, ile önceden yapılan komutların aynısını çalıştırmayı deneyin `sudo` . Bu başarısız olursa, Docker hizmetinin çalıştığını doğrulayın ve gerekirse başlatın.

```bash
sudo systemctl status docker
sudo systemctl start docker
```

Windows 'ta, PowerShell 'i veya komut istediğinizi yönetici olarak başlatdığınıza emin olun.

## <a name="azure-sql-edge-container-startup-errors"></a>Azure SQL Edge kapsayıcısı başlatma hataları

SQL Edge kapsayıcısı çalışamazsa, aşağıdaki testleri deneyin:

- Azure IoT Edge kullanıyorsanız, modül görüntülerinin başarılı bir şekilde indirildiğinden ve ortam değişkenlerinin ve kapsayıcı oluşturma seçeneklerinin modül bildiriminde doğru belirtildiğinden emin olun.

- Docker veya Kubernetes tabanlı dağıtım kullanıyorsanız, `docker run` komutun doğru biçimlendirildiğinden emin olun. Daha fazla bilgi için [Azure SQL Edge 'ı Docker Ile dağıtma](disconnected-deployment.md) ve [Kubernetes 'Te Azure SQL Edge kapsayıcısı dağıtma](deploy-kubernetes.md)konusuna bakın.

- Gibi bir hata alırsanız `failed to create endpoint CONTAINER_NAME on network bridge. Error starting proxy: listen tcp 0.0.0.0:1433 bind: address already in use.` , 1433 kapsayıcı bağlantı noktasını zaten kullanımda olan bir bağlantı noktasıyla eşlemeye çalışıyorsunuz. Bu durum, ana makinede SQL Edge 'i yerel olarak çalıştırıyorsanız gerçekleşebilir. İki SQL Edge kapsayıcısı başlatırsanız ve bunları aynı ana bilgisayar bağlantı noktasına eşlemenize çalışırsanız da bu durum oluşabilir. Bu durumda, `-p` 1433 kapsayıcı bağlantı noktasını farklı bir ana bilgisayar bağlantı noktasına eşlemek için parametresini kullanın. Örneğin: 

    ```bash
    sudo docker run --cap-add SYS_PTRACE -e 'ACCEPT_EULA=1' -e 'MSSQL_SA_PASSWORD=yourStrong(!)Password' -p 1433:1433 --name azuresqledge -d mcr.microsoft.com/azure-sql-edge-developer.
    ```

- `Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.30tdout=1&tail=all: dial unix /var/run/docker.sock: connect: permission denied`Bir kapsayıcıyı başlatmaya çalışırken bir hata alırsanız, daha sonra Ubuntu 'daki Docker grubuna kullanıcı ekleyin. Ardından oturumu kapatıp yeniden oturum açın, bu değişiklik yeni oturumları etkiler. 

   ```bash
    usermod -aG docker $USER
   ```

- Kapsayıcıdan hata iletileri olup olmadığına bakın.

   ```bash
   docker logs e69e056c702d
   ```

- Herhangi bir kapsayıcı yönetimi yazılımı kullanıyorsanız, kök olarak çalışan kapsayıcı süreçlerini desteklediğinden emin olun. Kapsayıcıdaki sqlservr işlemi kök olarak çalışır.

- Varsayılan olarak, Azure SQL Edge kapsayıcıları adlı kök olmayan bir kullanıcı olarak çalışır `mssql` . Verileri kalıcı hale getirmek için bağlama noktaları veya veri birimleri kullanıyorsanız, `mssql` kullanıcının birimde uygun izinlere sahip olduğundan emin olun. Daha fazla bilgi için bkz. [kök olmayan kullanıcı olarak çalıştırma](configure.md#run-azure-sql-edge-as-non-root-user) ve [verileri kalıcı hale](configure.md#persist-your-data)getirme.

- SQL Edge Docker Kapsayıcınız başladıktan hemen sonra çıkılırken Docker günlüklerinizi denetleyin. Komutuyla Windows üzerinde PowerShell kullanıyorsanız `docker run` , tek tırnak yerine çift tırnak işareti kullanın. PowerShell Core ile tek tırnak kullanın.

- [SQL Edge hata günlüklerini](#errorlogs)gözden geçirin.

## <a name="sql-edge-connection-failures"></a>SQL Edge bağlantı sorunları

Kapsayıcıda çalışan SQL Edge örneğine bağlanamıyorsanız, aşağıdaki testleri deneyin:

- Çıktının **durum** sütununa bakarak SQL Edge kapsayıcının çalıştığından emin olun `docker ps -a` . Aksi takdirde, `docker start <Container ID>` başlatmak için kullanın.

- Varsayılan olmayan bir ana bilgisayar bağlantı noktasına (1433 değil) eşlendiyseniz bağlantı dizinizdeki bağlantı noktasını belirttiğinizden emin olun. Bağlantı noktası eşlemenizi çıktının **bağlantı noktaları** sütununda görebilirsiniz `docker ps -a` . Azure SQL Edge 'e bağlanma hakkında daha fazla bilgi için bkz. [Azure SQL Edge 'e bağlanma ve sorgulama](connect.md)

- Daha önce eşlenmiş veri birimi veya veri birimi kapsayıcısı ile SQL Edge dağıttıysanız ve artık mevcut eşlenmiş veri birimini veya veri birimi kapsayıcısını kullanıyorsa, SQL Edge `MSSQL_SA_PASSWORD` ortam değişkeninin değerini yoksayar. Bunun yerine, daha önce yapılandırılmış olan SA Kullanıcı parolası kullanılır. Bu durum SQL Edge eşlenen birimde veya veri birimi kapsayıcısında var olan ana veritabanları dosyalarını yeniden kullandığı için oluşur. Bu sorunla karşılaşırsanız, aşağıdaki seçenekleri kullanabilirsiniz:

    - Hala kullanılabiliyorsa, daha önce kullanılan parolayı kullanarak bağlanın.
    - SQL Edge 'i farklı eşlenmiş birim veya veri birimi kapsayıcısı kullanacak şekilde yapılandırın.
    - Mevcut ana veritabanı dosyalarını (Master. mdf ve Mastlog. mdf) eşlenen birim veya veri birimi kapsayıcısından kaldırın.

- [SQL Edge hata günlüklerini](#errorlogs)gözden geçirin.

## <a name="sql-edge-setup-and-error-logs"></a><a id="errorlogs"></a> SQL Edge kurulumu ve hata günlükleri

Varsayılan olarak, SQL Edge hata günlükleri kapsayıcı içindeki **/var/seçenek/MSSQL/log** dizininde bulunur ve aşağıdaki yollarla erişilebilir:

- Kapsayıcınızı oluştururken **/var/seçenek/MSSQL** dizinine bir ana bilgisayar dizini bağladıysanız, bunun yerine konaktaki eşlenen yoldaki **günlük** alt dizinine bakabilirsiniz.
- Bir etkileşimli komut istemi kullanarak kapsayıcıya bağlanma. Kapsayıcı çalışmıyorsa, önce kapsayıcıyı başlatın. Ardından, günlükleri incelemek için etkileşimli bir komut istemi kullanın. Komutunu çalıştırarak kapsayıcı KIMLIĞINI alabilirsiniz `docker ps` .

    ```bash
    docker start <ContainerID>
    docker exec -it <ContainerID> "/bin/bash"
    ```

    Kapsayıcının içindeki Bash oturumunda aşağıdaki komutları çalıştırın:

    ```bash
    cd /var/opt/mssql/log
    cat errorlog
    ```
- SQL Edge kapsayıcısı çalışır durumda ve istemci araçlarını kullanarak örneğe bağlanıyorsanız, `sp_readerrorlog` SQL Edge hata günlüğünün içeriğini okumak için saklı yordamı kullanabilirsiniz.

## <a name="execute-commands-in-a-container"></a>Bir kapsayıcıda komutları yürütme

Çalışan bir kapsayıcınız varsa kapsayıcı içindeki komutları bir konak terminalinden çalıştırabilirsiniz.

Kapsayıcı KIMLIĞI 'nin çalıştırılmasını sağlamak için:

```bash
docker ps -a
```

Kapsayıcı çalıştırmasında bir bash terminali başlatmak için:

```bash
docker exec -it <Container ID> /bin/bash
```

Artık komutları kapsayıcının içindeki terminalde çalıştırıyor olsanız gibi çalıştırabilirsiniz. İşiniz bittiğinde yazın `exit` . Bu etkileşimli komut oturumunda çıkar, ancak Kapsayıcınız çalışmaya devam eder.

### <a name="enabling-verbose-logging"></a>Ayrıntılı günlük kaydı etkinleştiriliyor

Akış altyapısının varsayılan günlük düzeyi yeterli bilgi sağlamıyorsa, akış altyapısının hata ayıklama günlüğü SQL Edge 'de etkinleştirilebilir. Hata ayıklama günlüğünü etkinleştirmek için, `RuntimeLogLevel=debug` ortam DEĞIŞKENINI SQL Edge dağıtımınıza ekleyin. Hata ayıklama günlüğünü etkinleştirdikten sonra, sorunu yeniden oluşturmaya çalışın ve ilgili herhangi bir ileti veya özel durum için günlükleri denetleyin. 

> [!NOTE]
> Ayrıntılı günlük oluşturma seçeneği yalnızca sorun giderme için kullanılmalıdır, normal üretim iş yükleri için kullanılmamalıdır. 


## <a name="next-steps"></a>Sonraki adımlar

- [SQL Edge 'de ONNX ile Machine Learning ve yapay zeka](onnx-overview.md)
- [Azure SQL Edge 'de veri akışı](stream-data.md)
- [Veri saklama ve Temizleme](data-retention-overview.md)
- [Zaman boşlukları dolduruluyor ve eksik değerleri ayarlama](imputing-missing-values.md)







