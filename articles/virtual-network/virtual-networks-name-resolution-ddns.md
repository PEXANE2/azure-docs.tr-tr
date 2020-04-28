---
title: Azure 'da ana bilgisayar adlarını kaydettirmek için dinamik DNS kullanma | Microsoft Docs
description: Kendi DNS sunucularınızda ana bilgisayar adlarını kaydettirmek için dinamik DNS ayarlamayı öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60640387"
---
# <a name="use-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Kendi DNS sunucunuzda ana bilgisayar adlarını kaydetmek için dinamik DNS kullanma

Azure, sanal makineler (VM) ve rol örnekleri için [ad çözümlemesi sağlar](virtual-networks-name-resolution-for-vms-and-role-instances.md) . Ad çözünürlüğünüz, Azure 'un varsayılan DNS DNS 'in sağladığı özellikleri aştığında kendi DNS sunucularınızı sağlayabilirsiniz. Kendi DNS sunucularınızı kullanarak, DNS çözümünüzü kendi özel gereksinimlerinize uyacak şekilde uyarlayabilme olanağı elde edersiniz. Örneğin, Active Directory etki alanı denetleyiciniz aracılığıyla şirket içi kaynaklara erişmeniz gerekebilir.

Özel DNS sunucularınız Azure sanal makineleri olarak barındırılıyorsa, ana bilgisayar adlarını çözümlemek için aynı sanal ağa ait konak sorgularını Azure 'a iletebilirsiniz. Bu seçeneği kullanmak istemiyorsanız, sanal makine ana bilgisayar adlarını dinamik DNS (DDNS) kullanarak DNS sunucunuza kaydedebilirsiniz. Azure, DNS sunucularınızda kayıtları doğrudan oluşturmak için kimlik bilgilerine sahip değildir, bu nedenle diğer düzenlemeler genellikle gereklidir. Bazı yaygın senaryolar, alternatiflerle birlikte şunları izler:

## <a name="windows-clients"></a>Windows istemcileri
Etki alanına katılmış olmayan Windows istemcileri, önyüklenirken veya IP adresleri değiştiğinde güvenli olmayan DDNS güncelleştirmelerini dener. DNS adı, ana bilgisayar adı artı birincil DNS son ekidir. Azure birincil DNS sonekini boş bırakır, ancak son eki [Kullanıcı arabirimi](https://technet.microsoft.com/library/cc794784.aspx) veya [PowerShell](/powershell/module/dnsclient/set-dnsclient)aracılığıyla sanal makinede ayarlayabilirsiniz.

Etki alanına katılmış Windows istemcileri, IP adreslerini güvenli DDNS kullanarak etki alanı denetleyicisi ile kaydeder. Etki alanına ekleme işlemi, istemcideki birincil DNS sonekini ayarlar ve güven ilişkisini oluşturur ve korur.

## <a name="linux-clients"></a>Linux istemcileri
Linux istemcileri başlangıçta DNS sunucusuna kayıt yaptırmaz, DHCP sunucusunun bunu yaptığı varsayılmaktadır. Azure 'un DHCP sunucularının, DNS sunucunuzdaki kayıtları kaydetme kimlik bilgileri yoktur. DDNS güncelleştirmelerini göndermek için, `nsupdate`bağlama paketine dahil edilen adlı bir aracı kullanabilirsiniz. DDNS Protokolü standartlaştırılmış olduğundan, DNS sunucusunda bağlama kullanmadığınız durumlarda `nsupdate` bile kullanabilirsiniz.

DHCP istemcisi tarafından, DNS sunucusunda ana bilgisayar adı girişini oluşturmak ve korumak için sunulan kancaları kullanabilirsiniz. DHCP çevrimi sırasında istemci, komut dosyalarını */etc/DHCP/dhclient-Exit-kancas.d/* içinde yürütür. Kullanarak `nsupdate`yeni IP adresini kaydetmek için kancaları kullanabilirsiniz. Örneğin:

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

Güvenli DDNS güncelleştirmeleri gerçekleştirmek `nsupdate` için komutunu da kullanabilirsiniz. Örneğin, bir BIND DNS sunucusu kullanırken, ortak özel anahtar çifti [oluşturulur](http://linux.yyz.us/nsupdate/). DNS sunucusu, anahtarın genel bölümüyle [yapılandırılır](http://linux.yyz.us/dns/ddns-server.html) , böylece istekteki imzayı doğrulayabilirler. İçin `nsupdate`anahtar çiftini sağlamak üzere, DDNS Güncelleştirme isteğinin `-k` imzalanabilmesi için seçeneğini kullanın.

Bir Windows DNS sunucusu kullanırken, içindeki `-g` `nsupdate`parametresiyle Kerberos kimlik doğrulamasını kullanabilirsiniz, ancak Windows sürümünde kullanılamaz. `nsupdate` Kerberos 'u kullanmak için kimlik `kinit` bilgilerini yüklemek üzere kullanın. Örneğin, bir [keytab dosyasından](https://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)kimlik bilgilerini yükleyebilir ve sonra `nsupdate -g` önbellekten kimlik bilgilerini alabilir.

Gerekirse, sanal makinelerinize bir DNS arama son eki ekleyebilirsiniz. DNS son eki, */etc/resolv.exe* dosyasında belirtilir. Çoğu Linux, bu dosyanın içeriğini otomatik olarak yönetir, bu nedenle genellikle düzenleyemezsiniz. Ancak, DHCP istemcisinin `supersede` komutunu kullanarak soneki geçersiz kılabilirsiniz. Son eki geçersiz kılmak için, */etc/DHCP/dhclient.exe* dosyasına şu satırı ekleyin:

```
supersede domain-name <required-dns-suffix>;
```
