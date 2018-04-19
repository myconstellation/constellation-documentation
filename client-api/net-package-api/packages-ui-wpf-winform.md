---
ID: 1563
post_title: Créer des packages UI en Winform ou WPF
author: Sebastien Warin
post_date: 2016-03-22 17:12:30
post_excerpt: ""
layout: post
permalink: >
  https://developer.myconstellation.io/client-api/net-package-api/packages-ui-wpf-winform/
published: true
publish_post_category:
  - "14"
publish_to_discourse:
  - "1"
update_discourse_topic:
  - "0"
post_modified: 2018-04-19 09:56:31
---
Vous pouvez créer des applications graphiques et les déployer sur vos sentinelles UI grâce à Constellation.

Chaque package UI pourra <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/">invoquer</a> ou <a href="/client-api/net-package-api/messagecallbacks/">exposer</a> des MessageCallbacks, <a href="/client-api/net-package-api/consommer-des-stateobjects/">consommer</a> ou <a href="/client-api/net-package-api/stateobjects/">produire</a> des StateObjects, etc…

<!--more-->

<h3>Hello World WPF</h3>

Dans Visual Studio, vous créez un package WPF :

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-153.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Création d'un package UI" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-130.png" alt="Création d'un package UI" width="424" height="294" border="0" /></a></p>

<p align="left">Le template est une application WPF classique :</p>

<p align="left"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-154.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border-width: 0px;" title="Structure du package WPF" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-131.png" alt="Structure du package WPF" width="244" height="204" border="0" /></a></p>

<p align="left">Le <em><a href="/client-api/net-package-api/les-bases-des-packages-net/#Fonctionnement_de_base">IPackage</a></em> de ce package est la classe “App” (App.xaml.cs). Ce package lance la fenêtre MainWindow au démarrage (méthode “OnStart”).</p>

<p align="left">La “MainWindow” est une Window WPF classique à l’exception que dans le constructeur, on enregistre automatiquement les <a href="/client-api/net-package-api/consommer-des-stateobjects/#StateObjectLink_et_Notifier_personnalises">[StateObjectLink]</a> et les <a href="/client-api/net-package-api/messagecallbacks/#Exposer_des_methodes">[MessageCallback]</a> de la classe. De plus on renvoie la description du package (dans le cas où vous avez ajoutez des MessageCallbacks).</p>

<pre class="lang:default decode:true ">public partial class MainWindow : Window
{
    public MainWindow()
    {
        PackageHost.RegisterStateObjectLinks(this);
        PackageHost.RegisterMessageCallbacks(this);
        PackageHost.DeclarePackageDescriptor();
        InitializeComponent();
    }

    private void Window_Loaded(object sender, RoutedEventArgs e)
    {
        this.Title = string.Format("IsRunning: {0} - IsConnected: {1} - IsStandAlone: {2}", PackageHost.IsRunning, PackageHost.IsConnected, PackageHost.IsStandAlone);
        PackageHost.WriteInfo("I'm running !");
    }
}</pre>

<p align="left">Au chargement de la fenêtre on logge un message dans Constellation et on affiche quelques propriété sur l’état du package dans le titre de cette fenêtre !</p>

<p align="left">Ajoutons un simple label “Hello World” au centre de notre fenêtre :</p>

<pre class="lang:xml decode:true">&lt;Label x:Name="label" Content="Hello World" HorizontalAlignment="Center" VerticalAlignment="Center" FontSize="48"/&gt;</pre>

<p align="left">Le code XAML sera donc:</p>

<pre class="lang:xml decode:true">&lt;Window x:Class="MonPackageWPF.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded"&gt;
    &lt;Grid&gt;
        &lt;Label x:Name="label" Content="Hello World" HorizontalAlignment="Center" VerticalAlignment="Center" FontSize="48"/&gt;
    &lt;/Grid&gt;
&lt;/Window&gt;</pre>

<p align="left">Pour tester notre package en debug sans être connecté à Constellation : “F5”</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-167.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug du package en local" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-144.png" alt="Debug du package en local" width="424" height="344" border="0" /></a></p>

<p align="left">Vous noterez que les <a href="/client-api/net-package-api/les-bases-des-packages-net/#Ecrire_des_logs">WriteLog</a> Constellation sont toujours afficher dans la fenêtre de sortie de Visual Studio.</p>

<p align="left">Maintenant pour lançons le debug de notre package dans Visual Studio tout en le connectant à Constellation (raccourci Ctrl+Alt+F8)</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-168.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-145.png" alt="image" width="424" height="102" border="0" /></a></p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-169.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug du package dans Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-146.png" alt="Debug du package dans Constellation" width="424" height="228" border="0" /></a></p>

<h3 align="left">Invoquer des MessageCallbacks</h3>

<p align="left">Vous pouvez <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/">invoquer</a> ou <a href="/client-api/net-package-api/messagecallbacks/">exposer</a> des MessageCallbacks comme n’importe quel package connecté dans votre Constellation.</p>

<p align="left">Pour <a href="/client-api/net-package-api/messagecallbacks/">exposer</a> des MessageCallbacks (des méthodes NET), il suffit d’ajouter l’attribut [MessageCallback] sur vos méthodes.</p>

<p align="left">Pour <a href="/client-api/net-package-api/envoyer-des-messages-invoquer-des-messagecallbacks/">invoquer</a> des MessageCallbacks, il faut créer un scope et envoyer le message. Grace au proxy dynamique, vous pouvez invoquer un MessageCallback comme vous invoquerez une méthode .NET.</p>

<p align="left">Dans cet exemple nous allons invoquer des MessageCallbacks des packages WindowsControl et GoogleTraffic. Vous pouvez <a href="/getting-started/telecharger-et-deployer-des-packages-sur-vos-sentinelles/">suivre le guide ici</a> pour déployer ces deux packages.</p>

<p align="left">En vous rendant sur la page “MessageCallbacks Explorer” de la Console, vous pouvez explorer les MC exposés par les packages.</p>

<p align="left">Par exemple, le package WindowsControl expose plusieurs MessageCallbacks pour arrêter, redémarrer, mettre en veille ou verrouiller l’ordinateur (= la sentinelle) sur lequel le package est déployé.</p>

<p align="left">Le package GoogleTraffic expose un MessageCallback “GetRoute” pour calculer le temps de route en spécifiant un point de départ et d’arrivée. Ce MessageCallback  est une saga pour vous retourner la réponse.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-191.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="MessageCallbacks Explorer" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-168.png" alt="MessageCallbacks Explorer" width="424" height="237" border="0" /></a></p>

<p align="left">Pour simplifier le développement et éviter de travailler avec des types dynamiques, nous allons auto-générer le code.</p>

<p align="left">Cliquez sur le bouton “Generate Code” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-185.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Génération de code" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-162.png" alt="Génération de code" width="424" height="67" border="0" /></a></p>

<p align="left">Sélectionnez votre Constellation, cliquez sur “Discover” et sélectionnez les packages que vous souhaitez ajouter dans le code généré puis cliquez sur “Generate” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-186.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Génération de code" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-163.png" alt="Génération de code" width="424" height="474" border="0" /></a></p>

<p align="left">Editez le code de la MainWindow (<em>MainWindow.xaml.cs</em>) pour ajouter le code généré des MessageCallbacks pour les packages GoogleTraffic et WindowsControl :</p>

<pre class="lang:C# decode:true">using MonPackageWPF.GoogleTraffic.MessageCallbacks;
using MonPackageWPF.WindowsControl.MessageCallbacks;</pre>

<p align="left">Dans la vue XAML, ajoutons deux boutons : l’un pour mettre en veille et l’autre pour faire un test d’itinéraire :</p>

<pre class="lang:xml decode:true">&lt;Button x:Name="btSleep" Content="Sleep !" HorizontalAlignment="Left" VerticalAlignment="Bottom" Margin="10" Width="75" Click="btSleep_Click"/&gt;
&lt;Button x:Name="btTestRoute" Content="Test Route" HorizontalAlignment="Center" VerticalAlignment="Bottom" Margin="10,0,0,10" Width="75" Click="btTestRoute_Click" /&gt;</pre>

<p align="left">Pour le premier bouton, nous allons sélectionner l’instance du package “WindowsControl” sur la sentinelle “PO_SEB” pour créer un scope afin d’invoquer le MessageCallback “Sleep” :</p>

<pre class="lang:C# decode:true">private void btSleep_Click(object sender, RoutedEventArgs e)
{
    MyConstellation.PackageInstances.PO_SEB_WindowsControl.CreateWindowsControlScope().Sleep();
}</pre>

<p align="left">Prenez garde à créer un scope sur une instance (sentinelle + package) car sinon, si vous créez un scope sur le package “WindowsControl”, toutes les sentinelles hébergeant ce package se mettront en veille !</p>

<p align="left">Pour le deuxième bouton, nous allons demander les différentes routes pour un “Lille-Paris”. Comme il s’agit d’une saga (message avec réponse) nous allons l’invoquer en “async/await” et afficher dans le label la meilleure route :</p>

<pre class="lang:C# decode:true">private async void btTestRoute_Click(object sender, RoutedEventArgs e)
{
    label.Content = "Calcul en cours ...";
    var route = await MyConstellation.Packages.GoogleTraffic.CreateGoogleTrafficScope().GetRoutes("lille", "paris");
    var bestRoute = route.OrderBy(r =&gt; r.TimeWithTraffic).FirstOrDefault();
    label.Content = $"{bestRoute.Name}\nDistance:{bestRoute.DistanceInKm}km\nTemps : {bestRoute.TimeWithTraffic}";
}</pre>

<p align="left">Lancer le debug dans Constellation : <img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Debug On Constellation" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-196.png" alt="Debug On Constellation" width="104" height="34" border="0" />  (ou Ctrl+Alt+F8).</p>

<p align="left">Premier test, en cliquant sur Sleep, vous allez envoyer un message pour invoquer le MessageCallback “Sleep” du package “WindowsControl” sur la sentinelle ici nommée “PO-SEB”. Ainsi le Windows “PO-SEB” se mettra instantanément en veille !</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-193.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Test du MC &quot;Sleep&quot;" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-170.png" alt="Test du MC &quot;Sleep&quot;" width="244" height="165" border="0" /></a></p>

<p align="left">Deuxième test, pour invoquer le MessageCallback “GetRoutes” du package GoogleTraffic :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-194.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Test du MC &quot;GetRoutes&quot; en Async" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-171.png" alt="Test du MC &quot;GetRoutes&quot; en Async" width="244" height="165" border="0" /></a><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-195.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Réponse à la saga &quot;GetRoutes&quot;" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-172.png" alt="Réponse à la saga &quot;GetRoutes&quot;" width="244" height="165" border="0" /></a></p>

<h3 align="left">Consommer des StateObjects dans votre vue XAML</h3>

<p align="left">Assurez-vous d’avoir dans votre Constellation au moins un package “HWMonitor” déployé sur une sentinelle. Au besoin, vous pouvez <a href="/getting-started/telecharger-et-deployer-des-packages-sur-vos-sentinelles/">suivre ce guide ici</a>.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-197.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="HWMonitor" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-173.png" alt="HWMonitor" width="424" height="223" border="0" /></a></p>

<p align="left">Pour comprendre en détail, la <a href="/client-api/net-package-api/consommer-des-stateobjects/">consommation des StateObjects</a> dans vos packages n’hésitez pas à relire <a href="/client-api/net-package-api/consommer-des-stateobjects/">cet article</a>.</p>

<p align="left">Par exemple, pour afficher en temps réel la consommation CPU (StateObject nommé “/intelcpu/0/load/0”) mesurée par le package HWMonitor sur sa sentinelle (ici “PO-SEB”), ajoutons un StateObjectLink :</p>

<pre class="lang:C# decode:true">[StateObjectLink("PO-SEB", "HWMonitor", "/intelcpu/0/load/0")]
public StateObjectNotifier CPU { get; set; }</pre>

<p align="left">Vous pouvez également générer du code en sélectionnant le package HWMonitor. Vous pourrez ensuite ajouter un “using” vers les StateObjects de ce package :</p>

<pre class="lang:C# decode:true">using MonPackageWPF.HWMonitor.StateObjects;</pre>

<p align="left">Cela vous permettra d’utiliser un le “HWMonitorStateObjectLink” avec des énumérations générées pour vos sentinelles et nom de StateObjects :</p>

<pre class="lang:C# decode:true">[HWMonitorStateObjectLink(MyConstellation.Sentinels.PO_SEB, HWMonitorStateObjectNames._intelcpu_0_load_0)]
public StateObjectNotifier CPU { get; set; }</pre>

<p align="left">Comme vous le savez, le StateObjectNotifier implémente l’interface INotifyPropertyChanged. Vous pouvez donc lier cette propriété dans votre vue XAML pour voir votre StateObject en temps réel sur votre interface.</p>

<p align="left">Nous allons modifier le label “Hello World” pour afficher en temps réel votre consommation CPU. Pour cela changeons le contenu (Content) du label avec la propriété “Value” du StateObject publié par le package HWMonitor.</p>

<p align="left">Cette propriété contient la valeur (ici de l’utilisation du CPU) avec un nombre décimal. Nous ajoutons également l’attribut “ContentStringFormat” pour n’afficher que 2 chiffres après la virgule.</p>

<pre class="lang:xml decode:true">&lt;Label x:Name="label" Content="{Binding Path=CPU.DynamicValue.Value}" ContentStringFormat="{}{0:N2}%" HorizontalAlignment="Center" VerticalAlignment="Center" FontSize="48"/&gt;</pre>

<p align="left">Pour pourvoir utiliser des propriétés .NET comme binding dans votre vue XAML, vous devez spécifier le DataContext de votre fenêtre vers votre classe MainWindow soit via le code (<em>this.DataContent = this</em>) ou soit directement dans votre vue XAML en ajoutant cette attribut sur l’élément Window :</p>

<pre class="lang:default decode:true">DataContext="{Binding RelativeSource={RelativeSource Self}}"</pre>

<p align="left">Résultat, vous pouvez suivre en temps réel le CPU ici de la machine “PO-SEB” :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-200.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Binding au CPU du package HWMonitor" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-176.png" alt="Binding au CPU du package HWMonitor" width="424" height="284" border="0" /></a></p>

<p align="left">Allons un peu plus loin en ajoutons dans notre code C#, un nouveau StateObjectLink :</p>

<pre class="lang:C# decode:true">[HWMonitorStateObjectLink(HWMonitorStateObjectNames._intelcpu_0_load_0)]
public StateObjectCollectionNotifier CPUs { get; set; }</pre>

<p align="left">Ce “lien” ne précise que le nom du StateObject (ici “/intelcpu/0/load/0”) et le package (HWMonitorStateObjectLink est la classe générée qui spécifie implicitement le package à “HWMonitor).</p>

<p align="left">De ce fait, toutes les consommations CPU mesurées par les instances du package HWMonitor seront captées par ce “link” ! On utilisera donc un StateObject<strong>Collection</strong>Notifier car on aura autant de StateObjects qu’on a d’instance de ce package.</p>

<p align="left">Dans la vue XAML, ajoutons un menu déroulant (combobox) pour afficher le nom des sentinelles (= le nom des machines) des StateObjects de votre collection “CPUs” :</p>

<pre class="lang:xml decode:true">&lt;ComboBox x:Name="comboBox" ItemsSource="{Binding Path=CPUs}" DisplayMemberPath="Value.SentinelName" HorizontalAlignment="Left" Margin="10" VerticalAlignment="Top" /&gt;</pre>

<p align="left">Enfin, modifions une nouvelle fois notre label. Cette fois ci la valeur à afficher n’est pas celle du StateObject “CPU”, mais celle du StateObject sélectionné par la combobox :</p>

<pre class="lang:xml decode:true">&lt;Label x:Name="label" Content="{Binding ElementName=comboBox, Path=SelectedItem.DynamicValue.Value}" ContentStringFormat="{}{0:N2}%" HorizontalAlignment="Center" VerticalAlignment="Center" FontSize="48"/&gt;</pre>

<p align="left">On obtient donc la possibilité de suivre la consommation de chaque machine (= sentinelle) où le package HWMonitor est déployé :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-201.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-177.png" alt="image" width="424" height="286" border="0" /></a></p>

<p align="left">Pour terminer on pourrait également ajouter un StateObjectLink qui contiendrait TOUS les StateObjects produits par les packages HWMonitor, peut importe le nom du StateObject et la sentinelle :</p>

<pre class="lang:C# decode:true">[HWMonitorStateObjectLink]
public StateObjectCollectionNotifier HWMonitor { get; set; }</pre>

<p align="left">Pour afficher toutes ces données, on peut utiliser un DataGrid lié à votre collection de StateObject “HWMonitor” :</p>

<pre class="lang:xml decode:true">&lt;DataGrid ItemsSource="{Binding HWMonitor}"&gt;&lt;/DataGrid&gt;</pre>

<p align="left">On obtiendrait une vue avec deux colonnes, la propriété DynamicValue et Value des StateObjectNotifier :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-198.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="DataGrid sur une collection de StateObjectNotifier" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-174.png" alt="DataGrid sur une collection de StateObjectNotifier" width="424" height="426" border="0" /></a></p>

<p align="left">Pour rendre cela plus visuelle, définissions explicitement des colonnes avec le nom de la sentinelle, le nom du StateObject, la propriété “Name” de la valeur du StateObject  (le nom du compteur), la “Value” et l’unité de la mesure (Unit).</p>

<p align="left">En XAML cela se traduit par le code suivant :</p>

<pre class="lang:xml decode:true">&lt;DataGrid ItemsSource="{Binding HWMonitor}" AutoGenerateColumns="False" Margin="0, 0, 0, 40"&gt;
    &lt;DataGrid.Columns&gt;
        &lt;DataGridTextColumn Header="Sentinel" Binding="{Binding Path=Value.SentinelName}"&gt;&lt;/DataGridTextColumn&gt;
        &lt;DataGridTextColumn Header="StateObject name" Binding="{Binding Path=Value.Name}"&gt;&lt;/DataGridTextColumn&gt;
        &lt;DataGridTextColumn Header="Counter name" Binding="{Binding Path=DynamicValue.Name}"&gt;&lt;/DataGridTextColumn&gt;
        &lt;DataGridTextColumn Header="Value" Binding="{Binding Path=DynamicValue.Value}"&gt;&lt;/DataGridTextColumn&gt;
        &lt;DataGridTextColumn Header="Unit" Binding="{Binding Path=DynamicValue.Unit}"&gt;&lt;/DataGridTextColumn&gt;
    &lt;/DataGrid.Columns&gt;
&lt;/DataGrid&gt;</pre>

<p align="left">Le résultat :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-202.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="StateObjects des packages HWMonitor" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-178.png" alt="StateObjects des packages HWMonitor" width="424" height="615" border="0" /></a></p>

<p align="left">Prenez garde car les StateObjects sont mis à jour dans le StateObjectsCollectionNotifier par des threads différents. Vous risquez donc d’avoir des exceptions du type “<em>An ItemsControl is inconsistent with its items source</em>”. Afin d’éviter ce genre d’erreur, utilisez la  méthode “<em>BindingOperations.EnableCollectionSynchronization</em>”.</p>

<p align="left">Pour cela, dans votre classe, ajoutez un objet de synchronisation :</p>

<pre class="lang:C# decode:true">private static object _syncLock = new object();</pre>

<p align="left">Puis dans le constructeur de votre fenêtre, après le <em>InitializeComponent()</em>, activez la synchronisation de la collection sur votre StateObjectCollectionNotifier (ici nommé ‘HWMonitor’) :</p>

<pre class="lang:C# decode:true">BindingOperations.EnableCollectionSynchronization(HWMonitor, _syncLock);</pre>

<p align="left">Si lancez votre package dans une Constellation avec plusieurs instances du packages HWMonitor sur vos différentes sentinelles, vous aurez une vision temps réel de l’ensemble de vos machines Windows avec seulement ces quelques lignes de XAML et Constellation :</p>

<p align="left"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-203.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border: 0px;" title="StateObjects des packages HWMonitor" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-179.png" alt="StateObjects des packages HWMonitor" width="424" height="580" border="0" /></a></p>

<h3 align="left">Déployez votre package UI</h3>

<h4 align="left">Publier le package</h4>

<p align="left">Le sujet a été traité dans <a href="/getting-started/creez-votre-premier-package-constellation-en-csharp/#Publier_son_package_dans_Constellation">le guide de démarrage,</a> il suffit de cliquer-droit sur votre projet et sélectionner dans le menu Constellation “Publish On Constellation” ou directement depuis la toolbar :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-170.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Publication du package" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-147.png" alt="Publication du package" width="424" height="97" border="0" /></a></p>

<p align="left">Vous pourrez alors choisir le type de publication (Local ou Upload sur le serveur Constellation) :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-157.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Publication du package" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-134.png" alt="Publication du package" width="424" height="212" border="0" /></a></p>

<h4 align="left">Déployer le package sur une sentinelle UI</h4>

<p align="left">Vous devez impérativement déployer un package “UI” sur une sentinelle UI. Si vous tentez d’ajouter un package UI sur une sentinelle service, le package démarrera mais aucune fenêtre ne pourra être visible (le service ne peut pas interagir avec le bureau Windows).</p>

<p align="left">Les sentinelles UI ont le suffixe “_UI” dans leurs noms. Ici pour cette Constellation, il y a deux sentinelles connectées, l’une de type “Service” et l’autre “UI”, toutes deux sur la même machine (nommé “PO-SEB”).</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-171.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Sentinelles connectées" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-148.png" alt="Sentinelles connectées" width="424" height="263" border="0" /></a></p>

<p align="left">Pour ajouter notre package à la sentinelle UI, vous pouvez éditer la configuration de vos Constellation directement depuis Visual Studio :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-172.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Ajout du package depuis Visual Studio" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-149.png" alt="Ajout du package depuis Visual Studio" width="424" height="232" border="0" /></a></p>

<p align="left">Ou bien depuis la Console Constellation :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-173.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Ajout du package dans une sentinelle UI" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-150.png" alt="Ajout du package dans une sentinelle UI" width="424" height="263" border="0" /></a></p>

<p align="left">Pour déployer la configuration, cliquez sur le bouton “Save &amp; Deploy” depuis la Console, ou directement sur la page des “Packages” cliquez sur “Reload &amp; Deploy”.</p>

<p align="left">Votre package UI sera démarré et vous pourrez le contrôler depuis la Console comme pour les autres packages.</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-174.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="Contrôle du package UI sur la Console" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-151.png" alt="Contrôle du package UI sur la Console" width="424" height="263" border="0" /></a></p>

<h4 align="left">Démarrer son package “manuellement”</h4>

<p align="left">Si vous créez une application à destination d’une borne d’affichage, comme un miroir, le package est démarré automatiquement par la sentinelle (comportement par défaut) et est relancé si le package plante !</p>

<p align="left">Cependant, si votre package est destiné à être utilisé par un utilisateur comme une application Windows classique vous voudriez certainement ne pas la lancer automatiquement au démarrage de la sentinelle. Au contraire vous voudriez que ce soit l’utilisateur qui décide de la lancer en lançant un raccourci Windows par exemple sans devoir se connecter sur la Console de votre Constellation.</p>

<p align="left">Pour cela vous pouvez lancer la sentinelle en passant un ordre en paramètre :</p>

<pre class="lang:default decode:true">Constellation.Sentinel.UI.exe &lt;action&gt; &lt;package&gt;</pre>

<p align="left">Les actions peuvent être :</p>

<ul>
    <li>
<div align="left">Start</div></li>
    <li>
<div align="left">Stop</div></li>
    <li>
<div align="left">Restart</div></li>
    <li>
<div align="left">Reload</div></li>
</ul>

<p align="left">Par exemple, créons un raccourci sur le bureau vers :</p>

<pre class="lang:default decode:true">Constellation.Sentinel.UI.exe Reload MonPackageWPF</pre>

<p align="left">Ainsi dès que vous double-cliquerez sur ce raccourci, la sentinelle téléchargera la dernière version du package sur le serveur et lancera votre package (Reload) :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-175.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-152.png" alt="image" width="424" height="322" border="0" /></a></p>

<p align="left">Bien entendu, l’état du package sera automatiquement synchronisé dans la Constellation. Vous pourrez donc contrôler l’état du package depuis la Console par exemple.</p>

<p align="left">Par défaut, une sentinelle démarre tous les packages qui lui sont assignés. Si c’est un package destiné à être lancé manuellement par l’utilisateur, vous voudriez peut être ne pas lancer automatiquement le package. Vous pouvez donc définir l’attribut “autoStart” à false au niveau de la configuration de votre package.</p>

<p align="left">Aussi l’ordre d’arrêt d’un package doit provenir du hub de contrôle de Constellation, qui se chargera de communiquer l’ordre au package lui même (de s’arrêter) et à sa sentinelle (de tuer le package si il ne s’est pas arrêté dans le temps imparti).</p>

<p align="left">Seulement, dans un package UI de ce type, c’est à dire “application Windows classique”, l’utilisateur fermera naturellement l’application en cliquant sur la croix rouge en haut à droite !</p>

<p align="left">La sentinelle détectera la mort du processus du package alors qu’elle n’a pas eu l’ordre de Constellation d’arrêter le package ! Du point de vue de la sentinelle, c’est un arrêt brutal !</p>

<p align="left">Elle appliquera donc les options de récupération qui par défaut redémarre un package 30 secondes après un arrêt brutal :</p>

<p align="left"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-176.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border-width: 0px;" title="RecoveryOption par défaut" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-153.png" alt="RecoveryOption par défaut" width="424" height="46" border="0" /></a></p>

<p align="left">Les options par défaut sont définis dans la configuration de la Constellation :</p>

<p align="center"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-177.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="RecoveryOption par défaut" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-154.png" alt="RecoveryOption par défaut" width="424" height="137" border="0" /></a></p>

<p align="left">Dans notre cas, nous allons redéfinir ces options de récupération au niveau du package lui même pour ne pas redémarrer un package suite à un arrêt forcé et ne pas démarrer automatiquement notre package au démarrage. La configuration du package sera :</p>

<p align="left"><a href="https://developer.myconstellation.io/wp-content/uploads/2016/03/image-178.png"><img style="background-image: none; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; display: block; padding-right: 0px; margin-right: auto; border-width: 0px;" title="Configuration d'un package UI" src="https://developer.myconstellation.io/wp-content/uploads/2016/03/image_thumb-155.png" alt="Configuration d'un package UI" width="424" height="134" border="0" /></a></p>