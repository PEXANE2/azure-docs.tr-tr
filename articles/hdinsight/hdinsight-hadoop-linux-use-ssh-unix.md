---
title: SSH’yi Hadoop - Azure HDInsight ile Kullanma
description: Secure Shell (SSH) kullanarak HDInsight'a erişebilirsiniz. Bu belgede; Windows, Linux, Unix veya macOS istemcilerinden ssh ve scp komutlarını kullanarak HDInsight’a bağlanmaya ilişkin bilgi sağlanmıştır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.date: 02/28/2020
ms.openlocfilehash: 31e85876d60ae6fcd8f3b29633506d698a323acb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272441"
---
# <a name="connect-to-hdinsight-apache-hadoop-using-ssh"></a>SSH kullanarak HDInsight’a (Apache Hadoop) bağlanma

Azure HDInsight'ta Apache Hadoop'a güvenli bir şekilde bağlanmak için [Secure Shell'i (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) nasıl kullanacağınızı öğrenin. Sanal ağ üzerinden bağlanma hakkında daha fazla bilgi için [Azure HDInsight sanal ağ mimarisine](./hdinsight-virtual-network-architecture.md) bakın ve [Azure HDInsight kümeleri için sanal ağ dağıtımı planlayın.](./hdinsight-plan-virtual-network-deployment.md)

Aşağıdaki tablo, bir SSH istemcisi kullanarak HDInsight'a bağlanırken gereken adres ve bağlantı noktası bilgilerini içerir:

| Adres | Bağlantı noktası | Bağlandığı yer... |
| ----- | ----- | ----- |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Birincil baş düğüm |
| `<clustername>-ssh.azurehdinsight.net` | 23 | İkincil baş düğüm |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | kenar düğümü (HDInsight'ta ML Hizmetleri) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | kenar düğümü (kenar düğümü varsa başka bir küme türü) |

`<clustername>` değerini kümenizin adıyla değiştirin. `<edgenodename>` ifadesini kenar düğümünün adıyla değiştirin.

Kümeniz bir kenar düğümü içeriyorsa __kenar düğümüne her zaman SSH’yi kullanarak bağlanmanızı__ öneririz. Baş düğümler, Hadoop’un sistem durumu için kritik öneme sahip olan hizmetleri barındırır. Kenar düğümü yalnızca üzerine yerleştirdiğiniz öğeleri çalıştırır. Kenar düğümlerini kullanma hakkında daha fazla bilgi için bkz. [HDInsight’ta kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md#access-an-edge-node).

> [!TIP]  
> HDInsight'a ilk kez bağlandığınızda SSH istemciniz konağın orijinalliğinin belirlenemediği yönünde bir uyarı görüntüleyebilir. Konağı SSH istemcinizin güvenilen sunucular listesine eklemek isteyip istemediğiniz sorulduğunda "Evet"i seçin.
>
> Daha önceden aynı ada sahip bir sunucuya bağlandıysanız konak anahtarının sunucu konak anahtarıyla eşleşmediğine dair bir uyarı alabilirsiniz. Var olan sunucu adı girişini kaldırma talimatları için SSH istemcinizin belgelerine bakın.

## <a name="ssh-clients"></a>SSH istemcileri

Linux, Unix ve macOS sistemleri `ssh` ve `scp` komutlarını sağlar. `ssh` istemcisi, yaygın olarak Linux veya Unix tabanlı bir sistemle uzak komut satırı oturumu oluşturmak için kullanılır. `scp` istemcisi, dosyaları istemciniz ve uzak sistem arasında güvenli bir şekilde kopyalamak için kullanılır.

Microsoft Windows varsayılan olarak herhangi bir SSH istemcisi yüklemez. `ssh` ve `scp` istemcileri, aşağıdaki paketler aracılığıyla Windows için kullanılabilir:

* [OpenSSH İstemci](https://docs.microsoft.com/windows-server/administration/openssh/openssh_install_firstuse). Bu istemci, Windows 10 Fall Creators Update'te tanıtılan isteğe bağlı bir özelliktir.

* [Bash Ubuntu üzerinde Windows 10 .](https://docs.microsoft.com/windows/wsl/about)

* [Azure Bulut Kabuğu](../cloud-shell/quickstart.md). Bulut Kabuğu tarayıcınızda bir Bash ortamı sağlar.

* [Git.](https://git-scm.com/)

[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/) ve [MobaXterm](https://mobaxterm.mobatek.net/)gibi çeşitli grafik Selavar istemcileri de vardır. Bu istemciler HDInsight’a bağlanmak için kullanılabilse de bağlanma işlemi `ssh` yardımcı programını kullanmaktan farklıdır. Daha fazla bilgi için, kullanmakta olduğunuz grafik istemcisinin belgelerine bakın.

## <a name="authentication-ssh-keys"></a><a id="sshkey"></a>Kimlik doğrulaması: SSH Anahtarları

SSH anahtarları, SSH oturumlarının kimliğini doğrulamak için [ortak anahtar şifrelemesini](https://en.wikipedia.org/wiki/Public-key_cryptography) kullanır. SSH anahtarları parolalara göre daha güvenlidir ve Hadoop kümenize erişimin güvenliğini sağlamak için kolay bir yol sağlar.

SSH hesabınızın güvenliği bir anahtar yardımıyla sağlanıyorsa, bağlantı kurduğunuzda istemci eşleşen özel anahtarı sağlamalıdır:

* Çoğu istemci, bir __varsayılan anahtar__ kullanmak üzere yapılandırılmıştır. Örneğin, `ssh` istemcisi Linux ve Unix ortamlarında `~/.ssh/id_rsa` üzerinde bir özel anahtar arar.

* __Özel anahtarın yolunu__ belirtebilirsiniz. `ssh` istemcisi ile özel anahtarın yolunu belirtmek için `-i` parametresi kullanılır. Örneğin, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Farklı sunucularla kullanılacak __birden fazla özel anahtarınız__ varsa [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) gibi bir yardımcı program kullanmayı deneyin. `ssh-agent` yardımcı programı, SSH oturumu oluşturulurken kullanılacak anahtarı otomatik olarak seçmek için kullanılabilir.

> [!IMPORTANT]  
> Özel anahtarınızın güvenliğini şifre ile sağlıyorsanız, anahtarı kullanmak için şifreyi girmeniz gerekir. `ssh-agent` gibi yardımcı programlar, size kolaylık sağlamak için parolayı önbelleğe alabilir.

### <a name="create-an-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Ortak ve özel anahtar dosyaları oluşturmak için `ssh-keygen` komutunu kullanın. Aşağıdaki komut, HDInsight ile kullanılabilecek bir 2048-bit RSA anahtar çifti oluşturur:

    ssh-keygen -t rsa -b 2048

Anahtar oluşturma işlemi sırasında bilgi istenmeniz istenir. Örneğin, anahtarların nerede depolanacağı veya şifre kullanılıp kullanılmayacağı. İşlem tamamlandıktan sonra biri ortak anahtar, diğeri özel anahtar olmak üzere iki dosya oluşturulur.

* __Ortak anahtar__ bir HDInsight kümesi oluşturmak için kullanılır. Ortak anahtar `.pub` uzantısına sahiptir.

* __Özel anahtar__, HDInsight kümesinde istemcinizin kimliğini doğrulamak için kullanılır.

> [!IMPORTANT]  
> Anahtarlarınızın güvenliğini şifre ile sağlayabilirsiniz. Parola, aslında özel anahtarınız üzerindeki bir şifredir. Özel anahtarınız başkası tarafından ele geçirilirse, anahtarın kullanılması için şifrenin girilmesi gerekir.

### <a name="create-hdinsight-using-the-public-key"></a>Ortak anahtar kullanarak HDInsight oluşturma

| Oluşturma yöntemi | Ortak anahtarı kullanma |
| ------- | ------- |
| Azure portalında | SSH __için küme giriş parolasını kullan__ve ardından SSH kimlik doğrulama türü olarak Ortak __Anahtar'ı__ seç seçeneğini işaretleyin. Son olarak, ortak anahtar dosyasını seçin veya dosyanın metin içeriğini __SSH ortak anahtarı__ alanına yapıştırın.</br>![HDInsight küme oluşturma işleminde SSH ortak anahtarı iletişim kutusu](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| Azure PowerShell | `-SshPublicKey` [Yeni-AzHdinsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) cmdlet parametresini kullanın ve ortak anahtarın içeriğini bir dize olarak geçirin.|
| Azure CLI | az `--sshPublicKey` [hdinsight create](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) komutunun parametresini kullanın ve ortak anahtarın içeriğini bir dize olarak geçirin. |
| Resource Manager Şablonu | SSH anahtarlarını şablonla kullanma örneği için bkz. [HDInsight’ı SSH anahtarı ile Linux’a dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/). [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) dosyasında `publicKeys` öğesi, kümeyi oluştururken Azure’a anahtarları geçirmek için kullanılır. |

## <a name="authentication-password"></a>Kimlik doğrulaması: Parola

SSH hesaplarının güvenliği bir parola kullanılarak sağlanabilir. SSH kullanarak HDInsight'a bağlandığınızda, parolayı girmeniz istenir.

> [!WARNING]  
> Microsoft, SSH için parola kimlik doğrulamasının kullanılmasını önermez. Parolalar tahmin edilebilir ve deneme yanılma saldırılarına karşı savunmasızdır. Bunun yerine, [kimlik doğrulaması için SSH anahtarları](#sshkey) kullanmanız önerilir.

> [!IMPORTANT]  
> SSH hesabı parolasının, HDInsight kümesi oluşturulduktan 70 gün sonra süresi dolar. Parolanızın süresi dolarsa, [HDInsight’ı yönetme](hdinsight-administer-use-portal-linux.md#change-passwords) belgesindeki bilgileri kullanarak değiştirebilirsiniz.

### <a name="create-hdinsight-using-a-password"></a>Parola kullanarak HDInsight oluşturma

| Oluşturma yöntemi | Parola belirtme |
| --------------- | ---------------- |
| Azure portalında | Varsayılan olarak, SSH kullanıcı hesabı ile küme oturum açma hesabı aynı parolaya sahiptir. Farklı bir parola kullanmak __için, SSH için küme giriş parolasını kullan'ı__ve __ardından SSH parola__ alanına parolayı girin' i işaretleyin.</br>![HDInsight küme oluşturma işleminde SSH parolası iletişim kutusu](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| Azure PowerShell | [Yeni-AzHdinsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) cmdlet `--SshCredential` parametresini kullanın ve `PSCredential` SSH kullanıcı hesabı adı ve parolasını içeren bir nesneyi geçirin. |
| Azure CLI | az `--sshPassword` [hdinsight create](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create) komutunun parametresini kullanın ve parola değerini sağlayın. |
| Resource Manager Şablonu | Parolayı şablonla kullanma örneği için bkz. [HDInsight’ı SSH parolası ile Linux’a dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/). [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) dosyasındaki `linuxOperatingSystemProfile` öğesi, kümeyi oluştururken SSH hesabı adı ile parolasını Azure’a geçirmek için kullanılır.|

### <a name="change-the-ssh-password"></a>SSH parolasını değiştirme

SSH kullanıcı hesabı parolasını değiştirme hakkında bilgi için, [HDInsight’ı Yönetme](hdinsight-administer-use-portal-linux.md#change-passwords) belgesinin __Parolaları değiştirme__ bölümüne bakın.

## <a name="authentication-domain-joined-hdinsight"></a>Kimlik doğrulama etki alanı HDInsight'a katıldı

Etki alanı yla __birleştirilmiş HDInsight kümesi__kullanıyorsanız, `kinit` SSH yerel kullanıcısıyla bağlandıktan sonra komutu kullanmanız gerekir. Bu komut sizden bir etki alanı kullanıcı adı ile parolası ister ve kümenizle ilişkili Azure Active Directory etki alanını kullanarak oturumunuzun kimliğini doğrular.

Ayrıca, etki alanı hesabını kullanarak ssh için birleştirilmiş düğüm (örneğin, baş düğümü, kenar düğümü) her etki alanında Kerberos Kimlik Doğrulaması etkinleştirebilirsiniz. Bunu yapmak için sshd config dosyasını düzenleyin:

```bash
sudo vi /etc/ssh/sshd_config
```

açıklamayı kaldırın ve `KerberosAuthentication` değerini `yes` olarak değiştirin

```bash
sudo service sshd restart
```

Kerberos kimlik doğrulamasının başarılı olup olmadığını doğrulamak için komutu kullanın. `klist`

Daha fazla bilgi için bkz. [Etki alanına katılmış HDInsight yapılandırma](./domain-joined/apache-domain-joined-configure.md).

## <a name="connect-to-nodes"></a>Düğümlere bağlanma

Kafa düğümleri ve kenar düğümleri (varsa) 22 ve 23 numaralı bağlantı noktalarından internet üzerinden erişilebilir.

* __Baş düğümlere__ bağlanırken, birincil baş düğüme bağlanmak için __22__, ikincil baş düğüme bağlanmak için __23__ numaralı bağlantı noktasını kullanın. Kullanılacak tam etki alanı adı `clustername-ssh.azurehdinsight.net`‘tir, burada `clustername` kümenizin adıdır.

    ```bash
    # Connect to primary head node
    # port not specified since 22 is the default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect to secondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```

* __Kenar düğümüne__ bağlanırken 22 numaralı bağlantı noktasını kullanın. Kullanılacak tam etki alanı adı `edgenodename.clustername-ssh.azurehdinsight.net`‘tir, burada `edgenodename` kenar düğümü oluştururken girdiğiniz addır. `clustername`, kümenin adıdır.

    ```bash
    # Connect to edge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]  
> Önceki örneklerde, parola ile kimlik doğrulaması kullandığınız veya sertifika kimlik doğrulamasının otomatik olarak yapıldığı varsayılır. Kimlik doğrulaması için bir SSH anahtar çifti kullanıyorsanız ve sertifika otomatik olarak kullanılmıyorsa, özel anahtarı belirtmek için `-i` parametresini kullanın. Örneğin, `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.

Bağlandıktan sonra, ssh kullanıcı adını ve bağlı olduğunuz düğümü belirtmek için istem değişir. Örneğin, `sshuser` olarak birincil baş düğüme bağlıyken komut istemi `sshuser@<active-headnode-name>:~$` değerini gösterir.

### <a name="connect-to-worker-and-apache-zookeeper-nodes"></a>İşçi ve Apache Zookeeper düğümlerine bağlanın

İşçi düğümleri ve Zookeeper düğümleri doğrudan internetten erişilebilir değildir. Bunlara, küme baş düğümleri veya kenar düğümlerinden erişilebilir. Diğer düğümlere bağlanmak için uygulamanız gereken genel adımlar şunlardır:

1. SSH kullanarak bir baş veya kenar düğümüne bağlanın:

    ```bash
    ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net
    ```

2. Baş veya kenar düğümüne yaptığınız SSH bağlantısında `ssh` komutunu kullanarak kümedeki bir çalışan düğümüne bağlanın:

    ```bash
    ssh sshuser@wn0-myhdi
    ```

    Düğüm adlarının listesini almak için [Apache Ambari REST API belgesini kullanarak HDInsight'ı Yönet'e](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) bakın.

SSH hesabının güvenliği bir __parola__ kullanılarak sağlanıyorsa bağlanırken parolayı girin.

SSH hesabının güvenliği __SSH anahtarları__ kullanılarak sağlanıyorsa istemciden SSH iletmenin etkinleştirildiğinden emin olun.

> [!NOTE]  
> Kümedeki tüm düğümlere doğrudan erişmenin başka bir yolu ise HDInsight’ın bir Azure Sanal Ağına bağlanmasıdır. Bundan sonra, uzak makinenizi aynı sanal ağa bağlayabilir ve kümedeki tüm düğümlere doğrudan erişebilirsiniz.
>
> Daha fazla bilgi için [HDInsight için sanal ağ planı'na](hdinsight-plan-virtual-network-deployment.md)bakın.

### <a name="configure-ssh-agent-forwarding"></a>SSH aracı iletmeyi yapılandırma

> [!IMPORTANT]  
> Aşağıdaki adımlar, Linux veya UNIX tabanlı bir sistem için geçerlidir ve Windows 10 üzerinde Bash ile birlikte çalışır. Bu adımlar sisteminizde çalışmazsa SSH istemcinizin belgelerine bakmanız gerekebilir.

1. Bir metin düzenleyicisiyle `~/.ssh/config` dosyasını açın. Bu dosya yoksa, komut satırında `touch ~/.ssh/config` girerek oluşturabilirsiniz.

2. Aşağıdakileri metni `config` dosyasına ekleyin.

    ```
    Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
        ForwardAgent yes
    ```

    __Ana bilgisayar__ bilgisini, SSH kullanarak bağlandığınız düğümün adresiyle değiştirin. Önceki örnekte kenar düğümü kullanılmıştır. Bu giriş, belirtilen düğüm için SSH aracı iletmeyi yapılandırır.

3. Terminalde aşağıdaki komutu kullanarak, SSH aracı iletmeyi test edin:

    ```bash
    echo "$SSH_AUTH_SOCK"
    ```

    Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

    ```output
    /tmp/ssh-rfSUL1ldCldQ/agent.1792
    ```

    Hiçbir şey döndürülmezse, `ssh-agent` çalışmaz. Daha fazla bilgi için [ssh ile ssh-agent kullanma (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) sayfasındaki aracı başlatma betikleri bilgilerine bakın veya SSH istemcinizin belgelerine başvurun.

4. **Ssh-agent'in** çalıştığını doğruladıktan sonra, SSH özel anahtarınızı aracıya eklemek için aşağıdakileri kullanın:

    ```bash
    ssh-add ~/.ssh/id_rsa
    ```

    Özel anahtarınızı farklı bir dosyada saklanıyorsa, `~/.ssh/id_rsa` ile dosyanın yolunu değiştirin.

5. SSH kullanarak küme kenar düğümüne veya baş düğümlerine bağlanın. Ardından, SSH komutunu kullanarak bir çalışan veya zookeeper düğümüne bağlanın. İletilen anahtar kullanılarak bağlantı kurulur.

## <a name="copy-files"></a>Dosyaları kopyalama

`scp` yardımcı programı, kümedeki bireysel düğümlerde gelen ve giden dosyaları kopyalamak için kullanılabilir. Örneğin, aşağıdaki komut `test.txt` dizinini yerel sistemden birincil baş düğüme kopyalar.

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

`:` sonrasında yol sonra belirtilmezse dosya `sshuser` giriş dizinine yerleştirilir.

Aşağıdaki örnekte birincil baş düğümdeki `sshuser` giriş dizininden `test.txt` dosyası yerel sisteme kopyalanmaktadır:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]  
> `scp`, yalnızca küme içindeki tek düğümlerin dosya sistemine erişebilir. Küme için HDFS uyumlu depolama biriminde bulunan verilere erişmek için kullanılamaz.
>
> Bir kaynağı SSH oturumundan kullanmak için karşıya yüklemeniz gerektiğinde `scp` kullanın. Örneğin, bir Python betiğini karşıya yükleyin ve bir SSH oturumundan çalıştırın.
>
> Verileri HDFS uyumlu depolama alanına doğrudan yükleme hakkında daha fazla bilgi için aşağıdaki belgelere bakın:
>
> * [Azure Depolama kullanarak HDInsight](hdinsight-hadoop-use-blob-storage.md)
>
> * [Azure Veri Gölü Depolama'yı kullanarak HDInsight.](hdinsight-hadoop-use-data-lake-store.md)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight ile SSH tüneli kullanma](hdinsight-linux-ambari-ssh-tunnel.md)
* [HDInsight ile sanal ağ planlama](hdinsight-plan-virtual-network-deployment.md)
* [HDInsight’ta kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md#access-an-edge-node)
