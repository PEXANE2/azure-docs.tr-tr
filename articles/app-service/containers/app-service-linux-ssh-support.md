---
title: Linux kapsayıcıları için SSH erişimi
description: Azure App Service bir Linux kapsayıcısına SSH oturumu açabilirsiniz. Özel Linux kapsayıcıları, özel görüntinizdeki bazı değişikliklerle desteklenir.
keywords: Azure App Service, Web uygulaması, Linux, OSS
author: msangapu-msft
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.topic: article
ms.date: 02/25/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: dab13f222b441c7415a8d09d0d91ab3af5aaf836
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84695836"
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service için SSH desteği

[Secure Shell (SSH)](https://wikipedia.org/wiki/Secure_Shell) genellikle bir komut satırı terminalden uzaktan yönetim komutları yürütmek için kullanılır. Linux üzerinde App Service, uygulama kapsayıcısına SSH desteği sağlar. 

![Linux App Service SSH](./media/app-service-linux-ssh-support/app-service-linux-ssh.png)

Ayrıca, SSH ve SFTP kullanarak doğrudan yerel geliştirme makinenizden kapsayıcıya da bağlanabilirsiniz.

## <a name="open-ssh-session-in-browser"></a>SSH oturumunu tarayıcıda aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-no-h.md)]

## <a name="use-ssh-support-with-custom-docker-images"></a>Özel Docker görüntüleriyle SSH desteğini kullanma

Bkz. [özel KAPSAYıCıDA SSH yapılandırma](configure-custom-container.md#enable-ssh).

## <a name="open-ssh-session-from-remote-shell"></a>Uzak kabuktan SSH oturumu aç

> [!NOTE]
> Bu özellik şu anda önizleme aşamasındadır.
>

TCP tünelini kullanarak, geliştirme makineniz ile kimliği doğrulanmış bir WebSocket bağlantısı üzerinden Kapsayıcılar için Web App bir ağ bağlantısı oluşturabilirsiniz. Seçtiğiniz istemciden App Service çalıştıran kapsayıcınıza bir SSH oturumu açmanıza olanak sağlar.

Başlamak için [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)'yi yüklemeniz gerekir. Azure CLı yüklemeden nasıl çalıştığını görmek için [Azure Cloud Shell](../../cloud-shell/overview.md)açın. 

[Az WebApp Remote-Connection Create](/cli/azure/ext/webapp/webapp/remote-connection?view=azure-cli-latest#ext-webapp-az-webapp-remote-connection-create) komutunu kullanarak uygulamanıza uzak bir bağlantı açın. _\<subscription-id>_ _\<group-name>_ \_ \<app-name> Uygulamanız için ve _ öğesini belirtin.

```azurecli-interactive
az webapp create-remote-connection --subscription <subscription-id> --resource-group <resource-group-name> -n <app-name> &
```

> [!TIP]
> `&`komutun sonunda Cloud Shell kullanıyorsanız kolaylık sağlamak yeterlidir. Bir sonraki komutu aynı kabukta çalıştırabilmeniz için işlemi arka planda çalıştırır.

Komut çıktısı size bir SSH oturumu açmak için gereken bilgileri sağlar.

```output
Port 21382 is open
SSH is available { username: root, password: Docker! }
Start your favorite client and connect to port 21382
```

Yerel bağlantı noktasını kullanarak, istediğiniz istemcisiyle kapsayıcınıza sahip bir SSH oturumu açın. Aşağıdaki örnek varsayılan [SSH](https://ss64.com/bash/ssh.html) komutunu kullanır:

```bash
ssh root@127.0.0.1 -p <port>
```

İstendiğinde, `yes` bağlanmaya devam etmek için yazın. Bundan sonra parola istenir. `Docker!`Daha önce gösterilen ' i kullanın.

```output
Warning: Permanently added '[127.0.0.1]:21382' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
```

Kimliğiniz doğrulandıktan sonra oturum hoş geldiniz ekranını görmeniz gerekir.

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

Artık bağlayıcınıza bağlısınız.  

[En üstteki](https://ss64.com/bash/top.html) komutu çalıştırmayı deneyin. Uygulamanızın işlemini işlem listesinde görebilmeniz gerekir. Aşağıdaki örnek çıktıda, ile olan `PID 263` .

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

[Azure forumuna](https://docs.microsoft.com/answers/topics/azure-webapps.html)sorular ve sorunlar gönderebilirsiniz.

Kapsayıcılar için Web App hakkında daha fazla bilgi için bkz.

* [VS Code Azure App Service Node.js uygulamalarda uzaktan hata ayıklamayı tanıtma](https://medium.com/@auchenberg/introducing-remote-debugging-of-node-js-apps-on-azure-app-service-from-vs-code-in-public-preview-9b8d83a6e1f0)
* [Kapsayıcılar için Web App için özel bir Docker görüntüsü kullanma](quickstart-docker-go.md)
* [Linux üzerinde Azure App Service’te .NET Core Kullanma](quickstart-dotnetcore.md)
* [Linux üzerinde Azure App Service’te Ruby Kullanma](quickstart-ruby.md)
* [Kapsayıcılar için Azure App Service Web App SSS](app-service-linux-faq.md)
