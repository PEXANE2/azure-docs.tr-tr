---
title: InfluxData Telegraf aracısı ile Linux VM için özel ölçümler toplayın
description: InfluxData Telegraf aracısının Azure'daki bir Linux VM'de nasıl dağıtılanağacağına ve aracıyı Azure Monitor'da ölçümleri yayımlayacak şekilde yapılandırmaya ilişkin yönergeler.
author: anirudhcavale
services: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 0ed9144116c1d716124025ef0aae39e7783c5934
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655472"
---
# <a name="collect-custom-metrics-for-a-linux-vm-with-the-influxdata-telegraf-agent"></a>InfluxData Telegraf aracısı ile bir Linux VM için özel ölçümler toplayın

Azure Monitor'u kullanarak, uygulama telemetriniz, Azure kaynaklarınızda çalışan bir aracı ve hatta dışarıdan izleme sistemleri aracılığıyla özel ölçümler toplayabilirsiniz. Ardından bunları doğrudan Azure Monitor'a gönderebilirsiniz. Bu makalede, [InfluxData](https://www.influxdata.com/) Telegraf aracısının Azure'daki bir Linux VM'de nasıl dağıtılanıncaya kadar dağıtılanın caizdir ve aracıyı Azure Monitor'da ölçümleri yayımlayacak şekilde yapılandırın. 

## <a name="influxdata-telegraf-agent"></a>InfluxData Telegraf aracısı 

[Telegraf,](https://docs.influxdata.com/telegraf/) 150'den fazla farklı kaynaktan ölçümlerin toplanmasını sağlayan bir eklenti aracıdır. VM'nizde hangi iş yüklerinin çalıştırıldığına bağlı olarak, aracıyı ölçümleri toplamak için özel giriş eklentilerinden yararlanacak şekilde yapılandırabilirsiniz. Örnek Olarak MySQL, NGINX ve Apache verilebilir. Temsilci, çıktı eklentilerini kullanarak seçtiğiniz hedeflere yazabilir. Telegraf aracısı doğrudan Azure Monitor özel ölçümleri REST API ile tümleşik olmuştur. Azure Monitor çıktı eklentisi destekler. Aracı, bu eklentiyi kullanarak Linux VM'nizde iş yüküne özel ölçümler toplayabilir ve bunları Azure Monitor'a özel ölçümler olarak gönderebilir. 

 ![Telgraf temsilcisine genel bakış](./media/collect-custom-metrics-linux-telegraf/telegraf-agent-overview.png)

## <a name="send-custom-metrics"></a>Özel ölçümler gönderme 

Bu öğretici için, Ubuntu 16.04 LTS işletim sistemini çalıştıran bir Linux VM dağıtıyoruz. Telegraf aracısı çoğu Linux işletim sistemi için desteklenir. Hem Debian ve RPM paketleri [InfluxData indirme portalında](https://portal.influxdata.com/downloads)paketlenmemiş Linux ikilileri ile birlikte mevcuttur. Ek yükleme yönergeleri ve seçenekleri için bu [Telegraf yükleme kılavuzuna](https://docs.influxdata.com/telegraf/v1.8/introduction/installation/) bakın. 

[Azure portalında](https://portal.azure.com)oturum açın.

Yeni bir Linux VM oluşturun: 

1. Sol navigasyon bölmesinden **kaynak oluştur** seçeneğini seçin. 
1. Sanal **Makine**için arama .  
1. **Ubuntu 16.04 LTS'yi** seçin ve **Oluştur'u**seçin. 
1. **MyTelegrafVM**gibi bir VM adı sağlayın.  
1. Disk türünü **SSD**olarak bırakın. Ardından **azureuser**gibi bir **kullanıcı adı**sağlayın. 
1. **Kimlik Doğrulama türü için** **Parola'yı**seçin. Daha sonra bu VM'ye SSH için kullanacağınız bir parola girin. 
1. Yeni **kaynak grubu oluşturmayı**seçin. Ardından **myResourceGroup**gibi bir ad sağlayın. **Konumunuzu**seçin. Sonra **Tamam**’ı seçin. 

    ![Ubuntu sanal makinesi oluşturma](./media/collect-custom-metrics-linux-telegraf/create-vm.png)

1. VM için bir boyut seçin. Örneğin, **İşlem türü** veya **Disk türü**’ne göre filtreleyebilirsiniz. 

    ![Sanal makine boyutu Telegraph aracısı genel bakış](./media/collect-custom-metrics-linux-telegraf/vm-size.png)

1.  >  **Ağ****Ağı Güvenlik Grubu'ndaki** >  **Ayarlar** sayfasında ortak gelen**bağlantı noktalarını seçin,** **HTTP** ve **SSH (22) seçeneğini**belirleyin. Diğer varsayılan ayarları olduğu gibi bırakın ve **Tamam**’ı seçin. 

1. Özet sayfasında **Oluştur**’u seçerek sanal makine dağıtımını başlatın. 

1. VM, Azure portalı panosuna sabitlenir. Dağıtım bittikten sonra VM özeti otomatik olarak açılır. 

1. VM bölmesinde, **Kimlik** sekmesine gidin. **On** 
 
    ![Telegraf VM kimlik önizleme](./media/collect-custom-metrics-linux-telegraf/connect-to-VM.png)
 
## <a name="connect-to-the-vm"></a>VM’ye bağlanma 

VM ile bir SSH bağlantısı oluşturun. VM’nizin genel bakış sayfasından **Bağlan** düğmesini seçin. 

![Telegraf VM genel bakış sayfası](./media/collect-custom-metrics-linux-telegraf/connect-VM-button2.png)

**Sanal makineye bağlan** sayfasında, 22 numaralı bağlantı noktası üzerinden DNS adına göre bağlanmak için varsayılan seçenekleri olduğu gibi bırakın. **VM yerel hesabı kullanarak Giriş,** bir bağlantı komutu gösterilir. Komutu kopyalamak için düğmeyi seçin. Aşağıdaki örnekte SSH bağlantı komutunun nasıl göründüğü gösterilmiştir: 

```cmd
ssh azureuser@XXXX.XX.XXX 
```

SSH bağlantı komutunu Windows'ta Ubuntu'da Azure Bulut Kabuğu veya Bash gibi bir kabuk haline getirin veya bağlantıyı oluşturmak için seçtiğiniz bir SSH istemcisini kullanın. 

## <a name="install-and-configure-telegraf"></a>Telegraf'ı yükleme ve yapılandırma 

Telegraf Debian paketini VM'ye yüklemek için SSH oturumunuzdaki aşağıdaki komutları çalıştırın: 

```cmd
# download the package to the VM 
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.8.0~rc1-1_amd64.deb 
# install the package 
sudo dpkg -i telegraf_1.8.0~rc1-1_amd64.deb
```
Telegraf'ın yapılandırma dosyası Telegraf'ın işlemlerini tanımlar. Varsayılan olarak, bir örnek yapılandırma dosyası yol **/ etc/telegraf/telegraf.conf**yüklenir. Örnek yapılandırma dosyası, olası tüm giriş ve çıktı eklentilerini listeler. Ancak, özel bir yapılandırma dosyası oluşturacağız ve aracının aşağıdaki komutları çalıştırarak dosyayı kullanmasını sağlayacağız: 

```cmd
# generate the new Telegraf config file in the current directory 
telegraf --input-filter cpu:mem --output-filter azure_monitor config > azm-telegraf.conf 

# replace the example config with the new generated config 
sudo cp azm-telegraf.conf /etc/telegraf/telegraf.conf 
```

> [!NOTE]  
> Önceki kod yalnızca iki giriş eklentisi sağlar: **cpu** ve **mem**. Makinenizde çalışan iş yüküne bağlı olarak daha fazla giriş eklentisi ekleyebilirsiniz. Docker, MySQL ve NGINX buna örnek olarak verilebilir. Giriş eklentilerinin tam listesi için **Ek yapılandırma** bölümüne bakın. 

Son olarak, aracının yeni yapılandırmayı kullanmaya başlaması için aracıyı aşağıdaki komutları çalıştırarak durdurmaya ve başlatmaya zorlarız: 

```cmd
# stop the telegraf agent on the VM 
sudo systemctl stop telegraf 
# start the telegraf agent on the VM to ensure it picks up the latest configuration 
sudo systemctl start telegraf 
```
Artık aracı, belirtilen giriş eklentilerinin her birinden ölçümler toplayacak ve bunları Azure Monitor'a yayacak. 

## <a name="plot-your-telegraf-metrics-in-the-azure-portal"></a>Azure portalında Telegraf ölçümlerinizi çizin 

1. Azure [portalını](https://portal.azure.com)açın. 

1. Yeni **Monitör** sekmesine gidin. Ardından **Ölçümler'i**seçin.  

     ![Monitör - Ölçümler (önizleme)](./media/collect-custom-metrics-linux-telegraf/metrics.png)

1. Kaynak seçicide VM'nizi seçin.

     ![Metrik grafik](./media/collect-custom-metrics-linux-telegraf/metric-chart.png)

1. **Telegraf/CPU** ad alanını seçin ve **usage_system** metrik'i seçin. Bu metrikteki boyutlara göre filtre uygulama yapmayı veya üzerlerine bölmeyi seçebilirsiniz.  

     ![Ad alanı ve metrik seçin](./media/collect-custom-metrics-linux-telegraf/VM-resource-selector.png)

## <a name="additional-configuration"></a>Ek yapılandırma 

Önceki izbis, telegraf aracısını birkaç temel giriş eklentisinden ölçümler toplamak üzere nasıl yapılandırılabilenbilgiler sağlar. Telegraf aracısı 150'den fazla giriş eklentisi için destek sağlarken, bazıları ek yapılandırma seçeneklerini destekler. InfluxData desteklenen [eklentileri](https://docs.influxdata.com/telegraf/v1.7/plugins/inputs/) ve bunları [yapılandırmak için nasıl](https://docs.influxdata.com/telegraf/v1.7/administration/configuration/)talimatlar bir listesini yayımladı.  

Ayrıca, bu gözden geçirme de, aracının dağıtılan VM hakkında ölçümler yontmak için Telegraf aracısını kullandınız. Telegraf aracısı, diğer kaynaklar için ölçümlerin toplayıcısı ve iletmesi olarak da kullanılabilir. Aracıyı diğer Azure kaynakları için ölçümler yatacak şekilde nasıl yapılandıracağını öğrenmek [için, Telegraf için Azure Monitörü Özel Metrik](https://github.com/influxdata/telegraf/blob/fb704500386214655e2adb53b6eb6b15f7a6c694/plugins/outputs/azure_monitor/README.md)Çıktısı'na bakın.  

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Artık bunlara ihtiyaç duyulmadığında, kaynak grubunu, sanal makineyi ve ilgili tüm kaynakları silebilirsiniz. Bunu yapmak için, sanal makine için kaynak grubunu seçin ve **Sil'i**seçin. Ardından silmek için kaynak grubunun adını onaylayın. 

## <a name="next-steps"></a>Sonraki adımlar
- [Özel ölçümler](metrics-custom-overview.md)hakkında daha fazla bilgi edinin.



