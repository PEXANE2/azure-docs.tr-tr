---
title: Etki alanından bağımsız bir çalışma grubu kullanılabilirlik grubunu yapılandırma
description: Azure'daki bir SQL Server sanal makinesinde Her Zaman Kullanılabilirlik Grubunda Etkin Dizin Etki Alanı bağımsız bir çalışma grubunu nasıl yapılandırabilirsiniz öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77122681"
---
# <a name="configure-a-workgroup-availability-group"></a>Çalışma grubu kullanılabilirlik grubunu yapılandırma 

Bu makalede, her zaman kullanılabilirlik grubu olan Active Directory etki alanı bağımsız küme oluşturmak için gerekli adımları açıklar; bu, çalışma grubu kümesi olarak da bilinir. Bu makalede, çalışma grubu ve kullanılabilirlik grubunun hazırlanması ve yapılandırılmasıyla ilgili adımlar abartılır ve kümenin nasıl oluşturulması veya kullanılabilirlik grubunun nasıl dağıtılanacağı gibi diğer makalelerde kapsanan adımların üzerinde parlaklıklar yer alır. 


## <a name="prerequisites"></a>Ön koşullar

Bir çalışma grubu kullanılabilirlik grubunu yapılandırmak için aşağıdakilere ihtiyacınız var:
- En az iki Windows Server 2016 (veya daha yüksek) SANAL makineler SQL Server 2016 (veya daha yüksek) çalıştıran, statik IP adresleri kullanarak aynı kullanılabilirlik kümesine veya farklı kullanılabilirlik bölgelerine dağıtılır. 
- Alt ağda en az 4 ücretsiz IP adresi olan yerel bir ağ. 
- SQL Server içinde sysadmin hakları da olan yönetici grubundaki her makinede bir hesap. 
- Açık bağlantı noktaları: TCP 1433, TCP 5022, TCP 59999. 

Başvuru için, aşağıdaki parametreler bu makalede kullanılır, ancak gerektiği gibi değiştirilebilir: 

| **Adı** | **Parametre** |
| :------ | :---------------------------------- |
| **Düğüm1**   | AGNode1 (10.0.0.4) |
| **Düğüm2**   | AGNode2 (10.0.0.5) |
| **Küme adı** | AGWGAG (10.0.0.6) |
| **Dinleyici** | AGDinleyicier (10.0.0.7) | 
| **DNS Son Ek** | ag.wgcluster.example.com | 
| **Çalışma grubu adı** | AGÇalışma Grubu | 
| &nbsp; | &nbsp; |

## <a name="set-dns-suffix"></a>DNS soneki ayarlama 

Bu adımda, her iki sunucu için DeNS soneki yapılandırın. Örneğin, `ag.wgcluster.example.com`. Bu, ağınızda tam nitelikli bir adres olarak bağlanmak istediğiniz nesnenin adını kullanmanıza olanak sağlar. `AGNode1.ag.wgcluster.example.com` 

DNS sonekini yapılandırmak için aşağıdaki adımları izleyin:

1. RDP ilk düğümünüze ve Sunucu Yöneticisi'ni açın. 
1. **Yerel Sunucu'yu** seçin ve ardından Sanal Makinenizin adını **Bilgisayar adı**altında seçin. 
1. Bu bilgisayarı **yeniden adlandırmak için** **Değiştir'i...** seçin. 
1. Çalışma grubu adının adını anlamlı bir şey olacak `AGWORKGROUP`şekilde değiştirin: 

   ![Çalışma grubu adını değiştirme](media/virtual-machines-windows-sql-workgroup-availability-group/1-change-workgroup-name.png)

1. **DNS Soneki ve NetBIOS Bilgisayar Adı** iletişim kutusunu açmak için **Daha Fazla...** seçeneğini belirleyin. 
1. **Bu bilgisayarın Birincil DNS sonekinin altına DNS sonekinizin**adını yazın `ag.wgcluster.example.com` ve sonra **Tamam'ı**seçin: 

   ![DNS soneki ekle](media/virtual-machines-windows-sql-workgroup-availability-group/2-add-dns-suffix.png)

1. **Tam bilgisayar adının** artık DNS sonekini gösterdiğini doğrulayın ve değişikliklerinizi kaydetmek için **Tamam'ı** seçin: 

   ![DNS soneki ekle](media/virtual-machines-windows-sql-workgroup-availability-group/3-confirm-full-computer-name.png)

1. Sizden istendiğinde sunucuyu yeniden başlatın. 
1. Kullanılabilirlik grubu için kullanılacak diğer düğümlerde bu adımları yineleyin. 

## <a name="edit-host-file"></a>Ana bilgisayar dosyalarını düzenleyin

Etkin bir dizin olmadığından, windows bağlantılarını doğrulamanın bir yolu yoktur. Bu nedenle, ana bilgisayar dosyasını bir metin düzenleyicisi ile düzenleyerek güven atayın. 

Ana bilgisayar dosyasını düzenleyin, aşağıdaki adımları izleyin:

1. RDP sanal makineiçine. 
1. Gitmek için Dosya `c:\windows\system32\drivers\etc` **Gezgini'ni** kullanın. 
1. **Ana bilgisayar** dosyasını sağ tıklatın ve **Notepad** (veya başka bir metin düzenleyicisi) ile dosyayı açın.
1. Dosyanın sonunda, her düğüm, kullanılabilirlik grubu ve dinleyici `IP Address, DNS Suffix #comment` için aşağıdaki ler şeklinde bir giriş ekleyin: 

   ```
   10.0.0.4 AGNode1.ag.wgcluster.example.com #Availability group node
   10.0.0.5 AGNode2.ag.wgcluster.example.com #Availability group node
   10.0.0.6 AGWGAG.ag.wgcluster.example.com #Cluster IP
   10.0.0.7 AGListener.ag.wgcluster.example.com #Listener IP
   ```
 
   ![IP adresi, küme ve dinleyici için girişleri ana bilgisayar dosyasına ekleme](media/virtual-machines-windows-sql-workgroup-availability-group/4-host-file.png)

## <a name="set-permissions"></a>İzinleri ayarlama

İzinleri yönetmek için Etkin Dizin olmadığından, kümeoluşturmak için yerleşik olmayan bir yerel yönetici hesabının el ile izin vermeniz gerekir. 

Bunu yapmak için, her düğümüzerinde idari bir PowerShell oturumunda aşağıdaki PowerShell cmdlet çalıştırın: 

```PowerShell

new-itemproperty -path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System -Name LocalAccountTokenFilterPolicy -Value 1
```

## <a name="create-the-failover-cluster"></a>Yük devretme kümesini oluşturma

Bu adımda, failover kümesini oluşturursunuz. Bu adımları bilmiyorsanız, [bunları failover küme öğreticisinden](virtual-machines-windows-portal-sql-create-failover-cluster.md#step-2-configure-the-windows-server-failover-cluster-with-storage-spaces-direct)takip edebilirsiniz.

Öğretici ile bir çalışma grubu kümesi için yapılması gerekenler arasındaki önemli farklar:
- Küme doğrulaması çalıştırırken **Depolama**ve **Depolama Alanları Doğrudan'ın** denetimini kaldırın. 
- Düğümleri kümeye eklerken, aşağıdakiler gibi tam nitelikli adı ekleyin:
   - `AGNode1.ag.wgcluster.example.com`
   - `AGNode2.ag.wgcluster.example.com`
- İşaretle **Kümeye tüm uygun depolama alanını ekle.** 

Küme oluşturulduktan sonra statik bir Cluster IP adresi atayın. Bunu yapmak için şu adımları uygulayın:

1. Düğümlerden birinde **Failover Cluster Manager'ı**açın, kümeyi seçin, Adı sağ **tıklatın: \<ClusterNam>** **Küme Çekirdek Kaynakları** altında ve ardından **Özellikler'i**seçin. 

   ![Küme adı için başlatma özellikleri](media/virtual-machines-windows-sql-workgroup-availability-group/5-launch-cluster-name-properties.png)

1. **IP Adresleri'nin** altındaki IP adresini seçin ve **Edit'i**seçin. 
1. **Statik Kullan'ı**seçin, kümenin IP adresini girin ve sonra **Tamam'ı**seçin: 

   ![Küme için statik bir IP adresi sağlama](media/virtual-machines-windows-sql-workgroup-availability-group/6-provide-static-ip-for-cluster.png)

1. Ayarlarınızın doğru görünmesini doğrulayın ve bunları kaydetmek için **Tamam'ı** seçin:

   ![Küme özelliklerini doğrulama](media/virtual-machines-windows-sql-workgroup-availability-group/7-verify-cluster-properties.png)

## <a name="create-a-cloud-witness"></a>Bulut tanığı oluşturma 

Bu adımda, bir bulut paylaşımı tanığını yapılandırın. Adımlara aşina değilseniz, [failover kümesi öğreticisine](virtual-machines-windows-portal-sql-create-failover-cluster.md#create-a-cloud-witness)bakın. 

## <a name="enable-availability-group-feature"></a>Kullanılabilirlik grubu özelliğini etkinleştirme 

Bu adımda, kullanılabilirlik grubu özelliğini etkinleştirin. Adımlara aşina değilseniz, [kullanılabilirlik grubu öğreticisine](virtual-machines-windows-portal-sql-availability-group-tutorial.md#enable-availability-groups)bakın. 

## <a name="create-keys-and-certificate"></a>Anahtarlar ve sertifika oluşturma

Bu adımda, SQL girişinin şifreli bitiş noktasında kullandığı sertifikalar oluşturun. Sertifika yedeklemelerini tutmak için her düğümde bir `c:\certs`klasör oluşturun. 

İlk düğümü yapılandırmak için aşağıdaki adımları izleyin: 

1. **SQL Server Management Studio'u** açın ve ilk `AGNode1`düğümünüze bağlayın. 
1. **Yeni** Sorgu penceresi açın ve karmaşık ve güvenli bir parolaya güncelleştirdikten sonra aşağıdaki Transact-SQL (T-SQL) deyimini çalıştırın:

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

1. Ardından, HADR bitiş noktasını oluşturun ve bu Transact-SQL (T-SQL) deyimini çalıştırarak sertifikayı kimlik doğrulaması için kullanın:

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

1. **Sertifikanızın** bulunduğu dosya konumuna gitmek için Dosya `c:\certs`Gezgini'ni kullanın. 
1. Sertifikanın ilk düğümden , `AGNode1Cert.crt`örneğin bir kopyasını el ile yapın ve ikinci düğümde aynı konuma aktarın. 

İkinci düğümü yapılandırmak için aşağıdaki adımları izleyin: 

1. **SQL Server Management Studio**ile ikinci düğüme `AGNode2`bağlanın, örneğin . 
1. Yeni **Sorgu** penceresinde, karmaşık ve güvenli bir parolaya güncelleştirdikten sonra aşağıdaki Transact-SQL (T-SQL) deyimini çalıştırın: 

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

1. Ardından, HADR bitiş noktasını oluşturun ve bu Transact-SQL (T-SQL) deyimini çalıştırarak sertifikayı kimlik doğrulaması için kullanın:

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

1. **Sertifikanızın** bulunduğu dosya konumuna gitmek için Dosya `c:\certs`Gezgini'ni kullanın. 
1. Sertifikanın ikinci düğümden , `AGNode2Cert.crt`gibi bir kopyasını el ile yapın ve ilk düğümde aynı konuma aktarın. 

Kümede başka düğümvarsa, ilgili sertifika adlarını değiştirerek bu adımları da yineleyin. 

## <a name="create-logins"></a>Oturum açma

Sertifika kimlik doğrulaması düğümler arasında veri eşitlemek için kullanılır. Buna izin vermek için, diğer düğüm için bir giriş oluşturun, oturum açma için bir kullanıcı oluşturun, yedeklenen sertifikayı kullanmak için oturum açma sertifikası oluşturun ve ardından yansıtma bitiş noktasına bağlanma izni verin. 

Bunu yapmak için, ilk düğümde aşağıdaki Transact-SQL (T-SQL) sorgusunu `AGNode1`çalıştırın: 

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

Daha sonra, aşağıdaki Transact-SQL (T-SQL) sorgusunu ikinci `AGNode2`düğümde çalıştırın: 

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

Kümede başka düğümler varsa, ilgili sertifikayı ve kullanıcı adlarını değiştirerek bu adımları da burada yineleyin. 

## <a name="configure-availability-group"></a>Kullanılabilirlik grubunu yapılandırma

Bu adımda, kullanılabilirlik grubunuzu yapılandırın ve veritabanlarınızı bu gruba ekleyin. Şu anda bir dinleyici oluşturmayın. Adımları bilmiyorsanız, [kullanılabilirlik grubu öğreticisine](virtual-machines-windows-portal-sql-availability-group-tutorial.md#create-the-availability-group)bakın. Her şeyin olması gerektiği gibi çalıştığını doğrulamak için bir failover ve failback başlattığından emin olun. 

   > [!NOTE]
   > Eşitleme işlemi sırasında bir hata varsa, geçici olarak `NT AUTHORITY\SYSTEM` gibi `AGNode1` ilk düğümüzerinde küme kaynakları oluşturmak için sysadmin hakları vermek gerekebilir. 

## <a name="configure-load-balancer"></a>Yük dengeleyiciyi yapılandırma

Bu son adımda, [Azure portalını](virtual-machines-windows-portal-sql-alwayson-int-listener.md) veya [PowerShell'i](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) kullanarak yük bakiyesini yapılandırın


## <a name="next-steps"></a>Sonraki Adımlar

Kullanılabilirlik grubunu yapılandırmak için [Az SQL VM CLI'yi](virtual-machines-windows-sql-availability-group-cli.md) de kullanabilirsiniz. 


