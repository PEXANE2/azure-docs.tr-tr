---
title: GeçersizAğYapılandırmaHatası Kodu hatası - Azure HDInsight
description: Azure HDInsight'ta Geçersiz AğYapılandırmaHatası Kodu ile başarısız küme oluşturmalarının çeşitli nedenleri
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/22/2020
ms.openlocfilehash: 6dd4db999cb130c9816ad023888a4333e968c224
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720393"
---
# <a name="cluster-creation-fails-with-invalidnetworkconfigurationerrorcode-in-azure-hdinsight"></a>Azure HDInsight'ta Geçersiz AğYapılandırmaHatası Kodu ile küme oluşturma başarısız olur

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

"Sanal Ağ `InvalidNetworkConfigurationErrorCode` yapılandırması HDInsight Gereksinimi ile uyumlu değildir" açıklamasıyla birlikte hata kodu görürseniz, genellikle kümenizin [sanal ağ yapılandırmasıyla](../hdinsight-plan-virtual-network-deployment.md) ilgili bir sorun olduğunu gösterir. Hata açıklamasının geri kalanına bağlı olarak, sorununuzu çözmek için aşağıdaki bölümleri izleyin.

## <a name="hostname-resolution-failed"></a>"HostName Çözümü başarısız oldu"

### <a name="issue"></a>Sorun

Hata açıklaması "Ana Bilgisayar Adı Çözümlemesi başarısız oldu" içerir.

### <a name="cause"></a>Nedeni

Bu hata, özel DNS yapılandırmasıyla ilgili bir soruna işaret eder. Sanal ağdaki DNS sunucuları, bu sanal ağdaki ana bilgisayar adlarını çözmek için DNS sorgularını Azure'un özyinelemeli çözümleyicilerine iletebilir (ayrıntılar için [Sanal Ağlarda Ad Çözümü'ne](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) bakın). Azure'un özyinelemeli çözümleyicilerine erişim sanal IP 168.63.129.16 üzerinden sağlanır. Bu IP'ye yalnızca Azure VM'lerinden erişilebilir. Bu nedenle, Bir OnPrem DNS sunucusu kullanıyorsanız veya DNS sunucunuz kümenin sanal ağının bir parçası olmayan bir Azure VM'siyse çalışmaz.

### <a name="resolution"></a>Çözüm

1. Kümenin bir parçası olan VM'ye ssh `hostname -f`ve komutu çalıştırın. Bu, ev sahibinin tam nitelikli alan adını `<host_fqdn>` döndürecektir (aşağıdaki talimatlarda belirtildiği gibi).

1. Ardından, komutu `nslookup <host_fqdn>` çalıştırın `nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net`(örneğin, ). Bu komut, adı bir IP adresine çözerse, DNS sunucunuzun doğru çalıştığı anlamına gelir. Bu durumda, HDInsight ile bir destek durumu yükseltin ve sorununuzu araştıralım. Destek durumunuzda, yürüttüğünüz sorun giderme adımlarını ekleyin. Bu, sorunu daha hızlı çözmemize yardımcı olacaktır.

1. Yukarıdaki komut bir IP adresi döndürmüyorsa, çalıştırın `nslookup <host_fqdn> 168.63.129.16` (örneğin, `nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net 168.63.129.16`). Bu komut IP'yi çözebiliyorsa, DNS sunucunuzun sorguyu Azure'un DNS'sine iletmediği veya kümeyle aynı sanal ağın parçası olan bir VM olmadığı anlamına gelir.

1. Kümenin sanal ağında özel bir DNS sunucusu gibi davranabilen bir Azure VM'iniz yoksa, önce bunu eklemeniz gerekir. Sanal ağda, DNS iletme olarak yapılandırılacak bir VM oluşturun.

1. Sanal ağınızda bir VM dağıtıldıktan sonra, bu VM'deki DNS iletme kurallarını yapılandırın. Tüm iDNS ad çözümleme isteklerini 168.63.129.16'ya, geri kalanını da DNS sunucunuza iletin. [Burada](../hdinsight-plan-virtual-network-deployment.md) özel bir DNS sunucusu için bu kurulumun bir örneğidir.

1. Sanal Ağ DNS yapılandırması için ilk DNS girişi olarak bu VM'nin IP Adresini ekleyin.

---

## <a name="failed-to-connect-to-azure-storage-account"></a>"Azure Depolama Hesabı'na bağlanılamamış"

### <a name="issue"></a>Sorun

Hata açıklaması "Azure Depolama Hesabı'na bağlanılamadığını" veya "Azure SQL'e bağlanılamadığını" içerir.

### <a name="cause"></a>Nedeni

Azure Depolama ve SQL'de sabit IP Adresleri yoktur, bu nedenle bu hizmetlere erişebilmeleri için tüm IP'lere giden bağlantılara izin vermemiz gerekir. Tam çözüm adımları, bir Ağ Güvenlik Grubu (NSG) veya Kullanıcı Tanımlı Kurallar (UDR) ayarlayıp ayarlamadığınıza bağlıdır. Bu yapılandırmalarla ilgili ayrıntılar için [ağ güvenlik grupları ve kullanıcı tanımlı rotalarla HDInsight ile ağ trafiğini denetleme](../hdinsight-plan-virtual-network-deployment.md#hdinsight-ip) bölümüne bakın.

### <a name="resolution"></a>Çözüm

* Kümeniz bir [Ağ Güvenlik Grubu (NSG)](../../virtual-network/virtual-network-vnet-plan-design-arm.md)kullanıyorsa.

    Azure portalına gidin ve kümenin dağıtıldığı alt ağla ilişkili NSG'yi tanımlayın. Giden **güvenlik kuralları** bölümünde, sınır sınırlama olmaksızın internete giden erişime izin verin (burada daha küçük bir **öncelik** sayısının daha yüksek öncelik anlamına geldiğini unutmayın). Ayrıca, **alt ağlar** bölümünde, bu NSG küme alt ağına uygulanıp uygulanmayan doğrulayın.

* Kümeniz Kullanıcı [tanımlı Bir Rota (UDR)](../../virtual-network/virtual-networks-udr-overview.md)kullanıyorsa.

    Azure portalına gidin ve kümenin dağıtıldığı alt ağla ilişkili rota tablosunu tanımlayın. Alt ağ için rota tablosunu bulduğunuzda, alt ağdaki **rotalar** bölümünü inceleyin.

    Tanımlı yollar varsa, kümenin dağıtıldığı bölge için IP adresleri için rotalar olduğundan ve her rota için **NextHopType** **Internet**olduğundan emin olun. Yukarıda belirtilen makalede belgelenen her gerekli IP Adresi için tanımlanmış bir rota olmalıdır.

---

## <a name="virtual-network-configuration-is-not-compatible-with-hdinsight-requirement"></a>"Sanal ağ yapılandırması HDInsight gereksinimi ile uyumlu değildir"

### <a name="issue"></a>Sorun

Hata açıklamaları aşağıdaki gibi benzer iletiler içerir:

```
ErrorCode: InvalidNetworkConfigurationErrorCode
ErrorDescription: Virtual Network configuration is not compatible with HDInsight Requirement. Error: 'Failed to connect to Azure Storage Account; Failed to connect to Azure SQL; HostName Resolution failed', Please follow https://go.microsoft.com/fwlink/?linkid=853974 to fix it.
```

### <a name="cause"></a>Nedeni

Büyük olasılıkla özel DNS kurulumu ile ilgili bir sorun.

### <a name="resolution"></a>Çözüm

168.63.129.16'nın özel DNS zincirinde olduğunu doğrulayın. Sanal ağdaki DNS sunucuları, bu sanal ağdaki ana bilgisayar adlarını çözmek için DNS sorgularını Azure'un özyinelemeli çözümleyicilerine iletebilir. Daha fazla bilgi için [Sanal Ağlarda Ad Çözümlemesi'ne](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)bakın. Azure'un özyinelemeli çözümleyicilerine erişim sanal IP 168.63.129.16 üzerinden sağlanır.

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Aşağıdaki komutu yürütün:

    ```bash
    cat /etc/resolv.conf | grep nameserver*
    ```

    Şunun gibi bir görüntüyle karşılaşacaksınız:

    ```output
    nameserver 168.63.129.16
    nameserver 10.21.34.43
    nameserver 10.21.34.44
    ```

    Sonuca göre - aşağıdaki adımlardan birini seçin:

#### <a name="1686312916-is-not-in-this-list"></a>168.63.129.16 bu listede yok

**Seçenek 1**  
[Azure HDInsight için sanal ağ planı'nda](../hdinsight-plan-virtual-network-deployment.md)açıklanan adımları kullanarak sanal ağ için ilk özel DNS olarak 168.63.129.16 ekleyin. Bu adımlar yalnızca özel DNS sunucunuz Linux'ta çalıştığında uygulanabilir.

**Seçenek 2**  
Sanal ağ için bir DNS sunucusu VM dağıtın. Bu, aşağıdaki adımları içerir:

* Sanal ağda DNS iletme (Linux veya windows VM olabilir) olarak yapılandırılacak bir VM oluşturun.
* Bu VM'deki DNS iletme kurallarını yapılandırın (tüm iDNS ad çözümleme isteklerini 168.63.129.16'ya, geri kalanını da DNS sunucunuza iletin).
* Sanal Ağ DNS yapılandırması için ilk DNS girişi olarak bu VM IP Adresini ekleyin.

#### <a name="1686312916-is-in-the-list"></a>168.63.129.16 listede

Bu durumda, lütfen HDInsight ile bir destek durumu oluşturun ve sorununuzu inceleyin. Destek kılıfınıza aşağıdaki komutların sonucunu ekleyin. Bu, sorunu daha hızlı araştırmamıza ve çözmemize yardımcı olacaktır.

Kafa düğümündeki bir ssh oturumundan, aşağıdakileri düzenledirin ve çalıştırın:

```bash
hostname -f
nslookup <headnode_fqdn> (e.g.nslookup hn1-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net)
dig @168.63.129.16 <headnode_fqdn> (e.g. dig @168.63.129.16 hn0-hditest.5h6lujo4xvoe1kprq3azvzmwsd.hx.internal.cloudapp.net)
```

---

### <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
