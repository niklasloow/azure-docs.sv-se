---
title: Företags säkerhet med Azure AD DS – Azure HDInsight
description: Lär dig hur du konfigurerar och konfigurerar ett HDInsight-Enterprise Security Package kluster med Azure Active Directory Domain Services.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: seodec18,seoapr2020
ms.date: 04/17/2020
ms.openlocfilehash: 7792ac688ede32155ec32e1f4ba25b328102f86c
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86079844"
---
# <a name="enterprise-security-package-configurations-with-azure-active-directory-domain-services-in-hdinsight"></a>Enterprise Security Package konfigurationer med Azure Active Directory Domain Services i HDInsight

Enterprise Security Package (ESP)-kluster ger åtkomst till flera användare i Azure HDInsight-kluster. HDInsight-kluster med ESP är anslutna till en domän. Med den här anslutningen kan domän användare använda sina domänautentiseringsuppgifter för att autentisera med klustren och köra stora data jobb.

I den här artikeln får du lära dig hur du konfigurerar ett HDInsight-kluster med ESP genom att använda Azure Active Directory Domain Services (Azure AD DS).

> [!NOTE]  
> ESP är allmänt tillgängligt i HDInsight 3,6 och 4,0 för dessa kluster typer: Apache Spark, Interactive, Hadoop och HBase. ESP för kluster typen Apache Kafka är i för hands version med stöd för bästa prestanda. ESP-kluster som skapats före ESP GA-datumet (1 oktober 2018) stöds inte.

## <a name="enable-azure-ad-ds"></a>Aktivera Azure AD DS

> [!NOTE]  
> Endast klient organisations administratörer har behörighet att aktivera Azure AD DS. Om kluster lagringen är Azure Data Lake Storage Gen1 eller Gen2, måste du inaktivera Azure Multi-Factor Authentication endast för användare som behöver åtkomst till klustret med hjälp av grundläggande Kerberos-autentisering.
>
> Du kan använda [betrodda IP-adresser](../../active-directory/authentication/howto-mfa-mfasettings.md#trusted-ips) eller [villkorlig åtkomst](../../active-directory/conditional-access/overview.md) för att inaktivera Multi-Factor Authentication *endast* för vissa användare när de använder IP-intervallet för HDInsight-klustrets virtuella nätverk. Om du använder villkorlig åtkomst ser du till att den Active Directory tjänstens slut punkt är aktive rad i det virtuella HDInsight-nätverket.
>
> Om kluster lagringen är Azure Blob Storage ska du inte inaktivera Multi-Factor Authentication.

Att aktivera Azure AD DS är en förutsättning innan du kan skapa ett HDInsight-kluster med ESP. Mer information finns i [aktivera Azure Active Directory Domain Services med hjälp av Azure Portal](../../active-directory-domain-services/tutorial-create-instance.md).

När Azure AD DS är aktive rad börjar alla användare och objekt att synkronisera från Azure Active Directory (Azure AD) till Azure AD DS som standard. Längden på synkroniseringsåtgärden beror på antalet objekt i Azure AD. Synkroniseringen kan ta några dagar i hundratals tusentals objekt.

Domän namnet som du använder med Azure AD DS måste innehålla 39 tecken eller färre för att fungera med HDInsight.

Du kan välja att bara synkronisera de grupper som behöver åtkomst till HDInsight-kluster. Det här alternativet för att synkronisera endast vissa grupper kallas *omsynkronisering av omfång*. Instruktioner finns i [Konfigurera omfångs synkronisering från Azure AD till din hanterade domän](../../active-directory-domain-services/scoped-synchronization.md).

När du aktiverar säker LDAP ska du lägga till domän namnet i ämnes namnet. Och alternativt namn för certifikat mottagare i certifikatet. Om ditt domän namn är *contoso100.onmicrosoft.com*kontrollerar du att det exakta namnet finns i certifikatets ämnes namn och alternativt namn för certifikat mottagare. Mer information finns i [Konfigurera säker LDAP för en Azure AD DS-hanterad domän](../../active-directory-domain-services/tutorial-configure-ldaps.md).

I följande exempel skapas ett självsignerat certifikat. Domän namnet *contoso100.onmicrosoft.com* finns i båda `Subject` (ämnes namn) och `DnsName` (alternativt namn för certifikat mottagare).

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.onmicrosoft.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.onmicrosoft.com, contoso100.onmicrosoft.com
```

## <a name="check-azure-ad-ds-health-status"></a>Kontrol lera hälso status för Azure AD DS

Visa hälso status för Azure Active Directory Domain Services genom att välja **hälsa** i kategorin **Hantera** . Kontrol lera att statusen för Azure AD DS är grön (körs) och att synkroniseringen är klar.

![Azure AD DS-hälsa](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="create-and-authorize-a-managed-identity"></a>Skapa och auktorisera en hanterad identitet

Använd en *användardefinierad hanterad identitet* för att förenkla säker domän tjänst åtgärder. När du tilldelar rollen **HDInsight Domain Services Contributor** till den hanterade identiteten kan den läsa, skapa, ändra och ta bort Domain Services-åtgärder.

Vissa åtgärder för domän tjänster, till exempel skapa organisationsenheter och tjänstens huvud namn, krävs för HDInsight-Enterprise Security Package. Du kan skapa hanterade identiteter i vilken prenumeration som helst. Mer information om hanterade identiteter i allmänhet finns i [hanterade identiteter för Azure-resurser](../../active-directory/managed-identities-azure-resources/overview.md). Mer information om hur hanterade identiteter fungerar i Azure HDInsight finns i [hanterade identiteter i Azure HDInsight](../hdinsight-managed-identities.md).

Om du vill konfigurera ESP-kluster skapar du en användardefinierad hanterad identitet om du inte redan har en. Se [`Create, list, delete, or assign a role to a user-assigned managed identity by using the Azure portal`](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) .

Tilldela sedan rollen **HDInsight Domain Services Contributor** till den hanterade identiteten i **åtkomst kontroll** för Azure AD DS. Du behöver administratörs behörighet för Azure AD DS för att utföra den här roll tilldelningen.

![Azure Active Directory Domain Services åtkomst kontroll](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Genom att tilldela rollen **HDInsight Domain Services Contributor** ser du till att den här identiteten har rätt ( `on behalf of` ) åtkomst till Domain Services-åtgärder på Azure AD DS-domänen. Dessa åtgärder omfattar att skapa och ta bort organisationsenheter.

När den hanterade identiteten har fått rollen hanterar Azure AD DS-administratören vem som använder den. Först väljer administratören den hanterade identiteten i portalen. Väljer sedan **Access Control (IAM)** under **Översikt**. Administratören tilldelar rollen **hanterad identitets operatör** till användare eller grupper som vill skapa ESP-kluster.

Azure AD DS-administratören kan till exempel tilldela den här rollen till **MarketingTeam** -gruppen för **sjmsi** -hanterad identitet. Ett exempel visas i följande bild. Den här tilldelningen säkerställer att rätt personer i organisationen kan använda den hanterade identiteten för att skapa ESP-kluster.

![HDInsight-hanterad identitets operatör roll tilldelning](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-managed-identity-operator-role-assignment.png)

## <a name="network-considerations"></a>Nätverksöverväganden

> [!NOTE]  
> Azure AD DS måste distribueras i ett Azure Resource Manager-baserat virtuellt nätverk. Klassiska virtuella nätverk stöds inte för Azure AD DS. Mer information finns i [aktivera Azure Active Directory Domain Services med hjälp av Azure Portal](../../active-directory-domain-services/tutorial-create-instance-advanced.md#create-and-configure-the-virtual-network).

Aktivera Azure AD DS. Sedan körs en lokal Domain Name System-Server (DNS) på Active Directory virtuella datorerna (VM). Konfigurera det virtuella Azure AD DS-nätverket så att det använder dessa anpassade DNS-servrar. Du hittar rätt IP-adresser genom att välja **Egenskaper** i kategorin **Hantera** och titta under **IP-adress på virtuellt nätverk**.

![Hitta IP-adresser för lokala DNS-servrar](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns1.png)

Ändra konfigurationen för DNS-servrarna i det virtuella Azure AD DS-nätverket. Om du vill använda de här anpassade IP-adresserna väljer du **DNS-servrar** i kategorin **Inställningar** . Välj sedan det **anpassade** alternativet, ange den första IP-adressen i text rutan och välj **Spara**. Lägg till fler IP-adresser med hjälp av samma steg.

![Uppdaterar det virtuella nätverkets DNS-konfiguration](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)

Det är enklare att placera både Azure AD DS-instansen och HDInsight-klustret i samma virtuella Azure-nätverk. Om du planerar att använda olika virtuella nätverk måste du peer-koppla dessa virtuella nätverk så att domänkontrollanten är synlig för virtuella HDInsight-datorer. Mer information finns i [peering för virtuella nätverk](../../virtual-network/virtual-network-peering-overview.md).

När de virtuella nätverken har peer-kopplats konfigurerar du det virtuella HDInsight-nätverket så att det använder en anpassad DNS-server. Och ange de privata IP-adresserna för Azure AD DS som DNS-serveradresser. När båda virtuella nätverk använder samma DNS-servrar kommer ditt anpassade domän namn att matchas mot rätt IP-adress och kan komma att bli tillgängligt från HDInsight. Om ditt domän namn till exempel är `contoso.com` , ska du efter det här steget, `ping contoso.com` matcha till rätt Azure AD DS-IP.

![Konfigurera anpassade DNS-servrar för ett peer-kopplat virtuellt nätverk](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

Om du använder regler för nätverks säkerhets grupper (NSG) i ditt HDInsight-undernät, bör du tillåta de [IP-adresser som krävs](../hdinsight-management-ip-addresses.md) för både inkommande och utgående trafik.

Testa nätverks konfigurationen genom att ansluta en virtuell Windows-dator till det virtuella HDInsight-nätverket/under nätet och pinga domän namnet. (Den bör matcha till en IP-adress.) Kör **ldp.exe** för att få åtkomst till Azure AD DS-domänen. Anslut sedan den här virtuella Windows-datorn till domänen för att bekräfta att alla nödvändiga RPC-anrop lyckas mellan klienten och servern.

Använd **nslookup** för att bekräfta nätverks åtkomst till ditt lagrings konto. Eller en extern databas som du kan använda (till exempel externa Hive-metaarkiv eller Ranger DB). Se till att de [nödvändiga portarna](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10)#communication-to-domain-controllers) är tillåtna i Azure AD DS-undernätets NSG-regler, om en NSG skyddar Azure AD DS. Om domänen som ansluter till den här virtuella Windows-datorn lyckas kan du fortsätta till nästa steg och skapa ESP-kluster.

## <a name="create-an-hdinsight-cluster-with-esp"></a>Skapa ett HDInsight-kluster med ESP

När du har konfigurerat de föregående stegen korrekt är nästa steg att skapa HDInsight-klustret med ESP aktiverat. När du skapar ett HDInsight-kluster kan du aktivera Enterprise Security Package på fliken **säkerhet + nätverk** . Använd Portal upplevelsen en gång för en Azure Resource Manager mall för distribution. Hämta sedan den förifyllda mallen på sidan **Granska och skapa** för framtida åter användning.

Du kan också aktivera [HDInsight ID Broker-](identity-broker.md) funktionen när klustret skapas. Med funktionen ID Broker kan du logga in på Ambari med hjälp av Multi-Factor Authentication och hämta de nödvändiga Kerberos-biljetterna utan att behöva lösen ords-hashar i Azure AD DS.

> [!NOTE]  
> De första sex tecknen i ESP-klustrets namn måste vara unika i din miljö. Om du till exempel har flera ESP-kluster i olika virtuella nätverk väljer du en namngivnings konvention som säkerställer att de första sex tecknen i kluster namnen är unika.

![Domän validering för Azure HDInsight Enterprise Security Package](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp.png)

När du har aktiverat ESP identifieras och verifieras vanliga felkonfigurationer som är relaterade till Azure AD DS automatiskt. När du har åtgärdat felen kan du fortsätta med nästa steg.

![Azure HDInsight Enterprise Security Package domän verifieringen misslyckades](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp-error.png)

När du skapar ett HDInsight-kluster med ESP måste du ange följande parametrar:

* **Kluster administratörs användare**: Välj en administratör för klustret från den synkroniserade Azure AD DS-instansen. Det här domän kontot måste redan vara synkroniserat och tillgängligt i Azure AD DS.

* **Kluster åtkomst grupper**: de säkerhets grupper vars användare du vill synkronisera och har åtkomst till klustret ska vara tillgängliga i Azure AD DS. Ett exempel är HiveUsers-gruppen. Mer information finns i [skapa en grupp och lägga till medlemmar i Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

* **LDAPS-URL**: ett exempel är `ldaps://contoso.com:636` .

Den hanterade identitet som du har skapat kan väljas från List rutan **användarens tilldelade hanterade identitet** när du skapar ett nytt kluster.

![Azure HDInsight ESP Active Directory Domain Services hanterad identitet](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-identity.png).

## <a name="next-steps"></a>Nästa steg

* Om du vill konfigurera Hive-principer och köra Hive-frågor, se [konfigurera Apache Hive principer för HDInsight-kluster med ESP](apache-domain-joined-run-hive.md).
* Information om hur du använder SSH för att ansluta till HDInsight-kluster med ESP finns i [använda SSH med Linux-baserade Apache Hadoop på HDInsight från Linux, UNIX eller OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight).
