{:new_window: target="_blank"} 

# Initiation à {{site.data.keyword.blockstorageshort}} (Bêta)

Dernière mise à jour : 29 juillet 2016
{: .last-updated}

{{site.data.keyword.blockstoragefull}} permet d'accéder aux blocs de l'espace de stockage pour les charges de travail comportant de nombreuses transactions et les contextes d'exécution requérant un stockage permanent. Vous pouvez utiliser le service {{site.data.keyword.blockstorageshort}} pour gérer des cycles de vie de volume, connecter des volumes à vos serveurs virtuels IBM et créer des instantanés de vos volumes de stockage par blocs. 

Avant de commencer, prenez soin de lire les informations suivantes :

* Le service {{site.data.keyword.blockstorageshort}} est pris en charge uniquement dans un contexte non lié.  
* Vous devez avoir créé des {{site.data.keyword.virtualmachinesshort}} pour pouvoir connecter des volumes de stockage par blocs. Pour en savoir plus sur l'utilisation de volumes de stockage par blocs avec des {{site.data.keyword.virtualmachinesshort}}, voir [Volumes de stockage par blocs et serveurs virtuels IBM](../../virtualmachines/vm_create.html#storage_BS). 

Procédez comme suit pour commencer à utiliser {{site.data.keyword.blockstorageshort}} :

1. Créez un volume.
   
   a. Ouvrez le service {{site.data.keyword.blockstorageshort}}. 

   b. Cliquez sur **Créer** pour ouvrir la boîte de dialogue **Créer des volumes**. 

   c. Indiquez la taille que vous souhaitez affecter à ce volume. Les nombres décimaux ne sont pas acceptés. La taille est limitée par le quota affecté à votre organisation.
   
   d. Indiquez un nom. Il n'est utilisé qu'à des fins d'affichage.
   
   e. Facultatif : Donnez une description plus détaillée du volume.
   
   f. Cliquez sur **Créer** pour soumettre les informations, puis fermez la boîte de dialogue.

  La création d'un volume peut prendre quelques instants.

2. Connectez un volume à un serveur virtuel. 

   a. Ouvrez le service {{site.data.keyword.blockstorageshort}}. 
   
   b. Sélectionnez un volume dans la liste des volumes disponibles.
   
   c. Cliquez sur **Joindre**.
   
   d. Dans la boîte de dialogue Joindre, sélectionnez une instance de serveur virtuel dans la liste déroulante. 
   
   e. Facultatif : Indiquez l'unité à utiliser pour connecter ce volume. Si vous n'indiquez aucune unité, le système sélectionne automatiquement la première unité disponible sur le serveur virtuel.
   
   f. Cliquez sur **Joindre** pour soumettre les informations, puis fermez la boîte de dialogue.
   
   Le volume apparaît dans le tableau des volumes connectés parmi les informations relatives à l'instance de serveur virtuel. Le serveur virtuel peut désormais utiliser cette unité pour conserver des données. 
 
Etape suivante ?

Préparez le volume pour utilisation. Pour plus d'information, voir [Préparation de volumes](../BlockStorage/blockstorage_preparingvolume.html).

# Liens connexes
{: #rellinks}

## Tutoriels et exemples
{:id="samples"}

* [How to Use IBM Block Storage for Bluemix with IBM Virtual Servers](https://developer.ibm.com/bluemix/2016/02/24/use-block-storage-for-bluemix-with-virtual-servers/){: new_window}
* [Getting Started with IBM Block Storage for Bluemix](https://developer.ibm.com/bluemix/2016/02/15/getting-started-with-block-storage/){: new_window}
* [IBM Block Storage for Bluemix Demo](https://www.youtube.com/watch?v=3gCIHYKU1rE&list=PLzpeuWUENMK2d3L5qCITo2GQEt-7r0oqm&index=45){: new_window}

## Informations de référence sur l'API
{: #api}
* [API OpenStack Block Storage (Cinder) version 2](http://developer.openstack.org/api-ref-blockstorage-v2.html){: new_window}

