---
title: Linux bilgisayarlarını Azure Monitör'e bağlayın | Microsoft Dokümanlar
description: Bu makalede, diğer bulutlarda veya şirket içinde barındırılan Linux bilgisayarlarının Linux için Log Analytics aracısıyla Azure Monitor'a nasıl bağlanılacak olduğu açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 01/21/2020
ms.openlocfilehash: aa2356901403c7a63aa4aa96dcb38f9c0c971e58
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80528343"
---
# <a name="connect-linux-computers-to-azure-monitor"></a>Linux bilgisayarlarını Azure Monitörüne bağlayın

Azure Monitor ile yerel veri merkezinizdeki veya diğer bulut ortamınızdaki sanal makineleri veya fiziksel bilgisayarları izlemek ve yönetmek için Log Analytics aracısını dağıtmanız ve günlük analizi çalışma alanına rapor edecek şekilde yapılandırmanız gerekir. Aracı, Azure Otomasyonu için Karma Runbook Worker rolünü de destekler.

Linux için Log Analytics aracısı aşağıdaki yöntemlerden biri kullanılarak yüklenebilir. Her yöntemin kullanımıyla ilgili ayrıntılar daha sonra makalede verilmiştir.

* Aracıyı [el ile indirin ve kurun.](#install-the-agent-manually) Bu, Linux bilgisayarının Internet erişimi yoksa ve [Log Analytics ağ geçidi](gateway.md)aracılığıyla Azure Monitor veya Azure Otomasyonu ile iletişim kuracağı zaman gereklidir. 
* GitHub'da barındırılan [bir sarmalayıcı komut dosyası kullanarak Linux aracısını yükleyin.](#install-the-agent-using-wrapper-script) Bu, bilgisayar Internet ile doğrudan veya bir proxy sunucusu üzerinden bağlantı olduğunda aracıyı yüklemek ve yükseltmek için önerilen yöntemdir.

Desteklenen yapılandırmayı anlamak için [desteklenen Linux işletim sistemlerini](log-analytics-agent.md#supported-linux-operating-systems) ve [ağ güvenlik duvarı yapılandırmasını](log-analytics-agent.md#firewall-requirements) inceleyin.

>[!NOTE]
>Linux için Log Analytics aracısı birden fazla Log Analytics çalışma alanına raporlamak için yapılandırılamaz. Yalnızca bir Sistem Merkezi Operasyon Yöneticisi yönetim grubuna ve Log Analytics çalışma alanına aynı anda veya tek tek rapor lanacak şekilde yapılandırılabilir.

## <a name="agent-install-package"></a>Aracı yükleme paketi

Linux için Log Analytics aracısı birden fazla paketten oluşur. Sürüm dosyası, parametre ile `--extract` kabuk paketini çalıştırarak kullanılabilen aşağıdaki paketleri içerir:

**Paket** | **Sürüm** | **Açıklama**
----------- | ----------- | --------------
omsagent | 1.12.15 | Linux için Log Analytics Agent
omsconfig | 1.1.1 | Log Analytics aracısı için yapılandırma aracısı
omi | 1.6.3 | Açık Yönetim Altyapısı (OMI) -- hafif bir CIM Server. *OMI'nin hizmetin çalışması için gerekli olan cron işini çalıştırmak için kök erişimi gerektirdiğini unutmayın*
Scx | 1.6.3 | İşletim sistemi performans ölçümleri için OMI CIM Sağlayıcıları
apaçi-cimprov | 1.0.1 | OMI için Apache HTTP Server performans izleme sağlayıcısı. Yalnızca Apache HTTP Server algılanırsa yüklenir.
mysql-cimprov | 1.0.1 | OMI için MySQL Server performans izleme sağlayıcısı. Yalnızca MySQL/MariaDB sunucusu algılanırsa yüklenir.
docker-cimprov | 1.0.0 | OMI için Docker sağlayıcısı. Yalnızca Docker algılanırsa yüklenir.

### <a name="agent-installation-details"></a>Aracı yükleme ayrıntıları

Linux paketleri için Log Analytics aracısını yükledikten sonra, sistem genelinde aşağıdaki ek yapılandırma değişiklikleri uygulanır. Omsagent paketi kaldırıldığında bu yapılar kaldırılır.

* Adında ayrıcalıklı olmayan bir `omsagent` kullanıcı oluşturuldu. Daemon bu kimlik belgesinin altında çalışır. 
* Sudoers *dosya içerir.* `/etc/sudoers.d/omsagent` Bu, `omsagent` sislog ve omsagent daemons yeniden başlatma yetkisi. Sudo *dahil* yönergeleri sudo yüklü sürümünde desteklenmiyorsa, bu girişler `/etc/sudoers`.
* Syslog yapılandırması, olayların bir alt kümesini aracıya iletmek için değiştirilir. Daha fazla bilgi için Bkz. [Syslog veri toplamayı yapılandırın.](data-sources-syslog.md)

İzlenen bir Linux bilgisayarında, aracı `omsagent`. `omsconfig`her 5 dakikada bir yeni portal yan yapılandırması arayan Linux yapılandırma aracısı için Log Analytics aracısıdır. Yeni ve güncelleştirilmiş yapılandırma, 'de `/etc/opt/microsoft/omsagent/conf/omsagent.conf`bulunan aracı yapılandırma dosyalarına uygulanır.

## <a name="obtain-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarını alma

Linux için Log Analytics aracısını yüklemeden önce, Log Analytics çalışma alanınızın kimliği ve anahtarına ihtiyacınız olacak. Bu bilgiler, aracının kurulumu sırasında düzgün bir şekilde yapılandırması ve Azure Monitor ile başarılı bir şekilde iletişim kurabilmesi için gereklidir.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]  

1. Azure portalının sol üst köşesinde **Tüm hizmetler'i**seçin. Arama kutusuna, **Günlük Analizi'ni**girin. Siz yazarken, girişinize göre liste filtreler. **Günlük Analizi çalışma alanlarını**seçin.

2. Günlük Analizi çalışma alanları listenizde, daha önce oluşturduğunuz çalışma alanını seçin. (Buna **DefaultLAWorkspace**adını vermiş olabilirsiniz.)

3. **Gelişmiş ayarları**seçin:

    ![Azure portalında Günlük Analitiği için Gelişmiş Ayarlar menüsü](../learn/media/quick-collect-azurevm/log-analytics-advanced-settings-azure-portal.png) 
 
4. **Bağlı Kaynaklar**’ı seçin ve ardından **Linux Sunucuları**’nı seçin.

5. **Çalışma Alanı Kimliği** ve **Birincil Anahtar**’ın sağındaki değer. Her ikisini de kopyalayıp sık kullandığınız bir düzenleyiciye yapıştırın.

## <a name="install-the-agent-manually"></a>Aracıyı el ile yükleme

Linux için Log Analytics aracısı, kendi kendine ayıklayan ve yüklenebilir bir kabuk komut dosyası paketinde sağlanır. Bu paket, aracı bileşenlerinin her biri için Debian ve RPM paketleri içerir ve doğrudan yüklenebilir veya tek tek paketleri almak için ayıklanabilir. X64 ve x86 mimariler için bir paket sağlanır. 

> [!NOTE]
> Azure VM'ler için, aracıyı Linux için [Azure Log Analytics VM uzantısını](../../virtual-machines/extensions/oms-linux.md) kullanarak üzerlerine yüklemenizi öneririz. 

1. Scp/sftp kullanarak uygun paketi (x64 veya x86) Linux VM'nize veya fiziksel bilgisayarınıza [indirin](https://github.com/microsoft/OMS-Agent-for-Linux#azure-install-guide) ve aktarın.

2. Bağımsız değişkeni kullanarak `--install` paketi yükleyin. Yükleme sırasında log Analytics çalışma alanına dahil `-w <WorkspaceID>` olmak `-s <workspaceKey>` için, daha önce kopyalanan parametreleri sağlayın.

    >[!NOTE]
    >Linux için sistem `--upgrade` Merkezi Operasyon Yöneticisi aracısı zaten yüklüyse, bu bağımsız değişkeni kullanmanız gerekir, omi, scx, omsconfig veya eski sürümleri gibi bağımlı paketler yüklenirse, bu bağımsız değişkeni kullanmanız gerekir. 

    ```
    sudo sh ./omsagent-*.universal.x64.sh --install -w <workspace id> -s <shared key>
    ```

3. Linux aracısını bir Log Analytics ağ geçidi aracılığıyla bir Log Analytics çalışma alanını yükleyip bağlayacak şekilde yapılandırmak için proxy, çalışma alanı kimliği ve çalışma alanı anahtar parametrelerini sağlayan aşağıdaki komutu çalıştırın. Bu yapılandırma komut satırında . `-p [protocol://][user:password@]proxyhost[:port]` *Proxyhost* özelliği, Log Analytics ağ geçidi sunucusunun tam nitelikli alan adı veya IP adresini kabul eder.  

    ```
    sudo sh ./omsagent-*.universal.x64.sh --upgrade -p https://<proxy address>:<proxy port> -w <workspace id> -s <shared key>
    ```

    Kimlik doğrulama gerekiyorsa, kullanıcı adını ve parolayı belirtmeniz gerekir. Örnek: 
    
    ```
    sudo sh ./omsagent-*.universal.x64.sh --upgrade -p https://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
    ```

4. Linux bilgisayarını Azure Devlet bulutundaki Bir Log Analytics çalışma alanına bağlanacak şekilde yapılandırmak için, çalışma alanı kimliğini ve birincil anahtarı daha önce kopyalayan aşağıdaki komutu çalıştırın.

    ```
    sudo sh ./omsagent-*.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
    ```

Aracı paketlerini yüklemek ve daha sonra belirli bir Log Analytics çalışma alanına rapor olacak şekilde yapılandırmak istiyorsanız, aşağıdaki komutu çalıştırın:

```
sudo sh ./omsagent-*.universal.x64.sh --upgrade
```

Aracıpaketpaketlerini aracıyı yüklemeden paketten çıkarmak istiyorsanız, aşağıdaki komutu çalıştırın:

```
sudo sh ./omsagent-*.universal.x64.sh --extract
```

## <a name="install-the-agent-using-wrapper-script"></a>Sarma layıcı komut dosyasını kullanarak aracıyı yükleme

Aşağıdaki adımlar, Doğrudan veya bir proxy sunucusu aracılığıyla iletişim kurabilen Linux bilgisayarları için sarmalayıcı komut dosyasını kullanarak Azure ve Azure Devlet bulutundaki Log Analytics aracısının kurulumlarını yapılandırarak, GitHub'da barındırılan aracıyı karşıdan yükleyip aracıyı yükler.  

Linux bilgisayarınızın bir proxy sunucusu aracılığıyla Log Analytics'e iletişim kurması gerekiyorsa, `-p [protocol://][user:password@]proxyhost[:port]`bu yapılandırma komut satırına . *Protokol* özelliği kabul `http` `https`eder veya *proxyhost* özelliği proxy sunucusunun tam nitelikli etki alanı adı veya IP adresini kabul eder. 

Örneğin, `https://proxy01.contoso.com:30443`

Her iki durumda da kimlik doğrulama gerekiyorsa, kullanıcı adını ve parolayı belirtmeniz gerekir. Örneğin, `https://user01:password@proxy01.contoso.com:30443`

1. Linux bilgisayarını Log Analytics çalışma alanına bağlanacak şekilde yapılandırmak için çalışma alanı kimliğini ve birincil anahtarı sağlayan aşağıdaki komutu çalıştırın. Bu komut aracıyı indirir, sağlama toplamını doğrular ve aracıyı yükler.
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

    Aşağıdaki komut, `-p` proxy sunucunuz tarafından kimlik doğrulaması gerektiğinde proxy parametresini ve örnek sözdizimini içerir:

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://]<proxy user>:<proxy password>@<proxyhost>[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Linux bilgisayarını Azure Genel Bulut'taki Log Analytics çalışma alanına bağlayacak şekilde yapılandırmak için, daha önce kopyalanan çalışma alanı kimliğini ve birincil anahtarı sağlayan aşağıdaki komutu çalıştırın. Bu komut aracıyı indirir, sağlama toplamını doğrular ve aracıyı yükler. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

    Aşağıdaki komut, `-p` proxy sunucunuz tarafından kimlik doğrulaması gerektiğinde proxy parametresini ve örnek sözdizimini içerir:

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://]<proxy user>:<proxy password>@<proxyhost>[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ```
2. Aşağıdaki komutu çalıştırarak aracıyı yeniden başlatın: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="upgrade-from-a-previous-release"></a>Önceki sürümden yükseltme

Sürüm 1.0.0-47 ile başlayan önceki sürümden yükseltme, her sürümde desteklenir. Aracının tüm `--upgrade` bileşenlerini en son sürüme yükseltmek için yüklemeyi parametreyle gerçekleştirin.

## <a name="next-steps"></a>Sonraki adımlar

- Aracıyı sanal makineden nasıl yeniden yapılandırılabildiğini, yükseltecek veya kaldıracağız hakkında bilgi edinmek [için Windows ve Linux için Log Analytics aracısını yönetme](agent-manage.md) ve bakımını gözden geçirin.

- Aracıyı yüklerken veya yönetirken sorunlarla karşılaşırsanız [Linux aracısını](agent-linux-troubleshoot.md) gözden geçirin.
