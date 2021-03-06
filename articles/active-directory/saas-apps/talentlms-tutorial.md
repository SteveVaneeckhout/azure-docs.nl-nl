---
title: 'Zelfstudie: Azure Active Directory-integratie met TalentLMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TalentLMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c903d20d-18e3-42b0-b997-6349c5412dde
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 79e3b853728f6f997ad97a653ac3c119b2760cbd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430636"
---
# <a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Zelfstudie: Azure Active Directory-integratie met TalentLMS

In deze zelfstudie leert u hoe u TalentLMS integreren met Azure Active Directory (Azure AD).

TalentLMS integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TalentLMS heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij TalentLMS (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TalentLMS, moet u de volgende items:

- Een Azure AD-abonnement
- Een TalentLMS eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TalentLMS uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-talentlms-from-the-gallery"></a>TalentLMS uit de galerie toe te voegen
Voor het configureren van de integratie van TalentLMS in Azure AD, moet u TalentLMS uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TalentLMS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **TalentLMS**.

    ![Het maken van een Azure AD-testgebruiker](./media/talentlms-tutorial/tutorial_talentlms_search.png)

1. Selecteer in het deelvenster resultaten **TalentLMS**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/talentlms-tutorial/tutorial_talentlms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met TalentLMS op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in TalentLMS is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TalentLMS tot stand worden gebracht.

In TalentLMS, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TalentLMS, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker TalentLMS](#creating-a-talentlms-test-user)**  : als u wilt een equivalent van Britta Simon in TalentLMS die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing TalentLMS.

**Voor het configureren van Azure AD eenmalige aanmelding met TalentLMS, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TalentLMS** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/talentlms-tutorial/tutorial_talentlms_samlbase.png)

1. Op de **TalentLMS domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/talentlms-tutorial/tutorial_talentlms_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<tenant-name>.TalentLMSapp.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `http://<tenant-name>.talentlms.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [TalentLMS Client ondersteuningsteam](https://www.talentlms.com/contact) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![Eenmalige aanmelding configureren](./media/talentlms-tutorial/tutorial_talentlms_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/talentlms-tutorial/tutorial_general_400.png)

1. Op de **TalentLMS configuratie** sectie, klikt u op **configureren TalentLMS** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/talentlms-tutorial/tutorial_talentlms_configure.png)  

1. In een ander browservenster aanmelden bij uw bedrijf TalentLMS site als beheerder.

1. In de **& Accountinstellingen** sectie, klikt u op de **gebruikers** tabblad.
   
    ![& Accountinstellingen](./media/talentlms-tutorial/IC777296.png "& Accountinstellingen")

1. Klik op **Single Sign-On (SSO)**,

1. In de sectie Single Sign-On, moet u de volgende stappen uitvoeren:
   
    ![Single Sign-On](./media/talentlms-tutorial/IC777297.png "Single Sign-On")   

    a. Uit de **SSO Integratietype** in de lijst met **SAML 2.0**.

    b. In de **id-provider (IDP)** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.
 
    c. Plak de **vingerafdruk** -waarde uit Azure portal in de **certificaat vingerafdruk** tekstvak.    

    d.  In de **externe URL voor aanmelding bij** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.
 
    e. In de **externe URL voor afmelden** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    f. Vul in het volgende: 

    * In de **TargetedID** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
     
    * In de **voornaam** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`
    
    * In de **achternaam** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`
    
    * In de **e** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`
    
1. Klik op **Opslaan**.
 
> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/talentlms-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/talentlms-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/talentlms-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/talentlms-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-talentlms-test-user"></a>Het maken van een testgebruiker TalentLMS

Als u wilt dat Azure AD-gebruikers zich aanmelden bij TalentLMS, moeten ze worden ingericht voor TalentLMS. In het geval van TalentLMS is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **TalentLMS** tenant.

1. Klik op **gebruikers**, en klik vervolgens op **gebruiker toevoegen**.

1. Op de **gebruiker toevoegen** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Gebruiker toevoegen](./media/talentlms-tutorial/IC777299.png "gebruiker toevoegen")  

    a. In de **voornaam** tekstvak, geef de voornaam van de gebruiker, zoals **Julia**.

    b. In de **achternaam** tekstvak, geef de achternaam van de gebruiker, zoals **Simon**.
 
    c. In de **e-mailadres** tekstvak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    d. Klik op **gebruiker toevoegen**.

>[!NOTE]
>U kunt alle andere TalentLMS gebruiker-account maken van hulpprogramma's of API's geleverd door TalentLMS aan inrichten AAD-gebruikersaccounts.
 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TalentLMS.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan TalentLMS toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **TalentLMS**.

    ![Eenmalige aanmelding configureren](./media/talentlms-tutorial/tutorial_talentlms_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel TalentLMS in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing TalentLMS

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/talentlms-tutorial/tutorial_general_01.png
[2]: ./media/talentlms-tutorial/tutorial_general_02.png
[3]: ./media/talentlms-tutorial/tutorial_general_03.png
[4]: ./media/talentlms-tutorial/tutorial_general_04.png

[100]: ./media/talentlms-tutorial/tutorial_general_100.png

[200]: ./media/talentlms-tutorial/tutorial_general_200.png
[201]: ./media/talentlms-tutorial/tutorial_general_201.png
[202]: ./media/talentlms-tutorial/tutorial_general_202.png
[203]: ./media/talentlms-tutorial/tutorial_general_203.png

