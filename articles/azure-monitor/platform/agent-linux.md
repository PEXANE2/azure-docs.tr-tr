---
title: Linux bilgisayarlarını Azure Izleyici 'ye bağlama | Microsoft Docs
description: Bu makalede, diğer bulutlarda veya şirket içinde barındırılan Linux bilgisayarların Linux için Log Analytics aracısıyla Azure Izleyici 'ye nasıl bağlanacağı açıklanır.
ms.subservice: logs
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 01/21/2020
ms.openlocfilehash: accd7c3ad82853c1f2af0b632326b2798f85b36b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87073655"
---
# <a name="connect-linux-computers-to-azure-monitor"></a>Linux bilgisayarlarını Azure Izleyici 'ye bağlama

Yerel veri merkezinizdeki veya Azure Izleyici ile diğer bulut ortamlarınızdaki sanal makineleri veya fiziksel bilgisayarları izlemek ve yönetmek için Log Analytics aracısını dağıtmanız ve bir Log Analytics çalışma alanına rapor verecek şekilde yapılandırmanız gerekir. Aracı Ayrıca Azure Otomasyonu için karma Runbook Worker rolünü destekler.

Linux için Log Analytics Aracısı aşağıdaki yöntemlerden biri kullanılarak yüklenebilir. Her yöntemin kullanımıyla ilgili ayrıntılar, makalenin ilerleyen kısımlarında verilmiştir.

* Aracıyı [el ile indirip yükleyin](#install-the-agent-manually) . Bu, Linux bilgisayarın Internet erişimi olmadığında ve [Log Analytics ağ geçidi](gateway.md)aracılığıyla Azure Izleyici veya Azure Otomasyonu ile iletişim kurdukları durumlarda gereklidir. 
* GitHub 'da barındırılan [bir sarmalayıcı betiği kullanarak Linux için aracıyı yükler](#install-the-agent-using-wrapper-script) . Bu, bilgisayarın Internet bağlantısı olduğunda (doğrudan veya bir proxy sunucusu aracılığıyla) aracıyı yüklemek ve yükseltmek için önerilen yöntemdir.

Desteklenen yapılandırmayı anlamak için [desteklenen Linux işletim sistemlerini](log-analytics-agent.md#supported-linux-operating-systems) ve [ağ güvenlik duvarı yapılandırmasını](log-analytics-agent.md#network-requirements) inceleyin.

>[!NOTE]
>Linux için Log Analytics aracısı birden fazla Log Analytics çalışma alanına raporlamak için yapılandırılamaz. Yalnızca bir System Center Operations Manager yönetim grubuna ve Log Analytics çalışma alanına ya da tek başına rapor verecek şekilde yapılandırılabilir.

## <a name="agent-install-package"></a>Aracı yüklemesi paketi

Linux için Log Analytics Aracısı birden çok paketten oluşur. Yayın dosyası, Shell paketi şu parametreyle çalıştırılarak kullanılabilir olan aşağıdaki paketleri içerir `--extract` :

**Paket** | **Sürüm** | **Açıklama**
----------- | ----------- | --------------
omsagent | 1.12.15 | Linux için Log Analytics Aracısı
omsconfig | 1.1.1 | Log Analytics Aracısı için yapılandırma Aracısı
OMI | 1.6.3 | Açık yönetim altyapısı (OMı)-basit bir CıM sunucusu. *OMı 'nın hizmetin çalışması için gerekli bir cron işini çalıştırmak için kök erişimi gerektirdiğini unutmayın.*
SCX | 1.6.3 | İşletim sistemi performans ölçümleri için OMı CıM sağlayıcıları
Apache-cimprov | 1.0.1 | OMı için Apache HTTP sunucu performansı izleme sağlayıcısı. Yalnızca Apache HTTP sunucusu algılanırsa yüklenir.
MySQL-cimprov | 1.0.1 | OMı için MySQL Server performans izleme sağlayıcısı. Yalnızca MySQL/MariaDB sunucusu algılanırsa yüklenir.
Docker-cimprov | 1.0.0 | OMı için Docker sağlayıcısı. Yalnızca Docker algılanırsa yüklenir.

### <a name="agent-installation-details"></a>Aracı Yükleme ayrıntıları

Linux paketleri için Log Analytics aracısını yükledikten sonra, aşağıdaki ek sistem genelinde yapılandırma değişiklikleri uygulanır. Omsagent paketi kaldırıldığında bu yapıtlar kaldırılır.

* Ayrıcalıklı olmayan bir Kullanıcı adlı: `omsagent` oluşturulur. Daemon bu kimlik bilgisi altında çalışır. 
* İçinde bir sudoers *içerme* dosyası oluşturulur `/etc/sudoers.d/omsagent` . Bu `omsagent` , syslog ve omsagent Daemon 'ları 'ı yeniden başlatmayı yetkilendirir. Sudo *ekleme* yönergeleri, sudo 'ın yüklü sürümünde desteklenmiyorsa, bu girişlerin üzerine yazılır `/etc/sudoers` .
* Syslog yapılandırması bir olay alt kümesini aracıya iletecek şekilde değiştirilir. Daha fazla bilgi için bkz. [Syslog veri toplamayı yapılandırma](data-sources-syslog.md).

İzlenen bir Linux bilgisayarında aracı olarak listelenir `omsagent` . `omsconfig`, Linux yapılandırma aracısına yönelik Log Analytics aracısıdır ve her 5 dakikada bir yeni Portal tarafı yapılandırmasını arar. Yeni ve güncelleştirilmiş yapılandırma konumunda bulunan aracı yapılandırma dosyalarına uygulanır `/etc/opt/microsoft/omsagent/conf/omsagent.conf` .

## <a name="obtain-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarını alma

Linux için Log Analytics aracısını yüklemeden önce, Log Analytics çalışma alanınızın kimliği ve anahtarına ihtiyacınız olacak. Bu bilgi, aracının düzgün şekilde yapılandırılması ve Azure Izleyici ile başarılı bir şekilde iletişim kurabildiğinden emin olmak için aracının kurulumu sırasında gereklidir.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]  

1. Azure portal sol üst köşesinde **tüm hizmetler**' i seçin. Arama kutusuna **Log Analytics**girin. Siz yazarken, liste, girişinizi temel alarak filtreler. **Log Analytics çalışma alanlarını**seçin.

2. Log Analytics çalışma alanları listenizde, daha önce oluşturduğunuz çalışma alanını seçin. ( **Defaultlaworkspace**adında bir adlandırılmış olabilir.)

3. **Gelişmiş ayarları**seçin:

    ![Azure portal Log Analytics için Gelişmiş ayarlar menüsü](../learn/media/quick-collect-azurevm/log-analytics-advanced-settings-azure-portal.png) 
 
4. **Bağlı Kaynaklar**’ı seçin ve ardından **Linux Sunucuları**’nı seçin.

5. **Çalışma Alanı Kimliği** ve **Birincil Anahtar**’ın sağındaki değer. Her ikisini de kopyalayıp sık kullandığınız bir düzenleyiciye yapıştırın.

## <a name="install-the-agent-manually"></a>Aracıyı el ile yükleme

Linux için Log Analytics Aracısı kendiliğinden ayıklanan ve yüklenebilir bir kabuk betik paketinde sunulmaktadır. Bu paket, aracı bileşenlerinden her biri için Demcı ve RPM paketleri içerir ve tek tek paketleri almak için doğrudan veya ayıklanamaz. X64 için bir paket ve x86 mimarileri için bir paket sağlanır. 

> [!NOTE]
> Azure VM 'lerinde, Linux için [azure log ANALYTICS VM uzantısını](../../virtual-machines/extensions/oms-linux.md) kullanarak aracıyı bunlara yüklemenizi öneririz. 

1. SCP/SFTP kullanarak uygun paketi (x64 veya x86) Linux sanal makinenize veya fiziksel bilgisayarınıza [indirin](https://github.com/microsoft/OMS-Agent-for-Linux#azure-install-guide) ve aktarın.

2. Bağımsız değişkenini kullanarak paketi yükler `--install` . Yükleme sırasında bir Log Analytics çalışma alanına eklemek için, `-w <WorkspaceID>` `-s <workspaceKey>` daha önce kopyalanmış olan ve parametreleri sağlayın.

    >[!NOTE]
    >`--upgrade`Linux için System Center Operations Manager Aracısı zaten yüklüyse, OMI, SCX, omsmsconfig veya daha eski sürümleri gibi bağımlı paketler yüklüyse bağımsız değişkenini kullanmanız gerekir. 

    ```
    sudo sh ./omsagent-*.universal.x64.sh --install -w <workspace id> -s <shared key>
    ```

3. Linux aracısını, bir Log Analytics ağ geçidi aracılığıyla bir Log Analytics çalışma alanına yüklemek ve bağlanmak üzere yapılandırmak için, proxy, çalışma alanı KIMLIĞI ve çalışma alanı anahtarı parametrelerini sağlayan aşağıdaki komutu çalıştırın. Bu yapılandırma, öğesini ekleyerek komut satırında belirtilebilir `-p [protocol://][user:password@]proxyhost[:port]` . *Proxyhost* özelliği, Log Analytics ağ geçidi sunucusunun tam etki alanı adını veya IP adresini kabul eder.  

    ```
    sudo sh ./omsagent-*.universal.x64.sh --upgrade -p https://<proxy address>:<proxy port> -w <workspace id> -s <shared key>
    ```

    Kimlik doğrulaması gerekliyse, Kullanıcı adını ve parolayı belirtmeniz gerekir. Örneğin: 
    
    ```
    sudo sh ./omsagent-*.universal.x64.sh --upgrade -p https://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
    ```

4. Linux bilgisayarı Azure Kamu bulutundaki bir Log Analytics çalışma alanına bağlanacak şekilde yapılandırmak için, daha önce kopyalanmış çalışma alanı KIMLIĞI ve birincil anahtarı sağlamak üzere aşağıdaki komutu çalıştırın.

    ```
    sudo sh ./omsagent-*.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
    ```

Aracı paketlerini yüklemek ve daha sonra belirli bir Log Analytics çalışma alanına raporlamak üzere yapılandırmak istiyorsanız aşağıdaki komutu çalıştırın:

```
sudo sh ./omsagent-*.universal.x64.sh --upgrade
```

Aracıyı yüklemeden Aracı paketlerini paketten ayıklamak istiyorsanız aşağıdaki komutu çalıştırın:

```
sudo sh ./omsagent-*.universal.x64.sh --extract
```

## <a name="install-the-agent-using-wrapper-script"></a>Sarmalayıcı betiği kullanarak aracıyı yükler

Aşağıdaki adımlar, GitHub 'da barındırılan aracıyı indirmek ve aracıyı yüklemek için doğrudan veya bir proxy sunucusu üzerinden iletişim kurabilen Linux bilgisayarları için sarmalayıcı betiği kullanarak Azure ve Azure Kamu Bulutu 'nda Log Analytics için aracının kurulumunu yapılandırır.  

Linux bilgisayarınızın Log Analytics için bir proxy sunucusu üzerinden iletişim kurması gerekiyorsa, bu yapılandırma komut satırında dahil ederek belirlenebilir `-p [protocol://][user:password@]proxyhost[:port]` . *Protocol* özelliği `http` , veya kabul eder `https` ve *proxyhost* özelliği proxy sunucusunun tam etki alanı adını veya IP adresini kabul eder. 

Örnek: `https://proxy01.contoso.com:30443`

Her iki durumda da kimlik doğrulaması gerekliyse, Kullanıcı adını ve parolayı belirtmeniz gerekir. Örnek: `https://user01:password@proxy01.contoso.com:30443`

1. Linux bilgisayarı bir Log Analytics çalışma alanına bağlanacak şekilde yapılandırmak için, çalışma alanı KIMLIĞI ve birincil anahtar sağlamak üzere aşağıdaki komutu çalıştırın. Bu komut aracıyı indirir, sağlama toplamını doğrular ve aracıyı yükler.
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

    Aşağıdaki komut proxy `-p` parametresini ve kimlik doğrulaması proxy sunucunuz için gerekliyse örnek sözdizimini içerir:

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://]<proxy user>:<proxy password>@<proxyhost>[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Linux bilgisayarı Azure Kamu Bulutu 'nda Log Analytics çalışma alanına bağlanacak şekilde yapılandırmak için, daha önce kopyalanmış çalışma alanı KIMLIĞI ve birincil anahtar sağlamak üzere aşağıdaki komutu çalıştırın. Bu komut aracıyı indirir, sağlama toplamını doğrular ve aracıyı yükler. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

    Aşağıdaki komut proxy `-p` parametresini ve kimlik doğrulaması proxy sunucunuz için gerekliyse örnek sözdizimini içerir:

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://]<proxy user>:<proxy password>@<proxyhost>[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ```
2. Aşağıdaki komutu çalıştırarak aracıyı yeniden başlatın: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="supported-linux-hardening"></a>Desteklenen Linux sağlamlaştırma
OMS aracısının Linux için özelleştirme desteği sınırlıdır. 

Aşağıdakiler şu anda desteklenmektedir: 
- FIPS

Aşağıdakiler planlanmaktadır ancak henüz desteklenmemektedir:
- CıS-SELINUX

Diğer sağlamlaştırma ve özelleştirme yöntemleri, OMS Aracısı için desteklenmez veya planlanmaz.  


## <a name="upgrade-from-a-previous-release"></a>Önceki sürümden yükseltme

Önceki sürümden yükseltme, 1.0.0-47 sürümünden başlayarak her sürümde desteklenir. `--upgrade`Aracının tüm bileşenlerini en son sürüme yükseltmek için parametresiyle birlikte yüklemeyi gerçekleştirin.

## <a name="next-steps"></a>Sonraki adımlar

- Aracıyı sanal makineden yeniden yapılandırma, yükseltme veya kaldırma hakkında bilgi edinmek için [Windows ve Linux için Log Analytics aracısının yönetimini ve güvenliğini](agent-manage.md) gözden geçirin.

- Aracıyı yüklerken veya yönetirken sorunlarla karşılaşırsanız [, Linux aracısının sorunlarını giderme](agent-linux-troubleshoot.md) konusunu gözden geçirin.
