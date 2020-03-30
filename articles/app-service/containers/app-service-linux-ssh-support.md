---
title: Linux kapları için SSH erişimi
description: Azure Uygulama Hizmeti'nde bir Linux kapsayıcısına Bir SSH oturumu açabilirsiniz. Özel Linux kapları, özel resminizde yapılan bazı değişikliklerle desteklenir.
keywords: azure uygulama hizmeti, web uygulaması, linux, oss
author: msangapu-msft
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.topic: article
ms.date: 02/25/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: dab13f222b441c7415a8d09d0d91ab3af5aaf836
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280189"
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service için SSH desteği

[Secure Shell (SSH),](https://wikipedia.org/wiki/Secure_Shell) komut satırı terminalinden uzakta yönetim komutlarını yürütmek için yaygın olarak kullanılır. Linux'taki Uygulama Hizmeti, uygulama konteynerine SSH desteği sağlar. 

![Linux Uygulama Hizmeti SSH](./media/app-service-linux-ssh-support/app-service-linux-ssh.png)

Ayrıca, SSH ve SFTP kullanarak konteynere doğrudan yerel geliştirme makinenizden bağlanabilirsiniz.

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturumunu açma

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-no-h.md)]

## <a name="use-ssh-support-with-custom-docker-images"></a>Özel Docker görüntüleriyle SSH desteğini kullanma

Bkz. [Özel bir kapsayıcıda SSH yapılandırın.](configure-custom-container.md#enable-ssh)

## <a name="open-ssh-session-from-remote-shell"></a>Uzak kabuktan SSH oturumunu açma

> [!NOTE]
> Bu özellik şu anda Önizleme'dedir.
>

TCP tüneloluşturmayı kullanarak, kimlik doğrulaması yapılan bir WebSocket bağlantısı üzerinden geliştirme makineniz ile Kapsayıcılar için Web Uygulaması arasında bir ağ bağlantısı oluşturabilirsiniz. Seçtiğiniz istemciden Uygulama Hizmeti'nde çalışan kabınızla bir SSH oturumu açmanızı sağlar.

Başlamak için [Azure CLI'yi](/cli/azure/install-azure-cli?view=azure-cli-latest)yüklemeniz gerekir. Azure CLI'yi yüklemeden nasıl çalıştığını görmek için [Azure Bulut BulutU'nu](../../cloud-shell/overview.md)açın. 

[az webapp uzaktan bağlantı oluşturma](/cli/azure/ext/webapp/webapp/remote-connection?view=azure-cli-latest#ext-webapp-az-webapp-remote-connection-create) komutunu kullanarak uygulamanız için uzaktan bağlantı açın. Uygulamanız için _ \<abonelik kimliği>, _ _ \<grup adı>_ ve \_ \<uygulama adı>_ belirtin.

```azurecli-interactive
az webapp create-remote-connection --subscription <subscription-id> --resource-group <resource-group-name> -n <app-name> &
```

> [!TIP]
> `&`komutun sonunda, Cloud Shell kullanıyorsanız kolaylık sağlamak içindir. Bir sonraki komutu aynı kabukta çalıştırabilmeniz için işlemi arka planda çalıştırUr.

Komut çıktısı, bir SSH oturumu açmak için gereken bilgileri verir.

```output
Port 21382 is open
SSH is available { username: root, password: Docker! }
Start your favorite client and connect to port 21382
```

Yerel bağlantı noktasını kullanarak, seçtiğiniz istemciyle kabınızla bir SSH oturumu açın. Aşağıdaki örnekte varsayılan [ssh](https://ss64.com/bash/ssh.html) komutu kullanır:

```bash
ssh root@127.0.0.1 -p <port>
```

İstendiğinde, bağlanmaya devam etmek için yazın. `yes` Daha sonra parola için istenir. Daha `Docker!`önce size gösterilen kullanımı.

```output
Warning: Permanently added '[127.0.0.1]:21382' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
```

Kimliğinidoğruladıktan sonra, oturum karşılama ekranını görmeniz gerekir.

```output
  _____
  /  _  \ __________ _________   ____
 /  /_\  \___   /  |  \_  __ \_/ __ \
/    |    \/    /|  |  /|  | \/\  ___/
\____|__  /_____ \____/ |__|    \___  >
        \/      \/                  \/
A P P   S E R V I C E   O N   L I N U X

0e690efa93e2:~#
```

Artık konektörünüze bağlısınız.  

[En üst](https://ss64.com/bash/top.html) komutu çalıştırmayı deneyin. Uygulamanızın işlemini işlem listesinde görebilmelisiniz. Aşağıdaki örnek çıktıda, `PID 263`'.

```output
Mem: 1578756K used, 127032K free, 8744K shrd, 201592K buff, 341348K cached
CPU:   3% usr   3% sys   0% nic  92% idle   0% io   0% irq   0% sirq
Load average: 0.07 0.04 0.08 4/765 45738
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     S     1528   0%   0   0% /sbin/init
  235     1 root     S     632m  38%   0   0% PM2 v2.10.3: God Daemon (/root/.pm2)
  263   235 root     S     630m  38%   0   0% node /home/site/wwwroot/app.js
  482   291 root     S     7368   0%   0   0% sshd: root@pts/0
45513   291 root     S     7356   0%   0   0% sshd: root@pts/1
  291     1 root     S     7324   0%   0   0% /usr/sbin/sshd
  490   482 root     S     1540   0%   0   0% -ash
45539 45513 root     S     1540   0%   0   0% -ash
45678 45539 root     R     1536   0%   0   0% top
45733     1 root     Z        0   0%   0   0% [init]
45734     1 root     Z        0   0%   0   0% [init]
45735     1 root     Z        0   0%   0   0% [init]
45736     1 root     Z        0   0%   0   0% [init]
45737     1 root     Z        0   0%   0   0% [init]
45738     1 root     Z        0   0%   0   0% [init]
```

## <a name="next-steps"></a>Sonraki adımlar

Azure [forumunda](https://docs.microsoft.com/answers/topics/azure-webapps.html)soru ve endişenizi yayınlayabilirsiniz.

Kapsayıcılar için Web Uygulaması hakkında daha fazla bilgi için bkz:

* [Vs Kodu'ndan Azure Uygulama Hizmetinde Node.js uygulamalarının uzaktan hata ayıklama ile tanıtımı](https://medium.com/@auchenberg/introducing-remote-debugging-of-node-js-apps-on-azure-app-service-from-vs-code-in-public-preview-9b8d83a6e1f0)
* [Kapsayıcılar için Web Uygulaması için özel bir Docker görüntüsü nasıl kullanılır?](quickstart-docker-go.md)
* [Linux üzerinde Azure App Service’te .NET Core Kullanma](quickstart-dotnetcore.md)
* [Linux üzerinde Azure App Service’te Ruby Kullanma](quickstart-ruby.md)
* [Kapsayıcılar için Azure App Service Web App SSS](app-service-linux-faq.md)
