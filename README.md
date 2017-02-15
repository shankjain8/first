# first//////////////
<Window x:Class="Console1.UserControl1"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:Console1"
             mc:Ignorable="d" 
             d:DesignHeight="600" d:DesignWidth="900">
    <Grid Margin="0,-5,0,5">


        <ComboBox x:Name="Server" HorizontalAlignment="Left" Margin="21,41,0,0" VerticalAlignment="Top" Width="120"
             Loaded="ComboBox_Loaded" SelectionChanged="ComboBox_SelectionChanged" SelectedIndex="0" IsDropDownOpen="False" IsEditable="False" IsReadOnly="True" IsEnabled="True">
        </ComboBox>

        <GroupBox x:Name="App_Group" Header="Applications" HorizontalAlignment="Left" Margin="21,86,0,0"
                  VerticalAlignment="Top" Height="257" Width="138">
            <Grid HorizontalAlignment="Left" Height="251" VerticalAlignment="Top"
                  Width="124" Margin="10,0,0,0">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="34*"/>
                    <ColumnDefinition Width="5*"/>
                </Grid.ColumnDefinitions>
                <CheckBox x:Name="cb_apssia" Content="APSSIA" HorizontalAlignment="Left"
                          Margin="10,10,0,0" VerticalAlignment="Top" Height="15" Width="58" Checked="cb_apssia_Checked" Unchecked="cb_apssia_Unchecked"/>
                <CheckBox x:Name="cb_apsia" Content="APSIA" HorizontalAlignment="Left"
                          Margin="10,30,0,0" VerticalAlignment="Top" Height="15" Width="52" Checked="cb_apsia_Checked"/>
                <CheckBox x:Name="cb_sia" Content="SIA" HorizontalAlignment="Left"
                          Margin="10,50,0,0" VerticalAlignment="Top" Height="15" Width="37"/>
                <CheckBox x:Name="cb_sro" Content="SRO" HorizontalAlignment="Left"
                          Margin="10,70,0,0" VerticalAlignment="Top" Height="15" Width="42"/>
                <CheckBox x:Name="cb_biztalk" Content="BIZTALK" HorizontalAlignment="Left"
                          Margin="10,90,0,0" VerticalAlignment="Top" Height="15" Width="63"/>
                <CheckBox x:Name="cb_property" Content="PROPERTY" HorizontalAlignment="Left"
                          Margin="10,110,0,0" VerticalAlignment="Top" Height="15" Width="75"/>
                <CheckBox x:Name="cb_homes" Content="HOMES" HorizontalAlignment="Left"
                          Margin="10,130,0,0" VerticalAlignment="Top" Height="15" Width="61"/>
                <CheckBox x:Name="cb_boats" Content="BOATS" HorizontalAlignment="Left"
                          Margin="10,150,0,0" VerticalAlignment="Top" Height="15" Width="55"/>
                <CheckBox x:Name="cb_comprater" Content="COMPRATER" HorizontalAlignment="Left"
                          Margin="10,171,0,0" VerticalAlignment="Top" Height="15" Width="87"/>
                <CheckBox x:Name="cb_all" Content="ALL" HorizontalAlignment="Left"
                          Margin="10,192,0,0" VerticalAlignment="Top" Height="15" Width="39"/>
            </Grid>
        </GroupBox>
        <GroupBox x:Name="Silo_Group" Header="Silo's" HorizontalAlignment="Left" Margin="182,86,0,0"
                  VerticalAlignment="Top" Height="135" Width="92">
            <Grid HorizontalAlignment="Left" Height="112" VerticalAlignment="Top"
                  Width="76" Margin="10,0,-6,0" RenderTransformOrigin="0.572,0.511">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="41*"/>
                    <ColumnDefinition Width="5*"/>
                </Grid.ColumnDefinitions>
                <CheckBox x:Name="cb_s1" Content="silo1" HorizontalAlignment="Left"
                          Margin="9,12,0,0" VerticalAlignment="Top" Height="15" Width="44"/>
                <
