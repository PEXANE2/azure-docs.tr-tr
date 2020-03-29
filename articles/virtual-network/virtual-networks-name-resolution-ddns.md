---
title: Azure'da ana bilgisayar adlarını kaydetmek için dinamik DNS kullanma | Microsoft Dokümanlar
description: Kendi DNS sunucularınıza ana bilgisayar adlarını kaydetmek için dinamik DNS'yi nasıl kuracağınızı öğrenin.
services: dns
documentationcenter: na
author: subsarma
manager: vitinnan
editor: ''
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: subsarma
ms.openlocfilehash: c2ef842fd62ef060f06536d66387c3facd0627b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60640387"
---
# <a name="use-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Kendi DNS sunucunuzda ana bilgisayar adlarını kaydetmek için dinamik DNS kullanma

Azure, sanal makineler (VM) ve rol örnekleri için [ad çözümlemesi sağlar.](virtual-networks-name-resolution-for-vms-and-role-instances.md) Ad çözümlemesi gereksinimleriniz Azure'un varsayılan DNS'si tarafından sağlanan yetenekleri aştığında, kendi DNS sunucularınızı sağlayabilirsiniz. Kendi DNS sunucularınızı kullanmak, DNS çözümünüzü kendi özel gereksinimlerinize göre uyarlama olanağı sağlar. Örneğin, Etkin Dizin etki alanı denetleyiciniz aracılığıyla şirket içi kaynaklara erişmeniz gerekebilir.

Özel DNS sunucularınız Azure VM'ler olarak barındırıldığında, ana bilgisayar adlarını çözmek için aynı sanal ağın ana bilgisayar adı sorgularını Azure'a iletebilirsiniz. Bu seçeneği kullanmak istemiyorsanız, Dinamik DNS (DDNS) kullanarak VM ana bilgisayar adlarınızı DNS sunucunuza kaydedebilirsiniz. Azure,DNS sunucularınızda doğrudan kayıt oluşturacak kimlik bilgilerine sahip değildir, bu nedenle genellikle alternatif düzenlemelere ihtiyaç duyur. Alternatifleri ile bazı yaygın senaryolar izleyin:

## <a name="windows-clients"></a>Windows istemcileri
Etki alanına katılmayan Windows istemcileri, önyükleme yaptıklarında veya IP adresleri değiştiğinde güvenli olmayan DDNS güncelleştirmelerini dener. DNS adı ana bilgisayar adı artı birincil DNS sonekidir. Azure birincil DNS sonekini boş bırakır, ancak [kullanıcı arabirimi](https://technet.microsoft.com/library/cc794784.aspx) veya [PowerShell](/powershell/module/dnsclient/set-dnsclient)üzerinden VM'deki sonek ayarlayabilirsiniz.

Etki alanına katılan Windows istemcileri, güvenli DDNS kullanarak IP adreslerini etki alanı denetleyicisiyle kaydeder. Etki alanı birleştirme işlemi istemcide birincil DNS sonekiayarlar ve güven ilişkisini oluşturur ve korur.

## <a name="linux-clients"></a>Linux istemcileri
Linux istemcileri genellikle başlangıçta DNS sunucusu ile kendilerini kayıt yok, onlar DHCP sunucusu bunu varsayıyorum. Azure'un DHCP sunucuları, DNS sunucunuzdaki kayıtları kaydedecek kimlik bilgilerine sahip değildir. DDNS güncelleştirmelerini göndermek için Bind paketine dahil edilen bir `nsupdate`araç kullanabilirsiniz. DDNS protokolü standart olduğundan, DNS sunucusunda Bind kullanmadığınızda bile kullanabilirsiniz. `nsupdate`

DNS sunucusunda ana bilgisayar girişi oluşturmak ve korumak için DHCP istemcisi tarafından sağlanan kancaları kullanabilirsiniz. DHCP döngüsü sırasında istemci */etc/dhcp/dhclient-exit-hooks.d/* komut dosyalarını yürütür. Yeni IP adresini kullanmak `nsupdate`için kancaları kullanabilirsiniz. Örnek:

```bash
#!/bin/sh
requireddomain=mydomain.local

# only execute on the primary nic
if [ "$interface" != "eth0" ]
then
    return
fi

# When you have a new IP, perform nsupdate
if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
   [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
then
   host=`hostname`
   nsupdatecmds=/var/tmp/nsupdatecmds
     echo "update delete $host.$requireddomain a" > $nsupdatecmds
     echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
     echo "send" >> $nsupdatecmds

     nsupdate $nsupdatecmds
fi
```

Güvenli DDNS güncelleştirmeleri gerçekleştirmek için `nsupdate` komutu da kullanabilirsiniz. Örneğin, Bind DNS sunucusu kullanırken, ortak-özel anahtar çifti [oluşturulur.](http://linux.yyz.us/nsupdate/) DNS sunucusu, istekteki imzayı doğrulayabilmek için anahtarın ortak bölümüyle [yapılandırılır.](http://linux.yyz.us/dns/ddns-server.html) Anahtar çiftini sağlamak `nsupdate`için, `-k` DDNS güncelleştirme isteğinin imzalanması için bu seçeneği kullanın.

Bir Windows DNS sunucusu kullanırken, `-g` Kerberos kimlik doğrulamasını parametre `nsupdate`ile kullanabilirsiniz, ancak Windows sürümünde `nsupdate`kullanılamaz. Kerberos'u kullanmak `kinit` için kimlik bilgilerini yüklemek için kullanın. Örneğin, kimlik bilgilerini bir [anahtar sekme dosyasından](https://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)yükleyebilirsiniz), ardından `nsupdate -g` önbellekten kimlik bilgilerini alır.

Gerekirse, VM'lerinize bir DNS arama soneki ekleyebilirsiniz. */etc/resolv.conf* dosyasında DNS soneki belirtilir. Çoğu Linux dağıtımcısı bu dosyanın içeriğini otomatik olarak yönetir, bu nedenle genellikle bu dosyayı ayarlayamazsınız. Ancak, DHCP istemcikomutunu `supersede` kullanarak sonek geçersiz kılınabilir. Sonekleri geçersiz kılmak için */etc/dhcp/dhclient.conf* dosyasına aşağıdaki satırı ekleyin:

```
supersede domain-name <required-dns-suffix>;
```
