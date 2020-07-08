---
title: Etki alanı bağımsız bir çalışma grubu kullanılabilirlik grubunu yapılandırma
description: Azure 'da SQL Server bir sanal makinede Active Directory etki alanı bağımsız çalışma grubu Always on kullanılabilirlik grubu yapılandırma hakkında bilgi edinin.
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
ms.openlocfilehash: 93819332def05022272eabc130e0f2240938f244
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85955514"
---
# <a name="configure-a-workgroup-availability-group"></a>Çalışma grubu kullanılabilirlik grubunu yapılandırma 
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede, her zaman açık kullanılabilirlik grubu ile Active Directory bir etki alanı bağımsız kümesi oluşturmak için gereken adımlar açıklanmaktadır; Bu, çalışma grubu kümesi olarak da bilinir. Bu makale, çalışma grubu ve kullanılabilirlik grubunu hazırlamaya ve yapılandırmaya yönelik adımlara ve küme oluşturma veya kullanılabilirlik grubunu dağıtma gibi diğer makalelerde kapsanan adımları glosses. 


## <a name="prerequisites"></a>Ön koşullar

Bir çalışma grubu kullanılabilirlik grubunu yapılandırmak için şunlar gerekir:
- SQL Server 2016 (veya üzeri) çalıştıran sanal makineler, aynı Kullanılabilirlik kümesine veya statik IP adresleri kullanılarak farklı kullanılabilirlik bölgelerine dağıtılır. 2016 
- Alt ağda en az 4 boş IP adresi olan bir yerel ağ. 
- Yönetici grubundaki her makinede, SQL Server içinde sysadmin haklarına sahip bir hesap. 
- Açık bağlantı noktaları: TCP 1433, TCP 5022, TCP 59999. 

Başvuru için, bu makalede aşağıdaki parametreler kullanılır, ancak gerekli olduğu gibi değiştirilebilir: 

| **Adı** | **Parametre** |
| :------ | :---------------------------------- |
| **Düğüm1**   | AGNode1 (10.0.0.4) |
| **Düğüm2**   | AGNode2 (10.0.0.5) |
| **Küme adı** | AGWGAG (10.0.0.6) |
| **Dinleyici** | AGListener (kısmına 10.0.0.7) | 
| **DNS son eki** | ag.wgcluster.example.com | 
| **Çalışma grubu adı** | AGWorkgroup | 
| &nbsp; | &nbsp; |

## <a name="set-a-dns-suffix"></a>DNS son eki ayarlama 

Bu adımda, her iki sunucu için de DNS sonekini yapılandırın. Örneğin, `ag.wgcluster.example.com`. Bu, bağlanmak istediğiniz nesnenin adını ağınız içinde (gibi) tam bir adres olarak kullanmanıza olanak tanır `AGNode1.ag.wgcluster.example.com` . 

DNS sonekini yapılandırmak için aşağıdaki adımları izleyin:

1. İlk düğümünüz için RDP ve Sunucu Yöneticisi açın. 
1. **Yerel sunucu** ' yı seçin ve ardından **bilgisayar adı**bölümünde sanal makinenizin adını seçin. 
1. **Bu bilgisayarı yeniden adlandırmak Için** **Değiştir..** . öğesini seçin... 
1. Çalışma grubu adının adını, şöyle bir anlamlı olacak şekilde değiştirin `AGWORKGROUP` : 

   ![Çalışma grubu adını değiştir](./media/availability-group-clusterless-workgroup-configure/1-change-workgroup-name.png)

1. **DNS son eki ve NetBIOS bilgisayar adı** iletişim kutusunu açmak Için **daha fazla...** seçeneğini belirleyin. 
1. DNS son ekinin adını **Bu bilgisayarın BIRINCIL DNS son eki**altına yazın (gibi) `ag.wgcluster.example.com` ve ardından **Tamam**' ı seçin: 

   ![DNS son eki Ekle](./media/availability-group-clusterless-workgroup-configure/2-add-dns-suffix.png)

1. **Tam bilgisayar adının** artık DNS sonekini gösteriyor olduğunu onaylayın ve ardından değişikliklerinizi kaydetmek için **Tamam** ' ı seçin: 

   ![DNS son eki Ekle](./media/availability-group-clusterless-workgroup-configure/3-confirm-full-computer-name.png)

1. İstendiğinde sunucuyu yeniden başlatın. 
1. Kullanılabilirlik grubu için kullanılacak diğer düğümlerde bu adımları yineleyin. 

## <a name="edit-a-host-file"></a>Konak dosyasını düzenleme

Active Directory olmadığından, Windows bağlantılarının kimliklerinin doğrulanması bir yolu yoktur. Bu nedenle, ana bilgisayar dosyasını bir metin düzenleyicisiyle düzenleyerek güven atayın. 

Konak dosyasını düzenlemek için aşağıdaki adımları izleyin:

1. Sanal makinenize RDP. 
1. Gitmek için **Dosya Gezgini** 'ni kullanın `c:\windows\system32\drivers\etc` . 
1. **Hosts** dosyasına sağ tıklayın ve dosyayı **Notepad** (veya başka bir metin düzenleyici) ile açın.
1. Dosyanın sonunda, her düğüm için bir giriş, kullanılabilirlik grubu ve şunun gibi bir şekilde dinleyici ekleyin `IP Address, DNS Suffix #comment` : 

   ```
   10.0.0.4 AGNode1.ag.wgcluster.example.com #Availability group node
   10.0.0.5 AGNode2.ag.wgcluster.example.com #Availability group node
   10.0.0.6 AGWGAG.ag.wgcluster.example.com #Cluster IP
   10.0.0.7 AGListener.ag.wgcluster.example.com #Listener IP
   ```
 
   ![Ana bilgisayar dosyasına IP adresi, küme ve dinleyici girdilerini ekleyin](./media/availability-group-clusterless-workgroup-configure/4-host-file.png)

## <a name="set-permissions"></a>İzinleri ayarlama

İzinleri yönetmek için Active Directory olmadığından, yerleşik olmayan bir yerel yönetici hesabına kümeyi oluşturmak için el ile izin vermeniz gerekir. 

Bunu yapmak için, aşağıdaki PowerShell cmdlet 'ini her düğümde bir yönetim PowerShell oturumunda çalıştırın: 

```PowerShell

new-itemproperty -path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System -Name LocalAccountTokenFilterPolicy -Value 1
```

## <a name="create-the-failover-cluster"></a>Yük devretme kümesini oluşturma

Bu adımda, yük devretme kümesi oluşturacaksınız. Bu adımları tanımıyorsanız, bunları [Yük devretme kümesi öğreticiden](failover-cluster-instance-storage-spaces-direct-manually-configure.md)izleyebilirsiniz.

Öğretici ve bir çalışma grubu kümesi için yapılması gerekenler arasındaki önemli farklılıklar:
- **Depolama**işaretini kaldırın ve küme doğrulamayı çalıştırırken **depolama alanları doğrudan** . 
- Kümeye düğüm eklerken, tam adı şu şekilde ekleyin:
   - `AGNode1.ag.wgcluster.example.com`
   - `AGNode2.ag.wgcluster.example.com`
- **Tüm uygun depolamayı kümeye ekle**seçeneğinin işaretini kaldırın. 

Küme oluşturulduktan sonra bir statik küme IP adresi atayın. Bunu yapmak için şu adımları uygulayın:

1. Düğümlerden birinde, **Yük devretme kümesi Yöneticisi**açın, kümeyi seçin, **küme çekirdek kaynakları** altında, ** \<ClusterNam> ad** ' a sağ tıklayın ve ardından **Özellikler**' i seçin. 

   ![Küme adı için başlatma özellikleri](./media/availability-group-clusterless-workgroup-configure/5-launch-cluster-name-properties.png)

1. **IP adresleri** altında IP adresini seçin ve **Düzenle**' yi seçin. 
1. **Statik kullan**' ı seçin, kümenin IP adresini belirtin ve ardından **Tamam**' ı seçin: 

   ![Küme için statik bir IP adresi sağlayın](./media/availability-group-clusterless-workgroup-configure/6-provide-static-ip-for-cluster.png)

1. Ayarlarınızın doğru göründüğünü doğrulayın ve ardından bunları kaydetmek için **Tamam** ' ı seçin:

   ![Küme özelliklerini doğrulama](./media/availability-group-clusterless-workgroup-configure/7-verify-cluster-properties.png)

## <a name="create-a-cloud-witness"></a>Bulut tanığı oluşturma 

Bu adımda, bir bulut paylaşma tanığı yapılandırın. Adımlara alışkın değilseniz bkz. [Yük devretme kümesi Için bulut tanığı dağıtma](/windows-server/failover-clustering/deploy-cloud-witness). 

## <a name="enable-the-availability-group-feature"></a>Kullanılabilirlik grubu özelliğini etkinleştirme 

Bu adımda, kullanılabilirlik grubu özelliğini etkinleştirin. Adımlara alışkın değilseniz, [kullanılabilirlik grubu öğreticisine](availability-group-manually-configure-tutorial.md#enable-availability-groups)bakın. 

## <a name="create-keys-and-certificates"></a>Anahtar ve sertifika oluşturma

Bu adımda, bir SQL oturum açmanın şifreli uç noktada kullandığı sertifikalar oluşturun. Her düğümde, sertifika yedeklerini tutacak bir klasör oluşturun (örneğin,) `c:\certs` . 

İlk düğümü yapılandırmak için aşağıdaki adımları izleyin: 

1. **SQL Server Management Studio** açın ve ilk düğüme bağlanın, örneğin `AGNode1` . 
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

1. Sertifikanızın olduğu dosya konumuna gitmek için **Dosya Gezgini** 'ni kullanın `c:\certs` . 
1. Sertifikanın bir kopyasını (örneğin, ilk düğümden) el ile oluşturun `AGNode1Cert.crt` ve ikinci düğümdeki aynı konuma aktarın. 

İkinci düğümü yapılandırmak için aşağıdaki adımları izleyin: 

1. İkinci düğüme, gibi **SQL Server Management Studio**bağlayın `AGNode2` . 
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

1. Sertifikanızın olduğu dosya konumuna gitmek için **Dosya Gezgini** 'ni kullanın `c:\certs` . 
1. Sertifikanın bir kopyasını (örneğin, ikinci düğümden) el ile oluşturun `AGNode2Cert.crt` ve ilk düğümde aynı konuma aktarın. 

Kümede başka bir düğüm varsa, ilgili sertifika adlarını değiştirerek bu adımları da yineleyin. 

## <a name="create-logins"></a>Oturum açma işlemleri oluşturma

Sertifika kimlik doğrulaması, düğümleri arasında verileri eşzamanlı hale getirmek için kullanılır. Buna izin vermek için, diğer düğüm için bir oturum açın, oturum açma için bir kullanıcı oluşturun, yedeklenen sertifikayı kullanmak için oturum açma için bir sertifika oluşturun ve ardından yansıtma uç noktasında Connect 'e verin. 

Bunu yapmak için ilk düğümde aşağıdaki Transact-SQL (T-SQL) sorgusunu çalıştırın, örneğin `AGNode1` : 

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

Sonra, ikinci düğümde aşağıdaki Transact-SQL (T-SQL) sorgusunu çalıştırın, örneğin `AGNode2` : 

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

## <a name="configure-an-availability-group"></a>Kullanılabilirlik grubu yapılandırma

Bu adımda, kullanılabilirlik grubunuzu yapılandırın ve veritabanlarınızı ona ekleyin. Şu an bir dinleyici oluşturmayın. Adımlara alışkın değilseniz, [kullanılabilirlik grubu öğreticisi](availability-group-manually-configure-tutorial.md#create-the-availability-group)' ne bakın. Her şeyin olması gerektiği şekilde çalıştığını doğrulamak için bir yük devretme ve yeniden çalışma işlemi başlattığınızdan emin olun. 

   > [!NOTE]
   > Eşitleme işlemi sırasında bir hata oluşursa, `NT AUTHORITY\SYSTEM` ilk düğümde geçici olarak olduğu gibi küme kaynakları oluşturmak için sysadmin hakları vermeniz gerekebilir `AGNode1` . 

## <a name="configure-a-load-balancer"></a>Yük dengeleyiciyi yapılandırma

Bu son adımda, [Azure Portal](availability-group-load-balancer-portal-configure.md) veya [PowerShell](availability-group-listener-powershell-configure.md)'i kullanarak yük dengeleyiciyi yapılandırın.


## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilirlik grubunu yapılandırmak için [az SQL VM CLI](availability-group-az-cli-configure.md) de kullanabilirsiniz. 


