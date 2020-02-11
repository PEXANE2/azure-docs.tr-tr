---
title: Etki alanı bağımsız bir çalışma grubu kullanılabilirlik grubunu yapılandırma
description: Azure 'da SQL Server bir sanal makinede Active Directory Etki Alanı bağımsız bir çalışma grubu Always on kullanılabilirlik grubunu yapılandırmayı öğrenin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
editor: ''
tags: azure-service-management
ms.assetid: 53981f7e-8370-4979-b26a-93a5988d905f
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/29/2020
ms.author: mathoma
ms.openlocfilehash: 72c04cf5e3e5fbdeac2d267dfc7b2703bd37a1c2
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77122681"
---
# <a name="configure-a-workgroup-availability-group"></a>Çalışma grubu kullanılabilirlik grubunu yapılandırma 

Bu makalede, her zaman açık kullanılabilirlik grubu ile Active Directory bir etki alanı bağımsız kümesi oluşturmak için gereken adımlar açıklanmaktadır; Bu, çalışma grubu kümesi olarak da bilinir. Bu makale, çalışma grubu ve kullanılabilirlik grubunu hazırlamaya ve yapılandırmaya yönelik adımlara ve küme oluşturma veya kullanılabilirlik grubunu dağıtma gibi diğer makalelerde kapsanan adımları glosses. 


## <a name="prerequisites"></a>Önkoşullar

Bir çalışma grubu kullanılabilirlik grubunu yapılandırmak için şunlar gerekir:
- SQL Server 2016 (veya üzeri) çalıştıran sanal makineler, aynı Kullanılabilirlik kümesine veya statik IP adresleri kullanılarak farklı kullanılabilirlik bölgelerine dağıtılır. 2016 
- Alt ağda en az 4 boş IP adresi olan bir yerel ağ. 
- Yönetici grubundaki her makinede, SQL Server içinde sysadmin haklarına sahip bir hesap. 
- Açık bağlantı noktaları: TCP 1433, TCP 5022, TCP 59999. 

Başvuru için, bu makalede aşağıdaki parametreler kullanılır, ancak gerekli olduğu gibi değiştirilebilir: 

| **Ad** | **Parametresinin** |
| :------ | :---------------------------------- |
| **Düğüm1**   | AGNode1 (10.0.0.4) |
| **Düğüm2**   | AGNode2 (10.0.0.5) |
| **Küme adı** | AGWGAG (10.0.0.6) |
| **Oluşturulurken** | AGListener (kısmına 10.0.0.7) | 
| **DNS son eki** | ag.wgcluster.example.com | 
| **Çalışma grubu adı** | AGWorkgroup | 
| &nbsp; | &nbsp; |

## <a name="set-dns-suffix"></a>DNS sonekini ayarla 

Bu adımda, her iki sunucu için de DNS sonekini yapılandırın. Örneğin, `ag.wgcluster.example.com`. Bu, bağlanmak istediğiniz nesnenin adını ağınız içinde `AGNode1.ag.wgcluster.example.com`gibi tam bir adres olarak kullanmanıza olanak tanır. 

DNS sonekini yapılandırmak için aşağıdaki adımları izleyin:

1. İlk düğümünüz için RDP ve Sunucu Yöneticisi açın. 
1. **Yerel sunucu** ' yı seçin ve ardından **bilgisayar adı**bölümünde sanal makinenizin adını seçin. 
1. **Bu bilgisayarı yeniden adlandırmak Için** **Değiştir..** . öğesini seçin... 
1. Çalışma grubu adının adını `AGWORKGROUP`gibi bir anlamlı olacak şekilde değiştirin: 

   ![Çalışma grubu adını değiştir](media/virtual-machines-windows-sql-workgroup-availability-group/1-change-workgroup-name.png)

1. **DNS son eki ve NetBIOS bilgisayar adı** iletişim kutusunu açmak Için **daha fazla...** seçeneğini belirleyin. 
1. **Bu bilgisayarın BIRINCIL DNS son eki**altında, `ag.wgcluster.example.com` gibi DNS son ekinin adını yazın ve ardından **Tamam**' ı seçin: 

   ![DNS son eki Ekle](media/virtual-machines-windows-sql-workgroup-availability-group/2-add-dns-suffix.png)

1. **Tam bilgisayar adının** artık DNS sonekini gösteriyor olduğunu onaylayın ve ardından değişikliklerinizi kaydetmek için **Tamam** ' ı seçin: 

   ![DNS son eki Ekle](media/virtual-machines-windows-sql-workgroup-availability-group/3-confirm-full-computer-name.png)

1. İstendiğinde sunucuyu yeniden başlatın. 
1. Kullanılabilirlik grubu için kullanılacak diğer düğümlerde bu adımları yineleyin. 

## <a name="edit-host-file"></a>Konak dosyasını Düzenle

Active Directory olmadığından, Windows bağlantılarının kimliklerinin doğrulanması bir yolu yoktur. Bu nedenle, ana bilgisayar dosyasını bir metin düzenleyicisiyle düzenleyerek güven atayın. 

Konak dosyasını düzenlemek için aşağıdaki adımları izleyin:

1. Sanal makinenize RDP ekleyin. 
1. `c:\windows\system32\drivers\etc`gitmek için **Dosya Gezgini** 'ni kullanın. 
1. **Hosts** dosyasına sağ tıklayın ve dosyayı **Notepad** (veya başka bir metin düzenleyici) ile açın.
1. Dosyanın sonunda, her düğüm için bir giriş, kullanılabilirlik grubu ve `IP Address, DNS Suffix #comment` gibi bir dinleyici ekleyin: 

   ```
   10.0.0.4 AGNode1.ag.wgcluster.example.com #Availability group node
   10.0.0.5 AGNode2.ag.wgcluster.example.com #Availability group node
   10.0.0.6 AGWGAG.ag.wgcluster.example.com #Cluster IP
   10.0.0.7 AGListener.ag.wgcluster.example.com #Listener IP
   ```
 
   ![Ana bilgisayar dosyasına IP adresi, küme ve dinleyici girdilerini ekleyin](media/virtual-machines-windows-sql-workgroup-availability-group/4-host-file.png)

## <a name="set-permissions"></a>İzinleri ayarla

İzinleri yönetmek için Active Directory olmadığından, yerleşik olmayan bir yerel yönetici hesabına kümeyi oluşturmak için el ile izin vermeniz gerekir. 

Bunu yapmak için, aşağıdaki PowerShell cmdlet 'ini her düğümde bir yönetim PowerShell oturumunda çalıştırın: 

```PowerShell

new-itemproperty -path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System -Name LocalAccountTokenFilterPolicy -Value 1
```

## <a name="create-the-failover-cluster"></a>Yük devretme kümesi oluşturma

Bu adımda, yük devretme kümesi oluşturacaksınız. Bu adımları tanımıyorsanız, bunları [Yük devretme kümesi öğreticiden](virtual-machines-windows-portal-sql-create-failover-cluster.md#step-2-configure-the-windows-server-failover-cluster-with-storage-spaces-direct)izleyebilirsiniz.

Öğretici ve bir çalışma grubu kümesi için yapılması gerekenler arasındaki önemli farklılıklar:
- **Depolama**işaretini kaldırın ve küme doğrulamayı çalıştırırken **depolama alanları doğrudan** . 
- Kümeye düğüm eklerken, tam adı şu şekilde ekleyin:
   - `AGNode1.ag.wgcluster.example.com`
   - `AGNode2.ag.wgcluster.example.com`
- **Tüm uygun depolamayı kümeye ekle**seçeneğinin işaretini kaldırın. 

Küme oluşturulduktan sonra bir statik küme IP adresi atayın. Bunu yapmak için aşağıdaki adımları izleyin:

1. Düğümlerden birinde, **Yük devretme kümesi Yöneticisi**açın, kümeyi seçin, **küme çekirdek kaynakları** altında **\<clusternam > adına** sağ tıklayın ve ardından **Özellikler**' i seçin. 

   ![Küme adı için başlatma özellikleri](media/virtual-machines-windows-sql-workgroup-availability-group/5-launch-cluster-name-properties.png)

1. **IP adresleri** altında IP adresini seçin ve **Düzenle**' yi seçin. 
1. **Statik kullan**' ı seçin, kümenin IP adresini belirtin ve ardından **Tamam**' ı seçin: 

   ![Küme için statik bir IP adresi sağlayın](media/virtual-machines-windows-sql-workgroup-availability-group/6-provide-static-ip-for-cluster.png)

1. Ayarlarınızın doğru göründüğünü doğrulayın ve ardından bunları kaydetmek için **Tamam** ' ı seçin:

   ![Küme özelliklerini doğrulama](media/virtual-machines-windows-sql-workgroup-availability-group/7-verify-cluster-properties.png)

## <a name="create-a-cloud-witness"></a>Bulut tanığı oluşturma 

Bu adımda, bir bulut paylaşma tanığı yapılandırın. Adımları tanımıyorsanız [Yük devretme kümesi öğreticisi](virtual-machines-windows-portal-sql-create-failover-cluster.md#create-a-cloud-witness)' ne bakın. 

## <a name="enable-availability-group-feature"></a>Kullanılabilirlik grubu özelliğini etkinleştir 

Bu adımda, kullanılabilirlik grubu özelliğini etkinleştirin. Adımlara alışkın değilseniz, [kullanılabilirlik grubu öğreticisine](virtual-machines-windows-portal-sql-availability-group-tutorial.md#enable-availability-groups)bakın. 

## <a name="create-keys-and-certificate"></a>Anahtar ve sertifika oluşturma

Bu adımda, bir SQL oturum açmanın şifreli uç noktada kullandığı sertifikalar oluşturun. Her düğümde, `c:\certs`gibi sertifika yedeklerini tutacak bir klasör oluşturun. 

İlk düğümü yapılandırmak için aşağıdaki adımları izleyin: 

1. **SQL Server Management Studio** açın ve `AGNode1`gibi ilk düğümünüz bağlayın. 
1. Yeni bir **sorgu** penceresi açın ve karmaşık ve güvenli bir parolaya güncelleştirdikten sonra aşağıdaki Transact-SQL (T-SQL) ifadesini çalıştırın:

   ```sql
   USE master;  
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'PassWOrd123!';  
   GO

   --create a cert from the master key
   USE master;  
   CREATE CERTIFICATE AGNode1Cert   
      WITH SUBJECT = 'AGNode1 Certificate';  
   GO  

   --Backup the cert and transfer it to AGNode2
   BACKUP CERTIFICATE AGNode1Cert TO FILE = 'C:\certs\AGNode1Cert.crt';  
   GO  
   ```

1. Ardından, HADR uç noktasını oluşturun ve bu Transact-SQL (T-SQL) ifadesini çalıştırarak kimlik doğrulaması için sertifikayı kullanın:

   ```sql
   --CREATE or ALTER the mirroring endpoint
   CREATE ENDPOINT hadr_endpoint  
      STATE = STARTED  
      AS TCP (  
         LISTENER_PORT=5022  
         , LISTENER_IP = ALL  
      )   
      FOR DATABASE_MIRRORING (   
         AUTHENTICATION = CERTIFICATE AGNode1Cert  
         , ENCRYPTION = REQUIRED ALGORITHM AES  
         , ROLE = ALL  
      );  
   GO  
   ```

1. `c:\certs`gibi, sertifikanızın olduğu dosya konumuna gitmek için **Dosya Gezgini** 'ni kullanın. 
1. Sertifikanın `AGNode1Cert.crt`, ilk düğümden bir kopyasını el ile oluşturun ve ikinci düğümdeki aynı konuma aktarın. 

İkinci düğümü yapılandırmak için aşağıdaki adımları izleyin: 

1. `AGNode2`gibi **SQL Server Management Studio**ikinci düğüme bağlayın. 
1. Yeni bir **sorgu** penceresinde, karmaşık ve güvenli bir parolaya güncelleştirdikten sonra aşağıdaki Transact-SQL (T-SQL) ifadesini çalıştırın: 

   ```sql
   USE master;  
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'PassWOrd123!';  
   GO 
   
   --create a cert from the master key
   USE master;  
   CREATE CERTIFICATE AGNode2Cert   
      WITH SUBJECT = 'AGNode2 Certificate';  
   GO  
   --Backup the cert and transfer it to AGNode1
   BACKUP CERTIFICATE AGNode2Cert TO FILE = 'C:\certs\AGNode2Cert.crt';  
   GO
   ```

1. Ardından, HADR uç noktasını oluşturun ve bu Transact-SQL (T-SQL) ifadesini çalıştırarak kimlik doğrulaması için sertifikayı kullanın:

   ```sql
   --CREATE or ALTER the mirroring endpoint
   CREATE ENDPOINT hadr_endpoint  
      STATE = STARTED  
      AS TCP (  
         LISTENER_PORT=5022  
         , LISTENER_IP = ALL  
      )   
      FOR DATABASE_MIRRORING (   
         AUTHENTICATION = CERTIFICATE AGNode2Cert  
         , ENCRYPTION = REQUIRED ALGORITHM AES  
         , ROLE = ALL  
      );  
   GO  
   ```

1. `c:\certs`gibi, sertifikanızın olduğu dosya konumuna gitmek için **Dosya Gezgini** 'ni kullanın. 
1. Sertifikanın `AGNode2Cert.crt`, ikinci düğümden bir kopyasını el ile oluşturun ve ilk düğümdeki aynı konuma aktarın. 

Kümede başka bir düğüm varsa, ilgili sertifika adlarını değiştirerek bu adımları da yineleyin. 

## <a name="create-logins"></a>Oturum açma işlemleri oluşturma

Sertifika kimlik doğrulaması, düğümleri arasında verileri eşzamanlı hale getirmek için kullanılır. Buna izin vermek için, diğer düğüm için bir oturum açın, oturum açma için bir kullanıcı oluşturun, yedeklenen sertifikayı kullanmak için oturum açma için bir sertifika oluşturun ve ardından yansıtma uç noktasında Connect 'e verin. 

Bunu yapmak için önce ilk düğümde aşağıdaki Transact-SQL (T-SQL) sorgusunu çalıştırın, örneğin `AGNode1`: 

```sql
--create a login for the AGNode2
USE master;  
CREATE LOGIN AGNode2_Login WITH PASSWORD = 'PassWord123!';  
GO  

--create a user from the login
CREATE USER AGNode2_User FOR LOGIN AGNode2_Login;  
GO  

--create a certificate that the login uses for authentication
CREATE CERTIFICATE AGNode2Cert  
   AUTHORIZATION AGNode2_User  
   FROM FILE = 'C:\certs\AGNode2Cert.crt'  
GO 

--grant connect for login
GRANT CONNECT ON ENDPOINT::hadr_endpoint TO [AGNode2_login];  
GO
```

Ardından, `AGNode2`gibi ikinci düğümde aşağıdaki Transact-SQL (T-SQL) sorgusunu çalıştırın: 

```sql
--create a login for the AGNode1
USE master;  
CREATE LOGIN AGNode1_Login WITH PASSWORD = 'PassWord123!';  
GO  

--create a user from the login
CREATE USER AGNode1_User FOR LOGIN AGNode1_Login;  
GO  

--create a certificate that the login uses for authentication
CREATE CERTIFICATE AGNode1Cert  
   AUTHORIZATION AGNode1_User  
   FROM FILE = 'C:\certs\AGNode1Cert.crt'  
GO 

--grant connect for login
GRANT CONNECT ON ENDPOINT::hadr_endpoint TO [AGNode1_login];  
GO
```

Kümede başka bir düğüm varsa, ilgili sertifikayı ve Kullanıcı adlarını değiştirerek bu adımları da yineleyin. 

## <a name="configure-availability-group"></a>Kullanılabilirlik grubunu yapılandır

Bu adımda, kullanılabilirlik grubunuzu yapılandırın ve veritabanlarınızı ona ekleyin. Şu an bir dinleyici oluşturmayın. Adımlara alışkın değilseniz, [kullanılabilirlik grubu öğreticisi](virtual-machines-windows-portal-sql-availability-group-tutorial.md#create-the-availability-group)' ne bakın. Her şeyin olması gerektiği şekilde çalıştığını doğrulamak için bir yük devretme ve yeniden çalışma işlemi başlattığınızdan emin olun. 

   > [!NOTE]
   > Eşitleme işlemi sırasında bir hata oluşursa, ilk düğümde geçici olarak `AGNode1` gibi küme kaynakları oluşturmak için `NT AUTHORITY\SYSTEM` sysadmin hakları vermeniz gerekebilir. 

## <a name="configure-load-balancer"></a>Yük dengeleyiciyi yapılandırma

Bu son adımda, [Azure Portal](virtual-machines-windows-portal-sql-alwayson-int-listener.md) veya [PowerShell](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) 'i kullanarak yük dengeleyiciyi yapılandırın


## <a name="next-steps"></a>Sonraki Adımlar

Kullanılabilirlik grubunu yapılandırmak için [az SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md) de kullanabilirsiniz. 


